---
title: Pojęcia, terminologia oraz jednostki — Azure Scheduler | Microsoft Docs
description: Poznaj pojęcia, terminologię oraz hierarchię jednostek, łącznie z zadaniami i kolekcjami zadań, w usłudze Azure Scheduler.
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 3ef16fab-d18a-48ba-8e56-3f3e0a1bcb92
ms.topic: conceptual
ms.date: 08/18/2016
ms.openlocfilehash: d701fba39685d781d1a4c2d8a6cf194ca7eb2908
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60530935"
---
# <a name="concepts-terminology-and-entities-in-azure-scheduler"></a>Pojęcia, terminologia i jednostki w usłudze Azure Scheduler

> [!IMPORTANT]
> Usługa [Azure Logic Apps](../logic-apps/logic-apps-overview.md) zastępuje usługę Azure Scheduler, która zostanie wycofana. Zamiast niej [spróbuj używać usługi Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) do planowania zadań. 

## <a name="entity-hierarchy"></a>Hierarchia jednostek

Interfejs API REST usługi Azure Scheduler zawiera następujące jednostki główne, czyli zasoby, i korzysta z nich:

| Jednostka | Opis |
|--------|-------------|
| **Zadanie** | Definiuje jedną akcję cykliczną powiązaną z prostymi lub złożonymi strategiami wykonania. Akcje mogą obejmować żądania HTTP, żądania kolejki usługi Storage i żądania kolejki usługi Service Bus lub tematu usługi Service Bus. | 
| **Kolekcja zadań** | Zawiera grupę zadań wraz z ustawieniami, przydziałami i limitami wspólnymi dla zadań należących do kolekcji. Jako właściciel subskrypcji platformy Azure możesz tworzyć kolekcje zadań i grupować zadania na podstawie ich użycia lub granic zastosowania. Kolekcja zadań ma następujące atrybuty: <p>– Jest ograniczona do jednego regionu. <br>– Umożliwia wymuszanie limitów przydziałów, dzięki czemu można ograniczyć użycie dla wszystkich zadań w kolekcji. <br>– Przykłady przydziałów to MaxJobs (Maksymalna liczba zadań) i MaxRecurrence (Maksymalna liczba cykli). | 
| **Historia zadania** | Opisuje szczegóły dotyczące wykonania zadania, na przykład stan i wszelkie szczegóły odpowiedzi. |
||| 

## <a name="entity-management"></a>Zarządzanie jednostkami

Na wysokim poziomie interfejs API REST usługi Scheduler zawiera następujące operacje z zakresu zarządzania jednostkami.

### <a name="job-management"></a>Zarządzanie zadaniami

Obsługuje operacje tworzenia i edytowania zadań. Wszystkie zadania muszą należeć do istniejącej kolekcji zadań, w związku z czym nie ma możliwości niejawnego tworzenia. Aby uzyskać więcej informacji, zobacz [Interfejs API REST usługi Scheduler — zadania](https://docs.microsoft.com/rest/api/scheduler/jobs). Oto adresu URI do tych operacji:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}
```

### <a name="job-collection-management"></a>Zarządzanie kolekcją zadań

Obsługuje operacje tworzenia oraz edytowania zadań i kolekcji zadań, które są mapowane na przydziały i wspólne ustawienia. Przydziały dotyczą na przykład maksymalnej liczby zadań oraz najmniejszego interwału cyklu. Aby uzyskać więcej informacji, zobacz [Interfejs API REST usługi Scheduler — kolekcje zadań](https://docs.microsoft.com/rest/api/scheduler/jobcollections). Oto adresu URI do tych operacji:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}
```

### <a name="job-history-management"></a>Zarządzanie historią zadania

Obsługuje operację GET umożliwiającą pobranie historii wykonywania zadania z 60 dni, obejmującej m.in. informacje o czasie, który upłynął podczas zadania, oraz o wynikach wykonania zadania. Obejmuje obsługę parametru ciągu zapytania służącego do filtrowania na podstawie stanu i statusu. Aby uzyskać więcej informacji, zobacz [Interfejs API REST usługi Scheduler — lista historii zadania](https://docs.microsoft.com/rest/api/scheduler/jobs/listjobhistory). Poniżej przedstawiono adres URI dla tej operacji:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history
```

## <a name="job-types"></a>Typy zadań

Usługa Azure Scheduler obsługuje wiele typów zadań: 

* Zadania HTTP, w tym zadania protokołu HTTPS, które obsługują protokół SSL, używane w scenariuszach, gdy znany jest punkt końcowy dla istniejącej usługi lub istniejącego obciążenia
* Zadania kolejki usługi Storage dla obciążeń korzystających z kolejek usługi Storage, takich jak publikowanie komunikatów w kolejkach usługi Storage
* Zadania kolejki usługi Service Bus dla obciążeń korzystających z kolejek usługi Service Bus
* Zadania tematów usługi Service Bus dla obciążeń korzystających z tematów usługi Service Bus

## <a name="job-definition"></a>Definicja zadania

Na wysokim poziomie zadanie usługi Scheduler składa się z następujących elementów podstawowych:

* Akcja uruchamiana po wyzwoleniu czasomierza zadania
* Opcjonalnie: Czas wykonywania zadania
* Opcjonalnie: Kiedy i częstotliwość powtarzania zadania
* Opcjonalnie: Akcja błędu, który jest wykonywany, gdy akcja podstawowa zakończy się niepowodzeniem.

Zadanie zawiera również dane dostarczane przez system, takie jak czas następnego zaplanowanego uruchomienia zadania. Definicją kodu zadania jest obiekt w formacie JavaScript Object Notation (JSON), który zawiera następujące elementy:

| Element | Wymagany | Opis | 
|---------|----------|-------------| 
| [**startTime**](#start-time) | Nie | Godzina rozpoczęcia zadania z przesunięciem strefy czasowej w [formacie ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) | 
| [**action**](#action) | Yes | Szczegóły akcji podstawowej, które mogą zawierać obiekt **errorAction** | 
| [**errorAction**](#error-action) | Nie | Szczegóły akcji pomocniczej, która jest uruchamiana, gdy akcja podstawowa zakończy się niepowodzeniem |
| [**recurrence**](#recurrence) | Nie | Szczegóły, takie jak częstotliwość i interwał, dla zadania cyklicznego | 
| [**retryPolicy**](#retry-policy) | Nie | Szczegóły dotyczące częstotliwości ponawiania próby wykonania akcji | 
| [**state**](#state) | Yes | Szczegóły dotyczące bieżącego stanu zadania |
| [**status**](#status) | Yes | Szczegóły dotyczące bieżącego stanu zadania, który jest kontrolowany przez usługę |
||||

Oto przykład pokazujący kompleksową definicję zadania dla akcji HTTP z pełniejszymi szczegółami elementów opisanymi w kolejnych sekcjach: 

```json
"properties": {
   "startTime": "2012-08-04T00:00Z",
   "action": {
      "type": "Http",
      "request": {
         "uri": "http://contoso.com/some-method", 
         "method": "PUT",          
         "body": "Posting from a timer",
         "headers": {
            "Content-Type": "application/json"
         },
         "retryPolicy": { 
             "retryType": "None" 
         },
      },
      "errorAction": {
         "type": "Http",
         "request": {
            "uri": "http://contoso.com/notifyError",
            "method": "POST"
         }
      }
   },
   "recurrence": {
      "frequency": "Week",
      "interval": 1,
      "schedule": {
         "weekDays": ["Monday", "Wednesday", "Friday"],
         "hours": [10, 22]
      },
      "count": 10,
      "endTime": "2012-11-04"
   },
   "state": "Disabled",
   "status": {
      "lastExecutionTime": "2007-03-01T13:00:00Z",
      "nextExecutionTime": "2007-03-01T14:00:00Z ",
      "executionCount": 3,
      "failureCount": 0,
      "faultedCount": 0
   }
}
```

<a name="start-time"></a>

## <a name="starttime"></a>startTime

W obiekcie **startTime** możesz określić godzinę rozpoczęcia i przesunięcie strefy czasowej w [formacie ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

<a name="action"></a>

## <a name="action"></a>action

Twoje zadanie usługi Scheduler uruchamia **akcję** podstawową na podstawie określonego harmonogramu. Usługa Scheduler obsługuje akcje HTTP, akcje kolejki usługi Storage, akcje kolejki usługi Service Bus i akcje tematów usługi Service Bus. Jeśli **akcja** podstawowa zakończy się niepowodzeniem, usługa Scheduler może uruchomić pomocniczą akcję [**errorAction**](#erroraction), która obsługuje błąd. Obiekt **action** opisuje następujące elementy:

* Typ usługi akcji
* Szczegóły akcji
* Alternatywna akcja **errorAction**

Poprzedni przykład opisuje akcję HTTP. Oto przykład akcji kolejki usługi Storage:

```json
"action": {
   "type": "storageQueue",
   "queueMessage": {
      "storageAccount": "myStorageAccount",  
      "queueName": "myqueue",                
      "sasToken": "TOKEN",                   
      "message": "My message body"
    }
}
```

Oto przykład akcji kolejki usługi Service Bus:

```json
"action": {
   "type": "serviceBusQueue",
   "serviceBusQueueMessage": {
      "queueName": "q1",  
      "namespace": "mySBNamespace",
      "transportType": "netMessaging", // Either netMessaging or AMQP
      "authentication": {  
         "sasKeyName": "QPolicy",
         "type": "sharedAccessKey"
      },
      "message": "Some message",  
      "brokeredMessageProperties": {},
      "customMessageProperties": {
         "appname": "FromScheduler"
      }
   }
},
```

Oto przykład akcji tematu usługi Service Bus:

```json
"action": {
   "type": "serviceBusTopic",
   "serviceBusTopicMessage": {
      "topicPath": "t1",  
      "namespace": "mySBNamespace",
      "transportType": "netMessaging", // Either netMessaging or AMQP
      "authentication": {
         "sasKeyName": "QPolicy",
         "type": "sharedAccessKey"
      },
      "message": "Some message",
      "brokeredMessageProperties": {},
      "customMessageProperties": {
         "appname": "FromScheduler"
      }
   }
},
```

Aby uzyskać więcej informacji na temat tokenów sygnatury dostępu współdzielonego (SAS), zobacz [Autoryzacja za pomocą sygnatur dostępu współdzielonego](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

<a name="error-action"></a>

## <a name="erroraction"></a>errorAction

Jeśli **akcja** podstawowa zadania zakończy się niepowodzeniem, usługa Scheduler może uruchomić akcję **errorAction**, która obsługuje błąd. W **akcji** podstawowej można określić obiekt **errorAction**, aby usługa Scheduler mogła wywołać punkt końcowy obsługi błędów lub wysłać użytkownikowi powiadomienie. 

Jeśli na przykład podstawowy punkt końcowy ulegnie awarii, możesz użyć obiektu **errorAction** w celu wywołania pomocniczego punktu końcowego lub w celu powiadomienia punktu końcowego obsługującego błędy. 

Podobnie jak **akcja** podstawowa, akcja błędu może także charakteryzować się logiką prostą lub złożoną w oparciu o inne akcje. 

<a name="recurrence"></a>

## <a name="recurrence"></a>recurrence

Zadanie jest uruchamiane cyklicznie, jeśli definicja JSON zadania zawiera obiekt **recurrence**, na przykład:

```json
"recurrence": {
   "frequency": "Week",
   "interval": 1,
   "schedule": {
      "hours": [10, 22],
      "minutes": [0, 30],
      "weekDays": ["Monday", "Wednesday", "Friday"]
   },
   "count": 10,
   "endTime": "2012-11-04"
},
```

| Właściwość | Wymagany | Value | Opis | 
|----------|----------|-------|-------------| 
| **frequency** | Tak, gdy jest używany obiekt **recurrence** | „Minute”, „Hour”, „Day”, „Week”, „Month”, „Year” | Jednostka czasu między wystąpieniami | 
| **interval** | Nie | od 1 do 1000 (włącznie) | Dodatnia liczba całkowita określająca liczbę jednostek czasu między każdym wystąpieniem na podstawie właściwości **frequency** (częstotliwość) | 
| **schedule** | Nie | Różna | Szczegółowe informacje dla bardziej złożonych i zaawansowanych harmonogramów. Zobacz właściwości **hours**, **minutes**, **weekDays**, **months** i **monthDays** | 
| **hours** | Nie | Od 1 do 24 | Tablica z oznaczeniami godzin dla czasu uruchomienia zadania | 
| **minutes** | Nie | od 0 do 59 | Tablica z oznaczeniami minut dla czasu uruchomienia zadania | 
| **months** | Nie | Od 1 do 12 | Tablica z miesiącami dla czasu uruchomienia zadania | 
| **monthDays** | Nie | Różna | Tablica z dniami miesiąca dla czasu uruchomienia zadania | 
| **weekDays** | Nie | „Monday”, „Tuesday”, „Wednesday”, „Thursday”, „Friday”, „Saturday” lub „Sunday” | Tablica z dniami tygodnia dla czasu uruchomienia zadania | 
| **count** | Nie | <*none*> | Liczba cykli. Wartość domyślna to cykl nieskończony. Nie można używać jednocześnie właściwości **count** i **endTime**. Obowiązuje zasada, że uwzględniana jest wartość, która kończy zadanie jako pierwsza. | 
| **endTime** | Nie | <*none*> | Data i godzina zakończenia cyklu. Wartość domyślna to cykl nieskończony. Nie można używać jednocześnie właściwości **count** i **endTime**. Obowiązuje zasada, że uwzględniana jest wartość, która kończy zadanie jako pierwsza. | 
||||

Aby uzyskać więcej informacji na temat tych elementów, zobacz [Tworzenie złożonych i zaawansowanych harmonogramów cyklicznych](../scheduler/scheduler-advanced-complexity.md).

<a name="retry-policy"></a>

## <a name="retrypolicy"></a>retryPolicy

Dla przypadków, w których zadanie usługi Scheduler może zakończyć się niepowodzeniem, możesz skonfigurować zasadę ponawiania określającą, czy i w jaki sposób usługa Scheduler ponawia akcję. Domyślnie usługa Scheduler ponawia zadanie cztery razy w odstępach 30 sekund. Tę zasadę można zmienić tak, aby była bardziej lub mniej agresywna. Na przykład poniższa zasada ponawia akcję dwa razy dziennie:

```json
"retryPolicy": { 
   "retryType": "Fixed",
   "retryInterval": "PT1D",
   "retryCount": 2
},
```

| Właściwość | Wymagany | Value | Opis | 
|----------|----------|-------|-------------| 
| **retryType** | Yes | **Fixed**, **None** | Określa, czy zasada ponawiania jest zdefiniowana (**fixed**), czy nie (**none**). | 
| **retryInterval** | Nie | PT30S | Określa interwał i częstotliwość między ponownymi próbami w [formacie ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Wartość minimalna wynosi 15 sekund, natomiast wartość maksymalna to 18 miesięcy. | 
| **retryCount** | Nie | 4 | Określa liczbę ponownych prób. Wartość maksymalna to 20. | 
||||

Aby uzyskać więcej informacji, zobacz [Wysoka dostępność i niezawodność](../scheduler/scheduler-high-availability-reliability.md).

<a name="status"></a>

## <a name="state"></a>state

Stan zadania może mieć wartość **Enabled** (włączone), **Disabled** (wyłączone), **Completed** (ukończone) lub **Faulted** (wystąpił błąd), na przykład: 

`"state": "Disabled"`

Aby zmienić stan zadania na **Enabled** lub **Disabled**, można użyć operacji PUT lub PATCH na tych zadaniach.
Jeśli jednak zadanie ma stan **Completed** lub **Faulted**, nie można zaktualizować stanu, ale można wykonać operację DELETE na zadaniu. Usługa Scheduler usuwa zadania ukończone oraz zadania z błędami po upływie 60 dni. 

<a name="status"></a>

## <a name="status"></a>status

Po rozpoczęciu zadania usługa Scheduler zwraca informacje dotyczące statusu zadania za pośrednictwem obiektu **status**, który kontroluje tylko usługa Scheduler. Jednakże możesz znaleźć obiekt **status** wewnątrz obiektu **job**. Poniżej przedstawiono informacje, które zawiera status zadania:

* Czas poprzedniego wykonywania, jeśli istnieje
* Czas następnego zaplanowanego wykonania dla zadań w toku
* Liczba wykonań zadania
* Liczba niepowodzeń, jeśli wystąpiły
* Liczba błędów, jeśli wystąpiły

Na przykład:

```json
"status": {
   "lastExecutionTime": "2007-03-01T13:00:00Z",
   "nextExecutionTime": "2007-03-01T14:00:00Z ",
   "executionCount": 3,
   "failureCount": 0,
   "faultedCount": 0
}
```

## <a name="see-also"></a>Zobacz też

* [Czym jest Azure Scheduler?](scheduler-intro.md)
* [Pojęcia, terminologia oraz hierarchia jednostek](scheduler-concepts-terms.md)
* [Tworzenie złożonych i zaawansowanych harmonogramów cyklicznych](scheduler-advanced-complexity.md)
* [Limity, limity przydziału, wartości domyślne i kody błędów](scheduler-limits-defaults-errors.md)
* [Dokumentacja interfejsu API REST usługi Azure Scheduler](/rest/api/scheduler)
* [Dokumentacja poleceń cmdlet programu PowerShell dla usługi Azure Scheduler](scheduler-powershell-reference.md)
