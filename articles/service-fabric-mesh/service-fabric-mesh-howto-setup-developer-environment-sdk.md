---
title: Konfigurowanie środowiska deweloperskiego w systemie Windows w celu tworzenia aplikacji usługi Service Fabric Mesh | Microsoft Docs
description: Skonfigurowanie środowiska deweloperskiego w systemie Windows pozwala utworzyć aplikację usługi Service Fabric Mesh i wdrożyć ją w usłudze Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: dkkapur
ms.author: dekapur
ms.date: 12/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: chakdan
ms.openlocfilehash: 5ab817c65ab562f37b456cc3589624c1876084f0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428202"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-mesh-apps"></a>Konfigurowanie środowiska deweloperskiego w systemie Windows w celu tworzenia aplikacji usługi Service Fabric Mesh

Aby móc tworzyć i uruchamiać aplikacje usługi Azure Service Fabric Mesh na maszynie deweloperskiej z systemem Windows, musisz mieć:

* Docker
* Program Visual Studio 2017 lub nowszego
* Środowisko uruchomieniowe usługi Service Fabric Mesh
* Zestaw Service Fabric Mesh SDK i narzędzia.

Oraz jedną z następujących wersji systemu Windows:

* Windows 10 (Enterprise, Professional lub Education) w wersji 1709 (Fall Creators Update) lub 1803 (aktualizacja systemu Windows 10 z kwietnia 2018 r.)
* Windows Server w wersji 1709
* Windows Server w wersji 1803

Następujące będzie instrukcje pomocy zapewnia wszystko, co jest zainstalowany na podstawie wersji systemu Windows jest uruchomiona.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="visual-studio"></a>Visual Studio

Program Visual Studio 2017 lub nowszego jest wymagany do wdrażania aplikacji usługi Service Fabric siatki. [Zainstaluj wersję 15.6.0][download-visual-studio] lub nowszą i włącz następujące obciążenia:

* Tworzenie aplikacji na platformie ASP.NET i aplikacji internetowych
* Programowanie na platformie Azure

## <a name="install-docker"></a>Zainstaluj platformę Docker

Jeśli platforma Docker jest już zainstalowana, upewnij się, że masz najnowszą wersję. Docker może monitować użytkownika, gdy nowa wersja jest zewnętrzny, ale sprawdzać ręcznie, aby upewnić się, że masz najnowszą wersję.

#### <a name="install-docker-on-windows-10"></a>Instalowanie platformy Docker w systemie Windows 10

Pobierz i zainstaluj najnowszą wersję aplikacji [Docker Community Edition for Windows][download-docker] do obsługi konteneryzowanych aplikacji usługi Service Fabric używanych w usłudze Service Fabric Mesh.

Podczas instalacji wybierz opcję **Use Windows containers instead of Linux containers** (Użyj kontenerów systemu Windows zamiast kontenerów systemu Linux), gdy pojawi się odpowiedni monit.

Funkcji Hyper-V nie jest włączona na komputerze, będzie oferować je włączyć Instalatora platformy Docker. Jeśli pojawi się taki monit, kliknij przycisk **OK**, aby to zrobić.

#### <a name="install-docker-on-windows-server-2016"></a>Instalowanie platformy Docker w systemie Windows Server 2016

Jeśli nie masz włączonej roli funkcji Hyper-V, otwórz program PowerShell jako administrator i uruchom następujące polecenie, aby włączyć funkcję Hyper-V, a następnie uruchom ponownie komputer. Aby uzyskać więcej informacji, zobacz [Docker Enterprise Edition for Windows Server (Oprogramowanie Docker Enterprise Edition for Windows Server)][download-docker-server].

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Uruchom ponownie komputer.

Otwórz program PowerShell jako administrator i uruchom następujące polecenie, aby zainstalować aplikację Docker:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

## <a name="sdk-and-tools"></a>Zestaw SDK i narzędzia

Zainstaluj środowisko uruchomieniowe, zestaw SDK oraz narzędzia usługi Service Fabric Mesh w kolejności podanej poniżej.

1. Zainstaluj [zestaw SDK usługi Service Fabric Mesh][download-sdkmesh] za pomocą Instalatora platformy sieci Web. Spowoduje to również zainstalowanie zestawu SDK i środowiska uruchomieniowego usługi Microsoft Azure Service Fabric.
2. Zainstaluj [rozszerzenie Visual Studio Service Fabric Mesh Tools (wersja zapoznawcza)][download-tools] z witryny Visual Studio Marketplace.

## <a name="build-a-cluster"></a>Tworzenie klastra

> [!IMPORTANT]
> Utworzenie klastra **wymaga**  uruchomienia platformy Docker.
> Aby sprawdzić, czy platforma Docker jest uruchomiona, otwórz okno terminalu i uruchom polecenie `docker ps`. Jeśli odpowiedź nie wskazuje na wystąpienie błędu, oznacza to, że platforma Docker jest uruchomiona i można przystąpić do tworzenia klastra.

> [!Note]
> Jeśli tworzysz oprogramowanie na maszynie z aktualizacją systemu Windows Fall Creators Update (wersja 1709), możesz używać wyłącznie obrazów platformy Docker systemu Windows w wersji 1709.
> Jeśli tworzysz oprogramowanie na maszynie z aktualizacją systemu Windows 10 z kwietnia 2018 r. (wersja 1803), możesz używać obrazów platformy Docker systemu Windows w wersji 1709 lub 1803.

Jeśli używasz programu Visual Studio, możesz pominąć tę sekcję, ponieważ program Visual Studio spowoduje utworzenie klastra lokalnego dla Ciebie, jeśli nie masz.

Aby uzyskać najlepszą wydajność debugowania, tworzenie i uruchamianie pojedynczej aplikacji usługi Service Fabric w danym momencie Utwórz jednowęzłowej lokalny klaster projektowy. Jeśli korzystasz z wielu aplikacji w czasie, należy utworzyć pięcioma węzłami lokalny klaster projektowy. Klaster musi być uruchomiony podczas każdego wdrażania lub debugowania projektu usługi Service Fabric Mesh.

Po zainstalowaniu środowiska uruchomieniowego, zestawów SDK, narzędzi programu Visual Studio i platformy Docker oraz uruchomieniu platformy Docker utwórz klaster projektowy.

1. Zamknij okno programu PowerShell.
2. Otwórz nowe okno programu PowerShell jako administrator. Ten krok jest niezbędny do załadowania zainstalowanych uprzednio modułów usługi Service Fabric.
3. Utwórz klaster projektowy za pomocą następującego polecenia programu PowerShell:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateMeshCluster -CreateOneNodeCluster
    ```
4. Uruchom menedżera klastra lokalnego za pomocą następującego polecenia programu PowerShell:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\Tools\ServiceFabricLocalClusterManager\ServiceFabricLocalClusterManager.exe"
    ```
5. Gdy narzędzie menedżera klastra usługi jest uruchomione (jest wyświetlane na pasku zadań), kliknij je prawym przyciskiem myszy, a następnie kliknij pozycję **Uruchom klaster lokalny**.

![Rysunek 1. Uruchamianie klastra lokalnego](./media/service-fabric-mesh-howto-setup-developer-environment-sdk/start-local-cluster.png)

Możesz już tworzyć aplikacje usługi Service Fabric Mesh.

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z samouczkiem [Create an Azure Service Fabric app (Tworzenie aplikacji usługi Azure Service Fabric)](service-fabric-mesh-tutorial-create-dotnetcore.md).

Znajdź odpowiedzi na [typowe pytania i znane problemy](service-fabric-mesh-faq.md).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://aka.ms/sfmesh_vs2017tools
[download-visual-studio]: https://www.visualstudio.com/downloads/