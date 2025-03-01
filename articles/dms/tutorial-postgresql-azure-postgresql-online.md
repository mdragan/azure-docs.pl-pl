---
title: 'Samouczek: Azure Database Migration Service umożliwia wykonanie migracji online postgresql do usługi Azure Database for PostgreSQL | Dokumentacja firmy Microsoft'
description: Dowiedz się wykonać migracji do usługi online z postgresql w warstwie lokalnej do usługi Azure Database for PostgreSQL za pomocą usługi Azure Database Migration Service.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 06/28/2019
ms.openlocfilehash: 96bfb80602efe8e63f814fc9bf6cff3ae52e5983
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461533"
---
# <a name="tutorial-migrate-postgresql-to-azure-database-for-postgresql-online-using-dms"></a>Samouczek: Migrowanie bazy danych PostgreSQL do usługi Azure Database for PostgreSQL w trybie online przy użyciu usługi DMS

Azure Database Migration Service można użyć do migracji baz danych z lokalnego wystąpienia PostgreSQL do [— Azure Database for postgresql w warstwie](https://docs.microsoft.com/azure/postgresql/) przy minimalnych przestojach. Innymi słowy migracja może odbyć się przy minimalnych przestojach w aplikacji. W tym samouczku, wykonywana jest migracja **wypożyczeń DVD** przykładowej bazy danych z lokalnego wystąpienia programu PostgreSQL 9.6 do usługi Azure Database for PostgreSQL za pomocą działania migracji w trybie online w usłudze Azure Database Migration Service.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
>
> * Migrowanie schematu próbki, przy użyciu narzędzia pg_dump.
> * Tworzenie wystąpienia usługi Azure Database Migration Service.
> * Tworzenie projektu migracji za pomocą usługi Azure Database Migration Service.
> * Uruchamianie migracji.
> * Monitorowanie migracji.

> [!NOTE]
> Korzystanie z usługi Azure Database Migration Service w celu wykonania migracji do usługi online wymaga tworzenia wystąpienia na podstawie warstwy cenowej Premium.

> [!IMPORTANT]
> Środowisko optymalne migracji firma Microsoft zaleca tworzenie wystąpienia usługi Azure Database Migration Service w tym samym regionie platformy Azure jako docelowej bazy danych. Przenoszenie danych między regionami lub lokalizacjami geograficznymi może spowalniać proces migracji i powodować błędy.

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka niezbędne są następujące elementy:

* Pobierz i zainstaluj program [PostgreSQL Community Edition](https://www.postgresql.org/download/) w wersji 9.5, 9.6 lub 10. Źródłowy serwer PostgreSQL Server musi być w wersji 9.5.11, 9.6.7, 10 lub nowszej. Aby uzyskać więcej informacji, zobacz artykuł [Obsługiwane wersje bazy danych PostgreSQL](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions).

    Ponadto lokalna wersja programu PostgreSQL musi być zgodna z wersją usługi Azure Database for PostgreSQL. Na przykład program PostgreSQL 9.5.11.5 może być migrowany tylko do usługi Azure Database for PostgreSQL 9.5.11, a nie do wersji 9.6.7.

    > [!NOTE]
    > Database for PostgreSQL w wersji 10 aktualnie DMS obsługuje tylko migrację w wersji 10.3 do usługi Azure Database for PostgreSQL.

* [Utwórz wystąpienie usługi Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal).  
* Tworzenie usługi Azure Virtual Network (VNet) dla usługi Azure Database Migration Service przy użyciu modelu wdrażania usługi Azure Resource Manager, co zapewnia łączność lokacja lokacja z serwerów źródłowych w środowisku lokalnym przy użyciu [usługiExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) lub [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Aby uzyskać więcej informacji na temat tworzenia sieci wirtualnej, zobacz [dokumentacja usługi Virtual Network](https://docs.microsoft.com/azure/virtual-network/), a szczególnie artykułów Szybki Start krok po kroku szczegółowe informacje.

    > [!NOTE]
    > Podczas konfigurowania sieci wirtualnej, jeśli korzystasz z usługi ExpressRoute za pomocą komunikacji równorzędnej sieci do firmy Microsoft, Dodaj następujące usługi [punktów końcowych](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) do podsieci, w której będą udostępniane usługi:
    > * Docelowy punkt końcowy z bazy danych (na przykład SQL punktu końcowego, punktu końcowego usługi Cosmos DB i tak dalej)
    > * Punkt końcowy magazynu
    > * Punkt końcowy usługi Service bus
    >
    > Ta konfiguracja jest konieczne, ponieważ usługi Azure Database Migration Service nie ma łączności z Internetem.

* Upewnij się, że reguły sieciowej grupy zabezpieczeń (NSG) sieci wirtualnej nie blokują następujących portów komunikacji przychodzącej do usługi Azure Database Migration Service: 443, 53, 9354, 445, 12000. Aby uzyskać więcej szczegółowych informacji dotyczących filtrowania ruchu sieciowej grupy zabezpieczeń w sieci wirtualnej platformy Azure, zobacz artykuł [filtrowanie ruchu sieciowego przy użyciu sieciowych grup zabezpieczeń](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm).
* [Zapora sytemu Windows skonfigurowana pod kątem dostępu do aparatu bazy danych](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Otwórz zaporę Windows, aby umożliwić usłudze Azure Database Migration Service, uzyskiwanie dostępu do źródła serwera PostgreSQL, która domyślnie jest TCP port 5432.
* W przypadku korzystania z urządzenia zapory przed źródłową bazą danych konieczne może być dodanie reguł zapory, aby zezwolić usłudze Azure Database Migration Service na dostęp do źródłowej bazy danych podczas migracji.
* Tworzenie z poziomu serwera [reguły zapory](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) dla usługi Azure Database for PostgreSQL umożliwić usłudze Azure Database Migration Service do uzyskania dostępu do docelowych baz danych. Podaj zakres podsieci sieci wirtualnej używane dla usługi Azure Database Migration Service.
* Istnieją dwie metody wywoływania interfejsu wiersza polecenia:

  * Wybierz przycisk Cloud Shell w menu w prawym górnym rogu witryny Azure Portal:

       ![Przycisk Cloud Shell w witrynie Azure Portal](media/tutorial-postgresql-to-azure-postgresql-online/cloud-shell-button.png)

  * Lokalnie zainstaluj i uruchom interfejs wiersza polecenia. Interfejs wiersza polecenia w wersji 2.0 to narzędzie wiersza polecenia do zarządzania zasobami platformy Azure.

       Aby pobrać interfejs wiersza polecenia, postępuj zgodnie z instrukcjami podanymi w artykule [Install Azure CLI 2.0 (Instalowanie interfejsu wiersza polecenia platformy Azure w wersji 2.0)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Artykuł zawiera również listę platform, które obsługują interfejs wiersza polecenia w wersji 2.0.

       Aby skonfigurować podsystem Windows dla systemu Linux (WSL), postępuj zgodnie z instrukcjami w [Przewodniku instalacji systemu Windows 10](https://docs.microsoft.com/windows/wsl/install-win10)

* Włącz replikację logiczną w pliku postgresql.config i ustaw następujące parametry:

  * wal_level = **logical**
  * max_replication_slots = [liczba gniazd], zalecane ustawienie to **5 gniazd**
  * max_wal_senders = [liczba współbieżnych zadań] — parametr max_wal_senders ustawia liczbę współbieżnych zadań, które można uruchomić, przy czym zalecane ustawienie to **10 zadań**

## <a name="migrate-the-sample-schema"></a>Migrowanie przykładowego schematu

Aby utworzyć wszystkie obiekty bazy danych, takie jak schematy tabel, indeksy oraz procedury składowane, należy wyodrębnić schemat ze źródłowej bazy danych i zastosować go w bazie danych.

1. Użyj polecenia -s pg_dump, aby utworzyć plik zrzutu schematu dla bazy danych. 

    ```
    pg_dump -o -h hostname -U db_username -d db_name -s > your_schema.sql
    ```

    Na przykład aby wykonać zrzut pliku schematu bazy danych dvdrental:
    ```
    pg_dump -o -h localhost -U postgres -d dvdrental -s  > dvdrentalSchema.sql
    ```

    Aby uzyskać więcej informacji na temat korzystania z narzędzia pg_dump, zobacz przykłady w samouczku narzędzia [pg-dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html#PG-DUMP-EXAMPLES).

2. Utwórz pustą bazę danych w swoim środowisku docelowym, czyli w usłudze Azure Database for PostgreSQL.

    Zapoznaj się z artykułem [Tworzenie serwera usługi Azure Database for PostgreSQL w witrynie Azure Portal](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal), aby poznać szczegółowe informacje na temat sposobu nawiązywania połączenia z bazą danych i jej tworzenia.

3. Zaimportuj schemat do docelowej bazy danych utworzonej przez przywrócenie pliku zrzutu schematu.

    ```
    psql -h hostname -U db_username -d db_name < your_schema.sql 
    ```

    Na przykład:

    ```
    psql -h mypgserver-20170401.postgres.database.azure.com  -U postgres -d dvdrental < dvdrentalSchema.sql
    ```

4. Jeśli w schemacie znajdują się klucze obce, ładowanie początkowe i ciągła synchronizacja migracji zakończą się niepowodzeniem. Uruchom następujący skrypt w narzędziu PgAdmin lub psql w celu wyodrębnienia skryptu docelowego klucza obcego i dodania skryptu klucza obcego w miejscu docelowym (usłudze Azure Database for PostgreSQL).
  
    ```
    SELECT Queries.tablename
           ,concat('alter table ', Queries.tablename, ' ', STRING_AGG(concat('DROP CONSTRAINT ', Queries.foreignkey), ',')) as DropQuery
                ,concat('alter table ', Queries.tablename, ' ',
                                                STRING_AGG(concat('ADD CONSTRAINT ', Queries.foreignkey, ' FOREIGN KEY (', column_name, ')', 'REFERENCES ', foreign_table_name, '(', foreign_column_name, ')' ), ',')) as AddQuery
        FROM
        (SELECT
        tc.table_schema,
        tc.constraint_name as foreignkey,
        tc.table_name as tableName,
        kcu.column_name,
        ccu.table_schema AS foreign_table_schema,
        ccu.table_name AS foreign_table_name,
        ccu.column_name AS foreign_column_name
    FROM
        information_schema.table_constraints AS tc
        JOIN information_schema.key_column_usage AS kcu
          ON tc.constraint_name = kcu.constraint_name
          AND tc.table_schema = kcu.table_schema
        JOIN information_schema.constraint_column_usage AS ccu
          ON ccu.constraint_name = tc.constraint_name
          AND ccu.table_schema = tc.table_schema
    WHERE constraint_type = 'FOREIGN KEY') Queries
      GROUP BY Queries.tablename;
    ```

    Uruchom docelowy klucz obcy (znajduje się w drugiej kolumnie) w wyniku zapytania.

5. Wyzwalacze w danych (wyzwalacze wstawiania lub aktualizacji) wymuszą integralność danych w miejscu docelowym wcześniej replikowanych danych ze źródła. Zaleca się wyłączenie wyzwalaczy we wszystkich tabelach **w docelowej** podczas migracji, a następnie ponownie włącz ukończenia wyzwalaczy po migracji.

    Aby wyłączyć wyzwalacze w docelowej bazie danych, użyj następującego polecenia:

    ```
    select concat ('alter table ', event_object_table, ' disable trigger ', trigger_name)
    from information_schema.triggers;
    ```

6. W przypadku typu Wyliczeniowego typu danych w żadnych tabel, zaleca się tymczasowo zaktualizowanie go do typu danych "zmieniającego się znakiem" w tabeli docelowej. Po zakończeniu replikacji danych przywróć typ danych ENUM.

## <a name="provisioning-an-instance-of-dms-using-the-cli"></a>Aprowizowanie wystąpienia usługi DMS przy użyciu interfejsu wiersza polecenia

1. Zainstaluj rozszerzenie synchronizacji usługi dms:
   * Uruchom następujące polecenia, aby zalogować się na platformie Azure:
       ```
       az login
       ```

   * Po wyświetleniu monitu otwórz przeglądarkę internetową i wprowadź kod w celu uwierzytelnienia swojego urządzenia. Wykonaj wyświetlone instrukcje.
   * Dodaj rozszerzenie usługi dms:
       * Aby wyświetlić listę dostępnych rozszerzeń, uruchom następujące polecenie:

           ```
           az extension list-available –otable
           ```

       * Aby zainstalować rozszerzenie, uruchom następujące polecenie:

           ```
           az extension add –n dms-preview
           ```

   * Aby sprawdzić, czy rozszerzenie usługi dms zostało poprawnie zainstalowane, uruchom następujące polecenie:

       ```
       az extension list -otable
       ```
       Powinny zostać wyświetlone następujące dane wyjściowe:

       ```
       ExtensionType    Name
       ---------------  ------
       whl              dms
       ```

   * W dowolnym momencie wyświetl wszystkie polecenia obsługiwane w usłudze DMS, uruchamiając polecenie:

       ```
       az dms -h
       ```

   * Jeśli masz wiele subskrypcji platformy Azure, uruchom poniższe polecenie, aby ustawić subskrypcję, której chcesz użyć do aprowizacji wystąpienia usługi DMS.

        ```
       az account set -s 97181df2-909d-420b-ab93-1bff15acb6b7
        ```

2. Rozpocznij aprowizację wystąpienia usługi DMS, uruchamiając następujące polecenie:

   ```
   az dms create -l [location] -n <newServiceName> -g <yourResourceGroupName> --sku-name BusinessCritical_4vCores --subnet/subscriptions/{vnet subscription id}/resourceGroups/{vnet resource group}/providers/Microsoft.Network/virtualNetworks/{vnet name}/subnets/{subnet name} –tags tagName1=tagValue1 tagWithNoValue
   ```

   Na przykład poniższe polecenie utworzy usługę w:

   * Lokalizacja: Wschodnie stany USA 2
   * Subskrypcja: 97181df2-909d-420b-ab93-1bff15acb6b7
   * Nazwa grupy zasobów: PostgresDemo
   * Nazwa usługi DMS: PostgresCLI

   ```
   az dms create -l eastus2 -g PostgresDemo -n PostgresCLI --subnet /subscriptions/97181df2-909d-420b-ab93-1bff15acb6b7/resourceGroups/ERNetwork/providers/Microsoft.Network/virtualNetworks/AzureDMS-CORP-USC-VNET-5044/subnets/Subnet-1 --sku-name BusinessCritical_4vCores
   ```

   Utworzenie wystąpienia usługi DMS trwa około 10–12 minut.

3. W celu zidentyfikowania adresu IP agenta usługi DMS, aby można go było dodać do pliku pg_hba.conf Postgres, uruchom następujące polecenie:

    ```
    az network nic list -g <ResourceGroupName>--query '[].ipConfigurations | [].privateIpAddress'
    ```

    Na przykład:

    ```
    az network nic list -g PostgresDemo --query '[].ipConfigurations | [].privateIpAddress'
    ```

    Powinien zostać uzyskany wynik podobny do następującego adresu: 

    ```
    [
      "172.16.136.18"
    ]
    ```

4. Dodaj adres IP agenta usługi DMS do pliku Postgres pg_hba.conf.

    * Zwróć uwagę na adres IP usługi DMS po zakończeniu aprowizowania usługi DMS.
    * Dodaj adres IP do pliku pg_hba.conf dla źródła podobny do następującego wpisu:

        ```
        host    all     all     172.16.136.18/10    md5
        host    replication     postgres    172.16.136.18/10    md5
        ```

5. Następnie utwórz projekt migracji PostgreSQL, uruchamiając następujące polecenie:
    
    ```
    az dms project create -l <location> -g <ResourceGroupName> --service-name <yourServiceName> --source-platform PostgreSQL --target-platform AzureDbforPostgreSQL -n <newProjectName>
    ```

    Na przykład następujące polecenie tworzy projekt przy użyciu tych parametrów:

   * Lokalizacja: Środkowo-zachodnie stany USA
   * Nazwa grupy zasobów: PostgresDemo
   * Nazwa usługi: PostgresCLI
   * Nazwa projektu: PGMigration
   * Platforma źródłowa: PostgreSQL
   * Platforma docelowa: AzureDbForPostgreSql

     ```
     az dms project create -l eastus2 -n PGMigration -g PostgresDemo --service-name PostgresCLI --source-platform PostgreSQL --target-platform AzureDbForPostgreSql
     ```

6. Utwórz zadanie migracji PostgreSQL, wykonując następujące kroki.

    Ten krok obejmuje wykorzystanie źródłowego adresu IP, identyfikatora użytkownika i hasła, docelowego adresu IP, identyfikatora użytkownika, hasła i typu zadania do ustanowienia połączenia.

   * Aby wyświetlić pełną listę opcji, uruchom polecenie:

       ```
       az dms project task create -h
       ```

       Zarówno dla połączenia elementu źródłowego, jak i docelowego parametr wejściowy odwołuje się do pliku json, który ma listę obiektów.

       Format obiektu JSON połączenia dla połączeń PostgreSQL.
        
       ```
       {
                   "userName": "user name",    // if this is missing or null, you will be prompted
                   "password": null,           // if this is missing or null (highly recommended) you will
               be prompted
                   "serverName": "server name",
                   "databaseName": "database name", // if this is missing, it will default to the 'postgres'
               server
                   "port": 5432                // if this is missing, it will default to 5432
               }
       ```

   * Istnieje również pliku json opcji bazy danych, który zawiera listę obiektów json. W przypadku PostgreSQL format obiektu JSON opcji bazy danych został pokazany poniżej:

       ```
       [
           {
               "name": "source database",
               "target_database_name": "target database",
           },
           ...n
       ]
       ```

   * Utwórz plik json przy użyciu Notatnika, skopiuj następujące polecenia i wklej je do pliku, a następnie zapisz plik w C:\DMS\source.json.

        ```
       {
                   "userName": "postgres",    
                   "password": null,           
               be prompted
                   "serverName": "13.51.14.222",
                   "databaseName": "dvdrental", 
                   "port": 5432                
               }
        ```

   * Utwórz kolejny plik o nazwie target.json i zapisz go jako C:\DMS\target.json. Dołącz następujące polecenia:

       ```
       {
               "userName": " dms@builddemotarget",    
               "password": null,           
               "serverName": " builddemotarget.postgres.database.azure.com",
               "databaseName": "inventory", 
               "port": 5432                
           }
       ```

   * Utwórz plik json opcji bazy danych zawierający spis jako bazę danych do migracji:

       ``` 
       [
           {
               "name": "dvdrental",
               "target_database_name": "dvdrental",
           }
       ]
       ```

   * Uruchom następujące polecenie, które używa elementu źródłowego, docelowego i plików json opcji bazy danych.

       ``` 
       az dms project task create -g PostgresDemo --project-name PGMigration --source-platform postgresql --target-platform azuredbforpostgresql --source-connection-json c:\DMS\source.json --database-options-json C:\DMS\option.json --service-name PostgresCLI --target-connection-json c:\DMS\target.json –task-type OnlineMigration -n runnowtask    
       ```

     W tym momencie zadanie migracji zostało pomyślnie przesłane.

7. Aby wyświetlić postęp zadania, uruchom następujące polecenie:

   ```
   az dms project task show --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask
   ```

   LUB

    ```
   az dms project task show --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask --expand output
    ```

8. Możesz też wysyłać zapytanie dotyczące wartości migrationState z danych wyjściowych rozszerzenia:

    ```
    az dms project task show --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask --expand output --query 'properties.output[].migrationState | [0]' "READY_TO_COMPLETE"
    ```

## <a name="understanding-migration-task-status"></a>Opis stanu zadania migracji

W pliku wyjściowym istnieje kilka parametrów, które wskazują postęp migracji. Na przykład zobacz plik wyjściowy poniżej:

    ```
    "output": [                                 Database Level
          {
            "appliedChanges": 0,        //Total incremental sync applied after full load
            "cdcDeleteCounter": 0       // Total delete operation  applied after full load
            "cdcInsertCounter": 0,      // Total insert operation applied after full load
            "cdcUpdateCounter": 0,      // Total update operation applied after full load
            "databaseName": "inventory",
            "endedOn": null,
            "fullLoadCompletedTables": 2,   //Number of tables completed full load
            "fullLoadErroredTables": 0, //Number of tables that contain migration error
            "fullLoadLoadingTables": 0, //Number of tables that are in loading status
            "fullLoadQueuedTables": 0,  //Number of tables that are in queued status
            "id": "db|inventory",
            "incomingChanges": 0,       //Number of changes after full load
            "initializationCompleted": true,
            "latency": 0,
            "migrationState": "READY_TO_COMPLETE",  //Status of migration task. READY_TO_COMPLETE means the database is ready for cutover
            "resultType": "DatabaseLevelOutput",
            "startedOn": "2018-07-05T23:36:02.27839+00:00"
          },
          {
            "databaseCount": 1,
            "endedOn": null,
            "id": "dd27aa3a-ed71-4bff-ab34-77db4261101c",
            "resultType": "MigrationLevelOutput",
            "sourceServer": "138.91.123.10",
            "sourceVersion": "PostgreSQL",
            "startedOn": "2018-07-05T23:36:02.27839+00:00",
            "state": "PENDING",
            "targetServer": "builddemotarget.postgres.database.azure.com",
            "targetVersion": "Azure Database for PostgreSQL"
          },
          {                                     Table 1
            "cdcDeleteCounter": 0,
            "cdcInsertCounter": 0,
            "cdcUpdateCounter": 0,
            "dataErrorsCount": 0,
            "databaseName": "inventory",
            "fullLoadEndedOn": "2018-07-05T23:36:20.740701+00:00",  //Full load completed time
            "fullLoadEstFinishTime": "1970-01-01T00:00:00+00:00",
            "fullLoadStartedOn": "2018-07-05T23:36:15.864552+00:00",    //Full load started time
            "fullLoadTotalRows": 10,                    //Number of rows loaded in full load
            "fullLoadTotalVolumeBytes": 7056,               //Volume in Bytes in full load
            "id": "or|inventory|public|actor",          
            "lastModifiedTime": "2018-07-05T23:36:16.880174+00:00",
            "resultType": "TableLevelOutput",
            "state": "COMPLETED",                   //State of migration for this table
            "tableName": "public.catalog",              //Table name
            "totalChangesApplied": 0                //Total sync changes that applied after full load
          },
          {                                     Table 2
            "cdcDeleteCounter": 0,
            "cdcInsertCounter": 50,
            "cdcUpdateCounter": 0,
            "dataErrorsCount": 0,
            "databaseName": "inventory",
            "fullLoadEndedOn": "2018-07-05T23:36:23.963138+00:00",
            "fullLoadEstFinishTime": "1970-01-01T00:00:00+00:00",
            "fullLoadStartedOn": "2018-07-05T23:36:19.302013+00:00",
            "fullLoadTotalRows": 112,
            "fullLoadTotalVolumeBytes": 46592,
            "id": "or|inventory|public|address",
            "lastModifiedTime": "2018-07-05T23:36:20.308646+00:00",
            "resultType": "TableLevelOutput",
            "state": "COMPLETED",
            "tableName": "public.orders",
            "totalChangesApplied": 0
          }
        ],                          DMS migration task state
        "state": "Running", //Migration task state – Running means it is still listening to any changes that might come in                  
        "taskType": null
      },
      "resourceGroup": "PostgresDemo",
      "type": "Microsoft.DataMigration/services/projects/tasks"
    ```

## <a name="cutover-migration-task"></a>Zadanie migracji uruchomienia produkcyjnego

Baza danych jest gotowa do uruchomienia produkcyjnego, gdy pełne ładowanie zostało zakończone. W zależności od zajętości serwera źródłowego nowymi przychodzącymi transakcjami zadanie usługi DMS może nadal stosować zmiany po zakończeniu pełnego ładowania.

Aby upewnić się, że wszystkie dane zostały przechwycone, przeprowadź weryfikację liczby wierszy między źródłową i docelową bazą danych. Na przykład możesz użyć poniższego polecenia:

```
"migrationState": "READY_TO_COMPLETE", //Status of migration task. READY_TO_COMPLETE means database is ready for cutover
 "incomingChanges": 0,  //continue to check for a period of 5-10 minutes to make sure no new incoming changes that need to be applied to the target server
   "fullLoadTotalRows": 10, //full load for table 1
    "cdcDeleteCounter": 0,  //delete, insert and update counter on incremental sync after full load
    "cdcInsertCounter": 50,
    "cdcUpdateCounter": 0,
     "fullLoadTotalRows": 112,  //full load for table 2
```

1. Wykonaj zadanie migracji bazy danych uruchomienia produkcyjnego, używając następującego polecenia:

    ```
    az dms project task cutover -h
    ```

    Na przykład:

    ```
    az dms project task cutover --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask  --database-name Inventory
    ```

2. Aby monitorować postęp uruchomienia produkcyjnego, uruchom następujące polecenie:

    ```
    az dms project task show --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask
    ```

## <a name="service-project-task-cleanup"></a>Usługa, projekt, oczyszczanie zadania

Jeśli potrzebujesz anulować lub usunąć jakiekolwiek zadanie usługi DMS, projekt lub usługę, przeprowadź anulowanie w następującej kolejności:

* Anuluj wszelkie uruchomione zadania
* Usuń zadanie
* Usuń projekt
* Usuń usługę DMS

1. Aby anulować bieżące zadanie, użyj następującego polecenia:

    ```
    az dms project task cancel --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask
     ```

2. Aby usunąć bieżące zadanie, użyj następującego polecenia:
    ```
    az dms project task delete --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name Runnowtask
    ```

3. Aby anulować bieżący projekt, użyj następującego polecenia:
     ```
    az dms project task cancel -n runnowtask --project-name PGMigration -g PostgresDemo --service-name PostgresCLI
     ```

4. Aby usunąć bieżący projekt, użyj następującego polecenia:
    ```
    az dms project task delete -n runnowtask --project-name PGMigration -g PostgresDemo --service-name PostgresCLI
    ```

5. Aby usunąć usługę DMS, użyj następującego polecenia:

     ```
    az dms delete -g ProgresDemo -n PostgresCLI
     ```

## <a name="next-steps"></a>Następne kroki

* Aby uzyskać informacje o znanych problemach i ograniczeniach związanych z wykonywaniem migracji w trybie online do usługi Azure Database for PostgreSQL, zobacz artykuł [Znane problemy i obejścia problemów związanych z migracjami online usługi Azure Database for PostgreSQL](known-issues-azure-postgresql-online.md).
* Aby uzyskać informacje o usłudze Azure Database Migration Service, zobacz artykuł [Czym jest usługa Azure Database Migration Service?](https://docs.microsoft.com/azure/dms/dms-overview).
* Aby uzyskać informacje o usłudze Azure Database for PostgreSQL, zobacz artykuł [co to jest Azure Database for PostgreSQL?](https://docs.microsoft.com/azure/postgresql/overview).