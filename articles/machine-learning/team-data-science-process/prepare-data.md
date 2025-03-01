---
title: Oczyszczaj i przygotowuj dane do usługi Azure Machine Learning — zespołu danych dla celów naukowych
description: Wstępne przetwarzanie i czyszczenie danych, aby przygotować go do efektywnie używać uczenia maszynowego.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 2b3ec3352d6e1939b195bbba87b8a824404346ae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61044604"
---
# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Zadania w celu przygotowania danych do rozszerzonego uczenia maszynowego
Wstępne przetwarzanie i czyszczenie danych są ważne zadania, które zazwyczaj należy przeprowadzić przed zestawu danych można efektywnie używać uczenia maszynowego. Nieprzetworzone dane są często hałas i zawodnych i może brakować wartości. Przy użyciu tych danych do modelowania może spowodować wyświetlenie nieprawdziwych wyników. Te zadania są dostępne w ramach procesu do nauki o danych zespołu (TDSP) i zwykle należy wykonać początkową eksploracji zestawu danych używane do odnajdywania i planowanie wstępnego przetwarzania wymagane. Aby uzyskać szczegółowe instrukcje na temat procesu przetwarzania TDSP, zobacz kroki opisane w temacie [zespołu danych dla celów naukowych](overview.md).

Wstępne przetwarzanie i czyszczenie zadań, takich jak zadanie eksploracji danych, mogą odbywać się w wielu różnych środowiskach, takich jak SQL lub Hive lub usługi Azure Machine Learning Studio, a za pomocą różnych narzędzi i języków, takich jak R lub Python, w którym dane są przechowywane w zależności oraz sposób jego sformatowania. Ponieważ przetwarzanie TDSP iteracyjne pochylone, te zadania mogą być wykonywane w różnych kroków w przepływie pracy procesu.

W tym artykule przedstawiono różne pojęcia dotyczące przetwarzania danych i zadań, które można podjąć przed lub po pozyskaniu danych do usługi Azure Machine Learning.

Aby uzyskać przykład eksploracji danych i wstępnego przetwarzania wykonywane w programie Azure Machine Learning studio, zobacz [wstępne przetworzenie danych w usłudze Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) wideo.

## <a name="why-pre-process-and-clean-data"></a>Dlaczego wstępne przetwarzanie i czyszczenie danych?
Rzeczywiste dane są zbierane z różnych źródeł i procesy i może zawierać nieprawidłowości lub uszkodzone dane pogarszania jakości zestawu danych. Problemy z jakością typowych danych, które powstają są:

* **Niekompletne**: Danych nie ma atrybutów lub zawierające brakujące wartości.
* **Hałas**: Dane zawierają błędne rekordów lub elementy odstające.
* **Niespójne**: Dane zawierają rekordy powodujące konflikt lub niezgodności.

Dane dotyczące jakości jest wymaganiem wstępnym dla modeli predykcyjnych jakości. Aby uniknąć "wyrzucania elementów w pamięci out" i poprawić jakość danych i w związku z tym modelu wydajności, konieczne jest przeprowadzenie ekran kondycji danych wczesne wykrywanie problemów z danych i wybrać odpowiedniego przetwarzania danych i czynności czyszczenia.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Jakie są niektóre ekrany kondycji typowych danych, które są wykorzystywane?
Możemy sprawdzić ogólnej jakości danych, sprawdzając:

* Liczba **rekordów**.
* Liczba **atrybuty** (lub **funkcji**).
* Ten atrybut **typy danych** (nominalna, numer porządkowy lub ciągłe).
* Liczba **brakujących wartości**.
* **Składniowej** danych.
  * Jeśli dane znajdują się w TSV lub CSV, sprawdź, czy kolumna separatory i separatory wierszy zawsze poprawnie oddzielić kolumn i wierszy.
  * Jeśli dane są w formacie HTML lub XML, sprawdź, czy dane jest poprawnie sformułowany oparte na ich odpowiednich standardów.
  * Analiza kodu mogą być również konieczne w celu wyodrębnienia informacji z danych z częściową strukturą lub bez struktury.
* **Niespójne dane rekordów**. Sprawdź zakres wartości są dozwolone. np. Jeśli dane zawierają GPA uczniów, sprawdź, czy GPA znajduje się w zakresie wyznaczonym, powiedz 0 ~ 4.

Jeśli znajdziesz problemy z danymi, **kroki przetwarzania** są niezbędne, które często obejmuje czyszczenia brakujące wartości, normalizacji danych, dyskretyzacji, przetwarzanie tekstu, aby usunąć lub zamienić osadzone znaki, które mogą wpływać na danych wyrównanie, mieszane typy danych we wspólnych oraz inne pola.

**Usługa Azure Machine Learning wykorzystuje dane tabelaryczne z dobrze sformułowany**.  Jeśli dane są już w formie tabelarycznej, przetwarzanie wstępne danych można wykonać bezpośrednio z usługi Azure Machine Learning, Machine Learning Studio.  Jeśli dane nie są w formie tabelarycznej, powiedz, który jest w formacie XML, analizowania może być wymagane w celu konwersji danych w formie tabelarycznej.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Jakie są niektóre z najważniejszych zadań wstępne przetwarzanie danych?
* **Czyszczenie danych**:  Wypełnij lub brakujące wartości, wykrywanie i usuwanie danych hałas i elementy odstające.
* **Przekształcanie danych**:  Normalizacji danych, aby zmniejszyć wymiary i szumu.
* **Redukcja danych**:  Rejestruje dane przykładowe lub atrybutów w celu łatwiejszej obsługi danych.
* **Dane dyskretyzacji**:  Convert ciągłe atrybutów do kategorii atrybutów w celu ułatwienia za pomocą niektórych metod usługi machine learning.
* **Czyszczenie tekstu**: usunięcie osadzonych znaków, które mogą spowodować niespójność danych, dla np. karty osadzone w pliku danych rozdzielane znakami tabulacji osadzone nowe wiersze, które mogą przestać działać rekordów itp.

W poniższych sekcjach opisano niektóre z tych kroków przetwarzania danych.

## <a name="how-to-deal-with-missing-values"></a>Jak radzić sobie z brakującymi wartościami?
Aby poradzić sobie z brakującymi wartościami, najlepiej jest najpierw określić przyczynę brakujące wartości, aby lepiej obsługiwać ten problem. Typowe metody obsługi brakujące wartości to:

* **Usuwanie**: Usuń rekordy z brakującymi wartościami
* **Fikcyjny podstawienia**: Zamień wartości Brak wartości: np. *nieznany* dla kategorii lub równa 0 dla wartości liczbowych.
* **Oznacza podstawienia**: Jeśli brakujące dane liczbowe, Zastąp brakujące wartości średniej.
* **Częste odzyskiwanie pamięci podstawienia**: Jeśli brakujące dane są podzielone na kategorie, Zamień brakujące wartości najczęściej występujących elementów
* **Podstawianie regresji**: Aby zastąpić wartości pogorszonej brakujące wartości, należy użyć metody regresji.  

## <a name="how-to-normalize-data"></a>Jak normalizacji danych?
Normalizacji danych można skalować ponownie wartości liczbowych do określonego zakresu. Metody normalizacji danych popularnych obejmują:

* **Normalizacji minimum maksimum**: Liniowo przekształcać dane do zakresu, załóżmy, że z zakresu od 0 do 1, w którym wartość minimalna jest skalowany 0 i maksymalna wartość na 1.
* **Wynik Z normalizacji**: Skaluj dane na podstawie średniej i odchylenie standardowe: dzielenie różnicy między danymi a średnia, odchylenie standardowe.
* **Skalowanie dziesiętna**: Skalować dane, przenosząc punktu dziesiętnego, wartość atrybutu.  

## <a name="how-to-discretize-data"></a>Jak dyskretyzowania danych?
Dane można zdyskretyzować konwertując ciągłe wartości atrybutów nominalna lub odstępach czasu. W ten sposób niektóre sposoby są następujące:

* **Szerokość równe pakowania**: Podzielić zakresu wszystkich możliwych wartości atrybutu N grup w tej samej wielkości i przypisać wartości, które mieszczą się w pojemniku numerem bin.
* **Wysokość równe pakowania**: Podziel zakres wszystkie możliwe wartości atrybutu do N grup, każdy z nich zawierający tę samą liczbę wystąpień, a następnie przypisz wartości, które mieszczą się w pojemniku numerem bin.  

## <a name="how-to-reduce-data"></a>W jaki sposób zmniejszenia ilości danych?
Istnieją różne metody, aby zmniejszyć rozmiar danych do obsługi danych łatwiejsze. W zależności od rozmiaru danych i domeny można zastosować następujących metod:

* **Zarejestruj próbkowania**: Przykładowe rekordów danych i wybrać tylko reprezentatywnego podzestawu danych.
* **Atrybut próbkowania**: Wybierz tylko podzbiór najważniejsze atrybuty z danych.  
* **Agregacja**: Dzielenie danych na grupy i przechowujące liczby, dla każdej grupy. Na przykład przychód dziennej liczby sieć restauracji, w ciągu ostatnich 20 lat, może być agregowany dochodem miesięczny, aby zmniejszyć rozmiar danych.  

## <a name="how-to-clean-text-data"></a>Jak wyczyścić dane tekstowe?
**Pola tekstowe w danych tabelarycznych** może zawierać znaki, które mają wpływ na granicach kolumn wyrównania i/lub rekordu. Np. osadzone kart w niezgodność kolumny Przyczyna plik wartości rozdzielanych przecinkami kartę i osadzone znaki nowego wiersza przerwanie wiersze rekordów. Obsługa kodowania tekstu niewłaściwy podczas zapisu/odczytu tekstu prowadzi do utraty informacji przypadkowego wprowadzenia nie można go odczytać znaków, np. null i może również mogą wpłynąć na tekst podczas analizowania. Dokładnej analizy i edytowania może być wymagane, aby można było wyczyścić pola tekstowe do prawidłowego wyrównania i/lub dane wyodrębnione ze strukturą danych niestrukturyzowanych i strukturyzowanych tekstu.

**Eksploracja danych** oferuje wczesne wgląd w dane. Liczba problemów z danych może być niepokryty, w tym kroku i odpowiedniej metody mogą być stosowane w celu rozwiązania tych problemów.  Jest ważne zadawać pytania, takie jak co to jest źródłem problemu i jak problemu mogły zostać wprowadzone. Pomaga to również podjęcie decyzji na temat kroków przetwarzania danych, które należy podjąć w celu ich rozwiązania. Rodzaj szczegółowe informacje, które jeden zamierza pochodzić z danych można również określić priorytety nakład pracy przetwarzania danych.

## <a name="references"></a>Dokumentacja
> *Wyszukiwanie danych: Pojęcia i techniki*, wydanie trzecie Morgan Kaufmann, 2011 roku Hanowi Jiawei Micheline Kamber i Jian Pei
> 
> 

