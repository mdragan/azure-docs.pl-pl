---
title: Jak wykrywać Dryft danych (wersja zapoznawcza) we wdrożeniach usługi AKS
titleSuffix: Azure Machine Learning service
description: Dowiedz się, jak wykrywać Dryft danych w usłudze Azure Kubernetes Service wdrożone modelami w usłudze Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: copeters
author: cody-dkdc
ms.date: 06/20/2019
ms.openlocfilehash: e4deeab28fb643ff32624ba9dd16574e621f508c
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332505"
---
# <a name="how-to-detect-data-drift-preview-on-models-deployed-to-azure-kubernetes-service"></a>Jak wykrywać Dryft danych (wersja zapoznawcza) w modelach wdrożony w usłudze Azure Kubernetes Service
W tym artykule opisano sposób monitorowania [dryfu danych](concept-data-drift.md) między danymi zestawu danych i wnioskowania szkolenia wdrożony model. 

Przesunięcie danych jest jednym z głównych powodów gdzie spadku dokładności modelu wraz z upływem czasu. Zdarza się, gdy obsługiwany w środowisku produkcyjnym do modelu danych jest inny niż danych użytych do nauczenia modelu. Usługa Azure Machine Learning można monitorować dryfu danych przy użyciu danych detektora odejściem od tego stanu. W przypadku wykrycia dryfu usługa może wysłać alert użytkownikom.  

> [!Note]
> Ta usługa jest w (wersja zapoznawcza) i jest ograniczona w opcji konfiguracji. Zobacz nasze [dokumentacji interfejsu API](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/?view=azure-ml-py) i [wersji](azure-machine-learning-release-notes.md) szczegółowe informacje i aktualizacje. 

Za pomocą usługi Azure Machine Learning możesz monitorować dane wejściowe do modelu, który został wdrożony w usłudze AKS i porównaj te dane do zestawu danych szkoleniowych dla modelu. W regularnych odstępach czasu, to dane wnioskowania [migawki i profilowane](how-to-explore-prepare-data.md), następnie obliczonego w odniesieniu do zestawu danych linii bazowej do tworzenia analizy dryfu danych który: 

+ Mierzy wielkość dryfu danych, nazywane współczynnik odejściem od tego stanu.
+ Środki danych odstępstw materiałów przekazywanych przez funkcję i informacją o tym, funkcji, które spowodowało dryfu danych.
+ Środki na odległość metryki. Obecnie są obliczane Wasserstein i odległość energii.
+ Mierzy dystrybucje funkcji. Obecnie jądra gęstość szacowania i histogramów.
+ Wysyłanie alertów danych odstępstw za pośrednictwem poczty e-mail.

Aby uzyskać szczegółowe informacje dotyczące sposobu te metryki są obliczane, zobacz [danych kilka koncepcji](concept-data-drift.md) artykułu.

## <a name="prerequisites"></a>Wymagania wstępne

- Jeśli nie masz subskrypcji Azure, przed rozpoczęciem utwórz bezpłatne konto. Wypróbuj [bezpłatną lub płatną wersję usługi Azure Machine Learning](https://aka.ms/AMLFree) już dziś.

- Obszar roboczy usługi Azure Machine Learning i Azure Machine Learning SDK dla język Python jest zainstalowany. Dowiedz się, jak uzyskać te wymagania wstępne przy użyciu [sposób konfigurowania środowiska deweloperskiego](how-to-configure-environment.md) dokumentu.

- [Konfigurowanie środowiska](how-to-configure-environment.md), a następnie zainstaluj dryfu danych zestawu SDK przy użyciu następującego polecenia:

    ```
    pip install azureml-contrib-datadrift
    ```

- Tworzenie [dataset](how-to-create-register-datasets.md) z modelu danych szkoleniowych.

- Określ zestaw danych szkoleniowych podczas [rejestrowanie](concept-model-management-and-deployment.md) modelu. Poniższy przykład demonstruje użycie `datasets` parametru, aby określić zestaw danych szkoleniowych:

    ```python
    model = Model.register(model_path=model_file,
                        model_name=model_name,
                        workspace=ws,
                        datasets=datasets)

    print(model_name, image_name, service_name, model)
    ```

- Konfigurowanie [modułów zbierających dane modelu](how-to-enable-data-collection.md) do zbierania danych z modelu wdrożenia usługi AKS i upewnij się, dane są zbierane w `modeldata` kontenera obiektów blob.

## <a name="import-dependencies"></a>Importowanie zależności 
Zaimportuj zależności używanych w tym przewodniku:

```python
# Azure ML service packages 
from azureml.core import Experiment, Run, RunDetails
from azureml.contrib.datadrift import DataDriftDetector, AlertConfiguration
``` 

## <a name="configure-data-drift"></a>Konfigurowanie danych odejściem od tego stanu 

W poniższym przykładzie Python pokazuje Konfigurowanie `DataDriftDetector` obiektu:

```python
# if email address is specified, setup AlertConfiguration
alert_config = AlertConfiguration('your_email@contoso.com')

# create a new DatadriftDetector object
datadrift = DataDriftDetector.create(ws, model.name, model.version, services, frequency="Day", alert_config=alert_config)
    
print('Details of Datadrift Object:\n{}'.format(datadrift))
```

Aby uzyskać więcej informacji, zobacz [DataDrift](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/?view=azure-ml-py) odwołania.

## <a name="submit-a-datadriftdetector-run"></a>Prześlij Uruchom DataDriftDetector

Za pomocą DataDriftDetector skonfigurowane, możesz przesłać [dryfu danych uruchom](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector%28class%29?view=azure-ml-py#run-target-date--services--compute-target-name-none--create-compute-target-false--feature-list-none--drift-threshold-none-) w danym dniu dla modelu. 

```python
# adhoc run today
target_date = datetime.today()

# create a new compute - creates datadrift-server
run = datadrift.run(target_date, services, feature_list=feature_list, create_compute_target=True)

# or specify existing compute cluster
run = datadrift.run(target_date, services, feature_list=feature_list, compute_target='cpu-cluster')

# show details of the data drift run
exp = Experiment(ws, datadrift._id)
dd_run = Run(experiment=exp, run_id=run)
RunDetails(dd_run).show()
```

## <a name="get-data-drift-analysis-results"></a>Pobierz wyniki analizy danych odejściem od tego stanu

W poniższym przykładzie Python pokazuje, jak do wykreślenia metryki dryfu odpowiednie dane. Zwracane metryki służy do tworzenia niestandardowych wizualizacji:

```python
# start and end are datetime objects 
drift_metrics = datadrift.get_output(start_time=start, end_time=end)

# Show all data drift result figures, one per serivice.
# If setting with_details is False (by default), only the data drift magnitude will be shown; if it's True, all details will be shown.
drift_figures = datadrift.show(with_details=True)
```

![Pokaż dryfu danych](media/how-to-monitor-data-drift/drift_show.png)

Aby uzyskać szczegółowe informacje dotyczące metryk, które są obliczane, zobacz [danych kilka koncepcji](concept-data-drift.md) artykułu.

## <a name="schedule-data-drift-detection"></a>Wykrywanie dryfu danych harmonogramu 

Włączanie harmonogramu dryfu danych wykonuje DataDriftDetector, uruchom określoną częstotliwością. Jeśli współczynnik odejściem od tego stanu jest powyżej wartości progowej przez dany, zostanie wysłana wiadomość e-mail. 

```python
datadrift.enable_schedule()
datadrift.disable_schedule()
```

Konfigurację wykrywacz dryfu danych są widoczne na stronie szczegółów modelu w witrynie Azure portal.

![Dane Dryft konfiguracji witryny Azure portal](media/how-to-monitor-data-drift/drift_config.png)

## <a name="view-results-in-azure-ml-workspace-ui"></a>Wyświetl wyniki w interfejsie użytkownika z obszaru roboczego uczenia Maszynowego Azure

Aby wyświetlić wyniki w Interfejsie użytkownika usługi Azure ML obszaru roboczego, przejdź do strony modelu. Na karcie Szczegóły modelu danych Dryft konfiguracji jest wyświetlany. Na karcie "Dryfu danych (wersja zapoznawcza)" jest teraz dostępna, wizualizowanie metryk dryfu danych. 

![Przesunięcie danych o witrynie Azure portal](media/how-to-monitor-data-drift/drift_ui.png)

## <a name="setting-up-alerts"></a>Konfigurowanie alertów 

Przez ustawienie współczynnika dryfu próg alertów i podając adres e-mail [usługi Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) wiadomość e-mail jest wysyłana, gdy współczynnik odejściem od tego stanu jest powyżej wartości progowej. Wszystkie metryki dryfu dane są przechowywane w zasób insights aplikacji, które są skojarzone z obszarem roboczym usługi Azure Machine Learning można ustawić niestandardowe alerty lub akcji. Link w alercie e-mail można wykonać, aby zapytanie usługi app insights.

![Alert E-mail dryfu danych](media/how-to-monitor-data-drift/drift_email.png)

## <a name="next-steps"></a>Kolejne kroki

* Pełny przykład przy użyciu danych odejściem od tego stanu można wyświetlić [danych usługi Azure ML odstępstw notesu](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/data-drift/azure-ml-datadrift.ipynb). Ten notes Jupyter, który demonstruje sposób użycia [Otwórz zestaw danych usługi Azure](https://docs.microsoft.com/azure/open-datasets/overview-what-are-open-datasets) do nauczenia modelu do prognozowania pogody, wdrażanie w usłudze AKS i monitorować dane odejściem od tego stanu. 

* Znacznie prosimy o pytania, komentarze lub sugestie przemieszcza się dryfu danych kierunku ogólnej dostępności. Użyj przycisku opinii produktu poniżej! 
