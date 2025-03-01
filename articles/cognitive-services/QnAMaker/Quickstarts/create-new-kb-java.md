---
title: 'Szybki start: Tworzenie bazy wiedzy — środowisko REST, Java — QnA Maker'
titlesuffix: Azure Cognitive Services
description: Ten przewodnik Szybki start oparty na protokole Java REST zawiera omówienie programistycznego tworzenia przykładowej bazy wiedzy usługi QnA Maker, która zostanie wyświetlona na pulpicie nawigacyjnym platformy Azure w ramach konta interfejsu API usługi Cognitive Services.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/04/2019
ms.author: diberry
ms.openlocfilehash: c6fddec80da1baa852a2f3712a94b785d775919e
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67151041"
---
# <a name="quickstart-create-a-knowledge-base-in-qna-maker-using-java"></a>Szybki start: Tworzenie bazy wiedzy w usłudze QnA Maker przy użyciu języka Java

Ten przewodnik Szybki start przeprowadzi Cię przez programowe tworzenie przykładowej bazy wiedzy usługi QnA Maker. Usługa QnA Maker automatycznie wyodrębnia pytania i odpowiedzi z częściowo ustrukturyzowanej zawartości, na przykład często zadawanych pytań, ze [źródeł danych](../Concepts/data-sources-supported.md). Model bazy wiedzy jest zdefiniowany w formacie JSON wysyłanym w treści żądania interfejsu API.

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../../includes/cognitive-services-qnamaker-java-repo-note.md)]

## <a name="create-a-knowledge-base-file"></a>Tworzenie pliku bazy wiedzy

Utwórz plik o nazwie `CreateKB.java`.

## <a name="add-the-required-dependencies"></a>Dodawanie wymaganych zależności

Na początku pliku `CreateKB.java` dodaj następujące wiersze, aby dodać niezbędne zależności do projektu:

[!code-java[Add the required dependencies](~/samples-qnamaker-java/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java?range=1-5 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Dodawanie wymaganych stałych
Po poprzednich wymaganych zależnościach dodaj wymagane stałe do klasy `CreateKB` umożliwiające dostęp do usługi QnA Maker. Zastąp wartość zmiennej `subscriptionKey` własnym kluczem usługi QnA Maker. Nie jest konieczne dodawanie końcowego nawiasu klamrowego, aby zakończyć klasę — znajduje się on w ostatnim fragmencie kodu na końcu tego przewodnika Szybki Start.

[!code-java[Add the required constants](~/samples-qnamaker-java/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java?range=26-34 "Add the required constants")]


## <a name="add-the-kb-model-definition-classes"></a>Dodawanie klas definicji modelu bazy wiedzy
Poniżej stałych dodaj następujące klasy i funkcje wewnątrz klasy `CreateKB` w celu serializacji obiektu definicji modelu do postaci JSON.

[!code-java[Add the KB model definition classes](~/samples-qnamaker-java/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java?range=36-80 "Add the KB model definition classes")]

## <a name="add-supporting-functions"></a>Dodawanie funkcji pomocniczych

Następnie dodaj poniższe funkcje pomocnicze wewnątrz klasy `CreateKB`.

1. Dodaj następującą funkcję, aby wyświetlić dane JSON w czytelnym formacie:

    [!code-java[Add the PrettyPrint function](~/samples-qnamaker-java/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java?range=82-87 "Add the KB model definition classes")]

2. Dodaj następującą klasę służącą do zarządzania odpowiedzią HTTP:

    [!code-java[Add class to manage the HTTP response](~/samples-qnamaker-java/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java?range=89-97 "Add class to manage the HTTP response")]

3. Dodaj następującą metodę służącą do wysyłania żądania POST do interfejsów API usługi QnA Maker. `Ocp-Apim-Subscription-Key` to klucz usługi QnA Maker używany do uwierzytelniania.

    [!code-java[Add POST method](~/samples-qnamaker-java/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java?range=99-121 "Add POST method")]

4. Dodaj następującą metodę służącą do wysyłania żądania GET do interfejsów API usługi QnA Maker.

    [!code-java[Add GET method](~/samples-qnamaker-java/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java?range=123-137 "Add GET method")]

## <a name="add-a-method-to-create-the-kb"></a>Dodawanie metody służącej do tworzenia bazy wiedzy
Dodaj następującą metodę służącą do tworzenia bazy wiedzy przez wywołanie metody POST.

[!code-java[Add CreateKB method](~/samples-qnamaker-java/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java?range=139-144 "Add CreateKB method")]

To wywołanie interfejsu API zwraca odpowiedź w formacie JSON, która zawiera identyfikator operacji. Użyj tego identyfikatora operacji, aby określić, czy baza wiedzy została pomyślnie utworzona.

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "8dfb6a82-ae58-4bcb-95b7-d1239ae25681"
}
```

## <a name="add-a-method-to-get-status"></a>Dodawanie metody służącej do wyświetlania stanu
Dodaj następującą metodę służącą do sprawdzania stanu tworzenia.

[!code-java[Add GetStatus method](~/samples-qnamaker-java/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java?range=146-150 "Add GetStatus method")]

Powtarzaj wywołanie do momentu uzyskania stanu powodzenia lub niepowodzenia:

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-a-main-method"></a>Dodawanie metody głównej
Metoda główna tworzy bazę wiedzy, a następnie wykonuje sondowanie pod kątem stanu. Operacja identyfikator jest zwracany w polu nagłówka odpowiedzi WPIS **lokalizacji**, następnie użyta jako część tras w żądanie GET. `while` Pętli ponawia próbę stan, jeśli nie zostało ukończone.

[!code-java[Add main method](~/samples-qnamaker-java/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java?range=152-191 "Add main method")]

## <a name="compile-and-run-the-program"></a>Kompilowanie i uruchamianie programu

1. Upewnij się, że biblioteka GSON znajduje się w katalogu `./libs`. W wierszu polecenia skompiluj plik `CreateKB.java`:

    ```bash
    javac -cp ".;libs/*" CreateKB.java
    ```

2. Wprowadź następujące polecenie w wierszu polecenia, aby uruchomić program. Program wyśle żądanie do interfejsu API usługi QnA Maker, aby utworzyć bazę wiedzy, a następnie będzie sondować wyniki co 30 sekund. Każda odpowiedź jest wypisywana w oknie konsoli.

    ```base
    java -cp ",;libs/*" CreateKB
    ```

Utworzoną bazę wiedzy można wyświetlić w portalu usługi QnA Maker, na stronie [My knowledge bases (Moje bazy wiedzy)](https://www.qnamaker.ai/Home/MyServices).

[!INCLUDE [Clean up files and KB](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [QnA Maker (V4) REST API Reference (Dokumentacja interfejsu API REST usługi QnA Maker w wersji 4)](https://go.microsoft.com/fwlink/?linkid=2092179)
