---
title: Rozwiązania Oracle na maszynach wirtualnych platformy Azure | Dokumentacja firmy Microsoft
description: Więcej informacji na temat obsługiwanych konfiguracji i ograniczenia dotyczące obrazów maszyn wirtualnych oprogramowania Oracle w systemie Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: jeconnoc
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/23/2019
ms.author: rogirdh
ms.custom: seodec18
ms.openlocfilehash: 9dd7f7d07b34ed3c1076b46c0bf5185d6c8cd31a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074231"
---
# <a name="oracle-vm-images-and-their-deployment-on-microsoft-azure"></a>Obrazy maszyn wirtualnych Oracle i ich wdrażania w systemie Microsoft Azure

W tym artykule omówiono informacje o rozwiązaniach Oracle opartych na obrazach maszyn wirtualnych opublikowanych przez Oracle w witrynie Azure Marketplace. Jeśli interesują Cię takie rozwiązania aplikacji wielu chmur przy użyciu infrastruktury Chmurowej Oracle zobacz [rozwiązania aplikacji Oracle, integracji Microsoft Azure i infrastruktury chmury Oracle](oracle-oci-overview.md).

Aby uzyskać listę dostępnych obrazów, uruchom następujące polecenie:

```azurecli-interactive
az vm image list --publisher oracle -o table --all
```

Począwszy od maja 2019 dostępne są następujące obrazy:

```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Ee      Oracle       12.2.0.1                Oracle:Oracle-Database-Ee:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Ee      Oracle       18.3.0.0                Oracle:Oracle-Database-Ee:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Se      Oracle       12.2.0.1                Oracle:Oracle-Database-Se:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Se      Oracle       18.3.0.0                Oracle:Oracle-Database-Se:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Linux            Oracle       6.10                    Oracle:Oracle-Linux:6.10:6.10.20190506                       6.10.20190506
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20190506                         6.8.20190506
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20190506                         6.9.20190506
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20190506                         7.3.20190506
Oracle-Linux            Oracle       7.4                     Oracle:Oracle-Linux:7.4:7.4.20190506                         7.4.20190506
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20181207                         7.5.20181207
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20190506                         7.5.20190506
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20181207                         7.6.20181207
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20190506                         7.6.20190506
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Te obrazy są traktowane jako "Bring Your Own License" i jako takie tylko opłata wyniesie zasobów obliczeniowych, magazynu i sieci kosztów poniesionych przez uruchomienie maszyny Wirtualnej.  Zakłada się, że są poprawnie licencjonowane oprogramowanie Oracle i mieć bieżącej umowy dotyczącej pomocy technicznej w miejscu z firmą Oracle. Oracle, ma gwarancję funkcji przeniesienia licencji w ze środowiska lokalnego do platformy Azure. Zobacz opublikowanego [Oracle i Microsoft](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html) należy pamiętać, aby uzyskać szczegółowe informacje na temat przenoszenia licencji. 

Osoby także podstawowa swoje rozwiązania na obrazach niestandardowych tworzenie od podstaw na platformie Azure, lub Przekaż obraz niestandardowy z ich w środowisku lokalnym.

## <a name="support-for-jd-edwards"></a>Obsługa rozwiązań JD Edwards
Zgodnie z Uwaga obsługę Oracle [2178595.1 identyfikator dokumentu](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4), JD Edwards EnterpriseOne wer. 9.2 i nowszej są obsługiwane w **oferty w chmurze wszystkie publiczne** który spełnia ich określonych `Minimum Technical Requirements` (MTR).  Należy utworzyć niestandardowe obrazy, spełniające ich specyfikacje MTR zgodności aplikacji systemu operacyjnego i oprogramowania. 

## <a name="oracle-database-vm-images"></a>Obrazy maszyn wirtualnych na bazę danych Oracle
Firma Oracle obsługuje uruchomione wersje Oracle DB 12.1 Standard i Enterprise na platformie Azure w obrazach maszyn wirtualnych na podstawie w systemie Oracle Linux.  Aby uzyskać najlepszą wydajność dla obciążeń produkcyjnych, Oracle dB na platformie Azure należy prawidłowo rozmiar obrazu maszyny Wirtualnej i użycie usługi Managed Disks, która jest wspierana przez usługę Premium Storage. Do instrukcji dotyczących szybko rozpocząć Oracle DB pracę na platformie Azure przy użyciu programu Oracle opublikowanego obrazu maszyny Wirtualnej [spróbuj Przewodnik Szybki Start bazy danych Oracle](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Opcje konfiguracji dołączonych dysków

Dołączonych dysków zależy od usługi Azure Blob storage. Każdy dysk standardowy jest w stanie teoretyczne Maksimum około 500 operacji wejścia/wyjścia na sekundę (IOPS). Oferty dysku premium była preferowana dla różnych obciążeń bazy danych o wysokiej wydajności i można osiągnąć maksymalnie 5000 operacji We/Wy na dysk. Można użyć pojedynczego dysku, jeśli który spełnia Twoje potrzeby związane z wydajnością. Jednak może poprawić skuteczne wydajność operacji We/Wy, jeśli korzystają z wielu dysków dołączonych lub rozkłada ich dane z bazy danych, a następnie użyj Oracle automatyczne Storage Management (ASM). Zobacz [Omówienie automatycznego przechowywania Oracle](https://www.oracle.com/technetwork/database/index-100339.html) dla Oracle ASM określonych informacji. Na przykład jak zainstalować i skonfigurować rozwiązanie Oracle ASM na maszynie Wirtualnej platformy Azure z systemem Linux zobacz [Instalowanie i konfigurowanie programu Oracle automatyczne zarządzanie magazynem](configure-oracle-asm.md) samouczka.

### <a name="shared-storage-configuration-options"></a>Opcje konfiguracji magazynu udostępnionego

Usługa Azure Files NetApp zaprojektowano tak, aby spełnić wymagania podstawowe uruchamiania obciążeń o wysokiej wydajności, takich jak bazy danych w chmurze i zapewnia;
- Natywny platformy Azure udostępnionej usługi magazynu systemu plików NFS dla obciążeń Oracle albo za pomocą natywnego klienta systemu plików NFS maszyny Wirtualnej lub Oracle dNFS
- Skalowanie wydajności warstw, odzwierciedlające żądania na SEKUNDĘ w rzeczywistych warunkach zakresu
- Małe opóźnienia
- Wysoka dostępność, wysokiej niezawodności i możliwości zarządzania na dużą skalę, zwykle wymagane przez obciążeń kluczowych o znaczeniu (np. SAP i Oracle)
- Szybkie i wydajne i odzyskiwanie kopii zapasowych, aby osiągnąć najbardziej agresywne RTO i RPO SLA

Te możliwości są możliwe, ponieważ usługi Azure Files NetApp opiera się na NetApp ONTAP® samych pamięci flash komputerów z systemami w środowisku centrum danych platformy Azure — jako usługa platformy Azure, natywnej. Wynik jest technologii magazynowania idealną bazę danych, która może być aprowizowane i używane, podobnie jak inne opcje usługi Azure storage. Zobacz [dokumentacji usługi Azure Files NetApp](https://docs.microsoft.com/azure/azure-netapp-files/) Aby uzyskać więcej informacji na temat sposobu wdrażania i dostęp do woluminów Azure NetApp plików NFS. Zobacz [Oracle na Azure najlepszym rozwiązaniem jest przewodnik przy użyciu Azure NetApp pliki wdrożenia](https://www.netapp.com/us/media/tr-4780.pdf) dla najlepsze rozwiązania dotyczące pracy z bazą danych Oracle na platformie Azure NetApp plikach.


## <a name="oracle-real-application-cluster-oracle-rac"></a>Klaster rzeczywistej aplikacji Oracle (Oracle RAC)
Oracle RAC zaprojektowano w celu złagodzenia awarii jednego węzła w konfiguracji klastra wielowęzłowego w środowisku lokalnym. Opiera się na dwie technologie w środowisku lokalnym, które nie są w natywnych środowiskach chmury publicznej w hiperskali: rzutowanie wielu sieci, jak i udostępnionego dysku. Jeśli Twoje rozwiązanie bazy danych wymaga RAC Oracle na platformie Azure, potrzebujesz innych oprogramowanie innej firmy do włączenia tych technologii. Aby uzyskać więcej informacji na temat Oracle RAC, zobacz [strony FlashGrid SkyCluster](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="high-availability-and-disaster-recovery-considerations"></a>Zagadnienia wysoką dostępność i odzyskiwanie po awarii
Korzystając z bazami danych Oracle na platformie Azure, ponosisz odpowiedzialność za wdrażanie wysoką dostępność i odzyskiwanie po awarii rozwiązania odzyskiwania w celu uniknięcia żadnych przestojów. 

Wysoka dostępność i odzyskiwanie awaryjne programu Oracle Database Enterprise Edition (bez polegania na Oracle-RAC) może odbyć się na platformie Azure przy użyciu [Data Guard, aktywne Data Guard](https://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html), lub [środowiska Oracle GoldenGate](https://www.oracle.com/technetwork/middleware/goldengate), za pomocą dwóch baz danych na dwóch oddzielnych maszynach wirtualnych. Obie maszyny wirtualne powinny mieć ten sam [sieci wirtualnej](https://azure.microsoft.com/documentation/services/virtual-network/) zapewnienie uzyskaniem dostępu do siebie nawzajem za pośrednictwem prywatnego adresu IP na trwałe.  Ponadto zaleca się umieszczenie maszyn wirtualnych w ten sam zestaw dostępności, aby zezwolić na platformie Azure w celu umieszczenie ich w oddzielnych domenach błędów i uaktualnień. Należy chcesz mieć nadmiarowość geograficzna, skonfigurować dwie bazy danych do replikacji między dwoma różnymi regionami i łączenie dwóch wystąpień z bramą sieci VPN.

Samouczek [implementacji środowiska Oracle Data Guard na platformie Azure](configure-oracle-dataguard.md) przeprowadzi Cię przez procedurę podstawowa konfiguracja na platformie Azure.  

Za pomocą środowiska Oracle Data Guard, wysoką dostępność można osiągnąć przy użyciu podstawowej bazy danych w jednej maszyny wirtualnej pomocniczej bazy danych (w stanie wstrzymania) w innej maszyny wirtualnej, a replikacja jednokierunkowa między nimi. Wynik jest do odczytu kopię bazy danych. Za pomocą środowiska Oracle GoldenGate można skonfigurować dwukierunkowej replikacji między dwiema bazami danych. Aby uzyskać informacje o skonfigurowanie rozwiązania wysokiej dostępności dla bazy danych za pomocą tych narzędzi, zobacz [Active Data Guard](https://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) i [GoldenGate](https://docs.oracle.com/goldengate/1212/gg-winux/index.html) dokumentacji w witrynie internetowej Oracle. Jeśli użytkownik należy odczytu i zapisu dostępu do kopii bazy danych, możesz użyć [Active środowiska Oracle Data Guard](https://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

Samouczek [implementacji środowiska Oracle GoldenGate na platformie Azure](configure-oracle-golden-gate.md) przeprowadzi Cię przez procedurę podstawowa konfiguracja na platformie Azure.

Oprócz architekturę na platformie Azure rozwiązanie o wysokiej dostępności i odzyskiwania po awarii, należy mieć strategii tworzenia kopii zapasowej w celu przywrócenia bazy danych. Samouczek [tworzenia kopii zapasowych i odzyskiwanie bazy danych Oracle](oracle-backup-recovery.md) przeprowadzi Cię przez podstawowe procedury dotyczące tworzenia kopii zapasowej spójnej.

## <a name="oracle-weblogic-server-virtual-machine-images"></a>Do obrazów maszyn wirtualnych oprogramowania Oracle WebLogic Server

* **Klastrowanie jest obsługiwana w wersji Enterprise Edition tylko.** Są licencjonowane do użycia, WebLogic klastrowania tylko w przypadku korzystania z wersji Enterprise programu WebLogic Server. Nie należy używać klastrowania z WebLogic Server Standard Edition.
* **Multiemisji UDP nie jest obsługiwane.** Platforma Azure obsługuje emisji pojedynczej protokołu UDP, ale nie multiemisji lub emisji. WebLogic Server jest możliwe zagwarantowanie, że przy użyciu funkcji emisji pojedynczej protokołu UDP platformy Azure. Aby uzyskać najlepsze wyniki, opierając się na UDP emisji pojedynczej firma Microsoft zaleca, czy WebLogic rozmiar klastra są przechowywane w statycznych lub zachowano z nie więcej niż 10 zarządzanych serwerów.
* **WebLogic Server oczekuje portów publicznych i prywatnych w taki sam dostęp T3 (na przykład, korzystając z Enterprise JavaBeans).** Rozważmy scenariusz obejmujące wiele warstw, w którym działa aplikacji warstwy (EJB) usługi w klastrze WebLogic Server składającą się z co najmniej dwóch maszyn wirtualnych w sieci wirtualnej o nazwie *SLWLS*. Warstwa klienta znajduje się w innej podsieci w tej samej sieci wirtualnej, systemem prosty program Java próby wywołania EJB w warstwie usługi. Ponieważ jest on wymagany do warstwy usługi równoważenia obciążenia, publiczny punkt końcowy o zrównoważonym obciążeniu musi zostać utworzony dla maszyn wirtualnych w klastrze WebLogic Server. Jeśli port prywatny, który określisz różni się od portu publicznego (na przykład 7006:7008), wystąpi błąd, takie jak następujące:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Jest to spowodowane wszelkie dostępu zdalnego T3 WebLogic Server oczekuje, że port modułu równoważenia obciążenia i port serwera zarządzanego WebLogic być takie same. W poprzednim przypadku klient uzyskuje dostęp do portu 7006 (port modułu równoważenia obciążenia) i zarządzany serwer nasłuchuje na 7008 (port prywatny). To ograniczenie ma zastosowanie tylko w przypadku dostępu T3, a nie HTTP.

   Aby uniknąć tego problemu, należy użyć jednej z poniższych rozwiązań:

  * Takie same numery portów prywatnymi i publicznymi na użytek punktów końcowych ze zrównoważonym obciążeniem dedykowane T3 dostępu.
  * Podczas uruchamiania WebLogic Server, należy uwzględnić następujący parametr maszyny JVM:

    ```
    -Dweblogic.rjvm.enableprotocolswitch=true
    ```

Aby uzyskać powiązane informacje, zobacz artykuł bazy wiedzy **860340.1** na <https://support.oracle.com>.

* **Dynamiczne klastra i równoważenia obciążenia ograniczenia.** Załóżmy, że ma używać klaster dynamicznych w WebLogic Server i je ujawnić za pośrednictwem pojedynczego, publiczne równoważenia obciążenia punktu końcowego na platformie Azure. Można to zrobić tak długo, jak używać numeru portu stałą dla każdego z serwerów zarządzanych (nie dynamicznie przypisywany z zakresu) i nie rozpoczynaj więcej serwerów zarządzanych niż liczba maszyn, które służy do śledzenia przez administratora. Oznacza to, że ma nie więcej niż jednego serwera zarządzanego na maszynę wirtualną). Jeśli konfiguracji powoduje większej liczby serwerów WebLogic uruchamiany niż maszyny wirtualne (oznacza to, gdzie wiele wystąpień WebLogic Server udział w tej samej maszyny wirtualnej), a następnie nie jest możliwe dla więcej niż jednego z tych wystąpień serwerów WebLogic Aby powiązać danego numeru portu. Inne osoby na tej maszynie wirtualnej zakończyć się niepowodzeniem.

   Jeśli skonfigurujesz serwera administracyjnego, aby automatycznie przypisać unikatowe numery portu dla swoich serwerów zarządzanych, następnie równoważenia obciążenia nie jest możliwe ponieważ platformy Azure nie obsługuje mapowania z jednego portu publicznego do wielu portów prywatnych, jak będą wymagane dla tego Konfiguracja.
* **Wiele wystąpień WebLogic Server na maszynie wirtualnej.** W zależności od wymagań danego wdrożenia istnieje możliwość uruchomienia wielu wystąpień WebLogic Server na tej samej maszyny wirtualnej, jeśli maszyna wirtualna jest wystarczająco duży. Na przykład na maszynie wirtualnej lub średnim rozmiarze, który zawiera dwa rdzenie, możesz wybrać do uruchomienia do dwóch wystąpień WebLogic Server. Jednak nadal zaleca się unikanie wprowadzające pojedynczych punktów awarii do architektury, który będzie tak, jeśli używasz tylko jednej maszyny wirtualnej, która działa wiele wystąpień WebLogic Server. Przy użyciu co najmniej dwóch maszyn wirtualnych może być lepszym rozwiązaniem, a każda maszyna wirtualna, następnie można uruchomić wiele wystąpień WebLogic Server. Każde wystąpienie WebLogic Server, nadal może być częścią tego samego klastra. Jednak obecnie nie jest możliwe korzystanie z systemu Azure do punktów końcowych równoważenia obciążenia, które są dostępne w takich wdrożeniach WebLogic Server, w ramach tej samej maszyny wirtualnej, ponieważ usługa Azure load balancer wymaga serwerów ze zrównoważonym obciążeniem, będą rozpraszane unikatowy maszyny wirtualne.

## <a name="oracle-jdk-virtual-machine-images"></a>Obrazy maszyn wirtualnych zestawu JDK Oracle
* **Zestaw JDK 6 i 7 najnowszych aktualizacji.** Chociaż zaleca się używanie najnowszych publiczną, obsługiwanych wersję języka Java (obecnie Java 8), Azure również udostępnia zestaw JDK 6 i 7 obrazów. Jest przeznaczone dla starszych aplikacji, które nie są jeszcze gotowe do uaktualnienia na zestaw JDK 8. Podczas aktualizacji do poprzedniego zestawu JDK obrazów może nie być dostępne publicznie, biorąc pod uwagę firmą Microsoft z firmą Oracle, zestaw JDK 6 i 7 obrazów, dostarczone przez platformę Azure mają na celu zawiera najnowszą aktualizację niepublicznych, który jest zazwyczaj oferowany przez oprogramowanie Oracle, aby Wybierz grupy Oracle firmy obsługiwane klientów. Nowe wersje obrazów JDK zostanie udostępniona wraz z upływem czasu za pomocą zaktualizowanych wersji zestaw JDK 6 i 7.

   Zestaw JDK dostępne w zestaw JDK 6 i 7 obrazów maszyn wirtualnych i obrazy pochodzące z nich należy używać tylko w obrębie platformy Azure.
* **64-bit JDK.** Obrazy maszyn wirtualnych serwera Oracle WebLogic Server i obrazy maszyn wirtualnych Oracle JDK udostępnianych przez platformę Azure zawierają 64-bitowe wersje systemów Windows Server, jak i zestaw JDK.

## <a name="next-steps"></a>Kolejne kroki
Masz teraz przegląd bieżącego rozwiązania Oracle, na podstawie obrazów maszyn wirtualnych na platformie Microsoft Azure. Następnym krokiem jest, aby wdrożyć swoją pierwszą bazę danych Oracle na platformie Azure.

> [!div class="nextstepaction"]
> [Tworzenie bazy danych Oracle na platformie Azure](oracle-database-quick-create.md)
