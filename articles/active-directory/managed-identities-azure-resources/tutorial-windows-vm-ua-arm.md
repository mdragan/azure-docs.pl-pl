---
title: Używanie przypisanej przez użytkownika tożsamości zarządzanej na maszynie wirtualnej z systemem Windows do uzyskiwania dostępu do usługi Azure Resource Manager
description: Samouczek przedstawiający proces użycia przypisanej przez użytkownika tożsamości zarządzanej na maszynie wirtualnej z systemem Windows do uzyskiwania dostępu do usługi Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6fc7e13cf5ea3991f81d53edcabe0980d799cb0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60443504"
---
# <a name="tutorial-use-a-user-assigned-managed-identity-on-a-windows-vm-to-access-azure-resource-manager"></a>Samouczek: używanie przypisanej przez użytkownika tożsamości zarządzanej na maszynie wirtualnej z systemem Windows do uzyskiwania dostępu do usługi Azure Resource Manager

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

W tym samouczku przedstawiono sposób tworzenia tożsamości przypisanej przez użytkownika, przypisywania jej do maszyny wirtualnej z systemem Windows, a następnie używania tej tożsamości w celu uzyskania dostępu do interfejsu API usługi Azure Resource Manager. Tożsamości usługi zarządzanej są zarządzane automatycznie przez platformę Azure. Umożliwiają one uwierzytelnianie do usług, które obsługują uwierzytelnianie usługi Azure AD bez konieczności osadzania poświadczeń w kodzie. 

Omawiane kwestie:

> [!div class="checklist"]
> * Tworzenie tożsamości zarządzanej przypisanej przez użytkownika
> * Przypisywanie tożsamości przypisanej przez użytkownika do maszyny wirtualnej z systemem Windows
> * Udzielanie tożsamości przypisanej przez użytkownika dostępu do grupy zasobów w usłudze Azure Resource Manager 
> * Uzyskiwanie tokenu dostępu przy użyciu tożsamości przypisanej przez użytkownika oraz używanie go do wywołania usługi Azure Resource Manager 
> * Odczytywanie właściwości grupy zasobów

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

- [Zalogowanie się w witrynie Azure Portal](https://portal.azure.com)

- [Utworzenie maszyny wirtualnej z systemem Windows](/azure/virtual-machines/windows/quick-create-portal)

- Aby przeprowadzić tworzenie wymaganych zasobów i wykonać kroki zarządzania rolami opisane w tym samouczku, Twoje konto musi mieć uprawnienia „Właściciel” w odpowiednim zakresie (subskrypcji lub grupy zasobów). Jeśli potrzebujesz pomocy dotyczącej przypisania roli, zobacz [Korzystanie z kontroli dostępu opartej na rolach do zarządzania dostępem do zasobów subskrypcji platformy Azure](/azure/role-based-access-control/role-assignments-portal).
- [Zainstaluj najnowszą wersję modułu programu Azure PowerShell](/powershell/azure/install-az-ps). 
- Uruchom polecenie `Connect-AzAccount`, aby utworzyć połączenia z platformą Azure.
- Zainstaluj [najnowszą wersję modułu PowerShellGet](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).
- Uruchom polecenie `Install-Module -Name PowerShellGet -AllowPrerelease`, aby pobrać wersję wstępną modułu `PowerShellGet` (po uruchomieniu tego polecenia może być konieczne uruchomienie polecenia `Exit` umożliwiającego zakończenie bieżącej sesji programu PowerShell w celu zainstalowania modułu `Az.ManagedServiceIdentity`).
- Uruchom polecenie `Install-Module -Name Az.ManagedServiceIdentity -AllowPrerelease`, aby zainstalować wersję wstępną modułu `Az.ManagedServiceIdentity`, który umożliwia wykonanie opisanych w tym artykule operacji na tożsamości przypisanej przez użytkownika.

## <a name="create-a-user-assigned-identity"></a>Tworzenie tożsamości przypisanej przez użytkownika

Tożsamość przypisana przez użytkownika jest tworzona jako autonomiczny zasób platformy Azure. Za pomocą polecenia [New-AzUserAssignedIdentity](/powershell/module/az.managedserviceidentity/get-azuserassignedidentity) platforma Azure tworzy tożsamość w dzierżawie usługi Azure AD, którą można przypisać do co najmniej jednego wystąpienia usługi platformy Azure.

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```azurepowershell-interactive
New-AzUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1
```

Odpowiedź zawiera szczegóły utworzonej tożsamości przypisanej przez użytkownika, podobne do poniższego przykładu. Zwróć uwagę na wartości `Id` i `ClientId` tożsamości przypisanej przez użytkownika, ponieważ są one używane w kolejnych krokach:

```azurepowershell
{
Id: /subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1
ResourceGroupName : myResourceGroupVM
Name: ID1
Location: westus
TenantId: 733a8f0e-ec41-4e69-8ad8-971fc4b533f8
PrincipalId: e591178e-b785-43c8-95d2-1397559b2fb9
ClientId: af825a31-b0e0-471f-baea-96de555632f9
ClientSecretUrl: https://control-westus.identity.azure.net/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1/credentials?tid=733a8f0e-ec41-4e69-8ad8-971fc4b533f8&oid=e591178e-b785-43c8-95d2-1397559b2fb9&aid=af825a31-b0e0-471f-baea-96de555632f9
Type: Microsoft.ManagedIdentity/userAssignedIdentities
}
```

## <a name="assign-the-user-assigned-identity-to-a-windows-vm"></a>Przypisywanie tożsamości przypisanej przez użytkownika do maszyny wirtualnej z systemem Windows

Tożsamość przypisana przez użytkownika może być używana przez klientów w obrębie wielu zasobów platformy Azure. Użyj poniższych poleceń, aby przypisać tożsamość przypisaną przez użytkownika do pojedynczej maszyny wirtualnej. Użyj właściwości `Id` zwróconej w poprzednim kroku dla parametru `-IdentityID`.

```azurepowershell-interactive
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzVM -ResourceGroupName TestRG -VM $vm -IdentityType "UserAssigned" -IdentityID "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
```

## <a name="grant-your-user-assigned-identity-access-to-a-resource-group-in-azure-resource-manager"></a>Udzielanie tożsamości przypisanej przez użytkownika dostępu do grupy zasobów w usłudze Azure Resource Manager 

Tożsamości zarządzane dla zasobów platformy Azure udostępniają tożsamości, których kod może używać do żądania tokenów dostępu w celu uwierzytelniania w interfejsach API zasobów obsługujących uwierzytelnianie usługi Azure AD. W tym samouczku kod będzie uzyskiwać dostęp do interfejsu API usługi Azure Resource Manager. 

Aby kod mógł uzyskać dostęp do interfejsu API, musisz udzielić dostępu tożsamości do zasobu w usłudze Azure Resource Manager. W tym przypadku jest to grupa zasobów, w której znajduje się maszyna wirtualna. Zaktualizuj wartość `<SUBSCRIPTION ID>` zgodnie z wymaganiami środowiska.

```azurepowershell-interactive
$spID = (Get-AzUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1).principalid
New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/"
```

Odpowiedź zawiera szczegóły utworzonego przypisania roli, podobne do poniższego przykładu:

```azurepowershell
RoleAssignmentId: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM/providers/Microsoft.Authorization/roleAssignments/f9cc753d-265e-4434-ae19-0c3e2ead62ac
Scope: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM
DisplayName: ID1
SignInName:
RoleDefinitionName: Reader
RoleDefinitionId: acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId: e591178e-b785-43c8-95d2-1397559b2fb9
ObjectType: ServicePrincipal
CanDelegate: False
```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Uzyskiwanie tokenu dostępu przy użyciu tożsamości maszyny wirtualnej oraz używanie go do wywołania usługi Resource Manager 

W pozostałej części tego samouczka będziemy pracować z poziomu wcześniej utworzonej maszyny wirtualnej.

1. Zaloguj się do witryny Azure Portal pod adresem [https://portal.azure.com](https://portal.azure.com)

2. W portalu przejdź do pozycji **Maszyny wirtualne**, a następnie przejdź do maszyny wirtualnej z systemem Windows i w pozycji **Przegląd** kliknij przycisk **Połącz**.

3. Wprowadź **nazwę użytkownika** i **hasło** użyte podczas tworzenia maszyny wirtualnej z systemem Windows.

4. Teraz, po utworzeniu **połączenia pulpitu zdalnego** z maszyną wirtualną, otwórz program **PowerShell** w sesji zdalnej.

5. Używając polecenia `Invoke-WebRequest` programu PowerShell, wyślij żądanie do lokalnego punktu końcowego tożsamości zarządzanych dla zasobów platformy Azure, aby uzyskać token dostępu na potrzeby usługi Azure Resource Manager.  `client_id` to wartość zwracana po [utworzeniu tożsamości zarządzanej przypisanej przez użytkownika](#create-a-user-assigned-identity).

    ```azurepowershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=af825a31-b0e0-471f-baea-96de555632f9&resource=https://management.azure.com/' -Method GET -Headers @{Metadata="true"}
    $content = $response.Content | ConvertFrom-Json
    $ArmToken = $content.access_token
    ```

## <a name="read-the-properties-of-a-resource-group"></a>Odczytywanie właściwości grupy zasobów

Użyj tokenu dostępu pobranego w poprzednim kroku, aby uzyskać dostęp do usługi Azure Resource Manager i odczytaj właściwości grupy zasobów, której udzielono dostępu tożsamości przypisanej przez użytkownika. Zastąp element `<SUBSCRIPTION ID>` identyfikatorem subskrypcji danego środowiska.

```azurepowershell
(Invoke-WebRequest -Uri https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/myResourceGroupVM?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{Authorization ="Bearer $ArmToken"}).content
```
Odpowiedź zawiera informacje o określonej grupie zasobów, podobne do poniższego przykładu:

```json
{"id":"/subscriptions/<SUBSCRIPTIONID>/resourceGroups/myResourceGroupVM","name":"myResourceGroupVM","location":"eastus","properties":{"provisioningState":"Succeeded"}}
```

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku przedstawiono sposób tworzenia tożsamości przypisanych przez użytkownika i dołączyć go na maszynie wirtualnej platformy Azure na dostęp do interfejsu API usługi Azure Resource Manager.  Aby dowiedzieć się więcej o usłudze Azure Resource Manager, zobacz:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)
