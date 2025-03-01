---
title: Samouczek środowiska Node.js dotyczący interfejsu API SQL dla usługi Azure Cosmos DB
description: Samouczek środowiska Node.js, w którym przedstawiono, jak nawiązać połączenie z bazą danych Azure Cosmos DB i wykonywać względem niej zapytania za pomocą interfejsu API SQL
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/05/2019
ms.author: dech
Customer intent: As a developer, I want to build a Node.js console application to access and manage SQL API account resources in Azure Cosmos DB, so that customers can better use the service.
ms.openlocfilehash: 61569159d83493bb5338f8eda5b9201ef9164143
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734582"
---
# <a name="tutorial-build-a-nodejs-console-app-with-the-javascript-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Samouczek: Tworzenie aplikacji konsolowej Node.js przy użyciu zestawu SDK dla języka JavaScript na potrzeby zarządzania danymi interfejsu API SQL usługi Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET (wersja zapoznawcza)](sql-api-dotnet-get-started-preview.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET Core (wersja zapoznawcza)](sql-api-dotnet-core-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Java (asynchroniczny)](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Jako deweloper być może masz aplikacje, które używają danych dokumentów NoSQL. Konto interfejsu API SQL w usłudze Azure Cosmos DB umożliwia przechowywanie tych danych dokumentów i uzyskiwanie do nich dostępu. W tym samouczku pokazano, jak utworzyć aplikację konsolową Node.js na potrzeby tworzenia zasobów usługi Azure Cosmos DB i wykonywania zapytań względem nich.

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Tworzenie konta usługi Azure Cosmos DB i łączenie się z nim
> * Konfigurowanie aplikacji
> * Tworzenie bazy danych
> * Tworzenie kontenera
> * Dodawanie elementów do kontenera
> * Wykonywanie podstawowych operacji względem elementów, kontenera i bazy danych

## <a name="prerequisites"></a>Wymagania wstępne 

Upewnij się, że masz następujące zasoby:

* Aktywne konto platformy Azure. Jeśli go nie masz, możesz zarejestrować się w celu uzyskania [bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Platforma [Node.js](https://nodejs.org/) w wersji 6.0.0 lub nowszej.

## <a name="create-azure-cosmos-db-account"></a>Tworzenie konta usługi Azure Cosmos DB

Utwórzmy konto usługi Azure Cosmos DB. Jeśli masz już konto, którego chcesz użyć, możesz przejść od razu do kroku [Konfigurowanie aplikacji Node.js](#SetupNode). Jeśli używasz emulatora usługi Azure Cosmos DB, wykonaj czynności opisane w temacie [Emulator usługi Azure Cosmos DB](local-emulator.md), aby skonfigurować emulator, a następnie przejdź do sekcji [Konfigurowanie aplikacji Node.js](#SetupNode). 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>Konfigurowanie aplikacji Node.js

Przed rozpoczęciem pisania kodu w celu utworzenia aplikacji możesz utworzyć platformę dla swojej aplikacji. Wykonaj następujące kroki, aby skonfigurować aplikację Node.js zawierającą kod platformy:

1. Otwórz swój ulubiony terminal.
2. Zlokalizuj folder lub katalog, w którym chcesz zapisać aplikację Node.js.
3. Utwórz dwa puste pliki JavaScript za pomocą następujących poleceń:

   * W systemie Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```

   * W systemie Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```

4. Utwórz i zainicjuj plik `package.json`. Użyj następującego polecenia:
   * ```npm init -y```

5. Zainstaluj moduł @azure/cosmos za pomocą menedżera npm. Użyj następującego polecenia:
   * ```npm install @azure/cosmos --save```

## <a id="Config"></a>Ustawianie konfiguracji aplikacji

Gdy aplikacja już istnieje, musisz się upewnić, że może ona komunikować się z usługą Azure Cosmos DB. Aktualizując kilka ustawień konfiguracji, jak pokazano w poniższych krokach, możesz skonfigurować aplikację tak, aby komunikowała się z usługą Azure Cosmos DB:

1. Otwórz plik ```config.js``` w ulubionym edytorze tekstu.

1. Skopiuj i wklej poniższy fragment kodu oraz ustaw identyfikator URI punktu końcowego i klucz podstawowy usługi Azure Cosmos DB jako wartości właściwości ```config.endpoint``` i ```config.primaryKey```. Obie te konfiguracje można znaleźć w witrynie [Azure Portal](https://portal.azure.com).

   ![Zrzut ekranu przedstawiający pobieranie kluczy z witryny Azure Portal][keys]

   ```javascript
   // ADD THIS PART TO YOUR CODE
   var config = {}

   config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
   config.primaryKey = "~your primary key here~";
   ``` 

1. Skopiuj i wklej dane ```database```, ```container``` i ```items``` do obiektu ```config``` poniżej miejsca, w którym zostały ustawione właściwości ```config.endpoint``` i ```config.primaryKey```. Jeśli masz już dane, które chcesz przechowywać w bazie danych, możesz użyć narzędzia migracji danych w usłudze Azure Cosmos DB, zamiast definiować tutaj dane. Plik config.js powinna mieć następujący kod:

   [!code-javascript[nodejs-get-started](~/cosmosdb-nodejs-get-started/config.js)]

   Zestaw SDK dla języka JavaScript używa terminów *kontener* i *element* w znaczeniu ogólnym. Kontener może być kolekcją, wykresem lub tabelą. Element może być dokumentem, krawędzią/wierzchołkiem lub wierszem i stanowi zawartość znajdująca się w kontenerze. 
   
   `module.exports = config;` Kod isused, aby wyeksportować swoje ```config``` obiektu tak, aby podać go w ramach ```app.js``` pliku.

## <a id="Connect"></a>Łączenie się z kontem usługi Azure Cosmos DB

1. Otwórz pusty plik ```app.js``` w edytorze tekstu. Skopiuj i wklej kod poniżej, aby zaimportować moduł ```@azure/cosmos``` i nowo utworzony moduł ```config```.

   ```javascript
   // ADD THIS PART TO YOUR CODE
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');
   ```

1. Skopiuj i wklej kod, aby użyć wcześniej zapisanych właściwości ```config.endpoint``` i ```config.primaryKey``` do utworzenia nowego modułu CosmosClient.

   ```javascript
   const config = require('./config');

   // ADD THIS PART TO YOUR CODE
   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });
   ```
   
> [!Note]
> W przypadku nawiązywania połączenia z **Emulatorem usługi Cosmos DB** wyłącz weryfikację SSL przez utworzenie niestandardowych zasad połączenia.
>   ```
>   const connectionPolicy = new cosmos.ConnectionPolicy ()
>   connectionPolicy.DisableSSLVerification = true
>
>   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey }, connectionPolicy });
>   ```

Teraz, gdy masz kod służący do zainicjowania klienta usługi Azure Cosmos DB, przyjrzyjmy się sposobowi pracy z zasobami usługi Azure Cosmos DB.

## <a name="create-a-database"></a>Tworzenie bazy danych

1. Skopiuj i wklej poniższy kod, aby ustawić identyfikator bazy danych i identyfikator kontenera. Te identyfikatory określają sposób znajdowania właściwej bazy danych i kontenera przez klienta usługi Azure Cosmos DB.

   ```javascript
   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   // ADD THIS PART TO YOUR CODE
   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;
   const partitionKey = { kind: "Hash", paths: ["/Country"] };
   ```

   Bazę danych można utworzyć przy użyciu funkcji `createIfNotExists` lub funkcji create klasy **Databases**. Baza danych jest logicznym kontenerem elementów podzielonym na partycje w kontenerach. 

2. Skopiuj i wklej metody **createDatabase** i **readDatabase** do pliku app.js pod definicją ```databaseId``` i ```containerId```. Funkcja **createDatabase** utworzy nową bazę danych (jeśli jeszcze nie istnieje) o identyfikatorze ```FamilyDatabase```określonym na podstawie obiektu ```config```. Funkcja **readDatabase** odczyta definicję bazy danych w celu sprawdzenia, czy baza danych istnieje.

   ```javascript
   /**
    * Create the database if it does not exist
    */
   async function createDatabase() {
       const { database } = await client.databases.createIfNotExists({ id: databaseId });
       console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
      const { body: databaseDefinition } = await client.database(databaseId).read();
      console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }
   ```

3. Skopiuj i wklej kod poniżej miejsca ustawienia funkcji **createDatabase** i **readDatabase**, aby dodać funkcję pomocnika **exit**, która będzie wyświetlać komunikat zakończenia. 

   ```javascript
   // ADD THIS PART TO YOUR CODE
   function exit(message) {
      console.log(message);
      console.log('Press any key to exit');
      process.stdin.setRawMode(true);
      process.stdin.resume();
      process.stdin.on('data', process.exit.bind(process, 0));
   };
   ```

4. Skopiuj i wklej kod poniżej miejsca ustawienia funkcji **exit** w celu wywołania funkcji **createDatabase** i **readDatabase**.

   ```javascript
   createDatabase()
     .then(() => readDatabase())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error \${JSON.stringify(error)}`) });
   ```

   Na tym etapie kod w pliku ```app.js``` powinien wyglądać następująco:

   ```javascript
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');

   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;
   const partitionKey = { kind: "Hash", paths: ["/Country"] };

    /**
    * Create the database if it does not exist
    */
    async function createDatabase() {
     const { database } = await client.databases.createIfNotExists({ id: databaseId });
     console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
     const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }

   /**
   * Exit the app with a prompt
   * @param {message} message - The message to display
   */
   function exit(message) {
     console.log(message);
     console.log('Press any key to exit');
     process.stdin.setRawMode(true);
     process.stdin.resume();
     process.stdin.on('data', process.exit.bind(process, 0));
   }

   createDatabase()
     .then(() => readDatabase())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error) }`) });
   ```

5. W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie: 

   ```bash 
   node app.js
   ```

## <a id="CreateContainer"></a>Tworzenie kontenera

Następnie utwórz kontener w ramach konta usługi Azure Cosmos DB, aby móc przechowywać dane i wykonywać dotyczące ich zapytania. 

> [!WARNING]
> Utworzenie kontenera ma implikacje cenowe. Odwiedź naszą [stronę cennika](https://azure.microsoft.com/pricing/details/cosmos-db/), aby wiedzieć, czego oczekiwać.

Kontener można utworzyć przy użyciu funkcji `createIfNotExists` lub funkcji create z klasy **Containers**.  Kontener składa się z elementów (w przypadku interfejsu API SQL są to dokumenty JSON) i skojarzonej logiki aplikacji JavaScript.

1. Skopiuj i wklej funkcje **createContainer** i **readContainer** poniżej funkcji **readDatabase** w pliku app.js. Funkcja **createContainer** utworzy nowy kontener (jeśli jeszcze nie istnieje) o identyfikatorze ```containerId```określonym na podstawie obiektu ```config```. Funkcja **readContainer** odczyta definicję kontenera, aby sprawdzić, czy kontener istnieje.

   ```javascript
   /**
   * Create the container if it does not exist
   */

   async function createContainer() {

    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId, partitionKey }, { offerThroughput: 400 });
    console.log(`Created container:\n${config.container.id}\n`);
   }

   /**
    * Read the container definition
   */
   async function readContainer() {
      const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
   }
   ```

1. Skopiuj i wklej kod poniżej wywołania funkcji **readDatabase**, aby wykonać funkcje **createContainer** i **readContainer**.

   ```javascript
   createDatabase()
     .then(() => readDatabase())

     // ADD THIS PART TO YOUR CODE
     .then(() => createContainer())
     .then(() => readContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

   Na tym etapie kod w pliku ```app.js``` powinien wyglądać następująco:

   ```javascript
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');

   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;
   const partitionKey = { kind: "Hash", paths: ["/Country"] };

   /**
   * Create the database if it does not exist
   */
   async function createDatabase() {
     const { database } = await client.databases.createIfNotExists({ id: databaseId });
     console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
     const { body: databaseDefinition } = await client.database(databaseId).read();
     console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }

   /**
   * Create the container if it does not exist
   */

   async function createContainer() {

    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId, partitionKey }, { offerThroughput: 400 });
    console.log(`Created container:\n${config.container.id}\n`);
   }

   /**
    * Read the container definition
   */
   async function readContainer() {
      const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
   }

   /**
   * Exit the app with a prompt
   * @param {message} message - The message to display
   */
   function exit(message) {
     console.log(message);
     console.log('Press any key to exit');
     process.stdin.setRawMode(true);
     process.stdin.resume();
     process.stdin.on('data', process.exit.bind(process, 0));
   }

   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie: 

   ```bash 
   node app.js
   ```

## <a id="CreateItem"></a>Tworzenie elementu

Element można utworzyć za pomocą funkcji create klasy **Items**. Gdy korzystasz z interfejsu API SQL, elementy są przekazywane jako dokumenty, które stanowią zdefiniowaną przez użytkownika (dowolną) zawartość JSON. Teraz można wstawić element do bazy danych Azure Cosmos DB.

1. Skopiuj i wklej funkcję **createFamilyItem** poniżej funkcji **readContainer**. Funkcja **createFamilyItem** służy do tworzenia elementów zawierających dane JSON zapisane w obiekcie ```config```. Przed utworzeniem wykonamy sprawdzenie w celu sprawdzenia, czy taki sam identyfikator już nie istnieje.

   ```javascript
   /**
   * Create family item
   */
   async function createFamilyItem(itemBody) {
      const { item } = await client.database(databaseId).container(containerId).items.upsert(itemBody);
      console.log(`Created family item with id:\n${itemBody.id}\n`);
   };
   ```

1. Skopiuj i wklej kod poniżej wywołania funkcji **readContainer**, aby wykonać funkcję **createFamilyItem**.

   ```javascript
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())

     // ADD THIS PART TO YOUR CODE
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie: 

   ```bash 
   node app.js
   ```


## <a id="Query"></a>Wykonywanie zapytania względem zasobów usługi Azure Cosmos DB

Usługa Azure Cosmos DB obsługuje zaawansowane zapytania względem dokumentów JSON przechowywanych w poszczególnych kontenerach. Następujący przykładowy kod przedstawia zapytanie, które można uruchomić względem dokumentów w kontenerze.

1. Skopiuj i wklej funkcję **queryContainer** poniżej funkcji **createFamilyItem** w pliku app.js. Usługa Azure Cosmos DB obsługuje zapytania podobne do zapytań SQL, jak pokazano poniżej.

   ```javascript
   /**
   * Query the container using SQL
    */
   async function queryContainer() {
     console.log(`Querying container:\n${config.container.id}`);

     // query to return all children in a family
     const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec, {enableCrossPartitionQuery:true}).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
   };
   ```

1. Skopiuj i wklej kod poniżej wywołań funkcji **createFamilyItem**, aby wykonać funkcję **queryContainer**.

   ```javascript
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))

     // ADD THIS PART TO YOUR CODE
     .then(() => queryContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie:

   ```bash 
   node app.js
   ```


## <a id="ReplaceItem"></a>Zastępowanie elementu
Usługa Azure Cosmos DB obsługuje zastępowanie zawartości elementów.

1. Skopiuj i wklej funkcję **replaceFamilyItem** poniżej funkcji **queryContainer** w pliku app.js. Należy pamiętać, że właściwość „grade” elementu podrzędnego została zmieniona z poprzedniej wartości 5 na wartość 6.

   ```javascript
   // ADD THIS PART TO YOUR CODE
   /**
   * Replace the item by ID.
   */
   async function replaceFamilyItem(itemBody) {
      console.log(`Replacing item:\n${itemBody.id}\n`);
      // Change property 'grade'
      itemBody.children[0].grade = 6;
     const { item } = await client.database(databaseId).container(containerId).item(itemBody.id, itemBody.Country).replace(itemBody);
   };
   ```

1. Skopiuj i wklej kod poniżej wywołania funkcji **queryContainer**, aby wykonać funkcję **replaceFamilyItem**. Ponadto dodaj kod w celu ponownego wywołania funkcji **queryContainer**, aby sprawdzić, czy element został pomyślnie zmieniony.

   ```javascript
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))
     .then(() => queryContainer())

     // ADD THIS PART TO YOUR CODE
     .then(() => replaceFamilyItem(config.items.Andersen))
     .then(() => queryContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie:

   ```bash 
   node app.js
   ```


## <a id="DeleteItem"></a>Usuwanie elementu

Usługa Azure Cosmos DB obsługuje usuwanie elementów JSON.

1. Skopiuj i wklej funkcję **deleteFamilyItem** poniżej funkcji **replaceFamilyItem**.

   ```javascript
   /**
   * Delete the item by ID.
   */
   async function deleteFamilyItem(itemBody) {
     await client.database(databaseId).container(containerId).item(itemBody.id, itemBody.Country).delete(itemBody);
      console.log(`Deleted item:\n${itemBody.id}\n`);
   };
   ```

1. Skopiuj i wklej kod poniżej wywołania funkcji **queryContainer**, aby wykonać funkcję **deleteFamilyItem**.

   ```javascript
   createDatabase()
      .then(() => readDatabase())
      .then(() => createContainer())
      .then(() => readContainer())
      .then(() => createFamilyItem(config.items.Andersen))
      .then(() => createFamilyItem(config.items.Wakefield))
      .then(() => queryContainer
      ())
      .then(() => replaceFamilyItem(config.items.Andersen))
      .then(() => queryContainer())

    // ADD THIS PART TO YOUR CODE
      .then(() => deleteFamilyItem(config.items.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie: 

   ```bash 
   node app.js
   ```


## <a id="DeleteDatabase"></a>Usuwanie bazy danych

Usunięcie utworzonej bazy danych spowoduje usunięcie bazy danych i wszystkich zasobów podrzędnych (kontenerów, elementów itd.).

1. Skopiuj poniższą funkcję **cleanup** i wklej ją poniżej funkcji **deleteFamilyItem**, aby usunąć bazę danych i wszystkie jej zasoby podrzędne.

   ```javascript
   /**
   * Cleanup the database and container on completion
   */
   async function cleanup() {
     await client.database(databaseId).delete();
   }
   ```

1. Skopiuj i wklej kod poniżej wywołania funkcji **deleteFamilyItem**, aby wykonać funkcję **cleanup**.

   ```javascript
   createDatabase()
      .then(() => readDatabase())
      .then(() => createContainer())
      .then(() => readContainer())
      .then(() => createFamilyItem(config.items.Andersen))
      .then(() => createFamilyItem(config.items.Wakefield))
      .then(() => queryContainer())
      .then(() => replaceFamilyItem(config.items.Andersen))
      .then(() => queryContainer())
      .then(() => deleteFamilyItem(config.items.Andersen))

      // ADD THIS PART TO YOUR CODE
      .then(() => cleanup())
      // ENDS HERE

      .then(() => { exit(`Completed successfully`); })
      .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

## <a id="Run"></a>Uruchamianie aplikacji Node.js

Cały kod powinien wyglądać następująco:

[!code-javascript[nodejs-get-started](~/cosmosdb-nodejs-get-started/app.js)]

W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie: 

```bash 
node app.js
```

Powinny zostać wyświetlone dane wyjściowe aplikacji rozpoczynania pracy. Dane wyjściowe powinny odpowiadać tekstowi przykładu poniżej.

   ```
    Created database:
    FamilyDatabase

    Reading database:
    FamilyDatabase

    Created container:
    FamilyContainer

    Reading container:
    FamilyContainer

    Created family item with id:
    Anderson.1

    Created family item with id:
    Wakefield.7

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing item:
    Anderson.1

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleted item:
    Anderson.1

    Completed successfully
    Press any key to exit
   ```

## <a id="GetSolution"></a>Pobieranie kompletnego rozwiązania samouczka środowiska Node.js 

Jeśli nie masz czasu na ukończenie tego samouczka lub po prostu chcesz pobrać kod, możesz uzyskać go w serwisie [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started ). 

Do uruchomienia rozwiązania z wprowadzeniem, które zawiera cały kod znajdujący się w tym artykule, będą potrzebne następujące elementy: 

* [Konto usługi Azure Cosmos DB][create-account]. 
* Rozwiązanie [z wprowadzeniem](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started) dostępne w witrynie GitHub. 

Zainstaluj zależności projektu za pomocą menedżera npm. Użyj następującego polecenia: 

* ```npm install``` 

Następnie w pliku ```config.js``` zaktualizuj wartości config.endpoint i config.primaryKey, zgodnie z opisem w sekcji [Krok 3. Ustawianie konfiguracji aplikacji](#Config).  

W terminalu znajdź swój plik ```app.js```, a następnie uruchom następujące polecenie:  

```bash  
node app.js 
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Możesz usunąć grupę zasobów, konto usługi Azure Cosmos DB oraz wszystkie powiązane zasoby, gdy nie będą już potrzebne. Aby to zrobić, wybierz grupę zasobów używaną w przypadku konta usługi Azure Cosmos DB, wybierz pozycję **Usuń**, a następnie potwierdź nazwę grupy zasobów, którą chcesz usunąć.

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Monitorowanie konta usługi Azure Cosmos DB](monitor-accounts.md)

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png
