---
title: 'Szybki start: Tworzenie projektu wykrywania obiektów przy użyciu zestawu Custom Vision SDK dla języka Go'
titlesuffix: Azure Cognitive Services
description: Utwórz projekt, dodaj tagi, przekaż obrazy, wytrenuj projekt i wykrywaj obiekty przy użyciu zestawu Go SDK.
services: cognitive-services
author: areddish
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: daauld
ms.openlocfilehash: 77ba3144afcc48d68466341c154bc1d8eef54d3b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60609021"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-go-sdk"></a>Szybki start: Tworzenie projektu wykrywania obiektów przy użyciu zestawu Custom Vision SDK dla języka Go

Ten artykuł zawiera informacje i przykładowy kod, dzięki którym można łatwiej rozpocząć tworzenie modeli wykrywania obiektów za pomocą zestawu Custom Vision SDK i języka Go. Po jego utworzeniu można można dodać oznakowane regionów, przekazywać obrazy, szkolenie projektu, uzyskać adres URL punktu końcowego opublikowanej prognozowania projektu i używać punktu końcowego programowo testować obrazu. Użyj tego przykładu jako szablonu do utworzenia własnej aplikacji w języku Go.

## <a name="prerequisites"></a>Wymagania wstępne

- [Go 1.8+](https://golang.org/doc/install)

## <a name="install-the-custom-vision-sdk"></a>Instalowanie zestawu Custom Vision SDK

Aby zainstalować zestaw Custom Vision Service SDK dla języka Go, uruchom następujące polecenie w programie PowerShell:

```
go get -u github.com/Azure/azure-sdk-for-go/...
```

a jeśli używasz programu dep, w ramach repozytorium uruchom:
```
dep ensure -add github.com/Azure/azure-sdk-for-go
```

[!INCLUDE [get-keys](includes/get-keys.md)]

[!INCLUDE [python-get-images](includes/python-get-images.md)]

## <a name="add-the-code"></a>Dodawanie kodu

Utwórz nowy plik o nazwie *sample.go* w preferowanym katalogu projektu.

### <a name="create-the-custom-vision-service-project"></a>Tworzenie projektu Custom Vision Service

Dodaj następujący kod do skryptu, aby utworzyć nowy projekt Custom Vision Service. Wstaw klucze subskrypcji w odpowiednich definicjach. Zwróć uwagę, że tworzenie projektu wykrywania obiektów różni się od tworzenia projektu klasyfikacji obrazów domeną podaną w wywołaniu **create_project**.

```go
import(
    "context"
    "bytes"
    "fmt"
    "io/ioutil"
    "path"
    "log"
    "time"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v3.0/customvision/training"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v3.0/customvision/prediction"
)

var (
    training_key string = "<your training key>"
    prediction_key string = "<your prediction key>"
    prediction_resource_id = "<your prediction resource id>"
    endpoint string = "https://southcentralus.api.cognitive.microsoft.com"
    project_name string = "Go Sample OD Project"
    iteration_publish_name = "detectModel"
    sampleDataDirectory = "<path to sample images>"
)

func main() {
    fmt.Println("Creating project...")

    ctx = context.Background()

    trainer := training.New(training_key, endpoint)

    var objectDetectDomain training.Domain
    domains, _ := trainer.GetDomains(ctx)

    for _, domain := range *domains.Value {
        fmt.Println(domain, domain.Type)
        if domain.Type == "ObjectDetection" && *domain.Name == "General" {
            objectDetectDomain = domain
            break
        }
    }
    fmt.Println("Creating project...")
    project, _ := trainer.CreateProject(ctx, project_name, "", objectDetectDomain.ID, "")
```

### <a name="create-tags-in-the-project"></a>Tworzenie tagów w projekcie

Aby utworzyć tagi klasyfikacji dla projektu, dodaj następujący kod na końcu pliku *sample.go*:

```Go
    # Make two tags in the new project
    forkTag, _ := trainer.CreateTag(ctx, *project.ID, "fork", "A fork", string(training.Regular))
    scissorsTag, _ := trainer.CreateTag(ctx, *project.ID, "scissors", "Pair of scissors", string(training.Regular))
```

### <a name="upload-and-tag-images"></a>Przekazywanie i tagowanie obrazów

Oznaczając tagami obrazy w projektach wykrywania obiektów, należy określić region każdego otagowanego obiektu za pomocą znormalizowanych współrzędnych.

Aby dodać obrazy, tagi i regiony do projektu, wstaw następujący kod po utworzeniu tagów. Uwaga: w tym samouczku regiony są zapisane przy użyciu stałych w kodzie. Regiony określają pole ograniczenia w znormalizowanych współrzędnych, które podaje się w kolejności: lewa krawędź, górna krawędź, szerokość, wysokość.

```Go
    forkImageRegions := map[string][4]float64{
        "fork_1.jpg": [4]float64{ 0.145833328, 0.3509314, 0.5894608, 0.238562092 },
        "fork_2.jpg": [4]float64{ 0.294117659, 0.216944471, 0.534313738, 0.5980392 },
        "fork_3.jpg": [4]float64{ 0.09191177, 0.0682516545, 0.757352948, 0.6143791 },
        "fork_4.jpg": [4]float64{ 0.254901975, 0.185898721, 0.5232843, 0.594771266 },
        "fork_5.jpg": [4]float64{ 0.2365196, 0.128709182, 0.5845588, 0.71405226 },
        "fork_6.jpg": [4]float64{ 0.115196079, 0.133611143, 0.676470637, 0.6993464 },
        "fork_7.jpg": [4]float64{ 0.164215669, 0.31008172, 0.767156839, 0.410130739 },
        "fork_8.jpg": [4]float64{ 0.118872553, 0.318251669, 0.817401946, 0.225490168 },
        "fork_9.jpg": [4]float64{ 0.18259804, 0.2136765, 0.6335784, 0.643790841 },
        "fork_10.jpg": [4]float64{ 0.05269608, 0.282303959, 0.8088235, 0.452614367 },
        "fork_11.jpg": [4]float64{ 0.05759804, 0.0894935, 0.9007353, 0.3251634 },
        "fork_12.jpg": [4]float64{ 0.3345588, 0.07315363, 0.375, 0.9150327 },
        "fork_13.jpg": [4]float64{ 0.269607842, 0.194068655, 0.4093137, 0.6732026 },
        "fork_14.jpg": [4]float64{ 0.143382356, 0.218578458, 0.7977941, 0.295751631 },
        "fork_15.jpg": [4]float64{ 0.19240196, 0.0633497, 0.5710784, 0.8398692 },
        "fork_16.jpg": [4]float64{ 0.140931368, 0.480016381, 0.6838235, 0.240196079 },
        "fork_17.jpg": [4]float64{ 0.305147052, 0.2512582, 0.4791667, 0.5408496 },
        "fork_18.jpg": [4]float64{ 0.234068632, 0.445702642, 0.6127451, 0.344771236 },
        "fork_19.jpg": [4]float64{ 0.219362751, 0.141781077, 0.5919118, 0.6683006 },
        "fork_20.jpg": [4]float64{ 0.180147052, 0.239820287, 0.6887255, 0.235294119 },
    }
    
    scissorsImageRegions := map[string][4]float64{
        "scissors_1.jpg": [4]float64{ 0.4007353, 0.194068655, 0.259803921, 0.6617647 },
        "scissors_2.jpg": [4]float64{ 0.426470578, 0.185898721, 0.172794119, 0.5539216 },
        "scissors_3.jpg": [4]float64{ 0.289215684, 0.259428144, 0.403186262, 0.421568632 },
        "scissors_4.jpg": [4]float64{ 0.343137264, 0.105833367, 0.332107842, 0.8055556 },
        "scissors_5.jpg": [4]float64{ 0.3125, 0.09766343, 0.435049027, 0.71405226 },
        "scissors_6.jpg": [4]float64{ 0.379901975, 0.24308826, 0.32107842, 0.5718954 },
        "scissors_7.jpg": [4]float64{ 0.341911763, 0.20714055, 0.3137255, 0.6356209 },
        "scissors_8.jpg": [4]float64{ 0.231617644, 0.08459154, 0.504901946, 0.8480392 },
        "scissors_9.jpg": [4]float64{ 0.170343131, 0.332957536, 0.767156839, 0.403594762 },
        "scissors_10.jpg": [4]float64{ 0.204656869, 0.120539248, 0.5245098, 0.743464053 },
        "scissors_11.jpg": [4]float64{ 0.05514706, 0.159754932, 0.799019635, 0.730392158 },
        "scissors_12.jpg": [4]float64{ 0.265931368, 0.169558853, 0.5061275, 0.606209159 },
        "scissors_13.jpg": [4]float64{ 0.241421565, 0.184264734, 0.448529422, 0.6830065 },
        "scissors_14.jpg": [4]float64{ 0.05759804, 0.05027781, 0.75, 0.882352948 },
        "scissors_15.jpg": [4]float64{ 0.191176474, 0.169558853, 0.6936275, 0.6748366 },
        "scissors_16.jpg": [4]float64{ 0.1004902, 0.279036, 0.6911765, 0.477124184 },
        "scissors_17.jpg": [4]float64{ 0.2720588, 0.131977156, 0.4987745, 0.6911765 },
        "scissors_18.jpg": [4]float64{ 0.180147052, 0.112369314, 0.6262255, 0.6666667 },
        "scissors_19.jpg": [4]float64{ 0.333333343, 0.0274019931, 0.443627447, 0.852941155 },
        "scissors_20.jpg": [4]float64{ 0.158088237, 0.04047389, 0.6691176, 0.843137264 },
    }
```
Następnie użyj tego mapowania skojarzeń, aby przekazać każdy przykładowy obraz z jego współrzędnymi regionu. Dodaj następujący kod.

> [!NOTE]
> Należy zmienić ścieżkę do obrazów zależnie od tego, dokąd został wcześniej pobrany projekt z przykładami dotyczącymi zestawu SDK dla języka Go dla usług Cognitive Services.

```Go
    // Go through the data table above and create the images
    fmt.Println("Adding images...")
    var fork_images []training.ImageFileCreateEntry
    for file, region := range forkImageRegions {
        imageFile, _ := ioutil.ReadFile(path.Join(sampleDataDirectory, "fork", file))

        imageRegion := training.Region { 
            TagID:forkTag.ID,
            Left:&region[0],
            Top:&region[1],
            Width:&region[2],
            Height:&region[3],
        }

        fork_images = append(fork_images, training.ImageFileCreateEntry {
            Name: &file,
            Contents: &imageFile,
            Regions: &[]training.Region{ imageRegion },
        })
    }
        
    fork_batch, _ := trainer.CreateImagesFromFiles(ctx, *project.ID, training.ImageFileCreateBatch{ 
        Images: &fork_images,
    })

    if (!*fork_batch.IsBatchSuccessful) {
        fmt.Println("Batch upload failed.")
    }

    var scissor_images []training.ImageFileCreateEntry
    for file, region := range scissorsImageRegions {
        imageFile, _ := ioutil.ReadFile(path.Join(sampleDataDirectory, "scissors", file))

        imageRegion := training.Region { 
            TagID:scissorsTag.ID,
            Left:&region[0],
            Top:&region[1],
            Width:&region[2],
            Height:&region[3],
        }

        scissor_images = append(scissor_images, training.ImageFileCreateEntry {
            Name: &file,
            Contents: &imageFile,
            Regions: &[]training.Region{ imageRegion },
        })
    }
        
    scissor_batch, _ := trainer.CreateImagesFromFiles(ctx, *project.ID, training.ImageFileCreateBatch{ 
        Images: &scissor_images,
    })
     
    if (!*scissor_batch.IsBatchSuccessful) {
        fmt.Println("Batch upload failed.")
    }     
```

### <a name="train-the-project-and-publish"></a>Projekt uczenie i publikowanie

Ten kod tworzy pierwszą iteracją w projekcie, a następnie publikuje tej iteracji do endpoint prognoz. Nazwa nadana opublikowanych iteracji może służyć do wysyłania żądań do prognozowania. Iteracji nie jest dostępna w punkcie końcowym prognozowania, dopóki zostanie opublikowany.

```go
    iteration, _ := trainer.TrainProject(ctx, *project.ID)
    fmt.Println("Training status:", *iteration.Status)
    for {
        if *iteration.Status != "Training" {
            break
        }
        time.Sleep(5 * time.Second)
        iteration, _ = trainer.GetIteration(ctx, *project.ID, *iteration.ID)
        fmt.Println("Training status:", *iteration.Status)
    }

    trainer.PublishIteration(ctx, *project.ID, *iteration.ID, iteration_publish_name, prediction_resource_id))
```

### <a name="get-and-use-the-published-iteration-on-the-prediction-endpoint"></a>I korzystaj z opublikowanych iteracji w punkcie końcowym prognoz

Aby wysłać obraz do punktu końcowego przewidywania i uzyskać przewidywanie, dodaj na końcu pliku następujący kod:

```go
    fmt.Println("Predicting...")
    predictor := prediction.New(prediction_key, endpoint)

    testImageData, _ := ioutil.ReadFile(path.Join(sampleDataDirectory, "Test", "test_od_image.jpg"))
    results, _ := predictor.DetectImage(ctx, *project.ID, iteration_publish_name, ioutil.NopCloser(bytes.NewReader(testImageData)), "")

    for _, prediction := range *results.Predictions    {
        boundingBox := *prediction.BoundingBox

        fmt.Printf("\t%s: %.2f%% (%.2f, %.2f, %.2f, %.2f)", 
            *prediction.TagName,
            *prediction.Probability * 100,
            *boundingBox.Left,
            *boundingBox.Top,
            *boundingBox.Width,
            *boundingBox.Height)
        fmt.Println("")
    }
}
```

## <a name="run-the-application"></a>Uruchamianie aplikacji

Uruchom plik *sample.go*.

```powershell
go run sample.go
```

Dane wyjściowe aplikacji powinny pojawić się w konsoli. Możesz następnie sprawdzić, czy obraz testowy (znajdujący się w folderze **samples/vision/images/Test**) został odpowiednio otagowany i czy region wykrywania jest poprawny.

[!INCLUDE [clean-od-project](includes/clean-od-project.md)]

## <a name="next-steps"></a>Kolejne kroki

Teraz wiesz, jak można zrealizować w kodzie poszczególne kroki procesu wykrywania obiektów. W tym przykładzie jest wykonywana jedna iteracja szkolenia, ale często trzeba szkolić i testować model wiele razy, aby zwiększyć jego dokładność. Następny przewodnik dotyczy klasyfikacji obrazów. Jej zasady są podobne do wykrywania obiektów.

> [!div class="nextstepaction"]
> [Testowanie i ponowne szkolenie modelu](test-your-model.md)