---
title: Typy jednostek
titleSuffix: Language Understanding - Azure Cognitive Services
description: 'Jednostki wyodrębnianie danych z wypowiedź. Typy jednostek zapewniają przewidywalne wyodrębniania danych. Istnieją dwa typy jednostek: przedstawiono maszyny i przedstawiono maszyny. Warto wiedzieć, jakiego typu jednostki, w przypadku korzystania z wypowiedzi.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: diberry
ms.openlocfilehash: 628a96c4e912341226d67a7ed8f241194e7b7825
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080041"
---
# <a name="entity-types-and-their-purposes-in-luis"></a>Typy jednostek i ich celów w usługi LUIS

Jednostki wyodrębnianie danych z wypowiedź. Typy jednostek zapewniają przewidywalne wyodrębniania danych. Istnieją dwa typy jednostek: przedstawiono maszyny i przedstawiono maszyny. Warto wiedzieć, jakiego typu jednostki, w przypadku korzystania z wypowiedzi. 

## <a name="entity-compared-to-intent"></a>Jednostki w porównaniu do intencji

Jednostka reprezentuje wyraz lub frazę w polu wypowiedź, który ma zostać wyodrębniony. Wypowiedź może zawierać wiele jednostek lub brak wcale. Aplikacja kliencka może wymagać jednostki do wykonywania swoich zadań lub służyć jako przewodnik kilka opcji do zaprezentowania użytkownikowi. 

Jednostki:

* Reprezentuje klasę, łącznie z kolekcji podobnych obiektów (miejsc, rzeczy, osoby, zdarzenia lub pojęcia). 
* W tym artykule opisano informacje istotne dla intencji


Na przykład aplikacji wyszukiwania wiadomości mogą obejmować jednostki, takie jak "tematu", "źródło", "słowo kluczowe" i "publikowania daty" będące kluczowych danych do wyszukiwania wiadomości. W aplikacji rezerwacji podróży, "Lokalizacja", "Data", "linii lotniczych" "podróży class" i "bilety" są informacje o kluczu dla rezerwacji lotu (dotyczy na intencje "Zarezerwuj lotów").

Natomiast zamiar reprezentuje prognozowania całego wypowiedź. 

## <a name="entities-help-with-data-extraction-only"></a>Jednostki Pomoc dotycząca tylko wyodrębnianie danych

Etykiety lub znak jednostki na potrzeby it tylko wyodrębniania jednostki nie ułatwić intencji prognozy.

## <a name="entities-represent-data"></a>Jednostki reprezentujące dane

Jednostki są dane, które chcesz ściągnąć z wypowiedź. Może to być nazwy, daty, nazwa produktu lub dowolną grupę słów. 

|Wypowiedź|Jednostka|Dane|
|--|--|--|
|Kup 3 bilety w Nowym Jorku|Liczbę wstępnie<br>Location.Destination|3<br>Nowy Jork|
|Kup biletu z nowego Jorku do Londynu na 5 marca|Location.Origin<br>Location.Destination<br>Wstępnie utworzone datetimeV2|Nowy Jork<br>Londyn<br>5 marca 2018 r.|

## <a name="entities-are-optional-but-highly-recommended"></a>Jednostki są opcjonalne, ale zdecydowanie zalecane

Mimo że intencjami wymagane, jednostki są opcjonalne. Nie trzeba tworzyć jednostki, co pojęcie w swojej aplikacji, ale tylko wymagane dla aplikacji klienckiej i podjąć działania. 

Swoje wypowiedzi nie ma szczegółów, czego potrzebuje bota, aby kontynuować, nie muszą je dodać. Jak dojrzewa aplikacji, możesz je dodać później. 

Jeśli nie masz pewności, jak skorzystać z informacji, dodaj kilka typowych ze wstępnie utworzonych jednostek takich jak [datetimeV2](luis-reference-prebuilt-datetimev2.md), [porządkowe](luis-reference-prebuilt-ordinal.md), [e-mail](luis-reference-prebuilt-email.md), i [numer telefonu ](luis-reference-prebuilt-phonenumber.md).

## <a name="label-for-word-meaning"></a>Etykieta znaczenie słowa

Jeśli wybór programu word lub rozmieszczeniu word jest taka sama, ale nie oznaczają to samo, nie przypisuj jej etykiety bez jednostki. 

Następujące wypowiedzi, wyraz `fair` jest Homogram. Została wpisana takie same, ale ma inne znaczenie:

|Wypowiedź|
|--|
|Jakiego rodzaju targach hrabstwa występują na terenie Seattle tego lata?|
|Jest bieżąca ocena do przeglądu Seattle uczciwe?|

Jeśli chce się jednostkę zdarzeń, aby znaleźć wszystkie dane zdarzeń, etykieta wyraz `fair` w pierwszym wypowiedź, ale nie w ciągu sekundy.

## <a name="entities-are-shared-across-intents"></a>Jednostki są współdzielone przez intencji

Jednostki są współużytkowane przez intencji. Nie należą one do dowolnego pojedynczego celem. Intencje i podmioty można skojarzyć semantycznie, ale nie jest wyłączne relacji.

W polu wypowiedź "Zarezerwuj mnie biletu do Paryża", "Paryż" jest jednostką, odnoszące się do lokalizacji. Dzięki rozpoznawaniu jednostek, które są wymienione w wypowiedź użytkownika, usługi LUIS pomaga wybierz konkretne akcje podejmowane w celu spełnienia żądania użytkownika aplikacji klienckiej.

## <a name="mark-entities-in-none-intent"></a>Oznaczanie jednostek, żadna intencji

Wszystkie opcje, w tym **Brak** zostały oznaczone celem jednostki, gdy jest to możliwe. Dzięki temu usługi LUIS, Dowiedz się więcej o których jednostek wypowiedzi i jakie słowa wokół jednostki. 

## <a name="entity-status-for-predictions"></a>Stan jednostki dla prognoz

Portal usługi LUIS informuje, kiedy jednostki w przykładzie wypowiedź jest różni się od jednostki oznaczone lub jest zbyt Zamknij do innej jednostki i w związku z tym jasne. Jest to wskazywane przez czerwoną linią wypowiedź przykładu. 

Aby uzyskać więcej informacji, zobacz [prognozy stan jednostki](luis-how-to-add-example-utterances.md#entity-status-predictions). 

## <a name="types-of-entities"></a>Typy jednostek

Usługa LUIS oferuje wiele typów jednostek. Wybierz jednostki, na podstawie sposobu mają zostać wyodrębnione dane i jak powinny być reprezentowane po wyodrębnieniu go.

Jednostki można wyodrębnić z uczenia maszynowego, umożliwiający LUIS kontynuować zapoznawanie się jak jednostka jest wyświetlana w polu wypowiedź. Jednostki można wyodrębnić bez uczenia maszynowego, dopasowania tekstu do dokładnego dopasowania lub wyrażenie regularne. Jednostki we wzorcach można wyodrębnić z implementacją mieszane. 

Gdy jednostki są wyodrębniane, dane jednostki można reprezentowane jako pojedyncza jednostka informacji lub w połączeniu z innymi jednostkami w celu utworzenia jednostki informacji używanych przez aplikację klienta.

|Przedstawiono maszyny|Można oznaczyć|Samouczek|Przykład<br>Odpowiedź|Typ jednostki|Przeznaczenie|
|--|--|--|--|--|--|
|✔|✔|[✔](luis-tutorial-composite-entity.md)|[✔](luis-concept-data-extraction.md#composite-entity-data)|[**Composite**](#composite-entity)|Grupowanie jednostki, niezależnie od tego typu jednostki.|
|||[✔](luis-quickstart-intent-and-list-entity.md)|[✔](luis-concept-data-extraction.md#list-entity-data)|[**Lista**](#list-entity)|Lista elementów i ich synonimy wyodrębnione z tekstem dokładnie zgodne.|
|Mieszane||[✔](luis-tutorial-pattern.md)|[✔](luis-concept-data-extraction.md#patternany-entity-data)|[**Pattern.any**](#patternany-entity)|Jednostka, w których koniec jednostki trudno jest określić.|
|||[✔](luis-tutorial-prebuilt-intents-entities.md)|[✔](luis-concept-data-extraction.md#prebuilt-entity-data)|[**Prebuilt**](#prebuilt-entity)|Przeprowadzono już uczenie do wyodrębniania różnych rodzajów danych.|
|||[✔](luis-quickstart-intents-regex-entity.md)|[✔](luis-concept-data-extraction.md#regular-expression-entity-data)|[**Regular Expression**](#regular-expression-entity)|Używa wyrażeń regularnych w celu dopasowania tekstu.|
|✔|✔|[✔](luis-quickstart-primary-and-secondary-data.md)|[✔](luis-concept-data-extraction.md#simple-entity-data)|[**Simple**](#simple-entity)|Zawiera pojedynczy pojęciem wyrazu lub frazy.|

Tylko jednostki maszyny do opanowania konieczne oznaczone w wypowiedzi przykładu. Maszyny do opanowania jednostek działają najlepiej, jeśli testowany za pośrednictwem [kwerendy punktu końcowego](luis-concept-test.md#endpoint-testing) i [przeglądania punktu końcowego wypowiedzi](luis-how-to-review-endoint-utt.md). 

Jednostki pattern.any muszą być oznaczone w [wzorzec](luis-how-to-model-intent-pattern.md) przykłady szablonów, nie przykłady intencji użytkownika. 

Mieszane jednostki używają kombinacji metod wykrywania jednostki.

## <a name="machine-learned-entities-use-context"></a>Maszyny do opanowania jednostek używać kontekstu

Dowiedz się, że maszyna do opanowania jednostek z kontekstu w wypowiedź. Dzięki temu odmiany położenia w przykładzie wypowiedzi znaczące. 

## <a name="non-machine-learned-entities-dont-use-context"></a>Jednostki — przedstawiono maszyny nie należy używać kontekstu

Następująca innych niż maszyna przedstawiono jednostki nie uwzględnia kontekstu wypowiedź podczas dopasowywania jednostki: 

* [Wstępnie utworzonych jednostek](#prebuilt-entity)
* [Wyrażenie regularne jednostek](#regular-expression-entity)
* [Lista jednostek](#list-entity) 

Te jednostki, które nie wymagają etykietowania lub uczenia modelu. Gdy dodasz lub konfigurować jednostkę, są wyodrębniane jednostki. Jego wadą jest to, czy te jednostki mogą być overmatched, gdzie czy kontekst została wzięta pod uwagę, dopasowanie może nie wprowadzono. 

Dzieje się przy użyciu listy jednostek na nowe modele często. Tworzenie i testowanie modelu z jednostką listy, ale po opublikowaniu modelu i odbierać zapytań z punktu końcowego uznasz, że model jest overmatching z powodu braku kontekstu. 

Jeśli chcesz dopasować słów i fraz i uwzględnia kontekstu, masz dwie opcje. Pierwszy jest używać prostych jednostki, z listy fraz. Lista fraz nie będą używane do dopasowania, ale zamiast tego pomoże sygnału stosunkowo podobne słowa (Lista wymienne). Jeśli dokładne dopasowanie, zamiast listy fraz odmiany, użyj obiektami listy z określoną rolą, opisane poniżej.

### <a name="context-with-non-machine-learned-entities"></a>Kontekst z jednostkami przedstawiono maszyny

Chcąc kontekstu wypowiedź mają znaczenie dla jednostek nauczony-machine, należy używać [role](luis-concept-roles.md).

Istnienie przedstawiono maszyny jednostki, takie jak [ze wstępnie utworzonych jednostek](#prebuilt-entity), [wyrażenia regularnego](#regular-expression-entity) jednostek lub [listy](#list-entity) jednostek, które jest zgodny poza wystąpienia, należy wziąć pod uwagę Tworzenie jednej jednostki przy użyciu dwóch ról. Jedną rolę będzie przechwytywać, czego szukasz, a jedna rola zostanie przechwycony, co nie potrzebujesz. Obie wersje będą musieli oznaczone etykietą w przykładzie wypowiedzi.  

## <a name="composite-entity"></a>Złożone jednostki

Jednostka złożonego składa się z innych jednostek, takich jak wstępnie utworzonych jednostek prosty, wyrażeń regularnych i jednostek listy. Osobne jednostki tworzą całej jednostki. 

Ta jednostka jest bardzo dopasowania, gdy dane:

* Są ze sobą powiązane. 
* Są ze sobą powiązane w kontekście wypowiedzi.
* Używać różnych typów jednostek.
* Muszą być grupowane i przetwarzane przez aplikację klienta jako jednostka danych.
* Mają różne wypowiedzi użytkowników, które wymagają usługi machine learning.

![Złożone jednostki](./media/luis-concept-entities/composite-entity.png)

[Samouczek](luis-tutorial-composite-entity.md)<br>
[Przykładowa odpowiedź JSON dla jednostki](luis-concept-data-extraction.md#composite-entity-data)<br>

## <a name="list-entity"></a>Jednostka listy

Lista jednostek reprezentują zbiór powiązanych słów, wraz z ich synonimy stały, zamknięte. Usługa LUIS nie wykrywa dodatkowe wartości dla jednostek z listy. Użyj **zaleca się** funkcji, aby zobaczyć sugestie dotyczące nowych słów na podstawie bieżącej listy. Jeśli istnieje więcej niż jednej jednostki listy z taką samą wartość, każdy obiekt jest zwracany w kwerendy punktu końcowego. 

Jednostka jest bardzo dopasowania, gdy dane tekstowe:

* Są znanego zestawu.
* Nie zmieniają się często. Jeśli potrzebujesz często zmieniać listy lub ma na liście, aby rozwinąć własnym jednostki prostej wzmocnione z listą frazy jest lepszym rozwiązaniem. 
* Zestaw nie przekracza maksymalnych [granic](luis-boundaries.md) usługi LUIS dla tego typu jednostki.
* Tekst w wypowiedzi to dokładne dopasowanie synonimu lub nazwy kanonicznej. Usługa LUIS nie korzysta z listy poza dokładnymi dopasowaniami tekstu. Dopasowywania rozmytego, ignorowanie, analiza słowotwórcza, liczba mnoga i inne różnice nie są rozpoznawane za pomocą jednostki listy. Aby zarządzać wariacjami, rozważ użycie [wzorca](luis-concept-patterns.md#syntax-to-mark-optional-text-in-a-template-utterance) z opcjonalną składnią tekstu.

![Lista jednostek](./media/luis-concept-entities/list-entity.png)

[Samouczek](luis-quickstart-intent-and-list-entity.md)<br>
[Przykładowa odpowiedź JSON dla jednostki](luis-concept-data-extraction.md#list-entity-data)

## <a name="patternany-entity"></a>Jednostka Pattern.any

Pattern.any jest symbolem zastępczym o zmiennej długości, używana tylko w wypowiedź szablonu wzorca w do oznaczania, gdzie jednostka rozpoczyna się i kończy.  

Jednostka jest bardzo dopasowania, gdy:

* Koniec jednostki łatwo pomylić z pozostały tekst wypowiedź. 
[Samouczek](luis-tutorial-pattern.md)<br>
[Przykładowa odpowiedź JSON dla jednostki](luis-concept-data-extraction.md#patternany-entity-data)

**Przykład**  
Biorąc pod uwagę aplikację kliencką, która wyszukuje dla książek na podstawie tytułu, pattern.any wyodrębnia pełną tytuł. Wypowiedź szablonu, za pomocą pattern.any, jest to wyszukiwanie książki `Was {BookTitle} written by an American this year[?]`. 

W poniższej tabeli każdy wiersz zawiera dwie wersje wypowiedź. Najważniejsze wypowiedź jak LUIS początkowo widoczne wypowiedź, których nie jest jasne, z tytułu rozpoczyna się i kończy. Wypowiedź dolnej to, jak usługa LUIS wiedzieli na wzorcu są używane do wyodrębnienia tytułu. 

|Wypowiedź|
|--|
|Ataki typu Man kto Mistook His żoną Hat i innych kontrolne Clinical napisał American tego roku?<br><br>Został **Man kto Mistook His żoną Hat i innych kontrolne Clinical** napisane przez amerykański tego roku?|
|Była połowa uśpione w Pajamas żab napisane przez amerykański tego roku?<br><br>Został **połowa uśpione w Pajamas żab** napisane przez amerykański tego roku?|
|Został określonego smutek z Tort Tarta: Nowe, napisane przez amerykański tego roku?<br><br>Został **określonego smutek z Tort Tarta: Powieść** napisane przez amerykański tego roku?|
|To, że Wocket w mojej Pocket! napisane przez amerykański tego roku?<br><br>Został **w mojej Pocket jest Wocket!** napisane przez amerykański tego roku?|
||

## <a name="prebuilt-entity"></a>Wstępnie utworzone jednostki

Wstępnie utworzone jednostki są wbudowanych typów, które reprezentują typowe pojęcia, takie jak wiadomości e-mail, adres URL i numer telefonu. Wstępnie utworzone jednostki nazwy są zarezerwowane. [Wszystkie wstępnie utworzonych jednostek](luis-prebuilt-entities.md) dodawane do aplikacji są zwracane w zapytaniu prediction punktu końcowego, jeśli występują w wypowiedź. 

Jednostka jest bardzo dopasowania, gdy:

* Dane są zgodne typowy przypadek użycia, obsługiwane przez wstępnie utworzone jednostki dla Twojej kulturze języka. 

Wstępnie utworzone jednostki mogą być dodawane lub usuwane w dowolnym momencie.

![Liczba wstępnie utworzone jednostki](./media/luis-concept-entities/number-entity.png)

[Samouczek](luis-tutorial-prebuilt-intents-entities.md)<br>
[Przykładowa odpowiedź JSON dla jednostki](luis-concept-data-extraction.md#prebuilt-entity-data)

Niektóre z tych wstępnie utworzonych jednostek są zdefiniowane w typu open-source [aparatów rozpoznawania tekstu](https://github.com/Microsoft/Recognizers-Text) projektu. Jeśli z określoną kulturę lub jednostki nie jest obecnie obsługiwane, przyczyniają się do projektu. 

### <a name="troubleshooting-prebuilt-entities"></a>Rozwiązywanie problemów ze wstępnie utworzonych jednostek

W portalu usługi LUIS Jeśli wstępnie utworzone jednostki zostanie oznaczony zamiast swojej jednostki niestandardowej, masz kilka opcji sposobu rozwiązać ten problem.

Wstępnie utworzonych jednostek dodana do aplikacji będzie _zawsze_ zostać zwrócone, nawet wtedy, gdy wypowiedź należy wyodrębnić jednostki niestandardowe, ten sam tekst. 

#### <a name="change-tagged-entity-in-example-utterance"></a>Zmień jednostki oznakowanych w przykładzie wypowiedź

Jeśli wstępnie utworzone jednostki jest ten sam tekst lub tokenów jako jednostkę niestandardową, zaznacz tekst w polu wypowiedź przykład i zmienić wypowiedź oznakowane. 

Jeśli wstępnie utworzone jednostki jest oznaczane więcej tekstu lub tokeny od swojej jednostki niestandardowej, masz kilka opcji, jak rozwiązać ten problem:

* [Usuń wypowiedź przykład](#remove-example-utterance-to-fix-tagging) — metoda
* [Usuwanie wstępnie utworzone jednostki](#remove-prebuilt-entity-to-fix-tagging) — metoda

#### <a name="remove-example-utterance-to-fix-tagging"></a>Usuń wypowiedź przykładu, aby naprawić znakowanie 

Twój pierwszy wybór jest usunięcie wypowiedź przykładu. 

1. Usuń wypowiedź przykładu.
1. Ponowne szkolenie aplikacji. 
1. Dodaj ponownie tylko słowo lub frazę jednostki, która jest oznaczona jako wstępnie utworzone jednostki, jako wypowiedź kompletny przykład. Wyraz lub frazę, będą nadal mieć wstępnie utworzone jednostki oznaczone. 
1. Wybierz jednostkę w wypowiedź przykład **intencji** strony i zmianę do swojej jednostki niestandardowej i uczenie się ponownie. Powinno to zapobiec oznaczenie tego tekstu do dokładnego dopasowania jako wstępnie utworzone jednostki w wypowiedzi przykład korzystających z tego tekstu usługi LUIS. 
1. Cały, oryginalnym wypowiedź przykład ponownie dodać do intencji. Jednostki niestandardowej, powinna w dalszym ciągu można oznaczyć zamiast wstępnie utworzone jednostki. Jeśli nie jest oznaczona jednostki niestandardowej, należy dodać więcej przykładów ten tekst w wypowiedzi.

#### <a name="remove-prebuilt-entity-to-fix-tagging"></a>Usuwanie wstępnie utworzone jednostki, aby naprawić znakowanie

1. Wstępnie utworzone jednostki można usunąć z aplikacji. 
1. Na **intencji** strony, oznacz jednostki niestandardowej w wypowiedź przykładu.
1. Przeszkol aplikację.
1. Dodawanie wstępnie utworzone jednostki do aplikacji i uczenie aplikacji. Ta poprawka przyjęto założenie, że wstępnie utworzone jednostki nie należały do obiektu złożonego.

## <a name="regular-expression-entity"></a>Jednostka wyrażenia regularnego 

Wyrażenie regularne jest najlepsze dla tekstowe wypowiedź raw. On ignoruje wielkość liter i ignoruje wariant kultury.  Dopasowywanie wyrażeń regularnych są stosowane po sprawdzania pisowni zmiany na poziomie znak, a nie na poziomie tokenu. Jeśli wyrażenie regularne jest zbyt złożone, np. przy użyciu wielu nawiasie, nie możesz dodać wyrażenie do modelu. Używa części, ale nie wszystkie [wyrażenie regularne .NET](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions) biblioteki. 

Jednostka jest bardzo dopasowania, gdy:

* Dane są spójnie sformatowanych przy użyciu dowolnych wariantów, który również jest zgodny.
* Wyrażenie regularne nie potrzebuje więcej niż 2 poziomów zagnieżdżenia. 

![Jednostka wyrażenia regularnego](./media/luis-concept-entities/regex-entity.png)

[Samouczek](luis-quickstart-intents-regex-entity.md)<br>
[Przykładowa odpowiedź JSON dla jednostki](luis-concept-data-extraction.md#regular-expression-entity-data)<br>

Wyrażenia regularne może odpowiadać więcej niż oczekujesz, że do dopasowania. Na przykład jest liczbowe word dopasowywania, takich jak `one` i `two`. Są to na przykład poniższe wyrażenie regularne, która jest zgodna z liczbą `one` oraz inne liczby:

```javascript
(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*
``` 

To wyrażenie wyrażenia regularnego dopasowuje również wszystkie wyrazy, które kończą się te liczby, takich jak `phone`. Aby rozwiązać problemy, takie jak ta, upewnij się, że pasuje do wyrażenia regularnego bierze pod uwagę granicy słowa. Wyrażenie regularne, aby użyć granicy słowa w tym przykładzie jest używany w następujących wyrażeń regularnych:

```javascript
\b(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*\b
```

## <a name="simple-entity"></a>Prosta jednostka 

Proste jednostka jest jednostce ogólnej, opisujący pojedynczego pojęcia i udostępnionej z kontekstu maszyny do opanowania. Ponieważ proste jednostki są zwykle nazwy, takie jak nazwy firm, nazwy produktów lub innych kategoriach nazwy, Dodaj [listy fraz](luis-concept-feature.md) podczas korzystania z jednostki prostej do poprawienia sygnał nazwy używane. 

Jednostka jest bardzo dopasowania, gdy:

* Dane nie są spójnie sformatowanych, ale wskazują ten sam efekt. 

![proste jednostki](./media/luis-concept-entities/simple-entity.png)

[Samouczek](luis-quickstart-primary-and-secondary-data.md)<br/>
[Przykładowa odpowiedź dla jednostki](luis-concept-data-extraction.md#simple-entity-data)<br/>

## <a name="entity-limits"></a>Limity jednostek

Przegląd [limity](luis-boundaries.md#model-boundaries) Aby dowiedzieć się, ile poszczególnych typów obiektu można dodać do modelu.

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Jeśli potrzebujesz więcej niż maksymalna liczba jednostek 

Może być konieczne korzystanie z jednostek złożonego w połączeniu z rolami jednostki.

Composite jednostek reprezentują części całości. Na przykład jednostka złożone o nazwie PlaneTicketOrder niewykluczone jednostki podrzędne linii lotniczych, miejsce docelowe, DepartureCity, DepartureDate i PlaneTicketClass.

Usługa LUIS także listy Typ jednostki obsługiwanej przez nie przedstawiono maszyny, ale pozwala aplikacją usługi LUIS określić stałą listy wartości. Zobacz [granice LUIS](luis-boundaries.md) odwołania, aby zapoznać się ograniczenia typu listy jednostek. 

Jeśli zostały uznane za te jednostki i nadal potrzebujesz więcej niż limit, skontaktuj się z działem pomocy technicznej. Aby to zrobić, należy zebrać szczegółowe informacje o systemie, przejdź do [LUIS](luis-reference-regions.md#luis-website) witryny sieci Web, a następnie wybierz **pomocy technicznej**. Jeśli Twoja subskrypcja platformy Azure obejmują usługi pomocy technicznej, skontaktuj się z [technicznej platformy Azure](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Kolejne kroki

Pojęcia dotyczące dobrze [wypowiedzi](luis-concept-utterance.md). 

Zobacz [Dodaj jednostki](luis-how-to-add-entities.md) Aby dowiedzieć się więcej o sposobie dodawania jednostki z aplikacją usługi LUIS.
