---
title: Kopiowanie danych z oprogramowania Phoenix za pomocą usługi Azure Data Factory | Dokumentacja firmy Microsoft
description: Dowiedz się, jak skopiować dane z rozwiązania Phoenix do magazynów danych ujścia obsługiwane za pomocą działania kopiowania w potoku usługi Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: jingwang
ms.openlocfilehash: 012057c7d01924ab1998a010b6ea0c7d83651a4d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60405927"
---
# <a name="copy-data-from-phoenix-using-azure-data-factory"></a>Kopiowanie danych z oprogramowania Phoenix za pomocą usługi Azure Data Factory 

W tym artykule opisano sposób używania działania kopiowania w usłudze Azure Data Factory do kopiowania danych ze Phoenix. Opiera się na [omówienie działania kopiowania](copy-activity-overview.md) artykułu, który przedstawia ogólne omówienie działania kopiowania.

## <a name="supported-capabilities"></a>Obsługiwane funkcje

Możesz skopiować dane z rozwiązania Phoenix, do dowolnego obsługiwanego magazynu danych ujścia. Aby uzyskać listę magazynów danych, obsługiwane przez działanie kopiowania jako źródła/ujścia, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats) tabeli.

Usługa Azure Data Factory udostępnia wbudowanego sterownika, aby umożliwić łączność, dlatego nie trzeba ręcznie zainstalować dowolnego sterownika, za pomocą tego łącznika.

## <a name="getting-started"></a>Wprowadzenie

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Poniższe sekcje zawierają szczegółowe informacje dotyczące właściwości, które są używane do definiowania jednostek usługi Data Factory określonych Phoenix łącznikiem.

## <a name="linked-service-properties"></a>Właściwości usługi połączonej

Następujące właściwości są obsługiwane w przypadku Phoenix połączone usługi:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi być równa: **Phoenix** | Yes |
| host | Adres IP lub hosta nazwę serwera Phoenix. (czyli 192.168.222.160)  | Yes |
| port | Port TCP używany serwer Phoenix do nasłuchiwania połączeń klientów. Wartość domyślna to 8765. Jeśli łączysz się Azure HDInsights, należy określić port ustawiony na 443. | Nie |
| httpPath | Częściowe adres URL serwera Phoenix. (czyli /gateway/sandbox/phoenix/version). Określ `/hbasephoenix0` Jeśli używany klaster HDInsights.  | Nie |
| authenticationType | Mechanizm uwierzytelniania używany do łączenia się z serwerem Phoenix. <br/>Dozwolone wartości to: **Anonimowe**, **UsernameAndPassword**, **WindowsAzureHDInsightService** | Yes |
| username | Nazwa użytkownika używana do łączenia się z serwerem Phoenix.  | Nie |
| password | Hasło odpowiadający nazwie użytkownika. Oznacz to pole jako SecureString, aby bezpiecznie przechowywać w usłudze Data Factory lub [odwołanie wpisu tajnego przechowywanych w usłudze Azure Key Vault](store-credentials-in-key-vault.md). | Nie |
| enableSsl | Określa, czy połączenia z serwerem są szyfrowane przy użyciu protokołu SSL. Wartość domyślna to false.  | Nie |
| trustedCertPath | Pełna ścieżka pliku PEM, zawierająca zaufane certyfikaty urzędu certyfikacji w celu sprawdzenia serwer podczas nawiązywania połączenia za pośrednictwem protokołu SSL. Tę właściwość można ustawić tylko w przypadku korzystania z protokołu SSL na Self-Hosted IR Wartością domyślną jest instalowany z wewnątrz pliku cacerts.pem  | Nie |
| useSystemTrustStore | Określa, czy ma być używany certyfikat urzędu certyfikacji z magazynu zaufania systemu lub z określonego pliku PEM. Wartość domyślna to false.  | Nie |
| allowHostNameCNMismatch | Określa, czy wymagają nazwy certyfikatów wystawionych przez urząd certyfikacji SSL Period z nazwą hosta serwera podczas nawiązywania połączenia za pośrednictwem protokołu SSL. Wartość domyślna to false.  | Nie |
| allowSelfSignedServerCert | Określa, czy zezwalać na certyfikaty z podpisem własnym z serwera. Wartość domyślna to false.  | Nie |
| connectVia | [Środowiska Integration Runtime](concepts-integration-runtime.md) ma być używany do łączenia się z magazynem danych. Używając środowiskiem Integration Runtime lub Azure Integration Runtime (Jeśli magazyn danych jest publicznie dostępny). Jeśli nie zostanie określony, używa domyślnego środowiska Azure Integration Runtime. |Nie |

>[!NOTE]
>Jeśli klaster nie obsługuje trwałych sesji, np. HDInsight, należy jawnie dodać indeksu węzła na końcu ustawienia ścieżki http, np. Określ `/hbasephoenix0` zamiast `/hbasephoenix`.

**Przykład:**

```json
{
    "name": "PhoenixLinkedService",
    "properties": {
        "type": "Phoenix",
        "typeProperties": {
            "host" : "<cluster>.azurehdinsight.net",
            "port" : "443",
            "httpPath" : "/hbasephoenix0",
            "authenticationType" : "WindowsAzureHDInsightService",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Właściwości zestawu danych

Aby uzyskać pełną listę sekcje i właściwości dostępne Definiowanie zestawów danych, zobacz [zestawów danych](concepts-datasets-linked-services.md) artykułu. Ta sekcja zawiera listę właściwości obsługiwanych przez zestaw danych Phoenix.

Aby skopiować dane z rozwiązania Phoenix, należy ustawić właściwość typu zestawu danych na **PhoenixObject**. Obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość typu elementu dataset musi być równa: **PhoenixObject** | Yes |
| tableName | Nazwa tabeli. | Nie (Jeśli określono parametr "zapytanie" w źródle działania) |

**Przykład**

```json
{
    "name": "PhoenixDataset",
    "properties": {
        "type": "PhoenixObject",
        "linkedServiceName": {
            "referenceName": "<Phoenix linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Właściwości działania kopiowania

Aby uzyskać pełną listę sekcje i właściwości dostępne do definiowania działań zobacz [potoki](concepts-pipelines-activities.md) artykułu. Ta sekcja zawiera listę właściwości obsługiwanych przez źródło Phoenix.

### <a name="phoenix-as-source"></a>Phoenix jako źródło

Aby skopiować dane z rozwiązania Phoenix, należy ustawić typ źródła w działaniu kopiowania, aby **PhoenixSource**. Następujące właściwości są obsługiwane w działaniu kopiowania **źródła** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi być równa wartości właściwości type źródło działania kopiowania: **PhoenixSource** | Yes |
| query | Umożliwia odczytywanie danych niestandardowe zapytania SQL. Na przykład: `"SELECT * FROM MyTable"`. | Nie (Jeśli określono parametr "tableName" w zestawie danych) |

**Przykład:**

```json
"activities":[
    {
        "name": "CopyFromPhoenix",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Phoenix input dataset name>",
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
                "type": "PhoenixSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać listę magazynów danych obsługiwanych jako źródła i ujścia działania kopiowania w usłudze Azure Data Factory, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats).
