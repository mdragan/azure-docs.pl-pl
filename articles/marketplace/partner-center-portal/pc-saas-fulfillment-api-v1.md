---
title: Realizacja SaaS interfejsów API w wersji 1 | Portal Azure Marketplace
description: Wyjaśnia, jak tworzyć i zarządzać nimi to oferta SaaS w portalu Azure Marketplace, za pomocą interfejsów API w wersji 1 usługi realizacji skojarzonych.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 05/23/2019
ms.author: evansma
ROBOTS: NOINDEX
ms.openlocfilehash: 78162983601e9126bd34cb737e74783df982bacb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66258942"
---
# <a name="saas-fulfillment-apis-version-1-deprecated"></a>Interfejsami API usług SaaS realizacji w wersji 1 (przestarzałe)

W tym artykule opisano sposób tworzenia oferty SaaS za pomocą interfejsów API. Interfejsy API, składa się z pozostałych metod i punktów końcowych, są niezbędne dla Jeśli sprzedajesz za pośrednictwem wybrane platformy Azure, dzięki czemu subskrypcje do swojej oferty SaaS.  

> [!WARNING]
> Ta początkowa wersja interfejsu API realizacji SaaS jest przestarzała; Zamiast tego należy użyć [SaaS realizacji interfejsu API w wersji 2](./pc-saas-fulfillment-api-v2.md).  Tej wersji wstępnej interfejsu API jest obecnie są obsługiwana wyłącznie do obsługi istniejących wydawców. 

Aby pomóc w zintegrowaniu usługi SaaS dzięki platformie Azure dostępne są następujące interfejsy API:

-   Rozwiąż
-   Subskrybuj
-   Convert
-   Anulowanie subskrypcji


## <a name="api-methods-and-endpoints"></a>Metody interfejsu API i punktów końcowych

Poniżej opisano metody interfejsu API i punktów końcowych dostępnej włączania subskrypcji skorzystania z oferty SaaS.


### <a name="marketplace-api-endpoint-and-api-version"></a>Punkt końcowy interfejsu API w portalu Marketplace i wersja interfejsu API

Punkt końcowy dla interfejsu API usługi Azure Marketplace jest `https://marketplaceapi.microsoft.com`.

Bieżąca wersja interfejsu API to `api-version=2017-04-15`.


### <a name="resolve-subscription"></a>Rozwiązanie z subskrypcji

Akcja POST na rozpoznać punktu końcowego umożliwia użytkownikom można rozpoznać tokenu marketplace trwały identyfikator zasobu.  Identyfikator zasobu jest unikatowy identyfikator subskrypcji w modelu SAAS. 

Gdy użytkownik jest przekierowywany do witryny sieci Web niezależnych dostawców oprogramowania, adres URL zawiera token w parametrach zapytania. Niezależny dostawca oprogramowania oczekuje na używanie tego tokenu w celu wykonania żądania, aby go rozwiązać. Odpowiedź zawiera identyfikator subskrypcji w modelu SAAS unikatową nazwę, Identyfikatora oferty i plan dla zasobu. Ten token jest prawidłowy tylko godzinę.

*Żądanie*

**POST**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=2017-04-15**

|  **Nazwa parametru** |     **Opis**                                      |
|  ------------------ |     ---------------------------------------------------- |
|  api-version        |  Wersja operacji przy użyciu dla tego żądania.   |
|  |  |


*Nagłówki*

| **Klucz nagłówka**     | **Wymagane** | **Opis**                                                                                                                                                                                                                  |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Nie           | Unikatowy ciąg wartości do śledzenia żądania z klienta, najlepiej z identyfikatorem GUID. Jeśli ta wartość nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi.  |
| x-ms-correlationid | Nie           | Unikatowy ciąg wartości dla operacji na komputerze klienckim. To pole jest skorelowane wszystkie zdarzenia w operacji klienta ze zdarzeniami po stronie serwera. Jeśli ta wartość nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi. |
| Typ zawartości       | Tak          | `application/json`                                        |
| authorization      | Yes          | JSON web token (JWT) token elementu nośnego.                    |
| x-ms-marketplace-token| Tak| Parametr tokenu zapytania w adresie URL, gdy użytkownik jest przekierowywany do witryny sieci Web SaaS ISV na platformie Azure. **Uwaga:** Ten token jest prawidłowy tylko przez 1 godzinę. Ponadto dekodowanie URL wartość tokenu za pomocą przeglądarki przed jego użyciem.|
|  |  |  |
  

*Treść odpowiedzi*

``` json
{
    "id": "",
    "subscriptionName": "",
    "offerId": "",
    "planId": "",
}
```

| **Nazwa parametru** | **Typ danych** | **Opis**                       |
|--------------------|---------------|---------------------------------------|
| id                 | String        | Identyfikator subskrypcji SaaS.          |
| subscriptionName| String| Nazwa subskrypcji SaaS ustawione przez użytkownika w systemie Azure podczas subskrybowania usługi SaaS.|
| OfferId            | String        | Identyfikator oferty, które subskrybuje użytkownik. |
| planId             | String        | Identyfikator planu, który użytkownik.  |
|  |  |  |


*Kody odpowiedzi*

| **Kod stanu HTTP** | **Kod błędu:**     | **Opis**                                                                         |
|----------------------|--------------------| --------------------------------------------------------------------------------------- |
| 200                  | `OK`                 | Token zostały rozpoznane prawidłowo.                                                            |
| 400                  | `BadRequest`         | Wymagane albo brakuje nagłówków lub określono nieprawidłową wersję interfejsu api. Nie można rozpoznać tokenu, ponieważ albo token jest źle sformułowany lub wygasłe (token jest prawidłowy tylko przez 1 godzinę po wygenerowaniu). |
| 403                  | `Forbidden`          | Obiekt wywołujący nie ma uprawnień do wykonania tej operacji.                                 |
| 429                  | `RequestThrottleId`  | Usługa jest zajęty przetwarzaniem żądania, spróbuj ponownie później.                                |
| 503                  | `ServiceUnavailable` | Usługa jest w dół tymczasowo niedostępny, spróbuj ponownie później.                                        |
|  |  |  |


*Nagłówki odpowiedzi*

| **Klucz nagłówka**     | **Wymagane** | **Opis**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | Żądaj Identyfikatora otrzymanych od klienta.                                                                   |
| x-ms-correlationid | Tak          | Identyfikator korelacji, jeśli przekazany przez klienta, w przeciwnym razie wartość ta jest identyfikator serwera korelacji.                   |
| x-ms-activityid    | Tak          | Unikatowy ciąg wartości dla śledzenia żądania z usługi. Ten identyfikator jest używany do żadnych uzgadniania. |
| Retry-After        | Nie           | Ta wartość jest ustawiona tylko w przypadku odpowiedzi 429.                                                                   |
|  |  |  |


### <a name="subscribe"></a>Subskrybuj

Subskrybuj punktu końcowego umożliwia użytkownikom Rozpocznij subskrypcję do usługi SaaS dla danego planu i włączenia opcji naliczania opłat w systemie handlu.

**PUT**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{subscriptionId}* ?api-version=2017-04-15**

| **Nazwa parametru**  | **Opis**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Unikatowy identyfikator SaaS subskrypcja, która uzyskuje się po usunięciu tokenu za pośrednictwem rozwiązania interfejsu API.                              |
| api-version         | Wersja operacji przy użyciu dla tego żądania. |
|  |  |

*Nagłówki*

|  **Klucz nagłówka**        | **Wymagane** |  **Opis**                                                  |
| ------------------     | ------------ | --------------------------------------------------------------------------------------- |
| x-ms-requestid         |   Nie         | Unikatowy ciąg wartości do śledzenia żądania z klienta, najlepiej z identyfikatorem GUID. Jeśli nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi. |
| x-ms-correlationid     |   Nie         | Unikatowy ciąg wartości dla operacji na komputerze klienckim. Ta wartość jest do korelacji wszystkich zdarzeń z operacji klienta ze zdarzeniami po stronie serwera. Jeśli nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi. |
| If-Match/If-None-Match |   Nie         |   Silne modułu sprawdzania poprawności wartość elementu ETag.                                                          |
| Typ zawartości           |   Tak        |    `application/json`                                                                   |
|  authorization         |   Tak        |    JSON web token (JWT) token elementu nośnego.                                               |
| x-ms-marketplace-session-mode| Nie | Flaga włączenia trybu uruchomienia próbnego podczas subskrybowania oferty SaaS. Jeśli ustawiona, subskrypcja nie zostanie obciążona. Jest to przydatne dla niezależnych dostawców oprogramowania, testowania scenariuszy. Ustaw ją na **"próbnym uruchamianiem"**|
|  |  |  |

*Treść*

``` json
{
    "lanId": "",
}
```

| **Nazwa elementu** | **Typ danych** | **Opis**                      |
|------------------|---------------|--------------------------------------|
| planId           | (Wymagane) Ciąg        | Subskrybowanie identyfikator planu użytkownika usługi SaaS.  |
|  |  |  |

*Kody odpowiedzi*

| **Kod stanu HTTP** | **Kod błędu:**     | **Opis**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Aktywacja subskrypcji SaaS dla danego planu.                   |
| 400                  | `BadRequest`         | Wymagane albo brakuje nagłówków lub treść JSON jest nieprawidłowo sformułowany. |
| 403                  | `Forbidden`          | Obiekt wywołujący nie ma uprawnień do wykonania tej operacji.                   |
| 404                  | `NotFound`           | Nie znaleziono z danym Identyfikatorem subskrypcji                                  |
| 409                  | `Conflict`           | Trwa wykonywanie innej operacji w ramach subskrypcji.                     |
| 429                  | `RequestThrottleId`  | Usługa jest zajęty przetwarzaniem żądania, spróbuj ponownie później.                  |
| 503                  | `ServiceUnavailable` | Usługa jest w dół tymczasowo niedostępny, spróbuj ponownie później.                          |
|  |  |  |

Odpowiedzi 202 monitowanie o stanie operacji żądania w nagłówku "Operacja location". Uwierzytelnianie jest taka sama jak innych interfejsów API portalu Marketplace.

*Nagłówki odpowiedzi*

| **Klucz nagłówka**     | **Wymagane** | **Opis**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | Żądaj Identyfikatora otrzymanych od klienta.                                                                   |
| x-ms-correlationid | Tak          | Identyfikator korelacji, jeśli przekazany przez klienta, w przeciwnym razie wartość ta jest identyfikator serwera korelacji.                   |
| x-ms-activityid    | Tak          | Unikatowy ciąg wartości dla śledzenia żądania z usługi. Ta wartość jest używana do dowolnego uzgadniania. |
| Retry-After        | Yes          | Stan można sprawdzić na interwał, z którego komputera klienckiego.                                                       |
| Operacja lokalizacji | Tak          | Link do zasobu, można uzyskać stanu operacji.                                                        |
|  |  |  |

### <a name="change-plan-endpoint"></a>Punkt końcowy plan zmiany

Zmiana punktu końcowego umożliwia użytkownikowi konwertowanie ich aktualnie subskrybowanych plan do nowego planu.

**POPRAWKI**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{subscriptionId}* ?api-version=2017-04-15**

| **Nazwa parametru**  | **Opis**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Subskrypcja identyfikator SaaS.                              |
| api-version         | Wersja operacji przy użyciu dla tego żądania. |
|  |  |

*Nagłówki*

| **Klucz nagłówka**          | **Wymagane** | **Opis**                                                                                                                                                                                                                  |
|-------------------------|--------------|---------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid          | Nie           | Unikatowy ciąg wartości dla śledzenia żądania od klienta. Zaleca się identyfikator GUID. Jeśli nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi.   |
| x-ms-correlationid      | Nie           | Unikatowy ciąg wartości dla operacji na komputerze klienckim. Ta wartość jest do korelacji wszystkich zdarzeń z operacji klienta ze zdarzeniami po stronie serwera. Jeśli nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi. |
| If-Match /If-None-Match | Nie           | Silne modułu sprawdzania poprawności wartość elementu ETag.                              |
| Typ zawartości            | Tak          | `application/json`                                        |
| authorization           | Tak          | JSON web token (JWT) token elementu nośnego.                    |
|  |  |  |

*Treść*

```json
{
    "planId": ""
}
```

|  **Nazwa elementu** |  **Typ danych**  | **Opis**                              |
|  ---------------- | -------------   | --------------------------------------       |
|  planId           |  (Wymagane) Ciąg         | Subskrybowanie identyfikator planu użytkownika usługi SaaS.          |
|  |  |  |

*Kody odpowiedzi*

| **Kod stanu HTTP** | **Kod błędu:**     | **Opis**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Aktywacja subskrypcji SaaS dla danego planu.                   |
| 400                  | `BadRequest`         | Wymagane albo brakuje nagłówków lub treść JSON jest nieprawidłowo sformułowany. |
| 403                  | `Forbidden`          | Obiekt wywołujący nie ma uprawnień do wykonania tej operacji.                   |
| 404                  | `NotFound`           | Nie znaleziono z danym Identyfikatorem subskrypcji                                  |
| 409                  | `Conflict`           | Trwa wykonywanie innej operacji w ramach subskrypcji.                     |
| 429                  | `RequestThrottleId`  | Usługa jest zajęty przetwarzaniem żądania, spróbuj ponownie później.                  |
| 503                  | `ServiceUnavailable` | Usługa jest w dół tymczasowo niedostępny, spróbuj ponownie później.                          |
|  |  |  |

*Nagłówki odpowiedzi*

| **Klucz nagłówka**     | **Wymagane** | **Opis**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | Żądaj Identyfikatora otrzymanych od klienta.                                                                   |
| x-ms-correlationid | Tak          | Identyfikator korelacji, jeśli przekazany przez klienta, w przeciwnym razie wartość ta jest identyfikator serwera korelacji.                   |
| x-ms-activityid    | Yes          | Unikatowy ciąg wartości dla śledzenia żądania z usługi. Ta wartość jest używana do dowolnego uzgadniania. |
| Retry-After        | Tak          | Stan można sprawdzić na interwał, z którego komputera klienckiego.                                                       |
| Operacja lokalizacji | Tak          | Link do zasobu, można uzyskać stanu operacji.                                                        |
|  |  |  |

### <a name="delete-subscription"></a>Usuwanie subskrypcji

Akcja usuwania w punkcie końcowym Subskrybuj umożliwia użytkownikowi usuwanie subskrypcji o podanym identyfikatorze.

*Żądanie*

**DELETE**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{subscriptionId}* ?api-version=2017-04-15**

| **Nazwa parametru**  | **Opis**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Subskrypcja identyfikator SaaS.                              |
| api-version         | Wersja operacji przy użyciu dla tego żądania. |
|  |  |

*Nagłówki*

| **Klucz nagłówka**     | **Wymagane** | **Opis**                                                                                                                                                                                                                  |
|--------------------|--------------| ----------------------------------------------------------|
| x-ms-requestid     | Nie           | Unikatowy ciąg wartości dla śledzenia żądania od klienta. Zaleca się identyfikator GUID. Jeśli ta wartość nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi.                                                           |
| x-ms-correlationid | Nie           | Unikatowy ciąg wartości dla operacji na komputerze klienckim. Ta wartość jest do korelacji wszystkich zdarzeń z operacji klienta ze zdarzeniami po stronie serwera. Jeśli nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi. |
| authorization      | Yes          | JSON web token (JWT) token elementu nośnego.                    |
|  |  |  |

*Kody odpowiedzi*

| **Kod stanu HTTP** | **Kod błędu:**     | **Opis**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Aktywacja subskrypcji SaaS dla danego planu.                   |
| 400                  | `BadRequest`         | Wymagane albo brakuje nagłówków lub treść JSON jest nieprawidłowo sformułowany. |
| 403                  | `Forbidden`          | Obiekt wywołujący nie ma uprawnień do wykonania tej operacji.                   |
| 404                  | `NotFound`           | Nie znaleziono z danym Identyfikatorem subskrypcji                                  |
| 429                  | `RequestThrottleId`  | Usługa jest zajęty przetwarzaniem żądania, spróbuj ponownie później.                  |
| 503                  | `ServiceUnavailable` | Usługa jest tymczasowo w dół. Spróbuj ponownie później.                          |
|  |  |  |

Odpowiedzi 202 monitowanie o stanie operacji żądania w nagłówku "Operacja location". Uwierzytelnianie jest taka sama jak innych interfejsów API portalu Marketplace.

*Nagłówki odpowiedzi*

| **Klucz nagłówka**     | **Wymagane** | **Opis**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | Żądaj Identyfikatora otrzymanych od klienta.                                                                   |
| x-ms-correlationid | Tak          | Identyfikator korelacji, jeśli przekazany przez klienta, w przeciwnym razie, to jest identyfikator serwera korelacji.                   |
| x-ms-activityid    | Tak          | Unikatowy ciąg wartości dla śledzenia żądania z usługi. Służy to do dowolnego uzgadniania. |
| Retry-After        | Tak          | Stan można sprawdzić na interwał, z którego komputera klienckiego.                                                       |
| Operacja lokalizacji | Yes          | Link do zasobu, można uzyskać stanu operacji.                                                        |
|   |  |  |

### <a name="get-operation-status"></a>Pobierz stan operacji

Ten punkt końcowy umożliwia użytkownikowi śledzenie stanu operacji asynchronicznych wyzwolone (Subskrybuj/anulowania subskrypcji/Zmień plan).

*Żądanie*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/operations/ *{operationId}* ?api-version=2017-04-15**

| **Nazwa parametru**  | **Opis**                                       |
|---------------------|-------------------------------------------------------|
| operationId         | Unikatowy identyfikator dla tej operacji wyzwolone.                |
| api-version         | Wersja operacji przy użyciu dla tego żądania. |
|  |  |

*Nagłówki*

| **Klucz nagłówka**     | **Wymagane** | **Opis**                                                                                                                                                                                                                  |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Nie           | Unikatowy ciąg wartości dla śledzenia żądania od klienta. Zaleca się identyfikator GUID. Jeśli ta wartość nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi.   |
| x-ms-correlationid | Nie           | Unikatowy ciąg wartości dla operacji na komputerze klienckim. Ta wartość jest do korelacji wszystkich zdarzeń z operacji klienta ze zdarzeniami po stronie serwera. Jeśli ta wartość nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi.  |
| authorization      | Tak          | JSON web token (JWT) token elementu nośnego.                    |
|  |  |  | 

*Treść odpowiedzi*

```json
{
    "id": "",
    "status": "",
    "resourceLocation": "",
    "created": "",
    "lastModified": ""
}
```

| **Nazwa parametru** | **Typ danych** | **Opis**                                                                                                                                               |
|--------------------|---------------|-------------------------------------------------------------------------------------------|
| id                 | String        | Identyfikator operacji.                                                                      |
| status             | Enum          | Stan operacji, jedną z następujących: `In Progress`, `Succeeded`, lub `Failed`.          |
| resourceLocation   | String        | Link do subskrypcji, który został utworzony lub zmodyfikowany. Dzięki temu klient, aby uzyskać zaktualizowany stan operacji post. Ta wartość nie jest ustawiony dla `Unsubscribe` operacji. |
| Utworzone            | DateTime      | Czas utworzenia operacji w formacie UTC.                                                           |
| lastModified       | DateTime      | Ostatnia aktualizacja o nieudanej operacji w formacie UTC.                                                      |
|  |  |  |

*Kody odpowiedzi*

| **Kod stanu HTTP** | **Kod błędu:**     | **Opis**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Żądanie get zostały rozpoznane prawidłowo i treści zawiera odpowiedź.    |
| 400                  | `BadRequest`         | Wymagane albo brak nagłówków lub określono nieprawidłową wersję interfejsu api. |
| 403                  | `Forbidden`          | Obiekt wywołujący nie ma uprawnień do wykonania tej operacji.                      |
| 404                  | `NotFound`           | Nie odnaleziono subskrypcji z podanym identyfikatorze.                                     |
| 429                  | `RequestThrottleId`  | Usługa jest zajęty przetwarzaniem żądania, spróbuj ponownie później.                     |
| 503                  | `ServiceUnavailable` | Usługa jest w dół tymczasowo niedostępny, spróbuj ponownie później.                             |
|  |  |  |

*Nagłówki odpowiedzi*

| **Klucz nagłówka**     | **Wymagane** | **Opis**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Tak          | Żądaj Identyfikatora otrzymanych od klienta.                                                                   |
| x-ms-correlationid | Tak          | Identyfikator korelacji, jeśli przekazany przez klienta, w przeciwnym razie, to jest identyfikator serwera korelacji.                   |
| x-ms-activityid    | Tak          | Unikatowy ciąg wartości dla śledzenia żądania z usługi. Służy to do dowolnego uzgadniania. |
| Retry-After        | Tak          | Stan można sprawdzić na interwał, z którego komputera klienckiego.                                                       |
|  |  |  |

### <a name="get-subscription"></a>Uzyskiwanie subskrypcji

Subskrybowanie akcję Get na punkt końcowy pozwala na użytkownika, aby pobrać subskrypcji o identyfikatorze danego zasobu.

*Żądanie*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{subscriptionId}* ?api-version=2017-04-15**

| **Nazwa parametru**  | **Opis**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Subskrypcja identyfikator SaaS.                              |
| api-version         | Wersja operacji przy użyciu dla tego żądania. |
|  |  |

*Nagłówki*

| **Klucz nagłówka**     | **Wymagane** | **Opis**                                                                                           |
|--------------------|--------------|-----------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Nie           | Unikatowy ciąg wartości do śledzenia żądania z klienta, najlepiej z identyfikatorem GUID. Jeśli ta wartość nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi.                                                           |
| x-ms-correlationid | Nie           | Unikatowy ciąg wartości dla operacji na komputerze klienckim. Ta wartość jest do korelacji wszystkich zdarzeń z operacji klienta ze zdarzeniami po stronie serwera. Jeśli ta wartość nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi. |
| authorization      | Yes          | JSON web token (JWT) token elementu nośnego.                                                                    |
|  |  |  |

*Treść odpowiedzi*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Nazwa parametru**     | **Typ danych** | **Opis**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | String        | Zasób subskrypcji identyfikator SaaS na platformie Azure.    |
| offerId                | String        | Identyfikator oferty, które subskrybuje użytkownik.         |
| planId                 | String        | Identyfikator planu, który użytkownik.          |
| saasSubscriptionName   | String        | Nazwa subskrypcji SaaS.                |
| saasSubscriptionStatus | Enum          | Stan operacji.  Jeden z następujących czynności:  <br/> - `Subscribed`: Subskrypcja jest aktywna.  <br/> - `Pending`: Użytkownik utworzenia zasobu, ale nie zostanie aktywowany przez niezależnych dostawców oprogramowania.   <br/> - `Unsubscribed`: Anulowano subskrypcję użytkownika.   <br/> - `Suspended`: Użytkownik został zawieszony subskrypcji.   <br/> - `Deactivated`:  Subskrypcja platformy Azure została zawieszona.  |
| Utworzone                | DateTime      | Wartość sygnatury czasowej tworzenia subskrypcji w formacie UTC. |
| lastModified           | DateTime      | Subskrypcja zmodyfikowana wartość znacznika czasu w formacie UTC. |
|  |  |  |

*Kody odpowiedzi*

| **Kod stanu HTTP** | **Kod błędu:**     | **Opis**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Żądanie get zostały rozpoznane prawidłowo i treści zawiera odpowiedź.    |
| 400                  | `BadRequest`         | Wymagane albo brak nagłówków lub określono nieprawidłową wersję interfejsu api. |
| 403                  | `Forbidden`          | Obiekt wywołujący nie ma uprawnień do wykonania tej operacji.                      |
| 404                  | `NotFound`           | Nie znaleziono z danym Identyfikatorem subskrypcji                                     |
| 429                  | `RequestThrottleId`  | Usługa jest zajęty przetwarzaniem żądania, spróbuj ponownie później.                     |
| 503                  | `ServiceUnavailable` | Usługa jest w dół tymczasowo niedostępny, spróbuj ponownie później.                             |
|  |  |  |

*Nagłówki odpowiedzi*

| **Klucz nagłówka**     | **Wymagane** | **Opis**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | Żądaj Identyfikatora otrzymanych od klienta.                                                                   |
| x-ms-correlationid | Tak          | Identyfikator korelacji, jeśli przekazany przez klienta, w przeciwnym razie, to jest identyfikator serwera korelacji.                   |
| x-ms-activityid    | Tak          | Unikatowy ciąg wartości dla śledzenia żądania z usługi. Służy to do dowolnego uzgadniania. |
| Retry-After        | Nie           | Stan można sprawdzić na interwał, z którego komputera klienckiego.                                                       |
| eTag               | Tak          | Link do zasobu, można uzyskać stanu operacji.                                                        |
|  |  |  |

### <a name="get-subscriptions"></a>Pobiera subskrypcje

Akcja Get w punkcie końcowym subskrypcje umożliwia użytkownika, aby pobrać wszystkie subskrypcje dla wszystkich ofert z niezależnym dostawcą oprogramowania.

*Żądanie*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2017-04-15**

| **Nazwa parametru**  | **Opis**                                       |
|---------------------|-------------------------------------------------------|
| api-version         | Wersja operacji przy użyciu dla tego żądania. |
|  |  |

*Nagłówki*

| **Klucz nagłówka**     | **Wymagane** | **Opis**                                           |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Nie           | Unikatowy ciąg wartości dla śledzenia żądania od klienta. Zaleca się identyfikator GUID. Jeśli ta wartość nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi.             |
| x-ms-correlationid | Nie           | Unikatowy ciąg wartości dla operacji na komputerze klienckim. Ta wartość jest do korelacji wszystkich zdarzeń z operacji klienta ze zdarzeniami po stronie serwera. Jeśli ta wartość nie zostanie podany, jeden zostanie wygenerowany i podana w nagłówki odpowiedzi. |
| authorization      | Tak          | JSON web token (JWT) token elementu nośnego.                    |
|  |  |  |

*Treść odpowiedzi*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Nazwa parametru**     | **Typ danych** | **Opis**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | String        | Zasób subskrypcji identyfikator SaaS na platformie Azure    |
| offerId                | String        | Identyfikator oferty, który użytkownik         |
| planId                 | String        | Identyfikator planu, który użytkownik          |
| saasSubscriptionName   | String        | Nazwa subskrypcji SaaS                |
| saasSubscriptionStatus | Enum          | Stan operacji.  Jeden z następujących czynności:  <br/> - `Subscribed`: Subskrypcja jest aktywna.  <br/> - `Pending`: Użytkownik utworzenia zasobu, ale nie zostanie aktywowany przez niezależnych dostawców oprogramowania.   <br/> - `Unsubscribed`: Anulowano subskrypcję użytkownika.   <br/> - `Suspended`: Użytkownik został zawieszony subskrypcji.   <br/> - `Deactivated`:  Subskrypcja platformy Azure została zawieszona.  |
| Utworzone                | DateTime      | Wartość znacznika czasu tworzenia subskrypcji w formacie UTC |
| lastModified           | DateTime      | Subskrypcja zmodyfikować wartość znacznika czasu w formacie UTC |
|  |  |  |

*Kody odpowiedzi*

| **Kod stanu HTTP** | **Kod błędu:**     | **Opis**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Żądanie get zostały rozpoznane prawidłowo i treści zawiera odpowiedź.    |
| 400                  | `BadRequest`         | Wymagane albo brak nagłówków lub określono nieprawidłową wersję interfejsu api. |
| 403                  | `Forbidden`          | Obiekt wywołujący nie ma uprawnień do wykonania tej operacji.                      |
| 404                  | `NotFound`           | Nie znaleziono z danym Identyfikatorem subskrypcji                                     |
| 429                  | `RequestThrottleId`  | Usługa jest zajęty przetwarzaniem żądania, spróbuj ponownie później.                     |
| 503                  | `ServiceUnavailable` | Usługa jest tymczasowo w dół. Spróbuj ponownie później.                             |
|  |  |  |

*Nagłówki odpowiedzi*

| **Klucz nagłówka**     | **Wymagane** | **Opis**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | Żądaj Identyfikatora otrzymanych od klienta.                                                                   |
| x-ms-correlationid | Tak          | Identyfikator korelacji, jeśli przekazany przez klienta, w przeciwnym razie, to jest identyfikator serwera korelacji.                   |
| x-ms-activityid    | Yes          | Unikatowy ciąg wartości dla śledzenia żądania z usługi. Służy to do dowolnego uzgadniania. |
| Retry-After        | Nie           | Stan można sprawdzić na interwał, z którego komputera klienckiego.                                                       |
|  |  |  |

### <a name="saas-webhook"></a>SaaS Webhook

Element webhook SaaS jest używana do powiadamiania aktywnie zmiany w usłudze SaaS. Ten interfejs API POST powinien być nieuwierzytelnione i zostanie wywołana przez usługę Microsoft. Oczekiwano usługi SaaS do wywoływania operacji interfejsu API, aby zweryfikować i autoryzować przed podjęciem działania na powiadomień elementu webhook. 

*Treść*

``` json
  {
    "id": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "action": "Subscribe", // Subscribe/Unsubscribe/ChangePlan
    "operationRequestSource":"Azure",
    "timeStamp":"2018-12-01T00:00:00"
  }
```

| **Nazwa parametru**     | **Typ danych** | **Opis**                               |
|------------------------|---------------|-----------------------------------------------|
| id  | String       | Unikatowy identyfikator dla tej operacji wyzwolone.                |
| activityId   | String        | Unikatowy ciąg wartości dla śledzenia żądania z usługi. Służy to do dowolnego uzgadniania.               |
| subscriptionId                     | String        | Zasób subskrypcji identyfikator SaaS na platformie Azure.    |
| offerId                | String        | Identyfikator oferty, które subskrybuje użytkownik. Podany tylko w przypadku akcji "Aktualizuj".        |
| publisherId                | String        | Identyfikator wydawcy, oferty SaaS         |
| planId                 | String        | Identyfikator planu, który użytkownik. Podany tylko w przypadku akcji "Aktualizuj".          |
| action                 | String        | Akcja, która jest przyczyną tego powiadomienia. Możliwe wartości - aktywowania, Usuń, Wstrzymaj, przywrócenie, aktualizację          |
| timeStamp                 | String        | Wartość znacznika czasu w formacie UTC to powiadomienie zostało wyzwolone.          |
|  |  |  |


## <a name="next-steps"></a>Kolejne kroki

Deweloperzy mogą także programowo pobrać i manipulowania wydawcy, oferty i obciążeń, profile, przy użyciu [interfejsów API REST usługi Cloud Partner Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
