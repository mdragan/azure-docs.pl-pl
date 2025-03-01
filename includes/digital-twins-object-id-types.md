---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 12/20/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 40ab53c941a7ac619ebb09d381a4ae0450f26e8b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67183864"
---
`objectIdType` (Lub **typ identyfikatora obiektu**) odwołuje się do typu tożsamości, który znajduje się do roli. Z wyjątkiem `DeviceId` i `UserDefinedFunctionId` typy, typy identyfikatora obiektu odpowiadają właściwości obiektów w usłudze Azure Active Directory.

Poniższa tabela zawiera typy identyfikatorów obsługiwany obiekt w reprezentacji urządzeń cyfrowych platformy Azure:

| Typ | Opis |
| --- | --- |
| UserId | Przypisanie roli do użytkownika. |
| DeviceId | Przypisanie roli do urządzenia. |
| DomainName | Przypisuje rolę, aby wskazywała nazwę domeny. Każdego użytkownika mającego określoną nazwę domeny ma prawa dostępu do odpowiedniej roli. |
| Identyfikator dzierżawy | Przypisuje rolę do dzierżawy. Każdy użytkownik, który należy do określonego Identyfikatora dzierżawy usługi Azure AD ma prawa dostępu do odpowiedniej roli. |
| ServicePrincipalId | Przypisanie roli do identyfikatora obiektu jednostki usługi |
| UserDefinedFunctionId | Przypisuje rolę funkcji zdefiniowanej przez użytkownika (UDF). |