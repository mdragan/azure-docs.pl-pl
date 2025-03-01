---
title: Tworzenie fabryki Azure Data Factory przy użyciu interfejsu użytkownika usługi Azure Data Factory | Microsoft Docs
description: Tworzenie fabryki danych z potokiem, który kopiuje dane między lokalizacjami w usłudze Azure Blob Storage.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: quickstart
ms.date: 06/20/2018
ms.author: jingwang
ms.openlocfilehash: 6f5a4e04c0d135e85624b04dbcdcda6b7d15a427
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60315561"
---
# <a name="quickstart-create-a-data-factory-by-using-the-azure-data-factory-ui"></a>Szybki start: Tworzenie fabryki danych za pomocą interfejsu użytkownika usługi Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service that you are using:"]
> * [Wersja 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Bieżąca wersja](quickstart-create-data-factory-portal.md)

W tym przewodniku Szybki start opisano sposób używania interfejsu użytkownika usługi Azure Data Factory w celu tworzenia i monitorowania fabryki danych. Potok tworzony w tej fabryce danych *kopiuje* dane z jednego folderu do innego folderu w usłudze Azure Blob Storage. Aby zapoznać się z samouczkiem dotyczącym *przekształcania* danych przy użyciu usługi Azure Data Factory, zobacz [Tutorial: Transform data using Spark (Samouczek: Przekształcanie danych przy użyciu platformy Spark)](tutorial-transform-data-spark-portal.md).

> [!NOTE]
> Jeśli jesteś nowym użytkownikiem usługi Azure Data Factory, przed wykonaniem kroków zawartych w tym przewodniku Szybki start zobacz [Wprowadzenie do usługi Azure Data Factory](data-factory-introduction.md). 

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="video"></a>Połączenia wideo 
Obejrzenie tego filmu wideo ułatwi zapoznanie się z interfejsem użytkownika usługi Data Factory: 
>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Visually-build-pipelines-for-Azure-Data-Factory-v2/Player]

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych

1. Uruchom przeglądarkę internetową **Microsoft Edge** lub **Google Chrome**. Obecnie interfejs użytkownika usługi Data Factory jest obsługiwany tylko przez przeglądarki internetowe Microsoft Edge i Google Chrome.
1. Przejdź do witryny [Azure Portal](https://portal.azure.com). 
1. Wybierz pozycję **Utwórz zasób** w menu po lewej stronie, a następnie pozycje **Analiza** i **Data Factory**. 
   
   ![Wybór usługi Data Factory w okienku „Nowy”](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)
1. Na stronie **Nowa fabryka danych** wprowadź wartość **ADFTutorialDataFactory** w polu **Nazwa**. 
      
   ![Strona „Nowa fabryka danych”](./media/quickstart-create-data-factory-portal/new-azure-data-factory.png)
 
   Nazwa fabryki danych platformy Azure musi być *globalnie unikatowa*. Jeśli wystąpi poniższy błąd, zmień nazwę fabryki danych (na przykład **&lt;twojanazwa&gt;ADFTutorialDataFactory**) i spróbuj utworzyć ją ponownie. Artykuł [Usługa Data Factory — reguły nazewnictwa](naming-rules.md) zawiera reguły nazewnictwa artefaktów usługi Data Factory.
  
   ![Komunikat o błędzie występujący, jeśli nazwa jest niedostępna](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
1. W obszarze **Subskrypcja** wybierz subskrypcję platformy Azure, w której chcesz utworzyć fabrykę danych. 
1. W obszarze **Grupa zasobów** wykonaj jedną z następujących czynności:
     
   - Wybierz pozycję **Użyj istniejącej**, a następnie wybierz istniejącą grupę zasobów z listy. 
   - Wybierz pozycję **Utwórz nową**, a następnie wprowadź nazwę grupy zasobów.   
         
   Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).  
1. W obszarze **Wersja** wybierz pozycję **V2**.
1. W obszarze **Lokalizacja** wybierz lokalizację fabryki danych.

   Ta lista zawiera tylko lokalizacje, które są obsługiwane przez usługę Data Factory i w których będą przechowywane metadane usługi Azure Data Factory. Pamiętaj, że skojarzone magazyny danych (na przykład Azure Storage lub Azure SQL Database) i jednostki obliczeniowe (na przykład HDInsight) używane przez tę usługę Data Factory mogą być uruchomione w innych regionach.

1. Wybierz pozycję **Utwórz**.

1. Po zakończeniu tworzenia zostanie wyświetlona strona **Fabryka danych**. Wybierz kafelek **Tworzenie i monitorowanie**, aby na osobnej karcie uruchomić aplikację interfejsu użytkownika usługi Azure Data Factory.
   
   ![Strona główna fabryki danych z kafelkiem „Tworzenie i monitorowanie”](./media/quickstart-create-data-factory-portal/data-factory-home-page.png)
1. Na stronie **Wprowadzenie** przejdź do karty **Tworzenie** na lewym panelu. 

    ![Strona „Wprowadzenie”](./media/quickstart-create-data-factory-portal/get-started-page.png)

## <a name="create-a-linked-service"></a>Tworzenie usługi połączonej
Podczas tej procedury utworzysz połączoną usługę służącą do łączenia konta usługi Azure Storage z fabryką danych. Połączona usługa ma informacje o połączeniu, których usługa Data Factory używa w środowisku uruchomieniowym do nawiązywania z nią połączenia.

1. Wybierz pozycję **Połączenia**, a następnie wybierz przycisk **Nowe** na pasku narzędzi. 

   ![Przyciski do tworzenia nowego połączenia](./media/quickstart-create-data-factory-portal/new-connection-button.png)    
1. Na stronie **Nowa połączona usługa** wybierz pozycję **Azure Blob Storage**, a następnie wybierz pozycję **Dalej**. 

   ![Wybieranie kafelka „Azure Blob Storage”](./media/quickstart-create-data-factory-portal/select-azure-blob-linked-service.png)
1. Wykonaj następujące czynności: 

   a. Wprowadź wartość **AzureStorageLinkedService** w polu **Nazwa**.

   b. W polu **Nazwa konta magazynu** wybierz nazwę konta usługi Azure Storage.

   c. Wybierz pozycję **Testuj połączenie**, aby sprawdzić, czy usługa Data Factory może nawiązać połączenie z kontem magazynu. 

   d. Aby zapisać połączoną usługę, wybierz pozycję **Zakończ**. 

   ![Połączona usługa Azure Storage — ustawienia](./media/quickstart-create-data-factory-portal/azure-storage-linked-service.png) 

## <a name="create-datasets"></a>Tworzenie zestawów danych
W tej procedurze utworzysz dwa zestawy danych: **InputDataset** i **OutputDataset**. Te zestawy danych są typu **AzureBlob**. Odwołują się one do połączonej usługi Azure Storage utworzonej w poprzedniej sekcji. 

Wejściowy zestaw danych reprezentuje dane źródłowe w folderze wejściowym. W definicji wejściowego zestawu danych określany jest kontener obiektów blob (**adftutorial**), folder (**input**) i plik (**emp.txt**), który zawiera dane źródłowe. 

Wyjściowy zestaw danych reprezentuje dane, które są kopiowane do lokalizacji docelowej. W definicji wyjściowego zestawu danych określany jest kontener obiektów blob (**adftutorial**), folder (**output**) i plik, do którego kopiowane są dane. Każde uruchomienie potoku ma skojarzony ze sobą unikatowy identyfikator. Aby uzyskać dostęp do tego identyfikatora, skorzystaj ze zmiennej systemowej **RunId**. Nazwa pliku wyjściowego jest dynamicznie obliczana na podstawie identyfikatora uruchomienia potoku.   

W ustawieniach połączonej usługi określono konto usługi Azure Storage, które zawiera dane źródłowe. W ustawieniach zestawu danych źródłowych należy określić, gdzie dokładnie znajduje się źródło danych (kontener obiektów blob, folder i plik). W ustawieniach zestawu danych ujścia należy określić, gdzie kopiowane są dane (kontener obiektów blob, folder i plik). 
 
1. Wybierz przycisk **+** (znak plus), a następnie wybierz pozycję **Zestaw danych**.

   ![Menu do tworzenia zestawu danych](./media/quickstart-create-data-factory-portal/new-dataset-menu.png)
1. Na stronie **Nowy zestaw danych** wybierz pozycję **Azure Blob Storage**, a następnie wybierz przycisk **Zakończ**. 

   ![Wybieranie usługi „Azure Blob Storage”](./media/quickstart-create-data-factory-portal/select-azure-blob-dataset.png)
1. Na karcie **Ogólne** zestawu danych wprowadź wartość **InputDataset** w polu **Nazwa**. 

1. Przejdź do karty **Połączenie** i wykonaj następujące czynności: 

    a. Wybierz pozycję **AzureStorageLinkedService** w polu **Połączona usługa**.

    b. Kliknij przycisk **Przeglądaj** w polu **Ścieżka pliku**.

    ![Karta „Połączenie” i przycisk „Przeglądaj”](./media/quickstart-create-data-factory-portal/file-path-browse-button.png) c. W oknie **Wybieranie pliku lub folderu** przejdź do folderu **input** w kontenerze **adftutorial**, wybierz plik **emp.txt**, a następnie wybierz przycisk **Zakończ**.

    ![Przeglądanie w poszukiwaniu pliku wejściowego](./media/quickstart-create-data-factory-portal/choose-file-folder.png)
    
    d. Opcjonalnie: wybierz pozycję **Podgląd danych**, aby wyświetlić dane w pliku emp.txt.     

1. Powtórz kroki, aby utworzyć wyjściowy zestaw danych:  

   a. Wybierz przycisk **+** (znak plus), a następnie wybierz pozycję **Zestaw danych**.

   b. Na stronie **Nowy zestaw danych** wybierz pozycję **Azure Blob Storage**, a następnie wybierz przycisk **Zakończ**.

   c. Na karcie **Ogólne** określ wartość **OutputDataset** jako nazwę.

   d. Na karcie **Połączenie** wybierz pozycję **AzureStorageLinkedService** jako połączoną usługę, a następnie wprowadź **adftutorial/output** jako folder w polu katalogu. Jeśli folder **output** nie istnieje, działanie kopiowania utworzy go w czasie wykonywania.

## <a name="create-a-pipeline"></a>Tworzenie potoku 
Podczas tej procedury utworzysz potok i zweryfikujesz go za pomocą działania kopiowania, które korzysta z wejściowego i wyjściowego zestawu danych. Działanie kopiowania służy do kopiowania danych z pliku określonego w ustawieniach wejściowego zestawu danych do pliku określonego w ustawieniach wyjściowego zestawu danych. Jeśli wejściowy zestaw danych określa tylko folder (a nie nazwę pliku), działanie kopiowania kopiuje wszystkie pliki w folderze źródłowym do lokalizacji docelowej. 

1. Wybierz przycisk **+** (znak plus), a następnie wybierz pozycję **Potok**. 

   ![Menu do tworzenia nowego potoku](./media/quickstart-create-data-factory-portal/new-pipeline-menu.png)
1. Na karcie **Ogólne** określ wartość **CopyPipeline** w polu **Nazwa**. 

1. W przyborniku **Działania** rozwiń pozycję **Przenoszenie i przekształcanie**. Przeciągnij działanie **Copy** (Kopiowanie) z przybornika **Działania** na powierzchnię projektanta potoku. Możesz również wyszukać działania w przyborniku **Działania**. Wprowadź wartość **CopyFromBlobToBlob** w polu **Nazwa**.

   ![Ustawienia ogólne działania kopiowania](./media/quickstart-create-data-factory-portal/copy-activity-general-settings.png)
1. Przejdź do karty **Źródło** w ustawieniach działania kopiowania, a następnie wybierz wartość **InputDataset** w polu **Zestaw danych źródłowych**.

1. Przejdź do karty **Ujście** w ustawieniach działania kopiowania, a następnie wybierz wartość **OutputDataset** w polu **Zestaw danych ujścia**.

1. Aby sprawdzić poprawność ustawień potoku, kliknij pozycję **Weryfikuj** na pasku narzędzi potoku powyżej kanwy. Sprawdź, czy potok został pomyślnie zweryfikowany. Wybierz przycisk **>>** (strzałka w prawo), aby zamknąć dane wyjściowe weryfikacji. 

## <a name="debug-the-pipeline"></a>Debugowanie potoku
W tym kroku przeprowadzisz debugowanie potoku przed jego wdrożeniem w usłudze Data Factory. 

1. Na pasku narzędzi nad kanwą potoku kliknij pozycję **Debugowanie**, aby wyzwolić przebieg testu. 
    
1. Sprawdź, czy w dolnej części karty **Dane wyjściowe** ustawień potoku wyświetlany jest stan przebiegu potoku. 

1. Sprawdź, czy w folderze **output** kontenera **adftutorial** znajduje się plik wyjściowy. Jeśli folder wyjściowy nie istnieje, usługa Data Factory automatycznie go utworzy. 

## <a name="trigger-the-pipeline-manually"></a>Ręczne wyzwalanie potoku
Podczas tej procedury wdrożysz jednostki (połączone usługi, zestawy danych i potoki) w usłudze Azure Data Factory. Następnie ręcznie wyzwolisz przebieg potoku. 

1. Przed wyzwoleniem potoku należy opublikować jednostki w usłudze Data Factory. Aby przeprowadzić publikowanie, w górnej części wybierz przycisk **Opublikuj wszystkie**. 

   ![Przycisk Opublikuj](./media/quickstart-create-data-factory-portal/publish-button.png)
1. Aby ręcznie wyzwolić potok, wybierz pozycję **Wyzwól** na pasku narzędzi potoku, a następnie wybierz pozycję **Wyzwól teraz**. 

## <a name="monitor-the-pipeline"></a>Monitorowanie potoku

1. Przejdź do karty **Monitorowanie** po lewej stronie. Kliknij przycisk **Odśwież**, aby odświeżyć listę.

   ![Karta monitorowania przebiegów potoków z przyciskiem „Odśwież”](./media/quickstart-create-data-factory-portal/monitor-trigger-now-pipeline.png)
1. Wybierz link **Wyświetl uruchomienia działania** w obszarze **Akcje**. Na tej stronie zostanie wyświetlony stan uruchomienia działania kopiowania. 

   ![Uruchomienia działania potoku](./media/quickstart-create-data-factory-portal/pipeline-activity-runs.png)
1. Aby wyświetlić szczegółowe informacje na temat operacji kopiowania, wybierz link **Szczegóły** (obraz okularów) w kolumnie **Akcje**. Aby uzyskać więcej informacji o właściwościach, zobacz [Omówienie działania kopiowania](copy-activity-overview.md). 

   ![Szczegóły operacji kopiowania](./media/quickstart-create-data-factory-portal/copy-operation-details.png)
1. Sprawdź, czy nowy plik jest widoczny w folderze **output**. 
1. Możesz wrócić do widoku **Uruchomienia potoków** z widoku **Uruchomienia działania**, wybierając link **Potoki**. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>Wyzwalanie potoku zgodnie z harmonogramem
W tym samouczku ta procedura jest opcjonalna. Możesz utworzyć *wyzwalacz harmonogramu*, aby zaplanować okresowe uruchamianie potoku (co godzinę, codziennie itd.). Podczas tej procedury utworzysz wyzwalacz, który będzie uruchamiany co minutę, aż do daty/godziny określonej jako data zakończenia. 

1. Przejdź do karty **Tworzenie**. 

1. Przejdź do potoku, wybierz opcję **Wyzwalacz** na pasku narzędzi potoku, a następnie wybierz pozycję **Nowy/Edytuj**. 

1. Na stronie **Dodawanie wyzwalaczy** wybierz pozycję **Wybierz wyzwalacz**, a następnie wybierz przycisk **Nowy**. 

1. Na stronie **Nowy wyzwalacz** w polu **Koniec** wybierz pozycję **W dniu**, określ czas zakończenia jako kilka minut późniejszy niż czas bieżący, a następnie wybierz pozycję **Zastosuj**. 

   Za poszczególne uruchomienia potoku są naliczane opłaty, zatem określ czas zakończenia jako późniejszy tylko o kilka minut od czasu rozpoczęcia. Upewnij się, że przypada on tego samego dnia. Upewnij się również, że okres między czasem publikowania i czasem zakończenia będzie wystarczający do uruchomienia potoku. Wyzwalacz zaczyna obowiązywać dopiero po opublikowaniu rozwiązania w fabryce Data Factory, a nie po zapisaniu go w interfejsie użytkownika. 

   ![Ustawienia wyzwalacza](./media/quickstart-create-data-factory-portal/trigger-settings.png)
1. Na stronie**Nowy wyzwalacz** zaznacz pole wyboru **Aktywowano**, a następnie wybierz przycisk **Dalej**. 

   ![Pole wyboru „Aktywowano” i przycisk „Dalej”](./media/quickstart-create-data-factory-portal/trigger-settings-next.png)
1. Zapoznaj się z komunikatem ostrzegawczym, a następnie wybierz przycisk **Zakończ**.

   ![Komunikat ostrzegawczy i przycisk „Zakończ”](./media/quickstart-create-data-factory-portal/new-trigger-finish.png)
1. Wybierz pozycję **Opublikuj wszystkie**, aby opublikować zmiany w usłudze Data Factory. 

1. Przejdź do karty **Monitorowanie** po lewej stronie. Wybierz pozycję **Odśwież**, aby odświeżyć listę. Potok będzie uruchamiany raz na minutę od czasu opublikowania do czasu zakończenia. 

   Zwróć uwagę na wartości w kolumnie **Wyzwolone przez**. Ręczne uruchomienie wyzwalacza pochodzi z kroku wykonanego wcześniej (**Wyzwól teraz**). 

   ![Lista wyzwolonych uruchomień](./media/quickstart-create-data-factory-portal/monitor-triggered-runs.png)
1. Przełącz się do widoku **Uruchomienia wyzwalacza**. 

   ![Przełączanie do widoku „Uruchomienia wyzwalacza”](./media/quickstart-create-data-factory-portal/monitor-trigger-runs.png)    
1. Sprawdź, czy plik wyjściowy jest tworzony w folderze **output** dla każdego uruchomienia potoku aż do określonej daty/godziny zakończenia. 

## <a name="next-steps"></a>Kolejne kroki
Potok w tym przykładzie kopiuje dane z jednej lokalizacji do innej lokalizacji w usłudze Azure Blob Storage. Zapoznaj się z [samouczkami](tutorial-copy-data-portal.md), aby dowiedzieć się więcej o korzystaniu z usługi Data Factory w dalszych scenariuszach. 