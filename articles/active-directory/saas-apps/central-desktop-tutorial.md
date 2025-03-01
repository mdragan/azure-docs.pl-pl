---
title: 'Samouczek: Integracja usługi Azure Active Directory z aplikacją Central Desktop | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory i aplikacją Central Desktop.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58fcd21c7e6e3f2336d3e809584451dc38f6276b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67105735"
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Samouczek: Integracja usługi Azure Active Directory z aplikacją Central Desktop

Z tego samouczka dowiesz się, jak zintegrować aplikację Central Desktop z usługą Azure Active Directory (Azure AD).
Integracja aplikacji Central Desktop z usługą Azure AD zapewnia następujące korzyści:

* W usłudze Azure AD możesz kontrolować, kto ma dostęp do aplikacji Central Desktop.
* Możesz zezwolić swoim użytkownikom na automatyczne logowanie do aplikacji Central Desktop (logowanie jednokrotne) przy użyciu kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Do skonfigurowania integracji usługi Azure AD z aplikacją Central Desktop potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja aplikacji Central Desktop z obsługą logowania jednokrotnego

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Aplikacja Central Desktop obsługuje logowanie jednokrotne inicjowane przez **dostawcę usługi**

## <a name="adding-central-desktop-from-the-gallery"></a>Dodawanie aplikacji Central Desktop z galerii

Aby skonfigurować integrację aplikacji Central Desktop z usługą Azure AD, musisz dodać aplikację Central Desktop z galerii do swojej listy zarządzanych aplikacji SaaS.

**Aby dodać aplikację Central Desktop z galerii, wykonaj następujące czynności:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **Central Desktop**, wybierz pozycję **Central Desktop** z panelu wyników i kliknij przycisk **Dodaj**, aby dodać aplikację.

     ![Aplikacja Central Desktop na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD z aplikacją Central Desktop, korzystając z danych użytkownika testowego **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację połączenia między użytkownikiem usługi Azure AD i powiązanym użytkownikiem aplikacji Central Desktop.

Aby skonfigurować i przetestować logowanie jednokrotne usługi Azure AD z aplikacją Central Desktop, należy ukończyć poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w aplikacji Central Desktop](#configure-central-desktop-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego aplikacji Central Desktop](#create-central-desktop-test-user)** — aby mieć w aplikacji Central Desktop odpowiednik użytkownika Britta Simon połączony z reprezentacją użytkownika w usłudze Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD z aplikacją BC Central Desktop, wykonaj następujące kroki:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **Central Desktop** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. W sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące czynności:

    ![Informacje o domenie i adresach URL aplikacji Central Desktop na potrzeby logowania jednokrotnego](common/sp-identifier-reply.png)

    a. W polu tekstowym **Adres URL logowania** wpisz adres URL, korzystając z następującego wzorca: `https://<companyname>.centraldesktop.com`

    b. W polu **Identyfikator** wpisz adres URL, korzystając z następującego wzorca:
    
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|
    | |

    c. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca: `https://<companyname>.centraldesktop.com/saml2-assertion.php`

    > [!NOTE]
    > Te wartości nie są prawdziwe. Zastąp je rzeczywistymi wartościami adresu URL logowania, identyfikatora i adresu URL odpowiedzi. Aby uzyskać te wartości, skontaktuj się z [zespołem pomocy technicznej klienta aplikacji Central Desktop](https://imeetcentral.com/contact-us). Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

5. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **certyfikat (nieprzetworzony)** z podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/certificateraw.png)

6. W sekcji **Konfigurowanie aplikacji Central Desktop** skopiuj odpowiednie adresy URL zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    d. Adres URL wylogowywania

### <a name="configure-central-desktop-single-sign-on"></a>Konfigurowanie logowania jednokrotnego aplikacji Central Desktop

1. Zaloguj się do swojej dzierżawy aplikacji **Central Desktop**.

2. Przejdź do obszaru **Settings** (Ustawienia). Wybierz pozycję **Advanced** (Zaawansowane), a następnie wybierz pozycję **Single Sign On** (Logowanie jednokrotne).

    ![Konfiguracja — Zaawansowane](./media/central-desktop-tutorial/ic769563.png "Konfiguracja — Zaawansowane")

3. Na stronie **Single Sign On Settings** (Ustawienia logowania jednokrotnego) wykonaj następujące czynności:

    ![Ustawienia logowania jednokrotnego](./media/central-desktop-tutorial/ic769564.png "Ustawienia logowania jednokrotnego")

    a. Wybierz **Enable SAML v2 Single Sign On** (Włącz logowanie jednokrotne SAML w wersji 2).

    b. W polu tekstowym **SSO URL** (Adres URL logowania jednokrotnego) wklej wartość pola **Identyfikatora usługi Azure AD** skopiowaną z witryny Azure Portal.

    c. W polu **SSO Login URL** (Adres URL logowania SSO) wklej **adres URL logowania** skopiowany z witryny Azure Portal.

    d. W polu **SSO Logout URL** (Adres URL wylogowywania SSO) wklej **adres URL wylogowywania** skopiowany z witryny Azure Portal.

4. W sekcji **Message Signature Verification Method** (Metoda weryfikacji podpisu wiadomości) wykonaj następujące czynności:

    ![Metoda weryfikacji podpisu wiadomości](./media/central-desktop-tutorial/ic769565.png "Metoda weryfikacji podpisu wiadomości")
    
    a. Wybierz pozycję **Certyfikat**.

    b. Na liście **SSO Certificate** (Certyfikat logowania jednokrotnego) wybierz pozycję **RSH SHA256**.

    c. Otwórz pobrany certyfikat w Notatniku. Następnie skopiuj zawartość certyfikatu i wklej ją w polu **SSO Certificate** (Certyfikat logowania jednokrotnego).

    d. Wybierz pozycję **Display a link to your SAMLv2 login page** (Wyświetl link do strony logowania SAMLv2).

    e. Wybierz pozycję **Update** (Aktualizuj).

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

W tej sekcji włączysz użytkownikowi Britta Simon możliwość korzystania z logowania jednokrotnego platformy Azure, udzielając dostępu do aplikacji Central Desktop.

1. W witrynie Azure Portal wybierz pozycję **Aplikacje dla przedsiębiorstw**, pozycję **Wszystkie aplikacje**, a następnie pozycję **Central Desktop**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz pozycję **Central Desktop**.

    ![Link aplikacji Central Desktop na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-central-desktop-test-user"></a>Tworzenie użytkownika testowego aplikacji Central Desktop

Aby użytkownicy usługi Azure AD mogli się logować, należy ich aprowizować w aplikacji Central Desktop. W tej sekcji opisano sposób tworzenia kont użytkowników usługi Azure AD w aplikacji Central Desktop.

> [!NOTE]
> Aby aprowizować konta użytkowników usługi Azure AD, można użyć jakichkolwiek innych narzędzi tworzenia konta użytkownika lub interfejsów API dostarczonych przez aplikację Central Desktop.

**Aby aprowizować konta użytkowników w aplikacji Central Desktop:**

1. Zaloguj się do swojej dzierżawy aplikacji Central Desktop.

2. Wybierz pozycję **People** (Osoby), a następnie wybierz pozycję **Add Internal Members** (Dodaj członków wewnętrznych).

    ![Osoby](./media/central-desktop-tutorial/ic781051.png "Osoby")

3. W polu **Email Address of New Members** (Adres e-mail nowych członków) wpisz konto usługi Azure AD, które chcesz aprowizować, a następnie wybierz pozycję **Next** (Dalej).

    ![Adres e-mail nowych członków](./media/central-desktop-tutorial/ic781052.png "Adres e-mail nowych członków")

4. Wybierz pozycję **Add Internal member(s)** (Dodaj członków wewnętrznych).

    ![Dodawanie członka wewnętrznego](./media/central-desktop-tutorial/ic781053.png "Dodawanie członka wewnętrznego")
  
   > [!NOTE]
   > Dodani użytkownicy otrzymają wiadomość e-mail zawierającą link potwierdzający do aktywacji ich kont.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka Central Desktop w panelu dostępu powinno nastąpić automatyczne zalogowanie do aplikacji Central Desktop, dla której skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
