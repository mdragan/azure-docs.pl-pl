---
title: Łączenie do usługi Azure Database for PostgreSQL — pojedynczy serwer za pomocą języka Ruby
description: Ten przewodnik Szybki Start zawiera przykładowy kod języka Ruby, którego można użyć do nawiązywania połączeń i wykonywanie zapytań o dane z usługi Azure Database for PostgreSQL — pojedynczy serwer.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 5/6/2019
ms.openlocfilehash: 242f5724e81f9b8ac6177e5c9d3844b3fab9a7f2
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067278"
---
# <a name="azure-database-for-postgresql---single-server-use-ruby-to-connect-and-query-data"></a>Azure Database for PostgreSQL — pojedynczy serwer: używanie języka Ruby do nawiązywania połączeń i wykonywania zapytań o dane
Ten przewodnik Szybki start przedstawia sposób nawiązywania połączeń z usługą Azure Database for PostgreSQL przy użyciu aplikacji języka [Ruby](https://www.ruby-lang.org). Pokazano w nim, jak używać instrukcji języka SQL w celu wysyłania zapytań o dane oraz wstawiania, aktualizowania i usuwania danych w bazie danych. W krokach w tym artykule założono, że wiesz już, jak programować za pomocą języka Ruby, i dopiero zaczynasz pracę z usługą Azure Database for PostgreSQL.

## <a name="prerequisites"></a>Wymagania wstępne
Ten przewodnik Szybki start jako punktu wyjścia używa zasobów utworzonych w jednym z tych przewodników:
- [Tworzenie bazy danych — portal](quickstart-create-server-database-portal.md)
- [Tworzenie bazy danych — interfejs wiersza polecenia platformy Azure](quickstart-create-server-database-azure-cli.md)

Należy również mieć zainstalowany:
- [Ruby](https://www.ruby-lang.org/en/downloads/)
- Pg języka Ruby, moduł PostgreSQL dla języka Ruby

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu
Uzyskaj parametry połączenia potrzebne do nawiązania połączenia z usługą Azure Database for PostgreSQL. Potrzebna jest w pełni kwalifikowana nazwa serwera i poświadczenia logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. W menu po lewej stronie w witrynie Azure Portal kliknij pozycję **Wszystkie zasoby** i wyszukaj utworzony serwer, taki jak **mydemoserver**.
3. Kliknij nazwę serwera.
4. Po przejściu do panelu **Przegląd** serwera zanotuj **nazwę serwera** i **nazwę logowania administratora serwera**. Jeśli zapomnisz hasła, możesz również je zresetować z poziomu tego panelu.
 ![Nazwa serwera usługi Azure Database for PostgreSQL](./media/connect-ruby/1-connection-string.png)

> [!NOTE]
> `@` Symbol nazwę Azure Postgres został zakodowany jako adres url `%40` w parametrach połączenia. 

## <a name="connect-and-create-a-table"></a>Łączenie i tworzenie tabeli
Użyj poniższego kodu w celu nawiązania połączenia i utworzenia tabeli za pomocą instrukcji **CREATE TABLE** języka SQL, a następnie instrukcji **INSERT INTO** języka SQL, aby dodać wiersze do tabeli.

Kod używa obiektu [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection) z konstruktorem [new()](https://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) w celu nawiązania połączenia z usługą Azure Database for PostgreSQL. Następnie kod wywołuje metodę [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) w celu uruchomienia poleceń DROP, CREATE TABLE i INSERT INTO. Kod wyszukuje błędy za pomocą klasy [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error). W kolejnym kroku kod wywołuje metodę [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) w celu zamknięcia połączenia przed zakończeniem.

Zastąp ciągi `host`, `database`, `user` i `password` własnymi wartościami. 


```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin%40mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database'

    # Drop previous table of same name if one exists
    connection.exec('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    connection.exec('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    connection.exec("INSERT INTO inventory VALUES(1, 'banana', 150)")
    connection.exec("INSERT INTO inventory VALUES(2, 'orange', 154)")
    connection.exec("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="read-data"></a>Odczyt danych
Użyj poniższego kodu, aby nawiązać połączenie i odczytać dane za pomocą instrukcji **SELECT** języka SQL. 

Kod używa obiektu [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection) z konstruktorem [new()](https://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) w celu nawiązania połączenia z usługą Azure Database for PostgreSQL. Następnie kod wywołuje metodę [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) w celu uruchomienia polecenia SELECT z zachowaniem wyników w zestawie wyników. Kolekcja zestawów wyników jest iterowana przy użyciu pętli `resultSet.each do` z zachowaniem bieżących wartości wierszy w zmiennej `row`. Kod wyszukuje błędy za pomocą klasy [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error). W kolejnym kroku kod wywołuje metodę [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) w celu zamknięcia połączenia przed zakończeniem.

Zastąp ciągi `host`, `database`, `user` i `password` własnymi wartościami. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin%40mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :database => dbname, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    resultSet = connection.exec('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="update-data"></a>Aktualizowanie danych
Użyj poniższego kodu, aby nawiązać połączenie i zaktualizować dane za pomocą instrukcji **UPDATE** języka SQL.

Kod używa obiektu [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection) z konstruktorem [new()](https://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) w celu nawiązania połączenia z usługą Azure Database for PostgreSQL. Następnie kod wywołuje metodę [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) w celu uruchomienia polecenia UPDATE. Kod wyszukuje błędy za pomocą klasy [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error). W kolejnym kroku kod wywołuje metodę [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) w celu zamknięcia połączenia przed zakończeniem.

Zastąp ciągi `host`, `database`, `user` i `password` własnymi wartościami. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin%40mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
    puts 'Updated 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```


## <a name="delete-data"></a>Usuwanie danych
Użyj poniższego kodu, aby nawiązać połączenie i odczytać dane za pomocą instrukcji **DELETE** języka SQL. 

Kod używa obiektu [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection) z konstruktorem [new()](https://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) w celu nawiązania połączenia z usługą Azure Database for PostgreSQL. Następnie kod wywołuje metodę [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) w celu uruchomienia polecenia UPDATE. Kod wyszukuje błędy za pomocą klasy [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error). W kolejnym kroku kod wywołuje metodę [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) w celu zamknięcia połączenia przed zakończeniem.

Zastąp ciągi `host`, `database`, `user` i `password` własnymi wartościami. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin%40mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="next-steps"></a>Kolejne kroki
> [!div class="nextstepaction"]
> [Migrowanie bazy danych przy użyciu funkcji eksportowania i importowania](./howto-migrate-using-export-and-import.md)
