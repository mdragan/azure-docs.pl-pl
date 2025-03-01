---
title: Wprowadzenie do maszyny wirtualnej do nauki o danych platformy Azure dla systemów Linux i Windows | Microsoft Docs
description: Kluczowe składniki i scenariusze analizy dla maszyn wirtualnych do nauki o danych z systemami Windows i Linux.
keywords: data science tools, data science virtual machine, tools for data science, linux data science
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/22/2019
ms.author: gokuma
ms.openlocfilehash: 384cb274496670e0b0b5a33e001e78a0babed3f0
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427798"
---
# <a name="introduction-to-azure-data-science-virtual-machine-for-linux-and-windows"></a>Wprowadzenie do maszyny wirtualnej do nauki o danych platformy Azure dla systemów Linux i Windows

Maszyna wirtualna do nauki o danych to dostosowany obraz maszyny wirtualnej znajdujący się w chmurze Microsoft Azure, przeznaczony do nauki o danych. Zawiera ona wiele wstępnie zainstalowanych i skonfigurowanych, popularnych narzędzi do analizy danych, które pozwalają szybko rozpocząć tworzenie inteligentnych, zaawansowanych aplikacji analitycznych. Jest dostępna w systemach Windows Server i Linux. Oferujemy wersje maszyny wirtualnej dla systemów Windows Server 2016 i Windows Server 2012. Oferujemy maszynę wirtualną do nauki o danych dla systemu Linux w wersjach Ubuntu 16.04 LTS i CentOS 7.4.

W tym artykule opisano, co można zrobić za pomocą maszyny Wirtualnej do nauki o danych. Opisano niektóre kluczowe scenariusze używania maszyny Wirtualnej, a wyszczególniono najważniejsze funkcje dostępne w wersji systemów Windows i Linux. Artykuł także instrukcje na temat rozpocząć korzystanie z nich.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Jakie są możliwości maszyny wirtualnej do nauki o danych?
Maszyna wirtualna Data Science Virtual Machine ma oferować osobom zawodowo zajmującym się danymi na wszystkich poziomach umiejętności i we wszystkich branżach bezproblemowe, wstępnie skonfigurowane oraz w pełni zintegrowane środowisko nauki o danych. Zamiast samodzielnego wdrażania porównywalnego obszaru roboczego można aprowizować maszynę wirtualną do nauki o danych, aby zaoszczędzić dni lub nawet _tygodnie_ na procesach instalacji, konfiguracji i zarządzania pakietami. Po przydzieleniu maszyny wirtualnej do nauki o danych można natychmiast rozpocząć pracę nad projektem nauki o danych.

Maszyna wirtualna do nauki o danych została zaprojektowana i skonfigurowana do obsługi szerokiej gamy scenariuszy użycia. Środowisko w górę lub dół można skalować jako zmiany wymagań projektu. Można również użyć preferowanego języka programować zadania nauki o danych i instalować inne narzędzia, aby dostosować system do określonych wymagań.

## <a name="key-scenarios"></a>Kluczowe scenariusze
W tej sekcji przedstawiono niektóre najważniejsze scenariusze, w których można wdrożyć maszynę wirtualną do nauki o danych.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Wstępnie skonfigurowany pulpit analityczny w chmurze
Maszyna wirtualna do nauki o danych udostępnia konfigurację punktu odniesienia dla zespołów zajmujących się nauką o danych, które chcą zastąpić pulpity lokalne zarządzanym pulpitem w chmurze. Ten punkt odniesienia pozwala zagwarantować, że wszyscy naukowcy zajmujący się danymi w zespole mają spójną konfigurację umożliwiającą weryfikowanie eksperymentów i promowanie współpracy. Ponadto pozwala obniżyć koszty przez zmniejszenie nakładu pracy administratora systemu. Zapisuje to zmniejszenie obciążenia na czas potrzebny na ocenę, zainstalowanie i obsługa różnych pakietów oprogramowania wymaganych do zaawansowanej analizy.

### <a name="data-science-training-and-education"></a>Szkolenia i edukacja w zakresie nauki o danych
Instruktorzy przedsiębiorstwa i nauczycieli, pokazujące klas do nauki o danych zazwyczaj udostępniają obraz maszyny wirtualnej. Zapewniają one obrazu, aby upewnić się, że ich uczniów mają spójną konfigurację i przewidywalne działanie przykładów. Maszyna wirtualna do nauki o danych tworzy środowisko na żądanie ze spójną konfiguracją, która ułatwia radzenie sobie z problemami z obsługą i niezgodnością. To rozwiązanie oferuje znaczące korzyści w sytuacjach, w których jest konieczne częste tworzenie takich środowisk (zwłaszcza w przypadku krótszych szkoleń).

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Elastyczna wydajność na żądanie dla projektów w dużej skali
Maratony deweloperów i konkursy w zakresie nauki o danych lub modelowanie i eksplorowanie danych na dużą skalę wymagają wydajności sprzętu skalowanej w poziomie, zwykle przez krótki czas. Maszyna wirtualna do nauki o danych może pomóc replikowanie środowiska nauki o danych szybko na żądanie, na serwerach skalowanych w poziomie, które zezwalają na eksperymenty, tym o dużej wydajności zasobów komputerowych, do uruchomienia.

### <a name="custom-compute-power-for-azure-notebooks"></a>Niestandardowe mocy obliczeniowej dla notesów usługi Azure

[Notesy platformy Azure](/azure/notebooks/azure-notebooks-overview) to bezpłatna usługa hostowana na tworzenie, uruchamianie i udostępnianie aplikacji Jupyter notebooks w chmurze nie instalacji. Jednak w warstwie bezpłatna usługa i jest ograniczony do 4GB pamięci i 1GB danych. Aby zwolnić wszystkie limity, zostanie następnie dołączony projektu notesów do maszyny Wirtualnej do nauki o danych lub innej maszyny Wirtualnej serwerem Jupyter. Jeśli zalogujesz się do notesów usługi Azure przy użyciu konta, za pomocą usługi Azure Active Directory (na przykład konto firmowe), notesów automatycznie zawiera maszyny wirtualne do nauki o danych w dowolnej subskrypcji powiązanej z tym kontem. Aby uzyskać więcej informacji, zobacz [Konfigurowanie projektów i zarządzanie nimi — warstwa Obliczenia](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier).

### <a name="short-term-experimentation-and-evaluation"></a>Krótkoterminowe eksperymenty i ocena
Maszyna wirtualna do nauki o danych umożliwia ocenę lub naukę narzędzi, takich jak Microsoft ML Server, SQL Server, narzędzia programu Visual Studio, Jupyter, zestawy narzędzi do uczenia głębokiego i maszynowego, oraz nowych narzędzi popularnych w społeczności przy minimalnym zakresie konfiguracji. Ponieważ maszyna wirtualna do nauki o danych można skonfigurować pod kątem szybkiego, mogą być stosowane w innych krótkoterminowych scenariuszach użycia. Te scenariusze obejmują replikowanie opublikowanych eksperymentów, przeprowadzanie pokazów, wykonywanie po wskazówki w sesji online i samouczków konferencji.

### <a name="deep-learning"></a>Uczenie głębokie
Maszyna wirtualna do nauki o danych może być używana na potrzeby uczenia modeli za pomocą algorytmów uczenia głębokiego na sprzęcie opartym na procesorze GPU. Maszyna wirtualna Data Science Virtual Machine korzysta z możliwości skalowania chmury platformy Azure, aby ułatwić używanie sprzętu opartego na procesorze GPU w chmurze zgodnie z wymaganiami. Można przełączyć się do maszyny wirtualnej opartej na procesorze GPU w przypadku uczenia dużych modeli lub przeprowadzania szybkich obliczeń i zachować ten sam dysk systemu operacyjnego.  Maszyna wirtualna do nauki o danych dla systemu Windows Server 2016 jest oferowana z preinstalowanymi sterownikami procesora GPU, platformami i platformami uczenia głębokiego w wersjach dla procesorów GPU. W wersji dla systemu Linux uczenie głębokie procesora GPU jest włączone w przypadku maszyn wirtualnych do nauki o danych z systemami Ubuntu i CentOS. Można wdrożyć maszyny wirtualnej do nauki o danych w wersji Ubuntu, CentOS lub Windows 2016 bez opartą na procesorze GPU maszyny wirtualnej platformy Azure. W takim przypadku wszystkie platformy uczenia głębokiego powróci do trybu procesora CPU.

## <a name="whats-included-in-the-data-science-vm"></a>Co oferuje maszyna wirtualna do nauki o danych?
Maszyna wirtualna do nauki o danych obejmuje wiele popularnych narzędzi do nauki o danych i uczenia głębokiego, które są już zainstalowane i skonfigurowane. Zawiera również narzędzia, które ułatwiają pracę z różnymi produktami platformy Azure przeznaczonymi do obsługi danych i analizy. Wśród nich można wymienić narzędzie Microsoft ML Server (R, Python), które umożliwia tworzenie modeli predykcyjnych, lub program SQL Server 2017 pozwalający eksplorować zestawy danych w dużej skali. Maszyna wirtualna do nauki o danych obejmuje wiele innych narzędzi ze społeczności open source i firmy Microsoft, oraz przykładowy kod i notesy. W poniższej tabeli wyszczególniono i porównano główne składniki maszyny wirtualnej do nauki o danych w wersji dla systemów Windows i Linux.


| **Narzędzie**                                                           | **Wersja systemu Windows** | **Wersja systemu Linux** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R Open](https://mran.microsoft.com/open/) z preinstalowanymi popularnymi pakietami   |Tak                      | Tak             |
| Narzędzie [Microsoft ML Server (R, Python)](https://docs.microsoft.com/machine-learning-server/) Developer Edition zawiera następujące składniki: <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [RevoScaleR/revoscalepy](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler) — równoległa platforma rozproszona o wysokiej wydajności (R i Python)<br />  &nbsp;&nbsp;&nbsp;&nbsp;* [MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package) — najnowocześniejsze algorytmy uczenia maszynowego firmy Microsoft <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [Operacjonalizacja środowisk R i Python](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |Tak                      | Tak |
| [Microsoft Office](https://products.office.com/en-us/business/office-365-proplus-business-software) Pro-plus ze współużytkowaną aktywacją — Excel, Word i PowerPoint   |Tak                      |Nie              |
| [Anaconda Python](https://www.continuum.io/) 2.7 i 3.5 z preinstalowanymi popularnymi pakietami    |Tak                      |Tak              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) z preinstalowanymi popularnymi pakietami języka Julia                         |Tak                      |Tak              |
| Relacyjne bazy danych                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) (CentOS),<br/>[SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition (Ubuntu) |
| Narzędzia bazy danych                                                       | * SQL Server Management Studio <br/>* SQL Server Integration Services<br/>* [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> * Sterowniki ODBC/JDBC| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (narzędzie do obsługi zapytań), <br /> * bcp, sqlcmd <br /> * Sterowniki ODBC/JDBC|
| Skalowalna analiza w bazie danych z usługami uczenia maszynowego programu SQL Server (R, Python) | Tak     |Nie              |
| **[Serwer notesu Jupyter](https://jupyter.org/) z następującymi jądrami:**                                  | Tak     | Tak |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | Tak | Tak |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python | Tak | Tak |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Julia | Tak | Tak |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | Tak | Tak |
|     &nbsp;&nbsp;&nbsp;&nbsp;* [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | Nie | Tak (tylko Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | Nie | Tak |
| JupyterHub (serwer notesów dla wielu użytkowników)| Nie | Tak |
| JupyterLab (serwer notesów dla wielu użytkowników) | Nie | Tak (tylko Ubuntu) |
| **Narzędzia deweloperskie, środowiska IDE i kod edytorów**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio 2019 (Community Edition)](https://www.visualstudio.com/community/) z wtyczką Git, Azure HDInsight (Hadoop), Data Lake, SQL Server Data tools [Node.js](https://github.com/Microsoft/nodejstools), [Python](https://aka.ms/ptvs), i [R Tools for Visual Studio (RTVS)](https://microsoft.github.io/RTVS-docs/) | Tak | Nie |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio Code](https://code.visualstudio.com/) | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm Community Edition](https://www.jetbrains.com/pycharm/) | Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Atom](https://atom.io/) | Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Juno (Julia IDE)](https://junolab.org/)| Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* Vim i Emacs | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git i GitBash | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* .NET Framework | Tak | Nie |
| Power BI Desktop | Tak | Nie |
| Zestawy SDK umożliwiające dostęp do platformy Azure i pakietu usług Cortana Intelligence Suite | Tak | Tak |
| **Narzędzia do przenoszenia danych i zarządzania nimi** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Eksplorator usługi Azure Storage | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Interfejs wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure) | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Powershell | Tak | Nie |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | Tak | Nie |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Sterownik obiektów blob systemu FUSE](https://github.com/Azure/azure-storage-fuse) | Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Adlcopy (magazyn usługi Azure Data Lake)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | Tak | Nie |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Narzędzie do migracji danych DocDB](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | Tak | Nie |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Brama zarządzania danymi firmy Microsoft](https://msdn.microsoft.com/library/dn879362.aspx): Przenoszenie danych między środowiskiem lokalnym i chmurą | Tak | Nie |
| &nbsp;&nbsp;&nbsp;&nbsp;* Narzędzia wiersza polecenia systemu Unix/Linux | Tak | Tak |
| [Apache Drill](https://drill.apache.org) do eksploracji danych | Tak | Tak |
| **Narzędzia uczenia maszynowego** |||
| &nbsp;&nbsp;&nbsp;&nbsp;* Integracja z usługą [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Xgboost](https://github.com/dmlc/xgboost) | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Weka](https://www.cs.waikato.ac.nz/ml/weka/) | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rattle](https://togaware.com/rattle/) | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [LightGBM](https://github.com/Microsoft/LightGBM) | Nie | Tak (tylko Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CatBoost](https://tech.yandex.com/catboost/) | Nie | Tak (tylko Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/), [Sparkling Water](https://www.h2o.ai/sparkling-water/) | Nie | Tak (tylko Ubuntu) |
| **Narzędzia uczenia głębokiego** <br>Wszystkie narzędzia współdziałają z procesorem GPU lub CPU |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) (Windows 2016) | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow](https://www.tensorflow.org/) | Tak (Windows 2016) | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Horovod](https://github.com/uber/horovod) | Nie | Tak (Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet](https://mxnet.io/) | Tak (Windows 2016) | Tak|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Caffe i Caffe2](https://github.com/caffe2/caffe2) | Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Chainer](https://chainer.org/) | Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Torch](http://torch.ch/) | Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Theano](https://github.com/Theano/Theano) | Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Keras](https://keras.io/)| Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyTorch](https://pytorch.org/)| Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [NVidia Digits](https://github.com/NVIDIA/DIGITS) | Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet Model Server](https://github.com/awslabs/mxnet-model-server) | Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow Serving](https://www.tensorflow.org/serving/) | Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorRT](https://developer.nvidia.com/tensorrt) | Nie | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Sterownik CUDA, cuDNN, NVIDIA](https://developer.nvidia.com/cuda-toolkit) | Tak | Tak |
| **Platforma danych big data (tylko środowisko deweloperskie i testowe)**|||
| &nbsp;&nbsp;&nbsp;&nbsp;* Lokalna autonomiczna platforma [Spark](https://spark.apache.org/) | Tak | Tak |
| &nbsp;&nbsp;&nbsp;&nbsp;* Lokalna usługa [Hadoop](https://hadoop.apache.org/) (HDFS, YARN) | Nie | Tak |

## <a name="get-started"></a>Rozpoczęcie pracy

### <a name="windows-data-science-vm"></a>Maszyna wirtualna do nauki o danych z systemem Windows
* Aby uzyskać więcej informacji na temat tworzenia i używania maszyny wirtualnej do nauki o danych z systemem Windows, zobacz [Provision the Windows Data Science Virtual Machine (Aprowizowanie maszyny wirtualnej do nauki o danych z systemem Windows)](provision-vm.md). Aby uzyskać więcej informacji na temat wykonywania różnych zadań wymaganych dla projektu nauki o danych na maszynie wirtualnej do nauki o danych z systemem Windows, zobacz [Ten things you can do on the Data Science Virtual Machine (Dziesięć rzeczy, które można zrobić na maszynie wirtualnej do nauki o danych)](vm-do-ten-things.md).

### <a name="linux-data-science-vm"></a>Maszyna wirtualna do nauki o danych z systemem Linux
* Aby uzyskać więcej informacji na temat tworzenia i używania maszyny wirtualnej do nauki o danych z systemem Ubuntu, zobacz [Provision the Data Science Virtual Machine for Linux (Ubuntu) (Aprowizowanie maszyny wirtualnej do nauki o danych dla systemu Linux — Ubuntu)](dsvm-ubuntu-intro.md). Aby uzyskać więcej informacji na temat tworzenia i używania maszyny wirtualnej do nauki o danych z systemem CentOS, zobacz [Provision a Linux CentOS Data Science Virtual Machine on Azure (Aprowizowanie maszyny wirtualnej do nauki o danych z systemem Linux CentOS na platformie Azure)](linux-dsvm-intro.md).
* Przewodnik z instrukcjami dotyczącymi typowych zadań w zakresie nauki o danych na maszynie wirtualnej z systemem Linux (CentOS i Ubuntu) jest dostępny na stronie [Data science on the Linux Data Science Virtual Machine (Nauka o danych na maszynie wirtualnej do nauki o danych z systemem Linux)](linux-dsvm-walkthrough.md).

## <a name="next-steps"></a>Kolejne kroki
[R developer's guide to Azure](/azure/architecture/data-guide/technology-choices/r-developers-guide) (Przewodnik po platformie Azure dla deweloperów języka R)
