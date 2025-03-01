---
title: Wdrażanie zadania usługi Azure Stream Analytics z ciągłą integracją/ciągłym wdrażaniem przy użyciu usługi Azure DevOps
description: W tym artykule opisano sposób wdrażania zadania usługi Stream Analytics z ciągłą integracją/ciągłym wdrażaniem przy użyciu usługi Azure DevOps Services.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: tutorial
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 9e05e4eab8bd3c307334b62df00dc03e56ce60ad
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60763038"
---
# <a name="tutorial-deploy-an-azure-stream-analytics-job-with-cicd-using-azure-pipelines"></a>Samouczek: Wdrażanie zadania usługi Azure Stream Analytics z ciągłą integracją/ciągłym wdrażaniem przy użyciu usługi Azure Pipelines
W tym samouczku opisano konfigurowanie ciągłej integracji i ciągłego wdrażania na potrzeby zadania usługi Azure Stream Analytics przy użyciu usługi Azure Pipelines. 

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dodawanie kontroli źródła do projektu
> * Tworzenie potoku kompilacji w usłudze Azure Pipelines
> * Tworzenie potoku wydania w usłudze Azure Pipelines
> * Automatyczne wdrażanie i uaktualnianie aplikacji

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem upewnij się, że dysponujesz następującymi elementami:

* Jeśli nie masz subskrypcji platformy Azure, utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Zainstaluj program [Visual Studio](stream-analytics-tools-for-visual-studio-install.md) i obciążenie **Programowanie na platformie Azure** lub **Magazynowanie i przetwarzanie danych**.
* Utwórz [projekt usługi Stream Analytics w programie Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-vs).
* Utwórz organizację usługi [Azure DevOps](https://visualstudio.microsoft.com/team-services/).

## <a name="configure-nuget-package-dependency"></a>Konfigurowanie zależności pakietu NuGet
W celu wykonania automatycznej kompilacji i automatycznego wdrożenia na dowolnej maszynie, musisz użyć pakietu NuGet `Microsoft.Azure.StreamAnalytics.CICD`. Zapewnia on program MSBuild, uruchamianie lokalne oraz narzędzia wdrażania, które obsługują proces ciągłej integracji i ciągłego wdrażania projektów programu Visual Studio dla usługi Stream Analytics. Aby uzyskać więcej informacji, zobacz [Stream Analytics CI/CD tools (Narzędzia ciągłej integracji/ciągłego wdrażania usługi Stream Analytics)](stream-analytics-tools-for-visual-studio-cicd.md).

Dodaj plik **packages.config** do katalogu projektu.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
<package id="Microsoft.Azure.StreamAnalytics.CICD" version="1.0.0" targetFramework="net452" />
</packages>
```

## <a name="share-your-visual-studio-solution-to-a-new-azure-repos-git-repo"></a>Udostępnianie rozwiązania programu Visual Studio w nowym repozytorium Git w usłudze Azure Repos

Udostępnij pliki źródłowe aplikacji w projekcie zespołowym usługi Azure DevOps, aby umożliwić generowanie kompilacji.  

1. Utwórz nowe lokalne repozytorium Git dla projektu, wybierając pozycję **Dodaj do kontroli źródła**, a następnie pozycję **Git** na pasku stanu w prawym dolnym rogu programu Visual Studio. 

2. W widoku **Synchronizacja** w programie **Team Explorer** wybierz przycisk **Opublikuj repozytorium Git** w obszarze **Wypychanie do usługi Azure DevOps Services**.

   ![Przycisk Opublikuj repozytorium Git w obszarze Wypychanie do usługi Azure DevOps Services](./media/stream-analytics-tools-visual-studio-cicd-vsts/publish-git-repo-devops.png)

3. Sprawdź swój adres e-mail i z listy rozwijanej **Domena usługi Azure DevOps Services** wybierz swoją organizację. Wprowadź nazwę repozytorium i wybierz polecenie **Publikuj repozytorium**.

   ![Przycisk Publikuj repozytorium w obszarze Wypchnij repozytorium Git](./media/stream-analytics-tools-visual-studio-cicd-vsts/publish-repository-devops.png)

    Opublikowanie repozytorium powoduje utworzenie nowego projektu w Twojej organizacji o takiej samej nazwie co lokalne repozytorium. Aby utworzyć repozytorium w istniejącym projekcie, kliknij pozycję **Zaawansowane** obok pola **Nazwa repozytorium**, a następnie wybierz projekt. Aby wyświetlić kod w przeglądarce, wybierz polecenie **Wyświetl w Internecie**.
 
## <a name="configure-continuous-delivery-with-azure-devops"></a>Konfigurowanie ciągłego dostarczania za pomocą usługi Azure DevOps
Potok kompilacji usługi Azure Pipelines opisuje przepływ pracy, który składa się z kroków kompilacji wykonywanych sekwencyjnie. Dowiedz się więcej o [potokach kompilacji usługi Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav). 

Potok wydania usługi Azure Pipelines opisuje przepływ pracy, który wdraża pakiet aplikacji w klastrze. Jednoczesne użycie potoku kompilacji i potoku wydania powoduje wykonanie całego przepływu pracy, zaczynając od plików źródłowych, a kończąc na aplikacji uruchomionej w klastrze. Dowiedz się więcej o [potokach kompilacji](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts) usługi Azure Pipelines.

### <a name="create-a-build-pipeline"></a>Tworzenie potoku kompilacji
Otwórz przeglądarkę internetową i przejdź do nowo utworzonego projektu w usłudze [Azure DevOps](https://app.vsaex.visualstudio.com/). 

1. Na karcie **Kompilowanie i wydawanie** wybierz pozycję **Kompilacje**, a następnie opcję **+Nowa**.  Wybierz kolejno pozycje **Azure DevOps Services Git** i **Kontynuuj**.
    
    ![Wybieranie źródła DevOps Git w usłudze Azure DevOps](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-select-source-devops.png)

2. W obszarze **Wybierz szablon** kliknij pozycję **Pusty proces**, aby rozpocząć od pustego potoku.
    
    ![Wybieranie pozycji Pusty proces w opcjach szablonu w usłudze DevOps](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-select-template-empty-process.png)

3. W obszarze **Wyzwalacze** włącz ciągłą integrację, zaznaczając stan wyzwalacza **Włącz ciągłą integrację**.  Wybierz pozycję **Zapisz i dodaj do kolejki**, aby ręcznie uruchomić kompilację. 
    
    ![Włączanie stanu wyzwalania ciągłej integracji](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-trigger-status-ci.png)

4. Kompilacje są również wyzwalane przez wypchnięcie lub zaewidencjonowanie. Aby sprawdzić postęp kompilacji, przejdź do karty **Kompilacje**.  Po upewnieniu się, że kompilacja jest wykonywana prawidłowo, musisz określić potok wydania, który wdraża aplikację w klastrze. Kliknij prawym przyciskiem myszy wielokropek obok potoku kompilacji i wybierz pozycję **Edytuj**.

5.  W obszarze **Zadania** wprowadź wartość „Hostowana” w polu **Kolejka agentów**.
    
    ![Wybieranie kolejki agentów w menu Zadania](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-agent-queue-task.png) 

6. W obszarze **Faza 1** kliknij znak **+** i dodaj zadanie **NuGet**.
    
    ![Dodawanie zadania NuGet w kolejce agentów](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-add-nuget-task.png)

7. Rozwiń sekcję **Zaawansowane** i dodaj ścieżkę `$(Build.SourcesDirectory)\packages` w polu **Katalog docelowy**. Pozostałe wartości domyślne konfiguracji NuGet pozostaw bez zmian.

   ![Konfigurowanie zadania przywracania w rozwiązaniu NuGet](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-nuget-restore-config.png)

8. W obszarze **Faza 1** kliknij znak **+** i dodaj zadanie **MSBuild**.

   ![Dodawanie zadania MSBuild w kolejce agentów](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-add-msbuild-task.png)

9. Zmień wartość w polu **Argumenty MSBuild** na następującą:

   ```
   /p:CompilerTaskAssemblyFile="Microsoft.WindowsAzure.StreamAnalytics.Common.CompileService.dll"  /p:ASATargetsFilePath="$(Build.SourcesDirectory)\packages\Microsoft.Azure.StreamAnalytics.CICD.1.0.0\build\StreamAnalytics.targets"
   ```

   ![Konfigurowanie zadania MSBuild w usłudze DevOps](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-config-msbuild-task.png)

10. W obszarze **Faza 1** kliknij znak **+** i dodaj zadanie **Wdrożenie grupy zasobów platformy Azure**. 
    
    ![Dodawanie zadania Wdrożenie grupy zasobów platformy Azure](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-add-resource-group-deployment.png)

11. Rozwiń pozycję **Szczegóły platformy Azure** i wypełnij konfigurację następującymi informacjami:
    
    |**Ustawienie**  |**Sugerowana wartość**  |
    |---------|---------|
    |Subskrypcja  |  Wybierz subskrypcję.   |
    |Akcja  |  Utwórz lub zaktualizuj grupę zasobów   |
    |Grupa zasobów  |  Wprowadź nazwę grupy zasobów.   |
    |Szablon  | [ścieżka do Twojego rozwiązania]\bin\Debug\Deploy\\[nazwa Twojego projektu].JobTemplate.json   |
    |Parametry szablonu  | [ścieżka do Twojego rozwiązania]\bin\Debug\Deploy\\[nazwa Twojego projektu].JobTemplate.parameters.json   |
    |Zastąp parametry szablonu  | W polu tekstowym wpisz parametry szablonu, które mają zostać zastąpione. Na przykład –storageName fabrikam –adminUsername $(vmusername) –adminPassword $(password) –azureKeyVaultName $(fabrikamFibre). Ta właściwość jest opcjonalna, ale Twoja kompilacja będzie powodować błędy, jeśli kluczowe parametry nie zostaną zastąpione.    |
    
    ![Ustawianie właściwości dla wdrożenia grupy zasobów platformy Azure](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-deployment-properties.png)

12. Kliknij przycisk **Zapisz i dodaj do kolejki**, aby przetestować potok kompilacji.
    
    ![Zapisywanie kompilacji w usłudze DevOps i dodawanie jej do kolejki](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-save-and-queue-build.png)

### <a name="failed-build-process"></a>Niepowodzenie procesu kompilacji
Jeśli parametry szablonu nie zostały zastąpione w zadaniu **Wdrożenie grupy zasobów platformy Azure** Twojego potoku kompilacji, mogą wystąpić błędy dotyczące parametrów wdrożenia mających wartość null. Wróć do potoku kompilacji i zastąp parametry mające wartość null, aby naprawić błąd.

   ![Proces kompilowania usługi DevOps Stream Analytics zakończył się niepowodzeniem](./media/stream-analytics-tools-visual-studio-cicd-vsts/devops-build-process-failed.png)

### <a name="commit-and-push-changes-to-trigger-a-release"></a>Zatwierdzanie i wypychanie zmian w celu wyzwolenia wydania
Upewnij się, że potok ciągłej integracji działa prawidłowo przez zaewidencjonowanie zmian kodu w usłudze Azure DevOps.    

Podczas pisania kodu zmiany są automatycznie śledzone przez program Visual Studio. Zatwierdź zmiany w lokalnym repozytorium Git, wybierając ikonę oczekujących zmian na pasku stanu w prawym dolnym rogu.

1. W widoku **Zmiany** programu Team Explorer dodaj komunikat opisujący aktualizację i zatwierdź wprowadzone zmiany.

    ![Zatwierdzanie zmian w repozytorium z poziomu programu Visual Studio](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-commit-changes-visual-studio.png)

2. Wybierz ikonę paska stanu nieopublikowanych zmian lub widok synchronizacji w programie Team Explorer. Wybierz pozycję **Wypchnij**, aby zaktualizować kod w usłudze Azure DevOps Services.

    ![Wypychanie zmian z poziomu programu Visual Studio](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-push-changes-visual-studio.png)

Wypychanie zmian do usługi Azure DevOps Services automatycznie wyzwala kompilację.  Po pomyślnym zakończeniu potoku kompilacji automatycznie tworzone jest wydanie, które rozpoczyna aktualizację zadania w klastrze.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy grupa zasobów, zadanie przesyłania strumieniowego i wszystkie pokrewne zasoby nie będą już potrzebne, usuń je. Usunięcie zadania pozwala uniknąć opłat za jednostki przesyłania strumieniowego zużywane przez zadanie. Jeśli planujesz użycie zadania w przyszłości, możesz zatrzymać je i uruchomić ponownie później w razie potrzeby. Jeśli nie zamierzasz w przyszłości korzystać z tego zadania, wykonaj następujące kroki, aby usunąć wszystkie zasoby utworzone w ramach tego samouczka:

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu.  
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej na temat konfigurowania procesu ciągłej integracji i ciągłego wdrażania przy użyciu narzędzi Azure Stream Analytics Tools for Visual Studio, przejdź do artykułu na temat konfigurowania potoku ciągłej integracji/ciągłego wdrażania:

> [!div class="nextstepaction"]
> [Ciągła integracja i ciągłe opracowywanie za pomocą narzędzi usługi Stream Analytics](stream-analytics-tools-for-visual-studio-cicd.md)
