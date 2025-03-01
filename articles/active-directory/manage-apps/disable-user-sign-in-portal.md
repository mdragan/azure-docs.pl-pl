---
title: Wyłączanie logowania użytkowników dla aplikacji przedsiębiorstwa w usłudze Azure Active Directory | Dokumentacja firmy Microsoft
description: Jak wyłączyć aplikacji dla przedsiębiorstw, dzięki czemu użytkownicy mogą zalogować się w niej w usłudze Azure Active Directory
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 90000f34ff247fdd5939dc19971c170aa4b70386
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65824657"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Wyłączanie logowania użytkowników dla aplikacji przedsiębiorstwa w usłudze Azure Active Directory
To proste, można wyłączyć aplikacji dla przedsiębiorstw, dzięki czemu użytkownicy mogą zalogować się w niej w usłudze Azure Active Directory (Azure AD). Konieczne jest odpowiednie uprawnienia do zarządzania aplikacji przedsiębiorstwa. Ponadto musisz być administratorem globalnym katalogu.

## <a name="how-do-i-disable-user-sign-ins"></a>Jak wyłączyć logowania użytkowników?
1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
1. Wybierz **wszystkich usług**, wprowadź **usługi Azure Active Directory** w polu tekstowym, a następnie wybierz pozycję **Enter**.
1. Na **usługi Azure Active Directory** -  ***directoryname*** okienku (oznacza to, że usługa Azure AD dla katalogu zarządzasz), wybierz **aplikacje dla przedsiębiorstw**.
1. Na **aplikacje w przedsiębiorstwie — wszystkie aplikacje** okienku można wyświetlić listę aplikacje, którymi można zarządzać. Wybierz aplikację.
1. Na ***appname*** okienku (czyli nazwę wybranej aplikacji w tytule), wybierz **właściwości**.
1. Na ***appname*** - **właściwości** okienku wybierz **nie** dla **włączono dla użytkowników do logowania?** .
1. Wybierz **Zapisz** polecenia.

## <a name="next-steps"></a>Kolejne kroki
* [Zobacz wszystkie moje grupy](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Przypisywanie użytkownika lub grupy do aplikacji przedsiębiorstwa](assign-user-or-group-access-portal.md)
* [Usuń przypisanie użytkownika lub grupy z aplikacji przedsiębiorstwa](remove-user-or-group-access-portal.md)
* [Zmiana nazwy lub logo aplikacji przedsiębiorstwa](change-name-or-logo-portal.md)
