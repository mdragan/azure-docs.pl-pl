---
title: Aplikacja aranżacji poprawki w usłudze Azure Service Fabric | Dokumentacja firmy Microsoft
description: Aplikacja do Automatyzowanie stosowania poprawek systemu operacyjnego w klastrze usługi Service Fabric.
services: service-fabric
documentationcenter: .net
author: khandelwalbrijeshiitr
manager: chackdan
editor: ''
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/01/2019
ms.author: brkhande
ms.openlocfilehash: ccc0399b6ac886ec8d9ef7d207c3539f1d078070
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65951932"
---
# <a name="patch-the-windows-operating-system-in-your-service-fabric-cluster"></a>Stosowanie poprawek systemu operacyjnego Windows w klastrze usługi Service Fabric

> 
> [!IMPORTANT]
> Aplikacja w wersji 1.2. * jest przesyłane z pomocy technicznej na 30 kwietnia 2019 r. Przeprowadź uaktualnienie do najnowszej wersji.


[Automatyczne uaktualnienia obrazu systemu operacyjnego zestawu skalowania maszyn wirtualnych platformy Azure](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) jest najlepszym rozwiązaniem dla przechowywania przez systemy operacyjne poprawek na platformie Azure, a aplikacja Orchestration Patch (POA) otokę usługa usługi sieci szkieletowe RepairManager systemów który umożliwia konfiguracji na podstawie systemu operacyjnego poprawki planowania dla klastrów hostowanej spoza platformy Azure. POA nie jest wymagana w przypadku klastrów hostowanej spoza platformy Azure, ale planowanie instalacji poprawki uaktualnienia domen, jest wymagany do poprawiania hostów klastrów Service Fabric bez przestojów.

POA jest aplikacja usługi Azure Service Fabric, która automatyzuje systemu operacyjnego poprawek w klastrze usługi Service Fabric, bez przestojów.

Aplikacja orchestration poprawki zapewnia następujące funkcje:

- **Instalacja aktualizacji automatycznych systemu operacyjnego**. Aktualizacje systemu operacyjnego są automatycznie pobierane i instalowane. Węzły klastra są ponownie uruchamiane zgodnie z potrzebami bez przestoju klastra.

- **Typu cluster-aware, obsługą jej poprawek oraz kondycji integracji**. Podczas stosowania aktualizacji, aplikacja orchestration poprawki monitoruje kondycję węzłów klastra. Węzły klastra są uaktualnione jeden węzeł lub jedną domenę uaktualnienia w danym momencie. Jeśli kondycja klastra ulegnie awarii z powodu procesu stosowania poprawek, poprawek jest zatrzymana, aby zapobiec obciążające problem.

## <a name="internal-details-of-the-app"></a>Szczegóły wewnętrznej aplikacji

Aplikacji patch orchestration składa się z następujących Podskładniki:

- **Usługa koordynatora**: Ta usługa stanowa jest odpowiedzialny za:
    - Koordynowanie zadania Windows Update dla całego klastra.
    - Przechowywanie wyniku zakończone operacje Windows Update.
- **Usługa agenta węzła**: Tę usługę bezstanową działa we wszystkich węzłach klastra usługi Service Fabric. Usługa jest odpowiedzialna za:
    - Uruchamianie NTService agenta węzła.
    - Monitorowanie NTService agenta węzła.
- **Węzeł Agent NTService**: Ta usługa systemu Windows NT jest uruchamiane na wyższym poziomie uprawnień (SYSTEM). Z kolei Usługa agenta węzła oraz usługi koordynatora będzie działać na niższym poziomie uprawnień (Usługa sieciowa). Usługa jest odpowiedzialny za wykonanie następujących zadań Windows aktualizacji we wszystkich węzłach klastra:
    - Wyłączanie automatycznej aktualizacji Windows w węźle.
    - Pobieranie i instalowanie aktualizacji Windows zgodnie z zasadami użytkownik udostępnił.
    - Ponowne uruchamianie maszyny po instalacji aktualizacji Windows.
    - Przekazywanie wyników aktualizacji Windows z usługą koordynatora.
    - Raportów raportowania kondycji, w przypadku, gdy operacja nie powiodła się po wykorzystaniu ponawiania prób.

> [!NOTE]
> Aplikacja aranżacji poprawki używa usługę Service Fabric naprawy Menedżera systemu, aby wyłączyć lub włączyć węzła i sprawdzać kondycję. Zadanie naprawy tworzonych przez aplikację patch orchestration śledzi postęp aktualizacji Windows, dla każdego węzła.

## <a name="prerequisites"></a>Wymagania wstępne

> [!NOTE]
> Minimalna wersja programu .NET framework wymagana jest 4.6.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>Włącz usługę Menedżer naprawy (Jeśli nie jest już uruchomiona)

Aplikacja orchestration poprawki wymaga naprawy Menedżera systemu włączenia usługi w klastrze.

#### <a name="azure-clusters"></a>Klastry platformy Azure

Klastry platformy Azure w ramach warstwy trwałości silver ma usługa Menedżera naprawy, które są domyślnie włączone. Klastry platformy Azure w warstwa trwałości gold, ale nie może być usługa Menedżera naprawy, które są włączone, w zależności od tego, kiedy te klastry zostały utworzone. Klastry platformy Azure w ramach warstwy trwałości bronze domyślnie włączona usługa Menedżera naprawy nie jest konieczne. Jeśli usługa jest już włączony, można to sprawdzić w sekcji usługi systemowe Service Fabric Explorer.

##### <a name="azure-portal"></a>Azure Portal
Menedżer naprawy w witrynie Azure portal można włączyć w momencie utworzenia klastra. Wybierz **obejmują Menedżera naprawy** opcji w obszarze **funkcje dodatku** w czasie konfiguracji klastra.
![Obraz Menedżera naprawy Włączanie z witryny Azure portal](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="azure-resource-manager-deployment-model"></a>Model wdrażania usługi Azure Resource Manager
Możesz też użyć [modelu wdrażania usługi Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) włączyć usługę Menedżer naprawy na nowych i istniejących klastrów usługi Service Fabric. Pobierz szablon dla klastra, który chcesz wdrożyć. Można użyć przykładowych szablonów lub utworzyć szablon niestandardowy model wdrażania usługi Azure Resource Manager. 

Aby umożliwić używanie usługi Menedżera naprawy [szablon modelu wdrożenia usługi Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):

1. Najpierw sprawdź, czy `apiversion` ustawiono `2017-07-01-preview` dla `Microsoft.ServiceFabric/clusters` zasobów. Jeśli jest inny, a następnie należy zaktualizować `apiVersion` wartość `2017-07-01-preview` lub nowszej:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Teraz Włącz usługę Menedżer naprawy, dodając następujące `addonFeatures` sekcji po `fabricSettings` sekcji:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Po zaktualizowaniu szablonu klastra za pomocą tych zmian stosować je i umożliwić uaktualnienie Zakończ. Teraz widać usługa systemowa Menedżera naprawy działającym w klastrze. Jest on nazywany `fabric:/System/RepairManagerService` w sekcji usługi systemowe Service Fabric Explorer. 

### <a name="standalone-on-premises-clusters"></a>Autonomicznych klastrów w środowisku lokalnym

Możesz użyć [ustawienia konfiguracji dla autonomicznego klastra Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) włączyć usługę Menedżer naprawy na nowych i istniejących klastrów usługi Service Fabric.

Aby włączyć usługę Menedżer naprawy:

1. Najpierw sprawdź, czy `apiversion` w [konfiguracje klastra ogólne](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) ustawiono `04-2017` lub nowszej:

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. Teraz Włącz usługę Menedżer naprawy przez dodanie poniższego `addonFeatures` sekcji po `fabricSettings` sekcji, jak pokazano poniżej:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Zaktualizuj manifeście klastra przy użyciu tych zmian w manifeście klastra zaktualizowane [Utwórz nowy klaster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) lub [Uaktualnij konfigurację klastra](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server). Gdy klaster działa z manifestem zaktualizowane klastra, możesz teraz wyświetlić usługę systemu Menedżera naprawy działającym w klastrze, które jest wywoływane `fabric:/System/RepairManagerService`w obszarze sekcji narzędzia Service Fabric explorer usług systemowych.

### <a name="configure-windows-updates-for-all-nodes"></a>Konfigurowanie aktualizacji Windows dla wszystkich węzłów

Aktualizacje automatyczne Windows może prowadzić do utraty dostępności, ponieważ wiele węzłów klastra można uruchomić ponownie w tym samym czasie. Poprawka aplikacji aranżacji, domyślnie podejmie próbę wyłączenia automatycznej aktualizacji Windows w każdym węźle klastra. Jednakże jeśli ustawienia są zarządzane przez administratora lub zasad grupy, zalecamy ustawienie zasady Windows Update "Powiadom przed Download" jawnie.

## <a name="download-the-app-package"></a>Pobieranie pakietu aplikacji

Aby pobrać pakiet aplikacji, przejdź do strony GitHub wersji [strony](https://github.com/microsoft/Service-Fabric-POA/releases/latest/) Patch Orchestration aplikacji.

## <a name="configure-the-app"></a>Konfigurowanie aplikacji

Zachowanie aplikacji orkiestracji poprawek można skonfigurować do własnych potrzeb. Zastąp wartości domyślne, przekazując w parametrze aplikacji podczas tworzenia aplikacji lub aktualizacji. Można podać parametry aplikacji, określając `ApplicationParameter` do `Start-ServiceFabricApplicationUpgrade` lub `New-ServiceFabricApplication` polecenia cmdlet.

|**Parametr**        |**Typ**                          | **Szczegóły**|
|:-|-|-|
|MaxResultsToCache    |Długie                              | Maksymalna liczba wyników aktualizacji Windows, które mają być buforowane. <br>Wartość domyślna to 3000 zakładając, że: <br> -Liczba węzłów to 20. <br> -Liczba aktualizacje wykonywane w węźle na miesiąc wynosi pięć. <br> -Liczba wyników na operację może być 10. <br> — Powinny być przechowywane wyniki ostatnie trzy miesiące. |
|TaskApprovalPolicy   |Enum <br> { NodeWise, UpgradeDomainWise }                          |TaskApprovalPolicy wskazuje zasady, które ma być używany przez usługę koordynatora do instalowania aktualizacji Windows w węzłach klastra usługi Service Fabric.<br>                         Dozwolone wartości to: <br>                                                           <b>NodeWise</b>. Windows Update jest zainstalowane na jednym węźle naraz. <br>                                                           <b>UpgradeDomainWise</b>. Aktualizacja Windows jest zainstalowane jedną domenę uaktualnienia w danym momencie. (Maksymalnie, wszystkie węzły należące do domeny uaktualnienia można szukać Windows Update.)<br> Zapoznaj się [— często zadawane pytania](#frequently-asked-questions) sekcję dotyczącą sposobu określenia, który jest najbardziej odpowiednie zasady dla klastra.
|LogsDiskQuotaInMB   |Długie  <br> (Domyślnie: 1024)               |Maksymalny rozmiar patch orchestration aplikacja rejestruje się w MB, co może być utrwalony lokalnie w węzłach.
| WUQuery               | string<br>(Domyślnie: "IsInstalled = 0")                | Zapytanie w celu pobrania aktualizacji i Windows. Aby uzyskać więcej informacji, zobacz [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)
| InstallWindowsOSOnlyUpdates | Boolean <br> (domyślna: false)                 | Użyj tej flagi do kontroli, które aktualizacje powinny zostać pobrana i zainstalowana. Następujące wartości są dozwolone. <br>wartość true — instaluje tylko aktualizacje systemu operacyjnego Windows.<br>FALSE — instaluje wszystkie dostępne aktualizacje na komputerze.          |
| WUOperationTimeOutInMinutes | Int <br>(Domyślnie: 90)                   | Określa limit czasu dla wszelkich operacji Windows Update (wyszukiwania lub pobierania lub instalacji). Jeśli operacja nie jest ukończone przed upływem określonego limitu czasu, zostało przerwane.       |
| WURescheduleCount     | Int <br> (Domyślnie: 5)                  | Maksymalna liczba przypadków, w usłudze zmieni ustalony Windows aktualizacji w przypadku, gdy operacja stale kończy się niepowodzeniem.          |
| WURescheduleTimeInMinutes | Int <br>(Domyślnie: 30) | Interwał, w którym usługa zmieni ustalony aktualizacji Windows w przypadku, gdy błąd będzie nadal występować. |
| WUFrequency           | Ciąg rozdzielony przecinkami (domyślne: "Co tydzień, Środa, 7:00:00")     | Częstotliwość instalowania aktualizacji Windows. Format i możliwe wartości to: <br>—: Mm: ss co miesiąc, DD, na przykład co miesiąc, 5, 12: 22:32.<br>Dopuszczalne wartości dla pola DD (dzień) są liczby z zakresu od zakresu 1-28 i "last". <br> -Co tydzień, dzień, ss, na przykład co tydzień, Wtorek, 12:22:32.  <br> -Codziennie: mm: ss, na przykład codziennie, 12:22:32.  <br> -Brak wskazuje nie powinny odbywać Windows Update.  <br><br> Należy pamiętać, że godziny są w formacie UTC.|
| AcceptWindowsUpdateEula | Boolean <br>(Domyślnie: true) | Przez ustawienie tej flagi, aplikacja przyjmuje umowy licencji użytkownika końcowego for Windows Update w imieniu właściciela maszyny.              |

> [!TIP]
> Windows Update, natychmiastowe, ustawić `WUFrequency` względem czasu wdrożenia aplikacji. Na przykład, załóżmy, że klaster testu z pięcioma węzłami, a następnie zaplanować wdrożenie aplikacji o około 5:00 czasu UTC. Jeśli przyjęto założenie, że uaktualnienie aplikacji lub wdrożenia trwa 30 minut na maksymalnym, ustaw WUFrequency jako "Codziennie, 17:30:00"

## <a name="deploy-the-app"></a>Wdrażanie aplikacji

1. Zakończ wszystkie wstępnie wymagane kroki, aby przygotować klaster.
2. Wdrażanie aplikacji aranżacji poprawek, jak dowolną inną aplikację usługi Service Fabric. Aplikację można wdrożyć przy użyciu programu PowerShell. Postępuj zgodnie z instrukcjami w [Wdróż i usunąć aplikacje przy użyciu programu PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).
3. Aby skonfigurować aplikację w czasie wdrażania, należy przekazać `ApplicationParameter` do `New-ServiceFabricApplication` polecenia cmdlet. Dla Twojej wygody udostępniliśmy skryptu Deploy.ps1 wraz z aplikacji. Aby użyć skryptu:

    - Łączenie z klastrem usługi Service Fabric za pomocą `Connect-ServiceFabricCluster`.
    - Uruchom skrypt programu PowerShell Deploy.ps1 z odpowiednią `ApplicationParameter` wartość.

> [!NOTE]
> Zachowaj skrypt i folderu aplikacji PatchOrchestrationApplication w tym samym katalogu.

## <a name="upgrade-the-app"></a>Uaktualnianie aplikacji

Aby uaktualnić istniejącą aplikację orkiestracji poprawek za pomocą programu PowerShell, wykonaj kroki opisane w [uaktualnianie aplikacji usługi Service Fabric przy użyciu programu PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).

## <a name="remove-the-app"></a>Usuń aplikację

Aby usunąć aplikację, wykonaj kroki opisane w [Wdróż i usunąć aplikacje przy użyciu programu PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).

Dla Twojej wygody udostępniliśmy skryptu Undeploy.ps1 wraz z aplikacji. Aby użyć skryptu:

  - Łączenie z klastrem usługi Service Fabric za pomocą ```Connect-ServiceFabricCluster```.

  - Uruchom skrypt programu PowerShell Undeploy.ps1.

> [!NOTE]
> Zachowaj skrypt i folderu aplikacji PatchOrchestrationApplication w tym samym katalogu.

## <a name="view-the-windows-update-results"></a>Wyświetlanie wyników Windows Update

Aplikacja orchestration poprawki udostępnia interfejsy API REST, aby wyświetlić wyniki historyczne dla użytkownika. Przykład wyniku JSON:
```json
[
  {
    "NodeName": "_stg1vm_1",
    "WindowsUpdateOperationResults": [
      {
        "OperationResult": 0,
        "NodeName": "_stg1vm_1",
        "OperationTime": "2019-05-13T08:44:56.4836889Z",
        "OperationStartTime": "2019-05-13T08:44:33.5285601Z",
        "UpdateDetails": [
          {
            "UpdateId": "7392acaf-6a85-427c-8a8d-058c25beb0d6",
            "Title": "Cumulative Security Update for Internet Explorer 11 for Windows Server 2012 R2 (KB3185319)",
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of the issues that are included in this update, see the associated Microsoft Knowledge Base article. After you install this update, you may have to restart your system.",
            "ResultCode": 0,
            "HResult": 0
          }
        ],
        "OperationType": 1,
        "WindowsUpdateQuery": "IsInstalled=0",
        "WindowsUpdateFrequency": "Daily,10:00:00",
        "RebootRequired": false
      }
    ]
  },
  ...
]
```

Pola JSON zostały opisane poniżej.

Pole | Wartości | Szczegóły
-- | -- | --
OperationResult | 0 — Powodzenie<br> 1 — zakończyło się pomyślnie z błędami<br> 2 — nie powiodło się<br> 3 — zostało przerwane<br> 4 — zostało przerwane z przekroczeniem limitu czasu | Wskazuje wynik ogólny operacji (zazwyczaj instalacji co najmniej jednej aktualizacji).
ResultCode | Takie same jak klasy OperationResult | To pole wskazuje wynik operacji instalacji dla indywidualnej aktualizacji.
OperationType | 1 — Instalacja<br> 0 - wyszukiwanie i pobieranie.| Instalacja jest tylko typ operacji, która będzie wyświetlana w wynikach domyślnie.
WindowsUpdateQuery | Wartość domyślna to "IsInstalled = 0" |Windows zaktualizuj zapytanie, które zostało użyte do wyszukiwania aktualizacji. Aby uzyskać więcej informacji, zobacz [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)
RebootRequired | wartość true — ponowny rozruch nie jest wymagana<br> FALSE — ponowny rozruch nie jest wymagane | Wskazuje, jeśli ponowne uruchomienie komputera nie jest wymagana do ukończenia instalacji aktualizacji.
OperationStartTime | DateTime | Wskazuje czas, w których operation(Download/Installation) pracę.
OperationTime | DateTime | Wskazuje czas, w których operation(Download/Installation) ukończone.
Wartość HResult | 0 — pomyślnie<br> inne — błąd| Wskazuje przyczynę błędu aktualizacji systemu windows za pomocą updateID "7392acaf-6a85-427c-8a8d-058c25beb0d6".

Jeśli aktualizacja nie jest jeszcze zaplanowane, wynik JSON jest pusta.

Zaloguj się w klastrze do wykonywania zapytań w aktualizacji Windows wyników. Dowiedz się, repliki adres podstawowy usługi koordynatora i trafień adresu URL w przeglądarce: http://&lt;IP REPLIKI&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1 / GetWindowsUpdateResults.

Punkt końcowy REST dla usługi koordynatora ma portu dynamicznego. Aby sprawdzić adres URL, zapoznaj się z narzędzia Service Fabric Explorer. Na przykład, wyniki są dostępne pod adresem `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.

![Obraz punktu końcowego REST](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


Jeśli zwrotny serwer proxy jest włączony w klastrze, możesz uzyskać dostęp na adres URL z poza klastrem, jak również.
Punkt końcowy, który musi zostać osiągnięty to http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.

Aby włączyć zwrotny serwer proxy w klastrze, wykonaj kroki opisane w [zwrotny serwer proxy w usłudze Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy). 

> 
> [!WARNING]
> Po skonfigurowaniu zwrotny serwer proxy wszystkich wczesnych usług w klastrze, które ujawniają punkt końcowy HTTP adresowane z poza klastrem.

## <a name="diagnosticshealth-events"></a>Zdarzenia diagnostyczne/kondycji

Poniższa sekcja opowiada, jak do debugowania/diagnozowania problemów z poprawek i aktualizacji za pośrednictwem Patch Orchestration Application w klastrach usługi Service Fabric.

> [!NOTE]
> Powinny mieć wersję v1.4.0 POA zainstalowana tak, aby uzyskać liczbę poniżej przywołane własnym ulepszenia diagnostyki.

Tworzy NodeAgentNTService [naprawy zadania](https://docs.microsoft.com/dotnet/api/system.fabric.repair.repairtask?view=azure-dotnet) do instalowania aktualizacji w węzłach. Każde zadanie podrzędne jest następnie przygotowane przez CoordinatorService zgodnie z zasadami zatwierdzenia zadania. Przygotowany zadania finally są zatwierdzane przez naprawy menedżera, która nie będzie zatwierdzenia na dowolne zadanie, jeśli klaster jest w złej. Aby dowiedzieć się, jak kontynuować aktualizacje w węźle umożliwia Przejdź krok po kroku.

1. NodeAgentNTService, uruchomione w każdym węźle szuka dostępną aktualizację Windows w zaplanowanym czasie. Jeśli aktualizacje są dostępne, przechodzi z wyprzedzeniem i pobiera je w węźle.
2. Po pobraniu aktualizacji NodeAgentNTService, tworzy odpowiednie zadanie naprawy dla węzła o nazwie POS___ < unique_id >. Jeden wyświetlanie tych aplikacji naprawy zadania za pomocą polecenia cmdlet [Get ServiceFabricRepairTask](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricrepairtask?view=azureservicefabricps) lub SFX w sekcji szczegółów węzła. Po utworzeniu zadania naprawy, szybkie przeniesienie do [zgłosić stan](https://docs.microsoft.com/dotnet/api/system.fabric.repair.repairtaskstate?view=azure-dotnet).
3. Usługa koordynatora okresowo szuka zadania naprawy w stanie oświadczeniem przechodzi dalej i aktualizuje je do stanu, w oparciu o TaskApprovalPolicy przygotowywanie. Jeśli TaskApprovalPolicy jest skonfigurowany na NodeWise, zadanie naprawy odpowiadający węzła jest przygotowany tylko wtedy, gdy inne zadanie naprawy obecnie w stanie przygotowanie/zatwierdzone/Executing/przywracania. Podobnie w przypadku, gdy z UpgradeWise TaskApprovalPolicy, zapewnia się, w dowolnym momencie istnieją zadania w Stanach powyżej tylko dla węzłów, które należą do tej samej domenie uaktualnienia. Gdy zadanie naprawy jest przenoszony do stanu przygotowywanie, odpowiedniego węzła usługi Service Fabric jest [wyłączone](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) z zamiarem jako "Restart".

   POA(V1.4.0 and above) publikuje zdarzenia z właściwością "ClusterPatchingStatus" na CoordinaterService, aby wyświetlić węzły, które są Trwa poprawianie. Poniżej obrazu na _poanode_0 zainstalowano wprowadzenie pokazują aktualizacji:

    [![Obraz przedstawiający klaster poprawek stanu](media/service-fabric-patch-orchestration-application/clusterpatchingstatus.png)](media/service-fabric-patch-orchestration-application/clusterpatchingstatus.png#lightbox)

4. Gdy węzeł jest wyłączony, zadanie naprawy jest przenoszony do stanie wykonywanie. Uwaga: zadanie naprawy, została zablokowana na przygotowanie stan, po, ponieważ węzeł jest zablokowany w stanie wyłączenia może spowodować zablokowanie nowe zadanie naprawy i dlatego zatrzymanie poprawek klastra.
5. Gdy zadanie naprawy jest w stanie wykonywanie, rozpoczyna się instalacji poprawek, w tym węźle. W tym miejscu, po zainstalowaniu poprawki węzła może lub nie może zostać ponownie uruchomiona w zależności od poprawki. Wpis, że zadanie naprawy jest przenoszony do przywrócenia stanu, który umożliwia tworzenie kopii węzeł ponownie, a następnie go jest oznaczane jako ukończone.

   W v1.4.0 lub jego wyższej wersji aplikacji stan aktualizacji można znaleźć, analizując zdarzenia kondycji na NodeAgentService z właściwością "WUOperationStatus-[NodeName]". Wyróżnione sekcje zawierają obrazy poniżej Pokaż stan usługi windows update w węźle "poanode_0" i "poanode_2":

   [![Ilustracja przedstawiająca stan operacji dla Windows update](media/service-fabric-patch-orchestration-application/wuoperationstatusa.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusa.png#lightbox)

   [![Ilustracja przedstawiająca stan operacji dla Windows update](media/service-fabric-patch-orchestration-application/wuoperationstatusb.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusb.png#lightbox)

   Jeden można również uzyskać szczegółowe informacje przy użyciu programu powershell, łączenie z klastrem i pobieranie stanu zadania naprawy przy użyciu [Get ServiceFabricRepairTask](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricrepairtask?view=azureservicefabricps). Podobnie jak poniżej przedstawiono przykład tej "POS__poanode_2_125f2969-933c-4774-85 d 1-ebdf85e79f15" zadanie jest w stanie DownloadComplete. Oznacza to, że aktualizacje zostały pobrane w węźle "poanode_2" i instalacji zostanie podjęta, gdy zadanie przechodzi do stanu Executing.

   ``` powershell
    D:\service-fabric-poa-bin\service-fabric-poa-bin\Release> $k = Get-ServiceFabricRepairTask -TaskId "POS__poanode_2_125f2969-933c-4774-85d1-ebdf85e79f15"

    D:\service-fabric-poa-bin\service-fabric-poa-bin\Release> $k.ExecutorData
    {"ExecutorSubState":2,"ExecutorTimeoutInMinutes":90,"RestartRequestedTime":"0001-01-01T00:00:00"}
    ```

   Jeśli jest nadal można znaleźć następnie, zaloguj się do określonej maszyny Wirtualnej lub maszyny wirtualne, aby dowiedzieć się więcej na temat problemu przy użyciu dzienników zdarzeń Windows. Powyżej opisane zadania naprawy może mieć tylko te przetwarzania podstany:

      ExecutorSubState | Szczegóły
    -- | -- 
      Brak = 1 |  Wskazuje, czy nie było trwającą operację na węźle. Możliwe stany przejścia.
      DownloadCompleted=2 | Zakłada się, operacja pobierania została zakończona sukcesem częściowe niepowodzenie lub błąd.
      InstallationApproved=3 | Oznacza to operacja pobierania zakończyła się wcześniej, i napraw Manager zatwierdzonych instalacji.
      InstallationInProgress=4 | Odnosi się do stanu uruchomienia zadania naprawy.
      InstallationCompleted=5 | Oznacza to instalacja została zakończona z Powodzenie, częściowe powodzenie lub niepowodzenie.
      RestartRequested=6 | Oznacza to patch instalacja została zakończona i ma akcji oczekuje na ponowne uruchomienie, w węźle.
      RestartNotNeeded=7 |  Oznacza, że ponowne uruchomienie nie jest wymagane po ukończeniu instalacji poprawki.
      RestartCompleted=8 | Oznacza, że ponowne uruchomienie zostało ukończone pomyślnie.
      OperationCompleted=9 | Windows aktualizacji operacja została ukończona pomyślnie.
      OperationAborted=10 | Oznacza, że operacja aktualizacji systemu windows zostało przerwane.

6. W v1.4.0 i powyżej aplikacji, po zakończeniu próby aktualizacji na węźle, zdarzenia z właściwością "WUOperationStatus-[NodeName]" zostanie opublikowana w NodeAgentService powiadomienie, gdy podejmie następnego, aby pobrać i zainstalować aktualizację, uruchom. Zobacz poniższy obraz:

     [![Ilustracja przedstawiająca stan operacji dla Windows update](media/service-fabric-patch-orchestration-application/wuoperationstatusc.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusc.png#lightbox)

### <a name="diagnostic-logs"></a>Dzienniki diagnostyczne

Patch orchestration aplikacji dzienniki są zbierane w ramach dzienniki środowiska uruchomieniowego usługi Service Fabric.

W przypadku, gdy zachodzi potrzeba przechwycenia dzienniki za pośrednictwem narzędzia diagnostyczne/potoku wybranych przez użytkownika. Patch orchestration application używa poniżej dostawcy stałych identyfikatorów do rejestrowania zdarzeń za pośrednictwem [źródła zdarzeń](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netframework-4.5.1)

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Raportów o kondycji

Aplikacja orchestration poprawki są również publikowane raporty kondycji na podstawie usługa koordynatora lub usługę agenta węzła w następujących przypadkach:

#### <a name="the-node-agent-ntservice-is-down"></a>Węzeł NTService Agent nie działa

Jeśli węzeł NTService Agent działa w węźle, generowania raportu kondycji poziom ostrzeżeń dla usługi agenta węzła.

#### <a name="the-repair-manager-service-is-not-enabled"></a>Usługa Menedżera naprawy nie jest włączona

Jeśli usługa Menedżera naprawy nie zostanie znaleziony w klastrze, raport dotyczący kondycji poziom ostrzeżeń jest generowany dla usługi koordynatora.

## <a name="frequently-asked-questions"></a>Często zadawane pytania

PYTANIE: **Dlaczego widzisz Mój klaster w stanie błędu, gdy uruchomiona jest aplikacja orchestration poprawki?**

A. Podczas procesu instalacji aplikacji patch orchestration wyłącza lub ponowne uruchomienie węzły, które tymczasowo może spowodować kondycji klastra zostanie wyłączona.

Na podstawie zasad dla aplikacji, albo jeden węzeł można przejść w dół podczas operacji stosowania poprawek *lub* całej domeny uaktualnienia można jednocześnie wyłączane.

Przed zakończeniem instalacji aktualizacji Windows węzły są reenabled po ponownym uruchomieniu.

W poniższym przykładzie klaster przeszedł do stanu błędu tymczasowo z uwzględnieniem dwóch węzłów było wyłączone, ponieważ zasady MaxPercentageUnhealthyNodes został naruszony, zwiększyłaby. Ten błąd jest tymczasowy, dopóki trwa operacja stosowania poprawek.

![Obraz przedstawiający klaster w złej kondycji](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Jeśli problem będzie się powtarzać, zapoznaj się z sekcją rozwiązywanie problemów.

PYTANIE: **Aplikacja orchestration poprawki jest w stanie ostrzeżenia**

A. Sprawdź, czy raport o kondycji opublikowane w związku z aplikacją główną przyczynę. Zazwyczaj ostrzeżenie zawiera szczegółowe informacje o problemie. Jeśli problem będzie się przejściowy, aplikacja powinna automatyczne odzyskiwanie z tego stanu.

PYTANIE: **Co mogę zrobić, jeśli mój klaster jest w złej kondycji i muszę wykonać aktualizację pilne systemu operacyjnego?**

A. Aplikacji patch orchestration nie instaluje aktualizacji, podczas gdy klaster jest w złej kondycji. Spróbuj przełączyć klaster do stanu prawidłowego, aby odblokować poprawki aplikacji przepływu pracy.

PYTANIE: **Należy ustawić TaskApprovalPolicy jako "NodeWise" lub "UpgradeDomainWise" dla mojego klastra?**

A. "UpgradeDomainWise" sprawia, że ogólny klastra poprawki szybciej poprzez wdrażanie poprawek wszystkie węzły należące do domeny uaktualnienia równoległe. Oznacza to, węzłach należących do całej domeny uaktualnienia byłyby niedostępne (w [wyłączone](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabled) stanu) podczas procesu stosowania poprawek.

W odróżnieniu od nich "NodeWise" zasady poprawki tylko jeden węzeł w czasie, oznacza to, ogólną poprawek klastrów może potrwać dłuższy czas. Jednak w przypadku osiągnięcia maksymalnej liczby tylko jeden węzeł byłyby niedostępne (w [wyłączone](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabled) stanu) podczas procesu stosowania poprawek.

Jeśli klaster może tolerować systemem n-1-Liczba domen uaktualnienia podczas stosowania poprawek cyklu (gdzie N to liczba domen uaktualnienia w klastrze), a następnie ustawić zasady "UpgradeDomainWise", w przeciwnym razie ustaw ją na "NodeWise".

PYTANIE: **Ile czasu zajmuje take zastosowania poprawki względem węzła?**

A. Stosowanie poprawek węzła może potrwać (na przykład: [Aktualizacje definicji usługi Windows Defender](https://www.microsoft.com/en-us/wdsi/definitions)) do godzin (na przykład: [Aktualizacje zbiorcze Windows](https://www.catalog.update.microsoft.com/Search.aspx?q=windows%20server%20cumulative%20update)). Czas wymagany do poprawiania węzła zależy przede wszystkim na 
 - Rozmiar aktualizacji
 - Liczba aktualizacji, które ma być stosowana w okno stosowania poprawek
 - Czas wymagany do zainstalowania aktualizacji, ponowny rozruch węzła (jeśli jest to wymagane) i Zakończ kroki instalacji po ponownym uruchomieniu.
 - Wydajność maszyny Wirtualnej/machine i warunki sieciowe.

PYTANIE: **Jak długo trwa stosowanie poprawek do całego klastra?**

A. Czas potrzebny na stosowanie poprawek do całego klastra zależy od następujących czynników:

- Czas potrzebny do poprawiania węzła.
- Zasady usługi koordynatora. Domyślne zasady `NodeWise`, powoduje stosowanie poprawek tylko jeden węzeł w momencie, która będzie wolniejszy niż `UpgradeDomainWise`. Na przykład: Jeśli węzeł przyjmuje ~ 1 godziny do poprawienia, aby można było zastosować poprawki na 20 (ten sam typ węzłów) węzeł klastra z 5 domenami uaktualniania, każdy z nich zawierający 4 węzły.
    - Jeśli zasady powinno zająć ~ 20 godzin zastosowania poprawki względem całego klastra `NodeWise`
    - Jeśli zasady powinno zająć ~ 5 godzin `UpgradeDomainWise`
- Obciążenie klastra — każda operacja stosowania poprawek wymaga przenoszenie obciążenia klientów do innych dostępnych węzłów w klastrze. Węzeł w trakcie poprawki będą miały [wyłączenie](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabling) stanu, w tym czasie. Jeśli klaster działa w pobliżu szczytowego obciążenia, wyłączenie procesu zajęłoby dłuższy czas. Dlatego całego procesu stosowania poprawek może wydawać się wolno w takich warunkach stressed.
- Wszelkie klastra błędy kondycji podczas stosowania poprawek - [degradacji](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate?view=azure-dotnet#System_Fabric_Health_HealthState_Error) w [kondycji klastra](https://docs.microsoft.com/azure/service-fabric/service-fabric-health-introduction) spowoduje przerwanie procesu stosowania poprawek. Spowoduje to dodanie całkowity czas wymagany do poprawiania całego klastra.

PYTANIE: **Dlaczego widzę niektórych aktualizacji w aktualizacji Windows wyniki uzyskane za pośrednictwem interfejsu API REST, ale nie w obszarze Historia Windows Update na komputerze?**

A. Niektóre aktualizacje produktu pojawią się tylko w ich historii odpowiednich aktualizacji/poprawek. Na przykład aktualizacje programu Windows Defender może lub nie może być wyświetlany w historii aktualizacji Windows w systemie Windows Server 2016.

PYTANIE: **Może służyć do poprawiania Mój klaster dev (klastra z jednym węzłem) Orkiestracji poprawek aplikacji?**

A. Nie, Patch orchestration aplikacji nie może służyć do klastra z jednym węzłem poprawki. To ograniczenie jest zgodne z projektem, jako [usługi usługi systemowe Service fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-technical-overview#system-services) lub wszystkie aplikacje klienta będzie może wystąpić Przestój, a więc wszystkie zadania naprawy dla stosowania poprawek nigdy nie będzie zatwierdzenia przez Menedżera naprawy.

PYTANIE: **Jak zastosować poprawki węzłów klastra w systemie Linux**

A. Zobacz [automatyczne uaktualnienia obrazu systemu operacyjnego zestawu skalowania maszyn wirtualnych platformy Azure](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) do organizowania aktualizacji w systemie linux.

Pytanie:**Dlaczego cyklu aktualizacji trwa tak długo?**

A. Zapytanie o wynik json, a następnie go za pomocą wpisu cyklu aktualizacji dla wszystkich węzłów, a następnie, możesz spróbować sprawdzić czas instalacji aktualizacji w każdym węźle przy użyciu OperationStartTime i OperationTime(OperationCompletionTime). Jeśli było dużym oknie czasowym w nie aktualizacji, które dzieje się, może to być spowodowane klaster był w stanie błąd, i z powodu tej naprawy Menedżera nie zatwierdza inne zadania naprawy POA. Jeśli instalacja aktualizacji trwało długo w każdym węźle, następnie może być możliwe, że węzeł nie został zaktualizowany z dużo czasu i wiele aktualizacji zostały oczekujące na instalację, które miało czasu. Może to być również przypadek, w którym poprawek w węźle jest zablokowany z powodu węzła jest zablokowane w wyłączanie stanu, który zwykle dzieje się, ponieważ wyłączenie węzła może doprowadzić do sytuacji utraty danych/kworum.

PYTANIE: **Dlaczego jest on wymagany do wyłącz węzeł, gdy POA jest jej poprawek?**

A. Patch orchestration application wyłącza węzła z zamiarem "Uruchom ponownie", który/przydzieli zatrzymuje wszystkie usługi Service fabric usługi uruchomione w węźle. Odbywa się, aby upewnić się, że aplikacje nie znajdą się przy użyciu kombinacji nowym i starym bibliotek DLL, dlatego nie zaleca się stosowanie poprawek do węzła bez konieczności wyłączania go.

## <a name="disclaimers"></a>Zastrzeżenia

- Aplikacja orchestration poprawki akceptuje umowę licencji użytkownika końcowego programu Windows Update w imieniu użytkownika. Opcjonalnie ustawienia, można wyłączyć w konfiguracji aplikacji.

- Aplikacja orchestration poprawki gromadzi dane telemetryczne umożliwiający śledzenie użycia i wydajności. Telemetria usługi application następuje ustawienie ustawienie dane telemetryczne środowiska uruchomieniowego usługi Service Fabric, (która jest domyślnie włączone).

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="a-node-is-not-coming-back-to-up-state"></a>Węzeł nie jest powracające do stanu w górę

**Węzeł mogła zostać zablokowana w stanie wyłączenie, ponieważ**:

Sprawdzanie bezpieczeństwa jest w stanie oczekiwania. Aby rozwiązać ten problem, upewnij się, że wystarczającej liczby węzłów są dostępne w dobrej kondycji.

**Węzeł mogła zostać zablokowana w stanie wyłączenia, ponieważ**:

- Węzeł został wyłączony ręcznie.
- Węzeł został wyłączony z powodu zadania bieżące infrastruktury platformy Azure.
- Węzeł została tymczasowo wyłączona przez aplikację patch orchestration zastosowania poprawki względem węzła.

**Węzeł może zostać zatrzymane w stan naciśnięcia, ponieważ**:

- Węzeł został wprowadzony w stan naciśnięcia ręcznie.
- Węzeł jest w trakcie ponownego uruchomienia (które mogą być wyzwalane przez aplikację orkiestracji poprawek).
- Węzeł nie działa z powodu błędnej maszyny Wirtualnej lub problemy z połączeniem komputera lub sieci.

### <a name="updates-were-skipped-on-some-nodes"></a>Aktualizacje zostały pominięte niektóre węzły

Aplikacja orchestration poprawki próbuje zainstalować aktualizację Windows zgodnie z zasadami planowaniem mogą. Usługa próbuje odzyskać węzła i pominąć tę aktualizację, zgodnie z zasadami aplikacji.

W takim przypadku generowania raportu kondycji poziom ostrzeżeń dla usługi agenta węzła. Wynik dla aktualizacji Windows zawiera także możliwe przyczyny błędu.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a>Kondycja klastra jest przesyłany do błędu podczas instalacji aktualizacji

Uszkodzony aktualizacji Windows można obniżyć kondycję aplikacji lub klastra w określonym węźle lub domena uaktualnienia. Aplikacja orchestration poprawki zaprzestaje żadnych kolejnych operacji aktualizacji Windows aż klaster jest w dobrej kondycji ponownie.

Administrator musi interweniować i ustalić, dlaczego aplikacji lub klastra stało się złej kondycji ze względu na Windows Update.

## <a name="release-notes"></a>Informacje o wersji

>[!NOTE]
> Począwszy od wersji 1.4.0, informacje o wersji i wersje można znaleźć w witrynie GitHub wersji [strony](https://github.com/microsoft/Service-Fabric-POA/releases/).

### <a name="version-110"></a>Wersji 1.1.0
- Po publicznym udostępnieniu

### <a name="version-111"></a>Wersji 1.1.1
- Usunięto usterkę w SetupEntryPoint z NodeAgentService uniemożliwiający instalacji NodeAgentNTService.

### <a name="version-120"></a>Wersji 1.2.0 lub nowszej

- Poprawki błędów w całym systemie ponowne uruchomienie przepływu pracy.
- Naprawienie usterki w procesie tworzenia zadań Menedżera zasobów z powodu której kondycji wyboru podczas przygotowywania zadania naprawy nie działa zgodnie z oczekiwaniami.
- Zmienić trybu uruchamiania dla usługi systemu windows POANodeSvc z auto opóźnione automatycznie.

### <a name="version-121"></a>Wersji 1.2.1

- Naprawienie usterki w dół klastra w przepływie pracy. Wprowadzono logikę kolekcji wyrzucania elementów POA naprawy zadania należące do nieistniejącej węzłów.

### <a name="version-122"></a>Wersja 1.2.2

- Różne poprawki.
- Pliki binarne są teraz podpisane.
- Dodano łącze sfpkg dla aplikacji.

### <a name="version-130"></a>Wersja 1.3.0

- Ustawienie wartości false InstallWindowsOSOnlyUpdates teraz instaluje wszystkie dostępne aktualizacje.
- Zmienić logiki wyłączanie automatycznych aktualizacji. Naprawia błąd, gdy aktualizacje automatyczne nie wprowadzenie wyłączono systemie Server 2016 lub nowszym.
- Ograniczenie sparametryzowane umieszczania mikrousług POA dla przypadków użycia zaawansowane.

### <a name="version-131"></a>Wersji 1.3.1
- Naprawianie regresji, w którym POA 1.3.0 nie będzie działać w systemie Windows Server 2012 R2 lub niższą z powodu błędu podczas wyłączania automatycznej aktualizacji. 
- Naprawianie błędów gdzie InstallWindowsOSOnlyUpdates konfiguracji zawsze jest wybrany jako wartość True.
- Zmiana wartości domyślnej InstallWindowsOSOnlyUpdates na wartość False.

### <a name="version-132"></a>W wersji 1.3.2
- Naprawia problem, który dokonane poprawianie cyklu życia w węźle, w przypadku, gdy istnieją węzły z nazwą, która jest podzbiorem nazwę bieżącego węzła. Dla tych węzłów, jego możliwe, stosowanie poprawek brakuje lub Trwa oczekiwanie na ponowne. 