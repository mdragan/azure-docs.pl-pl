---
title: Łączenie aplikacji Node.js platformy Mongoose w usłudze Azure Cosmos DB
description: Dowiedz się, jak używać Framework platformy Mongoose do przechowywania danych i zarządzanie nimi w usłudze Azure Cosmos DB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.custom: seodec18
ms.openlocfilehash: 23275bc639b445b55cafb72c929514541ba00660
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61333468"
---
# <a name="connect-a-nodejs-mongoose-application-to-azure-cosmos-db"></a>Łączenie aplikacji Node.js platformy Mongoose w usłudze Azure Cosmos DB

W tym samouczku przedstawiono sposób użycia [Framework platformy Mongoose](https://mongoosejs.com/) w przypadku przechowywania danych w usłudze Cosmos DB. W tym przewodniku używamy interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB. Mongoose to platforma modelowania obiektów usługi MongoDB w środowisku Node.js. Udostępnia ona także proste, bazujące na schematach rozwiązanie do modelowania danych aplikacji.

Usługa cosmos DB to usługa globalnie dystrybuowana, wielomodelowa baza danych firmy Microsoft. Dzięki dystrybucji globalnej i możliwości skalowania poziomego w usłudze Cosmos DB możesz szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość oraz grafów.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[Node.js](https://nodejs.org/) w wersji 0.10.29 lub nowszej.

## <a name="create-a-cosmos-account"></a>Tworzenie konta usługi Cosmos

Utwórz konta usługi Cosmos. Jeśli masz już konto, którego chcesz użyć, możesz przejść od razu do zestawu aplikacji Node.js. Jeśli używasz emulatora usługi Azure Cosmos DB, wykonaj czynności opisane w temacie [emulatora usługi Azure Cosmos DB](local-emulator.md) Aby skonfigurować emulator, a następnie przejdź do sekcji do zestawu aplikacji Node.js.

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="set-up-your-nodejs-application"></a>Konfigurowanie aplikacji Node.js

>[!Note]
> Jeśli chcesz tylko prześledzić przykładowy kod bez konfigurowania samej aplikacji, sklonuj [przykład](https://github.com/Azure-Samples/Mongoose_CosmosDB) używany na potrzeby tego samouczka i skompiluj aplikację Mongoose napisaną w języku Node.js w usłudze Azure Cosmos DB.

1. Aby utworzyć aplikację Node.js w wybranym folderze, uruchom następujące polecenie w wierszu polecenia węzła.

    ```npm init```

    Po odpowiedzeniu na wyświetlone pytania projekt będzie gotowy.

1. Dodaj nowy plik do folderu i nadaj mu nazwę ```index.js```.
1. Zainstaluj wymagane pakiety przy użyciu jednej z opcji polecenia ```npm install```:
   * Mongoose: ```npm install mongoose@5 --save```

     > [!Note]
     > Poniższe połączenie przykład platformy Mongoose jest oparty na platformy Mongoose 5 +, które zostały zmienione od wcześniejszych wersji.
    
   * Dotenv (jeśli chcesz załadować wpisy tajne z pliku env): ```npm install dotenv --save```

     >[!Note]
     > Parametr ```--save``` powoduje dodanie zależności do pliku package.json.

1. Zaimportuj zależności w pliku index.js.
    ```JavaScript
    var mongoose = require('mongoose');
    var env = require('dotenv').load();    //Use the .env file to load the variables
    ```

1. Dodaj parametry połączenia usługi Cosmos DB i nazwę bazy danych Cosmos DB do pliku ```.env```.

    ```JavaScript
    COSMOSDB_CONNSTR=mongodb://{cosmos-user}.documents.azure.com:10255/{dbname}
    COSMODDB_USER=cosmos-user
    COSMOSDB_PASSWORD=cosmos-secret
    ```

1. Połączyć z usługą Cosmos DB przy użyciu platformy mongoose, dodając następujący kod na końcu pliku index.js.
    ```JavaScript
    mongoose.connect(process.env.COSMOSDB_CONNSTR+"?ssl=true&replicaSet=globaldb", {
      auth: {
        user: process.env.COSMODDB_USER,
        password: process.env.COSMOSDB_PASSWORD
      }
    })
    .then(() => console.log('Connection to CosmosDB successful'))
    .catch((err) => console.error(err));
    ```
    >[!Note]
    > W tym miejscu zmienne środowiskowe są ładowane przy użyciu metody process.env.{nazwa_zmiennej} i pakietu npm „dotenv”.

    Teraz, po nawiązaniu połączenia z usługą Azure Cosmos DB, możesz rozpocząć konfigurowanie modeli obiektów na platformie Mongoose.

## <a name="caveats-to-using-mongoose-with-cosmos-db"></a>Zastrzeżenia dotyczące korzystania z platformy Mongoose z usługą Cosmos DB

Dla każdego utworzonego modelu platforma Mongoose tworzy nową kolekcję. Jednak podane dla kolekcji model rozliczeń za usługę Cosmos DB, może nie być najbardziej efektywny kosztowo sposób sposób, aby przejść, jeśli masz wiele modeli obiektów, które słabo są wypełnione.

W tym przewodniku opisano oba modele. Najpierw zostanie omówiony proces przechowywania jednego typu danych w jednej kolekcji. Jest to domyślne zachowanie platformy Mongoose.

Na platformie Mongoose funkcjonuje też pojęcie [dyskryminatorów](https://mongoosejs.com/docs/discriminators.html). Dyskryminatory to mechanizm dziedziczenia schematów. Umożliwiają one posiadanie wielu modeli z nakładającymi się schematami w jednej bazowej kolekcji usługi MongoDB.

Dzięki temu można przechowywać różne modele danych w tej samej kolekcji, a następnie, korzystając z klauzuli filtru w czasie przetwarzania zapytania, pobierać tylko potrzebne dane.

### <a name="one-collection-per-object-model"></a>Jedna kolekcja na model obiektów

Domyślnym zachowaniem platformy Mongoose jest tworzenie kolekcji usługi MongoDB każdorazowo podczas tworzenia modelu obiektów. W tej sekcji przedstawiono, jak można to osiągnąć przy użyciu interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB. Ta metoda jest zalecana w przypadku przetwarzania modeli obiektów z dużą ilością danych. Jest to domyślny model działania platformy Mongoose, dlatego osoby korzystające z tej platformy nie będą miały problemów z jego implementacją.

1. Otwórz ponownie plik ```index.js```.

1. Utwórz definicję schematu modelu „Family” (Rodzina).

    ```JavaScript
    const Family = mongoose.model('Family', new mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
            gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }));
    ```

1. Utwórz obiekt w modelu „Family” (Rodzina).

    ```JavaScript
    const family = new Family({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });
    ```

1. Na koniec Zapisz ten obiekt w usłudze Cosmos DB. Spowoduje to utworzenie kolekcji w obiekcie nadrzędnym.

    ```JavaScript
    family.save((err, saveFamily) => {
        console.log(JSON.stringify(saveFamily));
    });
    ```

1. Teraz utwórzmy jeszcze jeden schemat i obiekt. Tym razem utwórzmy te elementy dla modelu „Vacation Destinations” (Miejsca spędzania wakacji) zawierającego miejsca atrakcyjne dla rodziny.
   1. Podobnie jak wcześniej utwórz schemat.
      ```JavaScript
      const VacationDestinations = mongoose.model('VacationDestinations', new mongoose.Schema({
       name: String,
       country: String
      }));
      ```

   1. Utwórz przykładowy obiekt (do tego schematu można dodać wiele obiektów) i zapisz go.
      ```JavaScript
      const vacaySpot = new VacationDestinations({
       name: "Honolulu",
       country: "USA"
      });

      vacaySpot.save((err, saveVacay) => {
       console.log(JSON.stringify(saveVacay));
      });
      ```

1. Teraz przechodząc do witryny Azure portal, zauważysz dwie kolekcje tworzone w usłudze Cosmos DB.

    ![Node.js samouczka — zrzut ekranu witryny Azure Portal przedstawiający konto usługi Azure Cosmos DB z wyróżnionymi wieloma nazwami kolekcji — baza danych Node][multiple-coll]

1. Na koniec spróbujmy odczytać dane z usługi Cosmos DB. Ponieważ korzystamy z domyślnego modelu działania platformy Mongoose, operacje odczytywania są takie same jak inne operacje odczytywania na platformie Mongoose.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Korzystanie z dyskryminatorów platformy Mongoose w celu umożliwienia przechowywania danych w pojedynczej kolekcji

W przypadku tej metody użyjemy [Dyskryminatorów platformy Mongoose](https://mongoosejs.com/docs/discriminators.html) aby pomóc zoptymalizować koszty poszczególnych kolekcji. Dyskryminatory umożliwiają zdefiniowanie różnicującego „klucza”, który umożliwi przechowywanie, różnicowanie i filtrowanie różnych modeli obiektów.

W tym miejscu utworzymy bazowy model obiektów, zdefiniujemy klucz różnicujący i dodamy modele „Family” i „VacationDestinations” jako rozszerzenia modelu bazowego.

1. Ustawmy podstawową konfigurację i zdefiniujmy klucz dyskryminatora.

    ```JavaScript
    const baseConfig = {
        discriminatorKey: "_type", //If you've got a lot of different data types, you could also consider setting up a secondary index here.
        collection: "alldata"   //Name of the Common Collection
    };
    ```

1. Następnie zdefiniujmy wspólny model obiektów.

    ```JavaScript
    const commonModel = mongoose.model('Common', new mongoose.Schema({}, baseConfig));
    ```

1. Teraz definiujemy model „Family”. Zauważ, że teraz używamy metody ```commonModel.discriminator``` zamiast ```mongoose.model```. Ponadto dodajemy również podstawową konfigurację do schematu mongoose. W tym przypadku kluczem dyskryminatora jest więc ```FamilyType```.

    ```JavaScript
    const Family_common = commonModel.discriminator('FamilyType', new     mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
           gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }, baseConfig));
    ```

1. Podobnie dodajmy kolejny schemat, tym razem dla modelu „VacationDestinations”. Tutaj kluczem dyskryminatora jest ```VacationDestinationsType```.

    ```JavaScript
    const Vacation_common = commonModel.discriminator('VacationDestinationsType', new mongoose.Schema({
        name: String,
        country: String
    }, baseConfig));
    ```

1. Na koniec utworzymy obiekty modelu i zapiszemy go.
   1. Dodajmy obiekty do modelu „Family”.
      ```JavaScript
      const family_common = new Family_common({
       lastName: "Volum",
       parents: [
           { firstName: "Thomas" },
           { firstName: "Mary Kay" }
       ],
       children: [
           { firstName: "Ryan", gender: "male", grade: 8 },
           { firstName: "Patrick", gender: "male", grade: 7 }
       ],
       pets: [
           { givenName: "Blackie" }
       ],
       address: { country: "USA", state: "WA", city: "Seattle" }
      });

      family_common.save((err, saveFamily) => {
       console.log("Saved: " + JSON.stringify(saveFamily));
      });
      ```

   1. Następnie dodajmy obiekty do modelu „VacationDestinations” i zapiszmy go.
      ```JavaScript
      const vacay_common = new Vacation_common({
       name: "Honolulu",
       country: "USA"
      });

      vacay_common.save((err, saveVacay) => {
       console.log("Saved: " + JSON.stringify(saveVacay));
      });
      ```

1. Jeśli teraz wrócisz do witryny Azure Portal, zauważysz, że znajduje się tam tylko jedna kolekcja o nazwie ```alldata```, która zawiera dane zarówno z modelu „Family”, jak i „VacationDestinations”.

    ![Node.js samouczka — zrzut ekranu witryny Azure Portal przedstawiający konto usługi Azure Cosmos DB z wyróżnioną nazwą kolekcji — baza danych Node][alldata]

1. Zauważ również, że każdy obiekt ma jeszcze jeden atrybut o nazwie ```__type```, który pomaga rozróżnić te dwa różne modele obiektów.

1. Na koniec spróbujmy odczytać dane przechowywane w usłudze Azure Cosmos DB. Platforma Mongoose dba o odpowiednie filtrowanie danych na podstawie modelu. Podczas odczytywania danych nie trzeba robić niczego więcej. Wystarczy tylko określić model (w tym przypadku ```Family_common```), a platforma Mongoose zajmie się filtrowaniem według klucza „DiscriminatorKey”.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Jak widać, praca z dyskryminatorami platformy Mongoose jest prosta. Dlatego jeśli masz aplikację, która używa platformy mongoose, ten samouczek jest sposób swoją aplikację wzwyż i uruchamiać aplikacje przy użyciu interfejsu API usługi Azure Cosmos dla bazy danych MongoDB bez konieczności zbyt dużej liczby zmian.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się, jak [korzystać z programu Studio 3T](mongodb-mongochef.md) za pomocą interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB.
- Dowiedz się, jak [korzystać z programu Robo 3T](mongodb-robomongo.md) za pomocą interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB.
- Eksploruj [przykłady](mongodb-samples.md) bazy danych MongoDB za pomocą interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB.

[alldata]: ./media/mongodb-mongoose/mongo-collections-alldata.png
[multiple-coll]: ./media/mongodb-mongoose/mongo-mutliple-collections.png
