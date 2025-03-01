---
title: Wymagania wstępne modułu usługi Azure IoT Edge | Portal Azure Marketplace
description: Wymagania wstępne dotyczące publikowania moduł usługi IoT Edge.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: pabutler
ms.openlocfilehash: a5d1d6fdaf07f8b27820021d4d2ac45ec67c9915
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64942098"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>Publikowanie wymagań wstępnych modułu usługi IoT Edge

W tym artykule opisano wymagania wstępne programu stawiane ofertom moduł usługi IoT Edge.  Jeśli nie zostało to jeszcze zrobione, zapoznaj się z [moduły usługi IoT Edge Podręcznik publikowania](../..//iot-edge-module.md).


## <a name="publishing-prerequisites"></a>Wymagania wstępne publikowania

Aby opublikować moduł usługi IoT Edge w portalu Azure Marketplace, należy spełnić następujące wymagania wstępne:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- Dostęp do [portalu dla partnerów w chmurze](https://cloudpartner.azure.com/). Aby uzyskać więcej informacji, zobacz [Podręcznik publikowania w portalu Azure Marketplace i AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Umowy [warunki witryny Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace-terms/)
- Hosta usługi IoT Edge modułu techniczne elementów zawartości w usłudze Azure Container Registry.  Aby uzyskać więcej informacji, zobacz [jak przygotować zasobów technicznych moduł usługi IoT Edge](./cpp-create-technical-assets.md)
- Ma metadane modułu usługi IoT Edge gotowe do użycia. Na przykład przygotować następujące zasoby:
    - Tytuł
    - Opis (format HTML)
    - Obraz logo (PNG format i rozmiary Stały obraz, w tym 40x40px 90x90px, 115x115px, 255x115px)
    - Okres użycia i zasady zachowania poufności
    - Domyślna konfiguracja modułu, która obejmuje: twin tras, żądanych właściwości, CreateOptions, można żądań i zmiennych środowiskowych.
    - Dokumentacja modułu
    - Kontakt z pomocą techniczną


## <a name="next-steps"></a>Kolejne kroki

Po utworzeniu [przygotowane element zawartości technicznej moduł usługi IoT Edge](./cpp-create-technical-assets.md), wszystko będzie gotowe do [Utwórz ofertę moduł usługi IoT Edge](./cpp-create-offer.md). 
