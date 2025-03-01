---
title: Uczenie modelu — niestandardowe w usłudze Translator
titleSuffix: Azure Cognitive Services
description: Uczenia modelu jest ważnym krokiem podczas tworzenia modelu tłumaczenia. Szkolenie odbywa się na podstawie na dokumentach, wybrany dla tego szkoleniach.
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 8804285bf419bce5ca85cc5070cd47ce9a87392a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447961"
---
# <a name="train-a-model"></a>Szkolenie modelu

Uczenia modelu jest ważnym krokiem do tworzenia modelu tłumaczenia, ponieważ bez szkolenia, nie można utworzyć modelu. Szkolenie odbywa się na podstawie na dokumentach, wybrany dla szkoleniach.

Do nauczenia modelu:

1.  Wybierz projekt, w którym chcesz utworzyć model.

2.  Na karcie danych dla projektu zostaną wyświetlone wszystkie dokumenty dla pary języka projektu. Ręcznie wybrać dokumenty, które chcesz użyć do uczenia modelu. Możesz wybrać szkolenia, dostosowywania i testowania dokumentów za pośrednictwem tego ekranu. Również możesz po prostu wybrać zestaw szkoleniowy i niestandardowe w usłudze Translator tworzenie, dostosowywanie i testowanie zestawy dla Ciebie.

    -  Nazwa dokumentu: Nazwa dokumentu.

    -  Parowanie: Jeśli w tym dokumencie jest równoległe lub jednojęzyczne dokumentu. Jednojęzyczne dokumenty nie są obecnie obsługiwane dla szkolenia.

    -  Typ dokumentu: Może być szkolenia, dostosowywania, testy lub słownika.

    -  Para języka: Pokaż to źródłowy i docelowy język dla projektu.

    -  Zdania źródła: Przedstawia liczbę zdań wyodrębnione z pliku źródłowego.

    -  Zdania docelowej: Przedstawia liczbę zdań wyodrębnione z pliku docelowego.

    ![Trenowanie modelu](media/how-to/how-to-train-model.png)

3.  Kliknij przycisk pociągu.

4.  W oknie dialogowym Określ nazwę dla modelu.

5.  Kliknij Train model.

    ![Szkolenie modelu w oknie dialogowym](media/how-to/how-to-train-model-2.png)

6.  Niestandardowe w usłudze Translator przesłać szkolenia i wyświetlić stan szkolenia na karcie modeli.

    ![Szkolenie modelu strony](media/how-to/how-to-train-model-3.png)

>[!Note]
>Niestandardowe w usłudze Translator obsługuje 10 równoczesnych szkoleniach, w obszarze roboczym w dowolnym momencie w czasie.


## <a name="edit-a-model"></a>Edytowanie modelu

Możesz edytować model przy użyciu łącza edycji na stronie szczegółów modelu.

1.  Kliknij ikonę ołówka.

    ![Edytuj model](media/how-to/how-to-edit-model.png)

2.  W polu Zmień okna dialogowego

    1.  Nazwa modelu (wymagane): Nazwij swój model zrozumiałe.

        ![Więcej okno dialogowe Edycja](media/how-to/how-to-edit-model-dialog.png)

3.  Kliknij pozycję Zapisz.


## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się, [sposobu wyświetlania szczegółów modelu](how-to-view-model-details.md).
