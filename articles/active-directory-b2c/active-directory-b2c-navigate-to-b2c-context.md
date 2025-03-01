---
title: Przełączanie do dzierżawy B2C w usłudze Azure Active Directory B2C | Dokumentacja firmy Microsoft
description: Jak przełączyć się do kontekstu dzierżawy usługi Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 4/13/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d1580931a94b58e772f9f11cb7b9948216e9063a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66509890"
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a>Przełączanie się do dzierżawy usługi Azure AD B2C

Aby skonfigurować usługę Azure AD B2C, musisz być w kontekście swojej dzierżawy usługi Azure AD B2C.

## <a name="log-into-azure-ad-b2c-tenant"></a>Logowanie się do dzierżawy usługi Azure AD B2C

Aby przejść do swojej dzierżawy usługi Azure AD B2C, musisz się zalogować do witryny Azure Portal jako administrator globalny dzierżawy usługi Azure AD B2C.

1. Zaloguj się do [Azure Portal](https://portal.azure.com).
1. Przełącz dzierżawy, klikając swój adres e-mail lub zdjęcie w prawym górnym rogu.
1. Z wyświetlonej listy `Directory` wybierz dzierżawę usługi Azure AD B2C, którą chcesz zarządzać.

Witryna Azure Portal zostanie odświeżona.  Od tej chwili kontekstem logowania w witrynie Azure Portal będzie Twoja dzierżawa usługi Azure AD B2C.

## <a name="navigate-to-the-b2c-features-pane"></a>Przejście do okienka funkcji B2C

1. Kliknij polecenie **Przeglądaj** w obszarze nawigacji po lewej stronie.
1. Kliknij pozycję **Wszystkie usługi**, a następnie wyszukaj tekst `Azure AD B2C` w okienku nawigacji po lewej stronie.  (Aby przypiąć tę pozycję do tablicy startowej po lewej stronie, kliknij gwiazdkę na lewo od usługi Azure AD B2C)
1. Kliknij pozycję **Azure AD B2C**, aby otworzyć okienko funkcji B2C.
   
    ![Zrzut ekranu z przejdź do okienka funkcji B2C](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> Tylko administrator globalny dzierżawy B2C może uzyskać dostęp do okienka funkcji B2C. Administrator globalny innej dzierżawy ani użytkownik dowolnej dzierżawy nie mogą uzyskać dostępu do tego bloku.  Na swoją dzierżawę B2C możesz przełączyć się, używając przełącznika dzierżawy w prawym górnym rogu witryny Azure Portal.
