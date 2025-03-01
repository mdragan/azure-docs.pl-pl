---
title: Tworzenie galerii udostępnionego obrazu przy użyciu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft
description: W tym artykule dowiesz się, jak tworzenie udostępnionego obrazu maszyny wirtualnej na platformie Azure za pomocą wiersza polecenia platformy Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: axayjo
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/06/2019
ms.author: akjosh; cynthn
ms.custom: ''
ms.openlocfilehash: bb6db6e5d5e33b7c7b5ba5a8711a06d6394b71f2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66226044"
---
# <a name="create-a-shared-image-gallery-with-the-azure-cli"></a>Utwórz galerię udostępnionego obrazu przy użyciu wiersza polecenia platformy Azure

A [galerii obrazów współdzielona](shared-image-galleries.md) upraszcza obraz niestandardowy do udostępniania całej organizacji. Obrazy niestandardowe są podobne do obrazów z platformy handlowej, ale tworzy się je samodzielnie. Obrazy niestandardowe mogą służyć do ładowania początkowego konfiguracji, na przykład do wstępnego ładowania aplikacji, konfiguracji aplikacji i innych konfiguracji systemu operacyjnego. 

Galeria obrazów współdzielona umożliwia udostępnianie niestandardowych obrazów maszyn wirtualnych z innymi osobami w organizacji, w ramach lub w wielu regionach, w ramach dzierżawy usługi AAD. Wybierz obrazy, które chcesz udostępnić, regiony mają być dostępne w nich i, którym chcesz udostępnić je za pomocą. Możesz utworzyć wiele galerii, dzięki czemu można grupować logicznie udostępnianych obrazów. 

Galeria jest zasobem najwyższego poziomu, który zapewnia kontrolę dostępu pełnej opartej na rolach (RBAC). Obrazy mogą być poddany kontroli wersji, a użytkownik może replikować każdej wersji obrazu z innym zestawem regiony platformy Azure. Galeria działa tylko z zarządzanych obrazów.

Funkcja galerii obrazów współdzielona ma wiele typów zasobów. Firma Microsoft będzie przy użyciu lub kompilowania w tym artykule:

| Resource | Opis|
|----------|------------|
| **Zarządzany obraz** | Jest to podstawowy obraz, które mogą być używane autonomicznie lub użyty do utworzenia **wersję obrazu** w galerii obrazów. Obrazy zarządzane są tworzone na podstawie uogólnionego maszyn wirtualnych. Zarządzany obraz to specjalny typ wirtualnego dysku twardego, który można utworzyć wiele maszyn wirtualnych i może teraz służyć do tworzenia wersji udostępnionego obrazu. |
| **Galeria obrazów** | W portalu Azure Marketplace, takich jak **galerii obrazów** to repozytorium do zarządzania i udostępniania obrazów, ale możesz kontrolować, kto ma dostęp. |
| **Definicję obrazu** | Obrazy w galerii są zdefiniowane i zawierają informacje dotyczące obrazu i wymagania dotyczące korzystania z niego wewnętrznie. Dotyczy to również, czy obraz jest Windows lub Linux, informacje o wersji i wymagań dotyczących minimalnej i maksymalnej pamięci. Jest definicja typu obrazu. |
| **Wersja obrazu** | **Wersję obrazu** , które jest używane do utworzenia maszyny Wirtualnej, podczas korzystania z galerii. Może mieć wiele wersji obrazu, zgodnie z potrzebami w danym środowisku. Gdy używasz, takie jak zarządzany obraz **wersję obrazu** tworzenie maszyny Wirtualnej, wersja obrazu jest używany do tworzenia nowych dysków dla maszyny Wirtualnej. Wersje obrazów można wielokrotnie. |



[!INCLUDE [virtual-machines-common-shared-images-cli](../../../includes/virtual-machines-common-shared-images-cli.md)]

## <a name="create-a-vm"></a>Tworzenie maszyny wirtualnej

Utwórz Maszynę wirtualną z najnowszą obraz wersji przy użyciu [tworzenie az vm](/cli/azure/vm#az-vm-create).

```azurecli-interactive 
az vm create\
   --resource-group myGalleryRG \
   --name myVM \
   --image "/subscriptions/subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition" \
   --generate-ssh-keys
```

Można również użyć określonej wersji za pomocą identyfikator wersji obrazu `--image` parametru. Na przykład, aby użyć wersji obrazu *1.0.0* typu: `--image "/subscriptions/<subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0"`.

[!INCLUDE [virtual-machines-common-gallery-list-cli](../../../includes/virtual-machines-common-gallery-list-cli.md)]

[!INCLUDE [virtual-machines-common-shared-images-update-delete-cli](../../../includes/virtual-machines-common-shared-images-update-delete-cli.md)]

## <a name="next-steps"></a>Kolejne kroki
[Kreator obrazów platformy Azure (wersja zapoznawcza)](image-builder-overview.md) można zautomatyzować tworzenie wersji obrazu, nawet służy do aktualizacji i [tworzy nowej wersji obrazu z istniejącą wersję obrazu](image-builder-gallery-update-image-version.md). 

Można również utworzyć zasoby udostępnione galerii obrazów za pomocą szablonów. Dostępnych jest kilka szablonów szybkiego startu platformy Azure: 

- [Tworzenie udostępnionego galerii obrazów](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Utwórz definicję obrazu w udostępnionym galerii obrazów](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Utwórz wersję obrazu w udostępnionym galerii obrazów](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Utwórz Maszynę wirtualną z obrazu wersji](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Aby uzyskać więcej informacji na temat udostępnione, galerie obrazów zobacz [Przegląd](shared-image-galleries.md). Jeśli napotkasz problemy, zobacz [Rozwiązywanie problemów udostępnione galerie obrazów](troubleshooting-shared-images.md).
