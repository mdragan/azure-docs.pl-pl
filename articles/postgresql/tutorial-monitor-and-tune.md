---
title: Samouczek dotyczący monitorowania i dostrajania w usłudze Azure Database for PostgreSQL — pojedynczy serwer
description: W tym samouczku przedstawiono monitorowanie i dostrajanie w usłudze Azure Database for PostgreSQL — pojedynczy serwer.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: tutorial
ms.date: 5/6/2019
ms.openlocfilehash: dcd8363db33809e2a33885f43e570c53807505fd
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/27/2019
ms.locfileid: "65073083"
---
# <a name="tutorial-monitor-and-tune-azure-database-for-postgresql---single-server"></a>Samouczek: Monitorowania i dostrajania bazy danych Azure Database for PostgreSQL — pojedynczy serwer

Usługa Azure Database for PostgreSQL zawiera funkcje, które pomagają zrozumieć i poprawić wydajność serwera. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Włączanie zbierania statystyk zapytań i oczekiwania
> * Uzyskiwanie dostępu do zebranych danych i korzystanie z nich
> * Wyświetlanie wydajności zapytań i statystyk oczekiwania w czasie
> * Analizowanie bazy danych w celu uzyskania zaleceń dotyczących wydajności
> * Stosowanie zaleceń dotyczących wydajności

## <a name="before-you-begin"></a>Przed rozpoczęciem
Potrzebujesz serwera usługi Azure Database for PostgreSQL z programem PostgreSQL w wersji 9.6 lub 10. Możesz wykonać kroki opisane w [samouczku dotyczącym tworzenia](tutorial-design-database-using-azure-portal.md), aby utworzyć serwer.

> [!IMPORTANT]
> Funkcje **Magazyn zapytań**, **Szczegółowe informacje o wydajności zapytań** i **Zalecenia dotyczące wydajności** są dostępne w publicznej wersji zapoznawczej.

## <a name="enabling-data-collection"></a>Włączanie zbierania danych
[Magazyn zapytań](concepts-query-store.md) przechwytuje historię zapytań i statystyki oczekiwania na serwerze i przechowuje je w bazie danych **azure_sys** na serwerze. Jest to funkcja opcjonalna. Aby ją włączyć:

1. Otwórz witrynę Azure Portal.

2. Wybierz serwer usługi Azure Database for PostgreSQL.

3. Wybierz pozycję **Parametry serwera**, znajdującą się w sekcji **Ustawienia** menu po lewej stronie.

4. Ustaw parametr **pg_qs.query_capture_mode** na wartość **TOP**, aby rozpocząć zbieranie danych o wydajności zapytań. Ustaw parametr **pgms_wait_sampling.query_capture_mode** na wartość **ALL**, aby rozpocząć zbieranie statystyk oczekiwania. Zapisz zmiany.
   
   ![Parametry serwera magazynu zapytań](./media/tutorial-performance-intelligence/query-store-parameters.png)

5. Poczekaj do 20 minut na utrwalenie pierwszej partii danych w bazie danych **azure_sys**.


## <a name="performance-insights"></a>Szczegółowe informacje o wydajności
W widoku [Szczegółowe informacje o wydajności zapytań](concepts-query-performance-insight.md) w witrynie Azure Portal będą ujawniane wizualizacje kluczowych informacji z magazynu zapytań. 

1. Na stronie portalu serwera usługi Azure Database for PostgreSQL wybierz pozycję **Szczegółowe informacje o wydajności zapytań** w sekcji **Pomoc techniczna i rozwiązywanie problemów** menu po lewej stronie.

2. Na karcie **Długotrwałe zapytania** jest wyświetlane pięć pierwszych zapytań według średniego czasu trwania na wykonanie, agregowanych w piętnastominutowych interwałach. 
   
   ![Szczegółowe informacje o wydajności zapytań, strona docelowa](./media/tutorial-performance-intelligence/query-performance-insight-landing-page.png)

   Możesz wyświetlić więcej zapytań, wybierając wartość z listy rozwijanej **Liczba zapytań**. Gdy to zrobisz, kolory wykresu dla określonego identyfikatora zapytania mogą ulec zmianie.

3. Klikając i przeciągając na wykresie, możesz zawęzić go do konkretnego przedziału czasu.

4. Użyj ikon powiększenia i pomniejszenia, aby wyświetlić odpowiednio mniejszy lub większy okres.

5. Wyświetl tabelę poniżej wykresu, aby uzyskać więcej szczegółów na temat długotrwałych zapytań w tym przedziale czasu.

6. Wybierz kartę **Statystyki oczekiwania**, aby wyświetlić odpowiednie wizualizacje oczekiwań na serwerze.
   
   ![Szczegółowe informacje o wydajności zapytań — statystyki oczekiwania](./media/tutorial-performance-intelligence/query-performance-insight-wait-statistics.png)

### <a name="permissions"></a>Uprawnienia
Uprawnienia **Właściciel** lub **Współautor** są wymagane do wyświetlania tekstu zapytań w widoku Szczegółowe informacje o wydajności zapytań. **Czytelnik** może wyświetlać wykresy i tabele, ale nie tekst zapytań.


## <a name="performance-recommendations"></a>Zalecenie dotyczące wydajności
Funkcja [Zalecenia dotyczące wydajności](concepts-performance-recommendations.md) analizuje obciążenia na serwerze, aby zidentyfikować indeksy z potencjałem poprawy wydajności.

1. Otwórz **Zalecenia dotyczące wydajności** w sekcji **Pomoc techniczna i rozwiązywanie problemów** paska menu na stronie serwera PostgreSQL w witrynie Azure Portal.
   
   ![Zalecenia dotyczące wydajności, strona docelowa](./media/tutorial-performance-intelligence/performance-recommendations-landing-page.png)

2. Wybierz opcję **Analiza** i wybierz bazę danych. Spowoduje to rozpoczęcie analizy.

3. W zależności od obciążenia jej ukończenie może potrwać kilka minut. Po zakończeniu analizy w portalu zostanie wyświetlone powiadomienie.

4. W oknie **Zalecenia dotyczące wydajności** zostanie wyświetlona lista zaleceń, jeśli jakieś znaleziono. 

5. Zalecenia będą zawierały informacje o odpowiedniej **bazie danych**, **tabeli**, **kolumnie** i **rozmiarze indeksu**.

   ![Zalecenia dotyczące wydajności, wyniki](./media/tutorial-performance-intelligence/performance-recommendations-result.png)

6. Aby wdrożyć zalecenie, skopiuj tekst zapytania i uruchom je z poziomu wybranego klienta.

### <a name="permissions"></a>Uprawnienia
Uprawnienia **Właściciel** lub **Współautor** są wymagane do uruchamiania analiz przy użyciu funkcji Zalecenia dotyczące wydajności.

## <a name="next-steps"></a>Kolejne kroki
- Dowiedz się więcej o [monitorowaniu i dostrajaniu](concepts-monitoring.md) w usłudze Azure Database for PostgreSQL.