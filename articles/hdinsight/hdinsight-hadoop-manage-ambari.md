---
title: Monitorowanie i zarządzanie nimi w usłudze Azure HDInsight przy użyciu interfejsu użytkownika sieci Web systemu Ambari
description: Dowiedz się, jak używać narzędzia Ambari do monitorowania i zarządzania klastrami HDInsight opartych na systemie Linux. W tym dokumencie dowiesz się, jak za pomocą interfejsu użytkownika sieci Web Ambari, które zostały dołączone do klastrów HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: hrasheed
ms.openlocfilehash: 49e8fbef7af16e109c1e9f1e0d8c9aab1a008e21
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66257997"
---
# <a name="manage-hdinsight-clusters-by-using-the-apache-ambari-web-ui"></a>Zarządzanie klastrami HDInsight przy użyciu Interfejsu sieci Web Apache Ambari

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari upraszcza zarządzanie i monitorowanie klastra Apache Hadoop, zapewniając łatwy do użycia interfejsu użytkownika sieci web i interfejsu API REST. Ambari znajduje się w klastrach HDInsight i służy do monitorowania klastra i wprowadzania zmian w konfiguracji.

W tym dokumencie dowiesz się, jak używać interfejsu użytkownika sieci Web Ambari z klastrem usługi HDInsight.

## <a id="whatis"></a>Co to jest Apache Ambari?

[Apache Ambari](https://ambari.apache.org) upraszcza zarządzanie usługi Hadoop, udostępniając łatwy w użyciu interfejsu użytkownika sieci web. Ambari służy do zarządzania i monitorowania klastrów platformy Hadoop. Deweloperzy mogą integrować możliwości swoich aplikacji za pomocą [interfejsów API REST Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

## <a name="connectivity"></a>Łączność

Interfejs użytkownika sieci Web Ambari jest dostępna w klastrze usługi HDInsight w `https://CLUSTERNAME.azurehdinsight.net`, gdzie `CLUSTERNAME` jest nazwą klastra.

> [!IMPORTANT]  
> Nawiązywanie połączenia z systemu Ambari w HDInsight wymaga protokołu HTTPS. Po wyświetleniu monitu o uwierzytelnienie, użyj nazwy konta administratora i hasło podane podczas tworzenia klastra.

## <a name="ssh-tunnel-proxy"></a>Tunel SSH (proxy)

Gdy Ambari klastra jest dostępny bezpośrednio przez Internet, niektóre linki w interfejsie użytkownika sieci Web Ambari, (np. JobTracker) nie są widoczne w Internecie. Aby uzyskać dostęp do tych usług, należy utworzyć tunel SSH. Aby uzyskać więcej informacji, zobacz [użycie tunelowania SSH z HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ambari-web-ui"></a>Ambari Web UI

> [!WARNING]  
> Nie wszystkie funkcje interfejsu użytkownika sieci Web Ambari, są obsługiwane w HDInsight. Aby uzyskać więcej informacji, zobacz [nieobsługiwane operacje](#unsupported-operations) części tego dokumentu.

Podczas nawiązywania połączenia Interfejsu sieci Web Ambari, monit uwierzytelniać się na stronie. Użyj użytkownika administratora klastra (ustawienie domyślne Admin) i hasło użyte podczas tworzenia klastra.

Po otwarciu strony, należy zwrócić uwagę na pasek u góry. Ten pasek zawiera następujące informacje i kontrolki:

![ambari nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

|Element |Opis |
|---|---|
|Logo systemu Ambari|Zostanie otwarty pulpit nawigacyjny, który może służyć do monitorowania klastra.|
|Platforma ops # nazwa klastra|Wyświetla liczbę trwające operacje Ambari. Wybieranie nazwy klastra lub **# ops** Wyświetla listę operacji w tle.|
|alerty (#)|Wyświetla ostrzeżenia lub krytyczny alertów, jeśli istnieje klastra.|
|Pulpit nawigacyjny|Wyświetla pulpitu nawigacyjnego.|
|Usługi|Informacje i ustawień konfiguracji dla usług w klastrze.|
|Hosts|Informacje i ustawień konfiguracji dla węzłów w klastrze.|
|Alerty|Dziennik informacje, ostrzeżenia i krytyczne alerty.|
|Jednostka administracyjna|Stos/usług oprogramowania, które są zainstalowane w klastrze, informacje o koncie usługi i zabezpieczeń protokołu Kerberos.|
|Przycisk administratora|Zarządzanie Ambari, ustawienia użytkownika i Wyloguj.|

## <a name="monitoring"></a>Monitorowanie

### <a name="alerts"></a>Alerty

Poniższa lista zawiera typowe stany alertów, używana przez narzędzia Ambari:

* **OK**
* **Ostrzeżenie**
* **KRYTYCZNE**
* **UNKNOWN**

Alerty innych niż **OK** spowodować **alerty (#)** wpis w górnej części strony, aby wyświetlić liczbę alertów. Wybranie tego wpisu wyświetla alerty wraz z ich stanami.

Alerty są podzielone na kilka domyślnych grup, które mogą być wyświetlane z **alerty** strony.

![Strona alertów](./media/hdinsight-hadoop-manage-ambari/alerts.png)

Grupy można zarządzać za pomocą **akcje** menu i wybierając polecenie **Zarządzaj grupami alertu**.

![Zarządzanie grupami alertu w oknie dialogowym](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

Można również zarządzać metodami alertów i tworzyć powiadomienia o alertach z **akcje** menu, wybierając __Zarządzanie powiadomienia Alert__. Wszelkie bieżące powiadomienia są wyświetlane. Powiadomienia można również utworzyć w tym miejscu. Powiadomienia mogą być wysyłane za pośrednictwem **E-mail** lub **SNMP** po wystąpieniu określonej ważności alertu/kombinacje. Na przykład, możesz wysłać wiadomość e-mail, gdy którykolwiek z alertów w **domyślne YARN** grupy jest ustawiona na **krytyczny**.

![Tworzenie okna dialogowego alertu](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

Na koniec, wybierając __Zarządzanie ustawieniami Alert__ z __akcje__ menu pozwala na ustawienie liczby alertu musi wystąpić przed wysłaniem powiadomienia. To ustawienie umożliwia zapobieganie powiadomienia dotyczące błędów przejściowych.

### <a name="cluster"></a>Klaster

**Metryki** karty Pulpit nawigacyjny zawiera szereg elementów widget, które ułatwiają monitorowanie stanu klastra na pierwszy rzut oka. Kilka widżetów, takich jak **użycie procesora CPU**, zawierają dodatkowe informacje po kliknięciu.

![pulpit nawigacyjny z metrykami](./media/hdinsight-hadoop-manage-ambari/metrics.png)

**Map cieplnych** karcie są wyświetlane metryki jako kolorowy map cieplnych, przechodząc od zielonego do czerwonego.

![pulpit nawigacyjny z mapy cieplne](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

Aby uzyskać więcej informacji na temat węzłów w klastrze, wybierz **hosty**. Następnie wybierz pozycję określonego węzła, który Cię interesuje.

![Szczegóły dotyczące hostów](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a>Usługi

**Usług** paska bocznego na pulpicie nawigacyjnym zapewnia szybki wgląd w stan usług działających w klastrze. Różne ikony są używane do wskazania stanu lub akcje, które należy podjąć. Na przykład symbol żółty odtwarzanie jest wyświetlany, jeśli usługa musi zostać odzyskany.

![pasek boczny usług](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]  
> Usług wyświetlonej różnią się między typami klastra HDInsight i wersjach. Usługi wyświetlane w tym miejscu mogą być inne niż usługi dla klastra.

Wybranie usługi zawiera więcej szczegółowych informacji w usłudze.

![Podsumowanie informacji o usłudze](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a>Szybkie łącza

Wyświetlanie niektórych usług **szybkich łączy** widocznego u góry strony. Może to służyć do dostępu do specyficzne dla usługi web UI, takich jak:

* **Historia zadań** -historii zadań MapReduce.
* **Menedżer zasobów** -interfejsie użytkownika YARN ResourceManager.
* **NameNode** — Hadoop Distributed pliku System (HDFS) NameNode w interfejsie użytkownika.
* **Interfejs użytkownika sieci Web programu Oozie** -Oozie interfejsu użytkownika.

Wybierając dowolną z poniższych linków, spowoduje otwarcie nowej karty w przeglądarce, który zawiera informacje na wybranej stronie.

> [!NOTE]  
> Wybieranie **szybkich łączy** wpis dla usług mogą zwracać błąd "nie można odnaleźć serwera". Jeśli wystąpi ten błąd, należy użyć tunelu SSH, korzystając z **szybkich łączy** wpis dla tej usługi. Aby uzyskać informacje, zobacz [użycie tunelowania SSH z HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="management"></a>Zarządzanie

### <a name="ambari-users-groups-and-permissions"></a>Ambari użytkowników, grup i uprawnień

Praca z użytkowników, grupy i uprawnienia są obsługiwane w przypadku korzystania z [przyłączonych do domeny](./domain-joined/apache-domain-joined-introduction.md) klastra HDInsight. Aby uzyskać informacji na temat używania interfejsu użytkownika zarządzania Ambari w klastrze przyłączonych do domeny, zobacz [Zarządzanie klastrami HDInsight przyłączone do domeny](./domain-joined/apache-domain-joined-introduction.md).

> [!WARNING]  
> Nie zmieniaj hasła strażnika Ambari (hdinsightwatchdog) w klastrze usługi HDInsight opartych na systemie Linux. Zmienianie hasła przerywa możliwość użyj akcji skryptu, lub wykonywać operacje skalowania na potrzeby klastra.

### <a name="hosts"></a>Hosts

**Hosty** strona zawiera listę wszystkich hostów w klastrze. Aby zarządzać hostami, wykonaj następujące kroki.

![strony hosty](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]  
> Dodawanie, wycofywanie z eksploatacji i recommissioning hosta nie należy używać z klastrami HDInsight.

1. Wybierz hosta, który ma być zarządzana.

2. Użyj **akcje** w menu akcji, którą chcesz wykonać:

    |Element |Opis |
    |---|---|
    |Uruchom wszystkie składniki|Uruchom wszystkie składniki na hoście.|
    |Zatrzymaj wszystkie składniki|Zatrzymaj wszystkie składniki na hoście.|
    |Uruchom ponownie wszystkie składniki|Uruchamiaj i zatrzymuj wszystkie składniki na hoście.|
    |Włącz tryb konserwacji|Pomija alertów dla hosta. W tym trybie powinno być włączone, jeśli są wykonywane działania, które generują alerty. Na przykład zatrzymanie i uruchomienie usługi.|
    |Wyłącz tryb konserwacji|Zwraca hosta do normalnego alertów.|
    |Stop|Zatrzymuje DataNode lub NodeManagers na hoście.|
    |Uruchamianie|Uruchamia DataNode lub NodeManagers na hoście.|
    |Ponowne uruchamianie|Zatrzymuje i uruchamia DataNode lub NodeManagers na hoście.|
    |Likwidowanie wdrożenia|Usuwa hosta z klastra. **Nie należy używać tej akcji w klastrach HDInsight.**|
    |Recommission|Dodaje wcześniej zlikwidowana hosta do klastra. **Nie należy używać tej akcji w klastrach HDInsight.**|

### <a id="service"></a>Usługi

Z **pulpit nawigacyjny** lub **usług** stronie **akcje** przycisk w dolnej części listy usług do zatrzymywania i uruchamiania wszystkich usług.

![działania usługi](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]  
> Gdy **Dodaj usługę** znajduje się w tym menu, nie można stosować nad dodaniem usług do klastra HDInsight. Powinny być dodawane nowe usługi przy użyciu akcji skryptów podczas inicjowania obsługi klastra. Aby uzyskać więcej informacji na temat korzystania z akcji skryptu, zobacz [HDInsight Dostosowywanie klastrów za pomocą akcji skryptu](hdinsight-hadoop-customize-cluster-linux.md).

Gdy **akcje** przycisku można uruchomić ponownie wszystkie usługi, często chcesz uruchomić, zatrzymać lub ponowne uruchamianie określonej usługi. Do wykonywania akcji względem pojedynczej usługi, należy użyć następujących czynności:

1. Z **pulpit nawigacyjny** lub **usług** stronie, wybierz usługę.

2. W górnej części **Podsumowanie** kartę, należy użyć **działania usługi** przycisk, a następnie wybierz akcję do wykonania. Spowoduje to ponowne uruchomienie usługi we wszystkich węzłach.

    ![Akcja usługi](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]  
   > Ponowne uruchamianie niektórych usług, po uruchomieniu klastra może generować alerty. Aby uniknąć alertów, można użyć **działania usługi** przycisk, aby włączyć **trybu konserwacji** usługi przed wykonaniem ponownego uruchomienia.

3. Po wybraniu akcji **# op** na początku zwiększa strony, aby pokazać, że odbywa się z operacji w tle. Jeśli skonfigurowana do wyświetlania, zostanie wyświetlona lista operacji w tle.

   > [!NOTE]  
   > Jeśli włączono **trybu konserwacji** dla usługi, pamiętaj, aby ją wyłączyć za pomocą **działania usługi** przycisku po zakończeniu operacji.

Aby skonfigurować usługę, użyj następujących kroków:

1. Z **pulpit nawigacyjny** lub **usług** stronie, wybierz usługę.

2. Wybierz **Configs** kartę. Bieżąca konfiguracja jest wyświetlana. Również zostanie wyświetlona lista powyższych konfiguracjach.

    ![Konfiguracje](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. Użyj pola wyświetlane, aby zmodyfikować konfigurację, a następnie wybierz **Zapisz**. Lub wybierz poprzednią konfigurację, a następnie wybierz **Ustaw jako bieżącą** wrócić do poprzednich ustawień.

## <a name="ambari-views"></a>Widoki Ambari

Widoki Ambari umożliwiają deweloperom Podłącz elementy interfejsu użytkownika do przy użyciu interfejsu użytkownika sieci Web Ambari [Apache Ambari widoków Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views). HDInsight udostępnia następujące widoki z typami klastrów usługi Hadoop:

* Widok programu hive: Hive View umożliwia uruchamianie zapytań Hive bezpośrednio w przeglądarce sieci web. Możesz zapisać zapytania, wyświetlić wyniki, Zapisz wyniki do magazynu klastra lub pobrać wyniki do systemu lokalnego. Aby uzyskać więcej informacji na temat korzystania z widoków programu Hive, zobacz [Użyj Apache Hive widoków z HDInsight](hadoop/apache-hadoop-use-hive-ambari-view.md).

* Widok aplikacji tez: Widok Tez pozwala lepiej zrozumieć i zadaniami optymalizacji. Można wyświetlić informacje, jak Tez zadania zostały wykonane i jakie zasoby są używane.

## <a name="unsupported-operations"></a>Nieobsługiwane operacje

Następujące operacje Ambari nie są obsługiwane w HDInsight:

* __Przenoszenie Usługa modułu zbierającego metryki__. Podczas wyświetlania informacji w usłudze modułu zbierającego metryk jest jedną z akcji, które są dostępne z menu Akcje usługi __modułu zbierającego przenieść metryki__. To jest nieobsługiwane w przypadku HDInsight.

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się, jak używać [Apache Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md) z HDInsight.