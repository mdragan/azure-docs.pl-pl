---
title: Skonfiguruj ograniczenia ruchu sieciowego ruchu wychodzącego w przypadku klastrów Azure HDInsight
description: Dowiedz się, jak skonfigurować ograniczenie ruchu sieciowego ruchu wychodzącego w przypadku klastrów Azure HDInsight.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: howto
ms.date: 05/30/2019
ms.openlocfilehash: 542813e0f82a1a52142a2b82bea3fdb101fdec28
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077174"
---
# <a name="configure-outbound-network-traffic-for-azure-hdinsight-clusters-using-firewall-preview"></a>Konfigurowanie wychodzącego ruchu sieciowego w przypadku klastrów Azure HDInsight przy użyciu zapory (wersja zapoznawcza)

Ten artykuł zawiera instrukcje umożliwiające bezpieczny ruch wychodzący z klastra usługi HDInsight przy użyciu zapory platformy Azure. W poniższych krokach przyjęto, konfigurowania zapory platformy Azure do istniejącego klastra. Jeżeli wdrażasz nowy klaster za zaporą, najpierw Tworzenie klastra HDInsight i podsieci i następnie wykonaj kroki opisane w tym przewodniku.

## <a name="background"></a>Tło

Klastry usługi Azure HDInsight są zwykle wdrażane w sieci wirtualnej. Klaster ma zależności od usługi spoza tej sieci wirtualnej, które wymagają dostępu do sieci, aby działać prawidłowo.

Istnieją pewne zależności, które wymagają ruchu przychodzącego. Nie można wysłać ruchu przychodzącego zarządzania za pomocą urządzenia zapory. Źródłowe adresy dla tego ruchu są znane i są publikowane [tutaj](hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip). Dzięki tym informacjom można zabezpieczyć ruch przychodzący do klastrów, można również tworzyć reguły sieciowej grupy zabezpieczeń (NSG).

Zależności ruch wychodzący HDInsight prawie całkowicie są definiowane przy użyciu nazw FQDN, które nie mają statyczne adresy IP spodem. Brak statycznych adresów oznacza, że sieciowe grupy zabezpieczeń (NSG) nie można zablokować ruch wychodzący z klastra. Zmianie adresów wystarczająco często nie jeden skonfigurować reguły oparte na bieżącym rozpoznawania nazw i używać go do skonfigurowania reguły sieciowej grupy zabezpieczeń.

Rozwiązania do zabezpieczania adresy ruchu wychodzącego jest korzystanie z urządzenia zapory, który można kontrolować ruch wychodzący na podstawie nazw domen. Zaporę platformy Azure można ograniczyć ruchu wychodzącego ruchu HTTP i HTTPS na podstawie nazwy FQDN docelowego lub [tagów w pełni kwalifikowaną nazwę domeny](https://docs.microsoft.com/azure/firewall/fqdn-tags).

## <a name="configuring-azure-firewall-with-hdinsight"></a>Konfigurowanie zapory platformy Azure za pomocą HDInsight

Podsumowanie kroki, aby zablokować ruch wychodzący z usługi HDInsight istniejących za pomocą zapory usługi Azure są następujące:
1. Tworzenie zapory.
1. Dodawanie reguły aplikacji do zapory
1. Dodawanie reguł sieci do zapory.
1. Tworzenie tabeli routingu.

### <a name="create-a-new-firewall-for-your-cluster"></a>Tworzenie nowej zapory dla klastra

1. Utwórz podsieć o nazwie **AzureFirewallSubnet** w sieci wirtualnej, gdy istnieje klastra. 
1. Tworzenie nowej zapory **FW01 testu** wykonując kroki w [samouczka: Wdróż i Skonfiguruj zaporę platformy Azure przy użyciu witryny Azure portal](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall).

### <a name="configure-the-firewall-with-application-rules"></a>Konfigurowanie zapory przy użyciu reguł aplikacji

Utwórz kolekcję reguł aplikacji, która umożliwia klastrowi wysyłać i odbierać wiadomości ważne.

Wybierz pozycję Nowa Zapora **FW01 testu** w witrynie Azure portal. Kliknij przycisk **reguły** w obszarze **ustawienia** > **kolekcji reguł aplikacji** > **dodać kolekcję reguł aplikacji**.

![Tytuł: Dodawanie kolekcji reguł aplikacji](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection.png)

Na **dodać kolekcję reguł aplikacji** ekranu, wykonaj następujące czynności:

1. Wprowadź **nazwa**, **priorytet**i kliknij przycisk **Zezwalaj** z **akcji** menu rozwijanego, a następnie wprowadź następujące reguły w **FQDN tagów sekcji** :

   | **Nazwa** | **Źródłowy adres** | **Nazwa FQDN tagu** | **Uwagi** |
   | --- | --- | --- | --- |
   | Rule_1 | * | HDInsight i Windows Update | Wymagane usługi HDI |

1. Dodaj następujące reguły, aby **sekcji nazw FQDN docelowego** :

   | **Nazwa** | **Źródłowy adres** | **Protocol:Port** | **Docelowy nazwy FQDN** | **Uwagi** |
   | --- | --- | --- | --- | --- |
   | Rule_2 | * | https:443 | login.windows.net | Umożliwia działanie logowania Windows |
   | Rule_3 | * | https:443,http:80 | <storage_account_name.blob.core.windows.net> | Jeśli klaster jest wspierany przez WASB, Dodaj regułę dla WASB. Aby używać tylko protokołu https połączeń upewnij się, ["Wymagany bezpieczny transfer"](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) jest włączona na koncie magazynu. |

1. Kliknij pozycję **Add** (Dodaj).

   ![Tytuł: Wprowadź szczegóły kolekcji reguł aplikacji](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection-details.png)

### <a name="configure-the-firewall-with-network-rules"></a>Konfigurowanie zapory przy użyciu reguł sieci

Tworzenie reguł sieci, aby poprawnie skonfigurować klastra usługi HDInsight.

1. Wybierz pozycję Nowa Zapora **FW01 testu** w witrynie Azure portal.
1. Kliknij przycisk **reguły** w obszarze **ustawienia** > **sieci kolekcji reguł** > **dodać kolekcję reguł sieci**.
1. Na **dodać kolekcję reguł sieci** ekranu, należy wprowadzić **nazwa**, **priorytet**i kliknij przycisk **Zezwalaj** z **akcji** menu rozwijanego.
1. Utworzenie następujących reguł w **adresów IP** sekcji:

   | **Nazwa** | **Protokół** | **Źródłowy adres** | **Adres docelowy** | **Port docelowy** | **Uwagi** |
   | --- | --- | --- | --- | --- | --- |
   | Rule_1 | UDP | * | * | `123` | Usługa Czas |
   | Rule_2 | Dowolne | * | DC_IP_Address_1, DC_IP_Address_2 | `*` | Jeśli używasz pakietu zabezpieczeń przedsiębiorstwa (ESP), Dodaj regułę sieci w sekcji adresów IP, która umożliwia komunikację za pomocą usługi AAD DS ESP klastrów. Adresy IP kontrolerów domeny w sekcji usługi AAD DS można znaleźć w portalu | 
   | Rule_3 | TCP | * | Adres IP Twojego konta usługi Data Lake Storage | `*` | Jeśli używasz usługi Azure Data Lake Storage, można dodać regułę sieci w sekcji adresów IP do rozwiązania problemu SNI ADLS Gen1 i Gen2. Ta opcja będzie kierować ruch do zapory, która może prowadzić do wyższych kosztów dla dużej ilości danych, ale ruch będzie rejestrowane i inspekcji w dziennikach zapory. Określ adres IP dla swojego konta usługi Data Lake Storage. Można użyć polecenia programu powershell, takie jak `[System.Net.DNS]::GetHostAddresses("STORAGEACCOUNTNAME.blob.core.windows.net")` rozpoznać nazwę FQDN jako adres IP.|
   | Rule_4 | TCP | * | * | `12000` | (Opcjonalnie) Jeśli używasz usługi Log Analytics, Utwórz regułę sieciowej w sekcji adresów IP, aby umożliwić komunikację z obszaru roboczego usługi Log Analytics. |

1. Utworzenie następujących reguł w **tagi usługi** sekcji:

   | **Nazwa** | **Protokół** | **Źródłowy adres** | **Tagi usługi** | **Port docelowy** | **Uwagi** |
   | --- | --- | --- | --- | --- | --- |
   | Rule_7 | TCP | * | * | `1433,11000-11999,14000-14999` | Skonfiguruj regułę sieci w sekcji tagi usługi dla programu SQL, która umożliwi Ci do logowania i inspekcji ruchu SQL, chyba że skonfigurowano punktów końcowych usługi dla programu SQL Server w podsieci HDInsight, który będzie ominąć zaporę. |

1. Kliknij przycisk **Dodaj** wymagana do ukończenia tworzenia Twojej kolekcji reguł sieci.

   ![Tytuł: Wprowadź szczegóły kolekcji reguł aplikacji](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-network-rule-collection.png)

### <a name="create-and-configure-a-route-table"></a>Tworzenie i konfigurowanie tabelę tras

Utwórz tabelę tras z następujących pozycji:

1. Sześć adresów z [tę listę wymaganych adresów IP zarządzania HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip) z następnym przeskokiem **Internet**:
    1. Cztery adresy IP dla wszystkich klastrów we wszystkich regionach
    1. Dwa adresy IP, które są specyficzne dla regionu, w którym został utworzony klaster
1. Jedna trasa urządzenie wirtualne dla adresu IP adresu 0.0.0.0/0 za pomocą następnego przeskoku jest prywatny adres IP swojej zapory usługi Azure.

Na przykład aby skonfigurować tabeli tras dla klastra, utworzone w regionie USA "Środkowe stany USA", należy użyć poniższe czynności:

1. Zaloguj się do Portalu Azure.
1. Wybierz pozycję Azure zapory **FW01 testu**. Kopiuj **prywatny adres IP** na **Przegląd** strony. W tym przykładzie użyjemy **przykładowy adres 10.1.1.4**
1. Utwórz nową tabelę tras.
1. Kliknij przycisk **trasy** w obszarze **ustawienia**.
1. Kliknij przycisk **Dodaj** można utworzyć trasy dla adresów IP w poniższej tabeli.

| Nazwa trasy | Prefiks adresu | Typ następnego skoku | Adres następnego skoku |
|---|---|---|---|
| 168.61.49.99 | 168.61.49.99/32 | Internet | Nie dotyczy |
| 23.99.5.239 | 23.99.5.239/32 | Internet | Nie dotyczy |
| 168.61.48.131 | 168.61.48.131/32 | Internet | Nie dotyczy |
| 138.91.141.162 | 138.91.141.162/32 | Internet | Nie dotyczy |
| 13.67.223.215 | 13.67.223.215/32 | Internet | Nie dotyczy |
| 40.86.83.253 | 40.86.83.253/32 | Internet | Nie dotyczy |
| 0.0.0.0 | 0.0.0.0/0 | Urządzenie wirtualne | 10.1.1.4 |

Zakończ konfigurację tabeli tras:

1. Przypisz tabeli tras podsieci usługi HDInsight utworzony przez kliknięcie przycisku **podsieci** w obszarze **ustawienia** i następnie **skojarzyć**.
1. Na **Skojarz podsieć** ekranu, wybierz sieć wirtualną, że klaster został utworzony w i **podsieci HDInsight** używane dla klastra usługi HDInsight.
1. Kliknij przycisk **OK**.

## <a name="edge-node-or-custom-application-traffic"></a>Węzłem krawędzi lub ruch do aplikacji niestandardowej

Powyższych czynności umożliwi klastra aby działać bez problemów. Nadal należy skonfigurować zależności, aby uwzględnić niestandardowe aplikacje uruchomione w węzłach brzegowych, jeśli ma to zastosowanie.

Zależności aplikacji, należy zidentyfikować i dodane do tabeli tras lub zapory usługi Azure.

Trasy musi zostać utworzony dla ruchu aplikacji uniknąć problemów z routingu asymetrycznego.

Jeśli aplikacje mają inne zależności, muszą można dodać do zapory platformy Azure. Tworzenie reguł aplikacji, które zezwalają na ruch HTTP/HTTPS i reguł sieciowych dla wszystkich innych.

## <a name="logging"></a>Rejestrowanie

Zaporę platformy Azure umożliwia wysyłanie dzienników do kilku systemów do innego magazynu. Instrukcje dotyczące konfigurowania rejestrowania dla zapory, postępuj zgodnie z instrukcjami w [samouczka: Monitoruj dzienniki zapory platformy Azure i metryk](../firewall/tutorial-diagnostics.md).

Po zakończeniu instalacji rejestrowania, jeśli dane logowania do usługi Log Analytics możesz wyświetlić zablokowanego ruchu z zapytaniem, takie jak następujące:

```
AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
```

Integrowanie zapory platformy Azure przy użyciu dzienników usługi Azure Monitor jest przydatne w przypadku, gdy najpierw pobierania aplikacji działać po użytkownik nie rozpoznają wszystkie zależności aplikacji. Dowiedz się więcej na temat dzienników usługi Azure Monitor z [Analizuj dane dzienników w usłudze Azure Monitor](../azure-monitor/log-query/log-query-overview.md)

## <a name="access-to-the-cluster"></a>Dostęp do klastra
Po pomyślnym konfiguracji zapory, można użyć wewnętrznego punktu końcowego (`https://<clustername>-int.azurehdinsight.net`) do dostępu Ambari z w obrębie sieci Wirtualnej. Aby użyć publicznego punktu końcowego (`https://<clustername>.azurehdinsight.net`) lub protokołu ssh punktu końcowego (`<clustername>-ssh.azurehdinsight.net`), upewnij się, masz prawo tras w tabeli tras i reguły sieciowej grupy zabezpieczeń Instalatora w celu uniknięcia problemu routingu assymetric wyjaśniono [tutaj](https://docs.microsoft.com/azure/firewall/integrate-lb).

## <a name="configure-another-network-virtual-appliance"></a>Skonfiguruj inną wirtualnego urządzenia sieciowego

>[!Important]
> Poniżej przedstawiono informacje **tylko** wymagane, jeśli chcesz skonfigurować wirtualnego urządzenia sieciowego (WUS) niż Zapora usługi Azure.

Poprzednich instrukcji pomocne w konfigurowaniu zapory usługi Azure w celu ograniczania ruchu wychodzącego z klastra usługi HDInsight. Zaporę platformy Azure jest automatycznie konfigurowany zezwalająca na ruch dla wielu typowych scenariuszy ważne. Jeśli chcesz użyć innego wirtualnego urządzenia sieciowego, należy ręcznie skonfigurować szereg dodatkowych funkcji. Pamiętać o następujących jako swojej skonfigurować wirtualne urządzenie sieciowe:

* Punkty końcowe usługi należy skonfigurować usługi zdolne do punktu końcowego usługi.
* Adres IP zależności są przeznaczone dla ruchu innego niż do protokołu HTTP/S (ruch TCP i UDP).
* Punktów końcowych HTTP/HTTPS nazwy FQDN można umieścić w urządzeniu WUS.
* Symbol wieloznaczny HTTP/HTTPS punkty końcowe są zależności, które mogą się różnić na podstawie liczby kwalifikatorów.
* Przypisz tabeli tras, które tworzysz do podsieci usługi HDInsight.

### <a name="service-endpoint-capable-dependencies"></a>Zależności zdolne do punktu końcowego usługi

| **Punkt końcowy** |
|---|
| Azure SQL |
| Azure Storage |
| Usługa Azure Active Directory |

#### <a name="ip-address-dependencies"></a>Adres IP zależności

| **Punkt końcowy** | **Szczegóły** |
|---|---|
| \*:123 | Sprawdzanie zegara NTP. Ruch jest sprawdzany w wielu punktach końcowych na port 123 |
| Opublikowane adresy IP [tutaj](hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip) | Są to usługi HDInsight |
| Klastry usługi AAD DS prywatnych adresów IP dla ESP |
| \*: 16800 aktywacji Windows usługi zarządzania Kluczami |
| \*12000 usługi Log Analytics |

#### <a name="fqdn-httphttps-dependencies"></a>Zależności w pełni kwalifikowaną nazwę domeny HTTP/HTTPS

>[!Important]
> Poniższa lista zawiera tylko kilka najważniejszych nazwy FQDN. Można uzyskać pełną listę nazw FQDN konfigurowania Twoje urządzenie WUS [w tym pliku](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json).

| **Punkt końcowy**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |
| wawsinfraprodbay063.blob.core.windows.net:443                         |
| registry-1.docker.io:443                                              |
| auth.docker.io:443                                                    |
| production.cloudflare.docker.com:443                                  |
| download.docker.com:443                                               |
| us.archive.ubuntu.com:80                                              |
| download.mono-project.com:80                                          |
| packages.treasuredata.com:80                                          |
| security.ubuntu.com:80                                                |
| azure.archive.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                |
| ocsp.digicert.com:80                                                |

## <a name="next-steps"></a>Kolejne kroki

* [Architektura sieci wirtualnej usługi Azure HDInsight](hdinsight-virtual-network-architecture.md)
