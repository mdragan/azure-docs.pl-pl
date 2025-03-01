---
title: Programowanie przy użyciu platformy .NET Core w usłudze AKS za pomocą usługi Azure Dev miejsca do magazynowania i programu Visual Studio
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
ms.author: zarhoads
ms.date: 03/22/2019
ms.topic: quickstart
description: Szybkie tworzenie w środowisku Kubernetes za pomocą kontenerów i mikrousług na platformie Azure
keywords: Docker, Kubernetes, Azure, usługi AKS, usłudze Azure Kubernetes Service, kontenerów, narzędzia Helm, usługa siatki, routing siatki usługi, narzędzia kubectl, k8s
manager: jeconnoc
ms.custom: vs-azure
ms.workload: azure-vs
ms.openlocfilehash: 110962c03f0236ebb26c9ed586981b51f36c635f
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399225"
---
# <a name="quickstart-develop-with-net-core-on-kubernetes-with-azure-dev-spaces-visual-studio"></a>Szybki start: Programowanie przy użyciu platformy .NET Core na platformie Kubernetes za pomocą usługi Azure Dev miejsca do magazynowania (Visual Studio)

Ten przewodnik zawiera informacje na temat wykonywania następujących czynności:

- Konfigurowanie usługi Azure Dev Spaces za pomocą zarządzanego klastra Kubernetes na platformie Azure.
- Iteracyjne tworzenie kodu w kontenerach przy użyciu programu Visual Studio.
- Debugować możesz kod działający w klastrze za pomocą programu Visual Studio.

## <a name="prerequisites"></a>Wymagania wstępne

- Subskrypcja platformy Azure. Jeśli nie masz, możesz utworzyć [bezpłatne konto](https://azure.microsoft.com/free).
- Visual Studio 2019 r na Windows z zainstalowanym obciążeniem programowanie na platformie Azure. Umożliwia także programu Visual Studio 2017 na Windows z pakietem roboczym programowania dla sieci Web i [Visual Studio Tools dla platformy Kubernetes](https://aka.ms/get-vsk8stools) zainstalowane. Jeśli nie masz zainstalowanego programu Visual Studio, pobierz ją [tutaj](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Utwórz klaster Azure Kubernetes Service

Należy utworzyć klaster usługi AKS w [obsługiwany region][supported-regions]. Aby utworzyć klaster:

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).
1. Wybierz *+ Utwórz zasób > Usługa Kubernetes*. 
1. Wprowadź _subskrypcji_, _grupy zasobów_, _nazwa klastra Kubernetes_, _Region_, _wersjaprogramuKubernetes_, i _prefiks nazwy DNS_.

    ![Tworzenie usługi AKS w witrynie Azure portal](media/get-started-netcore-visualstudio/create-aks-portal.png)

1. Kliknij pozycję *Przegląd + utwórz*.
1. Kliknij pozycję *Utwórz*.

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Włączanie usługi Azure Dev miejsca do magazynowania w klastrze usługi AKS

Przejdź do klastra usługi AKS w witrynie Azure portal, a następnie kliknij przycisk *Dev miejsca do magazynowania*. Zmiana *miejsca do magazynowania Dev Włącz* do *tak* i kliknij przycisk *Zapisz*.

![Włącz miejsca do magazynowania deweloperów w witrynie Azure portal](media/get-started-netcore-visualstudio/enable-dev-spaces-portal.png)

## <a name="create-a-new-aspnet-web-app"></a>Utwórz nową aplikację sieci web platformy ASP.NET

1. Otwórz program Visual Studio.
1. Tworzenie nowego projektu.
1. Wybierz *aplikacji sieci Web programu ASP.NET Core* i nazwij swój projekt *webfrontend*.
1. Kliknij przycisk *OK*.
1. Po wyświetleniu monitu wybierz *aplikacji sieci Web (Model-View-Controller)* dla szablonu.
1. Wybierz *platformy .NET Core* i *ASP.NET Core 2.0* u góry.
1. Kliknij przycisk *OK*.

## <a name="connect-your-project-to-your-dev-space"></a>Łączenie z projektu do obszaru dev

W projekcie, wybierz **miejsca do magazynowania Azure Dev** z rozwijanej Ustawienia uruchamiania, jak pokazano poniżej.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

W oknie dialogowym usługi Azure Dev miejsca do magazynowania wybierz swoje *subskrypcji* i *klastra Kubernetes usługi Azure*. Pozostaw *miejsca* równa *domyślne* i włączyć *publicznie* pola wyboru. Kliknij przycisk *OK*.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.png)

Ten proces wdrażania usługi w celu *domyślne* przestrzeń dev z publicznie dostępnego adresu URL. Jeśli wybierzesz klaster, który nie został skonfigurowany do pracy z usługą Azure Dev Spaces, zobaczysz komunikat z pytaniem, czy chcesz go skonfigurować. Kliknij przycisk *OK*.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

Publiczny adres URL dla usługi uruchomione w *domyślne* dev miejsca jest wyświetlany w *dane wyjściowe* okna:

```cmd
Starting warmup for project 'webfrontend'.
Waiting for namespace to be provisioned.
Using dev space 'default' with target 'MyAKS'
...
Successfully built 1234567890ab
Successfully tagged webfrontend:devspaces-11122233344455566
Built container image in 39s
Waiting for container...
36s

Service 'webfrontend' port 'http' is available at http://webfrontend.1234567890abcdef1234.eus.azds.io/
Service 'webfrontend' port 80 (http) is available at http://localhost:62266
Completed warmup for project 'webfrontend' in 125 seconds.
```

W powyższym przykładzie jest publiczny adres URL http://webfrontend.1234567890abcdef1234.eus.azds.io/. Przejdź do publicznego adresu URL usługi i korzystać z usługi uruchomione w obszarze deweloperów.

Ten proces mógł wyłączyć publicznego dostępu do usługi. Aby włączyć dostęp publiczny, możesz zaktualizować [wartość ruch przychodzący w *values.yaml*][ingress-update].

## <a name="update-code"></a>Aktualizowanie kodu

Jeśli program Visual Studio jest nadal połączony do obszaru dev, kliknij przycisk Zatrzymaj. Zmień wiersz 20 w `Controllers/HomeController.cs` do:
    
```csharp
ViewData["Message"] = "Your application description page in Azure.";
```

Zapisz zmiany i rozpocząć korzystanie z usługi service **miejsca do magazynowania Azure Dev** z listy rozwijanej ustawień uruchamiania. Otwórz publiczny adres URL usługi w przeglądarce i kliknij przycisk *o*. Sprawdź, czy zaktualizowane wiadomość jest wyświetlana.

Zamiast ponownego kompilowania lub wdrażania nowy obraz kontenera każdorazowo edycji kodu zostaną wprowadzone, miejsca do magazynowania Azure Dev przyrostowo następuje rekompilacja kodu w ramach istniejącego kontenera, aby zapewnić szybsze pętli Edycja i debugowanie.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Ustawianie i używanie punktów przerwania do debugowania

Jeśli program Visual Studio jest nadal połączony do obszaru dev, kliknij przycisk Zatrzymaj. Otwórz `Controllers/HomeController.cs` i kliknij przycisk gdzieś w wierszu 20 tam umieścić kursor. Aby ustawić punkt przerwania trafień *F9* lub kliknij przycisk *debugowania* następnie *Przełącz punkt przerwania*. Aby uruchomić swoją usługę w trybie debugowania, w obszarze dev, trafienia *F5* lub kliknij przycisk *debugowania* następnie *Rozpocznij debugowanie*.

Otwórz swoją usługę w przeglądarce i zwróć uwagę, że jest wyświetlany żaden komunikat. Wróć do programu Visual Studio i sprawdź, czy jest wyróżniony wiersz 20. Punkt przerwania, gdy ustawiasz została wstrzymana usługi w wierszu 20. Aby wznowić działanie usługi, trafienia *F5* lub kliknij przycisk *debugowania* następnie *Kontynuuj*. Wróć do przeglądarki i zwróć uwagę, że teraz jest wyświetlany komunikat.

Podczas uruchamiania usługi w usłudze Kubernetes załączonym debuggerze, masz pełny dostęp do debugowania informacje, takie jak stos wywołań, zmienne lokalne i informacje o wyjątku.

Usuń punkt przerwania, umieszczając kursor w wierszu 20 `Controllers/HomeController.cs` TAB i naciśnięcie klawisza *F9*.

## <a name="clean-up-your-azure-resources"></a>Oczyszczanie zasobów platformy Azure

Przejdź do grupy zasobów w witrynie Azure portal, a następnie kliknij przycisk *Usuń grupę zasobów*. Alternatywnie, można użyć [Usuń az aks](/cli/azure/aks#az-aks-delete) polecenia:

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Working with multiple containers and team development (Praca z wieloma kontenerami i programowanie zespołowe)](multi-service-netcore-visualstudio.md)

[ingress-update]: how-dev-spaces-works.md#how-running-your-code-is-configured
[supported-regions]: about.md#supported-regions-and-configurations
