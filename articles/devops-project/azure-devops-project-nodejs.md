---
title: 'Szybki start: Tworzenie potoku ciągłej integracji/ciągłego wdrażania dla środowiska Node.js w usłudze Azure DevOps Projects'
description: Usługa DevOps Projects ułatwia rozpoczęcie pracy na platformie Azure. Umożliwia uruchomienie aplikacji w wybranej usłudze platformy Azure w kilku prostych krokach.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: dadc394e9c9ddb152923095de10b8f727310d3f7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60554833"
---
#  <a name="create-a-cicd-pipeline-for--nodejs-with-azure-devops-projects"></a>Tworzenie potoku ciągłej integracji/ciągłego wdrażania dla środowiska Node.js w usłudze Azure DevOps Projects 

Usługa Azure DevOps Projects oferuje prosty sposób na utworzenie zasobów platformy Azure i skonfigurowanie potoku ciągłej integracji/ciągłego wdrażania dla aplikacji Node.js w usłudze Azure Pipelines.  

Jeśli nie masz subskrypcji platformy Azure, możesz uzyskać ją bezpłatnie za pośrednictwem programu [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Usługa DevOps Projects tworzy potok ciągłej integracji/ciągłego wdrażania w usłudze Azure Pipelines. Możesz utworzyć nową organizację usługi Azure DevOps lub użyć istniejącej organizacji. Usługa DevOps Projects tworzy również zasoby platformy Azure w wybranej subskrypcji platformy Azure.

1. Zaloguj się do [Portalu Microsoft Azure](https://portal.azure.com).

1. W okienku po lewej stronie wybierz pozycję **Utwórz zasób**, a następnie wyszukaj pozycję **DevOps Projects**. 

    ![Rozpoczynanie konfigurowania ciągłego dostarczania](_img/azure-devops-project-nodejs/fullbrowser.png)

## <a name="select-a-sample-application-and-azure-service"></a>Wybieranie przykładowej aplikacji i usługi platformy Azure

1. Wybierz przykładową aplikację środowiska Node.js.  
    Przykłady środowiska Node.js obejmują kilka struktur aplikacji do wyboru.

1. Domyślna struktura przykładowej aplikacji to struktura Express.js. Pozostaw ustawienie domyślne, a następnie wybierz pozycję **Dalej**.  
    Domyślnym celem wdrożenia jest funkcja **Web App on Windows**.  Wybrana poprzednio struktura aplikacji decyduje o dostępnym w tym miejscu typie celu wdrożenia usługi platformy Azure.  

2. Pozostaw usługę domyślną, a następnie wybierz pozycję **Dalej**.
 
## <a name="configure-azure-devops-and-an-azure-subscription"></a>Konfigurowanie usługi Azure DevOps i subskrypcji platformy Azure 

1. Utwórz nową organizację usługi Azure DevOps lub wybierz istniejącą organizację. 

    a. Wybierz nazwę projektu. 

    b. Wybierz lokalizację i subskrypcję platformy Azure, wybierz nazwę aplikacji, a następnie wybierz przycisk **Gotowe**.  
    Po kilku minutach w witrynie Azure Portal zostanie wyświetlony pulpit nawigacyjny usługi DevOps Projects. Aplikacja przykładowa zostanie skonfigurowana w repozytorium w organizacji usługi Azure DevOps, skompilowana i wdrożona na platformie Azure. Ten pulpit nawigacyjny zapewnia wgląd w repozytorium kodu, potok ciągłej integracji/ciągłego wdrażania i aplikację na platformie Azure.
     
3. Wybierz pozycję **Przeglądaj**, aby wyświetlić uruchomioną aplikację.

    ![Widok pulpitu nawigacyjnego](_img/azure-devops-project-nodejs/dashboardnopreview.png) 
    
Usługa DevOps Projects automatycznie skonfigurowała wyzwalacz kompilacji i wydania ciągłej integracji.  Możesz teraz rozpocząć pracę w zespole nad aplikacją Node.js w ramach procesu ciągłej integracji/ciągłego wdrażania, który umożliwia automatyczne wdrożenie najnowszej wersji w witrynie internetowej.

## <a name="commit-code-changes-and-execute-cicd"></a>Zatwierdzanie zmian kodu i wykonywanie ciągłej integracji/ciągłego wdrażania

Usługa DevOps Projects tworzy repozytorium Git w usłudze Azure Repos lub GitHub. Aby wyświetlić repozytorium i wprowadzić zmiany w kodzie aplikacji, wykonaj poniższe czynności.

1. Z lewej strony pulpitu nawigacyjnego usługi DevOps Projects wybierz link dla gałęzi master.  
Ten link otwiera widok nowo utworzonego repozytorium Git.

1. Aby wyświetlić adres URL klonowania repozytorium, wybierz pozycję **Klonuj** w prawym górnym rogu przeglądarki.   
    Możesz sklonować repozytorium Git w wybranym środowisku IDE. W kolejnych kilku krokach użyjesz przeglądarki internetowej, aby dokonać zmian w kodzie i zatwierdzić je bezpośrednio w gałęzi master.

1. Przejdź do pliku **views/index.pug** z lewej strony okna przeglądarki.

1. Wybierz pozycję **Edytuj** i wprowadź zmianę w nagłówku h2.  
    Wpisz na przykład hasło **Zaczynamy pracę z projektem DevOps platformy Azure** lub wprowadź inną zmianę.

1. Wybierz pozycję **Zatwierdź**, a następnie zapisz zmiany.

1. W przeglądarce przejdź do pulpitu nawigacyjnego usługi DevOps Projects.   
W tym momencie powinna być widoczna trwająca kompilacja. Wprowadzone zmiany są automatycznie kompilowane i wdrażane za pośrednictwem potoku ciągłej integracji/ciągłego wdrażania.

## <a name="examine-the-azure-cicd-pipeline"></a>Sprawdzanie potoku ciągłej integracji/ciągłego wdrażania platformy Azure

W poprzednim kroku usługa DevOps Projects automatycznie skonfigurowała pełny potok ciągłej integracji/ciągłego wdrażania. Możesz przeglądać i dostosowywać potok według potrzeb. Wykonaj poniższe kroki, aby zapoznać się z potokami kompilacji i wydania.

1. U góry pulpitu nawigacyjnego usługi DevOps Projects wybierz pozycję **Potoki kompilacji**.  
Ten link otwiera kartę przeglądarki i potok kompilacji dla nowego projektu.

1. Wskaż pole **Stan** i wybierz symbol wielokropka (...).  
    Ta akcja powoduje otwarcie menu, w którym możesz uruchomić kilka działań, takich jak dodawanie nowej kompilacji do kolejki, wstrzymywanie kompilacji i edytowanie potoku kompilacji.

1. Wybierz pozycję **Edit** (Edytuj).

1. W tym okienku możesz zapoznać się z różnymi zadaniami w potoku kompilacji.  
W ramach kompilacji są wykonywane różne zadania, takie jak pobieranie kodu źródłowego z repozytorium Git, przywracanie zależności i publikowanie danych wyjściowych używanych do wdrożenia.

1. W górnej części potoku kompilacji wybierz jego nazwę.

1. Zmień nazwę potoku kompilacji na bardziej opisową, wybierz pozycję **Zapisz i dodaj do kolejki**, a następnie wybierz pozycję **Zapisz**.

1. W obszarze nazwy potoku kompilacji wybierz pozycję **Historia**.   
W okienku **Historia** zostanie wyświetlony dziennik inspekcji zmian ostatnio wprowadzonych w kompilacji.  Usługa Azure Pipelines śledzi wszelkie zmiany wprowadzone do potoku kompilacji i pozwala na porównanie wersji.

1. Wybierz pozycję **Wyzwalacze**.   
 Usługa DevOps Projects automatycznie utworzyła wyzwalacz ciągłej integracji — każde zatwierdzenie w repozytorium uruchamia nową kompilację.  Opcjonalnie możesz zdecydować się dołączyć gałęzie do procesu ciągłej integracji lub je wykluczyć.

1. Wybierz pozycję **Przechowywanie**.   
W zależności od scenariusza możesz określić zasady przechowywania lub usuwania pewnej liczby kompilacji.

1. Wybierz pozycję **Kompilacja i wydanie**, a następnie wybierz pozycję **Wydania**.  
 Usługa DevOps Projects tworzy potok wydania w celu zarządzania wdrożeniami na platformie Azure.

1. Po lewej stronie wybierz symbol wielokropka (...) obok potoku wydania, a następnie wybierz pozycję **Edytuj**.  
Potok wydania definiuje proces tworzenia wydania.

12. W obszarze **Artefakty** wybierz polecenie **Porzuć**.  
    Potok kompilacji przedstawiony w poprzednich krokach generuje dane wyjściowe używane na potrzeby artefaktu. 

1. Obok ikony **Porzuć** wybierz pozycję **Wyzwalacz ciągłego wdrażania**.  
Ten potok wydania ma włączony wyzwalacz ciągłego wdrażania, który przeprowadza wdrożenie za każdym razem, gdy jest dostępny nowy artefakt kompilacji. Opcjonalnie możesz wyłączyć wyzwalacz. Wtedy wdrożenia będą wymagać ręcznego wykonania. 


1. Po lewej stronie wybierz pozycję **Zadania**.   
Zadania to działania wykonywane w procesie wdrażania. W tym przykładzie zostało utworzone zadanie w celu wdrożenia w usłudze Azure App Service.


1. Po prawej stronie wybierz pozycję **Wyświetl wydania**.  
Ten widok przedstawia historię wersji.

1. Wybierz symbol wielokropka (...) obok jednego z wydań i wybierz pozycję **Otwórz**.  
Dostępnych jest kilka menu, z którymi możesz się zapoznać, na przykład podsumowanie wydań, skojarzone elementy robocze i testy.

1. Wybierz pozycję **Zatwierdzenia**.   
Ten widok przedstawia zatwierdzenia kodu skojarzone z konkretnym wdrożeniem.

1. Wybierz pozycję **Dzienniki**.  
Dzienniki zawierają przydatne informacje na temat procesu wdrażania. Mogą być wyświetlane zarówno podczas wdrożeń, jak i po nich.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy usługa Azure App Service i inne utworzone powiązane zasoby nie będą już potrzebne, możesz je usunąć. Użyj funkcji **Usuń** na pulpicie nawigacyjnym usługi DevOps Projects.


## <a name="next-steps"></a>Kolejne kroki

Podczas konfigurowania procesu ciągłej integracji/ciągłego wdrażania zostały automatycznie utworzone potoki kompilacji i wydania. Możesz zmodyfikować potoki kompilacji i wydania, aby dopasować je do potrzeb swojego zespołu. Aby dowiedzieć się więcej na temat potoku ciągłej integracji/ciągłego wdrażania, zobacz:

> [!div class="nextstepaction"]
> [Dostosowywanie procesu ciągłego wdrażania](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Filmy wideo

> [!VIDEO https://www.youtube.com/embed/3etwjubReJs]
