---
title: Jednostki rozpoznawania wyszukiwania kognitywnego umiejętności — usługa Azure Search
description: Wyodrębnij różnych typów jednostek z pliku tekstowego w potoku usługi Azure Search w usłudze wyszukiwania poznawczego.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: f05161dbbfd9293cd7b1cbf447bb7ca1c313250c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65023443"
---
#    <a name="entity-recognition-cognitive-skill"></a>Umiejętności cognitive rozpoznawania jednostek

**Rozpoznawanie jednostek** umiejętności wyodrębnia jednostek o różnych typach z pliku tekstowego. Modele dostarczone przez uczenia maszynowego korzysta z tej umiejętności [analizy tekstu](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) w usługach Cognitive Services.

> [!NOTE]
> Możesz rozwiń zakres, zwiększając częstotliwości przetwarzania, dodając więcej dokumentów lub dodanie więcej algorytmów sztucznej Inteligencji, konieczne będzie [dołączyć płatnych zasobu usług Cognitive Services](cognitive-search-attach-cognitive-services.md). Opłaty są naliczane podczas wywoływania interfejsów API w usługach Cognitive Services i wyodrębniania obrazu jako część etap łamania dokumentów w usłudze Azure Search. Opłaty nie będą naliczane do wyodrębniania tekstu z dokumentów.
>
> Wykonanie wbudowanego umiejętności podlega opłacie za istniejącą [usług Cognitive Services, płatności — jako — można przejść cena](https://azure.microsoft.com/pricing/details/cognitive-services/). Cennik wyodrębniania obraz został opisany na [usługi Azure Search stronę z cennikiem](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.EntityRecognitionSkill

## <a name="data-limits"></a>Limity danych
Maksymalny rozmiar rekord powinien być 50 000 znaków, gdyż jest mierzone przez `String.Length`. Jeśli zachodzi potrzeba Podziel swoje dane przed wysłaniem ich do wyodrębnianie kluczowych fraz, rozważ użycie [umiejętności dzielenie tekstu](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Parametry kwalifikacji

Parametrów jest rozróżniana wielkość liter i są wszystkie opcjonalne.

| Nazwa parametru     | Opis |
|--------------------|-------------|
| categories    | Tablica kategorie, które mają zostać wyodrębnione.  Typy możliwe kategorii: `"Person"`, `"Location"`, `"Organization"`, `"Quantity"`, `"Datetime"`, `"URL"`, `"Email"`. Jeśli nie podano żadnej kategorii, zwracane są wszystkie typy.|
|defaultLanguageCode |  Kod języka tekstu wejściowego. Są obsługiwane następujące języki: `de, en, es, fr, it`|
|minimumPrecision | Nieużywane. Zarezerwowane do użytku w przyszłości. |
|includeTypelessEntities | Gdy ustawiona wartość true, jeśli tekst zawiera jednostkę dobrze znana, ale nie można podzielić na jeden z obsługiwanych kategorii, będzie zwracany jako część `"entities"` złożonych danych wyjściowych. 
To są jednostki, które są dobrze znane, ale nie zostały sklasyfikowane jako część bieżącego obsługiwanych "kategorie". Na przykład "systemu Windows 10" jest dobrze znanych jednostek (produkt), ale "Produkty" nie znajdują się w kategorii obecnie obsługiwane. Wartość domyślna to `false` |


## <a name="skill-inputs"></a>Dane wejściowe umiejętności

| Wprowadź nazwę      | Opis                   |
|---------------|-------------------------------|
| languageCode  | Opcjonalny. Wartość domyślna to `"en"`.  |
| tekst          | Tekst do przeanalizowania.          |

## <a name="skill-outputs"></a>Dane wyjściowe umiejętności

> [!NOTE]
> Nie wszystkie kategorie jednostki są obsługiwane dla wszystkich języków. Tylko _en_, _es_ obsługuje Ekstrakcja `"Quantity"`, `"Datetime"`, `"URL"`, `"Email"` typów.

| Nazwa wyjściowego     | Opis                   |
|---------------|-------------------------------|
| osoby      | Tablica ciągów, gdzie każdy ciąg reprezentuje imię osoby. |
| locations  | Tablica ciągów, w którym każdy ciąg reprezentuje lokalizację. |
| organizations  | Tablica ciągów, w której każdy ciąg reprezentuje organizacji. |
| ilości  | Tablica ciągów, w której każdy ciąg reprezentuje ilość. |
| Data/Godzina  | Tablica ciągów, gdzie każdy ciąg reprezentuje wartość typu DateTime (wyświetlaną w tekście) wartość. |
| adresy URL | Tablica ciągów, w którym każdy ciąg reprezentuje adres URL |
| wiadomości e-mail | Tablica ciągów, w której każdy ciąg reprezentuje wiadomość e-mail |
| namedEntities | Tablica typów złożonych zawiera następujące pola: <ul><li>category</li> <li>wartość (nazwa rzeczywistego jednostki)</li><li>Przesunięcie (lokalizację, w którym zostało znalezione w tekście)</li><li>zaufania (nieużywane teraz. Zostanie ustawiona na wartość -1)</li></ul> |
| Jednostki | Tablica typów złożonych zawiera rozbudowane informacje dotyczące jednostek wyodrębnione z pliku tekstowego przy użyciu następujących pól <ul><li> Nazwa (nazwa rzeczywistego jednostki. Ta pozycja reprezentuje formularz "znormalizowanych")</li><li> wikipediaId</li><li>wikipediaLanguage</li><li>wikipediaUrl (link do strony Wikipedii dla jednostki)</li><li>bingId</li><li>Typ (kategoria jednostki rozpoznawane)</li><li>Podtyp (dostępne tylko dla niektórych kategorii, daje to bardziej szczegółowego widoku typ jednostki)</li><li> Dopasowuje (złożoną kolekcję zawierającą)<ul><li>tekst (nieprzetworzony tekst dla jednostki)</li><li>Przesunięcie (lokalizację, w którym został znaleziony)</li><li>długość (długość tekstu nieprzetworzona jednostki)</li></ul></li></ul> |

##  <a name="sample-definition"></a>Przykładowa definicja

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "categories": [ "Person", "Email"],
    "defaultLanguageCode": "en",
    "includeTypelessEntities": true,
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      },
      {
        "name": "emails",
        "targetName": "contact"
      },
      {
        "name": "entities"
      }
    ]
  }
```
##  <a name="sample-input"></a>Przykładowe dane wejściowe

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Contoso corporation was founded by John Smith. They can be reached at contact@contoso.com",
             "languageCode": "en"
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Przykładowe dane wyjściowe

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "persons": [ "John Smith"],
        "emails":["contact@contoso.com"],
        "namedEntities": 
        [
          {
            "category":"Person",
            "value": "John Smith",
            "offset": 35,
            "confidence": -1
          }
        ],
        "entities":  
        [
          {
            "name":"John Smith",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Person",
            "subType": null,
            "matches": [{
                "text": "John Smith",
                "offset": 35,
                "length": 10
            }]
          },
          {
            "name": "contact@contoso.com",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Email",
            "subType": null,
            "matches": [
            {
                "text": "contact@contoso.com",
                "offset": 70,
                "length": 19
            }]
          },
          {
            "name": "Contoso",
            "wikipediaId": "Contoso",
            "wikipediaLanguage": "en",
            "wikipediaUrl": "https://en.wikipedia.org/wiki/Contoso",
            "bingId": "349f014e-7a37-e619-0374-787ebb288113",
            "type": null,
            "subType": null,
            "matches": [
            {
                "text": "Contoso",
                "offset": 0,
                "length": 7
            }]
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>W przypadku wystąpienia błędów
Jeśli kod języka dla dokumentu nie jest obsługiwany, zwracany jest błąd, a nie jednostki są wyodrębniane.

## <a name="see-also"></a>Zobacz także

+ [Wstępnie zdefiniowane umiejętności](cognitive-search-predefined-skills.md)
+ [Jak Definiowanie zestawu umiejętności](cognitive-search-defining-skillset.md)
