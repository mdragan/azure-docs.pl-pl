---
title: 'Samouczek: Integracja usługi Azure Active Directory z bramą za wynagrodzeniem | Dokumentacja firmy Microsoft'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługi Azure Active Directory i bramą osób trzecich.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 34336386-998a-4d47-ab55-721d97708e5e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 1cdc4420758b67dc3a578b9530fb7039ed53ee66
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092796"
---
# <a name="tutorial-azure-active-directory-integration-with-reward-gateway"></a>Samouczek: Integracja usługi Azure Active Directory z bramą za wynagrodzeniem

W tym samouczku dowiesz się, jak zintegrować nagradzania bramy za pomocą usługi Azure Active Directory (Azure AD).
Integrowanie bramy za wynagrodzeniem z usługą Azure AD zapewnia następujące korzyści:

* Możesz kontrolować, czy w usłudze Azure AD, kto ma dostęp do bramy osób trzecich.
* Użytkownikom można automatycznie zalogowany do nagrody bramy (logowanie jednokrotne) można włączyć za pomocą kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Do konfigurowania integracji z usługą Azure AD za pomocą bramy nagrody, potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie ma środowiska usługi Azure AD, możesz pobrać [bezpłatne konto](https://azure.microsoft.com/free/)
* Nagrody bramy logowanie jednokrotne włączone subskrypcji

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Nagradzaj Gateway obsługuje **tożsamości** jednokrotne logowanie inicjowane przez

## <a name="adding-reward-gateway-from-the-gallery"></a>Dodawanie bramy za wynagrodzeniem z galerii

Aby skonfigurować integrację za wynagrodzeniem bramy w usłudze Azure AD, należy dodać bramę z Silicon z galerii z listą zarządzanych aplikacji SaaS.

**Aby dodać bramę nagradzania z galerii, wykonaj następujące czynności:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **bramy za wynagrodzeniem**, wybierz opcję **bramy za wynagrodzeniem** z panelu wynik następnie kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

     ![Brama osób trzecich, na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji, konfigurowania i testowania usługi Azure AD logowania jednokrotnego z bramą za wynagrodzeniem w oparciu o nazwie użytkownika testowego **Britta Simon**.
Dla logowania jednokrotnego do pracy relację łącza między użytkownika usługi Azure AD i powiązanego użytkownika w bramie nagradzania musi zostać ustanowione.

Aby skonfigurować i testowanie usługi Azure AD logowania jednokrotnego z bramą za wynagrodzeniem, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie nagradzania bramy logowania jednokrotnego](#configure-reward-gateway-single-sign-on)**  — Aby skonfigurować ustawienia logowania jednokrotnego na stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego bramy za wynagrodzeniem](#create-reward-gateway-test-user)**  — aby odpowiednikiem Britta Simon w bramy nagrody, która jest połączona z usługi Azure AD reprezentacja użytkownika.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować usługi Azure AD logowania jednokrotnego z bramą nagrody, wykonaj następujące czynności:

1. W [witryny Azure portal](https://portal.azure.com/)na **bramy za wynagrodzeniem** strona integracji aplikacji, wybierz opcję **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** wykonaj następujące kroki:

    ![Nagrody bramy domena i adresy URL pojedynczego logowania jednokrotnego informacji](common/idp-intiated.png)

    a. W polu tekstowym **Identyfikator** wpisz adres URL, korzystając z następującego wzorca:
    
    | |
    |--|
    | `https://<companyname>.rewardgateway.com`|
    | `https://<companyname>.rewardgateway.co.uk/`|
    | `https://<companyname>.rewardgateway.co.nz/`|
    | `https://<companyname>.rewardgateway.com.au/`|

    b. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca:
    
    | |
    |--|
    |  `https://<companyname>.rewardgateway.com/Authentication/EndLogin?idp=<Unique Id>`|
    | `https://<companyname>.rewardgateway.co.uk/Authentication/EndLogin?idp=<Unique Id>`|
    | `https://<companyname>.rewardgateway.co.nz/Authentication/EndLogin?idp=<Unique Id>`|
    | `https://<companyname>.rewardgateway.com.au/Authentication/EndLogin?idp=<Unique Id>`|

    > [!NOTE]
    > Te wartości nie są prawdziwe. Zastąp te wartości rzeczywistymi wartościami identyfikatora i adresu URL odpowiedzi. Aby uzyskać te wartości, należy uruchomić konfigurowania integracji w portalu programu nagradzania Manager. Szczegółowe informacje można znaleźć w https://success.rewardgateway.com/authentication-integrations/microsoft-azure-for-authentication

5. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **kod XML metadanych federacji** na podstawie podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/metadataxml.png)

6. Na **konfigurowania bramy za wynagrodzeniem** sekcji, skopiuj odpowiednie adresy URL, zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    d. Adres URL wylogowywania

### <a name="configure-reward-gateway-single-sign-on"></a>Konfigurowanie nagradzania bramy logowania jednokrotnego

Aby skonfigurować logowanie jednokrotne na **bramy za wynagrodzeniem** po stronie, rozpocząć konfigurowanie integracji w portalu programu nagradzania Manager. Użyj pobranego metadanych do uzyskania certyfikatu podpisywania, a następnie przekaż, podczas konfiguracji. Szczegółowe informacje można znaleźć w https://success.rewardgateway.com/authentication-integrations/microsoft-azure-for-authentication

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD

W tej sekcji w witrynie Azure Portal utworzysz użytkownika testowego o nazwie Britta Simon.

1. W witrynie Azure Portal w okienku po lewej stronie wybierz pozycję **Azure Active Directory**, wybierz opcję **Użytkownicy**, a następnie wybierz pozycję **Wszyscy użytkownicy**.

    ![Linki „Użytkownicy i grupy” i „Wszyscy użytkownicy”](common/users.png)

2. Wybierz przycisk **Nowy użytkownik** w górnej części ekranu.

    ![Przycisk Nowy użytkownik](common/new-user.png)

3. We właściwościach użytkownika wykonaj następujące kroki.

    ![Okno dialogowe Użytkownik](common/user-properties.png)

    a. W polu **Nazwa** wprowadź **BrittaSimon**.
  
    b. W **nazwa_użytkownika** typ pola `brittasimon@yourcompanydomain.extension`  
    Na przykład: BrittaSimon@contoso.com

    d. Zaznacz pole wyboru **Pokaż hasło** i zanotuj wartość wyświetlaną w polu Hasło.

    d. Kliknij pozycję **Utwórz**.

### <a name="assign-the-azure-ad-test-user"></a>Przypisywanie użytkownika testowego usługi Azure AD

W tej sekcji możesz włączyć Britta Simon do użycia platformy Azure logowania jednokrotnego przez udzielenie dostępu do bramy osób trzecich.

1. W witrynie Azure portal wybierz **aplikacje dla przedsiębiorstw**, wybierz opcję **wszystkie aplikacje**, a następnie wybierz **bramy za wynagrodzeniem**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz **bramy za wynagrodzeniem**.

    ![Link nagradzania bramy na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-reward-gateway-test-user"></a>Tworzenie użytkownika testowego za wynagrodzeniem bramy

W tej sekcji utworzysz użytkownika o nazwie Britta Simon w bramie osób trzecich. Praca z [zespołu pomocy technicznej za wynagrodzeniem bramy](mailto:clientsupport@rewardgateway.com) Aby dodać użytkowników na platformie za wynagrodzeniem bramy. Użytkownicy muszą być tworzone i aktywowana, aby używać logowania jednokrotnego.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka nagradzania bramy w panelu dostępu, powinien zostać automatycznie zarejestrowaniu w usłudze bramy nagrody, dla którego skonfigurować logowanie Jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
