---
title: Optymalizuj wydajność na maszynach wirtualnych platformy Azure serii Lsv2 - Storage | Dokumentacja firmy Microsoft
description: Dowiedz się, jak zoptymalizować wydajność rozwiązania jest wyświetlany na maszynach wirtualnych serii Lsv2.
services: virtual-machines-windows
author: laurenhughes
manager: jeconnoc
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/17/2019
ms.author: joelpell
ms.openlocfilehash: ec6a6ea5d00d43f6dffa3aabc367d94ebb8db198
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075602"
---
# <a name="optimize-performance-on-the-lsv2-series-virtual-machines"></a>Optymalizuj wydajność na maszynach wirtualnych serii Lsv2

Maszyny wirtualne serii Lsv2 obsługi różnych obciążeń wymagających wysokiej operacji We/Wy i przepływność lokalnego magazynu w szerokim zakresie aplikacji i branżach każdego typu.  Seria Lsv2 idealnie nadaje się do obsługi dużych ilości danych, SQL, NoSQL baz danych, magazynowanie danych i dużych transakcyjnych baz danych, w tym Cassandra, MongoDB, Cloudera i Redis.

Projekt maszyny wirtualne serii Lsv2 (VM) maksymalizuje procesora AMD EPYC™ 7551, aby zapewnić najlepszą wydajność, od procesora, pamięci, NVMe urządzeń i maszyn wirtualnych. Oprócz maksymalizowanie wydajności sprzętu, maszyny wirtualne z serii Lsv2 zostały zaprojektowane do pracy z potrzebami systemów operacyjnych Windows i Linux w celu zapewnienia lepszej wydajności sprzętu i oprogramowania.

Dostrojenie sprzętu i oprogramowania spowodowało zoptymalizowana wersja [systemie Windows Datacenter 2019](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), wydanej w 2018 r. grudnia w portalu Azure Marketplace, który obsługuje maksymalną wydajność urządzenia NVMe w serii Lsv2 Maszyny wirtualne.

Ten artykuł zawiera porady i sugestie, aby upewnić się, obciążenia i aplikacje osiągnąć maksymalną wydajność, zaprojektowane do maszyn wirtualnych. Informacje na tej stronie jest uaktualniany po dodaniu więcej Lsv2 zoptymalizowane pod kątem obrazów w portalu Azure Marketplace.

## <a name="amd-eypc-chipset-architecture"></a>Architektura mikroukładu AMD EYPC™

Maszyny wirtualne z serii Lsv2 używają procesorów serwera AMD EYPC™, oparte na mikroarchitektury Zen. AMD opracowanych nieskończoności sieci szkieletowej (jeśli jest) dla EYPC™ jako skalowalne połączeń swój model NUMA, który może służyć do komunikacji na struktury, -package i wielu pakietów. W porównaniu z QPI (ścieżka szybkiego łączenia) i zaległej płatności za przedmiot (połączenie Ultra-Path) używane w nowoczesnych procesorów monolityczne struktury firmy Intel, firmy AMD NUMA wielu małych struktury architektury, mogą powodować korzyści obu wydajności oraz trudności. Rzeczywisty wpływ ograniczenia przepustowości i opóźnienia pamięci mogą być różne w zależności od typu obciążeń.

## <a name="tips-for-maximizing-performance"></a>Porady dotyczące maksymalizowanie wydajności

* Sprzęt, na której działają maszyny wirtualne z serii Lsv2 korzysta z urządzenia NVMe z ośmiu s pary kolejek operacji We/Wy (QP). Każdej kolejki We/Wy urządzenia NVMe jest w rzeczywistości parą: kolejki przesyłania i kolejki ukończenia. Sterownik NVMe jest ustawiane, aby zoptymalizować wykorzystanie tych osiem operacji We/Wy liczby zapytań na sekundę, dystrybucja I / zaplanować O w działanie okrężne. Aby uzyskać maksymalną wydajność, należy uruchomić osiem zadań na urządzeniu, aby dopasować.

* Należy unikać mieszania NVMe polecenia administratora (na przykład NVMe INTELIGENTNE zapytanie info, itp.) za pomocą poleceń NVMe operacji We/Wy podczas aktywne obciążenia. Urządzenia Lsv2 NVMe są wspierane przez technologii bezpośredniego NVMe funkcji Hyper-V, przełączyć się do "powolne tryb" zawsze wtedy, gdy trwa dowolne polecenia administratora NVMe. Lsv2 użytkowników można uzyskać znaczne wydajności, Porzuć wydajność We/Wy NVMe, jeśli tak się stanie.

* Użytkownicy Lsv2 nie należy polegać na NUMA informacje o urządzeniu (wszystkie 0) zgłoszone w ramach maszyny Wirtualnej dla dysków z danymi, aby określić koligację z architektury NUMA dla swoich aplikacji. Zalecaną metodą zapewnienia lepszej wydajności jest Rozkładaj obciążenia między procesorów CPU, jeśli jest to możliwe. 

* Głębokość maksymalnej obsługiwanej kolejki, na pary kolejki operacji We/Wy dla urządzenia NVMe maszyny Wirtualnej Lsv2 wynosi 1024 (vs. Amazon i3 32 głębokość kolejki limit). Użytkownicy Lsv2 należy ograniczyć swoje (syntetyczne) porównawczych obciążenia na głębokości kolejki, co najmniej 1024 niższych w celu uniknięcia wyzwalania warunkach pełnej kolejki, które może zmniejszyć wydajność.

## <a name="utilizing-local-nvme-storage"></a>Korzystanie z magazynu lokalnego NVMe

Lokalny magazyn na dysku NVMe 1.92 TB na wszystkich maszynach wirtualnych Lsv2 jest tymczasowych. Podczas pomyślnego standardowa ponowny rozruch maszyny wirtualnej zostanie utrzymany danych na dysku lokalnym NVMe. Dane nie zostanie utrzymany na NVMe, jeśli maszyna wirtualna zostanie ponownego wdrożenia, cofać przydziału lub usunięty. Dane nie zostanie utrzymany, jeśli inny problem powoduje, że maszyna wirtualna lub sprzętu, na którym jest uruchomiona, aby stać się złej kondycji. W takim przypadku wszystkie dane na starym hosta jest bezpiecznie wymazywane.

Będą również przypadki, gdy maszyna wirtualna wymaga zostanie przeniesiony na inny komputer hosta, na przykład podczas operacji zaplanowanej konserwacji. Operacje planowanej konserwacji i niektóre awarie sprzętowe, można oczekiwać z [Scheduled Events](scheduled-events.md). Scheduled Events powinna służyć do bądź na bieżąco na dostęp do przewidywanych konserwacji i operacje odzyskiwania.

W tym przypadku zdarzenie planowanej konserwacji wymaga maszyna wirtualna może być odtworzona na nowy host z pustych dysków lokalnych danych należy ponownie zsynchronizowane (ponownie, przy użyciu dowolnych danych w starej hosta jest bezpiecznie wymazywane). Dzieje się tak, ponieważ maszyny wirtualne z serii Lsv2 aktualnie nie obsługuje migracji na żywo na dysku lokalnym NVMe.

Istnieją dwa tryby planowanej konserwacji.

### <a name="standard-vm-customer-controlled-maintenance"></a>Standardowa konserwacji kontrolowane przez klienta maszyny Wirtualnej

- Maszyna wirtualna jest przenoszona na hoście zaktualizowanym przedziale 30-dniowego.
- Mogą zostać utracone, Lsv2 lokalnego magazynu danych, dlatego zaleca się tworzenia kopii zapasowych danych przed zdarzeniem.

### <a name="automatic-maintenance"></a>Automatycznej konserwacji

- Występuje, gdy klient nie jest wykonywane konserwacji kontrolowane przez klienta lub w przypadku procedur awaryjnych, takich jak zdarzenia zabezpieczeń zero day.
- Zachowaj dane klienta, ale do małych ryzyko blokowanie maszyny Wirtualnej lub ponowne uruchomienie komputera.
- Mogą zostać utracone, Lsv2 lokalnego magazynu danych, dlatego zaleca się tworzenia kopii zapasowych danych przed zdarzeniem.

Wszystkie zdarzenia usługi, która umożliwia wybierz czas na najbardziej odpowiednim dla aktualizacji proces konserwacji kontrolowany. Przed zdarzeniem możesz utworzyć kopię zapasową danych w usłudze premium storage. Po zakończeniu zdarzenia konserwacji, można zwrócić danych do odświeżenia maszyn wirtualnych Lsv2 NVMe magazynu lokalnego.

Scenariusze, Obsługa danych na dyskach lokalnych NVMe, które obejmują:

- Maszyna wirtualna jest uruchomiona i działa prawidłowo.
- Maszyna wirtualna jest uruchomiona ponownie w miejscu (przez Ciebie lub Azure).
- Maszyna wirtualna jest wstrzymana (zatrzymane bez dezalokacji).
- Większość planowanej konserwacji operacji obsługi.

Scenariusze, które bezpiecznie wymazać dane klienta obejmują:

- Maszyna wirtualna jest ponownie wdrażana, zatrzymana (cofnięty przydział) lub usuniesz ().
- Maszyna wirtualna staje się nieprawidłowy i ma usługą poprawianie do innego węzła z powodu problemu ze sprzętem.
- Niewielka liczba planowanej konserwacji operacji obsługi, które wymaga maszyny Wirtualnej, zostają przeniesione na innego hosta do obsługi.

Aby dowiedzieć się więcej o opcjach wykonywania kopii zapasowych danych w magazynie lokalnym, zobacz [kopia zapasowa i odzyskiwanie po awarii dla dysków Azure IaaS](backup-and-disaster-recovery-for-azure-iaas-disks.md).

## <a name="frequently-asked-questions"></a>Często zadawane pytania

* **Jak uruchomić wdrażanie maszyn wirtualnych z serii Lsv2**  
   Podobnie jak każdej innej maszyny Wirtualnej, należy użyć [Portal](quick-create-portal.md), [wiersza polecenia platformy Azure](quick-create-cli.md), lub [PowerShell](quick-create-powershell.md) utworzyć maszynę Wirtualną.

* **Pojedynczego uszkodzenia dysku NVMe spowoduje wszystkich maszyn wirtualnych na hoście nie powiedzie się?**  
   W przypadku wykrycia awarii dysku w węźle sprzętu, sprzęt jest w stanie niepowodzenia. W takiej sytuacji wszystkie maszyny wirtualne w węźle są cofać przydziału i automatycznie przeniesiona do węzła dobrej kondycji. Dla maszyn wirtualnych z serii Lsv2 oznacza to, czy danych klienta w węźle niepowodzenie jest również bezpiecznie wymazywane i musi zostać ponownie utworzone przez klienta w nowym węźle. Jak wspomniano, zanim migracji na żywo staje się dostępny dla Lsv2, w węźle niepowodzenie będą aktywnie przenieść dane z maszyn wirtualnych jak są przenoszone do innego węzła.

* **Należy wprowadzić korekty sondowania w Windows w systemie Windows Server 2012 lub Windows Server 2016?**  
   NVMe sondowania jest dostępna tylko na 2019 serwera systemu Windows na platformie Azure.  

* **Czy mogę przełączać do modelu tradycyjnym przerwania usług rutynowych (ISR)?**  
   Maszyny wirtualne z serii Lsv2 są zoptymalizowane dla NVMe sondowania. Aktualizacje ciągle są przekazywane do zwiększenia wydajności sondowania.

* **Czy można dostosować ustawienia sondowania w 2019 r Server systemu Windows?**  
   Ustawienia sondowania nie są zmieniane użytkownika.
   
## <a name="next-steps"></a>Kolejne kroki

* Zobacz specyfikacje dotyczące wszystkich [maszyny wirtualne zoptymalizowane pod kątem wydajności pamięci masowej](sizes-storage.md) na platformie Azure
