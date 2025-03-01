---
title: 'Samouczek: Integracja usługi Azure Active Directory z aplikacją Image Relay | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory i aplikacją Image Relay.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 37b7fe05f306f08d83de0832c5b1dd814f5560ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67100545"
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a>Samouczek: Integracja usługi Azure Active Directory z aplikacją Image Relay

Z tego samouczka dowiesz się, jak zintegrować aplikację Image Relay z usługą Azure Active Directory (Azure AD).
Zintegrowanie aplikacji Image Relay z usługą Azure AD zapewnia następujące korzyści:

* W usłudze Azure AD możesz kontrolować, kto ma dostęp do aplikacji Image Relay.
* Możesz zezwolić swoim użytkownikom na automatyczne logowanie do aplikacji Image Relay (logowanie jednokrotne) przy użyciu kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Do skonfigurowania integracji usługi Azure AD z aplikacją Image Relay potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja aplikacji Image Relay z obsługą logowania jednokrotnego

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Aplikacja Image Relay obsługuje logowanie jednokrotne inicjowane przez **dostawcę usług**

## <a name="adding-image-relay-from-the-gallery"></a>Dodawanie aplikacji Image Relay z galerii

Aby skonfigurować integrację aplikacji Image Relay z usługą Azure AD, należy z poziomu galerii dodać tę aplikację do listy zarządzanych aplikacji SaaS.

**Aby dodać aplikację Image Relay z galerii, wykonaj następujące czynności:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **Image Relay**, wybierz pozycję **Image Relay** z panelu wyników, a następnie kliknij przycisk **Dodaj**, aby dodać aplikację.

    ![Aplikacja Image Relay na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD z aplikacją Image Relay, korzystając z danych testowego użytkownika **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację połączenia między użytkownikiem usługi Azure AD i powiązanym użytkownikiem aplikacji Image Relay.

Aby skonfigurować i przetestować logowanie jednokrotne usługi Azure AD z aplikacją Image Relay, należy wykonać czynności opisane w poniższych blokach konstrukcyjnych:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w aplikacji Image Relay](#configure-image-relay-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego aplikacji Image Relay](#create-image-relay-test-user)** — aby w aplikacji Image Relay utworzyć odpowiednik użytkownika Britta Simon połączony z reprezentacją użytkownika w usłudze Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD przy użyciu aplikacji Image Relay, wykonaj następujące czynności:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **Image Relay** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. W sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące czynności:

    ![Informacje o domenie i adresach URL logowania jednokrotnego aplikacji Image Relay](common/sp-identifier.png)

    a. W polu tekstowym **Adres URL logowania** wpisz adres URL, używając następującego wzorca: `https://<companyname>.imagerelay.com/`

    b. W polu tekstowym **Identyfikator (identyfikator jednostki)** wpisz adres URL, używając następującego wzorca: `https://<companyname>.imagerelay.com/sso/metadata`

    > [!NOTE]
    > Te wartości nie są prawdziwe. Zaktualizuj te wartości przy użyciu rzeczywistego identyfikatora i adresu URL logowania. W celu uzyskania tych wartości skontaktuj się z [zespołem pomocy technicznej klienta aplikacji Image Relay](http://support.imagerelay.com/). Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

4. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **certyfikat (Base64)** z podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/certificatebase64.png)

6. W sekcji **Konfigurowanie aplikacji Image Relay** skopiuj odpowiednie adresy URL zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    d. Adres URL wylogowywania

### <a name="configure-image-relay-single-sign-on"></a>Konfigurowanie logowania jednokrotnego w aplikacji Image Relay

1. W innym oknie przeglądarki internetowej zaloguj się do firmowej witryny aplikacji Image Relay jako administrator.

2. Na górnym pasku narzędzi kliknij obciążenie **Users & Permissions** (Użytkownicy i uprawnienia).

    ![Konfigurowanie logowania jednokrotnego](./media/imagerelay-tutorial/tutorial_imagerelay_06.png) 

3. Kliknij pozycję **Create New Permission** (Utwórz nowe uprawnienie).

    ![Konfigurowanie logowania jednokrotnego](./media/imagerelay-tutorial/tutorial_imagerelay_08.png)

4. W obciążeniu **Single Sign On Settings** (Ustawienia logowania jednokrotnego) zaznacz pole wyboru **This Group can only sign-in via Single Sign On** (Ta grupa może logować się tylko za pośrednictwem logowania jednokrotnego), a następnie kliknij pozycję **Save** (Zapisz).

    ![Konfigurowanie logowania jednokrotnego](./media/imagerelay-tutorial/tutorial_imagerelay_09.png) 

5. Przejdź do pozycji **Account Settings** (Ustawienia konta).

    ![Konfigurowanie logowania jednokrotnego](./media/imagerelay-tutorial/tutorial_imagerelay_10.png) 

6. Przejdź do obciążenia **Single Sign On Settings** (Ustawienia logowania jednokrotnego).

    ![Konfigurowanie logowania jednokrotnego](./media/imagerelay-tutorial/tutorial_imagerelay_11.png)

7. W oknie dialogowym **SAML Settings** (Ustawienia SAML) wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/imagerelay-tutorial/tutorial_imagerelay_12.png)

    a. W polu tekstowym **Login URL** (Adres URL logowania) wklej wartość **adresu URL logowania** skopiowaną z witryny Azure Portal.

    b. W polu tekstowym **Logout URL** (Adres URL wylogowywania) wklej wartość **adresu URL wylogowywania** skopiowaną z witryny Azure Portal.

    c. Jako format **Name Id Format** (Format identyfikatora nazwy) wybierz pozycję **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.

    d. W obszarze **Binding Options for Requests from the Service Provider (Image Relay)** (Opcje wiązania dla żądań od dostawcy usług (Image Relay)) wybierz pozycję **POST Binding** (Powiązanie POST).

    e. W obszarze **x.509 Certificate** (Certyfikat x.509) kliknij pozycję **Update Certificate** (Zaktualizuj certyfikat).

    ![Konfigurowanie logowania jednokrotnego](./media/imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. Otwórz pobrany certyfikat w programie Notatnik, skopiuj zawartość, a następnie wklej ją w polu tekstowym **x.509 Certificate** (Certyfikat x.509).

    ![Konfigurowanie logowania jednokrotnego](./media/imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. W sekcji **Just-In-Time User Provisioning** (Aprowizowanie użytkowników typu just in time) wybierz pozycję **Enable Just-In-Time User Provisioning** (Włącz aprowizowanie użytkowników typu just in time).

    ![Konfigurowanie logowania jednokrotnego](./media/imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Wybierz grupę uprawnień (na przykład **SSO Basic** (Logowanie jednokrotne — podstawowe)), która umożliwia logowanie tylko za pośrednictwem logowania jednokrotnego.

    ![Konfigurowanie logowania jednokrotnego](./media/imagerelay-tutorial/tutorial_imagerelay_20.png)

    i. Kliknij pozycję **Zapisz**.

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

W tej sekcji włączysz dla użytkownika Britta Simon możliwość korzystania z logowania jednokrotnego platformy Azure, udzielając dostępu do usługi Image Relay.

1. W witrynie Azure Portal wybierz pozycję **Aplikacje dla przedsiębiorstw**, wybierz pozycję **Wszystkie aplikacje**, a następnie wybierz pozycję **Image Relay**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz pozycję **Image Relay**.

    ![Link aplikacji Image Relay na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-image-relay-test-user"></a>Tworzenie użytkownika testowego aplikacji Image Relay

W tej sekcji utworzysz użytkownika o nazwie Britta Simon w aplikacji Image Relay.

**Aby utworzyć użytkownika o nazwie Britta Simon w aplikacji Image Relay, wykonaj następujące kroki:**

1. Zaloguj się do firmowej witryny aplikacji Image Relay jako administrator.

2. Przejdź do obszaru **Users & Permissions** (Użytkownicy i uprawnienia) i wybierz pozycję **Create SSO User** (Utwórz użytkownika logowania jednokrotnego).

    ![Konfigurowanie logowania jednokrotnego](./media/imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. Wprowadź wartości następujących pól: **Email** (Adres e-mail), **First Name** (Imię), **Last Name** (Nazwisko) i **Company** (Firma) dla użytkownika, którego chcesz aprowizować, i wybierz grupę uprawnień (np. SSO Basic (Logowanie jednokrotne — podstawowe)), czyli grupę umożliwiającą logowania tylko za pośrednictwem logowania jednokrotnego.

    ![Konfigurowanie logowania jednokrotnego](./media/imagerelay-tutorial/tutorial_imagerelay_22.png)

4. Kliknij pozycję **Utwórz**.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka Image Relay w panelu dostępu powinno nastąpić automatyczne zalogowanie do aplikacji Image Relay, dla której skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)