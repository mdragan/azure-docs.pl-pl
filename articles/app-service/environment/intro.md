---
title: Wprowadzenie do środowisk App Service Environment — Azure
description: Krótkie omówienie środowisk Azure App Service Environment
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 3c7eaefa-1850-4643-8540-428e8982b7cb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 04/19/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 48b053b6520bff2ac83cd02af31194f81413e92c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62115910"
---
# <a name="introduction-to-the-app-service-environments"></a>Wprowadzenie do środowisk App Service Environment #
 
## <a name="overview"></a>Omówienie ##

Azure App Service Environment to funkcja usługi Azure App Service udostępniająca w pełni izolowane, dedykowane środowisko do bezpiecznego uruchamiania aplikacji usługi App Service w dużej skali. Ta funkcja umożliwia hostowanie:

* Aplikacji internetowych systemu Windows
* Aplikacji internetowych systemu Linux 
* Kontenerów Docker
* Aplikacji mobilnych
* Funkcje

Środowiska App Service Environment (ASE) są odpowiednie dla obciążeń aplikacji o następujących wymaganiach:

* Bardzo duża skala.
* Izolacja i bezpieczny dostęp do sieci.
* Wysoki poziom użycia pamięci.

Klienci mogą tworzyć wiele środowisk ASE w pojedynczym regionie platformy Azure lub w wielu regionach platformy Azure. Dzięki tej elastyczności środowiska ASE doskonale nadają się dla warstw aplikacji bezstanowych skalowanych w poziomie na potrzeby obsługi obciążeń o dużej liczbie żądań na sekundę (RPS).

Środowiska ASE są izolowane w celu uruchamiania aplikacji tylko jednego klienta i są zawsze wdrażane w sieci wirtualnej. Klienci mają precyzyjną kontrolę nad przychodzącym i wychodzącym ruchem sieciowym aplikacji. Aplikacje mogą nawiązywać bezpieczne szybkie połączenia za pośrednictwem sieci VPN z zasobami lokalnymi firmy.

* Środowiska ASE są oferowane w oddzielnej warstwie cenowej. Dowiedz się, jak [oferta Izolowana](https://channel9.msdn.com/Shows/Azure-Friday/Security-and-Horsepower-with-App-Service-The-New-Isolated-Offering?term=app%20service%20environment) pomaga zwiększać hiperskalowalność i bezpieczeństwo.
* [Środowiska usługi App Service w wersji 2](https://channel9.msdn.com/Blogs/Azure/Azure-Application-Service-Environments-v2-Private-PaaS-Environments-in-the-Cloud?term=app%20service%20environment) zapewniają otoczenie do ochrony aplikacji w podsieci sieci użytkownika oraz zapewniają własne prywatne wdrożenie usługi Azure App Service.
* Za pomocą wielu środowisk ASE można realizować skalowanie w poziomie. Aby uzyskać więcej informacji, dowiedz się, [jak skonfigurować rozproszony geograficznie zasięg aplikacji](app-service-app-service-environment-geo-distributed-scale.md).
* Za pomocą środowisk ASE można skonfigurować architekturę zabezpieczeń, jak zaprezentowano w tym filmie AzureCon Deep Dive. Aby dowiedzieć się, jak skonfigurowano architekturę zabezpieczeń przedstawioną w tym filmie AzureCon Deep Dive, zobacz [artykuł na temat implementowania warstwowej architektury zabezpieczeń](app-service-app-service-environment-layered-security.md) za pomocą środowisk App Service Environment.
* Dostęp aplikacji działających w środowiskach ASE może być kontrolowany za pomocą bram na urządzeniach nadrzędnych, takich jak zapory aplikacji internetowych (WAF). Aby uzyskać więcej informacji, zobacz temat [Zapora aplikacji internetowej (WAF)][AppGW].

## <a name="dedicated-environment"></a>Dedykowane środowisko ##

Środowisko ASE jest przeznaczone wyłącznie dla jednej subskrypcji i może hostować do 100 wystąpień planu usługi App Service. Zakres może obejmować 100 wystąpień w jednym planie usługi App Service lub 100 planów usługi App Service z jednym wystąpieniem każdy — możliwe są też wszelkie konfiguracje pośrednie.

Środowisko ASE składa się z frontonów i procesów roboczych. Frontony są odpowiedzialne za zakończenia połączeń HTTP/HTTPS i automatyczne równoważenie obciążenia żądań aplikacji w środowisku ASE. Frontony są automatycznie dodawane w przypadku skalowania w poziomie planów usługi App Service w środowisku ASE.

Procesy robocze to role, które hostują aplikacje klienta. Procesy robocze są dostępne w trzech stałych rozmiarach:

* Jeden procesor vCPU/3,5 GB pamięci RAM
* Dwa procesory vCPU/7 GB pamięci RAM
* Cztery procesory vCPU/14 GB pamięci RAM

Klienci nie muszą zarządzać frontonami ani procesami roboczymi. Wszystkie elementy infrastruktury są automatycznie dodawane, gdy klienci skalują plany usługi App Service. Gdy w środowisku ASE są tworzone lub skalowane plany usługi App Service, wymagane elementy infrastruktury są odpowiednio dodawane lub usuwane.

Obowiązuje zryczałtowana stawka miesięczna za środowisko ASE, która pokrywa koszty infrastruktury i nie zmienia się z rozmiarem tego środowiska. Ponadto obowiązują opłaty za procesory vCPU planów usługi App Service. Wszystkie aplikacje hostowane w środowisku ASE znajdują się w jednostce SKU wyceny „izolowanej” (Isolated). Aby uzyskać informacje o cenach dotyczących środowiska ASE, przejdź na stronę [App Service — cennik][Pricing] i zapoznaj się z dostępnymi opcjami środowisk ASE.

## <a name="virtual-network-support"></a>Obsługa sieci wirtualnej ##

Funkcja środowiska ASE polega na wdrożeniu usługi Azure App Service bezpośrednio w sieci wirtualnej usługi Azure Resource Manager klienta. Aby dowiedzieć się więcej o sieciach wirtualnych platformy Azure, zobacz [często zadawane pytania na ich temat](https://azure.microsoft.com/documentation/articles/virtual-networks-faq/). Środowisko ASE zawsze istnieje w sieci wirtualnej, a dokładniej — w podsieci sieci wirtualnej. Za pomocą funkcji zabezpieczeń sieci wirtualnych można sterować przychodzącą i wychodzącą komunikacją sieciową używanych aplikacji.

Środowisko ASE może być środowiskiem połączonym z Internetem za pomocą publicznego adresu IP lub wewnętrznym i mającym tylko adres wewnętrznego modułu równoważenia obciążenia (ILB, Internal Load Balancer) platformy Azure.

[Sieciowe grupy zabezpieczeń][NSGs] ograniczają komunikację sieciową przychodzącą do podsieci, w której znajduje się środowisko ASE. Za pomocą tych grup można uruchamiać aplikacje za urządzeniami i usługami nadrzędnymi, takimi jak zapora aplikacji internetowych i sieciowi dostawcy SaaS.

Aplikacje często muszą również uzyskiwać dostęp do zasobów firmy, takich jak wewnętrzne bazy danych i usługi internetowe. Jeśli środowisko ASE wdrożono w sieci wirtualnej mającej połączenie VPN z siecią lokalną, aplikacje w tym środowisku mają dostęp do zasobów lokalnych. Jest tak niezależnie od tego, czy połączenie VPN jest typu [lokacja-lokacja](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site), czy [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Aby uzyskać więcej informacji na temat współdziałania środowisk ASE z sieciami wirtualnymi i sieciami lokalnymi, zobacz [App Service Environment network considerations][ASENetwork] (Zagadnienia dotyczące sieci środowiska App Service Environment).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Application-Service-Environments-v2-Private-PaaS-Environments-in-the-Cloud/player]

## <a name="app-service-environment-v1"></a>Środowisko usługi App Service — wersja 1 ##

Środowisko App Service Environment występuje w dwóch wersjach: ASEv1 i ASEv2. Podane wcześniej informacje dotyczyły wersji 2 — ASEv2. W tej sekcji przedstawiono różnice między środowiskami ASEv1 i ASEv2. 

W środowisku ASEv1 wszystkimi zasobami trzeba zarządzać ręcznie. Obejmuje to frontony, procesy robocze oraz adresy IP używane do obsługi połączeń SSL opartych na protokole IP. Zanim będzie można skalować w poziomie plan usługi App Service, należy najpierw skalować w poziomie pulę procesów roboczych, w której będzie hostowany.

W przypadku środowiska ASEv1 używany jest inny model cenowy niż w przypadku środowiska ASEv2. W przypadku środowiska ASEv1 płacisz za każdy przydzielony procesor vCPU. Obejmuje to procesory vCPU obsługujące frontony i procesy robocze, które nie hostują żadnych obciążeń. W przypadku środowiska ASEv1 domyślny rozmiar w skali maksymalnej środowiska ASE to 55 hostów łącznie. Obejmuje to frontony i procesy robocze. Jedną z zalet środowiska ASEv1 jest to, że można je wdrożyć w klasycznej sieci wirtualnej oraz w sieci wirtualnej usługi Resource Manager. Aby dowiedzieć się więcej na temat środowiska ASEv1, zobacz [App Service Environment v1 introduction][ASEv1Intro] (Wprowadzenie do środowiska App Service Environment w wersji 1).

<!--Links-->
[App Service Environments v2]: https://channel9.msdn.com/Blogs/Azure/Azure-Application-Service-Environments-v2-Private-PaaS-Environments-in-the-Cloud?term=app%20service%20environment
[Isolated offering]: https://channel9.msdn.com/Shows/Azure-Friday/Security-and-Horsepower-with-App-Service-The-New-Isolated-Offering?term=app%20service%20environment
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/waf-overview.md
