---
title: Uruchamiać polecenia wiersza polecenia platformy Azure lub programu PowerShell przy użyciu poświadczeń usługi Azure AD na dostęp do danych obiektów blob lub kolejki | Dokumentacja firmy Microsoft
description: Program PowerShell i interfejsu wiersza polecenia platformy Azure obsługuje logowanie się przy użyciu poświadczeń usługi Azure AD, aby uruchamiać polecenia danych obiektów blob i kolejek usługi Azure Storage. Token dostępu jest podana w sesji i autoryzowanie wywołania operacji. Uprawnienia zależą od ról RBAC, przypisany do podmiotu zabezpieczeń usługi Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/19/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 41ca1c5f413e5e15691f336d203edb918f21dc1a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65147295"
---
# <a name="run-azure-cli-or-powershell-commands-with-azure-ad-credentials-to-access-blob-or-queue-data"></a>Uruchamiać polecenia wiersza polecenia platformy Azure lub programu PowerShell przy użyciu poświadczeń usługi Azure AD na dostęp do danych obiektów blob i kolejki

Usługa Azure Storage udostępnia rozszerzenia dla wiersza polecenia platformy Azure i programu PowerShell, które umożliwiają użytkownikowi Zaloguj, a następnie uruchom polecenia skryptu przy użyciu poświadczeń usługi Azure Active Directory (Azure AD). Po zalogowaniu do wiersza polecenia platformy Azure lub programu PowerShell przy użyciu poświadczeń usługi Azure AD, zwracany jest token dostępu OAuth 2.0. Ten token jest automatycznie używany przez interfejs wiersza polecenia lub programu PowerShell do autoryzowania danych kolejnych operacji dotyczących magazynu obiektów Blob i kolejki. Dla obsługiwanych operacji nie trzeba przekazać klucz konta lub token sygnatury dostępu Współdzielonego za pomocą polecenia.

Możesz przypisywać uprawnienia do danych obiektów blob i kolejek do podmiotu zabezpieczeń usługi Azure AD za pomocą kontroli dostępu opartej na rolach (RBAC). Aby uzyskać więcej informacji na temat ról RBAC w usłudze Azure Storage, zobacz [Zarządzaj prawa dostępu do danych usługi Azure Storage za pomocą funkcji RBAC](storage-auth-aad-rbac.md).

## <a name="supported-operations"></a>Obsługiwane operacje

Rozszerzenia są obsługiwane operacje na kontenerach i kolejek. Jakie operacje może wywołać zależy od uprawnienia przyznane podmiotu zabezpieczeń usługi Azure AD za pomocą którego logowania się do wiersza polecenia platformy Azure lub programu PowerShell. Uprawnienia do kontenerów usługi Azure Storage lub kolejki są przypisywane przy użyciu kontroli dostępu opartej na rolach (RBAC). Na przykład, jeśli są przypisywane **czytnik danych obiektu Blob** roli, należy uruchomić poleceń skryptu, które odczytują dane z kontenera lub kolejki. Jeśli użytkownik został przypisany **Współautor danych obiektu Blob** roli, możesz uruchamiać polecenia skryptów, odczytu, zapisu lub usuwania kontenera lub kolejki lub dane zawierają. 

Aby uzyskać szczegółowe informacje o uprawnieniach wymaganych dla każdej operacji magazynu platformy Azure, kontenera lub kolejki, zobacz [uprawnień do wywoływania operacji REST](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).  

## <a name="call-cli-commands-using-azure-ad-credentials"></a>Wywołanie polecenia interfejsu wiersza polecenia przy użyciu poświadczeń usługi Azure AD

Wiersza polecenia platformy Azure obsługuje `--auth-mode` parametr operacje na danych obiektów blob i kolejki:

- Ustaw `--auth-mode` parametr `login` zarejestrować się przy użyciu podmiotu zabezpieczeń usługi Azure AD.
- Ustaw `--auth-mode` parametr do starszego `key` znajdują się wartości, aby spróbować odpytać do klucza konta, gdy nie parametry uwierzytelniania dla konta. 

Poniższy przykład pokazuje, jak utworzyć kontener w nowe konto magazynu z wiersza polecenia platformy Azure przy użyciu poświadczeń usługi Azure AD. Pamiętaj, aby zastąpić wartości symboli zastępczych w nawiasy ostre własnymi wartościami: 

1. Upewnij się, że zainstalowano interfejs wiersza polecenia platformy Azure w wersji 2.0.46 lub nowszej. Uruchom `az --version` Aby sprawdzić zainstalowaną wersją.

1. Uruchom `az login` i Uwierzytelnij się w oknie przeglądarki: 

    ```azurecli
    az login
    ```
    
1. Określ żądaną subskrypcji. Utwórz grupę zasobów za pomocą polecenia [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create). Tworzenie konta magazynu w ramach tego zasobu grupy za pomocą [Tworzenie konta magazynu az](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create): 

    ```azurecli
    az account set --subscription <subscription-id>

    az group create \
        --name sample-resource-group-cli \
        --location eastus

    az storage account create \
        --name <storage-account> \
        --resource-group sample-resource-group-cli \
        --location eastus \
        --sku Standard_LRS \
        --encryption-services blob
    ```
    
1. Przed utworzeniem kontenera, Przypisz [Współautor danych obiektu Blob magazynu](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) roli do siebie. Nawet jeśli jesteś właścicielem konta, konieczne jest jawne uprawnienia do wykonywania operacji na danych na koncie magazynu. Aby uzyskać więcej informacji na temat przypisywania ról RBAC, zobacz [udzielić dostępu do obiektów blob i kolejek danych Azure przy użyciu funkcji RBAC w witrynie Azure portal](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > Przypisania ról RBAC może potrwać kilka minut na propagację.
    
1. Wywołaj [utworzyć kontenera magazynu az](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) polecenia `--auth-mode` parametr `login` do utworzenia kontenera przy użyciu poświadczeń usługi Azure AD:

    ```azurecli
    az storage container create \ 
        --account-name <storage-account> \ 
        --name sample-container \
        --auth-mode login
    ```

Zmienna środowiskowa skojarzone z `--auth-mode` parametr `AZURE_STORAGE_AUTH_MODE`. Można określić odpowiednią wartość w zmiennej środowiskowej, aby uniknąć go w tym w przypadku każdego wywołania operacji danych usługi Azure Storage.

## <a name="call-powershell-commands-using-azure-ad-credentials"></a>Wywołania poleceń programu PowerShell przy użyciu poświadczeń usługi Azure AD

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Za pomocą programu Azure PowerShell Zaloguj się i przeprowadzać kolejne operacje usługi Azure Storage przy użyciu poświadczeń usługi Azure AD, Utwórz kontekst magazynu, aby odwoływać się do konta magazynu z uwzględnieniem `-UseConnectedAccount` parametru.

Poniższy przykład przedstawia sposób tworzenia kontenera na nowym koncie magazynu za pomocą programu Azure PowerShell przy użyciu poświadczeń usługi Azure AD. Pamiętaj, aby zastąpić wartości symboli zastępczych w nawiasy ostre własnymi wartościami:

1. Zaloguj się do subskrypcji platformy Azure za pomocą `Connect-AzAccount` polecenia i postępuj zgodnie z wyświetlanymi na ekranie instrukcjami, aby wprowadzić swoje poświadczenia usługi Azure AD: 

    ```powershell
    Connect-AzAccount
    ```
    
1. Utwórz grupę zasobów platformy Azure przez wywołanie metody [New AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). 

    ```powershell
    $resourceGroup = "sample-resource-group-ps"
    $location = "eastus"
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

1. Tworzenie konta magazynu przez wywołanie metody [New AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount).

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
      -Name "<storage-account>" `
      -SkuName Standard_LRS `
      -Location $location `
    ```

1. Pobierz kontekst konta magazynu, który określa nowe konto magazynu, wywołując [New AzStorageContext](/powershell/module/az.storage/new-azstoragecontext). Działa na koncie magazynu, można odwoływać się do kontekstu, zamiast wielokrotnie przekazywać poświadczenia. Obejmują `-UseConnectedAccount` parametr wywołuj dowolne operacje następne dane przy użyciu poświadczeń usługi Azure AD:

    ```powershell
    $ctx = New-AzStorageContext -StorageAccountName "<storage-account>" -UseConnectedAccount
    ```

1. Przed utworzeniem kontenera, Przypisz [Współautor danych obiektu Blob magazynu](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) roli do siebie. Nawet jeśli jesteś właścicielem konta, konieczne jest jawne uprawnienia do wykonywania operacji na danych na koncie magazynu. Aby uzyskać więcej informacji na temat przypisywania ról RBAC, zobacz [udzielić dostępu do obiektów blob i kolejek danych Azure przy użyciu funkcji RBAC w witrynie Azure portal](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > Przypisania ról RBAC może potrwać kilka minut na propagację.

1. Utwórz kontener, wywołując [New AzStorageContainer](/powershell/module/az.storage/new-azstoragecontainer). Ponieważ to wywołanie używa kontekstu utworzony w poprzednich krokach, ten kontener jest tworzony przy użyciu poświadczeń usługi Azure AD. 

    ```powershell
    $containerName = "sample-container"
    New-AzStorageContainer -Name $containerName -Context $ctx
    ```

## <a name="next-steps"></a>Kolejne kroki

- Aby dowiedzieć się więcej na temat ról RBAC dla usługi Azure storage, zobacz [Zarządzaj praw dostępu do magazynu danych przy użyciu RBAC](storage-auth-aad-rbac.md).
- Aby dowiedzieć się więcej o korzystaniu z zarządzanych tożsamości dla zasobów platformy Azure z usługą Azure Storage, zobacz [uwierzytelniania dostępu do obiektów blob i kolejek usługi Azure Active Directory i zarządzanych tożsamości dla zasobów platformy Azure](storage-auth-aad-msi.md).
- Aby dowiedzieć się, jak autoryzować dostęp do kontenerów i kolejki ze w aplikacjach pamięci masowej, zobacz [Użyj usługi Azure AD z magazynu aplikacji](storage-auth-aad-app.md).
