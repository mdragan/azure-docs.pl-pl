---
title: 'Szybki start: Wykrywanie anomalii w danych szeregów czasowych za pomocą interfejsu API REST wykrywanie anomalii i C# | Dokumentacja firmy Microsoft'
description: Wykrywanie nieprawidłowości w serii danych, jako partii albo na strumieniu danych za pomocą interfejsu API wykrywanie anomalii.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 2a02b56c2fa0f99166cfde0f0089273ed2af4cb9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073207"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-c"></a>Szybki start: Wykrywanie anomalii w danych szeregów czasowych za pomocą interfejsu API REST wykrywanie anomalii iC# 

Użyj tego przewodnika Szybki Start, aby rozpocząć korzystanie z dwóch trybów wykrywania API wykrywanie anomalii do wykrycia anomalii w danych szeregów czasowych. To C# aplikacji wysyła dwa żądania interfejsu API, zawierająca dane szeregów czasowych w formacie JSON, a następnie pobiera odpowiedzi.

| Żądanie interfejsu API                                        | Dane wyjściowe aplikacji                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Wykrywaj anomalie jako zadania wsadowego                        | Odpowiedź JSON zawierający stan anomalii (i inne dane) dla każdego punktu danych w danych szeregów czasowych i pozycji wszelkie wykryte anomalie. |
| Wykrywanie anomalii stan najnowszego punktu danych | Odpowiedź JSON zawierający stan anomalii (i inne dane) najnowszy punkt danych w danych szeregów czasowych.                                                                                                                                         |

 Chociaż ta aplikacja jest napisana w języku C#, interfejs API jest usługą internetową zgodną z wzorcem REST i większością języków programowania.

## <a name="prerequisites"></a>Wymagania wstępne

- Dowolnej wersji programu [programu Visual Studio 2017 r. lub nowszej](https://visualstudio.microsoft.com/downloads/),

- Struktura [Json.NET](https://www.newtonsoft.com/json) dostępna jako pakiet NuGet. Aby zainstalować pakiet Newtonsoft.Json jako pakiet NuGet w programie Visual Studio:
    
    1. Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań**.
    2. Wybierz **Zarządzaj pakietami NuGet**.
    3. Wyszukaj *Newtonsoft.Json* i zainstalować pakiet.

- Jeśli używasz systemu Linux/MacOS tę aplikację można uruchomić za pomocą [Mono](https://www.mono-project.com/).

- Wskazuje JSON pliku zawierającego dane szeregów czasowych. Przykładowe dane dla tego przewodnika Szybki Start można znaleźć na [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

## <a name="create-a-new-application"></a>Tworzenie nowej aplikacji

1. W programie Visual Studio Utwórz nowe rozwiązanie konsoli i dodaj następujące pakiety. 

    ```csharp
    using System;
    using System.IO;
    using System.Net;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Utwórz zmienne dla swój klucz subskrypcji i punktu końcowego usługi. Poniżej przedstawiono identyfikatory URI, można użyć do wykrywania anomalii. Te będą dołączane do punktu końcowego usługi później, aby utworzyć interfejs API adresów URL żądania.

    |Metoda wykrywania  |Identyfikator URI  |
    |---------|---------|
    |Wykrywanie usługi Batch    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |Wykrywanie na najnowszy punkt danych     | `/anomalydetector/v1.0/timeseries/last/detect`        |
    
    ```csharp
    // Replace the subscriptionKey string value with your valid subscription key.
    const string subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";
    // Replace the endpoint URL with the correct one for your subscription. 
    // Your endpoint can be found in the Azure portal. For example: https://westus2.api.cognitive.microsoft.com
    const string endpoint = "[YOUR_ENDPOINT_URL]";
    // Replace the dataPath string with a path to the JSON formatted time series data.
    const string dataPath = "[PATH_TO_TIME_SERIES_DATA]";
    const string latestPointDetectionUrl = "/anomalydetector/v1.0/timeseries/last/detect";
    const string batchDetectionUrl = "/anomalydetector/v1.0/timeseries/entire/detect";
    ```

## <a name="create-a-function-to-send-requests"></a>Tworzenie funkcji na potrzeby wysyłania żądań

1. Tworzenie nowej funkcji asynchronicznej o nazwie `Request` przyjmującej zmienne utworzone powyżej.

2. Protokół zabezpieczeń klienta i informacje nagłówka przy użyciu `HttpClient` obiektu. Pamiętaj dodać klucz subskrypcji, aby `Ocp-Apim-Subscription-Key` nagłówka. Następnie utwórz `StringContent` obiekt dla żądania.

3. Wyślij żądanie za pomocą `PostAsync()`, a następnie zwraca odpowiedź.

```csharp
static async Task<string> Request(string apiAddress, string endpoint, string subscriptionKey, string requestData){
    using (HttpClient client = new HttpClient { BaseAddress = new Uri(apiAddress) }){
        System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12 | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls;
        client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

        var content = new StringContent(requestData, Encoding.UTF8, "application/json");
        var res = await client.PostAsync(endpoint, content);
        return await res.Content.ReadAsStringAsync();
    }
}
```

## <a name="detect-anomalies-as-a-batch"></a>Wykrywaj anomalie jako zadania wsadowego

1. Utwórz nową funkcję o nazwie `detectAnomaliesBatch()`. Konstruowania żądania i wysyłać je przez wywołanie metody `Request()` funkcji z punktu końcowego usługi, klucz subskrypcji, adres URL dla usługi batch, wykrywanie anomalii i danych szeregów czasowych.

2. Deserializacji obiektu JSON i zapisz je w konsoli.

3. Jeśli odpowiedź zawiera `code` pola, drukowania, kod błędu oraz komunikat o błędzie. 

4. W przeciwnym razie należy znaleźć pozycje anomalie w zestawie danych. Odpowiedź `isAnomaly` pole zawiera tablicę wartości logiczne, z których każdy określa, czy punkt danych jest anomalii. Czy przekonwertować to na tablicę ciągów, za pomocą obiektu odpowiedzi `ToObject<bool[]>()` funkcji. Wykonać iterację tablicy i drukowanie indeksu dowolnego `true` wartości. Te wartości odpowiadają indeks punktów danych nietypowe, jeśli którekolwiek.

```csharp
static void detectAnomaliesBatch(string requestData){
    System.Console.WriteLine("Detecting anomalies as a batch");

    var result = Request(
        endpoint,
        batchDetectionUrl,
        subscriptionKey,
        requestData).Result;

    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
    System.Console.WriteLine(jsonObj);

    if (jsonObj["code"] != null){
        System.Console.WriteLine($"Detection failed. ErrorCode:{jsonObj["code"]}, ErrorMessage:{jsonObj["message"]}");
    }
    else{
        bool[] anomalies = jsonObj["isAnomaly"].ToObject<bool[]>();
        System.Console.WriteLine("\nAnomalies detected in the following data positions:");
        for (var i = 0; i < anomalies.Length; i++){
            if (anomalies[i])
            {
                System.Console.Write(i + ", ");
            }
        }
    }
}
```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Wykrywanie anomalii stan najnowszego punktu danych

1. Utwórz nową funkcję o nazwie `detectAnomaliesLatest()`. Konstruowania żądania i wysyłać je przez wywołanie metody `Request()` funkcji z punktu końcowego usługi, klucz subskrypcji, adres URL do wykrywania anomalii w usłudze najnowszy punkt i danych szeregów czasowych.

2. Deserializacji obiektu JSON i zapisz je w konsoli.

```csharp
static void detectAnomaliesLatest(string requestData){
    System.Console.WriteLine("\n\nDetermining if latest data point is an anomaly");
    var result = Request(
        endpoint,
        latestPointDetectionUrl,
        subscriptionKey,
        requestData).Result;

    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
    System.Console.WriteLine(jsonObj);
}
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Ładowanie danych szeregów czasowych i wysłać żądanie

1. W metodzie głównej aplikacji, ładowanie danych szeregów czasowych JSON za pomocą `File.ReadAllText()`. 

2. Wywołanie funkcji wykrywania anomalii utworzonego powyżej. Użyj `System.Console.ReadKey()` do nie zamykaj okna konsoli po uruchomieniu aplikacji.

```csharp
static void Main(string[] args){

    var requestData = File.ReadAllText(dataPath);

    detectAnomaliesBatch(requestData);
    detectAnomaliesLatest(requestData);

    System.Console.ReadKey();
}
```

### <a name="example-response"></a>Przykładowa odpowiedź

Odpowiedź oznaczająca Powodzenie są zwracane w formacie JSON. Kliknij poniższe łącza, aby wyświetlić odpowiedź w formacie JSON w usłudze GitHub:
* [Przykładowa odpowiedź wykrywania usługi batch](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Przykład najnowszy punkt wykrywania odpowiedzi](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Dokumentacja interfejsu API REST](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)
