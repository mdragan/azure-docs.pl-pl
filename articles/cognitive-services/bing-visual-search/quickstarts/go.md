---
title: 'Szybki start: Uzyskaj szczegółowe informacje o obrazach za pomocą API REST wyszukiwania wizualnego Bing i języka Go'
titleSuffix: Azure Cognitive Services
description: Dowiedz się, jak przekazać obraz do interfejsu API wyszukiwania wizualnego Bing i uzyskać szczegółowe informacje na jego temat.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: rosh
ms.openlocfilehash: dcfb2d7dc49dae03c733a3016d37425e33fb0535
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65796477"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-go"></a>Szybki start: Uzyskaj szczegółowe informacje o obrazach za pomocą API REST wyszukiwania wizualnego Bing i języka Go

Ten przewodnik Szybki Start używa języka programowania Go do wywołania interfejsu API wyszukiwania wizualnego Bing i wyświetlać wyniki. Żądanie POST służy do przekazywania obrazu do punktu końcowego interfejsu API. Wyniki obejmują adresy URL i opisowe informacje na temat obrazy, podobne do przekazanego obrazu.

## <a name="prerequisites"></a>Wymagania wstępne

* Zainstaluj [Przejdź pliki binarne](https://golang.org/dl/).
* Drukarki głębokiego pretty go spew służy do wyświetlania wyników. Można zainstalować z rzeczywistym użyciem — spew z `$ go get -u https://github.com/davecgh/go-spew` polecenia.

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="project-and-libraries"></a>Projekt i bibliotek

Utwórz projekt z rzeczywistym użyciem w IDE lub edytora. Następnie zaimportuj `net/http` w przypadku żądań `ioutil` odczytać odpowiedzi i `encoding/json` do obsługi tekstu JSON wyników. `go-spew` Biblioteki służy do analizowania wyniki JSON.

```
package main

import (
    "bytes"
    "io"
    "fmt"
    "io/ioutil"
    "mime/multipart"
    "net/http"
    "os"
    "path/filepath"
    "encoding/json"
    "github.com/davecgh/go-spew/spew"
)

```

## <a name="struct-to-format-results"></a>Struktury, aby sformatować wyniki

`BingAnswer` Struktury formatuje dane zwrócone w odpowiedzi JSON, czyli kategoryzowanie i złożone. Następującą implementacją obejmuje niektóre podstawowe elementy:

```
type BingAnswer struct {
    Type         string `json:"_type"`
    Instrumentation struct {
        Type string   `json:"_type"`
        PingUrlBase   string  `json:"_pingUrlBase"`
        PageLoadPingUrl  string `json:"_pageLoadPingUrl"`
    } `json:"instrumentation"`
    Tags []struct  {
        DisplayName       string    `json:"displayName"`
        Actions                 []struct {
            Type      string `json:"_type"`
            ActionType    string    `json:"actionType"`
            Data             struct {
                Value     []struct {
                    WebSearchUrl          string `json:"webSearchUrl"`
                    Name        string  `json:"name"`
                }`json:"value"`
                ImageInsightsToken string `json:"imageInsightsToken"`
                InsightsMetadata struct {
                    PagesIncludingCount int `json:"pagesIncludingCount"`
                    AvailableSizesCount  int  `json:"availableSizesCount"`
                }
                ImageId string  `json:"imageId"`
                AccentColor  string `json:"accentColor"`
            } `json:"data"`
        } `json:"actions"`
    } `json:"tags"`
    Image struct {
        WebSearchUrl   string  `json:"webSearchUrl"`
        WebSearchUrlPingSuffix  string `json:"webSearchUrlPingSuffix"`
        Name   string   `json:"name"`
        IsFamilyFriendly   bool  `json:"isFamilyFriendly"`
        ContentSize   string   `json:"contentSize"`
        EncodingFormat    string   `json:"encodingFormat"`
        HostPageDisplayUrl   string    `json:"hostPageDisplayUrl"`
        Width     int   `json:"width"`
        Height    int    `json:"height"`
        Thumbnail    struct  {
            Width   int   `json:"width"`
            Height  int   `json:"height"`
        }
        InsightsMetadata  struct {
            PagesIncludingCount   int   `json:"pagesIncludingCount"`
            AvailableSizesCount    int    `json:"availableSizesCount"`
        }
        AccentColor   string     `json:"accentColor"`
        VisualWords   string   `json:"visualWords"`
    } `json:"image"`
}

```

## <a name="main-function-and-variables"></a>Funkcja Main i zmienne  

Poniższy kod deklaruje funkcję main i przypisuje zmienne wymagane. Upewnij się, że punkt końcowy jest poprawny, i zamień wartość `token` na odpowiedni klucz subskrypcji ze swojego konta platformy Azure. `batchNumber` Jest identyfikatorem GUID wymagane dla początkowe i końcowe granice danych POST. `fileName` Zmiennej identyfikuje plik obrazu dla wpisu. W poniższych sekcjach opisano szczegóły kodu:

```
func main() {
    // Verify the endpoint URI and replace the token string with a valid subscription key.se
    endpoint := "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch"
    token := "YOUR-ACCESS-KEY"
    client := &http.Client{}
    batchNumber := "d7ecc447-912f-413e-961d-a83021f1775f"
    fileName := "ElectricBike.JPG"

    body, contentType := createRequestBody(fileName, batchNumber)
    
    req, err := http.NewRequest("POST", endpoint, body)
    req.Header.Add("Content-Type", contentType)
    req.Header.Set("Ocp-Apim-Subscription-Key", token)
    
    resp, err := client.Do(req)
    if err != nil {
        panic(err)
    }
    
       defer resp.Body.Close()
    
    resbody, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        panic(err)
    }

    //fmt.Print(string(resbody))
    
    // Create a new answer.  
    ans := new(BingAnswer)
    err = json.Unmarshal(resbody, &ans)
    if err != nil {
        fmt.Println(err)
    }
    
    fmt.Print("Output of BingAnswer: \r\n\r\n")
    
    // Iterate over search results and print the result name and URL.
    for _, result := range ans.Tags {
        spew.Dump(result)
    }
}

```

## <a name="boundaries-of-post-body"></a>Granice treść wpisu

Żądania POST na punkt końcowy wyszukiwania wizualnego wymaga początkowe i końcowe granice otaczający danych POST. Wiodący granic zawiera numer wsadowym, identyfikatora typu zawartości `Content-Disposition: form-data; name="image"; filename=`, oraz nazwę pliku obrazu do wpisu. Końcowe granica jest po prostu numer wsadowym. Te funkcje nie są uwzględnione w `main` bloku:

```
func BuildFormDataStart(batNum string, fileName string) string{

    startBoundary := "--batch_" + batNum + "\r\n"
    requestBoundary := startBoundary + "\r\n" + "Content-Disposition: form-data; name=\"image\"; filename=" + fileName + "\r\n\r\n"
    return requestBoundary
}

func BuildFormDataEnd(batNum string) string{

    return "\r\n" + "\r\n" + "--batch_" + batNum + "\r\n"

}

```
## <a name="add-image-bytes-to-post-body"></a>Dodaj bajty obrazu do treść wpisu

Ten segment kodu tworzy żądanie POST, zawierający dane obrazu:

```
func createRequestBody(fileName string, batchNumber string) (*bytes.Buffer, string) {
    file, err := os.Open(fileName)
    if err != nil {
        panic(err)
    }
    defer file.Close()

    body := &bytes.Buffer{}
    writer := multipart.NewWriter(body)
    
    writer.SetBoundary(BuildFormDataStart(batchNumber, fileName))
    
    part, err := writer.CreateFormFile("image", filepath.Base(file.Name()))
    if err != nil {
        panic(err)
    }
    io.Copy(part, file)
    writer.WriteField("lastpart", BuildFormDataEnd(batchNumber))    
    writer.Close()
    return body, writer.FormDataContentType()
}

```

## <a name="send-the-request"></a>Wysyłanie żądania

Poniższy kod wysyła żądanie i odczytuje wyniki:

```
resp, err := client.Do(req)
    if err != nil {
        panic(err)
    }
    
       defer resp.Body.Close()
    
    resbody, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        panic(err)
    }

```

## <a name="handle-the-response"></a>Obsługa odpowiedzi

`Unmarshall` Funkcja wyodrębniania informacji z tekstu JSON, zwracany przez interfejsu API wyszukiwania wizualnego. `go-spew` Pretty drukarki są wyświetlane wyniki:

```
    // Create a new answer.  
    ans := new(BingAnswer)
    err = json.Unmarshal(resbody, &ans)
    if err != nil {
        fmt.Println(err)
    }
    
    fmt.Print("Output of BingAnswer: \r\n\r\n")
    
    // Iterate over search results and print the result name and URL.
    for _, result := range ans.Tags {
        spew.Dump(result)
    }

```
> [!NOTE]
> Francesco Giordano przyczynił się kod do tego przykładu.

## <a name="results"></a>Wyniki

Wyniki identyfikowania obrazów, które są podobne do obrazów znajdujących się w treści wpisu. Przydatne pola są `WebSearchUrl` i `Name`:

```
    Value: ([]struct { WebSearchUrl string "json:\"webSearchUrl\""; Name string "json:\"name\"" }) (len=66 cap=94) {
     (struct { WebSearchUrl string "json:\"webSearchUrl\""; Name string "json:\"name\"" }) {
      WebSearchUrl: (string) (len=129) "https://www.bing.com/images/search?view=detailv2&FORM=OIIRPO&id=B9E6621161769D578A9E4DD9FD742128DE65225A&simid=608046863828453626",
      Name: (string) (len=87) "26\"Foldable Electric Mountain Bike Bicycle Ebike W/Lithium Battery 250W 36V MY8L | eBay"
     },
     (struct { WebSearchUrl string "json:\"webSearchUrl\""; Name string "json:\"name\"" }) {
      WebSearchUrl: (string) (len=129) "https://www.bing.com/images/search?view=detailv2&FORM=OIIRPO&id=5554757348A9E88E9EEAB74217E228689E716B61&simid=607988237543409883",
      Name: (string) (len=96) "Best 25+ Electric mountain bike ideas on Pinterest | Mountain bicycle, Mtb mountain bike and ..."
     },
     (struct { WebSearchUrl string "json:\"webSearchUrl\""; Name string "json:\"name\"" }) {
      WebSearchUrl: (string) (len=129) "https://www.bing.com/images/search?view=detailv2&FORM=OIIRPO&id=B56F626A4803D829FE326842E2B97CE5B87F17F2&simid=607991699288949513",
      Name: (string) (len=100) "Electric Fat Bike Mountain Bicycle Snow Bike Cruiser Ebike Motor Lithium Battery Dual Oil Brakes ..."
     },
     (struct { WebSearchUrl string "json:\"webSearchUrl\""; Name string "json:\"name\"" }) {
      WebSearchUrl: (string) (len=129) "https://www.bing.com/images/search?view=detailv2&FORM=OIIRPO&id=CCAE400EE510DCA6F5740ABAF08A5310B4EF0698&simid=608003854048890458",
      Name: (string) (len=81) "Best Mountain Bikes Under 1500 (Outstanding Performance) | Mountain Bicycle World"
     },
     (struct { WebSearchUrl string "json:\"webSearchUrl\""; Name string "json:\"name\"" }) {
      WebSearchUrl: (string) (len=129) "https://www.bing.com/images/search?view=detailv2&FORM=OIIRPO&id=1244730E2E3F2D5DFBED9A9CFE1DBE27B09F08BC&simid=608005181199745622",
      Name: (string) (len=98) "ANCHEER Folding Electric Mountain Bike with 26\" Super Lightweight Magnesium (36V 250W) - GearScoot"
     },
     (struct { WebSearchUrl string "json:\"webSearchUrl\""; Name string "json:\"name\"" }) {
      WebSearchUrl: (string) (len=129) "https://www.bing.com/images/search?view=detailv2&FORM=OIIRPO&id=38AAD876E57BB0D502FBDD5B1CB29EB7EB8DD9E2&simid=608041654062220328",
      Name: (string) (len=97) "29 best CB Bikes Gadgets & Accessories images on Pinterest | Bicycles, Bicycling and Bike gadgets"
     },
     (struct { WebSearchUrl string "json:\"webSearchUrl\""; Name string "json:\"name\"" }) {
      WebSearchUrl: (string) (len=129) "https://www.bing.com/images/search?view=detailv2&FORM=OIIRPO&id=6892C8FD22D0E42911C99AFD8FE1FD37BD82E02C&simid=608052803759703185",
      Name: (string) (len=98) "ANCHEER Folding Electric Mountain Bike with 26\" Super Lightweight Magnesium (36V 250W) - GearScoot"
     },

```

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Co to jest interfejs API wyszukiwania wizualnego Bing? ](../overview.md) 
>  [Startu wyszukiwania w Internecie Bing w rzeczywistym użyciem](../../Bing-Web-Search/quickstarts/go.md)
