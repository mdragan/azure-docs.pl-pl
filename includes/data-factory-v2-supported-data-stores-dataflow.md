---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 05/24/2019
ms.author: jingwang
ms.openlocfilehash: 5054b552f4ca019125671903134bb080e0c9db4e
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67277397"
---
| Category | Magazyn danych | Obsługiwany jako [działanie kopiowania, które](../articles/data-factory/copy-activity-overview.md) źródła | Obsługiwany jako [działanie kopiowania, które](../articles/data-factory/copy-activity-overview.md) ujścia | Obsługiwane przez środowisko [Azure IR](../articles/data-factory/concepts-integration-runtime.md#azure-integration-runtime) | Obsługiwane przez [własne środowisko IR](../articles/data-factory/concepts-integration-runtime.md#self-hosted-integration-runtime) | Obsługiwane przez [przepływu danych](../articles/data-factory/concepts-data-flow-overview.md)
|:--- |:--- |:--- |:--- |:--- |:--- |
| **Azure** |[Azure Blob Storage](../articles/data-factory/connector-azure-blob-storage.md) |✓ |✓ |✓ |✓  | ✓ <br> <small>Obsługiwane formaty: Delimited Text, Parquet</small> |
| &nbsp; |[Azure Cosmos DB (interfejs API SQL)](../articles/data-factory/connector-azure-cosmos-db.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Interfejs API usługi Azure Cosmos DB, bazy danych mongodb](../articles/data-factory/connector-azure-cosmos-db-mongodb-api.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Azure Data Explorer](../articles/data-factory/connector-azure-data-explorer.md) |✓ |✓ |✓ |✓ ||
| &nbsp; |[Usługa Azure Data Lake Storage 1. generacji](../articles/data-factory/connector-azure-data-lake-store.md) |✓ |✓ |✓ |✓  |✓ <br> <small>Obsługiwane formaty: Delimited Text, Parquet</small> |
| &nbsp; |[Azure Data Lake Storage Gen2](../articles/data-factory/connector-azure-data-lake-storage.md) |✓ |✓ |✓ |✓  |✓ <br> <small>Obsługiwane formaty: Delimited Text, Parquet</small> |
| &nbsp; |[Azure Database for MariaDB](../articles/data-factory/connector-azure-database-for-mariadb.md) |✓ | |✓ |✓  ||
| &nbsp; |[Azure Database for MySQL](../articles/data-factory/connector-azure-database-for-mysql.md) |✓ | |✓ |✓  ||
| &nbsp; |[Azure Database for PostgreSQL](../articles/data-factory/connector-azure-database-for-postgresql.md) |✓ | |✓ |✓  ||
| &nbsp; |[Azure File Storage](../articles/data-factory/connector-azure-file-storage.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Azure SQL Database](../articles/data-factory/connector-azure-sql-database.md) |✓ |✓ |✓ |✓  |✓  |
| &nbsp; |[Wystąpienie zarządzane usługi Azure SQL Database](../articles/data-factory/connector-azure-sql-database-managed-insance.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Azure SQL Data Warehouse](../articles/data-factory/connector-azure-sql-data-warehouse.md) |✓ |✓ |✓ |✓  |✓  |
| &nbsp; |[Indeks usługi Azure Search](../articles/data-factory/connector-azure-search.md) | |✓ |✓ |✓  ||
| &nbsp; |[Azure Table Storage](../articles/data-factory/connector-azure-table-storage.md) |✓ |✓ |✓ |✓  ||
| **Baza danych** |[Amazon Redshift](../articles/data-factory/connector-amazon-redshift.md) |✓ | |✓ |✓  ||
| &nbsp; |[DB2](../articles/data-factory/connector-db2.md) |✓ | |✓ |✓  ||
| &nbsp; |[Drill (wersja zapoznawcza)](../articles/data-factory/connector-drill.md) |✓ | |✓ |✓  ||
| &nbsp; |[Google BigQuery](../articles/data-factory/connector-google-bigquery.md) |✓ | |✓ |✓  ||
| &nbsp; |[Greenplum](../articles/data-factory/connector-greenplum.md) |✓ | |✓ |✓  ||
| &nbsp; |[HBase](../articles/data-factory/connector-hbase.md) |✓ | |✓ |✓  ||
| &nbsp; |[Hive](../articles/data-factory/connector-hive.md) |✓ | |✓ |✓  ||
| &nbsp; |[Apache Impala (wersja zapoznawcza)](../articles/data-factory/connector-impala.md) |✓ | |✓ |✓  ||
| &nbsp; |[Informix](../articles/data-factory/connector-odbc.md#ibm-informix-source) |✓ | | |✓  ||
| &nbsp; |[MariaDB](../articles/data-factory/connector-mariadb.md) |✓ | |✓ |✓  ||
| &nbsp; |[Microsoft Access](../articles/data-factory/connector-odbc.md#microsoft-access-source) |✓ | | |✓  ||
| &nbsp; |[MySQL](../articles/data-factory/connector-mysql.md) |✓ | |✓ |✓  ||
| &nbsp; |[Netezza](../articles/data-factory/connector-netezza.md) |✓ | |✓ |✓  ||
| &nbsp; |[Oracle](../articles/data-factory/connector-oracle.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Phoenix](../articles/data-factory/connector-phoenix.md) |✓ | |✓ |✓  ||
| &nbsp; |[PostgreSQL](../articles/data-factory/connector-postgresql.md) |✓ | |✓ |✓  ||
| &nbsp; |[Presto (wersja zapoznawcza)](../articles/data-factory/connector-presto.md) |✓ | |✓ |✓  ||
| &nbsp; |[SAP Business Warehouse Otwórz Centrum](../articles/data-factory/connector-sap-business-warehouse-open-hub.md) |✓ | | |✓  ||
| &nbsp; |[SAP Business Warehouse przy użyciu wyrażeń MDX](../articles/data-factory/connector-sap-business-warehouse.md) |✓ | | |✓  ||
| &nbsp; |[SAP HANA](../articles/data-factory/connector-sap-hana.md) |✓ |✓ | |✓  ||
| &nbsp; |[SAP Table](../articles/data-factory/connector-sap-table.md) |✓ | | |✓  ||
| &nbsp; |[Spark](../articles/data-factory/connector-spark.md) |✓ | |✓ |✓  ||
| &nbsp; |[SQL Server](../articles/data-factory/connector-sql-server.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Sybase](../articles/data-factory/connector-sybase.md) |✓ | | |✓  ||
| &nbsp; |[Teradata](../articles/data-factory/connector-teradata.md) |✓ | | |✓  ||
| &nbsp; |[Vertica](../articles/data-factory/connector-vertica.md) |✓ | |✓ |✓  ||
| **NoSQL** |[Cassandra](../articles/data-factory/connector-cassandra.md) |✓ | |✓ |✓  ||
| &nbsp; |[Couchbase (wersja zapoznawcza)](../articles/data-factory/connector-couchbase.md) |✓ | |✓ |✓  ||
| &nbsp; |[MongoDB](../articles/data-factory/connector-mongodb.md) |✓ | |✓ |✓  ||
| **Plik** |[Amazon S3](../articles/data-factory/connector-amazon-simple-storage-service.md) |✓ | |✓ |✓  ||
| &nbsp; |[System plików](../articles/data-factory/connector-file-system.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[FTP](../articles/data-factory/connector-ftp.md) |✓ | |✓ |✓  ||
| &nbsp; |[Google Cloud Storage](../articles/data-factory/connector-google-cloud-storage.md) |✓ | |✓ |✓  ||
| &nbsp; |[HDFS](../articles/data-factory/connector-hdfs.md) |✓ | |✓ |✓  ||
| &nbsp; |[SFTP](../articles/data-factory/connector-sftp.md) |✓ | |✓ |✓  ||
| **Protokół ogólny** |[Ogólne HTTP](../articles/data-factory/connector-http.md) |✓ | |✓ |✓  ||
| &nbsp; |[Ogólne OData](../articles/data-factory/connector-odata.md) |✓ | |✓ |✓  ||
| &nbsp; |[Ogólne ODBC](../articles/data-factory/connector-odbc.md) |✓ |✓ | |✓  ||
| &nbsp; |[Ogólny REST](../articles/data-factory/connector-rest.md) |✓ | |✓ |✓  ||
| **Usługi i aplikacje** |[Amazon Marketplace Web Service (wersja zapoznawcza)](../articles/data-factory/connector-amazon-marketplace-web-service.md) |✓ | |✓ |✓  ||
| &nbsp; |[Common Data Service for Apps](../articles/data-factory/connector-dynamics-crm-office-365.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Concur (wersja zapoznawcza)](../articles/data-factory/connector-concur.md) |✓ | |✓ |✓  ||
| &nbsp; |[Dynamics 365](../articles/data-factory/connector-dynamics-crm-office-365.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Dynamics AX (wersja zapoznawcza)](../articles/data-factory/connector-dynamics-ax.md) |✓ | |✓ |✓  ||
| &nbsp; |[Dynamics CRM](../articles/data-factory/connector-dynamics-crm-office-365.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Google AdWords (wersja zapoznawcza)](../articles/data-factory/connector-google-adwords.md) |✓ | |✓ |✓  ||
| &nbsp; |[HubSpot (wersja zapoznawcza)](../articles/data-factory/connector-hubspot.md) |✓ | |✓ |✓  ||
| &nbsp; |[Jira (wersja zapoznawcza)](../articles/data-factory/connector-jira.md) |✓ | |✓ |✓  ||
| &nbsp; |[Magento (wersja zapoznawcza)](../articles/data-factory/connector-magento.md) |✓ | |✓ |✓  ||
| &nbsp; |[Marketo (wersja zapoznawcza)](../articles/data-factory/connector-marketo.md) |✓ | |✓ |✓  ||
| &nbsp; |[Office 365](../articles/data-factory/connector-office-365.md) |✓ | |✓ |✓  ||
| &nbsp; |[Oracle Eloqua (wersja zapoznawcza)](../articles/data-factory/connector-oracle-eloqua.md) |✓ | |✓ |✓  ||
| &nbsp; |[Oracle Responsys (wersja zapoznawcza)](../articles/data-factory/connector-oracle-responsys.md) |✓ | |✓ |✓  ||
| &nbsp; |[Usługa w chmurze firmy Oracle (wersja zapoznawcza)](../articles/data-factory/connector-oracle-service-cloud.md) |✓ | |✓ |✓  ||
| &nbsp; |[PayPal (wersja zapoznawcza)](../articles/data-factory/connector-paypal.md) |✓ | |✓ |✓  ||
| &nbsp; |[QuickBooks (wersja zapoznawcza)](../articles/data-factory/connector-quickbooks.md) |✓ | |✓ |✓  ||
| &nbsp; |[Salesforce](../articles/data-factory/connector-salesforce.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Salesforce Service Cloud](../articles/data-factory/connector-salesforce.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Salesforce Marketing Cloud (wersja zapoznawcza)](../articles/data-factory/connector-salesforce-marketing-cloud.md) |✓ | |✓ |✓  ||
| &nbsp; |[SAP Cloud for Customer (C4C)](../articles/data-factory/connector-sap-cloud-for-customer.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[SAP ECC](../articles/data-factory/connector-sap-ecc.md) |✓ | |✓ |✓ ||
| &nbsp; |[ServiceNow](../articles/data-factory/connector-servicenow.md) |✓ | |✓ |✓  ||
| &nbsp; |[Shopify (wersja zapoznawcza)](../articles/data-factory/connector-shopify.md) |✓ | |✓ |✓  ||
| &nbsp; |[Square (wersja zapoznawcza)](../articles/data-factory/connector-square.md) |✓ | |✓ |✓  ||
| &nbsp; |[Tabela internetowa (tabela HTML)](../articles/data-factory/connector-web-table.md) |✓ | | |✓  ||
| &nbsp; |[Xero (wersja zapoznawcza)](../articles/data-factory/connector-xero.md) |✓ | |✓ |✓  ||
| &nbsp; |[Zoho (wersja zapoznawcza)](../articles/data-factory/connector-zoho.md) |✓ | |✓ |✓  ||

> [!NOTE]
> Każdy łącznik oznaczony jako *wersja zapoznawcza* można wypróbować, a następnie przekazać nam opinię na jego temat.  Jeśli w swoim rozwiązaniu chcesz wprowadzić zależność od łączników w wersji zapoznawczej, skontaktuj się z [pomocą techniczną platformy Azure](https://azure.microsoft.com/support/).
