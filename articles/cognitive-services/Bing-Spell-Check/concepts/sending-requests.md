---
title: Wysyłanie żądań do interfejsu API sprawdzania pisowni Bing
titlesuffix: Azure Cognitive Services
description: Poznaj tryby pracy funkcji sprawdzania pisowni Bing, jej ustawienia i inne informacje dotyczące tego interfejsu API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: overview
ms.date: 06/27/2019
ms.author: aahi
ms.openlocfilehash: 2b33c27e7af603c73bf7b7c6188bd9aef5ebc669
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542713"
---
# <a name="sending-requests-to-the-bing-spell-check-api"></a>Wysyłanie żądań do interfejsu API sprawdzania pisowni Bing

Aby sprawdzić ciąg tekstowy pod kątem błędów pisowni i gramatyki, należy przesłać żądanie GET do następującego punktu końcowego:  

```
https://api.cognitive.microsoft.com/bing/v7.0/spellcheck
```  
  
Żądanie musi używać protokołu HTTPS.

Zalecamy, aby wszystkie żądania pochodziły z serwera. Dystrybuowanie klucza w ramach aplikacji klienckiej dostarcza więcej okazji do przejęcia go przez złośliwy kod innych firm. Serwer zapewnia również jeden punkt uaktualniania dla przyszłych wersji interfejsu API.

Żądanie musi określać parametr zapytania [text](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#text), który zawiera ciąg tekstowy do sprawdzenia. Chociaż jest to opcjonalne, żądanie powinno również określać parametr zapytania [mkt](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#mkt), identyfikujący rynek, z którego mają pochodzić wyniki. Aby uzyskać listę opcjonalnych parametrów zapytania, takich jak `mode`, zobacz [Parametry zapytania](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#query-parameters). Wszystkie wartości parametrów zapytania muszą być zakodowane w adresie URL.  
  
Żądanie musi określać nagłówek [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#subscriptionkey). Mimo że jest to opcjonalne, zachęcamy, aby określić również następujące nagłówki. Dzięki tym nagłówkom interfejs API sprawdzania pisowni Bing zwraca dokładniejsze wyniki:  
  
-   [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#clientid)  
-   [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#clientip)  
-   [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#location)  

Aby uzyskać listę wszystkich nagłówków żądań i odpowiedzi, zobacz [Nagłówki](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#headers).

Podczas wywołania interfejsu API sprawdzania pisowni Bing za pomocą języka JavaScript funkcje zabezpieczeń wbudowane w przeglądarce mogą uniemożliwić dostęp do wartości tych nagłówków.

Aby rozwiązać ten problem, możesz przekazywać żądania interfejsu API sprawdzania pisowni Bing za pośrednictwem serwera proxy CORS. Odpowiedź z tego serwera proxy zawiera nagłówek `Access-Control-Expose-Headers`, który zezwala na nagłówki odpowiedzi i udostępnia je dla języka JavaScript.

Zainstalowanie serwera proxy CORS w celu zezwolenia [aplikacji samouczka](../tutorials/spellcheck.md) na dostęp do opcjonalnych nagłówków klienta jest łatwe. Najpierw [zainstaluj platformę Node.js](https://nodejs.org/en/download/), jeśli jeszcze jej nie masz. Potem wprowadź poniższe polecenie w wierszu polecenia.

    npm install -g cors-proxy-server

Następnie zmień punkt końcowy interfejsu API sprawdzania pisowni Bing w pliku HTML na:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/

Na koniec uruchom serwer proxy CORS za pomocą następującego polecenia:

    cors-proxy-server

Podczas korzystania z aplikacji samouczka pozostaw okno polecenia otwarte, ponieważ jego zamknięcie spowoduje zatrzymanie serwera proxy. W rozwijanej sekcji nagłówków HTML poniżej wyników wyszukiwania można teraz zobaczyć nagłówek `X-MSEdge-ClientID` (pomiędzy innymi) i sprawdzić, czy jest on taki sam dla każdego żądania.

## <a name="example-api-request"></a>Przykładowe żądanie interfejsu API

Poniżej przedstawiono żądanie, które zawiera wszystkie sugerowane parametry zapytania i nagłówki. Jeśli jest to Twoje pierwsze wywoływanie dowolnego z interfejsów API Bing, nie dołączaj nagłówka identyfikatora klienta. Identyfikator klienta należy uwzględnić tylko wtedy, gdy interfejs API Bing został już wywołany i usługa Bing zwróciła identyfikator klienta dla kombinacji użytkownika i urządzenia. 
  
> ```  
> GET https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE  
> X-MSEdge-ClientIP: 999.999.999.999  
> X-Search-Location: lat:47.60357;long:-122.3295;re:100  
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
> Host: api.cognitive.microsoft.com  
> ```  

Poniżej przedstawiono odpowiedź na poprzednie żądanie. W przykładzie pokazano również nagłówki odpowiedzi specyficzne dla usługi Bing.

[!INCLUDE [cognitive-services-bing-url-note](../../../../includes/cognitive-services-bing-url-note.md)]

```json
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  

## <a name="next-steps"></a>Kolejne kroki

- [Czym jest interfejs API sprawdzania pisowni Bing?](../overview.md)
- [Dokumentacja interfejsu API sprawdzania pisowni Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
