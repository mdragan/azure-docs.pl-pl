---
title: 'Samouczek: Integracja usługi Azure Active Directory z aplikacją Meta Networks Connector | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory i aplikacją Meta Networks Connector.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4ae5f30d-113b-4261-b474-47ffbac08bf7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: 82097b075ab86c4de5ab45e3110c9bc192ac7edb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67097347"
---
# <a name="tutorial-azure-active-directory-integration-with-meta-networks-connector"></a>Samouczek: Integracja usługi Azure Active Directory z aplikacją Meta Networks Connector

Z tego samouczka dowiesz się, jak zintegrować aplikację Meta Networks Connector z usługą Azure Active Directory (Azure AD).
Integracja aplikacji Meta Networks Connector z usługą Azure AD zapewnia następujące korzyści:

* W usłudze Azure AD możesz kontrolować, kto ma dostęp do aplikacji Meta Networks Connector.
* Swoim użytkownikom możesz zezwolić na automatyczne logowanie do aplikacji Meta Networks Connector (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z aplikacją Meta Networks Connector, potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja aplikacji Meta Networks Connector z włączoną obsługą logowania jednokrotnego

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Aplikacja Meta Networks Connector obsługuje logowanie jednokrotne inicjowane przez **dostawcę usług** oraz **dostawcę tożsamości**
 
* Aplikacja Meta Networks Connector obsługuje aprowizację użytkowników typu **just in time**

## <a name="adding-meta-networks-connector-from-the-gallery"></a>Dodawanie aplikacji Meta Networks Connector z galerii

Aby skonfigurować integrację aplikacji Meta Networks Connector z usługą Azure AD, należy dodać aplikację Meta Networks Connector z galerii do swojej listy zarządzanych aplikacji SaaS.

**Aby dodać aplikację Meta Networks Connector z galerii, wykonaj następujące kroki:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz wartość **Meta Networks Connector**, wybierz pozycję **Meta Networks Connector** z panelu wyników, a następnie kliknij przycisk **Dodaj**, aby dodać aplikację.

     ![Aplikacja Meta Networks Connector na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w aplikacji Meta Networks Connector, korzystając z danych testowego użytkownika **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację połączenia między użytkownikiem usługi Azure AD i powiązanym użytkownikiem aplikacji Meta Networks Connector.

Aby skonfigurować i przetestować logowanie jednokrotne usługi Azure AD w aplikacji Meta Networks Connector, należy wykonać czynności opisane w poniższych blokach konstrukcyjnych:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w aplikacji Meta Networks Connector](#configure-meta-networks-connector-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego aplikacji Meta Networks Connector](#create-meta-networks-connector-test-user)** — aby w aplikacji Meta Networks Connector utworzyć odpowiednik użytkownika Britta Simon połączony z reprezentacją użytkownika w usłudze Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD w aplikacji Meta Networks Connector, wykonaj następujące czynności:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **Meta Networks Connector** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. Jeśli chcesz skonfigurować aplikację w trybie inicjowanym przez **dostawcę tożsamości**, w sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące kroki:

    ![Informacje o domenie i adresach URL logowania jednokrotnego aplikacji Meta Networks Connector](common/idp-intiated.png)

    a. W polu **Identyfikator** wpisz adres URL, korzystając z następującego wzorca: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/saml/metadata`

    b. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/sso/saml`

5. Kliknij przycisk **Ustaw dodatkowe adresy URL** i wykonaj następujący krok, jeśli chcesz skonfigurować aplikację w trybie inicjowania przez **dostawcę usług**:

    ![Informacje o domenie i adresach URL logowania jednokrotnego aplikacji Meta Networks Connector](common/both-advanced-urls.png)

    a. W polu tekstowym **Adres URL logowania** wpisz adres URL, korzystając z następującego wzorca: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/login`

    b. W polu tekstowym **Stan przekaźnika** wpisz adres URL, korzystając z następującego wzorca: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/#/`

    > [!NOTE]
    > Te wartości nie są prawdziwe. Zaktualizuj te wartości, używając rzeczywistego identyfikatora, adresu URL odpowiedzi i adresu URL logowania, które zostały opisane w dalszej części tego samouczka.

6. Aplikacja Meta Networks Connector oczekuje asercji SAML w określonym formacie, który wymaga dodania mapowań atrybutów niestandardowych do konfiguracji atrybutów tokenów języka SAML. Poniższy zrzut ekranu przedstawia listę atrybutów domyślnych. Kliknij ikonę  **Edytuj** , aby otworzyć okno dialogowe  **Atrybuty użytkownika** .

    ![image](common/edit-attribute.png)
    
7. Oprócz powyższych, aplikacja Meta Networks Connector oczekuje jeszcze kilku atrybutów, które powinny zostać przekazane w odpowiedzi SAML. W sekcji **Oświadczenia użytkownika** w oknie dialogowym **Atrybuty użytkownika** wykonaj następujące czynności, aby dodać atrybut tokenu SAML, jak pokazano w poniższej tabeli:
    
    | Name (Nazwa) | Atrybut źródłowy | Przestrzeń nazw|
    | ---------------| --------------- | -------- |
    | firstname | user.givenname | |
    | lastname | user.surname | |
    | emailaddress| user.mail| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | name | user.userprincipalname| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | phone | user.telephonenumber | |

    a. Kliknij przycisk **Dodaj nowe oświadczenie**, aby otworzyć okno dialogowe **Zarządzanie oświadczeniami użytkownika**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. W polu tekstowym **Nazwa** wpisz nazwę atrybutu pokazaną dla tego wiersza.

    d. Pozostaw pole **Przestrzeń nazw** puste.

    d. Dla opcji Źródło wybierz wartość **Atrybut**.

    e. Na liście **Atrybut źródłowy** wpisz wartość atrybutu pokazaną dla tego wiersza.

    f. Kliknij przycisk **OK**.

    g. Kliknij pozycję **Zapisz**.

8. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **certyfikat (Base64)** z podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/certificatebase64.png)

9. W sekcji **Konfigurowanie aplikacji Meta Networks Connector** skopiuj odpowiednie adresy URL zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    c. Adres URL wylogowywania

### <a name="configure-meta-networks-connector-single-sign-on"></a>Konfigurowanie logowania jednokrotnego w aplikacji Meta Networks Connector

1. Otwórz nową kartę w przeglądarce i zaloguj się na swoim koncie administratora aplikacji Meta Networks Connector.
    
    > [!NOTE]
    > Aplikacja Meta Networks Connector to bezpieczny system. Dlatego przed uzyskaniem dostępu do ich portalu należy uzyskać swój publiczny adres IP, które są dodawane do listy dozwolonych na bok. Aby uzyskać swój publiczny adres IP, skorzystaj z [tego](https://whatismyipaddress.com/) linku. Wyślij swój adres IP, aby [zespołem pomocy technicznej klienta łącznika sieci Meta](mailto:support@metanetworks.com) można pobrać adresu IP, dodane do listy dozwolonych.
    
2. Przejdź do obszaru **Administrator**, a następnie wybierz pozycję **Settings (Ustawienia)** .
    
    ![Konfigurowanie logowania jednokrotnego](./media/metanetworksconnector-tutorial/configure3.png)
    
3. Upewnij się, że ustawienia **Log Internet Traffic (Rejestruj ruch internetowy)** i **Force VPN MFA (Wymuś uwierzytelnianie wieloskładnikowe w sieci VPN)** są wyłączone.
    
    ![Konfigurowanie logowania jednokrotnego](./media/metanetworksconnector-tutorial/configure1.png)
    
4. Przejdź do obszaru **Administrator**, a następnie wybierz pozycję **SAML**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/metanetworksconnector-tutorial/configure4.png)
    
5. Wykonaj następujące kroki opisane na stronie **DETAILS (SZCZEGÓŁY)** :
    
    ![Konfigurowanie logowania jednokrotnego](./media/metanetworksconnector-tutorial/configure2.png)
    
    a. Skopiuj wartość pola **SSO URL (Adres URL logowania jednokrotnego)** i wklej ją w polu tekstowym **Sign-In URL (Adres URL logowania)** w sekcji **Meta Networks Connector Domain and URLs (Adresy URL i domena aplikacji Meta Networks Connector)** .
    
    b. Skopiuj wartość pola **Recipient URL (Adres URL adresata)** i wklej ją w polu tekstowym **Reply URL (Adres URL odpowiedzi)** w sekcji **Meta Networks Connector Domain and URLs (Adresy URL i domena aplikacji Meta Networks Connector)** .
    
    c. Skopiuj wartość pola **Audience URI (SP Entity ID) (Identyfikator URI — identyfikator jednostki dostawcy usług)** i wklej ją w polu tekstowym **Identifier (Entity ID) (Identyfikator — identyfikator jednostki)** w sekcji **Meta Networks Connector Domain and URLs (Adresy URL i domena aplikacji Meta Networks Connector)** .
    
    d. Włączanie protokołu SAML
    
6. Na karcie **GENERAL (OGÓLNE)** wykonaj następujące kroki:

    ![Konfigurowanie logowania jednokrotnego](./media/metanetworksconnector-tutorial/configure5.png)

    a. W polu **Identity Provider Single Sign-On URL (Adres URL logowania jednokrotnego dostawcy tożsamości)** wklej wartość pola **Adres URL logowania** skopiowaną z witryny Azure Portal.

    b. W polu **Identity Provider Issuer (Wystawca dostawcy tożsamości)** wklej wartość pola **Identyfikator usługi Azure AD** skopiowaną z witryny Azure Portal.

    c. Otwórz w Notatniku certyfikat pobrany z witryny Azure Portal, a następnie wklej go w polu tekstowym **X.509 Certificate (Certyfikat X.509)** .

    d. Włącz **aprowizację typu just in time**.

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD 

W tej sekcji w witrynie Azure Portal utworzysz użytkownika testowego o nazwie Britta Simon.

1. W witrynie Azure Portal w okienku po lewej stronie wybierz pozycję **Azure Active Directory**, wybierz opcję **Użytkownicy**, a następnie wybierz pozycję **Wszyscy użytkownicy**.

    ![Linki „Użytkownicy i grupy” i „Wszyscy użytkownicy”](common/users.png)

2. Wybierz przycisk **Nowy użytkownik** w górnej części ekranu.

    ![Przycisk Nowy użytkownik](common/new-user.png)

3. We właściwościach użytkownika wykonaj następujące kroki.

    ![Okno dialogowe Użytkownik](common/user-properties.png)

    a. W **nazwa** wprowadź **BrittaSimon**.
  
    b. W **nazwa_użytkownika** wpisz **brittasimon\@yourcompanydomain.extension**  
    Na przykład: BrittaSimon@contoso.com

    d. Zaznacz pole wyboru **Pokaż hasło** i zanotuj wartość wyświetlaną w polu Hasło.

    d. Kliknij pozycję **Utwórz**.

### <a name="assign-the-azure-ad-test-user"></a>Przypisywanie użytkownika testowego usługi Azure AD

W tej sekcji włączysz dla użytkownika Britta Simon możliwość korzystania z logowania jednokrotnego platformy Azure, udzielając dostępu do aplikacji Meta Networks Connector.

1. W witrynie Azure Portal wybierz pozycję **Aplikacje dla przedsiębiorstw**, wybierz pozycję **Wszystkie aplikacje**, a następnie wybierz pozycję **Meta Networks Connector**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz pozycję **Meta Networks Connector**.

    ![Link do aplikacji Meta Networks Connector na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-meta-networks-connector-test-user"></a>Tworzenie użytkownika testowego aplikacji Meta Networks Connector

W tej sekcji w aplikacji Meta Networks Connector jest tworzony użytkownik o nazwie Britta Simon. Aplikacja Meta Networks Connector obsługuje aprowizację typu just in time, która jest domyślnie włączona. W tej sekcji nie musisz niczego robić. Jeśli użytkownik jeszcze nie istnieje w aplikacji Meta Networks Connector, zostanie utworzony podczas próby uzyskania dostępu do tej aplikacji.

>[!Note]
>Jeśli konieczne jest ręczne utworzenie użytkownika, skontaktuj się z [zespołem pomocy technicznej klienta aplikacji Meta Networks Connector](mailto:support@metanetworks.com).

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka Meta Networks Connector na panelu dostępu powinno nastąpić automatyczne zalogowanie do aplikacji Meta Networks Connector, dla której skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

