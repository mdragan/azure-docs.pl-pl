---
title: 'Samouczek: Integracja usługi Azure Active Directory z aplikacją TOPdesk - Secure | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory i aplikacją TOPdesk - Secure.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8e06ee33-18f9-4c05-9168-e6b162079d88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: eded8eb446d36a321acf46231eee3e764ba41504
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67088452"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Samouczek: Integracja usługi Azure Active Directory z aplikacją TOPdesk - Secure

Z tego samouczka dowiesz się, jak zintegrować aplikację TOPdesk - Secure z usługą Azure Active Directory (Azure AD).
Zintegrowanie aplikacji TOPdesk - Secure z usługą Azure AD zapewnia następujące korzyści:

* W usłudze Azure AD możesz kontrolować, kto ma dostęp do aplikacji TOPdesk - Secure.
* Możesz zezwolić swoim użytkownikom na automatyczne logowanie do aplikacji TOPdesk - Secure (logowanie jednokrotne) przy użyciu kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z aplikacją TOPdesk - Secure, potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja aplikacji TOPdesk - Secure z obsługą logowania jednokrotnego

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Aplikacja TOPdesk - Secure obsługuje logowanie jednokrotne inicjowane przez **dostawcę usługi**

## <a name="adding-topdesk---secure-from-the-gallery"></a>Dodawanie aplikacji TOPdesk - Secure z galerii

Aby skonfigurować integrację aplikacji TOPdesk - Secure z usługą Azure AD, musisz dodać aplikację TOPdesk - Secure z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać aplikację TOPdesk - Secure z galerii, wykonaj następujące kroki:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **TOPdesk - Secure**, wybierz pozycję **TOPdesk - Secure** z panelu wyników i kliknij przycisk **Dodaj**, aby dodać aplikację.

     ![Aplikacja TOPdesk - Secure na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD z aplikacją TOPdesk - Secure, korzystając z danych testowego użytkownika **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację połączenia między użytkownikiem usługi Azure AD i powiązanym użytkownikiem aplikacji TOPdesk - Secure.

Aby skonfigurować i przetestować logowanie jednokrotne usługi Azure AD z aplikacją TOPdesk - Secure, musisz utworzyć poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w aplikacji TOPdesk - Secure](#configure-topdesk---secure-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego aplikacji TOPdesk - Secure](#create-topdesk---secure-test-user)** — aby mieć w aplikacji TOPdesk - Secure odpowiednik użytkownika Britta Simon połączony z reprezentacją użytkownika w usłudze Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD z aplikacją TOPdesk - Secure, wykonaj następujące kroki:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **TOPdesk - Secure** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. W sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące czynności:

    ![Domena i adresy URL aplikacji TOPdesk - Secure — informacje dotyczące logowania jednokrotnego](common/sp-identifier-reply.png)

    a. W polu tekstowym **Adres URL logowania** wpisz adres URL, korzystając z następującego wzorca: `https://<companyname>.topdesk.net`

    b. W polu **Identyfikator** wpisz adres URL, korzystając z następującego wzorca: `https://<companyname>.topdesk.net/tas/secure/login/verify`

    c. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca: `https://<companyname>.topdesk.net/tas/public/login/saml`

    > [!NOTE]
    > Te wartości nie są prawdziwe. Zastąp je rzeczywistymi wartościami adresu URL logowania, identyfikatora i adresu URL odpowiedzi. Skontaktuj się z [zespołem pomocy technicznej klienta TOPdesk - Secure](https://www.topdesk.com/us/support/), aby uzyskać te wartości. Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

5. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **kod XML metadanych federacji** na podstawie podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/metadataxml.png)

6. W sekcji **Skonfiguruj aplikację TOPdesk - Secure** skopiuj odpowiednie adresy URL zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    d. Adres URL wylogowywania

### <a name="configure-topdesk---secure-single-sign-on"></a>Konfigurowanie logowania jednokrotnego aplikacji TOPdesk - Secure

1. Zaloguj się do firmowej witryny aplikacji **TOPdesk - Secure** jako administrator.

2. W menu **TOPdesk** kliknij pozycję **Settings** (Ustawienia).

    ![Ustawienia](./media/topdesk-secure-tutorial/ic790598.png "Ustawienia")

3. Kliknij pozycję **Login Settings** (Ustawienia logowania).

    ![Ustawienia logowania](./media/topdesk-secure-tutorial/ic790599.png "Ustawienia logowania")

4. Rozwiń menu **Login Settings** (Ustawienia logowania), a następnie kliknij pozycję **General** (Ogólne).

    ![Ogólne](./media/topdesk-secure-tutorial/ic790600.png "Ogólne")

5. W części **Secure** (Bezpieczne) sekcji konfiguracji **SAML login** (Logowanie SAML) wykonaj następujące kroki:

    ![Ustawienia techniczne](./media/topdesk-secure-tutorial/ic790855.png "Ustawienia techniczne")

    a. Kliknij pozycję **Download** (Pobierz), aby pobrać publiczny plik metadanych, a następnie zapisz go lokalnie na komputerze.

    b. Otwórz plik metadanych i znajdź węzeł **AssertionConsumerService**.

    ![Assertion Consumer Service](./media/topdesk-secure-tutorial/ic790856.png "Assertion Consumer Service")

    c. Skopiuj wartość **AssertionConsumerService** i wklej ją w polu tekstowym adresu URL odpowiedzi w sekcji **TOPdesk - Secure Domain and URLs** (Domena i adresy URL aplikacji TOPdesk - Secure).

6. Aby utworzyć plik certyfikatu, wykonaj następujące kroki:

    ![Certyfikat](./media/topdesk-secure-tutorial/ic790606.png "Certyfikat")

    a. Otwórz plik metadanych pobrany w witrynie Azure Portal.

    b. Rozwiń węzeł **RoleDescriptor**, w którym element **xsi:type** ma wartość **fed:ApplicationServiceType**.

    c. Skopiuj wartość węzła**X509Certificate**.

    d. Zapisz skopiowaną wartość **X509Certificate** lokalnie na komputerze w pliku.

7. W sekcji **Public** (Publiczne) kliknij przycisk **Add** (Dodaj).

    ![Dodaj](./media/topdesk-secure-tutorial/ic790607.png "Dodaj")

8. W oknie dialogowym **SAML configuration assistant** (Asystent konfiguracji SAML) wykonaj następujące kroki:

    ![Asystent konfiguracji SAML](./media/topdesk-secure-tutorial/ic790608.png "Asystent konfiguracji SAML")

    a. Aby przekazać plik metadanych pobrany w witrynie Azure Portal, w obszarze **Federation Metadata** (Metadane federacji) kliknij przycisk **Browse** (Przeglądaj).

    b. Aby przekazać plik certyfikatu, w obszarze **Certificate (RSA)** (Certyfikat — RSA) kliknij przycisk **Browse** (Przeglądaj).

    c. W obszarze **Private key (RSA, PKCS8, DER)** (Klucz prywatny — RSA, PKCS8, DER) możesz przekazać własny klucz prywatny lub klucz uzyskany od [zespołu pomocy technicznej klienta TOPdesk - Secure](https://www.topdesk.com/us/support).

    d. Aby przekazać plik logo uzyskany od zespołu pomocy technicznej TOPdesk, w obszarze **Logo icon** (Ikona logo) kliknij przycisk **Browse** (Przeglądaj).

    e. W polu tekstowym **User name attribute** (Atrybut nazwy użytkownika) wpisz `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    f. W polu tekstowym **Display name** (Nazwa wyświetlana) wpisz nazwę konfiguracji.

    g. Kliknij pozycję **Zapisz**.

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

W tej sekcji włączysz dla użytkownika Britta Simon możliwość korzystania z logowania jednokrotnego platformy Azure, udzielając dostępu do aplikacji TOPdesk - Secure.

1. W witrynie Azure Portal wybierz pozycję **Aplikacje dla przedsiębiorstw**, wybierz pozycję **Wszystkie aplikacje**, a następnie wybierz pozycję **TOPdesk - Secure**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wpisz i wybierz **TOPdesk - Secure**.

    ![Link TOPdesk - Secure na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-topdesk---secure-test-user"></a>Tworzenie użytkownika testowego aplikacji TOPdesk - Secure

Aby umożliwić użytkownikom usługi Azure AD logowanie się do aplikacji TOPdesk - Secure, należy ich aprowizować w aplikacji TOPdesk - Secure.  
W przypadku aplikacji TOPdesk - Secure jest to zadanie ręczne.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować aprowizację użytkowników, wykonaj następujące kroki:

1. Zaloguj się do firmowej witryny aplikacji **TOPdesk - Secure** jako administrator.

2. W menu u góry kliknij kolejno pozycje **TOPdesk \> New \> Support Files \> Operator** (TOPdesk > Nowy > Pliki pomocnicze > Operator).

    ![Operator](./media/topdesk-secure-tutorial/ic790610.png "Operator")

3. W oknie dialogowym **New Operator** (Nowy operator) wykonaj następujące kroki:

    ![Nowy operator](./media/topdesk-secure-tutorial/ic790611.png "Nowy operator")

    a. Kliknij kartę **General** (Ogólne).

    b. W polu tekstowym **Surname** (Nazwisko) wpisz nazwisko użytkownika, takie jak **Simon**.

    c. W sekcji **Location** (Lokalizacja) w polu **Site** (Lokacja) wybierz lokację dla konta.

    d. W sekcji **TOPdesk Login** (Logowanie w aplikacji TOPdesk) w polu tekstowym **Login Name** (Nazwa logowania) wpisz nazwę logowania użytkownika.

    e. Kliknij pozycję **Zapisz**.

> [!NOTE]
> Aby aprowizować konta użytkowników usługi AAD, można użyć jakichkolwiek innych narzędzi do tworzenia konta użytkownika aplikacji TOPdesk - Secure lub interfejsów API dostarczonych przez firmę TOPdesk - Secure.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka TOPdesk - Secure w panelu dostępu powinno nastąpić automatyczne zalogowanie do aplikacji TOPdesk - Secure, dla której skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

