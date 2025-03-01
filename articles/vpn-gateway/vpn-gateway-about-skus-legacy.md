---
title: Starsze sieci wirtualnej platformy Azure jednostki SKU bramy sieci VPN | Dokumentacja firmy Microsoft
description: Sposób pracy ze starymi jednostkami SKU; bramy sieci wirtualnej Basic, Standard i HighPerformance.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/10/2019
ms.author: cherylmc
ms.openlocfilehash: 00f1677e2691f9be5bb4584b07ca00340a52b1e1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056436"
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Praca z bramy sieci wirtualnej jednostki SKU (starszych jednostek SKU)

Ten artykuł zawiera informacje o starszych bramy sieci wirtualnej (stare) jednostki SKU. Starszych jednostek SKU mają nadal działać w obu modelach wdrażania dla bram sieci VPN, które zostały już utworzone. Klasyczne bramy sieci VPN w dalszym ciągu używać starszych jednostek SKU zarówno dla bramy istniejących i nowych bram. Podczas tworzenia nowego Menedżera zasobów sieci VPN bramy, należy użyć nowej jednostki SKU bramy. Aby uzyskać informacje o nowych jednostek SKU, zobacz [VPN Gateway — informacje](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Jednostki SKU bramy

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

Możesz wyświetlić starszych bramy cen w **bramy sieci wirtualnej** sekcji, która znajduje się na [stronę z cennikiem usługi ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute).

## <a name="agg"></a>Szacowana agregowana przepływność według jednostki SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Obsługiwane konfiguracje według typu jednostki SKU i sieci VPN

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Zmień rozmiar bramy

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Można zmienić rozmiar bramy do jednostki SKU bramy w ramach tej samej rodziny jednostki SKU. Na przykład jeśli masz standardowej jednostki SKU, możesz zmienić rozmiar do wartości jednostki SKU. Nie można jednak zmienić rozmiar bramy sieci VPN między starych jednostek SKU i nowych rodzin SKU. Nie można na przykład, przejdź od standardowej jednostki SKU, do jednostki SKU VpnGw2 lub podstawowej jednostki SKU do VpnGw1.

Aby zmienić rozmiar bramy dla klasycznego modelu wdrażania, użyj następującego polecenia:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Aby zmienić rozmiar bramy dla modelu wdrażania usługi Resource Manager przy użyciu programu PowerShell, użyj następującego polecenia:

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```
Można również zmienić rozmiar bramy w witrynie Azure portal.

## <a name="change"></a>Zmień na nowe jednostki SKU bramy

[!INCLUDE [Change to the new SKUs](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat nowej jednostki SKU bramy, zobacz [jednostki SKU bramy](vpn-gateway-about-vpngateways.md#gwsku).

Aby uzyskać więcej informacji na temat ustawień konfiguracji, zobacz [ustawienia konfiguracji bramy sieci VPN — informacje](vpn-gateway-about-vpn-gateway-settings.md).
