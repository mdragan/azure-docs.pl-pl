---
title: Co to są strefy dostępności platformy Azure? | Microsoft Docs
description: Aby utworzyć wysoko dostępne i odporne na błędy aplikacje na platformie Azure, strefy dostępności zapewniają fizycznie oddzielone lokalizacje, używane do uruchamiania Twoich zasobów.
services: ''
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/20/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 70f03ee9f6d806733f70994cc152826d31f5c353
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295777"
---
# <a name="what-are-availability-zones-in-azure"></a>Co to są strefy dostępności na platformie Azure?
Strefy dostępności to oferta, która chroni aplikacje i dane przed awariami centrów danych o wysokiej dostępności. Strefy dostępności to unikatowe fizycznie lokalizacje w regionie platformy Azure. Każda strefa składa się z co najmniej jeden centrów danych, wyposażone w niezależne zasilanie, chłodzenie i usługi sieciowe. W celu zapewnienia odporności istnieją co najmniej trzy osobne strefy we wszystkich włączonych regionach. Fizyczna separacja stref dostępności w ramach regionu chroni aplikacje i dane przed awariami centrum danych. Strefowo nadmiarowe usługi replikować aplikacji i danych w różnych strefach dostępności, aby zapewnić ochronę przed pojedynczej punktami z awarią. Dzięki strefom dostępności platforma Azure oferuje najlepszą w branży umowę dotycząca poziomu usług (SLA) gwarantującą czas działania na poziomie 99,99%. Pełna treść [umowy SLA dotyczącej usługi Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/) wyjaśnia w całości kwestię gwarantowanej dostępności platformy Azure.

Strefa dostępności w regionie świadczenia usługi Azure jest kombinacją domeny błędów i domeny aktualizacji. Jeśli na przykład utworzysz trzy lub więcej maszyn wirtualnych w trzech strefach w regionie świadczenia usługi Azure, maszyny wirtualne będą rozproszone w trzech domenach błędów i trzech domenach aktualizacji. Platforma Azure rozpoznaje to rozproszenie w domenach aktualizacji, aby upewnić się, że maszyny wirtualne w różnych strefach nie są aktualizowane w tym samym czasie.

Rozbudowuj wysokiej dostępności architektury aplikacji, wspólnego przechowywania zasobów obliczeniowych, magazynu, sieci i danych w strefie i replikacji w innych strefach. Usługi platformy Azure, które obsługują strefy dostępności można podzielić na dwie kategorie:

- **Usługi strefowych** — możesz przypiąć zasób do określonej strefy (na przykład maszyny wirtualne, dyski zarządzane, adresy IP), lub
- **Strefowo nadmiarowe usługi** — platforma automatycznie replikuje dane w strefach (na przykład, Magazyn strefowo nadmiarowy, SQL Database).

Aby uzyskać kompleksowy zestaw funkcji ciągłości na platformie Azure, tworzenie architektury aplikacji za pomocą kombinacji strefach dostępności przy użyciu par regionów platformy Azure. Można synchroniczna replikacja aplikacji i danych przy użyciu stref dostępności w obrębie regionu platformy Azure dla wysokiej dostępności i informacje o asynchronicznym replikowaniu w różnych regionach platformy Azure, aby uzyskać ochronę odzyskiwania po awarii.
 
![Widok koncepcyjny jedną strefę, przechodząc w dół w regionie](./media/az-overview/az-graphic-two.png)

## <a name="services-support-by-region"></a>Obsługa usług według regionów

Kombinacje usług platformy Azure i regionów, które obsługują strefy dostępności są:


|                                 |Ameryki |              |           |           | Europa |              |          |              | Azja i Pacyfik |                 |
|----------------------------|----------|----------|---------|---------|--------------|------------|--------|----------|----------|-------------|
|          |Środkowe stany USA|Wschodnie stany USA|Wschodnie stany USA 2|Zachodnie stany USA 2|Francja Środkowa|Europa Północna|Południowe Zjednoczone Królestwo|Europa Zachodnia|Japonia Wschodnia|Azja Południowo-Wschodnia|
| **Obliczanie**                         |            |              |           |           |                |              |          |             |            |                |
| Maszyny wirtualne z systemem Linux          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Maszyny wirtualne z systemem Windows        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Zestawy skali maszyn wirtualnych      | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| **Storage**   |            |              |           |           |                |              |          |             |            |                |
| Dyski zarządzane                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Magazyn strefowo nadmiarowy          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| **Sieć**                     |            |              |           |           |                |              |          |             |            |                |
| Adres IP w warstwie standardowa        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Usługa Load Balancer w warstwie Standardowa     | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| VPN Gateway                     | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| Brama usługi ExpressRoute   | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| Application Gateway   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    | &#10003;       | &#10003;       |
| Azure Firewall           | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |  &#10003;       | &#10003;       |
| **Bazy danych**                     |            |              |           |           |                |              |          |             |            |                |
| SQL Database                    | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |            | &#10003;       |
| Azure Cache for Redis           | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |  &#10003;       | &#10003;       |
| Azure Cosmos DB                    |    |    |   |  |       |     | &#10003; |     |            | &#10003;       |
| **Analiza**                       |            |              |           |           |                |              |          |             |            |                |
| Event Hubs                      | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| **Integracja**                     |            |              |           |           |                |              |          |             |            |                |
| Usługa Service Bus (tylko w warstwie Premium) | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |



## <a name="services-resiliency"></a>Odporność usług
Wszystkie usługi zarządzania platformy Azure są zaprojektowana, aby była odporna z awariami na poziomie regionu. W szerokim zakresie błędów co najmniej jeden strefy dostępności w obrębie regionu mają mniejsze radius błędów w porównaniu do awarii całego regionu. Azure można odzyskać z awarią poziomu strefy usługi zarządzania w obrębie regionu lub z innego regionu platformy Azure. Platforma Azure przeprowadza konserwację krytyczne w jednej strefie w danym momencie w obrębie regionu, aby uniknąć błędów wpływające na ochronę zasobów klienta wdrożonych w różnych strefach dostępności w obrębie regionu.

## <a name="pricing"></a>Cennik
Nie ma żadnych dodatkowych kosztów, dla maszyn wirtualnych wdrożonych w strefie dostępności. Dostępność przez 99,99% dostępności maszyn wirtualnych umowa SLA jest oferowana gdy co najmniej dwie maszyny wirtualne są wdrożone w co najmniej dwóch strefach dostępności w obrębie regionu platformy Azure. Będzie istnieć dodatkowe między dostępności maszyny Wirtualnej VM strefy opłaty za transfer danych. Aby uzyskać więcej informacji, zobacz [cen przepustowości](https://azure.microsoft.com/pricing/details/bandwidth/) strony.


## <a name="get-started-with-availability-zones"></a>Rozpoczynanie pracy ze strefami dostępności
- [Tworzenie maszyny wirtualnej](../virtual-machines/windows/create-portal-availability-zone.md)
- [Dodawanie dysku zarządzanego przy użyciu programu PowerShell](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Tworzenie zestawu skalowania strefy nadmiarowe maszyny wirtualnej](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Równoważenie obciążenia maszyn wirtualnych w strefach przy użyciu standardowego modułu równoważenia obciążenia za pomocą frontonu strefowo nadmiarowy](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [Równoważenie obciążenia maszyn wirtualnych w strefie standardowego modułu równoważenia obciążenia przy użyciu strefowych frontonu](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [Magazyn strefowo nadmiarowy](../storage/common/storage-redundancy-zrs.md)
- [SQL Database](../sql-database/sql-database-high-availability.md#zone-redundant-configuration)
- [Geograficzne odzyskiwanie po awarii w usłudze Event Hubs](../event-hubs/event-hubs-geo-dr.md#availability-zones)
- [Geograficzne odzyskiwanie po awarii w usłudze Service Bus](../service-bus-messaging/service-bus-geo-dr.md#availability-zones)
- [Tworzenie strefowo nadmiarowej bramy sieci wirtualnej](../vpn-gateway/create-zone-redundant-vnet-gateway.md)
- [Dodaj region nadmiarowe strefy usługi Azure Cosmos DB](../cosmos-db/high-availability.md#availability-zone-support)
- [Wprowadzenie do usługi Azure Cache stref dostępności w pamięci podręcznej Redis](https://aka.ms/redis/az/getstarted)

## <a name="next-steps"></a>Kolejne kroki
- [Quickstart templates (Szablony szybkiego startu)](https://aka.ms/azqs)
