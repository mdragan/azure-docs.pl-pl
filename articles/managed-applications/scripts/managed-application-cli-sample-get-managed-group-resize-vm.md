---
title: Przykładowy skrypt interfejsu wiersza polecenia platformy Azure — pobieranie zarządzanej grupy zasobów i zmienianie rozmiaru maszyn wirtualnych | Microsoft Docs
description: Przykładowy skrypt interfejsu wiersza polecenia platformy Azure — pobieranie zarządzanej grupy zasobów i zmienianie rozmiaru maszyn wirtualnych
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: bbf03a0d53769c93a8aab304d3128ae0cc875a8f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61365898"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>Pobieranie zasobów z zarządzanej grupy zasobów i zmienianie rozmiaru maszyn wirtualnych za pomocą interfejsu wiersza polecenia platformy Azure

Ten skrypt pobiera zasoby z zarządzanej grupy zasobów i zmienia rozmiar maszyn wirtualnych w tej grupie zasobów.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/managed-applications/get-application/get-application.sh "Get application")]


## <a name="script-explanation"></a>Objaśnienia dla skryptu

Ten skrypt używa następujących poleceń do wdrożenia aplikacji zarządzanej. Każde polecenie w tabeli stanowi link do dokumentacji polecenia.

| Polecenie | Uwagi |
|---|---|
| [az managedapp list](https://docs.microsoft.com/cli/azure/managedapp#az-managedapp-list) | Wyświetla listę aplikacji zarządzanych. Należy podać wartości zapytania, aby zawęzić wyniki. |
| [az resource list](https://docs.microsoft.com/cli/azure/resource#az-resource-list) | Wyświetla listę zasobów. Należy podać grupę zasobów i wartości zapytania, aby zawęzić wyniki. |
| [az vm resize](https://docs.microsoft.com/cli/azure/vm#az-vm-resize) | Aktualizuje rozmiar maszyny wirtualnej. |


## <a name="next-steps"></a>Kolejne kroki

* Aby zapoznać się z wprowadzeniem do aplikacji zarządzanych, zobacz [Azure Managed Application overview](../overview.md) (Omówienie aplikacji zarządzanych platformy Azure).
* Aby uzyskać więcej informacji na temat interfejsu wiersza polecenia platformy Azure, zobacz [dokumentację interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure).
