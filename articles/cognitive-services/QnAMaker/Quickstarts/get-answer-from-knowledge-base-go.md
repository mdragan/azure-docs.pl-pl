---
title: 'Szybki start: Uzyskiwanie odpowiedzi z bazy wiedzy — środowisko REST, Go — QnA Maker'
titlesuffix: Azure Cognitive Services
description: W tym samouczku Szybki start opartym na protokole REST i języku Go opisano sposób programowego uzyskiwania odpowiedzi z bazy wiedzy.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: 49734e50d616b3f88149f3c759e2a306ff8f136a
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65794891"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-go"></a>Uzyskiwanie odpowiedzi na pytanie z bazy wiedzy przy użyciu języka Go

Ten przewodnik Szybki start przeprowadzi Cię przez programowe uzyskiwanie odpowiedzi z opublikowanej bazy wiedzy usługi QnA Maker. Baza wiedzy zawiera pytania i odpowiedzi ze [źródeł danych](../Concepts/data-sources-supported.md) takich jak często zadawane pytania. [Pytanie](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) są wysyłane do usługi QnA Maker. [Odpowiedzi](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) zawiera przewidywany górnej odpowiedzi. 

## <a name="prerequisites"></a>Wymagania wstępne

* [Środowisko Go w wersji 1.10.1](https://golang.org/dl/)
* [Visual Studio Code](https://code.visualstudio.com/)
* Musisz mieć [usługę QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Aby uzyskać klucz, wybierz pozycję **Klucze** w obszarze **Zarządzanie zasobami** na pulpicie nawigacyjnym platformy Azure dla zasobu usługi QnA Maker. 
* Ustawienia na stronie **Publikowanie**. Jeśli nie masz opublikowanej bazy wiedzy, utwórz pustą bazę wiedzy, zaimportuj bazę wiedzy na stronie **Ustawienia**, a następnie opublikuj. Możesz pobrać [tę podstawową bazę wiedzy](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv) i używać jej. 

    Ustawienia strony publikowania obejmują wartość trasy POST, wartość hosta i wartość elementu EndpointKey. 

    ![Ustawienia publikowania](../media/qnamaker-quickstart-get-answer/publish-settings.png)

Kod używany w tym przewodniku Szybki start znajduje się w repozytorium [https://github.com/Azure-Samples/cognitive-services-qnamaker-Go](https://github.com/Azure-Samples/cognitive-services-qnamaker-Go/tree/master/documentation-samples/quickstarts/get-answer). 

## <a name="create-a-go-file"></a>Tworzenie pliku Go

Otwórz program VSCode i utwórz nowy plik o nazwie `get-answer.go`, a następnie dodaj do niego następującą klasę:

```Go
package main

func main() {

}
```

## <a name="add-the-required-dependencies"></a>Dodawanie wymaganych zależności

Powyżej funkcji `main` na początku pliku `get-answer.go` dodaj wymagane zależności do projektu:

[!code-go[Add the required dependencies](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=3-9 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Dodawanie wymaganych stałych

Na początku funkcji `main` dodaj stałe wymagane w celu uzyskiwania dostępu do usługi QnA Maker. Te wartości znajdują się na stronie **Publikowanie** po opublikowaniu bazy wiedzy. 

[!code-go[Add the required constants](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=17-33 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-answer"></a>Dodawanie żądania POST wysyłającego zapytanie i uzyskującego odpowiedź

Poniższy kod umożliwia wysłanie żądania HTTPS do interfejsu API usługi QnA Maker w celu wysłania zapytania do bazy wiedzy oraz odebrania odpowiedzi:

[!code-go[Add a POST request to send question to knowledge base](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=35-48 "Add a POST request to send question to knowledge base")]

Wartość nagłówka `Authorization` zawiera ciąg `EndpointKey`. 

Dowiedz się więcej o [żądania](../how-to/metadata-generateanswer-usage.md#generateanswer-request) i [odpowiedzi](../how-to/metadata-generateanswer-usage.md#generateanswer-response).

## <a name="build-and-run-the-program"></a>Kompilowanie i uruchamianie programu

Skompiluj i uruchom program z poziomu wiersza polecenia. Automatycznie wyśle on żądanie do interfejsu API usługi QnA Maker, a następnie wyświetli informacje w oknie konsoli.

1. Skompiluj plik:

    ```bash
    go build get-answer.go
    ```

1. Uruchom plik:

    ```bash
    ./get-answer
    ```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)] 


[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [QnA Maker (V4) REST API Reference (Dokumentacja interfejsu API REST usługi QnA Maker w wersji 4)](https://go.microsoft.com/fwlink/?linkid=2092179)
