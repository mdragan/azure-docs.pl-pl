---
title: Konfigurowanie konta gMSA dla usługi kontenerów Azure Service Fabric | Dokumentacja firmy Microsoft
description: Dowiedz się teraz skonfigurować konta gMSA dla kontenera z systemem w usłudze Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2019
ms.author: aljo, subramar
ms.openlocfilehash: fc4edf4cb411ea2872437f4909f06e5ac2b9f622
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64926366"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Konfigurowanie konta gMSA dla kontenerów Windows uruchomiona w usłudze Service Fabric

Aby skonfigurować gMSA (grupy konta usług zarządzane), plik specyfikacji poświadczeń (`credspec`) jest umieszczany na wszystkich węzłach w klastrze. We wszystkich węzłach za pomocą rozszerzenia maszyny Wirtualnej, można skopiować pliku.  `credspec` Plik musi zawierać informacje o koncie gMSA. Aby uzyskać więcej informacji na temat `credspec` plików, zobacz [tworzenie Specyfikacja poświadczeń](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec). Specyfikacja poświadczeń i `Hostname` tag są określone w manifeście aplikacji. `Hostname` Tag musi odpowiadać nazwie konta gMSA, zgodną z kontenera.  `Hostname` Tagu umożliwia kontener, aby uwierzytelniać się do innych usług w domenie przy użyciu uwierzytelniania Kerberos.  Przykład określania `Hostname` i `credspec` w aplikacji manifest przedstawiono w poniższym fragmencie kodu:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
Kolejnym krokiem przeczytaj następujące artykuły:

* [Wdrażanie kontenera Windows w usłudze Service Fabric w systemie Windows Server 2016](service-fabric-get-started-containers.md)
* [Wdrażanie kontenera platformy Docker w usłudze Service Fabric w systemie Linux](service-fabric-get-started-containers-linux.md)
