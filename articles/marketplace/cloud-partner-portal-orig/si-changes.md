---
title: Sprzedawcy szczegółowe informacje o wersji
description: Zawiera informacje na temat zmian sprzedawcy wgląd w szczegółowe dane.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 3/3/2019
ms.author: pabutler
ms.openlocfilehash: c6e9e4fe672c7e171ed4b1cd60655f9e71a562e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64943122"
---
# <a name="seller-insights-release-notes"></a>Sprzedawcy szczegółowe informacje o wersji 

(Data wydania: 1 marca 2019 r.)

Ten artykuł zawiera informacje na temat zmian z tą funkcją sprzedawcy wgląd w [portalu Cloud Partner](https://cloudpartner.azure.com/#insights).

## <a name="release-highlights-for-march-1-2019"></a>Najważniejsze informacje o wersji dla 1 marca 2019 r.

* *Trend klienta* dodawane do podsumowania
* *Pierwszych pięć klientów* na podsumowanie odzwierciedlać wszystkich subskrypcji platformy Azure przez klienta
* *Znormalizowane trendów użycia i aktywne Trend zamówienia* na podsumowanie została przeniesiona do obszaru *miesięczne zamówienia na pierwszy rzut oka*
* *Informacje dotyczące wypłat raport uzgadniania* zaktualizowane
* *Pierwszych pięć klientów* na informacje dotyczące wypłat odzwierciedlać wszystkich subskrypcji platformy Azure przez klienta
* *Raport użycia* aktualizowane przy użyciu Identyfikatora klienta
* *Klientów na stanowisku* na zamówień i użycie odzwierciedla wszystkich subskrypcji platformy Azure przez klienta


(Data wydania: 28 lipca 2018 r.)

## <a name="release-highlights-for-july-28-2018"></a>Najważniejsze informacje o wersji dla 28 lipca 2018 r.


-   *Szacowane ceny* udostępnić widok opłat klienta waluty skutki konwersji.
-   *Prognozowane wypłaty* podać wcześniej wgląd w potencjalne wypłaty.
-  *Użycie odwołania identyfikatory* udostępnianie danych wierność między użycia przez klientów i zamówień wypłaty
-   *Użycie dokładnością* zapewnia większą szczegółowość i lepszy wgląd w użycie klienta.


### <a name="changes-to-data-structure-and-taxonomy"></a>Zmiany struktury danych i taksonomii

Poniższa tabela zawiera listę metryk, które zostały dodane lub znaczne zmiany w tej wersji. 

| **Nowy termin**                   |    **Definicja**                                                             |
|--------------------------------|  ---------------------------------------------------------------------------- |
| Cena (DW)                     | Cena jednostki użycia dla danej jednostki SKU (w walucie klienta).       |
| Szacowana opłata rozszerzone (DW) | Szacowany rozszerzonej opłaty za ilość jednostek użycia dla danej jednostki SKU (w walucie klienta). Ta wartość nie może być dokładne z powodu błędów zaokrąglania lub obcięcie.   |
| Wydawca waluty (PC)        | Preferowane przez wydawcę, aby uzyskać informacje dotyczące wypłat Waluta.                               |
| Szacowana cena (PC)           | Szacowana cena jednostki użycia dla danej jednostki SKU opartej na konwersji walutowych użycia Data jest obliczana (w walucie wydawcy). Ta wartość nie może być dokładne z powodu błędów zaokrąglania lub obcięcie.   |
| Szacowana opłata rozszerzone (PC) | Szacowany rozszerzonej opłaty za ilość jednostek użycia dla danej jednostki SKU opartej na konwersji walutowych użycia Data jest obliczana (w walucie wydawcy). Ta wartość nie może być dokładne z powodu błędów zaokrąglania lub obcięcie. |
| Informacje dotyczące wypłat szacowany (PC)          | Szacowany płatności za ilość jednostek użycia dla danej jednostki SKU na podstawie walutowych konwersja na datę, użycie jest obliczane (w walucie wydawcy). Ta wartość nie może być dokładne z powodu błędów zaokrąglania lub obcięcie.   |
| Odwołanie do użycia                | Identyfikator dla jednego lub więcej dni użycia przez klientów dla danej jednostki SKU, skojarzone z wpisem w raporcie informacje dotyczące wypłat. |
|  |  |
