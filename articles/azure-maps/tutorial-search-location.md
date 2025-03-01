---
title: Wyszukiwanie przy użyciu usługi Azure Maps | Microsoft Docs
description: Wyszukiwanie pobliskiego punktu orientacyjnego za pomocą usługi Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 03/07/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 21c7e996cfe2f07d9a5b056d8cbc22f238bb48fd
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357619"
---
# <a name="search-nearby-points-of-interest-using-azure-maps"></a>Wyszukiwanie pobliskich punktów orientacyjnych za pomocą usługi Azure Maps

W tym samouczku przedstawiono konfigurowanie konta przy użyciu usługi Azure Maps, a następnie wyszukiwanie punktu orientacyjnego za pomocą interfejsów API usługi Maps. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie konta usługi Azure Maps
> * Pobieranie klucza podstawowego dla konta usługi Maps
> * Tworzenie nowej strony internetowej przy użyciu interfejsu API kontrolki mapy
> * Wyszukiwanie pobliskiego punktu orientacyjnego przy użyciu usługi wyszukiwania Maps

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Tworzenie konta przy użyciu usługi Azure Maps

Utwórz nowe konto usługi Maps, wykonując następujące czynności:

1. W lewym górnym rogu witryny [Azure Portal](https://portal.azure.com) kliknij przycisk **Utwórz zasób**.
2. W polu *Wyszukaj w portalu Marketplace* wpisz **Maps**.
3. Z listy *Wyniki* wybierz pozycję **Maps**. Kliknij przycisk **Utwórz** znajdujący się poniżej mapy.
4. Na stronie **Tworzenie konta usługi Maps** wprowadź następujące wartości:
    * *Subskrypcja*, która ma być używana dla tego konta.
    * Nazwa *grupy zasobów* dla tego konta. Można wybrać pozycję *Utwórz nowe* lub *Użyj istniejącego* dla grupy zasobów.
    * *Nazwa* nowego konta.
    * *Warstwa cenowa* dla tego konta.
    * Zapoznaj się z *Licencją* oraz *Zasadami zachowania poufności informacji* i zaznacz pole wyboru, aby zaakceptować warunki.
    * Kliknij przycisk **Utwórz**.

![Tworzenie konta usługi Maps w portalu](./media/tutorial-search-location/create-account.png)

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Pobieranie klucza podstawowego konta

Po pomyślnym utworzeniu konta usługi Maps pobierz klucz, który umożliwia wysyłanie zapytań do interfejsów API usługi Maps.

1. Otwórz konto usługi Maps w portalu.
2. W obszarze Ustawienia zaznacz **uwierzytelniania**.
3. Skopiuj **klucz podstawowy** do schowka. Zapisz go lokalnie — będzie używany w dalszej części tego samouczka.

![Pobieranie klucza podstawowego w portalu](./media/tutorial-search-location/get-key.png)

<a id="createmap"></a>

## <a name="create-a-new-map"></a>Tworzenie nowej mapy

Interfejs API kontrolki mapy to wygodna biblioteka klienta, która ułatwia integrowanie usługi Maps z aplikacją internetową. Zmniejsza ona złożoność podstawowych wywołań usługi REST i zwiększa produktywność dzięki składnikom z możliwością definiowania stylu i dostosowywania. Poniższe kroki pokazują, jak utworzyć statyczną stronę HTML osadzoną przy użyciu interfejsu API kontrolki mapy.

1. Na maszynie lokalnej utwórz nowy plik i nadaj mu nazwę **MapSearch.html**.
2. Dodaj następujące składniki HTML do pliku:

   ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Map Search</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas-service.min.js"></script>

        <script>
        function GetMap(){
            //Add Map Control JavaScript code here.
        }
        </script>

        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #myMap {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>
    </html>
    ```

   Zwróć uwagę, że nagłówek HTML zawiera pliki zasobów CSS i JavaScript obsługiwane przez bibliotekę kontrolek mapy platformy Azure. Zwróć uwagę na zdarzenie `onload` w treści strony, które spowoduje wywołanie funkcji `GetMap` po załadowaniu treści strony. `GetMap` Funkcja będzie zawierać wbudowany kod JavaScript, aby dostęp do interfejsów API usługi Azure Maps.

3. Dodaj następujący kod JavaScript do funkcji `GetMap` w pliku HTML. Zastąp ciąg `<Your Azure Maps Key>` za pomocą klucza podstawowego, który został skopiowany z Twojego konta usługi Maps.

    ```JavaScript
    //Instantiate a map object
    var map = new atlas.Map("myMap", {
        //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
        authOptions: {
            authType: 'subscriptionKey',
            subscriptionKey: '<Your Azure Maps Key>'
        }
    });
    ```

   Ten segment inicjuje interfejs API kontrolki mapy dla klucza konta usługi Azure Maps. `atlas` jest przestrzenią nazw, który zawiera interfejs API i powiązane składniki wizualne. `atlas.Map` udostępnia kontrolkę wizualnej i interaktywnej sieci web mapy.

4. Zapisz zmiany w pliku i otwórz stronę HTML w przeglądarce. Jest to najbardziej podstawowym mapy, który może zgłaszać przez wywołanie metody `atlas.Map` przy użyciu klucza konta.

   ![Wyświetlanie mapy](./media/tutorial-search-location/basic-map.png)

5. W funkcji `GetMap` po zainicjowaniu mapy dodaj następujący kod JavaScript.

    ```JavaScript
    //Wait until the map resources are ready.
    map.events.add('ready', function() {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering point data.
        var resultLayer = new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: 'pin-round-darkblue',
                anchor: 'center',
                allowOverlap: true
            },
            textOptions: {
                anchor: "top"
            }
        });

        map.layers.add(resultLayer);
    });
    ```

   W tym segmencie kodu `ready` do map, które będą uruchamiane, gdy załadowano zasobów mapy i mapy jest gotowy do można uzyskać dostępu do zostanie dodane zdarzenie. Na mapie `ready` procedura obsługi zdarzeń, tworzone jest źródło danych do przechowywania danych wyników. Tworzona jest warstwa symboli, która jest następnie dołączana do źródła danych. Ta warstwa określa, jak powinny być renderowane dane wyników w źródle danych. W tym przypadku jest ona oznaczona ciemnoniebieską ikoną okrągłej pinezki wyśrodkowaną nad współrzędnymi wyników i umożliwiającą nakładanie się na inne ikony. Warstwa wynik jest dodawana do warstwy mapy.

<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Dodawanie funkcji wyszukiwania

W tej sekcji pokazano, jak używać Maps [interfejsu API wyszukiwania](https://docs.microsoft.com/rest/api/maps/search) można znaleźć punktu orientacyjnego na mapie. Jest to interfejs API RESTful umożliwiający deweloperom wyszukiwanie adresów, punktów orientacyjnych i innych informacji geograficznych. Usługa Search Service przypisuje informacje o długości i szerokości geograficznej do określonego adresu. Opisany poniżej **moduł usługi** może być używany do wyszukiwania lokalizacji za pomocą interfejsu API usługi Search w usłudze Maps.

### <a name="service-module"></a>Moduł usługi

1. Na mapie `ready` programu obsługi zdarzeń skonstruować adres URL usługi wyszukiwania, dodając następujący kod Javascript.

    ```JavaScript
   // Use SubscriptionKeyCredential with a subscription key
   var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

   // Use subscriptionKeyCredential to create a pipeline
   var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

   // Construct the SearchURL object
   var searchURL = new atlas.service.SearchURL(pipeline); 
   ```

   `SubscriptionKeyCredential` Tworzy `SubscriptionKeyCredentialPolicy` do uwierzytelniania żądań HTTP do usługi Azure Maps za pomocą klucza subskrypcji. `atlas.service.MapsURL.newPipeline()` Przyjmuje `SubscriptionKeyCredential` zasad i tworzy [potoku](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) wystąpienia. `searchURL` Reprezentuje adres URL do usługi Azure Maps [wyszukiwania](https://docs.microsoft.com/rest/api/maps/search) operacji.

2. Następnie dodaj poniższy blok skryptu w celu utworzenia zapytania wyszukiwania. Ten fragment kodu używa usługi Fuzzy Search Service — podstawowego interfejsu API wyszukiwania usługi Search Service. Usługa Fuzzy Search Service obsługuje większość rozmytych danych wejściowych, takich jak adresy, miejsca i punkty orientacyjne (POI). Ten kod wyszukuje pobliskich stacji benzynowych w obrębie wybranego promienia podana szerokości i długości geograficznej. Kolekcja funkcji GeoJSON z odpowiedzi jest wyodrębniany przy użyciu `geojson.getFeatures()` — metoda i dodać do źródła danych, co automatycznie powoduje danych są renderowane na mapie za pomocą warstwy symboli. Ostatnia część skrypt ustawia widok kamery mapy za pomocą pola ograniczenia wyników z wykorzystaniem właściwości mapy [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-).

    ```JavaScript
    var query =  'gasoline-station';
    var radius = 9000;
    var lat = 47.64452336193245;
    var lon = -122.13687658309935;

    searchURL.searchPOI(atlas.service.Aborter.timeout(10000), query, {
        limit: 10,
        lat: lat,
        lon: lon,
        radius: radius
    }).then((results) => {

        // Extract GeoJSON feature collection from the response and add it to the datasource
        var data = results.geojson.getFeatures();
        datasource.add(data);

        // set camera to bounds to show the results
        map.setCamera({
            bounds: data.bbox,
            zoom: 10
        });
    });
    ```

3. Zapisz plik **MapSearch.html** i odśwież przeglądarkę. Powinien zostać wyświetlony, że mapa skupia się na Seattle przy użyciu numerów PIN round niebieski lokalizacje stacji benzynowych w obszarze znakowania.

   ![Wyświetlanie mapy z wynikami wyszukiwania](./media/tutorial-search-location/pins-map.png)

4. Wprowadzenie następującego żądania HTTP w przeglądarce spowoduje wyświetlenie danych pierwotnych renderowanych przez mapę. Ciąg \<Your Azure Maps Key\> zastąp kluczem podstawowym.

   ```http
   https://atlas.microsoft.com/search/poi/json?api-version=2&query=gasoline%20station&subscription-key=<subscription-key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

Na stronie MapSearch można teraz wyświetlić lokalizacje punktów orientacyjnych, zwracanych przez zapytanie wyszukiwania rozmytego. Do lokalizacji dodamy informacje oraz interaktywne funkcje.

## <a name="add-interactive-data"></a>Dodawanie interaktywnych danych

W wynikach wyszukiwania na utworzonej mapie są używane tylko dane o długości i szerokości geograficznej. Widać jednak, że nieprzetworzone dane JSON zwracane przez usługę Search Service w usłudze Maps zawierają dodatkowe informacje o każdej stacji paliw, takie jak nazwa i adres. Dane te można zintegrować z mapą za pomocą interaktywnych wyskakujących okienek.

1. Dodaj następujące wiersze kodu na mapie `ready` program obsługi zdarzeń po kodzie do zapytań do usługi wyszukiwania rozmytego. Spowoduje to utworzenie wystąpienia wyskakującego okienka i dodanie zdarzenia mouseover do warstwy symboli.

    ```JavaScript
   //Create a popup but leave it closed so we can update it and display it later.
    popup = new atlas.Popup();

    //Add a mouse over event to the result layer and display a popup when this event fires.
    map.events.add('mouseover', resultLayer, showPopup);
    ```

    Interfejs API `sup` informacje okna zakotwiczone w wymaganych położeniu na mapie. 

2. W *skryptu* tagu po `GetMap` funkcji, Dodaj następujący kod, aby pokazać moused za pośrednictwem informacje na temat wyników w menu podręcznym.

    ```JavaScript
    function showPopup(e) {
        //Get the properties and coordinates of the first shape that the event occured on.

        var p = e.shapes[0].getProperties();
        var position = e.shapes[0].getCoordinates();

        //Create HTML from properties of the selected result.
        var html = ['<div style="padding:5px"><div><b>', p.poi.name,
            '</b></div><div>', p.address.freeformAddress,
            '</div><div>', position[1], ', ', position[0], '</div></div>'];

        //Update the content and position of the popup.
        popup.setPopupOptions({
            content: html.join(''),
            position: position
        });

        //Open the popup.
        popup.open(map);
    }
    ```

3. Zapisz plik i odśwież przeglądarkę. Teraz mapa w przeglądarce wyświetla wyskakujące okienka informacyjne po najechaniu kursorem na dowolny znacznik wyszukiwania.

    ![Kontrolka mapy platformy Azure i usługa Search Service](./media/tutorial-search-location/popup-map.png)

## <a name="next-steps"></a>Kolejne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie konta przy użyciu usługi Azure Maps
> * Pobieranie klucza podstawowego konta
> * Tworzenie nowej strony internetowej przy użyciu interfejsu API kontrolki mapy
> * Wyszukiwanie punktu orientacyjnego w pobliżu przy użyciu usługi Search Service

> [!div class="nextstepaction"]
> [Wyświetl pełny kod źródłowy](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/search.html)

> [!div class="nextstepaction"]
> [Wyświetl przykład na żywo](https://azuremapscodesamples.azurewebsites.net/?sample=Search%20for%20points%20of%20interest)

Następny samouczek przedstawia sposób wyświetlenia trasy między dwiema lokalizacjami.

> [!div class="nextstepaction"]
> [Trasa do miejsca docelowego](./tutorial-route-location.md)
