---
title: Rozpoznawanie jednostek za pomocą interfejsu API analizy tekstu
titleSuffix: Azure Cognitive Services
description: Dowiedz się, jak rozpoznać jednostki przy użyciu interfejsu API REST analizy tekstu.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 04/16/2019
ms.author: aahi
ms.openlocfilehash: ff4f9af82024e9d39ad89a39bcb2fe4130de9101
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304181"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Jak używać o nazwie rozpoznawania jednostek w analizy tekstu

[o nazwie jednostki interfejsu API rozpoznawania](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) przyjmuje tekstu bez struktury i dla każdego dokumentu JSON zwraca listę jednostek rozróżniane wraz z łączami do szczegółowych informacji w sieci web (Wikipedia i Bing). 

## <a name="entity-linking-and-named-entity-recognition"></a>Łączenie podmiotów i rozpoznawanie jednostek znaku

Text Analytics `entities` obsługuje punktu końcowego o nazwie rozpoznawanie jednostek (NER) i usługi entity linking.

### <a name="entity-linking"></a>Łączenie jednostek
Łączenie jednostek jest możliwość identyfikowania i odróżnić tożsamość jednostki w tekście (na przykład określenie, czy "Mars" jest używana jako planety lub Roman Boże z war). Ten proces wymaga obecności wiedzy, do którego został rozpoznany jednostki są połączone — Wikipedia jest używany jako knowledge base, aby `entities` punktu końcowego analizy tekstu.

### <a name="named-entity-recognition-ner"></a>Rozpoznawanie jednostek znaku (NER)
O nazwie rozpoznawanie jednostek (NER) to zdolność do identyfikacji różnych obiektów w tekście i kategoryzowanie je do wstępnie zdefiniowanych klas. Poniżej wymieniono obsługiwane klas jednostek.

W analizy tekstu [w wersji 2.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634), łączenie podmiotów i rozpoznawanie jednostek znaku (NER) są dostępne dla kilku języków. Zobacz [języki](../language-support.md#sentiment-analysis-key-phrase-extraction-and-named-entity-recognition) artykuł, aby uzyskać więcej informacji. 

### <a name="language-support"></a>Obsługa języków

Korzystanie z usługi entity linking w różnych językach wymaga, przy użyciu odpowiedniej wiedzy w każdym języku. W przypadku usługi entity linking w analizy tekstu, oznacza to każdy język, który jest obsługiwany przez `entities` punktu końcowego połączy się z odpowiedniego korpus Wikipedia, w tym języku. Ponieważ rozmiar korpusy różni się między językami oczekuje się, czy usługi entity linking odwołań w funkcji również będą się różnić.

## <a name="supported-types-for-named-entity-recognition"></a>Obsługiwane typy dla rozpoznawanie jednostek znaku

| Typ  | SubType | Przykład |
|:-----------   |:------------- |:---------|
| Person (Osoba)        | N/D\*         | "Jan", "Billa Gatesa"     |
| Lokalizacja      | N/D\*         | „Redmond, Washington”, „Paris”  |
| Organizacja  | N/D\*         | „Microsoft”   |
| Liczba      | Liczba        | „6”, „six”     | 
| Liczba      | Wartość procentowa    | „50%”, „fifty percent”| 
| Liczba      | Liczba porządkowa       | „2nd”, „second”     | 
| Liczba      | Zakres liczb   | „4 to 8”     | 
| Liczba      | Wiek           | "90 dni temu" lub "30 lat"    | 
| Liczba      | Waluta      | „$10,99”     | 
| Liczba      | Wymiar     | „10 miles”, „40 cm”     | 
| Liczba      | Temperatura   | „32 degrees”    |
| DateTime      | N/D\*         | „6:30PM February 4, 2012”      | 
| DateTime      | Date          | „May 2nd, 2017”, „05/02/2017”   | 
| DateTime      | Time          | "8: 00", "8:00"  | 
| DateTime      | Zakres dat     | „May 2nd to May 5th”    | 
| DateTime      | Zakres czasu     | „6pm to 7pm”     | 
| DateTime      | Czas trwania      | „1 minute and 45 seconds”   | 
| DateTime      | Zestaw           | „every Tuesday”     | 
| DateTime      | Strefa czasowa      |    | 
| Adres URL           | N/D\*         | "https:\//www.bing.com"    |
| Email         | N/D\*         | "support@contoso.com" |

\* W zależności od jednostki danych wejściowych i wyodrębnione może pominąć niektóre jednostki `SubType`.  Wszystkie wymienione typy obsługiwane jednostki są dostępne tylko w przypadku angielski, chiński uproszczony, francuskim, niemieckim i hiszpańskim językach.



## <a name="preparation"></a>Przygotowanie

Konieczne jest posiadanie dokumenty JSON w następującym formacie: Identyfikator, tekstu, język

Dla aktualnie obsługiwanych języków, zobacz [tej listy](../text-analytics-supported-languages.md).

Dokument musi mieć mniej niż 5120 znaków, a kolekcja może zawierać maksymalnie 1000 elementów (identyfikatorów). Kolekcja jest przesyłana w treści żądania. Poniższy przykład jest ilustrację zawartość, którą może przesłać w celu łączenia jednostek.

```
{"documents": [{"id": "1",
                "language": "en",
                "text": "Jeff bought three dozen eggs because there was a 50% discount."
                },
               {"id": "2",
                "language": "en",
                "text": "The Great Depression began in 1929. By 1933, the GDP in America fell by 25%."
                }
               ]
}
```    
    
## <a name="step-1-structure-the-request"></a>Krok 1: Określenie struktury żądania

Szczegółowe informacje na temat definicji żądania można znaleźć w artykule [Jak wywołać interfejs API analizy tekstu](text-analytics-how-to-call-api.md). Dla wygody poniżej ponownie podano odpowiednie kroki:

+ Utwórz żądanie **POST**. Zapoznaj się z dokumentacją interfejsu API dla tego żądania: [Interfejs API usługi Entity Linking](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

+ Ustaw punkt końcowy HTTP dla działania funkcji wydobywania podmiotów. Musi on obejmować zasób `/entities`: `https://[your-region].api.cognitive.microsoft.com/text/analytics/v2.1/entities`

+ Ustaw nagłówek żądania, tak aby zawierał klucz dostępu dla operacji analizy tekstu. Aby uzyskać więcej informacji, zobacz [How to find endpoints and access keys (Jak znajdować punkty końcowe i klucze dostępu)](text-analytics-how-to-access-key.md).

+ W treści żądania podaj kolekcję dokumentów JSON przygotowaną na potrzeby tej analizy.

> [!Tip]
> Użyj programu [Postman](text-analytics-how-to-call-api.md) lub otwórz **konsolę testowania interfejsu API** w [dokumentacji](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634), aby określić strukturę żądania i przesłać je do usługi za pomocą operacji POST.

## <a name="step-2-post-the-request"></a>Krok 2. Wysłanie żądania

Analiza jest wykonywana po odebraniu żądania. Zobacz [limity danych](../overview.md#data-limits) sekcja w przeglądzie, aby uzyskać informacje na temat rozmiaru i liczby żądań można wysyłać na minutę i sekundę.

Pamiętaj, że usługa jest bezstanowa. Żadne dane nie są przechowywane na koncie. Wyniki są zwracane natychmiast w odpowiedzi.

## <a name="step-3-view-results"></a>Krok 3. Wyświetlanie wyników

Wszystkie żądania POST zwracają odpowiedź w formacie JSON z identyfikatorami i wykrytymi właściwościami.

Dane wyjściowe są zwracane natychmiast. Wyniki można przesłać strumieniowo do aplikacji, która akceptuje kod JSON, lub zapisać do pliku w systemie lokalnym, a następnie zaimportować do aplikacji, która umożliwia sortowanie i wyszukiwanie danych oraz manipulowanie nimi.

Przykład danych wyjściowych do usługi entity linking pokazano dalej:

```json
{
    "Documents": [
        {
            "Id": "1",
            "Entities": [
                {
                    "Name": "Jeff",
                    "Matches": [
                        {
                            "Text": "Jeff",
                            "Offset": 0,
                            "Length": 4
                        }
                    ],
                    "Type": "Person"
                },
                {
                    "Name": "three dozen",
                    "Matches": [
                        {
                            "Text": "three dozen",
                            "Offset": 12,
                            "Length": 11
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50",
                    "Matches": [
                        {
                            "Text": "50",
                            "Offset": 49,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50%",
                    "Matches": [
                        {
                            "Text": "50%",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        },
        {
            "Id": "2",
            "Entities": [
                {
                    "Name": "Great Depression",
                    "Matches": [
                        {
                            "Text": "The Great Depression",
                            "Offset": 0,
                            "Length": 20
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Great Depression",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Great_Depression",
                    "BingId": "d9364681-98ad-1a66-f869-a3f1c8ae8ef8"
                },
                {
                    "Name": "1929",
                    "Matches": [
                        {
                            "Text": "1929",
                            "Offset": 30,
                            "Length": 4
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "By 1933",
                    "Matches": [
                        {
                            "Text": "By 1933",
                            "Offset": 36,
                            "Length": 7
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "Gross domestic product",
                    "Matches": [
                        {
                            "Text": "GDP",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Gross domestic product",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Gross_domestic_product",
                    "BingId": "c859ed84-c0dd-e18f-394a-530cae5468a2"
                },
                {
                    "Name": "United States",
                    "Matches": [
                        {
                            "Text": "America",
                            "Offset": 56,
                            "Length": 7
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "United States",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/United_States",
                    "BingId": "5232ed96-85b1-2edb-12c6-63e6c597a1de",
                    "Type": "Location"
                },
                {
                    "Name": "25",
                    "Matches": [
                        {
                            "Text": "25",
                            "Offset": 72,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "25%",
                    "Matches": [
                        {
                            "Text": "25%",
                            "Offset": 72,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        }
    ],
    "Errors": []
}
```


## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono pojęcia i przepływ pracy dotyczący łączenie podmiotów, za pomocą analizy tekstu w usługach Cognitive Services. Podsumowanie:

+ [Jednostki interfejsu API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) jest dostępny dla wybranych języków.
+ Dokumenty JSON w treści żądania obejmują identyfikator, tekstu i języka kodu.
+ Żądanie POST jest wysyłane do punktu końcowego `/entities` za pomocą spersonalizowanego [klucza dostępu i punktu końcowego](text-analytics-how-to-access-key.md) prawidłowego dla używanej subskrypcji.
+ Dane wyjściowe odpowiedzi, który składa się z połączonej jednostki (w tym pewność, że wyniki, przesunięcia i linków sieci web, dla każdego dokumentu Identyfikator) mogą być używane w dowolnej aplikacji

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Interfejs API analizy tekstu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

* [Text Analytics overview (Omówienie analizy tekstu)](../overview.md)  
* [Często zadawane pytania](../text-analytics-resource-faq.md)</br>
* [Strona produktu analizy tekstu](//go.microsoft.com/fwlink/?LinkID=759712) 
