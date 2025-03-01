---
title: Rozszerzenie maszyny wirtualnej platformy Azure Monitor zależności dla systemu Linux | Dokumentacja firmy Microsoft
description: Wdrażanie agenta usługi Azure Monitor zależności na maszynie wirtualnej systemu Linux przy użyciu rozszerzenia maszyny wirtualnej.
services: virtual-machines-linux
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2019
ms.author: magoedte
ms.openlocfilehash: 5faeebe799bd8cc0ba9a148508ac5b3a6d4b803a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67120210"
---
# <a name="azure-monitor-dependency-virtual-machine-extension-for-linux"></a>Rozszerzenie maszyny wirtualnej platformy Azure Monitor zależności dla systemu Linux

Usługi Azure Monitor dla funkcji mapy maszyny wirtualne, dane są pobierane z Microsoft Dependency agent. Rozszerzenie maszyny wirtualnej agenta zależności maszyn wirtualnych platformy Azure dla systemu Linux jest opublikowany i obsługiwane przez firmę Microsoft. Rozszerzenie instaluje agenta zależności na maszynach wirtualnych platformy Azure. W tym dokumencie przedstawiono obsługiwanych platform, konfiguracji i opcje wdrażania dla rozszerzenia maszyny wirtualnej agenta zależności maszyn wirtualnych platformy Azure dla systemu Linux.

## <a name="prerequisites"></a>Wymagania wstępne

### <a name="operating-system"></a>System operacyjny

Rozszerzenie agenta zależności maszyn wirtualnych platformy Azure dla systemu Linux mogą być uruchamiane względem obsługiwanych systemów operacyjnych wymienionych w [obsługiwane systemy operacyjne](../../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems) sekcji usługi Azure Monitor w artykule wdrożenie maszyn wirtualnych.

## <a name="extension-schema"></a>Schemat rozszerzenia

Następujący kod JSON zawiera schemat dla rozszerzenia agenta zależności maszyn wirtualnych platformy Azure na maszynie Wirtualnej systemu Linux platformy Azure. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "metadata": {
        "description": "The name of existing Linux Azure VM."
      }
    }
  },
  "variables": {
    "vmExtensionsApiVersion": "2017-03-30"
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "[concat(parameters('vmName'),'/DAExtension')]",
      "apiVersion": "[variables('vmExtensionsApiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
      ],
      "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentLinux",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
      }
    }
  ],
    "outputs": {
    }
}
```

### <a name="property-values"></a>Wartości właściwości

| Name (Nazwa) | Przykład/wartości |
| ---- | ---- |
| apiVersion | 2015-01-01 |
| publisher | Microsoft.Azure.Monitoring.DependencyAgent |
| — typ | DependencyAgentLinux |
| typeHandlerVersion | 9.5 |

## <a name="template-deployment"></a>Wdrażanie na podstawie szablonu

Można wdrożyć rozszerzeń maszyny Wirtualnej platformy Azure przy użyciu szablonów usługi Azure Resource Manager. Schemat JSON szczegółowo opisane w poprzedniej sekcji w szablonie usługi Azure Resource Manager umożliwia rozszerzenie agenta zależności maszyn wirtualnych platformy Azure są uruchamiane podczas wdrażania szablonu usługi Azure Resource Manager.

Kod JSON dla rozszerzenia maszyny wirtualnej może być zagnieżdżona w zasób maszyny wirtualnej. Możesz również umieścić go w katalogu głównego lub najwyższego poziomu szablon JSON usługi Resource Manager. Rozmieszczanie za pomocą pliku JSON ma wpływ na wartości nazwy i typu zasobu. Aby uzyskać więcej informacji, zobacz [Ustaw nazwę i typ zasobów podrzędnych](../../azure-resource-manager/resource-group-authoring-templates.md#child-resources).

W poniższym przykładzie założono, że rozszerzenia agenta zależności jest zagnieżdżona w obrębie zasobu maszyny wirtualnej. Możesz zagnieździć rozszerzenia zasobu, za pomocą pliku JSON jest umieszczany w `"resources": []` obiektu maszyny wirtualnej.


```json
{
    "type": "extensions",
    "name": "DAExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentLinux",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
    }
}
```

Po umieszczeniu rozszerzenia JSON w katalogu głównym szablonu Nazwa zasobu zawiera odwołanie do nadrzędnej maszyny wirtualnej. Typ odzwierciedla zagnieżdżonych konfiguracji. 

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/DAExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentLinux",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
    }
}
```

## <a name="azure-cli-deployment"></a>Wdrażania interfejs wiersza polecenia platformy Azure

Aby wdrożyć agenta zależności rozszerzenia maszyny Wirtualnej do istniejącej maszyny wirtualnej, można użyć wiersza polecenia platformy Azure.  

```azurecli

az vm extension set \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --name DependencyAgentLinux \
    --publisher Microsoft.Azure.Monitoring.DependencyAgent \
    --version 9.5 
```

## <a name="troubleshoot-and-support"></a>Rozwiązywanie problemów i pomocy technicznej

### <a name="troubleshoot"></a>Rozwiązywanie problemów

Dane dotyczące stanu wdrożeń rozszerzenia można pobrać z witryny Azure portal i za pomocą wiersza polecenia platformy Azure. Aby wyświetlić stan wdrożenia rozszerzeń dla danej maszyny Wirtualnej, uruchom następujące polecenie przy użyciu wiersza polecenia platformy Azure:

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Dane wyjściowe wykonywania rozszerzenia jest rejestrowany w następującym pliku:

```
/opt/microsoft/dependcency-agent/log/install.log 
```

### <a name="support"></a>Pomoc techniczna

Jeśli potrzebujesz dodatkowej pomocy w dowolnym momencie, w tym artykule, skontaktuj się z pomocą ekspertów platformy Azure na [forów platformy Azure z subskrypcją MSDN i Stack Overflow](https://azure.microsoft.com/support/forums/). Lub mogą zgłaszać zdarzenia pomocy technicznej platformy Azure. Przejdź do [witryny pomocy technicznej platformy Azure](https://azure.microsoft.com/support/options/) i wybierz **uzyskać pomoc techniczną**. Aby uzyskać informacji o korzystaniu z pomocy technicznej platformy Azure, przeczytaj [pomocy technicznej Microsoft Azure — często zadawane pytania](https://azure.microsoft.com/support/faq/).
