---
title: Tworzenie aplikacji internetowej za pomocą interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB i zestawu SDK platformy .NET
description: Przykładowy kod programu .NET, którego można używać do nawiązywania połączeń i wykonywania zapytań przy użyciu interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB.
author: rimman
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/21/2019
ms.author: rimman
ms.openlocfilehash: 73caa57fe7e721d69091bfb6ee74f7d88baf1ba3
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65979093"
---
# <a name="quickstart-build-a-net-web-app-using-azure-cosmos-dbs-api-for-mongodb"></a>Szybki start: Tworzenie aplikacji internetowej platformy .NET przy użyciu interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB 

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki dystrybucji globalnej i możliwości skalowania poziomego w usłudze Cosmos DB możesz szybko tworzyć i badać za pomocą zapytań bazy danych dokumentów, par klucz/wartość oraz grafów. 

Ten przewodnik Szybki start opisuje sposób tworzenia konta usługi Cosmos przy użyciu [interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB](mongodb-introduction.md). Następnie utworzysz i wdrożysz aplikację internetową listy zadań, korzystając ze [sterownika .NET MongoDB](https://docs.mongodb.com/ecosystem/drivers/csharp/).

## <a name="prerequisites-to-run-the-sample-app"></a>Wymagania wstępne dotyczące uruchamiania przykładowej aplikacji

Aby uruchomić przykład, musisz mieć program [Visual Studio](https://www.visualstudio.com/downloads/) i ważne konto usługi Microsoft Azure Cosmos DB.

Jeśli nie masz jeszcze programu Visual Studio, Pobierz [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia zainstalowanym w ramach instalacji.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

Przykład opisany w tym artykule jest zgodny ze sterownikiem bazy danych MongoDB.Driver w wersji 2.6.1.

## <a name="clone-the-sample-app"></a>Klonowanie przykładowej aplikacji

Najpierw pobierz przykładową aplikację z witryny GitHub. 

1. Otwórz wiersz polecenia, utwórz nowy folder o nazwie git-samples, a następnie zamknij wiersz polecenia.

    ```bash
    md "C:\git-samples"
    ```

2. Otwórz okno terminalu usługi Git, na przykład git bash, i użyj polecenia `cd`, aby przejść do nowego folderu instalacji aplikacji przykładowej.

    ```bash
    cd "C:\git-samples"
    ```

3. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. To polecenie tworzy kopię przykładowej aplikacji na komputerze. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

Jeśli nie chcesz korzystać z usługi Git, możesz [pobrać projekt jako plik ZIP](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started/archive/master.zip).

## <a name="review-the-code"></a>Przeglądanie kodu

Ten krok jest opcjonalny. Jeśli chcesz dowiedzieć się, jak zasoby bazy danych są tworzone w kodzie, możesz przejrzeć poniższe fragmenty kodu. W przeciwnym razie możesz od razu przejść do sekcji [Aktualizowanie parametrów połączenia](#update-your-connection-string). 

Wszystkie poniższe fragmenty kodu pochodzą z pliku Dal.cs znajdującego się w katalogu DAL.

* Inicjowanie klienta.

    ```cs
        MongoClientSettings settings = new MongoClientSettings();
        settings.Server = new MongoServerAddress(host, 10255);
        settings.UseSsl = true;
        settings.SslSettings = new SslSettings();
        settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

        MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
        MongoIdentityEvidence evidence = new PasswordEvidence(password);

        settings.Credential = new MongoCredential("SCRAM-SHA-1", identity, evidence);

        MongoClient client = new MongoClient(settings);
    ```

* Pobieranie bazy danych i kolekcji.

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* Pobieranie wszystkich dokumentów.

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

Tworzenie zadania i wstawianie go do kolekcji

   ```csharp
    public void CreateTask(MyTask task)
    {
        var collection = GetTasksCollectionForEdit();
        try
        {
            collection.InsertOne(task);
        }
        catch (MongoCommandException ex)
        {
            string msg = ex.Message;
        }
    }
   ```
   Podobnie możesz aktualizować i usuwać dokumenty za pomocą metod [collection.UpdateOne()](https://docs.mongodb.com/stitch/mongodb/actions/collection.updateOne/index.html) i [collection.DeleteOne()](https://docs.mongodb.com/stitch/mongodb/actions/collection.deleteOne/index.html). 

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji.

1. W witrynie [Azure Portal](https://portal.azure.com/), korzystając ze swojego konta usługi Cosmos, kliknij w lewym panelu nawigacyjnym pozycję **Parametry połączenia**, a następnie pozycję **Klucze odczytu i zapisu**. W następnym kroku, korzystając z przycisków kopiowania dostępnych po prawej stronie ekranu, skopiujesz nazwę użytkownika, hasło i hosta do pliku Dal.cs.

2. Otwórz plik **Dal.cs** w katalogu **DAL**. 

3. Skopiuj wartość **username** z portalu (przy użyciu przycisku kopiowania) i przypisz ją do klucza **username** w Twoim pliku **Dal.cs**. 

4. Następnie skopiuj wartość **host** z portalu i przypisz ją do klucza **host** w Twoim pliku **Dal.cs**. 

5. Na końcu skopiuj wartość **password** z portalu i przypisz ją do klucza **password** w Twoim pliku **Dal.cs**. 

Aplikacja została zaktualizowana i zawiera teraz wszystkie informacje potrzebne do nawiązania komunikacji z usługą Cosmos DB. 
    
## <a name="run-the-web-app"></a>Uruchamianie aplikacji sieci web

1. W programie Visual Studio kliknij projekt prawym przyciskiem myszy w **Eksploratorze rozwiązań**, a następnie kliknij polecenie **Zarządzaj pakietami NuGet**. 

2. W polu **Przeglądaj** obszaru pakietów NuGet wpisz ciąg *MongoDB.Driver*.

3. Korzystając z wyników, zainstaluj bibliotekę **MongoDB.Driver**. Spowoduje to zainstalowanie pakietu MongoDB.Driver, a także wszystkich zależności.

4. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Aplikacja zostanie wyświetlona w przeglądarce. 

5. Kliknij pozycję **Utwórz** w przeglądarce i utwórz kilka nowych zadań w swojej aplikacji listy zadań.

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Kolejne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Cosmos, tworzenia kolekcji i uruchamiania aplikacji konsolowej. Teraz możesz zaimportować dodatkowe dane do swojej bazy danych usługi Cosmos. 

> [!div class="nextstepaction"]
> [Importowanie danych z bazy danych MongoDB do usługi Azure Cosmos DB](mongodb-migrate.md)
