---
title: 'Samouczek: Integracja usługi Azure Active Directory z aplikacją Deputy | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory a aplikacją Deputy.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5665c3ac-5689-4201-80fe-fcc677d4430d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/25/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8e8b61bc01e729472c140253f8f936b6ec0dd1b0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67104245"
---
# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Samouczek: integracja usługi Azure Active Directory z aplikacją Deputy

Z tego samouczka dowiesz się, jak zintegrować aplikację Deputy z usługą Azure Active Directory (Azure AD).
Integrowanie aplikacji Deputy z usługą Azure AD zapewnia następujące korzyści:

* W usłudze Azure AD możesz kontrolować, kto ma dostęp do aplikacji Deputy.
* Możesz zezwolić swoim użytkownikom na automatyczne logowanie do aplikacji Deputy (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Do skonfigurowania integracji usługi Azure AD z aplikacją Deputy potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja aplikacji Deputy z włączoną obsługą logowania jednokrotnego

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Aplikacja Deputy obsługuje logowanie jednokrotne inicjowane przez **dostawcę usług** i **dostawcę tożsamości**

## <a name="adding-deputy-from-the-gallery"></a>Dodawanie aplikacji Deputy z galerii

Aby skonfigurować integrację aplikacji Deputy z usługą Azure AD, należy dodać aplikację Deputy z galerii do swojej listy zarządzanych aplikacji SaaS.

**Aby dodać aplikację Deputy z galerii, wykonaj następujące kroki:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **Deputy**, wybierz pozycję **Deputy** z panelu wyników, a następnie kliknij przycisk **Dodaj**, aby dodać aplikację.

     ![Aplikacja Deputy na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD z aplikacją Deputy, korzystając z danych użytkownika testowego o nazwie **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację połączenia między użytkownikiem usługi Azure AD a powiązanym użytkownikiem aplikacji Deputy.

Aby skonfigurować i przetestować logowanie jednokrotne usługi Azure AD z aplikacją Deputy, należy wykonać czynności opisane w poniższych blokach konstrukcyjnych:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w aplikacji Deputy](#configure-deputy-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego aplikacji Deputy](#create-deputy-test-user)** — aby udostępnić w aplikacji Deputy odpowiednik użytkownika Britta Simon połączony z reprezentacją użytkownika w usłudze Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD z aplikacją Deputy, wykonaj następujące kroki:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **Deputy** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. Jeśli chcesz skonfigurować aplikację w trybie inicjowanym przez **dostawcę tożsamości**, w sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące kroki:

    ![Informacje o domenie i adresach URL logowania jednokrotnego aplikacji Deputy](common/idp-intiated.png)

    a. W polu tekstowym **Identyfikator** wpisz adres URL, korzystając z następującego wzorca:

    |  |
    | ----|
    | `https://<subdomain>.<region>.au.deputy.com` |
    | `https://<subdomain>.<region>.ent-au.deputy.com` |
    | `https://<subdomain>.<region>.na.deputy.com`|
    | `https://<subdomain>.<region>.ent-na.deputy.com`|
    | `https://<subdomain>.<region>.eu.deputy.com` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com` |
    | `https://<subdomain>.<region>.as.deputy.com` |
    | `https://<subdomain>.<region>.ent-as.deputy.com` |
    | `https://<subdomain>.<region>.la.deputy.com` |
    | `https://<subdomain>.<region>.ent-la.deputy.com` |
    | `https://<subdomain>.<region>.af.deputy.com` |
    | `https://<subdomain>.<region>.ent-af.deputy.com` |
    | `https://<subdomain>.<region>.an.deputy.com` |
    | `https://<subdomain>.<region>.ent-an.deputy.com` |
    | `https://<subdomain>.<region>.deputy.com` |

    b. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca:
    
    | |
    |----|
    | `https://<subdomain>.<region>.au.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.ent-au.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.na.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.ent-na.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.eu.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-as.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.la.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.ent-la.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.af.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.ent-af.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.an.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.ent-an.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.deputy.com/exec/devapp/samlacs` |

5. Kliknij przycisk **Ustaw dodatkowe adresy URL** i wykonaj następujący krok, jeśli chcesz skonfigurować aplikację w trybie inicjowania przez **dostawcę usług**:

    ![Informacje o domenie i adresach URL logowania jednokrotnego aplikacji Deputy](common/metadata-upload-additional-signon.png)

    W polu tekstowym **Adres URL logowania** wpisz adres URL, korzystając z następującego wzorca: `https://<your-subdomain>.<region>.deputy.com`

    >[!NOTE]
    > Sufiks regionu aplikacji Deputy jest opcjonalny lub należy użyć jednego z następujących: au | na | eu |as |la |af |an |ent-au |ent-na |ent-eu |ent-as | ent-la | ent-af | ent-an

    > [!NOTE]
    > Te wartości nie są prawdziwe. Należy je zastąpić rzeczywistymi wartościami identyfikatora, adresu URL odpowiedzi i adresu URL logowania. Skontaktuj się z [zespołem obsługi klienta aplikacji Deputy](https://www.deputy.com/call-centers-customer-support-scheduling-software), aby uzyskać te wartości. Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

6. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **certyfikat (Base64)** z podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/certificatebase64.png)

7. W sekcji **Konfigurowanie aplikacji Deputy** skopiuj odpowiednie adresy URL zgodnie ze swoimi wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    d. Adres URL wylogowywania

### <a name="configure-deputy-single-sign-on"></a>Konfigurowanie logowania jednokrotnego aplikacji Deputy

1. Przejdź do następującego adresu URL:[https://(Twoja_poddomena).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config). Przejdź do pozycji **Security Settings (Ustawienia zabezpieczeń)** i kliknij przycisk **Edit (Edytuj)** .
   
    ![Konfigurowanie logowania jednokrotnego](./media/deputy-tutorial/tutorial_deputy_004.png)

2. Na tej stronie **Security Settings (Ustawienia zabezpieczeń)** wykonaj poniższe kroki.

    ![Konfigurowanie logowania jednokrotnego](./media/deputy-tutorial/tutorial_deputy_005.png)
    
    a. Włącz pozycję **Social Login (Logowanie za pomocą danych z serwisów społecznościowych)** .
   
    b. Otwórz w Notatniku certyfikat zakodowany w formacie Base64 pobrany z witryny Azure Portal, skopiuj zawartość do Schowka, a następnie wklej ją w polu tekstowym **OpenSSL Certificate** (Certyfikat OpenSSL).
   
    c. W polu tekstowym adresu URL logowania jednokrotnego SAML wpisz `https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d. W polu tekstowym adresu URL logowania jednokrotnego SAML zastąp `<your subdomain>` swoją poddomeną.
   
    e. W polu tekstowym adresu URL logowania jednokrotnego SAML zastąp `<saml sso url>` **adresem URL logowania** skopiowanym z witryny Azure Portal.
   
    f. Kliknij pozycję **Save Settings (Zapisz ustawienia)** .

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

W tej sekcji włączysz dla użytkownika Britta Simon możliwość korzystania z logowania jednokrotnego platformy Azure, udzielając dostępu do aplikacji Deputy.

1. W witrynie Azure Portal wybierz pozycję **Aplikacje dla przedsiębiorstw**, pozycję **Wszystkie aplikacje**, a następnie pozycję **Deputy**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz pozycję **Deputy**.

    ![Link aplikacji Deputy na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-deputy-test-user"></a>Tworzenie użytkownika testowego aplikacji Deputy

Aby umożliwić użytkownikom usługi Azure AD logowanie się do aplikacji Deputy, należy ich aprowizować w tej aplikacji. W przypadku aplikacji Deputy aprowizacja to zadanie ręczne.

#### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Aby aprowizować konto użytkownika, wykonaj następujące czynności:

1. Zaloguj się do swojej firmowej witryny aplikacji Deputy jako administrator.

2. W górnym okienku nawigacji kliknij pozycję **People (Osoby)** .
   
    ![Osoby](./media/deputy-tutorial/tutorial_deputy_001.png "Osoby")

3. Kliknij przycisk **Add People (Dodaj osoby)** , a następnie kliknij pozycję **Add a single person (Dodaj jedną osobę)** .
   
    ![Dodaj osoby](./media/deputy-tutorial/tutorial_deputy_002.png "Dodaj osoby")

4. Wykonaj poniższe kroki, a następnie kliknij pozycję **Save & Invite (Zapisz i zaproś)** .
   
    ![Nowy użytkownik](./media/deputy-tutorial/tutorial_deputy_003.png "Nowy użytkownik")

    a. W polu tekstowym **Name (Imię i nazwisko)** wpisz imię i nazwisko użytkownika, na przykład **BrittaSimon**.
   
    b. W polu tekstowym **Email (Adres e-mail)** wpisz adres e-mail konta usługi Azure AD, które chcesz aprowizować.
   
    c. W polu tekstowym **Work at (Pracuje w)** wpisz nazwę firmy.
   
    d. Kliknij przycisk **Save & Invite (Zapisz i zaproś)** .

5. Właściciel konta usługi AAD otrzyma wiadomość e-mail z linkiem umożliwiającym potwierdzenie konta, zanim stanie się ono aktywne. Aby aprowizować konta użytkowników usługi AAD, możesz użyć jakichkolwiek innych narzędzi tworzenia konta użytkownika aplikacji Deputy lub interfejsów API udostępnianych przez aplikację Deputy.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka Deputy w panelu dostępu powinno nastąpić automatyczne zalogowanie do aplikacji Deputy, dla której skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

