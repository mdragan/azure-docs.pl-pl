---
title: Występuje błąd wewnętrzny podczas tworzenia połączenia RDP na maszynach wirtualnych platformy Azure | Dokumentacja firmy Microsoft
description: Dowiedz się, jak rozwiązywać problemy z błędami wewnętrzny protokołu RDP w systemie Microsoft Azure. | Dokumentacja firmy Microsoft
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/22/2018
ms.author: genli
ms.openlocfilehash: 4476e4732dfcf8d79c9678a7ff4719eba10e48f3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60319432"
---
#  <a name="an-internal-error-occurs-when-you-try-to-connect-to-an-azure-vm-through-remote-desktop"></a>Występuje błąd wewnętrzny podczas próby połączenia z Maszyną wirtualną platformy Azure za pośrednictwem pulpitu zdalnego

W tym artykule opisano błędem, które mogą wystąpić podczas próby nawiązania połączenia z maszyną wirtualną (VM) w systemie Microsoft Azure.
> [!NOTE]
> Platforma Azure oferuje dwa różne modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [model wdrażania przy użyciu usługi Resource Manager i model klasyczny](../../azure-resource-manager/resource-manager-deployment-model.md). W tym artykule opisano, przy użyciu modelu wdrażania usługi Resource Manager, w którym firma Microsoft zaleca używanie w przypadku nowych wdrożeń zamiast klasycznego modelu wdrażania.

## <a name="symptoms"></a>Objawy

Nie można nawiązać Maszynie wirtualnej platformy Azure przy użyciu protokołu remote desktop protocol (RDP). Połączenie zablokowania w sekcji "Konfigurowanie zdalnego" lub pojawi się następujący komunikat o błędzie:

- Błąd wewnętrzny protokołu RDP
- Wystąpił błąd wewnętrzny
- Ten komputer nie można połączyć z komputerem zdalnym. Spróbuj ponownie nawiązać połączenie. Jeśli problem będzie się powtarzał, skontaktuj się z właścicielem komputera zdalnego lub z administratorem sieci


## <a name="cause"></a>Przyczyna

Ten problem może wystąpić z następujących powodów:

- Nie można uzyskać dostępu do lokalnych kluczy szyfrowania RSA.
- Protokół TLS jest wyłączony.
- Certyfikat jest uszkodzony lub wygasła.

## <a name="solution"></a>Rozwiązanie

Przed wykonaniem tych kroków należy utworzyć migawkę dysku systemu operacyjnego, których to dotyczy maszyny wirtualnej do przechowywania kopii zapasowych. Aby uzyskać więcej informacji, zobacz [Tworzenie migawki dysku](../windows/snapshot-copy-managed-disk.md).

Aby rozwiązać ten problem, należy użyć konsoli szeregowej lub [napraw maszynę Wirtualną w tryb offline](#repair-the-vm-offline) , dołączając dysk systemu operacyjnego maszyny wirtualnej do maszyny Wirtualnej odzyskiwania.


### <a name="use-serial-control"></a>Korzystanie z kontroli szeregowej

Połączyć się z [konsoli szeregowej i otwórz wystąpienie programu PowerShell](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). Jeśli na maszynie Wirtualnej nie włączono konsoli szeregowej, przejdź do strony [napraw maszynę Wirtualną w tryb offline](#repair-the-vm-offline) sekcji.

#### <a name="step-1-check-the-rdp-port"></a>Krok: 1 Sprawdź portów protokołu RDP

1. W wystąpieniu programu PowerShell, użyj [NETSTAT](https://docs.microsoft.com/windows-server/administration/windows-commands/netstat
) do sprawdzenia, czy port 8080 jest używany przez inne aplikacje:

        Netstat -anob |more
2. Jeśli Termservice.exe używa portu 8080, przejdź do kroku 2. Jeśli przez inną usługę lub aplikację inną niż Termservice.exe używa portu 8080, wykonaj następujące kroki:

    1. Zatrzymaj usługę dla aplikacji, która używa usługi 3389:

            Stop-Service -Name <ServiceName> -Force

    2. Uruchom usługę terminalu:

            Start-Service -Name Termservice

2. Jeśli nie można zatrzymać aplikacji lub jeśli ta metoda nie ma zastosowania do użytkownika, należy zmienić port dla protokołu RDP:

    1. Zmień numer portu:

            Set-ItemProperty -Path 'HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name PortNumber -value <Hexportnumber>

            Stop-Service -Name Termservice -Force
            
            Start-Service -Name Termservice 

    2. Ustaw zaporę dla nowego portu:

            Set-NetFirewallRule -Name "RemoteDesktop-UserMode-In-TCP" -LocalPort <NEW PORT (decimal)>

    3. [Aktualizowanie sieciowej grupy zabezpieczeń dla nowego portu](../../virtual-network/security-overview.md) w portalu Azure portem RDP.

#### <a name="step-2-set-correct-permissions-on-the-rdp-self-signed-certificate"></a>Krok 2: Ustaw uprawnienia poprawny certyfikat z podpisem własnym protokołu RDP

1.  W wystąpieniu programu PowerShell uruchom następujące polecenia pojedynczo, aby odnowić certyfikat z podpisem własnym protokołu RDP:

        Import-Module PKI 
    
        Set-Location Cert:\LocalMachine 
        
        $RdpCertThumbprint = 'Cert:\LocalMachine\Remote Desktop\'+((Get-ChildItem -Path 'Cert:\LocalMachine\Remote Desktop\').thumbprint) 
        
        Remove-Item -Path $RdpCertThumbprint

        Stop-Service -Name "SessionEnv"

        Start-Service -Name "SessionEnv"

2. Za pomocą tej metody nie mogą odnowić certyfikat, próbuje zdalnie odnawiania certyfikatu z podpisem własnym protokołu RDP:

    1. Z działającej maszyny Wirtualnej, który ma łączność z maszyną Wirtualną, z którym występują problemy, typ **mmc** w **Uruchom** okno, aby otworzyć konsolę Microsoft Management Console.
    2. Na **pliku** menu, wybierz opcję **Dodaj/Usuń przystawkę**, wybierz opcję **certyfikaty**, a następnie wybierz pozycję **Dodaj**.
    3. Wybierz **kont komputerów**, wybierz opcję **inny komputer**, a następnie dodaj adres IP problem maszyny Wirtualnej.
    4. Przejdź do **zdalnego Desktop\Certificates** folderu, kliknij prawym przyciskiem myszy certyfikat, wybierz opcję a następnie **Usuń**.
    5. W wystąpieniu programu PowerShell z poziomu konsoli szeregowej Uruchom ponownie usługę konfiguracji pulpitu zdalnego:

            Stop-Service -Name "SessionEnv"

            Start-Service -Name "SessionEnv"
3. Resetuj uprawnienia do folderu MachineKeys.

        remove-module psreadline icacls

        md c:\temp

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\BeforeScript_permissions.txt 
        
        takeown /f "C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys" /a /r

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\System:(F)"

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\NETWORK SERVICE:(R)"

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\AfterScript_permissions.txt 
        
        Restart-Service TermService -Force

4. Uruchom ponownie maszynę Wirtualną, a następnie spróbuj Start Podłączanie pulpitu zdalnego z maszyną wirtualną. Jeśli błąd będzie nadal występował, przejdź do następnego kroku.

Krok 3: Włącz wszystkie obsługiwane wersje protokołu TLS

Klient protokołu RDP korzysta z protokołu TLS 1.0 jako domyślnego protokołu. Jednak to można zmienić do protokołu TLS 1.1 stał się nowym standardem. Wyłączenie protokołu TLS 1.1 na maszynie Wirtualnej, połączenie nie powiedzie się.
1.  W wystąpieniu CMD Włącz protokół TLS:

        reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f
2.  Aby uniknąć zastępowania zmian zasad AD, Zatrzymaj tymczasowo aktualizacji zasad grupy:

        REG add "HKLM\SYSTEM\CurrentControlSet\Services\gpsvc" /v Start /t REG_DWORD /d 4 /f
3.  Tak, aby zmiany zaczęły obowiązywać, należy ponownie uruchomić maszynę Wirtualną. Jeśli ten problem zostanie rozwiązany, uruchom następujące polecenie, aby ponownie włączyć zasad grupy:

        sc config gpsvc start= auto sc start gpsvc

        gpupdate /force
    Zmiana zostanie wycofana, oznacza, że istnieje zasady usługi Active Directory w domenie firmy. Musisz zmienić te zasady, aby uniknąć tego problemu znowu.

### <a name="repair-the-vm-offline"></a>Napraw maszynę Wirtualną w tryb Offline

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Dołącz dysk systemu operacyjnego do maszyny Wirtualnej odzyskiwania

1. [Dołącz dysk systemu operacyjnego do maszyny Wirtualnej odzyskiwania](../windows/troubleshoot-recovery-disks-portal.md).
2. Po dołączeniu dysku systemu operacyjnego do maszyny Wirtualnej odzyskiwania, upewnij się, że dysk jest oznaczone jako **Online** w konsoli Zarządzanie dyskami. Zanotuj literę dysku, która jest przypisana do dołączonym dysku systemu operacyjnego.
3. Rozpocznij połączenie pulpitu zdalnego do maszyny Wirtualnej odzyskiwania.

#### <a name="enable-dump-log-and-serial-console"></a>Włącz dziennik zrzutu i konsoli szeregowej

Aby włączyć dziennik zrzutu i konsoli szeregowej, uruchom następujący skrypt.

1. Otwórz sesję wiersza polecenia z podwyższonym poziomem uprawnień (**Uruchom jako administrator**).
2. Uruchom następujący skrypt:

    W tym skrypcie przyjęto założenie, że litery dysku, która jest przypisana do dołączonym dysku systemu operacyjnego jest F. Zastąp tę literę dysku z odpowiednią wartością dla maszyny Wirtualnej.

    ```
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

#### <a name="reset-the-permission-for-machinekeys-folder"></a>Resetuj uprawnienia do folderu MachineKeys

1. Otwórz sesję wiersza polecenia z podwyższonym poziomem uprawnień (**Uruchom jako administrator**).
2. Uruchom poniższy skrypt. W tym skrypcie przyjęto założenie, że litery dysku, która jest przypisana do dołączonym dysku systemu operacyjnego jest F. Zastąp tę literę dysku z odpowiednią wartością dla maszyny Wirtualnej.

        Md F:\temp

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\BeforeScript_permissions.txt
        
        takeown /f "F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys" /a /r

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\System:(F)"

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\NETWORK SERVICE:(R)"

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\AfterScript_permissions.txt

#### <a name="enable-all-supported-tls-versions"></a>Włącz wszystkie obsługiwane wersje protokołu TLS

1.  Otwórz sesję wiersza polecenia z podwyższonym poziomem uprawnień (**Uruchom jako administrator**), a następnie uruchom następujące polecenia. Poniższy skrypt zakłada się, czy literę został przypisany do dołączonym dysku systemu operacyjnego jest Zamień F. tę literę dysku z odpowiednią wartością dla maszyny Wirtualnej.
2.  Sprawdzanie, który protokół TLS jest włączony:

        reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWO

3.  Jeśli klucz nie istnieje lub jego wartość wynosi **0**, Włącz protokół, uruchamiając następujące skrypty:

        REM Enable TLS 1.0, TLS 1.1 and TLS 1.2

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f

4.  Włączanie uwierzytelniania na poziomie sieci:

        REM Enable NLA

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 1 /f reg unload HKLM\BROKENSYSTEM
5.  [Odłącz dysk systemu operacyjnego i ponowne utworzenie maszyny Wirtualnej](../windows/troubleshoot-recovery-disks-portal.md), a następnie sprawdź, czy problem został rozwiązany.





