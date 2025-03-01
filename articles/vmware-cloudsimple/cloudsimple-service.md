---
title: VMware Solution by CloudSimple usługi
description: W tym artykule opisano omówienie CloudSimple usługi i pojęcia.
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6a4c0bc6070d372a279b74f81ac1f84f565559c3
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165237"
---
# <a name="cloudsimple-service-overview"></a>Omówienie usługi CloudSimple

W usłudze CloudSimple możesz użyć Azure VMware Solution by CloudSimple. Po utworzeniu usługi, można aprowizować węzły, rezerwa węzły i tworzenie chmur prywatnych. Utworzysz usługę CloudSimple w każdym regionie platformy Azure, w których usługa CloudSimple jest dostępna. Usługa zawiera definicję sieci krawędź Azure VMware Solution by CloudSimple. Sieć usługi edge obsługuje usług, które obejmują sieć VPN, usługi Azure ExpressRoute i łączność z Internetem do chmur prywatnych.

## <a name="gateway-subnet"></a>Podsieć bramy

Podsieć bramy jest wymagany na usługę CloudSimple i jest unikatowa w regionie, w którym zostanie utworzona. Podsieć bramy jest używany podczas tworzenia sieci krawędź i wymaga o rozmiarze/28 blok CIDR. Przestrzeń adresową podsieci bramy musi być unikatowa. Nie nakładać się z żadną siecią, która komunikuje się ze środowiskiem CloudSimple. Sieci, które komunikują się z CloudSimple składa się z sieciami lokalnymi i sieciami wirtualnymi platformy Azure. Nie można usunąć podsieci bramy, po jego utworzeniu. Podsieć bramy jest usuwany, gdy usługa zostanie usunięty.

## <a name="next-steps"></a>Kolejne kroki

* Dowiedz się, jak [utworzyć usługę CloudSimple na platformie Azure](quickstart-create-cloudsimple-service.md).
