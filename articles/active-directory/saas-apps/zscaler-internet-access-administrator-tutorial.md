---
title: 'Samouczek: Integracja usługi Azure Active Directory z aplikacją Zscaler Internet Access Administrator | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory i aplikacją Zscaler Internet Access Administrator.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ea555097-bf62-45dd-9b45-b75c50324a69
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b46b8644b9ba4f0dc6b0b97215a4a96b60d69c3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67086036"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-internet-access-administrator"></a>Samouczek: Integracja usługi Azure Active Directory z aplikacją Zscaler Internet Access Administrator

Z tego samouczka dowiesz się, jak zintegrować aplikację Zscaler Internet Access Administrator z usługą Azure Active Directory (Azure AD).
Integracja aplikacji Zscaler Internet Access Administrator z usługą Azure AD oferuje następujące korzyści:

* Możesz kontrolować w usłudze Azure AD, kto ma dostęp do aplikacji Zscaler Internet Access Administrator.
* Możesz zezwolić swoim użytkownikom na automatyczne logowanie do aplikacji Zscaler Internet Access Administrator (logowanie jednokrotne) przy użyciu kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Do skonfigurowania integracji usługi Azure AD z aplikacją Zscaler Internet Access Administrator potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja aplikacji Zscaler Internet Access Administrator

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Aplikacja Zscaler Internet Access Administrator obsługuje logowanie jednokrotne inicjowane przez **IDP**

## <a name="adding-zscaler-internet-access-administrator-from-the-gallery"></a>Dodawanie aplikacji Zscaler Internet Access Administrator z galerii

Aby skonfigurować integrację aplikacji Zscaler Internet Access Administrator w usłudze Azure AD, należy dodać aplikację Zscaler Internet Access Administrator z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać aplikację Zscaler Internet Access Administrator z galerii, wykonaj następujące kroki:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz frazę **Zscaler Internet Access Administrator**, wybierz pozycję **Zscaler Internet Access Administrator** w panelu wyników, a następnie kliknij przycisk **Dodaj**, aby dodać aplikację.

     ![Aplikacja Zscaler Internet Access Administrator na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji skonfigurujesz i przetestujesz usługi logowanie jednokrotne w usłudze Azure AD za pomocą aplikacji Zscaler Internet Access Administrator dla nazwy użytkownika testowego **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację połączenia między użytkownikiem usługi Azure AD i powiązanym użytkownikiem aplikacji Zscaler Internet Access Administrator.

Aby skonfigurować i przetestować logowanie jednokrotne w usłudze Azure AD przy użyciu aplikacji Zscaler Internet Access Administrator, należy ukończyć poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w aplikacji Zscaler Internet Access Administrator](#configure-zscaler-internet-access-administrator-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego aplikacji Zscaler Internet Access Administrator](#create-zscaler-internet-access-administrator-test-user)** — aby w aplikacji Zscaler Internet Access Administrator utworzyć odpowiednik użytkownika Britta Simon połączony z reprezentacją użytkownika w usłudze Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne w usłudze Azure AD przy użyciu aplikacji Zscaler Internet Access Administrator, wykonaj następujące kroki:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **Zscaler Internet Access Administrator** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij przycisk **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Adresy URL i domena aplikacji Zscaler Internet Access Administrator — informacje dotyczące logowania jednokrotnego](common/idp-intiated.png)

    a. W polu tekstowym **Identyfikator** wpisz adres URL zgodnie z wymaganiami:

    | |
    |--|
    | `https://admin.zscaler.net` |
    | `https://admin.zscalerone.net` |
    | `https://admin.zscalertwo.net` |
    | `https://admin.zscalerthree.net` |
    | `https://admin.zscloud.net` |
    | `https://admin.zscalerbeta.net` |

    b. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL zgodnie z wymaganiami:

    | |
    |--|
    | `https://admin.zscaler.net/adminsso.do` |
    | `https://admin.zscalerone.net/adminsso.do` |
    | `https://admin.zscalertwo.net/adminsso.do` |
    | `https://admin.zscalerthree.net/adminsso.do` |
    | `https://admin.zscloud.net/adminsso.do` |
    | `https://admin.zscalerbeta.net/adminsso.do` |

5. Aplikacja Zscaler Internet Access Administrator oczekuje potwierdzeń SAML w określonym formacie. Skonfiguruj następujące oświadczenia dla tej aplikacji. Wartościami tych atrybutów możesz zarządzać w sekcji **Atrybuty i oświadczenia użytkownika** na stronie integracji aplikacji. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij przycisk **Edytuj**, aby otworzyć okno dialogowe **Atrybuty i oświadczenia użytkownika**.

    ![Link do atrybutu](./media/zscaler-internet-access-administrator-tutorial/tutorial_zscaler-internet_attribute.png)

6. W sekcji **Oświadczenia użytkownika** w oknie dialogowym **Atrybuty użytkownika** skonfiguruj atrybut tokenu SAML, jak pokazano na ilustracji powyżej, i wykonaj następujące czynności:

    | Nazwa  | Atrybut źródłowy  |
    | ---------| ------------ |
    | Rola     | user.assignedroles |

    a. Kliknij przycisk **Dodaj nowe oświadczenie**, aby otworzyć okno dialogowe **Zarządzanie oświadczeniami użytkownika**.

    ![image](./common/new-save-attribute.png)
    
    ![image](./common/new-attribute-details.png)

    b. Z listy **Atrybut źródłowy** wybierz wartość atrybutu.

    c. Kliknij przycisk **OK**.

    d. Kliknij pozycję **Zapisz**.

    > [!NOTE]
    > Kliknij [tutaj](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management), aby dowiedzieć się, jak skonfigurować rolę w usłudze Azure AD

7. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **certyfikat (Base64)** z podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/certificatebase64.png)

8. W sekcji **konfigurowania aplikacji Zscaler Internet Access Administrator** skopiuj odpowiednie adresy URL zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    d. Adres URL wylogowywania

### <a name="configure-zscaler-internet-access-administrator-single-sign-on"></a>Konfigurowanie logowania jednokrotnego w aplikacji Zscaler Internet Access Administrator

1. W innym oknie przeglądarki internetowej zaloguj się do interfejsu użytkownika aplikacji Zscaler Internet Access Administrator.

2. Przejdź do pozycji **Administracja > Zarządzanie administratorami** i wykonaj następujące kroki, po czym kliknij przycisk Zapisz:

    ![Administracja](./media/zscaler-internet-access-administrator-tutorial/AdminSSO.png "Administracja")

    a. Zaznacz pozycję **Włącz uwierzytelnianie SAML**.

    b. Kliknij pozycję **Przekaż**, aby przekazać certyfikat podpisywania języka SAML na platformie Azure, który został pobrany z witryny Azure Portal w obrębie **publicznego certyfikatu SSL**.

    c. Opcjonalnie, w celu poprawy zabezpieczeń, dodaj szczegóły **wystawcy**, aby sprawdzić odpowiedź wystawcy SAML.

3. W interfejsie użytkownika administratora wykonaj następujące kroki:

    ![Administracja](./media/zscaler-internet-access-administrator-tutorial/ic800207.png)

    a. Umieść kursor nad menu **Aktywacja** w lewym dolnym rogu.

    b. Kliknij pozycję **Aktywuj**.

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD 

W tej sekcji w witrynie Azure Portal utworzysz użytkownika testowego o nazwie Britta Simon.

1. W witrynie Azure Portal w okienku po lewej stronie wybierz pozycję **Azure Active Directory**, wybierz opcję **Użytkownicy**, a następnie wybierz pozycję **Wszyscy użytkownicy**.

    ![Linki „Użytkownicy i grupy” i „Wszyscy użytkownicy”](common/users.png)

2. Wybierz przycisk **Nowy użytkownik** w górnej części ekranu.

    ![Przycisk Nowy użytkownik](common/new-user.png)

3. We właściwościach użytkownika wykonaj następujące kroki.

    ![Okno dialogowe Użytkownik](common/user-properties.png)

    a. W polu **Nazwa** wprowadź **BrittaSimon**.
  
    b. W **nazwa_użytkownika** typ pola **brittasimon\@yourcompanydomain.extension**  
    Na przykład: BrittaSimon@contoso.com

    d. Zaznacz pole wyboru **Pokaż hasło** i zanotuj wartość wyświetlaną w polu Hasło.

    d. Kliknij pozycję **Utwórz**.

### <a name="assign-the-azure-ad-test-user"></a>Przypisywanie użytkownika testowego usługi Azure AD

W tej sekcji włączysz możliwość logowania jednokrotnego na platformie Azure dla użytkownika, udzielając dostępu do aplikacji Zscaler Internet Access Administrator.

1. W witrynie Azure Portal wybierz pozycję **Aplikacje dla przedsiębiorstw**, wybierz pozycję **Wszystkie aplikacje**, a następnie wybierz pozycję **Zscaler Internet Access Administrator**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wpisz i wybierz ciąg **Zscaler Internet Access Administrator**.

    ![Link do aplikacji Zscaler Internet Access Administrator na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-zscaler-internet-access-administrator-test-user"></a>Tworzenie użytkownika testowego aplikacji Zscaler Internet Access Administrator

Celem tej sekcji jest utworzenie użytkownika o nazwie Britta Simon w aplikacji Zscaler Internet Access Administrator. Aplikacja Zscaler Internet Access nie obsługuje inicjowania aprowizacji Just-In-Time dla logowania jednokrotnego administratora. Musisz ręcznie utworzyć konto administratora.
Instrukcje dotyczące sposobu tworzenia konta administratora można znaleźć w dokumentacji aplikacji Zscaler:

https://help.zscaler.com/zia/adding-admins

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka aplikacji Zscaler Internet Access Administrator w panelu dostępu powinno nastąpić automatyczne zalogowanie do interfejsu użytkownika aplikacji Zscaler Internet Access Administrator, dla której skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
