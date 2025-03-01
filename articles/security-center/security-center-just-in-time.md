---
title: Dostęp just in time maszyny wirtualnej w usłudze Azure Security Center | Dokumentacja firmy Microsoft
description: W tym dokumencie przedstawiono kontrolować dostęp do maszyny Wirtualnej jak just-in-time w usłudze Azure Security Center ułatwia dostęp do usługi Azure virtual machines.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 671930b1-fc84-4ae2-bf7c-d34ea37ec5c7
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/17/2019
ms.author: v-mohabe
ms.openlocfilehash: eb9366acf82c94bdf99c4d4f0c7c6bdf4f51e06d
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295014"
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Zarządzanie dostępem maszyny wirtualnej przy użyciu just-in-time

Just-in-time (JIT) maszyny wirtualnej (VM) dostępu może służyć do blokowania ruchu przychodzącego do maszyn wirtualnych platformy Azure, zmniejszenia narażenia na ataki przy zapewnianiu łatwego dostępu, aby nawiązać połączenie z maszynami wirtualnymi w razie.

> [!NOTE]
> Funkcja just in time jest dostępne w warstwie standardowa usługi Security Center.  Zobacz [cennik](security-center-pricing.md), aby dowiedzieć się więcej na temat warstw cenowych usługi Security Center.


> [!NOTE]
> Usługa Security Center just-in-time w maszynie Wirtualnej dostęp do aktualnie obsługuje tylko maszyny wirtualne wdrożone za pośrednictwem usługi Azure Resource Manager. Aby dowiedzieć się więcej o klasyczny, jak i modelem wdrażania usługi Resource Manager, zobacz [usługi Azure Resource Manager, a wdrożeniem klasycznym](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="attack-scenario"></a>Ataku

Atak siłowy ataki często porty zarządzania docelowe w celu uzyskania dostępu do maszyny Wirtualnej. W przypadku powodzenia osoba atakująca może przejąć kontrolę nad maszyną Wirtualną i ustanowić przyczółka w środowisku.

Jest jednym ze sposobów, aby zmniejszyć narażenie na ataki siłowe Aby ograniczyć ilość czasu, który port jest otwarty. Porty zarządzania nie muszą być otwarte przez cały czas. Muszą być otwarte tylko wtedy, gdy nawiązano połączenie z maszyną wirtualną, np. aby wykonać zadania związane z zarządzaniem lub konserwacją. Gdy just in time jest włączone, usługa Security Center używa [sieciowej grupy zabezpieczeń](../virtual-network/security-overview.md#security-rules) (NSG) i reguł zapory usługi Azure, które ograniczają dostęp do portów zarządzania, dlatego nie może być obiektem ataków.

![Scenariusz just-in-time](./media/security-center-just-in-time/just-in-time-scenario.png)

## <a name="how-does-jit-access-work"></a>Jak działa dostęp JIT

Gdy just in time jest włączone, usługa Security Center zablokuje ruch przychodzący do maszyn wirtualnych platformy Azure, tworząc regułę sieciowej grupy zabezpieczeń. Należy wybrać porty na maszynie Wirtualnej, do którego zostanie zablokowany ruch przychodzący. Te porty są kontrolowane przez to rozwiązanie just in time.

Gdy użytkownik poprosi o dostęp do maszyny Wirtualnej, Centrum zabezpieczeń sprawdza, czy użytkownik ma [kontroli dostępu opartej na rolach (RBAC)](../role-based-access-control/role-assignments-portal.md) uprawnienia, które umożliwia pomyślnie poprosić o dostęp do maszyny Wirtualnej. Jeśli żądanie zostanie zatwierdzone, usługa Security Center automatycznie skonfiguruje sieciowe grupy zabezpieczeń (NSG) i zapory usługi Azure, aby zezwolić na ruch przychodzący do wybranych portów i żądane źródłowe adresy IP lub zakresów ilości czasu, który został określony. Po upływie czasu, usługa Security Center przywraca sieciowych grup zabezpieczeń do poprzedniego stanu. Tych połączeń, które już istnieją są nie są przerywane, jednak.

 > [!NOTE]
 > Jeśli żądanie dostępu JIT zostanie zatwierdzona do maszyny Wirtualnej z systemem Azure zaporą, następnie Centrum zabezpieczeń automatycznie zmieni reguły sieciowej grupy zabezpieczeń i zapory. Ilości czasu, który został określony reguły zezwalają na ruch przychodzący do portów wybranych i żądane źródłowe adresy IP lub zakresów. Po tym czasie usługa Security Center przywraca Zapora i reguły sieciowej grupy zabezpieczeń do poprzedniego stanu.


## <a name="permissions-needed-to-configure-and-use-jit"></a>Uprawnienia wymagane do konfigurowania i używania JIT

| Aby umożliwić użytkownikowi: | Uprawnienia do ustawienia|
| --- | --- |
| Konfigurowanie lub edytowanie zasad JIT dla maszyny Wirtualnej | *Te działania można przypisać do roli:*  W zakresie subskrypcji lub grupy zasobów, jest skojarzona z maszyną Wirtualną: ```Microsoft.Security/locations/jitNetworkAccessPolicies/write``` W zakresie subskrypcji lub grupy zasobów lub maszyny Wirtualnej: ```Microsoft.Compute/virtualMachines/write``` | 
| ||
|Żądanie dostępu JIT do maszyny Wirtualnej | *Przypisz te akcje użytkownika:*  W zakresie subskrypcji lub grupy zasobów, jest skojarzona z maszyną Wirtualną:  ```Microsoft.Security/locations/{the_location_of_the_VM}/jitNetworkAccessPolicies/initiate/action``` W zakresie subskrypcji lub grupy zasobów lub maszyny Wirtualnej: ```Microsoft.Compute/virtualMachines/read``` |


## <a name="configure-jit-on-a-vm"></a>Konfigurowanie JIT na maszynie Wirtualnej

Istnieją 3 sposoby, aby skonfigurować zasady JIT na maszynie Wirtualnej:

- [Skonfiguruj dostęp JIT w usłudze Azure Security Center](#jit-asc)
- [Skonfiguruj dostęp JIT w bloku maszyny Wirtualnej platformy Azure](#jit-vm)
- [Programowe Konfigurowanie zasad JIT na maszynie Wirtualnej](#jit-program)

## <a name="configure-jit-in-asc"></a>Konfigurowanie JIT w ASC

Od usługi ASC można skonfigurować zasady i żądanie dostępu JIT do maszyny Wirtualnej przy użyciu zasad JIT


### Konfigurowanie dostępu JIT do maszyny wirtualnej w ASC <a name="jit-asc"></a>

1. Otwórz pulpit nawigacyjny usługi **Security Center**.

2. W okienku po lewej stronie wybierz **Just-in-time dostęp do maszyny Wirtualnej**.

    ![Kafelek dostęp do maszyny Wirtualnej just-in-time](./media/security-center-just-in-time/just-in-time.png)

    **Just-in-time dostęp do maszyny Wirtualnej** zostanie otwarte okno.

      ![Włącz dostęp just in time](./media/security-center-just-in-time/enable-just-in-time.png)

    **Dostęp do maszyny Wirtualnej just-in-time** zawiera informacje o stanie maszyn wirtualnych:

    - **Skonfigurowane** — maszyny wirtualne, które zostały skonfigurowane do obsługi dostępu do maszyny Wirtualnej just-in-time. Dane prezentowane jest ostatniego tygodnia i liczbie dla każdej maszyny Wirtualnej zatwierdzonych wniosków, Data ostatniego dostępu i godziny oraz ostatniego użytkownika.
    - **Zaleca się** — maszyny wirtualne, które mogą obsługiwać dostęp do maszyny Wirtualnej w czasie, ale nie został skonfigurowany do. Firma Microsoft zaleca, włączyć kontrolę dostępu maszyny Wirtualnej just-in-time dla tych maszyn wirtualnych. 
    - **Brak zaleceń** — powody, dla których maszyna wirtualna może nie mieć zaleceń:
      - Brak sieciowej grupy zabezpieczeń — rozwiązanie just in time wymaga sieciowej grupy zabezpieczeń w miejscu.
      - Klasyczna maszyna wirtualna — dostęp do maszyny Wirtualnej just-in-time w usłudze Security Center obecnie obsługuje tylko maszyny wirtualne wdrożone za pośrednictwem usługi Azure Resource Manager. Wdrożenie klasyczne nie jest obsługiwana przez rozwiązanie just in time. 
      - Inne — maszyna wirtualna jest w tej kategorii, jeżeli rozwiązanie just in time jest wyłączone w zasadach zabezpieczeń subskrypcji lub grupy zasobów, czy maszyna wirtualna nie ma publicznego adresu IP i nie ma sieciową grupę zabezpieczeń w miejscu.

3. Wybierz **zalecane** kartę.

4. W obszarze **maszyny WIRTUALNEJ**, kliknij pozycję maszyny wirtualne, które chcesz włączyć. Spowoduje to umieszczenie znacznik wyboru obok maszyny Wirtualnej.

5. Kliknij przycisk **Włącz dostęp JIT na maszynach wirtualnych**.
   -. Ten blok zawiera porty domyślne zalecane przez usługę Azure Security Center:
      - 22 - PROTOKOŁU SSH
      - 3389 - RDP
      - 5985 - Usługa WinRM 
      - 5986 - Usługa WinRM
6. Można również skonfigurować porty niestandardowe:

      1. Kliknij pozycję **Add** (Dodaj). **Dodawanie konfiguracji portu** zostanie otwarte okno.
      2. Dla każdego portu możesz skonfigurować zarówno domyślne i niestandardowe, można dostosować następujące ustawienia:

    - **Typ protokołu**— protokół, który jest dozwolone na tym porcie, gdy żądanie zostanie zatwierdzone.
    - **Dozwolone źródłowe adresy IP**-zakresów IP, które są dozwolone dla tego portu, gdy żądanie zostanie zatwierdzone.
    - **Maksymalny czas żądania**— okno maksymalny czas, w którym można otworzyć określonego portu.

     3. Kliknij przycisk **OK**.

1. Kliknij pozycję **Zapisz**.

> [!NOTE]
>Po włączeniu dostępu do maszyny Wirtualnej JIT dla maszyny Wirtualnej usługi Azure Security Center tworzy "odmowa cały ruch przychodzący" dla wybranych portów w skojarzonej grupy zabezpieczeń sieci i zapory usługi Azure z nim. Jeśli inne reguły została utworzona na wybranych portach, następnie istniejące reguły pierwszeństwo przed nowe zasady "odmowa cały ruch przychodzący". Jeśli nie istnieją żadne istniejące reguły na wybranych portach, nowe zasady "odmowa cały ruch przychodzący" potrwać najwyższy priorytet w grupach zabezpieczeń sieci i zapory usługi Azure.


## <a name="request-jit-access-via-asc"></a>Żądanie dostępu JIT za pośrednictwem usługi ASC

Aby poprosić o dostęp do maszyny Wirtualnej za pośrednictwem usługi ASC:

1. W obszarze **Just in dostęp maszyny Wirtualnej w czasie**, wybierz opcję **skonfigurowane** kartę.

2. W obszarze **maszyny wirtualnej**, kliknij pozycję maszyny wirtualne, które chcesz, aby zażądać dostępu dla. Spowoduje to umieszczenie znacznik wyboru obok maszyny Wirtualnej.


    - Jest to ikona **szczegóły połączenia** kolumna wskazuje, czy JIT jest włączona w sieciowej grupie zabezpieczeń lub Zapory. Po włączeniu zarówno pojawia się tylko ikona zapory.

    - **Szczegóły połączenia** kolumna zawiera poprawne informacje wymagane do połączenia z maszyną Wirtualną, a także wskazuje otwarte porty.

      ![Żądanie dostępu just in time](./media/security-center-just-in-time/request-just-in-time-access.png)

3. Kliknij przycisk **żądania dostępu**. **Żądania dostępu** zostanie otwarte okno.

      ![Szczegóły JIT](./media/security-center-just-in-time/just-in-time-details.png)

4. W obszarze **żądania dostępu**źródłowych adresów IP, które port jest otwarty na i przedział czasu, dla którego numer portu będzie otwierać i dla każdej maszyny Wirtualnej, skonfiguruj porty, które chcesz otworzyć. Tylko będzie to możliwe zażądać dostępu do portów, które są skonfigurowane w zasadach just-in-time. Każdy port ma maksymalny czas dozwolony pochodną zasad just-in-time.

5. Kliknij przycisk **otworzyć porty**.

> [!NOTE]
> Jeśli użytkownik, który żąda dostępu jest za serwerem proxy, opcja **Mój adres IP** może nie działać. Może być konieczne zdefiniowanie pełny zakres adresów IP w organizacji.

## <a name="edit-a-jit-access-policy-via-asc"></a>Edytuj zasady dostępu JIT, za pośrednictwem usługi ASC

Możesz zmienić istniejące zasady just-in-time maszyny Wirtualnej przez dodanie i skonfigurowanie nowego portu do ochrony dla tej maszyny Wirtualnej lub zmieniając wszelkie inne ustawienia związane z portem już chronione.

Aby edytować istniejące zasady just-in-time maszyny wirtualnej:
1. W **skonfigurowane** , w obszarze **maszyn wirtualnych**, wybierz maszynę Wirtualną, dla której chcesz dodać port, klikając trzy kropki znajdujące się w wierszu dla tej maszyny Wirtualnej. 

1. Wybierz pozycję **Edit** (Edytuj).
1. W obszarze **Konfiguracja dostępu do maszyn wirtualnych JIT**, możesz edytować istniejące ustawienia portu już chronione lub dodać nowy port niestandardowy. 
  ![dostęp do maszyn wirtualnych JIT](./media/security-center-just-in-time/edit-policy.png)

## <a name="audit-jit-access-activity-in-asc"></a>Inspekcja aktywności dostęp JIT w ASC

Możesz uzyskać wgląd w działania maszyny Wirtualnej przy użyciu przeszukiwania dzienników. Aby wyświetlić dzienniki:

1. W obszarze **Just-in-time dostęp do maszyny Wirtualnej**, wybierz opcję **skonfigurowane** kartę.
2. W obszarze **maszyn wirtualnych**, wybierz maszynę Wirtualną, aby wyświetlić informacje o, klikając trzy kropki znajdujące się w wierszu dla tej maszyny Wirtualnej i wybierz **dziennika aktywności** w menu. **Dziennika aktywności** zostanie otwarty.

   ![Wybierz dziennik aktywności](./media/security-center-just-in-time/select-activity-log.png)

   **Dziennik aktywności** udostępnia widok filtrowany poprzednich operacji dla tej maszyny Wirtualnej wraz z godzina, Data i subskrypcji.

Informacje dziennika można pobrać, wybierając **kliknij tutaj, aby pobrać wszystkie elementy w formacie CSV**.

Zmodyfikuj filtry, a następnie kliknij przycisk **Zastosuj** do tworzenia wyszukiwania i dziennika.



## Skonfiguruj dostęp JIT w bloku maszyny Wirtualnej platformy Azure <a name="jit-vm"></a>

Dla Twojej wygody możesz połączyć z maszyną wirtualną przy użyciu JIT bezpośrednio z w ramach bloku maszyny Wirtualnej na platformie Azure.

### <a name="configure-jit-access-on-a-vm-via-the-azure-vm-blade"></a>Skonfiguruj dostęp JIT na maszynie Wirtualnej za pomocą bloku maszyny Wirtualnej platformy Azure

Ułatwia to dystrybucję dostępu just in time dla maszyn wirtualnych, można ustawić maszynę Wirtualną, aby zezwolić na dostęp tylko just in time do bezpośrednio z poziomu maszyny Wirtualnej.

1. W witrynie Azure portal wybierz **maszyn wirtualnych**.
2. Kliknij maszynę wirtualną, którą chcesz ograniczyć dostęp just in time do.
3. W menu, kliknij **konfiguracji**.
4. W obszarze **tylko w czasie dostępu** kliknij **Włącz zasady just-in-time**. 

Dzięki temu dostęp just in time do maszyny wirtualnej, używając następujących ustawień:

- Serwery Windows:
    - Port 3389 protokołu RDP
    - 3 godzin maksymalny dozwolony dostęp
    - Dozwolone źródłowe adresy IP jest ustawiony do dowolnej
- Serwery z systemem Linux:
    - Port 22 protokołu SSH
    - 3 godzin maksymalny dozwolony dostęp
    - Dozwolone źródłowe adresy IP jest ustawiony do dowolnej
     
Jeśli maszyna wirtualna ma już just-in-time włączone, po przejściu do strony konfiguracji będzie czy just in time jest włączone, i użyć linku, aby otworzyć zasady w usłudze Azure Security Center, aby wyświetlić i zmienić ustawienia.

![Konfiguracja JIT na maszynie wirtualnej](./media/security-center-just-in-time/jit-vm-config.png)

### <a name="request-jit-access-to-a-vm-via-the-azure-vm-blade"></a>Żądanie dostępu JIT do maszyny Wirtualnej za pomocą bloku maszyny Wirtualnej platformy Azure

W witrynie Azure portal podczas próby nawiązania połączenia z maszyną wirtualną Azure sprawdza w przypadku zasad dostępu just in time, skonfigurowane na tej maszynie Wirtualnej.

- Jeśli masz zasady JIT skonfigurowane na maszynie Wirtualnej, możesz kliknąć **żądania dostępu** umożliwiają dostęp zgodnie z zasadami JIT, ustaw dla maszyny Wirtualnej. 

  >![żądanie JIT](./media/security-center-just-in-time/jit-request.png)

  Żądania dostępu za pomocą następujących parametrów domyślnych:

  - **źródłowy adres IP**: "Any" (*) (nie można zmienić)
  - **zakres czasu**: 3 godziny (nie można zmienić)  <!--Isn't this set in the policy-->
  - **numer portu** RDP port 3389 dla Windows / port 22 dla systemu Linux (można go zmienić)

    > [!NOTE]
    > Po zatwierdzeniu żądania dla maszyny Wirtualnej chronione przez zaporę usługi Azure Security Center zapewnia użytkownikowi szczegóły prawidłowego połączenia (mapowanie portów z tabeli DNAT) do nawiązywania połączenia z maszyną wirtualną.

- Jeśli nie masz JIT skonfigurowane na maszynie Wirtualnej, użytkownik jest monitowany, aby skonfigurować zasady JIT go.

  ![wiersz JIT](./media/security-center-just-in-time/jit-prompt.png)

## Programowe Konfigurowanie zasad JIT na maszynie Wirtualnej  <a name="jit-program"></a>

Można skonfigurować i używać just-in-time za pośrednictwem interfejsów API REST i przy użyciu programu PowerShell.

## <a name="jit-vm-access-via-rest-apis"></a>Dostęp do maszyn wirtualnych JIT, za pośrednictwem interfejsów API REST

Funkcja dostępu just in time maszyny Wirtualnej może służyć za pośrednictwem interfejsu API usługi Azure Security Center. Możesz uzyskać informacje na temat skonfigurowanych maszyn wirtualnych, dodawać nowe, poproś o dostęp do maszyny Wirtualnej, a także bardziej, za pośrednictwem tego interfejsu API. Zobacz [zasad dostępu do sieci Jit](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies), aby dowiedzieć się więcej na temat interfejsu API REST just-in-time.

## <a name="jit-vm-access-via-powershell"></a>Dostęp do maszyn wirtualnych JIT, za pomocą programu PowerShell

Aby użyć rozwiązania dostępu just in time maszyny Wirtualnej, za pomocą programu PowerShell, użyj oficjalne Azure Security Center poleceń cmdlet programu PowerShell, a w szczególności `Set-AzJitNetworkAccessPolicy`.

Poniższy przykład ustawia zasady dostępu just in time maszyn wirtualnych w określonej maszyny Wirtualnej, a następnie ustawia następujące czynności:

1.  Zamykanie portów 22 i 3389.

2.  Ustaw maksymalny przedział czasu wynoszącego 3 godziny dla każdego, dzięki czemu można je otworzyć za zatwierdzone żądania.
3.  Umożliwia użytkownikowi, który żąda dostępu do kontrolowania źródłowych adresów IP i umożliwia użytkownikowi nawiązać pomyślnej sesji na żądanie zatwierdzone dostępu just in time.

W programie PowerShell, aby to zrobić, uruchom następujące polecenie:

1.  Przypisz zmienna, która zawiera zasady dostępu just in time maszyny Wirtualnej dla maszyny Wirtualnej:

        $JitPolicy = (@{
         id="/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
             number=22;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"},
             @{
             number=3389;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"})})

2.  Wstaw zasad maszyny Wirtualnej just-in-time maszyny Wirtualnej dostępu do tablicy:
    
        $JitPolicyArr=@($JitPolicy)

3.  Konfigurowanie zasad dostępu just in time maszyny Wirtualnej na wybranej maszynie Wirtualnej:
    
        Set-AzJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr 

#### <a name="request-access-to-a-vm-via-powershell"></a>Poproś o dostęp do maszyny Wirtualnej za pomocą programu PowerShell

W poniższym przykładzie widać just-in-time żądanie dostępu dla maszyny Wirtualnej do określonej maszyny Wirtualnej w port, który 22 jest wymagane do otwarcia dla określonego adresu IP i dla określonego przedziału czasu:

W programie PowerShell, uruchom następujące polecenie:
1.  Konfigurowanie właściwości maszyny Wirtualnej żądania dostępu

        $JitPolicyVm1 = (@{
          id="/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
           number=22;
           endTimeUtc="2018-09-17T17:00:00.3658798Z";
           allowedSourceAddressPrefix=@("IPV4ADDRESS")})})
2.  Wstaw parametry żądania dostępu do maszyny Wirtualnej w tablicy:

        $JitPolicyArr=@($JitPolicyVm1)
3.  Wyślij żądanie dostępu (Użyj Identyfikatora zasobu uzyskany w kroku 1)

        Start-AzJitNetworkAccessPolicy -ResourceId "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Security/locations/LOCATION/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr

Aby uzyskać więcej informacji zobacz dokumentację poleceń cmdlet programu PowerShell.

## <a name="next-steps"></a>Kolejne kroki
W tym artykule przedstawiono kontrolować dostęp do maszyny Wirtualnej jak just-in-time w usłudze Security Center ułatwia dostęp do usługi Azure virtual machines.

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń](tutorial-security-policy.md) — informacje o sposobie konfigurowania zasad zabezpieczeń dla subskrypcji platformy Azure i grup zasobów.
- [Zarządzanie zaleceniami dotyczącymi zabezpieczeń](security-center-recommendations.md) — Dowiedz się, w jaki sposób zalecenia ułatwiają ochronę zasobów platformy Azure.
- [Monitorowanie kondycji zabezpieczeń](security-center-monitoring.md) — informacje o sposobie monitorowania kondycji zasobów platformy Azure.
- [Reagowanie na alerty zabezpieczeń i zarządzanie nimi](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i reagować na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerskich](security-center-partner-solutions.md) — informacje o sposobie monitorowania stanu kondycji rozwiązań partnerskich.
- [Usługa Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
- [Blog Azure Security](https://blogs.msdn.microsoft.com/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.

