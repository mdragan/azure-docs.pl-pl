---
title: Usługa Azure Cosmos DB tworzenie aplikacji z listą zadań do wykonania za pomocą oprogramowania Xamarin
description: Przykładowy kod Xamarin, którego można używać do nawiązywania połączeń z usługą Azure Cosmos DB i wykonywania w niej zapytań
author: codemillmatt
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/30/2018
ms.author: masoucou
ms.openlocfilehash: e0d439edc35e70bc8ac477cf56a6bc25e41df083
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754728"
---
# <a name="quickstart-build-a-todo-app-with-xamarin-using-azure-cosmos-db-sql-api-account"></a>Szybki start: Tworzenie aplikacji z listą zadań do wykonania za pomocą platformy Xamarin przy użyciu konta interfejsu API SQL usługi Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](create-sql-api-dotnet.md)
> * [.NET (wersja zapoznawcza)](create-sql-api-dotnet-preview.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)
>  

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki dystrybucji globalnej i możliwości skalowania poziomego w usłudze Azure Cosmos DB możesz szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość oraz grafów.

> [!NOTE]
> Przykładowy kod dla całej kanonicznej przykładowej aplikacji platformy Xamarin z wieloma ofertami platformy Azure, w tym usługą CosmosDB, można znaleźć w witrynie GitHub [tutaj](https://github.com/xamarinhq/app-geocontacts). Ta aplikacja demonstruje wyświetlanie geograficznie rozproszonych kontaktów i umożliwianie tym kontaktom zaktualizowanie ich lokalizacji.

Ten przewodnik Szybki start przedstawia sposób tworzenia konta interfejsu SQL API usługi Azure Cosmos DB, bazy danych dokumentów i kolekcji przy użyciu witryny Azure Portal. Utworzysz w nim i wdrożysz aplikację internetową z listą zadań do wykonania opartą na [interfejsie SQL .NET API](sql-api-sdk-dotnet.md) i [oprogramowaniu Xamarin](https://docs.microsoft.com/xamarin/) przy użyciu [platformy Xamarin.Forms](https://docs.microsoft.com/xamarin/) i [wzorca architektury MVVM](https://docs.microsoft.com/xamarin/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm).

![Aplikacji Xamarin z listą zadań do wykonania działająca w systemie iOS](./media/create-sql-api-xamarin-dotnet/ios-todo-screen.png)

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli programujesz w Windows, a jeszcze nie masz programu Visual Studio 2019 r zainstalowany, możesz pobrać i używać **bezpłatne** [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/). Podczas instalacji programu Visual Studio upewnij się, że włączono pakiety robocze **Programowanie na platformie Azure** i **Opracowywanie aplikacji mobilnych za pomocą środowiska .NET**.

Jeśli używasz komputera Mac, możesz pobrać **bezpłatny** program [Visual Studio dla komputerów Mac](https://www.visualstudio.com/vs/mac/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>Dodawanie kontenera

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="add-sample-data"></a>Dodawanie danych przykładowych

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Wysyłanie zapytań dotyczących danych

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz sklonujemy aplikację interfejsu Xamarin SQL API z repozytorium GitHub, przejrzymy kod, uzyskamy klucze interfejsu API i uruchomimy aplikację. Zobaczysz, jak łatwo jest pracować programowo z danymi.

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
    git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-xamarin-getting-started.git
    ```

4. Następnie otwórz plik ToDoItems.sln z folderu samples/xamarin/ToDoItems w programie Visual Studio.

## <a name="obtain-your-api-keys"></a>Uzyskiwanie kluczy interfejsu API

Wróć do witryny Azure Portal, aby uzyskać informacje o kluczach interfejsu API i skopiować je do aplikacji.

1. W witrynie [Azure Portal](https://portal.azure.com/), korzystając ze swojego konta interfejsu API SQL usługi Azure Cosmos DB, kliknij na lewym panelu nawigacyjnym pozycję **Klucze**, a następnie pozycję **Klucze odczytu i zapisu**. W następnym kroku, korzystając z przycisków kopiowania dostępnych po prawej stronie ekranu, skopiujesz identyfikator URI i klucz podstawowy do pliku APIKeys.cs.

    ![Wyświetlanie i kopiowanie klucza dostępu w witrynie Azure Portal, blok Klucze](./media/create-sql-api-xamarin-dotnet/keys.png)

2. W Visual Studio 2019 lub Visual Studio dla komputerów Mac Otwórz plik APIKeys.cs w folderze azure-documentdb-dotnet/samples/xamarin/ToDoItems/ToDoItems.Core/Helpers.

3. Skopiuj wartość identyfikatora URI z portalu (przy użyciu przycisku kopiowania) i przypisz ją do zmiennej `CosmosEndpointUrl` w pliku APIKeys.cs.

    `public static readonly string CosmosEndpointUrl = "{Azure Cosmos DB account URL}";`

4. Następnie skopiuj wartość klucza podstawowego z portalu i przypisz ją do zmiennej `Cosmos Auth Key` w pliku APIKeys.cs.

    `public static readonly string CosmosAuthKey = "{Azure Cosmos DB secret}";`

[!INCLUDE [cosmos-db-auth-key-info](../../includes/cosmos-db-auth-key-info.md)]

## <a name="review-the-code"></a>Przeglądanie kodu

To rozwiązanie przedstawia sposób tworzenia aplikacji z listą zadań do wykonania przy użyciu interfejsu API SQL usługi Azure Cosmos DB i platformy Xamarin.Forms. Aplikacja ma dwie karty. Na pierwszej z nich jest widok listy zadań, które nie zostały jeszcze wykonane. Na drugiej karcie są wyświetlane zadania, które zostały już wykonane. Oprócz wyświetlania na pierwszej karcie zadań, które nie zostały jeszcze wykonane, można również dodawać nowe zadania do wykonania, edytować istniejące zadania i oznaczać zadania jako wykonane.

![Kopiowanie danych json i klikanie pozycji Zapisz w Eksploratorze danych w witrynie Azure Portal](./media/create-sql-api-xamarin-dotnet/android-todo-screen.png)

Kod w rozwiązaniu ToDoItems zawiera następujące elementy:

* ToDoItems.Core: jest to projekt .NET Standard zawierający projekt platformy Xamarin.Forms i współużytkowany kod logiki aplikacji obsługujący zadania do wykonania w usłudze Azure Cosmos DB.
* ToDoItems.Android: ten projekt zawiera aplikację dla systemu Android.
* ToDoItems.iOS: ten projekt zawiera aplikację dla systemu iOS.

Teraz dokonamy szybkiego przeglądu sposobu komunikowania się aplikacji z usługą Azure Cosmos DB.

* Wymagane jest dodanie pakietu NuGet [Microsoft.Azure.DocumentDb.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/) do wszystkich projektów.
* Klasa `ToDoItem` znajdująca się w folderze azure-documentdb-dotnet/samples/xamarin/ToDoItems/ToDoItems.Core/Models modeluje dokumenty w utworzonej wcześniej kolekcji **Items**. Należy pamiętać, że w nazwach właściwości jest rozróżniana wielkość liter.
* Klasa `CosmosDBService` znajdująca się w folderze azure-documentdb-dotnet/samples/xamarin/ToDoItems/ToDoItems.Core/Services hermetyzuje komunikację z usługą Azure Cosmos DB.
* Klasa `CosmosDBService` zawiera zmienną typu `DocumentClient`. Zmienna `DocumentClient` służy do konfigurowania i wykonywania żądań względem konta usługi Azure Cosmos DB. Jej wystąpienie jest tworzone w wierszu 31:

    ```csharp
    docClient = new DocumentClient(new Uri(APIKeys.CosmosEndpointUrl), APIKeys.CosmosAuthKey);
    ```

* Do wykonywania zapytań o dokumenty w kolekcji używana jest metoda `DocumentClient.CreateDocumentQuery<T>`, którą przedstawiono tutaj w funkcji `CosmosDBService.GetToDoItems`:

    ```csharp
    public async static Task<List<ToDoItem>> GetToDoItems()
    {
        var todos = new List<ToDoItem>();

        var todoQuery = docClient.CreateDocumentQuery<ToDoItem>(
                                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                                .Where(todo => todo.Completed == false)
                                .AsDocumentQuery();

        while (todoQuery.HasMoreResults)
        {
            var queryResults = await todoQuery.ExecuteNextAsync<ToDoItem>();

            todos.AddRange(queryResults);
        }

        return todos;
    }
    ```

    Funkcja `CreateDocumentQuery<T>` pobiera identyfikator URI, który wskazuje kolekcję utworzoną w poprzedniej sekcji. Można również określić operatory LINQ, na przykład klauzulę `Where`. W tym przypadku zwracane są tylko te zadania, które nie zostały jeszcze wykonane.

    Funkcja `CreateDocumentQuery<T>` jest wykonywana synchronicznie i zwraca obiekt `IQueryable<T>`. Jednak metoda `AsDocumentQuery` konwertuje obiekt `IQueryable<T>` do obiektu `IDocumentQuery<T>`, który może być wykonywany asynchronicznie. Dzięki temu wątek interfejsu użytkownika nie jest blokowany w aplikacjach mobilnych.

    Funkcja `IDocumentQuery<T>.ExecuteNextAsync<T>` pobiera stronę wyników z usługi Azure Cosmos DB, której funkcja `HasMoreResults` sprawdza, czy będą jeszcze zwracane dodatkowe wyniki.

> [!TIP]
> Niektóre funkcje obsługujące kolekcje i dokumenty usługi Azure Cosmos DB używają identyfikatora URI jako parametru określającego adres kolekcji lub dokumentu. Ten identyfikator URI jest tworzony przy użyciu klasy `URIFactory`. Za pomocą tej klasy można tworzyć wszystkie identyfikatory URI baz danych, kolekcji i dokumentów.

* Na przykładzie funkcji `ComsmosDBService.InsertToDoItem` w wierszu 107 przedstawiono sposób wstawiania nowego dokumentu:

    ```csharp
    public async static Task InsertToDoItem(ToDoItem item)
    {
        ...
        await docClient.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), item);
        ...
    }
    ```

    Określany jest tu zarówno identyfikator URI kolekcji dokumentów, jak również zadanie do wstawienia.

* Na przykładzie funkcji `CosmosDBService.UpdateToDoItem` w wierszu 124 pokazano, jak zastąpić istniejący dokument nowym:

    ```csharp
    public async static Task UpdateToDoItem(ToDoItem item)
    {
        ...
        var docUri = UriFactory.CreateDocumentUri(databaseName, collectionName, item.Id);

        await docClient.ReplaceDocumentAsync(docUri, item);
    }
    ```

    W tym miejscu nowy identyfikator URI jest niezbędne do jednoznacznego zidentyfikowania dokumentu do zastąpienia jest on uzyskiwany za pomocą `UriFactory.CreateDocumentUri` i przekazanie do niej nazwy bazy danych i kolekcji oraz Identyfikatora dokumentu.

    Funkcja `DocumentClient.ReplaceDocumentAsync` zastępuje dokument identyfikowany przez identyfikator URI dokumentem określonym jako parametr.

* Usuwanie zadania przedstawiono na przykładzie funkcji `CosmosDBService.DeleteToDoItem` w wierszu 115:

    ```csharp
    public async static Task DeleteToDoItem(ToDoItem item)
    {
        ...
        var docUri = UriFactory.CreateDocumentUri(databaseName, collectionName, item.Id);

        await docClient.DeleteDocumentAsync(docUri);
    }
    ```

    Ponownie zanotuj utworzony i przekazany do funkcji `DocumentClient.DeleteDocumentAsync` unikatowy identyfikator URI dokumentu.

## <a name="run-the-app"></a>Uruchamianie aplikacji

Aplikacja została zaktualizowana i zawiera teraz wszystkie informacje potrzebne do nawiązania komunikacji z usługą Azure Cosmos DB.

W poniższych krokach zademonstrowano sposób uruchamiania aplikacji przy użyciu debugera programu Visual Studio dla komputerów Mac.

> [!NOTE]
> Sposób użycia aplikacji w wersji dla systemu Android jest dokładnie taki sam. Ewentualne różnice zostaną przestawione w poniższych krokach. Jeśli wolisz debugować tę aplikację w programie Visual Studio w systemie Windows, [tutaj można znaleźć dokumentację dla systemu iOS](https://docs.microsoft.com/xamarin/ios/deploy-test/debugging-in-xamarin-ios?tabs=vswin), a [tutaj dla systemu Android](https://docs.microsoft.com/xamarin/android/deploy-test/debugging/).

1. Najpierw wybierz platformę docelową, klikając wyróżnioną listę rozwijaną i wybierając pozycję ToDoItems.iOS dla systemu iOS lub pozycję ToDoItems.Android dla systemu Android.

    ![Wybieranie platformy do debugowania w programie Visual Studio dla komputerów Mac](./media/create-sql-api-xamarin-dotnet/ide-select-platform.png)

2. Aby rozpocząć debugowanie aplikacji, naciśnij klawisze cmd+Enter lub kliknij przycisk odtwarzania.

    ![Uruchamianie na potrzeby debugowania w programie Visual Studio dla komputerów Mac](./media/create-sql-api-xamarin-dotnet/ide-start-debug.png)

3. Po uruchomieniu symulatora systemu iOS lub emulatora systemu Android w aplikacji zostaną wyświetlone 2 karty u dołu ekranu (w przypadku systemu iOS) lub w górnej części ekranu (w przypadku systemu Android). Na pierwszej karcie są wyświetlane zadania do wykonania, a na drugiej zadania już wykonane.

    ![Ekran z uruchomioną aplikacją z listą zadań do wykonania](./media/create-sql-api-xamarin-dotnet/ios-droid-started.png)

4. Aby oznaczyć zadanie jako wykonane w systemie iOS, przesuń zadanie w lewą stronę i naciśnij przycisk **Complete** (Wykonane). Aby oznaczyć zadanie jako wykonane w systemie Android, naciśnij je i przytrzymaj, a następnie naciśnij przycisk Complete (Wykonane).

    ![Oznaczanie zadania jako wykonane](./media/create-sql-api-xamarin-dotnet/simulator-complete.png)

5. Aby edytować zadanie do wykonania, naciśnij je. Zostanie wyświetlony nowy ekran umożliwiający wprowadzenie nowych wartości. Po naciśnięciu przycisku zapisywania zmiany zostaną utrwalone w usłudze Azure Cosmos DB.

    ![Edytowanie zadania do wykonania](./media/create-sql-api-xamarin-dotnet/simulator-edit.png)

6. Aby dodać zadanie do wykonania, naciśnij przycisk **Add** (Dodaj) w prawym górnym rogu ekranu głównego. Zostanie wyświetlona nowa pusta strona edycji.

    ![Dodawanie zadania do wykonania](./media/create-sql-api-xamarin-dotnet/simulator-add.png)

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Kolejne kroki

W tym przewodniku Szybki Start wyjaśniono sposób tworzenia konta usługi Azure Cosmos, Utwórz kontener za pomocą Eksploratora danych i tworzenie i wdrażanie aplikacji platformy Xamarin. Możesz teraz zaimportować dodatkowe dane do swojego konta usługi Azure Cosmos.

> [!div class="nextstepaction"]
> [Importowanie danych do usługi Azure Cosmos DB](import-data.md)
