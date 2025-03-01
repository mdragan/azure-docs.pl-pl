---
title: Często zadawane pytania — przetwarzanie obrazów
titlesuffix: Azure Cognitive Services
description: Uzyskaj odpowiedzi na często zadawane pytania dotyczące interfejsu API przetwarzania obrazów w usługach Azure Cognitive Services.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: fce3242746d47809c4fbbb1448453369d6460a9b
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203236"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Interfejs API przetwarzania obrazów — często zadawane pytania

> [!TIP]
> Jeśli nie możesz znaleźć odpowiedzi na pytania w tym — często zadawane pytania, zadaj społeczności interfejs API przetwarzania obrazów na [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) lub skontaktuj się z [Pomoc i obsługa techniczna w usłudze UserVoice](https://cognitive.uservoice.com/)

---

**Pytanie**: *Czy mogę uczyć się za pomocą tagów niestandardowych, interfejs API przetwarzania obrazów  Na przykład chcę otrzymywać kanału informacyjnego w obrazach RAS cat szkolenie sztucznej Inteligencji, a następnie wartość na rynku emulatorowi w żądaniu sztucznej Inteligencji.*

**Odpowiedź**: Ta funkcja nie jest obecnie dostępna. Jednak nasi inżynierowie zajmujący się nad przenieść tej funkcji do przetwarzania obrazów.

---

**Pytanie**: *Przetwarzanie obrazów można lokalnie bez połączenia z Internetem?*

**Odpowiedź**: Obecnie nie oferujemy lokalną lub rozwiązania lokalnego.

---

**Pytanie**: *Może służyć do odczytu rejestracyjnych przetwarzania obrazów?*

**Odpowiedź**: Interfejs API przetwarzania zapewnia dobre wykrywania tekstu za pomocą oprogramowania OCR, ale nie jest obecnie zoptymalizowany dla rejestracyjnych. Firma Microsoft stale dążymy do poprawy naszych usług i dodaliśmy optyczne rozpoznawanie znaków rozpoznawania płytkę licencji automatycznie do naszej listy żądania funkcji.

---

**Pytanie**: *Jakie typy zapisu powierzchni są obsługiwane dla rozpoznawania pisma ręcznego?*

**Odpowiedź**: Technologia działa z różnymi rodzajami powierzchnie, w tym tablic, oficjalny dokument i żółty notatki.

---

**Pytanie**: *Jak wykonać rozpoznawanie pisma ręcznego długo?*

**Odpowiedź**: Ilość czasu, który zajmuje, zależy od długości tekstu. Dla dłuższych tekstów może potrwać kilka sekund. W związku z tym po zakończeniu operacji rozpoznawanie tekstu odręcznego, konieczne może być oczekiwania, zanim będzie można pobrać wyniki za pomocą operacji Pobierz odręcznego wynik operacji tekstu.

---

**Pytanie**: *Jak technologia rozpoznawania pisma ręcznego obsługuje tekst, który został wstawiony przy użyciu daszek pośrodku linii?*

**Odpowiedź**: Takiego tekstu jest zwracana jako oddzielnym wierszu w przez operację rozpoznawania pisma ręcznego.

---

**Pytanie**: *Jak technologia rozpoznawania pisma ręcznego obsługuje przekreślonym słowa lub wierszy?*

**Odpowiedź**: Jeśli słowa zostanie przekroczony z wieloma wierszami do renderowania ich nierozpoznawalną, operacja rozpoznawania pisma ręcznego nie przejmą ich. Jednak jeśli słowa zostanie przekroczony przy użyciu pojedynczego wiersza, tego przejścia, jest traktowany jako hałasu i wyrazy nadal uzyskać odebrane przez operację rozpoznawania pisma ręcznego.

---

**Pytanie**: *Jakie orientacje tekst są obsługiwane w przypadku technologii rozpoznawania pisma ręcznego?*

**Odpowiedź**: Tekst o orientacji pod kątem programu do około 30 stopni na stopnie 40 może uzyskać pobierane przez operację rozpoznawania pisma ręcznego.

---
