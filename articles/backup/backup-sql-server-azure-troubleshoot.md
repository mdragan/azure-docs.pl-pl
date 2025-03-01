---
title: Rozwiązywanie problemów z kopii zapasowej bazy danych programu SQL Server w usłudze Azure Backup | Dokumentacja firmy Microsoft
description: Informacje dotyczące rozwiązywania problemów do wykonywania kopii zapasowych baz danych SQL Server uruchomiony na maszynach wirtualnych Azure z usługą Azure Backup.
services: backup
author: anuragm
manager: shivamg
ms.service: backup
ms.topic: article
ms.date: 06/18/2019
ms.author: anuragm
ms.openlocfilehash: 9ed30a35f30d1b6b9fdcd43110ed93618a10dbc3
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204188"
---
# <a name="troubleshoot-back-up-sql-server-on-azure"></a>Rozwiązywanie problemów z kopii zapasowych programu SQL Server na platformie Azure

Ten artykuł zawiera informacje dotyczące rozwiązywania problemów w celu ochrony maszyn wirtualnych serwera SQL na platformie Azure.

## <a name="feature-consideration-and-limitations"></a>Funkcja uwag i ograniczeń

Aby wyświetlić uwagę funkcji, zapoznaj się z artykułem [kopii zapasowych programu SQL Server, na maszynach wirtualnych Azure](backup-azure-sql-database.md#feature-consideration-and-limitations).

## <a name="sql-server-permissions"></a>Uprawnienia programu SQL Server

Można skonfigurować ochrony dla bazy danych programu SQL Server na maszynie wirtualnej **AzureBackupWindowsWorkload** rozszerzenia musi być zainstalowany na tej maszynie wirtualnej. Jeśli wystąpi błąd, **UserErrorSQLNoSysadminMembership**, oznacza to wystąpienie programu SQL nie ma wymaganych uprawnień do tworzenia kopii zapasowej. Aby naprawić ten błąd, wykonaj kroki opisane w [ustawić uprawnienia dla maszyn wirtualnych SQL spoza witryny marketplace](backup-azure-sql-database.md#set-vm-permissions).

## <a name="backup-type-unsupported"></a>Nieobsługiwany typ kopii zapasowej

| Severity | Opis | Możliwe przyczyny | Zalecana akcja |
|---|---|---|---|
| Ostrzeżenie | Bieżące ustawienia dla tej bazy danych nie obsługują niektórych rodzajów typów kopii zapasowych, które są obecne w skojarzonych zasad. | <li>**Bazy danych Master**: Tylko operacja tworzenia kopii zapasowej pełnej bazy danych może zostać wykonana w bazie danych master; ani **różnicowej** kopii zapasowych ani transakcji **dzienniki** możliwa jest kopia zapasowa. </li> <li>Wszystkie bazy danych w **model odzyskiwania prostego** nie zezwala na transakcji **dzienniki** kopii zapasowej należy podjąć.</li> | Zmodyfikuj ustawienia bazy danych w taki sposób, że wszystkie typy kopii zapasowych w ramach zasad są obsługiwane. Alternatywnie Zmień bieżące zasady w celu uwzględnienia tylko obsługiwane typy kopii zapasowych. W przeciwnym razie nieobsługiwane typy kopii zapasowych zostaną pominięte podczas zaplanowanej kopii zapasowej lub kopii zapasowej ad-hoc zadania tworzenia kopii zapasowej zakończy się niepowodzeniem.


## <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Ta baza danych SQL nie obsługuje żądanego typu kopii zapasowej. | Występuje, gdy model odzyskiwania bazy danych nie zezwalaj na kopie zapasowe żądanego typu. Ten błąd może się zdarzyć w następujących sytuacjach: <br/><ul><li>Bazę danych przy użyciu modelu odzyskiwania prostego nie zezwala na tworzenie kopii zapasowej dziennika.</li><li>Dziennika i różnicowe kopie zapasowe nie są dozwolone dla głównego bazy danych.</li></ul>Aby uzyskać więcej informacji, zobacz [modeli odzyskiwania SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server) dokumentacji. | Jeśli kopia zapasowa dziennika bazy danych w modelu odzyskiwania prostego nie powiedzie się, spróbuj wykonać jedną z następujących opcji:<ul><li>Jeśli baza danych jest w trybie odzyskiwania prostego, należy wyłączyć kopie zapasowe dziennika.</li><li>Użyj [dokumentacji programu SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) zmiany modelu odzyskiwania bazy danych na wartość Full lub dziennikiem. </li><li> Jeśli nie chcesz zmienić model odzyskiwania, a masz standardowych zasad, aby utworzyć kopię zapasową wielu baz danych, których nie można zmienić, zignoruj ten błąd. Pełnych i różnicowych kopii zapasowych będą działać na harmonogram. Kopie zapasowe dziennika zostanie pominięty, której oczekuje się, w tym przypadku.</li></ul>Jeśli wzorzec bazy danych i skonfigurowano różnicowej lub dziennika kopii zapasowej, użyj dowolnej z następujących czynności:<ul><li>Użyj portalu Aby zmienić harmonogram zasad tworzenia kopii zapasowej dla serwera głównego bazy danych, pełne.</li><li>W przypadku standardowych zasad, aby utworzyć kopię zapasową wielu baz danych, których nie można zmienić, zignoruj ten błąd. Pełnej kopii zapasowej będzie działać na harmonogram. Różnicowa lub dziennika kopii zapasowych nie wystąpi, której oczekuje się, w tym przypadku.</li></ul> |
| Operacja anulowana, ponieważ operacja powodująca konflikt była już uruchomiona na tej samej bazy danych. | Zobacz [wpis w blogu o kopii zapasowej i przywracanie ograniczenia](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) , działające równocześnie.| [Aby monitorować zadania tworzenia kopii zapasowej, należy użyć programu SQL Server Management Studio (SSMS).](manage-monitor-sql-database-backup.md) Gdy operacja powodująca konflikt nie powiedzie się, uruchom ponownie operację.|

## <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Baza danych SQL nie istnieje. | Baza danych, został usunięty lub zmieniono jego nazwę. | Sprawdź, jeśli baza danych została przypadkowo usunięty lub zmieniono ich nazwy.<br/><br/> Jeśli przypadkowo usunięto bazy danych, aby kontynuować tworzenie kopii zapasowych, należy przywrócić bazę danych do oryginalnej lokalizacji.<br/><br/> Jeśli usunięte bazy danych i nie potrzebujesz przyszłych kopie zapasowe, a następnie w magazynie usługi Recovery Services klikaj [zatrzymywanie tworzenia kopii zapasowych "Usuń/Zachowaj dane"](manage-monitor-sql-database-backup.md).

## <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Łańcuch dzienników został przerwany. | Baza danych lub maszyna wirtualna kopia zapasowa jest tworzona przy użyciu innego rozwiązania tworzenia kopii zapasowych, które obcina łańcuch dziennika.|<ul><li>Sprawdź, czy innego rozwiązania tworzenia kopii zapasowych lub skryptu jest w użyciu. Jeśli tak, Zatrzymaj inne rozwiązania tworzenia kopii zapasowych. </li><li>Jeśli wykonano kopię zapasową kopii zapasowej dziennika ad-hoc, Wyzwól pełnej kopii zapasowej, aby rozpocząć nowy łańcuch dziennika. Dziennik zaplanowanego tworzenia kopii zapasowych ponieważ usługa Azure Backup automatycznie spowoduje wyzwolenie pełnej kopii zapasowej, aby rozwiązać ten problem jest wymagana żadna akcja.</li>|

## <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Usługa Azure Backup nie jest w stanie połączyć się z wystąpieniem serwera SQL. | Usługa Azure Backup nie może połączyć się z wystąpieniem programu SQL. | Użyj dodatkowe szczegóły w menu błędzie portalu platformy Azure, aby zawęzić głównych przyczyn. Zapoznaj się [rozwiązywania kopii zapasowej SQL](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) Aby naprawić błąd.<br/><ul><li>Jeśli domyślne ustawienia programu SQL nie zezwalają na połączenia zdalne, należy zmienić ustawienia. Zobacz następujące artykuły, aby uzyskać informacje na temat zmiany ustawień.<ul><li>[MSSQLSERVER_-1](/previous-versions/sql/sql-server-2016/bb326495(v=sql.130))</li><li>[MSSQLSERVER_2](/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[MSSQLSERVER_53](/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Jeśli występują problemy podczas logowania, użyj poniższych linków, aby rozwiązać ten problem:<ul><li>[MSSQLSERVER_18456](/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[MSSQLSERVER_18452](/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

## <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Pierwsza pełna kopia zapasowa brakuje dla tego źródła danych. | Pełna kopia zapasowa jest Brak bazy danych. Dziennik i różnicowe kopie zapasowe element nadrzędny do pełnej kopii zapasowej, dzięki czemu pełnych kopii zapasowych należy podjąć przed wyzwoleniem różnicowej lub kopie zapasowe dzienników. | Wyzwalanie ad-hoc pełnej kopii zapasowej.   |

## <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Nie można przyjąć kopii zapasowej, ponieważ dziennik transakcji dla źródła danych jest pełny. | Przestrzeń dziennika transakcji bazy danych jest pełny. | Aby rozwiązać ten problem, zapoznaj się [dokumentacji programu SQL](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |

## <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Baza danych o takiej samej nazwie już istnieje w lokalizacji docelowej | Przywracanie docelowej już bazę danych o takiej samej nazwie.  | <ul><li>Zmień nazwę docelowej bazy danych</li><li>Lub użyj opcji Zastąp życie na stronie przywracania</li> |

## <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Przywracanie nie powiodło się, ponieważ bazy danych nie można było przełączyć w tryb offline. | Podczas wykonywania przywracania, docelowa baza danych musi zostać przełączone do trybu offline. Usługa Azure Backup nie jest w stanie wyświetlić te dane w trybie offline. | Użyj dodatkowe szczegóły w menu błędzie portalu platformy Azure, aby zawęzić głównych przyczyn. Aby uzyskać więcej informacji, zobacz [dokumentacji programu SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |

##  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Nie można odnaleźć certyfikatu serwera z odciskiem palca w elemencie docelowym. | Wzorzec bazy danych w wystąpieniu docelowym nie ma odcisku palca szyfrowania prawidłowe. | Zaimportuj prawidłowy odcisk palca używane w wystąpieniu źródłowym, do obiektu docelowego. |

## <a name="usererrorrestorenotpossiblebecauselogbackupcontainsbulkloggedchanges"></a>UserErrorRestoreNotPossibleBecauseLogBackupContainsBulkLoggedChanges

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Kopia zapasowa dziennika używana do odzyskiwania zawiera zmiany zarejestrowane zbiorczo. Nie można jej użyć do zatrzymania w dowolnym punkcie w czasie, zgodnie z wytycznymi SQL. | Gdy baza danych jest w trybie odzyskiwania zarejestrowane zbiorczo, nie można odzyskać danych między niepełnym dziennikiem transakcji i dalej dziennika transakcji. | Wybierz inny punkt w czasie do odzyskania. [Dowiedz się więcej](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms186229(v=sql.105))


## <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Nie można działać zgodnie z preferencjami kopii zapasowych dla zawsze włączonej grupy dostępności SQL, ponieważ niektóre węzły grupy dostępności nie zostały zarejestrowane. | Węzły wymagane do wykonywania kopii zapasowych nie są zarejestrowane lub są niedostępne. | <ul><li>Upewnij się, że wszystkie węzły, które są wymagane do wykonywania kopii zapasowych tej bazy danych są zarejestrowane i dobrej kondycji, a następnie spróbuj ponownie wykonać operację.</li><li>Zmiana zawsze włączonej grupy dostępności SQL preferencję tworzenia kopii zapasowych.</li></ul> |

## <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Program SQL server VM jest zamknięty lub niedostępny dla usługi Azure Backup. | Maszyna wirtualna jest zamknięta. | Upewnij się, że serwer SQL jest uruchomiony. |

## <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Usługa Azure Backup używa agenta gościa maszyny Wirtualnej platformy Azure tworzy kopie zapasowe, ale agent gościa nie jest dostępne na serwerze docelowym. | Agent gościa nie jest włączona lub jest w złej kondycji | [Zainstaluj agenta gościa maszyny Wirtualnej](../virtual-machines/extensions/agent-windows.md) ręcznie. |

## <a name="autoprotectioncancelledornotvalid"></a>AutoProtectionCancelledOrNotValid

| Komunikat o błędzie | Możliwe przyczyny | Zalecana akcja |
|---|---|---|
| Automatycznej ochrony został usunięty lub jest już prawidłowy. | Po włączeniu automatycznej ochrony wystąpienia SQL **konfigurowania kopii zapasowej** zadania są uruchamiane dla wszystkich baz danych w tym wystąpieniu. Po wyłączeniu automatycznej ochrony podczas wykonywania zadań, a następnie **w toku** są anulowane zadania przy użyciu tego kodu błędu. | Włącz automatyczną ochronę ponownie chronić wszystkie pozostałe bazy danych. |

## <a name="re-registration-failures"></a>Błędy ponownej rejestracji

Sprawdzaj co najmniej jeden z [objawy](#symptoms) przed wyzwoleniem operacji ponownej rejestracji.

### <a name="symptoms"></a>Objawy

* Wszystkie operacje takie jak Kopia zapasowa, Przywróć i skonfiguruj kopię zapasową kończą się niepowodzeniem na maszynie Wirtualnej przy użyciu jednego z następujących kody błędów: **WorkloadExtensionNotReachable**, **UserErrorWorkloadExtensionNotInstalled**, **WorkloadExtensionNotPresent**, **WorkloadExtensionDidntDequeueMsg**
* **Stan kopii zapasowej** przeznaczonego na kopie zapasowe są wyświetlane elementu **nieosiągalny**. Chociaż należy wykluczyć wszystkie powody, które mogą również wynikać w tym samym stanie:

  * Brak uprawnień do wykonania kopii zapasowej powiązanych operacji na maszynie Wirtualnej  
  * Maszyna wirtualna została zamknięta z powodu którego kopii zapasowych nie może mieć miejsce
  * Problemy z siecią  

    ![Zarejestruj ponownie maszyny Wirtualnej](./media/backup-azure-sql-database/re-register-vm.png)

* W przypadku, gdy z zawsze włączona grupa dostępności kopii zapasowych kończy się niepowodzeniem po zmianie preferencję tworzenia kopii zapasowych, lub podczas pracy w trybie failover

### <a name="causes"></a>Przyczyny
Tych objawów mogą wystąpić z powodu co najmniej jeden z następujących powodów:

  * Rozszerzenie został usunięty lub odinstalowany z portalu 
  * Rozszerzenie została odinstalowana z **Panelu sterowania** maszyny wirtualnej w obszarze **Odinstaluj lub zmień Program** interfejsu użytkownika
  * Maszyna wirtualna została przywrócona w czasie przy użyciu funkcji przywracania dysków w miejscu
  * Maszyny Wirtualnej zostało zamknięte przez dłuższy czas z powodu której konfiguracji rozszerzenia na nim wygasł
  * Maszyna wirtualna została usunięta, a inną maszynę Wirtualną utworzono o takiej samej nazwie i w tej samej grupie zasobów usuniętej maszyny Wirtualnej
  * Jednym z węzłów AG kompletna konfiguracja kopii zapasowej nie nadeszła, może się to zdarzyć, zarówno w czasie rejestracji grupy dostępności w magazynie lub po dodaniu nowego węzła  <br>
    W powyższych scenariuszach zalecane jest aby wyzwolić ponowne zarejestrowanie operację na maszynie Wirtualnej. Ta opcja jest tylko dostępne za pośrednictwem programów PowerShell i będzie wkrótce dostępna w witrynie Azure portal.

## <a name="files-size-limit-beyond-which-restore-happens-to-default-path"></a>Limit rozmiaru pliku poza przywracania, która stanie się domyślną ścieżkę

Ciąg łączny rozmiar plików nie tylko zależy od liczby plików, ale także na ich nazwy i ścieżki. Dla każdego z plików bazy danych Pobierz logicznej nazwy pliku i ścieżkę fizyczną.
Można użyć zapytania SQL podane poniżej:

  ```
  SELECT mf.name AS LogicalName, Physical_Name AS Location FROM sys.master_files mf
                 INNER JOIN sys.databases db ON db.database_id = mf.database_id
                 WHERE db.name = N'<Database Name>'"
 ```

Teraz rozmieszczanie ich w formacie podane poniżej:

  ```[{"path":"<Location>","logicalName":"<LogicalName>","isDir":false},{"path":"<Location>","logicalName":"<LogicalName>","isDir":false}]}
  ```

Przykład:

  ```[{"path":"F:\\Data\\TestDB12.mdf","logicalName":"TestDB12","isDir":false},{"path":"F:\\Log\\TestDB12_log.ldf","logicalName":"TestDB12_log","isDir":false}]}
  ```

Jeśli rozmiar ciągu w treści podane powyżej przekracza 20 000 bajtów, następnie pliki bazy danych są przechowywane inaczej, a podczas odzyskiwania nie można ustawić ścieżkę pliku docelowego dla przywracania. Pliki zostaną przywrócone do ścieżki domyślnej SQL dostępnych w programie SQL Server.

### <a name="override-the-default-target-restore-file-path"></a>Zastąp domyślna ścieżka pliku docelowego przywracania

Ścieżka pliku docelowego przywracania można zastąpić podczas operacji przywracania, umieszczając plik w formacie JSON, który zawiera mapowania pliku bazy danych do docelowej przywracania ścieżki. W tym tworzenie pliku `database_name.json` i umieść go w lokalizacji *C:\Program Files\Azure obciążenia Backup\bin\plugins\SQL*.

Zawartość pliku powinna mieć format podanych poniżej:
  ```[
    {
      "Path": "<Restore_Path>",
      "LogicalName": "<LogicalName>",
      "IsDir": "false"
    },
    {
      "Path": "<Restore_Path>",
      "LogicalName": "LogicalName",
      "IsDir": "false"
    },  
  ]
  ```

Przykład:

  ```[
      {
        "Path": "F:\\Data\\testdb2_1546408741449456.mdf",
        "LogicalName": "testdb7",
       "IsDir": "false"
      },
      {
        "Path": "F:\\Log\\testdb2_log_1546408741449456.ldf",
        "LogicalName": "testdb7_log",
        "IsDir": "false"
      },  
    ]
  ```

 
W powyższej zawartości można uzyskać nazwę logicznego pliku bazy danych przy użyciu zapytań SQL podane poniżej:

```
SELECT mf.name AS LogicalName FROM sys.master_files mf
                INNER JOIN sys.databases db ON db.database_id = mf.database_id
                WHERE db.name = N'<Database Name>'"
  ```


Ten plik powinien być umieszczony przed wyzwoleniem operacji przywracania.

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat usługi Azure Backup dla maszyn wirtualnych programu SQL Server (publiczna wersja zapoznawcza), zobacz [usługi Azure Backup dla maszyn wirtualnych SQL (publiczna wersja zapoznawcza)](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).
