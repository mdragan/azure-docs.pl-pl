---
title: Oracle wdrażania systemu DBMS na maszynach wirtualnych platformy Azure w przypadku obciążeń SAP | Dokumentacja firmy Microsoft
description: Wdrażanie systemu DBMS usługi Azure Virtual Machines oprogramowania Oracle dla obciążenia SAP
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5182b621779cf31f3c7da99674ab24fe6efe702d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60835264"
---
# <a name="azure-virtual-machines-dbms-deployment-for-sap-workload"></a>Wdrażania systemu DBMS na maszynach wirtualnych platformy Azure w przypadku obciążeń SAP

[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1114181]: https://launchpad.support.sap.com/#/notes/1114181
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]: https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]:https://launchpad.support.sap.com/#/notes/1619967
[1750510]:https://launchpad.support.sap.com/#/notes/1750510
[1752266]:https://launchpad.support.sap.com/#/notes/1752266
[1757924]:https://launchpad.support.sap.com/#/notes/1757924
[1757928]:https://launchpad.support.sap.com/#/notes/1757928
[1758182]:https://launchpad.support.sap.com/#/notes/1758182
[1758496]:https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]:https://launchpad.support.sap.com/#/notes/1814258
[1882376]:https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]:https://launchpad.support.sap.com/#/notes/1922555
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]: https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2039619]: https://launchpad.support.sap.com/#/notes/2039619
[2069760]: https://launchpad.support.sap.com/#/notes/2069760
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2171857]: https://launchpad.support.sap.com/#/notes/2171857
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]: https://launchpad.support.sap.com/#/notes/2243692

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md 
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f 
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b 
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d 
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c 
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8
[dbms-guide-managed-disks]:dbms-guide.md#f42c6cb5-d563-484d-9667-b07ae51bce29

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md 
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab 
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e 
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e 
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc 
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d 
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f 
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca 
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b 
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d 
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:https://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

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
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd 
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f 

[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../networking/networking-overview.md
[sap-pam]:https://support.sap.com/pam 
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../windows/disks-types.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:../../../resource-manager-deployment-model.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md 
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md 
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability-linux]:../../linux/manage-availability.md
[virtual-machines-manage-availability-windows]:../../windows/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes-linux]:../../linux/sizes.md
[virtual-machines-sizes-windows]:../../windows/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/resources/templates/sql-server-2014-alwayson-existing-vnet-and-ad/
[virtual-network-deploy-multinic-arm-cli]:../linux/multiple-nics.md
[virtual-network-deploy-multinic-arm-ps]:../windows/multiple-nics.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/template-samples.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/manage-virtual-network.md#create-a-virtual-network
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-network-deploy-multinic-classic-ps.md
[virtual-networks-nsg]:../../../virtual-network/security-overview.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md


W tym dokumencie opisano kilka różnych obszarów, aby wziąć pod uwagę podczas wdrażania bazy danych Oracle w przypadku obciążeń SAP w modelu IaaS platformy Azure. Przed przeczytaniem tego dokumentu, zalecamy przeczytanie [zagadnienia dotyczące wdrażania systemu DBMS na maszynach wirtualnych platformy Azure w przypadku obciążeń SAP](dbms_guide_general.md). Zaleca się również przeczytanie inne przewodniki w [obciążeń SAP w dokumentacji platformy Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started). 

Można znaleźć informacje o wersji programu Oracle i odpowiednie wersje systemów operacyjnych, które są obsługiwane przez uruchamianie oprogramowania SAP w bazie danych Oracle na platformie Azure w Uwaga SAP [2039619].

Ogólne informacje o systemie SAP Business Suite Oracle znajduje się w temacie [SAP w bazie danych Oracle](https://www.sap.com/community/topic/oracle.html).
Oprogramowanie Oracle jest obsługiwana przez oprogramowanie Oracle, aby uruchamiać w systemie Microsoft Azure. Aby uzyskać więcej informacji na temat ogólną pomoc techniczną dla funkcji Windows Hyper-V i platformą Azure, sprawdź [Oracle i często zadawane pytania dotyczące programu Microsoft Azure](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html). 

## <a name="sap-notes-relevant-for-oracle-sap-and-azure"></a>SAP Notes istotne dla Oracle, SAP i platformy Azure 

Poniższe uwagi SAP są związane z SAP na platformie Azure.

| Numer | Stanowisko |
| --- | --- |
| [1928533] |Aplikacje środowiska SAP na platformie Azure: Obsługiwane produkty i typy maszyn wirtualnych platformy Azure |
| [2015553] |SAP na platformie Microsoft Azure: Wymagania wstępne dotyczące obsługi |
| [1999351] |Rozwiązywanie problemów z rozszerzonego monitorowania dla rozwiązania SAP platformy Azure |
| [2178632] |Klucz metryki monitorowania dla rozwiązania SAP w systemie Microsoft Azure |
| [2191498] |SAP w systemie Linux przy użyciu platformy Azure: Rozszerzone monitorowanie |
| [2039619] |Aplikacje środowiska SAP na Microsoft Azure przy użyciu bazy danych programu Oracle: Obsługiwane produkty i wersje |
| [2243692] |Systemu Linux na platformie Microsoft Azure (IaaS) maszyny Wirtualnej: Problemy dotyczące licencji SAP |
| [2069760] |Oracle Linux 7.x SAP instalacji i uaktualniania |
| [1597355] |Zalecenie obszaru wymiany w systemie Linux |
| [2171857] |Oracle Database 12c - Obsługa systemu plików w systemie Linux |
| [1114181] |Bazą danych Oracle 11g - Obsługa systemu plików w systemie Linux |

Dokładne konfiguracje i funkcje, które są obsługiwane przez bazy danych Oracle i SAP na platformie Azure są udokumentowane w artykule Uwaga SAP [#2039619](https://launchpad.support.sap.com/#/notes/2039619).

Windows i Oracle Linux są tylko systemy operacyjne, które są obsługiwane przez bazy danych Oracle i SAP na platformie Azure. Powszechnie używanych dystrybucji systemu SLES i RHEL Linux nie są obsługiwane dla wdrażania składników oprogramowania Oracle na platformie Azure. Składniki Oracle obejmują klienta Oracle Database, która jest używana przez aplikacje SAP nawiązywania połączenia z bazami danych Oracle. 

Wyjątki, zgodnie z Uwaga SAP [#2039619](https://launchpad.support.sap.com/#/notes/2039619), są składnikami SAP, które nie używają klienta Oracle Database. Takie składniki SAP są SAP umieścić autonomicznej, serwer komunikatów, umieścić w kolejce replikacji usługi, WebDispatcher i bramy SAP.  

Nawet wtedy, gdy korzystasz z bazami danych Oracle i wystąpieniom aplikacji SAP w systemie Oracle Linux, można uruchomić usługi centralne SAP w systemie SLES lub RHEL i chronić go za pomocą klastra na podstawie program Pacemaker. Program pacemaker jako platforma o wysokiej dostępności nie jest obsługiwana w systemie Oracle Linux.

## <a name="specifics-for-oracle-database-on-windows"></a>Szczegółowe informacje o bazy danych Oracle na Windows

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms-on-windows"></a>Wskazówki dotyczące konfigurowania oprogramowania Oracle w przypadku instalacji SAP na maszynach wirtualnych Azure na Windows

Zgodnie z Podręcznika instalacji SAP pliki związane z bazy danych Oracle nie zainstalowane lub znajduje się w sterowniku system dla dysku systemu operacyjnego maszyny Wirtualnej (dysk c:). Maszyny wirtualne o różnych rozmiarach, może obsługiwać różne liczby dołączonych dysków. Mniejszych typów maszyn wirtualnych może obsługiwać mniejszej liczby dołączonych dysków. 

W przypadku mniejszych maszyn wirtualnych zaleca się instalowanie/lokalizowanie głównej bazy danych Oracle, etap, "saptrace", "saparch", "sapbackup", "sapcheck" lub "sapreorg" do dysku systemu operacyjnego. Te elementy składniki bazami danych Oracle nie są intensywne operacje We/Wy i operacje We/Wy przepływności. Oznacza to, że dysk systemu operacyjnego może obsłużyć wymagania dotyczące operacji We/Wy. Domyślny rozmiar dysku systemu operacyjnego jest 127 GB. 

Jeśli go nie ma wystarczającej ilości wolnego miejsca dostępna, może być dysk [ze zmienionym rozmiarem](https://docs.microsoft.com/azure/virtual-machines/windows/expand-os-disk) do 2048 GB. Oracle Database i wykonaj ponownie zaloguj się potrzebę pliki mają być przechowywane na dyskach osobne dane. Występuje wyjątek, aby uzyskać tymczasowy obszar tabel Oracle. Tempfiles mogą być tworzone na D: / (dysk-trwałe). Nietrwałe dysku D:\ oferuje również lepszą opóźnienia operacji We/Wy i przepływność (z wyjątkiem maszyny wirtualne z serii A). 

Aby określić odpowiednią ilość miejsca na tempfiles, możesz sprawdzić rozmiary tempfiles na istniejących systemów.

### <a name="storage-configuration"></a>Konfiguracja usługi Storage
Tylko jednego wystąpienia bazy danych Oracle przy użyciu systemu plików NTFS sformatowane dysków jest obsługiwana. Wszystkie pliki bazy danych muszą być przechowywane w systemie plików NTFS na dyskach zarządzanych (zalecane) lub na wirtualnych dyskach twardych. Te dyski są zainstalowane na maszynie Wirtualnej platformy Azure, a także są oparte na [magazynu obiektów blob platformy Azure strony](https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) lub [usługi Azure Managed Disks](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview). 

Zdecydowanie zalecamy używanie [usługi Azure Managed Disks](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview). Ponadto zdecydowanie zalecamy używanie [premium SSD](../../windows/disks-types.md) wdrożeń bazy danych Oracle.

Dyski sieciowe lub udziałach zdalnych, takich jak usługi plików platformy Azure nie są obsługiwane dla plików bazy danych Oracle. Aby uzyskać więcej informacji, zobacz:

- [Introducing Microsoft Azure File Service](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx) (Wprowadzenie do usługi plików platformy Microsoft Azure)

- [Persisting connections to Microsoft Azure Files](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx) (Utrwalanie połączeń z plikami platformy Microsoft Azure)


Jeśli używasz dysków, które są oparte na magazynie obiektów blob platformy Azure strony lub Managed Disks instrukcji w [zagadnienia dotyczące wdrażania systemu DBMS na maszynach wirtualnych platformy Azure w przypadku obciążeń SAP](dbms_guide_general.md) dotyczy także wdrożeń z bazą danych Oracle.

Istnieją limity przydziału przepustowość operacji We/Wy na dyskach platformy Azure. To pojęcie to wyjaśnione w [zagadnienia dotyczące wdrażania systemu DBMS na maszynach wirtualnych platformy Azure w przypadku obciążeń SAP](dbms_guide_general.md). Dokładne przydziały są zależne od typu maszyny Wirtualnej, którego używasz. Lista typów maszyn wirtualnych z ich przydziały znajduje się w temacie [rozmiary dla Windows maszyn wirtualnych na platformie Azure][virtual-machines-sizes-windows].

Aby określić obsługiwane typy maszyn wirtualnych platformy Azure, zobacz uwagę [1928533].

Minimalna konfiguracja jest następująca: 

| Składnik | Dysk | Buforowanie | Pula magazynów |
| --- | ---| --- | --- |
| \oracle\<SID > \origlogaA & mirrlogB | Premium | Brak | Nie jest wymagany |
| \oracle\<SID > \origlogaB & mirrlogA | Premium | Brak | Nie jest wymagany |
| \oracle\<SID>\sapdata1...n | Premium | Tylko do odczytu | Mogą być używane. |
| \oracle\<SID > \oraarch | Standardowa (Standard) | Brak | Nie jest wymagany |
| Strona główna programu Oracle saptrace... | Dysk systemu operacyjnego | | Nie jest wymagany |


Wybór dysków do obsługi dzienników online Powtórz powinien opierać się przez wymagania dotyczące operacji We/Wy. Istnieje możliwość przechowywania wszystkich sapdata1... n (obszary tabel) na jednym jednego zainstalowanego dysku tak długo, jak rozmiar, operacje We/Wy i przepływność spełniają wymagania. 

Konfiguracja wydajności jest następująca:

| Składnik | Dysk | Buforowanie | Pula magazynów |
| --- | ---| --- | --- |
| \oracle\<SID > \origlogaA | Premium | Brak | Mogą być używane.  |
| \oracle\<SID > \origlogaB | Premium | Brak | Mogą być używane. |
| \oracle\<SID > \mirrlogAB | Premium | Brak | Mogą być używane. |
| \oracle\<SID > \mirrlogBA | Premium | Brak | Mogą być używane. |
| \oracle\<SID>\sapdata1...n | Premium | Tylko do odczytu | Zalecane  |
| \oracle\SID\sapdata(n+1)* | Premium | Brak | Mogą być używane. |
| \oracle\<SID > \oraarch* | Premium | Brak | Nie jest wymagany |
| Strona główna programu Oracle saptrace... | Dysk systemu operacyjnego | Nie jest wymagany |

\* (n + 1): hosting systemu, TEMP i cofania obszary tabel. Wzorzec operacji We/Wy systemu i cofania obszary tabel różnią się od innych obszary tabel hostowania danych aplikacji. Brak buforowania jest najlepszym rozwiązaniem dla wydajności systemu i cofania obszary tabel.

\* oraarch: Pula magazynów nie jest konieczne z punktu widzenia wydajności. Może służyć do uzyskiwanie dodatkowego miejsca.

Więcej operacji We/Wy są wymagane, zaleca się tworzenie jednego urządzenia logicznego dużych przez wiele dysków zainstalowanych przy użyciu pul magazynu systemu Windows (tylko dostępne w systemie Windows Server 2012 lub nowszy). To podejście upraszcza administrowanie w czasie Zarządzanie miejscem na dysku i pomaga uniknąć nakład pracy ręcznie dystrybucji plików na wielu dyskach zainstalowanego.


#### <a name="write-accelerator"></a>Akcelerator zapisu
Dla maszyn wirtualnych serii M platformy Azure można zmniejszyć opóźnienie zapisu w dziennikach Powtórz online od czynników, w porównaniu do usługi Azure Premium Storage. Włączyć akcelerator zapisu platformy Azure dla dysków (VHD) na podstawie usługi Azure Premium Storage, które są używane dla plików dziennika ponownego wykonywania w trybie online. Aby uzyskać więcej informacji, zobacz [akceleratorem zapisu](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).


### <a name="backuprestore"></a>/ Przywracania kopii zapasowej
Dla funkcji backup/restore, SAP BR * narzędzia na oprogramowanie Oracle, są obsługiwane w taki sam sposób, ponieważ znajdują się na standardowych systemy operacyjne Windows Server. Menedżer odzyskiwania bazy danych Oracle (RMAN) jest również obsługiwana dla kopii zapasowych na dysku i przywrócenie z dysku.

Umożliwia także usługi Azure Backup do uruchomienia spójnych z aplikacją kopii zapasowych maszyn wirtualnych. Artykuł [Planowanie infrastruktury kopii zapasowej maszyny Wirtualnej na platformie Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction) wyjaśnia, jak usługa Azure Backup używa funkcji Windows VSS do wykonywania kopii zapasowych spójnych z aplikacją. Wersje bazami danych Oracle, które są obsługiwane na platformie Azure przez firmę SAP mogą korzystać z funkcji usługi VSS dla kopii zapasowych. Aby uzyskać więcej informacji, zobacz dokumentację programu Oracle [podstawowe pojęcia związane z kopii zapasowej bazy danych i odzyskiwania z usługami VSS](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/ntqrf/basic-concepts-of-database-backup-and-recovery-with-vss.html#GUID-C085101B-237F-4773-A2BF-1C8FD040C701).


### <a name="high-availability"></a>Wysoka dostępność
Środowiska Oracle Data Guard jest obsługiwana dla wysokiej dostępności i celów odzyskiwania po awarii. Aby osiągnąć automatycznej pracy awaryjnej w Data Guard, trzeba użyć szybkiego rozpoczęcia pracy awaryjnej (FSFA). Obserwator (FSFA) wyzwala przełączenie w tryb failover. Jeśli nie używasz FSFA, można używać tylko konfiguracji ręcznej pracy awaryjnej.

Aby uzyskać więcej informacji na temat odzyskiwania po awarii dla baz danych Oracle na platformie Azure, zobacz [odzyskiwanie po awarii dla bazy danych Oracle database 12c w środowisku platformy Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-disaster-recovery).

### <a name="accelerated-networking"></a>Wydajniejsze sieci
W przypadku wdrożeń oprogramowania Oracle na Windows zalecamy przyspieszonej łączności sieciowej zgodnie z opisem w [Azure accelerated networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/). Należy również rozważyć zaleceń, które zostały wprowadzone w [zagadnienia dotyczące wdrażania systemu DBMS na maszynach wirtualnych platformy Azure w przypadku obciążeń SAP](dbms_guide_general.md). 
### <a name="other"></a>Inne
[Zagadnienia dotyczące wdrażania systemu DBMS na maszynach wirtualnych platformy Azure w przypadku obciążeń SAP](dbms_guide_general.md) opisano inne ważne pojęcia związane z wdrożeniami maszyn wirtualnych przy użyciu bazy danych Oracle, łącznie z zestawami dostępności platformy Azure i monitorowanie SAP.

## <a name="specifics-for-oracle-database-on-oracle-linux"></a>Szczegółowe informacje o bazy danych Oracle w systemie Oracle Linux
Oprogramowanie Oracle jest obsługiwana przez oprogramowanie Oracle, aby uruchomić na Microsoft Azure z systemem Oracle Linux jako system operacyjny gościa. Aby uzyskać więcej informacji na temat ogólną pomoc techniczną dla funkcji Windows Hyper-V i platformą Azure, zobacz [platformy Azure i Oracle — często zadawane pytania](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html). 

Obsługiwane jest również konkretnego scenariusza aplikacji SAP z wykorzystaniem baz danych Oracle. Szczegóły zostały omówione w następnej części tego dokumentu.

### <a name="oracle-version-support"></a>Obsługa wersji programu Oracle
Aby uzyskać informacje o Oracle, które wersje i odpowiednie wersje systemu operacyjnego jest obsługiwane uruchamianie oprogramowania SAP w bazie danych Oracle na maszynach wirtualnych platformy Azure, patrz Uwaga SAP [2039619].

Ogólne informacje o systemie SAP Business Suite Oracle znajdują się w [SAP na stronę społeczności programu Oracle](https://www.sap.com/community/topic/oracle.html).

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms-on-linux"></a>Wskazówki dotyczące konfigurowania oprogramowania Oracle w przypadku instalacji SAP na maszynach wirtualnych Azure w systemie Linux

Zgodnie z instrukcje instalacji SAP pliki związane z bazy danych Oracle nie zainstalowane lub znajduje się w sterowniki dla dysku rozruchowego maszyny Wirtualnej systemu. Zróżnicowanie rozmiarów maszyn wirtualnych obsługują różne liczby dołączonych dysków. Mniejszych typów maszyn wirtualnych może obsługiwać mniejszej liczby dołączonych dysków. 

W takim przypadku firma Microsoft zaleca instalowanie/lokalizowanie głównej bazy danych Oracle, etap, saptrace, saparch, sapbackup, sapcheck lub sapreorg do dysku rozruchowego. Te elementy składniki bazami danych Oracle nie są intensywne operacje We/Wy i operacje We/Wy przepływności. Oznacza to, że dysk systemu operacyjnego może obsłużyć wymagania dotyczące operacji We/Wy. Domyślny rozmiar dysku systemu operacyjnego jest 30 GB. Dysk rozruchowy można rozwinąć przy użyciu witryny Azure portal, programu PowerShell lub interfejsu wiersza polecenia. Po rozwinięciu dysku rozruchowego możesz dodać dodatkowe partycji dla danych binarnych Oracle.


### <a name="storage-configuration"></a>Konfiguracja usługi Storage

Systemy plików ext4, xfs lub Oracle ASM są obsługiwane dla plików bazy danych Oracle na platformie Azure. Wszystkie pliki bazy danych muszą być przechowywane na tych systemów plików, na podstawie wirtualnych dysków twardych lub dysków Managed Disks. Te dyski są zainstalowane na maszynie Wirtualnej platformy Azure, a także są oparte na [magazynu obiektów blob platformy Azure strony](<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) lub [usługi Azure Managed Disks](../../windows/managed-disks-overview.md).

Dla bazy danych Oracle Linux UEK jądra, co najmniej UEK w wersji 4 jest wymagany do obsługi [Azure — wersja premium SSD](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-caching).

Zaleca się używać [usługi Azure managed disks](../../windows/managed-disks-overview.md). Również zdecydowanie zaleca się przy użyciu [Azure — wersja premium SSD](../../windows/disks-types.md) wdrożeń bazy danych Oracle.

Dyski sieciowe lub udziałach zdalnych, takich jak usługi plików platformy Azure nie są obsługiwane dla plików bazy danych Oracle. Aby uzyskać więcej informacji zobacz następujące tematy: 

- [Introducing Microsoft Azure File Service](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx) (Wprowadzenie do usługi plików platformy Microsoft Azure)

- [Persisting connections to Microsoft Azure Files](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx) (Utrwalanie połączeń z plikami platformy Microsoft Azure)

Jeśli używasz dysków strona usługi Azure blob storage lub dyski Managed Disks — na podstawie oświadczeń wprowadzone w [zagadnienia dotyczące wdrażania systemu DBMS na maszynach wirtualnych platformy Azure w przypadku obciążeń SAP](dbms_guide_general.md) dotyczy także wdrożeń z bazą danych Oracle.

 Istnieją limity przydziału przepustowość operacji We/Wy na dyskach platformy Azure. To pojęcie to wyjaśnione w [zagadnienia dotyczące wdrażania systemu DBMS na maszynach wirtualnych platformy Azure w przypadku obciążeń SAP](dbms_guide_general.md). Dokładne przydziały zależą od typu maszyny Wirtualnej, który jest używany. Aby uzyskać listę typów maszyn wirtualnych z ich przydziały zobacz [rozmiary maszyn wirtualnych systemu Linux na platformie Azure][virtual-machines-sizes-linux].

Aby określić obsługiwane typy maszyn wirtualnych platformy Azure, zobacz uwagę [1928533].

Minimalna konfiguracja:

| Składnik | Dysk | Buforowanie | Obcięcie * |
| --- | ---| --- | --- |
| /Oracle/\<SID > / origlogaA & mirrlogB | Premium | Brak | Nie jest wymagany |
| /Oracle/\<SID > / origlogaB & mirrlogA | Premium | Brak | Nie jest wymagany |
| /oracle/\<SID>/sapdata1...n | Premium | Tylko do odczytu | Mogą być używane. |
| /Oracle/\<SID > / oraarch | Standardowa (Standard) | Brak | Nie jest wymagany |
| Strona główna programu Oracle saptrace... | Dysk systemu operacyjnego | | Nie jest wymagany |

\* Obcięcie: LVM stripe lub MDADM przy użyciu RAID0

Wybranego dysku do obsługi dzienników Powtórz online firmy Oracle powinien opierać się przez wymagania dotyczące operacji We/Wy. Istnieje możliwość przechowywania wszystkich sapdata1... n (obszary tabel) na jednym dysku zainstalowanego tak długo, jak wolumin, operacje We/Wy i przepływność spełniają wymagania. 

Konfiguracja wydajności:

| Składnik | Dysk | Buforowanie | Obcięcie * |
| --- | ---| --- | --- |
| /Oracle/\<SID > / origlogaA | Premium | Brak | Mogą być używane.  |
| /Oracle/\<SID > / origlogaB | Premium | Brak | Mogą być używane. |
| /Oracle/\<SID > / mirrlogAB | Premium | Brak | Mogą być używane. |
| /Oracle/\<SID > / mirrlogBA | Premium | Brak | Mogą być używane. |
| /oracle/\<SID>/sapdata1...n | Premium | Tylko do odczytu | Zalecane  |
| /oracle/\<SID>/sapdata(n+1)* | Premium | Brak | Mogą być używane. |
| /Oracle/\<SID > / oraarch * | Premium | Brak | Nie jest wymagany |
| Strona główna programu Oracle saptrace... | Dysk systemu operacyjnego | Nie jest wymagany |

\* Obcięcie: LVM stripe lub MDADM przy użyciu RAID0

\* (n + 1): hosting systemu, TEMP i cofania obszary tabel: Wzorzec operacji We/Wy systemu i cofania obszary tabel różnią się od innych obszary tabel hostowania danych aplikacji. Brak buforowania jest najlepszym rozwiązaniem dla wydajności systemu i cofania obszary tabel.

\* oraarch: Pula magazynów nie jest konieczne z punktu widzenia wydajności.


Jeśli więcej operacji We/Wy są wymagane, zaleca się przy użyciu LVM (Menedżer woluminów logicznych) lub MDADM, aby utworzyć jedną dużą logiczne przez wiele dysków zainstalowanych. Aby uzyskać więcej informacji, zobacz [zagadnienia dotyczące wdrażania systemu DBMS na maszynach wirtualnych platformy Azure w przypadku obciążeń SAP](dbms_guide_general.md) dotyczące wskazówki i wskaźników o tym, jak korzystać z LVM lub MDADM. To podejście ułatwia zmniejszenie administracyjnych kosztów ogólnych zarządzania miejsca na dysku i pomaga uniknąć nakład pracy ręcznie dystrybucji plików na wielu dyskach zainstalowanego.


#### <a name="write-accelerator"></a>Akcelerator zapisu
Maszyn wirtualnych serii M platformy Azure korzystając z akceleratorem zapisu platformy Azure, opóźnienie zapisu w dziennikach Powtórz online można zmniejszyć czynników, w porównaniu do usługi Azure Premium Storage o wydajności. Włączyć akcelerator zapisu platformy Azure dla dysków (VHD) na podstawie usługi Azure Premium Storage, które są używane dla plików dziennika ponownego wykonywania w trybie online. Aby uzyskać więcej informacji, zobacz [akceleratorem zapisu](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).


### <a name="backuprestore"></a>/ Przywracania kopii zapasowej
Dla funkcji backup/restore, SAP BR * narzędzia dla oprogramowania Oracle są obsługiwane w taki sam sposób, jak na komputerach bez systemu operacyjnego i funkcji Hyper-V. Menedżer odzyskiwania bazy danych Oracle (RMAN) jest również obsługiwana dla kopii zapasowych na dysku i przywrócenie z dysku.

Aby uzyskać więcej informacji na temat sposobu korzystania z usług Azure Backup i Recovery wykonywania kopii zapasowych i odzyskiwania baz danych Oracle, zobacz [tworzenie kopii zapasowych i odzyskiwanie bazy danych Oracle database 12c na maszynie wirtualnej z systemem Linux platformy Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-backup-recovery).

### <a name="high-availability"></a>Wysoka dostępność
Środowiska Oracle Data Guard jest obsługiwana dla wysokiej dostępności i celów odzyskiwania po awarii. Aby osiągnąć automatycznej pracy awaryjnej w Data Guard, należy użyć szybkiego rozpoczęcia pracy awaryjnej (FSFA). Funkcje obserwatora (FSFA) wyzwala przełączenie w tryb failover. Jeśli nie używasz FSFA, można używać tylko konfiguracji ręcznej pracy awaryjnej. Aby uzyskać więcej informacji, zobacz [implementacji środowiska Oracle Data Guard na maszynie wirtualnej z systemem Linux platformy Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/configure-oracle-dataguard).


Aspekty odzyskiwania po awarii dla baz danych Oracle na platformie Azure są przedstawione w artykule [odzyskiwanie po awarii dla bazy danych Oracle database 12c w środowisku platformy Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-disaster-recovery).

### <a name="accelerated-networking"></a>Wydajniejsze sieci
Obsługa usługę Azure Accelerated Networking w systemie Oracle Linux jest dostarczany z systemów Oracle Linux 7 Update 5 (Oracle Linux 7.5). Jeśli nie można uaktualnić do najnowszej wersji programu Oracle Linux w wersji 7.5, przy użyciu firmę Red Hat zgodne jądra (RHCK) zamiast jądra Oracle UEK może istnieć obejście tego problemu. 

Jądro systemu RHEL w systemie Oracle Linux jest obsługiwana według Uwaga SAP [#1565179](https://launchpad.support.sap.com/#/notes/1565179). Na usługę Azure Accelerated Networking minimalna wersja jądra RHCKL musi być 3.10.0-862.13.1.el7. W przypadku korzystania w systemie Oracle Linux jądra UEK, w połączeniu z [usługę Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/), należy użyć programu Oracle UEK jądra w wersji 5.

Jeśli wdrażasz maszyn wirtualnych z obrazu, który nie jest oparta na portalu Azure Marketplace, należy skopiować pliki dodatkowej konfiguracji do maszyny Wirtualnej, uruchamiając następujący kod: 
<pre><code># Copy settings from GitHub to the correct place in the VM
sudo curl -so /etc/udev/rules.d/68-azure-sriov-nm-unmanaged.rules https://raw.githubusercontent.com/LIS/lis-next/master/hv-rhel7.x/hv/tools/68-azure-sriov-nm-unmanaged.rules 
</code></pre>


### <a name="other"></a>Inne
[Zagadnienia dotyczące wdrażania systemu DBMS na maszynach wirtualnych platformy Azure w przypadku obciążeń SAP](dbms_guide_general.md) opisano inne ważne pojęcia związane z wdrożeniami maszyn wirtualnych przy użyciu bazy danych Oracle, łącznie z zestawami dostępności platformy Azure i monitorowanie SAP.
