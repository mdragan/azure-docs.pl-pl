---
title: Warstwa ogólnego przeznaczenia usługi — Azure SQL Database | Dokumentacja firmy Microsoft
description: Dowiedz się więcej o usłudze Azure SQL Database w warstwie ogólnego przeznaczenia
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: sstein
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: b972ea985a09457d8b6a17a292e18754761f5a6e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66479196"
---
# <a name="general-purpose-service-tier---azure-sql-database"></a>Warstwy usług ogólnego przeznaczenia — usługi Azure SQL Database

> [!NOTE]
> Warstwa ogólnego przeznaczenia usługi w modelu zakupu opartego na rdzeniach wirtualnych nosi nazwę w modelu zakupu opartego na jednostkach DTU warstwie usługi standardowa. Dla porównania modelu zakupu opartego na rdzeniach wirtualnych za pomocą modelu zakupu opartego na jednostkach DTU, zobacz [zakupu modeli i zasobów bazy danych SQL Azure](sql-database-purchase-models.md).

Usługa Azure SQL Database jest oparty na architekturze aparatu bazy danych programu SQL Server zostało dostosowane do środowiska chmury w celu zapewnienia dostępności 99,99%, nawet w przypadku wystąpienia awarii infrastruktury. Istnieją trzy warstwy usług, które są używane w usłudze Azure SQL Database, każdy z różnych modeli architektury. Te warstwy usługi są:

- Zastosowania ogólne
- Krytyczne dla działania firmy
- Hiperskala

Architektury model dla warstwy usług ogólnego przeznaczenia opiera się na oddzielenie zasobów obliczeniowych i magazynowych. Ten model architektury opiera się na wysoką dostępność i niezawodność usługi Azure Blob storage, w sposób przezroczysty replikuje pliki bazy danych i gwarantuje bez utraty danych, jeśli podstawowy błąd infrastruktury się dzieje.

Na poniższej ilustracji przedstawiono cztery węzły w standardowych architektury modelu z rozdzielonych warstwy obliczeń i magazynu.

![Oddzielenie zasobów obliczeniowych i magazynu](media/sql-database-managed-instance/general-purpose-service-tier.png)

W modelu architektury dla warstwy usług ogólnego przeznaczenia istnieją dwie warstwy:

- Warstwy obliczeniowej bezstanowe uruchomioną `sqlserver.exe` przetwarzania i zawiera tylko błędy przejściowe i buforowanych danych (na przykład — pamięci podręcznej planu, puli buforów, pula magazynu kolumn). To bezstanowe węzła programu SQL Server jest obsługiwany przez usługi Azure Service Fabric inicjuje proces, który kontroluje kondycji węzła i wykonuje trybu failover w inne miejsce, jeśli to konieczne.
- Warstwy danych stanowych z plikami bazy danych (.mdf/.ldf), które są przechowywane w usłudze Azure Blob storage. Usługa Azure Blob storage gwarantuje, że będzie bez utraty danych, dla dowolnego rekordu, który znajduje się w dowolnym pliku bazy danych. Usługa Azure Storage ma wbudowane dostępność/nadmiarowości danych gwarantuje, że każdy rekord w pliku dziennika lub strony w pliku danych zostaną zachowane nawet, jeśli wystąpiła awaria procesu programu SQL Server.

Zawsze, gdy aparat bazy danych lub system operacyjny zostanie uaktualniony, część podstawowej infrastruktury nie powiedzie się lub jeśli jakiś problem krytyczny zostanie wykryte w procesie programu SQL Server, usługi Azure Service Fabric zostanie przesunięty bezstanowe procesu programu SQL Server do innego węzła obliczeniowego o bezstanowa. Istnieje zestaw węzłów zapasowych, który oczekuje na uruchomienie nowych usług obliczeniowych, jeśli występuje przejściu w tryb failover węzła podstawowego, aby zminimalizować czas pracy awaryjnej. W warstwie usługi Azure storage to nie miało wpływu na dane i pliki danych/dziennika są dołączone do nowo utworzonym procesu programu SQL Server. Tego procesu gwarantuje dostępność przez 99,99%, ale może mieć wpływ na niektóre wydajności na duże obciążenie, które działa ze względu na czas przejścia i fakt nowego węzła programu SQL Server, który rozpoczyna się od zimnych pamięci podręcznej.

## <a name="when-to-choose-this-service-tier"></a>Kiedy należy wybrać w tej warstwie usługi

Warstwy usług w usłudze ogólnego przeznaczenia jest domyślną warstwę usługi, w bazie danych SQL Azure, przeznaczony dla większości obciążeń ogólnego. Jeśli potrzebujesz aparatu w pełni zarządzana baza danych, z umową SLA dostępność przez 99,99% z opóźnieniem magazynu między 5-10 ms, odpowiadające platformie IaaS Azure SQL w większości przypadków, warstwa ogólnego przeznaczenia jest opcją w.

## <a name="next-steps"></a>Kolejne kroki

- Znajdowanie zasobów cechy (liczba rdzeni, we/wy, pamięć) warstwa ogólnego przeznaczenia/Standard w [wystąpienia zarządzanego](sql-database-managed-instance-resource-limits.md#service-tier-characteristics), pojedyncze bazy danych w [modelu rdzenia wirtualnego](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-storage-sizes-and-compute-sizes) lub [modelu jednostek DTU](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes), lub Elastycznej puli w [modelu rdzenia wirtualnego](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes) i [modelu jednostek DTU](sql-database-dtu-resource-limits-elastic-pools.md#standard-elastic-pool-limits).
- Dowiedz się więcej o [krytyczne dla działania firmy](sql-database-service-tier-business-critical.md) i [Hiperskali](sql-database-service-tier-hyperscale.md) warstw.
- Dowiedz się więcej o [usługi Service Fabric](../service-fabric/service-fabric-overview.md).
- Aby uzyskać więcej opcji wysokiej dostępności i odzyskiwania po awarii, zobacz [ciągłość prowadzenia działalności biznesowej](sql-database-business-continuity.md).
