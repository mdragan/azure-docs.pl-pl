---
title: Obsługiwane zasobów dla alertów dotyczących metryk w usłudze Azure Monitor
description: Dokumentacja na temat pomocy technicznej, metryk i dzienników dla alertów dotyczących metryk w usłudze Azure Monitor
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 965d1ace2afdad21a069193b508fc2b10fdf4700
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64697225"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Obsługiwane zasobów dla alertów dotyczących metryk w usłudze Azure Monitor

Platforma Azure obsługuje teraz Monitor [nowego typu alertu Metryka](../../azure-monitor/platform/alerts-overview.md) mającego znaczące korzyści w starszej wersji [klasycznego alertów dotyczących metryk](../../azure-monitor/platform/alerts-classic.overview.md). Metryki są dostępne dla [obszerne listy usług systemu Azure](../../azure-monitor/platform/metrics-supported.md). Nowszych alertów obsługuje podzbiór (rosnący) typów zasobów. W tym artykule wymieniono tego podzbioru.

Umożliwia także nowszych alertów metryk na popularnych dane dzienników przechowywane w obszarze roboczym usługi Log Analytics wyodrębnić jako metryki. Aby uzyskać więcej informacji, Wyświetl [alerty metryki dla dzienników](../../azure-monitor/platform/alerts-metric-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Portal, programu PowerShell, interfejsu wiersza polecenia, REST pomocy technicznej
Obecnie można tworzyć nowszych alertów metryk, tylko w witrynie Azure portal, [interfejsu API REST](https://docs.microsoft.com/rest/api/monitor/metricalerts/), lub [szablonów usługi Resource Manager](../../azure-monitor/platform/alerts-metric-create-templates.md). Wkrótce zostanie udostępniona Obsługa konfigurowania nowszych alertów przy użyciu programu PowerShell i wiersza polecenia platformy Azure w wersji 2.0 lub nowszej.

## <a name="metrics-and-dimensions-supported"></a>Metryki i wymiary obsługiwane
Nowszych alertów metryk obsługuje alerty dotyczące metryk, używanego przez wymiary. Wymiarów można użyć do filtrowania swoje metryki na odpowiedni poziom. Wszystkie obsługiwane metryki wraz z odpowiednich wymiarów można przeglądać i wizualizować z [usługi Azure Monitor — Eksplorator metryk](../../azure-monitor/platform/metrics-charts.md).

Poniżej przedstawiono pełną listę źródeł metryk usługi Azure monitor, obsługiwanych przez nowszych alertów:

|Typ zasobu  |Obsługiwane wymiary  | Dostępne metryki|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Yes        | [API Management](../../azure-monitor/platform/metrics-supported.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Tak   | [Konta usługi Automation](../../azure-monitor/platform/metrics-supported.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | ND| [Konta usługi Batch](../../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    ND     |[Azure Cache for Redis](../../azure-monitor/platform/metrics-supported.md#microsoftcacheredis)|
|Microsoft.CognitiveServices/accounts     |    ND     | [Cognitive Services](../../azure-monitor/platform/metrics-supported.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines     |    ND     | [Virtual Machines](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   ND      |[Zestawy skalowania maszyn wirtualnych](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Tak| [Grupy kontenerów](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.ContainerService/managedClusters | Tak | [Zarządzane klastry](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerservicemanagedclusters)|
|Microsoft.DataFactory/datafactories| Tak| [Data Factories V1](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Yes     |[Fabryki danych w wersji 2](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   ND      |[Bazy danych MySQL](../../azure-monitor/platform/metrics-supported.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    ND     | [DB dla PostgreSQL](../../azure-monitor/platform/metrics-supported.md#microsoftdbforpostgresqlservers)|
|Microsoft.Devices/IotHubs    | ND     |[Metryki usługi IoT Hub](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesiothubs)
|Microsoft.Devices/provisioningServices    | Tak     |[Metryki usługi DPS](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesprovisioningservices)
|Microsoft.EventHub/namespaces     |  Tak      |[Event Hubs](../../azure-monitor/platform/metrics-supported.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Nie | [Magazyny](../../azure-monitor/platform/metrics-supported.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     ND    |[Logic Apps](../../azure-monitor/platform/metrics-supported.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    ND     | [Bramy aplikacji](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/dnsZones | ND| [Strefy DNS](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkdnszones) |
|Microsoft.Network/expressRouteCircuits | ND |  [Express Route obwodów](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/loadBalancers (tylko w przypadku standardowej jednostki SKU)| Tak| [Moduły równoważenia obciążenia](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  ND       |[Publiczne adresy IP](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkpublicipaddresses)|
|Microsoft.Network/trafficManagerProfiles | Tak | [Profile usługi Traffic Manager](../../azure-monitor/platform/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.OperationalInsights/workspaces| Yes|[Obszary robocze usługi log Analytics](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces)|
|Microsoft.PowerBIDedicated/capacities | ND | [Pojemności](../../azure-monitor/platform/metrics-supported.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices     |   ND      |[Usługi wyszukiwania](../../azure-monitor/platform/metrics-supported.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Yes       |[Service Bus](../../azure-monitor/platform/metrics-supported.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Yes     | [Konta magazynu](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Yes    | [Obiekt blob usługi](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsblobservices), [usługi plików](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsfileservices), [kolejki usług](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsqueueservices) i [tabeli usług](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  ND       | [Stream Analytics](../../azure-monitor/platform/metrics-supported.md#microsoftstreamanalyticsstreamingjobs)|
| Microsoft.Web/serverfarms | Tak | [Plany usługi App Service](../../azure-monitor/platform/metrics-supported.md#microsoftwebserverfarms)  |
| Microsoft.Web/sites | Tak | [App Services](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-excluding-functions) i [funkcji](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-functions)|
| Microsoft.Web/sites/slots | Tak | [Miejsca usługi App Service](../../azure-monitor/platform/metrics-supported.md#microsoftwebsitesslots)|


## <a name="payload-schema"></a>Ładunek schematu

> [!NOTE]
> Można również użyć [wspólny schemat alertu](https://aka.ms/commonAlertSchemaDocs), zapewniającą zaletą pojedynczej rozszerzalne i ujednolicone ładunku alertu przez ten alert usługi w usłudze Azure Monitor, usługi integracji elementu webhook. [Więcej informacji na temat wspólnej definicji schematów alertu.](https://aka.ms/commonAlertSchemaDefinitions)


Operację POST zawiera następujące ładunek w formacie JSON i schematu dla wszystkich nowszych alertów metryk, jeśli jest odpowiednio skonfigurowany w pobliżu [grupy akcji](../../azure-monitor/platform/action-groups.md) jest używany:

```json
{
  "schemaId": "AzureMonitorMetricAlert",
  "data": {
    "version": "2.0",
    "status": "Activated",
    "context": {
      "timestamp": "2018-02-28T10:44:10.1714014Z",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
      "name": "StorageCheck",
      "description": "",
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "condition": {
        "windowSize": "PT5M",
        "allOf": [
          {
            "metricName": "Transactions",
            "dimensions": [
              {
                "name": "AccountResourceId",
                "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
              },
              {
                "name": "GeoType",
                "value": "Primary"
              }
            ],
            "operator": "GreaterThan",
            "threshold": "0",
            "timeAggregation": "PT5M",
            "metricValue": 1
          }
        ]
      },
      "subscriptionId": "00000000-0000-0000-0000-000000000000",
      "resourceGroupName": "Contoso",
      "resourceName": "diag500",
      "resourceType": "Microsoft.Storage/storageAccounts",
      "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
      "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
    },
    "properties": {
      "key1": "value1",
      "key2": "value2"
    }
  }
}
```

## <a name="next-steps"></a>Kolejne kroki

* Dowiedz się więcej o nowym [alerty środowisko](../../azure-monitor/platform/alerts-overview.md).
* Dowiedz się więcej o [alerty dzienników w usłudze Azure](../../azure-monitor/platform/alerts-unified-log.md).
* Dowiedz się więcej o [alertów na platformie Azure](../../azure-monitor/platform/alerts-overview.md).
