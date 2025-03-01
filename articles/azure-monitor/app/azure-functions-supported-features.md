---
title: Usługa Azure Application Insights — usługi Azure Functions obsługiwane funkcje | Dokumentacja firmy Microsoft
description: Usługa Application Insights obsługiwane funkcje dotyczące usługi Azure Functions
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: ''
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 4/23/2019
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: 0199d8f0c4a76a10fffcab7cf2819643d0ac2d68
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075358"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Usługa Application Insights dla usługi Azure Functions obsługiwane funkcje

Platforma Azure oferuje funkcje [wbudowana integracja](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) za pomocą usługi Application Insights, który jest dostępny za pośrednictwem interfejsu ILogger. Poniżej znajduje się lista aktualnie obsługiwanych funkcji. Przejrzyj przewodnik usługi Azure Functions dla [wprowadzenie](https://github.com/Azure/Azure-Functions/wiki/App-Insights).

## <a name="supported-features"></a>Obsługiwane funkcje

| Azure Functions                       | Wersja 1                | W wersji 2 (Konferencja Ignite 2018 r.)  | 
|-----------------------------------    |---------------    |------------------ |
| **Usługa Application Insights SDK platformy .NET**   | **2.5.0**       | **2.9.1**         |
| | | | 
| **Automatyczne zbieranie**        |                 |                   |               
| &bull; Żądania                     | Tak             | Tak               | 
| &bull; Wyjątki                   | Tak             | Tak               | 
| &bull; Liczniki wydajności         | Yes             | Yes               |
| &bull; Zależności                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Yes               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | Tak               | 
| &nbsp;&nbsp;&nbsp;&mdash; Centrum zdarzeń  |                 | Yes               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Tak               | 
| | | | 
| **Obsługiwane funkcje**                |                   |                   |               
| &bull; QuickPulse/LiveMetrics       | Yes             | Tak               | 
| &nbsp;&nbsp;&nbsp;&mdash; Bezpieczny kanał kontrolny|                 | Tak               | 
| &bull; Próbkowania                     | Tak             | Yes               | 
| &bull; Puls                   |                 | Tak               | 
| | | | 
| **Korelacja**                       |                   |                   |               
| &bull; ServiceBus                     |                   | Yes               | 
| &bull; Centrum zdarzeń                       |                   | Tak               | 
| | | | 
| **Można konfigurować**                      |                   |                   |           
| &bull;W pełni konfigurowalne.<br/>Zobacz [usługi Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) instrukcje.<br/>Zobacz [platformy Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) dla wszystkich opcji.               |                   | Tak                   | 


## <a name="performance-counters"></a>Liczniki wydajności

Automatyczne zbieranie liczników wydajności działają tylko Windows maszyn.


## <a name="live-metrics--secure-control-channel"></a>Metryki na żywo i bezpiecznego kanału kontroli

Filtry niestandardowe określonych kryteriów są wysyłane do składnika metryki na żywo w zestaw SDK usługi Application Insights. Filtry potencjalnie mogą zawierać poufne informacje, takie jak customerIDs. Kanał można zabezpieczyć przy użyciu klucza tajnego klucza interfejsu API. Zobacz [bezpieczny kanał kontrolny](https://docs.microsoft.com/azure/azure-monitor/app/live-stream#secure-the-control-channel) instrukcje.

## <a name="sampling"></a>Próbkowanie

Usługa Azure Functions umożliwia pobieranie próbek domyślnie w swojej konfiguracji. Aby uzyskać więcej informacji, zobacz [skonfigurować próbkowania](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling).

Projekt przyjmuje zależności zestawu SDK Application Insights w celu ręcznego telemetrii śledzenia, może wystąpią nietypowe zachowanie konfiguracji pobierania próbek różni się od funkcje konfiguracji pobierania próbek. 

Zalecamy używanie tej samej konfiguracji jako funkcje. Za pomocą **funkcje w wersji 2**, uzyskasz tę samą konfigurację przy użyciu iniekcji zależności w konstruktora:

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;

public class Function1 
{

    private readonly TelemetryClient telemetryClient;

    public Function1(TelemetryConfiguration configuration)
    {
        this.telemetryClient = new TelemetryClient(configuration);
    }

    [FunctionName("Function1")]
    public async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, ILogger logger)
    {
        this.telemetryClient.TrackTrace("C# HTTP trigger function processed a request.");
    }
}
```
