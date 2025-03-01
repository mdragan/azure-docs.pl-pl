---
title: SAML logowania jednokrotnego dla aplikacji w środowisku lokalnym za pomocą usługi Azure Active Directory serwera Proxy aplikacji (wersja zapoznawcza) | Dokumentacja firmy Microsoft
description: Dowiedz się, jak zapewnić logowanie jednokrotne dla lokalnych aplikacje opublikowane za pośrednictwem serwera Proxy aplikacji, które są zabezpieczone przy użyciu uwierzytelniania SAML.
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
ms.date: 05/20/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 907cb598d708bfa26f53d2e43fef5456258c21b1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66393046"
---
# <a name="saml-single-sign-on-for-on-premises-applications-with-application-proxy-preview"></a>SAML logowania jednokrotnego dla aplikacji w środowisku lokalnym dzięki serwerowi Proxy aplikacji (wersja zapoznawcza)

Logowanie jednokrotne (SSO) można udostępnić lokalne aplikacje, które są zabezpieczone przy użyciu uwierzytelniania SAML i zapewnić dostęp zdalny do tych aplikacji za pośrednictwem serwera Proxy aplikacji. Przy użyciu protokołu SAML logowania jednokrotnego usługi Azure Active Directory (Azure AD) uwierzytelnia się do aplikacji za pomocą konta usługi Azure AD. Usługa Azure AD komunikuje się informacji logowania jednokrotnego do aplikacji za pośrednictwem protokołu połączenia. Można również mapować użytkowników do ról aplikacji, na podstawie reguł zdefiniowanych w swoje oświadczenia języka SAML. Po włączeniu serwera Proxy aplikacji, oprócz logowania jednokrotnego SAML użytkownicy będą mieli dostępu do aplikacji i bezproblemowe logowanie Jednokrotne.

Aplikacje muszą mieć możliwość korzystania tokeny SAML wystawione przez **usługi Azure Active Directory**. Ta konfiguracja nie ma zastosowania do aplikacji przy użyciu dostawcy tożsamości w środowisku lokalnym. Dla tych scenariuszy, zaleca się, przeglądając [zasoby dotyczące migrowania aplikacji do usługi Azure AD](migration-resources.md).

Za pomocą funkcji szyfrowania tokenów SAML działa również logowania jednokrotnego SAML z serwerem Proxy aplikacji. Aby uzyskać więcej informacji, zobacz [szyfrowania tokenu Konfigurowanie usługi Azure AD SAML](howto-saml-token-encryption.md).

## <a name="publish-the-on-premises-application-with-application-proxy"></a>Publikowanie aplikacji w środowisku lokalnym dzięki serwerowi Proxy aplikacji

Przed zapewnieniem logowania jednokrotnego dla aplikacji w środowisku lokalnym, upewnij się, włączono serwer Proxy aplikacji i masz zainstalowanego łącznika. Zobacz [Dodawanie aplikacji w środowisku lokalnym dostępu zdalnego za pośrednictwem serwera Proxy aplikacji w usłudze Azure AD](application-proxy-add-on-premises-application.md) Aby dowiedzieć się, jak.

Należy pamiętać, następujące po zamierzasz instrukcje z samouczka:

* Opublikuj aplikację zgodnie z instrukcjami w tym samouczku. Upewnij się, że wybrano **usługi Azure Active Directory** jako **wstępne uwierzytelnianie** metodę dla aplikacji (krok 4 [Dodawanie aplikacji lokalnych do usługi Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad
)).
* Kopiuj **zewnętrzny adres URL** dla aplikacji.
* Najlepszym rozwiązaniem jest Użyj domen niestandardowych, jeśli to możliwe, aby środowiska zoptymalizowane użytkownika. Dowiedz się więcej o [Praca z domenami niestandardowymi na serwerze Proxy aplikacji usługi Azure AD](application-proxy-configure-custom-domain.md).
* Dodaj co najmniej jednego użytkownika do aplikacji i upewnij się, że testowe konto ma dostęp do aplikacji w środowisku lokalnym. Za pomocą testu konta testu, jeśli można uzyskać dostępu do aplikacji, odwiedzając **zewnętrzny adres URL** do sprawdzania poprawności serwera Proxy aplikacji są poprawnie skonfigurowane. Aby uzyskać informacje dotyczące rozwiązywania problemów, zobacz [Rozwiązywanie problemów z serwera Proxy aplikacji problemy i komunikaty o błędach](application-proxy-troubleshoot.md).

## <a name="set-up-saml-sso"></a>Konfigurowanie logowania jednokrotnego SAML

1. W witrynie Azure portal wybierz **usługi Azure Active Directory > aplikacje dla przedsiębiorstw** i wybierz aplikację z listy.
1. Z poziomu aplikacji **Przegląd** wybierz opcję **logowanie jednokrotne**.
1. Wybierz **SAML** jako pojedynczej metody logowania jednokrotnego.
1. W **Ustaw się logowanie jednokrotne z SAML** strony, Edytuj **podstawową konfigurację protokołu SAML** danych i postępuj zgodnie z instrukcjami w [Enter podstawową konfigurację protokołu SAML](configure-single-sign-on-non-gallery-applications.md#saml-based-single-sign-on) skonfigurować opartej na SAML uwierzytelnianie dla aplikacji.

   * Upewnij się, że **adres URL odpowiedzi** odpowiada **zewnętrzny adres URL** dla aplikacji w środowisku lokalnym, która opublikowane za pośrednictwem serwera Proxy aplikacji lub jest ścieżką, w obszarze **zewnętrzny adres URL**.
   * Usługi flow inicjowane przez dostawcę tożsamości, gdzie aplikacja wymaga innego **adres URL odpowiedzi** konfiguracji SAML, dodaj ją jako **dodatkowe** adres URL na liście i znacznik wyboru obok niego do wyznaczenia jako podstawowy **adres URL odpowiedzi**.
   * Przepływ zainicjowanego przez dostawcę usług upewnij się, że aplikacja wewnętrznej bazy danych określa poprawny **adres URL odpowiedzi** lub adres URL usługi konsumenta potwierdzenie służące do odbierania token uwierzytelniania.

     ![Wprowadzanie podstawowych danych konfiguracji protokołu SAML](./media/application-proxy-configure-single-sign-on-on-premises-apps/basic-saml-configuration.png)

    > [!NOTE]
    > Jeśli aplikacja wewnętrznej bazy danych oczekuje **adres URL odpowiedzi** jako wewnętrzny adres URL, należy użyć [domen niestandardowych](application-proxy-configure-custom-domain.md) dopasowania wewnętrzne i zewnętrzne adresy URL lub zainstaluj Moje aplikacje bezpiecznego logowania rozszerzenie na urządzeniach użytkowników. To rozszerzenie automatycznie nastąpi przekierowanie do odpowiedniej usługi serwera Proxy aplikacji. Aby zainstalować rozszerzenie, zobacz [Moje aplikacje bezpiecznego logowania rozszerzenia](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension).

## <a name="test-your-app"></a>Testowanie aplikacji

Po zakończeniu wszystkie te kroki aplikacji powinna być uruchomiona. Aby przetestować aplikację:

1. Otwórz przeglądarkę i przejdź do zewnętrznego adresu URL, który został utworzony podczas publikowania aplikacji. 
1. Zaloguj się przy użyciu konta testowego, który jest przypisany do aplikacji. Można załadować aplikacji i logowania jednokrotnego do aplikacji.

## <a name="next-steps"></a>Kolejne kroki

- [W jaki sposób serwer Proxy aplikacji usługi Azure AD zapewnia logowanie jednokrotne](application-proxy-single-sign-on.md)
- [Rozwiązywanie problemów z serwera Proxy aplikacji](application-proxy-troubleshoot.md)
