---
title: Tworzenie nowej aplikacji
titleSuffix: Language Understanding - Azure Cognitive Services
description: Utwórz aplikacje i zarządzaj nimi na stronie sieci Web Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: b8b0cebf4ba47f875caacfcfbf89b84551b41333
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341857"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>Utwórz nową aplikację usługi LUIS w portalu usługi LUIS
Istnieje kilka sposobów, aby utworzyć aplikację usługi LUIS. Można utworzyć aplikację usługi LUIS w [LUIS](https://www.luis.ai) portalu lub za pomocą usługi LUIS tworzenia [interfejsów API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

## <a name="using-the-luis-portal"></a>Za pomocą portalu usługi LUIS
Można utworzyć nową aplikację w portalu usługi LUIS na kilka sposobów:

* Rozpocznij od pusta aplikacja i tworzyć intencji, wypowiedzi i jednostek.
* Start z pustą aplikacją, a następnie dodaj [ze wstępnie utworzonych domen](luis-how-to-use-prebuilt-domains.md).
* Importowanie aplikacji usługi LUIS z pliku JSON, który zawiera już intencji, wypowiedzi i jednostek.

## <a name="using-the-authoring-apis"></a>Za pomocą tworzenia interfejsów API
Można utworzyć nową aplikację za pomocą tworzenia interfejsów API na kilka sposobów:

* [Rozpocznij](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) z pustą aplikację i utworzyć intencji, wypowiedzi i jednostek.
* [Rozpocznij](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/59104e515aca2f0b48c76be5) przy użyciu wbudowanych domeny.  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

## <a name="create-new-app-in-luis"></a>Utwórz nową aplikację w usługi LUIS

1. Na **Moje aplikacje** wybierz opcję **Utwórz nową aplikację**.

    ![Lista aplikacji LUIS](./media/luis-create-new-app/apps-list.png)


2. W oknie dialogowym Nazwa aplikacji "TravelAgent".

    ![Utwórz nowe okno dialogowe aplikacji](./media/luis-create-new-app/create-app.png)

3. Wybierz Twojej kulturze aplikacji (TravelAgent aplikacji, wybierz język angielski), a następnie wybierz pozycję **gotowe**. 

    > [!NOTE]
    > Kultury nie można zmienić po utworzeniu aplikacji. 

## <a name="import-an-app-from-file"></a>Importowanie aplikacji z pliku

1. Na **Moje aplikacje** wybierz opcję **importowania Nowa aplikacja**.
1. W podręcznym oknie dialogowym Wybierz plik prawidłową aplikację w formacie JSON, a następnie wybierz **gotowe**.

### <a name="import-errors"></a>Błędy importowania

Błędy możliwe są następujące: 

* Aplikacja o tej nazwie już istnieje. Aby rozwiązać ten problem, ponownie zaimportować aplikację i ustaw **opcjonalna nazwa** pod nową nazwą. 

## <a name="export-app-for-backup"></a>Eksportowanie aplikacji do tworzenia kopii zapasowych

1. Na **Moje aplikacje** wybierz opcję **wyeksportować**.
1. Wybierz **wyeksportować jako plik JSON**. Przeglądarka pobiera active wersję aplikacji.
1. Dodaj ten plik w systemie tworzenia kopii zapasowej do archiwizacji modelu.

## <a name="export-app-for-containers"></a>Eksportowanie aplikacji dla kontenerów

1. Na **Moje aplikacje** wybierz opcję **wyeksportować**.
1. Wybierz **wyeksportować jako kontener** następnie wybierz, jakie gniazda opublikowane (w środowisku produkcyjnym lub etapu), którą chcesz wyeksportować.
1. Korzystanie z tego pliku przy użyciu usługi [kontenera usługi LUIS](luis-container-howto.md). 

    Jeśli interesuje Cię eksportowanie przeszkolonych, ale nie jeszcze opublikowany model w celu użycia z kontenerem usługi LUIS, przejdź do strony **wersji** strony, a następnie wyeksportować z tego miejsca. 

## <a name="delete-app"></a>Usuwanie aplikacji

1. Na **Moje aplikacje** wybierz wielokropek (...) na końcu wiersza aplikacji,.
1. Wybierz **Usuń** z menu.
1. Wybierz **Ok** w oknie potwierdzenia.

## <a name="next-steps"></a>Kolejne kroki

Pierwsze zadanie w aplikacji jest [Dodawanie intencji](luis-how-to-add-intents.md).
