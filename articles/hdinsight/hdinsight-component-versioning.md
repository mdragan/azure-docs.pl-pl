---
title: Apache Hadoop, składniki i wersje — Azure HDInsight
description: Poznaj składniki platformy Apache Hadoop i wersje w HDInsight i poziomów usług dostępnych w tej dystrybucji Hortonworks Data Platform w chmurze.
keywords: wersji usługi hadoop, składniki ekosystemu platformy hadoop, składniki platformy hadoop, jak sprawdzić wersji usługi hadoop
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: overview
ms.date: 06/07/2019
ms.openlocfilehash: b92acaa7e8403dfa28c1182fb1dc008d066624c6
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67309969"
---
# <a name="what-are-the-apache-hadoop-components-and-versions-available-with-hdinsight"></a>Jakie są składniki platformy Apache Hadoop i wersje dostępne z HDInsight?

Dowiedz się więcej o [Apache Hadoop](https://hadoop.apache.org/) ekosystem, składników i wersji w programie Microsoft Azure HDInsight, a także pakiet Enterprise Security. Poznaj także sposób sprawdzić wersje składników usługi Hadoop w HDInsight.

## <a name="apache-hadoop-components-available-with-different-hdinsight-versions"></a>Składniki platformy Apache Hadoop dostępne z różnymi wersjami HDInsight

Usługa Azure HDInsight obsługuje wielu wersjach klastrów Hadoop, które można wdrożyć w dowolnym momencie. Każdy wybór wersji tworzy określoną wersję dystrybucji HDP i zestaw składników, które są zawarte w tej dystrybucji. 4 kwietnia 2017 r domyślną wersję klastra używane przez usługi Azure HDInsight jest 3.6 i opiera się na HDP 2.6.

Wersje składników skojarzone z wersji klastra HDInsight są wymienione w poniższej tabeli: 

> [!NOTE]  
> Wersja domyślna dla usługi HDInsight mogą ulec zmianie bez powiadomienia. Jeśli masz zależność wersji, wersji HDInsight można określić podczas tworzenia klastrów za pomocą zestawu SDK platformy .NET przy użyciu programu Azure PowerShell i klasycznego wiersza polecenia platformy Azure.

| Składnik | HDInsight 4.0 | HDInsight 3.6 (ustawienie domyślne) | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | HDInsight 3.2 |
|---------------------------|---------------|-----------------------------|---------------|---------------|---------------|----------------------|
| Hortonworks Data Platform | 3.0 | 2.6 | 2.5 | 2.4 | 2.3 | 2.2 |
| Apache Hadoop i YARN | 3.1.1 | 2.7.3 | 2.7.3 | 2.7.1 | 2.7.1 | 2.6.0 |
| Apache Tez | 0.9.1 | 0.7.0 | 0.7.0 | 0.7.0 | 0.7.0 | 0.5.2 |
| Apache Pig | 0.16.0 | 0.16.0 | 0.16.0 | 0.15.0 | 0.15.0 | 0.14.0 |
| Apache Hive i HCatalog | - | 1.2.1 | 1.2.1 | 1.2.1 | 1.2.1 | 0.14.0 |
| Apache Hive | 3.1.0 | 2.1.0 | - | - | - | - |
| Apache Tez Hive2 | - | 0.8.4 | - | - | - | - |
| Apache Ranger | 1.1.0 | 0.7.0 | 0.6.0 | - | - | - |
| Apache HBase | 2.0.1 | 1.1.2 | 1.1.2 | 1.1.2 | 1.1.1 | 0.98.4 |
| Apache Sqoop | 1.4.7 | 1.4.6 | 1.4.6 | 1.4.6 | 1.4.6 | 1.4.5 |
| Apache Oozie | 4.3.1 | 4.2.0 | 4.2.0 | 4.2.0 | 4.2.0 | 4.1.0 |
| Apache Zookeeper | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 |
| Apache Storm | - | 1.1.0 | 1.0.1 | 0.10.0 | 0.10.0 | 0.9.3 |
| Apache Mahout | - | 0.9.0+ | 0.9.0+ | 0.9.0+ | 0.9.0+ | 0.9.0 |
| Apache Phoenix | 5 | 4.7.0 | 4.7.0 | 4.4.0 | 4.4.0 | 4.2.0 |
| Apache Spark | 2.3.1, 2.4 | 2.3.0, 2.2.0, 2.1.0 | 1.6.2, 2.0 | 1.6.0 | 1.5.2 | 1.3.1 (tylko Windows) |
| Apache Livy | 0,5 | 0.4, 0.4, 0.3 | 0.3 | 0.3 | 0.2 | - |
| Apache Kafka | 1.1.1, 2.1 | 1.1, 1.0 * (zobacz uwaga poniżej) | 0.10.0 | 0.9.0 | - | - |
| Apache Ambari | 2.7.0 | 2.6.0 | 2.4.0 | 2.2.1 | 2.1.0 | - |
| Apache Zeppelin | 0.8.0 | 0.7.0 | - | - | - | - |
| Narzędzie mono | 4.2.1 | 4.2.1 | 4.2.1 | 3.2.8 | - | - |

> [!NOTE]
> Ze względu na zagadnienia związane z wydajnością systemu pomocy technicznej dla platformy Kafka w wersji 0.10 wygasła w usłudze marca 2019 r.

## <a name="check-for-current-hadoop-component-version-information"></a>Sprawdź, czy bieżące informacje o wersji składników usługi Hadoop

Wersje składników ekosystemu Hadoop skojarzone z wersji klastra HDInsight można zmienić za pomocą aktualizacji HDInsight. Aby sprawdzić składniki usługi Hadoop i sprawdź, które wersje są używane dla klastra, należy użyć interfejsu API REST Ambari. **GetComponentInformation** polecenie umożliwia pobranie informacji o składnikach usługi. Aby uzyskać więcej informacji, zobacz [dokumentację Apache Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

### <a name="release-notes"></a>Informacje o wersji

Zobacz [HDInsight wersji](hdinsight-release-notes.md) Aby uzyskać dodatkowe informacje o wersji w najnowszych wersjach HDInsight.

## <a name="supported-hdinsight-versions"></a>Obsługiwane wersje HDInsight

W poniższej tabeli wymieniono wersje HDInsight. Wersje HDP, które odpowiadają każdej wersji HDInsight są wyświetlane wraz z daty wydania produktu. Daty wygaśnięcia i wycofanie pomocy technicznej również są dostarczane, gdy są one znane.

### <a name="available-versions"></a>Dostępne wersje

W poniższej tabeli wymieniono wersje HDInsight, które są dostępne w witrynie Azure portal, jak również innych metod wdrażania, takich jak program PowerShell i zestawu SDK platformy .NET.

| HDInsight w wersji | Wersja HDP | VM OS | Data wydania | Data wygaśnięcia pomocy technicznej | Data wygaśnięcia | Wysoka dostępność |  Dostępność w witrynie Azure portal | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 |HDP 3.0 |Ubuntu 16.0.4 LTS |24 września 2018 r. | | |Tak |Tak |
| HDInsight 3.6 |HDP 2.6 |Ubuntu 16.0.4 LTS |4 kwietnia 2017 r. | 30 czerwca 2020 r. |Do 31 grudnia 2020 r. |Tak |Tak |


> [!NOTE]  
> Po pomocy technicznej dla wersji wygasła, może nie być dostępne za pośrednictwem portalu Microsoft Azure. Natomiast wersjach klastra nadal dostępne za pośrednictwem `Version` parametru w programie Windows PowerShell [New AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) polecenia i zestawu .NET SDK do wersji dacie wycofania.
>

### <a name="retired-versions"></a>Wycofane wersje

W poniższej tabeli wymieniono wersje HDInsight, które są **nie** dostępne w witrynie Azure portal.

| HDInsight w wersji | Wersja HDP | VM OS | Data wydania | Data wygaśnięcia pomocy technicznej | Data wygaśnięcia | Wysoka dostępność |  Dostępność w witrynie Azure portal | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.5 <br> (Inne niż Spark) |HDP 2.5 |Ubuntu 16.0.4 LTS |30 września 2016 r. |5 września 2017 r. |28 czerwca 2018 r. |Tak |Nie |
| HDInsight 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |29 marca 2016 r. |29 grudnia 2016 r. |9 stycznia 2018 r. |Tak |Nie |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2 grudnia 2015 r. |27 czerwca 2016 r. |31 lipca 2018 r. |Tak |Nie |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS |2 grudnia 2015 r. |27 czerwca 2016 r. |Do 31 lipca 2017 r. |Tak |Nie |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS lub Windows Server 2012 R2 |18 luty 2015 r. |1 marca 2016 r. |1 kwietnia 2017 r. |Yes |Nie |
| HDInsight 3.1 |HDP 2.1 |Windows Server 2012 R2 |24 czerwca 2014 r. |18 maja 2015 r. |30 czerwca 2016 r. |Tak |Nie |
| HDInsight 3.0 |HDP 2.0 |Windows Server 2012 R2 |11 lutego 2014 r. |17 września 2014 r. |Do 30 czerwca 2015 |Tak |Nie |
| HDInsight 2.1 |HDP 1.3 |Windows Server 2012 R2 |28 października 2013 |12 maja 2014 r. |Do 31 maja 2015 r. |Tak |Nie |
| HDInsight w wersji 1.6 |HDP 1.1 | |28 października 2013 |26 kwietnia 2014 r. |Do 31 maja 2015 r. |Nie |Nie |

> [!NOTE]  
> Klastrów o wysokiej dostępności przy użyciu dwa węzły główne są wdrażane domyślnie HDInsight w wersji 2.1 i nowszych. Nie są one dostępne dla klastrów HDInsight w wersji 1.6.

## <a name="enterprise-security-package-for-hdinsight"></a>Pakiet Enterprise Security for HDInsight

Zabezpieczenia przedsiębiorstwa jest opcjonalny pakiet, którą można dodać w klastrze usługi HDInsight jako część tworzenia klastra z przepływu pracy. Pakiet Enterprise Security obsługuje:

- Integracja z usługą Active Directory do uwierzytelniania.

    W przeszłości klastry HDInsight można utworzyć tylko użytkownika administratora lokalnego i lokalnego użytkownika SSH. Dostęp użytkownika administratora lokalnego, wszystkie pliki, foldery, tabele i kolumny.  Z pakietem Enterprise Security można włączyć kontroli dostępu opartej na rolach, integrując klastrów HDInsight za pomocą własnych usługi Active Directory, które obejmują lokalnej usługi Active Directory, Azure Active Directory Domain Services lub usługi Active Directory w infrastrukturze IaaS Maszyna wirtualna. Administrator domeny w klastrze można przyznać użytkownikom używanie własnej firmy (domena) nazwa użytkownika i hasło do dostępu do klastra. 

    Aby uzyskać więcej informacji, zobacz:

    - [Wprowadzenie do zabezpieczeń platformy Apache Hadoop przy użyciu klastrów HDInsight przyłączone do domeny](./domain-joined/apache-domain-joined-introduction.md)
    - [Planowanie Azure klastry platformy Apache Hadoop przyłączonych do domeny w HDInsight](./domain-joined/apache-domain-joined-architecture.md)
    - [Konfigurowanie środowiska izolowanego przyłączone do domeny](./domain-joined/apache-domain-joined-configure.md)
    - [Konfigurowanie klastrów HDInsight przyłączone do domeny za pomocą usługi Azure Active Directory Domain Services](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- Autoryzacja danych

  - Integracja przy użyciu struktury Apache Ranger autoryzacji dla gałęzi, Spark SQL i kolejek usługi Yarn.
  - Możesz ustawić kontroli dostępu do plików i folderów.

    Aby uzyskać więcej informacji, zobacz:

  - [Konfigurowanie zasad usługi Apache Hive HDInsight przyłączone do domeny](./domain-joined/apache-domain-joined-run-hive.md)

- Aby przejrzeć dziennik inspekcji dostępu do monitora i skonfigurowanych zasad. 

### <a name="supported-cluster-types"></a>Typy obsługiwane klastra

Obecnie tylko następujące typy klastrów obsługują pakiet Enterprise Security:

- Usługi Hadoop (tylko HDInsight 3.6)
- platforma Spark
- Zapytanie interakcyjne

### <a name="support-for-azure-data-lake-storage"></a>Obsługa usługi Azure Data Lake Storage

Obsługuje pakiet Enterprise Security, za pomocą usługi Azure Data Lake Storage jako magazynu głównego i dodatkowego magazynu.

### <a name="pricing-and-service-level-agreement"></a>Cennik i usługi Umowa dotycząca poziomu

Aby uzyskać informacje o cenach i umowy SLA pakiet Enterprise Security, zobacz [ceny HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).


## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>Umowa dotycząca poziomu usług dla wersji klastra HDInsight

Umowa dotycząca poziomu usług (SLA) jest definiowane w kategoriach _okna obsługi_. Okno obsługi jest czas, który klastra HDInsight w wersji jest obsługiwany przez dział obsługi klienta firmy Microsoft i pomocy technicznej. Jeśli wersja _obsługuje daty wygaśnięcia_ który został przekazany, klaster HDInsight znajduje się poza oknem obsługi. Obsługa datę wygaśnięcia określoną HDInsight w wersji X (po dostępna jest nowsza wersja X + 1) jest obliczany jako późniejsza od:  

* Formuła 1: Dodaj 180 dni do daty, kiedy wydanej wersji klastra HDInsight X.
* Formuła 2: Dodaj 90 dni do daty, kiedy wersji klastra HDInsight X + 1 ma zostać udostępnione w witrynie Azure portal.

_Dacie wycofania_ jest data, po upływie którego nie można utworzyć wersji klastra HDInsight. Od 31 lipca 2017 r. nie można rozmiaru klastra usługi HDInsight po dacie wycofania. 

> [!NOTE]  
> Klastry HDInsight Windows (z uwzględnieniem wersji 2.1, 3.0, 3.1, 3.2 i 3.3) uruchom rodziny systemów operacyjnych gościa platformy Azure w wersji 4, która korzysta z 64-bitowej wersji systemu Windows Server 2012 R2. Rodzina systemów operacyjnych gościa platformy Azure w wersji 4 obsługuje wersje .NET Framework 4.0, 4.5, 4.5.1 i 4.5.2.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks informacje o wersji skojarzony z wersjami HDInsight

Sekcja zawiera łącza do wersji, aby dystrybucji Hortonworks Data Platform i Apache składniki, które są używane w HDInsight.
* Dystrybucja usługi Hadoop, która jest oparta na korzysta z klastra HDInsight w wersji 4.0 [Hortonworks Data Platform 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html)
* Dystrybucja usługi Hadoop, która jest oparta na korzysta z klastra HDInsight w wersji 3.6 [Hortonworks Data Platform 2.6](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html).
* Wersja klastra HDInsight 3.5 używa dystrybucja usługi Hadoop, która jest oparta na [Hortonworks Data Platform 2.5](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html). Wersja klastra HDInsight 3.5 jest _domyślne_ klastra usługi Hadoop, który jest tworzony w witrynie Azure portal.
* Dystrybucja usługi Hadoop, która jest oparta na korzysta z klastra HDInsight w wersji 3.4 [Hortonworks Data Platform 2.4](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html).
* Dystrybucja usługi Hadoop, która jest oparta na korzysta z klastra HDInsight w wersji 3.3 [Hortonworks Data Platform 2.3](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * [Informacje o wersji platformy Apache Storm](https://storm.apache.org/2015/11/05/storm0100-released.html) są dostępne w witrynie sieci Web Apache.
  * [Informacje o wersji Apache Hive](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843) są dostępne w witrynie sieci Web Apache.
* Dystrybucja usługi Hadoop, która jest oparta na korzysta z klastra HDInsight w wersji 3.2 [Hortonworks Data Platform 2.2][hdp-2-2].

  * Informacje o wersji dotyczące określonych składników Apache są dostępne w następujący sposób: [Hive 0,14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0,14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [systemu plików HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [typowe](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), i [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* Dystrybucja usługi Hadoop, która jest oparta na korzysta z klastra HDInsight w wersji 3.1 [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. HDInsight 3.1 clusters created before November, 7, 2014, are based on [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* Dystrybucja usługi Hadoop, która jest oparta na korzysta z klastra HDInsight w wersji 3.0 [Hortonworks Data Platform w wersji 2.0][hdp-2-0-8].
* Dystrybucja usługi Hadoop, która jest oparta na korzysta z klastra HDInsight w wersji 2.1 [Hortonworks Data Platform 1.3][hdp-1-3-0].
* Dystrybucja usługi Hadoop, która jest oparta na korzysta z klastra HDInsight w wersji 1.6 [Hortonworks Data Platform 1.1][hdp-1-1-0].

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Domyślne rozmiary maszyn wirtualnych i konfiguracja węzła klastrów

W poniższej tabeli wymieniono domyślne rozmiary maszyny wirtualnej (VM) w przypadku klastrów HDInsight.  Ten wykres jest niezbędne do zrozumienia rozmiarów maszyn wirtualnych do użycia podczas tworzenia skryptów programu PowerShell lub wiersza polecenia platformy Azure, aby wdrożyć klastry HDInsight.

> [!IMPORTANT]  
> Jeśli potrzebujesz więcej niż 32 węzły procesu roboczego w klastrze, należy wybrać rozmiar węzła głównego z co najmniej 8 rdzeniami i 14 GB pamięci RAM.

* Wszystkie obsługiwane regiony z wyjątkiem Brazylii Południowej i Japonia, część zachodnia:

|Typ klastra|Hadoop|HBase|Zapytanie interakcyjne|Storm|platforma Spark|ML Server|Kafka|
|---|---|---|---|---|---|---|---|
|Główny: domyślny rozmiar maszyny Wirtualnej|D12 v2|D12 v2|D13 v2|A3|D12 v2|D12 v2|D3v2|
|Główny: zalecane rozmiary maszyn wirtualnych|D3 v2|D3 v2|D13|A4 v2|D12 v2|D12 v2|A2M, wersja 2|
||D4 v2|D4 v2|D14|A8 v2|D13 v2|D13 v2|D3 v2|
||D12 v2|D12 v2|E16 v3|A2m v2|D14 v2|D14 v2|D4 v2|
||E4 v3|E4 v3|E32 v3|E4 v3|E4 v3|E4 v3|D12 v2|
|Proces roboczy: domyślny rozmiar maszyny Wirtualnej|D4 v2|D4 v2|D14 v2|D3 v2|D13 v2|D4 v2|4 D12v2 z 2 dyski S30 dla każdego brokera|
|Proces roboczy: zalecane rozmiary maszyn wirtualnych|D3 v2|D3 v2|D13|D3 v2|D4 v2|D4 v2|D13 v2|
||D4 v2|D4 v2|D14|D4 v2|D12 v2|D12 v2|DS12 v2|
||D12 v2|D12 v2|E16 v3|D12 v2|D13 v2|D13 v2|DS13 v2|
||E4 v3|E4 v3|E20 v3|E4 v3|D14 v2|D14 v2|E4 v3|
||||E32 v3||E16 v3|E16 v3|ES4 v3|
||||E64 v3||E20 v3|E20 v3|E8 v3|
||||||E32 v3|E32 v3|ES8 v3|
||||||E64 v3|E64 v3||
|Dozorcy: domyślny rozmiar maszyny Wirtualnej||A4 v2|A4 v2|A4 v2||A2 v2|D3v2|
|Dozorcy: zalecane rozmiary maszyn wirtualnych||A4 v2||A2 v2|||A2M, wersja 2|
|||A8 v2||A4 v2|||D3 v2|
|||A2m v2||A8 v2|||E8 v3|
|Usługi ML: domyślny rozmiar maszyny Wirtualnej||||||D4 v2||
|Usługi ML: zalecany rozmiar maszyny Wirtualnej||||||D4 v2||
|||||||D12 v2||
|||||||D13 v2||
|||||||D14 v2||
|||||||E16 v3||
|||||||E20 v3||
|||||||E32 v3||
|||||||E64 v3||

* Brazylia Południowa i Japonia, część zachodnia tylko (nie rozmiary v2):

  | Typ klastra | Hadoop | HBase | Zapytanie interakcyjne |Storm | platforma Spark | Usługi ML |
  | --- | --- | --- | --- | --- | --- | --- |
  | Główny: domyślny rozmiar maszyny Wirtualnej |D12 |D12  | D13 |A3 |D12 |D12 |
  | Główny: zalecane rozmiary maszyn wirtualnych |D3,<br/> D4,<br/> D12 |D3,<br/> D4,<br/> D12  | D13,<br/> D14 |A3<br/> A4,<br/> A5 |D12,<br/> D13,<br/> D14 |D12,<br/> D13,<br/> D14 |
  | Proces roboczy: domyślny rozmiar maszyny Wirtualnej |D4 |D4  |  D14 |D3 |D13 |D4 |
  | Proces roboczy: zalecane rozmiary maszyn wirtualnych |D3,<br/> D4,<br/> D12 |D3,<br/> D4,<br/> D12  | D13,<br/> D14 |D3,<br/> D4,<br/> D12 |D4,<br/> D12,<br/> D13,<br/> D14 | D4,<br/> D12,<br/> D13,<br/> D14 |
  | Dozorcy: domyślny rozmiar maszyny Wirtualnej | |A4 v2 | A4 v2| A4 v2 | | A2 v2|
  | Dozorcy: zalecane rozmiary maszyn wirtualnych | |A2,<br/> A3<br/> A4 | |A2,<br/> A3<br/> A4 | | |
  | Usługi ML: domyślne rozmiarów maszyn wirtualnych | | | | | |D4 |
  | Usługi ML: zalecane rozmiary maszyn wirtualnych | | | | | |D4,<br/> D12,<br/> D13,<br/> D14 |

> [!NOTE]
> - Head jest znany jako *Nimbus* Storm typ klastra.
> - Proces roboczy jest znany jako *nadzorca* Storm typ klastra.
> - Proces roboczy jest znany jako *Region* dla bazy danych HBase typ klastra.

## <a name="next-steps"></a>Kolejne kroki
- [Klaster Instalatora dla technologii Apache Hadoop, Spark i więcej informacji na temat HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
- [Praca w technologii Apache Hadoop w HDInsight z Windows PC](hdinsight-hadoop-windows-tools.md)

[hdp-2-2]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.9/bk_HDP_RelNotes/content/ch_relnotes_v229.html

[hdp-2-1-7]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.1.1.16_1.html
