---
title: Skrypt programu Azure PowerShell — Azure Cosmos DB listy i Pobierz operacje dla programu SQL (rdzenie) interfejsu API
description: Skrypt programu Azure PowerShell — Azure Cosmos DB listy i Pobierz operacje dla programu SQL (rdzenie) interfejsu API
author: markjbrown
ms.service: cosmos-db
ms.topic: samples
ms.date: 05/18/2019
ms.author: mjbrown
ms.openlocfilehash: a2bcf8a123ffd19c84e2d39b38d0c9612ba43afb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66247352"
---
# <a name="list-and-get-databases-and-containers-for-azure-cosmos-db---sql-core-api"></a>Listy i pobieranie baz danych i kontenerów usługi Azure Cosmos DB - SQL (rdzenie) interfejsu API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-sql-list-get.ps1 "List or get databases or containers for SQL (Core) API")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia

Po wykonaniu przykładowego skryptu możesz uruchomić następujące polecenie, aby usunąć grupę zasobów i wszystkie skojarzone z nią zasoby.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Objaśnienia dla skryptu

W tym skrypcie użyto następujących poleceń. Każde polecenie w tabeli stanowi link do dokumentacji polecenia.

| Polecenie | Uwagi |
|---|---|
|**Zasoby platformy Azure**| |
| [Nowe AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | Tworzy zasób. |
|**Grupy zasobów platformy Azure**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Usuwa grupę zasobów wraz ze wszystkimi zagnieżdżonymi zasobami. |
|||

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat programu Azure PowerShell, zobacz [dokumentację programu Azure PowerShell](https://docs.microsoft.com/powershell/).

Więcej przykładowych skryptów programu PowerShell dla usługi Azure Cosmos DB można znaleźć w artykule [Azure Cosmos DB PowerShell scripts](../../../powershell-samples.md) (Skrypty programu PowerShell dla usługi Azure Cosmos DB).