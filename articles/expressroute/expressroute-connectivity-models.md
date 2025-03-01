---
title: 'Modele połączeń usługi ExpressRoute: Azure| Microsoft Docs'
description: W tym artykule opisano różne tryby połączeń między siecią klienta oraz platformą Microsoft Azure i usługami Office 365 i Dynamics 365. Klienci mogą korzystać z dostawców MPLS, wymian w chmurze i dostawców sieci Ethernet.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: ef53f9d3cefa35624d6f94dc98db4ebe8b6646cd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60367464"
---
# <a name="expressroute-connectivity-models"></a>Modele połączeń usługi ExpressRoute
Połączenie między siecią lokalną i chmurą firmy Microsoft można utworzyć na trzy sposoby: [kolokacja CloudExchange](#CloudExchange), [połączenie Ethernet typu punkt-punkt](#Ethernet) i [łączność typu dowolna-dowolna (IPVPN)](#IPVPN). Dostawcy połączenia oferują po jednym modelu łączności lub większą ich liczbę. Można współpracować z dostawcą połączenia w celu wybrania najlepszego dla siebie modelu.
<br><br>

![Diagram modelu połączeń usługi ExpressRoute](./media/expressroute-connectivity-models/expressroute-connectivity-models-diagram.png)

## <a name="CloudExchange"></a>Kolokowanie w ramach wymiany w chmurze
Jeśli użytkownik jest kolokowany w ramach infrastruktury z wymianą w chmurze, może zamówić łączność obejmującą wiele połączeń wirtualnych z chmurą Microsoft za pośrednictwem wymiany sieci Ethernet dostawy kolokacji. Dostawcy kolokacji mogą zaoferować łączność obejmującą wiele połączeń w warstwie 2 lub wiele zarządzanych połączeń w warstwie 3 między infrastrukturą w strukturze kolokacji i chmurą Microsoft.

## <a name="Ethernet"></a>Połączenia Ethernet typu punkt-punkt
Lokalne centra danych/biura można połączyć z chmurą Microsoft za pośrednictwem łączy Ethernet typu punkt-punkt. Dostawcy sieci Ethernet typu punkt-punkt oferują połączenia w warstwie 2 lub zarządzane połączenia w warstwie 3 między witryną użytkownika i chmurą Microsoft.

## <a name="IPVPN"></a>Sieci typu dowolna-dowolna (IPVPN)
Sieć WAN można zintegrować z chmurą Microsoft. Dostawcy sieci IPVPN (zwykle MPLS VPN) oferują łączność typu dowolna-dowolna między biurami oddziałów i centrami danych. Chmurę Microsoft można połączyć z siecią WAN w taki sposób, aby wyglądało to tak jak każde inne biuro oddziału. Dostawcy sieci WAN oferują zazwyczaj łączność zarządzaną w warstwie 3. Wszystkie możliwości i funkcje usługi ExpressRoute są identyczne we wszystkich powyższych modelach łączności. 

## <a name="next-steps"></a>Kolejne kroki
* Więcej informacji na temat połączeń ExpressRoute i domen routingu. Zobacz artykuł [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (Obwody i domeny routingu usługi ExpressRoute).
* Dowiedz się więcej na temat funkcji usługi ExpressRoute. Zobacz [ExpressRoute — opis techniczny](expressroute-introduction.md)
* Znajdź dostawcę usługi. Zobacz artykuł [ExpressRoute partners and peering locations](expressroute-locations.md) (Partnerzy i lokalizacje komunikacji równorzędnej usługi ExpressRoute).
* Upewnij się, że zostały spełnione wszystkie wymagania wstępne. Zobacz artykuł [ExpressRoute prerequisites](expressroute-prerequisites.md) (Wymagania wstępne usługi ExpressRoute).
* Zapoznaj się z wymaganiami dotyczącymi [routingu](expressroute-routing.md), [NAT](expressroute-nat.md) i [QoS](expressroute-qos.md).
* Skonfiguruj połączenie usługi ExpressRoute.
  * [Create an ExpressRoute circuit (Tworzenie obwodu usługi ExpressRoute)](expressroute-howto-circuit-portal-resource-manager.md)
  * [Configure routing (Konfigurowanie routingu)](expressroute-howto-routing-portal-resource-manager.md)
  * [Link a VNet to an ExpressRoute circuit (Łączenie sieci wirtualnej z obwodem usługi ExpressRoute)](expressroute-howto-linkvnet-portal-resource-manager.md)