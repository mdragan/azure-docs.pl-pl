---
title: 'Szybki start: używanie zestawu SDK wyszukiwania w Internecie Bing dla języka Python'
titleSuffix: Azure Cognitive Services
description: Zestaw SDK wyszukiwania w Internecie Bing ułatwia integrowanie wyszukiwania w Internecie Bing z aplikacją w języku Python. Z tego przewodnika Szybki start dowiesz się, jak wysłać żądanie, odebrać odpowiedź JSON oraz filtrować i analizować wyniki.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 03/12/2019
ms.author: aahi
ms.openlocfilehash: d6758fc5434406e42acf65ff3b712227b5cec0f8
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991617"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-python"></a>Szybki start: używanie zestawu SDK wyszukiwania w Internecie Bing dla języka Python

Zestaw SDK wyszukiwania w Internecie Bing ułatwia integrowanie wyszukiwania w Internecie Bing z aplikacją w języku Python. Z tego przewodnika Szybki start dowiesz się, jak wysłać żądanie, odebrać odpowiedź JSON oraz filtrować i analizować wyniki.

Chcesz zobaczyć kod teraz? [Przykłady zastosowania zestawu SDK wyszukiwania w Internecie Bing dla języka Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) są dostępne w usłudze GitHub.

[!INCLUDE [bing-web-search-quickstart-signup](../../../includes/bing-web-search-quickstart-signup.md)]

Zobacz też [Cennik usług Cognitive Services — interfejs API wyszukiwania Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="prerequisites"></a>Wymagania wstępne
Zestaw SDK wyszukiwania w Internecie Bing jest zgodny ze środowiskiem Python w wersjach 2.7, 3.3, 3.4, 3.5 i 3.6. Na potrzeby tego przewodnika Szybki start zalecamy używanie środowiska wirtualnego.

* Środowisko Python w wersji 2.7, 3.3, 3.4, 3.5 lub 3.6
* [Moduł virtualenv](https://docs.python.org/3/tutorial/venv.html) dla środowiska Python w wersji 2.7
* [Moduł venv](https://pypi.python.org/pypi/virtualenv) dla środowiska Python w wersji 3.x

## <a name="create-and-configure-your-virtual-environment"></a>Tworzenie i konfigurowanie środowiska wirtualnego

Instrukcje dotyczące instalowania i konfigurowania środowiska wirtualnego różnią się dla języków Python 2.x i Python 3.x. Wykonaj następujące kroki, aby utworzyć i zainicjować środowisko wirtualne.

### <a name="python-2x"></a>Python 2.x

Utwórz środowisko wirtualne za pomocą modułu `virtualenv` dla środowiska Python 2.7:

```console
virtualenv mytestenv
```

Aktywuj środowisko:

```console
cd mytestenv
source bin/activate
```

Zainstaluj zależności zestawu SDK wyszukiwania w Internecie Bing:

```console
python -m pip install azure-cognitiveservices-search-websearch
```

### <a name="python-3x"></a>Python 3.x

Utwórz środowisko wirtualne za pomocą modułu `venv` dla środowiska Python 3.x:

```console
python -m venv mytestenv
```

Zainstaluj zależności zestawu SDK wyszukiwania w Internecie Bing:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-websearch
```

## <a name="create-a-client-and-print-your-first-results"></a>Tworzenie klienta i wyświetlanie pierwszych wyników

Teraz, po skonfigurowaniu środowiska wirtualnego i zainstalowaniu zależności, utwórzmy klienta. Klient będzie obsługiwać żądania kierowane do interfejsu API wyszukiwania w Internecie Bing i odpowiedzi odbierane z tego interfejsu.

Jeśli odpowiedź zawiera strony internetowe, obrazy, wiadomości lub wideo, zostanie wyświetlony pierwszy wynik dla każdego z nich.

1. Utwórz nowy projekt języka Python przy użyciu ulubionego środowiska IDE lub edytora.

1. Skopiuj ten przykładowy kod do projektu:  

    ```python
    # Import required modules.
    from azure.cognitiveservices.search.websearch import WebSearchAPI
    from azure.cognitiveservices.search.websearch.models import SafeSearch
    from msrest.authentication import CognitiveServicesCredentials

    # Replace with your subscription key.
    subscription_key = "YOUR_SUBSCRIPTION_KEY"

    # Instantiate the client and replace with your endpoint.
    client = WebSearchAPI(CognitiveServicesCredentials(subscription_key), base_url = "YOUR_ENDPOINT")

    # Make a request. Replace Yosemite if you'd like.
    web_data = client.web.search(query="Yosemite")
    print("\r\nSearched for Query# \" Yosemite \"")

    '''
    Web pages
    If the search response contains web pages, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.web_pages, 'value'):

        print("\r\nWebpage Results#{}".format(len(web_data.web_pages.value)))

        first_web_page = web_data.web_pages.value[0]
        print("First web page name: {} ".format(first_web_page.name))
        print("First web page URL: {} ".format(first_web_page.url))

    else:
        print("Didn't find any web pages...")

    '''
    Images
    If the search response contains images, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.images, 'value'):

        print("\r\nImage Results#{}".format(len(web_data.images.value)))

        first_image = web_data.images.value[0]
        print("First Image name: {} ".format(first_image.name))
        print("First Image URL: {} ".format(first_image.url))

    else:
        print("Didn't find any images...")

    '''
    News
    If the search response contains news, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.news, 'value'):

        print("\r\nNews Results#{}".format(len(web_data.news.value)))

        first_news = web_data.news.value[0]
        print("First News name: {} ".format(first_news.name))
        print("First News URL: {} ".format(first_news.url))

    else:
        print("Didn't find any news...")

    '''
    If the search response contains videos, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.videos, 'value'):

        print("\r\nVideos Results#{}".format(len(web_data.videos.value)))

        first_video = web_data.videos.value[0]
        print("First Videos name: {} ".format(first_video.name))
        print("First Videos URL: {} ".format(first_video.url))

    else:
        print("Didn't find any videos...")
    ```

1. Zastąp parametr `SUBSCRIPTION_KEY` prawidłowym kluczem subskrypcji.

1. Zastąp `YOUR_ENDPOINT` z adresem url punktu końcowego w portalu.

1. Uruchom program. Na przykład: `python your_program.py`.

## <a name="define-functions-and-filter-results"></a>Definiowanie funkcji i filtrowanie wyników

Teraz, gdy użytkownik wprowadził swoje pierwsze wywołanie interfejsu API wyszukiwania w sieci Web Bing, Przyjrzyjmy się kilku funkcji. W poniższych sekcjach omówiono funkcje zestawu SDK do uściślenia zapytania i filtrowanie wyników. Każda funkcja można dodać do programu Python, który został utworzony w poprzedniej sekcji.

### <a name="limit-the-number-of-results-returned-by-bing"></a>Ograniczanie liczby wyników zwracanych przez usługę Bing

W tym przykładzie użyto parametrów `count` i `offset`, aby ograniczyć liczbę wyników zwracanych przy użyciu metody [ `search` zestawu SDK](https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python). Wartości `name` i `url` są wyświetlane dla pierwszego wyniku.

1. Dodaj następujący kod do projektu języka Python:

   ```python
    # Declare the function.
    def web_results_with_count_and_offset(subscription_key):
        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

        try:
            '''
            Set the query, offset, and count using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(query="Best restaurants in Seattle", offset=10, count=20)
            print("\r\nSearching for \"Best restaurants in Seattle\"")

            if web_data.web_pages.value:
                '''
                If web pages are available, print the # of responses, and the first and second
                web pages returned.
                '''
                print("Webpage Results#{}".format(len(web_data.web_pages.value)))

                first_web_page = web_data.web_pages.value[0]
                print("First web page name: {} ".format(first_web_page.name))
                print("First web page URL: {} ".format(first_web_page.url))

            else:
                print("Didn't find any web pages...")

        except Exception as err:
            print("Encountered exception. {}".format(err))
    ```

1. Uruchom program.

### <a name="filter-for-news-and-freshness"></a>Filtrowanie według wiadomości i aktualności

W tym przykładzie użyto parametrów `response_filter` i `freshness`, aby przefiltrować wyniki wyszukiwania przy użyciu metody [ `search` zestawu SDK](/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations). Zwracane wyniki wyszukiwania są ograniczane do artykułów z wiadomościami i stron, które usługa Bing odnalazła w ciągu ostatnich 24 godzin. Wartości `name` i `url` są wyświetlane dla pierwszego wyniku.

1. Dodaj następujący kod do projektu języka Python:

    ```python
    # Declare the function.
    def web_search_with_response_filter(subscription_key):
        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))
        try:
            '''
            Set the query, response_filter, and freshness using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(query="xbox",
                response_filter=["News"],
                freshness="Day")
            print("\r\nSearching for \"xbox\" with the response filter set to \"News\" and freshness filter set to \"Day\".")

            '''
            If news articles are available, print the # of responses, and the first and second
            articles returned.
            '''
            if web_data.news.value:

                print("# of news results: {}".format(len(web_data.news.value)))

                first_web_page = web_data.news.value[0]
                print("First article name: {} ".format(first_web_page.name))
                print("First article URL: {} ".format(first_web_page.url))

                print("")

                second_web_page = web_data.news.value[1]
                print("\nSecond article name: {} ".format(second_web_page.name))
                print("Second article URL: {} ".format(second_web_page.url))

            else:
                print("Didn't find any news articles...")

        except Exception as err:
            print("Encountered exception. {}".format(err))

    # Call the function.
    web_search_with_response_filter(subscription_key)
    ```

1. Uruchom program.

### <a name="use-safe-search-answer-count-and-the-promote-filter"></a>Używanie filtrów safe search, answer count i promote

W tym przykładzie użyto parametrów `answer_count`, `promote` i `safe_search`, aby przefiltrować wyniki wyszukiwania przy użyciu metody [ `search` zestawu SDK](https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python). Wartości `name` i `url` są wyświetlane dla pierwszego wyniku.

1. Dodaj następujący kod do projektu języka Python:

    ```python
    # Declare the function.
    def web_search_with_answer_count_promote_and_safe_search(subscription_key):

        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

        try:
            '''
            Set the query, answer_count, promote, and safe_search parameters using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(
                query="Niagara Falls",
                answer_count=2,
                promote=["videos"],
                safe_search=SafeSearch.strict  # or directly "Strict"
            )
            print("\r\nSearching for \"Niagara Falls\"")

            '''
            If results are available, print the # of responses, and the first result returned.
            '''
            if web_data.web_pages.value:

                print("Webpage Results#{}".format(len(web_data.web_pages.value)))

                first_web_page = web_data.web_pages.value[0]
                print("First web page name: {} ".format(first_web_page.name))
                print("First web page URL: {} ".format(first_web_page.url))

            else:
                print("Didn't see any Web data..")

        except Exception as err:
            print("Encountered exception. {}".format(err))
    ```

1. Uruchom program.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Pamiętaj, aby po zakończeniu pracy z tym projektem usunąć klucz subskrypcji z kodu programu i zdezaktywować swoje środowisko wirtualne.

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Przykłady dotyczące zestawu SDK dla języka Python dla usług Cognitive Services](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

## <a name="see-also"></a>Zobacz także

* [Dokumentacja zestawu Azure Python SDK](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/websearch)
