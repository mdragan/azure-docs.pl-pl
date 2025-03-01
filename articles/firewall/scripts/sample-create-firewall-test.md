---
title: Przykładowy skrypt programu Azure PowerShell — tworzenie środowiska testowego usługi Azure Firewall
description: Przykładowy skrypt programu Azure PowerShell — tworzenie środowiska testowego usługi Azure Firewall.
services: virtual-network
author: vhorne
ms.service: firewall
ms.devlang: powershell
ms.topic: sample
ms.date: 8/13/2018
ms.author: victorh
ms.openlocfilehash: 6e85bd6ec51cff27fed6d0b2d9e73f94325e4d4f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60194196"
---
# <a name="create-an-azure-firewall-test-environment"></a>Tworzenie środowiska testowego usługi Azure Firewall

Ten przykładowy skrypt tworzy zaporę i testowe środowisko sieciowe. Sieć ma jedną sieć wirtualną z trzema podsieciami: *AzureFirewallSubnet*, *ServersSubnet* i *JumpboxSubnet*. Podsieci ServersSubnet i JumpboxSubnet obejmują po jednej 2-rdzeniowej maszynie z systemem Windows Server.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Zapora znajduje się w podsieci AzureFirewallSubnet i jest w niej skonfigurowana kolekcja reguł aplikacji z pojedynczą regułą zezwalającą na dostęp do witryny www.microsoft.com.

Tworzona jest trasa zdefiniowana przez użytkownika, która kieruje ruch sieciowy z podsieci ServersSubnet przez zaporę, gdzie są stosowane reguły zapory.

Skrypt można uruchomić z poziomu usługi Azure [Cloud Shell](https://shell.azure.com/powershell) lub z lokalnej instalacji programu PowerShell. 

Po uruchomieniu programu PowerShell lokalnie, ten skrypt wymaga programu Azure PowerShell. Aby dowiedzieć się, jaka wersja została zainstalowana, uruchom polecenie `Get-Module -ListAvailable Az`. 

Jeśli konieczne jest uaktualnienie, możesz użyć modułu `PowerShellGet`, który jest wbudowany w system Windows 10 i Windows Server 2016.

> [!NOTE]
>Inne wersje systemu Windows wymagają zainstalowania modułu `PowerShellGet`, zanim będzie można go użyć. Aby określić, czy ten moduł jest zainstalowany w Twoim systemie, możesz uruchomić polecenie `Get-Module -Name PowerShellGet -ListAvailable | Select-Object -Property Name,Version,Path`. Jeśli dane wyjściowe są puste, musisz zainstalować najnowszą wersję pakietu [Windows Management Framework](https://www.microsoft.com/download/details.aspx?id=54616).

Aby uzyskać więcej informacji, zobacz [Instalowanie programu Azure PowerShell](/powershell/azure/install-Az-ps)

Dowolna istniejąca instalacja programu Azure PowerShell zainstalowana za pomocą Instalatora platformy internetowej będzie powodować konflikt z instalacją modułu PowerShellGet i musi zostać usunięta.

Pamiętaj, że jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Connect-AzAccount`, aby utworzyć połączenie z platformą Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt


[!code-azurepowershell-interactive[main](../../../powershell_scripts/firewall/create-fw-test.ps1  "Create a firewall test environment")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszynę wirtualną i wszystkie powiązane zasoby:

```powershell
Remove-AzResourceGroup -Name AzfwSampleScriptEastUS -Force
```

## <a name="script-explanation"></a>Objaśnienia dla skryptu

Ten skrypt zawiera następujące polecenia służące do tworzenia grupy zasobów, sieci wirtualnej i sieciowych grup zabezpieczeń. Każde polecenie w poniższej tabeli stanowi link do dokumentacji polecenia:

| Polecenie | Uwagi |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tworzy grupę zasobów, w której są przechowywane wszystkie zasoby. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Tworzy obiekt konfiguracji podsieci. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Tworzy sieć wirtualną i podsieć frontonu platformy Azure. |
| [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) | Tworzy reguły zabezpieczeń do przypisania do sieciowej grupy zabezpieczeń. |
| [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) |Tworzy reguły sieciowych grup zabezpieczeń, które zezwalają na użycie określonych portów w wybranych sieciach lub blokują te porty. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) | Tworzy skojarzenia sieciowych grup zabezpieczeń z podsieciami. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Tworzy publiczny adres IP umożliwiający uzyskiwanie dostępu do maszyny wirtualnej z Internetu. |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Tworzy interfejsy sieci wirtualnej i dołącza je do podsieci frontonu i zaplecza sieci wirtualnej. |
| [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig) | Tworzy konfigurację maszyny wirtualnej. Ta konfiguracja zawiera informacje, takie jak nazwa maszyny wirtualnej, system operacyjny i poświadczenia administracyjne. Jest ona używana podczas tworzenia maszyny wirtualnej. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Tworzy maszynę wirtualną. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Usuwa grupę zasobów i wszystkie zasoby w niej zawarte. |
|[Nowe AzFirewall](https://docs.microsoft.com/powershell/module/az.network/new-azfirewall)| Tworzy nową usługę Azure Firewall.|
|[Get-AzFirewall](https://docs.microsoft.com/powershell/module/az.network/get-azfirewall)|Pobiera obiekt usługi Azure Firewall.|
|[New-AzFirewallApplicationRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallapplicationrule)|Tworzy nową regułę aplikacji usługi Azure Firewall.|
|[Set-AzFirewall](https://docs.microsoft.com/powershell/module/az.network/set-azfirewall)|Zatwierdza zmiany w obiekcie usługi Azure Firewall.|

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat programu Azure PowerShell, zobacz [dokumentację programu Azure PowerShell](/powershell/azure/overview).

