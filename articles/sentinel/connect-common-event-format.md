---
title: Połącz dane CEF przez wartownika platformy Azure w wersji zapoznawczej | Dokumentacja firmy Microsoft
description: Dowiedz się, jak połączyć dane CEF z platformy Azure przez wartownika.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: cbf5003b-76cf-446f-adb6-6d816beca70f
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2019
ms.author: rkarlin
ms.openlocfilehash: 8e711c0586ce63d4293e2fb0914bbe884b55971f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389965"
---
# <a name="connect-your-external-solution-using-common-event-format"></a>Połączenia zewnętrzne rozwiązania przy użyciu Common Event Format

> [!IMPORTANT]
> Wartownik platformy Azure jest obecnie dostępna w publicznej wersji zapoznawczej.
> Ta wersja zapoznawcza nie jest objęta umową dotyczącą poziomu usług i nie zalecamy korzystania z niej w przypadku obciążeń produkcyjnych. Niektóre funkcje mogą być nieobsługiwane lub ograniczone. Aby uzyskać więcej informacji, zobacz [Uzupełniające warunki korzystania z wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Możesz połączyć przez wartownika platformy Azure, za pomocą rozwiązania zewnętrznego, który umożliwia zapisanie plików dziennika usługi SYSLOG. Jeśli urządzenie umożliwia zapisanie dzienniki jako Syslog Common Event Format (CEF), integracji z platformy Azure przez wartownika umożliwia w prosty sposób uruchamiaj analizy i zapytań względem danych.

> [!NOTE] 
> Dane są przechowywane w lokalizacji geograficznej w obszarze roboczym, na którym są uruchomione przez wartownika platformy Azure.

## <a name="how-it-works"></a>Jak to działa

Połączenie między przez wartownika platformy Azure i urządzeniem w formacie CEF odbywa się w trzech krokach:

1. Na urządzeniu, należy ustawić te wartości, tak aby urządzenie wysyła niezbędnych dzienników w odpowiednim formacie z agentem Azure przez wartownika Syslog. Tak długo, jak również zmodyfikować je w demona dziennika systemu Azure przez wartownika agenta, w której znajduje się urządzenie, można zmodyfikować te parametry.
    - Protokół = UDP
    - Port = 514
    - Funkcji = lokalne 4
    - Format = CEF
2. Agent usługi Syslog zbiera dane i bezpiecznie wysyła je do usługi Log Analytics, gdzie zostanie przeanalizowana i wzbogacony.
3. Agent przechowuje dane w obszarze roboczym usługi Log Analytics, dzięki czemu mogą być wyszukiwane zgodnie z potrzebami, korzystając z analizy, reguły korelacji i pulpitów nawigacyjnych.

> [!NOTE]
> Agent może zbierać dzienniki z wielu źródeł, ale musi być zainstalowany na komputerze dedykowanego serwera proxy.

## <a name="step-1-connect-to-your-cef-appliance-via-dedicated-azure-vm"></a>Krok 1: Łączenie do urządzenia w formacie CEF za pośrednictwem dedykowanej maszynie Wirtualnej platformy Azure

Należy wdrożyć agenta na dedykowanej maszynie z systemem Linux (maszyny Wirtualnej lub lokalnie) na potrzeby obsługi komunikacji między urządzeniem i przez wartownika Azure. Agenta można wdrożyć automatycznie lub ręcznie. Automatyczne wdrażanie jest oparte na szablonach usługi Resource Manager i mogą służyć tylko wtedy, gdy tworzysz na platformie Azure nowej maszyny Wirtualnej dedykowanej maszynie z systemem Linux.

 ![CEF na platformie Azure](./media/connect-cef/cef-syslog-azure.png)

Alternatywnie można wdrożyć agenta ręcznie na istniejącej Maszynie wirtualnej platformy Azure, na maszynie Wirtualnej w innej chmurze lub na maszynie lokalnej. 

 ![CEF w środowisku lokalnym](./media/connect-cef/cef-syslog-onprem.png)

### <a name="deploy-the-agent-in-azure"></a>Wdrażanie agenta w systemie Azure


1. W portalu Azure przez wartownika kliknij **łączników danych** i wybierz typ urządzenia. 

1. W obszarze **konfiguracji agenta systemu Linux Syslog**:
   - Wybierz **wdrażania automatycznego** Jeśli chcesz utworzyć nową maszynę, jest wstępnie zainstalowany za pomocą agenta usługi Azure przez wartownika, która obejmuje wszystkie niezbędne konfiguracji zgodnie z powyższym opisem. Wybierz **wdrażania automatycznego** i kliknij przycisk **wdrożenia agentami automatycznymi**. Spowoduje to przejście do strony zakupu dla dedykowanych maszyny Wirtualnej systemu Linux, który jest automatycznie połączony z obszarem roboczym. Maszyna wirtualna jest **standardowa D2s v3 (2 procesorów wirtualnych Vcpu, 8 GB pamięci RAM)** i ma publiczny adres IP.
      1. W **wdrożenie niestandardowe** strony, zapewniają szczegółowe informacje i wybierz nazwę użytkownika i hasło i jeśli zgadzasz się na warunki i postanowienia, zakup maszyny Wirtualnej.
      1. Konfigurowanie urządzenia w taki sposób, aby wysłać dzienniki przy użyciu ustawień wymienionych w na stronie połączenia. Łącznik ogólnego Common Event Format Użyj następujących ustawień:
         - Protokół = UDP
         - Port = 514
         - Funkcji = lokalne 4
         - Format = CEF
   - Wybierz **ręcznego wdrażania** Jeśli chcesz użyć istniejącej maszyny Wirtualnej jako dedykowane maszyny z systemem Linux, na którym powinien być zainstalowany agent usługi Azure przez wartownika. 
      1. W obszarze **pobrać i zainstalować agenta programu Syslog**, wybierz opcję **maszyny wirtualnej z systemem Linux platformy Azure**. 
      1. W **maszyn wirtualnych** ekran, który zostanie otwarty, wybierz maszynę, o których chcesz użyć, a następnie kliknij przycisk **Connect**.
      1. Na ekranie łącznika w obszarze **Konfiguruj i przekazywania usługi Syslog**Ustaw czy demona usługi Syslog jest **rsyslog.d** lub **demona syslog-ng**. 
      1. Skopiuj te polecenia i uruchamiać je na urządzeniu:
          - W przypadku wybrania rsyslog.d:
              
            1. Poinformuj demona dziennika systemu do nasłuchiwania local_4 funkcji i wysłać komunikaty dziennika systemu do platformy Azure przez wartownika agenta przy użyciu portu 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
            2. Pobierz i zainstaluj [pliku konfiguracyjnego security_events](https://aka.ms/asi-syslog-config-file-linux) , konfiguruje agenta usługi Syslog do nasłuchiwania na porcie 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Gdzie {0} należy zamienić na GUID obszaru roboczego.
            
            1. Uruchom ponownie demona usługi syslog `sudo service rsyslog restart`<br> Aby uzyskać więcej informacji, zobacz [rsyslog dokumentacji](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html)
           
          - W przypadku wybrania demona syslog-ng:

              1. Poinformuj demona dziennika systemu do nasłuchiwania local_4 funkcji i wysłać komunikaty dziennika systemu do platformy Azure przez wartownika agenta przy użyciu portu 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              2. Pobierz i zainstaluj [pliku konfiguracyjnego security_events](https://aka.ms/asi-syslog-config-file-linux) , konfiguruje agenta usługi Syslog do nasłuchiwania na porcie 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Gdzie {0} należy zamienić na GUID obszaru roboczego.

              3. Uruchom ponownie demona usługi syslog `sudo service syslog-ng restart` <br>Aby uzyskać więcej informacji, zobacz [demona syslog-ng dokumentacji](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/mutual-authentication-using-tls/2)
      2. Uruchom ponownie agenta usługi Syslog za pomocą tego polecenia: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Upewnij się, że nie ma żadnych błędów w dzienniku agenta, uruchamiając następujące polecenie: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

 Aby użyć odpowiednich schematu w usłudze Log Analytics dla zdarzeń CEF, możesz wyszukać `CommonSecurityLog`.


### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>Wdróż agenta na lokalny serwer systemu Linux

Jeśli nie używasz platformy Azure, ręcznie wdrożyć agenta przez wartownika platformy Azure, aby uruchomić na dedykowanym serwerze z systemem Linux.


1. W portalu Azure przez wartownika kliknij **łączników danych** i wybierz typ urządzenia.
1. Aby utworzyć dedykowane maszyny Wirtualnej systemu Linux w obszarze **konfiguracji agenta systemu Linux Syslog** wybierz **ręcznego wdrażania**.
   1. W obszarze **pobrać i zainstalować agenta programu Syslog**, wybierz opcję **maszyny z systemem Linux spoza platformy Azure**. 
   1. W **agent bezpośredni** ekran, który zostanie otwarty, wybierz **agenta dla systemu Linux** Pobierz agenta lub uruchom następujące polecenie, aby ją pobrać na maszynie z systemem Linux:   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`
      1. Na ekranie łącznika w obszarze **Konfiguruj i przekazywania usługi Syslog**Ustaw czy demona usługi Syslog jest **rsyslog.d** lub **demona syslog-ng**. 
      1. Skopiuj te polecenia i uruchamiać je na urządzeniu:
         - W przypadku wybrania rsyslog:
           1. Poinformuj demona dziennika systemu do nasłuchiwania local_4 funkcji i wysłać komunikaty dziennika systemu do platformy Azure przez wartownika agenta przy użyciu portu 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
           2. Pobierz i zainstaluj [pliku konfiguracyjnego security_events](https://aka.ms/asi-syslog-config-file-linux) , konfiguruje agenta usługi Syslog do nasłuchiwania na porcie 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Gdzie {0} należy zamienić na GUID obszaru roboczego.
           3. Uruchom ponownie demona usługi syslog `sudo service rsyslog restart`
         - W przypadku wybrania demona syslog-ng:
            1. Poinformuj demona dziennika systemu do nasłuchiwania local_4 funkcji i wysłać komunikaty dziennika systemu do platformy Azure przez wartownika agenta przy użyciu portu 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            2. Pobierz i zainstaluj [pliku konfiguracyjnego security_events](https://aka.ms/asi-syslog-config-file-linux) , konfiguruje agenta usługi Syslog do nasłuchiwania na porcie 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Gdzie {0} należy zamienić na GUID obszaru roboczego.
            3. Uruchom ponownie demona usługi syslog `sudo service syslog-ng restart`
      1. Uruchom ponownie agenta usługi Syslog za pomocą tego polecenia: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Upewnij się, że nie ma żadnych błędów w dzienniku agenta, uruchamiając następujące polecenie: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
  
 Aby użyć odpowiednich schematu w usłudze Log Analytics dla zdarzeń CEF, możesz wyszukać `CommonSecurityLog`.

## <a name="step-2-forward-common-event-format-cef-logs-to-syslog-agent"></a>Krok 2: Do przodu dzienniki Common Event Format (CEF), aby agent usługi Syslog

Ustaw rozwiązanie zabezpieczające do wysyłania komunikatów Syslog w formacie CEF do agenta programu Syslog. Upewnij się, że używasz te same parametry, które pojawiają się w konfiguracji agenta. Są to zazwyczaj:

- Port 514
- Funkcji local4

## <a name="step-3-validate-connectivity"></a>Krok 3: Zweryfikuj łączność

Może upłynąć zgłaszane 20 minut do momentu dzienników rozpocząć pojawiają się w usłudze Log Analytics. 

1. Upewnij się, że używasz prawo funkcji. Urządzenie to musi być takie same, w której znajduje się urządzenie i przez wartownika platformy Azure. Sprawdź, który plik funkcji jest używane w ramach platformy Azure przez wartownika i zmodyfikować go w pliku `security-config-omsagent.conf`. 

2. Upewnij się, że dzienniki pojawiają się na prawy port w agencie programu Syslog. Uruchom następujące polecenie na komputerze agenta usługi Syslog: `tcpdump -A -ni any  port 514 -vv` To polecenie wyświetla dzienniki, które są przesyłane z urządzenia do maszyny Syslog. Upewnij się, że dzienniki są otrzymywane z urządzenia źródłowego na prawy port i funkcji odpowiednie.

3. Upewnij się, że dzienniki wysyłane są zgodne z [RFC 5424](https://tools.ietf.org/html/rfc542).

4. Na komputerze z uruchomionym agentem Syslog, upewnij się, te porty 514, 25226 są otwarte i nasłuchuje, za pomocą polecenia `netstat -a -n:`. Aby uzyskać więcej informacji na temat korzystania z tego polecenia zobacz [netstat(8) - strony ataków typu man Linux](https://linux.die.net/man/8/netstat). Jeśli nasłuchuje poprawnie, zostanie wyświetlone to:

   ![Usługa Azure porty wartownik](./media/connect-cef/ports.png) 

5. Upewnij się, że demon jest ustawiony do nasłuchiwania na porcie 514, na którym one wysyłać dzienniki.
    - Demona rsyslog:<br>Upewnij się, że plik `/etc/rsyslog.conf` ta konfiguracja obejmuje:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Aby uzyskać więcej informacji, zobacz [imudp: Moduł danych wejściowych Syslog UDP](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) i [imtcp: Moduł danych wejściowych Syslog TCP](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module)

   - Demona syslog-NG:<br>Upewnij się, że plik `/etc/syslog-ng/syslog-ng.conf` ta konfiguracja obejmuje:

           # source s_network {
            network( transport(UDP) port(514));
             };
     Aby uzyskać więcej informacji, zobacz [imudp: Moduł danych wejściowych Syslog UDP] (Aby uzyskać więcej informacji, zobacz [demona syslog-ng Otwórz 3.16 wersji źródła — Przewodnik administrowania](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Sprawdź, czy komunikacja między demona usługi Syslog i agenta. Uruchom następujące polecenie na komputerze agenta usługi Syslog: `tcpdump -A -ni any  port 25226 -vv` To polecenie wyświetla dzienniki, które są przesyłane z urządzenia do maszyny Syslog. Upewnij się, że dzienniki są one odbierane w agencie.

6. Jeśli oba te polecenia pomyślne wyniki, sprawdź usługi Log Analytics, aby zobaczyć, jeśli dzienniki są odbierane. Wszystkie zdarzenia przesyłane strumieniowo ze te urządzenia są wyświetlane w nieprzetworzonej postaci, w usłudze Log Analytics w obszarze `CommonSecurityLog` typu.

7. Aby sprawdzić, czy istnieją błędy, lub jeśli dzienniki nie są przychodzących, poszukaj `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Jeśli występują błędy niezgodności format dziennika z nazwą, przejdź do strony `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` i spójrz na plik `security_events.conf`i upewnij się, że dzienniki zgodny z formatem wyrażeń regularnych, zostanie wyświetlony w tym pliku.

8. Upewnij się, że rozmiar domyślny wiadomości usługi Syslog wynosi 2048 bajtów (2KB). Jeśli dzienniki są zbyt długie, należy zaktualizować security_events.conf za pomocą tego polecenia: `message_length_limit 4096`


## <a name="next-steps"></a>Kolejne kroki
W tym dokumencie przedstawiono sposób łączenia urządzeń CEF do platformy Azure przez wartownika. Aby dowiedzieć się więcej na temat platformy Azure przez wartownika, zobacz następujące artykuły:
- Dowiedz się, jak [Uzyskaj wgląd w dane i potencjalne zagrożenia](quickstart-get-visibility.md).
- Rozpoczynanie pracy [wykrywanie zagrożeń za pomocą platformy Azure przez wartownika](tutorial-detect-threats.md).

