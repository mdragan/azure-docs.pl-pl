---
title: Integrowanie usługi API Management z usługą Service Fabric na platformie Azure | Microsoft Docs
description: Dowiedz się, jak szybko rozpocząć pracę przy użyciu usługi Azure API Management i kierować ruchem do usługi zaplecza Service Fabric.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/26/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: fc2c23d93a1800232b81c5eb2f861e8b71c3e437
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428059"
---
# <a name="integrate-api-management-with-service-fabric-in-azure"></a>Integracja usługi API Management z usługą Service Fabric na platformie Azure

Wdrażanie usługi Azure API Management z usługą Service Fabric odbywa się w ramach zaawansowanego scenariusza.  Usługa API Management jest przydatna, gdy trzeba opublikować interfejsy API z obszernym zestawem reguł rozsyłania dla usług zaplecza Service Fabric. Aplikacje w chmurze zwykle potrzebują bramy frontonu, aby udostępniać pojedynczy punkt danych przychodzących dla użytkowników, urządzeń lub innych aplikacji. W usłudze Service Fabric bramą może być dowolna usługa bezstanowa przeznaczona dla ruchu przychodzącego, na przykład aplikacja ASP.NET Core albo usługa Event Hubs, IoT Hub lub Azure API Management.

W tym artykule dowiesz się, jak skonfigurować [usługi Azure API Management](../api-management/api-management-key-concepts.md) z usługą Service Fabric na potrzeby kierowania ruchu do usługi zaplecza Service Fabric.  Wykonanie podanych instrukcji pozwoli wdrożyć usługę API Management w sieci wirtualnej i skonfigurować kierowanie ruchu do bezstanowych usług zaplecza za pomocą operacji interfejsu API. Aby dowiedzieć się więcej o scenariuszach użycia usługi Azure API Management z usługą Service Fabric, zobacz artykuł z [omówieniem](service-fabric-api-management-overview.md).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="availability"></a>Dostępność

> [!IMPORTANT]
> Ta funkcja jest dostępna w **Premium** i **Developer** warstw usługi API Management z powodu wymagane wirtualnych sieci pomocy technicznej.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem:

* Jeśli nie masz subskrypcji platformy Azure, utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Zainstaluj [programu Azure Powershell](https://docs.microsoft.com/powershell/azure/install-Az-ps) lub [wiersza polecenia platformy Azure](/cli/azure/install-azure-cli).
* Tworzenie bezpiecznego [klastra Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) w sieciowej grupie zabezpieczeń.
* W przypadku wdrażania klastra systemu Windows skonfiguruj środowisko deweloperskie w systemie Windows. Zainstaluj [Visual Studio 2019](https://www.visualstudio.com) i **programowanie na platformie Azure**, **ASP.NET i tworzenie aplikacji internetowych**, i **programowanie dla wielu platform .NET Core**obciążeń.  Następnie skonfiguruj [środowisko deweloperskie platformy .NET](service-fabric-get-started.md).

## <a name="network-topology"></a>Topologia sieci

Teraz, gdy masz bezpieczny [klastra Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) na platformie Azure, wdrażanie usługi API Management w sieci wirtualnej (VNET) należącej do podsieci oraz sieciową grupę zabezpieczeń przeznaczoną dla usługi API Management. W tym artykule jest wstępnie skonfigurowana pod kątem użycia nazwy sieci Wirtualnej, podsieci i sieciowej grupy zabezpieczeń, który został skonfigurowany w szablonie usługi Resource Manager w bazie wiedzy interfejsu API zarządzania [samouczka dotyczącego klastra Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) tym artykule opisano wdrażanie następująca topologia na platformie Azure w którym Usługa API Management i Service Fabric znajdują się w podsieciach tej samej sieci wirtualnej:

 ![Podpis pod obrazem][sf-apim-topology-overview]

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Logowanie do platformy Azure i wybieranie subskrypcji

Przed wykonaniem poleceń platformy Azure zaloguj się na konto platformy Azure i wybierz subskrypcję.

```powershell
Connect-AzAccount
Get-AzSubscription
Set-AzContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Wdrażanie usługi zaplecza Service Fabric

Do skonfigurowania usługi API Management pod kątem kierowania ruchu do usługi zaplecza Service Fabric potrzebna jest uruchomiona usługa, umożliwiająca akceptowanie żądań.  

Utwórz podstawowe bezstanowa ASP.NET Core usługi Reliable Service przy użyciu szablonu projektu domyślnego internetowego interfejsu API. Spowoduje to utworzenie punktu końcowego HTTP dla usługi, który należy udostępnić za pośrednictwem usługi Azure API Management.

Uruchom program Visual Studio jako administrator i utwórz usługę ASP.NET Core:

 1. W programie Visual Studio wybierz pozycję Plik -> Nowy projekt.
 2. W obszarze Chmura wybierz szablon aplikacji Service Fabric i nadaj mu nazwę **„ApiApplication”** .
 3. Wybierz szablon bezstanowej usługi ASP.NET Core i nazwij projekt **„WebApiService”** .
 4. Wybierz szablon projektu Web API ASP.NET Core 2.0.
 5. Po utworzeniu projektu otwórz plik `PackageRoot\ServiceManifest.xml` i usuń atrybut `Port` z konfiguracji zasobu punktu końcowego:

    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    Usuwanie portu pozwala usłudze Service Fabric określić port dynamicznie z zakresu portów aplikacji, otwartego za pomocą sieciowej grupy zabezpieczeń w szablonie usługi Resource Manager dla klastra, dzięki czemu przepływ ruchu do niej z usługi API Management.

 6. W programie Visual Studio naciśnij klawisz F5, aby sprawdzić, czy internetowy interfejs API jest dostępny lokalnie.

    Otwórz narzędzie Service Fabric Explorer i przejdź do konkretnego wystąpienia usługi ASP.NET Core, aby wyświetlić podstawowy adres, na jakim nasłuchuje ta usługa. Dodaj ciąg `/api/values` do podstawowego adresu, a następnie otwórz ten adres w przeglądarce. Spowoduje to wywołanie metody Get klasy ValuesController w szablonie internetowego interfejsu API. Szablon zwróci domyślną odpowiedź — tablicę JSON, zawierającą dwa ciągi:

    ```json
    ["value1", "value2"]`
    ```

    Jest to punkt końcowy, który należy udostępnić za pomocą usługi API Management na platformie Azure.

 7. Teraz wdróż aplikację w klastrze na platformie Azure. W programie Visual Studio kliknij prawym przyciskiem myszy projekt aplikacji, a następnie wybierz polecenie **Opublikuj**. Podaj punkt końcowy klastra (na przykład `mycluster.southcentralus.cloudapp.azure.com:19000`), aby wdrożyć aplikację w klastrze usługi Service Fabric na platformie Azure.

W klastrze powinna teraz być uruchomiona bezstanowa usługa ASP.NET Core o nazwie `fabric:/ApiApplication/WebApiService`.

## <a name="download-and-understand-the-resource-manager-templates"></a>Pobieranie szablonów usługi Resource Manager i zapoznawanie się z nimi

Pobierz i zapisz plik parametrów oraz następujące szablony usługi Resource Manager:

* [network-apim.json][network-arm]
* [network-apim.parameters.json][network-parameters-arm]
* [apim.json][apim-arm]
* [apim.parameters.json][apim-parameters-arm]

Szablon *network-apim.json* umożliwia wdrożenie nowej podsieci i sieciowej grupy zabezpieczeń w sieci wirtualnej, w której wdrożono klaster usługi Service Fabric.

W poniższych sekcjach opisano zasoby definiowane za pomocą szablonu *apim.json*. Poszczególne sekcje zawierają linki do dokumentacji szablonów, umożliwiające uzyskanie dodatkowych informacji. Konfigurowalne parametry, zdefiniowane w pliku *apim.parameters.json*, zostaną ustawione w dalszej części tego artykułu.

### <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

Szablon [Microsoft.ApiManagement/service](/azure/templates/microsoft.apimanagement/service) opisuje wystąpienie usługi API Management i zawiera następujące informacje: nazwę, jednostkę SKU lub warstwę, lokalizację grupy zasobów oraz informacje o wydawcy i sieci wirtualnej.

### <a name="microsoftapimanagementservicecertificates"></a>Microsoft.ApiManagement/service/certificates

Szablon [Microsoft.ApiManagement/service/certificates](/azure/templates/microsoft.apimanagement/service/certificates) umożliwia konfigurowanie zabezpieczeń usługi API Management. W ramach odnajdywania usługi usługa API Management musi zostać uwierzytelniona w klastrze usługi Service Fabric przy użyciu certyfikatu klienta, który ma dostęp do klastra. W tym artykule jest używany certyfikat określony wcześniej podczas tworzenia [klastra Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md#createvaultandcert_anchor), która domyślnie może służyć do dostępu do klastra.

W tym artykule używa tego samego certyfikatu uwierzytelniania klienta i zabezpieczeń między węzłami klastra. Jeśli masz skonfigurowany osobny certyfikat klienta, możesz go używać do korzystania z klastra usługi Service Fabric. Podaj **nazwę**, **hasło** i **dane** (szyfrowany ciąg base64) dotyczące pliku klucza prywatnego (pfx) certyfikatu klastra, który został określony podczas tworzenia klastra usługi Service Fabric.

### <a name="microsoftapimanagementservicebackends"></a>Microsoft.ApiManagement/service/backends

Szablon [Microsoft.ApiManagement/service/backends](/azure/templates/microsoft.apimanagement/service/backends) opisuje usługę zaplecza, do której jest przekazywany ruch.

W przypadku usługi Service Fabric zapleczem jest klaster usługi Service Fabric, a nie określona usługa Service Fabric. Dzięki temu za pomocą jednej zasady można kierować ruch do więcej niż jednej usługi w klastrze. Pole **url** zawiera w pełni kwalifikowaną nazwę usługi w klastrze, do której są domyślnie kierowane wszystkie żądania, jeśli w zasadach zaplecza nie podano nazwy usługi. Jeśli nie potrzebujesz usługi rezerwowej, możesz użyć wymyślonej nazwy, takiej jak „fabric:/fake/service”. Pole **resourceId** określa punkt końcowy zarządzania klastrem.  Pola **clientCertificateThumbprint** i **serverCertificateThumbprints** identyfikują certyfikaty używane do uwierzytelniania w klastrze.

### <a name="microsoftapimanagementserviceproducts"></a>Microsoft.ApiManagement/service/products

Szablon [Microsoft.ApiManagement/service/products](/azure/templates/microsoft.apimanagement/service/products) umożliwia utworzenie produktu. W usłudze Azure API Management produkt zawiera co najmniej jeden interfejs API oraz limit przydziału użycia i warunki użytkowania. Po opublikowaniu produktu deweloperzy mogą go zasubskrybować i zacząć korzystać z jego interfejsów API.

W polu **displayName** wpisz opisową nazwę produktu, a w polu **description** — opis. W tym artykule wymagana jest subskrypcja, ale subskrypcja zatwierdzenia przez administratora nie jest.  **Stan** tego produktu ma wartość „opublikowany” i jest widoczny dla subskrybentów.

### <a name="microsoftapimanagementserviceapis"></a>Microsoft.ApiManagement/service/apis

Szablon [Microsoft.ApiManagement/service/apis](/azure/templates/microsoft.apimanagement/service/apis) umożliwia utworzenie interfejsu API. Interfejs API usługi API Management reprezentuje zestaw operacji, które mogą być wywoływane przez aplikacje klienckie. Po dodaniu operacji do produktu jest dodawany interfejs API, który można opublikować. Opublikowany interfejs API można zasubskrybować, a deweloperzy mogą go używać.

* Pole **displayName** może zawierać dowolną nazwę interfejsu API. W tym artykule należy użyć "Service Fabric App".
* Pole **name** zawiera unikatową i opisową nazwę interfejsu API, taką jak „service-fabric-app”. Jest ona wyświetlana w portalach dewelopera i wydawcy.
* Pole **serviceUrl** zawiera odwołanie do usługi HTTP implementującej interfejs API. Usługa API Management przekazuje żądania na ten adres. W przypadku zapleczy usługi Service Fabric ta wartość adresu URL nie jest używana. Można tu wpisać dowolną wartość. W tym artykule, na przykład "http:\//servicefabric".
* Wartość pola **path** jest dołączana do podstawowego adresu URL usługi API Management. Podstawowy adres URL jest wspólny dla wszystkich interfejsów API hostowanych przez wystąpienie usługi API Management. W usłudze API Management interfejsy API są rozróżniane na podstawie sufiksów, dlatego sufiksy poszczególnych interfejsów API dla danego wydawcy muszą być unikatowe.
* Pole **protocols** określa, których protokołów można używać w celu uzyskania dostępu do interfejsu API. W tym artykule listy **http** i **https**.
* Pole **path** jest sufiksem interfejsu API. W tym artykule należy użyć "wartości myapp".

### <a name="microsoftapimanagementserviceapisoperations"></a>Microsoft.ApiManagement/service/apis/operations

[Microsoft.ApiManagement/service/apis/operations](/azure/templates/microsoft.apimanagement/service/apis/operations) Przed użyciem interfejsu API usługi API Management należy dodać do niego operacje.  Klienci zewnętrzni używają operacji do komunikowania się z bezstanową usługą ASP.NET Core uruchomioną w klastrze usługi Service Fabric.

Aby dodać operację interfejsu API frontonu, podaj następujące wartości:

* Pola **displayName** i **description** opisują operację. W tym artykule należy użyć "Values".
* Pole **method** określa polecenie HTTP.  W tym artykule, należy określić **UZYSKAĆ**.
* Wartość pola **urlTemplate** jest dołączana do podstawowego adresu URL interfejsu API i określa pojedynczą operację HTTP.  W tym artykule, należy użyć `/api/values` Jeśli dodano usługę zaplecza platformy .NET lub `getMessage` Jeśli dodano usługę zaplecza platformy Java.  Określona tutaj ścieżka URL jest domyślnie wysyłana do usługi Service Fabric zaplecza. W przypadku podania ścieżki URL używanej przez usługę, takiej jak „/api/values”, operacja działa bez dodatkowych modyfikacji. Można również podać ścieżkę URL, która różni się od ścieżki URL używanej przez usługę Service Fabric zaplecza. W takim przypadku należy później określić nadpisanie ścieżki w zasadach operacji.

### <a name="microsoftapimanagementserviceapispolicies"></a>Microsoft.ApiManagement/service/apis/policies

Szablon [Microsoft.ApiManagement/service/apis/policies](/azure/templates/microsoft.apimanagement/service/apis/policies) umożliwia utworzenie zasad zaplecza, które pozwalają zintegrować wszystkie elementy. Zasady te umożliwiają skonfigurowanie usługi Service Fabric zaplecza, do której są kierowane żądania. Zasady można zastosować do dowolnej operacji interfejsu API.  Aby uzyskać więcej informacji, zobacz [Omówienie zasad](/azure/api-management/api-management-howto-policies).

[Konfiguracja zaplecza dla usługi Service Fabric](/azure/api-management/api-management-transformation-policies#SetBackendService) udostępnia następujące możliwości sterowania routingiem żądań:

* Wybieranie wystąpienia usługi przez podanie nazwy wystąpienia usługi Service Fabric — zakodowanej (na przykład `"fabric:/myapp/myservice"`) lub wygenerowanej na podstawie żądania HTTP (na przykład `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
* Rozpoznawanie partycji przez wygenerowanie klucza partycji przy użyciu dowolnego schematu partycjonowania usługi Service Fabric.
* Wybieranie repliki dla usług stanowych.
* Warunki ponownego rozpoznawania, które umożliwiają określenie warunków ponownego rozpoznawania lokalizacji usługi i wysyłania żądania.

**policyContent** to zawartość XML zasad, ujęta w znaki Json zmieniające znaczenie.  Utwórz zasady zaplecza kierujące żądania bezpośrednio do usługi bezstanowej .NET lub Java wdrożonego wcześniej w tym artykule. Dodaj zasady `set-backend-service` w obszarze zasad ruchu przychodzącego.  Wartość *sf-service-instance-name* zastąp ciągiem `fabric:/ApiApplication/WebApiService`, jeśli wcześniej wdrożono usługę zaplecza platformy .NET, lub ciągiem `fabric:/EchoServerApplication/EchoServerService`, jeśli wdrożono usługę platformy Java.  Pole *backend-id* zawiera odwołanie do zasobu zaplecza, w tym przypadku zasobu `Microsoft.ApiManagement/service/backends` zdefiniowanego w szablonie *apim.json*. Pole *backend-id* może także odwoływać się do innego zasobu zaplecza, utworzonego za pomocą interfejsów API usługi API Management. W tym artykule, należy ustawić *backend-id* wartość *service_fabric_backend_name* parametru.

```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
        sf-resolve-condition="@(context.LastError?.Reason == "BackendConnectionFailure")" />
  </inbound>
  <backend>
    <base/>
  </backend>
  <outbound>
    <base/>
  </outbound>
</policies>
```

Aby uzyskać pełny zestaw atrybutów zasad zaplecza usługi Service Fabric, zobacz[API Management back-end documentation (Dokumentacja zaplecza usługi API Management)](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService).

## <a name="set-parameters-and-deploy-api-management"></a>Ustawianie parametrów i wdrażanie usługi API Management

Wypełnij następujące puste parametry szablonu *apim.parameters.json* dla danego wdrożenia.

|Parametr|Wartość|
|---|---|
|apimInstanceName|sf-apim|
|apimPublisherEmail|myemail@contosos.com|
|apimSku|Developer|
|serviceFabricCertificateName|sfclustertutorialgroup320171031144217|
|certificatePassword|q6D7nN%6ck@6|
|serviceFabricCertificateThumbprint|C4C1E541AD512B8065280292A8BA6079C3F26F10 |
|serviceFabricCertificate|&lt;ciąg zakodowany w formacie Base-64&gt;|
|url_path|/api/values|
|clusterHttpManagementEndpoint|https://mysfcluster.southcentralus.cloudapp.azure.com:19080|
|inbound_policy|&lt;Ciąg XML&gt;|

Parametry *certificatePassword* i *serviceFabricCertificateThumbprint* muszą być zgodne z certyfikatem klastra użytym podczas konfigurowania klastra.

Parametr *serviceFabricCertificate* zawiera certyfikat w postaci ciągu zakodowanego w formacie Base-64, który można wygenerować za pomocą następującego skryptu:

```powershell
$bytes = [System.IO.File]::ReadAllBytes("C:\mycertificates\sfclustertutorialgroup220171109113527.pfx");
$b64 = [System.Convert]::ToBase64String($bytes);
[System.Io.File]::WriteAllText("C:\mycertificates\sfclustertutorialgroup220171109113527.txt", $b64);
```

W parametrze *inbound_policy* wartość *sf-service-instance-name* zastąp ciągiem `fabric:/ApiApplication/WebApiService`, jeśli wcześniej wdrożono usługę zaplecza platformy .NET, lub ciągiem `fabric:/EchoServerApplication/EchoServerService`, jeśli wdrożono usługę platformy Java. Pole *backend-id* zawiera odwołanie do zasobu zaplecza, w tym przypadku zasobu `Microsoft.ApiManagement/service/backends` zdefiniowanego w szablonie *apim.json*. Pole *backend-id* może także odwoływać się do innego zasobu zaplecza, utworzonego za pomocą interfejsów API usługi API Management. W tym artykule, należy ustawić *backend-id* wartość *service_fabric_backend_name* parametru.

```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
        sf-resolve-condition="@(context.LastError?.Reason == "BackendConnectionFailure")" />
  </inbound>
  <backend>
    <base/>
  </backend>
  <outbound>
    <base/>
  </outbound>
</policies>
```

Następujący skrypt umożliwia wdrożenie szablonu i plików parametrów usługi Resource Manager dla usługi API Management:

```powershell
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"
$templatepath="C:\clustertemplates"

New-AzResourceGroupDeployment -ResourceGroupName $groupname -TemplateFile "$templatepath\network-apim.json" -TemplateParameterFile "$templatepath\network-apim.parameters.json" -Verbose

New-AzResourceGroupDeployment -ResourceGroupName $groupname -TemplateFile "$templatepath\apim.json" -TemplateParameterFile "$templatepath\apim.parameters.json" -Verbose
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group deployment create --name ApiMgmtNetworkDeployment --resource-group $ResourceGroupName --template-file network-apim.json --parameters @network-apim.parameters.json

az group deployment create --name ApiMgmtDeployment --resource-group $ResourceGroupName --template-file apim.json --parameters @apim.parameters.json
```

## <a name="test-it"></a>Testowanie

Teraz możesz wysłać żądanie do usługi zaplecza Service Fabric za pośrednictwem usługi API Management bezpośrednio z witryny [Azure Portal](https://portal.azure.com).

 1. W usłudze API Management wybierz pozycję **Interfejs API**.
 2. W interfejsie API **aplikacji usługi Service Fabric** utworzonym w poprzednich krokach wybierz kartę **Test**, a następnie operację **Values**.
 3. Kliknij przycisk **Wyślij**, aby wysłać żądanie testowe do usługi zaplecza.  Powinna zostać wyświetlona mniej więcej taka odpowiedź HTTP:

    ```http
    HTTP/1.1 200 OK

    Transfer-Encoding: chunked

    Content-Type: application/json; charset=utf-8

    Vary: Origin

    Ocp-Apim-Trace-Location: https://apimgmtstodhwklpry2xgkdj.blob.core.windows.net/apiinspectorcontainer/PWSQOq_FCDjGcaI1rdMn8w2-2?sv=2015-07-08&sr=b&sig=MhQhzk%2FEKzE5odlLXRjyVsgzltWGF8OkNzAKaf0B1P0%3D&se=2018-01-28T01%3A04%3A44Z&sp=r&traceId=9f8f1892121e445ea1ae4d2bc8449ce4

    Date: Sat, 27 Jan 2018 01:04:44 GMT


    ["value1", "value2"]
    ```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Klaster składa się z innych zasobów platformy Azure poza samym zasobem klastra. Najprostszym sposobem na usunięcie klastra i wszystkich wykorzystywanych przez niego zasobów jest usunięcie grupy zasobów.

Logowanie do platformy Azure i wybierz identyfikator subskrypcji, z którą chcesz usunąć klaster.  Identyfikator subskrypcji można uzyskać po zalogowaniu się do [witryny Azure Portal](https://portal.azure.com). Usuń grupę zasobów i wszystkie zasoby klastra, używając [polecenia cmdlet Remove-AzResourceGroup](/en-us/powershell/module/az.resources/remove-azresourcegroup).

```powershell
$ResourceGroupName = "sfclustertutorialgroup"
Remove-AzResourceGroup -Name $ResourceGroupName -Force
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się więcej o korzystaniu z [usługi API Management](/azure/api-management/import-and-publish).

[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[apim-arm]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/service-integration/apim.json
[apim-parameters-arm]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/service-integration/apim.parameters.json

[network-arm]: https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/service-integration/network-apim.json
[network-parameters-arm]: https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/service-integration/network-apim.parameters.json

<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-tutorial-deploy-api-management/sf-apim-topology-overview.png
vice-fabric-scripts-and-templates/blob/master/templates/service-integration/network-apim.parameters.jsonn

<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-tutorial-deploy-api-management/sf-apim-topology-overview.png
