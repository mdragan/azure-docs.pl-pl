---
title: 'Usługa Azure Backup: Przywracanie maszyn wirtualnych platformy Azure przy użyciu interfejsu API REST'
description: zarządzanie operacjami przywracania kopii zapasowych maszyn wirtualnych platformy Azure przy użyciu interfejsu API REST
services: backup
author: pvrk
manager: shivamg
keywords: INTERFEJS API REST; Kopia zapasowa maszyny Wirtualnej platformy Azure; Przywracanie maszyny Wirtualnej platformy Azure;
ms.service: backup
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: pullabhk
ms.assetid: b8487516-7ac5-4435-9680-674d9ecf5642
ms.openlocfilehash: 4a65e8a855b9be797c1ceeacf4b74fea74697d00
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60646660"
---
# <a name="restore-azure-virtual-machines-using-rest-api"></a>Przywracanie maszyn wirtualnych platformy Azure przy użyciu interfejsu API REST

Po zakończeniu tworzenia kopii zapasowej maszyny wirtualnej platformy Azure za pomocą usługi Azure Backup jeden przywrócić cały maszynach wirtualnych platformy Azure lub dysków lub pliki z tej samej kopii zapasowej. W tym artykule opisano, jak przywrócić Maszynę wirtualną platformy Azure lub dyski przy użyciu interfejsu API REST.

Dla każdej operacji przywracania, trzeba najpierw zidentyfikować punkt odzyskiwania istotne.

## <a name="select-recovery-point"></a>Wybierz punkt odzyskiwania

Dostępne punkty odzyskiwania elementu kopii zapasowej mogą być wyświetlane za pomocą [listy punkt odzyskiwania interfejsu API REST](https://docs.microsoft.com/rest/api/backup/recoverypoints/list). Jest to prosty *UZYSKAĆ* operację, podając odpowiednie wartości.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints?api-version=2016-12-01
```

`{containerName}` i `{protectedItemName}` są konstruowane [tutaj](backup-azure-arm-userestapi-backupazurevms.md#example-responses-1). `{fabricName}` to "Azure".

*UZYSKAĆ* identyfikator URI ma wszystkie wymagane parametry. Nie ma potrzeby treść żądania dodatkowych

### <a name="responses"></a>Responses

|Name (Nazwa)  |Typ  |Opis  |
|---------|---------|---------|
|200 OK     |   [RecoveryPointResourceList](https://docs.microsoft.com/rest/api/backup/recoverypoints/list#recoverypointresourcelist)      |       OK  |

#### <a name="example-response"></a>Przykładowa odpowiedź

Raz *UZYSKAĆ* identyfikatora URI jest przesyłany, zwrócił odpowiedź 200 (OK).

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: 03453538-2f8d-46de-8374-143ccdf60f40
x-ms-client-request-id: c48f4436-ce3f-42da-b537-12710d4d1a24; c48f4436-ce3f-42da-b537-12710d4d1a24
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14998
x-ms-correlation-request-id: 03453538-2f8d-46de-8374-143ccdf60f40
x-ms-routing-request-id: SOUTHINDIA:20180604T071851Z:03453538-2f8d-46de-8374-143ccdf60f40
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 07:18:51 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/VM;testRG;testVM/recoveryPoints/20982486783671",
      "name": "20982486783671",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints",
      "properties": {
        "objectType": "IaasVMRecoveryPoint",
        "recoveryPointType": "AppConsistent",
        "recoveryPointTime": "2018-06-04T06:06:00.5121087Z",
        "recoveryPointAdditionalInfo": "",
        "sourceVMStorageType": "NormalStorage",
        "isSourceVMEncrypted": false,
        "isInstantIlrSessionActive": false,
        "recoveryPointTierDetails": [
          {
            "type": 1,
            "status": 1
          },
          {
            "type": 2,
            "status": 1
          }
        ],
        "isManagedVirtualMachine": true,
        "virtualMachineSize": "Standard_A1_v2",
        "originalStorageAccountOption": false
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/VM;testRG;testVM/recoveryPoints/23358112038108",
      "name": "23358112038108",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints",
      "properties": {
        "objectType": "IaasVMRecoveryPoint",
        "recoveryPointType": "CrashConsistent",
        "recoveryPointTime": "2018-06-03T06:20:56.3043878Z",
        "recoveryPointAdditionalInfo": "",
        "sourceVMStorageType": "NormalStorage",
        "isSourceVMEncrypted": false,
        "isInstantIlrSessionActive": false,
        "recoveryPointTierDetails": [
          {
            "type": 1,
            "status": 1
          },
          {
            "type": 2,
            "status": 1
          }
        ],
        "isManagedVirtualMachine": true,
        "virtualMachineSize": "Standard_A1_v2",
        "originalStorageAccountOption": false
      }
    },
......
```

Punkt odzyskiwania jest identyfikowany za pomocą `{name}` pole powyżej odpowiedzi.

## <a name="restore-disks"></a>Przywróć dyski

W przypadku trzeba dostosować tworzenia maszyny Wirtualnej z kopii zapasowej danych jeden tylko przywracanie dysków na wybranym koncie magazynu i Utwórz Maszynę wirtualną z tych dysków, zgodnie z ich wymaganiami. Konto magazynu powinny znajdować się w tym samym regionie co magazyn usługi recovery services i nie powinny być strefowo nadmiarowe. Dyski, a także konfigurację kopii zapasowej maszyny Wirtualnej ("vmconfig.json"), będą przechowywane w danym koncie magazynu.

Wyzwalanie przywracania dysków jest *WPIS* żądania. Aby dowiedzieć się więcej na temat operację przywracania dysków, zapoznaj się ["wyzwalanie przywracania" interfejsu API REST](https://docs.microsoft.com/rest/api/backup/restores/trigger).

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints/{recoveryPointId}/restore?api-version=2016-12-01
```

`{containerName}` i `{protectedItemName}` są konstruowane [tutaj](backup-azure-arm-userestapi-backupazurevms.md#example-responses-1). `{fabricName}` jest "Azure" i `{recoveryPointId}` jest `{name}` pole punktu odzyskiwania wymienione [powyżej](#example-response).

### <a name="create-request-body"></a>Tworzenie treści żądania

Aby wyzwolić przywracania dysku z kopii zapasowej maszyny Wirtualnej platformy Azure, poniżej przedstawiono składniki w treści żądania.

|Name (Nazwa)  |Typ  |Opis  |
|---------|---------|---------|
|properties     | [IaaSVMRestoreRequest](https://docs.microsoft.com/rest/api/backup/restores/trigger#iaasvmrestorerequest)        |    RestoreRequestResourceProperties     |

Aby uzyskać pełną listę definicji treści żądania i inne szczegóły, zobacz [wyzwalanie przywracania interfejsu API REST dokumentu](https://docs.microsoft.com/rest/api/backup/restores/trigger#request-body).

#### <a name="example-request"></a>Przykładowe żądanie

Następującą treść żądania określa właściwości wymagane w celu wyzwolenia przywracania dysku.

```json
{
  "properties": {
    "objectType": "IaasVMRestoreRequest",
    "recoveryPointId": "20982486783671",
    "recoveryType": "RestoreDisks",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testAccount",
    "region": "westus",
    "createNewCloudService": false,
    "originalStorageAccountOption": false,
    "encryptionDetails": {
      "encryptionEnabled": false
    }
  }
}
```

### <a name="response"></a>Odpowiedź

Wyzwalanie przywracania dysku jest [operację asynchroniczną](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Oznacza to, że ta operacja tworzy inną operację, która musi być śledzona oddzielnie.

Zwraca ona dwie odpowiedzi: 202 (zaakceptowano), gdy inna operacja zostanie utworzony, a następnie 200 (OK), po zakończeniu tej operacji.

|Name (Nazwa)  |Typ  |Opis  |
|---------|---------|---------|
|202 zaakceptowano     |         |     Zaakceptowane    |

#### <a name="example-responses"></a>Przykładowe odpowiedzi

Po przesłaniu *WPIS* identyfikatora URI do wyzwalania przywracania dysków, wstępnej reakcji to 202 (zaakceptowano) za pomocą nagłówek lokalizacji lub Azure async nagłówka.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2016-12-01
X-Content-Type-Options: nosniff
x-ms-request-id: 893fe372-8d6c-4c56-b589-45a95eeef95f
x-ms-client-request-id: a15ce064-25bd-4ac6-87e5-e3bc6ec65c0b; a15ce064-25bd-4ac6-87e5-e3bc6ec65c0b
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1198
x-ms-correlation-request-id: 893fe372-8d6c-4c56-b589-45a95eeef95f
x-ms-routing-request-id: SOUTHINDIA:20180604T130003Z:893fe372-8d6c-4c56-b589-45a95eeef95f
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 13:00:03 GMT
Location: https://management.azure.com/subscriptions//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2016-12-01
X-Powered-By: ASP.NET
```

Następnie śledzenia wynikowych operacji przy użyciu nagłówek lokalizacji lub nagłówek operację asynchroniczną na platformie Azure przy użyciu prostego *UZYSKAĆ* polecenia.

```http
GET https://management.azure.com/subscriptions//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2016-12-01
```

Po zakończeniu tej operacji zwraca 200 (OK) z Identyfikatorem zadania przywracania wynikowe w treści odpowiedzi.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: ea2a8011-eb83-4a4b-9ed2-e694070a966a
x-ms-client-request-id: a7f3a144-ed80-41ee-bffe-ae6a90c35a2f; a7f3a144-ed80-41ee-bffe-ae6a90c35a2f
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14973
x-ms-correlation-request-id: ea2a8011-eb83-4a4b-9ed2-e694070a966a
x-ms-routing-request-id: SOUTHINDIA:20180604T130917Z:ea2a8011-eb83-4a4b-9ed2-e694070a966a
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 13:09:17 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "781a0f18-e250-4d73-b059-5e9ffed4069e",
  "name": "781a0f18-e250-4d73-b059-5e9ffed4069e",
  "status": "Succeeded",
  "startTime": "2018-06-04T13:00:03.8068176Z",
  "endTime": "2018-06-04T13:00:03.8068176Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "3021262a-fb3a-4538-9b37-e3e97a386093"
  }
}
```

Ponieważ zadania tworzenia kopii zapasowej jest operacją wymagającą dużo czasu, powinny być śledzone, zgodnie z objaśnieniem w [monitorowania zadań za pomocą interfejsu API REST dokumentu](backup-azure-arm-userestapi-managejobs.md#tracking-the-job).

Po zakończeniu długotrwałe zadanie, dyski i konfigurację kopii zapasowej maszyny wirtualnej ("VMConfig.json") będzie znajdować się w danego konta magazynu.

## <a name="restore-as-another-virtual-machine"></a>Przywróć jako innej maszyny wirtualnej

[Wybierz punkt odzyskiwania](#select-recovery-point) i utworzyć treść żądania podaną poniżej, aby utworzyć innej maszyny wirtualnej platformy Azure przy użyciu danych z punktu odzyskiwania.

Następującą treść żądania określa właściwości wymagane w celu wyzwolenia przywracania maszyny wirtualnej.

```json
{
  "parameters": {
        "subscriptionId": "00000000-0000-0000-0000-000000000000",
        "resourceGroupName": "testVaultRG",
        "vaultName": "testVault",
        "fabricName": "Azure",
        "containerName": "IaasVMContainer;iaasvmcontainerv2;testRG;testVM",
        "protectedItemName": "VM;iaasvmcontainerv2;testRG;testVM",
        "recoveryPointId": "348916168024334",
        "api-version": "2016-12-01",
      "parameters": {
        "properties": {
          "objectType":  "IaasVMRestoreRequest",
          "recoveryPointId": "348916168024334",
          "recoveryType": "AlternateLocation",
          "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
          "targetVirtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Compute/virtualmachines/targetVM",
          "targetResourceGroupId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG",
          "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testingAccount",
          "virtualNetworkId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Network/virtualNetworks/testNet",
          "subnetId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Network/virtualNetworks/testNet/subnets/default",
          "region": "westus",
          "createNewCloudService": false,
          "originalStorageAccountOption": false,
          "encryptionDetails": {
            "encryptionEnabled": false
          }
        }
      }
    }
}
```

Odpowiedź powinna być obsługiwane w taki sam sposób jak [wyjaśniono powyżej w celu przywrócenia dysków](#response).

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat interfejsów API REST usługi Azure Backup zobacz następujące dokumenty:

- [Interfejs API REST platformy Azure w zakresie dostawcy usługi Recovery Services](/rest/api/recoveryservices/)
- [Wprowadzenie do interfejsu API REST platformy Azure](/rest/api/azure/)