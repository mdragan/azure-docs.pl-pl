---
title: Tworzenie kopii zapasowych baz danych programu SQL Server na platformie Azure | Microsoft Docs
description: W tym samouczku opisano sposób tworzenia kopii zapasowych programu SQL Server na platformie Azure.
services: backup
author: dcurwin
manager: ''
ms.service: backup
ms.topic: tutorial
ms.date: 06/18/2019
ms.author: dacurwin
ms.openlocfilehash: 5fbbd2cf999ab8ba3183879bd9b417353aa5edd0
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203491"
---
# <a name="back-up-sql-server-databases-in-azure-vms"></a>Tworzenie kopii zapasowych baz danych programu SQL Server na maszynach wirtualnych platformy Azure



W tym artykule pokazano, jak utworzyć kopię zapasową bazy danych programu SQL Server uruchomionej na maszynie wirtualnej platformy Azure do magazynu usługi Azure Backup Recovery Services. W tym artykule omówiono sposób wykonywania następujących zadań:

> [!div class="checklist"]
> * Tworzenie i konfigurowanie magazynu.
> * Odnajdywanie baz danych i konfigurowanie kopii zapasowych.
> * Konfigurowanie ochrony automatycznej dla baz danych.
> * Uruchom kopię zapasową ad hoc.


## <a name="prerequisites"></a>Wymagania wstępne

Przed utworzeniem kopii zapasowej bazy danych programu SQL Server sprawdź następujące warunki:

1. Identyfikowanie lub [tworzenie](backup-sql-server-database-azure-vms.md#create-a-recovery-services-vault) magazyn usługi Recovery Services, w tym samym regionie lub ustawień regionalnych jako maszyny Wirtualnej obsługującego wystąpienie programu SQL Server.
2. [Sprawdź uprawnienia maszyny wirtualnej](backup-azure-sql-database.md#set-vm-permissions) potrzebne do tworzenia kopii zapasowych baz danych SQL.
3. Sprawdź, czy maszyna wirtualna ma [połączenie sieciowe](backup-sql-server-database-azure-vms.md#establish-network-connectivity).
4. Sprawdź, czy bazy danych programu SQL Server są nazwane zgodnie ze [wskazówkami dotyczącymi nazewnictwa](#verify-database-naming-guidelines-for-azure-backup) dla usługi Azure Backup.
5. Sprawdź, czy dla bazy danych nie są włączone inne rozwiązania do tworzenia kopii zapasowych. Wyłącz wszystkie inne kopie zapasowe programu SQL Server przed skonfigurowaniem tego scenariusza. Możesz bez konfliktów włączyć usługę Azure Backup dla maszyny wirtualnej platformy Azure razem z usługą Azure Backup dla bazy danych programu SQL Server uruchomioną na tej maszynie wirtualnej.


### <a name="establish-network-connectivity"></a>Ustawianie łączności sieciowej

Podczas wszystkich operacji maszyna wirtualna programu SQL Server musi mieć łączność z publicznymi adresami IP platformy Azure. Niepowodzenie operacji maszyny Wirtualnej (odnajdowaniu baz danych, skonfigurowanie kopii zapasowych, Planowanie tworzenia kopii zapasowych, przywracania punktów odzyskiwania i tak dalej) bez połączenia z publicznych adresów IP. Ustanów łączność, używając jednej z następujących opcji:

- **Zezwolenie na zakresy adresów IP centrum danych platformy Azure**: Zezwól na [zakresy adresów IP](https://www.microsoft.com/download/details.aspx?id=41653) dla pobierania. Aby uzyskać dostęp do sieciowej grupy zabezpieczeń (NSG), należy użyć **AzureNetworkSecurityRule zestaw** polecenia cmdlet.
- **Wdrożenie serwera proxy HTTP na potrzeby kierowania ruchu**: Gdy tworzysz kopię zapasową bazy danych programu SQL Server na maszynie wirtualnej platformy Azure, rozszerzenie kopii zapasowej na maszynie wirtualnej używa interfejsów API protokołu HTTPS do wysyłania poleceń zarządzania do usługi Azure Backup i danych do usługi Azure Storage. Rozszerzenie kopii zapasowej również używa usługi Azure Active Directory (Azure AD) do uwierzytelniania. Ruch rozszerzenia kopii zapasowej dla tych trzech usług należy kierować za pośrednictwem serwera proxy HTTP. Rozszerzenia są jedynym składnikiem, który jest skonfigurowany do uzyskiwania dostępu do publicznej sieci internet.

Każda opcja ma zalety i wady

**Option** | **Zalety** | **Wady**
--- | --- | ---
Zezwolenie na zakresy adresów IP | Brak dodatkowych kosztów. | Skomplikowane zarządzanie, ponieważ zakresy adresów IP zmieniają się z upływem czasu. <br/><br/> Zapewnia dostęp do całej platformy Azure, nie tylko do usługi Azure Storage.
Używanie serwera proxy HTTP   | Na serwerze jest dozwolona szczegółowa kontrola adresów URL magazynu. <br/><br/> Pojedynczy punkt dostępu do maszyn wirtualnych w Internecie. <br/><br/> Nie podlega zmianom adresów IP platformy Azure. | Dodatkowe koszty uruchamiania maszyny wirtualnej przy użyciu oprogramowania serwera proxy.

### <a name="set-vm-permissions"></a>Ustawianie uprawnień maszyny wirtualnej

Usługa Azure Backup wykonuje różne operacje podczas konfigurowania kopii zapasowych dla bazy danych programu SQL Server:

- Dodaje rozszerzenie **AzureBackupWindowsWorkload**.
- Aby odnaleźć bazy danych na maszynie wirtualnej, usługa Azure Backup tworzy konto **NT SERVICE\AzureWLBackupPluginSvc**. To konto służy do tworzenia kopii zapasowych i przywracania oraz wymaga uprawnień administratora systemu SQL.
- Usługa Azure Backup korzysta z konta **NT AUTHORITY\SYSTEM** na potrzeby odnajdywania/zapytań dotyczących bazy danych, dlatego to konto musi być publicznym logowaniem w środowisku SQL.

Jeśli nie utworzono maszyny wirtualnej programu SQL Server z witryny Azure Marketplace, może wystąpić błąd **UserErrorSQLNoSysadminMembership**. W takim przypadku [wykonaj te instrukcje](backup-azure-sql-database.md#set-vm-permissions).

### <a name="verify-database-naming-guidelines-for-azure-backup"></a>Wskazówki dotyczące weryfikowania nazewnictwa baz danych dla usługi Azure Backup

Należy unikać poniżej dla nazwy bazy danych:

  * Spacje końcowe/początkowe
  * Końcowy znak „!”
  * Zamykający nawias kwadratowy „]”
  * Nazwy baz danych, rozpoczynając od "F:\"

Oferujemy aliasy nieobsługiwanych znaków tabel platformy Azure, ale zalecamy ich unikanie. [Dowiedz się więcej](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN).


[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Odnajdowanie baz danych programu SQL Server

Odnajdź bazy danych uruchomione na maszynie wirtualnej.

1. W [witrynie Azure Portal](https://portal.azure.com) otwórz magazyn usługi Recovery Services używany do tworzenia kopii zapasowych bazy danych.

2. Na pulpicie nawigacyjnym **magazynu usługi Recovery Services** wybierz pozycję **Utwórz kopię zapasową**.

   ![Wybieranie pozycji Utwórz kopię zapasową w celu otwarcia menu Cel kopii zapasowej](./media/backup-azure-sql-database/open-backup-menu.png)

3. W obszarze **Cel kopii zapasowej** ustaw pozycję **Gdzie działa Twoje obciążenie** na wartość **Azure** (wartość domyślna).

4. W obszarze **Co ma zawierać kopia zapasowa** wybierz pozycję **SQL Server na maszynie wirtualnej platformy Azure**.

    ![Wybieranie programu SQL Server na maszynie wirtualnej platformy Azure na potrzeby tworzenia kopii zapasowej](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

5. W obszarze **Cel kopii zapasowej** > **Odnajdywanie baz danych na maszynach wirtualnych** wybierz pozycję **Uruchom odnajdywanie**, aby wyszukać niechronione maszyny wirtualne w subskrypcji. Może to trochę potrwać, zależnie od liczby niechronionych maszyn wirtualnych w subskrypcji.

   - Niechronione maszyny wirtualne powinny zostać wyświetlone na liście po odnalezieniu, uporządkowane według nazwy i grupy zasobów.
   - Jeśli którakolwiek maszyna wirtualna nie jest wyświetlana zgodnie z oczekiwaniami, sprawdź, czy jej kopie zapasowe są już tworzone w magazynie.
   - Wiele maszyn wirtualnych może mieć tę samą nazwę, ale należeć do różnych grup zasobów.

     ![Kopia zapasowa jest w stanie oczekiwania podczas wyszukiwania baz danych na maszynach wirtualnych](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. Na liście maszyn wirtualnych wybierz maszynę wirtualną z uruchomioną bazą danych programu SQL Server i wybierz pozycję **Odnajdź bazy danych**.

7. Śledź odnajdywanie baz danych w obszarze **Powiadomienia**. Wykonanie zadania może trochę potrwać, zależnie od liczby baz danych na maszynie wirtualnej. Po odnalezieniu wybranych baz danych zostanie wyświetlony komunikat o powodzeniu.

    ![Komunikat o powodzeniu wdrożenia](./media/backup-azure-sql-database/notifications-db-discovered.png)

8. Usługa Azure Backup odnajduje wszystkie bazy danych programu SQL Server na maszynie wirtualnej. Podczas odnajdowania poniżej przebiega w tle:

    - Usługa Azure Backup rejestruje maszynę wirtualną w magazynie na potrzeby kopii zapasowych obciążenia. Dla wszystkich baz danych na zarejestrowanej maszynie wirtualnej można tworzyć kopie zapasowe tylko w tym magazynie.
    - Usługa Azure Backup instaluje rozszerzenie **AzureBackupWindowsWorkload** na maszynie wirtualnej. Żaden agent nie jest instalowany w bazie danych SQL.
    - Usługa Azure Backup tworzy konto usługi **NT Service\AzureWLBackupPluginSvc** na maszynie wirtualnej.
      - Wszystkie operacje tworzenia kopii zapasowych i przywracania korzystają z konta usługi.
      - Usługa **NT Service\AzureWLBackupPluginSvc** musi mieć uprawnienia administratora systemu SQL. Wszystkie maszyny wirtualne programu SQL Server utworzone w witrynie Azure Marketplace mają zainstalowane rozszerzenie **SqlIaaSExtension**. Rozszerzenie **AzureBackupWindowsWorkload** używa rozszerzenia **SQLIaaSExtension** w celu automatycznego uzyskania wymaganych uprawnień.
    - Jeśli nie utworzono maszyny wirtualnej z witryny Marketplace, maszyna wirtualna nie ma zainstalowanego rozszerzenia **SqlIaaSExtension**, a operacja odnajdywania kończy się niepowodzeniem z komunikatem o błędzie **UserErrorSQLNoSysAdminMembership**. Postępuj zgodnie z [instrukcje](backup-azure-sql-database.md#set-vm-permissions) Aby rozwiązać ten problem.

        ![Wybieranie maszyny wirtualnej i bazy danych](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup"></a>Konfigurowanie kopii zapasowych  

Skonfiguruj tworzenie kopii zapasowych w następujący sposób:

1. W obszarze **Cel kopii zapasowej** wybierz pozycję **Konfiguruj kopię zapasową**.

   ![Wybieranie pozycji Konfiguruj kopię zapasową](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

2. Kliknij przycisk **konfigurowania kopii zapasowej**, **wybierz elementy do wykonywania kopii zapasowych** zostanie wyświetlony blok. Ta lista zawiera wszystkie grupy dostępności zarejestrowanych i autonomicznych serwerów SQL. Rozwiń cudzysłów ostrokątny po lewej stronie wiersz, aby wyświetlić wszystkie niechronionych baz danych w danym wystąpienie lub zawsze włączona grupa dostępności.  

    ![Wyświetlanie wszystkich wystąpień programu SQL Server z autonomicznymi bazami danych](./media/backup-azure-sql-database/list-of-sql-databases.png)

3. Wybierz wszystkie bazy danych, które chcesz chronić, i wybierz pozycję **OK**.

   ![Chronienie bazy danych](./media/backup-azure-sql-database/select-database-to-protect.png)

   Aby zoptymalizować obciążenia kopii zapasowych, usługa Azure Backup ustawia maksymalną liczbę baz danych w jednym zadaniu tworzenia kopii zapasowej na 50.

    
     * Alternatywnie, można włączyć ochrony automatycznej na całe wystąpienie lub zawsze na Availability group, wybierając **ON** opcji na liście rozwijanej odpowiedniego **AUTOPROTECT** kolumny. Funkcji automatycznej ochrony nie tylko umożliwia ochronę wszystkich istniejących baz danych w jednym z rzeczywistym użyciem, ale również automatycznie chronić żadnych nowych baz danych, które zostaną dodane do grupy dostępności lub tego wystąpienia w przyszłości.  

4. Kliknij przycisk **OK** otworzyć **zasady tworzenia kopii zapasowej** bloku.

    ![Włączanie automatycznej ochrony zawsze włączonej grupy dostępności](./media/backup-azure-sql-database/enable-auto-protection.png)

5. W **wybierz zasady tworzenia kopii zapasowej**, wybierz zasady, a następnie kliknij przycisk **OK**.

   - Wybrać zasady domyślne: HourlyLogBackup.
   - Wybrać istniejące zasady kopii zapasowych utworzone wcześniej dla środowiska SQL.
   - Definiowanie nowych zasad opartych na swój zakres od celu punktu odzyskiwania i przechowywania.

     ![Wybieranie pozycji Zasady kopii zapasowych](./media/backup-azure-sql-database/select-backup-policy.png)

6. Na **kopii zapasowej** menu, wybierz opcję **Włącz kopię zapasową**.

    ![Włączanie wybranych zasad kopii zapasowych](./media/backup-azure-sql-database/enable-backup-button.png)

7. Śledź postęp konfiguracji w **powiadomienia** obszaru portalu.

    ![Obszar powiadomień](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Tworzenie zasad kopii zapasowych

Zasady kopii zapasowych określają, kiedy są tworzone kopie zapasowe i jak długo są przechowywane.

- Zasady są tworzone na poziomie magazynu.
- Wiele magazynów może korzystać z tych samych zasad kopii zapasowych, ale do każdego magazynu należy zastosować zasady kopii zapasowych.
- Po utworzeniu zasad kopii zapasowych ustawieniem domyślnym będzie pełna kopia zapasowa tworzona codziennie.
- Możesz dodać różnicową kopię zapasową, ale tylko jeśli skonfigurujesz tworzenie pełnych kopii zapasowych co tydzień.
- [Dowiedz się więcej](backup-architecture.md#sql-server-backup-types) o różnych typach zasad kopii zapasowych.

Aby utworzyć nowe zasady kopii zapasowych:

1. W magazynie kliknij pozycję **Zasady kopii zapasowych** > **Dodaj**.
2. W menu **Dodaj** kliknij polecenie **Program SQL Server na maszynie wirtualnej platformy Azure**. Aby definiuje typ zasad.

   ![Wybieranie typu nowych zasad kopii zapasowych](./media/backup-azure-sql-database/policy-type-details.png)

3. W polu **Nazwa zasad** wprowadź nazwę nowych zasad.
4. W obszarze **Zasady pełnej kopii zapasowej** wybierz opcję **Częstotliwość tworzenia kopii zapasowej** (**Codziennie** lub **Co tydzień**).

   - W przypadku opcji **Codziennie** wybierz godzinę i strefę czasową rozpoczęcia zadania tworzenia kopii zapasowej.
   - Należy uruchomić pełnej kopii zapasowej, ponieważ nie można wyłączyć **pełnej kopii zapasowej** opcji.
   - Kliknij pozycję **Pełna kopia zapasowa**, aby wyświetlić zasady.
   - Nie można tworzyć różnicowych kopii zapasowych w przypadku codziennego tworzenia pełnych kopii zapasowych.
   - W przypadku opcji **Co tydzień** wybierz dzień tygodnia, godzinę i strefę czasową rozpoczęcia zadania tworzenia kopii zapasowej.

     ![Pola nowych zasad kopii zapasowych](./media/backup-azure-sql-database/full-backup-policy.png)  

5. Domyślnie wszystkie opcje **Zakres przechowywania** są wybrane. Wyczyść wszelkie ograniczenia zakresu przechowywania, których nie chcesz używać, oraz ustaw interwały do użycia.

    - Minimalny okres przechowywania dla dowolnego typu kopii zapasowej (pełnej/różnicowej/log) wynosi 7 dni.
    - Punkty odzyskiwania są oznaczone do przechowywania na podstawie ich zakresu przechowywania. Jeśli na przykład wybierzesz codzienne tworzenie pełnej kopii zapasowej, każdego dnia będzie wyzwalana tylko jedna pełna kopia zapasowa.
    - Kopia zapasowa dla określonego dnia jest oznaczana i przechowywana w oparciu o tygodniowy zakres przechowywania oraz ustawienie przechowywania co tydzień.
    - Miesięczne i roczne zakresy przechowywania zachowują się w podobny sposób.

   ![Ustawienia interwału zakresu przechowywania](./media/backup-azure-sql-database/retention-range-interval.png)

6. W menu **zasad pełnej kopii zapasowej** wybierz pozycję **OK**, aby zaakceptować ustawienia.
7. Aby dodać zasady tworzenia różnicowej kopii zapasowej, wybierz pozycję **Różnicowa kopia zapasowa**.

   ![Ustawienia interwału zakresu przechowywania](./media/backup-azure-sql-database/retention-range-interval.png)
   ![Otwieranie menu zasad różnicowej kopii zapasowej](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. W obszarze **Zasady różnicowej kopii zapasowej** wybierz pozycję **Włącz**, aby otworzyć kontrolki częstotliwości i przechowywania.

    - Można wyzwalać maksymalnie jedną różnicową kopię zapasową dziennie.
    - Różnicowe kopie zapasowe mogą być przechowywane przez maksymalnie 180 dni. Jeśli potrzebujesz dłuższego okresu przechowywania, musisz użyć pełnych kopii zapasowych.

9. Wybierz pozycję **OK**, aby zapisać zasady i wróć do głównego menu **zasad kopii zapasowych**.

10. Aby dodać zasady kopii zapasowej dziennika transakcji, wybierz pozycję **Kopia zapasowa dziennika**.
11. W obszarze **Kopia zapasowa dziennika** wybierz pozycję **Włącz**, a następnie ustaw kontrolki częstotliwości i przechowywania. Kopie zapasowe dziennika można tworzyć nawet co 15 minut. Można je również zachowywać przez maksymalnie 35 dni.
12. Wybierz pozycję **OK**, aby zapisać zasady i wróć do głównego menu **zasad kopii zapasowych**.

    ![Edytowanie zasad kopii zapasowej dziennika](./media/backup-azure-sql-database/log-backup-policy-editor.png)

13. W menu **Zasady kopii zapasowych** wybierz, czy chcesz włączyć **kompresję kopii zapasowej SQL**.
    - Kompresja jest domyślnie wyłączona.
    - Na zapleczu usługa Azure Backup używa natywnej kompresji kopii zapasowej SQL.

14. Po zakończeniu edycji zasad kopii zapasowych wybierz pozycję **OK**.

## <a name="run-an-ad-hoc-backup"></a>Uruchom kopię zapasową ad hoc

1. W magazynie usługi Recovery Services wybierz elementy kopii zapasowej.
2. Kliknij pozycję "SQL na maszynie Wirtualnej platformy Azure".
3. Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz pozycję "Utwórz teraz kopię zapasową".
4. Wybierz typ kopii zapasowej (pełnej/różnicowej/Log/Kopiuj tylko pełne) i kompresję (Włącz/Wyłącz)
5. Wybierz przycisk OK, aby rozpocząć tworzenie kopii zapasowej.
6. Monitorowanie zadania tworzenia kopii zapasowej, przechodząc do magazynu usługi Recovery Services i wybierając pozycję "Zadania tworzenia kopii zapasowej".


## <a name="next-steps"></a>Kolejne kroki

Podczas pracy z tym samouczkiem wykonano następujące czynności przy użyciu witryny Azure Portal:

> [!div class="checklist"]
> * Tworzenie i konfigurowanie magazynu.
> * Odnajdywanie baz danych i konfigurowanie kopii zapasowych.
> * Konfigurowanie ochrony automatycznej dla baz danych.
> * Uruchom kopię zapasową ad hoc.

Przejdź do następnego samouczka, aby przywrócić maszynę wirtualną platformy Azure z dysku.

> [!div class="nextstepaction"]
> [Przywracanie bazy danych SQL Server na maszynach wirtualnych platformy Azure](./restore-sql-database-azure-vm.md)
 

