---
title: Szybki start na platformie Azure — uruchamianie zadania usługi Batch — Python
description: Szybko uruchamiaj zadania i zadania podrzędne usługi Batch za pomocą biblioteki klienta Python usługi Batch.
services: batch
author: laurenhughes
manager: jeconnoc
ms.service: batch
ms.devlang: python
ms.topic: quickstart
ms.date: 11/27/2018
ms.author: lahugh
ms.custom: mvc
ms.openlocfilehash: a5218498b19fe99f810f2dfca8e044c6f9398a94
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341161"
---
# <a name="quickstart-run-your-first-batch-job-with-the-python-api"></a>Szybki start: uruchamianie pierwszego zadania usługi Batch za pomocą interfejsu API języka Python

Ten przewodnik Szybki start przedstawia uruchamianie zadania usługi Azure Batch z poziomu aplikacji utworzonej w oparciu o interfejs API języka Python usługi Azure Batch. Aplikacja przekazuje kilka plików danych wejściowych do usługi Azure Storage oraz tworzy *pulę* węzłów obliczeniowych (maszyn wirtualnych) usługi Batch. Następnie aplikacja tworzy przykładowe *zadanie*, które uruchamia *zadania podrzędne* w celu przetworzenia każdego pliku wejściowego w puli przy użyciu podstawowego polecenia. Po ukończeniu tego przewodnika Szybki start będziesz rozumieć kluczowe pojęcia związane z usługą Batch, co pozwoli na wypróbowanie tej usługi z bardziej realistycznymi obciążeniami na większą skalę.
 
![Szybki start — przepływ pracy aplikacji](./media/quick-run-python/sampleapp.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Wymagania wstępne

* [Środowisko Python w wersji 2.7 lub 3.3 albo nowszej](https://www.python.org/downloads/)

* Menedżer pakietów [pip](https://pip.pypa.io/en/stable/installing/)

* Konto usługi Azure Batch i połączone konto usługi Azure Storage. Aby utworzyć te konta, skorzystaj z przewodników Szybki start dla usługi Batch i [witryny Azure Portal](quick-create-portal.md) lub [interfejsu wiersza polecenia platformy Azure](quick-create-cli.md). 

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się do witryny Azure Portal pod adresem [https://portal.azure.com](https://portal.azure.com).

[!INCLUDE [batch-common-credentials](../../includes/batch-common-credentials.md)]

## <a name="download-the-sample"></a>Pobierz przykład

[Pobierz lub sklonuj przykładową aplikację](https://github.com/Azure-Samples/batch-python-quickstart) z usługi GitHub. Aby sklonować repozytorium przykładowej aplikacji za pomocą klienta Git, użyj następującego polecenia:

```
git clone https://github.com/Azure-Samples/batch-python-quickstart.git
```

Przejdź do katalogu, który zawiera skrypt w języku Python `python_quickstart_client.py`.

W środowisku projektowym Python zainstaluj wymagane pakiety przy użyciu menedżera `pip`.

```bash
pip install -r requirements.txt
```

Otwórz plik `config.py`. Zaktualizuj ciągi poświadczeń konta usługi Batch i konta magazynu, podając wartości uzyskane dla Twoich kont. Na przykład:

```Python
_BATCH_ACCOUNT_NAME = 'mybatchaccount'
_BATCH_ACCOUNT_KEY = 'xxxxxxxxxxxxxxxxE+yXrRvJAqT9BlXwwo1CwF+SwAYOxxxxxxxxxxxxxxxx43pXi/gdiATkvbpLRl3x14pcEQ=='
_BATCH_ACCOUNT_URL = 'https://mybatchaccount.mybatchregion.batch.azure.com'
_STORAGE_ACCOUNT_NAME = 'mystorageaccount'
_STORAGE_ACCOUNT_KEY = 'xxxxxxxxxxxxxxxxy4/xxxxxxxxxxxxxxxxfwpbIC5aAWA8wDu+AFXZB827Mt9lybZB1nUcQbQiUrkPtilK5BQ=='
```

## <a name="run-the-app"></a>Uruchamianie aplikacji

Aby zobaczyć przepływ pracy usługi Batch w akcji, uruchom skrypt:

```
python python_quickstart_client.py
```

Po uruchomieniu skryptu przejrzyj kod, aby poznać działanie poszczególnych części aplikacji. 

Po uruchomieniu aplikacji przykładowej dane wyjściowe w konsoli będą wyglądać mniej więcej następująco. W czasie wykonywania nastąpi wstrzymanie operacji w momencie wyświetlenia komunikatu `Monitoring all tasks for 'Completed' state, timeout in 00:30:00...` podczas uruchamiania węzłów obliczeniowych puli. Zadania podrzędne zostaną umieszczone w kolejce do uruchomienia zaraz po uruchomieniu pierwszego węzła obliczeniowego. Przejdź do konta usługi Batch w witrynie [Azure Portal](https://portal.azure.com), aby monitorować pulę, węzły obliczeniowe, zadanie i zadania podrzędne w ramach konta usługi Batch.

```
Sample start: 11/26/2018 4:02:54 PM

Container [input] created.
Uploading file taskdata0.txt to container [input]...
Uploading file taskdata1.txt to container [input]...
Uploading file taskdata2.txt to container [input]...
Creating pool [PythonQuickstartPool]...
Creating job [PythonQuickstartJob]...
Adding 3 tasks to job [PythonQuickstartJob]...
Monitoring all tasks for 'Completed' state, timeout in 00:30:00...
```

Po zakończeniu zadań podrzędnych dla każdego z nich zostaną wyświetlone dane wyjściowe podobne do następujących:

```
Printing task output...
Task: Task0
Node: tvm-2850684224_3-20171205t000401z
Standard out:
Batch processing began with mainframe computers and punch cards. Today it still plays a central role in business, engineering, science, and other pursuits that require running lots of automated tasks....
...
```

Typowy czas wykonywania wynosi mniej więcej 3 minuty w przypadku uruchomienia aplikacji w konfiguracji domyślnej. Początkowa konfiguracja puli zajmuje najwięcej czasu.

## <a name="review-the-code"></a>Przeglądanie kodu

Aplikacja w języku Python omawiana w tym przewodniku Szybki start wykonuje następujące akcje:

* Przekazuje trzy małe pliki tekstowe do kontenera obiektów blob na koncie usługi Azure Storage. Te pliki to dane wejściowe do przetworzenia przez zadania podrzędne usługi Batch.
* Tworzy pulę dwóch węzłów obliczeniowych z systemem Ubuntu 18.04 LTS.
* Tworzy zadanie i trzy zadania podrzędne do uruchomienia w tych węzłach. Każde zadanie podrzędne przetwarza jeden z plików wejściowych przy użyciu wiersza polecenia powłoki Bash.
* Wyświetla pliki zwrócone przez zadania podrzędne.

Aby uzyskać szczegółowe informacje, zapoznaj się z plikiem `python_quickstart_client.py` i poniższymi sekcjami.

### <a name="preliminaries"></a>Akcje wstępne

Aby nawiązać interakcję z kontem magazynu, aplikacja używa pakietu [azure-storage-blob](https://pypi.python.org/pypi/azure-storage-blob) do utworzenia obiektu [BlockBlobService](/python/api/azure.storage.blob.blockblobservice.blockblobservice).

```python
blob_client = azureblob.BlockBlobService(
    account_name=config._STORAGE_ACCOUNT_NAME,
    account_key=config._STORAGE_ACCOUNT_KEY)
```

Za pomocą odwołania `blob_client` aplikacja tworzy kontener w ramach konta magazynu i przekazuje pliki danych do kontenera. Pliki w magazynie są definiowane jako obiekty [ResourceFile](/python/api/azure.batch.models.resourcefile) usługi Batch, które następnie mogą być pobierane przez tę usługę do węzłów obliczeniowych.

```python
input_file_paths = [os.path.join(sys.path[0], 'taskdata0.txt'),
                    os.path.join(sys.path[0], 'taskdata1.txt'),
                    os.path.join(sys.path[0], 'taskdata2.txt')]

input_files = [
    upload_file_to_container(blob_client, input_container_name, file_path)
    for file_path in input_file_paths]
```

Aplikacja tworzy obiekt [BatchServiceClient](/python/api/azure.batch.batchserviceclient) na potrzeby tworzenia pul, zadań i zadań podrzędnych w usłudze Batch oraz zarządzania nimi. Klient usługi Batch w przykładzie korzysta z uwierzytelniania za pomocą klucza wspólnego. Usługa Batch obsługuje też uwierzytelnianie za pomocą usługi Azure Active Directory.

```python
credentials = batch_auth.SharedKeyCredentials(config._BATCH_ACCOUNT_NAME,
                                              config._BATCH_ACCOUNT_KEY)

batch_client = batch.BatchServiceClient(
    credentials,
    batch_url=config._BATCH_ACCOUNT_URL)
```

### <a name="create-a-pool-of-compute-nodes"></a>Tworzenie puli węzłów obliczeniowych

Aby utworzyć pulę usługi Batch, aplikacja ustawia liczbę węzłów, rozmiar maszyny wirtualnej i konfigurację puli za pomocą klasy [PoolAddParameter](/python/api/azure.batch.models.pooladdparameter). W tym przypadku obiekt [VirtualMachineConfiguration](/python/api/azure.batch.models.virtualmachineconfiguration) określa parametr [ImageReference](/python/api/azure.batch.models.imagereference) z odwołaniem do obrazu systemu Ubuntu Server 18.04 LTS opublikowanego w witrynie Azure Marketplace. Usługa Batch obsługuje szeroki zakres obrazów systemów Linux i Windows Server pochodzących z witryny Azure Marketplace, a także niestandardowe obrazy maszyn wirtualnych.

Liczba węzłów (`_POOL_NODE_COUNT`) i rozmiar maszyny wirtualnej (`_POOL_VM_SIZE`) to zdefiniowane stałe. Domyślnie przykładowa aplikacja tworzy pulę składającą się z 2 węzłów o rozmiarze *Standard_A1_v2*. Sugerowany rozmiar oferuje dobry kompromis między wydajnością a kosztem na potrzeby tego krótkiego przykładu.

Metoda [pool.add](/python/api/azure.batch.operations.pooloperations) przesyła pulę do usługi Batch.

```python
new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=batchmodels.ImageReference(
            publisher="Canonical",
            offer="UbuntuServer",
            sku="18.04-LTS",
            version="latest"
        ),
        node_agent_sku_id="batch.node.ubuntu 18.04"),
    vm_size=config._POOL_VM_SIZE,
    target_dedicated_nodes=config._POOL_NODE_COUNT
)
batch_service_client.pool.add(new_pool)
```

### <a name="create-a-batch-job"></a>Tworzenie zadania usługi Batch

Zadanie usługi Batch to logiczna grupa zawierająca co najmniej jedno zadanie podrzędne. Zadanie uwzględnia wspólne ustawienia zadań podrzędnych, takie jak priorytet i pula, w której zadania podrzędne mają być uruchamiane. Aplikacja tworzy zadanie w puli za pomocą klasy [JobAddParameter](/python/api/azure.batch.models.jobaddparameter). Metoda [job.add](/python/api/azure.batch.operations.joboperations) przesyła pulę do usługi Batch. Początkowo zadanie nie zawiera zadań podrzędnych.

```python
job = batch.models.JobAddParameter(
    id=job_id,
    pool_info=batch.models.PoolInformation(pool_id=pool_id))
batch_service_client.job.add(job)
```

### <a name="create-tasks"></a>Tworzenie zadań podrzędnych

Aplikacja tworzy listę obiektów zadań podrzędnych przy użyciu klasy [TaskAddParameter](/python/api/azure.batch.models.taskaddparameter). Każde zadanie podrzędne przetwarza wejściowy obiekt `resource_files` przy użyciu parametru `command_line`. W tym przykładzie wiersz polecenia uruchamia polecenie `cat` powłoki Bash, aby wyświetlić plik tekstowy. To polecenie to prosty przykład dla celów demonstracyjnych. Podczas korzystania z usługi Batch aplikację lub skrypt określa się w wierszu polecenia. Usługa Batch udostępnia kilka sposobów wdrażania aplikacji i skryptów w węzłach obliczeniowych.

Następnie aplikacja dodaje zadania podrzędne do zadania za pomocą metody [task.add_collection](/python/api/azure.batch.operations.taskoperations), która tworzy kolejkę zadań podrzędnych do uruchomienia w węzłach obliczeniowych. 

```python
tasks = list()

for idx, input_file in enumerate(input_files):
    command = "/bin/bash -c \"cat {}\"".format(input_file.file_path)
    tasks.append(batch.models.TaskAddParameter(
        id='Task{}'.format(idx),
        command_line=command,
        resource_files=[input_file]
    )
    )
batch_service_client.task.add_collection(job_id, tasks)
```

### <a name="view-task-output"></a>Wyświetlanie danych wyjściowych zadania podrzędnego

Aplikacja monitoruje stan zadań podrzędnych, aby upewnić się, że zostaną one ukończone. Następnie aplikacja wyświetla plik `stdout.txt` generowany przez każde ukończone zadanie podrzędne. Po pomyślnym uruchomieniu zadania podrzędnego dane wyjściowe polecenia tego zadania zostaną zapisane w pliku `stdout.txt`:

```python
tasks = batch_service_client.task.list(job_id)

for task in tasks:

    node_id = batch_service_client.task.get(job_id, task.id).node_info.node_id
    print("Task: {}".format(task.id))
    print("Node: {}".format(node_id))

    stream = batch_service_client.file.get_from_task(
        job_id, task.id, config._STANDARD_OUT_FILE_NAME)

    file_text = _read_stream_as_string(
        stream,
        encoding)
    print("Standard output:")
    print(file_text)
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Aplikacja automatycznie usuwa utworzony kontener magazynu, a także udostępnia opcję usunięcia puli i zadania usługi Batch. Opłaty za pulę są naliczane, dopóki węzły działają, nawet jeśli nie zostały zaplanowane żadne zadania. Gdy pula nie jest już potrzebna, usuń ją. W przypadku usunięcia puli usuwane są również wszystkie dane wyjściowe zadań podrzędnych w węzłach. 

Gdy grupa zasobów, konto usługi Batch i konto magazynu nie będą już potrzebne, usuń je. W tym celu w witrynie Azure Portal zaznacz grupę zasobów konta usługi Batch i kliknij pozycję **Usuń grupę zasobów**.

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start uruchomiono niewielką aplikację utworzoną za pomocą interfejsu API języka Python usługi Batch w celu utworzenia puli i zadania usługi Batch. Zadanie uruchomiło przykładowe zadania podrzędne i pobrało dane wyjściowe utworzone w węzłach. Teraz, gdy już rozumiesz kluczowe pojęcia związane z usługą Batch, możesz wypróbować tę usługę z bardziej realistycznymi obciążeniami na większą skalę. Aby dowiedzieć się więcej o usłudze Azure Batch i zapoznać się z przewodnikiem dotyczącym obciążenia równoległego o rzeczywistym zastosowaniu, kontynuuj naukę w ramach samouczka języka Python usługi Batch.

> [!div class="nextstepaction"]
> [Przetwarzanie obciążenia równoległego za pomocą języka Python](tutorial-parallel-python.md)
