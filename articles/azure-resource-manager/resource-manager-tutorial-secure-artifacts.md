---
title: Zabezpieczanie artefaktów w przypadku wdrożeń szablonów usługi Azure Resource Manager | Microsoft Docs
description: Dowiedz się, jak zabezpieczać artefakty używane w szablonach usługi Azure Resource Manager.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 02/25/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: bf004f07558ae1f252a6bd26b4fd59ea9e4eea6e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069270"
---
# <a name="tutorial-secure-artifacts-in-azure-resource-manager-template-deployments"></a>Samouczek: Zabezpieczanie artefaktów w przypadku wdrożeń szablonów usługi Azure Resource Manager

Dowiedz się, jak zabezpieczać artefakty używane w szablonach usługi Azure Resource Manager za pomocą konta usługi Azure Storage przy użyciu sygnatur dostępu współdzielonego (SAS). Artefakty wdrożenia to dowolne pliki oprócz pliku szablonu głównego, które są potrzebne do ukończenia wdrożenia. Na przykład w artykule [Samouczek: Importowanie plików BACPAC bazy danych SQL za pomocą szablonów usługi Azure Resource Manager](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md) szablon główny tworzy bazę danych Azure SQL Database oraz wywołuje plik BACPAC w celu utworzenia tabel i wstawienia danych. Plik BACPAC jest tu artefaktem. Artefakt jest przechowywany na koncie usługi Azure Storage z dostępem publicznym. W tym samouczku użyjesz sygnatury dostępu współdzielonego w celu przyznania ograniczonego dostępu do pliku BACPAC na swoim koncie usługi Azure Storage. Aby uzyskać więcej informacji o sygnaturze dostępu współdzielonego, zobacz [Using Shared Access Signatures (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (Używanie sygnatur dostępu współdzielonego).

Aby dowiedzieć się, jak zabezpieczyć połączony szablon, zobacz [Samouczek: Tworzenie połączonych szablonów usługi Azure Resource Manager](./resource-manager-tutorial-create-linked-templates.md).

Ten samouczek obejmuje następujące zadania:

> [!div class="checklist"]
> * Przygotowywanie pliku BACPAC
> * Otwieranie istniejącego szablonu
> * Edytowanie szablonu
> * Wdrożenie szablonu
> * Weryfikowanie wdrożenia

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć pracę z tym artykułem, potrzebne są następujące zasoby:

* Program [Visual Studio Code](https://code.visualstudio.com/) z rozszerzeniem Resource Manager Tools. Zobacz [Instalowanie rozszerzenia](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Przejrzyj [Samouczek: Importowanie plików BACPAC bazy danych SQL za pomocą szablonów usługi Azure Resource Manager](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md). W tym samouczku został utworzony szablon używany w ramach bieżącego samouczka. W tym artykule udostępniono link do pobrania ukończonego szablonu.
* Aby zwiększyć bezpieczeństwo, użyj wygenerowanego hasła dla konta administratora programu SQL Server. Poniżej przedstawiono przykład służący do generowania hasła:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Usługa Azure Key Vault została zaprojektowana w celu ochrony kluczy kryptograficznych i innych wpisów tajnych. Aby uzyskać więcej informacji, zobacz [Samouczek: Integracja z usługą Azure Key Vault podczas wdrażania szablonu usługi Resource Manager](./resource-manager-tutorial-use-key-vault.md). Zalecamy również aktualizowanie hasła co trzy miesiące.

## <a name="prepare-a-bacpac-file"></a>Przygotowywanie pliku BACPAC

W tej sekcji przygotujesz plik BACPAC, aby można było bezpiecznie uzyskiwać do niego dostęp podczas wdrażania szablonu usługi Resource Manager. W tej sekcji opisano pięć procedur:

* Pobieranie pliku BACPAC.
* Tworzenie konta usługi Azure Storage.
* Tworzenie kontenera obiektów blob na koncie magazynu.
* Przekazywanie pliku BACPAC do kontenera.
* Pobieranie tokenu SAS pliku BACPAC.

Informacje umożliwiające zautomatyzowanie tych czynności przy użyciu skryptu programu PowerShell znajdują się w sekcji [Przekazywanie połączonego szablonu](./resource-manager-tutorial-create-linked-templates.md#upload-the-linked-template).

### <a name="download-the-bacpac-file"></a>Pobieranie pliku BACPAC

Pobierz [plik BACPAC](https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac) i zapisz go na lokalnym komputerze, pozostawiając nazwę **SQLDatabaseExtension.bacpac**.

### <a name="create-a-storage-account"></a>Tworzenie konta magazynu

1. Wybierz poniższy obraz, aby otworzyć szablon usługi Resource Manager w witrynie Azure Portal.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-storage-account-create%2fazuredeploy.json" target="_blank"><img src="./media/resource-manager-tutorial-secure-artifacts/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Wprowadź następujące właściwości:

    * **Subskrypcja**: Wybierz swoją subskrypcję platformy Azure.
    * **Grupa zasobów**: Wybierz pozycję **Utwórz nową** i nadaj nowej grupie nazwę. Grupa zasobów jest kontenerem zasobów platformy Azure używanym na potrzeby zarządzania. W tym samouczku możesz użyć tej samej grupy zasobów dla konta magazynu i bazy danych Azure SQL Database. Zanotuj nazwę tej grupy zasobów, ponieważ będzie ona później potrzebna podczas tworzenia bazy danych Azure SQL Database w następnych samouczkach.
    * **Lokalizacja**: Wybierz region. Na przykład **Środkowe stany USA**.
    * **Typ konta magazynu**: użyj wartości domyślnej — **Standard_LRS**.
    * **Lokalizacja**: Użyj wartości domyślnej — **[resourceGroup().location]** . Oznacza to, że używasz lokalizacji grupy zasobów dla konta magazynu.
    * **Wyrażam zgodę na powyższe warunki i postanowienia**: (zaznaczone)
3. Wybierz pozycję **Kup**.
4. Wybierz ikonę powiadomienia (ikonę dzwonka) w prawym górnym rogu portalu, aby wyświetlić stan wdrożenia.

    ![Okienko powiadomień portalu z samouczka dotyczącego usługi Resource Manager](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-portal-notifications-pane.png)
5. Po pomyślnym wdrożeniu konta magazynu wybierz pozycję **Przejdź do grupy zasobów** w okienku powiadomienia, aby otworzyć grupę zasobów.

### <a name="create-a-blob-container"></a>Tworzenie kontenera obiektów blob

Kontener obiektów blob jest wymagany, aby można było przekazywać pliki.

1. Wybierz konto magazynu, aby je otworzyć. W grupie zasobów zobaczysz tylko jedno konto magazynu. Nazwa Twojego konta magazynu będzie inna niż ta pokazana na poniższym zrzucie ekranu.

    ![Konto magazynu z samouczka dotyczącego usługi Resource Manager](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-storage-account.png)

2. Wybierz kafelek **Obiekty blob**.

    ![Kafelek Obiekty blob z samouczka dotyczącego usługi Resource Manager](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-blobs.png)
3. Wybierz pozycję **+ Kontener** w górnej części ekranu, aby utworzyć nowy kontener.
4. Wprowadź następujące wartości:

    * **Nazwa**: wprowadź **sqlbacpac**.
    * **Poziom dostępu publicznego**: użyj wartości domyślnej **Prywatny (bez dostępu anonimowego)** .
5. Kliknij przycisk **OK**.
6. Wybierz pozycję **sqlbacpac**, aby otworzyć nowo utworzony kontener.

### <a name="upload-the-bacpac-file-to-the-container"></a>Przekazywanie pliku BACPAC do kontenera

1. Wybierz pozycję **Przekaż**.
2. Wprowadź następujące wartości:

    * **Pliki**: Postępuj zgodnie z instrukcjami, aby wybrać pobrany wcześniej plik BACPAC. Jego nazwa domyślna to **SQLDatabaseExtension.bacpac**.
    * **Typ uwierzytelniania**: Wybierz pozycję **SAS**.  Wartość *SAS* jest wartością domyślną.
3. Wybierz pozycję **Przekaż**.  Po pomyślnym przekazaniu pliku jego nazwa będzie widoczna w kontenerze.

### <a name="a-namegenerate-a-sas-token-generate-a-sas-token"></a><a name="generate-a-sas-token" />Generowanie tokenu SAS

1. Kliknij prawym przyciskiem myszy plik **SQLDatabaseExtension.bacpac** w kontenerze, a następnie wybierz polecenie **Generuj sygnaturę dostępu współdzielonego**.
2. Wprowadź następujące wartości:

    * **Uprawnienie**: Użyj wartości domyślnej **Odczyt**.
    * **Data/godzina rozpoczęcia i utraty ważności**: Wartość domyślna umożliwia korzystanie z tokenu SAS przez osiem godzin. Jeśli potrzebujesz więcej czasu na ukończenie tego samouczka, zaktualizuj wartość **Data wygaśnięcia**.
    * **Dozwolone adresy IP**: Pozostaw to pole puste.
    * **Dozwolone protokoły**: użyj wartości domyślnej — **HTTPS**.
    * **Klucz podpisywania**: użyj wartości domyślnej — **Klucz 1**.
3. Wybierz pozycję **Generuj token i adres URL sygnatury dostępu współdzielonego obiektu blob**.
4. Utwórz kopię **adresu URL sygnatury dostępu współdzielonego obiektu blob**. W środku adresu URL jest nazwa pliku **SQLDatabaseExtension.bacpac**.  Ta nazwa pliku dzieli adres URL na trzy części:

   - **Lokalizacja artefaktu**: https://xxxxxxxxxxxxxx.blob.core.windows.net/sqlbacpac/. Upewnij się, że lokalizacja kończy się znakiem „/”.
   - **Nazwa pliku BACPAC**: SQLDatabaseExtension.bacpac.
   - **Token sygnatury dostępu współdzielonego lokalizacji artefaktu**: Upewnij się, że token jest poprzedzony znakiem „?”.

     Te trzy wartości będą Ci potrzebne w sekcji [Wdrażanie szablonu](#deploy-the-template).

## <a name="open-an-existing-template"></a>Otwieranie istniejącego szablonu

W tej sesji zmodyfikujesz szablon utworzony w artykule [Samouczek: Importowanie plików BACPAC bazy danych SQL za pomocą szablonów usługi Azure Resource Manager](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md) w celu wywołania pliku BACPAC przy użyciu tokenu SAS.  Szablon opracowany w samouczku dotyczącym rozszerzenia SQL jest udostępniany w pliku [ https://armtutorials.blob.core.windows.net/sqlextensionbacpac/azuredeploy.json ](https://armtutorials.blob.core.windows.net/sqlextensionbacpac/azuredeploy.json).

1. W programie Visual Studio Code wybierz pozycję **File (Plik)** >**Open File (Otwórz plik)** .
2. W polu **File name (Nazwa pliku)** wklej następujący adres URL:

    ```url
    https://armtutorials.blob.core.windows.net/sqlextensionbacpac/azuredeploy.json
    ```
3. Wybierz pozycję **Open (Otwórz)** , aby otworzyć plik.

    W tym szablonie zdefiniowano pięć zasobów:

   * `Microsoft.Sql/servers`. Zobacz [dokumentację szablonu](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers).
   * `Microsoft.SQL/servers/securityAlertPolicies`. Zobacz [dokumentację szablonu](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/securityalertpolicies).
   * `Microsoft.SQL/servers/filewallRules`. Zobacz [dokumentację szablonu](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers/firewallrules).
   * `Microsoft.SQL/servers/databases`.  Zobacz [dokumentację szablonu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).
   * `Microsoft.SQL/server/databases/extensions`.  Zobacz [dokumentację szablonu](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions).

     Warto uzyskać podstawową wiedzę na temat szablonu przed rozpoczęciem jego dostosowywania.
4. Wybierz pozycję **Plik**>**Zapisz jako**, aby zapisać kopię pliku o nazwie **azuredeploy.json** na komputerze lokalnym.

## <a name="edit-the-template"></a>Edytowanie szablonu

Dodaj następujące dodatkowe parametry:

```json
"_artifactsLocation": {
    "type": "string",
    "metadata": {
        "description": "The base URI where artifacts required by this template are located."
    }
},
"_artifactsLocationSasToken": {
    "type": "securestring",
    "metadata": {
        "description": "The sasToken required to access _artifactsLocation."
    },
    "defaultValue": ""
},
"bacpacFileName": {
    "type": "string",
    "defaultValue": "SQLDatabaseExtension.bacpac",
    "metadata": {
        "description": "The bacpac for configure the database and tables."
    }
}
```

![Parametry artefaktów zabezpieczeń z samouczka dotyczącego usługi Resource Manager](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-secure-artifacts-parameters.png)

Zaktualizuj wartości następujących dwóch elementów:

```json
"storageKey": "[parameters('_artifactsLocationSasToken')]",
"storageUri": "[uri(parameters('_artifactsLocation'), parameters('bacpacFileName'))]",
```

## <a name="deploy-the-template"></a>Wdrożenie szablonu

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Zapoznaj się sekcją [Wdrażanie szablonu](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template), gdzie znajduje się procedura wdrażania. Alternatywnie możesz też użyć następującego skryptu wdrażania programu PowerShell:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the virtual machine admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$artifactsLocation = Read-Host -Prompt "Enter the artifacts location"
$artifactsLocationSasToken = Read-Host -Prompt "Enter the artifacts location SAS token" -AsSecureString
$bacpacFileName = Read-Host -Prompt "Enter the BACPAC file name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -_artifactsLocation $artifactsLocation `
    -_artifactsLocationSasToken $artifactsLocationSasToken `
    -bacpacFileName $bacpacFileName `
    -TemplateFile "$HOME/azuredeploy.json"
```

Użyj wygenerowanego hasła. Zobacz [Wymagania wstępne](#prerequisites).
Aby uzyskać wartości elementów _artifactsLocation, _artifactsLocationSasToken i bacpacFileName, zobacz [Generowanie tokenu SAS](#generate-a-sas-token).

## <a name="verify-the-deployment"></a>Weryfikowanie wdrożenia

W portalu wybierz bazę danych SQL z nowo wdrożonej grupy zasobów. Wybierz pozycję **Edytor zapytań (wersja zapoznawcza)** , a następnie wprowadź poświadczenia administratora. Zobaczysz dwie tabele zaimportowane do bazy danych:

![Plik BACPAC umożliwiający wdrażanie rozszerzeń SQL usługi Azure Resource Manager](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy zasoby platformy Azure nie będą już potrzebne, wyczyść wdrożone zasoby, usuwając grupę zasobów.

1. W witrynie Azure Portal wybierz pozycję **Grupa zasobów** z menu po lewej stronie.
2. Wprowadź nazwę grupy zasobów w polu **Filtruj według nazwy**.
3. Wybierz nazwę grupy zasobów.  W grupie zasobów zostanie wyświetlonych łącznie sześć zasobów.
4. Wybierz pozycję **Usuń grupę zasobów** z górnego menu.

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku wdrożono program SQL Server i usługę SQL Database oraz zaimportowano plik BACPAC przy użyciu tokenu SAS. Aby dowiedzieć się, jak utworzyć potok usługi Azure stale tworzenie i wdrażanie szablonów usługi Resource Manager, zobacz

> [!div class="nextstepaction"]
> [Ciągła Integracja z usługą Azure potoku](./resource-manager-tutorial-use-azure-pipelines.md)
