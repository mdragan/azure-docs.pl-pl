---
title: 'Szybki start: Tworzenie i wysyłanie zapytań usługi Azure SQL Data Warehouse — Azure portal | Dokumentacja firmy Microsoft'
description: Tworzenie i wysyłanie zapytań hurtowni danych przy użyciu usługi Azure SQL Data Warehouse w witrynie Azure portal.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: development
ms.date: 05/28/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: sqlfreshmay19
ms.openlocfilehash: 9072caf29be0ebf47207266b7313e989034c3a18
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428044"
---
# <a name="quickstart-create-and-query-an-azure-sql-data-warehouse-in-the-azure-portal"></a>Szybki start: Tworzenie magazynu danych Azure SQL Data Warehouse w witrynie Azure Portal i wykonywanie zapytania skierowanego do niego

Szybkie tworzenie i wykonywanie zapytań usługi Azure SQL data warehouse przy użyciu witryny Azure portal.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

> [!NOTE]
> Utworzenie bazy danych w usłudze SQL Data Warehouse może skutkować powstaniem nowej usługi płatnej. Aby uzyskać więcej informacji, zobacz [Cennik usługi SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Pobierz i zainstaluj najnowszą wersję programu [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

## <a name="create-a-data-warehouse"></a>Tworzenie magazynu danych

Magazyn danych Azure SQL Data Warehouse jest tworzony ze zdefiniowanym zestawem [zasobów obliczeniowych](memory-and-concurrency-limits.md). Baza danych jest tworzona w [grupie zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) oraz na [serwerze logicznym SQL platformy Azure](../sql-database/sql-database-logical-servers.md). 

Wykonaj następujące kroki, aby utworzyć magazyn danych SQL Data Warehouse zawierający przykładowe dane AdventureWorksDW. 

1. W lewym górnym rogu witryny Azure Portal kliknij przycisk **Utwórz zasób**.

2. Na stronie **Nowy** wybierz pozycję **Bazy danych**, a następnie na stronie **Nowy** w obszarze **Polecane** wybierz pozycję **SQL Data Warehouse**.

    ![tworzenie pustego magazynu danych](media/load-data-from-azure-blob-storage-using-polybase/create-empty-data-warehouse.png)

3. Wypełnij formularz magazynu danych SQL Data Warehouse, używając następujących informacji:

    | Ustawienie | Sugerowana wartość | Opis |
    | :------ | :-------------- | :---------- |
    | **Nazwa bazy danych** | mySampleDataWarehouse | Prawidłowe nazwy baz danych opisano w artykule [Database Identifiers](/sql/relational-databases/databases/database-identifiers) (Identyfikatory baz danych). Pamiętaj, że magazyn danych to typ bazy danych.|
    | **Subskrypcja** | Twoja subskrypcja | Aby uzyskać szczegółowe informacje o subskrypcjach, zobacz [Subskrypcje](https://account.windowsazure.com/Subscriptions). |
    | **Grupa zasobów** | myResourceGroup | Prawidłowe nazwy grup zasobów opisano w artykule [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Reguły i ograniczenia nazewnictwa). |
    | **Wybierz źródło** | Sample | Wskazuje, aby załadować przykładową bazę danych. Pamiętaj, że magazyn danych jest jednym z typów bazy danych. |
    | **Wybierz przykład** | AdventureWorksDW | Wskazuje, aby załadować przykładową bazę danych AdventureWorksDW. |
    ||||

    ![tworzenie magazynu danych](media/create-data-warehouse-portal/select-sample.png)

4. Kliknij pozycję **Serwer**, aby utworzyć i skonfigurować nowy serwer dla nowej bazy danych. Wypełnij **formularz nowego serwera**, używając następujących informacji: 

    | Ustawienie | Sugerowana wartość | Opis |
    | :------ | :-------------- | :---------- |
    | **Nazwa serwera** | Dowolna nazwa unikatowa w skali globalnej | Prawidłowe nazwy serwera opisano w artykule [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Reguły i ograniczenia nazewnictwa). |
    | **Identyfikator logowania administratora serwera** | Dowolna prawidłowa nazwa | Prawidłowe nazwy identyfikatorów logowania opisano w artykule [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Identyfikatory baz danych).|
    | **Hasło** | Dowolne prawidłowe hasło | Hasło musi mieć co najmniej osiem znaków i musi zawierać znaki z trzech z następujących kategorii: wielkie litery, małe litery, cyfry i znaki inne niż alfanumeryczne. |
    | **Lokalizacja** | Dowolna prawidłowa lokalizacja | Aby uzyskać informacje na temat regionów, zobacz temat [Regiony platformy Azure](https://azure.microsoft.com/regions/). |
    ||||

    ![tworzenie serwera bazy danych](media/load-data-from-azure-blob-storage-using-polybase/create-database-server.png)

5. Kliknij pozycję **Wybierz**.

6. Kliknij przycisk **poziom wydajności** Aby określić konfigurację wydajności magazynu danych.

7. Na potrzeby tego samouczka wybierz **Gen2**. Suwak, domyślnie jest ustawiony na **DW1000c**. Spróbuj przesunąć go w górę i w dół, aby zobaczyć, jak działa. 

    ![konfigurowanie wydajności](media/load-data-from-azure-blob-storage-using-polybase/configure-performance.png)

8. Kliknij przycisk **Zastosuj**.

9. Teraz, że ukończono formularza SQL Data Warehouse, kliknij przycisk **Utwórz** do udostępniania bazy danych. Aprowizacja zajmuje kilka minut.

    ![kliknięcie przycisku utwórz](media/load-data-from-azure-blob-storage-using-polybase/click-create.png)

10. Na pasku narzędzi kliknij pozycję **Powiadomienia**, aby monitorować proces wdrażania.
    
     ![powiadomienie](media/load-data-from-azure-blob-storage-using-polybase/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Tworzenie reguły zapory na poziomie serwera

Usługa SQL Data Warehouse tworzy zaporę na poziomie serwera. Ta Zapora uniemożliwia zewnętrznym aplikacjom i narzędziom łączenie się z serwerem i wszelkimi bazami danych na serwerze. Aby umożliwić łączność, możesz dodać reguły zezwalające na połączenia dla konkretnych adresów IP. Wykonaj następujące kroki, aby utworzyć [regułę zapory na poziomie serwera](../sql-database/sql-database-firewall-configure.md) dla Twojego adresu IP klienta.

> [!NOTE]
> Usługa SQL Data Warehouse komunikuje się przez port 1433. Jeśli próbujesz nawiązać połączenie z sieci firmowej, ruch wychodzący na porcie 1433 może być blokowany przez zaporę sieciową. Jeśli nastąpi taka sytuacja, nie będzie można nawiązać połączenia z serwerem usługi Azure SQL Database, chyba że dział IT otworzy port 1433.

1. Po zakończeniu wdrożenia wybierz **wszystkich usług** z menu po lewej stronie. Wybierz **baz danych**, wybierz ikonę gwiazdki obok **magazyny danych SQL** dodać magazyny danych SQL do ulubionych.
1. Wybierz **magazyny danych SQL** z menu po lewej stronie, a następnie kliknij przycisk **mySampleDatabase** na **magazyny danych SQL** strony. Zostanie otwarta strona przeglądu bazy danych zawierająca w pełni kwalifikowaną nazwę serwera (takich jak **mynewserver-20180430.database.windows.net**) i opcje dalszej konfiguracji.
1. Skopiuj tę w pełni kwalifikowaną nazwę serwera do użycia nawiązać połączenie z serwerem i jego baz danych, w tym i innych samouczkach szybkiego startu. Aby otworzyć ustawienia serwera, kliknij nazwę serwera.

   ![znajdowanie nazwy serwera](media/load-data-from-azure-blob-storage-using-polybase/find-server-name.png)

1. Kliknij pozycję **Pokaż ustawienia zapory**.

   ![ustawienia serwera](media/load-data-from-azure-blob-storage-using-polybase/server-settings.png)

1. Zostanie otwarta strona **Ustawienia zapory** dla serwera usługi SQL Database.

   ![reguła zapory serwera](media/load-data-from-azure-blob-storage-using-polybase/server-firewall-rule.png)

1. Aby dodać bieżący adres IP do nowej reguły zapory, kliknij pozycję **Dodaj adres IP klienta** na pasku narzędzi. Reguła zapory może otworzyć port 1433 dla pojedynczego adresu IP lub zakresu adresów IP.

1. Kliknij pozycję **Zapisz**. Dla bieżącego adresu IP zostanie utworzona reguła zapory na poziomie serwera otwierająca port 1433 na serwerze logicznym.

1. Kliknij przycisk **OK**, a następnie zamknij stronę **Ustawienia zapory**.

Teraz możesz łączyć się z serwerem SQL i jego magazynami danych przy użyciu tego adresu IP. Połączenie działa z programu SQL Server Management Studio lub dowolnego innego narzędzia. Przy łączeniu się używaj wcześniej utworzonego konta administratora serwera.

> [!IMPORTANT]
> Domyślnie dostęp za pośrednictwem zapory usługi SQL Database jest włączony dla wszystkich usług platformy Azure. Kliknij pozycję **WYŁĄCZ** na tej stronie, a następnie kliknij przycisk **Zapisz**, aby wyłączyć zaporę dla wszystkich usług platformy Azure.

## <a name="get-the-fully-qualified-server-name"></a>Uzyskiwanie w pełni kwalifikowanej nazwy serwera

Uzyskaj w pełni kwalifikowaną nazwę serwera dla swojego serwera SQL w witrynie Azure Portal. Nazwa ta będzie używana później przy nawiązywaniu połączenia z serwerem.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. Wybierz **magazyny danych SQL** z menu po lewej stronie, a następnie kliknij pozycję Magazyn danych na **magazyny danych SQL** strony.
3. W okienku **Essentials** na stronie bazy danych w witrynie Azure Portal zlokalizuj i skopiuj **nazwę serwera**. W tym przykładzie w pełni kwalifikowana nazwa to mynewserver-20180430.database.windows.net.

    ![informacje o połączeniu](media/load-data-from-azure-blob-storage-using-polybase/find-server-name.png)

## <a name="connect-to-the-server-as-server-admin"></a>Nawiąż połączenie z serwerem jako administrator serwera

W tej sekcji używany jest program [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) do nawiązywania połączenia z serwerem SQL platformy Azure.

1. Otwórz program SQL Server Management Studio.

2. W oknie dialogowym **Połącz z serwerem** wprowadź następujące informacje:

   | Ustawienie | Sugerowana wartość | Opis |
   | :------ | :-------------- | :---------- |
   | Typ serwera | Aparat bazy danych | Ta wartość jest wymagana |
   | Nazwa serwera | W pełni kwalifikowana nazwa serwera | Oto przykład: **mynewserver-20180430.database.windows.net**. |
   | Authentication | Uwierzytelnianie programu SQL Server | Uwierzytelnianie SQL to jedyny typ uwierzytelniania skonfigurowany w tym samouczku. |
   | Login | Konto administratora serwera | Konto określone podczas tworzenia serwera. |
   | Hasło | Hasło konta administratora serwera | Hasło określone podczas tworzenia serwera. |
   ||||

    ![łączenie z serwerem](media/load-data-from-azure-blob-storage-using-polybase/connect-to-server.png)

3. Kliknij przycisk **Połącz**. W programie SSMS zostanie otwarte okno Eksplorator obiektów. 

4. W Eksploratorze obiektów rozwiń pozycję **Bazy danych**. Następnie rozwiń pozycję **mySampleDatabase**, aby wyświetlić obiekty w nowej bazy danych.

    ![obiekty bazy danych](media/create-data-warehouse-portal/connected.png) 

## <a name="run-some-queries"></a>Uruchamianie zapytań

Usługa SQL Data Warehouse używa T-SQL jako języka zapytań. Aby otworzyć okno zapytania i uruchomić kilka zapytań T-SQL, użyj następujących kroków:

1. Kliknij prawym przyciskiem myszy pozycję **mySampleDataWarehouse** i wybierz pozycję **Nowe zapytanie**. Otworzy się okno nowego zapytania.
2. W oknie zapytania wprowadź następujące polecenie, aby wyświetlić listę baz danych.

    ```sql
    SELECT * FROM sys.databases
    ```

3. Kliknij przycisk **Execute** (Wykonaj). W wynikach zapytania są widoczne dwie bazy danych: **master** i **mySampleDataWarehouse**.

    ![Wykonywanie zapytań kierowanych do baz danych](media/create-data-warehouse-portal/query-databases.png)

4. Aby zobaczyć trochę danych, wpisz następujące polecenie w celu wyświetlenia liczby klientów o nazwisku Adams, którzy mają trójkę dzieci. Wynikiem jest lista z sześcioma klientami. 

    ```sql
    SELECT LastName, FirstName FROM dbo.dimCustomer
    WHERE LastName = 'Adams' AND NumberChildrenAtHome = 3;
    ```

    ![Zapytanie dotyczące tabeli dbo.dimCustomer](media/create-data-warehouse-portal/query-customer.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Opłaty będą naliczane za jednostki magazynu danych i dane przechowywane w magazynie danych. Opłaty za te zasoby obliczeniowe i magazynowe są naliczane osobno.

- Jeśli chcesz przechowywać dane w magazynie, możesz wstrzymać obliczenia, gdy nie korzystasz z magazynu danych. Po wstrzymaniu obliczeń, są naliczane tylko za magazyn danych. Obliczenia możesz wstrzymać w każdym przypadku, gdy wszystko będzie gotowe do pracy z danymi.
- Aby uniknąć opłat w przyszłości, możesz usunąć magazyn danych.

Wykonaj następujące kroki, aby wyczyścić zasoby, które nie są już potrzebne.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) i kliknij swój magazyn danych.

    ![Oczyszczanie zasobów](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Aby wstrzymać obliczenia, kliknij przycisk **Wstrzymaj**. Jeśli magazyn danych jest wstrzymany, zobaczysz **Wznów** przycisku. Aby wznowić obliczenia, kliknij przycisk **wznowić**.

3. Aby usunąć magazyn danych, więc nie są naliczane opłaty za obliczenia oraz magazynowanie, kliknij przycisk **Usuń**.

4. Aby usunąć utworzony serwer SQL, kliknij **mynewserver-20180430.database.windows.net** w poprzednim obrazie, a następnie kliknij przycisk **Usuń**. Należy zachować ostrożność podczas usuwania, ponieważ usunięcie serwera spowoduje również usunięcie wszystkich baz danych przypisanych do tego serwera.

5. Aby usunąć grupę zasobów, kliknij pozycję **myResourceGroup**, a następnie kliknij pozycję **Usuń grupę zasobów**.

## <a name="next-steps"></a>Kolejne kroki

Teraz został utworzony magazyn danych, utworzyć regułę zapory na nawiązanie połączenia z magazynem danych i uruchomiono kilka zapytań. Aby dowiedzieć się więcej na temat usługi Azure SQL Data Warehouse, przejdź do samouczka na temat ładowania danych.

> [!div class="nextstepaction"]
> [Ładowanie danych do magazynu danych SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
