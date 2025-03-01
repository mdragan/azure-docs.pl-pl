---
title: Omówienie protokołu BGP i sieci VPN platformy Azure bram | Dokumentacja firmy Microsoft
description: Ten artykuł zawiera omówienie użycia protokołu BGP z bramami sieci VPN na platformie Azure.
services: vpn-gateway
author: yushwang
manager: rossort
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/12/2017
ms.author: yushwang
ms.openlocfilehash: 91e9fe1eb6b3df0b64d05f2b1e300403a9e01db9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60762285"
---
# <a name="about-bgp-with-azure-vpn-gateway"></a>Temat protokołu BGP z bramą sieci VPN platformy Azure
Ten artykuł zawiera omówienie obsługi protokołu BGP (Border Gateway Protocol) w usłudze Azure VPN Gateway.

BGP to standardowy protokół routingu używany często w Internecie do wymiany informacji o routingu i osiągalności między dwiema lub wieloma sieciami. W przypadku jego użycia w kontekście sieci wirtualnych Azure, protokół BGP umożliwia bramom sieci VPN na platformie Azure i lokalnym urządzeniom sieci VPN (nazywanym elementami równorzędnymi lub sąsiednimi BGP), przeprowadzaną za pośrednictwem bram lub routerów wymianę „tras” zawierających przeznaczone dla obu bram informacje na temat dostępności i osiągalności tych prefiksów. Protokół BGP umożliwia również włączenie routingu tranzytowego między wieloma sieciami poprzez propagowanie tras, których brama BGP uczy się od jednego elementu równorzędnego BGP, we wszystkich innych elementach równorzędnych BGP. 

## <a name="why"></a>Dlaczego warto używać protokołu BGP?
Protokół BGP stanowi funkcję opcjonalną, której można używać z bramami sieci VPN na platformie Azure opartymi na trasie. Przed włączeniem funkcji należy również upewnić się, że lokalne urządzenia sieci VPN obsługują protokół BGP. Bram sieci VPN na platformie Azure i lokalnych urządzeń sieci VPN można używać także bez protokołu BGP. Odpowiada to użyciu tras statycznych (bez stosowania protokołu BGP) *w porównaniu do* użycia routingu dynamicznego z zastosowaniem protokołu BGP między sieciami i platformą Azure.

Protokół BGP oferuje kilka zalet i nowych możliwości:

### <a name="prefix"></a>Obsługa automatycznych i elastycznych aktualizacji prefiksów
Za pomocą protokołu BGP wystarczy zadeklarować minimalny prefiks do określonego elementu równorzędnego protokołu BGP przez tunel VPN S2S protokołu IPsec. Może to być nawet sam prefiks hosta (/32) adresu IP elementu równorzędnego BGP dla lokalnego urządzenia sieci VPN. Można określić, które prefiksy sieci lokalnej mają być ogłaszane na platformie Azure w celu umożliwienia dostępu sieci wirtualnej Azure.

Można również ogłaszać dłuższe prefiksy, które mogą obejmować niektóre spośród prefiksy adresów sieci wirtualnej, takie jak duża przestrzeń prywatnych IP adresu (np. 10.0.0.0/8). Należy pamiętać, że prefiksy nie może być identyczny z jednym z prefiksy sieci wirtualnej. Trasy identyczne z prefiksami w sieci wirtualnej zostaną odrzucone.

### <a name="multitunnel"></a>Obsługa wielu tuneli między siecią wirtualną i lokacją lokalną za pomocą automatycznej pracy awaryjnej, w oparciu o Protokół BGP
Można utworzyć wiele połączeń między swoją siecią wirtualną na platformie Azure i lokalnymi urządzeniami sieci VPN w tej samej lokalizacji. Ta funkcja pozwala korzystać z wielu tuneli (ścieżek) między dwiema sieciami w konfiguracji aktywne/aktywne. Jeśli jeden z tuneli jest rozłączony, odpowiednie trasy zostaną wycofane za pośrednictwem protokołu BGP, a ruch jest kierowany do pozostałych tuneli.

Na poniższym diagramie przedstawiono prosty przykład tej konfiguracji charakteryzującej się wysoką dostępnością:

![Wiele aktywnych ścieżek](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

### <a name="transitrouting"></a>Obsługa routingu tranzytowego między sieciami w środowisku lokalnym i wieloma sieciami wirtualnymi platformy Azure
Protokół BGP umożliwia uzyskiwanie i propagowanie przez wiele bram prefiksów z różnych sieci, bez względu na to, czy są połączone bezpośrednio lub pośrednio. Umożliwia to realizowany przy użyciu bram sieci VPN na platformie Azure routing tranzytowy między lokalnymi lokacjami lub w obrębie wielu sieci wirtualnych Azure.

Na poniższym diagramie przedstawiono przykład topologii z wieloma przeskokami z wielu ścieżek, która umożliwia przesyłanie ruchu między dwiema sieciami lokalnymi za pośrednictwem bram sieci VPN na platformie Azure w ramach usługi Microsoft Networks:

![Przesyłanie z wieloma przeskokami](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="faq"></a>PROTOKÓŁ BGP — CZĘSTO ZADAWANE PYTANIA
[!INCLUDE [vpn-gateway-faq-bgp-include](../../includes/vpn-gateway-faq-bgp-include.md)]

## <a name="next-steps"></a>Kolejne kroki
Opis procedury konfigurowania protokołu BGP pod kątem połączeń obejmujących wiele lokalizacji i połączeń między sieciami wirtualnymi zawiera artykuł [Getting started with BGP on Azure VPN gateways](vpn-gateway-bgp-resource-manager-ps.md) (Rozpoczęcie pracy z protokołem BGP dla bram sieci VPN na platformie Azure).

