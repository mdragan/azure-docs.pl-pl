---
title: 'Samouczek: Integracja usługi Power BI z usługą analizy tekstu w usłudze Cognitive Service'
titleSuffix: Azure Cognitive Services
description: Dowiedz się, jak za pomocą funkcji analizy tekstu wyodrębniać frazy kluczowe z tekstu w usłudze Power BI.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: tutorial
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: 24767f73e3e1409f81262ad57f3fd5152a4ec319
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60828448"
---
# <a name="tutorial-integrate-power-bi-with-the-text-analytics-cognitive-service"></a>Samouczek: Integracja usługi Power BI z usługą analizy tekstu w usłudze Cognitive Service

Microsoft Power BI Desktop to bezpłatna aplikacja umożliwiająca nawiązywanie połączeń z danymi oraz ich przekształcanie i wizualizowanie. Usługa analizy tekstu, składnik usług Microsoft Azure Cognitive Services, umożliwia przetwarzanie języka naturalnego. Dysponując nieprzetworzonym tekstem bez struktury, potrafi wyodrębnić najważniejsze frazy, przeanalizować tonację i zidentyfikować dobrze znane jednostki, takie jak marki. Używając tych narzędzi razem, można szybko zorientować się, o czym mówią klienci i jakie są ich opinie.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Importowanie i przekształcanie danych przy użyciu aplikacji Power BI Desktop
> * Tworzenie funkcji niestandardowej w aplikacji Power BI Desktop
> * Integracja aplikacji Power BI Desktop z interfejsem API fraz kluczowych usługi analizy tekstu
> * Wyodrębnianie najważniejszych fraz z opinii klientów przy użyciu interfejsu API fraz kluczowych usługi analizy tekstu
> * Tworzenie chmury słów na podstawie opinii klientów

## <a name="prerequisites"></a>Wymagania wstępne
<a name="Prerequisites"></a>

- Program Microsoft Power BI Desktop. [Pobierz bezpłatnie](https://powerbi.microsoft.com/get-started/).
- Konto platformy Microsoft Azure. [Zacznij korzystać z bezpłatnej wersji próbnej](https://azure.microsoft.com/free/) lub [zaloguj się](https://portal.azure.com/).
- Konto interfejsu API usług Cognitive Services z interfejsem API analizy tekstu. Jeśli go nie masz, możesz [utworzyć konto](../../cognitive-services-apis-create-account.md) i skorzystać z bezpłatnej warstwy 5000 transakcji miesięcznie (zobacz [szczegóły cennika](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/)), aby ukończyć ten samouczek.
- [Klucz dostępu do analizy tekstu](../how-tos/text-analytics-how-to-access-key.md) wygenerowany dla Ciebie podczas tworzenia konta.
- Komentarze klientów. Możesz użyć [naszych danych przykładowych](https://aka.ms/cogsvc/ta) lub własnych danych. W tym samouczku przyjęto założenie, że używasz naszych danych przykładowych.

## <a name="load-customer-data"></a>Ładowanie danych klientów
<a name="LoadingData"></a>

Aby rozpocząć, otwórz aplikację Power BI Desktop i załaduj plik z wartościami rozdzielanymi przecinkami (CSV) `FabrikamComments.csv`, który został pobrany w sekcji [Wymagania wstępne](#Prerequisites). Ten plik zawiera hipotetyczną aktywność z jednego dnia na forum pomocy technicznej fikcyjnej małej firmy.

> [!NOTE]
> Usługa Power BI może używać danych z wielu różnych źródeł, takich jak Facebook czy baza danych SQL. Więcej informacji można znaleźć na stronach [Integracja usługi Facebook z usługą Power BI](https://powerbi.microsoft.com/integrations/facebook/) i [Integracja programu SQL Server z usługą Power BI](https://powerbi.microsoft.com/integrations/sql-server/).

W głównym oknie aplikacji Power BI Desktop wybierz wstążkę **Narzędzia główne**. W grupie **Dane zewnętrzne** wstążki otwórz menu rozwijane **Pobierz dane** i wybierz pozycję **Tekst/CSV**.

![[Przycisk Pobierz dane]](../media/tutorials/power-bi/get-data-button.png)

Zostanie wyświetlone okno dialogowe Otwieranie. Przejdź do folderu Pobrane lub do folderu, do którego został pobrany plik `FabrikamComments.csv`. Kliknij pozycję `FabrikamComments.csv`, a następnie przycisk **Otwórz**. Zostanie wyświetlone okno dialogowe importowania pliku CSV.

![[Okno dialogowe importowania pliku CSV]](../media/tutorials/power-bi/csv-import.png)

W oknie dialogowym importowania pliku CSV można sprawdzić, czy program Power BI Desktop poprawnie wykrył zestaw znaków, ogranicznik, wiersze nagłówka i typy kolumn. Wszystkie te informacje są poprawne, dlatego kliknij pozycję **Załaduj**.

Aby zobaczyć załadowane dane, kliknij przycisk **Widok danych** przy lewej krawędzi obszaru roboczego usługi Power BI. Zostanie otwarta tabela zawierająca dane, jak w programie Microsoft Excel.

![[Widok początkowy zaimportowanych danych]](../media/tutorials/power-bi/initial-data-view.png)

## <a name="prepare-the-data"></a>Przygotowywanie danych
<a name="PreparingData"></a>

Zanim dane będą gotowe do przetworzenia przez interfejs API fraz kluczowych usługi analizy danych, może być konieczne ich przekształcenie w aplikacji Power BI Desktop.

Przykładowe dane zawierają kolumny `subject` i `comment`. Korzystając z funkcji Scal kolumny w aplikacji Power BI Desktop, możesz wyodrębnić kluczowe frazy z danych w obu tych kolumnach, a nie tylko w kolumnie `comment`.

W aplikacji Power BI Desktop wybierz wstążkę **Narzędzia główne**. W grupie **Dane zewnętrzne** kliknij pozycję **Edytuj zapytania**.

![[Grupa Dane zewnętrzne na wstążce Narzędzia główne]](../media/tutorials/power-bi/edit-queries.png)

Na liście **Zapytania** po lewej stronie okna wybierz pozycję `FabrikamComments`, jeśli nie została jeszcze wybrana.

Teraz zaznacz obie kolumny (`subject` i `comment`) w tabeli. W celu wyświetlenia tych kolumn może być konieczne przewinięcie w poziomie. Najpierw kliknij nagłówek kolumny `subject`, a następnie przytrzymaj klawisz Control i kliknij nagłówek kolumny `comment`.

![[Zaznaczanie pól do scalenia]](../media/tutorials/power-bi/select-columns.png)

Wybierz wstążkę **Przekształć**. W grupie **Kolumny tekstowe** na wstążce kliknij pozycję **Scal kolumny**. Zostanie wyświetlone okno dialogowe Scal kolumny.

![[Scalanie pól za pomocą okna dialogowego Scal kolumny]](../media/tutorials/power-bi/merge-columns.png)

W oknie dialogowym Scal kolumny wybierz `Tab` jako separator, a następnie kliknij przycisk **OK**.

Możesz również rozważyć odfiltrowanie pustych wiadomości, używając filtru Usuń puste lub usuwając znaki niedrukowalne przy użyciu przekształcenia Wyczyść. Jeśli dane zawierają kolumnę podobną do kolumny `spamscore` w przykładowym pliku, możesz pominąć komentarze „spam”, używając filtru Liczba.

## <a name="understand-the-api"></a>Opis interfejsu API
<a name="UnderstandingAPI"></a>

[Interfejs API fraz kluczowych](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c6) usługi analizy tekstu może przetworzyć nawet tysiąc dokumentów tekstowych w ramach jednego żądania HTTP. Usługa Power BI preferuje obsługę pojedynczych rekordów po kolei, dlatego w tym samouczku każde Twoje wywołanie interfejsu API będzie zawierać tylko jeden dokument. Dla każdego przetwarzanego dokumentu interfejs API fraz kluczowych wymaga poniższych pól.

| | |
| - | - |
| `id`  | Unikatowy identyfikator dla tego dokumentu w żądaniu. Odpowiedź również zawiera to pole. Dzięki temu podczas przetwarzania więcej niż jednego dokumentu można łatwo skojarzyć wyodrębnione frazy kluczowe z dokumentem, z którego pochodzą. Ponieważ w tym samouczku przetwarzasz tylko jeden dokument w każdym żądaniu, możesz trwale zakodować wartość `id`, aby była taka sama dla każdego żądania.|
| `text`  | Tekst do przetworzenia. Wartość tego pola pochodzi z kolumny `Merged` zawierającej wiersz tematu połączony z tekstem komentarza, która została utworzona w [poprzedniej sekcji](#PreparingData). Interfejs API fraz kluczowych wymaga, aby te dane nie były dłuższe niż około 5120 znaków.|
| `language` | Kod języka naturalnego, w którym został napisany dokument. Wszystkie wiadomości w przykładowych danych są w języku angielskim, dlatego możesz trwale zakodować wartość `en` dla tego pola.|

## <a name="create-a-custom-function"></a>Tworzenie funkcji niestandardowej
<a name="CreateCustomFunction"></a>

Teraz możesz przystąpić do tworzenia funkcji niestandardowej, która zintegruje usługę Power BI z funkcją analizy tekstu. Funkcja otrzymuje tekst do przetworzenia w formie parametru. Konwertuje ona dane na i z wymaganego dokumentu w formacie JSON, a następnie wysyła żądanie HTTP do interfejsu API fraz kluczowych. Następnie funkcja analizuje odpowiedź z interfejsu API i zwraca ciąg zawierający rozdzielaną przecinkami listę wyodrębnionych fraz kluczowych.

> [!NOTE]
> Funkcje niestandardowe programu Power BI Desktop pisze się w [języku formuł Power Query M](https://msdn.microsoft.com/library/mt211003.aspx), w skrócie „M”. M to funkcjonalny język programowania oparty na języku [F#](https://docs.microsoft.com/dotnet/fsharp/). Jednak nie trzeba być programistą, aby ukończyć ten samouczek. Wymagany kod znajduje się poniżej.

W aplikacji Power BI Desktop upewnij się, że nadal jesteś w oknie edytora zapytań. W przeciwnym razie wybierz wstążkę **Narzędzia główne**, a następnie w grupie **Dane zewnętrzne** kliknij pozycję **Edytuj zapytania**.

Teraz na wstążce **Narzędzia główne** w grupie **Nowe zapytanie** otwórz menu rozwijane **Nowe źródło** i wybierz pozycję **Puste zapytanie**. 

Na liście Zapytania zostanie wyświetlone nowe zapytanie, początkowo noszące nazwę `Query1`. Kliknij dwukrotnie ten wpis i nadaj mu nazwę `KeyPhrases`.

Teraz na wstążce **Narzędzia główne** w grupie **Zapytanie** kliknij pozycję **Edytor zaawansowany**, aby otworzyć okno Edytora zaawansowanego. Usuń kod, który znajduje się już w tym oknie, a następnie wklej poniższy kod. 

> [!NOTE]
> W poniższych przykładach założono, że punkt końcowy interfejsu API analizy tekstu zaczyna się od `https://westus.api.cognitive.microsoft.com`. Funkcja analizy tekstu umożliwia utworzenie subskrypcji w 13 różnych regionach. Jeśli utworzysz konto usługi w innym regionie, upewnij się, że używasz punktu końcowego dla wybranego regionu. Ten punkt końcowy możesz znaleźć, logując się do witryny [Azure Portal](https://azure.microsoft.com/features/azure-portal/), wybierając swoją subskrypcję usługi analizy tekstu, a następnie wybierając stronę Przegląd.

```fsharp
// Returns key phrases from the text in a comma-separated list
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    keyphrases  = Text.Lower(Text.Combine(jsonresp[documents]{0}[keyPhrases], ", "))
in  keyphrases
```

Zamień klucz `YOUR_API_KEY_HERE` na klucz dostępu usługi analizy tekstu. Ten klucz również możesz znaleźć, logując się do witryny [Azure Portal](https://azure.microsoft.com/features/azure-portal/), wybierając swoją subskrypcję usługi analizy tekstu, a następnie wybierając stronę Przegląd. Przed i za kluczem zostaw cudzysłowy. Następnie kliknij przycisk **Gotowe**.

## <a name="use-the-custom-function"></a>Używanie funkcji niestandardowej
<a name="UseCustomFunction"></a>

Teraz możesz użyć funkcji niestandardowej, aby wyodrębnić frazy kluczowe z poszczególnych komentarzy klientów i zapisać je w nowej kolumnie w tabeli. 

W oknie Edytora zapytań aplikacji Power BI Desktop przejdź z powrotem do zapytania `FabrikamComments`. Wybierz wstążkę **Dodaj kolumnę**. W grupie **Ogólne** kliknij przycisk **Wywołaj funkcję niestandardową**.

![[Przycisk Wywołaj funkcję niestandardową]](../media/tutorials/power-bi/invoke-custom-function-button.png)<br><br>

Zostanie wyświetlone okno dialogowe Wywołaj funkcję niestandardową. W polu **Nazwa nowej kolumny** wprowadź `keyphrases`. W polu **Zapytanie funkcji** wybierz utworzoną funkcję niestandardową `KeyPhrases`.

W oknie dialogowym zostanie wyświetlone nowe pole **tekst (opcjonalnie)**. To pole jest pytaniem, której kolumny chcemy użyć, aby dostarczyć wartości dla parametru `text` interfejsu API fraz kluczowych. (Pamiętaj, że wartości parametrów `language` i `id` zostały już trwale zakodowane). Z menu rozwijanego wybierz pozycję `Merged` (kolumnę, która została utworzona [wcześniej](#PreparingData) przez scalenie pól tematu i treści wiadomości).

![[Wywoływanie funkcji niestandardowej]](../media/tutorials/power-bi/invoke-custom-function.png)

Na koniec kliknij przycisk **OK**.

Jeśli wszystko jest gotowe, usługa Power BI wywołuje funkcję niestandardową jednokrotnie dla każdego wiersza w tabeli. Wysyła ona zapytania do interfejsu API fraz kluczowych i dodaje do tabeli nową kolumnę na potrzeby przechowywania wyników. Jednak zanim tak się stanie, może być konieczne określenie ustawień uwierzytelniania i prywatności.

## <a name="authentication-and-privacy"></a>Uwierzytelnianie i prywatność
<a name="Authentication"></a>

Po zamknięciu okna dialogowego Wywołaj funkcję niestandardową może zostać wyświetlony transparent z prośbą o określenie sposobu łączenia z interfejsem API fraz kluczowych.

![[transparent poświadczeń]](../media/tutorials/power-bi/credentials-banner.png)

Kliknij pozycję **Edytuj poświadczenia**, upewnij się, że w oknie dialogowym jest wybrana opcja `Anonymous`, a następnie kliknij pozycję **Połącz**. 

> [!NOTE]
> Opcja `Anonymous` została wybrana, ponieważ usługa analizy tekstu uwierzytelnia Cię przy użyciu Twojego klucza dostępu, dzięki czemu usługa Power BI nie musi podawać poświadczeń dla samego żądania HTTP.

![[ustawianie uwierzytelniania anonimowego]](../media/tutorials/power-bi/access-web-content.png)

Jeśli nawet po wybraniu dostępu anonimowego jest wyświetlany transparent Edytuj poświadczenia, być może do kodu w [funkcji niestandardowej](#CreateCustomFunction) `KeyPhrases` nie został wklejony klucz dostępu usługi analizy tekstu.

Następnie może zostać wyświetlony transparent z prośbą o podanie informacji na temat ochrony prywatności źródeł danych. 

![[transparent ochrony prywatności]](../media/tutorials/power-bi/privacy-banner.png)

Kliknij przycisk **Kontynuuj** i dla każdego źródła danych w oknie dialogowym wybierz pozycję `Public`. Następnie kliknij przycisk **Zapisz**.

![[ustawianie prywatności źródła danych]](../media/tutorials/power-bi/privacy-dialog.png)

## <a name="create-the-word-cloud"></a>Tworzenie chmury słów
<a name="WordCloud"></a>

Po uporaniu się ze wszystkimi wyświetlanymi transparentami kliknij pozycję **Zamknij i zastosuj** na wstążce Narzędzia główne, aby zamknąć Edytor zapytań.

Wykonanie niezbędnych żądań HTTP przez program Power BI Desktop potrwa chwilę. W przypadku każdego wiersza w tabeli nowa kolumna `keyphrases` zawiera frazy kluczowe wykryte w tekście przez interfejs API fraz kluczowych. 

Teraz za pomocą tej kolumny wygenerujesz chmurę słów. Aby rozpocząć, kliknij przycisk **Raport** w głównym oknie aplikacji Power BI Desktop z lewej strony obszaru roboczego.

> [!NOTE]
> Dlaczego lepiej wygenerować chmurę słów przy użyciu wyodrębnionych fraz kluczowych, a nie pełnego tekstu poszczególnych komentarzy? Frazy kluczowe dostarczają nam *ważnych* słów z komentarzy naszych klientów, a nie po prostu *najpopularniejszych* słów. Ponadto liczba słów w chmurze wynikowej nie jest zniekształcona przez częste użycie słowa w stosunkowo małej liczbie komentarzy.

Jeśli nie masz jeszcze zainstalowanej wizualizacji niestandardowej Word Cloud, zainstaluj ją. W panelu Wizualizacje po prawej stronie obszaru roboczego kliknij przycisk z wielokropkiem (**...**) i wybierz pozycję **Zaimportuj ze sklepu**. Następnie wyszukaj słowo „cloud” i kliknij przycisk **Dodaj** obok wizualizacji Word Cloud. Usługa Power BI zainstaluje wizualizację Word Cloud i poinformuje Cię o jej pomyślnym zainstalowaniu.

![[dodawanie wizualizacji niestandardowej]](../media/tutorials/power-bi/add-custom-visuals.png)<br><br>

Najpierw kliknij ikonę Word Cloud w panelu Wizualizacje.

![[Ikona Word Cloud w panelu wizualizacji]](../media/tutorials/power-bi/visualizations-panel.png)

W obszarze roboczym zostanie wyświetlony nowy raport. Przeciągnij pole `keyphrases` z panelu Pola do pola Kategoria w panelu Wizualizacje. Wewnątrz raportu pojawi się chmura słów.

Teraz przełącz na stronę Format panelu Wizualizacje. W kategorii Słowa ignorowane włącz opcję **Domyślne słowa ignorowane**, aby wyeliminować z chmury krótkie, popularne słowa, takie jak „z”. 

![[aktywowanie domyślnych słów ignorowanych]](../media/tutorials/power-bi/default-stop-words.png)

Przejdź nieco niżej w tym panelu, a następnie wyłącz opcje **Obróć tekst** i **Tytuł**.

![[aktywowanie trybu koncentracji uwagi]](../media/tutorials/power-bi/word-cloud-focus-mode.png)

Kliknij narzędzie trybu koncentracji uwagi w raporcie, aby uzyskać lepszy widok na naszą chmurę słów. Narzędzie rozwija chmurę słów, aby wypełnić cały obszar roboczy, jak pokazano poniżej.

![[Word Cloud]](../media/tutorials/power-bi/word-cloud.png)

## <a name="more-text-analytics-services"></a>Dodatkowe usługi analizy tekstu
<a name="MoreServices"></a>

Usługa analizy tekstu, jedna z usług Cognitive Services oferowanych na platformie Microsoft Azure, umożliwia również analizę opinii i wykrywanie języka. Wykrywanie języka jest szczególnie przydatne, gdy opinia klienta nie jest w języku angielskim.

Oba te interfejsy API są podobne do interfejsu API fraz kluczowych. Oznacza to, że możesz zintegrować je z aplikacją Power BI Desktop, używając funkcji niestandardowych, które są prawie identyczne z utworzonymi w ramach tego samouczka. Wystarczy jak poprzednio utworzyć puste zapytanie i wkleić do Edytora zaawansowanego odpowiedni kod podany poniżej. (Nie zapomnij o kluczu dostępu!) Następnie, tak jak wcześniej, użyj funkcji dodawania nowej kolumny do tabeli.

Poniższa funkcja analizy opinii zwraca wynik informujący o tym, na ile pozytywna jest opinia wyrażona w tekście.

```fsharp
// Returns the sentiment score of the text, from 0.0 (least favorable) to 1.0 (most favorable)
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    sentiment   = jsonresp[documents]{0}[score]
in  sentiment
```

Poniżej przedstawiono dwie wersje funkcji wykrywania języka. Pierwsza zwraca kod języka ISO (na przykład `en` w przypadku języka angielskiego), podczas gdy druga zwraca „przyjazną” nazwę (na przykład `English`). Można zauważyć, że obie wersje różnią się tylko ostatnim wierszem treści.

```fsharp
// Returns the two-letter language code (for example, 'en' for English) of the text
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/languages",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    language    = jsonresp[documents]{0}[detectedLanguages]{0}[iso6391Name]
in  language
```
```fsharp
// Returns the name (for example, 'English') of the language in which the text is written
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/languages",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    language    = jsonresp[documents]{0}[detectedLanguages]{0}[name]
in  language
```

Na koniec przedstawiamy wariant już omówionej funkcji fraz kluczowych, który zwraca frazy w postaci obiektu listy, a nie pojedynczego ciągu fraz rozdzielonych przecinkami. 

> [!NOTE]
> Zwracanie pojedynczego ciągu miało na celu uproszczenie naszego przykładu chmury słów. Jednak lista jest bardziej elastycznym formatem do pracy ze zwracanymi frazami w usłudze Power BI. Obiektami listy w programie Power BI Desktop można manipulować przy użyciu grupy Kolumna strukturalna na wstążce Przekształć Edytora zapytań.

```fsharp
// Returns key phrases from the text as a list object
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    keyphrases  = jsonresp[documents]{0}[keyPhrases]
in  keyphrases
```

## <a name="next-steps"></a>Kolejne kroki
<a name="NextSteps"></a>

Dowiedz się więcej na temat usługi analizy tekstu, języka formuł Power Query M lub usługi Power BI.

> [!div class="nextstepaction"]
> [Dokumentacja interfejsu API analizy tekstu](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c6)

> [!div class="nextstepaction"]
> [Dokumentacja języka Power Query M](https://msdn.microsoft.com/library/mt211003.aspx)

> [!div class="nextstepaction"]
> [Dokumentacja usługi Power BI](https://powerbi.microsoft.com/documentation/powerbi-landing-page/)
