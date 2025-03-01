---
title: Tworzenie usługi Azure Load Balancer przy użyciu interfejsu API REST
titlesuffix: Azure Load Balancer
description: Dowiedz się, jak utworzyć za pomocą interfejsu API REST usługi Azure Load Balancer.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: load-balancer
ms.date: 06/06/2018
ms.author: kumud
ms.openlocfilehash: 159fe9d6a891858d8d2cc2315e9544b79eb44cff
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60884983"
---
# <a name="create-an-azure-basic-load-balancer-using-rest-api"></a>Tworzenie usługi Azure podstawowego modułu równoważenia obciążenia przy użyciu interfejsu API REST

Usługi Azure Load Balancer dystrybuuje nowych przepływów ruchu przychodzącego, przychodzące do frontonu modułu równoważenia obciążenia do wystąpień puli zaplecza, zgodnie z zasadami i sondy kondycji. Moduł równoważenia obciążenia jest dostępny w dwóch jednostek SKU: Podstawowa i Standardowa. Aby zrozumieć różnicę między dwoma wersjami jednostki SKU [porównania jednostki SKU modułu równoważenia obciążenia](load-balancer-overview.md#skus).
 
Niniejszy instruktaż pokazuje, jak utworzyć Azure podstawowego modułu równoważenia obciążenia przy użyciu [interfejsu API REST usługi Azure](/rest/api/azure/) aby ułatwić obciążenia równoważenia przychodzącego żądania na wielu maszynach wirtualnych w ramach sieci wirtualnej platformy Azure. Pełna dokumentacja i dodatkowe przykłady są dostępne w [odwołania REST modułu równoważenia obciążenia Azure](/rest/api/load-balancer/).
 
## <a name="build-the-request"></a>Żądanie kompilacji
Użyj następujące żądanie HTTP PUT, aby utworzyć nowy Azure podstawowego modułu równoważenia obciążenia.
 ```HTTP
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}?api-version=2018-02-01
  ```
### <a name="uri-parameters"></a>Parametry identyfikatora URI

|Name (Nazwa)  |W  |Wymagane |Typ |Opis |
|---------|---------|---------|---------|--------|
|subscriptionId   |  ścieżka       |  True       |   ciąg      |  Poświadczenia subskrypcji, które jednoznacznie identyfikują subskrypcji Microsoft Azure. Identyfikator subskrypcji jest częścią identyfikatora URI, dla każdego wywołania usługi.      |
|resourceGroupName     |     ścieżka    | True        |  ciąg       |   Nazwa grupy zasobów.     |
|loadBalancerName     |  ścieżka       |      True   |    ciąg     |    Nazwa modułu równoważenia obciążenia.    |
|api-version    |   query     |  True       |     ciąg    |  Wersja interfejsu API klienta.      |



### <a name="request-body"></a>Treść żądania

Wymagany jest tylko parametr `location`. Jeżeli nie zdefiniujesz *jednostki SKU* wersji podstawowy moduł równoważenia obciążenia jest domyślnie tworzone.  Użyj [następujące parametry opcjonalne](https://docs.microsoft.com/rest/api/load-balancer/loadbalancers/createorupdate#request-body) do dostosowywania modułu równoważenia obciążenia.

| Name (Nazwa) | Typ | Opis |
| :--- | :--- | :---------- |
| location | ciąg | Lokalizacja zasobu. Pobierz bieżącą listę lokalizacji przy użyciu [listy lokalizacji](https://docs.microsoft.com/rest/api/resources/subscriptions/listlocations) operacji. |


## <a name="example-create-and-update-a-basic-load-balancer"></a>Przykład: Tworzenie i aktualizowanie podstawowego modułu równoważenia obciążenia

W tym przykładzie należy najpierw utworzyć podstawowy moduł równoważenia obciążenia wraz z jej zasobami. Następnie należy skonfigurować zasobów modułu równoważenia obciążenia, które zawierają konfigurację adresu IP frontonu, puli adresów zaplecza, reguły równoważenia obciążenia, sondy kondycji oraz regułę ruchu przychodzącego translatora adresów Sieciowych.

Przed przystąpieniem do tworzenia modułu równoważenia obciążenia przy użyciu w poniższym przykładzie, należy utworzyć sieć wirtualną o nazwie *vnetlb* z podsiecią o nazwie *subnetlb* w grupie zasobów o nazwie *rg1* w **Wschodnie stany USA** lokalizacji.

### <a name="step-1-create-a-basic-load-balancer"></a>KROK 1. Tworzenie podstawowego modułu równoważenia obciążenia
W tym kroku utworzysz podstawowego modułu równoważenia obciążenia, o nazwie *lb* na **wschodnie stany USA** lokalizacji w ramach *rg1* grupy zasobów.
#### <a name="sample-request"></a>Przykładowe żądanie

  ```HTTP    
  PUT https://management.azure.com/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb?api-version=2018-02-01
  ```
#### <a name="request-body"></a>Treść żądania

  ```JSON
   {
    "location": "eastus",
   }
  ```
### <a name="step-2-configure-load-balancer-resources"></a>KROK 2. Konfigurowanie zasobów modułu równoważenia obciążenia
W tym kroku skonfigurujesz modułu równoważenia obciążenia *lb* zasoby, które obejmują konfigurację adresu IP frontonu (*fe-lb*), puli adresów zaplecza (*lb można*), (reguły równoważenia obciążenia *rulelb*), sondy kondycji (*lb sondy*) i przychodzącej reguły NAT (*w nat-rule*).
#### <a name="sample-request"></a>Przykładowe żądanie

  ```HTTP    
  PUT https://management.azure.com/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb?api-version=2018-02-01
  ```
#### <a name="request-body"></a>Treść żądania

  ```JSON
{
  "properties": {
    "frontendIPConfigurations": [
      {
        "name": "fe-lb",
        "properties": {
          "subnet": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/virtualNetworks/vnetlb/subnets/subnetlb"
          },
          "loadBalancingRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/loadBalancingRules/rulelb"
            }  ],
          "inboundNatRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/inboundNatRules/in-nat-rule"
            }  ]  }  }  ],
    "backendAddressPools": [
      {
        "name": "be-lb",
        "properties": {
          "loadBalancingRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/loadBalancingRules/rulelb"
            }  ]   }   }   ],
    "loadBalancingRules": [
      {
        "name": "rulelb",
        "properties": {
          "frontendIPConfiguration": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/frontendIPConfigurations/fe-lb"
          },
          "frontendPort": 80,
          "backendPort": 80,
          "enableFloatingIP": true,
          "idleTimeoutInMinutes": 15,
          "protocol": "Tcp",
          "loadDistribution": "Default",
          "backendAddressPool": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/backendAddressPools/be-lb"
          },
          "probe": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/probes/probe-lb"
          }  }  }  ],
    "probes": [
      {
        "name": "probe-lb",
        "properties": {
          "protocol": "Http",
          "port": 80,
          "requestPath": "healthcheck.aspx",
          "intervalInSeconds": 15,
          "numberOfProbes": 2,
          "loadBalancingRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/loadBalancingRules/rulelb"
            }  ]  }  } ],
    "inboundNatRules": [
      {
        "name": "in-nat-rule",
        "properties": {
          "frontendIPConfiguration": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/frontendIPConfigurations/fe-lb"
          },
          "frontendPort": 3389,
          "backendPort": 3389,
          "enableFloatingIP": true,
          "idleTimeoutInMinutes": 15,
          "protocol": "Tcp"
        } } ],
    "inboundNatPools": [],
    "outboundNatRules": []
  }  }
```
