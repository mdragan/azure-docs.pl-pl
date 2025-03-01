---
title: 'Samouczek: tworzenie strony internetowej z funkcją wyszukiwania niestandardowego — wyszukiwanie niestandardowe Bing'
titlesuffix: Azure Cognitive Services
description: W tym artykule opisano, jak skonfigurować wystąpienie wyszukiwania niestandardowego i zintegrować je ze stroną internetową.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 0870983fe9f0ae64090652f02c526a370064b8ab
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65595894"
---
# <a name="tutorial-build-a-custom-search-web-page"></a>Samouczek: Tworzenie strony internetowej z funkcją wyszukiwania niestandardowego

Wyszukiwanie niestandardowe Bing umożliwia tworzenie dostosowanych funkcji wyszukiwania dla interesujących Cię tematów. Jeśli na przykład masz witrynę internetową poświęconą sztukom walki, w której znajduje się funkcja wyszukiwania, możesz określić domeny, witryny podrzędne i strony internetowe, które będą przeszukiwane przez usługę Bing. Użytkownicy zobaczą wyniki dostosowane do zawartości, która ich interesuje, zamiast przewijać strony ogólnych wyników wyszukiwania, które mogą zawierać nieistotną zawartość. 

W tym samouczku opisano, jak skonfigurować wystąpienie wyszukiwania niestandardowego i zintegrować je ze stroną internetową.

Wykonane zadania:

> [!div class="checklist"]
> - Tworzenie wystąpienia wyszukiwania niestandardowego
> - Dodawanie aktywnych wpisów
> - Dodawanie wpisów zablokowanych
> - Dodawanie wpisów przypiętych
> - Integrowanie wyszukiwania niestandardowego ze stroną internetową

## <a name="prerequisites"></a>Wymagania wstępne

- Aby skorzystać z samouczka, potrzebny jest klucz subskrypcji interfejsu API wyszukiwania niestandardowego Bing.  Aby uzyskać klucz, zobacz [Wypróbuj usługi Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
- Jeśli nie masz jeszcze programu Visual Studio 2017 lub nowszy jest zainstalowany, możesz pobrać i używać **bezpłatne** [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/).

## <a name="create-a-custom-search-instance"></a>Tworzenie wystąpienia wyszukiwania niestandardowego

Aby utworzyć wystąpienie wyszukiwania niestandardowego Bing:

1. Otwórz przeglądarkę internetową.  
  
2. Przejdź do [portalu](https://customsearch.ai) wyszukiwania niestandardowego.  
  
3. Zaloguj się do portalu przy użyciu konta Microsoft (MSA). Jeśli nie masz konta MSA, kliknij pozycję **Create a Microsoft account** (Utwórz konto Microsoft). Jeśli korzystasz z portalu po raz pierwszy, zostanie wyświetlony monit o udzielenie uprawnień dostępu do danych. Kliknij przycisk **Yes** (Tak).  
  
4. Po zalogowaniu się kliknij pozycję **New custom search** (Nowe wyszukiwanie niestandardowe). W oknie **Create a new custom search instance** (Tworzenie nowego wystąpienia wyszukiwania niestandardowego) wprowadź istotną nazwę opisową typu zawartości zwracanej przez funkcję wyszukiwania. Nazwę można zmienić w dowolnym momencie.  
  
   ![Zrzut ekranu pola Create a new custom search instance (Tworzenie nowego wystąpienia wyszukiwania niestandardowego)](../media/newCustomSrch.png)  
  
5. Kliknij przycisk OK, określ adres URL oraz czy mają zostać dołączone podstrony adresu URL.  
  
   ![Zrzut ekranu strony definicji adresu URL](../media/newCustomSrch1-a.png)  


## <a name="add-active-entries"></a>Dodawanie aktywnych wpisów

Aby uwzględnić wyniki z określonych witryn internetowych lub adresów URL, dodaj je do karty **Active** (Aktywne).

1.  Na stronie **Configuration** (Konfiguracja) kliknij kartę **Active** (Aktywne) i wprowadź adres URL jednej lub większej liczby witryn internetowych, które mają zostać uwzględnione w wyszukiwaniu.

    ![Zrzut ekranu przedstawiający kartę wpisów aktywnych w edytorze definicji](../media/customSrchEditor.png)

2.  Aby upewnić się, że wystąpienie zwraca wyniki, wprowadź zapytanie w okienku podglądu po prawej stronie. Wyszukiwarka Bing zwraca tylko wyniki z witryn publicznych, które zostały przez nią zindeksowane.

## <a name="add-blocked-entries"></a>Dodawanie wpisów zablokowanych

Aby wykluczyć wyniki z określonych witryn internetowych lub adresów URL, dodaj je do karty **Blocked** (Zablokowane).

1. Na stronie **Configuration** (Konfiguracja) kliknij kartę **Blocked** (Zablokowane) i wprowadź adres URL jednej lub większej liczby witryn internetowych, które mają zostać wykluczone z wyszukiwania.

    ![Zrzut ekranu przedstawiający kartę wpisów zablokowanych w edytorze definicji](../media/blockedCustomSrch.png)


2. Aby upewnić się, że wystąpienie nie zwraca wyników z zablokowanych witryn, wprowadź zapytanie w okienku podglądu po prawej stronie. 

## <a name="add-pinned-entries"></a>Dodawanie wpisów przypiętych

Aby przypiąć określoną stronę internetową na początku wyników wyszukiwania, dodaj stronę internetową i termin zapytania do karty **Pinned** (Przypięte). Karta **Pinned** (Przypięte) zawiera listę par stron internetowych i terminów zapytań określających stronę internetową, która jest wyświetlana jako najlepszy wynik dla określonego zapytania. Ta strona internetowa zostaje przypięta tylko wtedy, gdy ciąg zapytania użytkownika jest zgodny z ciągiem zapytania przypiętego elementu na podstawie jego warunku dopasowania. W wyszukiwaniach będą wyświetlane tylko indeksowane strony internetowe. Aby uzyskać więcej informacji, zobacz [Define your custom view (Definiowanie widoku niestandardowego)](../define-your-custom-view.md#pin-slices-to-the-top-of-search-results).

1. Na stronie **Configuration** (Konfiguracja) kliknij kartę **Pinned** (Przypięte) i wprowadź stronę internetową oraz termin zapytania strony internetowej, które mają być zwracane jako najlepszy wynik.  
  
2. Domyślnie ciąg zapytania użytkownika musi dokładnie odpowiadać ciągowi zapytania przypiętego elementu, aby usługa Bing zwróciła stronę internetową jako najlepszy wynik. Aby zmienić warunek dopasowania, edytuj przypięty element (kliknij ikonę ołówka), kliknij pozycję Exact (Dokładne) w kolumnie **Query match condition** (Warunek dopasowania zapytania) i wybierz warunek dopasowania odpowiedni dla aplikacji.  
  
    ![Zrzut ekranu przedstawiający kartę wpisów przypiętych w edytorze definicji](../media/pinnedCustomSrch.png)
  
3. Ab upewnić się, że wystąpienie zwraca określoną stronę internetową jako najlepszy wynik, wprowadź przypięty termin zapytania w okienku podglądu po prawej stronie.

## <a name="configure-hosted-ui"></a>Konfigurowanie hostowanego interfejsu użytkownika

Wyszukiwanie niestandardowe zapewnia hostowany interfejs użytkownika do renderowania odpowiedzi JSON wystąpienia wyszukiwania niestandardowego. Aby zdefiniować środowisko interfejsu użytkownika:

1. Kliknij kartę **Hosted UI** (Hostowany interfejs użytkownika).  
  
2. Wybierz układ.  
  
   ![Zrzut ekranu przedstawiający krok wyboru układu na karcie Hosted UI (Hostowany interfejs użytkownika)](./media/custom-search-hosted-ui-select-layout.png)  
  
3. Wybierz motyw kolorów.  
  
   ![Zrzut ekranu przedstawiający wybieranie motywu kolorów na karcie Hosted UI (Hostowany interfejs użytkownika)](./media/custom-search-hosted-ui-select-color-theme.png)  

   Aby dostosować motyw kolorów w celu zapewnienia lepszej integracji z aplikacją internetową, kliknij pozycję **Customize theme** (Dostosuj motyw). Nie wszystkie konfiguracje kolorów mają zastosowanie do wszystkich motywów układu. Aby zmienić kolor, wprowadź wartość SZESNASTKOWĄ RGB (na przykład #366eb8) w odpowiednim polu tekstowym. Możesz również kliknąć przycisk koloru i wybrać odpowiedni odcień. Podczas wybierania kolorów należy zawsze brać pod uwagę ułatwienia dostępu.
  
   ![Zrzut ekranu przedstawiający dostosowywanie motywu kolorów hostowanego interfejsu użytkownika](./media/custom-search-hosted-ui-customize-color-theme.png)  

  
4. Określ dodatkowe opcje konfiguracji.  
  
   ![Zrzut ekranu przedstawiający krok wybierania dodatkowych opcji konfiguracji hostowanego interfejsu użytkownika](./media/custom-search-hosted-ui-additional-configurations.png)  
  
   Aby uzyskać dostęp do zaawansowanych opcji konfiguracji, kliknij pozycję **Show advanced configurations** (Pokaż zaawansowane opcje konfiguracji). Spowoduje to dodanie opcji konfiguracji, takich jak *Link target* (Element docelowy linku), do opcji wyszukiwania w Internecie, *Enable filters* (Włącz filtry) do opcji obrazów i wideo oraz *Search box text placeholder* (Symbol zastępczy tekstu pola wyszukiwania) do obszaru opcji Miscellaneous (Różne).

   ![Zrzut ekranu przedstawiający krok wybierania zaawansowanych opcji konfiguracji hostowanego interfejsu użytkownika](./media/custom-search-hosted-ui-advanced-configurations.png)  
  
5. Wybierz klucze subskrypcji z list rozwijanych. Alternatywnie możesz ręcznie wprowadzić klucz subskrypcji. Więcej informacji na temat uzyskiwania kluczy można znaleźć na stronie [Możliwość wypróbowania usług Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search-api).  
  
   ![Zrzut ekranu przedstawiający krok wybierania dodatkowych opcji konfiguracji hostowanego interfejsu użytkownika](./media/custom-search-hosted-ui-subscription-key.png)

[!INCLUDE [publish or revert](../includes/publish-revert.md)]

<a name="consuminghostedui"></a>
## <a name="consuming-hosted-ui"></a>Korzystanie z hostowanego interfejsu użytkownika

Istnieją dwa sposoby korzystania z hostowanego interfejsu użytkownika.  

- Opcja 1: zintegrowanie dostarczonego fragmentu kodu JavaScript z aplikacją.
- Opcja 2: użycie dostarczonego punktu końcowego HTML.

W dalszej części tego samouczka przedstawiono **opcję 1 — fragment kodu JavaScript**.  

## <a name="set-up-your-visual-studio-solution"></a>Konfigurowanie rozwiązania programu Visual Studio

1. Otwórz program **Visual Studio** na komputerze.  
  
2. W menu **Plik** wybierz polecenie **Nowy**, a następnie wybierz pozycję **Projekt**.  
  
3. W oknie **Nowy projekt** wybierz pozycję **Aplikacja internetowa / Aplikacja internetowa platformy Visual C# / ASP.NET Core**, nazwij swój projekt, a następnie kliknij przycisk **OK**.  
  
   ![Zrzut ekranu przedstawiający okno nowego projektu](./media/custom-search-new-project.png)  
  
4. W oknie **Nowa aplikacja internetowa platformy ASP.NET Core** wybierz pozycję **Aplikacja internetowa** i kliknij przycisk **OK**.  
  
   ![Zrzut ekranu przedstawiający okno nowego projektu](./media/custom-search-new-webapp.png)  

## <a name="edit-indexcshtml"></a>Edytowanie pliku index.cshtml

1. W **Eksploratorze rozwiązań** rozwiń węzeł **Strony** i kliknij dwukrotnie plik **index.cshtml**, aby go otworzyć.  
  
   ![Zrzut ekranu przedstawiający Eksploratora rozwiązań z rozwiniętym węzłem Strony i wybranym plikiem index.cshtml](./media/custom-search-visual-studio-webapp-solution-explorer-index.png)  
  
2. W pliku index.cshtml usuń całą zawartość od wiersza 7 w dół.  
  
   ```razor
   @page
   @model IndexModel
   @{
      ViewData["Title"] = "Home page";
   }    
   ```  
  
3. Dodaj element podziału wiersza i tag div pełniący rolę kontenera.  
  
   ```html
   @page
   @model IndexModel
   @{
      ViewData["Title"] = "Home page";
   }
   <br />
   <div id="customSearch"></div>
   ```  
  
4. Na stronie **Hosted UI** (Hostowany interfejs użytkownika) przewiń w dół do sekcji o nazwie **Consuming the UI** (Korzystanie z interfejsu użytkownika). Kliknij pozycję *Endpoints* (Punkty końcowe), aby uzyskać dostęp do fragmentu kodu JavaScript. Fragment kodu możesz wyświetlić również, klikając pozycję **Production** (Produkcja), a następnie wybierając kartę **Hosted UI** (Hostowany interfejs użytkownika).
  
   <!-- Get new screenshot after prod gets new bits
   ![Screenshot of the Hosted UI save button](./media/custom-search-hosted-ui-consuming-ui.png)  
   -->
  
5. Wklej element script w kontenerze, którzy został dodany.  
  
   ``` html
   @page
   @model IndexModel
   @{
      ViewData["Title"] = "Home page";
   }
   <br />
   <div id="customSearch">
      <script type="text/javascript" 
          id="bcs_js_snippet"
          src="https://ui.customsearch.ai /api/ux/rendering-js?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate&version=latest&q=">
      </script>
   </div>
   ```  
  
6. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy pozycję **wwwroot** i kliknij polecenie **Wyświetl w przeglądarce**.  
  
   ![Zrzut ekranu przedstawiający wybieranie polecenia Wyświetl w przeglądarce w menu kontekstowym elementu wwwroot w Eksploratorze rozwiązań](./media/custom-search-webapp-view-in-browser.png)  

Twoja nowa strona wyszukiwania niestandardowego powinna wyglądać mniej więcej tak:

![Zrzut ekranu przedstawiający stronę internetową wyszukiwania niestandardowego](./media/custom-search-webapp-browse-index.png)

Wykonanie wyszukiwania powoduje renderowanie wyników na następujący sposób:

![Zrzut ekranu przedstawiający wyniki wyszukiwania niestandardowego](./media/custom-search-webapp-results.png)

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Wywoływanie punktu końcowego wyszukiwania niestandardowego Bing (C#)](../call-endpoint-csharp.md)
