---
title: Debugowanie kontenerów Windows za pomocą usługi Service Fabric i programu VS | Dokumentacja firmy Microsoft
description: Dowiedz się, jak można debugować kontenery Windows w usłudze Azure Service Fabric przy użyciu programu Visual Studio 2019 r.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/14/2019
ms.author: aljo, mikhegn
ms.openlocfilehash: 15f288d5400b49ec05c9ffb936fd2097cc61bae8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428151"
---
# <a name="how-to-debug-windows-containers-in-azure-service-fabric-using-visual-studio-2019"></a>Instrukcje: Debugowanie kontenerów Windows w usłudze Azure Service Fabric przy użyciu programu Visual Studio 2019 r.

Za pomocą programu Visual Studio 2019 r umożliwia debugowanie aplikacji .NET w kontenerach, jako usługi Service Fabric. W tym artykule przedstawiono sposób konfigurowania środowiska, a następnie debugować aplikacji .NET w kontenerze uruchomiona w lokalnym klastrze usługi Service Fabric.

## <a name="prerequisites"></a>Wymagania wstępne

* W systemie Windows 10 tego przewodnika Szybki Start do [Konfigurowanie systemu Windows 10 do uruchamiania kontenerów Windows](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10)
* W systemie Windows Server 2016, należy wykonać ten przewodnik Szybki start dotyczący [skonfigurować 2016 Windows umożliwiające uruchamianie kontenerów Windows](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server)
* Konfigurowanie środowiska lokalnego usługi Service Fabric, wykonując [przygotowanie środowiska projektowego w Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)

## <a name="configure-your-developer-environment-to-debug-containers"></a>Skonfiguruj środowisko dla deweloperów do debugowania kontenerów

1. Upewnij się, że platformy Docker dla usługi systemu Windows jest uruchomiona przed przejściem do następnego kroku.

1. Aby umożliwić obsługę rozpoznawania nazw DNS między kontenerów, musisz skonfigurować lokalnego klastra projektowego, używana jest nazwa komputera. Te kroki są również wymagane, aby adres usług przez zwrotny serwer proxy.
   1. Otwórz program PowerShell jako administrator
   2. Przejdź do folderu instalacyjnego zestawu SDK klastra, zwykle `C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup`.
   3. Uruchom skrypt `DevClusterSetup.ps1`

      ``` PowerShell
        C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1
      ```

      > [!NOTE]
      > Możesz użyć `-CreateOneNodeCluster` konfigurowania klastra z jednym węzłem. Wartość domyślna spowoduje utworzenie lokalnego klastra o pięciu węzłach.
      >

      Aby dowiedzieć się więcej na temat usługi DNS w usłudze Service Fabric, zobacz [usługa DNS w usłudze Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice). Aby dowiedzieć się więcej o korzystaniu z usługi Service Fabric zwrotny serwer proxy z usług działających w kontenerze, zobacz [zwrotny serwer proxy specjalna obsługa usług działających w kontenerach](service-fabric-reverseproxy.md#special-handling-for-services-running-in-containers).

### <a name="known-limitations-when-debugging-containers-in-service-fabric"></a>Znane ograniczenia podczas debugowania kontenerów w usłudze Service Fabric

Poniżej przedstawiono listę znanych ograniczeń polecenia za pomocą debugowania kontenerów w usłudze Service Fabric i możliwe rozwiązania:

* Przy użyciu localhost dla ClusterFQDNorIP nie obsługuje rozpoznawanie nazw DNS w kontenerach.
    * Rozwiązanie: Konfigurowanie lokalnego klastra przy użyciu nazwy komputera (zobacz powyżej)
* Uruchamianie Windows10 na maszynie wirtualnej nie będą otrzymywać odpowiedzi DNS do kontenera.
    * Rozwiązanie: Wyłącz Odciążanie sumy kontrolnej protokołu UDP dla protokołu IPv4 na karcie interfejsu Sieciowego maszyn wirtualnych
    * Uruchamianie Windows10 spowoduje zmniejszenie wydajności sieci na maszynie.
    * https://github.com/Azure/service-fabric-issues/issues/1061
* Rozpoznawanie usług w tej samej aplikacji przy użyciu systemu DNS nazwy usługi nie działa w przypadku Windows10, jeśli aplikacja została wdrożona przy użyciu narzędzia Docker Compose
    * Rozwiązanie: Użyj servicename.applicationname rozpoznać punkty końcowe usługi
    * https://github.com/Azure/service-fabric-issues/issues/1062
* Jeśli używasz adresu IP dla ClusterFQDNorIP, zmiana podstawowego adresu IP na hoście spowoduje uszkodzenie funkcjonalności DNS.
    * Rozwiązanie: Utwórz ponownie klaster przy użyciu nowego podstawowego adresu IP na hoście lub użyj nazwy komputera. Uszkodzenie to jest celowe.
* Jeśli klaster został utworzony za pomocą nazwy FQDN nie jest rozpoznawany w sieci, DNS zakończy się niepowodzeniem.
    * Rozwiązanie: Utwórz ponownie klaster lokalny przy użyciu podstawowego adresu IP hosta. Ten błąd jest celowe.
* Podczas debugowania kontenera, dzienniki platformy docker tylko będą dostępne w oknie danych wyjściowych programu Visual Studio nie przy użyciu usługi Service Fabric interfejsów API, w tym narzędzia Service Fabric Explorer

## <a name="debug-a-net-application-running-in-docker-containers-on-service-fabric"></a>Debugowanie aplikacji .NET działających w kontenerach platformy docker w usłudze Service Fabric

1. Uruchom program Visual Studio jako administrator.

1. Otwórz istniejącą aplikację platformy .NET lub Utwórz nową.

1. Kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj -> Obsługa Orkiestratora kontenerów -> Usługa Service Fabric**

1. Naciśnij klawisz **F5** można rozpocząć debugowania aplikacji.

    Program Visual Studio obsługuje konsoli i typów projektów programu ASP.NET, .NET i .NET Core.

## <a name="next-steps"></a>Kolejne kroki
Aby dowiedzieć się więcej o możliwościach usługi Service Fabric i kontenerów, zobacz overview](service-fabric-containers-overview.md) kontenerów usługi Service Fabric.
