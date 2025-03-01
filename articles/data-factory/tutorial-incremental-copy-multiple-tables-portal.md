---
title: Przyrostowe kopiowanie wielu tabel przy użyciu usługi Azure Data Factory | Microsoft Docs
description: W tym samouczku utworzysz potok usługi Azure Data Factory służący do przyrostowego kopiowania danych różnicowych z wielu tabel w lokalnej bazie danych SQL Server do bazy danych Azure SQL Database.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/20/2018
ms.author: yexu
ms.openlocfilehash: b9dafd31ed84298c97932b1cdb5593eb17769ef9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60582384"
---
# <a name="incrementally-load-data-from-multiple-tables-in-sql-server-to-an-azure-sql-database"></a>Przyrostowe ładowanie danych z wielu tabel w programie SQL Server do bazy danych Azure SQL Database
W tym samouczku utworzysz fabrykę danych Azure Data Factory z potokiem służącym do ładowania danych różnicowych z wielu tabel na lokalnym serwerze SQL Server do bazy danych Azure SQL Database.    

Ten samouczek obejmuje następujące procedury:

> [!div class="checklist"]
> * Przygotowanie źródłowych i docelowych magazynów danych
> * Tworzenie fabryki danych.
> * Utwórz własne środowisko Integration Runtime.
> * Instalowanie środowiska Integration Runtime. 
> * Tworzenie połączonych usług. 
> * Tworzenie zestawów danych źródła, ujścia i limitu.
> * Tworzenie, uruchamianie i monitorowanie potoku
> * Przejrzyj wyniki.
> * Dodawanie lub aktualizowanie danych w tabelach źródłowych.
> * Ponowne uruchamianie i monitorowanie potoku.
> * Przegląd wyników końcowych.

## <a name="overview"></a>Omówienie
Poniżej przedstawiono ważne czynności związane z tworzeniem tego rozwiązania: 

1. **Wybierz kolumnę limitu**.
    
    Wybierz jedną kolumnę dla każdej tabeli w magazynie danych źródłowych, która może służyć do identyfikowania nowych lub zaktualizowanych rekordów dla każdego uruchomienia. Zazwyczaj dane w tej wybranej kolumnie (na przykład last_modify_time lub ID) rosną wraz z tworzeniem i aktualizacją wierszy. Maksymalna wartość w tej kolumnie jest używana jako limit.

1. **Przygotuj magazyn danych do przechowywania wartości limitu**.   
    
    W tym samouczku wartość limitu jest przechowywana w bazie danych SQL.

1. **Utwórz potok z następującymi działaniami**: 
    
    a. Utwórz działanie ForEach służące do przeprowadzania iteracji po liście nazw tabel źródłowych przekazywanych jako parametr do potoku. Dla każdej tabeli źródłowej wywołuje ono następujące działania służące do wykonywania ładowania przyrostowego dla tej tabeli.

    b. Utwórz dwa działania lookup. Użyj pierwszego działania Lookup do pobrania ostatniej wartości limitu. Użyj drugiego działania Lookup do pobrania nowej wartości limitu. Te wartości limitu są przekazywane do działania Copy.

    d. Utwórz działanie Copy, które kopiuje wiersze z magazynu danych źródłowych o wartości kolumny limitu większej niż poprzednia wartość limitu i mniejszej niż nowa wartość limitu. Następnie kopiuje dane różnicowe ze źródłowego magazynu danych do usługi Azure Blob Storage jako nowy plik.

    d. Utwórz działanie StoredProcedure, które aktualizuje wartość limitu dla potoku przy następnym uruchomieniu. 

    Diagram ogólny rozwiązania wygląda następująco: 

    ![Przyrostowe ładowanie danych](media/tutorial-incremental-copy-multiple-tables-portal/high-level-solution-diagram.png)


Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="prerequisites"></a>Wymagania wstępne
* **SQL Server**. Użyj lokalnej bazy danych SQL Server jako źródłowego magazynu danych w tym samouczku. 
* **Usługa Azure SQL Database**. Jako magazyn danych ujścia używana jest baza danych SQL. Jeśli nie masz bazy danych SQL, utwórz ją, wykonując czynności przedstawione w artykule [Tworzenie bazy danych Azure SQL Database](../sql-database/sql-database-get-started-portal.md). 

### <a name="create-source-tables-in-your-sql-server-database"></a>Tworzenie tabel źródłowych w bazie danych SQL Server

1. Otwórz program SQL Server Management Studio, a następnie nawiąż połączenie z lokalną bazą danych programu SQL Server.

1. W **Eksploratorze serwera** kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz pozycję **Nowe zapytanie**.

1. Uruchom następujące polecenie SQL względem bazy danych w celu utworzenia tabel o nazwach `customer_table` i `project_table`:

    ```sql
    create table customer_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );
    
    create table project_table
    (
        Project varchar(255),
        Creationtime datetime
    );
        
    INSERT INTO customer_table
    (PersonID, Name, LastModifytime)
    VALUES
    (1, 'John','9/1/2017 12:56:00 AM'),
    (2, 'Mike','9/2/2017 5:23:00 AM'),
    (3, 'Alice','9/3/2017 2:36:00 AM'),
    (4, 'Andy','9/4/2017 3:21:00 AM'),
    (5, 'Anny','9/5/2017 8:06:00 AM');
    
    INSERT INTO project_table
    (Project, Creationtime)
    VALUES
    ('project1','1/1/2015 0:00:00 AM'),
    ('project2','2/2/2016 1:23:00 AM'),
    ('project3','3/4/2017 5:16:00 AM');
    
    ```

### <a name="create-destination-tables-in-your-azure-sql-database"></a>Tworzenie tabel docelowych w bazie danych Azure SQL Database
1. Otwórz program SQL Server Management Studio i nawiąż połączenie z bazą danych Azure SQL Database.

1. W **Eksploratorze serwera** kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz pozycję **Nowe zapytanie**.

1. Uruchom następujące polecenie SQL względem bazy danych SQL w celu utworzenia tabel o nazwach `customer_table` i `project_table`:  
    
    ```sql
    create table customer_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );
    
    create table project_table
    (
        Project varchar(255),
        Creationtime datetime
    );

    ```

### <a name="create-another-table-in-the-azure-sql-database-to-store-the-high-watermark-value"></a>Tworzenie innej tabeli w bazie danych Azure SQL Database do przechowywania wartości górnego limitu
1. Uruchom następujące polecenie SQL względem bazy danych SQL, aby utworzyć tabelę o nazwie `watermarktable` w celu przechowywania wartości limitu: 
    
    ```sql
    create table watermarktable
    (
    
        TableName varchar(255),
        WatermarkValue datetime,
    );
    ```
1. Wstaw początkowe wartości limitu dla obu tabel źródłowych w tabeli wartości limitu.

    ```sql

    INSERT INTO watermarktable
    VALUES 
    ('customer_table','1/1/2010 12:00:00 AM'),
    ('project_table','1/1/2010 12:00:00 AM');
    
    ```

### <a name="create-a-stored-procedure-in-the-azure-sql-database"></a>Tworzenie procedury składowanej w bazie danych Azure SQL Database 

Uruchom następujące polecenie, aby utworzyć procedurę składowaną w bazie danych SQL. Ta procedura składowana służy do aktualizowania wartość limitu po każdym uruchomieniu potoku. 

```sql
CREATE PROCEDURE usp_write_watermark @LastModifiedtime datetime, @TableName varchar(50)
AS

BEGIN

    UPDATE watermarktable
    SET [WatermarkValue] = @LastModifiedtime 
WHERE [TableName] = @TableName

END

```

### <a name="create-data-types-and-additional-stored-procedures-in-azure-sql-database"></a>Tworzenie typów danych i dodatkowych procedur składowanych w bazie danych Azure SQL Database
Uruchom następujące zapytanie, aby utworzyć dwie procedury składowane i dwa typy danych w bazie danych SQL. Służą one do scalania danych z tabel źródłowych w tabelach docelowych.

Aby ułatwić podróży w zaczynać, firma Microsoft bezpośrednio korzystać z tych procedur składowanych, przekazując dane różnicowe w za pośrednictwem zmiennej tabeli, a następnie scalić je do docelowego magazynu. Należy zachować ostrożność, jego ma być przechowywany w zmiennej tabeli nie jest oczekiwane "large" Liczba zmian wierszy (ponad 100).  

Jeśli musisz scalić dużą liczbę wierszy delta Magazyn docelowy, sugerujemy przy użyciu działania kopiowania do skopiowania danych różnicowych do tabeli tymczasowej "tymczasową" w miejscu docelowym przechowywania pierwszy, a następnie kompilowany procedury przechowywanej bez użycia vari tabeli można scalić je z tabeli "tymczasową" do tabeli "final". 


```sql
CREATE TYPE DataTypeforCustomerTable AS TABLE(
    PersonID int,
    Name varchar(255),
    LastModifytime datetime
);

GO

CREATE PROCEDURE usp_upsert_customer_table @customer_table DataTypeforCustomerTable READONLY
AS

BEGIN
  MERGE customer_table AS target
  USING @customer_table AS source
  ON (target.PersonID = source.PersonID)
  WHEN MATCHED THEN
      UPDATE SET Name = source.Name,LastModifytime = source.LastModifytime
  WHEN NOT MATCHED THEN
      INSERT (PersonID, Name, LastModifytime)
      VALUES (source.PersonID, source.Name, source.LastModifytime);
END

GO

CREATE TYPE DataTypeforProjectTable AS TABLE(
    Project varchar(255),
    Creationtime datetime
);

GO

CREATE PROCEDURE usp_upsert_project_table @project_table DataTypeforProjectTable READONLY
AS

BEGIN
  MERGE project_table AS target
  USING @project_table AS source
  ON (target.Project = source.Project)
  WHEN MATCHED THEN
      UPDATE SET Creationtime = source.Creationtime
  WHEN NOT MATCHED THEN
      INSERT (Project, Creationtime)
      VALUES (source.Project, source.Creationtime);
END

```

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych

1. Uruchom przeglądarkę internetową **Microsoft Edge** lub **Google Chrome**. Obecnie interfejs użytkownika usługi Data Factory jest obsługiwany tylko przez przeglądarki internetowe Microsoft Edge i Google Chrome.
1. Kliknij przycisk **Nowy** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij pozycję **Data Factory**. 
   
   ![Nowy-> Fabryka danych](./media/tutorial-incremental-copy-multiple-tables-portal/new-azure-data-factory-menu.png)
1. Na stronie **Nowa fabryka danych** wprowadź wartość **ADFMultiIncCopyTutorialDF** w polu **nazwa**. 
      
     ![Strona Nowa fabryka danych](./media/tutorial-incremental-copy-multiple-tables-portal/new-azure-data-factory.png)
 
   Nazwa fabryki danych platformy Azure musi być **globalnie unikatowa**. Jeśli zostanie zwrócony następujący błąd, zmień nazwę fabryki danych (np. na twoja_nazwaADFMultiIncCopyTutorialDF) i ponów próbę utworzenia fabryki. Artykuł [Data Factory — Naming Rules (Usługa Data Factory — reguły nazewnictwa)](naming-rules.md) zawiera reguły nazewnictwa artefaktów usługi Data Factory.
  
       `Data factory name ADFMultiIncCopyTutorialDF is not available`
1. Wybierz **subskrypcję** Azure, w której chcesz utworzyć fabrykę danych. 
1. Dla opcji **Grupa zasobów** wykonaj jedną z następujących czynności:
     
      - Wybierz pozycję **Użyj istniejącej**, a następnie wybierz istniejącą grupę zasobów z listy rozwijanej. 
      - Wybierz pozycję **Utwórz nową**, a następnie wprowadź nazwę grupy zasobów.   
         
        Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).  
1. Wybierz wartość **V2 (wersja zapoznawcza)** dla **wersji**.
1. Na liście **lokalizacja** wybierz lokalizację fabryki danych. Na liście rozwijanej są wyświetlane tylko obsługiwane lokalizacje. Magazyny danych (Azure Storage, Azure SQL Database itp.) i jednostki obliczeniowe (HDInsight itp.) używane przez fabrykę danych mogą mieścić się w innych regionach.
1. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**.     
1. Kliknij pozycję **Utwórz**.      
1. Na pulpicie nawigacyjnym jest widoczny następujący kafelek ze stanem: **Wdrażanie fabryki danych**. 

    ![kafelek Wdrażanie fabryki danych](media/tutorial-incremental-copy-multiple-tables-portal/deploying-data-factory.png)
1. Po zakończeniu tworzenia zostanie wyświetlona strona **Fabryka danych**, jak pokazano na poniższej ilustracji.
   
   ![Strona główna fabryki danych](./media/tutorial-incremental-copy-multiple-tables-portal/data-factory-home-page.png)
1. Kliknij kafelek **Tworzenie i monitorowanie**, aby uruchomić interfejs użytkownika usługi Azure Data Factory na osobnej karcie.
1. Na stronie wprowadzenia w interfejsie użytkownika usługi Azure Data Factory kliknij pozycję **Utwórz potok** lub przejdź do karty **Edycja**. 

   ![Strona Wprowadzenie](./media/tutorial-incremental-copy-multiple-tables-portal/get-started-page.png)

## <a name="create-self-hosted-integration-runtime"></a>Tworzenie własnego środowiska Integration Runtime
Podczas przenoszenia danych z magazynu danych w sieci prywatnej (lokalnej) do magazynu danych platformy Azure zainstaluj własne środowisko Integration Runtime (IR) w środowisku lokalnym. Własne środowisko IR przenosi dane między siecią prywatną a platformą Azure. 

1. Kliknij pozycję **Połączenia** u dołu okienka po lewej stronie i przejdź do obszaru **Produkty Integration Runtime** w oknie **Połączenia**. 

   ![Karta Połączenia](./media/tutorial-incremental-copy-multiple-tables-portal/connections-tab.png)
1. Na karcie **Produkty Integration Runtime** kliknij pozycję **+ Nowy**. 

   ![Nowe środowisko Integration Runtime — przycisk](./media/tutorial-incremental-copy-multiple-tables-portal/new-integration-runtime-button.png)
1. W oknie **Konfiguracja środowiska Integration Runtime** wybierz pozycję **Wykonuj przenoszenie danych i wysyłaj działania do obliczeń zewnętrznych**, a następnie kliknij pozycję **Dalej**. 

   ![Wybieranie typu środowiska Integration Runtime](./media/tutorial-incremental-copy-multiple-tables-portal/select-integration-runtime-type.png)
1. Wybierz pozycję ** Sieć prywatna ** i kliknij pozycję **Dalej**. 

   ![Wybór sieci prywatnej](./media/tutorial-incremental-copy-multiple-tables-portal/select-private-network.png)
1. Wprowadź ciąg **MySelfHostedIR** w polu **Nazwa** i kliknij pozycję **Dalej**. 

   ![Nazwa własnego środowiska IR](./media/tutorial-incremental-copy-multiple-tables-portal/self-hosted-ir-name.png)
1. Kliknij pozycję **Kliknij tutaj, aby uruchomić instalację ekspresową dla tego komputera** w sekcji **Opcja 1: Instalacja ekspresowa**. 

   ![Kliknij link instalacji ekspresowej](./media/tutorial-incremental-copy-multiple-tables-portal/click-express-setup.png)
1. W oknie **Instalacja ekspresowa środowiska Integration Runtime (Self-hosted)** kliknij przycisk **Zamknij**. 

   ![Instalacja środowiska Integration Runtime — pomyślna](./media/tutorial-incremental-copy-multiple-tables-portal/integration-runtime-setup-successful.png)
1. W przeglądarce internetowej w oknie **instalatora środowiska Integration Runtime** kliknij przycisk **Zakończ**. 

   ![Instalacja środowiska Integration Runtime — zakończenie](./media/tutorial-incremental-copy-multiple-tables-portal/click-finish-integration-runtime-setup.png)
1. Upewnij się, że na liście środowisk Integration Runtime jest widoczna pozycja **MySelfHostedIR**.

    ![Środowiska Integration Runtime — lista](./media/tutorial-incremental-copy-multiple-tables-portal/integration-runtimes-list.png)

## <a name="create-linked-services"></a>Tworzenie połączonych usług
Połączone usługi tworzy się w fabryce danych w celu połączenia magazynów danych i usług obliczeniowych z fabryką danych. W tej sekcji utworzysz usługi połączone z używaną, lokalną bazą danych SQL Server i bazą danych SQL. 

### <a name="create-the-sql-server-linked-service"></a>Tworzenie usługi połączonej z serwerem SQL Server
W tym kroku połączysz lokalną bazę danych programu SQL Server z fabryką danych.

1. W oknie **Połączenia** przejdź z karty **Środowiska Integration Runtime** do karty **Połączone usługi**, a następnie kliknij pozycję **+ Nowa**.

    ![Przycisk Nowa połączona usługa](./media/tutorial-incremental-copy-multiple-tables-portal/new-sql-server-linked-service-button.png)
1. W oknie **Nowa połączona usługa** wybierz pozycję **SQL Server**, a następnie kliknij pozycję **Kontynuuj**. 

    ![Wybieranie pozycji SQL Server](./media/tutorial-incremental-copy-multiple-tables-portal/select-sql-server.png)
1. W oknie **Nowa połączona usługa** wykonaj następujące czynności:

    1. Wprowadź jako **nazwę** wartość **SqlServerLinkedService**. 
    1. Wybierz pozycję **MySelfHostedIR** w polu **Połącz za pośrednictwem środowiska Integration Runtime**. Jest to **ważny** krok. Domyślne środowisko Integration Runtime nie może połączyć się z lokalnym magazynem danych. Użyj własnego środowiska Integration Runtime utworzonego wcześniej. 
    1. W polu **Nazwa serwera** wprowadź nazwę komputera, na którym znajduje się baza danych programu SQL Server.
    1. W polu **Nazwa bazy danych** wprowadź nazwę bazy danych programu SQL Server, która zawiera dane źródłowe. Tworzenie tabeli i wstawienie danych do tej bazy danych zostało przeprowadzone w ramach wymagań wstępnych. 
    1. W polu **Typ uwierzytelniania** wybierz **typ uwierzytelniania**, którego chcesz używać podczas łączenia się z bazą danych. 
    1. W polu **Nazwa użytkownika** wprowadź nazwę użytkownika mającego dostęp do bazy danych programu SQL Server. Jeśli musisz użyć znaku ukośnika (`\`) w nazwie konta użytkownika lub nazwie serwera, użyj znaku ucieczki (`\`). Może to być na przykład `mydomain\\myuser`.
    1. W polu **Hasło** wprowadź **hasło** użytkownika. 
    1. Aby sprawdzić, czy fabryka danych może połączyć się z bazą danych programu SQL Server, kliknij pozycję **Testuj połączenie**. Usuń wszelkie błędy, tak aby połączenie zostało nawiązane pomyślnie. 
    1. Aby zapisać połączoną usługę, kliknij przycisk **Zapisz**.

        ![Usługę połączona z serwerem SQL Server — ustawienia](./media/tutorial-incremental-copy-multiple-tables-portal/sql-server-linked-service-settings.png)

### <a name="create-the-azure-sql-database-linked-service"></a>Tworzenie połączonej usługi Azure SQL Database
W ostatnim kroku utworzysz połączoną usługę w celu połączenia źródłowej bazy danych programu SQL Server z fabryką danych. W tym kroku połączysz docelową bazę danych (bazę danych ujścia) Azure SQL Database z fabryką danych. 

1. W oknie **Połączenia** przejdź z karty **Środowiska Integration Runtime** do karty **Połączone usługi**, a następnie kliknij pozycję **+ Nowa**.

    ![Przycisk Nowa połączona usługa](./media/tutorial-incremental-copy-multiple-tables-portal/new-sql-server-linked-service-button.png)
1. W oknie **Nowa połączona usługa** wybierz pozycję **Azure SQL Database**, a następnie kliknij pozycję **Kontynuuj**. 
1. W oknie **Nowa połączona usługa** wykonaj następujące czynności:

    1. Wprowadź wartość **AzureSqlDatabaseLinkedService** w polu **Nazwa**. 
    1. Z listy rozwijanej **Nazwa serwera** wybierz nazwę serwera Azure SQL Database. 
    1. W polu **Nazwa bazy danych** wybierz bazę danych Azure SQL Database, w której zostały utworzone tabele customer_table i project_table w ramach wymagań wstępnych. 
    1. W polu **Nazwa użytkownika** wprowadź nazwę użytkownika mającego dostęp do bazy danych Azure SQL Database. 
    1. W polu **Hasło** wprowadź **hasło** użytkownika. 
    1. Aby sprawdzić, czy fabryka danych może połączyć się z bazą danych programu SQL Server, kliknij pozycję **Testuj połączenie**. Usuń wszelkie błędy, tak aby połączenie zostało nawiązane pomyślnie. 
    1. Aby zapisać połączoną usługę, kliknij przycisk **Zapisz**.

        ![Połączona usługa Azure SQL — ustawienia](./media/tutorial-incremental-copy-multiple-tables-portal/azure-sql-linked-service-settings.png)
1. Upewnij się, że lista zawiera dwie połączone usługi. 
   
    ![Dwie połączone usługi](./media/tutorial-incremental-copy-multiple-tables-portal/two-linked-services.png) 

## <a name="create-datasets"></a>Tworzenie zestawów danych
W tym kroku utworzysz zestawy danych reprezentujące źródło danych, docelową lokalizację danych i lokalizację, w której będzie przechowywana wartość limitu.

### <a name="create-a-source-dataset"></a>Tworzenie zestawu danych źródłowych

1. W lewym okienku kliknij pozycję **+ (plus)**, a następnie kliknij pozycję **Zestaw danych**.

   ![Menu Nowy zestaw danych](./media/tutorial-incremental-copy-multiple-tables-portal/new-dataset-menu.png)
1. W oknie **Nowy zestaw danych** wybierz pozycję **SQL Server** i kliknij pozycję **Zakończ**. 

   ![Wybieranie pozycji SQL Server](./media/tutorial-incremental-copy-multiple-tables-portal/select-sql-server-for-dataset.png)
1. W przeglądarce sieci Web zostanie otwarta nowa karta służąca do konfigurowania zestawu danych. Zostanie też wyświetlony zestaw danych w widoku drzewa. U dołu karty **Ogólne** w oknie właściwości wprowadź wartość **SourceDataset** w polu **Nazwa**. 

   ![Zestaw danych źródłowych — nazwa](./media/tutorial-incremental-copy-multiple-tables-portal/source-dataset-general.png)
1. Przejdź do karty **Połączenie** w oknie właściwości, a następnie wybierz pozycję **SqlServerLinkedService** w polu **Połączona usługa**. Nie należy wybierać tabeli w tym miejscu. Działanie Copy w potoku korzysta z zapytania SQL do załadowania danych, a nie całej tabeli.

   ![Źródłowy zestaw danych — połączenie](./media/tutorial-incremental-copy-multiple-tables-portal/source-dataset-connection.png)


### <a name="create-a-sink-dataset"></a>Tworzenie ujścia zestawu danych
1. W lewym okienku kliknij pozycję **+ (plus)**, a następnie kliknij pozycję **Zestaw danych**.

   ![Menu Nowy zestaw danych](./media/tutorial-incremental-copy-multiple-tables-portal/new-dataset-menu.png)
1. W oknie **Nowy zestaw danych** wybierz pozycję **Azure SQL Database** i kliknij przycisk **Zakończ**. 

   ![Wybieranie usługi Azure SQL Database](./media/tutorial-incremental-copy-multiple-tables-portal/select-azure-sql-database.png)
1. W przeglądarce sieci Web zostanie otwarta nowa karta służąca do konfigurowania zestawu danych. Zostanie też wyświetlony zestaw danych w widoku drzewa. U dołu karty **Ogólne** w oknie właściwości wprowadź wartość **SinkDataset** w polu **Nazwa**.

   ![Zestaw danych będący ujściem — ogólne](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-general.png)
1. Przejdź do karty **Parametry** w oknie Właściwości i wykonaj następujące czynności: 

    1. Kliknij pozycję **Nowy** w sekcji **Parametry tworzenia/aktualizacji**. 
    1. Wprowadź wartość **SinkTableName** w polu **nazwa** i wartość **Ciąg** w polu **typ**. Ten zestaw danych otrzymuje wartość **SinkTableName** jako parametr. Parametr SinkTableName jest ustawiany dynamicznie przez potok w czasie wykonywania. Działanie ForEach w potoku przeprowadza iterację po liście nazw i przekazuje nazwę tabeli do tego zestawu danych w każdej iteracji.
   
       ![Zestaw danych będący ujściem — właściwości](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-parameters.png)
1. Przejdź do karty **Połączenie** w oknie właściwości, a następnie wybierz pozycję **AzureSqlLinkedService** w polu **Połączona usługa**. W obszarze właściwości **Tabela** kliknij pozycję **Dodaj zawartość dynamiczną**. 

   ![Zestaw danych będący ujściem — połączenie](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-connection.png)
    
    
1. W sekcji **Parametry** wybierz pozycję **SinkTableName**.
   
   ![Zestaw danych będący ujściem — połączenie](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-connection-dynamicContent.png)

   
 1. Po kliknięciu przycisku **Zakończ** zostanie wyświetlona nazwa tabeli **@dataset().SinkTableName**.
   
   ![Zestaw danych będący ujściem — połączenie](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-connection-completion.png)

### <a name="create-a-dataset-for-a-watermark"></a>Tworzenie zestawu danych dla limitu
W tym kroku utworzysz zestaw danych do przechowywania wartości górnego limitu. 

1. W lewym okienku kliknij pozycję **+ (plus)**, a następnie kliknij pozycję **Zestaw danych**.

   ![Menu Nowy zestaw danych](./media/tutorial-incremental-copy-multiple-tables-portal/new-dataset-menu.png)
1. W oknie **Nowy zestaw danych** wybierz pozycję **Azure SQL Database** i kliknij przycisk **Zakończ**. 

   ![Wybieranie usługi Azure SQL Database](./media/tutorial-incremental-copy-multiple-tables-portal/select-azure-sql-database.png)
1. U dołu karty **Ogólne** w oknie właściwości wprowadź wartość **WatermarkDataset** w polu **Nazwa**.
1. Przejdź do karty **Połączenie** i wykonaj następujące czynności: 

    1. Wybierz wartość **AzureSqlDatabaseLinkedService** w polu **Połączona usługa**.
    1. Wybierz element **[dbo].[watermarktable]** dla pozycji **Tabela**.

       ![Zestaw danych limitu — połączenie](./media/tutorial-incremental-copy-multiple-tables-portal/watermark-dataset-connection.png)

## <a name="create-a-pipeline"></a>Tworzenie potoku
Potok przyjmuje listę nazw tabel jako parametr. Działanie ForEach służy do przeprowadzania iteracji po liście nazw tabel i wykonywania następujących operacji: 

1. Użyj działania Lookup do pobrania starej wartość limitu (wartości początkowej lub wartości użytej w ostatniej iteracji).

1. Użyj działania Lookup do pobrania nowej wartości limitu (maksymalnej wartości kolumny limitu w tabeli źródłowej).

1. Użyj działania Copy do skopiowania danych między tymi dwiema wartościami limitu ze źródłowej bazy danych do docelowej bazy danych.

1. Użyj działania StoredProcedure do zaktualizowania starej wartości limitu, która zostanie użyta w pierwszym kroku następnej iteracji. 

### <a name="create-the-pipeline"></a>Tworzenie potoku

1. W lewym okienku kliknij pozycję **+ (plus)**, a następnie kliknij pozycję **Potok**.

    ![Nowy potok — menu](./media/tutorial-incremental-copy-multiple-tables-portal/new-pipeline-menu.png)
1. Na karcie **Ogólne** w oknie **Właściwości** wprowadź wartość **IncrementalCopyPipeline** w polu **Nazwa**. 

    ![Nazwa potoku](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-name.png)
1. W oknie **Właściwości** wykonaj następujące czynności: 

    1. Kliknij pozycję **+ Nowy**. 
    1. Wprowadź ciąg **tableList** jako **nazwę** parametru. 
    1. Wybierz pozycję **Obiekt** dla parametru **typ**.

    ![Parametry potoku](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-parameters.png) 
1. W przyborniku **Działania** rozwiń pozycję **Iteracja i warunki**, a następnie przeciągnij i upuść działanie **ForEach** na powierzchni projektanta potoku. Na karcie **Ogólne** w oknie **Właściwości** wprowadź wartość **IterateSQLTables**. 

    ![Działanie ForEach — nazwa](./media/tutorial-incremental-copy-multiple-tables-portal/foreach-name.png)
1. Przejdź do karty **Ustawienia** w oknie **Właściwości** i wprowadź wartość `@pipeline().parameters.tableList` w polu **Elementy**. Działanie ForEach przeprowadza iterację po liście tabel i wykonuje operację kopiowania przyrostowego. 

    ![Działanie ForEach — ustawienia](./media/tutorial-incremental-copy-multiple-tables-portal/foreach-settings.png)
1. Wybierz działanie **ForEach** w potoku, jeśli jeszcze nie zostało wybrane. Kliknij przycisk **Edytuj (ikonę ołówka)**.

    ![Działanie ForEach — edycja](./media/tutorial-incremental-copy-multiple-tables-portal/edit-foreach.png)
1. W przyborniku **Działania** rozwiń pozycję **SQL Database**, przeciągnij i upuść działanie **Lookup** (Wyszukiwanie) na powierzchni projektanta potoku, a następnie wprowadź wartość **LookupOldWaterMarkActivity** w polu **Nazwa**.

    ![Pierwsze działanie Lookup — nazwa](./media/tutorial-incremental-copy-multiple-tables-portal/first-lookup-name.png)
1. Przejdź do karty **Ustawienia** w oknie **Właściwości** i wykonaj następujące czynności: 

    1. Wybierz pozycję **WatermarkDataset** w polu **Zestaw danych będący źródłem**.
    1. Wybierz pozycję **Zapytanie** w polu **Użyj zapytania**. 
    1. W obszarze **Zapytanie** wprowadź następujące zapytanie SQL. 

        ```sql
        select * from watermarktable where TableName  =  '@{item().TABLE_NAME}'
        ```

        ![Pierwsze działanie Lookup — ustawienia](./media/tutorial-incremental-copy-multiple-tables-portal/first-lookup-settings.png)
1. Przeciągnij działanie **Lookup** z przybornika **Działania** i wprowadź wartość **LookupNewWaterMarkActivity** w polu **Nazwa**.
        
    ![Drugie działanie Lookup — nazwa](./media/tutorial-incremental-copy-multiple-tables-portal/second-lookup-name.png)
1. Przejdź do karty **Ustawienia**.

    1. Wybierz pozycję **SourceDataset** w obszarze **Zestaw danych będący źródłem**. 
    1. Wybierz pozycję **Zapytanie** w polu **Użyj zapytania**.
    1. W obszarze **Zapytanie** wprowadź następujące zapytanie SQL.

        ```sql    
        select MAX(@{item().WaterMark_Column}) as NewWatermarkvalue from @{item().TABLE_NAME}
        ```
    
        ![Drugie działanie Lookup — ustawienia](./media/tutorial-incremental-copy-multiple-tables-portal/second-lookup-settings.png)
1. Przeciągnij działanie **Copy** (Kopiuj) z przybornika **Działania** i wprowadź wartość **IncrementalCopyActivity** w polu **Nazwa**. 

    ![Działanie Copy (Kopiowanie) — nazwa](./media/tutorial-incremental-copy-multiple-tables-portal/copy-activity-name.png)
1. Kolejno połącz działania **Lookup** z działaniem **Copy**. Aby utworzyć połączenie, zacznij przeciąganie w **zielonym** polu połączonym z działaniem **Lookup** i upuść je na działaniu **Copy**. Po zmianie koloru obramowania działania Copy na **niebieski** zwolnij przycisk myszy.

    ![Łączenie działań Lookup z działaniem Copy](./media/tutorial-incremental-copy-multiple-tables-portal/connect-lookup-to-copy.png)
1. Wybierz działanie **Copy** w potoku. Przejdź do karty **Źródło** w oknie **Właściwości**. 

    1. Wybierz pozycję **SourceDataset** w obszarze **Zestaw danych będący źródłem**. 
    1. Wybierz pozycję **Zapytanie** w polu **Użyj zapytania**. 
    1. W obszarze **Zapytanie** wprowadź następujące zapytanie SQL.

        ```sql
        select * from @{item().TABLE_NAME} where @{item().WaterMark_Column} > '@{activity('LookupOldWaterMarkActivity').output.firstRow.WatermarkValue}' and @{item().WaterMark_Column} <= '@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}'        
        ```

        ![Działanie Copy (Kopiowanie) — ustawienia źródła](./media/tutorial-incremental-copy-multiple-tables-portal/copy-source-settings.png)
1. Przejdź do karty **Ujście** i wybierz pozycję **SinkDataset** w polu **Zestaw danych będący ujściem**. 
        
    ![Działanie Copy (Kopiowanie) — ustawienia ujścia](./media/tutorial-incremental-copy-multiple-tables-portal/copy-sink-settings.png)
1. Wykonaj następujące czynności:

    1. W **Dataset** właściwości dla **SinkTableName** parametrów, wprowadź `@{item().TABLE_NAME}`.
    1. Aby uzyskać **Nazwa procedury składowanej** właściwości wprowadź `@{item().StoredProcedureNameForMergeOperation}`.
    1. Aby uzyskać **typ tabeli** właściwości wprowadź `@{item().TableType}`.


        ![Działanie Copy — parametry](./media/tutorial-incremental-copy-multiple-tables-portal/copy-activity-parameters.png)
1. Przeciągnij działanie **Stored Procedure** (Procedura składowana) z przybornika **Działania** do powierzchni projektanta potoku. Połącz działanie **Copy** z działaniem **Stored Procedure**. 

    ![Działanie Copy — parametry](./media/tutorial-incremental-copy-multiple-tables-portal/connect-copy-to-sproc.png)
1. Wybierz działanie **Stored Procedure** w potoku, a następnie wprowadź wartość **StoredProceduretoWriteWatermarkActivity** w polu **Nazwa** na karcie **Ogólne** w oknie **Właściwości**. 

    ![Działanie procedury składowanej — nazwa](./media/tutorial-incremental-copy-multiple-tables-portal/sproc-activity-name.png)
1. Przejdź do karty **Konto SQL** i wybierz wartość **AzureSqlDatabaseLinkedService** w polu **Połączona usługa**.

    ![Działanie Stored Procedure (Procedura składowana) — konto SQL](./media/tutorial-incremental-copy-multiple-tables-portal/sproc-activity-sql-account.png)
1. Przejdź do karty **Procedura składowana** i wykonaj następujące czynności:

    1. W polu **Nazwa procedury składowanej** wybierz wartość `usp_write_watermark`. 
    1. Wybierz pozycję **Importuj parametr**. 
    1. Określ wartości następujących parametrów: 

        | Name (Nazwa) | Typ | Wartość | 
        | ---- | ---- | ----- |
        | LastModifiedtime | DateTime | `@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}` |
        | TableName | String | `@{activity('LookupOldWaterMarkActivity').output.firstRow.TableName}` |
    
        ![Działanie procedury składowanej — ustawienia procedury składowanej](./media/tutorial-incremental-copy-multiple-tables-portal/sproc-activity-sproc-settings.png)
1. W okienku po lewej stronie kliknij pozycję **Opublikuj**. Ta czynność spowoduje opublikowanie utworzonych jednostek w usłudze Data Factory. 

    ![Przycisk Opublikuj](./media/tutorial-incremental-copy-multiple-tables-portal/publish-button.png)
1. Poczekaj na wyświetlenie komunikatu **Pomyślnie opublikowano**. Aby wyświetlić powiadomienia, kliknij link **Pokaż powiadomienia**. Zamknij okno powiadomień, klikając przycisk **X**.

    ![Wyświetlanie powiadomień](./media/tutorial-incremental-copy-multiple-tables-portal/notifications.png)

 
## <a name="run-the-pipeline"></a>Uruchamianie potoku

1. Na pasku narzędzi dla potoku kliknij pozycję **Wyzwól**, a następnie kliknij pozycję **Wyzwól teraz**.     

    ![Wyzwól teraz](./media/tutorial-incremental-copy-multiple-tables-portal/trigger-now.png)
1. W oknie **Uruchamianie potoku** wprowadź następującą wartość dla parametru **tableList**, a następnie kliknij pozycję **Zakończ**. 

    ```
    [
        {
            "TABLE_NAME": "customer_table",
            "WaterMark_Column": "LastModifytime",
            "TableType": "DataTypeforCustomerTable",
            "StoredProcedureNameForMergeOperation": "usp_upsert_customer_table"
        },
        {
            "TABLE_NAME": "project_table",
            "WaterMark_Column": "Creationtime",
            "TableType": "DataTypeforProjectTable",
            "StoredProcedureNameForMergeOperation": "usp_upsert_project_table"
        }
    ]
    ```

    ![Argumenty uruchomienia potoku](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-run-arguments.png)

## <a name="monitor-the-pipeline"></a>Monitorowanie potoku

1. Przejdź do karty **Monitorowanie** po lewej stronie. Zostanie wyświetlone uruchomienie potoku, które zostało wyzwolone za pomocą **wyzwalacza ręcznego**. Kliknij przycisk **Odśwież**, aby odświeżyć listę. Linki w kolumnie **Działania** umożliwiają wyświetlanie uruchomień działań skojarzonych z uruchomieniem potoku oraz ponowne uruchamianie potoku. 

    ![Uruchomienia potoków](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-runs.png)
1. Kliknij link **Wyświetl uruchomienia działania** w kolumnie **Akcje**. Zostaną wyświetlone wszystkie uruchomienia działania skojarzone z wybranym uruchomieniem potoku. 

    ![Uruchomienia działania](./media/tutorial-incremental-copy-multiple-tables-portal/activity-runs.png)

## <a name="review-the-results"></a>Sprawdzanie wyników
W programu SQL Server Management Studio uruchom następujące zapytania względem docelowej bazy danych Azure SQL Database, aby sprawdzić, czy dane zostały skopiowane z tabel źródłowych do tabel docelowych: 

**Zapytanie** 
```sql
select * from customer_table
```

**Dane wyjściowe**
```
===========================================
PersonID    Name    LastModifytime
===========================================
1           John    2017-09-01 00:56:00.000
2           Mike    2017-09-02 05:23:00.000
3           Alice   2017-09-03 02:36:00.000
4           Andy    2017-09-04 03:21:00.000
5           Anny    2017-09-05 08:06:00.000
```

**Zapytanie**

```sql
select * from project_table
```

**Dane wyjściowe**

```
===================================
Project     Creationtime
===================================
project1    2015-01-01 00:00:00.000
project2    2016-02-02 01:23:00.000
project3    2017-03-04 05:16:00.000
```

**Zapytanie**

```sql
select * from watermarktable
```

**Dane wyjściowe**

```
======================================
TableName       WatermarkValue
======================================
customer_table  2017-09-05 08:06:00.000
project_table   2017-03-04 05:16:00.000
```

Należy zauważyć, że wartości limitu dla obu tabel zostały zaktualizowane. 

## <a name="add-more-data-to-the-source-tables"></a>Dodawanie większej ilości danych do tabel źródłowych

Uruchom następujące zapytanie względem źródłowej bazy danych programu SQL Server, aby zaktualizować istniejący wiersz w tabeli customer_table. Wstaw nowy wiersz do tabeli project_table. 

```sql
UPDATE customer_table
SET [LastModifytime] = '2017-09-08T00:00:00Z', [name]='NewName' where [PersonID] = 3

INSERT INTO project_table
(Project, Creationtime)
VALUES
('NewProject','10/1/2017 0:00:00 AM');
``` 

## <a name="rerun-the-pipeline"></a>Ponowne uruchamianie potoku
1. W oknie przeglądarki sieci Web przejdź do karty **Edycja** po lewej stronie. 
1. Na pasku narzędzi dla potoku kliknij pozycję **Wyzwól**, a następnie kliknij pozycję **Wyzwól teraz**.   

    ![Wyzwól teraz](./media/tutorial-incremental-copy-multiple-tables-portal/trigger-now.png)
1. W oknie **Uruchamianie potoku** wprowadź następującą wartość dla parametru **tableList**, a następnie kliknij pozycję **Zakończ**. 

    ```
    [
        {
            "TABLE_NAME": "customer_table",
            "WaterMark_Column": "LastModifytime",
            "TableType": "DataTypeforCustomerTable",
            "StoredProcedureNameForMergeOperation": "usp_upsert_customer_table"
        },
        {
            "TABLE_NAME": "project_table",
            "WaterMark_Column": "Creationtime",
            "TableType": "DataTypeforProjectTable",
            "StoredProcedureNameForMergeOperation": "usp_upsert_project_table"
        }
    ]
    ```

## <a name="monitor-the-pipeline-again"></a>Ponowne monitorowanie potoku

1. Przejdź do karty **Monitorowanie** po lewej stronie. Zostanie wyświetlone uruchomienie potoku, które zostało wyzwolone za pomocą **wyzwalacza ręcznego**. Kliknij przycisk **Odśwież**, aby odświeżyć listę. Linki w kolumnie **Działania** umożliwiają wyświetlanie uruchomień działań skojarzonych z uruchomieniem potoku oraz ponowne uruchamianie potoku. 

    ![Uruchomienia potoków](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-runs.png)
1. Kliknij link **Wyświetl uruchomienia działania** w kolumnie **Akcje**. Zostaną wyświetlone wszystkie uruchomienia działania skojarzone z wybranym uruchomieniem potoku. 

    ![Uruchomienia działania](./media/tutorial-incremental-copy-multiple-tables-portal/activity-runs.png) 

## <a name="review-the-final-results"></a>Przegląd wyników końcowych
W programie SQL Server Management Studio uruchom następujące zapytania względem docelowej bazy danych, aby sprawdzić, czy zaktualizowane lub nowe dane zostały skopiowane z tabel źródłowych do tabel docelowych. 

**Zapytanie** 
```sql
select * from customer_table
```

**Dane wyjściowe**
```
===========================================
PersonID    Name    LastModifytime
===========================================
1           John    2017-09-01 00:56:00.000
2           Mike    2017-09-02 05:23:00.000
3           NewName 2017-09-08 00:00:00.000
4           Andy    2017-09-04 03:21:00.000
5           Anny    2017-09-05 08:06:00.000
```

Zwróć uwagę na nowe wartości właściwości **Name** i **LastModifytime** dla identyfikatora **PersonID** numeru 3. 

**Zapytanie**

```sql
select * from project_table
```

**Dane wyjściowe**

```
===================================
Project     Creationtime
===================================
project1    2015-01-01 00:00:00.000
project2    2016-02-02 01:23:00.000
project3    2017-03-04 05:16:00.000
NewProject  2017-10-01 00:00:00.000
```

Zwróć uwagę, że do tabeli project_table dodano pozycję **NewProject**. 

**Zapytanie**

```sql
select * from watermarktable
```

**Dane wyjściowe**

```
======================================
TableName       WatermarkValue
======================================
customer_table  2017-09-08 00:00:00.000
project_table   2017-10-01 00:00:00.000
```

Należy zauważyć, że wartości limitu dla obu tabel zostały zaktualizowane.
     
## <a name="next-steps"></a>Kolejne kroki
W ramach tego samouczka wykonano następujące procedury: 

> [!div class="checklist"]
> * Przygotowanie źródłowych i docelowych magazynów danych
> * Tworzenie fabryki danych.
> * Tworzenie własnego środowiska Integration Runtime.
> * Instalowanie środowiska Integration Runtime.
> * Tworzenie połączonych usług. 
> * Tworzenie zestawów danych źródła, ujścia i limitu.
> * Tworzenie, uruchamianie i monitorowanie potoku
> * Przejrzyj wyniki.
> * Dodawanie lub aktualizowanie danych w tabelach źródłowych.
> * Ponowne uruchamianie i monitorowanie potoku.
> * Przegląd wyników końcowych.

Przejdź do następującego samouczka, aby dowiedzieć się więcej o przekształcaniu danych za pomocą klastra Spark na platformie Azure:

> [!div class="nextstepaction"]
>[Przyrostowe ładowanie danych z bazy danych Azure SQL Database do magazynu Azure Blob Storage z użyciem technologii Change Tracking](tutorial-incremental-copy-change-tracking-feature-portal.md)


