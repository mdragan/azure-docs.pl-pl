---
title: Tworzenie maszyny platformy Apache Spark uczenia potok — Azure HDInsight
description: Biblioteka usługi Apache Spark machine learning służy do tworzenia potoków danych.
ms.service: hdinsight
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/19/2018
ms.openlocfilehash: c539460177a0a85938b886d161803e1fdf0e9e68
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64730200"
---
# <a name="create-an-apache-spark-machine-learning-pipeline"></a>Tworzenie potoku uczenia maszynowego platformy Apache Spark

Biblioteka uczenia skalowalne maszyny platformy Apache Spark (Biblioteka MLlib) zapewnia funkcje modelowania w środowisku rozproszonym. Pakiet platformy Spark [ `spark.ml` ](https://spark.apache.org/docs/latest/ml-pipeline.html) to zbiór interfejsów API wysokiego poziomu, oparta na elementy Dataframe. Te interfejsy API ułatwiają tworzenie i dostosowywanie praktyczne potokach uczenia maszynowego.  *Platforma Spark jest uczenie maszynowe* odwołuje się do tego na podstawie MLlib ramkę danych interfejsu API, nie starsza na podstawie RDD potoku interfejsu API.

Usługi machine learning (ML) potoku jest kompletny przepływ pracy, łącząc wiele algorytmów były ze sobą uczenia maszynowego. Może istnieć wiele kroków wymaganych do przetwarzania i ucz się od danych, wymagających sekwencji algorytmów. Potoki definiować etapy i kolejność procesu uczenia maszynowego. W MLlib etapach potoku są reprezentowane przez określonej kolejności PipelineStages, gdzie transformatora, jak i narzędzie do szacowania wykonywać zadania.

Transformer jest algorytm, który przekształca jeden ramkę danych do innego za pomocą `transform()` metody. Na przykład transformatora funkcji można odczytać jedną kolumnę ramkę danych, dokonaj mapowania go do innej kolumny i wyjściowe nowej ramki danych z kolumną zamapowanego dołączone do niego.

Narzędzie do szacowania to Abstrakcja algorytmów uczenia i jest odpowiedzialny za dopasowanie lub szkolenia w zestawie danych do wygenerowania transformatora. Narzędzie do szacowania implementuje metodę o nazwie `fit()`, który akceptuje ramkę danych i tworzy ramkę danych, czyli przekształcania.

Każde wystąpienie bezstanowe transformatora lub narzędzie do szacowania ma swój własny unikatowy identyfikator, który jest używany podczas określania parametrów. Zarówno na użytek interfejsem API spójnym określania tych parametrów.

## <a name="pipeline-example"></a>Przykładowy potok

Aby zademonstrować praktyczne wykorzystanie potoku uczenia Maszynowego, w tym przykładzie użyto przykładu `HVAC.csv` pliku danych, który jest wstępnie załadowane na domyślny magazyn dla klastra HDInsight, Azure Storage lub magazynu usługi Data Lake. Aby wyświetlić zawartość pliku, przejdź do `/HdiSamples/HdiSamples/SensorSampleData/hvac` katalogu. `HVAC.csv` zawiera zestaw uzyskane przy użyciu docelowej i rzeczywiste temperatury HVAC (*grzejników, wentylatorów i klimatyzacja*) systemów w różnych budynkach. Celem jest uczenie modelu danych i generuje prognozy temperatury dla danego kompilowania.

Poniższy kod:

1. Definiuje `LabeledDocument`, które sklepy `BuildingID`, `SystemInfo` systemu identyfikator i wiek, a `label` (1.0, jeśli kompilacja jest zbyt gorąca 0.0, w przeciwnym razie).
2. Tworzy funkcję analizatora niestandardowego pozwalającego `parseDocument` , pobiera wiersz danych (wiersz) i określa, czy kompilacja jest "gorącymi" przez porównanie temperatury docelowej do rzeczywistego temperatury.
3. Stosuje się analizator składni podczas wyodrębniania danych źródłowych.
4. Tworzy dane szkoleniowe.

```python
from pyspark.ml import Pipeline
from pyspark.ml.classification import LogisticRegression
from pyspark.ml.feature import HashingTF, Tokenizer
from pyspark.sql import Row

# The data structure (column meanings) of the data array:
# 0 Date
# 1 Time
# 2 TargetTemp
# 3 ActualTemp
# 4 System
# 5 SystemAge
# 6 BuildingID

LabeledDocument = Row("BuildingID", "SystemInfo", "label")

# Define a function that parses the raw CSV file and returns an object of type LabeledDocument
def parseDocument(line):
    values = [str(x) for x in line.split(',')]
    if (values[3] > values[2]):
        hot = 1.0
    else:
        hot = 0.0        

    textValue = str(values[4]) + " " + str(values[5])

    return LabeledDocument((values[6]), textValue, hot)

# Load the raw HVAC.csv file, parse it using the function
data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
training = documents.toDF()
```

Ten przykładowy potok ma trzy etapy: `Tokenizer` i `HashingTF` (zarówno transformatory) i `Logistic Regression` (narzędzie do szacowania).  Dane wyodrębnione i przeanalizowane `training` ramkę danych przechodzą przez potok podczas `pipeline.fit(training)` jest wywoływana.

1. Pierwszy etap, `Tokenizer`, dzieli `SystemInfo` kolumny wejściowej (składający się z wartości identyfikatora i wieku systemu) do `words` kolumny wyjściowej. Ta nowa `words` kolumna zostanie dodana do ramki danych. 
2. Drugi etap `HashingTF`, konwertuje nowy `words` kolumny na wektorów funkcji. Ta nowa `features` kolumna zostanie dodana do ramki danych. Te dwa pierwsze etapów transformatory. 
3. Trzeci etap `LogisticRegression`, to narzędzie do szacowania i dlatego potok wywołuje `LogisticRegression.fit()` metody do tworzenia `LogisticRegressionModel`. 

```python
tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
lr = LogisticRegression(maxIter=10, regParam=0.01)

# Build the pipeline with our tokenizer, hashingTF, and logistic regression stages
pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

model = pipeline.fit(training)
```

Aby zobaczyć nowe `words` i `features` kolumnami dodanymi za pomocą `Tokenizer` i `HashingTF` transformatory i przykładowym `LogisticRegression` narzędzie do szacowania, uruchom `PipelineModel.transform()` metody w oryginalnym ramki danych. W kodzie produkcyjnym następnym krokiem powinno być do przekazania w teście ramkę danych, aby sprawdzić poprawność szkolenia.

```python
peek = model.transform(training)
peek.show()

# Outputs the following:
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+
|BuildingID|SystemInfo|label|   words|            features|       rawPrediction|         probability|prediction|
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+
|         4|     13 20|  0.0|[13, 20]|(262144,[250802,2...|[0.11943986671420...|[0.52982451901740...|       0.0|
|        17|      3 20|  0.0| [3, 20]|(262144,[89074,25...|[0.17511205617446...|[0.54366648775222...|       0.0|
|        18|     17 20|  1.0|[17, 20]|(262144,[64358,25...|[0.14620993833623...|[0.53648750722548...|       0.0|
|        15|      2 23|  0.0| [2, 23]|(262144,[31351,21...|[-0.0361327091023...|[0.49096780538523...|       1.0|
|         3|      16 9|  1.0| [16, 9]|(262144,[153779,1...|[-0.0853679939336...|[0.47867095324139...|       1.0|
|         4|     13 28|  0.0|[13, 28]|(262144,[69821,25...|[0.14630166986618...|[0.53651031790592...|       0.0|
|         2|     12 24|  0.0|[12, 24]|(262144,[187043,2...|[-0.0509556393066...|[0.48726384581522...|       1.0|
|        16|     20 26|  1.0|[20, 26]|(262144,[128319,2...|[0.33829638728900...|[0.58377663577684...|       0.0|
|         9|      16 9|  1.0| [16, 9]|(262144,[153779,1...|[-0.0853679939336...|[0.47867095324139...|       1.0|
|        12|       6 5|  0.0|  [6, 5]|(262144,[18659,89...|[0.07513008136562...|[0.51877369045183...|       0.0|
|        15|     10 17|  1.0|[10, 17]|(262144,[64358,25...|[-0.0291988646553...|[0.49270080242078...|       1.0|
|         7|      2 11|  0.0| [2, 11]|(262144,[212053,2...|[0.03678030020834...|[0.50919403860812...|       0.0|
|        15|      14 2|  1.0| [14, 2]|(262144,[109681,2...|[0.06216423725633...|[0.51553605651806...|       0.0|
|         6|       3 2|  0.0|  [3, 2]|(262144,[89074,21...|[0.00565582077537...|[0.50141395142468...|       0.0|
|        20|     19 22|  0.0|[19, 22]|(262144,[139093,2...|[-0.0769288695989...|[0.48077726176073...|       1.0|
|         8|     19 11|  0.0|[19, 11]|(262144,[139093,2...|[0.04988910033929...|[0.51246968885151...|       0.0|
|         6|      15 7|  0.0| [15, 7]|(262144,[77099,20...|[0.14854929135994...|[0.53706918109610...|       0.0|
|        13|      12 5|  0.0| [12, 5]|(262144,[89689,25...|[-0.0519932532562...|[0.48700461408785...|       1.0|
|         4|      8 22|  0.0| [8, 22]|(262144,[98962,21...|[-0.0120753606650...|[0.49698119651572...|       1.0|
|         7|      17 5|  0.0| [17, 5]|(262144,[64358,89...|[-0.0721054054871...|[0.48198145477106...|       1.0|
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+

only showing top 20 rows
```

`model` Obiektu może teraz służyć do przewidywania przyszłych zdarzeń. Aby uzyskać pełny przykład tej aplikacji i instrukcje krok po kroku dotyczące uruchamiania go uczenia maszynowego, zobacz [kompilacji platformy Apache Spark usługi machine learning aplikacji w usłudze Azure HDInsight](apache-spark-ipython-notebook-machine-learning.md).

## <a name="see-also"></a>Zobacz także

* [Analiza danych przy użyciu języka Scala i platformy Apache Spark na platformie Azure](../../machine-learning/team-data-science-process/scala-walkthrough.md)
