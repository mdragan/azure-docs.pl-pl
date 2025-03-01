---
title: 'Samouczek: Wdrażanie i konfigurowanie usługi Azure Firewall przy użyciu witryny Azure Portal'
description: W ramach tego samouczka dowiesz się, jak wdrożyć i skonfigurować usługę Azure Firewall przy użyciu witryny Azure Portal.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 4/9/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 9d7b9673101ed3b6ff85a9981ba061bc870762b1
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405681"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Samouczek: Wdrażanie i konfigurowanie usługi Azure Firewall przy użyciu witryny Azure Portal

Kontrolowanie dostępu do sieciowego ruchu wychodzącego jest ważną częścią ogólnego planu zabezpieczeń sieci. Na przykład możesz ograniczyć dostęp do witryn sieci web. Lub możesz chcieć ograniczyć wychodzące adresy IP i portów, które mogą być udostępniane.

Jednym ze sposobów kontrolowania dostępu do sieciowego ruchu wychodzącego z podsieci platformy Azure jest użycie usługi Azure Firewall. Za pomocą usługi Azure Firewall można skonfigurować następujące reguły:

* Reguły aplikacji, które definiują w pełni kwalifikowane nazwy domen (FQDN), do których można uzyskać dostęp z podsieci.
* Reguły sieci, które definiują adres źródłowy, protokół, port docelowy i adres docelowy.

Ruch sieciowy podlega skonfigurowanym regułom zapory podczas kierowania ruchu sieciowego do zapory jako bramy domyślnej podsieci.

W tym samouczku utworzysz uproszczoną pojedynczą sieć wirtualną z trzema podsieciami w celu łatwego wdrażania. W przypadku wdrożeń produkcyjnych [modelu topologi gwiaździstej](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) jest to zalecane, gdy Zapora jest we własnej sieci wirtualnej. Obciążenia są serwery w wirtualnych sieciach równorzędnych, w tym samym regionie przy użyciu co najmniej jednej podsieci.

* **AzureFirewallSubnet** — w tej podsieci znajduje się zapora.
* **Workload-SN** — w tej podsieci znajduje się serwer obciążeń. Ruch sieciowy tej podsieci przechodzi przez zaporę.
* **Jump-SN** — w tej podsieci znajduje się serwer przesiadkowy. Serwer przesiadkowy ma publiczny adres IP, z którym możesz nawiązać połączenie przy użyciu pulpitu zdalnego. Stamtąd możesz połączyć się (przy użyciu innego pulpitu zdalnego) z serwerem obciążeń.

![Infrastruktura sieci samouczka](media/tutorial-firewall-rules-portal/Tutorial_network.png)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie testowego środowiska sieciowego
> * Wdrażanie zapory
> * Tworzenie trasy domyślnej
> * Skonfiguruj reguły aplikacji, aby umożliwić dostęp do www.google.com
> * Konfigurowanie reguły sieci w celu umożliwienia dostępu do zewnętrznych serwerów DNS
> * Testowanie zapory

Jeśli chcesz, możesz wykonać kroki tego samouczka przy użyciu [programu Azure PowerShell](deploy-ps.md).

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="set-up-the-network"></a>Konfigurowanie sieci

Najpierw utwórz grupę zasobów zawierającą zasoby wymagane do wdrożenia zapory. Następnie utwórz sieć wirtualną, podsieci i serwery do obsługi testowania.

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Grupa zasobów zawiera wszystkie zasoby wymagane w tym samouczku.

1. Zaloguj się do witryny Azure Portal pod adresem [https://portal.azure.com](https://portal.azure.com).
2. W witrynie Azure portal głównej wybierz **grup zasobów** > **Dodaj**.
3. W polu **Nazwa grupy zasobów** wpisz **Test-FW-RG**.
4. W polu **Subskrypcja** wybierz subskrypcję.
5. W polu **Lokalizacja grupy zasobów** wybierz lokalizację. Wszystkie kolejne zasoby, które utworzysz, muszą znajdować się w tej samej lokalizacji.
6. Wybierz pozycję **Utwórz**.

### <a name="create-a-vnet"></a>Tworzenie sieci wirtualnej

Ta sieć wirtualna będzie zawierać trzy podsieci.

1. Z platformy Azure strony głównej portalu, wybierz **Utwórz zasób**.
2. W obszarze **sieć**, wybierz opcję **sieć wirtualna**.
4. W polu **Nazwa** wpisz wartość **Test-FW-VN**.
5. W polu **Przestrzeń adresowa** wpisz wartość **10.0.0.0/16**.
6. W polu **Subskrypcja** wybierz subskrypcję.
7. Aby uzyskać **grupy zasobów**, wybierz opcję **Test-PD-RG**.
8. W polu **Lokalizacja** wybierz tę samą lokalizację, która była używana poprzednio.
9. W obszarze **Podsieci** w polu **Nazwa** wpisz wartość **AzureFirewallSubnet**. Zapora będzie znajdować się w tej podsieci, a nazwą podsieci **musi** być AzureFirewallSubnet.
10. W polu **Zakres adresów** wpisz wartość **10.0.1.0/24**.
11. Zaakceptuj ustawienia domyślne, a następnie wybierz **Utwórz**.

> [!NOTE]
> Minimalny rozmiar podsieci AzureFirewallSubnet to /26.

### <a name="create-additional-subnets"></a>Tworzenie dodatkowych podsieci

Następnie należy utworzyć podsieci dla serwera przesiadkowego oraz podsieci dla serwerów obciążeń.

1. W witrynie Azure portal głównej wybierz **grup zasobów** > **Test-PD-RG**.
2. Wybierz **Test-PD-VN** sieci wirtualnej.
3. Wybierz **podsieci** > **+ podsieć**.
4. W polu **Nazwa** wpisz wartość **Workload-SN**.
5. W polu **Zakres adresów** wpisz wartość **10.0.2.0/24**.
6. Kliknij przycisk **OK**.

Utwórz kolejną podsieć z nazwą **Jump-SN** i zakresem adresów **10.0.3.0/24**.

### <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Teraz utwórz maszyny wirtualne przesiadkową i obciążeń, a następnie umieść je w odpowiednich podsieciach.

1. W witrynie Azure Portal wybierz pozycję **Utwórz zasób**.
2. Wybierz pozycję **Compute**, a następnie z listy Polecane wybierz pozycję **Windows Server 2016 Datacenter**.
3. Wprowadź poniższe wartości dla maszyny wirtualnej:

   |Ustawienie  |Wartość  |
   |---------|---------|
   |Grupa zasobów     |**Test PD RG**|
   |Nazwa maszyny wirtualnej     |**Szybkie SRV**|
   |Obszar     |Takie same jak poprzednie|
   |Nazwa użytkownika administratora     |**azureuser**|
   |Hasło     |**Azure123456!**|

4. W obszarze **reguły portów wejściowych**, dla **publiczne porty wejściowe**, wybierz opcję **Zezwalaj na wybranych portach**.
5. W obszarze **Wybierz porty wejściowe** wybierz pozycję **RDP (3389)**.

6. Zaakceptuj wartości domyślne i wybierz pozycję **dalej: Dyski**.
7. Zaakceptuj wartości domyślne dysku i wybierz pozycję **dalej: Sieć**.
8. Upewnij się, że wybrano sieć wirtualną **Test-FW-VN** i podsieć **Jump-SN**.
9. Aby uzyskać **publiczny adres IP**, Zaakceptuj nowy publiczny adres ip adres nazwę domyślną (Srv — szybkie ip).
11. Zaakceptuj wartości domyślne i wybierz pozycję **dalej: Zarządzanie**.
12. Wybierz **poza** Aby wyłączyć diagnostykę rozruchu. Zaakceptuj wartości domyślne i wybierz pozycję **przeglądu + Utwórz**.
13. Przejrzyj ustawienia na stronie podsumowania, a następnie wybierz **Utwórz**.

Skorzystaj z informacji w poniższej tabeli, aby skonfigurować kolejną maszynę wirtualną o nazwie **pracy Srv**. Pozostała część konfiguracji jest taka sama jak w przypadku maszyny wirtualnej Srv-Jump.

|Ustawienie  |Wartość  |
|---------|---------|
|Podsieć|**Workload-SN**|
|Publiczny adres IP|**Brak**|
|Publiczne porty wejściowe|**Brak**|

## <a name="deploy-the-firewall"></a>Wdrażanie zapory

Wdróż zaporę w sieci wirtualnej.

1. Na stronie głównej portalu wybierz **Utwórz zasób**.
2. Typ **zapory** w polu wyszukiwania, a następnie naciśnij klawisz **Enter**.
3. Wybierz **zapory** , a następnie wybierz **Utwórz**.
4. Na stronie **Tworzenie zapory** strony skorzystaj z poniższej tabeli, aby skonfigurować zaporę:

   |Ustawienie  |Wartość  |
   |---------|---------|
   |Subskrypcja     |\<Twoja subskrypcja\>|
   |Grupa zasobów     |**Test PD RG** |
   |Name (Nazwa)     |**Test-FW01**|
   |Lokalizacja     |Wybierz tę samą lokalizację, której użyto poprzednio|
   |Wybierz sieć wirtualną     |**Użyj istniejącej**: **Test-FW-VN**|
   |Publiczny adres IP     |**Utwórz nową**. Publiczny adres IP musi mieć typ Standardowa jednostka SKU.|

5. Wybierz pozycję **Przegląd + utwórz**.
6. Przejrzyj podsumowanie, a następnie wybierz **Utwórz** do utworzenia zapory.

   Wdrożenie potrwa klika minut.
7. Po zakończeniu wdrażania przejdź do **Test-PD-RG** zasobu, grupy i wybierz **FW01 testu** zapory.
8. Zanotuj prywatny adres IP. Użyjesz go później podczas tworzenia trasy domyślnej.

## <a name="create-a-default-route"></a>Tworzenie trasy domyślnej

Na potrzeby podsieci **Workload-SN** skonfiguruj trasę domyślną ruchu wychodzącego, aby przechodziła przez zaporę.

1. Z platformy Azure strony głównej portalu, wybierz **wszystkich usług**.
2. W obszarze **sieć**, wybierz opcję **tabele tras**.
3. Wybierz pozycję **Dodaj**.
4. W polu **Nazwa** wpisz wartość **Firewall-route**.
5. W polu **Subskrypcja** wybierz subskrypcję.
6. Aby uzyskać **grupy zasobów**, wybierz opcję **Test-PD-RG**.
7. W polu **Lokalizacja** wybierz tę samą lokalizację, która była używana poprzednio.
8. Wybierz pozycję **Utwórz**.
9. Wybierz **Odśwież**, a następnie wybierz pozycję **trasy zapory** tabeli tras.
10. Wybierz **podsieci** , a następnie wybierz **skojarzyć**.
11. Wybierz **sieć wirtualna** > **Test-PD-VN**.
12. Aby uzyskać **podsieci**, wybierz opcję **SN obciążenia**. Upewnij się, że wybrano tylko **obciążenia-SN** podsieci dla tej trasy, w przeciwnym razie Zapora nie będzie działać poprawnie.

13. Kliknij przycisk **OK**.
14. Wybierz **trasy** , a następnie wybierz **Dodaj**.
15. Dla **nazwy trasy**, typ **dg PD**.
16. W polu **Prefiks adresu** wpisz wartość **0.0.0.0/0**.
17. W obszarze **Typ następnego skoku** wybierz pozycję **Urządzenie wirtualne**.

    Usługa Azure Firewall to w rzeczywistości usługa zarządzana, ale urządzenie wirtualne działa w tej sytuacji.
18. W polu **Adres następnego skoku** wpisz wcześniej zanotowany prywatny adres IP zapory.
19. Kliknij przycisk **OK**.

## <a name="configure-an-application-rule"></a>Konfigurowanie reguły aplikacji

Jest to reguła aplikacji, która umożliwia dostęp ruchu wychodzącego do www.google.com.

1. Otwórz **Test-PD-RG**i wybierz **FW01 testu** zapory.
2. Na **FW01 testu** w obszarze **ustawienia**, wybierz opcję **reguły**.
3. Wybierz **kolekcji reguł aplikacji** kartę.
4. Wybierz **dodać kolekcję reguł aplikacji**.
5. W polu **Nazwa** wpisz wartość **App-Coll01**.
6. W polu **Priorytet** wpisz wartość **200**.
7. W polu **Akcja** wybierz opcję **Zezwalaj**.
8. W obszarze **reguły**, **nazw FQDN docelowego**, dla **nazwa**, typ **dozwolonych przez firmę Google**.
9. W polu **Adresy źródłowe** wpisz wartość **10.0.2.0/24**.
10. W polu **Protocol:port** wpisz wartość **http, https**.
11. Aby uzyskać **nazw FQDN docelowego**, typ **www.google.com**
12. Wybierz pozycję **Dodaj**.

Usługa Azure Firewall zawiera wbudowaną kolekcję reguł dla nazw FQDN infrastruktury, które domyślnie są dozwolone. Te nazwy FQDN są specyficzne dla platformy i nie można ich używać do innych celów. Aby uzyskać więcej informacji, zobacz [Infrastrukturalne nazwy FQDN](infrastructure-fqdns.md).

## <a name="configure-a-network-rule"></a>Konfigurowanie reguły sieci

Jest to reguła sieci, która umożliwia ruchowi wychodzącemu dostęp do dwóch adresów IP na porcie 53 (DNS).

1. Wybierz **sieci kolekcji reguł** kartę.
2. Wybierz **dodać kolekcję reguł sieci**.
3. W polu **Nazwa** wpisz wartość **Net-Coll01**.
4. W polu **Priorytet** wpisz wartość **200**.
5. W polu **Akcja** wybierz opcję **Zezwalaj**.

6. W obszarze **reguły**, dla **nazwa**, typ **DNS Zezwalaj**.
7. W polu **Protokół** wybierz **UDP**.
8. W polu **Adresy źródłowe** wpisz wartość **10.0.2.0/24**.
9. W polu Adres docelowy wpisz wartość **209.244.0.3,209.244.0.4**
10. W polu **Porty docelowe** wpisz wartość **53**.
11. Wybierz pozycję **Dodaj**.

### <a name="change-the-primary-and-secondary-dns-address-for-the-srv-work-network-interface"></a>Zmienianie podstawowego i pomocniczego adresu DNS dla interfejsu sieciowego **Srv-Work**

Do celów testowych w ramach tego samouczka, skonfiguruj adresy DNS serwera podstawowego i pomocniczego. Nie jest ona ogólnych wymagań zapory usługi Azure.

1. W witrynie Azure Portal otwórz grupę zasobów **Test-FW-RG**.
2. Wybierz interfejs sieciowy dla **pracy Srv** maszyny wirtualnej.
3. W obszarze **ustawienia**, wybierz opcję **serwerów DNS**.
4. W obszarze **serwerów DNS**, wybierz opcję **niestandardowe**.
5. Wpisz wartość **209.244.0.3** w polu tekstowym **Dodaj serwer DNS**, a następnie wpisz wartość **209.244.0.4** w następnym polu tekstowym.
6. Wybierz pozycję **Zapisz**.
7. Uruchom ponownie maszynę wirtualną **Srv-Work**.

## <a name="test-the-firewall"></a>Testowanie zapory

Teraz należy sprawdzić, zapory, aby upewnić się, że działa zgodnie z oczekiwaniami.

1. W witrynie Azure Portal sprawdź ustawienia sieci dla maszyny wirtualnej **Srv-Work** i zanotuj prywatny adres IP.
2. Pulpit zdalny, aby połączyć **szybkie Srv** maszynę wirtualną i zaloguj się. Z tego miejsca Otwórz Podłączanie pulpitu zdalnego z **pracy Srv** prywatny adres IP.

3. Otwórz program Internet Explorer i przejdź do https://www.google.com.
4. Wybierz **OK** > **Zamknij** na alerty zabezpieczeń programu Internet Explorer.

   Powinna zostać wyświetlona strona główna firmy Google.

5. Przejdź do https://www.microsoft.com.

   Dostęp powinien zostać zablokowany przez zaporę.

Teraz gdy masz pewność, czy działają reguły zapory:

* Możesz przejść do jednej z dozwolonych nazw FQDN, ale nie do innych.
* Możesz rozpoznać nazwy DNS przy użyciu skonfigurowanego zewnętrznego serwera DNS.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Możesz zachować zasoby zapory na potrzeby kolejnego samouczka, a jeśli nie będą już potrzebne, możesz usunąć grupę zasobów **Test-FW-RG**, aby usunąć wszystkie zasoby związane z zaporą.

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Samouczek: Monitorowanie dzienników usługi Azure Firewall](./tutorial-diagnostics.md)
