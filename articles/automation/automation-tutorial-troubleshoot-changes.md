---
title: Rozwiązywanie problemów ze zmianą w maszynie wirtualnej platformy Azure | Microsoft Docs
description: Użyj śledzenia zmian, aby rozwiązać problemy ze zmianami w maszynie wirtualnej platformy Azure.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
keywords: zmiana, śledzenie, automatyzacja
author: jennyhunter-msft
ms.author: jehunte
ms.date: 12/05/2018
ms.topic: tutorial
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 92f25d956bc8f1f930ae6ebbf7ee48c144bf8a30
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476859"
---
# <a name="troubleshoot-changes-in-your-environment"></a>Rozwiązywanie problemów ze zmianami we własnym środowisku

W tym samouczku dowiesz się jak rozwiązywać problemy ze zmianami w maszynie wirtualnej platformy Azure. Włączając śledzenie zmian, możesz śledzić zmiany w oprogramowaniu, plikach, demonach systemu Linux, usługach systemu Windows i kluczach rejestru systemu Windows znajdujących się na komputerach.
Identyfikowanie tych zmian konfiguracji może pomóc w określeniu problemów z działaniem w swoim środowisku.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dodawanie maszyny wirtualnej do śledzenia zmian i spisu
> * Wyszukiwanie dzienników zmian dla zatrzymanej usługi
> * Konfigurowanie śledzenia zmian
> * Włączanie połączenia dziennika aktywności
> * Wyzwalanie zdarzenia
> * Przeglądanie zmian
> * Konfigurowanie alertów

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka niezbędne są następujące elementy:

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Konto automatyzacji](automation-offering-get-started.md) do przechowywania obserwatora i elementów Runbook akcji oraz zadania obserwatora.
* [Maszyna wirtualna](../virtual-machines/windows/quick-create-portal.md) do dołączenia.

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się do witryny Azure Portal pod adresem https://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Włączanie śledzenia zmian i spisu

Najpierw musisz włączyć śledzenie zmian i spisu dla maszyny wirtualnej w tym samouczku. Jeśli inne rozwiązanie automatyzacji zostało wcześniej włączone dla maszyny wirtualnej, ten krok nie jest konieczny.

1. W menu po lewej stronie wybierz pozycję **Maszyny wirtualne** i wybierz z listy maszynę wirtualną
1. W menu po lewej stronie w sekcji **OPERACJE** kliknij pozycję **Spis**. Zostanie otwarta strona **Śledzenie zmian**.

![Włączanie zmian](./media/automation-tutorial-troubleshoot-changes/enableinventory.png) Zostanie otwarty ekran **Śledzenie zmian**. Skonfiguruj lokalizację, obszar roboczy usługi Log Analytics i konto usługi Automation, a następnie kliknij pozycję **Włącz**. Jeśli pola są wygaszone, oznacza to, że inne rozwiązanie automatyzacji jest włączone dla maszyny wirtualnej, a tym samym należy użyć tego samego obszaru roboczego i konta automatyzacji.

Obszar roboczy usługi [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) służy do zbierania danych generowanych przez funkcje i usługi, takie jak spis.
Obszar roboczy zawiera pojedynczą lokalizację do przeglądania i analizowania danych z wielu źródeł.

Podczas dołączania maszyna wirtualna jest aprowizowana za pomocą programu Microsoft Monitoring Agent (MMA) i hybrydowego procesu roboczego.
Ten agent jest używany do komunikacji z maszyną wirtualną i uzyskiwania informacji dotyczących zainstalowanego oprogramowania.

Włączanie rozwiązania może potrwać do 15 minut. W tym czasie nie należy zamykać okna przeglądarki.
Po włączeniu rozwiązania informacje dotyczące zainstalowanego oprogramowania i zmian w maszynie wirtualnej są przekazywane do dzienników usługi Azure Monitor.
Udostępnienie danych do analizy może potrwać od 30 minut do 6 godzin.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="using-change-tracking-in-azure-monitor-logs"></a>Korzystanie ze śledzenia zmian w dziennikach usługi Azure Monitor

Śledzenie zmian generuje dane dziennika, które są wysyłane do dzienników usługi Azure Monitor.
Aby wyszukiwać w dziennikach za pomocą uruchamiania zapytań, wybierz pozycję **Log Analytics** w górnej części okna **Śledzenie zmian**.
Dane śledzenia zmian są przechowywane w obszarze typu **ConfigurationChange**.
Następujące przykładowe zapytanie usługi Log Analytics zwraca wszystkie usługi systemu Windows, które zostały zatrzymane.

```loganalytics
ConfigurationChange
| where ConfigChangeType == "WindowsServices" and SvcState == "Stopped"
```

Aby dowiedzieć się więcej na temat uruchamiania i wyszukiwania plików dziennika usługi Azure Monitor, zobacz [Dzienniki usługi Azure Monitor](../azure-monitor/log-query/log-query-overview.md).

## <a name="configure-change-tracking"></a>Konfigurowanie śledzenia zmian

Śledzenie zmian daje możliwość śledzenia zmian konfiguracji na własnej maszynie wirtualnej. Poniższe kroki pokazują, jak skonfigurować śledzenie kluczy rejestru i plików.

Aby wybrać, które pliki i klucze rejestru mają być zbierane i śledzone, wybierz pozycję **Edytuj ustawienia** w górnej części strony **Śledzenie zmian**.

> [!NOTE]
> Spis i śledzenie zmian używają tych samych ustawień gromadzenia, a ustawienia zostaną skonfigurowane na poziomie obszaru roboczego.

W oknie **Konfiguracja obszaru roboczego** dodaj klucze rejestru systemu Windows, pliki systemu Windows lub pliki systemu Linux, które mają być śledzone, zgodnie z opisem w trzech kolejnych sekcjach.

### <a name="add-a-windows-registry-key"></a>Dodawanie klucza rejestru systemu Windows

1. Na karcie **Rejestr systemu Windows** wybierz opcję **Dodaj**.
    Zostanie otwarte okno **Dodawanie rejestru systemu Windows dla śledzenia zmian**.

1. Na stronie **Dodawanie rejestru systemu Windows do śledzenia zmian** wprowadź informacje o kluczu, który ma być śledzony, i kliknij przycisk **Zapisz**

|Właściwość  |Opis  |
|---------|---------|
|Enabled (Włączony)     | Określa, czy ustawienie jest stosowane        |
|Nazwa elementu     | Przyjazna nazwa pliku, który ma być śledzony        |
|Grupa     | Nazwa grupy do logicznego grupowania plików        |
|Klucz rejestru systemu Windows   | Ścieżka do sprawdzania pliku, na przykład: „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup”      |

### <a name="add-a-windows-file"></a>Dodawanie pliku systemu Windows

1. Na karcie **Pliki systemu Windows** wybierz opcję **Dodaj**. Zostanie otwarte okno **Dodawanie pliku systemu Windows dla śledzenia zmian**.

1. Na stronie **Dodawanie pliku systemu Windows do śledzenia zmian** wprowadź informacje o pliku lub katalogu, który ma być śledzony, i kliknij przycisk **Zapisz**

|Właściwość  |Opis  |
|---------|---------|
|Enabled (Włączony)     | Określa, czy ustawienie jest stosowane        |
|Nazwa elementu     | Przyjazna nazwa pliku, który ma być śledzony        |
|Grupa     | Nazwa grupy do logicznego grupowania plików        |
|Wprowadzanie ścieżki     | Ścieżka do sprawdzania pliku, na przykład: „c:\temp\\\*.txt”<br>Możesz użyć również zmiennych środowiskowych, takich jak „%winDir%\System32\\\*.*”         |
|Rekursja     | Określa, czy podczas wyszukiwania elementu, który ma być śledzony, ma być używana rekursja.        |
|Przekaż zawartość pliku dla wszystkich ustawień| Włącza lub wyłącza przekazywanie zawartości pliku dla śledzonych zmian. Dostępne opcje: **True** lub **False**.|

### <a name="add-a-linux-file"></a>Dodawanie pliku systemu Linux

1. Na karcie **Pliki systemu Linux** wybierz opcję **Dodaj**. Zostanie otwarte okno **Dodawanie pliku systemu Linux dla śledzenia zmian**.

1. Na stronie **Dodawanie pliku systemu Linux do śledzenia zmian** wprowadź informacje o pliku lub katalogu, który ma być śledzony, i kliknij przycisk **Zapisz**.

|Właściwość  |Opis  |
|---------|---------|
|Enabled (Włączony)     | Określa, czy ustawienie jest stosowane        |
|Nazwa elementu     | Przyjazna nazwa pliku, który ma być śledzony        |
|Grupa     | Nazwa grupy do logicznego grupowania plików        |
|Wprowadzanie ścieżki     | Ścieżka do sprawdzania pliku, na przykład: „/etc/*.conf”       |
|Typ ścieżki     | Typ elementu, który ma być monitorowany; możliwe wartości to Plik i Katalog        |
|Rekursja     | Określa, czy podczas wyszukiwania elementu, który ma być śledzony, ma być używana rekursja.        |
|Użyj polecenia Sudo     | To ustawienie określa, czy podczas sprawdzania elementu jest używane polecenie sudo.         |
|Linki     | To ustawienie określa, w jaki sposób są obsługiwane linki symboliczne podczas przechodzenia między katalogami.<br> **Ignoruj** — ignoruje linki symboliczne i nie uwzględnia plików/katalogów, do których się odwołują<br>**Śledź** — śledzi linki symboliczne podczas rekursji i uwzględnia również pliki/katalogi, do których się odwołują<br>**Zarządzaj** — śledzi linki symboliczne i umożliwia obsługę zwracanej zawartości      |
|Przekaż zawartość pliku dla wszystkich ustawień| Włącza lub wyłącza przekazywanie zawartości pliku dla śledzonych zmian. Dostępne opcje: **True** lub **False**.|

   > [!NOTE]
   > Opcja linków „Zarządzaj” nie jest zalecana. Pobieranie zawartości plików nie jest obsługiwane.

## <a name="enable-activity-log-connection"></a>Włączanie połączenia dziennika aktywności

Ze strony **Śledzenie zmian** na swojej maszynie wirtualnej wybierz pozycję **Zarządzanie połączeniem dziennika aktywności**. To zadanie powoduje otwarcie strony **Dziennik aktywności platformy Azure**. Wybierz pozycję **Połącz**, aby połączyć śledzenie zmian z dziennikiem aktywności platformy Azure dla Twojej maszyny wirtualnej.

Po włączeniu tego ustawienia przejdź do strony **Omówienie** dla maszyny wirtualnej i wybierz pozycję **Zatrzymaj**, aby zatrzymać swoją maszynę wirtualną. Po wyświetleniu monitu wybierz pozycję **Tak**, aby zatrzymać maszynę wirtualną. Po cofnięciu jej przydziału wybierz pozycję **Start**, aby ponownie uruchomić maszynę wirtualną.

Zatrzymanie i uruchomienie maszyny wirtualnej rejestruje zdarzenie w jego dzienniku aktywności. Przejdź z powrotem do strony **Śledzenie zmian**. Wybierz kartę **Zdarzenia** u dołu strony. Po chwili zdarzenia są wyświetlane na wykresie i w tabeli. Podobnie jak w poprzednim kroku, każde zdarzenie można wybrać, aby wyświetlić szczegółowe informacje o zdarzeniu.

![Wyświetlanie szczegółów zmiany w portalu](./media/automation-tutorial-troubleshoot-changes/viewevents.png)

## <a name="view-changes"></a>Przeglądanie zmian

Po włączeniu rozwiązania śledzenia zmian i spisu możesz wyświetlić wyniki na stronie **Śledzenie zmian**.

W ramach maszyny wirtualnej zaznacz pozycję **Śledzenie zmian** w obszarze **OPERACJE**.

![Zrzut ekranu przedstawiający listę zmian dotyczących maszyny wirtualnej](./media/automation-tutorial-troubleshoot-changes/change-tracking-list.png)

Wykres pokazuje zmiany, które wystąpiły w czasie.
Po dodaniu połączenia dziennika aktywności wykres liniowy u góry wyświetla zdarzenia dziennika aktywności platformy Azure.
Każdy wiersz wykresów słupkowych reprezentuje innego typu zmiany umożliwiające śledzenie.
Do tych typów należą demony systemu Linux, pliki, klucze rejestru systemu Windows, oprogramowanie i usługi systemu Windows.
Karta zmiany przedstawia szczegółowe informacje dla zmian pokazanych w wizualizacji w kolejności malejącej według czasu, kiedy wystąpiła zmiana (najnowsze na początku).
Na karcie **Zdarzenia** są wyświetlane w tabeli zdarzenia połączonego dziennika aktywności wraz z ich odpowiednimi szczegółami, przy czym najnowsze są na początku.

W wynikach możesz zobaczyć, że w systemie było wiele zmian łącznie ze zmianami usług i oprogramowania. Możesz użyć filtrów u góry strony, aby przefiltrować wyniki według **Typu zmiany** lub zakresu czasu.

Wybierz zmianę **WindowsServices**, a spowoduje to otwarcie okna **Szczegóły zmiany**. Okno Szczegóły zmiany zawiera szczegółowe informacje o zmianie oraz wartości przed i po zmianie. W tym wystąpieniu zatrzymano usługę ochrony oprogramowania.

![Wyświetlanie szczegółów zmiany w portalu](./media/automation-tutorial-troubleshoot-changes/change-details.png)

## <a name="configure-alerts"></a>Konfigurowanie alertów

Wyświetlanie zmian wprowadzonych w witrynie Azure Portal może być przydatne, ale bardziej korzystna jest możliwość otrzymywania alertów po wystąpieniu zmiany (np. po zatrzymaniu usługi).

Aby dodać alert dotyczący zatrzymanej usługi, w witrynie Azure Portal przejdź do pozycji **Monitorowanie**. Następnie w obszarze **Usługi udostępnione** wybierz pozycję **Alerty** i kliknij przycisk **+ Nowa reguła alertu**

Kliknij polecenie **Wybierz**, aby wybrać zasób. Na stronie **Wybierz zasób** wybierz pozycję **Log Analytics** na liście rozwijanej **Filtruj według typu zasobu**. Wybierz swój obszar roboczy usługi Log Analytics, a następnie wybierz pozycję **Gotowe**.

![Wybieranie zasobu](./media/automation-tutorial-troubleshoot-changes/select-a-resource.png)

Kliknij opcję **Dodaj warunek**, a na stronie **Konfigurowanie logiki sygnału** wybierz opcję **Przeszukiwanie dzienników niestandardowych** w tabeli. W polu tekstowym Zapytanie wyszukiwania wprowadź poniższe zapytanie:

```loganalytics
ConfigurationChange | where ConfigChangeType == "WindowsServices" and SvcName == "W3SVC" and SvcState == "Stopped" | summarize by Computer
```

To zapytanie zwraca informacje o komputerach, na których w określonym przedziale czasu została zatrzymana usługa W3SVC.

W obszarze **Logika alertu** w polu **Próg** wprowadź **0**. Po zakończeniu wybierz pozycję **Gotowe**.

![Konfigurowanie logiki sygnału](./media/automation-tutorial-troubleshoot-changes/configure-signal-logic.png)

W obszarze **Grupy akcji** wybierz pozycję **Utwórz nową**. Grupa akcji to grupa składająca się z akcji, których można używać w wielu alertach. Akcje mogą obejmować powiadomienia e-mail, elementy runbook i webhook oraz wiele innych. Aby dowiedzieć się więcej o grupach akcji, zobacz [Create and manage action groups (Tworzenie grup akcji i zarządzanie nimi)](../azure-monitor/platform/action-groups.md).

W obszarze **Szczegóły alertu** wprowadź nazwę i opis alertu. Ustaw **Ważność** na **Informacyjny (ważność 2)** , **Ostrzegawczy (ważność 1)** lub **Krytyczny (ważność 0)** .

W polu **Nazwa grupy akcji** wprowadź nazwę alertu oraz krótką nazwę. Krótka nazwa jest używana zamiast pełnej nazwy grupy akcji podczas przesyłania powiadomień przy użyciu danej grupy.

W obszarze **Akcje** wprowadź nazwę akcji, na przykład **Wyślij wiadomość e-mail do administratorów**. W obszarze **TYP AKCJI** wybierz pozycję **E-mail/SMS/Push/Głos**. W obszarze **SZCZEGÓŁY** wybierz pozycję **Edytuj szczegóły**.

![Dodawanie grupy akcji](./media/automation-tutorial-troubleshoot-changes/add-action-group.png)

W okienku **E-mail/SMS/Push/Głos** wprowadź nazwę. Zaznacz pole wyboru **E-mail**, a następnie wprowadź prawidłowy adres e-mail. Kliknij przycisk **OK** na stronie **E-mail/SMS/Push/Głos**, a następnie kliknij przycisk **OK** na stronie **Dodawanie grupy akcji**.

Aby dostosować temat wiadomości e-mail alertu, w oknie **Tworzenie reguły** w obszarze **Dostosowywanie akcji** wybierz pozycję **Temat wiadomości e-mail**. Po zakończeniu wybierz pozycję **Utwórz regułę alertu**. Alert informuje użytkownika o pomyślnym wdrożeniu aktualizacji oraz o maszynach będących elementami danego uruchomienia wdrożenia aktualizacji.

Poniższa ilustracja przedstawia przykładową wiadomość e-mail odebraną po zatrzymaniu usługi W3SVC.

![email](./media/automation-tutorial-troubleshoot-changes/email.png)

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dodawanie maszyny wirtualnej do śledzenia zmian i spisu
> * Wyszukiwanie dzienników zmian dla zatrzymanej usługi
> * Konfigurowanie śledzenia zmian
> * Włączanie połączenia dziennika aktywności
> * Wyzwalanie zdarzenia
> * Przeglądanie zmian
> * Konfigurowanie alertów

Przejdź do omówienia rozwiązania śledzenia zmian i spisu, aby uzyskać więcej informacji.

> [!div class="nextstepaction"]
> [Change management and Inventory solution](automation-change-tracking.md) (Rozwiązanie zarządzania zmianami i spisem)

