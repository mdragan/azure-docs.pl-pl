---
title: Korzystanie z ponownego uruchamiania maszyny Wirtualnej infrastruktury platformy Azure do osiągnięcia "wyższej dostępności" systemu SAP | Dokumentacja firmy Microsoft
description: Korzystanie z ponownego uruchamiania maszyny Wirtualnej infrastruktury platformy Azure do osiągnięcia "wyższej dostępności" aplikacje SAP
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: f0b2f8f0-e798-4176-8217-017afe147917
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cda0b1c0774ed33bf550e0edf329cc22a2807be3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60825665"
---
# <a name="utilize-azure-infrastructure-vm-restart-to-achieve-higher-availability-of-an-sap-system"></a>Korzystanie z ponownego uruchamiania maszyny Wirtualnej infrastruktury platformy Azure do osiągnięcia "wyższej dostępności" systemu SAP

[1909114]: https://launchpad.support.sap.com/#/notes/1909114
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
[sap-higher-availability]:sap-higher-availability-architecture-scenarios.md
[sap-high-availability-architecture-scenarios-sap-app-ha]:sap-high-availability-architecture-scenarios.md#baed0eb3-c662-4405-b114-24c10a62954e

[planning-guide]:planning-guide.md  
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92

[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/planning-monitoring-overview-2502.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-2901]:media/virtual-machines-shared-sap-planning-guide/2901-azure-ha-sap-ha-md.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-3201]:media/virtual-machines-shared-sap-planning-guide/3201-sap-ha-with-sql-md.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

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

[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md

> W tej sekcji mają zastosowanie do:
>
> ![Windows][Logo_Windows] Windows i ![Linux][Logo_Linux] Linux
>

Jeśli zdecydujesz się nie należy używać funkcji, takich jak Windows Server Failover Clustering (WSFC) lub program Pacemaker w systemie Linux (obecnie obsługiwane tylko w przypadku systemu SUSE Linux Enterprise Server [SLES] 12 i nowsze), ponowne uruchomienie maszyny Wirtualnej platformy Azure jest używana. Chroni systemów SAP przed planowanych lub nieplanowanych przestojów infrastruktury platformy Azure serwer fizyczny i ogólną podstawowej platformy Azure.

> [!NOTE]
> Ponownego uruchamiania maszyny Wirtualnej platformy Azure przede wszystkim chroni maszyny wirtualne i *nie* aplikacji. Mimo że ponownego uruchamiania maszyny Wirtualnej nie oferują wysoką dostępność aplikacji SAP, oferuje pewien poziom dostępność infrastruktury. Oferuje ona pośrednio "wyższą dostępność" systemów SAP. Czas, jaki zajmuje ponowne uruchomienie maszyny Wirtualnej po awarii hosta planowane lub nieplanowane, co sprawia, że ta metoda wysokiej dostępności jest nieodpowiedni dla krytycznych składników systemu SAP również jest objęta umową SLA. Przykłady krytycznych składników może być wystąpienie ASCS/SCS lub system zarządzania bazy danych (DBMS).
>
>

Inny element ważne infrastruktury wysokiej dostępności jest magazyn. Na przykład umowę SLA magazynu platformy Azure jest przez 99,9% czasu. W przypadku wdrożenia wszystkich maszyn wirtualnych i ich dysków na koncie jednego magazynu platformy Azure, potencjalne niedostępność usługi Azure Storage spowoduje niedostępność wszystkich maszyn wirtualnych, które są umieszczane w ramach tego konta magazynu i wszystkie składniki SAP działające wewnątrz maszyn wirtualnych.  

Zamiast umieszczenie wszystkich maszyn wirtualnych pod uwagę pojedynczy magazyn platformy Azure, możesz użyć kont dedykowanych dla magazynu dla każdej maszyny Wirtualnej. Za pomocą wielu kont niezależnie od magazynu platformy Azure, można zwiększyć ogólną dostępność aplikacji maszyny Wirtualnej i SAP.

Usługi Azure managed disks są automatycznie umieszczane w domenie błędów maszyny wirtualnej, które są dołączone. Jeśli umieszczenia dwóch maszyn wirtualnych w zestawie dostępności, użyj usługi managed disks platformy zajmuje się dystrybucja dysków zarządzanych w różnych domenach błędów także. Jeśli zamierzasz używać konta usługi premium storage, zdecydowanie zalecamy używanie dysków zarządzanych.

Przykładowa architektura systemu SAP NetWeaver, który korzysta z infrastruktury platformy Azure o wysokiej dostępności kont usług i storage może wyglądać następująco:

![Korzystanie z wysoką dostępność infrastruktury platformy Azure do osiągnięcia SAP aplikacji "wyższej dostępności"][planning-guide-figure-2900]

Przykładowa architektura systemu SAP NetWeaver, który korzysta z infrastruktury platformy Azure o wysokiej dostępności i dyski zarządzane, może wyglądać następująco:

![Korzystanie z wysoką dostępność infrastruktury platformy Azure do osiągnięcia SAP aplikacji "wyższej dostępności"][planning-guide-figure-2901]

Dla krytycznych składników SAP otrzymano poniżej do tej pory:

* Wysoką dostępność serwerów aplikacji SAP

    Wystąpień serwera aplikacji SAP są składnikom nadmiarowym. Każde wystąpienie serwera aplikacji SAP jest wdrażana na własnej maszynie Wirtualnej działa w różnych domenach błędów platformy Azure i domeny uaktualnień. Aby uzyskać więcej informacji, zobacz [domeny błędów] [ planning-guide-3.2.1] i [uaktualnień] [ planning-guide-3.2.2] sekcje. 

    Ta konfiguracja może upewnij się, korzystając z zestawami dostępności platformy Azure. Aby uzyskać więcej informacji, zobacz [zestawami dostępności platformy Azure] [ planning-guide-3.2.3] sekcji. 

    Potencjalne planowane lub nieplanowane niedostępność błędów platformy Azure lub domena uaktualnienia spowoduje, że niedostępność ograniczonej liczby maszyn wirtualnych z ich wystąpień serwera aplikacji SAP.

    Każde wystąpienie serwera aplikacji SAP jest umieszczany w swoje własne konta magazynu platformy Azure. Potencjalne niedostępności jedno konto magazynu platformy Azure spowoduje niedostępność tylko jednej maszyny Wirtualnej z wystąpieniem serwera aplikacji SAP. Należy jednak pamiętać, że istnieje ograniczenie liczby kont usługi Azure storage w ramach jednej subskrypcji platformy Azure. Aby zapewnić automatyczne uruchamianie wystąpienia ASCS/SCS po ponownym uruchomieniu maszyny Wirtualnej, należy ustawić parametr automatyczne uruchamianie w profilu uruchamiania wystąpienia ASCS/SCS, który został opisany w [przy użyciu funkcji Autostart dla wystąpień SAP] [ planning-guide-11.5] sekcja.
  
    Aby uzyskać więcej informacji, zobacz [wysokiej dostępności dla serwerów aplikacji SAP][planning-guide-11.4.1].

    Nawet wtedy, gdy korzystają z dysków zarządzanych dysków są przechowywane na koncie magazynu platformy Azure i mogą być niedostępne w przypadku awarii magazynu.

* *Wyższą dostępność* SAP ASCS/SCS wystąpień

    W tym scenariuszu wykorzystywać ponownego uruchomienia maszyny Wirtualnej platformy Azure, aby chronić maszynę Wirtualną za pomocą zainstalowanego wystąpienia SAP ASCS/SCS. W przypadku planowanych lub nieplanowanych przestojów serwerami platformy Azure maszyny wirtualne zostaną uruchomione ponownie na innym serwerze dostępne. Jak wspomniano wcześniej, ponowne uruchomienie maszyny Wirtualnej platformy Azure przede wszystkim chroni maszyny wirtualne i *nie* aplikacji, w tym przypadku wystąpienia ASCS/SCS. Za pomocą ponownego uruchamiania maszyny Wirtualnej pośrednio osiągnąć "wyższa dostępność" na wystąpienie SAP ASCS/SCS. 

    Aby zapewnić automatyczne uruchamianie wystąpienia ASCS/SCS po ponownym uruchomieniu maszyny Wirtualnej, ustaw dla parametru automatyczne uruchamianie w profilu uruchamiania wystąpienia ASCS/SCS, zgodnie z opisem w [przy użyciu funkcji Autostart dla wystąpień SAP] [ planning-guide-11.5] sekcji . To ustawienie oznacza, że wystąpienie ASCS/SCS jako pojedynczy punkt awarii (SPOF), uruchomiony w jednej maszynie Wirtualnej określi dostępności całe środowisko SAP.

* *Wyższą dostępność* serwera systemu DBMS

    Tak jak w poprzednim wystąpienia SAP ASCS/SCS przypadek użycia, korzystanie z ponownego uruchomienia maszyny Wirtualnej platformy Azure w celu ochrony tej maszyny Wirtualnej z zainstalowanym oprogramowaniem DBMS i osiągnięcia "wyższą dostępność" DBMS oprogramowania za pośrednictwem ponownego uruchamiania maszyny Wirtualnej.
  
    System DBMS, działający w pojedynczej maszyny Wirtualnej jest również SPOF i jest dominującego współczynnik dostępność całe środowisko SAP.

## <a name="using-autostart-for-sap-instances"></a>Za pomocą funkcji Autostart dla wystąpieniami platformy SAP
Oprogramowanie SAP udostępnia ustawienia, która umożliwia uruchamianie wystąpieniami platformy SAP natychmiast po rozpoczęciu systemu operacyjnego na maszynie wirtualnej. Instrukcje są udokumentowane w artykule bazy wiedzy SAP [1909114]. Jednak SAP nie są już zalecane jest stosowanie ustawienia, ponieważ nie zezwala na formant kolejności ponowne uruchamianie wystąpienia, jeśli występuje więcej niż jednej maszyny Wirtualnej lub wielu wystąpień są uruchomione na maszynie Wirtualnej. 

Zakładając, że typowy scenariusz Azure jednego wystąpienia serwera aplikacji SAP w Maszynę wirtualną i pojedynczą maszynę Wirtualną po pewnym czasie ponownie uruchomić pobieranie, Autostart nie jest krytyczny. Jednak możesz je włączyć, dodając następujący parametr do profilu uruchamiania wystąpienia SAP Advanced Business Application programowania (ABAP) lub Java:

      Autostart = 1


  > [!NOTE]
  > Parametr Autostart ma również niektórych wad. W szczególności parametru wyzwalane początku wystąpienia SAP ABAP i Java, po uruchomieniu usługa Windows lub Linux pokrewna wystąpienia. Czy sekwencji występuje, gdy jest uruchamiany system operacyjny. Jednak ponowne uruchomienie usługi SAP są również wystąpieniem typowe dla funkcji zarządzania cyklem życia oprogramowania SAP, takie jak Menedżer aktualizacji oprogramowania (SUM) lub innych aktualizacji lub uaktualnienia. Te funkcje nie są oczekiwane wystąpienie, które automatycznie uruchomiony ponownie. W związku z tym parametr Autostart powinna być wyłączona, przed uruchomieniem zadania, takie. Parametr Autostart powinna nie również dla wystąpień SAP, które są klastrowane, takich jak ASCS/SCS/ciągłej integracji.
  >
  >

  Aby uzyskać więcej informacji o Autostart wystąpieniami platformy SAP zobacz następujące artykuły:

  * [Uruchamianie lub zatrzymywanie SAP oraz usługi Unix serwera uruchomień/zatrzymań](https://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
  * [Uruchamianie i zatrzymywanie agentów zarządzania środowiska SAP NetWeaver](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
  * [Jak włączyć automatyczne uruchamianie bazy danych HANA](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać informacji na temat pełnego środowiska SAP NetWeaver wykrywaniem aplikacji wysokiej dostępności, zobacz [wysoka dostępność aplikacji SAP na platformie IaaS Azure][sap-high-availability-architecture-scenarios-sap-app-ha].
