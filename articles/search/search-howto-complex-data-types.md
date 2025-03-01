---
title: Jak modelowanie złożonych typów danych — usługa Azure Search
description: Mogą być modelowane struktury zagnieżdżonej lub hierarchiczny danych do indeksu usługi Azure Search przy użyciu danych typu ComplexType i kolekcje.
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
tags: complex data types; compound data types; aggregate data types
services: search
ms.service: search
ms.topic: conceptual
ms.date: 06/13/2019
ms.custom: seodec2018
ms.openlocfilehash: 61f2094449995e26c3d321aaf35deb3d60ff250c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056587"
---
# <a name="how-to-model-complex-data-types-in-azure-search"></a>Jak modelowanie złożonych typów danych w usłudze Azure Search

Zewnętrznych zestawów danych używanych do wypełniania indeksu usługi Azure Search może mogą mieć różne kształty. Czasami te aktualizacje obejmują podstruktury hierarchiczne lub zagnieżdżone. Przykłady może obejmują wiele adresów dla jednego klienta, wiele kolory i rozmiary dla jednej jednostki SKU wielu autorów jednej książce i tak dalej. Modelowanie warunki, można napotkać te struktury, nazywane *złożonych*, *złożone*, *złożonego*, lub *agregacji* typów danych. Termin używa usługi Azure Search to pojęcie jest **typu złożonego**. W usłudze Azure Search typy złożone są modelowane przy użyciu **pól złożonych**. Złożone pole jest polem, który zawiera elementy podrzędne (pola podrzędne), które mogą być dowolnego typu danych, w tym inne typy złożone. Działa to podobnie jako typy danych ze strukturą w języku programowania.

Złożone pól reprezentują pojedynczy obiekt w dokumencie lub tablicę obiektów, w zależności od typu danych. Pola typu `Edm.ComplexType` reprezentowania pojedynczych obiektów, podczas pola typu `Collection(Edm.ComplexType)` reprezentują tablic obiektów.

Usługa Azure Search natywnie obsługuje typy złożone i kolekcje. Te typy umożliwiają modelu prawie każdej struktury JSON do indeksu usługi Azure Search. W poprzednich wersjach interfejsów API usługi Azure Search tylko spłaszczone wierszy, które mogły zostać zaimportowane zestawy. W najnowszej wersji indeksu można teraz ściślej odpowiadają źródła danych. Innymi słowy Jeśli źródło danych zawiera typy złożone, indeksu może mieć typy złożone również.

Aby rozpocząć pracę, firma Microsoft zaleca [zestawu danych hotele](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md), w celu ich załadowania w **importowania danych** kreatora w witrynie Azure portal. Kreator wykrywa typy złożone w źródle i sugeruje schemat indeksu, w oparciu o wykryte struktury.

> [!Note]
> Obsługa złożonych typów jest ogólnie dostępna w `api-version=2019-05-06`. 
>
> Jeśli rozwiązanie wyszukiwania jest oparta na wcześniej obejścia spłaszczonych zestawów danych w kolekcji, należy zmienić indeksu obejmuje dodatkowe typy złożone jako obsługiwane w najnowszej wersji interfejsu API. Aby uzyskać więcej informacji na temat uaktualniania wersji interfejsu API, zobacz [uaktualnienia do najnowszej wersji interfejsu API REST](search-api-migration.md) lub [uaktualnienia do najnowszej wersji zestawu SDK platformy .NET](search-dotnet-sdk-migration-version-9.md).

## <a name="example-of-a-complex-structure"></a>Przykład strukturę złożoną

Poniższy dokument JSON składa się z pól proste i złożone. Złożone pola, takie jak `Address` i `Rooms`, ma pola podrzędne. `Address` ma jeden zestaw wartości dla tych pól podrzędnych, ponieważ jest to pojedynczy obiekt w dokumencie. Z kolei `Rooms` ma wiele zestawów wartości dla pól podrzędnych, po jednym dla każdego obiektu w kolekcji.

```json
{
  "HotelId": "1",
  "HotelName": "Secret Point Motel",
  "Description": "Ideally located on the main commercial artery of the city in the heart of New York.",
  "Address": {
    "StreetAddress": "677 5th Ave",
    "City": "New York",
    "StateProvince": "NY"
  },
  "Rooms": [
    {
      "Description": "Budget Room, 1 Queen Bed (Cityside)",
      "Type": "Budget Room",
      "BaseRate": 96.99,
    },
    {
      "Description": "Deluxe Room, 2 Double Beds (City View)",
      "Type": "Deluxe Room",
      "BaseRate": 150.99,
    },
  ]
}
```

## <a name="creating-complex-fields"></a>Tworzenie złożonych pola

Jak przy użyciu dowolnej definicji indeksu, mogą korzystać z portalu, [interfejsu API REST](https://docs.microsoft.com/rest/api/searchservice/create-index), lub [zestawu .NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) do tworzenia schemat, który zawiera typy złożone. 

Poniższy przykład przedstawia schematu indeksu JSON za pomocą prostego pola, kolekcje i typy złożone. Należy zauważyć, że w ramach typu złożonego, każde pole podrzędne ma typ i mogą mieć atrybutów, czy pola po prostu najwyższego poziomu. Schemat odnosi się do przykładowych danych powyżej. `Address` jest polem złożony, który nie jest kolekcją (hotelu ma jeden adres). `Rooms` To pole złożonych kolekcji (hotelu ma wiele pokojów).

<!---
For indexes used in a [push-model data import](search-what-is-data-import.md) strategy, where you are pushing a JSON data set to an Azure Search index, you can only have the basic syntax shown here: single complex types like `Address`, or a `Collection(Edm.ComplexType)` like `Rooms`. You cannot have complex types nested inside other complex types in an index used for push-model data ingestion.

Indexers are a different story. When defining an indexer, in particular one used to build a knowledge store, your index can have nested complex types. An indexer is able to hold a chain of complex data structures in-memory, and when it includes a skillset, it can support highly complex data forms. For more information and an example, see [How to get started with knowledge store](knowledge-store-howto.md).
-->

```json
{
  "name": "hotels",
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false },
    { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
    { "name": "Address", "type": "Edm.ComplexType",
      "fields": [
        { "name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true },
        { "name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
        { "name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true }
      ]
    },
    { "name": "Rooms", "type": "Collection(Edm.ComplexType)",
      "fields": [
        { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
        { "name": "Type", "type": "Edm.String", "searchable": true },
        { "name": "BaseRate", "type": "Edm.Double", "filterable": true, "facetable": true }
      ]
    }
  ]
}
```

## <a name="updating-complex-fields"></a>Aktualizowanie pól złożonych

Wszystkie [indeksowanie reguły](search-howto-reindex.md) które są stosowane do pól ogólnie rzecz biorąc nadal dotyczą złożonych pól. Przekształcenie kilka głównych zasad w tym miejscu dodanie pola nie wymaga odbudowywanie indeksu, ale nie większość modyfikacji.

### <a name="structural-updates-to-the-definition"></a>Strukturalne aktualizacje definicji

W dowolnym momencie bez konieczności odbudowywanie indeksu, można dodać nowe pola podrzędnego do złożonych pola. Na przykład dodanie "Kod pocztowy", aby `Address` lub "Pozwalającego", aby `Rooms` jest dozwolony, podobnie jak dodanie pola najwyższego poziomu do indeksu. Istniejące dokumenty mają wartość null dla nowego pola, aż jawnie wypełniania tych pól, aktualizując swoje dane.

Należy zauważyć, że w ramach typu złożonego, każde pole podrzędne ma typ i mogą mieć atrybutów, czy pola po prostu najwyższego poziomu

### <a name="data-updates"></a>Aktualizacje danych

Aktualizowanie istniejących dokumentów do indeksu przy użyciu `upload` akcji działa tak samo dla pól złożone i prosta — wszystkie pola są zastępowane. Jednak `merge` (lub `mergeOrUpload` po zastosowaniu do istniejącego dokumentu) nie działa takie same we wszystkich polach. W szczególności `merge` nie obsługuje scalania elementów w obrębie kolekcji. To ograniczenie istnieje dla kolekcji typów pierwotnych i złożonych kolekcji. Można zaktualizować kolekcji, można będzie potrzebne do pobrania wartości pełnego wprowadzić zmiany, a następnie dołącz nową kolekcję w żądaniu indeks interfejsu API.

## <a name="searching-complex-fields"></a>Wyszukiwanie pól złożonych

Wyszukiwanie dowolnych wyrażeń działają zgodnie z oczekiwaniami typów złożonych. Jeśli w dowolnym polu możliwym do przeszukania lub podrzędne pole w dowolnym miejscu w dokumencie jest zgodny, samego dokumentu jest dopasowanie.

Pobierz zapytania więcej złożonych masz wiele warunków i operatory, gdy niektóre terminy mają nazwy pól określone, jest to możliwe dzięki [składni Lucene](query-lucene-syntax.md). Na przykład to zapytanie próbuje dopasować dwa terminy, "Portland" i "Lub" względem dwa pola podrzędne pola Adres:

    search=Address/City:Portland AND Address/State:OR

Kwerend, takich jak się to *nieskorelowane* dla wyszukiwania pełnotekstowego, w przeciwieństwie do filtrów. W filtrach, zapytań pól podrzędnych złożonych kolekcji są skorelowane, za pomocą zmiennych zakresy w [ `any` lub `all` ](search-query-odata-collection-operators.md). Powyższe zapytań Lucene zwraca dokumenty zawierające wraz z innymi nazwami miast w Oregon "Portland, Maine" i "Portland, Oregon". Dzieje się tak, ponieważ każdej klauzuli ma zastosowanie do wszystkich wartości z jej pól w całym dokumencie, więc nie ma żadnych koncepcji "bieżący dokument podrzędnych". Aby uzyskać więcej informacji na temat tego, zobacz [OData opis kolekcji filtrów w usłudze Azure Search](search-query-understand-collection-filters.md).

## <a name="selecting-complex-fields"></a>Wybieranie pól złożonych

`$select` Parametr jest używany do wybierz pola, które są zwracane w wynikach wyszukiwania. Używać tego parametru, aby wybrać określone pola podrzędnego złożonych pola, należy uwzględnić pole nadrzędne i podrzędne pola, oddzielone ukośnikiem (`/`).

    $select=HotelName, Address/City, Rooms/BaseRate

Pola musi być oznaczona jako możliwość pobierania w indeksie, jeśli chcesz w wynikach wyszukiwania. Tylko pola oznaczone jako możliwość pobierania mogą być używane w `$select` instrukcji.

## <a name="filter-facet-and-sort-complex-fields"></a>Filtr, reguł i złożone pola sortowania

Taki sam [składnia ścieżki OData](query-odata-filter-orderby-syntax.md) używanej do filtrowania i fielded wyszukiwania może również służyć do tworzenie aspektów, sortowanie i wybierając pola w żądaniu wyszukiwania. W przypadku typów złożonych mają zastosowanie reguły określające pola podrzędne, które mogą zostać oznaczone jako sortowania i tworzenia aspektów. Aby uzyskać więcej informacji dotyczących tych reguł zobacz [odwołanie do interfejsu API tworzenia indeksu](https://docs.microsoft.com/rest/api/searchservice/create-index#request).

### <a name="faceting-sub-fields"></a>Kategoryzowanie pól podrzędnych

Każde pole podrzędne może być oznaczona jako tworzenia aspektów, chyba że jest on typu `Edm.GeographyPoint` lub `Collection(Edm.GeographyPoint)`.

Liczby dokumentów w wynikach reguł są obliczane dla dokumentu nadrzędnego (hotelu), nie podrzędnych dokumentów w przypadku złożonych kolekcji (pokojów). Na przykład załóżmy, że hotelu ma 20 pokojów typu "pakiet". Podany parametr ten aspekt `facet=Rooms/Type`, liczba reguł będzie, jeden dla hotelu nie 20 dla pomieszczeniach.

### <a name="sorting-complex-fields"></a>Sortowanie pola złożone

Operacjach sortowania są stosowane do dokumentów (hotele) i nie podrzędnych dokumentów (pokojów). Masz kolekcję typu złożonego, takie jak pokoje jest zdawać sobie sprawę, że nie można sortować w pokojach w ogóle. W rzeczywistości nie można sortować w żadnej kolekcji.

Sortowanie działać, gdy pola mają pojedynczą wartość na dokument, czy pole jest prostym polem lub podrzędne pola w typie złożonym. Na przykład `Address/City` może być można sortować, ponieważ istnieje tylko jeden adres dla każdego hotelu, więc `$orderby=Address/City` sortowania hotele według miast.

### <a name="filtering-on-complex-fields"></a>Filtrowanie według pól złożonych

Mogą odwoływać się do pól podrzędnych złożonych pola w wyrażeniu filtru. Po prostu używać tego samego [składnia ścieżki OData](query-odata-filter-orderby-syntax.md) używanego do obsługi tworzenie aspektów, sortowanie i wybierając pola. Na przykład następujący filtr zwróci wszystkie hotele, w Kanadzie:

    $filter=Address/Country eq 'Canada'

Aby filtrować według pola złożonych kolekcji, można użyć **wyrażenia lambda** z [ `any` i `all` operatory](search-query-odata-collection-operators.md). W takim przypadku **zmiennej zakresu** wyrażenia lambda wyrażenia jest obiektem, za pomocą pól podrzędnych. Mogą odwoływać się do tych pól podrzędnych przy użyciu standardowej składni ścieżki OData. Na przykład następujący filtr zwróci wszystkie hotele za pomocą co najmniej jeden pokoju deluxe i wszystkich innych palenia pokoje:

    $filter=Rooms/any(room: room/Type eq 'Deluxe Room') and Rooms/all(room: not room/SmokingAllowed)

Jak za pomocą prostego pola najwyższego poziomu prostego pola podrzędne złożonych pól mogą być uwzględniane w filtrach jeśli mają one **filtrowanie** ustawioną wartość atrybutu `true` w definicji indeksu. Aby uzyskać więcej informacji, zobacz [odwołanie do interfejsu API tworzenia indeksu](https://docs.microsoft.com/rest/api/searchservice/create-index#request).

## <a name="next-steps"></a>Kolejne kroki

Spróbuj [zestawu danych hotele](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md) w **importowania danych** kreatora. Informacje o połączeniu usługi Cosmos DB, które zostały podane w pliku readme dostępu do danych jest konieczne.

Dysponując tą informacją w kasie Twoim pierwszym krokiem w kreatorze jest utworzenie nowego źródła danych usługi Azure Cosmos DB. Dodatkowo na kreatora, po przejściu do strony indeksu docelowego, zobaczysz indeksu z typów złożonych. Utworzyć i załadować ten indeks, a następnie wykonywania zapytania, aby poznać nową strukturę.

> [!div class="nextstepaction"]
> [Szybki Start: portal Kreatora importu, indeksowania i zapytań](search-get-started-portal.md)