---
title: 'Szybki start: Tworzenie klastrów usługi Apache Hadoop przy użyciu usługi Resource Manager — usługi Azure HDInsight'
description: W tym przewodniku Szybki Start utworzysz klastra Apache Hadoop w usłudze Azure HDInsight przy użyciu szablonu usługi Resource Manager
keywords: wprowadzenie do usługi hadoop,hadoop linux,hadoop szybki start,wprowadzenie do usługi hive,hive szybki start
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.topic: quickstart
ms.date: 06/12/2019
ms.openlocfilehash: 8eb297c555d2d7f95419b2a9aa11a26cf5c7230f
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67137471"
---
# <a name="quickstart-create-apache-hadoop-cluster-in-azure-hdinsight-using-resource-manager-template"></a>Szybki start: Tworzenie klastra Apache Hadoop w usłudze Azure HDInsight przy użyciu szablonu usługi Resource Manager

W tym przewodniku Szybki Start dowiesz się, jak utworzyć klaster Apache Hadoop w usłudze Azure HDInsight przy użyciu szablonu usługi Resource Manager.

Można wyświetlić podobne szablony na [szablony szybkiego startu platformy](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Hdinsight&pageNumber=1&sort=Popular). Dokumentację szablonu można znaleźć [tutaj](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/allversions).  Można również utworzyć klaster przy użyciu [witryny Azure portal](apache-hadoop-linux-create-cluster-get-started-portal.md).  

Obecnie usługa HDInsight obejmuje [siedem różnych typów klastrów](./apache-hadoop-introduction.md#cluster-types-in-hdinsight). Każdy typ klastra obsługuje inny zestaw składników. Wszystkie typy klastrów obsługują technologię Hive. Aby uzyskać listę obsługiwanych składników w usłudze HDInsight, zobacz artykuł [Nowości w wersjach klastra Hadoop dostarczanych z usługą HDInsight](../hdinsight-component-versioning.md)  

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

<a name="create-cluster"></a>
## <a name="create-a-hadoop-cluster"></a>Tworzenie klastra usługi Hadoop

1. Wybierz **Wdróż na platformie Azure** przycisk poniżej, aby zalogować się do platformy Azure, a następnie otwórz szablon usługi Resource Manager w witrynie Azure portal.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-ssh-password%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Wprowadź lub wybierz poniższe wartości:

    |Właściwość  |Opis  |
    |---------|---------|
    |**Subskrypcja**     |  Wybierz swoją subskrypcję platformy Azure. |
    |**Grupa zasobów**     | Utwórz grupę zasobów lub wybierz istniejącą grupę zasobów.  Grupa zasobów jest kontenerem składników platformy Azure.  W tym przypadku grupa zasobów zawiera klaster usługi HDInsight i zależne konto usługi Azure Storage. |
    |**Lokalizacja**     | Wybierz lokalizację platformy Azure, w której chcesz utworzyć klaster.  Wybierz lokalizację znajdującą się blisko, aby zapewnić lepszą wydajność. |
    |**Nazwa klastra**     | Wprowadź nazwę klastra usługi Hadoop. Ponieważ wszystkie klastry w usłudze HDInsight używają tej samej przestrzeni nazw DNS, ta nazwa musi być unikatowa. Nazwa może zawierać tylko małe litery, cyfry i łączniki oraz musi zaczynać się literą.  Przed i za każdym łącznikiem musi znajdować się znak inny niż łącznik.  Nazwa musi również zawierać od 3 do 59 znaków. |
    |**Typ klastra**     | Wybierz pozycję **hadoop**. |
    |**Nazwa użytkownika i hasło logowania do klastra**     | Domyślna nazwa logowania to **admin**. Hasło musi składać się z co najmniej 10 znaków i musi zawierać co najmniej jedną cyfrę, jedną wielką i jedną małą literę oraz jeden znak inny niż alfanumeryczny (z wyjątkiem znaków ' " ` \). Upewnij się, że **nie zostało podane** typowe hasło, takie jak „Pass@word1”.|
    |**Nazwa użytkownika i hasło protokołu SSH**     | Domyślna nazwa użytkownika to **sshuser**.  Nazwę użytkownika SSH można zmienić.  Hasło użytkownika SSH ma te same wymagania co hasło logowania klastra.|

    Niektóre właściwości zostały umieszczone w kodzie w szablonie.  Te wartości można skonfigurować z szablonu. Aby uzyskać więcej informacji o tych właściwościach, zobacz artykuł [Create Hadoop clusters in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) (Tworzenie klastrów platformy Hadoop w usłudze HDInsight).

    > [!NOTE]  
    > Te wartości muszą być unikatowe i zgodne z wytycznymi dotyczącymi nazewnictwa. Szablon nie wykonuje testów walidacyjnych. Jeśli okaże się, że podane wartości są już używane lub nie są zgodne z wytycznymi, po przesłaniu szablonu wystąpi błąd.  

    ![HDInsight Linux pobiera wprowadzenie szablonu usługi Resource Manager w witrynie portal](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png "wdrażanie klastra usługi Hadoop w HDInsight przy użyciu witryny Azure portal i szablonu usługi resource manager grupy")

3. Zaznacz pozycję **Wyrażam zgodę na powyższe warunki i postanowienia**, a następnie kliknij przycisk **Kup**. Otrzymasz powiadomienie, że wdrożenie jest w toku.  Utworzenie klastra trwa około 20 minut.

4. Po utworzeniu klastra otrzymasz powiadomienie **Wdrażanie zakończyło się pomyślnie** z linkiem **Przejdź do grupy zasobów**.  Strona **Grupa zasobów** będzie zawierać listę nowego klastra usługi HDInsight oraz domyślny magazyn skojarzony z klastrem. Każdy klaster zależy od [konta usługi Azure Storage](../hdinsight-hadoop-use-blob-storage.md) lub od [konta usługi Azure Data Lake Storage](../hdinsight-hadoop-use-data-lake-store.md). Jest ono określane jako domyślne konto magazynu. Klaster HDInsight i jego domyślne konto magazynu muszą wspólnie przechowywane w tym samym regionie platformy Azure. Usunięcie klastrów nie powoduje usunięcia konta magazynu.

> [!NOTE]  
> Inne metody tworzenia klastrów i opis właściwości używanych w tym przewodniku Szybki Start, zobacz [klastrów HDInsight tworzenie](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Po ukończeniu tego przewodnika Szybki Start możesz usunąć klaster. Dzięki usłudze HDInsight dane są przechowywane w usłudze Azure Storage, więc można bezpiecznie usunąć klaster, gdy nie jest używany. Opłaty za klaster usługi HDInsight są naliczane nawet wtedy, gdy nie jest używany. Ponieważ opłaty za klaster są wielokrotnie większe niż opłaty za magazyn, ze względów ekonomicznych warto usuwać klastry, gdy nie są używane.

> [!NOTE]  
> Jeśli chcesz *natychmiast* przejść do następnego samouczka, aby dowiedzieć się, jak uruchomić operacje ETL przy użyciu usługi Hadoop w usłudze HDInsight, warto zachować działający klaster. Jest tak, ponieważ w tym samouczku trzeba ponownie utworzyć klaster Hadoop. Jeśli jednak nie chcesz od razu przechodzić do następnego samouczka, musisz teraz usunąć klaster.

**Usuwanie klastra i/lub domyślnego konta magazynu**

1. Wróć do karty przeglądarki, na której znajduje się witryna Azure Portal. Musisz mieć otwartą stronę omówienia klastra. Jeśli chcesz tylko usunąć klaster, zachowując domyślne konto magazynu, wybierz pozycję **Usuń**.

    ![Usuwanie klastra usługi HDInsight](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-delete-cluster.png "Usuwanie klastra usługi HDInsight")

2. Jeśli chcesz usunąć klaster oraz domyślne konto magazynu, wybierz nazwę grupy zasobów (wyróżnioną na poprzednim zrzucie ekranu), aby otworzyć stronę grupy zasobów.

3. Wybierz pozycję **Usuń grupę zasobów**, aby usunąć grupę zasobów zawierającą klaster i domyślne konto magazynu. Uwaga: usunięcie grupy zasobów powoduje usunięcie konta magazynu. Jeśli chcesz zachować konta magazynu, wybierz opcję usunięcia tylko klastra.

## <a name="next-steps"></a>Kolejne kroki

W tym przewodniku Szybki Start przedstawiono sposób tworzenia klastra Apache Hadoop w HDInsight przy użyciu szablonu usługi Resource Manager. W następnym artykule dowiesz się, jak przeprowadzić operację wyodrębniania, transformacji i ładowania (ETL, extract, transform, and load) przy użyciu usługi Hadoop w usłudze HDInsight.

> [!div class="nextstepaction"]
>[Wyodrębnianie, przekształcanie i ładowanie danych przy użyciu usługi Apache Hive w usłudze HDInsight](../hdinsight-analyze-flight-delay-data-linux.md)