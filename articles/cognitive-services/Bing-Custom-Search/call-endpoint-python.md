---
title: 'Szybki start: Wywoływanie punktu końcowego wyszukiwania niestandardowego Bing przy użyciu języka Python | Microsoft Docs'
titlesuffix: Azure Cognitive Services
description: Użyj tego przewodnika Szybki start, aby rozpocząć żądanie wyników wyszukiwania z wystąpienia wyszukiwania niestandardowego Bing w języku Python
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: a6f8d3964c71d036053fe0d4136468fde690f872
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60593156"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-python"></a>Szybki start: Wywoływanie punktu końcowego wyszukiwania niestandardowego Bing przy użyciu języka Python

Użyj tego przewodnika Szybki start, aby rozpocząć żądanie wyników wyszukiwania z wystąpienia wyszukiwania niestandardowego Bing. Chociaż ta aplikacja jest napisana w języku Python, interfejs API wyszukiwania niestandardowego Bing jest usługą internetową RESTful zgodną z większością języków programowania. Kod źródłowy tego przykładu jest dostępny w usłudze [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingCustomSearchv7.py).

## <a name="prerequisites"></a>Wymagania wstępne

- Wystąpienie wyszukiwania niestandardowego Bing. Zobacz [Szybki start: Tworzenie pierwszego wystąpienia usługi wyszukiwania niestandardowego Bing](quick-start.md), aby uzyskać więcej informacji.
- Środowisko [Python](https://www.python.org/) 2.x lub 3.x

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Tworzenie i inicjowanie aplikacji

1. Utwórz nowy plik w języku Python w ulubionym środowisku IDE lub edytorze i dodaj następujące instrukcje importu. Utwórz zmienne dla swojego klucza subskrypcji, identyfikatora konfiguracji niestandardowej i terminu wyszukiwania. 

    ```python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    ```

## <a name="send-and-receive-a-search-request"></a>Wysyłanie i odbieranie żądania wyszukiwania 

1. Skonstruuj adres URL żądania, dodając termin wyszukiwania do parametru zapytania `q=` oraz identyfikator konfiguracji niestandardowej wystąpienia wyszukiwania do parametru `customconfig=`. Oddziel parametry za pomocą znaku `&`. 

    ```python
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 'q=' + searchTerm + '&' + 'customconfig=' + customConfigId
    ```

2. Wyślij żądanie do swojego wystąpienia wyszukiwania niestandardowego Bing i wyświetl zwrócone wyniki wyszukiwania.  

    ```python
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Tworzenie aplikacji internetowej z funkcją wyszukiwania niestandardowego](./tutorials/custom-search-web-page.md)
