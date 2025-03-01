---
title: Omówienie usługi Azure przemysłowego Internetu rzeczy | Dokumentacja firmy Microsoft
description: Omówienie przemysłowego Internetu rzeczy
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: bda40470e3ccf3a5d7b23dca38b21090e864b16a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60888483"
---
# <a name="what-is-industrial-iot-iiot"></a>Co to jest przemysłowego Internetu rzeczy (IIoT)

IIoT jest przemysłowego Internetu rzeczy. IIoT zwiększa efektywność przemysłowych za pośrednictwem aplikacji IoT w branży produkcyjnej. 

## <a name="improve-industrial-efficiencies"></a>Poprawa efektywności przemysłowych

Zwiększ produktywność operacyjną i zyskowność dzięki akcelerator rozwiązania połączonej fabryki. Połącz sprzęt i urządzenia przemysłowe — w tym również maszyny, które już pracują w fabryce — i monitoruj je w chmurze. Analizuj dane IoT, aby wyciągać wnioski, które pomogą w zwiększeniu wydajności całej fabryki.

Zmniejszenia czasochłonne, uzyskiwania dostępu do fabryki floor maszyn z bliźniaczej reprezentacji OPC i skoncentrować się czas na tworzenie rozwiązań IIoT. Usprawnianie zarządzania certyfikatami i zasobów przemysłowych integracji z usługą Magazyn OPC i zagwarantować, że łączność zasobów są zabezpieczone. Takich mikrousług zapewnia interfejs API REST podobne w górnej części [składniki przemysłowego Internetu rzeczy Azure](https://github.com/Azure/azure-iiot-opc-ua). Interfejs API usługi zapewnia kontrolę nad funkcji modułu. 

![Przegląd przemysłowych IoT](media/overview-iot-industrial/overview.png)

> [!NOTE]
> Aby uzyskać więcej informacji o usługach przemysłowego Internetu rzeczy platformy Azure, zobacz GitHub [repozytorium](https://github.com/Azure/azure-iiot-services).
Jeśli znasz jak działają moduły usługi Azure IoT Edge, zaczynać się następujące artykuły:
- [Azure IoT Edge — informacje](../iot-edge/about-iot-edge.md)
- [Moduły platformy Azure IoT Edge](../iot-edge/iot-edge-modules.md)

## <a name="connected-factory"></a>Połączona fabryka

[Połączona fabryka](../iot-accelerators/iot-accelerators-connected-factory-features.md) to implementacja architektury referencyjnej przemysłowego Internetu rzeczy Azure firmy Microsoft, które mogą być dostosowane do wymagań biznesowych. Kod pełne rozwiązanie jest typu open source i dostępne w repozytorium GitHub akcelerator rozwiązania połączonej fabryki. Można go użyć jako punktu wyjścia dla produktu komercyjnego i wdrożyć wstępnie utworzone rozwiązanie w ramach subskrypcji platformy Azure w ciągu kilku minut. 

## <a name="factory-floor-connectivity"></a>Fabryka floor łączności

Bliźniacza reprezentacja OPC jest składnikiem IIoT automatyzuje odnajdywanie urządzeń i rejestracji, która oferuje zdalnego sterowania urządzeń przemysłowych za pośrednictwem interfejsów API REST. OPC Twin używa usługi Azure IoT Edge i IoT Hub do łączenia z chmury i sieci fabryki. Bliźniacza reprezentacja OPC umożliwia deweloperom IIoT skoncentrowanie się na tworzeniu aplikacji IIoT bez konieczności martwienia się o tym, jak bezpieczny dostęp do maszyn lokalnych.

## <a name="security"></a>Bezpieczeństwo

OPC Vault jest implementacją z serwera OPC UA globalnego odnajdywania serwera (GDS), konfigurować, rejestrowanie i zarządzanie cyklem życia certyfikatu w przypadku serwera OPC UA i aplikacje klienckie w chmurze. OPC Vault upraszcza wdrażanie i konserwowanie zasobów bezpiecznej łączności w przemysłowe miejsca. Dzięki automatyzacji zarządzania certyfikatami, magazynie OPC zwalnia operatory fabryki z ręcznie, jak i złożone procesy związane z łącznością i zarządzanie certyfikatami.

## <a name="next-steps"></a>Kolejne kroki

Teraz, gdy użytkownik miał wprowadzenie do przemysłowego Internetu rzeczy i jego składników, poniżej przedstawiono sugerowany następny krok:

> [!div class="nextstepaction"]
> [Co to jest bliźniaczej reprezentacji OPC](overview-opc-twin.md)
