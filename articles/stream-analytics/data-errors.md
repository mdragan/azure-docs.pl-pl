---
title: Błędów danych dziennik diagnostyczny funkcji w usłudze Azure Stream Analytics
description: W tym artykule opisano różne dane wejściowe i błędów danych wyjściowych, które mogą wystąpić w przypadku korzystania z usługi Azure Stream Analytics.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: ecc7077bf208adf1ac89adcce2f2e480ce34888e
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67329594"
---
# <a name="azure-stream-analytics-data-errors"></a>Błędy danych w usłudze Azure Stream Analytics

Dane błędów są błędów występujących podczas przetwarzania danych.  Te błędy w większości przypadków odbywały się w danych deserializacji, serializacji, operacji i zapisu.  Gdy wystąpi błąd danych, usługi Stream Analytics zapisuje szczegółowe informacje i przykłady zdarzeń do dzienników diagnostycznych.  W niektórych przypadkach również udostępniane jest podsumowanie tych informacji za pośrednictwem powiadomienia w portalu.

W tym artykule opisano typy różnych błędów, przyczyny i dziennik diagnostyczny szczegóły błędów w danych wejściowych i wyjściowych.

## <a name="diagnostic-log-schema"></a>Dziennik diagnostyczny schematu

Zobacz [Rozwiązywanie problemów z usługą Azure Stream Analytics przy użyciu dzienników diagnostyki](stream-analytics-job-diagnostic-logs.md#diagnostics-logs-schema) Aby wyświetlić schemat dla dzienników diagnostycznych. Następujący kod JSON jest wartością przykład **właściwości** pola dziennik diagnostyczny dla błędów danych.

```json
{
    "Source": "InputTelemetryData",
    "Type": "DataError",
    "DataErrorType": "InputDeserializerError.InvalidData",
    "BriefMessage": "Json input stream should either be an array of objects or line separated objects. Found token type: Integer",
    "Message": "Input Message Id: https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt Error: Json input stream should either be an array of objects or line separated objects. Found token type: Integer",
    "ExampleEvents": "[\"1,2\\\\u000d\\\\u000a3,4\\\\u000d\\\\u000a5,6\"]",
    "FromTimestamp": "2019-03-22T22:34:18.5664937Z",
    "ToTimestamp": "2019-03-22T22:34:18.5965248Z",
    "EventCount": 1
}
```

## <a name="input-data-errors"></a>Błędy w danych wejściowych

### <a name="inputdeserializererrorinvalidcompressiontype"></a>InputDeserializerError.InvalidCompressionType

* Przyczyna: Wybranego typu danych wejściowych kompresji nie pasują do danych.
* Podany powiadomieniu portalu: Tak
* Poziom dzienniki diagnostyczne: Ostrzeżenie
* Wpływ: Komunikaty, w których błędy deserializacji, w tym nieprawidłowy typ kompresji są usuwane z danych wejściowych.
* Szczegóły dziennika
   * Wprowadź identyfikator wiadomości. Centrum zdarzeń, aby uzyskać identyfikator jest PartitionId, przesunięcie i numer sekwencyjny.

**komunikat o błędzie**

```json
"BriefMessage": "Unable to decompress events from resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt'. Please ensure compression setting fits the data being processed."
```

### <a name="inputdeserializererrorinvalidheader"></a>InputDeserializerError.InvalidHeader

* Przyczyna: Nagłówek danych wejściowych jest nieprawidłowy. Na przykład woluminu CSV zawiera kolumny o takich samych nazwach.
* Podany powiadomieniu portalu: Tak
* Poziom dzienniki diagnostyczne: Ostrzeżenie
* Wpływ: Komunikaty, w których błędy deserializacji, w tym nieprawidłowy nagłówek są usuwane z danych wejściowych.
* Szczegóły dziennika
   * Wprowadź identyfikator wiadomości. 
   * Rzeczywiste obciążenie do kilku kilobajtów.

**komunikat o błędzie**

```json
"BriefMessage": "Invalid CSV Header for resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt'. Please make sure there are no duplicate field names."
```

### <a name="inputdeserializererrormissingcolumns"></a>InputDeserializerError.MissingColumns

* Przyczyna: Kolumny wejściowe zdefiniowane za pomocą polecenia CREATE TABLE lub za pośrednictwem TIMESTAMP BY nie istnieje.
* Podany powiadomieniu portalu: Yes
* Poziom dzienniki diagnostyczne: Ostrzeżenie
* Wpływ: Zdarzenia z kolumnami Brak są usuwane z danych wejściowych.
* Szczegóły dziennika
   * Wprowadź identyfikator wiadomości. 
   * Nazwy kolumn, które są nieobecne. 
   * Rzeczywiste obciążenie do kilku kilobajtów.

**Komunikaty o błędach**

```json
"BriefMessage": "Could not deserialize the input event(s) from resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt' as Csv. Some possible reasons: 1) Malformed events 2) Input source configured with incorrect serialization format" 
```

```json
"Message": "Missing fields specified in query or in create table. Fields expected:ColumnA Fields found:ColumnB"
```

### <a name="inputdeserializererrortypeconversionerror"></a>InputDeserializerError.TypeConversionError

* Przyczyna: Nie można skonwertować danych wejściowych do typu określonego w instrukcji CREATE TABLE.
* Podany powiadomieniu portalu: Tak
* Poziom dzienniki diagnostyczne: Ostrzeżenie
* Wpływ: Zdarzenia z powodu błędu konwersji typu są usuwane z danych wejściowych.
* Szczegóły dziennika
   * Wprowadź identyfikator wiadomości. 
   * Nazwa kolumny i oczekiwanego typu.

**Komunikaty o błędach**

```json
"BriefMessage": "Could not deserialize the input event(s) from resource '''https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt ' as Csv. Some possible reasons: 1) Malformed events 2) Input source configured with incorrect serialization format" 
```

```json
"Message": "Unable to convert column: dateColumn to expected type."
```

### <a name="inputdeserializererrorinvaliddata"></a>InputDeserializerError.InvalidData

* Przyczyna: Dane wejściowe nie jest w odpowiednim formacie. Na przykład wartość wejściowa nie jest prawidłowym plikiem JSON.
* Podany powiadomieniu portalu: Tak
* Poziom dzienniki diagnostyczne: Ostrzeżenie
* Wpływ: Wszystkie zdarzenia w komunikacie po napotkał błąd nieprawidłowe dane są usuwane z danych wejściowych.
* Szczegóły dziennika
   * Wprowadź identyfikator wiadomości. 
   * Rzeczywiste obciążenie do kilku kilobajtów.

**Komunikaty o błędach**

```json
"BriefMessage": "Json input stream should either be an array of objects or line separated objects. Found token type: String"
```

```json
"Message": "Json input stream should either be an array of objects or line separated objects. Found token type: String"
```

### <a name="invalidinputtimestamp"></a>InvalidInputTimeStamp

* Przyczyna: Nie można przekonwertować wartości wyrażenia TIMESTAMP BY do daty/godziny.
* Podany powiadomieniu portalu: Tak
* Poziom dzienniki diagnostyczne: Ostrzeżenie
* Wpływ: Zdarzenia z nieprawidłową sygnaturą czasową w danych wejściowych są usuwane z danych wejściowych.
* Szczegóły dziennika
   * Wprowadź identyfikator wiadomości. 
   * Komunikat o błędzie. 
   * Rzeczywiste obciążenie do kilku kilobajtów.

**komunikat o błędzie**

```json
"BriefMessage": "Unable to get timestamp for resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt ' due to error 'Cannot convert string to datetime'"
```

### <a name="invalidinputtimestampkey"></a>InvalidInputTimeStampKey

* Przyczyna: Wartość timestampColumn TIMESTAMP BY OVER ma wartość NULL.
* Podany powiadomieniu portalu: Tak
* Poziom dzienniki diagnostyczne: Ostrzeżenie
* Wpływ: Zdarzenia z nieprawidłowym znacznikiem czasu wprowadzania klucza są usuwane z danych wejściowych.
* Szczegóły dziennika
   * Rzeczywiste obciążenie do kilku kilobajtów.

**komunikat o błędzie**

```json
"BriefMessage": "Unable to get value of TIMESTAMP BY OVER COLUMN"
```

### <a name="lateinputevent"></a>LateInputEvent

* Przyczyna: Różnica między czasem aplikacji i czas nadejścia jest większa niż okno tolerancji spóźnionego przybycia.
* Podany powiadomieniu portalu: Nie
* Poziom dzienniki diagnostyczne: Informacje
* Wpływ:  Opóźnione zdarzenia wejściowe są obsługiwane zgodnie z "Obsługa innych zdarzeń" ustawienie w zdarzeniu kolejność sekcji konfiguracji zadania. Aby uzyskać więcej informacji, zobacz [czas obsługi zasad](https://docs.microsoft.com/stream-analytics-query/time-skew-policies-azure-stream-analytics).
* Szczegóły dziennika
   * Czas aplikacji i czasu odbioru. 
   * Rzeczywiste obciążenie do kilku kilobajtów.

**komunikat o błędzie**

```json
"BriefMessage": "Input event with application timestamp '2019-01-01' and arrival time '2019-01-02' was sent later than configured tolerance."
```

### <a name="earlyinputevent"></a>EarlyInputEvent

* Przyczyna: Różnica między czasem aplikacji i czas nadejścia jest większa niż 5 minut.
* Podany powiadomieniu portalu: Nie
* Poziom dzienniki diagnostyczne: Informacje
* Wpływ:  Wczesne zdarzenia wejściowe są obsługiwane zgodnie z "Obsługa innych zdarzeń" ustawienie w zdarzeniu kolejność sekcji konfiguracji zadania. Aby uzyskać więcej informacji, zobacz [czas obsługi zasad](https://docs.microsoft.com/stream-analytics-query/time-skew-policies-azure-stream-analytics).
* Szczegóły dziennika
   * Czas aplikacji i czasu odbioru. 
   * Rzeczywiste obciążenie do kilku kilobajtów.

**komunikat o błędzie**

```json
"BriefMessage": "Input event arrival time '2019-01-01' is earlier than input event application timestamp '2019-01-02' by more than 5 minutes."
```

### <a name="outoforderevent"></a>OutOfOrderEvent

* Przyczyna: Zdarzenie jest traktowane jako poza kolejnością według okno tolerancji poza kolejnością, zdefiniowane.
* Podany powiadomieniu portalu: Nie
* Poziom dzienniki diagnostyczne: Informacje
* Wpływ:  Poza kolejnością, zdarzenia są obsługiwane zgodnie z "Obsługa innych zdarzeń" ustawienie w zdarzeniu porządkowanie sekcji konfiguracji zadania. Aby uzyskać więcej informacji, zobacz [czas obsługi zasad](https://docs.microsoft.com/stream-analytics-query/time-skew-policies-azure-stream-analytics).
* Szczegóły dziennika
   * Rzeczywiste obciążenie do kilku kilobajtów.

**komunikat o błędzie**

```json
"Message": "Out of order event(s) received."
```

## <a name="output-data-errors"></a>Błędów danych wyjściowych

### <a name="outputdataconversionerrorrequiredcolumnmissing"></a>OutputDataConversionError.RequiredColumnMissing

* Przyczyna: Kolumna wymagane dla danych wyjściowych nie istnieje. Na przykład istnieje kolumna zdefiniowana jako hierarchyinfoguid nie jest PartitionKey tabeli platformy Azure.
* Podany powiadomieniu portalu: Tak
* Poziom dzienniki diagnostyczne: Ostrzeżenie
* Wpływ:  Błędy z konwersji danych wszystkie dane wyjściowe z tym brak wymaganej kolumny są obsługiwane zgodnie z opisem w [zasad danych dane wyjściowe](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ustawienie.
* Szczegóły dziennika
   * Nazwa kolumny i identyfikator rekordu lub częścią rekordu.

**komunikat o błędzie**

```json
"Message": "The output record does not contain primary key property: [deviceId] Ensure the query output contains the column [deviceId] with a unique non-empty string less than '255' characters."
```

### <a name="outputdataconversionerrorcolumnnameinvalid"></a>OutputDataConversionError.ColumnNameInvalid

* Przyczyna: Wartość kolumny nie są zgodne z danymi wyjściowymi. Na przykład nazwa kolumny nie jest kolumną prawidłowej tabeli platformy Azure.
* Podany powiadomieniu portalu: Tak
* Poziom dzienniki diagnostyczne: Ostrzeżenie
* Wpływ:  Błędy z konwersji danych wszystkie dane wyjściowe z tym Nieprawidłowa nazwa kolumny są obsługiwane zgodnie z opisem w [zasad danych dane wyjściowe](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ustawienie.
* Szczegóły dziennika
   * Nazwa kolumny i identyfikator rekordu lub częścią rekordu.

**komunikat o błędzie**

```json
"Message": "Invalid property name #deviceIdValue. Please refer MSDN for Azure table property naming convention."
```

### <a name="outputdataconversionerrortypeconversionerror"></a>OutputDataConversionError.TypeConversionError

* Przyczyna: Nie można przekonwertować kolumnę na prawidłowy typ w danych wyjściowych. Na przykład wartość kolumny jest niezgodna z ograniczenia lub typu zdefiniowanego w tabeli SQL.
* Podany powiadomieniu portalu: Tak
* Poziom dzienniki diagnostyczne: Ostrzeżenie
* Wpływ:  Błędy z konwersji danych wszystkie dane wyjściowe z tym błąd konwersji typu są obsługiwane zgodnie z opisem w [zasad danych dane wyjściowe](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ustawienie.
* Szczegóły dziennika
   * Nazwa kolumny.
   * Identyfikator rekordu lub częścią rekordu.

**komunikat o błędzie**

```json
"Message": "The column [id] value null or its type is invalid. Ensure to provide a unique non-empty string less than '255' characters."
```

### <a name="outputdataconversionerrorrecordexceededsizelimit"></a>OutputDataConversionError.RecordExceededSizeLimit

* Przyczyna: Wartość komunikatu jest większy niż rozmiar obsługiwanych danych wyjściowych. Na przykład rekord jest większy niż 1 MB dla danych wyjściowych Centrum zdarzeń.
* Podany powiadomieniu portalu: Tak
* Poziom dzienniki diagnostyczne: Ostrzeżenie
* Wpływ:  Błędy z konwersji danych wszystkie dane wyjściowe z tym limit rozmiaru Przekroczono rekordu są obsługiwane zgodnie z opisem w [zasad danych dane wyjściowe](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ustawienie.
* Szczegóły dziennika
   * Identyfikator rekordu lub częścią rekordu.

**komunikat o błędzie**

```json
"BriefMessage": "Single output event exceeds the maximum message size limit allowed (262144 bytes) by Event Hub."
```

### <a name="outputdataconversionerrorduplicatekey"></a>OutputDataConversionError.DuplicateKey

* Przyczyna: Rekord zawiera już kolumnę z taką samą nazwę jak kolumna systemowa. Na przykład danych wyjściowych cosmos DB, za pomocą kolumny o nazwie identyfikator, gdy kolumna ID jest do innej kolumny.
* Podany powiadomieniu portalu: Yes
* Poziom dzienniki diagnostyczne: Ostrzeżenie
* Wpływ:  Błędy z konwersji danych wszystkie dane wyjściowe z tym zduplikowanego klucza są obsługiwane zgodnie z opisem w [zasad danych dane wyjściowe](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ustawienie.
* Szczegóły dziennika
   * Nazwa kolumny.
   * Identyfikator rekordu lub częścią rekordu.

```json
"BriefMessage": "Column 'devicePartitionKey' is being mapped to multiple columns."
```

## <a name="next-steps"></a>Kolejne kroki

* [Rozwiązywanie problemów z usługą Azure Stream Analytics przy użyciu dzienników diagnostycznych](stream-analytics-job-diagnostic-logs.md)

* [Omówienie monitorowania zadań usługi Stream Analytics oraz monitorowanie zapytań](stream-analytics-monitoring.md)
