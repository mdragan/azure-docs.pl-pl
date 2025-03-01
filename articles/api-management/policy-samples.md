---
title: Przykłady zasad usługi Azure API Management | Microsoft Docs
description: Dowiedz się więcej o zasadach dostępnych do użycia w usłudze Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cflower
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: sample
ms.date: 10/31/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: 550161ce39aa944d0e01bb349ba48acbf719a860
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60859379"
---
# <a name="api-management-policy-samples"></a>Przykłady zasad usługi API Management

[Zasady](api-management-howto-policies.md) są zaawansowaną możliwością systemu, która pozwala wydawcy zmieniać zachowanie interfejsu API za pomocą konfiguracji. Zasady to zbiór instrukcji, które są wykonywane sekwencyjnie podczas żądania lub odpowiedzi interfejsu API. Poniższa tabela zawiera linki do przykładów i krótki opis każdego przykładu.

|                                                                                                                                                                      |                                                                                                                                                                                                                             |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Zasady ruchu przychodzącego**                                                                                                                                                 |                                                                                                                                                                                                                             |
| [Dodawanie nagłówka Forwarded w celu umożliwienia interfejsowi API zaplecza tworzenia odpowiednich adresów URL](./policies/set-header-to-enable-backend-to-construct-urls.md?toc=api-management/toc.json) | Przedstawia sposób dodawania nagłówka Forwarded do żądania przychodzącego w celu umożliwienia interfejsowi API zaplecza tworzenia odpowiednich adresów URL.                                                                                                        |
| [Dodawanie nagłówka zawierającego identyfikator korelacji](./policies/add-correlation-id.md?toc=api-management/toc.json)                                                             | Przedstawia sposób dodawania nagłówka zawierającego identyfikator korelacji do żądania przychodzącego.                                                                                                                                        |
| [Dodawanie możliwości do usługi zaplecza i buforowanie odpowiedzi](./policies/cache-response.md?toc=api-management/toc.json)                                             | Pokazuje sposób dodawania możliwości do usługi zaplecza. Na przykład akceptowania nazwy miejsca zamiast współrzędnych geograficznych w interfejsie API prognozy pogody.                                                                    |
| [Autoryzacja dostępu na podstawie oświadczeń JWT](./policies/authorize-request-based-on-jwt-claims.md?toc=api-management/toc.json)                                              | Pokazuje sposób autoryzowania dostępu do konkretnych metod HTTP w interfejsie API opartym na oświadczeniach JWT.                                                                                                                                       |
| [Autoryzacja żądań za pomocą zewnętrznego obiektu autoryzującego](./policies/authorize-request-using-external-authorizer.md)                                                   | Pokazuje sposób zabezpieczania dostępu do interfejsu API za pomocą zewnętrznego obiektu autoryzującego.                                                                                                                                                               |
| [Autoryzacja dostępu za pomocą tokenu OAuth usługi Google](./policies/use-google-as-oauth-token-provider.md?toc=api-management/toc.json)                                            | Pokazuje sposób autoryzowania dostępu do punktów końcowych przy użyciu usługi Google jako dostawcy tokenów OAuth.                                                                                                                                    |
| [Generowanie sygnatury dostępu współdzielonego i przekazywanie żądania do usługi Azure Storage](./policies/generate-shared-access-signature.md?toc=api-management/toc.json)                  | Pokazuje sposób generowania [sygnatury dostępu współdzielonego](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) przy użyciu wyrażeń i przekazywania żądania do usługi Azure Storage za pomocą zasad ponownego zapisywania identyfikatorów URI. |
| [Uzyskiwanie tokenu dostępu OAuth2 z usługi AAD i przekazywanie go do zaplecza](./policies/use-oauth2-for-authorization.md?toc=api-management/toc.json)                             | Udostępnia przykład użycia protokołu OAuth2 do autoryzacji między bramą i zapleczem. Pokazuje sposób uzyskiwania tokenu dostępu z usługi AAD i przekazywania go do zaplecza.                                                    |
| [Uzyskiwanie tokenu X-CSRF z bramy SAP przy użyciu zasad żądania wysłania](./policies/get-x-csrf-token-from-sap-gateway.md?toc=api-management/toc.json)                           | Pokazuje sposób implementowania wzorca X-CSRF używanego przez wiele interfejsów API. Ten przykład jest specyficzny dla bramy SAP.                                                                                                                           |
| [Kierowanie żądania na podstawie rozmiaru jego treści](./policies/route-requests-based-on-size.md?toc=api-management/toc.json)                                            | Przedstawia kierowanie żądań na podstawie rozmiaru ich treści.                                                                                                                                                       |
| [Wysyłanie informacji o kontekście żądania do usługi zaplecza](./policies/send-request-context-info-to-backend-service.md?toc=api-management/toc.json)                    | Pokazuje sposób wysyłania pewnych informacji dotyczących kontekstu do usługi zaplecza na potrzeby rejestrowania lub przetwarzania.                                                                                                                                |
| [Ustawianie czasu trwania pamięci podręcznej odpowiedzi](./policies/set-cache-duration.md?toc=api-management/toc.json)                                                                          | Pokazuje sposób ustawiania czasu trwania pamięci podręcznej odpowiedzi za pomocą wartości maxAge nagłówka Cache-Control wysyłanego przez zaplecze.                                                                                                             |
| **Zasady ruchu wychodzącego**                                                                                                                                                |                                                                                                                                                                                                                             |
| [Filtrowanie zawartości odpowiedzi](./policies/filter-response-content.md?toc=api-management/toc.json)                                                                         | Przedstawia sposób filtrowania elementów danych w ładunku odpowiedzi na podstawie produktu skojarzonego z żądaniem.                                                                                                        |
| **Zasady dla wystąpienia błędu**                                                                                                                                                |                                                                                                                                                                                                                             |
| [Rejestrowanie błędów w usłudze Stackify](./policies/log-errors-to-stackify.md?toc=api-management/toc.json)                                                                           | Pokazuje sposób dodawania zasad rejestrowania błędów w celu wysyłania błędów do usługi Stackify na potrzeby rejestrowania.                                                                                                                                            |
