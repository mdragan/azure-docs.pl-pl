---
title: Rozwiązywanie problemów z agentem systemu Linux analizy dzienników platformy Azure | Dokumentacja firmy Microsoft
description: Opisz objawy, przyczyny i rozwiązania typowych problemów z agentem usługi Log Analytics dla systemu Linux w usłudze Azure Monitor.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: 83f9cc050694344cdc5f4f5a2070bc875fcba3d9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071656"
---
# <a name="how-to-troubleshoot-issues-with-the-log-analytics-agent-for-linux"></a>Jak rozwiązywać problemy związane z agenta usługi Log Analytics dla systemu Linux 

Ten artykuł zawiera pomocy Rozwiązywanie problemów z błędami, które mogą wystąpić przy użyciu agenta usługi Log Analytics dla systemu Linux w usłudze Azure Monitor i sugeruje możliwe rozwiązania, aby je rozwiązać.

Jeśli żadna z powyższych czynności działa, następujących kanałów pomocy technicznej dostępne są również:

* Korzyści z pomocy technicznej klientom planu Premier mogą Otwórz żądanie obsługi z [Premier](https://premier.microsoft.com/).
* Klienci z umowami pomocy technicznej platformy Azure mogą otworzyć żądania pomocy technicznej [w witrynie Azure portal](https://manage.windowsazure.com/?getsupport=true).
* Diagnozowanie problemów OMI z [przewodnik rozwiązywania problemów OMI](https://github.com/Microsoft/omi/blob/master/Unix/doc/diagnose-omi-problems.md).
* Plik [problem w usłudze GitHub](https://github.com/Microsoft/OMS-Agent-for-Linux/issues).
* Odwiedź stronę Log Analytics opinii, aby Przegląd przesłane pomysły i usterek [ https://aka.ms/opinsightsfeedback ](https://aka.ms/opinsightsfeedback) lub nowy plik.  

## <a name="important-log-locations-and-log-collector-tool"></a>Narzędzie moduł zbierający i ważne dziennika lokalizacji

 Plik | Ścieżka
 ---- | -----
 Agent analizy dziennika dla pliku dziennika systemu Linux | `/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`
 Plik dziennika konfiguracji agenta analizy dzienników | `/var/opt/microsoft/omsconfig/omsconfig.log`

 Firma Microsoft zaleca używanie naszego narzędzia do modułu zbierającego dzienników do pobierania dzienników ważne podczas rozwiązywania problemów lub przed przesłaniem problem w usłudze GitHub. Możesz przeczytać więcej na temat narzędzia i sposobu jego uruchamiania [tutaj](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md).

## <a name="important-configuration-files"></a>Pliki konfiguracji ważne

 Kategoria | Lokalizacja pliku
 ----- | -----
 Dziennik systemu | `/etc/syslog-ng/syslog-ng.conf` lub `/etc/rsyslog.conf` lub `/etc/rsyslog.d/95-omsagent.conf`
 Wydajność, Nagios, Zabbix, danych wyjściowych usługi Log Analytics i ogólne agenta | `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`
 Dodatkowe konfiguracje | `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/*.conf`

 >[!NOTE]
 >Edycja plików konfiguracji, licznikach wydajności i Syslog jest zastępowany, jeśli kolekcja jest skonfigurowana z [danych menu Zaawansowane ustawienia usługi Analiza dzienników](../../azure-monitor/platform/agent-data-sources.md#configuring-data-sources) w witrynie Azure portal dla obszaru roboczego. Aby wyłączyć konfigurację dla wszystkich agentów, należy wyłączyć kolekcji z usługą Log Analytics **Zaawansowane ustawienia** lub jednego agenta, uruchom następujące polecenie:  
> `sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable`

## <a name="installation-error-codes"></a>Kody błędów instalacji

| Kod błędu | Znaczenie |
| --- | --- |
| NOT_DEFINED | Ponieważ nie są zainstalowane niezbędne zależności, wtyczka auditd auoms nie zostaną zainstalowane | Instalację auoms nie powiodło się, wykorzystują pakiet. |
| 2 | Nieprawidłowa opcja udostępniane przez pakiet powłoki. Uruchom `sudo sh ./omsagent-*.universal*.sh --help` za użycie |
| 3 | Brak opcji udostępniane przez pakiet powłoki. Uruchom `sudo sh ./omsagent-*.universal*.sh --help` do użycia. |
| 4 | Nieprawidłowy pakiet typu lub nieprawidłowe ustawienia serwera proxy; omsagent -*obr. / min*SH pakiety można zainstalować tylko na komputerach z systemem obr. / min, a następnie omsagent*deb*SH pakiety można zainstalować tylko w systemach oparta na rozwiązaniu Debian. Jest zaleca się używać universal Instalatora z [najnowszej wersji](../../azure-monitor/learn/quick-collect-linux-computer.md#install-the-agent-for-linux). Sprawdź również, aby sprawdzić ustawienia serwera proxy. |
| 5 | Pakiet shell musi zostać wykonana jako główny lub wystąpił błąd 403 zwrócił zestawu dokumentacji podczas dołączania. Uruchamianie przy użyciu polecenia `sudo`. |
| 6 | Nieprawidłowy pakiet architektury lub wystąpił błąd błąd 200 zwrócił zestawu dokumentacji podczas dołączania; omsagent -*x64.sh pakiety można zainstalować tylko w systemach 64-bitowych, a następnie omsagent*x86.sh pakiety można zainstalować tylko w systemach 32-bitowych. Pobierz właściwy pakiet dla architektury z [najnowszej wersji](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/latest). |
| 17 | Nie można zainstalować pakietu OMS. Przejrzyj dane wyjściowe polecenia niepowodzenia głównego. |
| 19 | Nie można zainstalować pakiet OMI. Przejrzyj dane wyjściowe polecenia niepowodzenia głównego. |
| 20 | Instalacja pakietu SCX nie powiodło się. Przejrzyj dane wyjściowe polecenia niepowodzenia głównego. |
| 21 | Nie można zainstalować dostawcy zestawów. Przejrzyj dane wyjściowe polecenia niepowodzenia głównego. |
| 22 | Nie można zainstalować pakietu powiązane. Przejrzyj dane wyjściowe polecenia niepowodzenia głównego |
| 23 | Pakiet SCX lub OMI już zainstalowane. Użyj `--upgrade` zamiast `--install` do zainstalowania pakietu powłoki. |
| 30 | Błąd wewnętrzny pakiet. Plik [problem w usłudze GitHub](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) szczegółowe informacje z danych wyjściowych. |
| 55 | Openssl nieobsługiwana wersja lub nie można nawiązać połączenie z usługi Azure Monitor lub serwerach jest zablokowany lub brak curl program. |
| 61 | Brak biblioteki ctypes Python. Zainstaluj biblioteki ctypes Python lub pakietów (python ctypes). |
| 62 | Brakujący program docelowy, tar instalacji. |
| 63 | Brakujący program sed, sed. instalacji |
| 64 | Brakujący program curl, zainstaluj narzędzie curl. |
| 65 | Brakujący program gpg, gpg instalacji. |

## <a name="onboarding-error-codes"></a>Kody błędów dołączania

| Kod błędu | Znaczenie |
| --- | --- |
| 2 | Nieprawidłowa opcja przekazywane do skryptu omsadmin. Uruchom `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -h` do użycia. |
| 3 | Nieprawidłowa konfiguracja przekazywane do skryptu omsadmin. Uruchom `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -h` do użycia. |
| 4 | Nieprawidłowy serwer proxy, przekazywane do skryptu omsadmin. Sprawdź serwer proxy, a następnie zobacz nasze [dokumentacji przy użyciu serwera proxy HTTP](log-analytics-agent.md#network-firewall-requirements). |
| 5 | Błąd HTTP 403 odebranych z usługi Azure Monitor. Zobacz pełne dane wyjściowe skryptu omsadmin, aby uzyskać szczegółowe informacje. |
| 6 | Błąd HTTP inne niż 200 odebranych z usługi Azure Monitor. Zobacz pełne dane wyjściowe skryptu omsadmin, aby uzyskać szczegółowe informacje. |
| 7 | Nie można nawiązać połączenia z usługi Azure Monitor. Zobacz pełne dane wyjściowe skryptu omsadmin, aby uzyskać szczegółowe informacje. |
| 8 | Błąd dołączania do obszaru roboczego usługi Log Analytics. Zobacz pełne dane wyjściowe skryptu omsadmin, aby uzyskać szczegółowe informacje. |
| 30 | Wewnętrzny błąd skryptu. Plik [problem w usłudze GitHub](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) szczegółowe informacje z danych wyjściowych. |
| 31 | Błąd podczas generowania agenta identyfikatora. Plik [problem w usłudze GitHub](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) szczegółowe informacje z danych wyjściowych. |
| 32 | Wystąpił błąd podczas generowania certyfikatów. Zobacz pełne dane wyjściowe skryptu omsadmin, aby uzyskać szczegółowe informacje. |
| 33 | Błąd podczas generowania metaconfiguration dla omsconfig. Plik [problem w usłudze GitHub](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) szczegółowe informacje z danych wyjściowych. |
| 34 | Metaconfiguration generowania skryptu nie istnieje. Ponów próbę wykonania dołączania przy użyciu `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -w <Workspace ID> -s <Workspace Key>`. |

## <a name="enable-debug-logging"></a>Włączenie rejestrowania debugowania
### <a name="oms-output-plugin-debug"></a>Debugowanie wtyczki dane wyjściowe pakietu OMS
 Fluentd umożliwiający umożliwia poziomów rejestrowania specyficznych dla wtyczki, dzięki czemu możesz określić poziomy dziennika dla danych wejściowych i wyjściowych. Do określenia poziomu dziennika pakietu OMS w danych wyjściowych, zmień konfigurację agenta ogólne na `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`.  

 We wtyczce dane wyjściowe pakietu OMS, przed końcem pliku konfiguracji, należy zmienić `log_level` właściwość `info` do `debug`:

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

Rejestrowanie debugowania umożliwia zobaczenie wsadowej przekazywanie do usługi Azure Monitor rozdzielone typ, liczbę elementów danych i czas potrzebny do wysyłania:

*Przykładowy dziennik debugowania włączone:*

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

### <a name="verbose-output"></a>Pełne dane wyjściowe
Zamiast korzystania z wtyczki dane wyjściowe pakietu OMS mogą również danych wyjściowych elementów danych bezpośrednio do `stdout`, która jest widoczna w agenta usługi Log Analytics dla pliku dziennika systemu Linux.

W pliku konfiguracyjnym ogólne agenta usługi Log Analytics w `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, wtyczka danych wyjściowych komentarz pakietu OMS, dodając `#` przed każdym wierszu:

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

Poniższe dane wyjściowe wtyczki, Usuń komentarz poniższej sekcji, usuwając `#` przed każdym wierszu:

```
<match **>
  type stdout
</match>
```

## <a name="issue--unable-to-connect-through-proxy-to-azure-monitor"></a>Problem:  Nie można połączyć za pośrednictwem serwera proxy do usługi Azure Monitor

### <a name="probable-causes"></a>Prawdopodobne przyczyny
* Nieprawidłowy serwer proxy określony zestawu dokumentacji podczas dołączania
* Usługi Azure Monitor i Azure Automation — punkty końcowe usługi nie są na liście dozwolonych w centrum danych 

### <a name="resolution"></a>Rozwiązanie
1. Reonboard do usługi Azure Monitor za pomocą agenta usługi Log Analytics dla systemu Linux przy użyciu następującego polecenia z opcją `-v` włączone. Umożliwia ona pełne dane wyjściowe z agenta, łącząc się za pośrednictwem serwera proxy do usługi Azure Monitor. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <Workspace ID> -s <Workspace Key> -p <Proxy Conf> -v`

2. Zapoznaj się z sekcją [zaktualizować ustawień serwera proxy](agent-manage.md#update-proxy-settings) można sprawdzić zostało poprawnie skonfigurowane agenta do komunikowania się za pośrednictwem serwera proxy.    
* Sprawdź, czy za pomocą następujących punktów końcowych usługi Azure Monitor na liście dozwolonych:

    |Zasób agenta| Porty | Kierunek |
    |------|---------|----------|  
    |*.ods.opinsights.azure.com | Port 443| Dla ruchu przychodzącego i wychodzącego |  
    |*.oms.opinsights.azure.com | Port 443| Dla ruchu przychodzącego i wychodzącego |  
    |*.blob.core.windows.net | Port 443| Dla ruchu przychodzącego i wychodzącego |  
    |*.azure-automation.net | Port 443| Dla ruchu przychodzącego i wychodzącego | 

## <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Problem: Komunikat o błędzie 403 podczas próby dołączyć

### <a name="probable-causes"></a>Prawdopodobne przyczyny
* Data i godzina jest nieprawidłowa na serwerze z systemem Linux 
* Identyfikator obszaru roboczego i klucz obszaru roboczego, używane są niepoprawne

### <a name="resolution"></a>Rozwiązanie

1. Sprawdź czas na serwerze Linux z datą polecenia. Jeśli czas +/-15 minut od bieżącego czasu dołączania kończy się niepowodzeniem. Aby poprawne to zaktualizuj datę i/lub strefy czasowej serwera systemu Linux. 
2. Sprawdź, czy zainstalowano najnowszą wersję agenta usługi Log Analytics dla systemu Linux.  Najnowszą wersję teraz powiadamia, że możesz Jeśli czasowego powoduje błąd dołączania.
3. Reonboard przy użyciu poprawny identyfikator obszaru roboczego i klucz obszaru roboczego, postępując zgodnie z instrukcjami instalacji we wcześniejszej części tego artykułu.

## <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Problem: Zostanie wyświetlony bezpośrednio po dołączeniu 500 i 404 błąd w pliku dziennika
Jest to znany problem występujący w pierwszym przekazywania danych z systemem Linux do obszaru roboczego usługi Log Analytics. Nie dotyczy to danych wysłanego lub usługi.


## <a name="issue-you-see-omiagent-using-100-cpu"></a>Problem: Zobacz omiagent przy użyciu procesora CPU w 100%

### <a name="probable-causes"></a>Prawdopodobne przyczyny
Regresja w pakiecie nss pem [v1.0.3 5.el7](https://centos.pkgs.org/7/centos-x86_64/nss-pem-1.0.3-5.el7.x86_64.rpm.html) przyczyną problemów z wydajnością poważny, który firma Microsoft została chwilą pojawiają się znacznie w Redhat/Centos 7.x dystrybucji. Aby dowiedzieć się więcej na temat tego problemu, zapoznaj się z dokumentacją następujące: Błąd [1667121 regresji wydajności w libcurl](https://bugzilla.redhat.com/show_bug.cgi?id=1667121).

Błędy nie występują przez cały czas i są bardzo trudne do odtworzenia związanych z wydajnością. Jeśli wystąpi taki problem z omiagent należy użyć omiHighCPUDiagnostics.sh skrypt, który będzie zbierać ślad stosu omiagent, po przekroczeniu określonego progu.

1. Pobierz skrypt <br/>
`wget https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/tools/LogCollector/source/omiHighCPUDiagnostics.sh`

2. Uruchom diagnostykę przez 24 godziny, o 30% progu procesora CPU <br/>
`bash omiHighCPUDiagnostics.sh --runtime-in-min 1440 --cpu-threshold 30`

3. Stos wywołań będzie można utworzyć zrzutu w pliku omiagent_trace, jeśli można zauważyć wiele Curl oraz wywołań funkcji NSS, wykonaj poniższe kroki rozwiązania.

### <a name="resolution-step-by-step"></a>Rozpoznawanie (krok po kroku)

1. Uaktualnij pakiet nss pem do [v1.0.3 5.el7_6.1](https://centos.pkgs.org/7/centos-updates-x86_64/nss-pem-1.0.3-5.el7_6.1.x86_64.rpm.html). <br/>
`sudo yum upgrade nss-pem`

2. Jeśli nie jest dostępna dla uaktualnienie nss pem (przede wszystkim ma miejsce na Centos), następnie starszą wersję programu curl do 7.29.0-46. Jeśli przez pomyłkę Uruchom "yum update", a następnie curl zostaną uaktualnione do 7.29.0-51 i problemu spowoduje ponowne wystąpienie. <br/>
`sudo yum downgrade curl libcurl`

3. Uruchom ponownie OMI: <br/>
`sudo scxadmin -restart`

## <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Problem: Nie widzisz żadnych danych w witrynie Azure portal

### <a name="probable-causes"></a>Prawdopodobne przyczyny

- Dołączanie do usługi Azure Monitor nie powiodło się.
- Połączenie do usługi Azure Monitor jest zablokowane
- Log Analytics agent dla danych z systemem Linux jest wykonywana kopia zapasowa

### <a name="resolution"></a>Rozwiązanie
1. Sprawdź, czy dołączania do usługi Azure Monitor powiodła się, sprawdzając, jeśli istnieje następujący plik: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Za pomocą Reonboard `omsadmin.sh` instrukcje wiersza polecenia
3. Jeśli używasz serwera proxy, skorzystaj z procedury opisanej rozpoznawanie serwera proxy, podany wcześniej.
4. W niektórych przypadkach gdy agenta usługi Log Analytics dla systemu Linux nie może komunikować się z usługą, dane na agencie znajduje się w kolejce do rozmiaru buforu pełnej, czyli 50 MB. Agent należy ponownie uruchomić, uruchamiając następujące polecenie: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Ten problem jest rozwiązany w 1.1.0-28 wersji agenta i nowszych wersjach.


## <a name="issue-you-are-not-seeing-forwarded-syslog-messages"></a>Problem: Nie widzisz przekazywanych dalej komunikatów usługi Syslog 

### <a name="probable-causes"></a>Prawdopodobne przyczyny
* Konfiguracja stosowany na serwerze z systemem Linux nie zezwala na zbiór wysłane urządzeń i/lub poziomy dziennika
* SYSLOG nie jest przesyłane dalej poprawnie na serwerze z systemem Linux
* Liczba komunikatów przesyłanych na sekundę są zbyt duże, podstawowej konfiguracji agenta usługi Log Analytics dla systemu Linux w celu obsługi

### <a name="resolution"></a>Rozwiązanie
* Sprawdź, czy konfiguracja w obszarze roboczym usługi Log Analytics dziennika systemowego zawiera wszystkich urządzeń i poziomy dziennika prawidłowy. Przegląd [skonfigurować zbieranie dzienników systemowych w witrynie Azure portal](../../azure-monitor/platform/data-sources-syslog.md#configure-syslog-in-the-azure-portal)
* Sprawdź natywnych dziennika systemu obsługi komunikatów demonów (`rsyslog`, `syslog-ng`) mogą odbierać wiadomości przesyłanych dalej
* Sprawdź ustawienia zapory na serwerze Syslog, aby upewnić się, że komunikaty nie są blokowane
* Symulowanie komunikat dziennika systemu do usługi Log Analytics przy użyciu `logger` polecenia
  * `logger -p local0.err "This is my test message"`

## <a name="issue-you-are-receiving-errno-address-already-in-use-in-omsagent-log-file"></a>Problem: Otrzymujesz adres Errno już używane w pliku dziennika omsagent
Jeśli widzisz `[error]: unexpected error error_class=Errno::EADDRINUSE error=#<Errno::EADDRINUSE: Address already in use - bind(2) for "127.0.0.1" port 25224>` w omsagent.log.

### <a name="probable-causes"></a>Prawdopodobne przyczyny
Ten błąd wskazuje, że rozszerzenie diagnostyczne systemu Linux (LAD) jest instalowany obok rozszerzenia maszyny Wirtualnej systemu Linux programu Log Analytics i używa tego samego portu usługi syslog zbierania danych jako omsagent.

### <a name="resolution"></a>Rozwiązanie
1. Jako użytkownik główny wykonaj następujące polecenia (Zauważ, że 25224 znajduje się przykład i jest to możliwe, że w środowisku zostanie wyświetlony inny numer portu używany przez LAD):

    ```
    /opt/microsoft/omsagent/bin/configure_syslog.sh configure LAD 25229

    sed -i -e 's/25224/25229/' /etc/opt/microsoft/omsagent/LAD/conf/omsagent.d/syslog.conf
    ```

    Następnie trzeba edytować poprawny `rsyslogd` lub `syslog_ng` konfiguracji pliku i zmiany konfiguracji odnoszące się do LAD do zapisu do portu 25229.

2. Jeśli maszyna wirtualna jest uruchomiona `rsyslogd`, plik, który ma zostać zmodyfikowana jest: `/etc/rsyslog.d/95-omsagent.conf` (jeśli istnieje inne `/etc/rsyslog`). Jeśli maszyna wirtualna jest uruchomiona `syslog_ng`, plik, który ma zostać zmodyfikowana jest: `/etc/syslog-ng/syslog-ng.conf`.
3. Uruchom ponownie omsagent `sudo /opt/microsoft/omsagent/bin/service_control restart`.
4. Uruchom ponownie usługę syslog.

## <a name="issue-you-are-unable-to-uninstall-omsagent-using-purge-option"></a>Problem: Nie można odinstalować za pomocą opcji przeczyszczania omsagent

### <a name="probable-causes"></a>Prawdopodobne przyczyny

* Zainstalowano rozszerzenia diagnostycznego systemu Linux
* Rozszerzenia diagnostycznego systemu Linux była instalowana i odinstalowywana, ale nadal wyświetlony komunikat o błędzie o omsagent używany przez mdsd i nie można go usunąć.

### <a name="resolution"></a>Rozwiązanie
1. Odinstaluj rozszerzenie diagnostyczne systemu Linux (LAD).
2. Usuń pliki rozszerzenia diagnostycznego systemu Linux na tej maszynie, jeśli są obecne w następującej lokalizacji: `/var/lib/waagent/Microsoft.Azure.Diagnostics.LinuxDiagnostic-<version>/` i `/var/opt/microsoft/omsagent/LAD/`.

## <a name="issue-you-cannot-see-data-any-nagios-data"></a>Problem: Dane nie zobaczą żadnych danych Nagios 

### <a name="probable-causes"></a>Prawdopodobne przyczyny
* Omsagent użytkownik nie ma uprawnień do odczytu z pliku dziennika Nagios
* Źródło Nagios i filtr nie zostały jeszcze pozbawionym znaków komentarza wierszu z pliku omsagent.conf

### <a name="resolution"></a>Rozwiązanie
1. Dodaj użytkownika omsagent można odczytać z pliku Nagios, wykonując te [instrukcje](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts).
2. Agenta usługi Log Analytics dla pliku konfiguracji ogólnej systemu Linux w `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, upewnij się, że **zarówno** źródłowy Nagios filtru jest pozbawionym znaków komentarza wierszu.

    ```
    <source>
      type tail
      path /var/log/nagios/nagios.log
      format none
      tag oms.nagios
    </source>

    <filter oms.nagios>
      type filter_nagios_log
    </filter>
    ```

## <a name="issue-you-are-not-seeing-any-linux-data"></a>Problem: Nie widzisz żadnych danych z systemem Linux 

### <a name="probable-causes"></a>Prawdopodobne przyczyny
* Dołączanie do usługi Azure Monitor nie powiodło się.
* Połączenie do usługi Azure Monitor jest zablokowane
* Maszyna wirtualna została uruchomiona ponownie.
* Pakiet OMI ręcznie został uaktualniony do nowszej wersji w porównaniu do czego została zainstalowana przez agenta usługi Log Analytics dla pakietu systemu Linux
* Dzienniki zasobów DSC *nie znaleziono klasy* błąd `omsconfig.log` pliku dziennika
* Agent analizy dziennika dla danych jest wykonywana kopia zapasowa
* Dzienniki DSC *bieżącej konfiguracji nie istnieje. Wykonaj polecenie Start-DscConfiguration z - parametru Path, aby określić plik konfiguracji i najpierw utworzyć bieżącej konfiguracji.* w `omsconfig.log` pliku dziennika, ale żaden komunikat dziennika istnieje o `PerformRequiredConfigurationChecks` operacji.

### <a name="resolution"></a>Rozwiązanie
1. Zainstaluj wszystkie zależności, takie jak wykorzystują pakiet.
2. Sprawdź, czy dołączenie do usługi Azure Monitor powiodła się, sprawdzając, jeśli istnieje następujący plik: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.  Jeśli nie, przy użyciu wiersza polecenia omsadmin.sh reonboard [instrukcje](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line).
4. Jeśli używasz serwera proxy, sprawdź Rozwiązywanie problemów z powyższych czynności serwer proxy.
5. W niektórych systemach dystrybucji platformy Azure demon serwer OMI omid nie uruchamia się po uruchomieniu maszyny wirtualnej. W rezultacie zostanie nie widzisz danych inspekcji, śledzenia zmian lub UpdateManagement związane z rozwiązania. Należy ręcznie uruchomić serwer omi, uruchamiając `sudo /opt/omi/bin/service_control restart`.
6. Pakiet OMI jest ręcznie uaktualnione do nowszej wersji, musi on, należy ręcznie uruchomić ponownie agenta usługi Log Analytics kontynuować działanie. Ten krok jest wymagany dla niektórych dystrybucjach, w którym serwer OMI nie zostanie uruchomiona automatycznie po uaktualnieniu. Uruchom `sudo /opt/omi/bin/service_control restart` ponownego uruchomienia OMI.
7. Jeśli widzisz zasobów DSC *nie znaleziono klasy* błąd omsconfig.log, uruchom `sudo /opt/omi/bin/service_control restart`.
8. W niektórych przypadkach gdy agenta usługi Log Analytics dla systemu Linux nie może komunikować się z usługi Azure Monitor danych na agencie kopia zapasowa jest tworzona z rozmiarem buforu pełnej: 50 MB. Agent należy ponownie uruchomić, uruchamiając następujące polecenie `/opt/microsoft/omsagent/bin/service_control restart`.

    >[!NOTE]
    >Ten problem został rozwiązany w 1.1.0-28 wersji agenta lub nowszym
    >

* Jeśli `omsconfig.log` pliku dziennika nie wskazują, że `PerformRequiredConfigurationChecks` operacje są okresowo uruchomionych w systemie, może to być problem z zadania/usługi cron. Upewnij się, zadanie cron istnieje w ramach `/etc/cron.d/OMSConsistencyInvoker`. Jeśli to konieczne, uruchom następujące polecenia, aby utworzyć zadanie cron:

    ```
    mkdir -p /etc/cron.d/
    echo "*/15 * * * * omsagent /opt/omi/bin/OMSConsistencyInvoker >/dev/null 2>&1" | sudo tee /etc/cron.d/OMSConsistencyInvoker
    ```

    Ponadto upewnij się, że usługi cron jest uruchomiony. Możesz użyć `service cron status` Debian, Ubuntu, SUSE, lub `service crond status` z systemem RHEL, CentOS, Oracle Linux, aby sprawdzić stan tej usługi. Jeśli usługa nie istnieje, możesz zainstalować pliki binarne i uruchom usługę przy użyciu następujących:

    **Ubuntu/Debian**

    ```
    # To Install the service binaries
    sudo apt-get install -y cron
    # To start the service
    sudo service cron start
    ```

    **SUSE**

    ```
    # To Install the service binaries
    sudo zypper in cron -y
    # To start the service
    sudo systemctl enable cron
    sudo systemctl start cron
    ```

    **RHEL/CeonOS**

    ```
    # To Install the service binaries
    sudo yum install -y crond
    # To start the service
    sudo service crond start
    ```

    **Oracle Linux**

    ```
    # To Install the service binaries
    sudo yum install -y cronie
    # To start the service
    sudo service crond start
    ```

## <a name="issue-when-configuring-collection-from-the-portal-for-syslog-or-linux-performance-counters-the-settings-are-not-applied"></a>Problem: Podczas konfigurowania kolekcji z poziomu portalu, liczniki wydajności protokołu Syslog lub Linux, nie są stosowane ustawienia

### <a name="probable-causes"></a>Prawdopodobne przyczyny
* Agenta usługi Log Analytics dla systemu Linux nie pobierane najnowszą konfiguracją
* Zmiany ustawień w portalu nie zostały zastosowane.

### <a name="resolution"></a>Rozwiązanie
**Tło:** `omsconfig` jest agenta usługi Log Analytics dla systemu Linux konfiguracji agenta, który wyszukuje nowy portal — konfiguracja po stronie co pięć minut. Ta konfiguracja jest następnie stosowane do agenta usługi Log Analytics dla systemu Linux plików konfiguracyjnych, znajdujący się w /etc/opt/microsoft/omsagent/conf/omsagent.conf.

* W niektórych przypadkach agenta usługi Log Analytics dla systemu Linux konfiguracji agenta może nie móc komunikować się z usługą konfiguracji portalu skutkuje najnowszą konfigurację, które nie są stosowane.
  1. Sprawdź, czy `omsconfig` agent jest zainstalowany, uruchamiając `dpkg --list omsconfig` lub `rpm -qi omsconfig`.  Jeśli nie jest zainstalowany, zainstaluj ponownie najnowszą wersję agenta usługi Log Analytics dla systemu Linux.

  2. Sprawdź, czy `omsconfig` agenta może komunikować się z usługą Azure Monitor, uruchamiając następujące polecenie `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'`. To polecenie zwraca konfigurację agenta otrzymuje z usługi, w tym ustawień usługi Syslog, liczniki wydajności systemu Linux i dzienników niestandardowych. Jeśli to polecenie nie powiedzie się, uruchom następujące polecenie `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py'`. To polecenie wymusza omsconfig agentowi komunikować się z usługi Azure Monitor i pobrania najnowszej konfiguracji.

## <a name="issue-you-are-not-seeing-any-custom-log-data"></a>Problem: Nie widzisz żadnych niestandardowych danych dziennika 

### <a name="probable-causes"></a>Prawdopodobne przyczyny
* Dołączanie do usługi Azure Monitor nie powiodło się.
* Ustawienie **Zastosuj poniższą konfigurację do serwerów z systemem Linux** nie został wybrany.
* omsconfig nie jest pobierane z ostatnią konfiguracją dzienników niestandardowych z usługi.
* Log Analytics agent dla użytkownika w systemie Linux `omsagent` nie może uzyskać dostępu do dzienników niestandardowych z powodu uprawnień lub nie znaleziono.  Może pojawić się następujące błędy:
 * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
 * `[DATETIME] [error]: file not accessible by omsagent.`
* Znany problem z rozwiązane w agenta usługi Log Analytics dla systemu Linux w wersji 1.1.0-217 sytuacja wyścigu

### <a name="resolution"></a>Rozwiązanie
1. Sprawdź, dołączania do usługi Azure Monitor zakończyło się pomyślnie, sprawdzając, jeśli istnieje następujący plik: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`. Jeśli nie, albo:  

  1. Przy użyciu wiersza polecenia omsadmin.sh Reonboard [instrukcje](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line).
  2. W obszarze **Zaawansowane ustawienia** w witrynie Azure portal, upewnij się, że ustawienie **Zastosuj poniższą konfigurację do serwerów z systemem Linux** jest włączona.  

2. Sprawdź, czy `omsconfig` agenta może komunikować się z usługą Azure Monitor, uruchamiając następujące polecenie `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'`.  To polecenie zwraca konfigurację agenta otrzymuje z usługi, w tym ustawień usługi Syslog, liczniki wydajności systemu Linux i dzienników niestandardowych. Jeśli to polecenie nie powiedzie się, uruchom następujące polecenie `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py`. To polecenie wymusza omsconfig agentowi komunikować się z usługi Azure Monitor i pobrania najnowszej konfiguracji.

**Tło:** Zamiast agenta usługi Log Analytics dla systemu Linux uruchomiony jako uprawnionego użytkownika - `root`, agent działa jako `omsagent` użytkownika. W większości przypadków można udzielić jawne uprawnienia tego użytkownika w kolejności, w przypadku niektórych plików do odczytu. Aby udzielić uprawnień do `omsagent` użytkownika, uruchom następujące polecenia:

1. Dodaj `omsagent` użytkownika do określonej grupy `sudo usermod -a -G <GROUPNAME> <USERNAME>`
2. Uniwersalny dostęp do wymaganego pliku `sudo chmod -R ugo+rx <FILE DIRECTORY>`

Istnieje znany problem z sytuacja wyścigu przy użyciu agenta usługi Log Analytics dla systemu Linux w wersji wcześniejszej niż 1.1.0-217. Po zaktualizowaniu do najnowszego agenta, uruchom następujące polecenie, aby pobrać najnowszą wersję dodatku dane wyjściowe `sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`.

## <a name="issue-you-are-trying-to-reonboard-to-a-new-workspace"></a>Problem: Próbujesz reonboard z nowym obszarem roboczym
Podczas próby reonboard agenta pod kątem nowego obszaru roboczego, konfigurację agenta usługi Log Analytics musi być oczyszczone przed reonboarding. Aby wyczyścić stare konfiguracji z poziomu agenta, jest uruchamiany pakiet shell przy użyciu `--purge`

```
sudo sh ./omsagent-*.universal.x64.sh --purge
```
Lub

```
sudo sh ./onboard_agent.sh --purge
```

Możesz kontynuować reonboard po użyciu `--purge` opcji

## <a name="log-analytics-agent-extension-in-the-azure-portal-is-marked-with-a-failed-state-provisioning-failed"></a>Rozszerzenia log Analytics agent extension w witrynie Azure portal jest oznaczona stanem nie powiodło się: Aprowizowanie nie powiodło się

### <a name="probable-causes"></a>Prawdopodobne przyczyny
* Log Analytics agent został usunięty z systemu operacyjnego
* Usługę agenta usługi log Analytics nie działa, wyłączone lub nieskonfigurowane

### <a name="resolution"></a>Rozwiązanie 
Wykonaj poniższe kroki, aby rozwiązać ten problem.
1. Usuń rozszerzenie z witryny Azure portal.
2. Zainstalować agenta po [instrukcje](../../azure-monitor/learn/quick-collect-linux-computer.md).
3. Uruchom ponownie agenta, uruchamiając następujące polecenie: `sudo /opt/microsoft/omsagent/bin/service_control restart`.
* Poczekaj kilka minut i aprowizowania stan zmieni się na **Aprowizowanie zakończyło się pomyślnie**.


## <a name="issue-the-log-analytics-agent-upgrade-on-demand"></a>Problem: Usługa Log Analytics agent uaktualniania na żądanie

### <a name="probable-causes"></a>Prawdopodobne przyczyny

Pakiety agentów usługi Log Analytics na hoście są nieaktualne.

### <a name="resolution"></a>Rozwiązanie 
Wykonaj poniższe kroki, aby rozwiązać ten problem.

1. Sprawdź najnowszą wersją na [strony](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/).
2. Pobierz skrypt instalacji (1.4.2-124 jako przykład wersję):

    ```
    wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.2-124/omsagent-1.4.2-124.universal.x64.sh
    ```

3. Uaktualnij pakiety, wykonując `sudo sh ./omsagent-*.universal.x64.sh --upgrade`.
