---
title: Tworzenie aplikacji Angular przy użyciu interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB — dodawanie funkcji CRUD do aplikacji
titleSuffix: Azure Cosmos DB
description: Część 6 z serii samouczków o tworzeniu aplikacji bazy danych MongoDB przy użyciu usługi Angular i języka Node dla usługi Azure Cosmos DB przy użyciu dokładnie tych samych interfejsów API, które były używane dla bazy danych MongoDB
author: johnpapa
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: jopapa
ms.custom: seodec18
ms.reviewer: sngun
ms.openlocfilehash: 42015ca816f2744ef28660c5396db4cfd93a76f0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60766444"
---
# <a name="create-an-angular-app-with-azure-cosmos-dbs-api-for-mongodb---add-crud-functions-to-the-app"></a>Tworzenie aplikacji Angular przy użyciu interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB — dodawanie funkcji CRUD do aplikacji

Ten wieloczęściowy samouczek pokazuje, jak utworzyć nową aplikację napisaną w środowisku Node.js za pomocą programu Express i platformy Angular, a następnie podłączyć ją do [konta usługi Cosmos skonfigurowanego przy użyciu interfejsu API usługi Cosmos DB dla bazy danych MongoDB](mongodb-introduction.md). Część 6 samouczka jest oparta na [Części 5](tutorial-develop-mongodb-nodejs-part5.md) i obejmuje następujące zadania:

> [!div class="checklist"]
> * Tworzenie funkcji Post, Put i Delete dla usługi hero
> * Uruchamianie aplikacji

> [!VIDEO https://www.youtube.com/embed/Y5mdAlFGZjc]

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tej części samouczka upewnij się, że zostały wykonane czynności opisane w [Części 5](tutorial-develop-mongodb-nodejs-part5.md) samouczka.

> [!TIP]
> Ten samouczek przedstawia instrukcje tworzenia aplikacji krok po kroku. Jeśli chcesz pobrać gotowy projekt, ukończoną aplikację znajdziesz w [repozytorium angular-cosmosdb](https://github.com/Azure-Samples/angular-cosmosdb) w witrynie GitHub.

## <a name="add-a-post-function-to-the-hero-service"></a>Dodawanie funkcji Post do usługi hero

1. W programie Visual Studio Code otwórz obok siebie pliki **routes.js** i **hero.service.js**, naciskając przycisk **Split Editor** (Podziel edytor) ![Przycisk Split Editor (Podziel edytor) w programie Visual Studio](./media/tutorial-develop-mongodb-nodejs-part6/split-editor-button.png).

    Zauważ, że wiersz 7 pliku routes.js wywołuje funkcję `getHeroes` w wierszu 5 pliku **hero.service.js**.  Musimy utworzyć to samo parowanie dla funkcji post, put i delete. 

    ![Pliki routes.js i hero.service.js w programie Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part6/routes-heroservicejs.png)
    
    Zacznijmy od kodowania usługi hero. 

2. Skopiuj następujący kod do pliku **hero.service.js** po funkcji `getHeroes` i przed elementem `module.exports`. Ten kod:  
   * Używa modelu hero w celu opublikowania nowego elementu hero.
   * Sprawdza odpowiedzi, aby sprawdzić, czy wystąpił błąd, i zwraca wartość stanu 500.

   ```javascript
   function postHero(req, res) {
     const originalHero = { uid: req.body.uid, name: req.body.name, saying: req.body.saying };
     const hero = new Hero(originalHero);
     hero.save(error => {
       if (checkServerError(res, error)) return;
       res.status(201).json(hero);
       console.log('Hero created successfully!');
     });
   }

   function checkServerError(res, error) {
     if (error) {
       res.status(500).send(error);
       return error;
     }
   }
   ```

3. W pliku **hero.service.js** zaktualizuj element `module.exports`, aby uwzględnić nową funkcję `postHero`. 

    ```javascript
    module.exports = {
      getHeroes,
      postHero
    };
    ```

4. W pliku **routes.js** dodaj router dla funkcji `post` po routerze `get`. Ten router publikuje jeden element hero w danym momencie. Taka struktura pliku routera jasno przedstawia wszystkie dostępne punkty końcowe interfejsu API i pozostawia faktyczną pracę do wykonania plikowi **hero.service.js**.

    ```javascript
    router.post('/hero', (req, res) => {
      heroService.postHero(req, res);
    });
    ```

5. Sprawdź, czy wszystko działa, uruchamiając aplikację. W programie Visual Studio Code zapisz wszystkie zmiany wybierz przycisk **Debuguj** (Debuguj) ![Ikona Debug (Debuguj) w programie Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part6/debug-button.png) po lewej stronie, a następnie wybierz przycisk **Start Debugging** (Rozpocznij debugowanie) ![Ikona Start Debugging (Rozpocznij debugowanie) w programie Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part6/start-debugging-button.png).

6. Teraz wróć do przeglądarki internetowej i otwórz kartę Narzędzia programistyczne > Sieć, naciskając klawisz F12 w przypadku większości komputerów. Przejdź do adresu [http://localhost:3000](http://localhost:3000), aby obejrzeć wywołania wykonywane w sieci.

    ![Karta Sieć w przeglądarce Chrome z widoczną aktywnością sieci](./media/tutorial-develop-mongodb-nodejs-part6/add-new-hero.png)

7. Dodaj nowy element hero, wybierając przycisk **Add New Hero** (Dodaj nowy element hero). Wprowadź identyfikator „999”, nazwę „Fred” i powiedzenie „Hello”, a następnie wybierz przycisk **Save** (Zapisz). Na karcie Sieć powinno pojawić się wysłane żądanie POST dotyczące nowego elementu hero. 

    ![Karta Sieć w przeglądarce Chrome z aktywnością sieci dla funkcji Get i Post](./media/tutorial-develop-mongodb-nodejs-part6/post-new-hero.png)

    Teraz przejdź wstecz i dodaj funkcje Put i Delete do aplikacji.

## <a name="add-the-put-and-delete-functions"></a>Dodawanie funkcji Put i Delete

1. W pliku **routes.js** dodaj routery `put` i `delete` po routerze post.

    ```javascript
    router.put('/hero/:uid', (req, res) => {
      heroService.putHero(req, res);
    });

    router.delete('/hero/:uid', (req, res) => {
      heroService.deleteHero(req, res);
    });
    ```

2. Skopiuj następujący kod do pliku **hero.service.js** po funkcji `checkServerError`. Ten kod:
   * Tworzy funkcje `put` i `delete`
   * Sprawdza, czy element hero został znaleziony
   * Obsługuje błędy 

   ```javascript
   function putHero(req, res) {
     const originalHero = {
       uid: parseInt(req.params.uid, 10),
       name: req.body.name,
       saying: req.body.saying
     };
     Hero.findOne({ uid: originalHero.uid }, (error, hero) => {
       if (checkServerError(res, error)) return;
       if (!checkFound(res, hero)) return;

      hero.name = originalHero.name;
       hero.saying = originalHero.saying;
       hero.save(error => {
         if (checkServerError(res, error)) return;
         res.status(200).json(hero);
         console.log('Hero updated successfully!');
       });
     });
   }

   function deleteHero(req, res) {
     const uid = parseInt(req.params.uid, 10);
     Hero.findOneAndRemove({ uid: uid })
       .then(hero => {
         if (!checkFound(res, hero)) return;
         res.status(200).json(hero);
         console.log('Hero deleted successfully!');
       })
       .catch(error => {
         if (checkServerError(res, error)) return;
       });
   }

   function checkFound(res, hero) {
     if (!hero) {
       res.status(404).send('Hero not found.');
       return;
     }
     return hero;
   }
   ```

3. W pliku **hero.service.js** eksportuj nowe moduły:

   ```javascript
    module.exports = {
      getHeroes,
      postHero,
      putHero,
      deleteHero
    };
    ```

4. Teraz po zaktualizowaniu kodu wybierz przycisk **Restart** (Uruchom ponownie) ![Przycisk Restart (Uruchom ponownie) w programie Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part6/restart-debugger-button.png) w programie Visual Studio Code.

5. Odśwież stronę w przeglądarce internetowej, a następnie wybierz przycisk **Add New Hero** (Dodaj nowy element hero). Dodaj nowy element hero z identyfikatorem „9”, nazwą „Starlord” i powiedzeniem „Hi”. Wybierz przycisk **Save** (Zapisz), aby zapisać nowy element hero.

6. Teraz wybierz element hero **Starlord** i zmień powiedzenie z „Hi” na „Bye”, a następnie wybierz przycisk **Save** (Zapisz). 

    Teraz możesz wybrać identyfikator na karcie Sieć w celu wyświetlenia ładunku. W ładunku widać powiedzenie ustawione na „Bye”.

    ![Aplikacja Heroes i karta Sieć z przedstawionym ładunkiem](./media/tutorial-develop-mongodb-nodejs-part6/put-hero-function.png) 

    Możesz również usunąć jeden z elementów hero w interfejsie użytkownika i sprawdzić, ile czasu pozostało do ukończenia operacji usuwania. Wypróbuj tę funkcję, wybierz przycisk „Delete” (Usuń) dla elementu hero o nazwie „Fred”.

    ![Aplikacja Heroes i karta Sieć pokazująca czas potrzebny do ukończenia działania funkcji](./media/tutorial-develop-mongodb-nodejs-part6/times.png) 

    Po odświeżeniu strony na karcie sieci zostanie wyświetlony czas potrzebny do pobrania elementów hero. Czasy te są krótkie, ale wiele zależy od lokalizacji danych na świecie i możliwości ich replikacji geograficznej w obszarze znajdującym się w pobliżu użytkowników. Więcej informacji na temat replikacji geograficznej będzie można znaleźć w następnym samouczku, który udostępnimy wkrótce.

## <a name="next-steps"></a>Następne kroki

W tej części samouczka zostały wykonane następujące zadania:

> [!div class="checklist"]
> * Dodano funkcje Post, Put i Delete do aplikacji 

Wróć tu wkrótce, aby obejrzeć dodatkowe filmy wideo w tej serii samouczków.

