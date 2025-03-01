---
title: Tworzenie, uczenie i publikowanie bazy wiedzy knowledge base — QnA Maker
titleSuffix: Azure Cognitive Services
description: Na podstawie własnej zawartości, takiej jak często zadawane pytania lub podręczniki produktów, możesz utworzyć bazę wiedzy usługi QnA Maker. Usługa QnA Maker wiedzy, w tym przykładzie jest tworzony z prostą stronę często zadawane pytania dotyczące odpowiedzi na pytania dotyczące klucza odzyskiwania funkcji BitLocker.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 05/10/2019
ms.author: diberry
ms.openlocfilehash: 609d71898d8db027f1cfee9e1b73039367ec94f4
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693375"
---
# <a name="create-train-and-publish-your-qna-maker-knowledge-base"></a>Tworzenie, szkolenie i publikowanie bazy wiedzy usługi QnA Maker

Na podstawie własnej zawartości, takiej jak często zadawane pytania lub podręczniki produktów, możesz utworzyć bazę wiedzy usługi QnA Maker. Ten artykuł zawiera przykład tworzenia usługi QnA Maker wiedzy z simple Web — często zadawane pytania, odpowiadać na pytania dotyczące klucza odzyskiwania funkcji BitLocker.

## <a name="prerequisite"></a>Wymagania wstępne

> [!div class="checklist"]
> * Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-qna-maker-knowledge-base"></a>Tworzenie bazy wiedzy usługi QnA Maker

1. Zaloguj się do [QnAMaker.ai](https://QnAMaker.ai) portalu przy użyciu swoich poświadczeń platformy Azure.

1. W portalu narzędzia QnA Maker wybierz **tworzenie bazy wiedzy**.

   ![Zrzut ekranu usługi QnA Maker w witrynie portal](../media/qna-maker-create-kb.png)

1. Na stronie **Tworzenie** w kroku 1 wybierz pozycję **Utwórz usługę QnA**. Nastąpi przekierowanie do witryny [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) w celu skonfigurowania usługi QnA Maker w ramach subskrypcji. Jeśli upłynie limit czasu witryny Azure Portal, wybierz w witrynie pozycję **Ponów próbę**. Po nawiązaniu połączenia zostanie wyświetlony pulpit nawigacyjny platformy Azure.

1. Po pomyślnym utworzeniu nowej usługi QnA Maker na platformie Azure wróć do strony qnamaker.ai/create. Wybierz usługę QnA Maker z listy rozwijanej w kroku 2. Jeśli utworzono nową usługę QnA Maker, pamiętaj odświeżyć stronę.

   ![Zrzut ekranu przedstawiający Wybieranie usługi QnA Maker wiedzy](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

1. W kroku 3, nazwa bazy wiedzy **Moje bazy wiedzy pytań i odpowiedzi przykładowe**.

1. Do dodawania zawartości do bazy wiedzy, wybierz trzy rodzaje źródeł danych. W kroku 4, w obszarze **Wypełnij bazę wiedzy** dodaj adres URL [Często zadawane pytania dotyczące odzyskiwania funkcji BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) w polu **adresu URL**.

   ![Zrzut ekranu przedstawiający dodawanie źródeł danych](../media/qnamaker-quickstart-kb/add-datasources.png)

1. W kroku 5 wybierz pozycję **Tworzenie bazy wiedzy**.

1. Gdy narzędzie QnA Maker tworzy w bazie wiedzy knowledge base, pojawi się okno podręczne. Proces wyodrębniania zajmuje kilka minut, możesz więc przeczytać stronę HTML i zidentyfikować pytania i odpowiedzi.

1. Po utworzeniu usługi QnA Maker pomyślnie bazy wiedzy knowledge base, **wiedzy** zostanie otwarta strona. Możesz edytować zawartość bazy wiedzy knowledge base na tej stronie.

## <a name="edit-the-knowledge-base"></a>Edytuj bazy wiedzy

1. W portalu narzędzia QnA Maker na **Edytuj** zaznacz **pary dodawanie pytań i odpowiedzi** Aby dodać nowy wiersz do bazy wiedzy knowledge base. W obszarze **Pytanie** wprowadź **Hi.** (Cześć). W obszarze **Odpowiedź** wprowadź **Hello. Pytaj pytań w funkcji BitLocker.**

    ![Zrzut ekranu usługi QnA Maker w witrynie portal](../media/qnamaker-quickstart-kb/add-qna-pair.png)

1. W prawym górnym rogu wybierz pozycję **Zapisz i przeszkol**, aby zapisać wprowadzone zmiany i przeszkolić model usługi QnA Maker. Zmiany nie są przechowywane, o ile nie zostaną zapisane.

## <a name="test-the-knowledge-base"></a>Testowanie bazy wiedzy

1. W prawym górnym rogu portalu narzędzia QnA Maker wybierz **Test** do przetestowania, czy zmiany wprowadzone weszło w życie. Wprowadź w polu `hi there`, a następnie naciśnij klawisz Enter. Powinna zostać wyświetlona odpowiedź utworzona jako reakcja.

1. Wybierz pozycję **Zbadaj**, aby bardziej szczegółowo sprawdzić odpowiedź. Okna testów służy do testowania zmiany w bazie wiedzy knowledge base, zanim są publikowane.

    ![Zrzut ekranu przedstawiający panelu testu](../media/qnamaker-quickstart-kb/inspect.png)

1. Wybierz ponownie pozycję **Test**, aby zamknąć wyskakujące okienko **Testowanie**.

## <a name="publish-the-knowledge-base"></a>Publikowanie bazy wiedzy

Podczas publikowania bazy wiedzy pytań i odpowiedzi zawartość bazy wiedzy przenosi się z Indeks testu do produkcji indeksu w usłudze Azure search.

![Zrzut ekranu przedstawiający przenoszenia zawartości bazy wiedzy](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

1. W menu obok portalu narzędzia QnA Maker **Edytuj**, wybierz opcję **Publikuj**. Aby potwierdzić, wybierz na stronie pozycję **Opublikuj**.

1. Usługa QnA Maker została teraz pomyślnie opublikowana. Możesz użyć punktu końcowego w swojej aplikacji lub kodu bota.

    ![Zrzut ekranu przedstawiający pomyślne publikowanie](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="create-a-bot"></a>Tworzenie botów

Po opublikowaniu tworzenia botów z **Publikuj** strony: 

* Boty kilka można utworzyć szybko, wskazując wszystkie do tej samej bazy wiedzy w różnych regionach lub cennikami dla poszczególnych botów. 
* Jeśli chcesz tylko jeden bot bazy wiedzy, użyj **wyświetlić wszystkie swoje Boty w witrynie Azure portal** link, aby wyświetlić listę bieżącego botów. 

Po wprowadzeniu zmian w bazie wiedzy knowledge base i ponownie opublikować dane, nie trzeba podjąć dalsze działania z botami. Jest już skonfigurowana do pracy z bazy wiedzy knowledge base i współpracuje z wszystkich przyszłych zmian w bazie wiedzy knowledge base. Za każdym razem, gdy opublikujesz wiedzy, wszystkie Boty, które są dołączone do niego są automatycznie aktualizowane.

1. W portalu narzędzia QnA Maker na **Publikuj** wybierz opcję **tworzenie botów**. Ten przycisk jest wyświetlana tylko wtedy, gdy zostały opublikowane bazy wiedzy knowledge base.

    ![Zrzut ekranu przedstawiający tworzenie robota](../media/qnamaker-create-publish-knowledge-base/create-bot-from-published-knowledge-base-page.png)

1. Dla witryny Azure portal, za pomocą strony tworzenia usługa Azure Bot zostanie otwarta nowa karta przeglądarki. Skonfiguruj usługę Azure bot. Aby uzyskać więcej informacji na temat tych ustawień konfiguracji, zobacz [tworzenie Bota pytań i odpowiedzi za pomocą usługi Azure Bot Service w wersji 4](../tutorials/create-qna-bot.md).
    
    * Nie należy zmieniać następujących ustawień w witrynie Azure portal, podczas tworzenia bota. Są one wstępnie wypełnione dla istniejącej bazy wiedzy: 
        * Klucz uwierzytelniania pytań i odpowiedzi
        * Plan usługi App service i lokalizacji
        * Azure Storage
    * Bot i usługi QnA Maker można udostępnić plan usługi aplikacji sieci web, ale nie można udostępnić w aplikacji sieci web. Oznacza to, że **nazwy aplikacji** musi być inna niż nazwa aplikacji, zostanie użyta podczas tworzenia usługi QnA Maker. 


## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Tworzenie bazy wiedzy](../How-To/create-knowledge-base.md)
