---
title: Regiony, w których jest dostępna — indeksator wideo platformy Azure
titlesuffix: Azure Media Services
description: Ten artykuł zawiera informacje o regionach platformy Azure, w których dostępna jest Video Indexer.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: 404aaf91c0cb30df0a83353ef7397987ec3f8e80
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65799415"
---
# <a name="azure-regions-in-which-video-indexer-exists"></a>Regiony platformy Azure, w których istnieje indeksatora wideo

Zawiera interfejsów API klipów wideo indeksatora **lokalizacji** parametr, który należy ustawić w regionie platformy Azure, do którego powinny być kierowane wywołania. Musi to być [region platformy Azure, w której jest dostępny Video Indexer](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).

## <a name="locations"></a>Lokalizacje

**Lokalizacji** parametr należy podać nazwę kod regionu platformy Azure jako jego wartość. Jeśli używasz indeksatora wideo w wersji zapoznawczej, należy umieścić *"próbne"* jako wartość. W przeciwnym razie można uzyskać nazwy kodu, czy Twoje konto jest w i czy połączenia powinny być kierowane do regionu platformy Azure, możesz uruchomić następujący wiersz w [wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest):

```bash
az account list-locations
```

Po uruchomieniu wiersz powyżej, zostanie wyświetlona lista wszystkich regionów świadczenia usługi Azure. Przejdź do regionu platformy Azure, która ma *displayName* szukasz i używać jej *nazwa* wartość **lokalizacji** parametru.

Na przykład dla regionu platformy Azure zachodnie stany USA 2 (wyświetlane poniżej), użytkownik użyje "westus2" **lokalizacji** parametru.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/35c2594a-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="next-steps"></a>Kolejne kroki

- [Dostosuj model języka, za pomocą interfejsów API](customize-language-model-with-api.md)
- [Dostosuj model marek przy użyciu interfejsów API](customize-brands-model-with-api.md)
- [Dostosuj model osoby za pomocą interfejsów API](customize-person-model-with-api.md)
