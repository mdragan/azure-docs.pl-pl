---
title: Przykładowy skrypt interfejsu wiersza polecenia platformy Azure — tworzenie dwóch maszyn wirtualnych przy użyciu wewnętrznej i zewnętrznej sieciowej grupy zabezpieczeń | Microsoft Docs
description: Przykładowy skrypt interfejsu wiersza polecenia platformy Azure — tworzenie dwóch maszyn wirtualnych przy użyciu wewnętrznej i zewnętrznej sieciowej grupy zabezpieczeń
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: be4b42b84feaee9a7f0eb54a2d27bc4dba28b7d2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61127790"
---
# <a name="secure-network-traffic-between-virtual-machines"></a>Bezpieczny ruch sieciowy między maszynami wirtualnymi

Ten skrypt tworzy dwie maszyny wirtualne i zabezpiecza ruch przychodzący do obydwu. Pierwsza maszyna wirtualna jest dostępna w Internecie i ma sieciową grupę zabezpieczeń skonfigurowaną tak, aby zezwolić na ruch w portach 22 i 80. Pierwsza maszyna wirtualna nie jest dostępna w Internecie i ma sieciową grupę zabezpieczeń skonfigurowaną tak, aby zezwolić tylko na ruch z pierwszej maszyny wirtualnej.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-vm-nsg.sh "Create VM with NSG")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszynę wirtualną i wszystkie powiązane zasoby.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Objaśnienia dla skryptu

Ten skrypt zawiera następujące polecenia służące do tworzenia grupy zasobów, maszyny wirtualnej i wszystkich powiązanych zasobów. Każde polecenie w tabeli stanowi link do dokumentacji polecenia.

| Polecenie | Uwagi |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Tworzy grupę zasobów, w której są przechowywane wszystkie zasoby. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Tworzy sieć wirtualną i podsieć platformy Azure. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet) | Tworzy podsieć. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Tworzy maszynę wirtualną i łączy ją z kartą sieciową, siecią wirtualną, podsiecią i sieciową grupą zabezpieczeń. To polecenie określa również obraz maszyny wirtualnej do użycia oraz poświadczenia administracyjne.  |
| [az network nsg rule list](https://docs.microsoft.com/cli/azure/network/nsg/rule) | Zwraca informacje dotyczące reguły sieciowej grupy zabezpieczeń. W tym przykładzie nazwa reguły jest przechowywana w zmiennej na potrzeby późniejszego użycia w skrypcie. |
| [az network nsg rule update](https://docs.microsoft.com/cli/azure/network/nsg/rule) | Aktualizuje regułę sieciowej grupy zabezpieczeń. W tym przykładzie reguła zaplecza jest aktualizowana w celu przekazywania ruchu tylko z podsieci frontonu. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Usuwa grupę zasobów wraz ze wszystkimi zagnieżdżonymi zasobami. |

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat interfejsu wiersza polecenia platformy Azure, zobacz [dokumentację interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure).

Więcej przykładowych skryptów interfejsu wiersza polecenia maszyny wirtualnej można znaleźć w [dokumentacji dotyczącej maszyny wirtualnej platformy Azure z systemem Linux](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
