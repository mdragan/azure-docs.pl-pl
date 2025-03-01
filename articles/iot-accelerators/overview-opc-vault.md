---
title: Co to jest magazyn OPC — Azure | Dokumentacja firmy Microsoft
description: Omówienie magazynu OPC
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 99dfcaeb1ef5b52e6827f1b3ac65d6201557a8fb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60887764"
---
# <a name="what-is-opc-vault"></a>Co to jest magazyn OPC?

OPC Vault to mikrousług, konfigurowanie, rejestrowanie i zarządzanie cyklem życia certyfikatu w przypadku serwera OPC UA i aplikacje klienckie w chmurze. W tym artykule opisano magazynu OPC prostych zastosowań.

## <a name="certificate-management"></a>Zarządzanie certyfikatami

Na przykład firmy produkcyjnej związanych musi połączyć ich maszynę serwera OPC UA do swojej aplikacji w nowo utworzonego klienta. Gdy producent sprawia, że początkowe dostęp do komputera serwera, w aplikacji serwera OPC UA, aby wskazać, że aplikacja kliencka nie jest bezpieczny natychmiast wyświetlany jest komunikat o błędzie. Ten mechanizm jest wbudowana w komputerze serwera OPC UA, aby uniemożliwić dostęp dowolnej nieautoryzowanych aplikacji, co uniemożliwia vicious stosowanie metod hakerskich produkcyjnych.

## <a name="application-security-management"></a>Zarządzanie zabezpieczeniami aplikacji
Professional zabezpieczeń używa magazynu OPC mikrousługi można łatwo włączyć serwer OPC UA do komunikowania się z dowolnej aplikacji klienckiej, ponieważ Magazyn OPC ma wszystkie funkcje rejestru certyfikatu, magazynu i zarządzanie cyklem życia. Teraz bezpiecznie jest połączony z serwerem OPC UA, będzie mogła komunikować się do aplikacji nowo utworzonego klienta

## <a name="the-complete-opc-vault-architecture"></a>Pełnej architektury magazynu OPC
Na poniższym diagramie przedstawiono pełnej architektury OPC magazynu.

![Architektura magazynu OPC](media/overview-opc-vault-architecture/opc-vault.png)