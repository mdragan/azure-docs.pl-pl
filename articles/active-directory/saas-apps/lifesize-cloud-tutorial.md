---
title: 'Samouczek: integracja usługi Azure Active Directory z aplikacją Lifesize Cloud | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory i aplikacją Lifesize Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/4/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f751eb9dfb8ef65bea80993ddbd3a682b1d47111
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67098059"
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Samouczek: integracja usługi Azure Active Directory z aplikacją Lifesize Cloud

Z tego samouczka dowiesz się, jak zintegrować aplikację Lifesize Cloud z usługą Azure Active Directory (Azure AD).
Integracja aplikacji Lifesize Cloud z usługą Azure AD zapewnia następujące korzyści:

* W usłudze Azure AD możesz kontrolować, kto ma dostęp do aplikacji Lifesize Cloud.
* Twoi użytkownicy mogą być automatycznie logowani do aplikacji Lifesize Cloud (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Do skonfigurowania integracji usługi Azure AD z aplikacją Lifesize Cloud potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja aplikacji Lifesize Cloud z obsługą logowania jednokrotnego

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Aplikacja Lifesize Cloud obsługuje logowanie jednokrotne inicjowane przez **dostawcę usługi**

* Aplikacja Lifesize Cloud obsługuje **zautomatyzowaną** aprowizację użytkowników

## <a name="adding-lifesize-cloud-from-the-gallery"></a>Dodawanie aplikacji Lifesize Cloud z galerii

Aby skonfigurować integrację aplikacji Lifesize Cloud z usługą Azure AD, należy dodać aplikację Lifesize Cloud z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać aplikację Lifesize Cloud z galerii, wykonaj następujące kroki:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **Lifesize Cloud**, wybierz pozycję **Lifesize Cloud** z panelu wyników, a następnie kliknij przycisk **Dodaj**, aby dodać aplikację.

     ![Aplikacja Lifesize Cloud na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD z aplikacją Lifesize Cloud, korzystając z danych użytkownika testowego o nazwie **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację połączenia między użytkownikiem usługi Azure AD i powiązanym użytkownikiem aplikacji Lifesize Cloud.

Aby skonfigurować i przetestować logowanie jednokrotne usługi Azure AD z aplikacją Lifesize Cloud, należy wykonać czynności opisane w poniższych blokach konstrukcyjnych:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w aplikacji Lifesize Cloud](#configure-lifesize-cloud-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego aplikacji Lifesize Cloud](#create-lifesize-cloud-test-user)** — aby udostępnić w aplikacji Lifesize Cloud odpowiednik użytkownika Britta Simon połączony z reprezentacją użytkownika w usłudze Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD w aplikacji Lifesize Cloud, wykonaj następujące kroki:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **Lifesize Cloud** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. W sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące czynności:

    ![Informacje o domenie i adresach URL logowania jednokrotnego aplikacji Lifesize Cloud](common/sp-identifier-relay.png)

    a. W polu tekstowym **Adres URL logowania** wpisz adres URL, korzystając z następującego wzorca: `https://login.lifesizecloud.com/ls/?acs`

    b. W polu **Identyfikator** wpisz adres URL, korzystając z następującego wzorca: `https://login.lifesizecloud.com/<companyname>`

    c. Kliknij pozycję **Ustaw dodatkowe adresy URL**.

    d. W polu tekstowym **Stan przekaźnika** wpisz adres URL, korzystając z następującego wzorca: `https://webapp.lifesizecloud.com/?ent=<identifier>`

    > [!NOTE]
    > Te wartości nie są prawdziwe. Zastąp je rzeczywistymi wartościami adresu URL logowania, identyfikatora i stanu przekazywania. Skontaktuj się z [zespołem pomocy technicznej klienta aplikacji Lifesize Cloud](https://www.lifesize.com/en/support), aby uzyskać adres URL logowania i wartości identyfikatora, zaś wartość stanu przekazywania możesz pobrać z konfiguracji logowania jednokrotnego, która jest opisana w dalszej części tego samouczka. Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

5. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **certyfikat (Base64)** z podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/certificatebase64.png)

6. W sekcji **Konfigurowanie aplikacji Lifesize Cloud** skopiuj odpowiednie adresy URL zgodnie ze swoimi wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    d. Adres URL wylogowywania

### <a name="configure-lifesize-cloud-single-sign-on"></a>Konfigurowanie logowania jednokrotnego aplikacji Lifesize Cloud

1. Aby skonfigurować logowanie jednokrotne dla Twojej aplikacji, zaloguj się do aplikacji Lifesize Cloud z uprawnieniami administratora.

2. W prawym górnym rogu kliknij swoją nazwę, a następnie kliknij pozycję **Ustawienia zaawansowane**.

    ![Konfigurowanie logowania jednokrotnego](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

3. W ustawieniach zaawansowanych kliknij teraz link **Konfiguracja logowania jednokrotnego**. Spowoduje to otwarcie strony konfiguracji logowania jednokrotnego dla Twojego wystąpienia.

    ![Konfigurowanie logowania jednokrotnego](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

4. Teraz skonfiguruj następujące wartości w interfejsie użytkownika konfiguracji logowania jednokrotnego.

    ![Konfigurowanie logowania jednokrotnego](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    a. W polu tekstowym **Wystawca dostawcy tożsamości** wklej wartość pola **Identyfikator usługi Azure AD** skopiowaną z witryny Azure Portal.

    b.  W polu tekstowym **Login URL** (Adres URL logowania) wklej wartość **adresu URL logowania** skopiowaną z witryny Azure Portal.

    c. Otwórz w Notatniku swój certyfikat zakodowany w formacie base-64 pobrany z witryny Azure Portal, skopiuj zawartość do schowka, a następnie wklej ją w polu tekstowym **Certyfikat X.509**.
  
    d. W mapowaniach atrybutu protokołu SAML dla pola tekstowego Imię wprowadź wartość jako `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

    e. W mapowaniu atrybutu protokołu SAML dla pola tekstowego **Nazwisko** wprowadź wartość jako `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`

    f. W mapowaniu atrybutu protokołu SAML dla pola tekstowego **Adres e-mail** wprowadź wartość jako `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

5. Aby sprawdzić konfigurację, możesz kliknąć przycisk **Test**.

    >[!NOTE]
    >W celu zapewnienia pomyślnego testowania należy zakończyć działanie kreatora konfiguracji w usłudze Azure AD oraz zapewnić dostęp do użytkowników lub grup, które mogą wykonać test.

6. Włącz logowanie jednokrotne, wybierając przycisk **Włącz logowanie jednokrotne**.

7. Teraz kliknij przycisk **Aktualizuj**, aby wszystkie ustawienia zostały zapisane. Spowoduje to wygenerowanie wartości RelayState. Skopiuj wartość RelayState, która jest generowana w polu tekstowym, wklej ją w polu tekstowym **Stan przekazywania** w sekcji **Domena i adresy URL aplikacji Lifesize Cloud**.

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

W tej sekcji włączysz dla użytkownika Britta Simon możliwość korzystania z logowania jednokrotnego platformy Azure, udzielając dostępu do aplikacji Lifesize Cloud.

1. W witrynie Azure Portal wybierz pozycję **Aplikacje dla przedsiębiorstw**, wybierz pozycję **Wszystkie aplikacje**, a następnie wybierz pozycję **Lifesize Cloud**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz pozycję **Lifesize Cloud**.

    ![Link do aplikacji Lifesize Cloud na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-lifesize-cloud-test-user"></a>Tworzenie użytkownika testowego aplikacji Lifesize Cloud

W tej sekcji utworzysz użytkownika o nazwie Britta Simon w aplikacji Lifesize Cloud. Aplikacja Lifesize Cloud obsługuje automatyczną aprowizację użytkownika. Po pomyślnym uwierzytelnieniu w usłudze Azure AD użytkownik będzie automatycznie aprowizowany w aplikacji.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka Lifesize Cloud w panelu dostępu przejdziesz do strony logowania aplikacji Lifesize Cloud. W tym miejscu musisz wprowadzić swoją nazwę użytkownika, a następnie zostaniesz przekierowany do strony głównej aplikacji.

Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
