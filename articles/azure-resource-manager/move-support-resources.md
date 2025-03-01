---
title: Przenoszenie obsługi operacji przez typ zasobu platformy Azure
description: Wyświetla listę typów zasobów platformy Azure, które mogą zostać przeniesione do nowej grupy zasobów lub subskrypcji.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 6/6/2019
ms.author: tomfitz
ms.openlocfilehash: 9ab8fbd8fa0453ca6c89f3e7ad91bea95b0b9096
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331988"
---
# <a name="move-operation-support-for-resources"></a>Przenoszenie obsługi operacji dla zasobów
W tym artykule wymieniono, czy typ zasobu platformy Azure obsługuje operacji przenoszenia. Mimo że typ zasobu obsługuje operację przenoszenia, może to być warunki, które uniemożliwiają przenoszonego zasobu. Aby uzyskać szczegółowe informacje o warunkach, które wpływają na operacji przenoszenia, zobacz [przenoszenie zasobów do nowej grupy zasobów lub subskrypcji](resource-group-move-resources.md).

Aby uzyskać te same dane w formacie wartości rozdzielanych przecinkami, Pobierz [move-pomocy technicznej — resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv).

## <a name="microsoftaad"></a>Microsoft.AAD
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| domainservices | Nie | Nie |

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Dzierżaw | Nie | Nie |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| actionrules | Tak | Yes |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Serwery | Tak | Yes |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| usługa | Yes | Tak |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| configurationstores | Tak | Tak |

## <a name="microsoftappservice"></a>Microsoft.AppService
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| aplikacje interfejsów API | Nie | Nie |
| appidentities | Nie | Nie |
| Bramy | Nie | Nie |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| policyassignments | Nie | Nie |

## <a name="microsoftautomation"></a>Microsoft.Automation
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| automationaccounts | Tak | Tak |
| automationaccounts/configurations | Tak | Tak |
| automationaccounts/elementów runbook | Tak | Tak |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| b2cdirectories | Tak | Tak |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| rejestracje | Tak | Tak |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| backupvault | Nie | Nie |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| batchaccounts | Tak | Yes |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Klastry | Nie | Nie |
| fileservers | Nie | Nie |
| Zadania | Nie | Nie |
| Obszary robocze | Nie | Nie |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| mapapis | Nie | Nie |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| biztalk | Tak | Tak |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| blockchainmembers | Tak | Tak |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| blueprintassignments | Nie | Nie |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| botservices | Tak | Tak |

## <a name="microsoftcache"></a>Microsoft.Cache
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Redis Cache | Yes | Tak |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Profile | Tak | Tak |
| Profile/punktów końcowych | Tak | Tak |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| zamówień certyfikatów | Tak | Tak |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| domainnames | Yes | Nie |
| maszyn wirtualnych | Yes | Nie |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| networksecuritygroups | Nie | Nie |
| reservedips | Nie | Nie |
| virtualnetworks | Nie | Nie |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| storageaccounts | Tak | Nie |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| accounts | Tak | Tak |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| availabilitysets | Tak | Yes |
| Dyski | Tak | Yes |
| Galerie | Nie | Nie |
| galerie/obrazów | Nie | Nie |
| Galerie/obrazy/wersji | Nie | Nie |
| hostgroups | Nie | Nie |
| hostgroups/hosts | Nie | Nie |
| images | Tak | Yes |
| proximityplacementgroups | Nie | Nie |
| restorepointcollections | Nie | Nie |
| sharedvmimages | Nie | Nie |
| sharedvmimages/wersji | Nie | Nie |
| Migawki | Tak | Tak |
| maszyn wirtualnych | Tak | Tak |
| maszyn wirtualnych/rozszerzenia | Yes | Tak |
| virtualmachinescalesets | Tak | Yes |

## <a name="microsoftcontainer"></a>Microsoft.Container
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| containergroups | Nie | Nie |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| containergroups | Nie | Nie |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| rejestry | Yes | Tak |
| rejestry/buildtasks | Tak | Tak |
| rejestry/replikacji | Tak | Tak |
| rejestry/zadań | Tak | Tak |
| rejestry/elementów webhook | Tak | Yes |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| containerservices | Nie | Nie |
| managedclusters | Nie | Nie |
| openshiftmanagedclusters | Nie | Nie |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| aplikacje | Yes | Yes |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| accounts | Nie | Nie |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| przedsiębiorstw | Tak | Tak |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Koncentratory | Tak | Yes |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Zadania | Nie | Nie |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| databoxedgedevices | Nie | Nie |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Obszary robocze | Nie | Nie |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| katalogi | Tak | Tak |
| datacatalogs | Nie | Nie |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| connectionmanagers | Nie | Nie |

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Pakiety | Nie | Nie |
| Plany | Nie | Nie |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| datafactories | Tak | Tak |
| fabryki | Tak | Tak |

## <a name="microsoftdatalake"></a>Microsoft.DataLake
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| datalakeaccounts | Nie | Nie |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| accounts | Tak | Tak |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| accounts | Tak | Yes |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| services | Nie | Nie |
| usługi/projekty | Nie | Nie |
| gniazda | Nie | Nie |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Serwery | Yes | Tak |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Serwery | Yes | Yes |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| servergroups | Nie | Nie |
| Serwery | Tak | Tak |
| serversv2 | Tak | Tak |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| artifactsources | Tak | Tak |
| Wprowadzanie | Tak | Tak |
| servicetopologies | Yes | Yes |
| servicetopologies/usługi | Yes | Tak |
| servicetopologies/services/serviceunits | Tak | Tak |
| kroki | Tak | Tak |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| elasticpools | Nie | Nie |
| elasticpools/iothubtenants | Nie | Nie |
| iothubs | Yes | Yes |
| provisioningservices | Tak | Yes |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Kontrolery | Nie | Nie |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| labcenters | Nie | Nie |
| Warsztaty | Tak | Nie |
| laboratoria/środowisk | Yes | Tak |
| laboratoria/servicerunners | Yes | Tak |
| laboratoria/maszyn wirtualnych | Yes | Nie |
| Harmonogramy | Yes | Tak |

## <a name="microsoftdns"></a>microsoft.dns
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| dnszones | Nie | Nie |
| dnszones/a | Nie | Nie |
| dnszones/aaaa | Nie | Nie |
| dnszones/cname | Nie | Nie |
| dnszones/mx | Nie | Nie |
| dnszones/ptr | Nie | Nie |
| dnszones/srv | Nie | Nie |
| dnszones/txt | Nie | Nie |
| trafficmanagerprofiles | Nie | Nie |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| databaseaccounts | Tak | Tak |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| domeny | Yes | Tak |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| services | Yes | Yes |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| domeny | Tak | Tak |
| Tematy | Yes | Tak |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Klastry | Yes | Tak |
| Przestrzenie nazw | Tak | Tak |

## <a name="microsoftgenomics"></a>Microsoft.Genomics
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| accounts | Nie | Nie |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| hanainstances | Tak | Tak |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Klastry | Tak | Tak |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| services | Tak | Tak |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| maszyny | Nie | Nie |

## <a name="microsofthybriddata"></a>Microsoft.HybridData
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| datamanagers | Tak | Tak |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Zadania | Tak | Tak |

## <a name="microsoftinsights"></a>microsoft.insights
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| accounts | Nie | Nie |
| actiongroups | Tak | Yes |
| alerty dziennika aktywności | Nie | Nie |
| alertrules | Tak | Yes |
| wartość autoscalesettings | Yes | Yes |
| Składniki | Yes | Tak |
| guestdiagnosticsettings | Nie | Nie |
| metricalerts | Nie | Nie |
| notificationgroups | Nie | Nie |
| notificationrules | Nie | Nie |
| scheduledqueryrules | Tak | Tak |
| testy internetowe | Yes | Yes |
| Skoroszyty | Tak | Tak |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| iotapps | Yes | Tak |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| checknameavailability | Yes | Tak |
| graph | Tak | Yes |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| hsmpools | Nie | Nie |
| Magazyny | Tak | Tak |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Klastry | Tak | Tak |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| labaccounts | Yes | Yes |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| accounts | Tak | Tak |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| accounts | Nie | Nie |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| hostingenvironments | Nie | Nie |
| integrationaccounts | Tak | Yes |
| integrationserviceenvironments | Nie | Nie |
| isolatedenvironments | Nie | Nie |
| Przepływy pracy | Tak | Tak |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| commitmentplans | Tak | Tak |
| usługi sieci Web | Tak | Nie |
| Obszary robocze | Yes | Tak |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| operationalizationclusters | Yes | Tak |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| accounts | Nie | Nie |
| konta/obszarów roboczych | Nie | Nie |
| obszary robocze/kont/projektów | Nie | Nie |
| teamaccounts | Nie | Nie |
| teamaccounts/obszarów roboczych | Nie | Nie |
| obszary robocze/teamaccounts/projektów | Nie | Nie |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| accounts | Tak | Tak |

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft.MachineLearningOperationalization
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| hostingaccounts | Nie | Nie |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Obszary robocze | Nie | Nie |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| userassignedidentities | Nie | Nie |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| accounts | Tak | Tak |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| classicdevservices | Nie | Nie |

## <a name="microsoftmedia"></a>Microsoft.Media
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| mediaservices | Tak | Tak |
| mediaservices/liveevents | Tak | Tak |
| mediaservices/punkty | Tak | Yes |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| assessmentprojects | Nie | Nie |
| migrateprojects | Nie | Nie |
| Projekty | Nie | Nie |

## <a name="microsoftnetapp"></a>Microsoft.NetApp
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| netappaccounts | Nie | Nie |
| netappaccounts/capacitypools | Nie | Nie |
| netappaccounts/capacitypools/volumes | Nie | Nie |
| netappaccounts/capacitypools/volumes/mounttargets | Nie | Nie |
| netappaccounts/capacitypools/volumes/snapshots | Nie | Nie |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| applicationgateways | Nie | Nie |
| applicationgatewaywebapplicationfirewallpolicies | Nie | Nie |
| applicationsecuritygroups | Tak | Tak |
| azurefirewalls | Yes | Tak |
| bastionhosts | Nie | Nie |
| Połączenia | Yes | Tak |
| ddoscustompolicies | Tak | Tak |
| ddosprotectionplans | Nie | Nie |
| dnszones | Tak | Tak |
| expressroutecircuits | Nie | Nie |
| expressroutecrossconnections | Nie | Nie |
| expressroutegateways | Nie | Nie |
| expressrouteports | Nie | Nie |
| frontdoors | Nie | Nie |
| frontdoorwebapplicationfirewallpolicies | Nie | Nie |
| loadbalancers | Yes | Tak |
| localnetworkgateways | Tak | Tak |
| natgateways | Tak | Yes |
| networkintentpolicies | Tak | Tak |
| interfejsy | Tak | Tak |
| networkprofiles | Nie | Nie |
| networksecuritygroups | Yes | Tak |
| networkwatchers | Tak | Yes |
| networkwatchers/connectionmonitors | Tak | Tak |
| networkwatchers/obiektywów | Tak | Tak |
| networkwatchers/pingmeshes | Tak | Tak |
| p2svpngateways | Nie | Nie |
| privatednszones | Tak | Tak |
| privatednszones/virtualnetworklinks | Tak | Yes |
| privateendpoints | Nie | Nie |
| privatelinkservices | Nie | Nie |
| publicipaddresses | Tak | Yes |
| publicipprefixes | Tak | Tak |
| routefilters | Nie | Nie |
| routetables | Tak | Yes |
| securegateways | Yes | Tak |
| serviceendpointpolicies | Yes | Tak |
| trafficmanagerprofiles | Yes | Tak |
| virtualhubs | Nie | Nie |
| virtualnetworkgateways | Yes | Yes |
| virtualnetworks | Tak | Tak |
| virtualnetworktaps | Nie | Nie |
| virtualwans | Nie | Nie |
| vpngateways | Nie | Nie |
| vpnsites | Nie | Nie |
| webapplicationfirewallpolicies | Yes | Tak |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Przestrzenie nazw | Yes | Tak |
| przestrzenie nazw/notificationhubs | Tak | Tak |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Obszary robocze | Yes | Yes |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| managementconfigurations | Yes | Yes |
| rozwiązania | Yes | Tak |
| Widoki | Tak | Tak |

## <a name="microsoftpeering"></a>Microsoft.Peering
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Komunikacja równorzędna | Nie | Nie |

## <a name="microsoftportal"></a>Microsoft.Portal
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Pulpity nawigacyjne | Tak | Tak |

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| rootresources | Nie | Nie |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| workspacecollections | Tak | Yes |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Pojemności | Tak | Yes |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| accounts | Nie | Nie |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Magazyny | Tak | Yes |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Przestrzenie nazw | Tak | Tak |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| aplikacje | Tak | Nie |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| przepływy | Tak | Tak |
| kolekcjach | Tak | Yes |

## <a name="microsoftsearch"></a>Microsoft.Search
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| searchservices | Yes | Yes |

## <a name="microsoftsecurity"></a>Microsoft.Security
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| iotsecuritysolutions | Tak | Tak |

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Bramy | Nie | Nie |
| węzły | Nie | Nie |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Przestrzenie nazw | Yes | Tak |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| aplikacje | Nie | Nie |
| Klastry | Tak | Tak |
| containergroups | Nie | Nie |
| containergroupsets | Nie | Nie |
| edgeclusters | Nie | Nie |
| Sieci | Nie | Nie |
| secretstores | Nie | Nie |
| volumes | Nie | Nie |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| aplikacje | Tak | Tak |
| containergroups | Nie | Nie |
| Bramy | Yes | Tak |
| Sieci | Yes | Tak |
| wpisy tajne | Tak | Tak |
| volumes | Tak | Tak |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| signalr | Tak | Tak |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| siterecoveryvault | Nie | Nie |

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| appliancedefinitions | Nie | Nie |
| urządzenia | Nie | Nie |
| applicationdefinitions | Nie | Nie |
| aplikacje | Nie | Nie |
| jitrequests | Nie | Nie |

## <a name="microsoftsql"></a>Microsoft.Sql
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| instancepools | Yes | Tak |
| managedinstances | Yes | Yes |
| managedinstances/baz danych | Yes | Yes |
| Serwery | Yes | Yes |
| serwery/baz danych | Tak | Tak |
| servers/elasticpools | Tak | Tak |
| virtualclusters | Tak | Tak |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| sqlvirtualmachinegroups | Tak | Tak |
| sqlvirtualmachines | Yes | Tak |

## <a name="microsoftsqlvm"></a>Microsoft.SqlVM
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| dwvm | Nie | Nie |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| storageaccounts | Tak | Tak |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Pamięci podręczne | Nie | Nie |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| storagesyncservices | Tak | Yes |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| storagesyncservices | Nie | Nie |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| storagesyncservices | Nie | Nie |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Menedżerowie | Nie | Nie |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| streamingjobs | Yes | Tak |

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Środowiska | Nie | Nie |
| środowiska/eventsources | Nie | Nie |
| wystąpienia | Nie | Nie |
| wystąpienia/środowisk | Nie | Nie |
| środowiska/wystąpienia/eventsources | Nie | Nie |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| providerregistrations | Nie | Nie |
| Zasoby | Nie | Nie |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Środowiska | Tak | Yes |
| środowiska/eventsources | Tak | Tak |
| środowiska/referencedatasets | Tak | Yes |

## <a name="microsofttoken"></a>Microsoft.Token
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| przechowuje | Nie | Nie |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| imagetemplates | Nie | Nie |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| account | Yes | Tak |
| konta i rozszerzeń | Tak | Yes |
| konto lub projekt docelowy | Tak | Tak |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| dedicatedcloudnodes | Tak | Tak |
| dedicatedcloudservices | Tak | Yes |
| maszyn wirtualnych | Tak | Tak |

## <a name="microsoftweb"></a>Microsoft.Web
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| Certyfikaty | Nie | Yes |
| connectiongateways | Tak | Tak |
| Połączenia | Yes | Yes |
| customapis | Tak | Tak |
| hostingenvironments | Nie | Nie |
| farm serwerów | Tak | Yes |
| Lokacje | Tak | Tak |
| sites/premieraddons | Tak | Tak |
| Lokacje/miejsca | Tak | Tak |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| deviceservices | Nie | Nie |

## <a name="microsoftwindowsvirtualdesktop"></a>Microsoft.WindowsVirtualDesktop
| Typ zasobu | Grupa zasobów | Subskrypcja |
| ------------- | ----------- | ---------- |
| applicationgroups | Nie | Nie |
| hostpools | Nie | Nie |
| Obszary robocze | Nie | Nie |

## <a name="third-party-services"></a>Usługi innych firm

Usługi innych firm nie obsługują obecnie operacji przenoszenia.

## <a name="next-steps"></a>Kolejne kroki
Poleceń przenieść zasoby można znaleźć [przenoszenie zasobów do nowej grupy zasobów lub subskrypcji](resource-group-move-resources.md).
