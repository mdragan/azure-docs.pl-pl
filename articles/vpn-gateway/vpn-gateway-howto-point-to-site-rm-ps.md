---
title: 'Połącz komputer z siecią wirtualną platformy Azure przy użyciu punkt-lokacja i natywnego platformy Azure uwierzytelniania certyfikatu: Program PowerShell | Dokumentacja firmy Microsoft'
description: Bezpieczne łączenie klientów systemu Windows i Mac OS X z siecią wirtualną platformy Azure przy użyciu połączeń typu punkt-lokacja oraz certyfikatów z podpisem własnym lub wystawionych przez urząd certyfikacji. W tym artykule używany jest program PowerShell.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: cherylmc
ms.openlocfilehash: 822cbc7401de90d63f9079561ced0dfbb911fa2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65989440"
---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-native-azure-certificate-authentication-powershell"></a>Konfigurowanie połączenia punkt-lokacja z siecią wirtualną przy użyciu uwierzytelniania certyfikatu platformy Azure native: PowerShell

W tym artykule opisano sposób bezpiecznego łączenia poszczególnych klientów z systemem Windows, Linux lub Mac OS X z siecią wirtualną platformy Azure. Połączenia sieci VPN typu punkt-lokacja przydają się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, podczas pracy zdalnej z domu lub konferencji. Możesz również użyć połączenia typu punkt-lokacja zamiast połączenia sieci VPN typu lokacja-lokacja w przypadku niewielkiej liczby klientów, którzy muszą się łączyć z siecią wirtualną. Połączenia typu punkt-lokacja nie wymagają urządzenia sieci VPN ani publicznego adresu IP. Połączenie typu punkt-lokacja tworzy połączenie sieci VPN nawiązywane za pośrednictwem protokołu SSTP (Secure Socket Tunneling Protocol) lub IKEv2. Aby uzyskać więcej informacji na temat połączeń sieci VPN typu punkt-lokacja, zobacz [About Point-to-Site VPN (Informacje o sieci VPN typu punkt-lokacja)](point-to-site-about.md).

![Łączenie komputera z siecią wirtualną platformy Azure — diagram połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/p2snativeportal.png)

## <a name="architecture"></a>Architektura

Natywne połączenia uwierzytelniania certyfikatów platformy Azure typu punkt-lokacja używają następujących elementów, które można skonfigurować w ramach tego ćwiczenia:

* Brama sieci VPN oparta na trasie.
* Klucz publiczny (plik cer) dla certyfikatu głównego, przekazany na platformę Azure. Przekazany certyfikat jest uznawany za certyfikat zaufany i używany do uwierzytelniania.
* Certyfikat klienta generowany na podstawie certyfikatu głównego. Certyfikat klienta instalowany na każdym komputerze klienckim, który będzie łączyć się z siecią wirtualną. Ten certyfikat jest używany do uwierzytelniania klientów.
* Konfiguracja klienta sieci VPN. Pliki konfiguracji klienta sieci VPN zawierają informacje niezbędne klientowi do połączenia się z siecią wirtualną. Pliki te służą do konfigurowania istniejącego, natywnego dla systemu operacyjnego klienta sieci VPN . Każdy klient, który nawiązuje połączenie, musi być skonfigurowany przy użyciu ustawień w tych plikach konfiguracji.

## <a name="before-you-begin"></a>Przed rozpoczęciem

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Sprawdź, czy masz subskrypcję platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial).

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

Większość czynności w tym artykule można używać usługi Cloud Shell. Jednakże aby przekazać klucz publiczny certyfikatu głównego, należy użyć programu PowerShell lokalnie lub w portalu Azure.

### <a name="example"></a>Przykładowe wartości

Wartości przykładowych możesz użyć do tworzenia środowiska testowego lub odwoływać się do tych wartości, aby lepiej zrozumieć przykłady w niniejszym artykule. Zmienne są ustawiane w sekcji [1](#declare) artykułu. Można postępować zgodnie z opisanymi krokami i użyć przedstawionych wartości bez ich zmieniania lub zmienić je, aby odzwierciedlały dane środowisko.

* **Nazwa: VNet1**
* **Przestrzeń adresowa: 192.168.0.0/16** i **10.254.0.0/16**<br>W tym przykładzie jest używana więcej niż jedna przestrzeń adresowa, aby zilustrować, że ta konfiguracja współpracuje z wieloma przestrzeniami adresowymi. Jednak ta konfiguracja nie wymaga wielu przestrzeni adresowych.
* **Nazwa podsieci: Frontonu**
  * **Zakres adresów podsieci: 192.168.1.0/24**
* **Nazwa podsieci: BackEnd**
  * **Zakres adresów podsieci: 10.254.1.0/24**
* **Nazwa podsieci: GatewaySubnet**<br>Nazwa podsieci *GatewaySubnet* jest obowiązkowa, aby brama VPN mogła działać.
  * **Zakres adresów podsieci: 192.168.200.0/24** 
* **Pula adresów klienta sieci VPN: 172.16.201.0/24**<br>Klienci sieci VPN łączący się z siecią wirtualną, którzy korzystają z tego połączenia punkt-lokacja, otrzymują adresy IP z puli adresów klientów sieci VPN.
* **Subskrypcja:** Jeśli masz więcej niż jedną subskrypcję, sprawdź, czy używane są poprawne.
* **Grupa zasobów: TestRG**
* **Lokalizacja: Wschodnie stany USA**
* **Serwer DNS: Adres IP** serwera DNS, który ma być używany do rozpoznawania nazw. (opcjonalnie)
* **Nazwa GW: Vnet1GW**
* **Publiczna nazwa adresu IP: VNet1GWPIP**
* **VpnType: RouteBased** 

## <a name="declare"></a>1. Zaloguj się i Ustawianie zmiennych

W tej sekcji, zaloguj się i zadeklarować wartości używane dla tej konfiguracji. Zadeklarowane wartości są używane w przykładowych skryptach. Można zmienić wartości, aby odzwierciedlić własne środowisko. Ale można też użyć zadeklarowanych wartości i postępować zgodnie z opisanymi krokami tylko w celach szkoleniowych.

### <a name="sign-in"></a>Logowanie

[!INCLUDE [sign in](../../includes/vpn-gateway-cloud-shell-ps-login.md)]

### <a name="declare-variables"></a>Zadeklaruj zmienne

Zadeklaruj zmienne, których chcesz użyć. Użyj poniższego przykładu, podstawiając własne wartości tam, gdzie to konieczne. Po zamknięciu sesji programu PowerShell/Cloud Shell w dowolnym momencie podczas wykonywania po prostu skopiuj i Wklej wartości ponownie, aby ponownie zadeklarować zmienne.

  ```azurepowershell-interactive
  $VNetName  = "VNet1"
  $FESubName = "FrontEnd"
  $BESubName = "Backend"
  $GWSubName = "GatewaySubnet"
  $VNetPrefix1 = "192.168.0.0/16"
  $VNetPrefix2 = "10.254.0.0/16"
  $FESubPrefix = "192.168.1.0/24"
  $BESubPrefix = "10.254.1.0/24"
  $GWSubPrefix = "192.168.200.0/26"
  $VPNClientAddressPool = "172.16.201.0/24"
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## <a name="ConfigureVNet"></a>2. Konfigurowanie sieci wirtualnej

1. Utwórz grupę zasobów.

   ```azurepowershell-interactive
   New-AzResourceGroup -Name $RG -Location $Location
   ```
2. Utwórz konfiguracje podsieci dla sieci wirtualnej, nadając im nazwy *FrontEnd*, *BackEnd* i *GatewaySubnet*. Prefiksy te muszą być częścią zadeklarowanej przestrzeni adresowej sieci wirtualnej.

   ```azurepowershell-interactive
   $fesub = New-AzVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
   $besub = New-AzVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
   $gwsub = New-AzVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
   ```
3. Utwórz sieć wirtualną.

   W tym przykładzie parametr serwera -DnsServer jest opcjonalny. Określenie wartości nie powoduje utworzenia nowego serwera DNS. Określony adres IP serwera DNS powinien być adresem serwera będącego w stanie rozpoznawać nazwy zasobów, z którymi nawiązywane jest połączenie z Twojej sieci wirtualnej. W tym przykładzie użyto prywatnego adresu IP, ale prawdopodobnie nie jest to adres IP Twojego serwera DNS. Pamiętaj, aby użyć własnych wartości. Wartość, którą określasz, jest używana przez zasoby wdrażane w sieci wirtualnej, nie przez połączenie typu punkt-lokacja czy klienta sieci VPN.

   ```azurepowershell-interactive
   New-AzVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
   ```
4. Określ zmienne dla utworzonej sieci wirtualnej.

   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $RG
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
   ```
5. Brama sieci VPN musi mieć publiczny adres IP. Najpierw żąda się zasobu adresu IP, a następnie odwołuje do niego podczas tworzenia bramy sieci wirtualnej. Adres IP jest dynamicznie przypisywany do zasobu podczas tworzenia bramy sieci VPN. Brama sieci VPN aktualnie obsługuje tylko *dynamiczne* przypisywanie publicznych adresów IP. Nie można zażądać przypisania statycznego publicznego adresu IP. Nie oznacza to jednak, że adres IP zmienia się po przypisaniu go do bramy sieci VPN. Jedyną sytuacją, w której ma miejsce zmiana publicznego adresu IP, jest usunięcie bramy i jej ponowne utworzenie. Nie zmienia się on w przypadku zmiany rozmiaru, zresetowania ani przeprowadzania innych wewnętrznych czynności konserwacyjnych bądź uaktualnień bramy sieci VPN.

   Zażądaj dynamicznie przydzielanego publicznego adresu IP.

   ```azurepowershell-interactive
   $pip = New-AzPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
   $ipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
   ```

## <a name="creategateway"></a>3. Tworzenie bramy sieci VPN

Skonfiguruj i utwórz bramę sieci wirtualnej dla sieci wirtualnej.

* Zmienna -GatewayType musi mieć wartość **Vpn**, a zmienna **-VpnType** musi mieć wartość RouteBased.
* Parametr -VpnClientProtocol służy do określania typów tuneli, które mają zostać włączone. Opcje tuneli to **OpenVPN, SSTP** i **IKEv2**. Można włączyć jedną z nich lub dowolnej obsługiwanej kombinacji. Jeśli chcesz włączyć wiele typów, następnie określane są nazwy rozdzielone przecinkami. OpenVPN i SSTP nie można włączyć jednocześnie. Klient strongSwan w systemach Android i Linux oraz natywny klient sieci VPN IKEv2 w systemach iOS i OSX będą używać do łączenia się tylko tuneli IKEv2. Klienci w systemie Windows będą najpierw próbowali użyć protokołu IKEv2, a jeśli połączenie nie zostanie nawiązane, użyją protokołu SSTP. Klient OpenVPN służy do nawiązywania połączeń typu tunelu OpenVPN.
* "Podstawowe" jednostki SKU bramy sieci wirtualnej nie obsługuje uwierzytelniania protokołu IKEv2, OpenVPN lub serwera RADIUS. Jeśli planowane jest posiadanie Mac klienci łączą się z siecią wirtualną, nie należy używać podstawowej jednostki SKU.
* Tworzenie bramy sieci VPN może zająć do 45 minut, zależnie od wybranej [jednostki sku bramy](vpn-gateway-about-vpn-gateway-settings.md). W tym przykładzie użyto protokołu IKEv2.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocol "IKEv2"
```

## <a name="addresspool"></a>4. Dodawanie puli adresów klienta sieci VPN

Po zakończeniu tworzenia bramy VPN Gateway możesz dodać pulę adresów klienta sieci VPN. Pula adresów klienta sieci VPN to zakres, z którego klienci sieci VPN otrzymują adres IP podczas nawiązywania połączenia. Używaj zakresu prywatnych adresów IP nienakładającego się na lokalizację lokalną, z której się łączysz, ani na sieć wirtualną, z którą chcesz się łączyć. W tym przykładzie pula adresów klienta sieci VPN jest deklarowana jako [zmienna](#declare) w kroku 1.

```azurepowershell-interactive
$Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
```

## <a name="Certificates"></a>5. Generowanie certyfikatów

Certyfikaty są używane przez platformę Azure do uwierzytelniania klientów sieci VPN w obrębie sieci VPN typu punkt-lokacja. Informacje o kluczu publicznym certyfikatu głównego należy przekazać na platformę Azure. Klucz publiczny jest wtedy uważany za „zaufany”. Certyfikaty klienta muszą być generowane na podstawie zaufanego certyfikatu głównego, a następnie instalowane na każdym komputerze klienckim w magazynie certyfikatów Certificates-Current User/Personal. Certyfikat jest używany do uwierzytelniania klienta, gdy inicjuje on połączenie z siecią wirtualną. 

Jeśli używasz certyfikatów z podpisem własnym, należy je utworzyć przy użyciu określonych parametrów. Certyfikat z podpisem własnym można utworzyć przy użyciu instrukcji dotyczących [środowiska PowerShell i systemu Windows 10](vpn-gateway-certificates-point-to-site.md) lub, jeśli nie masz systemu Windows 10, możesz użyć polecenia [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Istotne jest, aby podczas generowania certyfikatów głównych z podpisem własnym i certyfikatów klientów wykonać kroki opisane w instrukcjach. W przeciwnym razie wygenerowane przez Ciebie certyfikaty nie będą zgodne z połączeniami typu punkt-lokacja i zostanie wyświetlony błąd połączenia.

### <a name="cer"></a>1. Uzyskiwanie pliku cer dla certyfikatu głównego

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]


### <a name="generate"></a>2. Generowanie certyfikatu klienta

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>6. Przekazywanie informacji o kluczu publicznym certyfikatu głównego

Upewnij się, że zakończono tworzenie bramy VPN Gateway. Po ukończeniu możesz przekazać plik cer (który zawiera informacje o kluczu publicznym) dla zaufanego certyfikatu głównego do platformy Azure. Po przekazaniu pliku cer platforma Azure może używać go do uwierzytelniania klientów, którzy mają zainstalowany certyfikat klienta wygenerowany na podstawie zaufanego certyfikatu głównego. Jeśli okaże się to konieczne, dodatkowe pliki zaufanego certyfikatu głównego możesz przekazać później — maksymalnie może ich być 20.

Nie można przekazać te informacje przy użyciu usługi Azure Cloud Shell. Możesz użyć programu PowerShell lokalnie na komputerze, [Azure portal kroki](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile).

1. Zadeklaruj zmienną dla nazwy certyfikatu, zastępując wartość swoją własną.

   ```azurepowershell
   $P2SRootCertName = "P2SRootCert.cer"
   ```
2. Zastąp ścieżkę, podając własną, a następnie uruchom polecenia cmdlet.

   ```azurepowershell
   $filePathForCert = "C:\cert\P2SRootCert.cer"
   $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
   $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
   $p2srootcert = New-AzVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
   ```
3. Przekaż informacje o kluczu publicznym do platformy Azure. Po przekazaniu informacji o certyfikacie platforma Azure traktuje jako zaufany certyfikat główny.

   ```azurepowershell
   Add-AzVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
   ```

## <a name="clientcertificate"></a>7. Instalowanie wyeksportowanego certyfikatu klienta

Jeśli chcesz utworzyć połączenie punkt-lokacja z komputerem klienckim innym niż użyty do wygenerowania certyfikatów klienta, należy zainstalować certyfikat klienta. Podczas instalowania certyfikatu klienta potrzebne jest hasło, które zostało utworzone w trakcie eksportowania certyfikatu klienta.

Upewnij się, że certyfikat klienta został wyeksportowany jako plik pfx wraz z całym łańcuchem certyfikatów (jest to ustawienie domyślne). W przeciwnym razie informacje o certyfikacie głównym nie będą dostępne na komputerze klienckim i klient nie będzie mógł się poprawnie uwierzytelnić. 

Aby zapoznać się z krokami instalacji, zobacz [Install a client certificate](point-to-site-how-to-vpn-client-install-azure-cert.md) (Instalowanie certyfikatu klienta).

## <a name="clientconfig"></a>8. Konfigurowanie natywnego klienta sieci VPN

Pliki konfiguracji klienta sieci VPN zawierają ustawienia do konfigurowania urządzeń pod kątem łączenia się z siecią wirtualną przez połączenie punkt-lokacja. Aby uzyskać instrukcje dotyczące generowania i instalowania plików konfiguracji klienta sieci VPN, zobacz [Create and install VPN client configuration files for native Azure certificate authentication P2S configurations](point-to-site-vpn-client-configuration-azure-cert.md) (Tworzenie i instalowanie plików konfiguracji klienta sieci VPN dla konfiguracji połączeń punkt-lokacja z natywnym uwierzytelnianiem certyfikatów platformy Azure).

## <a name="connect"></a>9. Nawiązywanie połączenia z usługą Azure

### <a name="to-connect-from-a-windows-vpn-client"></a>Aby połączyć się z klienta sieci VPN w systemie Windows

>[!NOTE]
>Musisz mieć uprawnienia administratora na komputerze klienckim z systemem Windows, z którym nawiązujesz połączenie.
>
>

1. Aby nawiązać połączenie z siecią wirtualną na komputerze klienckim, przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**. Może pojawić się komunikat podręczny, który odwołuje się do użycia certyfikatu. Kliknij przycisk **Kontynuuj**, aby skorzystać z podwyższonego poziomu uprawnień. 
2. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.

   ![Łączenie klienta sieci VPN z platformą Azure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. Połączenie zostało ustanowione.

   ![Ustanowiono połączenie](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

#### <a name="troubleshooting-windows-client-p2s-connections"></a>Rozwiązywanie problemów z połączeniami typu punkt-lokacja klienta w systemie Windows

[!INCLUDE [client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="to-connect-from-a-mac-vpn-client"></a>Aby połączyć się z klienta sieci VPN na komputerze Mac

W oknie dialogowym Sieć znajdź profil klienta, którego chcesz użyć, a następnie kliknij polecenie **Połącz**.
Sprawdź [zainstalować - Mac (OS X)](https://docs.microsoft.com/azure/vpn-gateway/point-to-site-vpn-client-configuration-azure-cert#installmac) szczegółowe informacje. Jeśli występują problemy z połączeniem, sprawdź, czy brama sieci wirtualnej nie korzysta z podstawowej jednostki SKU. Podstawowa jednostka SKU nie jest obsługiwana dla klientów na komputery Mac.

  ![Połączenie z komputerem Mac](./media/vpn-gateway-howto-point-to-site-rm-ps/applyconnect.png)

## <a name="verify"></a>Aby zweryfikować połączenie

Te instrukcje dotyczą klientów w systemie Windows.

1. Aby sprawdzić, czy połączenie sieci VPN jest aktywne, otwórz wiersz polecenia z podwyższonym poziomem uprawnień, a następnie uruchom polecenie *ipconfig/all*.
2. Przejrzyj wyniki. Zwróć uwagę na fakt, że otrzymany adres IP należy do określonej podczas konfiguracji połączenia punkt-lokacja puli adresów klienta sieci VPN. Wyniki są podobne, jak w następującym przykładzie:

   ```
   PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
   ```

## <a name="connectVM"></a>Nawiązywanie połączenia z maszyną wirtualną

Te instrukcje dotyczą klientów w systemie Windows.

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="addremovecert"></a>Dodawanie lub usuwanie certyfikatu głównego

Zaufane certyfikaty główne można dodawać do platformy Azure lub z niej usuwać. Po usunięciu certyfikatu głównego klienci, którzy mają certyfikat wygenerowany na podstawie tego certyfikatu głównego, nie będą w stanie się uwierzytelnić i nie będą mogli nawiązywać połączeń. Jeśli chcesz, aby klienci mogli uwierzytelniać się i nawiązywać połączenia, musisz zainstalować nowy certyfikat klienta wygenerowany na podstawie certyfikatu głównego, który jest traktowany jako zaufany przez platformę Azure (przekazany na tę platformę).

### <a name="addtrustedroot"></a>Aby dodać zaufany certyfikat główny

Na platformie Azure można dodać maksymalnie 20 plików cer certyfikatu głównego. Poniższa procedura jest pomocna w dodawaniu certyfikatu głównego:

#### <a name="certmethod1"></a>Metoda 1


Ta metoda jest najbardziej skutecznym sposobem Przekaż certyfikat główny. Wymaga to polecenia cmdlet programu Azure PowerShell zainstalowane lokalnie na komputerze (nie usługi Azure Cloud Shell).

1. Przygotuj plik cer do przekazania:

   ```azurepowershell
   $filePathForCert = "C:\cert\P2SRootCert3.cer"
   $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
   $CertBase64_3 = [system.convert]::ToBase64String($cert.RawData)
   $p2srootcert = New-AzVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64_3
   ```
2. Przekaż plik. Jednocześnie możesz przekazać tylko jeden plik.

   ```azurepowershell
   Add-AzVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64_3
   ```

3. Aby sprawdzić, czy plik certyfikatu został przekazany:

   ```azurepowershell
   Get-AzVpnClientRootCertificate -ResourceGroupName "TestRG" `
   -VirtualNetworkGatewayName "VNet1GW"
   ```

#### <a name="certmethod2"></a>Metoda 2 — witryna Azure portal

Ta metoda obejmuje więcej czynności niż metoda 1, ale daje ten sam wynik. Przedstawiono ją na potrzeby wyświetlania danych certyfikatu. Wymaga to polecenia cmdlet programu Azure PowerShell zainstalowane lokalnie na komputerze (nie usługi Azure Cloud Shell).

1. Utwórz i przygotuj nowy certyfikat główny, który ma zostać dodany do platformy Azure. Wyeksportuj klucz publiczny jako certyfikat x.509 (.CER) szyfrowany algorytmem Base-64 i otwórz go w edytorze tekstu. Skopiuj wartości, tak jak to pokazano w poniższym przykładzie:

   ![certyfikat](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

   > [!NOTE]
   > Podczas kopiowania danych dotyczących certyfikatu upewnij się, że kopiujesz tekst jako jeden ciągły wiersz bez znaków powrotu karetki i wysuwu wiersza. Może zajść potrzeba zmodyfikowania widoku w edytorze tekstu na potrzeby pokazywania symboli lub pokazywania wszystkich znaków, aby wyświetlić znaki powrotu karetki i wysuwu wiersza.
   >
   >

2. Określi nazwę certyfikatu i informacje o kluczu jako zmienną. Zastąp informacje własnymi, tak jak pokazano w poniższym przykładzie:

   ```azurepowershell
   $P2SRootCertName2 = "ARMP2SRootCert2.cer"
   $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
   ```
3. Dodaj nowy certyfikat główny. Można dodawać tylko jeden certyfikat naraz.

   ```azurepowershell
   Add-AzVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
   ```
4. Można sprawdzić, czy nowy certyfikat został poprawnie dodany, korzystając z następującego przykładu:

   ```azurepowershell
   Get-AzVpnClientRootCertificate -ResourceGroupName "TestRG" `
   -VirtualNetworkGatewayName "VNet1GW"
   ```

### <a name="removerootcert"></a>Aby usunąć certyfikat główny

1. Zadeklaruj zmienne.

   ```azurepowershell-interactive
   $GWName = "Name_of_virtual_network_gateway"
   $RG = "Name_of_resource_group"
   $P2SRootCertName2 = "ARMP2SRootCert2.cer"
   $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
   ```
2. Usuń certyfikat.

   ```azurepowershell-interactive
   Remove-AzVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
   ```
3. Użyj następującego przykładu, aby zweryfikować, czy certyfikat został pomyślnie usunięty.

   ```azurepowershell-interactive
   Get-AzVpnClientRootCertificate -ResourceGroupName "TestRG" `
   -VirtualNetworkGatewayName "VNet1GW"
   ```

## <a name="revoke"></a>Aby odwołać certyfikat klienta

Certyfikaty klienta można odwołać. Lista odwołania certyfikatów umożliwia dokonanie selektywnej odmowy połączenia punkt-lokacja w oparciu o indywidualne certyfikaty klienta. Różni się to od usuwania zaufanego certyfikatu głównego. Jeśli usuniesz plik cer zaufanego certyfikatu głównego z platformy Azure, spowoduje to odwołanie dostępu dla wszystkich certyfikatów klienta wygenerowanych lub podpisanych przez odwołany certyfikat główny. Odwołanie certyfikatu klienta zamiast certyfikatu głównego pozwala dalej używać innych certyfikatów wygenerowanych na podstawie certyfikatu głównego do uwierzytelniania.

Częstą praktyką jest użycie certyfikatu głównego do zarządzania dostępem na poziomach zespołu lub organizacji przy jednoczesnym korzystaniu z odwołanych certyfikatów klienta dla bardziej precyzyjnej kontroli dostępu poszczególnych użytkowników.

### <a name="revokeclientcert"></a>Odwoływanie certyfikatu klienta

1. Pobierz odcisk palca certyfikatu klienta. Aby uzyskać więcej informacji, zobacz [Jak pobrać odcisk palca certyfikatu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Skopiuj informacje do edytora tekstu i usuń wszelkie spacje, tak aby powstał ciąg bez odstępów. Ten ciąg jest deklarowany jako zmienna w następnym kroku.
3. Zadeklaruj zmienne. Pamiętaj, aby zadeklarować odcisk palca pobrany w poprzednim kroku.

   ```azurepowershell-interactive
   $RevokedClientCert1 = "NameofCertificate"
   $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
   $GWName = "Name_of_virtual_network_gateway"
   $RG = "Name_of_resource_group"
   ```
4. Dodaj odcisk palca do listy odwołanych certyfikatów. Po dodaniu odcisku palca zostanie wyświetlony komunikat „Powodzenie”.

   ```azurepowershell-interactive
   Add-AzVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
   -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
   -Thumbprint $RevokedThumbprint1
   ```
5. Sprawdź, czy odcisk palca został dodany do listy odwołania certyfikatów.

   ```azurepowershell-interactive
   Get-AzVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
   ```
6. Po dodaniu odcisku palca nie będzie można używać certyfikatu do nawiązywania połączeń. Klienci, którzy spróbują połączyć się za pomocą tego certyfikatu, zobaczą komunikat informujący o tym, że certyfikat nie jest już ważny.

### <a name="reinstateclientcert"></a>Aby przywrócić certyfikat klienta

Certyfikat klienta można przywrócić przez usunięcie odcisku palca z listy odwołanych certyfikatów klienta.

1. Zadeklaruj zmienne. Pamiętaj, aby zadeklarować poprawny odcisk palca dla certyfikatu, który chcesz przywrócić.

   ```azurepowershell-interactive
   $RevokedClientCert1 = "NameofCertificate"
   $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
   $GWName = "Name_of_virtual_network_gateway"
   $RG = "Name_of_resource_group"
   ```
2. Usuń odcisk palca certyfikatu z listy odwołania certyfikatów.

   ```azurepowershell-interactive
   Remove-AzVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
   -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
   ```
3. Sprawdź, czy odcisk palca został usunięty z listy odwołania.

   ```azurepowershell-interactive
   Get-AzVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
   ```

## <a name="faq"></a>Często zadawane pytania dotyczące połączeń typu punkt-lokacja

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="next-steps"></a>Kolejne kroki
Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/) (Maszyny wirtualne). Aby dowiedzieć się więcej o sieci i maszynach wirtualnych, zobacz [Azure and Linux VM network overview](../virtual-machines/linux/azure-vm-network-overview.md) (Omówienie sieci maszyny wirtualnej z systemem Linux i platformy Azure).

Aby uzyskać informacje dotyczące rozwiązywania problemów P2S [Rozwiązywanie problemów: Problemy z połączeniem usługi Azure point-to-site](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
