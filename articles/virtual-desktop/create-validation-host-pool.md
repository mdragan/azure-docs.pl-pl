---
title: Tworzenie puli hosta Windows pulpitu wirtualnego (wersja zapoznawcza) do sprawdzania poprawności aktualizacji usługi — Azure
description: Jak utworzyć pulę hosta sprawdzania poprawności do monitorowania usługi aktualizacje przed zarządzeniem aktualizacji do środowiska produkcyjnego.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 05/08/2019
ms.author: v-chjenk
ms.openlocfilehash: c9b2a593a6943fe2e9577acc61b1d5a7bcd98607
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67070662"
---
# <a name="tutorial-create-a-host-pool-to-validate-service-updates"></a>Samouczek: Tworzenie puli hostów na potrzeby weryfikacji aktualizacji usług

Pule hosta to zbiór przynajmniej jednej identycznych maszyn wirtualnych w środowiskach dzierżawy Windows wirtualnego Desktop w wersji zapoznawczej. Przed wdrożeniem pule hostów do środowiska produkcyjnego, zdecydowanie zalecamy utworzenie puli hosta sprawdzania poprawności. Aktualizacje są stosowane najpierw do pul hosta sprawdzania poprawności, co pozwala na monitorowanie aktualizacji usługi przed zarządzeniem do środowiska produkcyjnego. Bez pulę hosta sprawdzania poprawności może nie wykryć zmiany, które wprowadzają błędy, które może skutkować przestojem dla użytkowników w środowisku produkcyjnym.

Aby zapewnić działanie Twojej aplikacji z najnowszymi aktualizacjami, puli hosta weryfikacji powinny być podobne do hosta pul w środowisku produkcyjnym, jak to możliwe. Użytkownicy należy tak często, łączyć się z puli hosta sprawdzania poprawności do puli hosta produkcji. Jeśli zautomatyzowano testowania w puli hosta powinien być dołączany do automatycznego testowania w puli hosta sprawdzania poprawności.

Możesz debugować błędy w puli hosta sprawdzania poprawności z oboma [funkcji diagnostyki](diagnostics-role-service.md) lub [pulpitu wirtualnego Windows artykuły dotyczące rozwiązywania problemów](https://docs.microsoft.com/Azure/virtual-desktop/troubleshoot-set-up-overview).

>[!NOTE]
> Zaleca się pozostawienie puli hosta sprawdzania poprawności w miejscu, aby przetestować wszystkie przyszłe aktualizacje.

Przed przystąpieniem do wykonywania [Pobierz i zaimportuj moduł programu PowerShell pulpitu wirtualnego Windows](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), jeśli jeszcze go.

## <a name="create-your-host-pool"></a>Tworzenie puli hosta

Aby utworzyć pulę hosta, postępując zgodnie z instrukcjami w jednym z następujących artykułów:
- [Samouczek: Utwórz pulę hosta za pomocą portalu Azure Marketplace](create-host-pools-azure-marketplace.md)
- [Utwórz pulę hosta przy użyciu szablonu usługi Azure Resource Manager](create-host-pools-arm-template.md)
- [Utwórz pulę hosta przy użyciu programu PowerShell](create-host-pools-powershell.md)

## <a name="define-your-host-pool-as-a-validation-host-pool"></a>Zdefiniuj puli hosta jako pula hosta sprawdzania poprawności

Uruchom następujące polecenia cmdlet programu PowerShell nowej puli hosta jest definiowana jako pulę hosta sprawdzania poprawności. Zastąp wartości w cudzysłowie według wartości, które są odpowiednie do danej sesji:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Uruchom następujące polecenia cmdlet programu PowerShell, aby upewnić się, że ustawiono właściwość sprawdzania poprawności. Zastąp wartości w cudzysłowie według wartości, które są odpowiednie do danej sesji.

```powershell
Get-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Wyniki polecenia cmdlet powinny wyglądać podobnie do tych danych wyjściowych:

```
    TenantName          : contoso 
    TenantGroupName     : Default Tenant Group
    HostPoolName        : contosoHostPool
    FriendlyName        :
    Description         :
    Persistent          : False 
    CustomRdpProperty   : use multimon:i:0;
    MaxSessionLimit     : 10
    LoadBalancerType    : BreadthFirst
    ValidationEnv       : True
    Ring                :
```

## <a name="update-schedule"></a>Aktualizacja harmonogramu

W wersji zapoznawczej aktualizacji usługi występuje w około miesięczne wydawania oprogramowania. Jeśli występują poważne problemy, aktualizacje krytyczne będą dostarczane tempa częściej.

## <a name="next-steps"></a>Kolejne kroki

Teraz, po utworzeniu puli hosta sprawdzania poprawności, można dowiesz się, jak wdrożyć i nawiązać połączenie z narzędziem do zarządzania do zarządzania zasobami pulpitu wirtualnego firmy Microsoft.

> [!div class="nextstepaction"]
> [Samouczek narzędzia do zarządzania wdrażania](./manage-resources-using-ui.md)
