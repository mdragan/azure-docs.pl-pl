---
title: Obliczenia o wysokiej wydajności — Usługa Azure Virtual Machines | Dokumentacja firmy Microsoft
description: Poznaj obliczenia o wysokiej wydajności na platformie Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: bcd121127721a6f76d76f11edf6574165c8e8ddb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66809815"
---
# <a name="optimization-for-linux"></a>Optymalizacja na potrzeby systemu Linux

W tym artykule przedstawiono kilka technik klucza, aby zoptymalizować obraz systemu operacyjnego. Dowiedz się więcej o [Włączanie InfiniBand](enable-infiniband.md) i optymalizacja obrazów systemu operacyjnego.

## <a name="update-lis"></a>Aktualizacja LIS

W przypadku wdrażania przy użyciu niestandardowego obrazu (na przykład, starszy system operacyjny, takie jak CentOS/RHEL 7.4 lub 7.5), należy zaktualizować LIS na maszynie Wirtualnej.

```bash
wget https://aka.ms/lis
tar xzf lis
pushd LISISO
./upgrade.sh
```

## <a name="reclaim-memory"></a>Odzyskiwanie pamięci

Automatyczne odzyskiwanie pamięci w celu uniknięcia zdalny dostęp do pamięci, aby zwiększyć wydajność.

```bash
echo 1 >/proc/sys/vm/zone_reclaim_mode
```

Aby to są zachowywane po ponownym uruchomieniu maszyny Wirtualnej:

```bash
echo "vm.zone_reclaim_mode = 1" >> /etc/sysctl.conf sysctl -p
```

## <a name="disable-firewall-and-selinux"></a>Wyłącz zaporę i SELinux

Wyłącz zaporę i SELinux.

```bash
systemctl stop iptables.service
systemctl disable iptables.service
systemctl mask firewalld
systemctl stop firewalld.service
systemctl disable firewalld.service
iptables -nL
sed -i -e's/SELINUX=enforcing/SELINUX=disabled/g'/etc/selinux/config
```

## <a name="disable-cpupower"></a>Wyłącz cpupower

Wyłącz cpupower.

```bash
service cpupower status
if enabled, disable it:
service cpupower stop
sudo systemctl disable cpupower
```

## <a name="next-steps"></a>Kolejne kroki

* Dowiedz się więcej o [Włączanie InfiniBand](enable-infiniband.md) i optymalizacja obrazów systemu operacyjnego.

* Dowiedz się więcej o [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) na platformie Azure.
