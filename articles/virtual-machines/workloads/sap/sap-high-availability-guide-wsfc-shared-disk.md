---
title: Klaster wystąpienie SAP ASCS/SCS na klastrze pracy awaryjnej Windows przy użyciu udostępnionego dysku klastra na platformie Azure | Dokumentacja firmy Microsoft
description: Dowiedz się, jak klastra wystąpienie SAP ASCS/SCS na klastrze pracy awaryjnej Windows, korzystając z udostępnionym dysku klastra.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: f6fb85f8-c77a-4af1-bde8-1de7e4425d2e
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 987cd1e5860b0fe340ba8a5163d844bec29b541c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65205101"
---
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[ha-guide]:sap-high-availability-guide.md
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:sap-high-availability-guide.md
[sap-ha-guide-2]:#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-4]:#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-8]:#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.9]:#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.3]:#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:#a97ad604-9094-44fe-a364-f89cb39bf097

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (Konfiguracja wysokiej dostępności — wiele identyfikatorów SID SAP)

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:./media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png


[sap-ha-guide-figure-8001]:./media/virtual-machines-shared-sap-high-availability-guide/8001.png
[sap-ha-guide-figure-8002]:./media/virtual-machines-shared-sap-high-availability-guide/8002.png
[sap-ha-guide-figure-8003]:./media/virtual-machines-shared-sap-high-availability-guide/8003.png
[sap-ha-guide-figure-8004]:./media/virtual-machines-shared-sap-high-availability-guide/8004.png
[sap-ha-guide-figure-8005]:./media/virtual-machines-shared-sap-high-availability-guide/8005.png
[sap-ha-guide-figure-8006]:./media/virtual-machines-shared-sap-high-availability-guide/8006.png
[sap-ha-guide-figure-8007]:./media/virtual-machines-shared-sap-high-availability-guide/8007.png
[sap-ha-guide-figure-8008]:./media/virtual-machines-shared-sap-high-availability-guide/8008.png
[sap-ha-guide-figure-8009]:./media/virtual-machines-shared-sap-high-availability-guide/8009.png
[sap-ha-guide-figure-8010]:./media/virtual-machines-shared-sap-high-availability-guide/8010.png
[sap-ha-guide-figure-8011]:./media/virtual-machines-shared-sap-high-availability-guide/8011.png
[sap-ha-guide-figure-8012]:./media/virtual-machines-shared-sap-high-availability-guide/8012.png
[sap-ha-guide-figure-8013]:./media/virtual-machines-shared-sap-high-availability-guide/8013.png
[sap-ha-guide-figure-8014]:./media/virtual-machines-shared-sap-high-availability-guide/8014.png
[sap-ha-guide-figure-8015]:./media/virtual-machines-shared-sap-high-availability-guide/8015.png
[sap-ha-guide-figure-8016]:./media/virtual-machines-shared-sap-high-availability-guide/8016.png
[sap-ha-guide-figure-8017]:./media/virtual-machines-shared-sap-high-availability-guide/8017.png
[sap-ha-guide-figure-8018]:./media/virtual-machines-shared-sap-high-availability-guide/8018.png
[sap-ha-guide-figure-8019]:./media/virtual-machines-shared-sap-high-availability-guide/8019.png
[sap-ha-guide-figure-8020]:./media/virtual-machines-shared-sap-high-availability-guide/8020.png
[sap-ha-guide-figure-8021]:./media/virtual-machines-shared-sap-high-availability-guide/8021.png
[sap-ha-guide-figure-8022]:./media/virtual-machines-shared-sap-high-availability-guide/8022.png
[sap-ha-guide-figure-8023]:./media/virtual-machines-shared-sap-high-availability-guide/8023.png
[sap-ha-guide-figure-8024]:./media/virtual-machines-shared-sap-high-availability-guide/8024.png
[sap-ha-guide-figure-8025]:./media/virtual-machines-shared-sap-high-availability-guide/8025.png


[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md

# <a name="cluster-an-sap-ascsscs-instance-on-a-windows-failover-cluster-by-using-a-cluster-shared-disk-in-azure"></a>Klaster wystąpienie SAP ASCS/SCS na klastrze pracy awaryjnej Windows przy użyciu udostępnionego dysku klastra na platformie Azure

> ![Windows][Logo_Windows] Windows
>

Klaster trybu failover systemu Windows Server jest podstawą SAP ASCS/SCS instalacji o wysokiej dostępności i systemu DBMS na platformie Windows.

Klaster trybu failover to grupa 1 + n niezależnych serwerów (węzłów), które współpracują ze sobą, aby zwiększyć dostępność aplikacji i usług. W przypadku awarii węzła klastra trybu failover systemu Windows Server oblicza liczbę błędów, które mogą wystąpić i nadal utrzymuje dobrej kondycji klastra, aby zapewnić aplikacji i usług. Istnieje możliwość z trybów kworum różnych osiągnąć klastra trybu failover.

## <a name="prerequisites"></a>Wymagania wstępne
Przed przystąpieniem do wykonywania zadań w tym artykule, przejrzyj następujący artykuł:

* [Architektura wysokiej dostępności na platformie Azure maszyn wirtualnych i scenariusze SAP NetWeaver][sap-high-availability-architecture-scenarios]


## <a name="windows-server-failover-clustering-in-azure"></a>Windows Server klaster trybu failover na platformie Azure

W porównaniu do wdrożenia bez systemu operacyjnego lub prywatnej chmury, maszyn wirtualnych platformy Azure wymaga dodatkowych kroków, aby skonfigurować klaster trybu failover systemu Windows Server. Podczas tworzenia klastra, musisz ustawić kilka adresów IP i nazwy hostów wirtualnych na potrzeby wystąpienia SAP ASCS/SCS.

### <a name="name-resolution-in-azure-and-the-cluster-virtual-host-name"></a>Rozpoznawanie nazw na platformie Azure i nazwę hosta wirtualnego klastra

Platforma usług w chmurze platformy Azure nie oferuje opcję, aby skonfigurować wirtualne adresy IP, takie jak adresy IP zmiennoprzecinkowy. Należy skonfigurować wirtualny adres IP, aby dotrzeć do zasobu klastra w chmurze alternatywnym rozwiązaniem. 

Usługa Azure Load Balancer zapewnia *wewnętrznego modułu równoważenia obciążenia* dla platformy Azure. Przy użyciu wewnętrznego modułu równoważenia obciążenia klienci dotrzeć do klastra za pośrednictwem wirtualnego adresu IP klastra. 

Wdrażanie wewnętrznego modułu równoważenia obciążenia w grupie zasobów, która zawiera węzły klastra. Następnie należy skonfigurować wszystkie niezbędne portu reguły przekazywania za pomocą sondy porty wewnętrznego modułu równoważenia obciążenia. Klienci mogą łączyć się za pomocą nazwy hosta wirtualnego. Serwer DNS jest rozpoznawana jako adres IP klastra i port uchwyty modułu równoważenia obciążenia wewnętrznego, przekazywania do aktywnego węzła klastra.

![Rysunek 1: Windows awaryjnej konfiguracji na platformie Azure bez udostępnionego dysku][sap-ha-guide-figure-1001]

_**Rysunek 1:** Konfiguracja na platformie Azure bez udostępnionego dysku awaryjnej w systemie Windows Server_

### <a name="sap-ascsscs-ha-with-cluster-shared-disks"></a>SAP ASCS/SCS wysokiej dostępności z dyskami udostępnionego klastra
W Windows wystąpienie SAP ASCS/SCS zawiera usług SAP central services, serwer komunikatów SAP, procesów serwera umieścić w kolejce i pliki globalne hosta SAP. Pliki globalne hosta SAP przechowywać centralnej pliki dla całego systemu SAP.

Wystąpienie SAP ASCS/SCS zawiera następujące składniki:

* Usług SAP central services:
    * Dwa procesy, wiadomości i umieścić serwer i < ASCS/SCS wirtualny host name >, który umożliwia dostęp do tych dwóch procesów.
    * Struktura plików: S:\usr\sap\\&lt;SID&gt;\ASCS/SCS\<numer wystąpienia\>


* Pliki globalne hosta SAP:
  * Struktura plików: S:\usr\sap\\&lt;SID&gt;\SYS\...
  * Sapmnt udział plików, która umożliwia dostęp do tych globalnego S:\usr\sap\\&lt;SID&gt;\SYS\... plików przy użyciu następującej ścieżki UNC:

    \\\\< nazwa hosta wirtualnego ASCS/SCS > \sapmnt\\&lt;SID&gt;\SYS\....


![Rysunek 2: Procesy, struktura plików i udział plików sapmnt hosta globalnego wystąpienia SAP ASCS/SCS][sap-ha-guide-figure-8001]

_**Rysunek 2:** Procesy, struktura plików i udział plików sapmnt hosta globalnego wystąpienia SAP ASCS/SCS_

W ustawieniach wysokiej dostępności klastra są wystąpienia SAP ASCS/SCS. Używamy *udostępnione dyski klastrowane* (dysk S, w tym przykładzie), aby umieścić SAP ASCS/SCS i SAP globalne obsługi plików.

![Rysunek 3: Architektura SAP ASCS/SCS wysokiej dostępności z udostępnionym dyskiem][sap-ha-guide-figure-8002]

_**Rysunek 3:** Architektura SAP ASCS/SCS wysokiej dostępności z udostępnionym dyskiem_

> [!IMPORTANT]
> Uruchom te dwa składniki w ramach tego samego wystąpienia SAP ASCS/SCS:
>* < ASCS/SCS wirtualny host na nazwie > umożliwia dostęp do procesów serwera komunikat i umieścić w kolejce SAP i pliki globalne hosta SAP za pośrednictwem udziału plików sapmnt.
>* Tym samym klastrze udostępniony dysk, który S jest współużytkowana przez je.
>


![Rysunek 4: Architektura SAP ASCS/SCS wysokiej dostępności z udostępnionym dyskiem][sap-ha-guide-figure-8003]

_**Rysunek 4:** Architektura SAP ASCS/SCS wysokiej dostępności z udostępnionym dyskiem_

### <a name="shared-disks-in-azure-with-sios-datakeeper"></a>Udostępnione dyski na platformie Azure oprogramowanie SIOS DataKeeper

Potrzebujesz klastra magazynu udostępnionego dla wystąpienia SAP ASCS/SCS wysokiej dostępności.

Za pomocą oprogramowania innych firm oprogramowanie SIOS DataKeeper Cluster Edition utworzyć dublowany magazynu, która symuluje sieć magazynu udostępnionego klastra. Rozwiązanie oprogramowanie SIOS udostępnia w czasie rzeczywistym synchronicznej replikacji danych.

Aby utworzyć zasób udostępnionego dysku dla klastra:

1. Dodatkowy dysk należy dołączyć do każdej maszyny wirtualnej w konfiguracji klastra Windows.
2. Uruchom oprogramowanie SIOS DataKeeper Cluster Edition na obu węzłach maszyny wirtualnej.
3. Oprogramowanie SIOS DataKeeper Cluster Edition należy skonfigurować tak, aby go odzwierciedla zawartość woluminu dodatkowy dysk dołączony ze źródłowej maszyny wirtualnej do ilości dodatkowy dysk dołączony docelowej maszyny wirtualnej. Oprogramowanie SIOS DataKeeper przenosi źródłowe i docelowe woluminy lokalne, a następnie wyświetla je do systemu Windows Server klaster pracy awaryjnej jako jeden dysk udostępniony.

Uzyskaj więcej informacji o [oprogramowanie SIOS DataKeeper](https://us.sios.com/products/datakeeper-cluster/).

![Rysunek 5: Konfiguracja na platformie Azure przy użyciu oprogramowanie SIOS DataKeeper awaryjnej w systemie Windows Server][sap-ha-guide-figure-1002]

_**Rysunek 5:** Konfiguracja na platformie Azure przy użyciu oprogramowanie SIOS DataKeeper awaryjnej Windows_

> [!NOTE]
> Udostępnione dyski nie ma potrzeby wysokiej dostępności przy użyciu niektórych produktów systemu DBMS, takich jak program SQL Server. AlwaysOn programu SQL Server są replikowane DBMS plików danych i dziennika z dysku lokalnego o jednym węźle klastra na lokalny dysk w innym węźle klastra. W tym przypadku konfiguracji klastra Windows nie wymaga udostępnionego dysku.
>

## <a name="next-steps"></a>Kolejne kroki

* [Przygotowanie infrastruktury platformy Azure do SAP wysokiej dostępności przy użyciu klastra pracy awaryjnej Windows i udostępnionego dysku dla wystąpienia usługi SAP ASCS/SCS][sap-high-availability-infrastructure-wsfc-shared-disk]

* [Zainstaluj oprogramowanie SAP NetWeaver zaświadczanie o kondycji klastra pracy awaryjnej Windows i udostępnionym dyskiem na potrzeby wystąpienia SAP ASCS/SCS][sap-high-availability-installation-wsfc-shared-disk]
