---
title: Dokumentacja składni wyrażenia OData — usługa Azure Search
description: Formalną specyfikację gramatyki i składnię wyrażeń OData w zapytaniach usługi Azure Search.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: cccfb749af07d1deeeda6e94de9c2cd5ce5396f3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079667"
---
# <a name="odata-expression-syntax-reference-for-azure-search"></a>Dokumentacja składni wyrażenia OData dla usługi Azure Search

Usługa Azure Search używa [wyrażenia OData](http://docs.oasis-open.org/odata/odata/v4.01/odata-v4.01-part2-url-conventions.html) jako parametry w interfejsie API. Najczęściej wyrażenia OData są używane `$orderby` i `$filter` parametrów. Wyrażenia te mogą być skomplikowane, zawierające wiele klauzul, funkcje i operatory. Jednak nawet proste wyrażenia OData, takich jak właściwości, których ścieżki są używane w wielu częściach interfejsu API REST usługi Azure Search. Na przykład ścieżka wyrażenia są używane do odwoływania się do pól podrzędnych pól złożonych wszędzie, gdzie w interfejsie API, np. gdy lista pól podrzędnych w [sugestora](index-add-suggesters.md), [oceniania funkcji](index-add-scoring-profiles.md), `$select` parametru , a nawet [fielded wyszukiwania zapytań Lucene](query-lucene-syntax.md).

W tym artykule opisano te rodzaje wyrażeń OData przy użyciu gramatyka formalnych. Istnieje również [interaktywnym diagramem](#syntax-diagram) ułatwiające sięgającym gramatyki.

## <a name="formal-grammar"></a>Posiadanie gramatyki

Opisujemy podzbiór języka OData obsługiwane przez usługę Azure Search przy użyciu EBNF ([rozszerzony formularz Backus-Naur](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) gramatyki. Reguły są wyświetlane "góra dół", począwszy od najbardziej złożonych wyrażeń i potężne do bardziej pierwotnych wyrażeń. U góry są reguły gramatyki, które odpowiadają określone parametry interfejsu API REST usługi Azure Search:

- [`$filter`](search-query-odata-filter.md), zdefiniowane przez `filter_expression` reguły.
- [`$orderby`](search-query-odata-orderby.md), zdefiniowane przez `order_by_expression` reguły.
- [`$select`](search-query-odata-select.md), zdefiniowane przez `select_expression` reguły.
- Pole ścieżki, zdefiniowane przez `field_path` reguły. Pole ścieżki są używane w interfejsie API. Mogą odwoływać się do obu najwyższego poziomu pola indeksu lub pól podrzędnych z co najmniej [pole złożone](search-howto-complex-data-types.md) elementów nadrzędnych.

Po można przeglądać EBNF [diagram składni](https://en.wikipedia.org/wiki/Syntax_diagram) umożliwiająca interakcyjne poznawanie gramatyki i relacje między regułach.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
/* Top-level rules */

filter_expression ::= boolean_expression

order_by_expression ::= order_by_clause(',' order_by_clause)*

select_expression ::= '*' | field_path(',' field_path)*

field_path ::= identifier('/'identifier)*


/* Shared base rules */

identifier ::= [a-zA-Z_][a-zA-Z_0-9]*


/* Rules for $orderby */

order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'


/* Rules for $filter */

boolean_expression ::=
    collection_filter_expression
    | logical_expression
    | comparison_expression
    | boolean_literal
    | boolean_function_call
    | '(' boolean_expression ')'
    | variable

/* This can be a range variable in the case of a lambda, or a field path. */
variable ::= identifier | field_path

collection_filter_expression ::=
    field_path'/all(' lambda_expression ')'
    | field_path'/any(' lambda_expression ')'
    | field_path'/any()'

lambda_expression ::= identifier ':' boolean_expression

logical_expression ::=
    boolean_expression ('and' | 'or') boolean_expression
    | 'not' boolean_expression

comparison_expression ::= 
    variable_or_function comparison_operator constant | 
    constant comparison_operator variable_or_function

variable_or_function ::= variable | function_call

comparison_operator ::= 'gt' | 'lt' | 'ge' | 'le' | 'eq' | 'ne'


/* Rules for constants and literals */

constant ::=
    string_literal
    | date_time_offset_literal
    | integer_literal
    | float_literal
    | boolean_literal
    | 'null'

string_literal ::= "'"([^'] | "''")*"'"

date_time_offset_literal ::= date_part'T'time_part time_zone

date_part ::= year'-'month'-'day

time_part ::= hour':'minute(':'second('.'fractional_seconds)?)?

zero_to_fifty_nine ::= [0-5]digit

digit ::= [0-9]

year ::= digit digit digit digit

month ::= '0'[1-9] | '1'[0-2]

day ::= '0'[1-9] | [1-2]digit | '3'[0-1]

hour ::= [0-1]digit | '2'[0-3]

minute ::= zero_to_fifty_nine

second ::= zero_to_fifty_nine

fractional_seconds ::= integer_literal

time_zone ::= 'Z' | sign hour':'minute

sign ::= '+' | '-'

/* In practice integer literals are limited in length to the precision of
the corresponding EDM data type. */
integer_literal ::= digit+

float_literal ::=
    sign? whole_part fractional_part? exponent?
    | 'NaN'
    | '-INF'
    | 'INF'

whole_part ::= integer_literal

fractional_part ::= '.'integer_literal

exponent ::= 'e' sign? integer_literal

boolean_literal ::= 'true' | 'false'


/* Rules for functions */

function_call ::=
    geo_distance_call |
    boolean_function_call

geo_distance_call ::=
    'geo.distance(' variable ',' geo_point ')'
    | 'geo.distance(' geo_point ',' variable ')'

geo_point ::= "geography'POINT(" lon_lat ")'"

lon_lat ::= float_literal ' ' float_literal

boolean_function_call ::=
    geo_intersects_call |
    search_in_call |
    search_is_match_call

geo_intersects_call ::=
    'geo.intersects(' variable ',' geo_polygon ')'

/* You need at least four points to form a polygon, where the first and
last points are the same. */
geo_polygon ::=
    "geography'POLYGON((" lon_lat ',' lon_lat ',' lon_lat ',' lon_lat_list "))'"

lon_lat_list ::= lon_lat(',' lon_lat)*

search_in_call ::=
    'search.in(' variable ',' string_literal(',' string_literal)? ')'

/* Note that it is illegal to call search.ismatch or search.ismatchscoring
from inside a lambda expression. */
search_is_match_call ::=
    'search.ismatch'('scoring')?'(' search_is_match_parameters ')'

search_is_match_parameters ::=
    string_literal(',' string_literal(',' query_type ',' search_mode)?)?

query_type ::= "'full'" | "'simple'"

search_mode ::= "'any'" | "'all'"
```

## <a name="syntax-diagram"></a>Diagram składni

Aby wizualnie eksplorować gramatyki języka OData, które są obsługiwane przez usługę Azure Search, spróbuj diagram interaktywne składni:

> [!div class="nextstepaction"]
> [Diagram składni OData dla usługi Azure Search](https://azuresearch.github.io/odata-syntax-diagram/)

## <a name="see-also"></a>Zobacz także  

- [Filtry w usłudze Azure Search](search-filters.md)
- [Wyszukiwanie w dokumentach &#40;interfejsu API REST usługi Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
- [Składnia zapytań Lucene](query-lucene-syntax.md)
- [Prosta składnia zapytań w usłudze Azure Search](query-simple-syntax.md)
