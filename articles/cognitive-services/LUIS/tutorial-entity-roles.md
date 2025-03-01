---
title: Dane kontekstowe z rolami - Language Understanding
titleSuffix: Azure Cognitive Services
description: Znajdź powiązane dane na podstawie kontekstu. Na przykład powiązane są lokalizacje początkowa i docelowa dla fizycznego przeniesienia z jednego budynku i biura do innego budynku i biura.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: a0ab928ef3b8551e3e20ff3c4b16533c80ee4b7d
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65149299"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Samouczek: wyodrębnianie danych powiązanych kontekstowo z wypowiedzi

W tym samouczku znajdziesz powiązane elementy danych na podstawie kontekstu. Na przykład lokalizację początkową i docelową dla przeniesienia z jednego miasta do innego. Potrzebne mogą być oba elementy danych. Są one powiązane ze sobą.  

Rolę można używać z dowolnego typu wbudowanych lub niestandardowych instytucji rządowej USA i używać zarówno w przykładzie wypowiedzi i wzorce. 

**Ten samouczek zawiera informacje na temat wykonywania następujących czynności:**

> [!div class="checklist"]
> * Tworzenie nowej aplikacji
> * Dodawanie intencji 
> * Uzyskaj informacje początkowego i docelowego przy użyciu ról
> * Szkolenie
> * Publikowanie
> * Pobierz intencje i ról jednostki z punktu końcowego

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="related-data"></a>Pokrewne dane

Ta aplikacja określa, dokąd ma zostać przeniesiony pracownik z miasta źródłowego do miasta docelowego. Używa ona GeographyV2 wstępnie utworzone jednostki, aby zidentyfikować nazwy miast i używa ról, aby określić typy lokalizacji (źródło i miejsce docelowe) w ramach wypowiedź.

Roli powinny być używane, gdy dane jednostki do wyodrębnienia:

* Jest powiązany ze sobą w kontekście wypowiedź.
* Używa określony wyraz wyboru, aby wskazać każdej roli. Przykładowe wyrazy tego typu to: from/to (od/do), leaving/headed to (opuszczać/kierować się do), away from/toward (w kierunku od/do).
* Obie te role są często stosowane w tej samej wypowiedź, dzięki czemu usługa LUIS dowiedzieć się więcej, podczas korzystania z tej częste kontekstowych.
* Te informacje należy grupować i przetwarzać jako całość w aplikacji klienckiej.

## <a name="create-a-new-app"></a>Tworzenie nowej aplikacji

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-an-intent-to-move-employees-between-cities"></a>Tworzenie intencji przeniesienia pracowników między miastami

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Wybierz pozycję **Create new intent** (Utwórz nową intencję). 

1. Wprowadź ciąg `MoveEmployeeToCity` w wyświetlonym oknie dialogowym, a następnie wybierz pozycję **Done** (Gotowe). 

    ![Zrzut ekranu z wyskakującym oknem dialogowym Create new intent (Tworzenie nowej intencji)](./media/tutorial-entity-roles/create-new-intent-move-employee-to-city.png)

1. Dodaj przykładowe wypowiedzi do intencji.

    |Przykładowe wypowiedzi|
    |--|
    |Przenoś Jan Kowalski W., pozostawiając Seattle z nagłówkiem do Orlando|
    |transfer Jill Jones from Seattle to Cairo (przenieś Jill Jones z Seattle do Kairu)|
    |Place John Jackson away from Tampa, coming to Atlanta (Umieść Johna Jacksona opuszczającego Tampę w Atlancie) |
    |move Debra Doughtery to Tulsa from Chicago|
    |mv Jill Jones leaving Cairo headed to Tampa (przesuń Jill Jones z Kairu do Tampy)|
    |Shift Alice Anderson to Oakland from Redmond (Transferuj Alice Anderson do Oakland z Redmond)|
    |Carl Chamerlin from San Francisco to Redmond (Carl Chamerlin z San Francisco do Redmond)|
    |Transfer Steve Standish from San Diego toward Bellevue (Transferuj Steve’a Standisha z San Diego do Bellevue) |
    |lift Tanner Thompson from Kansas city and shift to Chicago (Tannera Thompsona z Kansas City przenieś do Chicago)|

    [![Zrzut ekranu usługi LUIS z nowymi wypowiedziami w intencji MoveEmployee](./media/tutorial-entity-roles/hr-enter-utterances.png)](./media/tutorial-entity-roles/hr-enter-utterances.png#lightbox)

## <a name="add-prebuilt-entity-geographyv2"></a>Dodaj geographyV2 wstępnie utworzone jednostki

Wstępnie utworzone jednostki, geographyV2, wyodrębnianie informacji o lokalizacji, w tym nazwy miast. Ponieważ wypowiedzi ma dwie nazwy miasta, odnoszące się do siebie nawzajem w kontekście ról można użyć do wyodrębnienia z tego kontekstu.

1. Wybierz **jednostek** z okienka nawigacji po lewej stronie.

1. Wybierz **Dodawanie wstępnie utworzone jednostki**, a następnie wybierz `geo` na pasku wyszukiwania, aby filtrować ze wstępnie utworzonych jednostek. 

    ![Dodaj geographyV2 wstępnie utworzone jednostki do aplikacji](media/tutorial-entity-roles/add-geographyV2-prebuilt-entity.png)
1. Zaznacz pole wyboru, a następnie wybierz pozycję **gotowe**.
1. W **jednostek** listy wybierz **geographyV2** można otworzyć nowego obiektu. 
1. Dodaj dwie role `Origin`, i `Destination`. 

    ![Dodawanie ról do wstępnie utworzone jednostki](media/tutorial-entity-roles/add-roles-to-prebuilt-entity.png)
1. Wybierz **intencji** z okienka nawigacji po lewej stronie, następnie wybierz pozycję **MoveEmployeeToCity** intencji. Zwróć uwagę, nazwy miast są oznaczone etykietami od wstępnie utworzone jednostki **geogrpahyV2**.
1. W pierwszym wypowiedź listy wybierz lokalizację pochodzenia. Zostanie wyświetlone menu rozwijane. Wybierz **geographyV2** na liście, a następnie wykonaj menu na, aby wybrać **pochodzenia**.
1. Aby oznaczyć wszystkie role lokalizacji w wszystkich wypowiedzi, należy użyć metody z poprzedniego kroku. 


## <a name="add-example-utterances-to-the-none-intent"></a>Dodawanie przykładowych wypowiedzi do intencji None 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Trenowanie aplikacji w celu umożliwienia testowania zmian w intencji 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Publikowanie aplikacji w celu umożliwienia wysyłania zapytań z punktu końcowego do trenowanego modelu

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Pobieranie przewidywania intencji i jednostek z punktu końcowego

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Przejdź na koniec adresu URL na pasku adresu i wprowadź ciąg `Please move Carl Chamerlin from Tampa to Portland`. Ostatni parametr ciągu zapytania to `q`, czyli **query** (zapytanie) wypowiedzi. Ten wypowiedź nie jest taka sama jak dowolnego z etykietami wypowiedzi, więc to dobry test umożliwiający i powinien zwrócić `MoveEmployee` intencji z jednostką wyodrębnione.

    ```json
    {
      "query": "Please move Carl Chamerlin from Tampa to Portland",
      "topScoringIntent": {
        "intent": "MoveEmployeeToCity",
        "score": 0.979823351
      },
      "intents": [
        {
          "intent": "MoveEmployeeToCity",
          "score": 0.979823351
        },
        {
          "intent": "None",
          "score": 0.0156363435
        }
      ],
      "entities": [
        {
          "entity": "geographyV2",
          "role": "Destination",
          "startIndex": 41,
          "endIndex": 48,
          "score": 0.6044041
        },
        {
          "entity": "geographyV2",
          "role": "Origin",
          "startIndex": 32,
          "endIndex": 36,
          "score": 0.739491045
        }
      ]
    }
    ```
    
    Przewiduje się poprawne intencji i Tablica jednostki ma początkowego i docelowego ról w odpowiednich **jednostek** właściwości.
    
## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Informacje pokrewne

* [Pojęcia dotyczące jednostek](luis-concept-entity-types.md)
* [Pojęcia dotyczące ról](luis-concept-roles.md)
* [Lista ze wstępnie utworzonych jednostek](luis-reference-prebuilt-entities.md)
* [Jak trenować](luis-how-to-train.md)
* [Jak opublikować](luis-how-to-publish-app.md)
* [Jak przeprowadzać testy w portalu usługi LUIS](luis-interactive-test.md)
* [Role](luis-concept-roles.md)

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku utworzono nową intencję i dodano przykładowe wypowiedzi dla uczonych kontekstowo danych dotyczących lokalizacji początkowej i docelowej. Gdy aplikacja zostanie wyszkolona i opublikowana, aplikacja kliencka będzie mogła używać tych informacji do tworzenia biletu przeniesienia z odpowiednimi informacjami.

> [!div class="nextstepaction"] 
> [Dowiedz się, jak dodać jednostkę złożoną](luis-tutorial-composite-entity.md) 
