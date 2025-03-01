---
title: Kopiowanie danych do i z usługi Azure SQL Data Warehouse przy użyciu usługi Azure Data Factory | Dokumentacja firmy Microsoft
description: Dowiedz się, jak skopiować dane z magazynów obsługiwanego źródła do usługi Azure SQL Data Warehouse lub SQL Data Warehouse sklepy obsługiwane ujście przy użyciu usługi fabryka danych.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: jingwang
ms.openlocfilehash: 68d2f126ee32f61d13d170712bf58581101036e8
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206071"
---
# <a name="copy-data-to-or-from-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Kopiuj dane do / z usługi Azure SQL Data Warehouse przy użyciu usługi Azure Data Factory 
> [!div class="op_single_selector" title1="Wybierz wersję usługi Data Factory, którego korzystasz:"]
> * [Version1](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [Bieżąca wersja](connector-azure-sql-data-warehouse.md)

W tym artykule opisano sposób kopiowania danych do i z usługi Azure SQL Data Warehouse. Aby dowiedzieć się więcej na temat usługi Azure Data Factory, przeczytaj [artykuł wprowadzający](introduction.md).

## <a name="supported-capabilities"></a>Obsługiwane funkcje

Ten łącznik obiektu Blob platformy Azure jest obsługiwane dla następujących działań:

- [Działanie kopiowania, które](copy-activity-overview.md) z [obsługiwane źródło/ujście macierzy](copy-activity-overview.md) tabeli
- [Mapowanie przepływu danych](concepts-data-flow-overview.md)
- [Działanie Lookup](control-flow-lookup-activity.md)
- [Działanie GetMetadata](control-flow-get-metadata-activity.md)

W szczególności ten łącznik usługi Azure SQL Data Warehouse obsługuje te funkcje:

- Kopiowanie danych przy użyciu uwierzytelniania SQL i uwierzytelnianie tokenu aplikacji usługi Azure Active Directory (Azure AD) przy użyciu tożsamości podmiotu zabezpieczeń lub zarządzanej usługi dla zasobów platformy Azure.
- Jako źródło pobierać dane przy użyciu zapytania SQL lub procedury składowanej.
- Jako obiekt sink ładowanie danych przy użyciu technologii PolyBase lub zbiorczego wstawiania. Firma Microsoft zaleca program PolyBase do podniesienia wydajności kopiowania.

> [!IMPORTANT]
> W przypadku kopiowania danych za pomocą usługi Azure Data Factory Integration Runtime, skonfiguruj [zapory serwera Azure SQL](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) tak, aby usług platformy Azure mają dostęp do serwera.
> W przypadku kopiowania danych przy użyciu własnego środowiska integration runtime, należy skonfigurować zaporę serwera Azure SQL, aby umożliwić odpowiedni zakres adresów IP. Ten zakres obejmuje adres IP komputera, który jest używany do łączenia z usługą Azure SQL Database.

## <a name="get-started"></a>Rozpoczęcie pracy

> [!TIP]
> Aby uzyskać najlepszą wydajność, ładowanie danych do usługi Azure SQL Data Warehouse przy użyciu technologii PolyBase. [Przy użyciu technologii PolyBase do ładowania danych do usługi Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) sekcja zawiera szczegółowe informacje. Aby uzyskać wskazówki z przypadkami użycia, zobacz [ładowanie 1 TB w usłudze Azure SQL Data Warehouse z mniej niż 15 minut przy użyciu usługi Azure Data Factory](load-azure-sql-data-warehouse.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Poniższe sekcje zawierają szczegółowe informacje o właściwościach, które definiują określonych jednostek usługi Data Factory do łącznika usługi Azure SQL Data Warehouse.

## <a name="linked-service-properties"></a>Właściwości usługi połączonej

Następujące właściwości są obsługiwane dla usługi Azure SQL Data Warehouse połączone:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi być równa **AzureSqlDW**. | Yes |
| Parametry połączenia | Podaj informacje wymagane do nawiązania wystąpienia usługi Azure SQL Data Warehouse dla **connectionString** właściwości. <br/>Oznacz to pole jako SecureString, aby bezpiecznie przechowywać w usłudze Data Factory. Możesz również umieścić klucz jednostki usługi/hasła w usłudze Azure Key Vault oraz czy jest ściągnięcia uwierzytelniania SQL `password` konfiguracji poza parametry połączenia. Zobacz przykład kodu JSON pod tabelą i [Store poświadczeń w usłudze Azure Key Vault](store-credentials-in-key-vault.md) artykułu z bardziej szczegółowymi informacjami. | Yes |
| servicePrincipalId | Określ identyfikator klienta aplikacji. | Tak, gdy używasz uwierzytelniania usługi Azure AD przy użyciu jednostki usługi. |
| servicePrincipalKey | Określ klucz aplikacji. Oznacz to pole jako SecureString, aby bezpiecznie przechowywać w usłudze Data Factory lub [odwołanie wpisu tajnego przechowywanych w usłudze Azure Key Vault](store-credentials-in-key-vault.md). | Tak, gdy używasz uwierzytelniania usługi Azure AD przy użyciu jednostki usługi. |
| tenant | Określ informacje dzierżawy (identyfikator nazwy lub dzierżawy domeny), w którym znajduje się aplikacja. Można je pobrać, ustawiając kursor myszy w prawym górnym rogu witryny Azure Portal. | Tak, gdy używasz uwierzytelniania usługi Azure AD przy użyciu jednostki usługi. |
| connectVia | [Środowiska integration runtime](concepts-integration-runtime.md) ma być używany do łączenia się z magazynem danych. (Jeśli Twój magazyn danych znajduje się w sieci prywatnej), można użyć środowiska Azure Integration Runtime lub własnego środowiska integration runtime. Jeśli nie zostanie określony, używa domyślnego środowiska Azure Integration Runtime. | Nie |

Różnymi typami uwierzytelniania można znaleźć w następnych sekcjach dotyczących wymagań wstępnych i przykłady kodu JSON odpowiednio:

- [Uwierzytelnianie SQL](#sql-authentication)
- Uwierzytelnianie usługi Azure AD aplikacji tokenu: [Jednostka usługi](#service-principal-authentication)
- Uwierzytelnianie usługi Azure AD aplikacji tokenu: [Tożsamości zarządzane dla zasobów platformy Azure](#managed-identity)

>[!TIP]
>Jeśli osiągnięty błąd z kodem jako "UserErrorFailedToConnectToSqlServer", a wiadomości, takich jak "limit sesji dla bazy danych jest XXX i został osiągnięty.", Dodaj `Pooling=false` parametry połączenia i spróbuj ponownie.

### <a name="sql-authentication"></a>Uwierzytelnianie SQL

#### <a name="linked-service-example-that-uses-sql-authentication"></a>Przykład połączonej usługi, który używa uwierzytelniania SQL

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Hasło w usłudze Azure Key Vault:**

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Uwierzytelnianie jednostki usługi

Aby użyć uwierzytelniania tokenu aplikacji usługi oparte na jednostce usługi Azure AD, wykonaj następujące kroki:

1. **[Tworzenie aplikacji usługi Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)**  w witrynie Azure portal. Zanotuj nazwę aplikacji i następujące wartości, które definiują połączonej usługi:

    - Identyfikator aplikacji
    - Klucz aplikacji
    - Identyfikator dzierżawy

2. **[Aprowizowanie administrator usługi Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  dla serwera Azure SQL w witrynie Azure portal, jeśli jeszcze tego nie zrobiłeś. Administrator usługi Azure AD może być użytkownika usługi Azure AD lub grupy usługi Azure AD. Przyznanie grupie z tożsamości zarządzanej rolę administratora, pomiń kroki 3 i 4. Administrator będą mieć pełny dostęp do bazy danych.

3. **[Tworzenie użytkowników zawartej bazy danych](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  dla jednostki usługi. Łączenie z magazynem danych, z lub do której należy skopiować dane za pomocą narzędzi, takich jak program SSMS, za pomocą tożsamości usługi Azure AD, który ma co najmniej uprawnienie ALTER ANY użytkownika. Uruchom polecenie języka T-SQL:
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **Przyznaj nazwy głównej usługi potrzebnych uprawnień** , jak zwykle dla użytkowników SQL lub inne osoby. Uruchom poniższy kod lub odwoływać się do więcej opcji [tutaj](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017). Jeśli chcesz załadować dane, Dowiedz się, przy użyciu technologii PolyBase [wymagane uprawnienie bazy danych](#required-database-permission).

    ```sql
    EXEC sp_addrolemember db_owner, [your application name];
    ```

5. **Konfigurowanie usługi Azure SQL Data Warehouse połączone** w usłudze Azure Data Factory.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Przykład połączonej usługi, który używa uwierzytelniania jednostki usługi

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            },
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Zarządzanych tożsamości do uwierzytelniania zasobów platformy Azure

Fabryka danych może być skojarzony z [tożsamości zarządzanej dla zasobów platformy Azure](data-factory-service-identity.md) reprezentujący określonego fabryki. Ta tożsamość zarządzaną służy do uwierzytelniania usługi Azure SQL Data Warehouse. Fabryka wyznaczonym mogą uzyskiwać dostęp do i kopiowania danych z lub do danych magazynu przy użyciu tej tożsamości.

Aby użyć uwierzytelniania tożsamości zarządzanej, wykonaj następujące kroki:

1. **[Aprowizowanie administrator usługi Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  dla serwera Azure SQL w witrynie Azure portal, jeśli jeszcze tego nie zrobiłeś. Administrator usługi Azure AD może być użytkownika usługi Azure AD lub grupy usługi Azure AD. Przyznanie grupie z tożsamości zarządzanej rolę administratora, pomiń kroki 3 i 4. Administrator będą mieć pełny dostęp do bazy danych.

2. **[Tworzenie użytkowników zawartej bazy danych](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  tożsamości zarządzanej usługi Data Factory. Łączenie z magazynem danych, z lub do której należy skopiować dane za pomocą narzędzi, takich jak program SSMS, za pomocą tożsamości usługi Azure AD, który ma co najmniej uprawnienie ALTER ANY użytkownika. Uruchom języka T-SQL. 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. **Udzielanie tożsamości zarządzanej usługi Data Factory wymaganych uprawnień** , jak zwykle dla użytkowników SQL i innym osobom. Uruchom poniższy kod lub odwoływać się do więcej opcji [tutaj](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017). Jeśli chcesz załadować dane, Dowiedz się, przy użyciu technologii PolyBase [wymagane uprawnienie bazy danych](#required-database-permission).

    ```sql
    EXEC sp_addrolemember db_owner, [your Data Factory name];
    ```

5. **Konfigurowanie usługi Azure SQL Data Warehouse połączone** w usłudze Azure Data Factory.

**Przykład:**

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Właściwości zestawu danych

Aby uzyskać pełną listę sekcje i właściwości dostępne Definiowanie zestawów danych, zobacz [zestawów danych](concepts-datasets-linked-services.md) artykułu. Ta sekcja zawiera listę właściwości obsługiwanych przez zestaw danych usługi Azure SQL Data Warehouse.

Aby skopiować dane z lub do usługi Azure SQL Data Warehouse, obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | **Typu** właściwości zestawu danych musi być równa **AzureSqlDWTable**. | Yes |
| tableName | Nazwa tabeli lub widoku w wystąpieniu usługi Azure SQL Data Warehouse, która połączona usługa przywołuje. | Brak źródła tak dla ujścia |

#### <a name="dataset-properties-example"></a>Przykład właściwości zestawu danych

```json
{
    "name": "AzureSQLDWDataset",
    "properties":
    {
        "type": "AzureSqlDWTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Data Warehouse linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Właściwości działania kopiowania

Aby uzyskać pełną listę sekcje i właściwości dostępne do definiowania działań zobacz [potoki](concepts-pipelines-activities.md) artykułu. Ta sekcja zawiera listę właściwości obsługiwanych przez usługi Azure SQL Data Warehouse źródła i ujścia.

### <a name="azure-sql-data-warehouse-as-the-source"></a>Usługa Azure SQL Data Warehouse jako źródło

Aby skopiować dane z usługi Azure SQL Data Warehouse, należy ustawić **typu** właściwość źródła działania kopiowania do **SqlDWSource**. Następujące właściwości są obsługiwane w działaniu kopiowania **źródła** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | **Typu** właściwość źródła działania kopiowania musi być równa **SqlDWSource**. | Yes |
| sqlReaderQuery | Umożliwia odczytywanie danych niestandardowe zapytania SQL. Przykład: `select * from MyTable`. | Nie |
| sqlReaderStoredProcedureName | Nazwa procedury składowanej, która odczytuje dane z tabeli źródłowej. Ostatnią instrukcję SQL musi być instrukcja SELECT w procedurze składowanej. | Nie |
| storedProcedureParameters | Parametry procedury składowanej.<br/>Dozwolone wartości to pary nazw ani wartości. Nazwy i wielkość liter w wyrazie parametry muszą być zgodne, nazwy i wielkość liter w wyrazie parametrów procedury składowanej. | Nie |

### <a name="points-to-note"></a>Uwagi na

- Jeśli **sqlReaderQuery** jest określona dla **SqlSource**, działanie kopiowania jest uruchamiane to zapytanie względem źródła usługi Azure SQL Data Warehouse, aby uzyskać dane. Lub można określić procedury przechowywanej. Określ **sqlReaderStoredProcedureName** i **storedProcedureParameters** Jeśli procedura składowana pobiera parametry.
- Jeśli nie podasz **sqlReaderQuery** lub **sqlReaderStoredProcedureName**, kolumn zdefiniowanych w **struktury** części zestawu danych JSON są używane do Utwórz zapytanie. `select column1, column2 from mytable` działa w odniesieniu do usługi Azure SQL Data Warehouse. Jeśli nie ma w definicji zestawu danych **struktury**, wybrano wszystkie kolumny z tabeli.

#### <a name="sql-query-example"></a>Przykład zapytania SQL

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlDWSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="stored-procedure-example"></a>Przykład procedury składowanej

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlDWSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="stored-procedure-definition"></a>Definicja procedury składowanej

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-data-warehouse-as-sink"></a> Usługa Azure SQL Data Warehouse jako ujście

Aby skopiować dane do usługi Azure SQL Data Warehouse, należy ustawić typ ujścia w działaniu kopiowania, aby **SqlDWSink**. Następujące właściwości są obsługiwane w działaniu kopiowania **ujścia** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | **Typu** właściwość ujścia działania kopiowania musi być równa **SqlDWSink**. | Yes |
| allowPolyBase | Wskazuje, czy przy użyciu technologii PolyBase, jeśli ma to zastosowanie, zamiast mechanizmu BULKINSERT. <br/><br/> Firma Microsoft zaleca, ładowanie danych do usługi SQL Data Warehouse przy użyciu programu PolyBase. Zobacz [przy użyciu technologii PolyBase do ładowania danych do usługi Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) sekcji ograniczeń i szczegółów.<br/><br/>Dozwolone wartości to **True** i **False** (ustawienie domyślne).  | Nie |
| polyBaseSettings | Grupa właściwości, które może być określony, gdy **allowPolybase** właściwość jest ustawiona na **true**. | Nie |
| rejectValue | Określa liczbę lub wartość procentowa wierszy, które można odrzucić przed zapytanie nie powiedzie się.<br/><br/>Dowiedz się więcej na temat opcji odrzucania w technologii PolyBase w sekcji argumenty [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx). <br/><br/>Dozwolone wartości to 0 (domyślnie), 1, 2, itp. |Nie |
| rejectType | Określa, czy **rejectValue** opcja jest wartością literałową lub wartości procentowej.<br/><br/>Dozwolone wartości to **wartość** (ustawienie domyślne) i **procent**. | Nie |
| rejectSampleValue | Określa liczbę wierszy do pobrania, zanim program PolyBase ponownie oblicza odsetek odrzuconych wierszy.<br/><br/>Dozwolone wartości to 1, 2, itp. | Tak, jeśli **rejectType** jest **procent**. |
| useTypeDefault | Określa sposób obsługi brakujących wartości w rozdzielanych plików tekstowych, jeśli funkcja PolyBase pobiera dane z pliku tekstowego.<br/><br/>Dowiedz się więcej na temat tej właściwości z sekcji argumentów w [tworzenie EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).<br/><br/>Dozwolone wartości to **True** i **False** (ustawienie domyślne).<br><br>**Zobacz [wskazówki dotyczące rozwiązywania problemów](#polybase-troubleshooting) związane z tego ustawienia.** | Nie |
| writeBatchSize | Liczba wierszy do wstawienia do tabeli SQL **na partię**. Ma zastosowanie tylko wtedy, gdy PolyBase nie jest używany.<br/><br/>Dozwolone wartości to **całkowitą** (liczba wierszy). Domyślnie Data Factory dynamiczne określanie rozmiar partii odpowiednie, w zależności od rozmiaru wiersza. | Nie |
| writeBatchTimeout | Czas na zakończenie przed upływem limitu czasu operacji wstawiania wsadowego oczekiwania. Ma zastosowanie tylko wtedy, gdy PolyBase nie jest używany.<br/><br/>Dozwolone wartości to **timespan**. Przykład: "00: 30:00" (30 minut). | Nie |
| preCopyScript | Określ zapytanie SQL, działanie kopiowania do uruchomienia przed zapisaniem danych do usługi Azure SQL Data Warehouse w każdym przebiegu. Ta właściwość służy do oczyszczania załadowanych danych. | Nie |

#### <a name="sql-data-warehouse-sink-example"></a>Przykład obiektu sink SQL Data Warehouse

```json
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

Dowiedz się więcej o tym, jak można efektywnie obciążenia usługa SQL Data Warehouse w następnej sekcji przy użyciu technologii PolyBase.

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Ładowanie danych do usługi Azure SQL Data Warehouse przy użyciu technologii PolyBase

Za pomocą [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) to wydajny sposób ładowania dużych ilości danych do usługi Azure SQL Data Warehouse o wysokiej przepływności. Zobaczysz duże korzyści przepływności przy użyciu programu PolyBase zamiast domyślnego mechanizmu BULKINSERT. Zobacz [dotyczące wydajności](copy-activity-performance.md#performance-reference) szczegółowe porównanie. Aby uzyskać wskazówki z przypadkami użycia, zobacz [ładowanie 1 TB w usłudze Azure SQL Data Warehouse](v1/data-factory-load-sql-data-warehouse.md).

* Jeśli źródło danych znajduje się w **obiektów Blob platformy Azure, Azure Data Lake Storage Gen1 lub Azure Data Lake Storage Gen2**i **format jest PolyBase zgodne**, działanie kopiowania umożliwia bezpośrednio wywołać program PolyBase, aby umożliwić platformie Azure Usługa SQL Data Warehouse ściągania danych ze źródła. Aby uzyskać więcej informacji, zobacz  **[bezpośrednie kopiowania przy użyciu programu PolyBase](#direct-copy-by-using-polybase)** .
* Jeśli Twoje źródłowy magazyn danych i format pierwotnie nie jest obsługiwana przez program PolyBase, użyj **[kopiowania etapowego za pomocą programu PolyBase](#staged-copy-by-using-polybase)** są wyposażone w zamian. Funkcja kopiowania przejściowego zapewnia także większą przepływność. Automatycznie konwertuje dane w formacie zgodnym z programu PolyBase. I przechowuje dane w usłudze Azure Blob storage. Następnie ładuje dane do usługi SQL Data Warehouse.

>[!TIP]
>Dowiedz się więcej o [najlepsze rozwiązania dotyczące przy użyciu technologii PolyBase](#best-practices-for-using-polybase).

### <a name="direct-copy-by-using-polybase"></a>Bezpośrednie kopiowania przy użyciu programu PolyBase

Program PolyBase magazynu danych SQL obsługuje bezpośrednio obiektów Blob platformy Azure, Azure Data Lake Storage Gen1 i Azure Data Lake Storage Gen2. Jeśli źródło danych spełnia kryteria opisane w tej sekcji, skopiuj bezpośrednio z magazynu danych źródłowych do usługi Azure SQL Data Warehouse przy użyciu technologii PolyBase. W przeciwnym razie użyj [kopiowania etapowego za pomocą programu PolyBase](#staged-copy-by-using-polybase).

> [!TIP]
> Aby efektywnie kopiowania danych do usługi SQL Data Warehouse, Dowiedz się więcej z [usługi Azure Data Factory umożliwia jeszcze łatwiejsze i wygodne uzyskiwanie szczegółowych informacji z danych, korzystając z programu Data Lake Store z usługą SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Jeśli nie są spełnione wymagania, usługi Azure Data Factory umożliwia sprawdzenie ustawień i automatycznie powraca do przenoszenia danych przy użyciu mechanizmu BULKINSERT.

1. **Źródła połączoną usługę** z następujących typów i metod uwierzytelniania:

    | Typ magazynu danych obsługiwanego źródła | Obsługiwany typ uwierzytelniania źródła |
    |:--- |:--- |
    | [Azure Blob](connector-azure-blob-storage.md) | Konto uwierzytelniania za pomocą klucza uwierzytelniania tożsamości zarządzanej |
    | [Usługa Azure Data Lake Storage 1. generacji](connector-azure-data-lake-store.md) | Uwierzytelnianie jednostki usługi |
    | [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md) | Konto uwierzytelniania za pomocą klucza uwierzytelniania tożsamości zarządzanej |

    >[!IMPORTANT]
    >Azure Storage jest skonfigurowany z punktu końcowego usługi sieci wirtualnej, należy użyć uwierzytelniania tożsamości zarządzanej - dotyczą [wpływ za pomocą punktów końcowych usługi sieci wirtualnej z usługą Azure storage](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage). Dowiedz się, wymagane konfiguracje w usłudze Data Factory z [obiektów Blob platformy Azure — uwierzytelnianie tożsamości zarządzanej](connector-azure-blob-storage.md#managed-identity) i [Gen2 magazynu Azure Data Lake — uwierzytelniania tożsamości zarządzanej](connector-azure-data-lake-storage.md#managed-identity) sekcji odpowiednio.

2. **Formatu danych źródłowych** jest **Parquet**, **ORC**, lub **tekst rozdzielany**, w następujący sposób:

   1. Ścieżka folderu nie zawierają filtr z symbolami wieloznacznymi.
   2. Nazwa pliku wskazuje na pojedynczy plik lub jest `*` lub `*.*`.
   3. `rowDelimiter` musi być **\n**.
   4. `nullValue` albo ustawiono **pusty ciąg** ("") lub w lewo jako domyślny, i `treatEmptyAsNull` jest pozostanie domyślnie lub wartość true.
   5. `encodingName` ustawiono **utf-8**, która jest wartością domyślną.
   6. `quoteChar`, `escapeChar`, i `skipLineCount` nie są określone. Obsługa technologii PolyBase, Pomiń wiersz nagłówka, w którym można skonfigurować jako `firstRowAsHeader` w usłudze ADF.
   7. `compression` może być **bez kompresji**, **GZip**, lub **Deflate**.

```json
"activities":[
    {
        "name": "CopyFromAzureBlobToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "BlobDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            }
        }
    }
]
```

### <a name="staged-copy-by-using-polybase"></a>Kopiowania przejściowego przy użyciu programu PolyBase

Jeśli źródło danych nie spełnia kryteriów w poprzedniej sekcji, włączać dane kopiowanie za pośrednictwem tymczasowego przemieszczania wystąpienia magazynu obiektów Blob platformy Azure. Nie można go z usługi Azure Premium Storage. W tym przypadku usługi Azure Data Factory automatycznie uruchamia przekształcenia na danych, które spełniają wymagania dotyczące formatu danych PolyBase. Następnie używa programu PolyBase do ładowania danych do usługi SQL Data Warehouse. Na koniec go czyści dane tymczasowe z magazynu obiektów blob. Zobacz [kopiowania etapowego](copy-activity-performance.md#staged-copy) szczegółowe informacje na temat kopiowania danych za pośrednictwem przemieszczania wystąpienia magazynu obiektów Blob platformy Azure.

Aby użyć tej funkcji, należy utworzyć [połączonej usługi Azure Storage](connector-azure-blob-storage.md#linked-service-properties) odwołujący się do konta usługi Azure storage za pomocą magazynu obiektów blob przejściowym. Następnie określ `enableStaging` i `stagingSettings` właściwości dla działania kopiowania, jak pokazano w poniższym kodzie:

```json
"activities":[
    {
        "name": "CopyFromSQLServerToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "SQLServerDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
]
```

## <a name="best-practices-for-using-polybase"></a>Najlepsze rozwiązania dotyczące przy użyciu technologii PolyBase

W poniższych sekcjach przedstawiono najlepsze rozwiązania, oprócz tych wymienionych w [najlepsze rozwiązania dotyczące usługi Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Uprawnienia wymagane bazy danych

Aby korzystać z technologii PolyBase, musi mieć użytkownik, który ładuje dane do usługi SQL Data Warehouse [uprawnienie "CONTROL"](https://msdn.microsoft.com/library/ms191291.aspx) do docelowej bazy danych. Jest jednym ze sposobów, aby to osiągnąć, aby dodać użytkownika jako członka **db_owner** roli. Dowiedz się, jak to zrobić w [omówienie SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limits"></a>Rozmiar wiersza i danych typ ograniczenia

Obciążenia funkcji PolyBase są ograniczone do wierszy jest mniejszy niż 1 MB. Nie można załadować VARCHR(MAX), NVARCHAR(MAX) lub VARBINARY(MAX). Aby uzyskać więcej informacji, zobacz [limitów pojemności usługi SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Źródło danych zawiera wiersze przekracza 1 MB, można w pionie z kilku małych sieci podzielić tabel źródłowych. Upewnij się, że największy rozmiar dla każdego wiersza nie przekracza limit. Mniejsze tabel można następnie ładowane przy użyciu programu PolyBase i scalane w usłudze Azure SQL Data Warehouse.

Alternatywnie w danych za pomocą takich szerokości kolumn, ładowanie danych za pomocą usługi ADF, wyłączając "Zezwalaj na aparat PolyBase" można użyć innych technologii PolyBase ustawienie.

### <a name="polybase-troubleshooting"></a>Rozwiązywanie problemów z technologii PolyBase

**Ładowanie do dziesiętną kolumny**

Jeśli źródło danych jest w tekście, format lub inne zgodne innych technologii PolyBase przechowuje (przy użyciu kopiowania przejściowego i programu PolyBase) i zawiera pustą wartość, należy załadować do kolumny SQL Data Warehouse dziesiętną, może wystąpić następujący błąd:

```
ErrorCode=FailedDbOperation, ......HadoopSqlException: Error converting data type VARCHAR to DECIMAL.....Detailed Message=Empty string can't be converted to DECIMAL.....
```

Rozwiązaniem jest zaznaczenie "**domyślny typ użycia**" opcji ujścia działania kopiowania (jako FAŁSZ) -> Ustawienia programu PolyBase. "[USE_TYPE_DEFAULT](https://docs.microsoft.com/sql/t-sql/statements/create-external-file-format-transact-sql?view=azure-sqldw-latest#arguments
)" jest Konfiguracja natywnych PolyBase, który określa sposób obsługi brakujących wartości w rozdzielanych plików tekstowych, jeśli funkcja PolyBase pobiera dane z pliku tekstowego. 

**Inne**

Więcej technologii PolyBase problemów, zapoznaj się [obciążenie Rozwiązywanie problemów z PolyBase magazynu danych SQL Azure](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md#polybase).

### <a name="sql-data-warehouse-resource-class"></a>Klasa zasobów SQL Data Warehouse

Aby osiągnąć najlepsze możliwe przepływność, należy przypisać do większej klasy zasobów do użytkownika, który ładuje dane do usługi SQL Data Warehouse za pomocą programu PolyBase.

### <a name="tablename-in-azure-sql-data-warehouse"></a>**Właściwość tableName** w usłudze Azure SQL Data Warehouse

W poniższej tabeli przedstawiono przykłady sposobu określania **tableName** właściwość w zestawie danych JSON. Pokazuje kilka kombinacji nazwy schematu i tabeli.

| Schemat bazy danych | Nazwa tabeli | **Właściwość tableName** właściwość JSON |
| --- | --- | --- |
| dbo | MyTable | MyTable lub dbo. MyTable lub [dbo]. [MyTable] |
| dbo1 | MyTable | dbo1. MyTable lub [dbo1]. [MyTable] |
| dbo | My.Table | [My.Table] lub [dbo].[My.Table] |
| dbo1 | My.Table | [dbo1]. [My.Table] |

Jeśli zostanie wyświetlony następujący błąd, problem może polegać na wartość określona dla **tableName** właściwości. Można znaleźć w poprzedniej tabeli prawidłowym sposobem, aby określić wartości dla **tableName** właściwość JSON.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Kolumny z wartościami domyślnymi

Obecnie funkcja PolyBase w usłudze Data Factory akceptuje tylko tę samą liczbę kolumn w tabeli docelowej. Przykładem jest tabela z czterech kolumn, w którym jeden z nich jest zdefiniowana z wartością domyślną. Dane wejściowe nadal musi mieć czterech kolumn. Wejściowy zestaw danych trzy kolumny daje błąd podobny do następującego:

```
All columns of the table must be specified in the INSERT BULK statement.
```

Wartość NULL jest specjalną forma wartości domyślnej. Jeśli kolumna ma wartość null, dane wejściowe w obiekcie blob dla tej kolumny może być pusta. Ale nie może być Brak wejściowego zestawu danych. Program PolyBase wstawia o wartości NULL dla brakujących wartości w usłudze Azure SQL Data Warehouse.

## <a name="mapping-data-flow-properties"></a>Mapowanie właściwości z przepływu danych

Dowiedz się, szczegółowe informacje z [źródła przekształcenia](data-flow-source.md) i [ujścia przekształcania](data-flow-sink.md) w mapowanie przepływu danych.

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>Mapowanie typu danych dla usługi Azure SQL Data Warehouse

Podczas kopiowania danych z lub do usługi Azure SQL Data Warehouse, następujące mapowania są używane do typów danych tymczasowych usługi Azure Data Factory z typów danych Azure SQL Data Warehouse. Zobacz [schemat i dane mapowanie typu](copy-activity-schema-and-type-mapping.md) Aby dowiedzieć się, jak działania kopiowania mapuje typ schematu i danych źródła do ujścia.

>[!TIP]
>Zapoznaj się [typy danych w usłudze Azure SQL Data Warehouse w tabelach](../sql-data-warehouse/sql-data-warehouse-tables-data-types.md) artykuł dotyczący usługi SQL DW obsługiwane typy danych i rozwiązań dla te nieobsługiwane.

| Typ danych w usłudze Azure SQL Data Warehouse | Typ danych tymczasowych fabryki danych |
|:--- |:--- |
| bigint | Int64 |
| binary | Byte[] |
| bit | Boolean |
| char | String, Char[] |
| date | DateTime |
| Datetime | DateTime |
| datetime2 | DateTime |
| Datetimeoffset | DateTimeOffset |
| Decimal | Decimal |
| FILESTREAM attribute (varbinary(max)) | Byte[] |
| Float | Double |
| image | Byte[] |
| int | Int32 |
| money | Decimal |
| nchar | String, Char[] |
| numeric | Decimal |
| nvarchar | String, Char[] |
| real | Single |
| rowversion | Byte[] |
| smalldatetime | DateTime |
| smallint | Int16 |
| smallmoney | Decimal |
| time | TimeSpan |
| tinyint | Byte |
| uniqueidentifier | Guid |
| varbinary | Byte[] |
| varchar | String, Char[] |

## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać listę magazynów danych obsługiwanych jako źródła i ujścia przez działanie kopiowania w usłudze Azure Data Factory, zobacz [obsługiwane magazyny danych i formatów](copy-activity-overview.md##supported-data-stores-and-formats).
