---
title: Diagnozowanie problemu z filtrowaniem ruchu sieciowego maszyny wirtualnej — Szybki start — Azure PowerShell | Microsoft Docs
description: W tym przewodniku Szybki start zawarto informacje na temat sposobu diagnozowania problemu z filtrowaniem ruchu sieciowego maszyny wirtualnej przy użyciu możliwości weryfikowania przepływu adresów IP w usłudze Azure Network Watcher.
services: network-watcher
documentationcenter: network-watcher
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose a virtual machine (VM) network traffic filter problem that prevents communication to and from a VM.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 5cc735c6ad3986161b155ab97bbb3d6be5713d15
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729873"
---
# <a name="quickstart-diagnose-a-virtual-machine-network-traffic-filter-problem---azure-powershell"></a>Szybki start: diagnozowanie problemu z filtrowaniem ruchu sieciowego maszyny wirtualnej — Azure PowerShell

W tym przewodniku Szybki start wdrożysz maszynę wirtualną, a następnie sprawdzisz komunikację z adresem IP i adresem URL oraz komunikację z adresu IP. Określisz przyczynę niepowodzenia komunikacji oraz sposób rozwiązania problemu.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować program PowerShell i używać lokalnie, ten przewodnik Szybki Start wymaga programu Azure PowerShell `Az` modułu. Aby dowiedzieć się, jaka wersja została zainstalowana, uruchom polecenie `Get-Module -ListAvailable Az`. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-Az-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Connect-AzAccount`, aby utworzyć połączenie z platformą Azure.



## <a name="create-a-vm"></a>Tworzenie maszyny wirtualnej

Przed utworzeniem maszyny wirtualnej musisz utworzyć grupę zasobów, która będzie zawierała maszynę wirtualną. Utwórz grupę zasobów za pomocą polecenia [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup). Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

Utwórz maszynę wirtualną za pomocą polecenia [New-AzVM](/powershell/module/az.compute/new-azvm). Podczas wykonywania tego kroku jest wyświetlany monit o poświadczenia. Wprowadzane wartości są konfigurowane jako nazwa użytkownika i hasło dla maszyny wirtualnej.

```azurepowershell-interactive
$vM = New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "East US"
```

W ciągu kilku minut zostanie utworzona maszyna wirtualna. Wykonywanie pozostałych kroków możesz zacząć dopiero wtedy, gdy maszyna wirtualna zostanie utworzona i program PowerShell zwróci dane wyjściowe.

## <a name="test-network-communication"></a>Testowanie komunikacji sieciowej

Aby przetestować komunikację sieciową za pomocą usługi Network Watcher, należy najpierw włączyć usługę Network Watcher w regionie, w którym znajduje się maszyna wirtualna do przetestowania, a następnie użyć możliwości weryfikowania przepływu adresów IP dostępnej w usłudze Network Watcher.

### <a name="enable-network-watcher"></a>Włączanie usługi Network Watcher

Jeśli masz już usługę network watcher włączone w regionie wschodnie stany USA, należy użyć [Get AzNetworkWatcher](/powershell/module/az.network/get-aznetworkwatcher) można pobrać usługi network watcher. Poniższy przykład pobiera istniejącą usługę Network Watcher o nazwie *NetworkWatcher_eastus* znajdującą się w grupie zasobów *NetworkWatcherRG*:

```azurepowershell-interactive
$networkWatcher = Get-AzNetworkWatcher `
  -Name NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG
```

Jeśli nie masz jeszcze usługi network watcher włączone w regionie wschodnie stany USA, należy użyć [New AzNetworkWatcher](/powershell/module/az.network/new-aznetworkwatcher) do utworzenia usługi network watcher w regionie wschodnie stany USA:

```azurepowershell-interactive
$networkWatcher = New-AzNetworkWatcher `
  -Name "NetworkWatcher_eastus" `
  -ResourceGroupName "NetworkWatcherRG" `
  -Location "East US"
```

### <a name="use-ip-flow-verify"></a>Korzystanie z weryfikowania przepływu adresów IP

Podczas tworzenia maszyny wirtualnej platforma Azure domyślnie zezwala na i blokuje ruch sieciowy do i z maszyny wirtualnej. Domyślne ustawienia platformy Azure można później zastąpić, aby zezwalać lub nie zezwalać na dodatkowe typy ruchu. Aby sprawdzić, czy ruch jest dozwolony lub zablokowany do różnych miejsc docelowych i ze źródłowego adresu IP, należy użyć [AzNetworkWatcherIPFlow testu](/powershell/module/az.network/test-aznetworkwatcheripflow) polecenia.

Przetestuj komunikację wychodzącą z maszyny wirtualnej do jednego z adresów IP domeny www.bing.com:

```azurepowershell-interactive
Test-AzNetworkWatcherIPFlow `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $vM.Id `
  -Direction Outbound `
  -Protocol TCP `
  -LocalIPAddress 192.168.1.4 `
  -LocalPort 60000 `
  -RemoteIPAddress 13.107.21.200 `
  -RemotePort 80
```

Wynik zwrócony po kilku sekundach informuje, że dostęp jest dozwolony dzięki regule zabezpieczeń o nazwie **AllowInternetOutbound**.

Przetestuj komunikację wychodzącą z maszyny wirtualnej do adresu 172.31.0.100:

```azurepowershell-interactive
Test-AzNetworkWatcherIPFlow `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $vM.Id `
  -Direction Outbound `
  -Protocol TCP `
  -LocalIPAddress 192.168.1.4 `
  -LocalPort 60000 `
  -RemoteIPAddress 172.31.0.100 `
  -RemotePort 80
```

Zwrócony wynik informuje o odmowie dostępu przez regułę zabezpieczeń o nazwie **DefaultOutboundDenyAll**.

Przetestuj komunikację przychodzącą do maszyny wirtualnej z adresu 172.31.0.100:

```azurepowershell-interactive
Test-AzNetworkWatcherIPFlow `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $vM.Id `
  -Direction Inbound `
  -Protocol TCP `
  -LocalIPAddress 192.168.1.4 `
  -LocalPort 80 `
  -RemoteIPAddress 172.31.0.100 `
  -RemotePort 60000
```

Zwrócony wynik informuje o odmowie dostępu z powodu reguły zabezpieczeń o nazwie **DefaultInboundDenyAll**. Gdy już wiesz, które reguły zabezpieczeń zezwalają na ruch lub blokują ruch do lub z maszyny wirtualnej, możesz określić, jak rozwiązać problemy.

## <a name="view-details-of-a-security-rule"></a>Wyświetlanie szczegółów reguły zabezpieczeń

Aby ustalić, dlaczego reguły w [Test komunikacji sieciowej](#test-network-communication) zezwalające lub uniemożliwia komunikację, przejrzyj obowiązujących reguł zabezpieczeń dla interfejsu sieciowego z [Get-AzEffectiveNetworkSecurityGroup](/powershell/module/az.network/get-azeffectivenetworksecuritygroup):

```azurepowershell-interactive
Get-AzEffectiveNetworkSecurityGroup `
  -NetworkInterfaceName myVm `
  -ResourceGroupName myResourceGroup
```

Zwrócone dane wyjściowe obejmują następujący tekst dla reguły **AllowInternetOutbound**, która zezwalała na dostęp ruchu wychodzącego do witryny www.bing.com w sekcji [Korzystanie z weryfikowania przepływu adresów IP](#use-ip-flow-verify):

```powershell
{
  "Name":
"defaultSecurityRules/AllowInternetOutBound",
  "Protocol": "All",
  "SourcePortRange": [
    "0-65535"
  ],
  "DestinationPortRange": [
    "0-65535"
  ],
  "SourceAddressPrefix": [
    "0.0.0.0/0"
  ],
  "DestinationAddressPrefix": [
    "Internet"
  ],
  "ExpandedSourceAddressPrefix": [],
  "ExpandedDestinationAddressPrefix": [
    "1.0.0.0/8",
    "2.0.0.0/7",
    "4.0.0.0/6",
    "8.0.0.0/7",
    "11.0.0.0/8",
    "12.0.0.0/6",
    ...
    ],
    "Access": "Allow",
    "Priority": 65001,
    "Direction": "Outbound"
  },
```

W danych wyjściowych można zobaczyć, że właściwość **DestinationAddressPrefix** jest określona jako **Internet**. Nie jest jednak jasne, jak adres 13.107.21.200, który był testowany w kroku [Korzystanie z weryfikowania przepływu adresów IP](#use-ip-flow-verify), jest związany z **Internetem**. W parametrze **ExpandedDestinationAddressPrefix** znajduje się kilka prefiksów adresów. Jednym z prefiksów na liście jest prefiks **12.0.0.0/6**, który obejmuje zakres adresów IP 12.0.0.1-15.255.255.254. Ponieważ adres 13.107.21.200 mieści się w tym zakresie adresów, reguła **AllowInternetOutBound** zezwala na ruch wychodzący. Ponadto na liście danych wyjściowych zwracanych przez polecenie `Get-AzEffectiveNetworkSecurityGroup` nie ma żadnych reguł o wyższym **priorytecie** (niższym numerze), które przesłaniają tę regułę. Aby blokować komunikację wychodzącą z adresem 13.107.21.200, możesz dodać regułę zabezpieczeń o wyższym priorytecie, która blokuje ruch wychodzący na porcie 80 do tego adresu IP.

Po uruchomieniu polecenia `Test-AzNetworkWatcherIPFlow` w celu przetestowania komunikacji wychodzącej na adres 172.131.0.100 w kroku [Korzystanie z weryfikowania przepływu adresów IP](#use-ip-flow-verify) dane wyjściowe zawierały informację, że reguła **DefaultOutboundDenyAll** blokuje komunikację. Reguła **DefaultOutboundDenyAll** odpowiada regule **DenyAllOutBound** wymienionej w następujących danych wyjściowych polecenia `Get-AzEffectiveNetworkSecurityGroup`:

```powershell
{
"Name": "defaultSecurityRules/DenyAllOutBound",
"Protocol": "All",
"SourcePortRange": [
  "0-65535"
],
"DestinationPortRange": [
  "0-65535"
],
"SourceAddressPrefix": [
  "0.0.0.0/0"
],
"DestinationAddressPrefix": [
  "0.0.0.0/0"
],
"ExpandedSourceAddressPrefix": [],
"ExpandedDestinationAddressPrefix": [],
"Access": "Deny",
"Priority": 65500,
"Direction": "Outbound"
}
```

Reguła wyświetla wartość **0.0.0.0/0** dla właściwości **DestinationAddressPrefix**. Reguła blokuje komunikację wychodzącą z adresem 172.131.0.100, ponieważ ten adres nie mieści się w ramach prefiksu **DestinationAddressPrefix** żadnej innej reguły ruchu wychodzącego w danych wyjściowych polecenia `Get-AzEffectiveNetworkSecurityGroup`. Aby zezwolić na komunikację wychodzącą, możesz dodać regułę zabezpieczeń o wyższym priorytecie, która zezwala na ruch wychodzący na porcie 80 pod adresem 172.131.0.100.

Po uruchomieniu polecenia `Test-AzNetworkWatcherIPFlow` w celu przetestowania komunikacji przychodzącej z adresu 172.131.0.100 w kroku [Korzystanie z weryfikowania przepływu adresów IP](#use-ip-flow-verify) dane wyjściowe zawierały informację, że reguła **DefaultInboundDenyAll** blokuje komunikację. Reguła **DefaultInboundDenyAll** odpowiada regule **DenyAllInBound** wymienionej w następujących danych wyjściowych polecenia `Get-AzEffectiveNetworkSecurityGroup`:

```powershell
{
"Name": "defaultSecurityRules/DenyAllInBound",
"Protocol": "All",
"SourcePortRange": [
  "0-65535"
],
"DestinationPortRange": [
  "0-65535"
],
"SourceAddressPrefix": [
  "0.0.0.0/0"
],
"DestinationAddressPrefix": [
  "0.0.0.0/0"
],
"ExpandedSourceAddressPrefix": [],
"ExpandedDestinationAddressPrefix": [],
"Access": "Deny",
"Priority": 65500,
"Direction": "Inbound"
},
```

Reguła **DenyAllInBound** jest stosowana, ponieważ, jak pokazano w danych wyjściowych polecenia `Get-AzEffectiveNetworkSecurityGroup`, nie istnieje żadna inna reguła o wyższym priorytecie, która zezwala na ruch przychodzący na porcie 80 do maszyny wirtualnej z adresu 172.131.0.100. Aby zezwolić na komunikację przychodzącą, możesz dodać regułę zabezpieczeń o wyższym priorytecie, która zezwala na ruch przychodzący na porcie 80 z adresu 172.131.0.100.

Za pomocą testów w tym przewodniku Szybki start przetestowano konfigurację platformy Azure. Jeśli testy zwróciły oczekiwane wyniki, ale nadal występują problemy z siecią, upewnij się, że między maszyną wirtualną a punktem końcowym, z którym się komunikujesz, nie znajduje się zapora oraz że system operacyjny maszyny wirtualnej nie ma zapory, która zezwala na komunikację lub ją blokuje.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie będą już potrzebne, możesz użyć [AzResourceGroup Usuń](/powershell/module/az.resources/remove-azresourcegroup) Aby usunąć grupę zasobów i wszystkie zawarte w niej zasoby:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Kolejne kroki

W tym przewodniku Szybki utworzono maszynę wirtualną i zdiagnozowano filtry przychodzącego i wychodzącego ruchu sieciowego. Wiesz już, że reguły sieciowej grupy zabezpieczeń zezwalają na lub blokują ruch do i z maszyny wirtualnej. Dowiedz się więcej o [regułach zabezpieczeń](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) oraz [tworzeniu reguł zabezpieczeń](../virtual-network/manage-network-security-group.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-security-rule).

Nawet przy zastosowaniu odpowiednich filtrów ruchu sieciowego komunikacja z maszyną wirtualną może nadal kończyć się niepowodzeniem z powodu konfiguracji routingu. Aby dowiedzieć się, jak diagnozować problemy z routingiem sieci maszyny wirtualnej, zobacz [Diagnozowanie problemów z routingiem maszyny wirtualnej](diagnose-vm-network-routing-problem-powershell.md). Aby za pomocą jednego narzędzia diagnozować problemy z routingiem ruchu wychodzącego, opóźnieniem i filtrowaniem ruchu, zobacz [Rozwiązywanie problemów z połączeniami](network-watcher-connectivity-powershell.md).
