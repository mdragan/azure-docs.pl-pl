---
title: Tworzenie niestandardowej sondy — Azure Application Gateway — PowerShell | Dokumentacja firmy Microsoft
description: Dowiedz się, jak tworzenie niestandardowej sondy dla bramy aplikacji przy użyciu programu PowerShell w usłudze Resource Manager
services: application-gateway
documentationcenter: na
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.assetid: 68feb660-7fa4-4f69-a7e4-bdd7bdc474db
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: victorh
ms.openlocfilehash: acd70bacd23755cd764bc782a297d80db3622424
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66135241"
---
# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Tworzenie niestandardowej sondy dla usługi Azure Application Gateway przy użyciu programu PowerShell dla usługi Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-create-probe-ps.md)
> * [Klasyczny portal Azure — program PowerShell](application-gateway-create-probe-classic-ps.md)

W tym artykule należy dodać niestandardowej sondy do istniejącej bramy aplikacji przy użyciu programu PowerShell. Niestandardowe sondy są przydatne w przypadku aplikacji używających konkretnej strony kontroli kondycji lub dla aplikacji, które nie są oferowane pomyślnej odpowiedzi na domyślnej aplikacji sieci web.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway-with-a-custom-probe"></a>Tworzenie bramy aplikacji za pomocą niestandardowej sondy

### <a name="sign-in-and-create-resource-group"></a>Zaloguj się i Utwórz grupę zasobów

1. Użyj `Connect-AzAccount` do uwierzytelniania.

   ```powershell
   Connect-AzAccount
   ```

1. Uzyskaj subskrypcje dla konta.

   ```powershell
   Get-AzSubscription
   ```

1. Wybierz subskrypcję platformy Azure do użycia.

   ```powershell
   Select-AzSubscription -Subscriptionid '{subscriptionGuid}'
   ```

1. Utwórz grupę zasobów. Jeśli masz istniejącą grupę zasobów, można pominąć ten krok.

   ```powershell
   New-AzResourceGroup -Name appgw-rg -Location 'West US'
   ```

Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. Ta lokalizacja będzie używana jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Upewnij się, że wszystkich poleceniach służących do tworzenia bramy aplikacji użyj tej samej grupie zasobów.

W powyższym przykładzie utworzyliśmy grupę zasobów o nazwie **appgw-RG** w lokalizacji **zachodnie stany USA**.

### <a name="create-a-virtual-network-and-a-subnet"></a>Tworzenie sieci wirtualnej i podsieci

Poniższy przykład tworzy sieć wirtualną i podsieć dla bramy aplikacji. Usługa Application gateway wymaga własnej podsieci, do użytku. Z tego powodu podsieć tworzona dla usługi application gateway należy mniejszy niż przestrzeni adresowej sieci Wirtualnej, aby umożliwić innych podsieci, aby być tworzone i używane.

```powershell
# Assign the address range 10.0.0.0/24 to a subnet variable to be used to create a virtual network.
$subnet = New-AzVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network named appgwvnet in resource group appgw-rg for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24.
$vnet = New-AzVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Assign a subnet variable for the next steps, which create an application gateway.
$subnet = $vnet.Subnets[0]
```

### <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Tworzenie publicznego adresu IP dla konfiguracji frontonu

Utwórz zasób publicznego adresu IP **publicIP01** w grupie zasobów **appgw-rg** dla regionu Zachodnie stany USA. W tym przykładzie używa publicznego adresu IP dla adresu IP frontonu bramy aplikacji.  Usługa Application gateway wymaga publicznego adresu IP, w związku z tym mają dynamicznie utworzoną nazwę DNS `-DomainNameLabel` nie można określić podczas tworzenia publicznego adresu IP.

```powershell
$publicip = New-AzPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location 'West US' -AllocationMethod Dynamic
```

### <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

Możesz skonfigurować wszystkie elementy konfiguracji przed utworzeniem bramy aplikacji. Poniższy przykład tworzy elementy konfiguracji, które są potrzebne do zasobu bramy aplikacji.

| **Składnik** | **Opis** |
|---|---|
| **Konfiguracja protokołu IP bramy** | Konfiguracja adresu IP bramy aplikacji.|
| **Pula zaplecza** | Pula adresów IP, nazw FQDN lub kart sieciowych znajdujących się na serwerach aplikacji, które hostują aplikację sieci web|
| **Sonda kondycji** | Niestandardowe sondy służy do monitorowania kondycji składowych puli zaplecza|
| **Ustawienia HTTP** | Kolekcja ustawień w tym, port, protokół, koligacji na podstawie plików cookie, sondy i limitu czasu.  Te ustawienia określają, jak ruch jest kierowany do składowych puli zaplecza|
| **Port frontonu** | Numer portu, którego bramy application gateway nasłuchuje ruchu|
| **Odbiornik** | Kombinacja protokołu, konfiguracja adresu IP frontonu i portu frontonu. Jest to, co nasłuchuje przychodzących żądań.
|**Reguły**| Trasy ruchu do odpowiedniego serwera zaplecza zgodnie z ustawieniami protokołu HTTP.|

```powershell
# Creates an application gateway Frontend IP configuration named gatewayIP01
$gipconfig = New-AzApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

#Creates a back-end IP address pool named pool01 with IP addresses 134.170.185.46, 134.170.188.221, 134.170.185.50.
$pool = New-AzApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Creates a probe that will check health at http://contoso.com/path/path.htm
$probe = New-AzApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/path.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Creates the backend http settings to be used. This component references the $probe created in the previous command.
$poolSetting = New-AzApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

# Creates a frontend port for the application gateway to listen on port 80 that will be used by the listener.
$fp = New-AzApplicationGatewayFrontendPort -Name frontendport01 -Port 80

# Creates a frontend IP configuration. This associates the $publicip variable defined previously with the front-end IP that will be used by the listener.
$fipconfig = New-AzApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Creates the listener. The listener is a combination of protocol and the frontend IP configuration $fipconfig and frontend port $fp created in previous steps.
$listener = New-AzApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Creates the rule that routes traffic to the backend pools.  In this example we create a basic rule that uses the previous defined http settings and backend address pool.  It also associates the listener to the rule
$rule = New-AzApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Sets the SKU of the application gateway, in this example we create a small standard application gateway with 2 instances.
$sku = New-AzApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# The final step creates the application gateway with all the previously defined components.
$appgw = New-AzApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location 'West US' -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Dodawanie sondy do istniejącej bramy aplikacji

Poniższy fragment kodu dodaje sondy do istniejącej bramy aplikacji.

```powershell
# Load the application gateway resource into a PowerShell variable by using Get-AzApplicationGateway.
$getgw =  Get-AzApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Create the probe object that will check health at http://contoso.com/path/path.htm
$getgw = Add-AzApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/custompath.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Set the backend HTTP settings to use the new probe
$getgw = Set-AzApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

# Save the application gateway with the configuration changes
Set-AzApplicationGateway -ApplicationGateway $getgw
```

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Usuwanie sondy z istniejącej bramie aplikacji

Poniższy fragment kodu usuwa sondę z istniejącej bramie aplikacji.

```powershell
# Load the application gateway resource into a PowerShell variable by using Get-AzApplicationGateway.
$getgw =  Get-AzApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Remove the probe from the application gateway configuration object
$getgw = Remove-AzApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

# Set the backend HTTP settings to remove the reference to the probe. The backend http settings now use the default probe
$getgw = Set-AzApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

# Save the application gateway with the configuration changes
Set-AzApplicationGateway -ApplicationGateway $getgw
```

## <a name="get-application-gateway-dns-name"></a>Pobieranie nazwy DNS bramy aplikacji

Po utworzeniu bramy następnym krokiem jest skonfigurowanie frontonu na potrzeby komunikacji. Gdy jest używany publiczny adres IP, brama aplikacji wymaga dynamicznie przypisywanej nazwy DNS, która nie jest przyjazna. Aby upewnić się, że użytkownicy końcowi mogą trafić bramę aplikacji, można użyć rekordu CNAME w celu wskazania publicznego punktu końcowego bramy aplikacji. [Konfigurowanie niestandardowej nazwy domeny dla platformy Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Aby to zrobić, pobierz szczegóły bramy aplikacji i skojarzony adres IP oraz nazwę DNS, używając elementu PublicIPAddress dołączonego do bramy aplikacji. Nazwa DNS bramy aplikacji powinna zostać użyta w celu utworzenia rekordu CNAME, który wskazuje dwóm aplikacjom internetowym tę nazwę DNS. Korzystanie z rekordów A nie jest zalecane, ponieważ adres VIP może ulec zmianie po ponownym uruchomieniu bramy aplikacji.

```powershell
Get-AzPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się skonfigurować odciążanie protokołu SSL, odwiedzając: [Konfigurowanie odciążania SSL](application-gateway-ssl-arm.md)

