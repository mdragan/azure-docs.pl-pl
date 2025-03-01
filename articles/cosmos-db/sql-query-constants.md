---
title: Stałe SQL w usłudze Azure Cosmos DB
description: Dowiedz się więcej o stałych SQL w usłudze Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: tisande
ms.openlocfilehash: ed337c8330eb8b3e4ad97c92cca1d0c5977f9588
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342588"
---
# <a name="azure-cosmos-db-sql-query-constants"></a>Stałe zapytania w usłudze Azure Cosmos DB SQL  

 Stała, znana także jako literał lub wartość skalarną, jest symbol, który reprezentuje wartość określonych danych. Format stałej zależy od typu danych wartość, którą reprezentuje.  
  
 **Obsługiwane skalarnych typów danych:**  
  
|**Typ**|**Kolejność wartości**|  
|-|-|  
|**Niezdefiniowane**|Pojedyncza wartość: **niezdefiniowane**|  
|**Null**|Pojedyncza wartość: **o wartości null**|  
|**Wartość logiczna**|Wartości: **false**, **true**.|  
|**Liczba**|Podwójnej precyzji liczba zmiennoprzecinkowa, IEEE 754 standardowych.|  
|**Ciąg**|Sekwencja zero lub więcej znaków Unicode. Parametry muszą być ujęte w pojedyncze lub podwójne cudzysłowy.|  
|**Tablica**|Sekwencja zero lub więcej elementów. Każdy element może być wartością typu danych skalarnych, z wyjątkiem **niezdefiniowane**.|  
|**Obiekt**|Nieuporządkowana zestaw par nazwa/wartość zero lub więcej. Nazwa jest ciągiem Unicode, wartość może być dowolnego typu danych skalarnych, z wyjątkiem **niezdefiniowane**.|  
  
## <a name="bk_syntax"></a>Składnia
  
```sql  
<constant> ::=  
   <undefined_constant>  
     | <null_constant>   
     | <boolean_constant>   
     | <number_constant>   
     | <string_constant>   
     | <array_constant>   
     | <object_constant>   
  
<undefined_constant> ::= undefined  
  
<null_constant> ::= null  
  
<boolean_constant> ::= false | true  
  
<number_constant> ::= decimal_literal | hexadecimal_literal  
  
<string_constant> ::= string_literal  
  
<array_constant> ::=  
    '[' [<constant>][,...n] ']'  
  
<object_constant> ::=   
   '{' [{property_name | "property_name"} : <constant>][,...n] '}'  
  
```  
  
##  <a name="bk_arguments"></a> argumenty
  
* `<undefined_constant>; Undefined`  
  
  Wartość reprezentuje Niezdefiniowany typ niezdefiniowane.  
  
* `<null_constant>; null`  
  
  Reprezentuje **null** wartości typu **Null**.  
  
* `<boolean_constant>`  
  
  Reprezentuje stałą typu Boolean.  
  
* `false`  
  
  Reprezentuje **false** wartość typu Boolean.  
  
* `true`  
  
  Reprezentuje **true** wartość typu Boolean.  
  
* `<number_constant>`  
  
  Reprezentuje stałą.  
  
* `decimal_literal`  
  
  Literały dziesiętna są cyfry, reprezentowane za pomocą notacji dziesiętnej lub notacji wykładniczej.  
  
* `hexadecimal_literal`  
  
  Literały szesnastkowe są cyfry, reprezentowane za pomocą prefiks "0 x" następuje co najmniej jedna cyfra szesnastkowa.  
  
* `<string_constant>`  
  
  Reprezentuje stałą typu String.  
  
* `string _literal`  
  
  Literały ciągów są reprezentowane przez Sekwencja zero lub więcej znaków Unicode lub sekwencji unikowych ciągów znaków Unicode. Literały ciągu są ujmowane w cudzysłów pojedynczy (apostrof: ") lub podwójny cudzysłów (cudzysłów:").  
  
  Poniższe sekwencje ucieczki są dozwolone:  
  
|**Sekwencja unikowa**|**Opis**|**Znak Unicode**|  
|-|-|-|  
|\\'|apostrof (')|U+0027|  
|\\"|znak cudzysłowu (")|U+0022|  
|\\\ |kreska ułamkowa odwrotnej (\\)|U+005C|  
|\\/|kreska ułamkowa (/)|U+002F|  
|\b|BACKSPACE|U+0008|  
|\f|Wysuw strony|U+000C|  
|\n|Wysuw wiersza|U+000A|  
|\r|Powrót karetki|U+000D|  
|\t|tabulator|U+0009|  
|\uXXXX|Znak Unicode, zdefiniowane przez 4 cyfr szesnastkowych.|U+XXXX|  

## <a name="next-steps"></a>Kolejne kroki

- [Przykłady dla platformy .NET w usłudze Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Modelowanie danych dokumentów](modeling-data.md)
