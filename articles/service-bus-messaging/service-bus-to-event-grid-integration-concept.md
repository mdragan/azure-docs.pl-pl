---
title: Omówienie integracji usług Azure Service Bus i Event Grid | Microsoft Docs
description: Opis integracji wiadomości usługi Service Bus z usługą Event Grid
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: aschhab
ms.openlocfilehash: 5d4ece6b631882200c6f98f6de5daa543fdf7ce4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072132"
---
# <a name="azure-service-bus-to-event-grid-integration-overview"></a>Omówienie integracji usług Azure Service Bus i Event Grid

W usłudze Azure Service Bus uruchomiono nową opcję integracji z usługą Azure Event Grid. Kluczowy scenariusz tej funkcji polega na tym, że subskrypcje lub kolejki usługi Service Bus z małą liczbą komunikatów nie muszą mieć odbiornika przeprowadzającego sondowanie w poszukiwaniu komunikatów. 

Usługa Service Bus może teraz emitować zdarzenia do usługi Event Grid, jeśli kolejka lub subskrypcja zawiera komunikaty, a nie ma żadnych odbiorników. Można tworzyć subskrypcje usługi Event Grid powiązane z przestrzeniami nazw usługi Service Bus, nasłuchiwać tych zdarzeń, a następnie reagować na nie przez uruchomienie odbiornika. W przypadku tej funkcji usługa Service Bus może być używana w reaktywnych modelach programowania.

Do włączenia tej funkcji potrzebne są następujące elementy:

* Przestrzeń nazw usługi Service Bus w warstwie Premium z co najmniej jedną kolejką lub tematem usługi Service Bus i co najmniej jedną subskrypcją.
* Dostęp współautora do przestrzeni nazw usługi Service Bus.
* Ponadto potrzebujesz subskrypcji usługi Event Grid dla przestrzeni nazw usługi Service Bus. Ta subskrypcja otrzymuje z usługi Event Grid powiadomienia o komunikatach do pobrania. Typowymi subskrybentami mogą być funkcja Logic Apps usługi Azure App Service, funkcje Azure Functions lub element webhook kontaktujący się z aplikacją internetową. Następnie subskrybent przetwarza komunikaty. 

![19][]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="verify-that-you-have-contributor-access"></a>Weryfikowanie dostępu współautora
Przejdź do przestrzeni nazw usługi Service Bus, a następnie wybierz opcję **Kontrola dostępu (IAM)** i kartę **Przypisania ról**. Sprawdź, czy masz dostęp współautora do przestrzeni nazw. 

### <a name="events-and-event-schemas"></a>Zdarzenia i schematy zdarzeń

Obecnie usługa Service Bus wysyła zdarzenia w przypadku dwóch scenariuszy:

* [ActiveMessagesWithNoListenersAvailable](#active-messages-available-event)
* DeadletterMessagesAvailable

Ponadto usługa Service Bus używa standardowych zabezpieczeń usługi Event Grid i [mechanizmów uwierzytelniania](https://docs.microsoft.com/azure/event-grid/security-authentication).

Aby uzyskać więcej informacji, zobacz [Azure Event Grid event schemas (Schematy zdarzeń usługi Azure Event Grid)](https://docs.microsoft.com/azure/event-grid/event-schema).

#### <a name="active-messages-available-event"></a>Zdarzenie Dostępne aktywne komunikaty

To zdarzenie jest generowane, jeśli masz aktywne komunikaty w kolejce lub subskrypcji i nie ma nasłuchujących odbiorników.

Schemat tego zdarzenia jest następujący:

```JSON
{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}
```

#### <a name="dead-letter-messages-available-event"></a>Zdarzenie Dostępne komunikaty utracone

Odbierasz co najmniej jedno zdarzenie na kolejkę utraconych komunikatów, która zawiera komunikaty, ale nie ma aktywnych odbiorników.

Schemat tego zdarzenia jest następujący:

```JSON
[{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="how-many-events-are-emitted-and-how-often"></a>Ile zdarzeń jest emitowanych i jak często?

Jeśli masz wiele kolejek i tematów/subskrypcji w przestrzeni nazw, odbierasz co najmniej jedno zdarzenie na kolejkę i jedno na subskrypcję. Zdarzenia są emitowane natychmiast, jeśli w jednostce usługi Service Bus nie ma komunikatów i nadejdzie nowy komunikat. W przeciwnym razie zdarzenia są emitowane co dwie minuty, chyba że usługa Service Bus wykryje aktywny odbiornik. Przeglądanie wiadomości nie przerywa zdarzeń.

Domyślnie usługa Service Bus emituje zdarzenia dla wszystkich jednostek w przestrzeni nazw. Jeśli chcesz odbierać zdarzenia tylko dla konkretnych jednostek, zapoznaj się z kolejną sekcją.

### <a name="use-filters-to-limit-where-you-get-events-from"></a>Odbieranie zdarzeń tylko z wybranych jednostek przy użyciu filtrów

Jeśli na przykład chcesz odbierać zdarzenia tylko z jednej kolejki lub jednej subskrypcji w przestrzeni nazw, możesz użyć filtrów *Zaczyna się od* i *Kończy się na* udostępnianych przez usługę Event Grid. W niektórych interfejsach filtry mają nazwy *Prefiks* i *Sufiks*. Jeśli chcesz odbierać zdarzenia z wielu, ale nie wszystkich, kolejek i subskrypcji, możesz utworzyć wiele subskrypcji usługi Event Grid i określić filtr dla każdej z nich.

## <a name="create-event-grid-subscriptions-for-service-bus-namespaces"></a>Tworzenie subskrypcji usługi Event Grid dla przestrzeni nazw usługi Service Bus

Istnieją trzy różne sposoby tworzenia subskrypcji usługi Event Grid dla przestrzeni nazw usługi Service Bus:

* W witrynie Azure Portal
* W [interfejsie wiersza polecenia platformy Azure](#azure-cli-instructions)
* W programie [PowerShell](#powershell-instructions)

## <a name="azure-portal-instructions"></a>Instrukcje dotyczące witryny Azure Portal

Aby utworzyć nową subskrypcję usługi Event Grid, wykonaj następujące czynności:
1. W witrynie Azure Portal przejdź do przestrzeni nazw.
2. W okienku po lewej stronie wybierz pozycję **Event Grid**. 
3. Wybierz pozycję **Subskrypcja zdarzeń**.  

   Poniższa ilustracja przedstawia przestrzeń nazw z subskrypcją usługi Event Grid:

   ![Subskrypcje usługi Event Grid](./media/service-bus-to-event-grid-integration-concept/sbtoeventgridportal.png)

   Na poniższej ilustracji przedstawiono sposób subskrybowania funkcji lub elementu webhook bez żadnego konkretnego filtru:

   ![21][]

## <a name="azure-cli-instructions"></a>Instrukcje dotyczące interfejsu wiersza polecenia platformy Azure

Najpierw upewnij się, że zainstalowano interfejs wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. [Pobierz instalator](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Naciśnij klawisze **Windows + X**, a następnie otwórz nową konsolę programu PowerShell z uprawnieniami administratora. Możesz również użyć powłoki poleceń w witrynie Azure Portal.

Wykonaj następujący kod:

 ```azurecli-interactive
az login

az account set -s "<Azure subscription name>"

namespaceid=$(az resource show --namespace Microsoft.ServiceBus --resource-type namespaces --name "<service bus namespace>" --resource-group "<resource group that contains the service bus namespace>" --query id --output tsv

az eventgrid event-subscription create --resource-id $namespaceid --name "<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>" --endpoint "<your_function_url>" --subject-ends-with "<YOUR SERVICE BUS SUBSCRIPTION NAME>"
```

Jeśli używasz powłoki BASH 

## <a name="powershell-instructions"></a>Instrukcje dotyczące programu PowerShell

Upewnij się, że masz zainstalowany program Azure PowerShell. [Pobierz instalator](https://docs.microsoft.com/powershell/azure/install-Az-ps). Naciśnij klawisze **Windows + X** i otwórz nową konsolę programu PowerShell z uprawnieniami administratora. Możesz również użyć powłoki poleceń w witrynie Azure Portal.

```powershell-interactive
Connect-AzAccount

Select-AzSubscription -SubscriptionName "<YOUR SUBSCRIPTION NAME>"

# This might be installed already
Install-Module Az.ServiceBus

$NSID = (Get-AzServiceBusNamespace -ResourceGroupName "<YOUR RESOURCE GROUP NAME>" -Na
mespaceName "<YOUR NAMESPACE NAME>").Id

New-AzEVentGridSubscription -EventSubscriptionName "<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>" -ResourceId $NSID -Endpoint "<YOUR FUNCTION URL>” -SubjectEndsWith "<YOUR SERVICE BUS SUBSCRIPTION NAME>"
```

W tym miejscu możesz eksplorować inne opcje instalacji lub przetestować przepływające zdarzenia.

## <a name="next-steps"></a>Kolejne kroki

* Pobierz [przykłady](service-bus-to-event-grid-integration-example.md) dla usług Service Bus i Event Grid.
* Dowiedz się więcej o usłudze [Event Grid](https://docs.microsoft.com/azure/event-grid/).
* Dowiedz się więcej na temat usługi [Azure Functions](https://docs.microsoft.com/azure/azure-functions/).
* Dowiedz się więcej na temat usługi [Logic Apps](https://docs.microsoft.com/azure/logic-apps/).
* Dowiedz się więcej na temat usługi [Service Bus](https://docs.microsoft.com/azure/service-bus/).

[1]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgrid1.png
[19]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgriddiagram.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
