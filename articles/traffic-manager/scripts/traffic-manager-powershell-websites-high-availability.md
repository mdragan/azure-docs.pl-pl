---
title: Przykładowy skrypt programu PowerShell Azure — kierowanie ruchu w celu zapewnienia wysokiej dostępności aplikacji | Dokumentacja firmy Microsoft
description: Przykładowy skrypt programu PowerShell Azure — kierowanie ruchu w celu zapewnienia wysokiej dostępności aplikacji
services: traffic-manager
documentationcenter: traffic-manager
author: asudbring
manager: twooley
editor: ''
tags: azure-infrastructure
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 04/26/2018
ms.author: allensu
ms.openlocfilehash: 1c04859e2fe8841eb679f0b3e22b54ce71f88230
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050974"
---
# <a name="route-traffic-for-high-availability-of-applications-using-azure-powershell"></a>Kierowanie ruchem w celu zapewnienia wysokiej dostępności aplikacji przy użyciu programu Azure PowerShell

Ten skrypt tworzy grupę zasobów, dwóch planów usługi app service, dwie aplikacje internetowe, profilu usługi traffic manager i dwa punkty końcowe Menedżer ruchu. Usługa Traffic Manager kieruje ruch do aplikacji w jednym regionie w postaci regionu podstawowego, a także do regionu pomocniczego, gdy aplikacji w regionie głównym jest niedostępne. Przed wykonaniem skryptu, możesz zmienić wartości MyWebApp, MyWebAppL1 i MyWebAppL2 unikatowych wartości na platformie Azure. Po uruchomieniu skryptu dostęp do aplikacji w regionie podstawowym z mywebapp.trafficmanager.net adresu URL.

W razie potrzeby zainstaluj program Azure PowerShell, korzystając z instrukcji w [przewodniku programu Azure PowerShell](/powershell/azure), a następnie uruchom polecenie `Connect-AzAccount`, aby utworzyć połączenie z platformą Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]


Uruchom następujące polecenie, aby usunąć grupę zasobów, maszynę wirtualną i wszystkie powiązane zasoby.

```powershell
Remove-AzResourceGroup -Name myResourceGroup1
Remove-AzResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a>Objaśnienia dla skryptu

W tym skrypcie użyto następujących poleceń do utworzenia grupy zasobów, aplikacji internetowej, profilu usługi Traffic Manager i wszystkich powiązanych zasobów. Każde polecenie w tabeli stanowi link do dokumentacji polecenia.

| Polecenie | Uwagi |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)  | Tworzy grupę zasobów, w której są przechowywane wszystkie zasoby. |
| [New-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | Tworzy plan usługi App Service. Przypomina farmę serwerów dla aplikacji sieci web platformy Azure. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Tworzy aplikację internetową platformy Azure w ramach planu usługi App Service. |
| [Set-AzResource](/powershell/module/az.resources/new-azresource) | Tworzy aplikację internetową platformy Azure w ramach planu usługi App Service. |
| [New-AzTrafficManagerProfile](/powershell/module/az.trafficmanager/new-aztrafficmanagerprofile) | Tworzy profil usługi Azure Traffic Manager. |
| [New-AzTrafficManagerEndpoint](/powershell/module/az.trafficmanager/new-aztrafficmanagerendpoint) | Dodaje punkt końcowy do profilu usługi Azure Traffic Manager. |

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat programu Azure PowerShell, zobacz [dokumentację programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Dodatkowe przykłady skryptów programu PowerShell dla sieci można znaleźć w [dokumentacji i omówieniu sieci platformy Azure](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
