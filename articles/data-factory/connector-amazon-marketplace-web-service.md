---
title: Kopiowanie danych z usługa internetowa witryny Amazon Marketplace przy użyciu usługi Azure Data Factory (wersja zapoznawcza) | Dokumentacja firmy Microsoft
description: Dowiedz się, jak skopiować dane z usługa internetowa witryny Amazon Marketplace do magazynów danych ujścia obsługiwane za pomocą działania kopiowania w potoku usługi Azure Data Factory.
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
ms.openlocfilehash: 45208b5c6538ea523a7b87d6dbdeb99e792783ff
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61262304"
---
# <a name="copy-data-from-amazon-marketplace-web-service-using-azure-data-factory-preview"></a>Kopiowanie danych z usługa internetowa witryny Amazon Marketplace przy użyciu usługi Azure Data Factory (wersja zapoznawcza)

W tym artykule opisano sposób używania działania kopiowania w usłudze Azure Data Factory do kopiowania danych ze usługa internetowa witryny Amazon Marketplace. Opiera się na [omówienie działania kopiowania](copy-activity-overview.md) artykułu, który przedstawia ogólne omówienie działania kopiowania.

> [!IMPORTANT]
> Ten łącznik jest obecnie w wersji zapoznawczej. Możesz wypróbować tę funkcję i przekaż nam swoją opinię. Jeśli w swoim rozwiązaniu chcesz wprowadzić zależność od łączników w wersji zapoznawczej, skontaktuj się z [pomocą techniczną platformy Azure](https://azure.microsoft.com/support/).

## <a name="supported-capabilities"></a>Obsługiwane funkcje

Możesz skopiować dane z usługa internetowa witryny Amazon Marketplace, do dowolnego obsługiwanego magazynu danych ujścia. Aby uzyskać listę magazynów danych, obsługiwane przez działanie kopiowania jako źródła/ujścia, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats) tabeli.

Usługa Azure Data Factory udostępnia wbudowanego sterownika, aby umożliwić łączność, dlatego nie trzeba ręcznie zainstalować dowolnego sterownika, za pomocą tego łącznika.

## <a name="getting-started"></a>Wprowadzenie

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Poniższe sekcje zawierają szczegółowe informacje dotyczące właściwości, które są używane do definiowania jednostek usługi fabryka danych określonej do łącznika usługa internetowa witryny Amazon Marketplace.

## <a name="linked-service-properties"></a>Właściwości usługi połączonej

Usługa internetowa witryny Amazon Marketplace połączone usługi są obsługiwane następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi być równa: **AmazonMWS** | Tak |
| endpoint | Punkt końcowy serwera Amazon MWS, (czyli mws.amazonservices.com)  | Yes |
| marketplaceID | Identyfikator witryny Marketplace Amazon, którego chcesz pobrać dane z. Do pobierania danych z wielu identyfikatorów portalu Marketplace, rozdziel je przecinkami (`,`). (czyli A2EUQ1WTGCTBG2)  | Yes |
| sellerID | Identyfikator Amazon sprzedawcy.  | Yes |
| mwsAuthToken | Token uwierzytelniania Amazon MWS. Oznacz to pole jako SecureString, aby bezpiecznie przechowywać w usłudze Data Factory lub [odwołanie wpisu tajnego przechowywanych w usłudze Azure Key Vault](store-credentials-in-key-vault.md). | Yes |
| accessKeyId | Dostęp do klucza identyfikator używany do dostępu do danych.  | Yes |
| secretKey | Klucz tajny, który umożliwia dostęp do danych. Oznacz to pole jako SecureString, aby bezpiecznie przechowywać w usłudze Data Factory lub [odwołanie wpisu tajnego przechowywanych w usłudze Azure Key Vault](store-credentials-in-key-vault.md). | Yes |
| useEncryptedEndpoints | Określa, czy punkty końcowe źródła danych są szyfrowane przy użyciu protokołu HTTPS. Wartość domyślna to true.  | Nie |
| useHostVerification | Określa, czy wymagają zgodności nazwy hosta w certyfikacie serwera, aby dopasować nazwę hosta serwera podczas nawiązywania połączenia za pośrednictwem protokołu SSL. Wartość domyślna to true.  | Nie |
| usePeerVerification | Określa, czy do zweryfikowania tożsamości serwera, podczas nawiązywania połączenia za pośrednictwem protokołu SSL. Wartość domyślna to true.  | Nie |

**Przykład:**

```json
{
    "name": "AmazonMWSLinkedService",
    "properties": {
        "type": "AmazonMWS",
        "typeProperties": {
            "endpoint" : "mws.amazonservices.com",
            "marketplaceID" : "A2EUQ1WTGCTBG2",
            "sellerID" : "<sellerID>",
            "mwsAuthToken": {
                 "type": "SecureString",
                 "value": "<mwsAuthToken>"
            },
            "accessKeyId" : "<accessKeyId>",
            "secretKey": {
                 "type": "SecureString",
                 "value": "<secretKey>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Właściwości zestawu danych

Aby uzyskać pełną listę sekcje i właściwości dostępne Definiowanie zestawów danych, zobacz [zestawów danych](concepts-datasets-linked-services.md) artykułu. Ta sekcja zawiera listę właściwości obsługiwanych przez zestaw danych usługa internetowa witryny Amazon Marketplace.

Aby skopiować dane z usługa internetowa witryny Amazon Marketplace, należy ustawić właściwość typu zestawu danych na **AmazonMWSObject**. Obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość typu elementu dataset musi być równa: **AmazonMWSObject** | Tak |
| tableName | Nazwa tabeli. | Nie (Jeśli określono parametr "zapytanie" w źródle działania) |

**Przykład**

```json
{
    "name": "AmazonMWSDataset",
    "properties": {
        "type": "AmazonMWSObject",
        "linkedServiceName": {
            "referenceName": "<AmazonMWS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}

```

## <a name="copy-activity-properties"></a>Właściwości działania kopiowania

Aby uzyskać pełną listę sekcje i właściwości dostępne do definiowania działań zobacz [potoki](concepts-pipelines-activities.md) artykułu. Ta sekcja zawiera listę właściwości obsługiwanych przez źródło usługa internetowa witryny Amazon Marketplace.

### <a name="amazon-mws-as-source"></a>Amazon MWS jako źródło

Aby skopiować dane z usługa internetowa witryny Amazon Marketplace, należy ustawić typ źródła w działaniu kopiowania, aby **AmazonMWSSource**. Następujące właściwości są obsługiwane w działaniu kopiowania **źródła** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi być równa wartości właściwości type źródło działania kopiowania: **AmazonMWSSource** | Tak |
| query | Umożliwia odczytywanie danych niestandardowe zapytania SQL. Na przykład: `"SELECT * FROM Orders where  Amazon_Order_Id = 'xx'"`. | Nie (Jeśli określono parametr "tableName" w zestawie danych) |

**Przykład:**

```json
"activities":[
    {
        "name": "CopyFromAmazonMWS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<AmazonMWS input dataset name>",
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
                "type": "AmazonMWSSource",
                "query": "SELECT * FROM Orders where Amazon_Order_Id = 'xx'"
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
