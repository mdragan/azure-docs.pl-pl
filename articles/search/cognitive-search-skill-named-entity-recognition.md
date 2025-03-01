---
title: O nazwie rozpoznawanie jednostek wyszukiwania kognitywnego umiejętności — usługa Azure Search
description: Wyodrębnij nazwane jednostek dla organizacji, lokalizacji i osoby z pliku tekstowego w potoku usługi Azure Search w usłudze wyszukiwania poznawczego.
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
ms.openlocfilehash: b7af4d0a48f002f7523def971a306d1fa2077c70
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65952038"
---
#    <a name="named-entity-recognition-cognitive-skill"></a>Nazwane umiejętności cognitive rozpoznawanie jednostek

**Rozpoznawania jednostki o nazwie** umiejętności wyodrębnia nazwane jednostek z pliku tekstowego. Dostępne jednostki i obejmuje dodatkowe typy `person`, `location` i `organization`.

> [!IMPORTANT]
> Rozpoznawanie jednostek znaku umiejętności jest już obsługiwany zastępuje [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md). Obsługa zatrzymana na 15 lutego 2019 i interfejs API został usunięty z produktu w dniu 2 maja 2019 r. Postępuj zgodnie z zaleceniami w [przestarzałe umiejętności wyszukiwania kognitywnego](cognitive-search-skill-deprecated.md) migrację do obsługiwanych umiejętności.

> [!NOTE]
> Możesz rozwiń zakres, zwiększając częstotliwości przetwarzania, dodając więcej dokumentów lub dodanie więcej algorytmów sztucznej Inteligencji, konieczne będzie [dołączyć płatnych zasobu usług Cognitive Services](cognitive-search-attach-cognitive-services.md). Opłaty są naliczane podczas wywoływania interfejsów API w usługach Cognitive Services i wyodrębniania obrazu jako część etap łamania dokumentów w usłudze Azure Search. Opłaty nie będą naliczane do wyodrębniania tekstu z dokumentów.
>
> Wykonanie wbudowanego umiejętności podlega opłacie za istniejącą [usług Cognitive Services, płatności — jako — można przejść cena](https://azure.microsoft.com/pricing/details/cognitive-services/). Cennik wyodrębniania obraz został opisany na [usługi Azure Search stronę z cennikiem](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="data-limits"></a>Limity danych
Maksymalny rozmiar rekord powinien być 50 000 znaków, gdyż jest mierzone przez `String.Length`. Jeśli zachodzi potrzeba Podziel swoje dane przed wysłaniem ich do wyodrębnianie kluczowych fraz, rozważ użycie [umiejętności dzielenie tekstu](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Parametry kwalifikacji

Parametrów jest rozróżniana wielkość liter.

| Nazwa parametru     | Opis |
|--------------------|-------------|
| categories    | Tablica kategorie, które mają zostać wyodrębnione.  Typy możliwe kategorii: `"Person"`, `"Location"`, `"Organization"`. Jeśli nie podano żadnej kategorii, zwracane są wszystkie typy.|
|defaultLanguageCode |  Kod języka tekstu wejściowego. Są obsługiwane następujące języki: `de, en, es, fr, it`|
| minimumPrecision  | Liczba od 0 do 1. Jeśli dokładność jest niższa niż ta wartość, nie są zwracane jednostki. Wartość domyślna to 0.|

## <a name="skill-inputs"></a>Dane wejściowe umiejętności

| Wprowadź nazwę      | Opis                   |
|---------------|-------------------------------|
| languageCode  | Opcjonalny. Wartość domyślna to `"en"`.  |
| tekst          | Tekst do przeanalizowania.          |

## <a name="skill-outputs"></a>Dane wyjściowe umiejętności

| Nazwa wyjściowego     | Opis                   |
|---------------|-------------------------------|
| osoby      | Tablica ciągów, gdzie każdy ciąg reprezentuje imię osoby. |
| locations  | Tablica ciągów, w którym każdy ciąg reprezentuje lokalizację. |
| organizations  | Tablica ciągów, w której każdy ciąg reprezentuje organizacji. |
| Jednostki | Tablica typów złożonych. Każdy typ złożony zawiera następujące pola: <ul><li>Kategoria (`"person"`, `"organization"`, lub `"location"`)</li> <li>wartość (nazwa rzeczywistego jednostki)</li><li>Przesunięcie (lokalizację, w którym zostało znalezione w tekście)</li><li>zaufania (wartość z zakresu od 0 do 1, który reprezentuje tego zaufania, wartość rzeczywista jednostki)</li></ul> |

##  <a name="sample-definition"></a>Przykładowa definicja

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person", "Location", "Organization"],
    "defaultLanguageCode": "en",
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
             "text": "This is the loan application for Joe Romero, a Microsoft employee who was born in Chile and who then moved to Australia… Ana Smith is provided as a reference.",
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
        "persons": [ "Joe Romero", "Ana Smith"],
        "locations": ["Chile", "Australia"],
        "organizations":["Microsoft"],
        "entities":  
        [
          {
            "category":"person",
            "value": "Joe Romero",
            "offset": 33,
            "confidence": 0.87
          },
          {
            "category":"person",
            "value": "Ana Smith",
            "offset": 124,
            "confidence": 0.87
          },
          {
            "category":"location",
            "value": "Chile",
            "offset": 88,
            "confidence": 0.99
          },
          {
            "category":"location",
            "value": "Australia",
            "offset": 112,
            "confidence": 0.99
          },
          {
            "category":"organization",
            "value": "Microsoft",
            "offset": 54,
            "confidence": 0.99
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
+ [Umiejętności rozpoznawania jednostek](cognitive-search-skill-entity-recognition.md)
