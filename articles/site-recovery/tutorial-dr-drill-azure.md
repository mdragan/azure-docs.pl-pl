---
title: Uruchamianie próbnego odzyskiwania po awarii maszyn lokalnych na platformę Azure przy użyciu usługi Azure Site Recovery | Microsoft Docs
description: Informacje o uruchamianiu próbnego odzyskiwania po awarii ze środowiska lokalnego na platformę Azure przy użyciu usługi Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 4ed66e3a0237eced852c806e78a8af6bdf8d8579
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417827"
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Uruchamianie próbnego odzyskiwania na platformie Azure

W tym artykule opisano sposób uruchamiania próbnego odzyskiwania po awarii dla maszyny lokalnej na platformę Azure dzięki [usługi Azure Site Recovery](site-recovery-overview.md) usługi. Próba pozwala zweryfikować działanie strategii replikacji bez ryzyka utraty danych.


Jest to czwarty samouczek z serii, która pokazuje, jak skonfigurować odzyskiwanie po awarii na platformie Azure dla maszyn lokalnych.

W tym samouczku pokazano, jak:

> [!div class="checklist"]
> * Konfigurowanie sieci izolowanej do testowego przełączania w tryb failover
> * Przygotowywanie do połączenia z maszyną wirtualną platformy Azure po przejściu do trybu failover
> * Uruchom testowanie trybu failover dla pojedynczej maszyny.

> [!NOTE]
> W samouczkach pokazano to najprostsza ścieżka wdrażania scenariusza. Jeśli to możliwe, używają opcji domyślnych i nie przedstawiają wszystkich możliwych ustawień i ścieżek. Jeśli chcesz poznać kroki testowania odzyskiwania po awarii, bardziej szczegółowo [zapoznaj się z tym artykułem](site-recovery-test-failover-to-azure.md).

## <a name="before-you-start"></a>Przed rozpoczęciem

Wykonaj poprzednich samouczków:

1. Upewnij się, że masz [Konfigurowanie platformy Azure](tutorial-prepare-azure.md) do odzyskiwania po awarii lokalnych maszyn wirtualnych VMware, maszyny wirtualne funkcji Hyper-V i maszyn fizycznych do platformy Azure.
2. Przygotowywanie lokalnej [VMware](vmware-azure-tutorial-prepare-on-premises.md) lub [funkcji Hyper-V](hyper-v-prepare-on-premises-tutorial.md) środowiska na potrzeby odzyskiwania po awarii. Jeśli podczas konfigurowania odzyskiwania po awarii dla serwerów fizycznych, zapoznaj się z [macierz obsługi](vmware-physical-secondary-support-matrix.md).
3. Konfigurowanie odzyskiwania po awarii dla [maszyny wirtualne VMware](vmware-azure-tutorial.md), [maszyn wirtualnych funkcji Hyper-V](hyper-v-azure-tutorial.md), lub [maszyn fizycznych](physical-azure-disaster-recovery.md).
 

## <a name="verify-vm-properties"></a>Sprawdzanie właściwości maszyn wirtualnych

Przed uruchomieniem testowego przełączenia w tryb failover sprawdź właściwości maszyny wirtualnej i upewnij się, że [maszyna wirtualna Hyper-V](hyper-v-azure-support-matrix.md#replicated-vms) lub [maszyna wirtualna VMware](vmware-physical-azure-support-matrix.md#replicated-machines) spełnia wymagania platformy Azure.

1. W obszarze **Chronione elementy** kliknij kolejno pozycje **Zreplikowane elementy** > i Maszyna wirtualna.
2. Okienko **Zreplikowany element** zawiera podsumowanie informacji na temat maszyny wirtualnej, jej kondycję oraz najnowsze dostępne punkty odzyskiwania. Kliknij przycisk **Właściwości**, aby wyświetlić więcej szczegółów.
3. W obszarze **Obliczenia i sieć** można zmodyfikować nazwę platformy Azure, grupę zasobów, rozmiar docelowy, zestaw dostępności i ustawienia dysku zarządzanego.
4. Możesz wyświetlać i modyfikować ustawienia sieciowe, w tym sieć/podsieć, w której zlokalizowana będzie maszyna wirtualna na platformie Azure po wejściu w tryb failover, oraz adres IP, który będzie do niej przypisany.
5. W obszarze **Dyski** można znaleźć informacje dotyczące systemu operacyjnego i dysków danych maszyny wirtualnej.

## <a name="create-a-network-for-test-failover"></a>Tworzenie sieci do testowania pracy w trybie failover

Do testowania pracy w trybie failover zalecamy wybór sieci izolowanej od sieci produkcyjnej lokacji odzyskiwania, określonej w obszarze ustawień **Obliczenia i sieć** poszczególnych maszyn wirtualnych. Tworzone sieci wirtualne platformy Azure są domyślnie izolowane od innych sieci. Sieć testowa powinna odzwierciedlać sieć produkcyjną:

- Sieć testowa powinna mieć taką samą liczbę podsieci jak sieć produkcyjna. Podsieci powinny mieć takie same nazwy.
- W sieci testowej powinien być używany taki sam zakres adresów IP.
- Zaktualizuj system DNS sieci testowej, używając adresu IP określonego dla maszyny wirtualnej systemu DNS w obszarze ustawień **Obliczenia i sieć**. Aby uzyskać więcej informacji, zobacz sekcję [Zagadnienia dotyczące testowania trybu failover dla usługi Active Directory](site-recovery-active-directory.md#test-failover-considerations).

## <a name="run-a-test-failover-for-a-single-vm"></a>Uruchamianie testu trybu failover dla pojedynczej maszyny wirtualnej

Po uruchomieniu próby przejścia w tryb failover wykonywane są następujące operacje:

1. Uruchamiane jest sprawdzanie wymagań wstępnych, aby upewnić się, że zostały spełnione wszystkie warunki przejścia w tryb failover.
2. Tryb failover przetwarza dane, aby umożliwić utworzenie maszyny wirtualnej platformy Azure. Jeśli wybierzesz najnowszy punkt odzyskiwania, punkt odzyskiwania zostanie utworzony na podstawie danych.
3. Tworzona jest maszyna wirtualna platformy Azure przy użyciu danych przetworzonych w poprzednim kroku.

Uruchom testowe przełączenie w tryb failover w następujący sposób:

1. W obszarze **Ustawienia** > **Elementy zreplikowane** kliknij pozycje Maszyna wirtualna > **+Testowe przełączenie w tryb failover**.
2. Na potrzeby tego samouczka wybierz punkt odzyskiwania **Najnowszy przetworzony**. Spowoduje to przełączenie maszyny wirtualnej w tryb failover do najnowszego dostępnego punktu w czasie. Wyświetlana jest sygnatura czasowa. Ta opcja zapewnia niską wartość celu czasu odzyskiwania (RTO, Recovery Time Objective), ponieważ nie trzeba poświęcać czasu na przetwarzanie danych.
3. W obszarze **Test pracy w trybie failover** wybierz sieć platformy Azure, z którą maszyny wirtualne platformy Azure zostaną połączone po przejściu w tryb failover.
4. Kliknij przycisk **OK**, aby rozpocząć tryb failover. Możesz śledzić postęp, klikając maszynę wirtualną i otwierając jej właściwości. Możesz też kliknąć zadanie **Testowe przełączenie w tryb failover** po wybraniu pozycji nazwa magazynu > **Ustawienia** > **Zadania** >
   **Zadania usługi Site Recovery**.
5. Po zakończeniu trybu failover w obszarze Azure Portal > **Maszyny wirtualne** będzie widoczna replika maszyny wirtualnej platformy Azure. Upewnij się, że maszyna wirtualna ma prawidłowy rozmiar, jest połączona z odpowiednią siecią i jest uruchomiona.
6. Powinno być teraz możliwe nawiązanie połączenia ze zreplikowaną maszyną wirtualną na platformie Azure.
7. Aby usunąć maszyny wirtualne platformy Azure utworzone podczas testowego przełączania w tryb failover, kliknij pozycję **Wyczyść test pracy w trybie failover** na maszynie wirtualnej. W obszarze **Uwagi** zarejestruj i zapisz wszelkie obserwacje związane z testem pracy w trybie failover.

W niektórych scenariuszach tryb failover wymaga dodatkowego przetwarzania, którego przeprowadzenie zajmuje około 8–10 minut. Dłuższy czas testowego przełączania w tryb failover może występować w przypadku maszyn VMware z systemem Linux, maszyn wirtualnych VMware, które nie mają włączonej usługi DHCP, oraz maszyn wirtualnych VMware, które nie mają następujących sterowników rozruchowych: storvsc, vmbus, storflt, intelide, atapi.

## <a name="connect-after-failover"></a>Połącz po włączeniu trybu failover

Jeśli chcesz nawiązać połączenie przy użyciu protokołu RDP/SSH po przejściu w tryb failover maszyn wirtualnych platformy Azure [przygotować się do połączenia](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover). Jeśli napotkasz jakiekolwiek problemy łączności po włączeniu trybu failover, wykonaj [Rozwiązywanie problemów z](site-recovery-failover-to-azure-troubleshoot.md) przewodnik.

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Uruchamianie trybu failover i powrotu po awarii dla maszyn wirtualnych VMware](vmware-azure-tutorial-failover-failback.md).
> [Uruchamianie trybu failover i powrotu po awarii dla maszyn wirtualnych funkcji Hyper-V](hyper-v-azure-failover-failback-tutorial.md).
> [Uruchamianie trybu failover i powrotu po awarii dla maszyn fizycznych](physical-to-azure-failover-failback.md)
