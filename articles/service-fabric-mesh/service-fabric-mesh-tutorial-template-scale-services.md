---
title: Samouczek — skalowanie aplikacji uruchomionej w usłudze Azure Service Fabric Mesh | Microsoft Docs
description: W tym samouczku dowiesz się, jak skalować usługi w aplikacji uruchomionej w usłudze Service Fabric Mesh.
services: service-fabric-mesh
documentationcenter: .net
author: dkkapur
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/11/2019
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: 31b1e7a777c65a270b7b08673fcd73a5ab63f11f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60810434"
---
# <a name="tutorial-scale-an-application-running-in-service-fabric-mesh"></a>Samouczek: Skalowanie aplikacji uruchomionej w usłudze Service Fabric Mesh

Ten samouczek jest drugą częścią serii. Dowiedz się, jak ręcznie skalować liczbę wystąpień usług aplikacji, którą [wcześniej wdrożono w usłudze Service Fabric Mesh](service-fabric-mesh-tutorial-template-deploy-app.md). Po zakończeniu będziesz mieć usługę frontonu z trzema wystąpieniami oraz usługę danych z dwoma wystąpieniami.

Część druga serii zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie żądanej liczby wystąpień usług
> * Wykonywanie uaktualnienia

Ta seria samouczków zawiera informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * [Wdrażanie aplikacji w usłudze Service Fabric Mesh przy użyciu szablonu](service-fabric-mesh-tutorial-template-deploy-app.md)
> * Skalowanie aplikacji uruchomionej w usłudze Service Fabric Mesh
> * [Uaktualnianie aplikacji uruchomionej w usłudze Service Fabric Mesh](service-fabric-mesh-tutorial-template-upgrade-app.md)
> * [Usuwanie aplikacji](service-fabric-mesh-tutorial-template-remove-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka:

* Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem możesz [utworzyć bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* [Zainstaluj lokalnie interfejs wiersza polecenia platformy Azure oraz interfejs wiersza polecenia usługi Service Fabric Mesh](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli).

## <a name="manually-scale-your-services-in-or-out"></a>Ręczne skalowanie usług w pionie lub poziomie

Jedną z głównych zalet wdrażania aplikacji w usłudze Service Fabric Mesh jest możliwość łatwego skalowania usług w pionie lub poziomie. Powinno to służyć do obsługi różnych ilości obciążenia w usługach lub do poprawiania dostępności.

W tym samouczku jako przykładu użyto listy zadań do wykonania, którą [wcześniej wdrożono](service-fabric-mesh-tutorial-template-deploy-app.md) i która powinna być teraz uruchomiona. Aplikacja ma dwie usługi: WebFrontEnd i ToDoService. Każdą z usług pierwotnie wdrożono z liczbą replik wynoszącą 1.  Aby wyświetlić liczbę uruchomionych replik usługi WebFrontEnd, uruchom następujące polecenie:

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp --query "replicaCount"
```

Aby wyświetlić liczbę uruchomionych replik usługi ToDoService, uruchom następujące polecenie:

```azurecli
az mesh service show --resource-group myResourceGroup --name ToDoService --app-name todolistapp --query "replicaCount"
```

W szablonie wdrażania dla zasobu aplikacji każda usługa ma właściwość *replicaCount*, która może służyć do ustawiania liczby wdrożeń usługi. Aplikacja może składać się z wielu usług, z których każda ma unikatową liczbę właściwości *replicaCount* oraz które są razem wdrażane i zarządzane. Aby skalować liczbę replik usług, zmodyfikuj wartość *replicaCount* dla każdej usługi, którą chcesz skalować, w szablonie wdrożenia lub pliku parametrów.  Następnie uaktualnij aplikację.

### <a name="modify-the-deployment-template-parameters"></a>Modyfikowanie parametrów szablonu wdrożenia

Gdy masz w szablonie wartości, które planujesz zmienić po wdrożeniu aplikacji, lub chcesz mieć możliwość ich zmiany w poszczególnych wdrożeniach (jeśli planujesz ponownie użyć tego szablonu dla innych wdrożeń), najlepszym rozwiązaniem jest sparametryzowanie wartości.

Wcześniej aplikacja została wdrożona przy użyciu plików [mesh_rp.windows.json deployment template](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json) i [mesh_rp.windows.parameter.json parameters](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json).

Otwórz plik [mesh_rp.windows.parameter.json parameters](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json) lokalnie i ustaw wartość *frontEndReplicaCount* na 3 oraz wartość *serviceReplicaCount* na 2:

```json
      "frontEndReplicaCount":{
        "value": "3"
      },
      "serviceReplicaCount":{
        "value": "2"
      }
```

Zapisz zmiany w pliku parametrów.  Parametry *frontEndReplicaCount* i *serviceReplicaCount* są deklarowane w sekcji *parameters* pliku [mesh_rp.windows.json deployment template](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json):

```json
"frontEndReplicaCount":{
      "defaultValue": "1",
      "type": "string"
    },
    "serviceReplicaCount":{
      "defaultValue": "1",
      "type": "string"
    }
```

Właściwość *replicaCount* usługi WebFrontEnd odwołuje się do parametru *frontEndReplicaCount*, a właściwość *replicaCount* usługi ToDoService odwołuje się do parametru *serviceReplicaCount*:

```json
    "services": [
    {
    "name": "WebFrontEnd",
    "properties": {
        "description": "WebFrontEnd description.",
        "osType": "Windows",
        "codePackages": [
        {
            ...
        }
        ],
        "replicaCount": "[parameters('frontEndReplicaCount')]",
        "networkRefs": [
        {
            "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'todolistappNetwork')]"
        }
        ]
    }
    },
    {
    "name": "ToDoService",
    "properties": {
        "description": "ToDoService description.",
        "osType": "Windows",
        "codePackages": [
        {
            ...
        }
        ],
        "replicaCount": "[parameters('serviceReplicaCount')]",
        "networkRefs": [
        {
            "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'todolistappNetwork')]"
        }
        ]
    }
    }
],
```

Po zmodyfikowaniu szablonu uaktualnij aplikację.

### <a name="upgrade-your-application"></a>Uaktualnianie aplikacji

Gdy aplikacja jest uruchomiona, możesz uaktualnić ją przez ponowne wdrożenie szablonu i zaktualizowanego pliku parametrów:

```azurecli
az mesh deployment create --resource-group myResourceGroup --template-file c:\temp\mesh_rp.windows.json --parameters c:\temp\mesh_rp.windows.parameters.json
```

Spowoduje to uruchomienie uaktualnienia stopniowego do aplikacji, a po kilku minutach powinno być widoczne zwiększenie liczby wystąpień usług.  Aby wyświetlić liczbę uruchomionych replik usługi WebFrontEnd, uruchom następujące polecenie:

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp --query "replicaCount"
```

Aby wyświetlić liczbę uruchomionych replik usługi ToDoService, uruchom następujące polecenie:

```azurecli
az mesh service show --resource-group myResourceGroup --name ToDoService --app-name todolistapp --query "replicaCount"
```

## <a name="next-steps"></a>Kolejne kroki

W tej części samouczka zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie żądanej liczby wystąpień usług
> * Wykonywanie uaktualnienia

Przejdź do następnego samouczka:
> [!div class="nextstepaction"]
> [Uaktualnianie aplikacji uruchomionej w usłudze Service Fabric Mesh](service-fabric-mesh-tutorial-template-upgrade-app.md)
