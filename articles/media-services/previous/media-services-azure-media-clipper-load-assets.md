---
title: Ładowanie zasobów do usługi Azure Media Clipper | Dokumentacja firmy Microsoft
description: Kroki dotyczące ładowania zasobów do usługi Azure Media Clipper
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 03/14/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: ec8cd06be78bbd8df0bca390696e736c3a6ee075
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61465885"
---
# <a name="loading-assets-into-azure-media-clipper"></a>Trwa ładowanie zasobów do usługi Azure Media Clipper  

Zasoby mogą zostać załadowane do usługi Azure Media Clipper, za pomocą dwóch metod:
1. Statycznie przekazując biblioteki zasobów
2. Dynamiczne generowanie listy zasobów za pomocą interfejsu API

## <a name="statically-load-videos-into-clipper"></a>Statycznie ładowanie filmów wideo do Clipper
Statycznie ładowania wideo Clipper, która umożliwia użytkownikom tworzenie klipów bez zaznaczania filmy wideo z panelu wyboru zasobów.

W tym przypadku są przekazywane w statyczny zestaw zasobów do Clipper. Każdy zasób zawiera identyfikator zasobu/filtru usługi AMS, nazwa, opublikowanego adresu URL przesyłania strumieniowego. Jeśli to konieczne, ochrony zawartości tokenu uwierzytelniania lub tablicę miniaturę adresy URL może być przekazywane w. Jeśli przekazany, miniatury są umieszczane w interfejsie. W scenariuszach, gdzie biblioteki zawartości jest statyczna i małe można przekazać w kontrakcie zasobów dla każdego zasobu w bibliotece.

> [!NOTE]
> Statycznie ładowanie zasobów do Clipper, zasoby są dodawane **bezpośrednio do osi czasu** i **okienko zasobów nie są odtwarzane**. Pierwszy element zawartości zostanie dodany do osi czasu i pozostałe zasoby są ułożone po prawej stronie na osi czasu).

Aby załadować bibliotekę zawartości statycznej, użyj **obciążenia** metodę, aby przekazać reprezentacja JSON każdego zasobu. Poniższy przykład kodu ilustruje reprezentacji JSON dla jednego zasobu.

```javascript
var assets = [
    {
      /* Required: represents the Azure Media Services asset Id when "type" === "asset"; otherwise, represents the dynamic manifest asset filter Id ("type" === "filter")  */
      "id": "my-asset-or-dynamic-manifest-asset-filter-id",
    
      /* Required: represents the asset name as shown in the Clipper interface */
      "name": "My Asset or Dynamic Manifest Asset Filter Name",
    
      /* Required: must be one of the following values: "asset" or "filter" */
      /* NOTE: "asset" type represents a rendered asset; "filter" type represents a dynamic manifest asset filter */
      "type": "asset",
    
      /* Required */
      "source": {
    
        /* Required: represents the asset streaming locator, the base Smooth Streaming URL */
        "src": "//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
    
        /* Optional: default value "application/vnd.ms-sstr+xml" */
        "type": "application/vnd.ms-sstr+xml",
    
        /* Required: If the asset has content protection applied, then you must include an array with the different protection types along with the token to request the license/key; otherwise, provide an empty array */
        "protectionInfo": [{
            "type": "AES",
            "authenticationToken": "Bearer aes-token-placeholder"
          },
          {
            "type": "PlayReady",
            "authenticationToken": "Bearer playready-token-placeholder"
          },
          {
            "type": "Widevine",
            "authenticationToken": "Bearer widevine-token-placeholder"
          },
          {
            "type": "FairPlay",
            "certificateUrl": "//example/path/to/myfairplay.der",
            "authenticationToken": "Bearer fairplay-token-placeholder"
          }
        ]
      },
    
      /* Optional: array containing thumbnail URLs for the video. */
      /* NOTE: For the thumbnail URLs to work as expected in the Clipper timeline they must be evenly distributed across the video (based on the duration) and in chronological order within the array. */
      "thumbnails": [
        "//example/path/thumbnail_001.jpg",
        "//example/path/thumbnail_002.jpg",
        "//example/path/thumbnail_003.jpg",
        "//example/path/thumbnail_004.jpg",
        "//example/path/thumbnail_005.jpg"
        ]
    }
];

var subclipper = new subclipper({
    selector: '#root',
    restVersion: '2.0',
    submitSubclipCallback: onSubmitSubclip,
});
subclipper.ready(function () {
    subclipper.load(assets);
});

```

> [!NOTE]
> Zaleca się łańcucha metody load() wywołanie metody ready(handler), jak pokazano w powyższym przykładzie. Poprzedni przykład gwarantuje, że widżet jest gotowy, przed załadowaniem zasoby.

> [!NOTE]
> Do miniatury adresy URL, aby działać zgodnie z oczekiwaniami na osi czasu Clipper musi zostać równomiernie rozłożone na wideo (oparte na czasie trwania) w kolejności chronologicznej w tablicy. Podczas generowania obrazów za pomocą procesor "Media Encoder Standard", można użyć następujących wstępnie zdefiniowanych fragmentu kodu JSON jako odwołanie do przykładowej:

```json
{
  "Start": "0",
  "Step": "00:00:05",
  "Range": "100%",
  "Type": "PngImage",
  "PngLayers": [
    {
      "Type": "PngLayer",
      "Width": 48,
      "Height": 26
    }
  ]
}
```

## <a name="dynamically-load-videos-in-clipper"></a>Dynamiczne ładowanie wideo w Clipper
Dynamiczne ładowanie filmów wideo do Clipper, aby umożliwić użytkownikom końcowym Wybieranie filmy wideo z panelu wyboru zasobów, kiedy należy przyciąć względem.

Alternatywnie można załadować zasoby dynamicznie za pośrednictwem wywołania zwrotnego. W scenariuszach, w których zasoby są generowane dynamicznie lub biblioteka jest duża powinny zostać załadowane za pośrednictwem wywołania zwrotnego. Aby załadować dynamicznie zasobów, musi implementować onLoadAssets opcjonalnych funkcji wywołania zwrotnego. Ta funkcja jest przekazywana do Clipper podczas inicjowania. Rozwiązany zasobów powinien spełniać te same kontrakty statycznie załadowanych zasoby. Poniższy przykład kodu ilustruje, podpis metody oczekiwanych danych wejściowych i oczekiwanych danych wyjściowych.

> [!NOTE]
> Po dynamiczne ładowanie zasobów do Clipper, zasoby są renderowane w **panel wyboru zasobów**.

```javascript
// Video Assets Pane Callback
    //
    // Filter Parameters:
    // - search: string value term that will be used in the back-end to filter assets by name.
    // - skip: int value used for pagination in the back-end that allows skipping a number of assets in the response.
    // - take: int value used for pagination in the back-end that allows defining the number of assets to include in the response.
    // - type: ('filter', 'asset') value that will be used in the back-end to filter assets by type.
    //
    // Returns: a Promise object that, when resolved, returns an object containing an array of assets (input contract)
    //          that satisfies the filter parameters, plus optionally the total types of files available:
    // {
    //  total: 100,
    //  assets: [{...}],
    // }
    var onLoadAssets = function (search, skip, take, type) {
        var promise = new Promise(function (resolve, reject) {
            // TODO: implement the getAssetsFromBackend method to get the assets from the back-end using the filter parameters (search, skip, take, type).
            getAssetsFromBackend(search, skip, take, type)
                .then(function (assets) {
                    resolve({
                        total: assets.length,
                        assets: assets
                    });
                }).catch(function () {
                    reject(Error("error details"));
                });
        });
    
        return promise;
    };

    // Create widget instance:
    // - using a root element selector
    // - enabling the Video Assets panel by registering a callback in the 'assetsPanelLoaderCallback' option parameter.
    var widget = new subclipper({
        selector: '#root',

        // Enable the Video Assets panel in the widget to automatically load assets (input contract)
        assetsPanelLoaderCallback: onLoadAssets
    });

    // ...
    // The widget will automatically invoke the 'assetsPanelLoaderCallback' callback with the filter parameters specified by the user 
    // and load assets returned by the Promise into the Video Assets panel.
    // ...
```
