---
title: Ponowne włączanie ochrony przełączone w tryb failover maszyn wirtualnych platformy Azure ponownie podstawowego regionu platformy Azure za pomocą usługi Azure Site Recovery | Dokumentacja firmy Microsoft
description: Opisuje sposób ponownego włączenia ochrony maszyn wirtualnych platformy Azure w regionie pomocniczym, po włączeniu trybu failover z regionu podstawowego, za pomocą usługi Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: eabb7d194a3ef65282befab1ae59e85ba56f2f5b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65472160"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Ponowne włączanie ochrony nie powiodło się na maszynach wirtualnych platformy Azure, do regionu podstawowego


Gdy możesz [w trybie Failover](site-recovery-failover.md) maszyn wirtualnych platformy Azure z jednego regionu do innego za pomocą [usługi Azure Site Recovery](site-recovery-overview.md), maszyny wirtualne rozruchu w regionie pomocniczym w stanie niechronionym. Powrót po awarii maszyn wirtualnych do regionu podstawowego, należy wykonać następujące czynności:

- Ponowne włączanie ochrony maszyn wirtualnych w regionie pomocniczym, aby rozpoczynały się na replikację do regionu podstawowego.
- Po zakończeniu ponownego włączania ochrony, replikowania maszyn wirtualnych, można przełączać je za pośrednictwem z pomocniczej do regionu podstawowego.

## <a name="prerequisites"></a>Wymagania wstępne
1. Trybu failover maszyny Wirtualnej z serwera podstawowego do regionu pomocniczego, muszą być zatwierdzone.
2. Witryna podstawowy cel powinny być dostępne i powinno być możliwe otworzyć lub utworzyć zasobów w danym regionie.

## <a name="reprotect-a-vm"></a>Ponowne włączanie ochrony maszyny Wirtualnej

1. W **magazynu** > **zreplikowane elementy**, kliknij prawym przyciskiem myszy nieudane przez maszynę Wirtualną i wybierz **ponownego włączenia ochrony**. Kierunek ponownego włączania ochrony powinny być wyświetlane z dodatkowej do głównej.

   ![Ponowne włączanie ochrony](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Przejrzyj zestawy grup, sieci, magazynu i dostępności zasobów. Następnie kliknij przycisk **OK**. Jeśli istnieją wszystkie zasoby oznaczone jako nowe, są one tworzone w ramach procesu ponownego włączania ochrony.
3. Zadanie ponownego włączania ochrony inicjuje lokację docelową, przy użyciu najnowszych danych. Po zakończeniu którego odbywa się replikacja różnicowa. Następnie można powrotu po awarii za pośrednictwem do lokacji głównej. Można wybrać konto magazynu lub sieci, z którą ma zostać użyty podczas ponownego włączenia ochrony, za pomocą opcji Dostosuj.

   ![Dostosowywanie opcji](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Dostosuj ustawienia ponownego włączania ochrony

Można dostosować następujące właściwości obiektu docelowego VMe podczas ponownego włączania ochrony.

![Dostosuj](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Właściwość |Uwagi  |
|---------|---------|
|Docelowa grupa zasobów     | Zmodyfikuj docelowa grupa zasobów, w którym zostanie utworzona maszyna wirtualna. W ramach ponownego włączania ochrony docelowa maszyna wirtualna jest usuwana. Można wybrać nową grupę zasobów, pod którym chcesz utworzyć maszynę Wirtualną po włączeniu trybu failover.        |
|Docelowa sieć wirtualna     | Nie można zmienić sieci docelowej podczas wykonywania zadania ponownego włączania ochrony. Aby zmienić sieci, ponów mapowania sieci.         |
|Magazyn docelowy (pomocniczej maszyny Wirtualnej nie korzystają z dysków zarządzanych)     | Można zmienić konta magazynu, używanego przez maszynę Wirtualną po włączeniu trybu failover.         |
|Repliki usługi managed disks (pomocniczej maszyny Wirtualnej korzysta z dysków zarządzanych)    | Usługa Site Recovery tworzy dyski zarządzane repliki w regionie podstawowym w celu zdublowania dysków zarządzanych pomocniczej maszyny Wirtualnej.         |
|Cache Storage     | Można określić konto magazynu pamięci podręcznej do użycia podczas replikacji. Domyślnie nowe konto magazynu pamięci podręcznej jest możliwe, jeśli nie istnieje.         |
|Zestaw dostępności     |Jeśli maszynę Wirtualną w regionie pomocniczym jest częścią zestawu dostępności, możesz wybrać zestaw dostępności dla docelowej maszyny Wirtualnej w regionie podstawowym. Domyślnie usługa Site Recovery podejmie próbę odnalezienia istniejący zestaw dostępności w regionie podstawowym, a jej używać. Podczas dostosowywania można określić nowy zestaw dostępności.         |


### <a name="what-happens-during-reprotection"></a>Co się dzieje podczas ponownego włączania ochrony?

Domyślnie mają miejsce następujące zdarzenia:

1. Konto magazynu pamięci podręcznej jest tworzony w regionie, w przypadku, gdy działa w trybie Failover maszyny Wirtualnej.
2. Jeśli konto magazynu docelowego (oryginalnego konta magazynu w regionie podstawowym) nie istnieje, zostanie utworzony nowy. Nazwa konta magazynu przypisany jest nazwa konta magazynu używanego przez pomocniczej maszyny Wirtualnej sufiks "asr".
3. Jeśli maszyna wirtualna używa dysków zarządzanych, zarządzane repliki dyski są tworzone w regionie podstawowym do przechowywania danych replikacji z dysków pomocniczej maszyny Wirtualnej.
4. Jeśli docelowy zestaw dostępności nie istnieje, nowy jest tworzony jako część zadania ponownego włączania ochrony, jeśli jest to wymagane. Jeśli dostosowano ustawienia ponownego włączania ochrony wybranego zestawu jest używana.

Gdy użytkownik zainicjuje Zadanie włączania ponownej ochrony i docelowa maszyna wirtualna istnieje, są następujące operacje:

1. Stronie docelowej, do których maszyna wirtualna jest wyłączona w przypadku działa.
2. Jeśli maszyna wirtualna używa dysków zarządzanych, kopię oryginalnych dysków są tworzone za pomocą "-ASRReplica" sufiks. Oryginalnych dysków są usuwane. "-ASRReplica" kopie są używane do replikacji.
3. Jeśli maszyna wirtualna używa dysków niezarządzanych, docelowej maszyny Wirtualnej dyski danych są odłączone i używane na potrzeby replikacji. Kopię dysku systemu operacyjnego jest utworzone i dołączone na maszynie Wirtualnej. Oryginalny dysk systemu operacyjnego jest odłączona i używane na potrzeby replikacji.
4. Od dysku źródłowego i docelowego dysku synchronizowane będą tylko zmiany. Różnice są obliczane przez porównanie obu dyskach tak i następnie przeniesiona. Aby znaleźć wyboru szacowany czas poniżej.
5. Po ukończeniu synchronizacji replikacja różnicowa rozpoczyna się i tworzy punkt odzyskiwania zgodnie z zasadami replikacji.

Możesz wyzwolić Zadanie włączania ponownej ochrony, gdy nie istnieją docelową maszynę Wirtualną i dyski, są następujące operacje:
1. Jeśli maszyna wirtualna używa dysków zarządzanych, dyski repliki są tworzone za pomocą "-ASRReplica" sufiks. "-ASRReplica" kopie są używane do replikacji.
2. Jeśli maszyna wirtualna używa dysków niezarządzanych, dyski repliki są tworzone w docelowym koncie magazynu.
3. Cały dyski są kopiowane z nieudane przez region, aby nowy region docelowy.
4. Po ukończeniu synchronizacji replikacja różnicowa rozpoczyna się i tworzy punkt odzyskiwania zgodnie z zasadami replikacji.

#### <a name="estimated-time--to-do-the-reprotection"></a>Szacowany czas do ponownego wykonać 

W większości przypadków usługi Azure Site Recovery nie replikuje kompletne dane do regionu źródłowego. Poniżej przedstawiono warunki, które określa, ile danych będą replikowane:

1.  Jeśli źródło danych maszyny Wirtualnej jest usunięte, uszkodzony lub niedostępny z jakiegoś powodu, takie jak grupy zasobów zmieniać/usuwać, następnie podczas ponownego włączania ochrony pełne środowisko IR nastąpi, ponieważ dane nie są dostępne w regionie źródłowym do użycia.
2.  Jeśli źródło danych maszyny Wirtualnej jest dostępny tylko różnice są obliczane przez porównanie obu dyskach tak i następnie przeniesiona. Sprawdź pod tabelą, aby uzyskać szacowany czas 

|** Przykład sytuacji ** | ** Czas potrzebny na ponowne objęcie ochroną ** |
|--- | --- |
|Region źródłowy ma 1 maszyny Wirtualnej przy użyciu dysku 1 TB w warstwie standardowa<br/>-Tylko 127 GB danych jest używana, a resztę dysku jest pusty<br/>— Typ dysku jest standardem w 60 MiB/S przepływności<br/>-Brak zmian danych, po włączeniu trybu failover| Przybliżony czas 45 minut — 1,5 godziny<br/> — Podczas ponownego włączania ochrony Usługa Site Recovery zostanie wypełniony sumy kontrolnej całego danych, co spowoduje przejście do 127 GB / 45 MB ~ 45 minut<br/>— Niektórzy obciążenie czas jest wymagany dla usługi Site Recovery, automatycznego skalowania, który jest 20 – 30 minutach<br/>— Brak opłat za ruch wychodzący |
|Region źródłowy ma 1 maszyny Wirtualnej przy użyciu dysku 1 TB w warstwie standardowa<br/>-Tylko 127 GB danych jest używana, a resztę dysku jest pusty<br/>— Typ dysku jest standardem w 60 MiB/S przepływności<br/>-Zmiany danych 45 GB po włączeniu trybu failover| Przybliżony czas 1 – 2 godzin<br/>— Podczas ponownego włączania ochrony Usługa Site Recovery zostanie wypełniony sumy kontrolnej całego danych, co spowoduje przejście do 127 GB / 45 MB ~ 45 minut<br/>-Transferu czasu w celu zastosowania zmian 45 GB, który wynosi 45 GB / 45 MB/s ~ 17 w ciągu kilku minut<br/>— Opłaty za ruch wychodzący będzie tylko dla danych 45 GB dla sumy kontrolnej|
 



## <a name="next-steps"></a>Kolejne kroki

Po włączeniu ochrony maszyny Wirtualnej może zainicjować trybu failover. Przełączenie w tryb failover zamykania maszyny Wirtualnej w regionie pomocniczym tworzy i uruchamia maszynę Wirtualną w regionie podstawowym, przy użyciu przestój małych. Zaleca się, w związku z tym wybierz godzinę i uruchomić test trybu failover przed zainicjowaniem pełnego trybu failover do lokacji głównej. [Dowiedz się więcej](site-recovery-failover.md) informacje o trybie failover.
