---
title: Usługa Azure Monitor — usługa Azure Application Insights przesłonić domyślne punkty końcowe z zestawu SDK | Dokumentacja firmy Microsoft
description: Zmodyfikuj domyślne punkty końcowe zestawu SDK usługi Azure Application Insights dla regionów, takich jak Azure dla instytucji rządowych.
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mbullwin
ms.openlocfilehash: 6e49fc96a9664b9f37b7b02fc0183f94a05db263
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078445"
---
 # <a name="application-insights-overriding-default-endpoints"></a>Zastępowanie domyślnych punktów końcowych w usłudze Application Insights

Aby wysyłać dane z usługi Application Insights, w niektórych regionach, należy zastąpić domyślny adres punktu końcowego. Każdy zestaw SDK wymaga nieco inne modyfikacje, które są opisane w tym artykule. Te zmiany wymagają dostosowania przykładowy kod i zastępowanie wartości symboli zastępczych `QuickPulse_Endpoint_Address`, `TelemetryChannel_Endpoint_Address`, i `Profile_Query_Endpoint_address` z adresami istniejący punkt końcowy dla określonego regionu. Na końcu tego artykułu zawiera linki do adresy punktów końcowych dla regionów, w którym ta konfiguracja jest wymagana.

## <a name="sdk-code-changes"></a>Zmiany w kodzie zestawu SDK

### <a name="net-with-applicationinsightsconfig"></a>.NET za pomocą pliku applicationinsights.config

```xml
<ApplicationInsights>
  ...
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <QuickPulseServiceEndpoint>Custom_QuickPulse_Endpoint_Address</QuickPulseServiceEndpoint>
    </Add>
  </TelemetryModules>
    ...
  <TelemetryChannel>
     <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </TelemetryChannel>
  ...
  <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
    <ProfileQueryEndpoint>Profile_Query_Endpoint_address</ProfileQueryEndpoint>
  </ApplicationIdProvider>
  ...
</ApplicationInsights>
```

### <a name="net-core"></a>.NET Core

Zmodyfikuj plik appsettings.json w projekcie w następujący sposób, aby dopasować główny punkt końcowy:

```json
"ApplicationInsights": {
    "InstrumentationKey": "instrumentationkey",
    "TelemetryChannel": {
      "EndpointAddress": "TelemetryChannel_Endpoint_Address"
    }
  }
```

Wartości metryk na żywo i punktu końcowego zapytania profilu można ustawić tylko przy użyciu kodu. Aby zastąpić wartości domyślne dla wszystkich wartości punktu końcowego za pośrednictwem kodu, należy wprowadzić następujące zmiany w `ConfigureServices` metody `Startup.cs` pliku:

```csharp
using Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse; //place at top of Startup.cs file

   services.ConfigureTelemetryModule<QuickPulseTelemetryModule>((module, o) => module.QuickPulseServiceEndpoint="QuickPulse_Endpoint_Address");

   services.AddSingleton(new ApplicationInsightsApplicationIdProvider() { ProfileQueryEndpoint = "Profile_Query_Endpoint_address" });

   services.AddSingleton<ITelemetryChannel>(new ServerTelemetryChannel() { EndpointAddress = "TelemetryChannel_Endpoint_Address" });

    //place in ConfigureServices method. If present, place this prior to   services.AddApplicationInsightsTelemetry("instrumentation key");
```

### <a name="java"></a>Java

Zmodyfikuj plik applicationinsights.xml, aby zmienić domyślny adres punktu końcowego.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>ffffeeee-dddd-cccc-bbbb-aaaa99998888</InstrumentationKey>
  <TelemetryModules>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
  </TelemetryModules>
  <TelemetryInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>
  </TelemetryInitializers>
  <!--Add the following Channel value to modify the Endpoint address-->
  <Channel type="com.microsoft.applicationinsights.channel.concrete.inprocess.InProcessTelemetryChannel">
  <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </Channel>
</ApplicationInsights>
```

### <a name="spring-boot"></a>Spring Boot

Modyfikowanie `application.properties` pliku i Dodaj:

```yaml
azure.application-insights.channel.in-process.endpoint-address= TelemetryChannel_Endpoint_Address
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY');
appInsights.defaultClient.config.endpointUrl = "TelemetryChannel_Endpoint_Address"; // ingestion
appInsights.defaultClient.config.profileQueryEndpoint = "Profile_Query_Endpoint_address"; // appid/profile lookup
appInsights.defaultClient.config.quickPulseHost = "QuickPulse_Endpoint_Address"; //live metrics
appInsights.Configuration.start();
```

Punktów końcowych, które można skonfigurować w taki sposób, przy użyciu zmiennych środowiskowych:

```
Instrumentation Key: “APPINSIGHTS_INSTRUMENTATIONKEY”
Profile Endpoint: “Profile_Query_Endpoint_address”
Live Metrics Endpoint: "QuickPulse_Endpoint_Address"
```

### <a name="javascript"></a>JavaScript

```javascript
<script type="text/javascript">
var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){i[e]=function(){var n=arguments;i.queue.push(function(){i[e].apply(i,n)})}}var i={config:e};i.initialize=!0;var a=document,t=window;setTimeout(function(){var n=a.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/next/ai.2.min.js",a.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{i.cookie=a.cookie}catch(e){}i.queue=[],i.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var o="Track"+r[0];if(n("start"+o),n("stop"+o),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var s=t[r];t[r]=function(e,n,a,t,o){var c=s&&s(e,n,a,t,o);return!0!==c&&i["_"+r]({message:e,url:n,lineNumber:a,columnNumber:t,error:o}),c},e.autoExceptionInstrumented=!0}return i}
(
    {
    instrumentationKey:"INSTRUMENTATION_KEY",
    endpointUrl: "TelemetryChannel_Endpoint_Address"
  }
);
window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>

```

## <a name="regions-that-require-endpoint-modification"></a>Regiony, które wymagają modyfikacji punktu końcowego

Obecnie jest to jedyny region, który wymaga modyfikacji punktu końcowego [Azure dla instytucji rządowych](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights).

## <a name="next-step"></a>Następny krok

- Aby dowiedzieć się więcej na temat modyfikacji niestandardowych dla platformy Azure Government, zapoznaj się z szczegółowych wskazówek dotyczących [monitorowania platformy Azure i zarządzania](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights).