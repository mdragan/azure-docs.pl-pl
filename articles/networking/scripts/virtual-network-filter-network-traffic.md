---
title: Przykładowy skrypt interfejsu wiersza polecenia platformy Azure — filtrowanie ruchu sieciowego maszyny wirtualnej | Microsoft Docs
description: Przykładowy skrypt interfejsu wiersza polecenia platformy Azure — filtrowanie przychodzącego i wychodzącego ruchu sieciowego maszyny wirtualnej.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: 6045cabdc97dad72a54e28a6f4fbee3126056d2e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60624041"
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Filtrowanie ruchu przychodzącego i wychodzącego ruchu sieciowego maszyny Wirtualnej

Ten przykładowy skrypt tworzy sieć wirtualną z podsieciami frontonu i zaplecza. Ruch sieciowy przychodzący do podsieci frontonu jest ograniczony do protokołów HTTP, HTTPS i SSH, a ruch wychodzący do Internetu z podsieci zaplecza jest niedozwolony. Po uruchomieniu skryptu będziesz mieć jedną maszynę wirtualną z dwiema kartami sieciowymi. Każda karta sieciowa jest połączona z inną podsiecią.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszynę wirtualną i wszystkie powiązane zasoby.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Objaśnienia dla skryptu

Ten skrypt zawiera następujące polecenia służące do tworzenia grupy zasobów, sieci wirtualnej i sieciowych grup zabezpieczeń. Każde polecenie w tabeli stanowi link do dokumentacji polecenia.

| Polecenie | Uwagi |
|---|---|
| [az group create](/cli/azure/group) | Tworzy grupę zasobów, w której są przechowywane wszystkie zasoby. |
| [az network vnet create](/cli/azure/network/vnet) | Tworzy sieć wirtualną i podsieć frontonu platformy Azure. |
| [az network subnet create](/cli/azure/network/vnet/subnet) | Tworzy podsieć zaplecza. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet) | Tworzy skojarzenia sieciowych grup zabezpieczeń z podsieciami. |
| [az network public-ip create](/cli/azure/network/public-ip) | Tworzy publiczny adres IP, dostęp do maszyny Wirtualnej z Internetu. |
| [az network nic create](/cli/azure/network/nic) | Tworzy interfejsy sieci wirtualnej i dołącza je do podsieci frontonu i zaplecza sieci wirtualnej. |
| [az network nsg create](/cli/azure/network/nsg) | Tworzy sieciowe grupy zabezpieczeń, które zostały skojarzone z podsieciami frontonu i zaplecza. |
| [az network nsg rule create](/cli/azure/network/nsg/rule) |Tworzy reguły sieciowych grup zabezpieczeń, które zezwalają na użycie określonych portów w wybranych sieciach lub blokują te porty. |
| [az vm create](/cli/azure/vm) | Tworzy maszyny wirtualne i dołącza kartę sieciową do każdej maszyny wirtualnej. To polecenie określa również obraz maszyny wirtualnej do użycia oraz poświadczenia administracyjne. |
| [az group delete](/cli/azure/group) | Usuwa grupę zasobów i wszystkie zasoby, które zawiera. |

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat interfejsu wiersza polecenia platformy Azure, zobacz [dokumentację interfejsu wiersza polecenia platformy Azure](/cli/azure).

Więcej przykładowych skryptów interfejsu wiersza polecenia platformy sieci można znaleźć w [dokumentacji platformy Azure, omówienie sieci](../cli-samples.md)
