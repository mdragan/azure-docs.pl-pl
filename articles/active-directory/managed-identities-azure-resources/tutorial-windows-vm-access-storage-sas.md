---
title: Umożliwia dostęp do usługi Azure Storage przy użyciu poświadczeń sygnatury dostępu Współdzielonego tożsamości zarządzanej przypisana przez system Windows VM
description: Samouczek, w którym pokazano, jak za pomocą maszyny Wirtualnej Windows przypisany systemowo zarządzanych tożsamości w celu dostępu do usługi Azure Storage przy użyciu poświadczeń sygnatury dostępu Współdzielonego zamiast klucza dostępu do konta magazynu.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/24/2019
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 23ec4d2a67beb9b5f903aa0b7f03196b47db3f78
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66226460"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-storage-via-a-sas-credential"></a>Samouczek: Umożliwia dostęp do usługi Azure Storage za pomocą poświadczeń sygnatury dostępu Współdzielonego tożsamości zarządzanej przypisana przez system Windows VM

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

W tym samouczku dowiesz się, jak używać tożsamości przypisanych przez system Windows maszyny wirtualnej (VM) do uzyskania poświadczeń sygnatury dostępu współdzielonego (SAS) magazynu. W szczególności [poświadczeń SAS usługi](/azure/storage/common/storage-dotnet-shared-access-signature-part-1?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Sygnatury dostępu Współdzielonego usługi umożliwia przyznawanie ograniczonego dostępu do obiektów na koncie magazynu przez ograniczony czas i określonej usługi (w naszym przypadku na usługę blob service), bez narażania klucz dostępu konta. Możesz użyć poświadczeń SAS w zwykły sposób wykorzystywany podczas wykonywania operacji magazynu, np. podczas używania zestawu SDK usługi Storage. W tym samouczku pokażemy, przekazywanie i pobieranie obiektu blob przy użyciu programu PowerShell magazynu platformy Azure. Poznasz następujące czynności:

> [!div class="checklist"]
> * Tworzenie konta magazynu
> * Udzielanie maszynie wirtualnej dostępu do sygnatury SAS konta magazynu w usłudze Resource Manager 
> * Uzyskiwanie tokenu dostępu przy użyciu tożsamości maszyny wirtualnej oraz używanie go do pobrania sygnatury SAS z usługi Resource Manager 

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="create-a-storage-account"></a>Tworzenie konta magazynu 

Jeśli jeszcze nie masz konta magazynu, teraz je utworzysz. Można również pominąć ten krok i udzielić dostępu przypisany systemowo tożsamości zarządzanej maszyny Wirtualnej do poświadczeń sygnatury dostępu Współdzielonego istniejącego konta magazynu. 

1. Kliknij przycisk **+/Utwórz nową usługę** znajdujący się w lewym górnym rogu witryny Azure Portal.
2. Kliknij opcję **Magazyn**, a następnie **Konto magazynu**. Zostanie wyświetlony nowy panel „Utwórz konto magazynu”.
3. Wprowadź nazwę konta magazynu, której będziesz używać później.  
4. Opcje **Model wdrażania** i **Rodzaj konta** należy ustawić na „Resource Manager” i „Ogólnego przeznaczenia”. 
5. Upewnij się, że **Subskrypcja** i **Grupa zasobów** pasują do wartości określonych podczas tworzenia maszyny wirtualnej w poprzednim kroku.
6. Kliknij pozycję **Utwórz**.

    ![Tworzenie nowego konta magazynu](./media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Tworzenie kontenera obiektów blob na koncie magazynu

Później przekażemy i pobierzemy plik do nowego konta magazynu. Ponieważ pliki wymagają magazynu obiektów blob, musimy utworzyć kontener obiektów blob, w którym będziemy przechowywać plik.

1. Przejdź z powrotem do nowo utworzonego konta magazynu.
2. Kliknij link **Kontenery** w panelu po lewej stronie, w sekcji „Usługa obiektów blob”.
3. Kliknij pozycję **+ Kontener** w górnej części strony, aby wysunąć panel „Nowy kontener”.
4. Podaj nazwę kontenera, wybierz poziom dostępu, a następnie kliknij przycisk **OK**. Podana nazwa będzie używana w dalszej części tego samouczka. 

    ![Tworzenie kontenera magazynu](./media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-system-assigned-managed-identity-access-to-use-a-storage-sas"></a>Udzielanie przypisanej przez system tożsamości zarządzanej maszyny wirtualnej dostępu do używania sygnatury SAS magazynu 

Usługa Azure Storage nie zapewnia natywnej obsługi uwierzytelniania usługi Azure AD.  Jednak można użyć tożsamości zarządzanej można pobrać sygnatury dostępu Współdzielonego magazynu usługi Resource Manager, a następnie dostępu do magazynu przy użyciu sygnatury dostępu Współdzielonego.  W tym kroku udzielasz przypisanej przez system tożsamości zarządzanej maszyny wirtualnej dostępu do sygnatury SAS konta magazynu.   

1. Przejdź z powrotem do nowo utworzonego konta magazynu.   
2. Kliknij link **Kontrola dostępu (IAM)** w panelu po lewej stronie.  
3. Kliknij przycisk **+ Dodaj przypisanie roli** w górnej części strony, aby dodać nowe przypisanie roli dla maszyny wirtualnej
4. Ustaw opcję **Rola** na „Współautor konta magazynu” w prawej części strony.  
5. Na kolejnej liście rozwijanej ustaw opcję **Przypisz dostęp do** na zasób „Maszyna wirtualna”.  
6. Następnie upewnij się, że odpowiednia subskrypcja znajduje się na liście rozwijanej **Subskrypcja**, po czym ustaw opcję **Grupa zasobów** na wartość „Wszystkie grupy zasobów”.  
7. Na koniec w pozycji **Wybierz** użyj listy rozwijanej, aby wybrać maszynę wirtualną z systemem Windows, i kliknij przycisk **Zapisz**. 

    ![Alternatywny tekst obrazu](./media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Uzyskiwanie tokenu dostępu przy użyciu tożsamości maszyny wirtualnej oraz używanie go do wywołania usługi Azure Resource Manager 

W pozostałej części tego samouczka będziemy pracować z poziomu wcześniej utworzonej maszyny wirtualnej.

W tej części będzie wymagane użycie poleceń cmdlet programu PowerShell w usłudze Azure Resource Manager.  Jeżeli nie jest on zainstalowany, [pobierz najnowszą wersję](https://docs.microsoft.com/powershell/azure/overview) przed kontynuowaniem.

1. W witrynie Azure Portal przejdź do pozycji **Maszyny wirtualne**, przejdź do maszyny wirtualnej z systemem Windows, a następnie na stronie **Przegląd** kliknij opcję **Połącz** u góry.
2. Wprowadź **nazwę użytkownika** i **hasło** dodane podczas tworzenia maszyny wirtualnej z systemem Windows. 
3. Teraz, po utworzeniu **połączenia pulpitu zdalnego** z maszyną wirtualną, otwórz program PowerShell w sesji zdalnej. 
4. Używając polecenia Invoke-WebRequest programu PowerShell, wyślij żądanie do lokalnego punktu końcowego tożsamości zarządzanej dla zasobów platformy Azure, aby uzyskać token dostępu na potrzeby usługi Azure Resource Manager.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > Wartość parametru „resource” musi być dokładnie dopasowana do tego, czego oczekujemy od usługi Azure AD. W przypadku użycia identyfikatora zasobu usługi Azure Resource Manager należy uwzględnić końcowy ukośnik w identyfikatorze URI.
    
    Następnie wyodrębnij element„Content”, który jest przechowywany w ciągu w formacie JavaScript Object Notation (JSON) w obiekcie $response. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Następnie wyodrębnij token dostępu z odpowiedzi.
    
    ```powershell
    $ArmToken = $content.access_token
    ```

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>Pobieranie poświadczeń SAS z usługi Azure Resource Manager w celu wykonywania wywołań do magazynu 

Teraz można użyć programu PowerShell do wywołania usługi Resource Manager przy użyciu tokenu dostępu, możemy pobrać w poprzedniej sekcji, aby utworzyć poświadczenia sygnatury dostępu Współdzielonego magazynu. Gdy będziemy już mieć poświadczenia sygnatury dostępu Współdzielonego, firma Microsoft może wywoływać operacje magazynu.

W przypadku tego żądania użyjemy następujących parametrów żądania HTTP, aby utworzyć poświadczenia SAS:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite. Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Te parametry są uwzględnione w treści żądania POST dla poświadczeń SAS. Aby uzyskać więcej informacji o parametrach potrzebnych do tworzenia poświadczeń SAS, zobacz [Dokumentację interfejsu REST sygnatury dostępu współdzielonego usługi listy](/rest/api/storagerp/storageaccounts/listservicesas).

Najpierw należy przekonwertować parametrów do formatu JSON, a następnie wywołaj magazynu `listServiceSas` poświadczeń punktu końcowego, aby utworzyć sygnaturę dostępu Współdzielonego:

```powershell
$params = @{canonicalizedResource="/blob/<STORAGE-ACCOUNT-NAME>/<CONTAINER-NAME>";signedResource="c";signedPermission="rcw";signedProtocol="https";signedExpiry="2017-09-23T00:00:00Z"}
$jsonParams = $params | ConvertTo-Json
```

```powershell
$sasResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT-NAME>/listServiceSas/?api-version=2017-06-01 -Method POST -Body $jsonParams -Headers @{Authorization="Bearer $ArmToken"}
```
> [!NOTE] 
> W adresie URL rozróżniana jest wielkość liter, więc upewnij się, że użyto takich samych wartości jak wcześniej, podczas nazywania grupy zasobów — z wielką literą „G” w nazwie „resourceGroups”. 

Teraz możemy wyodrębnić poświadczenia sygnatury dostępu Współdzielonego z odpowiedzi:

```powershell
$sasContent = $sasResponse.Content | ConvertFrom-Json
$sasCred = $sasContent.serviceSasToken
```

Zbadaj poświadczenie sygnatury dostępu Współdzielonego zostanie wyświetlony podobny do poniższego:

```powershell
PS C:\> $sasCred
sv=2015-04-05&sr=c&spr=https&se=2017-09-23T00%3A00%3A00Z&sp=rcw&sig=JVhIWG48nmxqhTIuN0uiFBppdzhwHdehdYan1W%2F4O0E%3D
```

Następnie utworzymy plik o nazwie „test.txt”. Następnie użyj poświadczeń sygnatury dostępu Współdzielonego do uwierzytelniania za pomocą `New-AzStorageContent` polecenia cmdlet, przekaż go do naszego kontenera obiektów blob, a następnie pobrać plik.

```bash
echo "This is a test text file." > test.txt
```

Pamiętaj, aby najpierw zainstalować polecenia cmdlet usługi Azure Storage przy użyciu polecenia `Install-Module Azure.Storage`. Następnie przekaż właśnie utworzony obiekt blob przy użyciu polecenia cmdlet `Set-AzStorageBlobContent` programu PowerShell:

```powershell
$ctx = New-AzStorageContext -StorageAccountName <STORAGE-ACCOUNT-NAME> -SasToken $sasCred
Set-AzStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Odpowiedź:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

Możesz też pobrać właśnie przekazany obiekt blob przy użyciu polecenia cmdlet `Get-AzStorageBlobContent` programu PowerShell:

```powershell
Get-AzStorageBlobContent -Blob testblob -Container <CONTAINER-NAME> -Destination test2.txt -Context $ctx
```

Odpowiedź:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku przedstawiono sposób użycia maszyn wirtualnych Windows przypisany systemowo tożsamości zarządzanej dostępu do magazynu platformy Azure przy użyciu poświadczeń sygnatury dostępu Współdzielonego.  Aby dowiedzieć się więcej o sygnaturze dostępu współdzielonego usługi Azure Storage, zobacz:

> [!div class="nextstepaction"]
>[Używanie sygnatury dostępu współdzielonego (SAS)](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)


