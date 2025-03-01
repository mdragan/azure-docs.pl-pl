---
title: Opuścić organizację jako użytkownika-gościa — usługi Azure Active Directory | Dokumentacja firmy Microsoft
description: Pokazuje, jak użytkownik-Gość usługi Azure AD B2B można pozostawić organizacji za pomocą panelu dostępu.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 26d9eb883cc014c1bea092a12e22b6d144a37994
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112991"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Opuścić organizację jako użytkownika-gościa

Użytkownik-Gość usługi Azure Active Directory (Azure AD) B2B można zdecyduję się opuścić organizacji w dowolnym momencie, jeśli nie są już potrzebne, korzystanie z aplikacji z tej organizacji lub zachowanie skojarzenia. Użytkownik może pozostać organizacji samodzielnie, bez konieczności kontaktowania się z administratorem.

> [!NOTE]
> Użytkownik-Gość nie może opuścić organizacji, jeśli konta zostały wyłączone w głównej dzierżawy i dzierżawy zasobów. Wyłączenie konta użytkownika gościa należy skontaktować się z administratorem dzierżawy, który można usunąć konta gościa lub włączenia konta gościa, dzięki czemu użytkownik może narazić organizację.

## <a name="leave-an-organization"></a>Opuszczanie organizacji

Aby opuścić organizację, wykonaj następujące kroki.

1. Przejdź do strony swojego profilu panelu dostępu, wykonując jedną z następujących czynności:
   
   - W [witryny Azure portal](https://portal.azure.com), kliknij swoją nazwę użytkownika w prawym górnym rogu i wybierz **wyświetlić konto**.
   - Otwórz swoje [panelu dostępu](https://myapps.microsoft.com), kliknij swoją nazwę użytkownika w prawym górnym rogu i dalej, aby **organizacje**, wybierz ikonę ustawienia (koło zębate).
 
   ![Zrzut ekranu przedstawiający ustawienia użytkownika w panelu dostępu](media/leave-the-organization/UserSettings.png) 

   > [!NOTE]
   > Jeśli jeszcze nie zostało to zrobione organizacji chcesz pozostawić w obszarze **organizacje**, kliknij przycisk **Zaloguj się opuścić organizację** łącze obok nazwy w organizacji. Po użytkownik jest zalogowany, kliknij swoją nazwę użytkownika ponownie w prawym górnym rogu, a obok **organizacje**, wybierz ikonę ustawienia (koło zębate).

3. W obszarze **organizacje**, Znajdź, którą chcesz wystawić, a następnie wybierz organizację **opuścić organizację**.

   ![Zrzut ekranu przedstawiający opcję organizacji pozostaw w interfejsie użytkownika](media/leave-the-organization/LeaveOrg.png)

4. Po wyświetleniu monitu o potwierdzenie, wybierz **pozostaw**. 

## <a name="account-removal"></a>Usuwanie konta

Gdy użytkownik opuszcza organizację, usunięcia konta użytkownika "soft" w katalogu. Domyślnie obiekt użytkownika przenosi do **usuniętych użytkowników** obszaru w usłudze Azure AD, ale nie jest trwale usunięte przez 30 dni. Ta usuwania nietrwałego umożliwia administratorowi przywrócić konto użytkownika (w tym grupy i uprawnienia), jeśli użytkownik wysyła żądanie, aby przywrócić konto w ramach 30-dniowego okresu.

Jeśli to konieczne, administrator dzierżawy trwale usunąć to konto w dowolnym momencie w trakcie 30-dniowego okresu. W tym celu:

1. W [witryny Azure portal](https://portal.azure.com), wybierz opcję **usługi Azure Active Directory**.
2. W obszarze **Zarządzaj** wybierz pozycję **Użytkownicy**.
3. Wybierz **usuniętych użytkowników**.
4. Zaznacz pole wyboru obok usuniętego użytkownika, a następnie wybierz **trwałe usunięcie**.

Jeśli trwale usunąć użytkownika, ta akcja jest nieodwracalny.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Kolejne kroki

- Omówienie usługi Azure AD B2B, zobacz [czym jest współpraca B2B w usłudze Azure AD?](what-is-b2b.md)



