---
title: 'Szybki start: wykrywanie twarzy na obrazie przy użyciu interfejsu API REST platformy Azure i języka Java'
titleSuffix: Azure Cognitive Services
description: W tym przewodniku Szybki start użyjesz interfejsu API REST rozpoznawania twarzy platformy Azure i języka Java do wykrywania twarzy na obrazie.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 03/27/2019
ms.author: pafarley
ms.openlocfilehash: c0c1b9c1e9afc84e9702f6c1897d372a017be868
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60815599"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-java"></a>Szybki start: wykrywanie twarzy na obrazie przy użyciu interfejsu API REST i języka Java

W tym przewodniku Szybki start użyjesz interfejsu API REST rozpoznawania twarzy platformy Azure i języka Java do wykrywania ludzkich twarzy na obrazie.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 

## <a name="prerequisites"></a>Wymagania wstępne

- Klucz subskrypcji interfejsu API rozpoznawania twarzy. Klucz subskrypcji bezpłatnej wersji próbnej możesz uzyskać na stronie [Wypróbuj usługi Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Możesz też wykonać instrukcje z tematu [Create a Cognitive Services account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) (Tworzenie konta usług Cognitive Services), aby subskrybować usługę interfejsu API rozpoznawania twarzy i uzyskać klucz.
- Wybrane środowisko IDE Java.

## <a name="create-the-java-project"></a>Tworzenie projektu języka Java

1. Utwórz nową aplikację wiersza polecenia języka Java w środowisku IDE i dodaj klasę **Main** z metodą **main**.
1. Zaimportuj poniższe biblioteki do projektu języka Java. Jeśli używasz programu Maven, współrzędne programu Maven są udostępniane w każdej bibliotece.
   - [Klienta Apache HTTP](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.6)
   - [Podstawowe Apache HTTP](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.10)
   - [Biblioteka JSON](https://github.com/stleary/JSON-java) (org.json:json:20180130)
   - [Rejestrowanie Apache Commons](https://commons.apache.org/proper/commons-logging/download_logging.cgi) (commons — rejestrowanie: commons — rejestrowanie: 1.1.2)

## <a name="add-face-detection-code"></a>Dodawanie kodu służącego do wykrywania twarzy

Otwórz główną klasę projektu. W tym pliku dodasz kod wymagany do ładowania obrazów i wykrywania twarzy.

### <a name="import-packages"></a>Importowanie pakietów

Dodaj poniższe instrukcje `import` na początku pliku.

```java
// This sample uses Apache HttpComponents:
// http://hc.apache.org/httpcomponents-core-ga/httpcore/apidocs/
// https://hc.apache.org/httpcomponents-client-ga/httpclient/apidocs/

import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONArray;
import org.json.JSONObject;
```

### <a name="add-essential-fields"></a>Dodawanie podstawowych pól

Zastąp **Main** klasy z następującym kodem. Te dane służą do określania sposobu nawiązywania połączenia z usługą rozpoznawania twarzy i lokalizacji, z której można pobrać dane wejściowe. Należy zaktualizować pole `subscriptionKey` wartością klucza subskrypcji. Konieczna może być również zmiana ciągu `uriBase` w taki sposób, aby zawierał on poprawny identyfikator regionu (zobacz [dokumentację interfejsu API rozpoznawania twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), aby zapoznać się z listą wszystkich punktów końcowych regionów). Możesz również ustawić wartość `imageWithFaces` na ścieżkę, która wskazuje na inny plik obrazu.

Pole `faceAttributes` to po prostu lista atrybutów określonych typów. Będzie ono określać informacje dotyczące wykrytych twarzy, które mają zostać pobrane.

```Java
public class Main {
    // Replace <Subscription Key> with your valid subscription key.
    private static final String subscriptionKey = "<Subscription Key>";

    // NOTE: You must use the same region in your REST call as you used to
    // obtain your subscription keys. For example, if you obtained your
    // subscription keys from westus, replace "westcentralus" in the URL
    // below with "westus".
    //
    // Free trial subscription keys are generated in the "westus" region. If you
    // use a free trial subscription key, you shouldn't need to change this region.
    private static final String uriBase =
        "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect";

    private static final String imageWithFaces =
        "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}";

    private static final String faceAttributes =
        "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";
```

### <a name="call-the-face-detection-rest-api"></a>Wywoływanie interfejsu API REST wykrywania twarzy

Dodaj **głównego** metoda następującym kodem. Tworzy on wywołanie REST do interfejsu API rozpoznawania twarzy w celu wykrycia informacji o twarzy na obrazie zdalnym (ciąg `faceAttributes` określa, które atrybuty dotyczące twarzy mają zostać pobrane). Następnie zapisuje dane wyjściowe do ciągu JSON.

```Java
    public static void main(String[] args) {
        HttpClient httpclient = HttpClientBuilder.create().build();

        try
        {
            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameters. All of them are optional.
            builder.setParameter("returnFaceId", "true");
            builder.setParameter("returnFaceLandmarks", "false");
            builder.setParameter("returnFaceAttributes", faceAttributes);

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity reqEntity = new StringEntity(imageWithFaces);
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();
```

### <a name="parse-the-json-response"></a>Analizowanie odpowiedzi w formacie JSON

Bezpośrednio poniżej powyższego kodu dodaj następujący blok, który służy do konwertowania zwróconych danych JSON do bardziej czytelnego formatu przed wyświetleniem ich w konsoli. Na koniec zamknąć bloku try / catch **głównego** metody i **Main** klasy.

```Java
            if (entity != null)
            {
                // Format and display the JSON response.
                System.out.println("REST Response:\n");

                String jsonString = EntityUtils.toString(entity).trim();
                if (jsonString.charAt(0) == '[') {
                    JSONArray jsonArray = new JSONArray(jsonString);
                    System.out.println(jsonArray.toString(2));
                }
                else if (jsonString.charAt(0) == '{') {
                    JSONObject jsonObject = new JSONObject(jsonString);
                    System.out.println(jsonObject.toString(2));
                } else {
                    System.out.println(jsonString);
                }
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
        }
    }
}
```

## <a name="run-the-app"></a>Uruchamianie aplikacji

Skompiluj kod i uruchom go. Odpowiedź oznaczająca powodzenie będzie zawierać dane dotyczące rozpoznawania twarzy w czytelnym formacie JSON w oknie konsoli. Na przykład:

```json
[{
  "faceRectangle": {
    "top": 131,
    "left": 177,
    "width": 162,
    "height": 162
  },
  "faceAttributes": {
    "makeup": {
      "eyeMakeup": true,
      "lipMakeup": true
    },
    "facialHair": {
      "sideburns": 0,
      "beard": 0,
      "moustache": 0
    },
    "gender": "female",
    "accessories": [],
    "blur": {
      "blurLevel": "low",
      "value": 0.06
    },
    "headPose": {
      "roll": 0.1,
      "pitch": 0,
      "yaw": -32.9
    },
    "smile": 0,
    "glasses": "NoGlasses",
    "hair": {
      "bald": 0,
      "invisible": false,
      "hairColor": [
        {
          "color": "brown",
          "confidence": 1
        },
        {
          "color": "black",
          "confidence": 0.87
        },
        {
          "color": "other",
          "confidence": 0.51
        },
        {
          "color": "blond",
          "confidence": 0.08
        },
        {
          "color": "red",
          "confidence": 0.08
        },
        {
          "color": "gray",
          "confidence": 0.02
        }
      ]
    },
    "emotion": {
      "contempt": 0,
      "surprise": 0.005,
      "happiness": 0,
      "neutral": 0.986,
      "sadness": 0.009,
      "disgust": 0,
      "anger": 0,
      "fear": 0
    },
    "exposure": {
      "value": 0.67,
      "exposureLevel": "goodExposure"
    },
    "occlusion": {
      "eyeOccluded": false,
      "mouthOccluded": false,
      "foreheadOccluded": false
    },
    "noise": {
      "noiseLevel": "low",
      "value": 0
    },
    "age": 22.9
  },
  "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f"
}]
```

## <a name="next-steps"></a>Kolejne kroki

W tym przewodniku Szybki start utworzono prostą aplikację konsolową Java, która korzysta z wywołań REST wraz z interfejsem API rozpoznawania twarzy platformy Azure do wykrywania twarzy na obrazie i zwrócenia ich atrybutów. Następnie dowiesz się, jak wykonać inne operacje za pomocą tych funkcji w aplikacji systemu Android.

> [!div class="nextstepaction"]
> [Samouczek: tworzenie aplikacji systemu Android wykrywającej i oznaczającej ramką twarze](../Tutorials/FaceAPIinJavaForAndroidTutorial.md)
