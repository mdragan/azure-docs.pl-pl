---
title: Konfigurowanie potoku ciągłej integracji/ciągłego wdrażania przy użyciu zadania kompilacji emulatora usługi Azure Cosmos DB
description: Samouczek dotyczący sposobu konfigurowania przepływu pracy kompilacji i wydawania w usłudze Azure DevOps przy użyciu zadania kompilacji emulatora usługi Cosmos DB
author: deborahc
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 05/23/2019
ms.author: dech
ms.reviewer: sngun
ms.openlocfilehash: 19ced9767d77b0d7bfcec6f01425ab1089a55d54
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069224"
---
# <a name="set-up-a-cicd-pipeline-with-the-azure-cosmos-db-emulator-build-task-in-azure-devops"></a>Konfigurowanie potoku ciągłej integracji/ciągłego wdrażania przy użyciu zadania kompilacji emulatora usługi Azure Cosmos DB w usłudze Azure DevOps

Emulator usługi Azure Cosmos DB zapewnia środowisko lokalne, które emuluje usługę Azure Cosmos DB do celów programistycznych. Emulator umożliwia lokalne opracowywanie i testowanie aplikacji bez konieczności tworzenia subskrypcji platformy Azure i ponoszenia kosztów. 

Zadanie kompilacji emulatora usługi Azure Cosmos DB dla usługi Azure DevOps umożliwia wykonanie tych czynności w środowisku ciągłej integracji. Dzięki zadaniu kompilacji możesz uruchamiać testy z użyciem emulatora w ramach przepływów pracy kompilacji i wydania. Zadanie uruchamia kontener platformy Docker z uruchomionym emulatorem i dostarcza punkt końcowy, który może być używany przez pozostałą część definicji kompilacji. Możesz utworzyć i uruchomić dowolną liczbę wystąpień emulatora — każde w oddzielnym kontenerze. 

W tym artykule pokazano, jak skonfigurować potok ciągłej integracji w usłudze Azure DevOps dla aplikacji ASP.NET, w której wykorzystano zadanie kompilacji emulatora usługi Cosmos DB w celu wykonywania testów. Za pomocą podobnego podejścia można skonfigurować potok ciągłej integracji dla aplikacji Node.js lub Python. 

## <a name="install-the-emulator-build-task"></a>Instalowanie zadania kompilacji emulatora

Aby użyć zadania kompilacji, najpierw należy zainstalować je w ramach organizacji usługi Azure DevOps. Znajdź rozszerzenie **Azure Cosmos DB Emulator** w witrynie [Marketplace](https://marketplace.visualstudio.com/items?itemName=azure-cosmosdb.emulator-public-preview) i kliknij polecenie **Get it free** (Uzyskaj bezpłatnie).

![Znajdowanie i instalowanie zadania kompilacji emulatora usługi Azure Cosmos DB w witrynie Marketplace usługi Azure DevOps](./media/tutorial-setup-ci-cd/addExtension_1.png)

Następnie wybierz organizację, w której chcesz zainstalować rozszerzenie. 

> [!NOTE]
> Aby zainstalować rozszerzenie organizacji DevOps platformy Azure, musi być właścicielem konta lub administrator kolekcji projektów. Jeśli nie masz uprawnień, ale jesteś użytkownikiem konta, możesz w takiej sytuacji zażądać rozszerzenia. [Dowiedz się więcej.](https://docs.microsoft.com/azure/devops/marketplace/faq-extensions?view=vsts#install-request-assign-and-access-extensions)

![Wybierz organizację DevOps platformy Azure, w którym chcesz zainstalować rozszerzenie](./media/tutorial-setup-ci-cd/addExtension_2.png)

## <a name="create-a-build-definition"></a>Utwórz definicję kompilacji

Teraz, gdy rozszerzenie jest zainstalowane, zaloguj się do swojego konta usługi Azure DevOps i znajdź swój projekt na pulpicie nawigacyjnym projektów. Do projektu możesz dodać [potok kompilacji](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav) lub zmodyfikować istniejący potok kompilacji. Jeśli masz już potok kompilacji, możesz przejść od razu do sekcji [Dodawanie zadania kompilacji emulatora do definicji kompilacji](#addEmulatorBuildTaskToBuildDefinition).

1. Aby utworzyć nową definicję kompilacji, przejdź do karty **Builds** (Kompilacje) w usłudze Azure DevOps. Wybierz pozycję **+New** (+Nowe). \> **Nowy potok kompilacji**

   ![Tworzenie nowego potoku kompilacji](./media/tutorial-setup-ci-cd/CreateNewBuildDef_1.png)

2. Wybierz żądane **źródło** oraz odpowiednie opcje w polach **Team project** (Projekt zespołowy), **Repository** (Repozytorium) i **Default branch for manual and scheduled builds** (Domyślna gałąź dla kompilacji ręcznych i zaplanowanych). Po wybraniu wymaganych opcji wybierz przycisk **Continue** (Kontynuuj)

   ![Wybieranie projektu zespołowego, repozytorium i gałęzi dla potoku kompilacji](./media/tutorial-setup-ci-cd/CreateNewBuildDef_2.png)

3. Na koniec wybierz odpowiedni szablon dla potoku kompilacji. Do celów tego samouczka wybierzemy szablon **ASP.NET**. 

> [!NOTE]
> Pula agentów należy wybrać ten element CI powinny mieć Docker for Windows zainstalowany, chyba, że instalacja odbywa się ręcznie w poprzednie zadanie jako część elementu konfiguracji. Zobacz [agentów hostowanych przez Microsoft](https://docs.microsoft.com/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml) artykuł dotyczący wybór pul agentów; zaleca się zaczynać `Hosted VS2017` lub `Hosted VS2019`. 

Mamy już potok kompilacji, który możemy skonfigurować, aby użyć zadania kompilacji emulatora usługi Azure Cosmos DB. 

## <a name="addEmulatorBuildTaskToBuildDefinition"></a>Dodawanie zadania do potoku kompilacji

1. Przed dodaniem zadania do potoku kompilacji należy dodać zadanie agenta. Przejdź do potoku kompilacji, wybierz pozycję **...** i wybierz polecenie **Add an agent job** (Dodaj zadanie agenta).

1. Następnie wybierz symbol **+** obok zadania agenta, aby dodać zadanie kompilacji emulatora. Wyszukaj ciąg **cosmos** w polu wyszukiwania, wybierz pozycję **Azure Cosmos DB Emulator** (Emulator usługi Azure Cosmos DB) i dodaj ten emulator do zadania agenta. Zadanie kompilacji będzie uruchamiać kontener, na którym już będzie uruchomione wystąpienie emulatora usługi Cosmos DB. Zadanie emulatora usługi Azure Cosmos DB powinno znajdować się przed wszystkimi innymi zadaniami, które oczekują, że emulator będzie działać.

   ![Dodawanie zadania kompilacji emulatora do definicji kompilacji](./media/tutorial-setup-ci-cd/addExtension_3.png)

W tym samouczku dodasz zadanie na początku, aby upewnić się, że emulator jest dostępny przed rozpoczęciem wykonywania naszych testów.

## <a name="configure-tests-to-use-the-emulator"></a>Konfigurowanie testów w celu używania emulatora

Teraz skonfigurujemy nasze testy tak, aby używały emulatora. Zadanie kompilacji emulatora eksportuje zmienną środowiskową „CosmosDbEmulator.Endpoint”, wobec której wszystkie zadania w dalszej części potoku kompilacji mogą wysyłać żądania. 

W tym samouczku użyjemy zadania [Visual Studio Test](https://github.com/Microsoft/azure-pipelines-tasks/blob/master/Tasks/VsTestV2/README.md), aby uruchomić testy jednostkowe skonfigurowane za pomocą pliku **.runsettings**. Aby dowiedzieć się więcej na temat konfigurowania testów jednostkowych, zapoznaj się z [dokumentacją](https://docs.microsoft.com/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file?view=vs-2017). Kompletny przykład kodu aplikacji z listą zadań do wykonania używany w tym dokumencie jest dostępny w serwisie [Github](https://github.com/Azure-Samples/documentdb-dotnet-todo-app)

Poniżej przedstawiono przykładowy plik **.runsettings**, który definiuje parametry przekazywane do testów jednostkowych aplikacji. Należy pamiętać, że używana zmienna `authKey` to [dobrze znany klucz](https://docs.microsoft.com/azure/cosmos-db/local-emulator#authenticating-requests) emulatora. Ta zmienna `authKey` to klucz oczekiwany przez zadanie kompilacji emulatora i powinna być zdefiniowana w pliku **.runsettings**.

```csharp
<RunSettings>
    <TestRunParameters>
    <Parameter name="endpoint" value="https://localhost:8081" />
    <Parameter name="authKey" value="C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==" />
    <Parameter name="database" value="ToDoListTest" />
    <Parameter name="collection" value="ItemsTest" />
  </TestRunParameters>
</RunSettings>
```

Jeśli konfigurujesz potok ciągłej integracji/ciągłego wdrażania dla aplikacji, która używa interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB, to parametry połączenia bazy danych MongoDB domyślnie zawierają numer portu 10255. Jednak ten port nie jest obecnie otwarty. Jako alternatywny należy używać portu 10250 w celu ustanowienia połączenia. Parametry połączenia interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB pozostają takie same, z wyjątkiem obsługiwanego numeru portu: 10250 zamiast 10255.

Te parametry `TestRunParameters` są wywoływane za pośrednictwem właściwości `TestContext` w projekcie testowym aplikacji. Oto przykład testu wykonywanego względem usługi Cosmos DB.

```csharp
namespace todo.Tests
{
    [TestClass]
    public class TodoUnitTests
    {
        public TestContext TestContext { get; set; }

        [TestInitialize()]
        public void Initialize()
        {
            string endpoint = TestContext.Properties["endpoint"].ToString();
            string authKey = TestContext.Properties["authKey"].ToString();
            Console.WriteLine("Using endpoint: ", endpoint);
            DocumentDBRepository<Item>.Initialize(endpoint, authKey);
        }
        [TestMethod]
        public async Task TestCreateItemsAsync()
        {
            var item = new Item
            {
                Id = "1",
                Name = "testName",
                Description = "testDescription",
                Completed = false,
                Category = "testCategory"
            };

            // Create the item
            await DocumentDBRepository<Item>.CreateItemAsync(item);
            // Query for the item
            var returnedItem = await DocumentDBRepository<Item>.GetItemAsync(item.Id, item.Category);
            // Verify the item returned is correct.
            Assert.AreEqual(item.Id, returnedItem.Id);
            Assert.AreEqual(item.Category, returnedItem.Category);
        }

        [TestCleanup()]
        public void Cleanup()
        {
            DocumentDBRepository<Item>.Teardown();
        }
    }
}
```

Przejdź do opcji wykonywania w zadaniu Visual Studio Test. W opcji **Settings file** (Plik ustawień) określ, czy testy zostały skonfigurowane przy użyciu pliku **.runsettings**. W opcji **Override test run parameters** (Zastąp parametry przebiegu testu) dodaj element `-endpoint $(CosmosDbEmulator.Endpoint)`. W ten sposób skonfigurujesz zadanie Test tak, aby odwoływało się do punktu końcowego zadania kompilacji emulatora, a nie do punktu końcowego zdefiniowanego w pliku **.runsettings**.  

![Zastępowanie zmiennej punktu końcowego punktem końcowym zadania kompilacji emulatora](./media/tutorial-setup-ci-cd/addExtension_5.png)

## <a name="run-the-build"></a>Uruchamianie kompilacji

Teraz zapisz kompilację i dodaj ją do kolejki, wybierając pozycję **Save and queue** (Zapisz i dodaj do kolejki). 

![Zapisywanie i uruchamianie kompilacji](./media/tutorial-setup-ci-cd/runBuild_1.png)

Po uruchomieniu kompilacji zwróć uwagę, że zadanie emulatora usługi Cosmos DB rozpoczęło ściąganie obrazu platformy Docker z zainstalowanym emulatorem. 

![Zapisywanie i uruchamianie kompilacji](./media/tutorial-setup-ci-cd/runBuild_4.png)

Po ukończeniu kompilacji zwróć uwagę, że testy uruchomione względem emulatora usługi Cosmos DB za pomocą zadania kompilacji zakończyły się powodzeniem.

![Zapisywanie i uruchamianie kompilacji](./media/tutorial-setup-ci-cd/buildComplete_1.png)

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat korzystania z emulatora do celów lokalnego programowania i testowania, zobacz [Programowanie i testowanie w środowisku lokalnym przy użyciu emulatora usługi Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/local-emulator).

Aby wyeksportować certyfikaty SSL emulatora, zobacz [Eksportowanie certyfikatów emulatora usługi Azure Cosmos DB do użycia w językach Java, Python i Node.js](https://docs.microsoft.com/azure/cosmos-db/local-emulator-export-ssl-certificates)
