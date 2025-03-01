---
title: Tworzenie pierwszej funkcji w systemie Linux na platformie Azure
description: Dowiedz się, jak utworzyć pierwszą funkcję hostowaną w systemie Linux na platformie Azure przy użyciu narzędzi Azure Functions Core Tools i interfejsu wiersza polecenia platformy Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 03/12/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc, fasttrack-edit
ms.devlang: javascript
manager: jeconnoc
ms.openlocfilehash: f79fbc46104eb886a758802aff79a46feb4f5a3b
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64866629"
---
# <a name="create-your-first-function-hosted-on-linux-using-core-tools-and-the-azure-cli-preview"></a>Tworzenie pierwszej funkcji hostowanej w systemie Linux za pomocą narzędzi Core Tools i interfejsu wiersza polecenia platformy Azure (wersja zapoznawcza)

Usługa Azure Functions umożliwia wykonywanie kodu w środowisku [bezserwerowym](https://azure.com/serverless) systemu Linux bez konieczności uprzedniego tworzenia maszyny wirtualnej lub publikowania aplikacji internetowej. Hosting systemu Linux wymaga [funkcje w wersji 2.0 środowisko uruchomieniowe](functions-versions.md). Pomocy technicznej, aby uruchomić aplikację funkcji w systemie Linux w bez użycia serwera [planu zużycie](functions-scale.md#consumption-plan) jest obecnie dostępna w wersji zapoznawczej. Aby dowiedzieć się więcej, zobacz [w tym artykule uwagi dotyczące wersji zapoznawczej](https://aka.ms/funclinux).

W tym artykule Szybki start przedstawiono sposób użycia interfejsu wiersza polecenia platformy Azure w celu utworzenia pierwszej aplikacji funkcji działającej w systemie Linux. Kod funkcji jest tworzony lokalnie, a następnie wdrażany na platformie Azure za pomocą narzędzi [Azure Functions Core Tools](functions-run-local.md).

Poniższe kroki można wykonać na komputerze Mac, w systemie Windows lub w systemie Linux. W tym artykule opisano sposób tworzenia funkcji w języku JavaScript lub C#. Aby dowiedzieć się, jak tworzyć funkcje języka Python, zobacz [Create your first Python function using Core Tools and the Azure CLI (preview) (Tworzenie pierwszej funkcji języka Python za pomocą narzędzi Core Tools i interfejsu wiersza polecenia platformy Azure — wersja zapoznawcza)](functions-create-first-function-python.md).

## <a name="prerequisites"></a>Wymagania wstępne

Przed uruchomieniem tego przykładu należy dysponować następującymi elementami:

- Zainstaluj [podstawowych narzędzi usługi Azure Functions](./functions-run-local.md#v2) wersji 2.6.666 lub nowszej.

+ Zainstaluj [interfejs wiersza polecenia platformy Azure]( /cli/azure/install-azure-cli). Ten artykuł wymaga interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, z jakiej wersji korzystasz. Możesz również użyć usługi [Azure Cloud Shell](https://shell.azure.com/bash).

+ Aktywna subskrypcja platformy Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Tworzenie lokalnego projektu aplikacji funkcji

Uruchom następujące polecenie w wierszu polecenia, aby utworzyć projekt aplikacji funkcji w folderze `MyFunctionProj` bieżącego katalogu lokalnego. Ponadto w folderze `MyFunctionProj` jest tworzone repozytorium GitHub.

```bash
func init MyFunctionProj
```

Po wyświetleniu monitu użyj klawiszy strzałek, aby wybrać środowisko uruchomieniowe procesu roboczego z następujących opcji języka:

+ `dotnet`: tworzy projekt biblioteki klas platformy .NET (csproj).
+ `node`: tworzy projekt języka JavaScript lub TypeScript. Po wyświetleniu monitu wybierz `JavaScript`.
+ `python`: tworzy projekt w języku Python. Aby uzyskać informacje na temat języka Python, zobacz [przewodnik Szybki start dla języka Python](functions-create-first-function-python.md).

Gdy polecenie zostanie wykonane, zostaną zwrócone dane wyjściowe podobne do następujących:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

Użyj następującego polecenia, aby przejść do nowego folderu projektu `MyFunctionProj`.

```bash
cd MyFunctionProj
```

## <a name="reference-bindings"></a>Odwołanie do powiązania

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Tworzenie aplikacji funkcji systemu Linux na platformie Azure

Do obsługi wykonywania funkcji w systemie Linux potrzebna jest aplikacja funkcji. Aplikacja funkcji zapewnia bezserwerowe środowisko do wykonywania kodu funkcji. Umożliwia ona grupowanie funkcji w ramach jednostki logicznej, co ułatwia wdrażanie i udostępnianie zasobów oraz zarządzanie nimi. Utwórz aplikację funkcji działającą w systemie Linux przy użyciu polecenia [az functionapp create](/cli/azure/functionapp#az-functionapp-create).

W poniższym poleceniu zamiast symbolu zastępczego `<app_name>` użyj unikatowej nazwy aplikacji funkcji, a zamiast symbolu zastępczego `<storage_name>` użyj nazwy konta magazynu. `<app_name>` jest również domyślną domeną DNS aplikacji funkcji. Ta nazwa musi być unikatowa dla wszystkich aplikacji na platformie Azure. Należy również ustawić `<language>` środowisko uruchomieniowe dla aplikacji funkcji z `dotnet` (C#), `node` (języka JavaScript/TypeScript) lub `python`.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westus --os-type Linux \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

Po utworzeniu aplikacji funkcji zostanie wyświetlony następujący komunikat:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Teraz możesz opublikować swój projekt w nowej aplikacji funkcji na platformie Azure.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]