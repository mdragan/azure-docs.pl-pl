---
title: Kompilowanie aplikacji internetowej Java w systemie Linux — Azure App Service
description: Kompilowanie, wdrażanie i skalowanie aplikacji internetowych Java na platformie Spring Boot za pomocą usługi Azure App Service w systemie Linux i usłudze Azure Cosmos DB.
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.service: app-service-web
ms.devlang: java
ms.topic: tutorial
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: 069bc213695de813ad6b878db54f38a909efd1df
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956041"
---
# <a name="tutorial-build-a-java-web-app-using-spring-and-azure-cosmos-db"></a>Samouczek: Kompilowanie aplikacji internetowej w języku Java przy użyciu platformy Spring i usługi Azure Cosmos DB

Ten samouczek przeprowadzi Cię przez proces tworzenia, konfigurowania, wdrażania i skalowania aplikacji internetowych w języku Java na platformie Azure. Po zakończeniu będziesz mieć aplikację [Spring Boot](https://projects.spring.io/spring-boot/) przechowującą dane w bazie danych [Azure Cosmos DB](/azure/cosmos-db) i działającą w usłudze [Azure App Service w systemie Linux](/azure/app-service/containers).

![Aplikacja Java uruchomiona w usłudze Azure App Service](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie bazy danych Cosmos DB
> * Łączenie przykładowej aplikacji z bazą danych i jej lokalne testowanie
> * Wdrażanie przykładowej aplikacji na platformie Azure
> * Przesyłanie strumieniowe dzienników diagnostycznych z usługi App Service
> * Dodawanie dodatkowych wystąpień do skalowania w poziomie przykładowej aplikacji

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Wymagania wstępne

* [Interfejs wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/overview) zainstalowany na własnym komputerze. 
* [Usługa Git](https://git-scm.com/)
* [Java JDK](https://aka.ms/azure-jdks)
* [Maven](https://maven.apache.org)

## <a name="clone-the-sample-todo-app-and-prepare-the-repo"></a>Klonowanie przykładowej aplikacji TODO i przygotowanie repozytorium

W tym samouczku jest używana przykładowa aplikacja listy zadań do wykonania (TODO) mająca internetowy interfejs użytkownika, który wywołuje interfejs API REST platformy Spring wspierany przez [dane platformy Spring usługi Azure Cosmos DB](https://github.com/Microsoft/spring-data-cosmosdb). Kod aplikacji jest dostępny [w serwisie GitHub](https://github.com/Microsoft/spring-todo-app). Aby dowiedzieć się więcej na temat pisania aplikacji Java przy użyciu platformy Spring i bazy danych Cosmos DB, zobacz [Spring Boot Starter with the Azure Cosmos DB SQL API tutorial (Samouczek dotyczący używania funkcji Spring Boot Starter z interfejsem API SQL usługi Azure Cosmos DB)](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db ) i [Spring Data Azure Cosmos DB quick start (Przewodnik Szybki start dotyczący danych platformy Spring w usłudze Azure Cosmos DB)](https://github.com/Microsoft/spring-data-cosmosdb#quick-start).


Uruchom następujące polecenia w terminalu, aby sklonować przykładowe repozytorium i skonfiguruj środowisko aplikacji przykładowej.

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
cd e2e-java-experience-in-app-service-linux-part-2
yes | cp -rf .prep/* .
```

## <a name="create-an-azure-cosmos-db"></a>Tworzenie bazy danych Azure Cosmos DB

Wykonaj następujące kroki, aby utworzyć bazę danych Azure Cosmos DB w ramach swojej subskrypcji. Aplikacja listy TODO będzie nawiązywać połączenie z tą bazą danych i przechowywać jej dane podczas pracy, utrwalając stan aplikacji niezależnie od tego, gdzie ją uruchomisz.

1. Zaloguj się w interfejsie wiersza polecenia platformy Azure i opcjonalnie ustaw subskrypcję, jeśli masz więcej niż jedną połączoną z poświadczeniami logowania.

    ```bash
    az login
    az account set -s <your-subscription-id>
    ```   

2. Utwórz grupę zasobów platformy Azure i zanotuj jej nazwę.

    ```bash
    az group create -n <your-azure-group-name> \
        -l <your-resource-group-region>
    ```

3. Utwórz bazę danych Azure Cosmos DB przy użyciu rodzaju `GlobalDocumentDB`. Nazwa bazy danych Cosmos DB musi zawierać tylko małe litery. Zanotuj pole `documentEndpoint` w odpowiedzi na polecenie.

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
    ```

4. Pobierz klucz bazy danych Azure Cosmos DB, aby nawiązać połączenie z aplikacją. Zachowaj `primaryMasterKey`, `documentEndpoint` znajduje się w pobliżu będą potrzebne im w następnym kroku.

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="configure-the-todo-app-properties"></a>Konfigurowanie właściwości aplikacji TODO

Otwórz okno terminalu na komputerze. Skopiuj przykładowy plik skryptu w sklonowanym repozytorium, aby można go było dostosować do właśnie utworzonej bazy danych Cosmos DB.

```bash
cd initial/spring-todo-app
cp set-env-variables-template.sh .scripts/set-env-variables.sh
```
 
Edytuj `.scripts/set-env-variables.sh` w ulubionym edytorze i podaj informacje o połączeniu bazy danych Azure Cosmos DB. W przypadku konfiguracji systemu Linux dla usługi App Service użyj tego samego regionu, jak poprzednio (`your-resource-group-region`), i grupy zasobów (`your-azure-group-name`) użytej podczas tworzenia bazy danych Cosmos DB. Wybierz nazwę WEBAPP_NAME, która jest unikatowa, ponieważ nie może ona być duplikatem jakiejkolwiek nazwy aplikacji internetowej w jakimkolwiek wdrożeniu platformy Azure.

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>

# App Service Linux Configuration
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

Następnie uruchom skrypt:

```bash
source .scripts/set-env-variables.sh
```
   
Te zmienne środowiskowe są używane w obszarze `application.properties` w aplikacji listy TODO. Pola w pliku właściwości określają domyślną konfigurację repozytorium dla danych platformy Spring:

```properties
azure.cosmosdb.uri=${COSMOSDB_URI}
azure.cosmosdb.key=${COSMOSDB_KEY}
azure.cosmosdb.database=${COSMOSDB_DBNAME}
```

```java
@Repository
public interface TodoItemRepository extends DocumentDbRepository<TodoItem, String> {
}
```

Następnie przykładowa aplikacja używa adnotacji `@Document` zaimportowanych z `com.microsoft.azure.spring.data.cosmosdb.core.mapping.Document` w celu konfigurowania typu jednostki do przechowania i zarządzania przez usługę Cosmos DB:

```java
@Document
public class TodoItem {
    private String id;
    private String description;
    private String owner;
    private boolean finished;
```

## <a name="run-the-sample-app"></a>Uruchamianie przykładowej aplikacji

Uruchom przykład przy użyciu narzędzia Maven.

```bash
mvn package spring-boot:run
```

Dane wyjściowe powinny wyglądać podobnie do poniższych.

```bash
bash-3.2$ mvn package spring-boot:run
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 


[INFO] SimpleUrlHandlerMapping - Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] SimpleUrlHandlerMapping - Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] WelcomePageHandlerMapping - Adding welcome page: class path resource [static/index.html]
2018-10-28 15:04:32.101  INFO 7673 --- [           main] c.m.azure.documentdb.DocumentClient      : Initializing DocumentClient with serviceEndpoint [https://sample-cosmos-db-westus.documents.azure.com:443/], ConnectionPolicy [ConnectionPolicy [requestTimeout=60, mediaRequestTimeout=300, connectionMode=Gateway, mediaReadMode=Buffered, maxPoolSize=800, idleConnectionTimeout=60, userAgentSuffix=;spring-data/2.0.6;098063be661ab767976bd5a2ec350e978faba99348207e8627375e8033277cb2, retryOptions=com.microsoft.azure.documentdb.RetryOptions@6b9fb84d, enableEndpointDiscovery=true, preferredLocations=null]], ConsistencyLevel [null]
[INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
[INFO] TomcatWebServer - Tomcat started on port(s): 8080 (http) with context path ''
[INFO] TodoApplication - Started TodoApplication in 45.573 seconds (JVM running for 76.534)
```

Dostęp do aplikacji TODO platformy Spring można uzyskać lokalnie za pomocą tego linku po uruchomieniu aplikacji: [http://localhost:8080/](http://localhost:8080/).

 ![](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

Jeśli widzisz wyjątki, zamiast komunikatu "TodoApplication uruchomiona", sprawdź, czy `bash` skryptu w poprzednim kroku wyeksportowane zmienne środowiskowe poprawnie i czy wartości są poprawne dla bazy danych Azure Cosmos DB utworzonego.

## <a name="configure-azure-deployment"></a>Konfigurowanie wdrożenia usługi Azure

Otwórz plik `pom.xml` w katalogu `initial/spring-boot-todo` i dodaj następującą konfigurację [wtyczki Maven dla usługi Azure App Service](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md).

```xml    
<plugins> 

    <!--*************************************************-->
    <!-- Deploy to Java SE in App Service Linux           -->
    <!--*************************************************-->
       
    <plugin>
        <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.4.0</version>
            <configuration>
            <deploymentType>jar</deploymentType>
            
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>jre8</linuxRuntime>
            
            <appSettings>
                <property>
                    <name>COSMOSDB_URI</name>
                    <value>${COSMOSDB_URI}</value>
                </property>
                <property>
                    <name>COSMOSDB_KEY</name>
                    <value>${COSMOSDB_KEY}</value>
                </property>
                <property>
                    <name>COSMOSDB_DBNAME</name>
                    <value>${COSMOSDB_DBNAME}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Dserver.port=80</value>
                </property>
            </appSettings>
            
        </configuration>
    </plugin>            
    ...
</plugins>
```

## <a name="deploy-to-app-service-on-linux"></a>Wdrażanie w usłudze App Service w systemie Linux

Użyj celu `azure-webapp:deploy` narzędzia Maven, aby wdrożyć aplikację TODO w usłudze Azure App Service w systemie Linux.

```bash

# Deploy
bash-3.2$ mvn azure-webapp:deploy
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- azure-webapp-maven-plugin:1.4.0:deploy (default-cli) @ spring-todo-app ---
[INFO] Authenticate with Azure CLI 2.0
[INFO] Target Web App doesn't exist. Creating a new one...
[INFO] Creating App Service Plan 'ServicePlanb6ba8178-5bbb-49e7'...
[INFO] Successfully created App Service Plan.
[INFO] Successfully created Web App.
[INFO] Trying to deploy artifact to spring-todo-app...
[INFO] Successfully deployed the artifact to https://spring-todo-app.azurewebsites.net
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:19 min
[INFO] Finished at: 2018-10-28T15:32:03-07:00
[INFO] Final Memory: 50M/574M
[INFO] ------------------------------------------------------------------------
```

Dane wyjściowe zawierają adres URL wdrożonej aplikacji (w tym przykładzie: `https://spring-todo-app.azurewebsites.net`). Możesz skopiować ten adres URL do przeglądarki internetowej lub uruchomić następujące polecenie w oknie terminalu, aby załadować aplikację.

```bash
open https://spring-todo-app.azurewebsites.net
```

Powinna zostać wyświetlona aplikacja działająca ze zdalnym adresem URL na pasku adresu:

 ![](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-in-app-service.jpg)

## <a name="stream-diagnostic-logs"></a>Przesyłanie strumieniowe dzienników diagnostycznych

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]


## <a name="scale-out-the-todo-app"></a>Skalowanie w poziomie aplikacji TODO

Skaluj aplikację w poziomie, dodając inny proces roboczy:

```bash
az appservice plan update --number-of-workers 2 \
   --name ${WEBAPP_PLAN_NAME} \
   --resource-group <your-azure-group-name>
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie potrzebujesz tych zasobów w innym samouczku (zobacz [Następne kroki](#next)), możesz je usunąć, uruchamiając następujące polecenie w usłudze Cloud Shell: 
  
```bash
az group delete --name <your-azure-group-name>
```

<a name="next"></a>

## <a name="next-steps"></a>Kolejne kroki

[Platforma Azure dla deweloperów języka Java](/java/azure/)
[Spring Boot](https://spring.io/projects/spring-boot), [Spring Data for Cosmos DB](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable), [Azure Cosmos DB](/azure/cosmos-db/sql-api-introduction) i [App Service Linux](app-service-linux-intro.md).

Dowiedz się więcej na temat uruchamiania aplikacji w języku Java w usłudze App Service dla systemu Linux w przewodniku dla deweloperów.

> [!div class="nextstepaction"] 
> [Java in App Service Linux dev guide](configure-language-java.md) (Przewodnik dla deweloperów dotyczący języka Java w usłudze App Service dla systemu Linux)
