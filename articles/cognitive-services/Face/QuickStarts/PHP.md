---
title: 'Szybki start: Wykrywanie twarzy na obrazie przy użyciu interfejsu API REST i języka PHP'
titleSuffix: Azure Cognitive Services
description: W tym przewodniku Szybki start omówiono wykrywanie twarzy na obrazie przy użyciu interfejsu API rozpoznawania twarzy z językiem PHP.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 03/27/2019
ms.author: pafarley
ms.openlocfilehash: 280143d54e516fb626bb2d5afd01653e03d8a82c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60815454"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-php"></a>Szybki start: Wykrywanie twarzy na obrazie przy użyciu interfejsu API REST i języka PHP

W tym przewodniku Szybki start użyjesz interfejsu API REST rozpoznawania twarzy platformy Azure i języka PHP do wykrywania ludzkich twarzy na obrazie.

## <a name="prerequisites"></a>Wymagania wstępne

- Klucz subskrypcji interfejsu API rozpoznawania twarzy. Klucz subskrypcji bezpłatnej wersji próbnej możesz uzyskać na stronie [Wypróbuj usługi Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Możesz też wykonać instrukcje z tematu [Create a Cognitive Services account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) (Tworzenie konta usług Cognitive Services), aby subskrybować usługę interfejsu API rozpoznawania twarzy i uzyskać klucz.
- Edytor kodu, takie jak [programu Visual Studio Code](https://code.visualstudio.com/download).
- PHP [HTTP_Request2](https://pear.php.net/package/HTTP_Request2) pakietu.
- Przeglądarka sieci web z włączonymi PHP. Jeśli nie zdefiniowano to, możesz to zrobić przez zainstalowanie i skonfigurowanie [XAMPP](https://www.apachefriends.org/) na swojej maszynie.

## <a name="initialize-the-html-file"></a>Inicjowanie pliku HTML

Utwórz nowy plik HTML o nazwie *detectFaces.html* i dodaj poniższy kod.

```html
<html>
    <head>
        <title>Face Detect Sample</title>
    </head>
    <body></body>
</html>
```

## <a name="write-the-php-script"></a>Pisanie skryptu w języku PHP

Dodaj następujący kod wewnątrz elementu `body` dokumentu. Spowoduje to utworzenie podstawowego interfejsu użytkownika z polem adresu URL, przyciskiem **analizowania twarzy**, okienkiem odpowiedzi i okienkiem wyświetlania obrazu.

```php
<?php
// Replace <Subscription Key> with a valid subscription key.
$ocpApimSubscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to obtain
// your subscription keys. For example, if you obtained your subscription keys
// from westus, replace "westcentralus" in the URL below with "westus".
$uriBase = 'https://westcentralus.api.cognitive.microsoft.com/face/v1.0/';

$imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg';

// This sample uses the PHP5 HTTP_Request2 package
// (https://pear.php.net/package/HTTP_Request2).
require_once 'HTTP/Request2.php';

$request = new Http_Request2($uriBase . '/detect');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',
    'Ocp-Apim-Subscription-Key' => $ocpApimSubscriptionKey
);
$request->setHeader($headers);

$parameters = array(
    // Request parameters
    'returnFaceId' => 'true',
    'returnFaceLandmarks' => 'false',
    'returnFaceAttributes' => 'age,gender,headPose,smile,facialHair,glasses,' .
        'emotion,hair,makeup,occlusion,accessories,blur,exposure,noise');
$url->setQueryVariables($parameters);

$request->setMethod(HTTP_Request2::METHOD_POST);

// Request body parameters
$body = json_encode(array('url' => $imageUrl));

// Request body
$request->setBody($body);

try
{
    $response = $request->send();
    echo "<pre>" .
        json_encode(json_decode($response->getBody()), JSON_PRETTY_PRINT) . "</pre>";
}
catch (HttpException $ex)
{
    echo "<pre>" . $ex . "</pre>";
}
?>
```

Należy zaktualizować pole `subscriptionKey` wartością klucza subskrypcji. Konieczna może być również zmiana ciągu `uriBase` w taki sposób, aby zawierał on poprawny identyfikator regionu (zobacz [dokumentację interfejsu API rozpoznawania twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), aby zapoznać się z listą wszystkich punktów końcowych regionów). Pole `returnFaceAttributes` określa atrybuty twarzy do pobrania; możesz zmienić ten ciąg w zależności od planowanego użycia.

## <a name="run-the-script"></a>Uruchamianie skryptu

Otwórz plik w przeglądarce internetowej z obsługą języka PHP. Powinien zostać wyświetlony ciąg JSON danych twarzy, podobny do poniższego.

```json
[
    {
        "faceId": "e93e0db1-036e-4819-b5b6-4f39e0f73509",
        "faceRectangle": {
            "top": 621,
            "left": 616,
            "width": 195,
            "height": 195
        },
        "faceAttributes": {
            "smile": 0,
            "headPose": {
                "pitch": 0,
                "roll": 6.8,
                "yaw": 3.7
            },
            "gender": "male",
            "age": 37,
            "facialHair": {
                "moustache": 0.4,
                "beard": 0.4,
                "sideburns": 0.1
            },
            "glasses": "NoGlasses",
            "emotion": {
                "anger": 0,
                "contempt": 0,
                "disgust": 0,
                "fear": 0,
                "happiness": 0,
                "neutral": 0.999,
                "sadness": 0.001,
                "surprise": 0
            },
            "blur": {
                "blurLevel": "high",
                "value": 0.89
            },
            "exposure": {
                "exposureLevel": "goodExposure",
                "value": 0.51
            },
            "noise": {
                "noiseLevel": "medium",
                "value": 0.59
            },
            "makeup": {
                "eyeMakeup": true,
                "lipMakeup": false
            },
            "accessories": [],
            "occlusion": {
                "foreheadOccluded": false,
                "eyeOccluded": false,
                "mouthOccluded": false
            },
            "hair": {
                "bald": 0.04,
                "invisible": false,
                "hairColor": [
                    {
                        "color": "black",
                        "confidence": 0.98
                    },
                    {
                        "color": "brown",
                        "confidence": 0.87
                    },
                    {
                        "color": "gray",
                        "confidence": 0.85
                    },
                    {
                        "color": "other",
                        "confidence": 0.25
                    },
                    {
                        "color": "blond",
                        "confidence": 0.07
                    },
                    {
                        "color": "red",
                        "confidence": 0.02
                    }
                ]
            }
        }
    },
    {
        "faceId": "37c7c4bc-fda3-4d8d-94e8-b85b8deaf878",
        "faceRectangle": {
            "top": 693,
            "left": 1503,
            "width": 180,
            "height": 180
        },
        "faceAttributes": {
            "smile": 0.003,
            "headPose": {
                "pitch": 0,
                "roll": 2,
                "yaw": -2.2
            },
            "gender": "female",
            "age": 56,
            "facialHair": {
                "moustache": 0,
                "beard": 0,
                "sideburns": 0
            },
            "glasses": "NoGlasses",
            "emotion": {
                "anger": 0,
                "contempt": 0.001,
                "disgust": 0,
                "fear": 0,
                "happiness": 0.003,
                "neutral": 0.984,
                "sadness": 0.011,
                "surprise": 0
            },
            "blur": {
                "blurLevel": "high",
                "value": 0.83
            },
            "exposure": {
                "exposureLevel": "goodExposure",
                "value": 0.41
            },
            "noise": {
                "noiseLevel": "high",
                "value": 0.76
            },
            "makeup": {
                "eyeMakeup": false,
                "lipMakeup": false
            },
            "accessories": [],
            "occlusion": {
                "foreheadOccluded": false,
                "eyeOccluded": false,
                "mouthOccluded": false
            },
            "hair": {
                "bald": 0.06,
                "invisible": false,
                "hairColor": [
                    {
                        "color": "black",
                        "confidence": 0.99
                    },
                    {
                        "color": "gray",
                        "confidence": 0.89
                    },
                    {
                        "color": "other",
                        "confidence": 0.64
                    },
                    {
                        "color": "brown",
                        "confidence": 0.34
                    },
                    {
                        "color": "blond",
                        "confidence": 0.07
                    },
                    {
                        "color": "red",
                        "confidence": 0.03
                    }
                ]
            }
        }
    }
]
```

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się więcej na temat interfejsów API rozpoznawania twarzy używanych do wykrywania ludzkich twarzy na obrazach, rozpoznawania ich obrysów i zwracania atrybutów, takich jak wiek i płeć.

> [!div class="nextstepaction"]
> [Interfejsy API rozpoznawania twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
