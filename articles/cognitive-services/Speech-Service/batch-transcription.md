---
title: Jak używać transkrypcji usługi Batch — usługi mowy
titlesuffix: Azure Cognitive Services
description: Transkrypcja partii jest idealnym rozwiązaniem, jeśli chcesz także dużej ilości dźwięk w magazynie, takim jak obiektów blob platformy Azure. Za pomocą dedykowanego interfejsu API REST, można wskazać pliki audio, przy użyciu sygnatury dostępu współdzielonego (SAS) identyfikator URI i asynchronicznie otrzymywać transkrypcji.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 2/20/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 45ed0167f5a83fa843a224ada35e96672a6752a1
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67311841"
---
# <a name="why-use-batch-transcription"></a>Dlaczego warto używać usługi Batch transkrypcji?

Transkrypcja partii jest idealnym rozwiązaniem, jeśli chcesz także dużej ilości dźwięk w magazynie, takim jak obiektów blob platformy Azure. Za pomocą dedykowanego interfejsu API REST, można wskazać pliki audio, przy użyciu sygnatury dostępu współdzielonego (SAS) identyfikator URI i asynchronicznie otrzymywać transkrypcji.

## <a name="prerequisites"></a>Wymagania wstępne

### <a name="subscription-key"></a>Klucz subskrypcji

Jak z wszystkich funkcji usługi rozpoznawania mowy, tworzenie klucz subskrypcji z [witryny Azure portal](https://portal.azure.com) postępując zgodnie z naszym [Wprowadzenie — przewodnik](get-started.md). Jeśli planujesz uzyskiwanie transkrypcje modeli podstawowych w naszym Tworzenie klucza jest wszystko, co należy zrobić.

>[!NOTE]
> Standardowa subskrypcja (S0) dla usług przetwarzania mowy wymagane jest wprowadzenie transkrypcji usługi batch. Bezpłatna subskrypcja kluczy (F0) nie będzie działać. Aby uzyskać więcej informacji, zobacz [ceny i limity](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Modele niestandardowe

Jeśli zamierzasz dostosować modele akustyczne lub języka, wykonaj kroki opisane w [dostosowywanie modeli akustycznych](how-to-customize-acoustic-models.md) i [dostosowywanie modeli językowych](how-to-customize-language-model.md). Używanie modeli utworzonych w transkrypcji usługi batch należy ich identyfikatory modelu. Ten identyfikator nie jest identyfikator punktu końcowego, który można znaleźć w widoku Szczegóły punktu końcowego, jest identyfikator modelu, który można pobrać po wybraniu szczegółów modeli.

## <a name="the-batch-transcription-api"></a>Transkrypcji interfejsu API usługi Batch

Interfejs API transkrypcji usługi Batch oferuje asynchroniczne transkrypcja mowy na tekst, oraz dodatkowe funkcje. To interfejs API REST, który udostępnia metody:

1. Tworzenie żądań przetwarzania wsadowego
1. Stan zapytania
1. Pobieranie transkrypcji

> [!NOTE]
> Interfejs API transkrypcji usługi Batch jest idealny dla wywołania roboczych, które zazwyczaj są gromadzone tysiące godzin audio. Ułatwia on także bardzo dużych ilości nagrania audio.

### <a name="supported-formats"></a>Obsługiwane formaty

Interfejs API transkrypcji usługi Batch obsługuje następujące formaty:

| Format | Koder-dekoder | Szybkość transmisji bitów | Częstotliwość próbkowania |
|--------|-------|---------|-------------|
| WAV | MODUŁU PCM | 16-bitowych | stereo mono, kHz, 8 lub 16 |
| MP3 | MODUŁU PCM | 16-bitowych | stereo mono, kHz, 8 lub 16 |
| OGG | DZIELE | 16-bitowych | stereo mono, kHz, 8 lub 16 |

Dla strumieni audio stereo transkrypcji interfejsu API usługi Batch dzieli kanału lewy i prawy podczas transkrypcji. Każdy dwa pliki JSON z wynikiem są tworzone z pojedynczy kanał. Sygnatury czasowe na wypowiedź Włącz dla deweloperów utworzyć uporządkowany końcowego transkrypcji. To przykładowe żądanie zawiera właściwości filtrowania wulgaryzmów, znaki interpunkcyjne i sygnatury czasowe z poziomu programu word. 

### <a name="configuration"></a>Konfigurowanie

Parametry konfiguracji są dostarczane jako dane JSON:

```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to us, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True",
    "AddSentiment" : "True"
  }
}
```

> [!NOTE]
> Interfejs API transkrypcji usługi Batch korzysta z usługi REST, do żądania transkrypcje, stanu i skojarzonych wyników. Można użyć interfejsu API z dowolnego języka. W następnej sekcji opisano sposób użycia interfejsu API.

### <a name="configuration-properties"></a>Właściwości konfiguracji

Aby skonfigurować transkrypcji, wykonaj następujące opcjonalne właściwości:

| Parametr | Opis |
|-----------|-------------|
| `ProfanityFilterMode` | Określa sposób obsługi wulgaryzmów w wyniki rozpoznawania. Akceptowane wartości to `none` która wyłącza filtrowanie wulgaryzmów `masked` gwiazdek, która zastępuje wulgaryzmów `removed` z wyników, które powoduje usunięcie wszystkich wulgaryzmów lub `tags` dodaje tagi "wulgaryzmów". Ustawieniem domyślnym jest `masked`. |
| `PunctuationMode` | Określa sposób obsługi znaków interpunkcyjnych w wyniki rozpoznawania. Akceptowane wartości to `none` która wyłącza znak interpunkcyjny, `dictated` co oznacza jawne znak interpunkcyjny, `automatic` umożliwiającą dekodera przeciwdziałania znak interpunkcyjny, lub `dictatedandautomatic` co oznacza definiowane znaków interpunkcyjnych lub automatyczny. |
 | `AddWordLevelTimestamps` | Określa, jeśli sygnatury czasowe z poziomu programu word powinna być dodana do danych wyjściowych. Akceptowane wartości to `true` umożliwiająca sygnatury czasowe z poziomu programu word i `false` (wartość domyślna) można ją wyłączyć. |
 | `AddSentiment` | Określa, że wskaźniki nastrojów klientów powinny zostać dodane do wypowiedź. Akceptowane wartości to `true` umożliwiająca tonacji na wypowiedź i `false` (wartość domyślna) można ją wyłączyć. |
 | `AddDiarization` | Określa, że ten alalysis diarization powinny być przeprowadzane na danych wejściowych, który powinien być mono kanał, zawierający dwie głosów. Akceptowane wartości to `true` umożliwiająca diarization i `false` (wartość domyślna) można ją wyłączyć. Wymaga to również `AddWordLevelTimestamps` należy ustawić na wartość true.|

### <a name="storage"></a>Magazyn

Usługa Batch obsługuje transkrypcji [usługi Azure Blob storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) do odczytywania audio i transkrypcje zapisywania do magazynu.

## <a name="webhooks"></a>Elementy webhook 

Sondowania stanu transkrypcji nie może być większość wydajne lub zapewnia najlepsze środowisko użytkownika. Aby wykonać sondowanie dotyczące stanu, należy zarejestrować wywołania zwrotne, które powiadomi klienta po zakończeniu długotrwałych zadań transkrypcji.

Aby uzyskać więcej informacji, zobacz [elementów Webhook](webhooks.md).

## <a name="speaker-separation-diarization"></a>Rozdzielenie osoby mówiącej (Diarization)

Diarization jest proces głośników w fragmentu audio. Nasz potok usługi Batch obsługuje Diarization i jest w stanie rozpoznawać dwóch prelegentów na nagrania mono kanału.

Aby zażądać, że żądania transkrypcję audio są przetwarzane dla diarization, wystarczy dodać odpowiedniego parametru w żądaniu HTTP, jak pokazano poniżej.

 ```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to us, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "AddWordLevelTimestamps" : "True",
    "AddDiarization" : "True"
  }
}
```

Sygnatury czasowe z poziomu programu Word będzie również musiał zostać "włączona", jak wskazać parametrów w żądaniu powyżej. 

Odpowiadające im pliki audio będzie zawierać Prelegenci identyfikowana przez numer (obecnie obsługujemy tylko dwa głosy, więc prelegentów zostaną zidentyfikowane jako "osoby mówiącej 1" i "Osoby mówiącej 2") następuje dane wyjściowe transkrypcji.

Należy również zauważyć, że Diarization nie jest dostępna w Stereo nagrania. Ponadto JSON wszystkie dane wyjściowe będą zawierać tag osoby mówiącej. Jeżeli nie zastosowano diarization, widoczna będzie "osoby mówiącej: Wartość null "w kodzie JSON, dane wyjściowe.

> [!NOTE]
> Diarization jest dostępna we wszystkich regionach, jak i dla wszystkich ustawień regionalnych!

## <a name="sentiment"></a>Opinia

Wskaźniki nastrojów klientów jest nowa funkcja interfejsu API usługi Batch transkrypcji i ważną funkcję w domenie Centrum połączenia. Klienci mogą używać `AddSentiment` parametrów na ich żądania 

1.  Uzyskuj szczegółowe informacje dotyczące zadowolenia klientów
2.  Uzyskaj wgląd na wydajność agentów (zespół przyjmowanie wywołań)
3.  Wykrywanie dokładny moment w czasie, gdy wywołanie trwało Włącz w kierunku ujemna
4.  Wykrywanie, co poszło dobrze w przypadku, gdy włączenie wywołania ujemny wynik dodatni
5.  Identyfikowanie klientów lubi i co one podoba produktu lub usługi

Wynik tonacji skategoryzowano według segmentu audio, gdzie audio segmentu jest zdefiniowany jako okres od początku wypowiedź (przesunięciem) i wyciszenia wykrywania końca strumienia bajtów. Cały tekst w tym segmencie jest używane do obliczania wskaźniki nastrojów klientów. Firma Microsoft nie obliczać wartości tonacji agregacji całego połączenia lub całego mowy poszczególnych kanałów. Tych agregacji są pozostawiane do właściciela domeny, do dalszego zastosowania.

Wskaźniki nastrojów klientów są stosowane w formularzu leksykalne.

Przykładowe dane wyjściowe JSON wygląda jak poniżej:

```json
{
  "AudioFileResults": [
    {
      "AudioFileName": "Channel.0.wav",
      "AudioFileUrl": null,
      "SegmentResults": [
        {
          "RecognitionStatus": "Success",
          "ChannelNumber": null,
          "Offset": 400000,
          "Duration": 13300000,
          "NBest": [
            {
              "Confidence": 0.976174,
              "Lexical": "what's the weather like",
              "ITN": "what's the weather like",
              "MaskedITN": "what's the weather like",
              "Display": "What's the weather like?",
              "Words": null,
              "Sentiment": {
                "Negative": 0.206194,
                "Neutral": 0.793785,
                "Positive": 0.0
              }
            }
          ]
        }
      ]
    }
  ]
}
```
Funkcja korzysta z modelu tonacji, która jest obecnie dostępna w wersji Beta.

## <a name="sample-code"></a>Przykładowy kod

Pełne przykłady są dostępne w [repozytorium przykładów GitHub](https://aka.ms/csspeech/samples) wewnątrz `samples/batch` podkatalogu.

Trzeba dostosować przykładowego kodu, informacje o subskrypcji, usługa region sygnatury dostępu Współdzielonego identyfikator URI wskazujący na plik dźwiękowy transkrypcja i identyfikatory modelu, w przypadku, gdy chcesz użyć niestandardowego modelu akustycznego lub języka. 

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

Przykładowy kod będzie instalacji klienta i przesłać żądanie transkrypcji. Zostanie następnie sondowania pod kątem informacji o stanie i Drukuj szczegóły o postępie transkrypcji.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Aby uzyskać szczegółowe informacje dotyczące poprzedniego wywołania, zobacz nasze [dokument struktury Swagger](https://westus.cris.ai/swagger/ui/index). Aby uzyskać pełny przykład pokazano poniżej, przejdź do [GitHub](https://aka.ms/csspeech/samples) w `samples/batch` podkatalogu.

Zwróć uwagę na asynchroniczne Instalatora w celu opublikowanie audio i odbieranie stanu transkrypcji. Klient, który tworzysz to klient HTTP platformy .NET. Brak `PostTranscriptions` Metoda przesyłania plików audio i `GetTranscriptions` metody do odbierania wyników. `PostTranscriptions` Zwraca uchwyt, a `GetTranscriptions` używa jej do utworzenia dojście można pobrać stanu transkrypcji.

Bieżący kod przykładowy nie określono modelu niestandardowego. Usługa używa modele planu bazowego dla przepisywania pliku lub plików. Aby określić te modele, można przekazać w tej samej metody identyfikatory modelu akustycznego i modelu języka.

> [!NOTE]
> W transkrypcji punktu odniesienia nie jest wymagana do deklarowania Identyfikatora dla modeli linii bazowej. Jeśli określisz tylko język, w identyfikator modelu (a nie Identyfikatora model akustyczny), pasujące model akustyczny wybierana jest. Jeśli określono tylko identyfikator model akustyczny, dopasowania modelu języka jest wybierana.

## <a name="download-the-sample"></a>Pobierz przykład

Można znaleźć przykład w `samples/batch` katalogu w [repozytorium przykładów GitHub](https://aka.ms/csspeech/samples).

> [!NOTE]
> Batch transkrypcji zadania zaplanowane na optymalne, nie ma żadnych szacowany czas podczas zadania zmieni się w stanie uruchomienia. Jeden raz w stanie uruchomienia, rzeczywiste transkrypcji są przetwarzane szybciej niż audio czasu rzeczywistego.

## <a name="next-steps"></a>Kolejne kroki

* [Pobierz subskrypcję usługi mowy w wersji próbnej](https://azure.microsoft.com/try/cognitive-services/)
