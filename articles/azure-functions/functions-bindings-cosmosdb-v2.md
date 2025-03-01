---
title: Azure Cosmos DB powiązania funkcji 2.x
description: Dowiedz się, jak używać usługi Azure Cosmos DB, wyzwalaczy i powiązań w usłudze Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: usługi Azure functions, funkcje, przetwarzanie zdarzeń, obliczanie dynamiczne, architektura bez serwera
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/21/2017
ms.author: cshoe
ms.openlocfilehash: 4274c695779d25034b4277ac5386b6beee242cca
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342282"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-2x"></a>Azure Cosmos DB powiązania usługi Azure Functions 2.x

> [!div class="op_single_selector" title1="Wybierz wersję środowiska uruchomieniowego usługi Azure Functions, którego używasz: "]
> * [Wersja 1](functions-bindings-cosmosdb.md)
> * [Wersja 2](functions-bindings-cosmosdb-v2.md)

W tym artykule wyjaśniono, jak pracować z [usługi Azure Cosmos DB](../cosmos-db/serverless-computing-database.md) powiązań w usłudze Azure Functions 2.x. Usługi Azure Functions obsługuje wyzwalanie, dane wejściowe i wyjściowe powiązań usługi Azure Cosmos DB.

> [!NOTE]
> Ten artykuł jest przeznaczony dla [usługi Azure Functions w wersji 2.x](functions-versions.md).  Aby uzyskać informacje o sposobie używania tych powiązań w funkcjach 1.x, zobacz [powiązań usługi Azure Cosmos DB dla usługi Azure Functions 1.x](functions-bindings-cosmosdb.md).
>
> To powiązanie pierwotnie nosiła nazwę bazy danych DocumentDB. W funkcji w wersji 2.x, wyzwalacz, powiązania i pakietu są wszystkie o nazwie usługi Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-apis"></a>Obsługiwane interfejsy API

[!INCLUDE [SQL API support only](../../includes/functions-cosmosdb-sqlapi-note.md)]

## <a name="packages---functions-2x"></a>Pakiety — funkcje 2.x

Powiązania usługi Azure Cosmos DB dla funkcji w wersji 2.x znajdują się w [Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB) pakietu NuGet w wersji 3.x. Kod źródłowy dla powiązania znajduje się w [zestawu sdk rozszerzenia, usługi azure webjobs w-](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/) repozytorium GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Wyzwalacz

Wyzwalacz usługi Azure Cosmos DB używa [usługi Azure Cosmos DB kanału informacyjnego zmian](../cosmos-db/change-feed.md) do nasłuchiwania pod kątem operacji wstawienia i aktualizacje w różnych partycjach. Kanał informacyjny zmian publikuje wstawienia i aktualizacje, nie do usunięcia.

## <a name="trigger---example"></a>Wyzwalacz — przykład

Zobacz przykład specyficzny dla języka:

* [C#](#trigger---c-example)
* [Skryptu C# (csx)](#trigger---c-script-example)
* [Java](#trigger---java-example)
* [JavaScript](#trigger---javascript-example)
* [Python](#trigger---python-example)

Pomiń przykłady wyzwalacza

### <a name="trigger---c-example"></a>Wyzwalacz — przykład w języku C#

W poniższym przykładzie przedstawiono [funkcja języka C#](functions-dotnet-class-library.md) , wywoływany, gdy istnieją wstawia lub aktualizuje w określonej bazy danych i kolekcji.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents, 
            ILogger log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.LogInformation($"Documents modified: {documents.Count}");
                log.LogInformation($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

Pomiń przykłady wyzwalacza

### <a name="trigger---c-script-example"></a>Wyzwalacz — przykładowy skrypt w języku C#

W poniższym przykładzie pokazano wyzwalacz usługi Cosmos DB, powiązania w *function.json* pliku i [funkcji skryptu w języku C#](functions-reference-csharp.md) powiązania, który używa. Funkcja zapisuje komunikaty dziennika, po zmodyfikowaniu rekordy usługi Cosmos DB.

Oto powiązanie danych w *function.json* pliku:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Poniżej przedstawiono kod skryptu języka C#:

```cs
    #r "Microsoft.Azure.DocumentDB.Core"

    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    using Microsoft.Extensions.Logging;

    public static void Run(IReadOnlyList<Document> documents, ILogger log)
    {
      log.LogInformation("Documents modified " + documents.Count);
      log.LogInformation("First document Id " + documents[0].Id);
    }
```

Pomiń przykłady wyzwalacza

### <a name="trigger---javascript-example"></a>Wyzwalacz — przykład JavaScript

W poniższym przykładzie pokazano wyzwalacz usługi Cosmos DB, powiązania w *function.json* pliku i [funkcji JavaScript](functions-reference-node.md) powiązania, który używa. Funkcja zapisuje komunikaty dziennika, po zmodyfikowaniu rekordy usługi Cosmos DB.

Oto powiązanie danych w *function.json* pliku:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Poniżej przedstawiono kod JavaScript:

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

### <a name="trigger---java-example"></a>Wyzwalacz - przykładzie w języku Java

W poniższym przykładzie pokazano wyzwalacz usługi Cosmos DB, powiązania w *function.json* pliku i [funkcja Java](functions-reference-java.md) powiązania, który używa. Funkcja jest zaangażowane w przypadku wstawienia lub aktualizacji w określonej bazy danych i kolekcji.

```json
{
    "type": "cosmosDBTrigger",
    "name": "items",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "AzureCosmosDBConnection",
    "databaseName": "ToDoList",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": false
}
```

Oto kodu Java:

```java
    @FunctionName("cosmosDBMonitor")
    public void cosmosDbProcessor(
        @CosmosDBTrigger(name = "items",
            databaseName = "ToDoList",
            collectionName = "Items",
            leaseCollectionName = "leases",
            createLeaseCollectionIfNotExists = true,
            connectionStringSetting = "AzureCosmosDBConnection") String[] items,
            final ExecutionContext context ) {
                context.getLogger().info(items.length + "item(s) is/are changed.");
            }
```


W [Java funkcje biblioteki środowiska uruchomieniowego](/java/api/overview/azure/functions/runtime), użyj `@CosmosDBTrigger` adnotacji w parametrach, którego wartość może pochodzić z usługi Cosmos DB.  Ta adnotacja mogą być używane z natywnych typów języka Java, obiektów typu Pojo lub wartości dopuszczających wartości null przy użyciu opcjonalnie<T>.


Pomiń przykłady wyzwalacza

### <a name="trigger---python-example"></a>Wyzwalacz — przykładem w języku Python

W poniższym przykładzie pokazano wyzwalacz usługi Cosmos DB, powiązania w *function.json* pliku i [funkce Pythonu](functions-reference-python.md) powiązania, który używa. Funkcja zapisuje komunikaty dziennika, po zmodyfikowaniu rekordy usługi Cosmos DB.

Oto powiązanie danych w *function.json* pliku:

```json
{
    "name": "documents",
    "type": "cosmosDBTrigger",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Poniżej przedstawiono kod języka Python:

```python
    import logging
    import azure.functions as func


    def main(documents: func.DocumentList) -> str:
        if documents:
            logging.info('First document Id modified: %s', documents[0]['id'])
```

## <a name="trigger---c-attributes"></a>Wyzwalacz — C# atrybutów

W [bibliotek klas języka C#](functions-dotnet-class-library.md), użyj [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs) atrybutu.

Konstruktor atrybutu ma nazwę bazy danych i nazwę kolekcji. Aby uzyskać informacje o tych ustawieniach i inne właściwości, które można skonfigurować, zobacz [wyzwalacza — Konfiguracja](#trigger---configuration). Oto `CosmosDBTrigger` przykład atrybutu w podpisie metody:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        ILogger log)
    {
        ...
    }
```

Aby uzyskać kompletny przykład, zobacz [wyzwalacza — przykład w języku C#](#trigger---c-example).


## <a name="trigger---configuration"></a>Wyzwalacz — Konfiguracja

W poniższej tabeli opisano właściwości konfiguracji powiązania, które można ustawić w *function.json* pliku i `CosmosDBTrigger` atrybutu.

|Właściwość Function.JSON | Właściwość atrybutu |Opis|
|---------|---------|----------------------|
|**type** || Musi być równa `cosmosDBTrigger`. |
|**direction** || Musi być równa `in`. Ten parametr jest ustawiany automatycznie po utworzeniu wyzwalacza w witrynie Azure portal. |
|**name** || Nazwa zmiennej, używany w kodzie funkcji, który reprezentuje listę dokumentów za pomocą zmian. |
|**connectionStringSetting**|**ConnectionStringSetting** | Nazwa ustawienia aplikacji zawierającego parametry połączenia używane do łączenia z konta usługi Azure Cosmos DB są monitorowane. |
|**databaseName**|**DatabaseName**  | Nazwa bazy danych Azure Cosmos DB za pomocą kolekcji są monitorowane. |
|**collectionName** |**collectionName** | Nazwa kolekcji są monitorowane. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (Opcjonalnie) Nazwa ustawienia aplikacji zawierającego parametry połączenia z usługą, która zawiera kolekcję dzierżaw. Gdy nie są ustawione, `connectionStringSetting` wartość jest używana. Ten parametr jest automatycznie ustawiana podczas tworzenia powiązania w portalu. Parametry połączenia dla kolekcji dzierżaw musi mieć uprawnienia do zapisu.|
|**leaseDatabaseName** |**LeaseDatabaseName** | (Opcjonalnie) Nazwa bazy danych, który zawiera kolekcję używaną do przechowywania dzierżaw. Jeśli nie ustawiona, wartość `databaseName` ustawienie jest używane. Ten parametr jest automatycznie ustawiana podczas tworzenia powiązania w portalu. |
|**leaseCollectionName** | **LeaseCollectionName** | (Opcjonalnie) Nazwa kolekcji, używany do przechowywania dzierżaw. Kiedy nie ustawiona, wartość `leases` jest używany. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (Opcjonalnie) Po ustawieniu `true`, kolekcję dzierżaw zostało automatycznie utworzone po już nie istnieje. Wartość domyślna to `false`. |
|**leasesCollectionThroughput**| **leasesCollectionThroughput**| (Opcjonalnie) Definiuje liczbę jednostek żądania do przypisania, po utworzeniu kolekcji dzierżaw. To ustawienie jest tylko wtedy, gdy używane `createLeaseCollectionIfNotExists` ustawiono `true`. Ten parametr jest automatycznie ustawiana, jeśli wiązanie jest tworzony przy użyciu portalu.
|**leaseCollectionPrefix**| **leaseCollectionPrefix**| (Opcjonalnie) Po ustawieniu dodaje prefiks do dzierżawy utworzone w kolekcję dzierżaw dla tej funkcji, co skutecznie dwie oddzielne funkcje platformy Azure na udostępnianie tej samej kolekcji dzierżawy przy użyciu różnych prefiksów.
|**feedPollDelay**| **feedPollDelay**| (Opcjonalnie) Gdy zestaw, definiuje, w milisekundach, opóźnienie między sondowaniem partycji dla nowych zmian w źródle danych, gdy wszystkie bieżące zmiany są opróżniane. Wartością domyślną jest 5000 (5 sekund).
|**leaseAcquireInterval**| **leaseAcquireInterval**| (Opcjonalnie) Po ustawieniu definiuje, w milisekundach, interwał można uruchamiać zadania obliczeniowe, jeśli partycje są dystrybuowane równomiernie między wystąpieniami znanych hostów. Wartość domyślna to 13000 (w sekundach 13).
|**leaseExpirationInterval**| **leaseExpirationInterval**| (Opcjonalnie) Po ustawieniu definiuje, w milisekundach, interwał, dla której dzierżawy jest pobierany podczas dzierżawy, reprezentujący partycji. Jeśli dzierżawa nie zostanie odnowiony w tym przedziale czasu, spowoduje jego wygaśnięcia i własność partycji przejdzie do innego wystąpienia. Domyślna to 60 000 (60 sekund).
|**leaseRenewInterval**| **leaseRenewInterval**| (Opcjonalnie) Po ustawieniu definiuje, w milisekundach, interwał odnawiania dla wszystkich dzierżaw dla partycji aktualnie trzymana przez wystąpienie. Wartość domyślna to 17000 (17 w sekundach).
|**checkpointFrequency**| **checkpointFrequency**| (Opcjonalnie) Po ustawieniu definiuje, w milisekundach, interwał między punktami kontrolnymi dzierżawy. Wartością domyślną jest zawsze po każdym wywołaniu funkcji.
|**maxItemsPerInvocation**| **maxItemsPerInvocation**| (Opcjonalnie) Po ustawieniu dostosowuje maksymalna ilość elementów odebranych na wywołanie funkcji.
|**startFromBeginning**| **startFromBeginning**| (Opcjonalnie) Po ustawieniu informuje wyzwalacz, aby rozpocząć odczyt zmiany wprowadzone od początku historii kolekcja zamiast bieżącego czasu. Działa to tylko przy pierwszym uruchomieniu wyzwalacza, tak jak w kolejnych przebiegów, punkty kontrolne są już przechowywane. Ustawienie tej opcji na `true` w przypadku dzierżaw utworzone nie ma wpływu.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Wyzwalacz — użycie

Wyzwalacz wymaga druga kolekcja, która jest używana do przechowywania _dzierżawy_ poszczególnych partycjach. Kolekcja monitorowanych i kolekcji, która zawiera dzierżaw musi być dostępny dla wyzwalacza do pracy.

>[!IMPORTANT]
> Jeśli wiele funkcji są skonfigurowane do użycia wyzwalacz usługi Cosmos DB na potrzeby tej samej kolekcji, każda z tych funkcji należy używać kolekcji bezpiecznych dedykowanych dzierżawy lub określić inną `LeaseCollectionPrefix` dla każdej funkcji. W przeciwnym razie tylko jeden funkcje zostaną wyzwolone. Aby uzyskać informacji na temat prefiksu, zobacz [sekcji konfiguracji](#trigger---configuration).

Wyzwalacz nie wskazuje, czy zaktualizowane lub wstawić dokumentu, po prostu zapewnia samego dokumentu. Jeśli wymagana jest obsługa aktualizacje i wstawienia w różny sposób, możesz można zrobić, implementując pola sygnatury czasowej dla wstawiania lub aktualizacji.

## <a name="input"></a>Dane wejściowe

Powiązania danych wejściowych usługi Azure Cosmos DB używa interfejsu API SQL do pobrania jednego lub więcej dokumentów usługi Azure Cosmos DB i przekazuje je do parametr wejściowy funkcji. Można określić parametry identyfikator lub kwerendę dokumentu oparte na wyzwalacz, który wywołuje funkcję.

## <a name="input---examples"></a>Dane wejściowe — przykłady

Zobacz przykłady specyficzny dla języka, które odczytują pojedynczy dokument, określając wartość Identyfikatora:

* [C#](#input---c-examples)
* [Skryptu C# (csx)](#input---c-script-examples)
* [F#](#input---f-examples)
* [Java](#input---java-examples)
* [JavaScript](#input---javascript-examples)
* [Python](#input---python-examples)

[Pomiń przykłady danych wejściowych](#input---attributes)

### <a name="input---c-examples"></a>Dane wejściowe — przykłady w języku C#

Ta sekcja zawiera następujące przykłady:

* [Wyzwalacz kolejki, odnośnika identyfikator z formatu JSON](#queue-trigger-look-up-id-from-json-c)
* [Wyzwalacz HTTP, Sprawdź identyfikator z ciągu zapytania](#http-trigger-look-up-id-from-query-string-c)
* [Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy](#http-trigger-look-up-id-from-route-data-c)
* [Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy, używając SqlQuery](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [HTTP wyzwalacza, Uzyskaj dokumentację wielu, za pomocą SqlQuery](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [HTTP wyzwalacza, Uzyskaj dokumentację wielu, za pomocą DocumentClient](#http-trigger-get-multiple-docs-using-documentclient-c)

Przykłady dotyczą prostego `ToDoItem` typu:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-c"></a>Wyzwalacz kolejki, odnośnika identyfikator z formatu JSON (C#)

W poniższym przykładzie przedstawiono [funkcja języka C#](functions-dotnet-class-library.md) , który pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez komunikatu w kolejce, która zawiera obiekt JSON. Wyzwalacz kolejki analizuje dane JSON na obiekt o nazwie `ToDoItemLookup`, który zawiera identyfikator aby wyszukać. Identyfikator służy do pobierania `ToDoItem` dokument z określonej bazy danych i kolekcji.

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{ToDoItemId}")]ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId}");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c"></a>Wyzwalacz HTTP, Sprawdź identyfikator z ciągu zapytania (C#)

W poniższym przykładzie przedstawiono [funkcja języka C#](functions-dotnet-class-library.md) , który pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, która wykorzystuje ciąg kwerendy do określenia Identyfikatora do wyszukania. Identyfikator służy do pobierania `ToDoItem` dokument z określonej bazy danych i kolekcji.

>[!NOTE]
>Parametr ciągu zapytania HTTP jest uwzględniana wielkość liter.
>

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{Query.id}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c"></a>Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy (C#)

W poniższym przykładzie przedstawiono [funkcja języka C#](functions-dotnet-class-library.md) , który pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, że używa kierować dane do określenia Identyfikatora do wyszukania. Identyfikator służy do pobierania `ToDoItem` dokument z określonej bazy danych i kolekcji.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems/{id}")]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{id}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy, używając SqlQuery (C#)

W poniższym przykładzie przedstawiono [funkcja języka C#](functions-dotnet-class-library.md) , który pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, że używa kierować dane do określenia Identyfikatora do wyszukania. Identyfikator służy do pobierania `ToDoItem` dokument z określonej bazy danych i kolekcji.

W przykładzie pokazano sposób użycia wyrażenia wiązania w `SqlQuery` parametru. Możesz przekazać dane trasy do `SqlQuery` parametru, jak pokazano, ale obecnie [nie można przekazać wartości ciągu zapytania](https://github.com/Azure/azure-functions-host/issues/2554#issuecomment-392084583).


```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems2/{id}")]HttpRequest req,
            [CosmosDB("ToDoItems", "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "select * from ToDoItems r where r.id = {id}")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c"></a>HTTP wyzwalacza, Uzyskaj dokumentację wielu, za pomocą SqlQuery (C#)

W poniższym przykładzie przedstawiono [funkcja języka C#](functions-dotnet-class-library.md) , który pobiera listę dokumentów. Funkcja jest wyzwalana przez żądanie HTTP. Zapytanie jest określona w `SqlQuery` atrybutu właściwości.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}

```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>HTTP wyzwalacza, Uzyskaj dokumentację wielu, za pomocą DocumentClient (C#)

W poniższym przykładzie przedstawiono [funkcja języka C#](functions-dotnet-class-library.md) , który pobiera listę dokumentów. Funkcja jest wyzwalana przez żądanie HTTP. Kod używa `DocumentClient` podanego przez powiązanie usługi Azure Cosmos DB można odczytać listy dokumentów wystąpienia. `DocumentClient` Wystąpienia może również służyć do operacji zapisu.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = null)]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            var searchterm = req.Query["searchterm"];
            if (string.IsNullOrWhiteSpace(searchterm))
            {
                return (ActionResult)new NotFoundResult();
            }

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");

            log.LogInformation($"Searching for: {searchterm}");

            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.LogInformation(result.Description);
                }
            }
            return new OkResult();
        }
    }
}
```

[Pomiń przykłady danych wejściowych](#input---attributes)

### <a name="input---c-script-examples"></a>Dane wejściowe — przykłady skryptów w języku C#

Ta sekcja zawiera następujące przykłady:

* [Wyzwalacz kolejki, odnośnika identyfikator ciągu](#queue-trigger-look-up-id-from-string-c-script)
* [Wyzwalacz kolejki, Uzyskaj dokumentację wielu, za pomocą SqlQuery](#queue-trigger-get-multiple-docs-using-sqlquery-c-script)
* [Wyzwalacz HTTP, Sprawdź identyfikator z ciągu zapytania](#http-trigger-look-up-id-from-query-string-c-script)
* [Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy](#http-trigger-look-up-id-from-route-data-c-script)
* [HTTP wyzwalacza, Uzyskaj dokumentację wielu, za pomocą SqlQuery](#http-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP wyzwalacza, Uzyskaj dokumentację wielu, za pomocą DocumentClient](#http-trigger-get-multiple-docs-using-documentclient-c-script)

Przykłady wyzwalacza HTTP można znaleźć prostego `ToDoItem` typu:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-string-c-script"></a>Wyzwalacz kolejki, odnośnika identyfikator ciągu (skrypt języka C#)

Poniższy przykład pokazuje usługi Cosmos DB, powiązania danych wejściowych w *function.json* pliku i [funkcji skryptu w języku C#](functions-reference-csharp.md) powiązania, który używa. Funkcja odczytuje pojedynczy dokument i aktualizuje wartość tekstu dokumentu.

Oto powiązanie danych w *function.json* pliku:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```
[Konfiguracji](#input---configuration) sekcji opisano te właściwości.

Poniżej przedstawiono kod skryptu języka C#:

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding
    public static void Run(string myQueueItem, dynamic inputDocument)
    {   
      inputDocument.text = "This has changed.";
    }
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-c-script"></a>Wyzwalacz kolejki, Uzyskaj dokumentację wielu, za pomocą SqlQuery (skrypt języka C#)

W poniższym przykładzie pokazano powiązania danych wejściowych usługi Azure Cosmos DB w *function.json* pliku i [funkcji skryptu w języku C#](functions-reference-csharp.md) powiązania, który używa. Funkcja pobiera wiele dokumentów, określonych przez zapytanie SQL, przy użyciu wyzwalacza kolejki, aby dostosować parametry zapytania.

Wyzwalacz kolejki zawiera parametr `departmentId`. Komunikat kolejki `{ "departmentId" : "Finance" }` zwróci wszystkie rekordy dla działu finansowego.

Oto powiązanie danych w *function.json* pliku:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

[Konfiguracji](#input---configuration) sekcji opisano te właściwości.

Poniżej przedstawiono kod skryptu języka C#:

```csharp
    public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
    {   
        foreach (var doc in documents)
        {
            // operate on each document
        }    
    }

    public class QueuePayload
    {
        public string departmentId { get; set; }
    }
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c-script"></a>Wyzwalacz HTTP, Sprawdź identyfikator z ciągu zapytania (skrypt języka C#)

W poniższym przykładzie przedstawiono [funkcji skryptu w języku C#](functions-reference-csharp.md) , który pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, która wykorzystuje ciąg kwerendy do określenia Identyfikatora do wyszukania. Identyfikator służy do pobierania `ToDoItem` dokument z określonej bazy danych i kolekcji.

Oto *function.json* pliku:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": false
}
```

Poniżej przedstawiono kod skryptu języka C#:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c-script"></a>Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy (skrypt języka C#)

W poniższym przykładzie przedstawiono [funkcji skryptu w języku C#](functions-reference-csharp.md) , który pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, że używa kierować dane do określenia Identyfikatora do wyszukania. Identyfikator służy do pobierania `ToDoItem` dokument z określonej bazy danych i kolekcji.

Oto *function.json* pliku:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

Poniżej przedstawiono kod skryptu języka C#:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c-script"></a>HTTP wyzwalacza, Uzyskaj dokumentację wielu, za pomocą SqlQuery (skrypt języka C#)

W poniższym przykładzie przedstawiono [funkcji skryptu w języku C#](functions-reference-csharp.md) , który pobiera listę dokumentów. Funkcja jest wyzwalana przez żądanie HTTP. Zapytanie jest określona w `SqlQuery` atrybutu właściwości.

Oto *function.json* pliku:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "sqlQuery": "SELECT top 2 * FROM c order by c._ts desc"
    }
  ],
  "disabled": false
}
```

Poniżej przedstawiono kod skryptu języka C#:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, IEnumerable<ToDoItem> toDoItems, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    foreach (ToDoItem toDoItem in toDoItems)
    {
        log.LogInformation(toDoItem.Description);
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c-script"></a>HTTP wyzwalacza, Uzyskaj dokumentację wielu, za pomocą DocumentClient (skrypt języka C#)

W poniższym przykładzie przedstawiono [funkcji skryptu w języku C#](functions-reference-csharp.md) , który pobiera listę dokumentów. Funkcja jest wyzwalana przez żądanie HTTP. Kod używa `DocumentClient` podanego przez powiązanie usługi Azure Cosmos DB można odczytać listy dokumentów wystąpienia. `DocumentClient` Wystąpienia może również służyć do operacji zapisu.

Oto *function.json* pliku:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "client",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "inout"
    }
  ],
  "disabled": false
}
```

Poniżej przedstawiono kod skryptu języka C#:

```cs
#r "Microsoft.Azure.Documents.Client"

using System.Net;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Extensions.Logging;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, DocumentClient client, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
    string searchterm = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
        .Value;

    if (searchterm == null)
    {
        return req.CreateResponse(HttpStatusCode.NotFound);
    }

    log.LogInformation($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
    IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
        .Where(p => p.Description.Contains(searchterm))
        .AsDocumentQuery();

    while (query.HasMoreResults)
    {
        foreach (ToDoItem result in await query.ExecuteNextAsync())
        {
            log.LogInformation(result.Description);
        }
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Pomiń przykłady danych wejściowych](#input---attributes)

### <a name="input---javascript-examples"></a>Dane wejściowe — przykłady kodu JavaScript

Ta sekcja zawiera następujące przykłady, które odczytują pojedynczy dokument, określając wartość Identyfikatora z różnych źródeł:

* [Wyzwalacz kolejki, odnośnika identyfikator z formatu JSON](#queue-trigger-look-up-id-from-json-javascript)
* [Wyzwalacz HTTP, Sprawdź identyfikator z ciągu zapytania](#http-trigger-look-up-id-from-query-string-javascript)
* [Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy](#http-trigger-look-up-id-from-route-data-javascript)
* [Wyzwalacz kolejki, Uzyskaj dokumentację wielu, za pomocą SqlQuery](#queue-trigger-get-multiple-docs-using-sqlquery-javascript)

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-javascript"></a>Wyzwalacz kolejki, odnośnika identyfikator z formatu JSON (JavaScript)

Poniższy przykład pokazuje usługi Cosmos DB, powiązania danych wejściowych w *function.json* pliku i [funkcji JavaScript](functions-reference-node.md) powiązania, który używa. Funkcja odczytuje pojedynczy dokument i aktualizuje wartość tekstu dokumentu.

Oto powiązanie danych w *function.json* pliku:

```json
{
    "name": "inputDocumentIn",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```
[Konfiguracji](#input---configuration) sekcji opisano te właściwości.

Poniżej przedstawiono kod JavaScript:

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {   
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-javascript"></a>Wyzwalacz HTTP, Sprawdź identyfikator z ciągu zapytania (JavaScript)

W poniższym przykładzie przedstawiono [funkcji JavaScript](functions-reference-node.md) , który pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, która wykorzystuje ciąg kwerendy do określenia Identyfikatora do wyszukania. Identyfikator służy do pobierania `ToDoItem` dokument z określonej bazy danych i kolekcji.

Oto *function.json* pliku:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": false
}
```

Poniżej przedstawiono kod JavaScript:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-javascript"></a>Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy (JavaScript)

W poniższym przykładzie przedstawiono [funkcji JavaScript](functions-reference-node.md) , który pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, która wykorzystuje ciąg kwerendy do określenia Identyfikatora do wyszukania. Identyfikator służy do pobierania `ToDoItem` dokument z określonej bazy danych i kolekcji.

Oto *function.json* pliku:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

Poniżej przedstawiono kod JavaScript:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-javascript"></a>Wyzwalacz kolejki, Uzyskaj dokumentację wielu, za pomocą SqlQuery (JavaScript)

W poniższym przykładzie pokazano powiązania danych wejściowych usługi Azure Cosmos DB w *function.json* pliku i [funkcji JavaScript](functions-reference-node.md) powiązania, który używa. Funkcja pobiera wiele dokumentów, określonych przez zapytanie SQL, przy użyciu wyzwalacza kolejki, aby dostosować parametry zapytania.

Wyzwalacz kolejki zawiera parametr `departmentId`. Komunikat kolejki `{ "departmentId" : "Finance" }` zwróci wszystkie rekordy dla działu finansowego.

Oto powiązanie danych w *function.json* pliku:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

[Konfiguracji](#input---configuration) sekcji opisano te właściwości.

Poniżej przedstawiono kod JavaScript:

```javascript
    module.exports = function (context, input) {    
        var documents = context.bindings.documents;
        for (var i = 0; i < documents.length; i++) {
            var document = documents[i];
            // operate on each document
        }       
        context.done();
    };
```

[Pomiń przykłady danych wejściowych](#input---attributes)

### <a name="input---python-examples"></a>Dane wejściowe — przykłady języka Python

Ta sekcja zawiera następujące przykłady, które odczytują pojedynczy dokument, określając wartość Identyfikatora z różnych źródeł:

* [Wyzwalacz kolejki, odnośnika identyfikator z formatu JSON](#queue-trigger-look-up-id-from-json-python)
* [Wyzwalacz HTTP, Sprawdź identyfikator z ciągu zapytania](#http-trigger-look-up-id-from-query-string-python)
* [Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy](#http-trigger-look-up-id-from-route-data-python)
* [Wyzwalacz kolejki, Uzyskaj dokumentację wielu, za pomocą SqlQuery](#queue-trigger-get-multiple-docs-using-sqlquery-python)

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-python"></a>Wyzwalacz kolejki, odnośnika identyfikator z formatu JSON (Python)

Poniższy przykład pokazuje usługi Cosmos DB, powiązania danych wejściowych w *function.json* pliku i [funkce Pythonu](functions-reference-python.md) powiązania, który używa. Funkcja odczytuje pojedynczy dokument i aktualizuje wartość tekstu dokumentu.

Oto powiązanie danych w *function.json* pliku:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
},
{
    "name": "$return",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[Konfiguracji](#input---configuration) sekcji opisano te właściwości.

Poniżej przedstawiono kod języka Python:

```python
import azure.functions as func


def main(queuemsg: func.QueueMessage, documents: func.DocumentList) -> func.Document:
    if documents:
        document = documents[0]
        document['text'] = 'This was updated!'
        return document
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-python"></a>Wyzwalacz HTTP, Sprawdź identyfikator z ciągu zapytania (Python)

W poniższym przykładzie przedstawiono [funkce Pythonu](functions-reference-python.md) , który pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, która wykorzystuje ciąg kwerendy do określenia Identyfikatora do wyszukania. Identyfikator służy do pobierania `ToDoItem` dokument z określonej bazy danych i kolekcji.

Oto *function.json* pliku:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": true,
  "scriptFile": "__init__.py"
}
```

Poniżej przedstawiono kod języka Python:

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])

    return 'OK'
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-python"></a>Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy (Python)

W poniższym przykładzie przedstawiono [funkce Pythonu](functions-reference-python.md) , który pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, która wykorzystuje ciąg kwerendy do określenia Identyfikatora do wyszukania. Identyfikator służy do pobierania `ToDoItem` dokument z określonej bazy danych i kolekcji.

Oto *function.json* pliku:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

Poniżej przedstawiono kod języka Python:

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])
    return 'OK'
```

[Pomiń przykłady danych wejściowych](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-python"></a>Wyzwalacz kolejki, Uzyskaj dokumentację wielu, za pomocą SqlQuery (Python)

W poniższym przykładzie pokazano powiązania danych wejściowych usługi Azure Cosmos DB w *function.json* pliku i [funkce Pythonu](functions-reference-python.md) powiązania, który używa. Funkcja pobiera wiele dokumentów, określonych przez zapytanie SQL, przy użyciu wyzwalacza kolejki, aby dostosować parametry zapytania.

Wyzwalacz kolejki zawiera parametr `departmentId`. Komunikat kolejki `{ "departmentId" : "Finance" }` zwróci wszystkie rekordy dla działu finansowego.

Oto powiązanie danych w *function.json* pliku:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

[Konfiguracji](#input---configuration) sekcji opisano te właściwości.

Poniżej przedstawiono kod języka Python:

```python
import azure.functions as func

def main(queuemsg: func.QueueMessage, documents: func.DocumentList):
    for document in documents:
        # operate on each document
```


[Pomiń przykłady danych wejściowych](#input---attributes)

<a name="infsharp"></a>

### <a name="input---f-examples"></a>Dane wejściowe - F# przykłady

Poniższy przykład pokazuje usługi Cosmos DB, powiązania danych wejściowych w *function.json* pliku i [ F# funkcja](functions-reference-fsharp.md) powiązania, który używa. Funkcja odczytuje pojedynczy dokument i aktualizuje wartość tekstu dokumentu.

Oto powiązanie danych w *function.json* pliku:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```

[Konfiguracji](#input---configuration) sekcji opisano te właściwości.

Oto F# kodu:

```fsharp
    (* Change input document contents using Azure Cosmos DB input binding *)
    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, inputDocument: obj) =
    inputDocument?text <- "This has changed."
```

W tym przykładzie wymaga `project.json` pliku, który określa `FSharp.Interop.Dynamic` i `Dynamitey` zależności NuGet:

```json
{
    "frameworks": {
        "net46": {
            "dependencies": {
                "Dynamitey": "1.0.2",
                "FSharp.Interop.Dynamic": "3.0.0"
            }
        }
    }
}
```

Aby dodać `project.json` plików, zobacz [ F# Zarządzanie pakietami](functions-reference-fsharp.md#package).

### <a name="input---java-examples"></a>Dane wejściowe - przykładów w języku Java

Ta sekcja zawiera następujące przykłady:

* [Wyzwalacz HTTP, odnośnika identyfikator z ciągu zapytania — parametr ciągu](#http-trigger-look-up-id-from-query-string---string-parameter-java)
* [Wyzwalacz HTTP, Sprawdź identyfikator z ciągu zapytania — parametr obiektu typu POJO](#http-trigger-look-up-id-from-query-string---pojo-parameter-java)
* [Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy](#http-trigger-look-up-id-from-route-data-java)
* [Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy, używając SqlQuery](#http-trigger-look-up-id-from-route-data-using-sqlquery-java)
* [HTTP wyzwalacza, Uzyskaj dokumentację wielu z danych trasy, używając SqlQuery](#http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java)

Przykłady dotyczą prostego `ToDoItem` typu:

```java
public class ToDoItem {

  private String id;
  private String description;  

  public String getId() {
    return id;
  }

  public String getDescription() {
    return description;
  }
  
  @Override
  public String toString() {
    return "ToDoItem={id=" + id + ",description=" + description + "}";
  }
}
```

#### <a name="http-trigger-look-up-id-from-query-string---string-parameter-java"></a>Wyzwalacz HTTP, Sprawdź identyfikator z ciągu zapytania — parametr ciągu (Java)

Poniższy przykład przedstawia funkcję języka Java, która pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, które wykorzystuje ciąg kwerendy do określenia Identyfikatora do wyszukania. Ten identyfikator służy do pobierania dokumentu z określonej bazy danych i kolekcji w postaci ciągu.

```java
public class DocByIdFromQueryString {

    @FunctionName("DocByIdFromQueryString")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.id}",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            Optional<String> item,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string 
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

W [Java funkcje biblioteki środowiska uruchomieniowego](/java/api/overview/azure/functions/runtime), użyj `@CosmosDBInput` adnotacja dla parametrów funkcji, którego wartość może pochodzić z usługi Cosmos DB.  Ta adnotacja mogą być używane z natywnych typów języka Java, obiektów typu Pojo lub wartości dopuszczających wartości null przy użyciu opcjonalnie<T>.

#### <a name="http-trigger-look-up-id-from-query-string---pojo-parameter-java"></a>Wyzwalacz HTTP, Sprawdź identyfikator z ciągu zapytania — parametr obiektu typu POJO (Java)

Poniższy przykład przedstawia funkcję języka Java, która pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, które wykorzystuje ciąg kwerendy do określenia Identyfikatora do wyszukania. Ten identyfikator służy do pobierania dokumentu z określonej bazy danych i kolekcji. Dokument jest następnie konwertowana do wystąpienia ```ToDoItem``` obiektu typu POJO wcześniej utworzony i przekazany jako argument do funkcji.

```java
public class DocByIdFromQueryStringPojo {

    @FunctionName("DocByIdFromQueryStringPojo")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.id}",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            ToDoItem item,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Item from the database is " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

#### <a name="http-trigger-look-up-id-from-route-data-java"></a>Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy (Java)

Poniższy przykład przedstawia funkcję języka Java, która pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, które używa parametru trasy w celu określenia Identyfikatora do wyszukania. Czy identyfikator jest używany do pobierania dokumentu z określonej bazy danych i kolekcji, zwracanie go jako ```Optional<String>```.

```java
public class DocByIdFromRoute {

    @FunctionName("DocByIdFromRoute")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems/{id}")
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{id}",
              partitionKey = "{id}",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            Optional<String> item,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string 
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-java"></a>Wyzwalacz HTTP, wyszukiwania Identyfikatora z danych trasy, używając SqlQuery (Java)

Poniższy przykład przedstawia funkcję języka Java, która pobiera pojedynczego dokumentu. Funkcja jest wyzwalana przez żądanie HTTP, które używa parametru trasy w celu określenia Identyfikatora do wyszukania. Czy identyfikator jest używany do pobierania dokumentu z określonej bazy danych i kolekcji, wynik konwersji równa ```ToDoItem[]```, ponieważ wiele dokumentów mogą być zwracane w zależności od kryteriów zapytania.

```java
public class DocByIdFromRouteSqlQuery {

    @FunctionName("DocByIdFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems2/{id}") 
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where r.id = {id}",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            ToDoItem[] item,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Items from the database are " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

#### <a name="http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java"></a>HTTP wyzwalacza, Uzyskaj dokumentację wielu z danych trasy, używając SqlQuery (Java)

W poniższym przykładzie pokazano funkcję Java wiele dokumentów. Funkcja jest wyzwalana przez żądanie HTTP, który używa parametru trasy ```desc``` Aby określić ciąg do wyszukania w ```description``` pola. Termin wyszukiwania jest używana do pobierania kolekcji dokumentów z określonej bazy danych i kolekcji, zestaw wyników do konwertowania ```ToDoItem[]``` i przekazanie do niej jako argument do funkcji.

```java
public class DocsFromRouteSqlQuery {

    @FunctionName("DocsFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET}, 
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems3/{desc}")
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where contains(r.description, {desc})",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            ToDoItem[] items,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Number of items from the database is " + (items == null ? 0 : items.length));

        // Convert and display
        if (items == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("No documents found.")
                          .build();
        } 
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(items)
                          .build();
        }
    }
}
 ```

## <a name="input---attributes"></a>Dane wejściowe — atrybuty

W [bibliotek klas języka C#](functions-dotnet-class-library.md), użyj [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) atrybutu.

Konstruktor atrybutu ma nazwę bazy danych i nazwę kolekcji. Aby uzyskać informacje o tych ustawieniach i inne właściwości, które można skonfigurować, zobacz [w poniższej sekcji konfiguracji](#input---configuration).

## <a name="input---configuration"></a>Dane wejściowe — Konfiguracja

W poniższej tabeli opisano właściwości konfiguracji powiązania, które można ustawić w *function.json* pliku i `CosmosDB` atrybutu.

|Właściwość Function.JSON | Właściwość atrybutu |Opis|
|---------|---------|----------------------|
|**type**     || Musi być równa `cosmosDB`.        |
|**direction**     || Musi być równa `in`.         |
|**name**     || Nazwa parametru powiązania, który reprezentuje dokument w funkcji.  |
|**databaseName** |**DatabaseName** |Baza danych zawiera dokument.        |
|**collectionName** |**collectionName** | Nazwa kolekcji, która zawiera dokument. |
|**id**    | **Identyfikator** | Identyfikator dokumentu do pobrania. Ta właściwość obsługuje [powiązania wyrażeń](./functions-bindings-expressions-patterns.md). Nie należy ustawiać zarówno **identyfikator** i **sqlQuery** właściwości. Jeśli nie ustawisz pojedynczo, zostanie pobrana całą kolekcję. |
|**sqlQuery**  |**SqlQuery**  | Zapytania SQL usługi Azure Cosmos DB używane do pobierania wiele dokumentów. Właściwość obsługuje powiązań środowiska uruchomieniowego, jak w poniższym przykładzie: `SELECT * FROM c where c.departmentId = {departmentId}`. Nie należy ustawiać zarówno **identyfikator** i **sqlQuery** właściwości. Jeśli nie ustawisz pojedynczo, zostanie pobrana całą kolekcję.|
|**connectionStringSetting**     |**ConnectionStringSetting**|Nazwa ustawienia aplikacji zawierającego parametry połączenia usługi Azure Cosmos DB.        |
|**partitionKey**|**PartitionKey**|Określa wartość klucza partycji do wyszukiwania. Może zawierać parametrów wiązania.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Dane wejściowe — użycie

W C# i F# funkcji, jeśli funkcja kończy działanie pomyślnie, wszelkie zmiany wprowadzone do dokument wejściowy za pośrednictwem danych wejściowych o nazwie parametry są automatycznie zachowywane.

W funkcji języka JavaScript nie zostało zaktualizowane automatycznie po wyjściu z funkcji. Zamiast tego należy użyć `context.bindings.<documentName>In` i `context.bindings.<documentName>Out` Aby wprowadzić aktualizacje. Zobacz przykład JavaScript.

## <a name="output"></a>Output

Dane wyjściowe usługi Azure Cosmos DB, powiązania pozwala zapisać nowy dokument z bazą danych Azure Cosmos DB przy użyciu interfejsu API SQL.

## <a name="output---examples"></a>Dane wyjściowe — przykłady

Zobacz przykłady specyficzny dla języka:

* [C#](#output---c-examples)
* [Skryptu C# (csx)](#output---c-script-examples)
* [F#](#output---f-examples)
* [Java](#output---java-examples)
* [JavaScript](#output---javascript-examples)

Zobacz też [wejściowych przykład](#input---c-examples) , który używa `DocumentClient`.

[Pomiń przykładowe dane wyjściowe](#output---attributes)

### <a name="output---c-examples"></a>Dane wyjściowe — C# przykłady

Ta sekcja zawiera następujące przykłady:

* Wyzwalacz kolejki, doc jeden zapisu
* Wyzwalacz kolejki, dokumentacja zapisu przy użyciu IAsyncCollector

Przykłady dotyczą prostego `ToDoItem` typu:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Pomiń przykładowe dane wyjściowe](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c"></a>Wyzwalacz kolejki, doc jeden zapis (C#)

W poniższym przykładzie przedstawiono [funkcja języka C#](functions-dotnet-class-library.md) , dodanie dokumentu do bazy danych przy użyciu danych podany w wiadomości z usługi Queue storage.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using System;

namespace CosmosDBSamplesV2
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            ILogger log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.LogInformation($"C# Queue trigger function inserted one row");
            log.LogInformation($"Description={queueMessage}");
        }
    }
}
```

[Pomiń przykładowe dane wyjściowe](#output---attributes)

#### <a name="queue-trigger-write-docs-using-iasynccollector-c"></a>Wyzwalacz kolejki, dokumentacja zapisu przy użyciu IAsyncCollector (C#)

W poniższym przykładzie przedstawiono [funkcja języka C#](functions-dotnet-class-library.md) kolekcji dokumentów, który dodaje w bazie danych, przy użyciu danych w komunikatu w kolejce JSON.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.LogInformation($"Description={toDoItem.Description}");
                await toDoItemsOut.AddAsync(toDoItem);
            }
        }
    }
}
```

[Pomiń przykładowe dane wyjściowe](#output---attributes)

### <a name="output---c-script-examples"></a>Dane wyjściowe — przykłady skryptów w języku C#

Ta sekcja zawiera następujące przykłady:

* Wyzwalacz kolejki, doc jeden zapisu
* Wyzwalacz kolejki, dokumentacja zapisu przy użyciu IAsyncCollector

[Pomiń przykładowe dane wyjściowe](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c-script"></a>Wyzwalacz kolejki, doc jeden zapisu (skrypt języka C#)

W poniższym przykładzie pokazano powiązania w danych wyjściowych usługi Azure Cosmos DB *function.json* pliku i [funkcji skryptu w języku C#](functions-reference-csharp.md) powiązania, który używa. Funkcja używa powiązania danych wejściowych w kolejce dla kolejki, która odbiera JSON w następującym formacie:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Funkcja tworzy dokumentów usługi Azure Cosmos DB w następującym formacie, dla każdego rekordu:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Oto powiązanie danych w *function.json* pliku:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```

[Konfiguracji](#output---configuration) sekcji opisano te właściwości.

Poniżej przedstawiono kod skryptu języka C#:

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
    using Newtonsoft.Json.Linq;
    using Microsoft.Extensions.Logging;

    public static void Run(string myQueueItem, out object employeeDocument, ILogger log)
    {
      log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");

      dynamic employee = JObject.Parse(myQueueItem);

      employeeDocument = new {
        id = employee.name + "-" + employee.employeeId,
        name = employee.name,
        employeeId = employee.employeeId,
        address = employee.address
      };
    }
```

#### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Wyzwalacz kolejki, dokumentacja zapisu przy użyciu IAsyncCollector

Aby utworzyć wiele dokumentów, można powiązać `ICollector<T>` lub `IAsyncCollector<T>` gdzie `T` jest jednym z obsługiwanych typów.

Ten przykład dotyczy prostego `ToDoItem` typu:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

Poniżej przedstawiono plik function.json:

```json
{
  "bindings": [
    {
      "name": "toDoItemsIn",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "todoqueueforwritemulti",
      "connectionStringSetting": "AzureWebJobsStorage"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItemsOut",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Poniżej przedstawiono kod skryptu języka C#:

```cs
using System;
using Microsoft.Extensions.Logging;

public static async Task Run(ToDoItem[] toDoItemsIn, IAsyncCollector<ToDoItem> toDoItemsOut, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

    foreach (ToDoItem toDoItem in toDoItemsIn)
    {
        log.LogInformation($"Description={toDoItem.Description}");
        await toDoItemsOut.AddAsync(toDoItem);
    }
}
```

[Pomiń przykładowe dane wyjściowe](#output---attributes)

### <a name="output---javascript-examples"></a>Dane wyjściowe — przykłady kodu JavaScript

W poniższym przykładzie pokazano powiązania w danych wyjściowych usługi Azure Cosmos DB *function.json* pliku i [funkcji JavaScript](functions-reference-node.md) powiązania, który używa. Funkcja używa powiązania danych wejściowych w kolejce dla kolejki, która odbiera JSON w następującym formacie:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Funkcja tworzy dokumentów usługi Azure Cosmos DB w następującym formacie, dla każdego rekordu:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Oto powiązanie danych w *function.json* pliku:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```

[Konfiguracji](#output---configuration) sekcji opisano te właściwości.

Poniżej przedstawiono kod JavaScript:

```javascript
    module.exports = function (context) {

      context.bindings.employeeDocument = JSON.stringify({
        id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
        name: context.bindings.myQueueItem.name,
        employeeId: context.bindings.myQueueItem.employeeId,
        address: context.bindings.myQueueItem.address
      });

      context.done();
    };
```

[Pomiń przykładowe dane wyjściowe](#output---attributes)

### <a name="output---f-examples"></a>Dane wyjściowe — F# przykłady

W poniższym przykładzie pokazano powiązania w danych wyjściowych usługi Azure Cosmos DB *function.json* pliku i [ F# funkcja](functions-reference-fsharp.md) powiązania, który używa. Funkcja używa powiązania danych wejściowych w kolejce dla kolejki, która odbiera JSON w następującym formacie:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Funkcja tworzy dokumentów usługi Azure Cosmos DB w następującym formacie, dla każdego rekordu:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Oto powiązanie danych w *function.json* pliku:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```
[Konfiguracji](#output---configuration) sekcji opisano te właściwości.

Oto F# kodu:

```fsharp
    open FSharp.Interop.Dynamic
    open Newtonsoft.Json
    open Microsoft.Extensions.Logging

    type Employee = {
      id: string
      name: string
      employeeId: string
      address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: ILogger) =
      log.LogInformation(sprintf "F# Queue trigger function processed: %s" myQueueItem)
      let employee = JObject.Parse(myQueueItem)
      employeeDocument <-
        { id = sprintf "%s-%s" employee?name employee?employeeId
          name = employee?name
          employeeId = employee?employeeId
          address = employee?address }
```

W tym przykładzie wymaga `project.json` pliku, który określa `FSharp.Interop.Dynamic` i `Dynamitey` zależności NuGet:

```json
{
    "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
           }
        }
    }
}
```

Aby dodać `project.json` plików, zobacz [ F# Zarządzanie pakietami](functions-reference-fsharp.md#package).

### <a name="output---java-examples"></a>Dane wyjściowe — przykładów w języku Java

* [Wyzwalacz kolejki, Zapisz komunikat do bazy danych za pomocą wartości zwracanej](#queue-trigger-save-message-to-database-via-return-value-java)
* [Wyzwalacz HTTP, Zapisz jednego dokumentu do bazy danych za pomocą wartości zwracanej](#http-trigger-save-one-document-to-database-via-return-value-java)
* [Wyzwalacz HTTP, Zapisz jednego dokumentu do bazy danych za pośrednictwem OutputBinding](#http-trigger-save-one-document-to-database-via-outputbinding-java)
* [Wyzwalacz HTTP, Zapisz wiele dokumentów do bazy danych za pośrednictwem OutputBinding](#http-trigger-save-multiple-documents-to-database-via-outputbinding-java)


#### <a name="queue-trigger-save-message-to-database-via-return-value-java"></a>Wyzwalacz kolejki, Zapisz komunikat do bazy danych za pomocą wartości zwracanej (Java)

Funkcja języka Java, która dodaje dokumentu do bazy danych przy użyciu danych z wiadomości w usłudze Queue storage można znaleźć w poniższym przykładzie.

```java
@FunctionName("getItem")
@CosmosDBOutput(name = "database", 
  databaseName = "ToDoList", 
  collectionName = "Items", 
  connectionStringSetting = "AzureCosmosDBConnection")
public String cosmosDbQueryById(
    @QueueTrigger(name = "msg", 
      queueName = "myqueue-items", 
      connection = "AzureWebJobsStorage") 
    String message,
    final ExecutionContext context)  {
     return "{ id: \"" + System.currentTimeMillis() + "\", Description: " + message + " }";
   }
```

#### <a name="http-trigger-save-one-document-to-database-via-return-value-java"></a>Wyzwalacz HTTP, Zapisz jednego dokumentu do bazy danych za pomocą wartości zwracanej (Java)

W poniższym przykładzie pokazano funkcję języka Java, którego podpis jest oznaczony za pomocą ```@CosmosDBOutput``` i nie zwraca wartości typu ```String```. Dokument JSON zwróconą przez funkcję będą automatycznie zapisywane do odpowiedniej kolekcji bazy danych cosmos DB.

```java
    @FunctionName("WriteOneDoc")
    @CosmosDBOutput(name = "database", 
      databaseName = "ToDoList",
      collectionName = "Items", 
      connectionStringSetting = "Cosmos_DB_Connection_String")
    public String run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());

        // Parse query parameter        
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Generate random ID
        final int id = Math.abs(new Random().nextInt());

        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                    "\"description\": \"" + name + "\"}";

        context.getLogger().info("Document to be saved: " + jsonDocument);

        return jsonDocument;
    }
```

#### <a name="http-trigger-save-one-document-to-database-via-outputbinding-java"></a>Wyzwalacz HTTP, Zapisz jednego dokumentu do bazy danych za pośrednictwem OutputBinding (Java)

W poniższym przykładzie pokazano funkcję języka Java, która zapisuje dokumentu cosmos DB za pomocą ```OutputBinding<T>``` parametr wyjściowy. Należy pamiętać, że w tej konfiguracji jest ```outputItem``` parametr, który musi być oznaczona przy użyciu ```@CosmosDBOutput```, nie sygnatura funkcji. Za pomocą ```OutputBinding<T>``` umożliwia funkcji zalet powiązania, które można zapisać dokumentu do usługi CosmosDB zachowaniu zwraca inną wartość do obiektu wywołującego funkcji, takich jak JSON lub XML dokumentu.

```java
    @FunctionName("WriteOneDocOutputBinding")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBOutput(name = "database", 
              databaseName = "ToDoList", 
              collectionName = "Items", 
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            OutputBinding<String> outputItem,
            final ExecutionContext context) {
  
        // Parse query parameter
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
      
        // Generate random ID
        final int id = Math.abs(new Random().nextInt());

        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                    "\"description\": \"" + name + "\"}";

        context.getLogger().info("Document to be saved: " + jsonDocument);

        // Set outputItem's value to the JSON document to be saved
        outputItem.setValue(jsonDocument);

        // return a different document to the browser or calling client.
        return request.createResponseBuilder(HttpStatus.OK)
                      .body("Document created successfully.")
                      .build();
    }
```

#### <a name="http-trigger-save-multiple-documents-to-database-via-outputbinding-java"></a>Wyzwalacz HTTP, aby zapisać wiele dokumentów do bazy danych za pośrednictwem OutputBinding (Java)

W poniższym przykładzie pokazano funkcję języka Java, która zapisuje wiele dokumentów do bazy danych cosmos DB za pomocą ```OutputBinding<T>``` parametr wyjściowy. Należy pamiętać, że w tej konfiguracji jest ```outputItem``` parametr, który musi być oznaczona przy użyciu ```@CosmosDBOutput```, nie sygnatura funkcji. Parametr wyjściowy ```outputItem``` zawiera listę ```ToDoItem``` obiektów jako jego typu parametru szablonu. Za pomocą ```OutputBinding<T>``` umożliwia funkcji zalet powiązania, które można zapisać dokumenty do usługi CosmosDB zachowaniu zwraca inną wartość do obiektu wywołującego funkcji, takich jak JSON lub XML dokumentu.

```java
    @FunctionName("WriteMultipleDocsOutputBinding")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBOutput(name = "database", 
              databaseName = "ToDoList", 
              collectionName = "Items", 
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            OutputBinding<List<ToDoItem>> outputItem,
            final ExecutionContext context) {
  
        // Parse query parameter
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
      
        // Generate documents
        List<ToDoItem> items = new ArrayList<>();

        for (int i = 0; i < 5; i ++) {
          // Generate random ID
          final int id = Math.abs(new Random().nextInt());

          // Create ToDoItem
          ToDoItem item = new ToDoItem(String.valueOf(id), name);
          
          items.add(item);
        }

        // Set outputItem's value to the list of POJOs to be saved
        outputItem.setValue(items);
        context.getLogger().info("Document to be saved: " + items);

        // return a different document to the browser or calling client.
        return request.createResponseBuilder(HttpStatus.OK)
                      .body("Documents created successfully.")
                      .build();
    }
```

W [Java funkcje biblioteki środowiska uruchomieniowego](/java/api/overview/azure/functions/runtime), użyj `@CosmosDBOutput` adnotacji w parametrach, które zostaną zapisane w bazie danych Cosmos DB.  Typ parametru adnotacji powinny być ```OutputBinding<T>```, gdzie T jest typem natywnym Java lub obiektu typu POJO.


## <a name="output---attributes"></a>Dane wyjściowe — atrybuty

W [bibliotek klas języka C#](functions-dotnet-class-library.md), użyj [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/master/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) atrybutu.

Konstruktor atrybutu ma nazwę bazy danych i nazwę kolekcji. Aby uzyskać informacje o tych ustawieniach i inne właściwości, które można skonfigurować, zobacz [dane wyjściowe — Konfiguracja](#output---configuration). Oto `CosmosDB` przykład atrybutu w podpisie metody:

```csharp
    [FunctionName("QueueToDocDB")]        
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [CosmosDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Aby uzyskać kompletny przykład, zobacz dane wyjściowe — C# przykład.

## <a name="output---configuration"></a>Dane wyjściowe — Konfiguracja

W poniższej tabeli opisano właściwości konfiguracji powiązania, które można ustawić w *function.json* pliku i `CosmosDB` atrybutu.

|Właściwość Function.JSON | Właściwość atrybutu |Opis|
|---------|---------|----------------------|
|**type**     || Musi być równa `cosmosDB`.        |
|**direction**     || Musi być równa `out`.         |
|**name**     || Nazwa parametru powiązania, który reprezentuje dokument w funkcji.  |
|**databaseName** | **DatabaseName**|Baza danych, zawierający kolekcję, w którym zostanie utworzona dokumentu.     |
|**collectionName** |**collectionName**  | Nazwa kolekcji jest tworzona dokumentu. |
|**createIfNotExists**  |**createIfNotExists**    | Wartość logiczna, aby wskazać, czy kolekcja jest tworzona, gdy nie istnieje. Wartość domyślna to *false* ponieważ nowych kolekcji są tworzone z zarezerwowaną przepływnością, co ma koszt skutki. Aby uzyskać więcej informacji, odwiedź [stronę cennika](https://azure.microsoft.com/pricing/details/cosmos-db/).  |
|**partitionKey**|**PartitionKey** |Gdy `CreateIfNotExists` ma wartość true, określa ścieżkę klucza partycji dla utworzonej kolekcji.|
|**collectionThroughput**|**collectionThroughput**| Gdy `CreateIfNotExists` ma wartość true, określa [przepływności](../cosmos-db/set-throughput.md) utworzonej kolekcji.|
|**connectionStringSetting**    |**ConnectionStringSetting** |Nazwa ustawienia aplikacji zawierającego parametry połączenia usługi Azure Cosmos DB.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Dane wyjściowe — użycie

Domyślnie podczas zapisu do parametru wyjściowego w funkcji, tworzony jest dokument w bazie danych. Ten dokument ma automatycznie generowanym identyfikatorze GUID jako identyfikatora dokumentu. Identyfikator dokumentu dokumentu wyjściowego można określić, podając `id` właściwości w obiekcie JSON przekazanego do parametru wyjściowego.

> [!Note]  
> Po określeniu identyfikator istniejącego dokumentu, pobiera to zastąpione przez nowy dokument danych wyjściowych.

## <a name="exceptions-and-return-codes"></a>Wyjątki i kody powrotne

| Powiązanie | Informacje ogólne |
|---|---|
| CosmosDB | [Kody błędów CosmosDB](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>Ustawienia host.JSON

W tej sekcji opisano globalne ustawienia konfiguracji dostępne dla tego powiązania w wersji 2.x. Aby uzyskać więcej informacji na temat globalne ustawienia konfiguracji w wersji 2.x, zobacz [dokumentacja pliku host.JSON dla usługi Azure Functions w wersji 2.x](functions-host-json.md).

```json
{
    "version": "2.0",
    "extensions": {
        "cosmosDB": {
            "connectionMode": "Gateway",
            "protocol": "Https",
            "leaseOptions": {
                "leasePrefix": "prefix1"
            }
        }
    }
}
```  

|Właściwość  |Domyślne | Opis |
|---------|---------|---------| 
|GatewayMode|Brama|Tryb połączenia używane przez funkcję, podczas nawiązywania połączenia z usługą Azure Cosmos DB. Dostępne są opcje `Direct` i `Gateway`|
|Protocol|Https|Protokół połączenia używane przez funkcję podczas połączenia z usługą Azure Cosmos DB.  Odczyt [tutaj objaśnienia dotyczące oba tryby](../cosmos-db/performance-tips.md#networking)| 
|leasePrefix|Nie dotyczy|Prefiks dzierżawy do użycia we wszystkich funkcji w aplikacji.| 

## <a name="next-steps"></a>Kolejne kroki

* [Dowiedz się więcej o bazie danych bez użycia serwera, obliczeń w usłudze Cosmos DB](../cosmos-db/serverless-computing-database.md)
* [Dowiedz się więcej na temat usługi Azure functions, wyzwalaczami i powiązaniami](functions-triggers-bindings.md)

<!---
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Cosmos DB trigger](functions-create-cosmos-db-triggered-function.md)
--->
