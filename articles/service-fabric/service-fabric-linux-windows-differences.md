---
title: Różnice w usłudze Azure Service Fabric w systemie Linux i Windows | Microsoft Docs
description: Różnice między usługą Azure Service Fabric w systemie Linux a usługą Azure Service Fabric w systemie Windows.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 829a3ea6e8fafa24831f1731e402a3991c0258c7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60719995"
---
# <a name="differences-between-service-fabric-on-linux-and-windows"></a>Różnice między usługą Service Fabric w systemach Linux i Windows

Pewne funkcje, które są obsługiwane w systemie Windows, nie są jeszcze obsługiwane w systemie Linux. Po pewnym czasie zestawy funkcji będą działać w obydwu systemach i w każdej nowej wersji ta różnica między funkcjami będzie się zmniejszać. Oto różnice między najnowszymi dostępnymi wersjami.

* Usługa envoy (odwrotnego serwera Proxy) jest dostępna w wersji zapoznawczej w systemie Linux
* Autonomiczny instalator dla systemu Linux nie jest dostępny w systemie Linux
* Przekierowywanie konsoli (nieobsługiwane w klastrach produkcyjnych w systemie Linux ani Windows)
* Usługa analizy błędów w systemie Linux
* Usługa DNS dla usług Service Fabric (usługa DNS jest obsługiwana dla kontenerów w systemie Linux)
* Odpowiedniki poleceń interfejsu wiersza polecenia dla niektórych poleceń programu PowerShell (lista znajduje się poniżej — większość z nich ma zastosowanie tylko do klastrów autonomicznych)

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a>Polecenia cmdlet programu PowerShell, które nie działają względem klastra usługi Service Fabric z systemem Linux

* Invoke-ServiceFabricChaosTestScenario
* Invoke-ServiceFabricFailoverTestScenario
* Invoke-ServiceFabricPartitionDataLoss
* Invoke-ServiceFabricPartitionQuorumLoss
* Restart-ServiceFabricPartition
* Start-ServiceFabricNode
* Stop-ServiceFabricNode
* Get-ServiceFabricImageStoreContent
* Get-ServiceFabricChaosReport
* Get-ServiceFabricNodeTransitionProgress
* Get-ServiceFabricPartitionDataLossProgress
* Get-ServiceFabricPartitionQuorumLossProgress
* Get-ServiceFabricPartitionRestartProgress
* Get-ServiceFabricTestCommandStatusList
* Remove-ServiceFabricTestState
* Start-ServiceFabricChaos
* Start-ServiceFabricNodeTransition
* Start-ServiceFabricPartitionDataLoss
* Start-ServiceFabricPartitionQuorumLoss
* Start-ServiceFabricPartitionRestart
* Stop-ServiceFabricChaos
* Stop-ServiceFabricTestCommand
* Get-ServiceFabricNodeConfiguration
* Get-ServiceFabricClusterConfiguration
* Get-ServiceFabricClusterConfigurationUpgradeStatus
* Get-ServiceFabricPackageDebugParameters
* New-ServiceFabricPackageDebugParameter
* New-ServiceFabricPackageSharingPolicy
* Add-ServiceFabricNode
* Copy-ServiceFabricClusterPackage
* Get-ServiceFabricRuntimeSupportedVersion
* Get-ServiceFabricRuntimeUpgradeVersion
* New-ServiceFabricCluster
* New-ServiceFabricNodeConfiguration
* Remove-ServiceFabricCluster
* Remove-ServiceFabricClusterPackage
* Remove-ServiceFabricNodeConfiguration
* Test-ServiceFabricClusterManifest
* Test-ServiceFabricConfiguration
* Update-ServiceFabricNodeConfiguration
* Approve-ServiceFabricRepairTask
* Complete-ServiceFabricRepairTask
* Get-ServiceFabricRepairTask
* Invoke-ServiceFabricDecryptText
* Invoke-ServiceFabricEncryptSecret
* Invoke-ServiceFabricEncryptText
* Invoke-ServiceFabricInfrastructureCommand
* Invoke-ServiceFabricInfrastructureQuery
* Remove-ServiceFabricRepairTask
* Start-ServiceFabricRepairTask
* Stop-ServiceFabricRepairTask
* Update-ServiceFabricRepairTaskHealthPolicy



## <a name="next-steps"></a>Kolejne kroki
* [Przygotowywanie środowiska projektowego w systemie Linux](service-fabric-get-started-linux.md)
* [Przygotowywanie środowiska projektowego w systemie OSX](service-fabric-get-started-mac.md)
* [Create and deploy your first Service Fabric Java application on Linux using Yeoman](service-fabric-create-your-first-linux-application-with-java.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu programu Yeoman)
* [Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse](service-fabric-get-started-eclipse.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu wtyczki usługi Service Fabric dla środowiska Eclipse)
* [Create your first CSharp application on Linux](service-fabric-create-your-first-linux-application-with-csharp.md) (Tworzenie pierwszej aplikacji CSharp w systemie Linux)
* [Use the Service Fabric CLI to manage your applications](service-fabric-application-lifecycle-sfctl.md) (Zarządzanie aplikacjami przy użyciu interfejsu wiersza polecenia usługi Service Fabric)
