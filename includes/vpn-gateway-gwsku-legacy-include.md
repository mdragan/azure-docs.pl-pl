---
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: cherylmc
ms.openlocfilehash: f2ee442d0d6fecf34449ad28f058615a1274bbea
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67183119"
---
Starsze (stare) jednostki SKU bramy sieci VPN to:

* Podstawowa
* Standardowa (Standard)
* Wysoka wydajność (HighPerformance)

Brama sieci VPN nie korzysta z jednostki SKU bramy UltraPerformance. Informacje o jednostce SKU UltraPerformance można znaleźć w dokumentacji dotyczącej usługi [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md).

Pracując ze starszymi jednostkami SKU, rozważ następujące kwestie:

* Jeśli chcesz użyć typu sieci VPN PolicyBased, należy użyć podstawowej jednostki SKU. Sieć VPN PolicyBased (nazywana wcześniej routingiem statycznym) nie jest obsługiwana w żadnych innych jednostkach SKU.
* Protokół BGP nie jest obsługiwany w ramach podstawowej jednostki SKU.
* Współistniejące konfiguracje bramy sieci VPN usługi ExpressRoute nie są obsługiwane w ramach podstawowej jednostki SKU.
* Połączenia usługi VPN Gateway typu lokacja do lokacji (S2S) aktywne-aktywne można skonfigurować tylko w ramach jednostki SKU o wysokiej wydajności.