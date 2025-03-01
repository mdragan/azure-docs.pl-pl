---
title: Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu interfejsu wiersza polecenia platformy Azure
description: Dowiedz się, jak utworzyć konto usługi Azure Data Lake Analytics i przesyłanie zadania U-SQL przy użyciu interfejsu wiersza polecenia platformy Azure.
ms.service: data-lake-analytics
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 06/18/2017
ms.openlocfilehash: 9d9d5a7232529989901709013dcfac12f94afad0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60812763"
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli"></a>Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu interfejsu wiersza polecenia platformy Azure
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

W tym artykule opisano sposób używania interfejsu wiersza polecenia wiersza polecenia platformy Azure do tworzenia kont usługi Azure Data Lake Analytics, przesłać zadania skryptu u-SQL i katalogi. Zadanie odczytuje zawartość pliku z wartościami rozdzielanymi tabulatorami (TSV) i konwertuje je do pliku z wartościami rozdzielanymi przecinkami (CSV). 

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem musisz mieć następujące elementy:

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* Ten artykuł wymaga, że uruchomieniu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure]( /cli/azure/install-azure-cli). 



## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Aby zalogować się do subskrypcji platformy Azure:

```
azurecli
az login
```

Zostanie wyświetlona prośba o przejście do adresu URL i wpisanie kodu uwierzytelniania.  Następnie postępuj zgodnie z instrukcjami, aby wprowadzić swoje poświadczenia.

Po zalogowaniu polecenie logowania spowoduje wyświetlenie listy subskrypcji.

Aby użyć określonej subskrypcji:

```
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a>Tworzenie konta usługi Data Lake Analytics
Aby można było uruchomić jakiekolwiek zadanie, musisz mieć konto usługi Data Lake Analytics. Aby utworzyć takie konto, należy określić następujące elementy:

* **Grupa zasobów platformy Azure**. W grupie zasobów platformy Azure należy utworzyć konto usługi Data Lake Analytics. Usługa [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) umożliwia pracę z zasobami w aplikacji jak z grupą. Wszystkie zasoby aplikacji można wdrożyć, zaktualizować lub usunąć w jednej, skoordynowanej operacji.  

Aby wyświetlić listę istniejących grup zasobów w ramach subskrypcji:

```
az group list
```

Aby utworzyć nową grupę zasobów:

```
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* **Nazwa konta usługi Data Lake Analytics**. Każde konto usługi Data Lake Analytics ma nazwę.
* **Lokalizacja**. Użyj centrum danych platformy Azure, które obsługuje usługę Data Lake Analytics.
* **Domyślne Data Lake Store konta**: Każde konto usługi Data Lake Analytics ma domyślne konto Data Lake Store.

Aby wyświetlić istniejące konto usługi Data Lake Store:

```
az dls account list
```

Aby utworzyć nowe konto usługi Data Lake Store:

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

Użyj następującej składni, aby utworzyć konto usługi Data Lake Analytics:

```
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

Po utworzeniu konta można użyć następujących poleceń w celu wyświetlenia listy kont i szczegółów konta:

```
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-to-data-lake-store"></a>Przekazywanie danych do usługi Data Lake Store
W ramach tego samouczka przetworzysz wybrane dzienniki wyszukiwania.  Dziennik wyszukiwania może być przechowywany w usłudze Data Lake Store lub w usłudze Azure Blob Storage.

Witryna Azure Portal udostępnia interfejs użytkownika umożliwiający skopiowanie przykładowych plików danych na domyślne konto usługi Data Lake Store. Pliki te obejmują również dziennik wyszukiwania. Zobacz temat [Przygotowanie danych źródłowych](data-lake-analytics-get-started-portal.md), aby przekazać dane na domyślne konto usługi Data Lake Store.

Aby przekazać pliki przy użyciu wiersza polecenia platformy Azure, użyj następujących poleceń:

```
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

Usługa Data Lake Analytics może także uzyskiwać dostęp do usługi Azure Blob Storage.  Aby uzyskać informacje o przekazywaniu danych do usługi Azure Blob Storage, zobacz temat [Korzystanie z interfejsu wiersza polecenia platformy Azure w usłudze Azure Storage](../storage/common/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Przesyłanie zadań usługi Data Lake Analytics
Zadania usługi Data Lake Analytics są napisane w języku U-SQL. Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL](data-lake-analytics-u-sql-get-started.md) i [Dokumentację języka U-SQL](https://go.microsoft.com/fwlink/?LinkId=691348).

**Aby utworzyć skrypt zadania usługi Data Lake Analytics**

Utwórz plik tekstowy zawierający następujący skrypt w języku U-SQL i zapisz go na swojej stacji roboczej:

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

Ten skrypt U-SQL odczytuje źródłowy plik danych przy użyciu ekstraktora **Extractors.Tsv()** , a następnie tworzy plik csv przy użyciu ekstraktora **Outputters.Csv()** .

Nie należy modyfikować tych dwóch ścieżek, jeśli plik źródłowy nie został skopiowany do innej lokalizacji.  Jeśli folder wyjściowy nie istnieje, usługa Data Lake Analytics go utworzy.

Użycie ścieżek względnych jest łatwiejsze w przypadku plików przechowywanych na domyślnych kontach usługi Data Lake Store. Można także użyć ścieżek bezwzględnych.  Na przykład:

```
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

Aby uzyskać dostęp do plików w połączonych kontach magazynu, należy użyć ścieżek bezwzględnych.  Składnia dla plików przechowywanych na połączonym koncie usługi Azure Storage jest następująca:

```
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> Kontenery obiektów blob platformy Azure zawierające publiczne obiekty blob nie są obsługiwane.      
> Kontenery obiektów blob platformy Azure zawierające publiczne kontenery nie są obsługiwane.      
>

**Aby przesłać zadania**

Użyj następującej składni, aby przesłać zadanie.

```
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

Na przykład:

```
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

**Aby wyświetlić listę zadań i szczegóły zadania**

```
azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

**Aby anulować zadania**

```
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a>Pobieranie wyników zadania

Po zakończeniu zadania można użyć następujących poleceń, aby wyświetlić pliki wyjściowe i pobrać pliki:

```
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destination>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs download --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destination-path "<Destination Path and File Name>"
```

Na przykład:

```
az dls fs download --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destination-path "C:\DLA\myfile.csv"
```

## <a name="next-steps"></a>Kolejne kroki

* Aby wyświetlić dokument referencyjny dotyczący interfejsu wiersza polecenia programu Data Lake Analytics Azure, zobacz [usługi Data Lake Analytics](/cli/azure/dla).
* Aby wyświetlić dokument referencyjny dotyczący interfejsu wiersza polecenia programu Data Lake Store Azure, zobacz [Data Lake Store](/cli/azure/dls).
* Aby uzyskać informacje na temat bardziej złożonego zapytania, zobacz temat [Analizowanie dzienników witryn sieci Web przy użyciu usługi Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
