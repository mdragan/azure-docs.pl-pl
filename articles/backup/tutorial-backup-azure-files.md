---
title: Tworzenie kopii zapasowej udziałów plików usługi Azure Files za pomocą usługi Kopia zapasowa Azure
description: W tym samouczku wyjaśniono sposób tworzenia kopii zapasowej udziałów plików platformy Azure.
services: backup
author: dcurwin
ms.author: dacurwin
ms.date: 06/10/2019
ms.topic: tutorial
ms.service: backup
manager: carmonm
ms.openlocfilehash: 474d5454e30c35d3f3ccf4ea994093ef47bd6ceb
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275992"
---
# <a name="back-up-azure-file-shares"></a>Tworzenie kopii zapasowej udziałów plików platformy Azure
W tym artykule opisano sposób tworzenia kopii zapasowej i przywracania [udziałów plików platformy Azure](../storage/files/storage-files-introduction.md) przy użyciu witryny Azure Portal.

Niniejszy przewodnik zawiera informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Konfigurowanie magazynu usługi Recovery Services na potrzeby tworzenia kopii zapasowej w usłudze Azure Files
> * Uruchomianie zadania tworzenia kopii zapasowej na żądanie w celu tworzenia punktu przywracania


## <a name="prerequisites"></a>Wymagania wstępne
Przed utworzeniem kopii zapasowej udziału plików platformy Azure sprawdź, czy znajduje się on na [koncie magazynu jednego z obsługiwanych typów](tutorial-backup-azure-files.md#limitations-for-azure-file-share-backup-during-preview). Jeśli tak, oznacza to, że możesz chronić swoje udziały plików.

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Ograniczenia dotyczące kopii zapasowej udziału plików platformy Azure w wersji zapoznawczej
Funkcja tworzenia kopii zapasowych udziałów plików platformy Azure jest dostępna w wersji zapoznawczej. Obsługiwane są udziały plików platformy Azure na kontach magazynu ogólnego przeznaczenia w wersji 1 i 2. Następujące scenariusze tworzenia kopii zapasowej nie są obsługiwane w przypadku udziałów plików platformy Azure:
- Nie można chronić udziałów plików platformy Azure w ramach kont magazynu, które mają włączone sieci wirtualne lub zaporę.
- Nie ma dostępnego interfejsu wiersza polecenia do ochrony usługi Azure Files z poziomu usługi Azure Backup.
- Maksymalna liczba zaplanowanych kopii zapasowych to jedna dziennie.
- Maksymalna liczba kopii zapasowych na żądanie to cztery dziennie.
- Aby zapobiec przypadkowemu usunięciu kopii zapasowych z magazynu usługi Recovery Services, użyj [blokad zasobów](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) na koncie magazynu.
- Nie usuwaj migawek utworzonych przy użyciu usługi Azure Backup. Usunięcie migawek może spowodować utratę punktów odzyskiwania i/lub błędy przywracania.
- Nie usuwaj udziałów plików, które są chronione przez usługę Azure Backup. Bieżące rozwiązanie usunie wszystkie migawki utworzone przez usługę Azure Backup, gdy udział plików zostanie usunięty, i tym samym utraci wszystkie punkty przywracania.

Powrót do udziałów plików platformy Azure w ramach kont magazynu [magazynu strefowo nadmiarowego](../storage/common/storage-redundancy-zrs.md) replikacji (ZRS) jest obecnie dostępna tylko w centralnej USA (CUS), wschodnie stany USA (EUS), wschodnie stany USA 2 (EUS2), Europa Północna (NE) Azja południowo-wschodnia (SEA) Europa Zachodnia (WE) i zachodnie stany USA 2 (WUS2).

## <a name="configuring-backup-for-an-azure-file-share"></a>Konfigurowanie kopii zapasowej udziału plików platformy Azure
W tym samouczku przyjęto założenie, że ustanowiono już udział plików platformy Azure. Aby utworzyć kopię zapasową udziału plików platformy Azure:

1. Utwórz magazyn usługi Recovery Services w tym samym regionie, co udziału plików. Jeśli magazyn już istnieje, otwórz stronę z jego przeglądem, a następnie kliknij pozycję **Kopia zapasowa**.

    ![Na stronie przeglądu własnego magazynu kliknij pozycję Kopia zapasowa](./media/backup-file-shares/overview-backup-page.png)

2. W **cel kopii zapasowej** menu z **co chcesz utworzyć kopię zapasową?** , wybierz udział plików platformy Azure.

    ![Wybieranie udział plików platformy Azure jako celu kopii zapasowej](./media/backup-file-shares/choose-azure-fileshare-from-backup-goal.png)

3. Kliknij pozycję **Utwórz kopię zapasową**, aby skonfigurować udział plików platformy Azure na potrzeby magazynu usługi Recovery Services.

   ![Klikanie pozycji Utwórz kopię zapasową, aby skojarzyć udział plików platformy Azure z magazynem](./media/backup-file-shares/set-backup-goal.png)

    Po magazynu skojarzonego z udziałem plików platformy Azure, menu kopia zapasowa otwiera i wyświetla monit o wybranie konta magazynu. Menu wyświetla wszystkie obsługiwane konta magazynu w regionie, gdzie istnieje magazynu nie są już skojarzone z magazynem usługi Recovery Services.

   ![Wybierz konto magazynu](./media/backup-file-shares/list-of-storage-accounts.png)

4. Na liście kont magazynu wybierz konto, a następnie kliknij przycisk **OK**. Na platformie Azure zostanie wykonane wyszukiwanie udziałów plików w ramach magazynu, dla których można utworzyć kopię zapasową. Jeśli niedawno dodano udziały plików i nie ma ich na liście, poczekaj chwilę na ich pojawienie się.

   ![Udziały plików są odnajdywane](./media/backup-file-shares/discover-file-shares.png)

5. Z **udziałów plików** listy, wybierz co najmniej jeden z udziałów plików, aby utworzyć kopię zapasową, a następnie kliknij przycisk **OK**.

6. Po wybraniu udziałów plików menu Kopia zapasowa zostanie przełączone na menu **Zasady tworzenie kopii zapasowych**. Z poziomu tego menu wybierz istniejące zasady tworzenia kopii zapasowych lub utwórz nowe, a następnie kliknij pozycję **Włącz kopię zapasową**.

   ![Wybierz zasady tworzenia kopii zapasowych lub Utwórz nową](./media/backup-file-shares/apply-backup-policy.png)

    Po ustanowieniu zasad tworzenia kopii zapasowych w zaplanowanym czasie zostanie utworzona migawka udziałów plików, a punkt odzyskiwania zostanie zachowany przez wybrany okres.

## <a name="create-an-on-demand-backup"></a>Tworzenie kopii zapasowej na żądanie
Po skonfigurowaniu zasad tworzenia kopii zapasowej, należy utworzyć kopii zapasowej na żądanie, aby upewnić się, że Twoje dane są chronione, aż do następnej zaplanowanej kopii zapasowej.


### <a name="to-create-an-on-demand-backup"></a>Tworzenie kopii zapasowej na żądanie

1. Otwórz magazyn usługi Recovery Services zawierający punkty odzyskiwania udziałów plików, a następnie kliknij pozycję **Elementy kopii zapasowej**. Zostanie wyświetlona lista typów elementów kopii zapasowych.

   ![Lista elementów kopii zapasowych](./media/backup-file-shares/list-of-backup-items.png)

2. Z listy wybierz pozycję **Azure Storage (Azure Files)** . Zostanie wyświetlona lista udziałów plików platformy Azure.

   ![Lista udziałów plików platformy Azure](./media/backup-file-shares/list-of-azure-files-backup-items.png)

3. Z listy udziałów plików platformy Azure wybierz żądany udział plików. Zostanie otwarte menu elementu kopii zapasowej dla wybranego udziału plików.

   ![Udostępnianie kopii zapasowej menu elementu dla wybranego pliku](./media/backup-file-shares/backup-item-menu.png)

4. W menu Element kopii zapasowej kliknij pozycję **Utwórz kopię zapasową teraz**. Ponieważ jest to zadanie tworzenia kopii zapasowej na żądanie, nie istnieją żadne zasady przechowywania skojarzone z punktem odzyskiwania. Zostanie otwarte okno dialogowe **Utwórz kopię zapasową teraz**. Określ ostatni dzień zachowywania punktu odzyskiwania.

   ![Wybierz datę przechowywania punktu odzyskiwania](./media/backup-file-shares/backup-now-menu.png)


## <a name="next-steps"></a>Kolejne kroki

Podczas pracy z tym samouczkiem wykonano następujące czynności przy użyciu witryny Azure Portal:

> [!div class="checklist"]
> * Konfigurowanie magazynu usługi Recovery Services na potrzeby tworzenia kopii zapasowej w usłudze Azure Files
> * Uruchomianie zadania tworzenia kopii zapasowej na żądanie w celu tworzenia punktu przywracania

Przejdź do następnego artykułu, aby przywrócić z kopii zapasowej udziału plików platformy Azure.

> [!div class="nextstepaction"]
> [Przywracanie z kopii zapasowej udziałów plików platformy Azure](./backup-azure-files.md#restore-from-backup-of-azure-file-share)
 
