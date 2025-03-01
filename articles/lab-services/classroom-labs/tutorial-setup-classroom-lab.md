---
title: Konfigurowanie laboratorium na potrzeby zajęć przy użyciu usługi Azure Lab Services | Microsoft Docs
description: W tym samouczku skonfigurujesz laboratorium do użycia podczas zajęć.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 803fe6eff8804dbd407642386865fe975c8db524
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67123263"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Samouczek: Konfigurowanie laboratorium na potrzeby zajęć 
W tym samouczku skonfigurujesz laboratorium na potrzeby zajęć z maszynami wirtualnymi używanymi przez uczniów podczas zajęć.  

W tym samouczku wykonasz następujące czynności:

> [!div class="checklist"]
> * Tworzenie laboratorium na potrzeby zajęć
> * Dodawanie użytkowników do laboratorium
> * Wysyłanie linków rejestracyjnych do uczniów

## <a name="prerequisites"></a>Wymagania wstępne
Aby skonfigurować laboratorium na potrzeby zajęć na koncie laboratorium, musi być członkiem jednej z tych ról w ramach konta laboratorium: Właściciel, Twórca laboratorium lub Współautor. Konto, którego użyto do utworzenia konta laboratorium, jest automatycznie dodawane do roli właściciela.

Właściciel laboratorium może dodawać innych użytkowników do roli **Twórca laboratorium**. Właściciel laboratorium dodaje na przykład nauczycieli akademickich do roli twórcy laboratorium. Następnie nauczyciele akademiccy tworzą laboratoria za pomocą maszyn wirtualnych dla swoich zajęć. Studenci rejestrują się w laboratoriach przy użyciu linku rejestracji otrzymanego od nauczycieli akademickich. Po zarejestrowaniu się studenci mogą używać maszyn wirtualnych w laboratoriach do wykonywania zadań na zajęciach lub pracy domowej. Aby uzyskać szczegółowe informacje na temat dodawania użytkowników do roli Twórca laboratorium, zobacz [Dodawanie użytkownika do roli Twórca laboratorium](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Tworzenie laboratorium na potrzeby zajęć

1. Przejdź do [witryny internetowej usługi Azure Lab Services](https://labs.azure.com). Należy pamiętać, że program Internet Explorer 11 nie jest jeszcze obsługiwany. 
2. Wybierz pozycję **Zaloguj się** i wprowadź swoje poświadczenia. Usługa Azure Lab Services obsługuje konta organizacji i konta Microsoft. 
3. W oknie **Nowe laboratorium** wykonaj następujące czynności: 
    1. Określ **nazwę** dla swojego laboratorium. 
    2. Określ maksymalną **liczbę maszyn wirtualnych** w laboratorium. Liczbę maszyn wirtualnych można zmniejszyć lub zwiększyć po utworzeniu laboratorium lub w istniejącym laboratorium. Aby uzyskać więcej informacji, zobacz [Update number of VMs in a lab (Aktualizowanie liczby maszyn wirtualnych w laboratorium)](how-to-configure-student-usage.md#update-number-of-virtual-machines-in-lab)
    6. Wybierz pozycję **Zapisz**.

        ![Tworzenie laboratorium na potrzeby zajęć](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Na stronie **Wybór specyfikacji maszyny wirtualnej** wykonaj następujące czynności:
    1. Wybierz **rozmiar** maszyn wirtualnych tworzonych w laboratorium. Obecnie **małych**, **średni**, **medium (wirtualizacja)** , **dużych**, i **procesora GPU** o rozmiarach dozwolone.
    3. Wybierz **obraz maszyny wirtualnej**, którego chcesz używać do tworzenia maszyn wirtualnych w laboratorium. Jeśli wybierzesz obraz systemu Linux, zobaczysz opcję, aby włączyć połączenie pulpitu zdalnego dla niego. Aby uzyskać więcej informacji, zobacz [Włączanie połączeń usług pulpitu zdalnego dla systemu Linux](how-to-enable-remote-desktop-linux.md).
    4. Wybierz opcję **Dalej**.

        ![Określanie specyfikacji maszyny wirtualnej](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. Na stronie **Ustawianie poświadczeń** określ domyślne poświadczenia dla wszystkich maszyn wirtualnych w laboratorium. 
    1. Określ **nazwę użytkownika** dla wszystkich maszyn wirtualnych w laboratorium.
    2. Podaj **hasło** użytkownika. 

        > [!IMPORTANT]
        > Zanotuj nazwę użytkownika i hasło. Nie zostaną one ponownie wyświetlone.
    3. Wybierz pozycję **Utwórz**. 

        ![Ustawianie poświadczeń](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. Na stronie **Konfigurowanie szablonu** zostanie wyświetlony stan procesu tworzenia laboratorium. Tworzenie szablonu w laboratorium trwa maksymalnie 20 minut. 

    ![Konfigurowanie szablonu](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Po zakończeniu konfiguracji szablonu zostanie wyświetlona następująca strona: 

    ![Strona Konfigurowanie szablonu po zakończeniu konfiguracji](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Na stronie **Konfigurowanie szablonu** wykonaj następujące kroki: Te kroki samouczka są **opcjonalne**.
    2. Nawiąż połączenie z maszyną wirtualną szablonu, wybierając pozycję **Połącz**. Jeśli szablon maszyny Wirtualnej systemu Linux, wybierz, czy chcesz nawiązać połączenie przy użyciu klienta SSH lub RDP (jeśli został włączony protokół RDP).
    1. Wybierz **Resetuj hasło** resetowania hasła dla maszyny Wirtualnej. 
    1. Zainstaluj i skonfiguruj oprogramowania na maszynie wirtualnej szablonu. 
    1. **Zatrzymaj** maszynę wirtualną.  
    1. Wprowadź **opis** szablonu.
9. Wybierz przycisk **Dalej** na stronie szablonu. 
10. Na stronie **Publikowanie szablonu** wykonaj następujące czynności. 
    1. Aby natychmiast opublikować szablonu, wybierz **Publikuj**.  

        > [!WARNING]
        > Nie można cofnąć publikowania szablonu. 
    2. Aby opublikować później, wybierz pozycję **Zachowaj na później**. Po zakończeniu pracy kreatora, możesz opublikować szablonu maszyny Wirtualnej. Aby uzyskać więcej informacji na temat sposobu konfigurowania i publikowania, po zakończeniu pracy kreatora, zobacz [opublikować szablon](how-to-create-manage-template.md#publish-the-template-vm) sekcji [sposób zarządzania laboratorium na potrzeby zajęć](how-to-manage-classroom-labs.md) artykułu.

        ![Publikowanie szablonu](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Zostanie wyświetlony **postęp publikowania** szablonu. Ten proces może potrwać do godziny. 

    ![Publikowanie szablonu — postęp](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Po pomyślnym opublikowaniu szablonu zostanie wyświetlona następująca strona. Wybierz pozycję **Done** (Gotowe).

    ![Publikowanie szablonu — powodzenie](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Zostanie wyświetlony **pulpit nawigacyjny** laboratorium. 
    
    ![Pulpit nawigacyjny laboratorium na potrzeby zajęć](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Przejdź na stronę **Maszyny wirtualne**, wybierając pozycję Maszyny wirtualne w menu po lewej stronie lub wybierając kafelek Maszyny wirtualne. Sprawdź, czy są widoczne maszyny wirtualne o stanie **Nie przypisano**. Te maszyny wirtualne nie zostały jeszcze przypisane do uczniów. Powinny mieć stan **Zatrzymano**. Z poziomu tej strony możesz uruchomić maszynę wirtualną ucznia, połączyć się z maszyną wirtualną, zatrzymać maszynę wirtualną i usunąć maszynę wirtualną. Maszyny wirtualne możesz uruchomić na tej stronie lub pozwolić, aby zrobili to uczniowie. 

    ![Maszyny wirtualne w stanie Zatrzymano](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="add-users-to-the-lab"></a>Dodawanie użytkowników do laboratorium

1. W menu po lewej stronie wybierz pozycję **Użytkownicy**. Domyślnie włączona jest opcja **Ogranicz dostęp**. Gdy to ustawienie jest włączone, a użytkownik nie znajduje się na liście użytkowników, nie może zarejestrować się w laboratorium nawet przy użyciu linku rejestracji. Tylko użytkownicy na liście mogą zarejestrować się w laboratorium przy użyciu przesłanego linku rejestracji. Podczas tej procedury dodasz użytkowników do listy. Możesz też wyłączyć opcję **Ogranicz dostęp**, co umożliwi użytkownikom zarejestrowanie się w laboratorium, pod warunkiem, że mają link rejestracji. 
2. Na pasku narzędzi wybierz pozycję **Dodaj użytkowników**. 

    ![Przycisk Dodaj użytkowników](../media/how-to-configure-student-usage/add-users-button.png)
1. Na stronie **Dodawanie użytkowników** wpisz adresy e-mail użytkowników w osobnych wierszach lub w jednym wierszu, rozdzielając je średnikami. 

    ![Dodawanie adresów e-mail użytkowników](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Wybierz pozycję **Zapisz**. Na liście zostaną wyświetlone adresy e-mail użytkowników oraz ich stan rejestracji. 

    ![Lista użytkowników](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-for-users"></a>Ustawianie przydziałów dla użytkowników
Można ustawić limity przydziału dla poszczególnych użytkowników wykonując następujące kroki: 

1. Wybierz **użytkowników** w menu po lewej stronie, jeśli strona nie jest jeszcze aktywna. 
2. Wybierz **limitu przydziału dla poszczególnych użytkowników:** na pasku narzędzi. 
3. Na **limitu przydziału dla poszczególnych użytkowników** Określ liczbę godzin, które ma zostać przypisany do każdego użytkownika (dla uczniów): 
    1. **0 godz. (tylko dla harmonogramu)** . Użytkownicy mogą używać swoich maszyn wirtualnych tylko w zaplanowanym terminie lub jako właściciel laboratorium zmieni się na maszynach wirtualnych dla nich.

        ![Zero godzin — tylko zaplanowanym czasie](../media/how-to-configure-student-usage/zero-hours.png)
    1. **Łączną liczbę godzin laboratorium na użytkownika**. Użytkownicy mogą używać swoich maszyn wirtualnych przez liczbę godzin (określone dla tego pola) **poza zaplanowanym terminie**. Jeśli wybierzesz tę opcję, wprowadź **liczby godzin** w polu tekstowym. 

        ![Liczba godzin na użytkownika](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
    4. Wybierz pozycję **Zapisz**. 
5. Teraz widać zmienionymi wartościami na pasku narzędzi: **Limit przydziału dla poszczególnych użytkowników: &lt;liczby godzin&gt;** . 

    ![Limit przydziału dla poszczególnych użytkowników](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="set-a-schedule-for-the-lab"></a>Ustaw harmonogram dla laboratorium
Jeśli skonfigurowano ustawienie limitu przydziału, aby **0 godz. (tylko dla harmonogramu)** , musisz ustawić harmonogram dla laboratorium. W tym samouczku można ustawić harmonogram jako cykliczny harmonogram tygodniowy.

1. Przełącz się do **harmonogramy** strony i wybierz **harmonogramu Dodaj** na pasku narzędzi. 

    ![Dodaj przycisk harmonogramu na stronie harmonogramów](../media/how-to-create-schedules/add-schedule-button.png)
2. Na **harmonogramu Dodaj** strony, przełącz się do **tygodniowy** u góry. 
3. Aby uzyskać **zaplanować dni (wymagane)** , wybierz dni, w których ma harmonogram, aby zastosować zmiany. W poniższym przykładzie wybrano od poniedziałku do piątku. 
4. Dla **z** wprowadź **zaplanować Data rozpoczęcia** lub wybierz datę, wybierając **kalendarza** przycisku. To pole jest wymagane. 
5. Aby uzyskać **Data zakończenia harmonogramu**wprowadź lub wybierz datę zakończenia, w którym maszyny wirtualne mają być zamknięty. 
6. Aby uzyskać **czas rozpoczęcia**, wybierz godzinę maszyn wirtualnych ma zostać uruchomiony. Czas rozpoczęcia jest wymagany, jeśli nie ustawiono czas przestoju. Wybierz **Usuń zdarzenie początkowe** Jeśli chcesz określić tylko godzina zatrzymania. Jeśli **czas rozpoczęcia** jest wyłączona, wybierz **Dodaj zdarzenie rozpoczęcia** obok listy rozwijanej, aby go włączyć. 
7. Aby uzyskać **czas zakończenia**, wybierz godzinę maszyn wirtualnych można zamknąć. Czas przestoju jest wymagany, jeśli nie ustawiono czasu rozpoczęcia. Wybierz **zdarzenie stop usuwania** Jeśli chcesz określić tylko godziny rozpoczęcia. Jeśli **czas zakończenia** jest wyłączona, wybierz **zdarzenia zatrzymania Dodaj** obok listy rozwijanej, aby go włączyć.
8. Aby uzyskać **strefy czasowej (wymagane)** , wybierz strefę czasową uruchomienia i zatrzymania razy określono.  
9. Aby uzyskać **uwagi**, wprowadź opis lub notatki dla harmonogramu. 
10. Wybierz pozycję **Zapisz**. 

    ![Harmonogram tygodniowy](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="send-an-email-with-the-registration-link"></a>Wyślij wiadomość e-mail z linkiem rejestracji

1. Jeśli jeszcze nie jesteś na tej stronie, przełącz się do widoku **Użytkownicy**. 
2. Wybierz z listy określonych lub wszystkich użytkowników. Aby wybrać określonych użytkowników, zaznacz pola wyboru w kolumnie pierwszej listy. Aby wybrać wszystkich użytkowników, zaznacz pole wyboru przed tytuł pierwszą kolumnę (**nazwa**) lub zaznacz wszystkie pola wyboru dla wszystkich użytkowników na liście. Można wyświetlić stan **stanu zaproszenia** na tej liście.  Na poniższej ilustracji stan zaproszenie dla wszystkich studentów jest ustawiony na **zaproszenie nie zostało wysłane**. 

    ![Wybierz uczniów](../media/tutorial-setup-classroom-lab/select-students.png)
1. Wybierz **ikonę poczty e-mail (koperta)** na jeden z wierszy (lub) select **wysłać zaproszenia** na pasku narzędzi. Możesz również umieścić kursor myszy nazwę uczniów na liście, aby wyświetlić ikonę poczty e-mail. 

    ![Wyślij łącze rejestracji za pośrednictwem poczty e-mail](../media/tutorial-setup-classroom-lab/send-email.png)
4. Na **Wyślij link rejestracji za pośrednictwem poczty e-mail** strony, wykonaj następujące kroki: 
    1. Typ **opcjonalną wiadomość** , którą chcesz wysłać dla uczniów i studentów. Wiadomości e-mail automatycznie dołącza łącze rejestracji. 
    2. Na **Wyślij link rejestracji za pośrednictwem poczty e-mail** wybierz opcję **wysyłania**. Zostanie wyświetlony stan zaproszenia zmiana **wysyłanie zaproszenia** a **zaproszenie zostało wysłane**. 
        
        ![Zaproszenia wysłane](../media/tutorial-setup-classroom-lab/invitations-sent.png)

## <a name="next-steps"></a>Kolejne kroki
W tym samouczku utworzono i skonfigurowano laboratorium na potrzeby zajęć. Aby dowiedzieć się, jak uczeń może uzyskać dostęp do maszyny wirtualnej w laboratorium przy użyciu linku rejestracyjnego, przejdź do następnego samouczka:

> [!div class="nextstepaction"]
> [Nawiązywanie połączenia z maszyną wirtualną w laboratorium używanym podczas zajęć](tutorial-connect-virtual-machine-classroom-lab.md)

