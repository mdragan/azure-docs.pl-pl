---
title: Zarządzanie zastrzeżeniami platformy Azure | Dokumentacja firmy Microsoft
description: Dowiedz się, jak zmienić zakres subskrypcji i zarządzanie dostępem dla platformy Azure rezerwacji.
ms.service: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: 9a5b200ffb9441b90875c7764786004ff5f1e8a1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66127143"
---
# <a name="manage-reservations-for-azure-resources"></a>Zarządzanie rezerwacji dla zasobów platformy Azure

Po kupujesz rezerwacji dla platformy Azure może być konieczne zastosowanie rezerwacji do innej subskrypcji, zmienić, kto może zarządzać rezerwacji lub zmienić zakres rezerwacji. Możesz również podzielić rezerwacji do dwóch rezerwacji do stosowania niektórych wystąpień, których używasz do innej subskrypcji.

Jeśli zakupiono Azure Reserved Virtual Machine Instances, możesz zmienić ustawienie optymalizacji dla rezerwacji. Rabat związany z rezerwacją można zastosować do maszyn wirtualnych w tej samej serii, lub możesz zarezerwować pojemnością centrum danych dla określonego rozmiaru maszyny Wirtualnej.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="reservation-order-and-reservation"></a>Zamówienie rezerwacji i rezerwacji

Po zakupie rezerwacji tworzone są dwa obiekty: **Zamówienie rezerwacji** i **rezerwacji**.

W chwili zakupu Zamówienie rezerwacji ma jedną rezerwację w nim. Akcje, takie jak podziału, scalanie, częściowego zwrotu lub exchange tworzenie nowych zastrzeżeń w ramach **zamówienia rezerwacji**.

Aby wyświetlić zamówienia rezerwacji, przejdź do **rezerwacje** > Wybierz rezerwacji, a następnie kliknij przycisk **identyfikator zamówienia rezerwacji**.

![Przykład przedstawiający identyfikator zamówienia rezerwacji szczegóły zamówienia rezerwacji ](./media/billing-manage-reserved-vm-instance/reservation-order-details.png)

Rezerwacja dziedziczy uprawnienia po jego zamówienia rezerwacji.

## <a name="change-the-reservation-scope"></a>Zmień zakres rezerwacji

 Rabat związany z rezerwacją ma zastosowanie do maszyn wirtualnych, baz danych SQL, Azure Cosmos DB i inne zasoby, które odpowiada rezerwacji i uruchamiać w zakresie rezerwacji. Kontekstu rozliczeń zależy od subskrypcji zakupu rezerwacji.

Aby zaktualizować zakresu rezerwacji:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Wybierz **wszystkich usług** > **rezerwacje**.
3. Wybierz rezerwacji.
4. Wybierz pozycję **Ustawienia** > **Konfiguracja**.
5. Zmień zakres.

W przypadku zmiany z udostępnionego przez pojedynczy zakres, można wybrać tylko te subskrypcje, których jesteś właścicielem. Wybrać można tylko subskrypcje w ramach tego samego kontekstu rozliczeń, co rezerwacja.

Zakres ma zastosowanie tylko do oferty płatności zgodnie z rzeczywistym użyciem MS-AZR-0003P lub MS-AZR-0023P, oferty Enterprise MS-AZR-0017P lub MS-AZR-0148P albo typów subskrypcji dostawcy rozwiązań w chmurze.

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Dodawanie lub zmienianie użytkowników, którzy mogą zarządzać rezerwacją

Możesz delegować Zarządzanie rezerwacji przez dodanie osoby do ról w zamówieniu rezerwacji lub rezerwacji. Domyślnie osoba, która umieszcza zamówienia rezerwacji i administrator konta mają Rola właściciela na zamówienie rezerwacji i rezerwacji.

Można zarządzać dostępem do zamówienia rezerwacji i zastrzeżenia, niezależnie od subskrypcji, które zawierają rabat związany z rezerwacją. Jeśli nadasz innej osobie uprawnienia do zarządzania zamówienia rezerwacji lub rezerwacji, go nie nadać im uprawnienia do zarządzania subskrypcją. Podobnie jeśli nadasz innej osobie uprawnienia do zarządzania subskrypcją w zakresie rezerwacji, go nie zapewnia im praw do zarządzania zamówienia rezerwacji lub rezerwacji.

Aby wykonać programu exchange lub refundacji, użytkownik musi mieć dostęp do zamówienia rezerwacji. Podczas nadawania uprawnień ktoś najlepiej przyznaj uprawnienia do zamówieniu rezerwacji nie rezerwacji.


Aby delegować zarządzanie dostępem dla rezerwacji:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Wybierz **wszystkich usług** > **rezerwacji** do listy rezerwacji, które mają dostęp do.
3. Wybierz zastrzeżenie, którego chcesz delegować dostęp do innych użytkowników.
4. Wybierz **kontrola dostępu (IAM)** .
5. Wybierz **Dodaj przypisanie roli** > **roli** > **właściciela**. Ewentualnie, jeśli chcesz nadać ograniczony dostęp, wybierz inną rolę.
6. Wpisz adres e-mail użytkownika, którego chcesz dodać jako właściciela.
7. Wybierz użytkownika, a następnie wybierz polecenie **Zapisz**.

## <a name="split-a-single-reservation-into-two-reservations"></a>Podziel pojedynczy rezerwacji do dwóch rezerwacji

 Po możesz kupić więcej niż jedno wystąpienie zasobów w ramach rezerwacji, możesz przypisać wystąpienia w ramach tej rezerwacji do różnych subskrypcji. Domyślnie wszystkie wystąpienia mają jeden zakres - albo pojedynczej subskrypcji lub udostępniony. Na przykład zakupiono 10 wystąpień rezerwacji i określony zakres do subskrypcji A. Teraz można zmienić zakresu dla rezerwacji 7 do subskrypcji A i pozostałych 3 do subskrypcji B. podział rezerwacji umożliwia dystrybucji wystąpienia dla szczegółowego zakres zarządzania. Wybierając zakres udostępniony, można uprościć alokacji do subskrypcji. Jednak do celów zarządzania lub budżetowania koszt, można przydzielić ilości do określonej subskrypcji.

 Możesz podzielić rezerwacji do dwóch rezerwacji do programu PowerShell, interfejsu wiersza polecenia lub przy użyciu interfejsu API.

### <a name="split-a-reservation-by-using-powershell"></a>Podziel rezerwacji przy użyciu programu PowerShell

1. Pobierz identyfikator zamówienia rezerwacji, uruchamiając następujące polecenie:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzReservationOrder
    ```

2. Pobierz szczegóły rezerwacji:

    ```powershell
    Get-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. Podziel rezerwacji na dwa i dystrybucja wystąpień:

    ```powershell
    # Split the reservation. The sum of the reservations, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
4. Można zaktualizować zakresu, uruchamiając następujące polecenie:

    ```powershell
    Update-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="cancellations-and-exchanges"></a>Anulowanie i wymiany

W zależności od typu rezerwację można anulować lub wymiany rezerwacji. Aby uzyskać więcej informacji zobacz anulowania i wymiany sekcje w następujących tematach:

- [Prepay for Virtual Machines with Azure Reserved VM Instances (Opłacanie maszyn wirtualnych z góry przy użyciu usługi Azure Reserved VM Instances)](..//virtual-machines/windows/prepay-reserved-vm-instances.md#cancellations-and-exchanges)
- [Prepay for SUSE software plans from Azure Reservations (Opłacanie planów oprogramowania SUSE z góry z poziomu usługi Azure Reservations)](../virtual-machines/linux/prepay-suse-software-charges.md#cancellation-and-exchanges-not-allowed)
- [Prepay for SQL Database compute resources with Azure SQL Database reserved capacity (Opłacanie zasobów obliczeniowych usługi SQL Database z góry przy użyciu zarezerwowanej pojemności usługi Azure SQL Database)](../sql-database/sql-database-reserved-capacity.md#cancellations-and-exchanges)

## <a name="change-optimize-setting-for-reserved-vm-instances"></a>Zmiana zoptymalizować ustawienie zarezerwowanych wystąpień maszyn wirtualnych

 W przypadku dokonywania zakupu wystąpienia zarezerwowanego maszyny Wirtualnej, możesz wybrać elastyczność rozmiar wystąpienia lub priorytet pojemności. Elastyczność rozmiaru wystąpienia ma zastosowanie rabatu związanego z rezerwacją do innych maszyn wirtualnych w tym samym [grupie rozmiarów maszyn wirtualnych](https://aka.ms/RIVMGroups). Priorytet pojemności priorytet pojemności centrum danych wdrożeń. Ta opcja zapewnia dodatkową pewność co do możliwości uruchomienia wystąpienia maszyny Wirtualnej, gdy ich potrzebujesz.

Domyślnie gdy zakres rezerwacji jest udostępniony, elastyczność rozmiaru wystąpienia są. Możliwości Centrum danych nie są uszeregowane według priorytetów dla wdrożeń maszyn wirtualnych.

Dla zastrzeżenia, których zakres jest pojedynczym można zoptymalizować Rezerwacja priorytet pojemności zamiast elastyczność rozmiaru wystąpienia maszyny Wirtualnej.

Aby zaktualizować ustawienia optymalizacji dla rezerwacji:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Wybierz **wszystkich usług** > **rezerwacje**.
3. Wybierz rezerwacji.
4. Wybierz pozycję **Ustawienia** > **Konfiguracja**.
5. Zmiana **Optymalizuj pod kątem** ustawienie.

## <a name="need-help-contact-us"></a>Potrzebujesz pomocy? Skontaktuj się z nami.

Jeśli masz pytania lub potrzebujesz pomocy, [Utwórz żądanie obsługi](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej na temat rezerwacji Azure, zobacz następujące artykuły:

- [Co to są rezerwacji dla platformy Azure?](billing-save-compute-costs-reservations.md)

Kup plan usługi:
- [Prepay for Virtual Machines with Azure Reserved VM Instances (Opłacanie maszyn wirtualnych z góry przy użyciu usługi Azure Reserved VM Instances)](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Prepay for SQL Database compute resources with Azure SQL Database reserved capacity (Opłacanie zasobów obliczeniowych usługi SQL Database z góry przy użyciu zarezerwowanej pojemności usługi Azure SQL Database)](../sql-database/sql-database-reserved-capacity.md)
- [Zapłać z góry za zasoby usługi Azure Cosmos DB z pojemnością zarezerwowane usługi Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md)

Kup plan oprogramowania:
- [Zapłać z góry za plany oprogramowania Red Hat z listy zastrzeżeń platformy Azure](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Prepay for SUSE software plans from Azure Reservations (Opłacanie planów oprogramowania SUSE z góry z poziomu usługi Azure Reservations)](../virtual-machines/linux/prepay-suse-software-charges.md)

Poznaj rabat w wysokości i użycia:
- [Zrozumienie, jak jest stosowany rabat związany z rezerwacją maszyny Wirtualnej](billing-understand-vm-reservation-charges.md)
- [Zrozumienie, jak jest stosowany rabat w wysokości Red Hat Enterprise Linux plan oprogramowania](../billing/billing-understand-rhel-reservation-charges.md)
- [Zrozumienie, jak jest stosowany rabat plan oprogramowania SUSE Linux Enterprise](../billing/billing-understand-suse-reservation-charges.md)
- [Zrozumienie sposobu stosowania rabatów rezerwacji](billing-understand-reservation-charges.md)
- [Opis zastrzeżenia dla Twojej subskrypcji zgodnie z rzeczywistym użyciem](billing-understand-reserved-instance-usage.md)
- [Opis zastrzeżenia dla Twojej rejestracji Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Koszty oprogramowania Windows nie jest dołączony do rezerwacji](billing-reserved-instance-windows-software-costs.md)
