---
title: Funkcje szablonu usługi Resource Manager platformy Azure — zasoby | Dokumentacja firmy Microsoft
description: Opisuje funkcje, aby użyć w szablonie usługi Azure Resource Manager można pobrać wartości dotyczące zasobów.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 05/21/2019
ms.author: tomfitz
ms.openlocfilehash: dcad4b988f37d46a0b843fbf905e18011bc4e313
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65990756"
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Funkcje zasobów dla szablonów usługi Azure Resource Manager

Usługa Resource Manager zapewnia następujące funkcje w celu uzyskania wartości zasobu:

* [Lista *](#list)
* [dostawcy](#providers)
* [Odwołanie](#reference)
* [resourceGroup](#resourcegroup)
* [resourceId](#resourceid)
* [Subskrypcja](#subscription)

Aby uzyskać wartości parametrów, zmienne lub bieżącego wdrożenia, zobacz [funkcji wartość wdrożeniowych](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="list"></a>Lista *

`list{Value}(resourceName or resourceIdentifier, apiVersion, functionValues)`

Składnia tej funkcji zależy od nazwy operacji listy. Każda implementacja zwraca wartości dla typu zasobu, który obsługuje operację listy. Nazwa operacji musi rozpoczynać się `list`. Niektóre typowego użycia są `listKeys` i `listSecrets`. 

### <a name="parameters"></a>Parametry

| Parametr | Wymagane | Typ | Opis |
|:--- |:--- |:--- |:--- |
| resourceName lub resourceIdentifier |Yes |ciąg |Unikatowy identyfikator dla zasobu. |
| apiVersion |Yes |ciąg |Wersja interfejsu API stanu środowiska uruchomieniowego zasobu. Zazwyczaj w formacie **rrrr mm-dd**. |
| functionValues |Nie |obiekt | Obiekt, który zawiera wartości dla funkcji. Podaj tylko ten obiekt funkcji, które obsługują odbieranie obiekt o wartości parametrów, takich jak **listAccountSas** na koncie magazynu. W tym artykule przedstawiono przykładowy przekazywania wartości funkcji. | 

### <a name="implementations"></a>Implementacje

Możliwości wykorzystania listy * są wyświetlane w poniższej tabeli.

| Typ zasobu | Nazwa funkcji |
| ------------- | ------------- |
| Microsoft.AnalysisServices/servers | [listGatewayStatus](/rest/api/analysisservices/servers/listgatewaystatus) |
| Microsoft.Automation/automationAccounts | [klucze list](/rest/api/automation/keys/listbyautomationaccount) |
| Microsoft.Batch/batchAccounts | [listkeys](/rest/api/batchmanagement/batchaccount/getkeys) |
| Microsoft.BatchAI/workspaces/experiments/jobs | [listoutputfiles](/rest/api/batchai/jobs/listoutputfiles) |
| Microsoft.Cache/redis | [klucze list](/rest/api/redis/redis/listkeys) |
| Microsoft.CognitiveServices/accounts | [klucze list](/rest/api/cognitiveservices/accountmanagement/accounts/listkeys) |
| Microsoft.ContainerRegistry/registries | [listBuildSourceUploadUrl](/rest/api/containerregistry/registries%20(tasks)/getbuildsourceuploadurl) |
| Microsoft.ContainerRegistry/registries | [listCredentials](/rest/api/containerregistry/registries/listcredentials) |
| Microsoft.ContainerRegistry/registries | [listPolicies](/rest/api/containerregistry/registries/listpolicies) |
| Microsoft.ContainerRegistry/registries | [listUsages](/rest/api/containerregistry/registries/listusages) |
| Microsoft.ContainerRegistry/registries/webhooks | [listEvents](/rest/api/containerregistry/webhooks/listevents) |
| Microsoft.ContainerService/managedClusters | [listClusterAdminCredential](/rest/api/aks/managedclusters/listclusteradmincredentials) |
| Microsoft.ContainerService/managedClusters | [listClusterUserCredential](/rest/api/aks/managedclusters/listclusterusercredentials) |
| Microsoft.DataFactory/datafactories/gateways | listauthkeys |
| Microsoft.DataFactory/factories/integrationruntimes | [listauthkeys](/rest/api/datafactory/integrationruntimes/listauthkeys) |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers | [listSasTokens](/rest/api/datalakeanalytics/storageaccounts/listsastokens) |
| Microsoft.Devices/iotHubs | [listkeys](/rest/api/iothub/iothubresource/listkeys) |
| Microsoft.Devices/provisioningServices/keys | [listkeys](/rest/api/iot-dps/iotdpsresource/listkeysforkeyname) |
| Microsoft.Devices/provisioningServices | [listkeys](/rest/api/iot-dps/iotdpsresource/listkeys) |
| Microsoft.DevTestLab/labs | [ListVhds](/rest/api/dtl/labs/listvhds) |
| Microsoft.DevTestLab/labs/schedules | [ListApplicable](/rest/api/dtl/schedules/listapplicable) |
| Microsoft.DevTestLab/labs/users/serviceFabrics | [ListApplicableSchedules](/rest/api/dtl/servicefabrics/listapplicableschedules) |
| Microsoft.DevTestLab/labs/virtualMachines | [ListApplicableSchedules](/rest/api/dtl/virtualmachines/listapplicableschedules) |
| Microsoft.DocumentDB/databaseAccounts | [listConnectionStrings](/rest/api/cosmos-db-resource-provider/databaseaccounts/listconnectionstrings) |
| Microsoft.DocumentDB/databaseAccounts | [klucze list](/rest/api/cosmos-db-resource-provider/databaseaccounts/listkeys) |
| Microsoft.DomainRegistration | [listDomainRecommendations](/rest/api/appservice/domains/listrecommendations) |
| Microsoft.DomainRegistration/topLevelDomains | [listAgreements](/rest/api/appservice/topleveldomains/listagreements) |
| Microsoft.EventGrid/topics | [klucze list](/rest/api/eventgrid/topics/listsharedaccesskeys) |
| Microsoft.EventHub/namespaces/authorizationRules | [listkeys](/rest/api/eventhub/namespaces/listkeys) |
| Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules | [listkeys](/rest/api/eventhub/disasterrecoveryconfigs/listkeys) |
| Microsoft.EventHub/namespaces/eventhubs/authorizationRules | [listkeys](/rest/api/eventhub/eventhubs/listkeys) |
| Microsoft.ImportExport/jobs | [listBitLockerKeys](/rest/api/storageimportexport/bitlockerkeys/list) |
| Microsoft.Logic/integrationAccounts/agreements | [listContentCallbackUrl](/rest/api/logic/agreements/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/assemblies | [listContentCallbackUrl](/rest/api/logic/integrationaccountassemblies/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts | [listCallbackUrl](/rest/api/logic/integrationaccounts/getcallbackurl) |
| Microsoft.Logic/integrationAccounts | [listKeyVaultKeys](/rest/api/logic/integrationaccounts/listkeyvaultkeys) |
| Microsoft.Logic/integrationAccounts/maps | [listContentCallbackUrl](/rest/api/logic/maps/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/partners | [listContentCallbackUrl](/rest/api/logic/partners/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/schemas | [listContentCallbackUrl](/rest/api/logic/schemas/listcontentcallbackurl) |
| Microsoft.Logic/workflows | [listCallbackUrl](/rest/api/logic/workflows/listcallbackurl) |
| Microsoft.Logic/workflows | [listSwagger](/rest/api/logic/workflows/listswagger) |
| Microsoft.MachineLearning/webServices | [listkeys](/rest/api/machinelearning/webservices/listkeys) |
| Microsoft.MachineLearning/Workspaces | listworkspacekeys |
| Microsoft.MachineLearningServices/workspaces/computes | klucze list |
| Microsoft.MachineLearningServices/workspaces | klucze list |
| Microsoft.Maps/accounts | [klucze list](/rest/api/maps-management/accounts/listkeys) |
| Microsoft.Media/mediaservices/assets | [listContainerSas](/rest/api/media/assets/listcontainersas) |
| Microsoft.Media/mediaservices/assets | [listStreamingLocators](/rest/api/media/assets/liststreaminglocators) |
| Microsoft.Media/mediaservices/streamingLocators | [listContentKeys](/rest/api/media/streaminglocators/listcontentkeys) |
| Microsoft.Media/mediaservices/streamingLocators | [listPaths](/rest/api/media/streaminglocators/listpaths) |
| Microsoft.NotificationHubs/Namespaces/authorizationRules | [listkeys](/rest/api/notificationhubs/namespaces/listkeys) |
| Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules | [listkeys](/rest/api/notificationhubs/notificationhubs/listkeys) |
| Microsoft.OperationalInsights/workspaces | [klucze list](/rest/api/loganalytics/workspaces%202015-03-20/listkeys) |
| Microsoft.Relay/namespaces/authorizationRules | [listkeys](/rest/api/relay/namespaces/listkeys) |
| Microsoft.Relay/namespaces/HybridConnections/authorizationRules | [listkeys](/rest/api/relay/hybridconnections/listkeys) |
| Microsoft.Relay/namespaces/WcfRelays/authorizationRules | [listkeys](/rest/api/relay/wcfrelays/listkeys) |
| Microsoft.Search/searchServices | [listAdminKeys](/rest/api/searchmanagement/adminkeys/get) |
| Microsoft.Search/searchServices | [listQueryKeys](/rest/api/searchmanagement/querykeys/listbysearchservice) |
| Microsoft.ServiceBus/namespaces/authorizationRules | [listkeys](/rest/api/servicebus/namespaces/listkeys) |
| Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules | [listkeys](/rest/api/servicebus/disasterrecoveryconfigs/listkeys) |
| Microsoft.ServiceBus/namespaces/queues/authorizationRules | [listkeys](/rest/api/servicebus/queues/listkeys) |
| Microsoft.ServiceBus/namespaces/topics/authorizationRules | [listkeys](/rest/api/servicebus/topics/listkeys) |
| Microsoft.SignalRService/SignalR | [listkeys](/rest/api/signalr/signalr/listkeys) |
| Microsoft.Storage/storageAccounts | [listAccountSas](/rest/api/storagerp/storageaccounts/listaccountsas) |
| Microsoft.Storage/storageAccounts | [listkeys](/rest/api/storagerp/storageaccounts/listkeys) |
| Microsoft.Storage/storageAccounts | [listServiceSas](/rest/api/storagerp/storageaccounts/listservicesas) |
| Microsoft.StorSimple/managers/devices | [listFailoverSets](/rest/api/storsimple/devices/listfailoversets) |
| Microsoft.StorSimple/managers/devices | [listFailoverTargets](/rest/api/storsimple/devices/listfailovertargets) |
| Microsoft.StorSimple/managers | [listActivationKey](/rest/api/storsimple/managers/getactivationkey) |
| Microsoft.StorSimple/managers | [listPublicEncryptionKey](/rest/api/storsimple/managers/getpublicencryptionkey) |
| Microsoft.Web/connectionGateways | ListStatus |
| Microsoft.Web/Connections | listconsentlinks |
| Microsoft.Web/customApis | listWsdlInterfaces |
| microsoft.web/locations | listwsdlinterfaces |
| Microsoft.Web/Sites/Functions | [listsecrets](/rest/api/appservice/webapps/listfunctionsecrets) |
| Microsoft.Web/Sites/hybridconnectionnamespaces/relays | [listkeys](/rest/api/appservice/webapps/listhybridconnectionkeys) |
| Microsoft.Web/Sites | [listsyncfunctiontriggerstatus](/rest/api/appservice/webapps/listsyncfunctiontriggers) |
| Microsoft.Web/Sites/Slots/Functions | [listsecrets](/rest/api/appservice/webapps/listfunctionsecretsslot) |

Aby ustalić, typów zasobów, które mają operacja listy, dostępne są następujące opcje:

* Widok [operacji interfejsu API REST](/rest/api/) dla dostawcy zasobów i wygląd na potrzeby operacji listy. Na przykład konta magazynu mają [operacji listKeys](/rest/api/storagerp/storageaccounts).
* Użyj [Get AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) polecenia cmdlet programu PowerShell. Poniższy przykład pobiera wszystkie operacje dotyczące list dla kont magazynu:

  ```powershell
  Get-AzProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Użyj następującego polecenia wiersza polecenia platformy Azure, aby filtrować Wyświetl listę operacji:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

### <a name="return-value"></a>Wartość zwracana

Zwrócony obiekt jest zależna od funkcji listy, której używasz. Na przykład klucze listy dla konta magazynu zwraca następujący format:

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

Inne funkcje listy mają różne formaty zwrotu. Aby wyświetlić format funkcji, dołączyć sekcję danych wyjściowych przedstawiony przykładowy szablon.

### <a name="remarks"></a>Uwagi

Określ zasób, używając nazwy zasobu lub [funkcja resourceId](#resourceid). Podczas korzystania z funkcji listy, w tym samym szablonie, który wdraża przywoływany zasób, należy użyć nazwy zasobu.

Jeśli używasz **listy** w zasobie warunkowo wdrożeniu funkcji szacowania funkcji będzie nawet, jeśli zasób nie jest wdrożona. Jeśli wystąpi błąd **listy** funkcja odnosi się do zasobu, który nie istnieje. Użyj **Jeśli** funkcję, aby upewnić się, czy funkcja jest oceniane tylko, gdy zasób jest wdrażany. Zobacz [Jeśli funkcja](resource-group-template-functions-logical.md#if) przykładowego szablonu, który używa, jeśli i listę warunkowo wdrożony zasób.

### <a name="example"></a>Przykład

Następujące [przykładowy szablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/listkeys.json) przedstawia sposób zwrócenia klucze podstawowe i pomocnicze z konta magazynu w sekcji danych wyjściowych. Zwraca token sygnatury dostępu Współdzielonego dla konta magazynu. 

Aby uzyskać token sygnatury dostępu Współdzielonego, należy przekazać obiekt czas wygaśnięcia. Czas wygaśnięcia musi przypadać w przyszłości. W tym przykładzie jest przeznaczona do korzystania z funkcji listy. Zazwyczaj można będzie użyć tokenu sygnatury dostępu Współdzielonego w wartości zasobów zamiast zwracanie go jako wartość wyjściową. Wartości wyjściowe są zapisywane w historii wdrożenia i nie są bezpieczne.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagename": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus"
        },
        "accountSasProperties": {
            "type": "object",
            "defaultValue": {
                "signedServices": "b",
                "signedPermission": "r",
                "signedExpiry": "2018-08-20T11:00:00Z",
                "signedResourceTypes": "s"
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2018-02-01",
            "name": "[parameters('storagename')]",
            "location": "[parameters('location')]",
            "type": "Microsoft.Storage/storageAccounts",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {
                "supportsHttpsTrafficOnly": false,
                "accessTier": "Hot",
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        },
                        "file": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "keys": {
            "type": "object",
            "value": "[listKeys(parameters('storagename'), '2018-02-01')]"
        },
        "accountSAS": {
            "type": "object",
            "value": "[listAccountSas(parameters('storagename'), '2018-02-01', parameters('accountSasProperties'))]"
        }
    }
}
```

## <a name="providers"></a>dostawcy

`providers(providerNamespace, [resourceType])`

Zwraca informacje o dostawcy zasobów i jego obsługiwane typy zasobów. Jeśli nie podasz zasobu o typie, funkcja zwraca obsługiwane typy dla dostawcy zasobów.

### <a name="parameters"></a>Parametry

| Parametr | Wymagane | Typ | Opis |
|:--- |:--- |:--- |:--- |
| providerNamespace |Yes |ciąg |Namespace dostawcy |
| Typ zasobu |Nie |ciąg |Typ zasobu w ramach określonego obszaru nazw. |

### <a name="return-value"></a>Wartość zwracana

Każdego obsługiwanego typu, jest zwracany w następującym formacie: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Kolejność zwracanych wartości tablicy nie jest gwarantowana.

### <a name="example"></a>Przykład

Następujące [przykładowy szablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/providers.json) pokazuje, jak korzystać z funkcji dostawcy:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "providerNamespace": {
            "type": "string"
        },
        "resourceType": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers(parameters('providerNamespace'), parameters('resourceType'))]",
            "type" : "object"
        }
    }
}
```

Aby uzyskać **Microsoft.Web** dostawcy zasobów i **witryn** typ zasobu w poprzednim przykładzie zwracany obiekt w następującym formacie:

```json
{
  "resourceType": "sites",
  "locations": [
    "South Central US",
    "North Europe",
    "West Europe",
    "Southeast Asia",
    ...
  ],
  "apiVersions": [
    "2016-08-01",
    "2016-03-01",
    "2015-08-01-preview",
    "2015-08-01",
    ...
  ]
}
```

## <a name="reference"></a>Odwołanie

`reference(resourceName or resourceIdentifier, [apiVersion], ['Full'])`

Zwraca obiekt reprezentujący stan czasu wykonywania zasobu.

### <a name="parameters"></a>Parametry

| Parametr | Wymagane | Typ | Opis |
|:--- |:--- |:--- |:--- |
| resourceName lub resourceIdentifier |Yes |ciąg |Nazwa lub identyfikator zasobu. |
| apiVersion |Nie |ciąg |Wersja interfejsu API określonego zasobu. Ten parametr należy uwzględnić, jeśli zasób nie jest zainicjowana obsługa administracyjna w obrębie tego samego szablonu. Zazwyczaj w formacie **rrrr mm-dd**. |
| "Full" |Nie |ciąg |Wartość określająca, czy należy zwrócić obiekt wszystkich zasobów. Jeśli nie określisz `'Full'`, zwracany jest tylko obiekt do właściwości zasobu. Pełny obiekt zawiera wartości, takie jak identyfikator zasobu i lokalizacji. |

### <a name="return-value"></a>Wartość zwracana

Każdy typ zasobu zwraca różne właściwości odwołanie funkcji. Funkcja nie zwraca jednego, wstępnie zdefiniowany format. Ponadto zwrócona wartość różni się zależnie od tego, czy określony obiekt pełne. Aby wyświetlić właściwości dla typu zasobu, należy przywrócić obiektu w sekcji danych wyjściowych, jak pokazano w przykładzie.

### <a name="remarks"></a>Uwagi

Funkcja odwołanie pobiera stan środowiska uruchomieniowego wdrożonej wcześniej zasobów lub zasobu, wdrożonych w bieżącym szablonie. W tym artykule przedstawiono przykłady oba scenariusze. Podczas odwoływania się do zasobów w bieżącym szablonie, podaj nazwę zasobu jako parametr. Podczas odwoływania się do wcześniej wdrożony zasób, należy podać wersję interfejsu API i identyfikator zasobu dla zasobu. Należy określić prawidłowe wersje interfejsu API dla zasobu w [odwołanie do szablonu](/azure/templates/).

Odwołanie funkcji należy używać tylko w właściwości definicji zasobu i sekcję danych wyjściowych szablonu lub wdrożenia. Gdy jest używane z [iteracji właściwość](resource-group-create-multiple.md#property-iteration), możesz użyć funkcji odwołania dla `input` ponieważ wyrażenie jest przypisany do właściwości zasobu. Nie można używać z `count` ponieważ musi zostać określona liczba, przed odwołanie funkcji nie zostanie rozwiązany.

Za pomocą funkcji odwołania, niejawnie Deklarujesz, jeden zasób jest zależny od innego zasobu, jeśli przywoływany zasób jest obsługiwana w ramach tego samego szablonu i odwołania do zasobu według jego nazwy (a nie identyfikator zasobu). Nie musisz również użyć właściwości dependsOn. Funkcja nie jest obliczane, dopóki nie zakończy się przywoływany zasób wdrożenia.

Jeśli używasz **odwołania** w zasobie warunkowo wdrożeniu funkcji szacowania funkcji będzie nawet, jeśli zasób nie jest wdrożona.  Jeśli wystąpi błąd **odwołania** funkcja odnosi się do zasobu, który nie istnieje. Użyj **Jeśli** funkcję, aby upewnić się, czy funkcja jest oceniane tylko, gdy zasób jest wdrażany. Zobacz [Jeśli funkcja](resource-group-template-functions-logical.md#if) przykładowego szablonu, który używa, jeśli i odwołania z warunkowo wdrożony zasób.

Aby wyświetlić nazwy właściwości i wartości dla typu zasobu, należy utworzyć szablon, który zwraca obiekt, w sekcji danych wyjściowych. W przypadku istniejącego zasobu tego typu szablonu zwraca obiekt bez wdrażania wszystkie nowe zasoby. 

Zazwyczaj można użyć **odwołania** funkcja zwraca określoną wartość z obiektu, takie jak identyfikator URI punktu końcowego obiektu blob lub w pełni kwalifikowaną nazwę domeny.

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')), '2016-03-30').dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

Użyj `'Full'` potrzebne wartości zasobów, które nie są częścią schematu właściwości. Na przykład aby ustawić zasady dostępu magazynu kluczy, pobieranie właściwości tożsamości dla maszyny wirtualnej.

```json
{
  "type": "Microsoft.KeyVault/vaults",
  "properties": {
    "tenantId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.tenantId]",
    "accessPolicies": [
      {
        "tenantId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.tenantId]",
        "objectId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.principalId]",
        "permissions": {
          "keys": [
            "all"
          ],
          "secrets": [
            "all"
          ]
        }
      }
    ],
    ...
```

Aby uzyskać kompletny przykład powyższy szablon, zobacz [Windows do usługi Key Vault](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo08.msiWindowsToKeyvault.json). Podobny przykład jest dostępny dla [Linux](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo07.msiLinuxToArm.json).

### <a name="example"></a>Przykład

Następujące [przykładowy szablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/referencewithstorage.json) wdraża zasobu, a następnie odwołuje się do tego zasobu.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      },
      "fullReferenceOutput": {
        "type": "object",
        "value": "[reference(parameters('storageAccountName'), '2016-12-01', 'Full')]"
      }
    }
}
``` 

Powyższy przykład zwraca dwa obiekty. Obiekt właściwości znajduje się w następującym formacie:

```json
{
   "creationTime": "2017-10-09T18:55:40.5863736Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

Pełny obiekt znajduje się w następującym formacie:

```json
{
  "apiVersion":"2016-12-01",
  "location":"southcentralus",
  "sku": {
    "name":"Standard_LRS",
    "tier":"Standard"
  },
  "tags":{},
  "kind":"Storage",
  "properties": {
    "creationTime":"2017-10-09T18:55:40.5863736Z",
    "primaryEndpoints": {
      "blob":"https://examplestorage.blob.core.windows.net/",
      "file":"https://examplestorage.file.core.windows.net/",
      "queue":"https://examplestorage.queue.core.windows.net/",
      "table":"https://examplestorage.table.core.windows.net/"
    },
    "primaryLocation":"southcentralus",
    "provisioningState":"Succeeded",
    "statusOfPrimary":"available",
    "supportsHttpsTrafficOnly":false
  },
  "subscriptionId":"<subscription-id>",
  "resourceGroupName":"functionexamplegroup",
  "resourceId":"Microsoft.Storage/storageAccounts/examplestorage",
  "referenceApiVersion":"2016-12-01",
  "condition":true,
  "isConditionTrue":true,
  "isTemplateResource":false,
  "isAction":false,
  "provisioningOperation":"Read"
}
```

Następujące [przykładowy szablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/reference.json) odwołuje się do konta magazynu, który nie jest wdrożony w tym szablonie. Konto magazynu już istnieje w ramach tej samej subskrypcji.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageResourceGroup": {
            "type": "string"
        },
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]",
            "type": "object"
        }
    }
}
```

## <a name="resourcegroup"></a>resourceGroup

`resourceGroup()`

Zwraca obiekt, który reprezentuje bieżącą grupę zasobów. 

### <a name="return-value"></a>Wartość zwracana

Zwrócony obiekt jest w następującym formacie:

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

### <a name="remarks"></a>Uwagi

`resourceGroup()` Nie można użyć funkcji w szablonie, który jest [wdrożone na poziomie subskrypcji](deploy-to-subscription.md). Można można używać tylko w szablonach, które są wdrożone w grupie zasobów.

Typowym zastosowaniem funkcji resourceGroup jest do tworzenia zasobów w tej samej lokalizacji co grupa zasobów. W poniższym przykładzie użyto lokalizacji grupy zasobów, aby przypisać lokalizacji dla witryny sieci web.

```json
"resources": [
   {
      "apiVersion": "2016-08-01",
      "type": "Microsoft.Web/sites",
      "name": "[parameters('siteName')]",
      "location": "[resourceGroup().location]",
      ...
   }
]
```

### <a name="example"></a>Przykład

Następujące [przykładowy szablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourcegroup.json) zwraca właściwości grupy zasobów.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "resourceGroupOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

Powyższy przykład zwraca obiekt, w następującym formacie:

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

## <a name="resourceid"></a>resourceId

`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

Zwraca unikatowy identyfikator zasobu. Aby użyć tej funkcji, jeśli nazwa zasobu jest niejednoznaczny lub nie jest aprowizowany w ramach tego samego szablonu. 

### <a name="parameters"></a>Parametry

| Parametr | Wymagane | Typ | Opis |
|:--- |:--- |:--- |:--- |
| subscriptionId |Nie |ciąg (format identyfikatora GUID w) |Wartością domyślną jest bieżąca subskrypcja. Należy określić tę wartość, gdy jest potrzebne do pobierania zasobów w innej subskrypcji. |
| resourceGroupName |Nie |ciąg |Wartość domyślna to bieżącej grupie zasobów. Należy określić tę wartość, gdy jest potrzebne do pobierania zasobów w innej grupie zasobów. |
| Typ zasobu |Yes |ciąg |Typ zasobu, włącznie z przestrzenią nazw dostawcy zasobów. |
| resourceName1 |Yes |ciąg |Nazwa zasobu. |
| resourceName2 |Nie |ciąg |Następny zasobu Nazwa segment Jeśli zasób jest zagnieżdżony. |

### <a name="return-value"></a>Wartość zwracana

Identyfikator jest zwracany w następującym formacie:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Uwagi

Gdy jest używane z [wdrażania na poziomie subskrypcji](deploy-to-subscription.md), `resourceId()` funkcji można pobrać jedynie identyfikator zasobami wdrożonymi na tym samym poziomie. Na przykład możesz uzyskać identyfikator definicji zasad lub definicji roli, ale nie identyfikator konta magazynu. W przypadku wdrożeń w grupie zasobów przeciwieństwo ma wartość true. Nie można pobrać Identyfikatora zasobu zasobów wdrożonych na poziomie subskrypcji.

Wartości parametrów, które określisz, zależą od tego, czy zasób jest w tej samej subskrypcji i grupie zasobów co bieżącego wdrożenia. Aby uzyskać identyfikator zasobu dla konta magazynu w tej samej subskrypcji i grupie zasobów, użyj:

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

Aby uzyskać identyfikator zasobu dla konta magazynu w tej samej subskrypcji, ale inną grupę zasobów, użyj:

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Aby uzyskać identyfikator zasobu dla konta magazynu w innej subskrypcji i grupie zasobów, użyj:

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Aby uzyskać identyfikator zasobu dla bazy danych w innej grupie zasobów, należy użyć:

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

Aby uzyskać identyfikator zasobu zasobów poziom subskrypcji, w przypadku wdrażania w zakresie subskrypcji, użyj:

```json
"[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
```

Często należy użyć tej funkcji, korzystając z konta magazynu lub sieci wirtualnej w grupie zasobów alternatywne. Poniższy przykład pokazuje, jak łatwo można używać zasobów z grupy zasobów zewnętrznych:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "subnet1Ref": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworkName'), parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="example"></a>Przykład

Następujące [przykładowy szablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourceid.json) zwraca identyfikator zasobu dla konta magazynu w grupie zasobów:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "sameRGOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentRGOutput": {
            "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentSubOutput": {
            "value": "[resourceId('11111111-1111-1111-1111-111111111111', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "nestedResourceOutput": {
            "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]",
            "type" : "string"
        }
    }
}
```

Dane wyjściowe z poprzedniego przykładu z wartościami domyślnymi będą:

| Name (Nazwa) | Typ | Wartość |
| ---- | ---- | ----- |
| sameRGOutput | Ciąg | /Subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/Providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Ciąg | /Subscriptions/{Current-Sub-ID}/resourceGroups/otherResourceGroup/Providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Ciąg | /Subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/otherResourceGroup/Providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Ciąg | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName |

## <a name="subscription"></a>subskrypcja

`subscription()`

Zwraca szczegółowe informacje o subskrypcji dla bieżącego wdrożenia. 

### <a name="return-value"></a>Wartość zwracana

Funkcja zwraca następujący format:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Przykład

Następujące [przykładowy szablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/subscription.json) przedstawia funkcję subskrypcji o nazwie w sekcję danych wyjściowych. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

## <a name="next-steps"></a>Kolejne kroki

* Aby uzyskać opis sekcje szablonu usługi Azure Resource Manager, zobacz [tworzenia usługi Azure Resource Manager](resource-group-authoring-templates.md).
* Aby scalić wiele szablonów, zobacz [przy użyciu szablonów połączonych z usługą Azure Resource Manager](resource-group-linked-templates.md).
* Do iteracji określoną liczbę razy podczas tworzenia dla typu zasobów, zobacz [tworzenie wielu wystąpień zasobów w usłudze Azure Resource Manager](resource-group-create-multiple.md).
* Aby zobaczyć, jak wdrożyć szablon został utworzony, zobacz [wdrażania aplikacji przy użyciu szablonu usługi Azure Resource Manager](resource-group-template-deploy.md).

