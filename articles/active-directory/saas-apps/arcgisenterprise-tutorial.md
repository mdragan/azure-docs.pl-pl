---
title: 'Samouczek: Integracja usługi Azure Active Directory z aplikacją ArcGIS Enterprise | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory i aplikacją ArcGIS Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 24809e9d-a4aa-4504-95a9-e4fcf484f431
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/28/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9642dc67a3707862009efa59d99f0cef54b9b878
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67106684"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-enterprise"></a>Samouczek: Integracja usługi Azure Active Directory z aplikacją ArcGIS Enterprise

W tym samouczku dowiesz się, jak zintegrować aplikację ArcGIS Enterprise z usługą Azure Active Directory (Azure AD).
Integracja aplikacji ArcGIS Enterprise z usługą Azure AD zapewnia następujące korzyści:

* Możesz kontrolować w usłudze Azure AD, kto ma dostęp do aplikacji ArcGIS Enterprise.
* Możesz zezwolić swoim użytkownikom na automatyczne logowanie w aplikacji ArcGIS Enterprise (logowanie jednokrotne) przy użyciu kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Do skonfigurowania integracji usługi Azure AD z aplikacją ArcGIS Enterprise potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja aplikacji ArcGIS Enterprise z włączonym logowaniem jednokrotnym

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.



* Aplikacja ArcGIS Enterprise obsługuje logowanie jednokrotne inicjowane zarówno przez **dostawcę usługi (SP), jak i dostawcę tożsamości (IDP)**
* Aplikacja ArcGIS Enterprise obsługuje aprowizowanie użytkowników typu **Just In Time**


## <a name="adding-arcgis-enterprise-from-the-gallery"></a>Dodawanie aplikacji ArcGIS Enterprise z galerii

Aby skonfigurować integrację aplikacji ArcGIS Enterprise z usługą Azure AD, należy dodać aplikację ArcGIS Enterprise z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać aplikację ArcGIS Enterprise z galerii, wykonaj następujące kroki:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **ArcGIS Enterprise**, wybierz opcję **ArcGIS Enterprise** z panelu wyników, a następnie kliknij przycisk **Dodaj**, aby dodać aplikację.

     ![Aplikacja ArcGIS Enterprise na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD z aplikacją [Application name] w oparciu o użytkownika testowego o nazwie **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację łącza między użytkownikiem usługi Azure AD i powiązanym użytkownikiem aplikacji [Application name].

Aby skonfigurować i przetestować logowanie jednokrotne usługi Azure AD z aplikacją [Application name], należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w aplikacji ArcGIS Enterprise](#configure-arcgis-enterprise-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego aplikacji ArcGIS Enterprise](#create-arcgis-enterprise-test-user)** — aby utworzyć odpowiednik użytkownika Britta Simon w aplikacji ArcGIS Enterprise, który będzie powiązany z reprezentacją użytkownika usługi Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD z aplikacją [Application name], wykonaj następujące czynności:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **ArcGIS Enterprise** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. Jeśli chcesz skonfigurować aplikację w trybie inicjowanym przez **dostawcę tożsamości**, w sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące kroki:

    ![Informacje o domenie i adresach URL pojedynczego logowania aplikacji ArcGIS Enterprise](common/idp-intiated.png)

    a. W polu **Identyfikator** wpisz adres URL, korzystając z następującego wzorca: `<EXTERNAL_DNS_NAME>.portal`

    b. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin2`

    c. Kliknij przycisk **Ustaw dodatkowe adresy URL** i wykonaj następujący krok, jeśli chcesz skonfigurować aplikację w trybie inicjowania przez **dostawcę usług**:

    ![Informacje o domenie i adresach URL pojedynczego logowania aplikacji ArcGIS Enterprise](common/metadata-upload-additional-signon.png)

    W polu tekstowym **Adres URL logowania** wpisz adres URL, korzystając z następującego wzorca: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin`

    > [!NOTE]
    > Te wartości nie są prawdziwe. Należy je zastąpić rzeczywistymi wartościami identyfikatora, adresu URL odpowiedzi i adresu URL logowania. Aby uzyskać te wartości, skontaktuj się z [zespołem pomocy technicznej klienta aplikacji ArcGIS Enterprise](mailto:support@esri.com). Wartość identyfikatora można uzyskać z **sekcji Set Identity Provider (Ustawianie dostawcy tożsamości)** , co wyjaśniono w dalszej części tego samouczka.

5. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij przycisk kopiowania, aby skopiować **adres URL metadanych federacji aplikacji** i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/copy-metadataurl.png)

### <a name="configure-arcgis-enterprise-single-sign-on"></a>Konfigurowanie logowania jednokrotnego aplikacji ArcGIS Enterprise

1. W innym oknie przeglądarki internetowej zaloguj się w witrynie firmowej ArcGIS Enterprise jako administrator.

2. Wybierz pozycję **Organization >EDIT SETTINGS** (Organizacja > EDYTUJ USTAWIENIA).

    ![Konfiguracja aplikacji ArcGIS Enterprise](./media/arcgisenterprise-tutorial/configure1.png)

3. Wybierz kartę **Security** (Zabezpieczenia).

    ![Konfiguracja aplikacji ArcGIS Enterprise](./media/arcgisenterprise-tutorial/configure2.png)

4. Przewiń w dół do sekcji **Enterprise Logins via SAML** (Logowanie dla przedsiębiorstwa za pomocą protokołu SAML) i wybierz pozycję **SET ENTERPRISE LOGIN** (USTAW LOGOWANIE DLA PRZEDSIĘBIORSTWA).

    ![Konfiguracja aplikacji ArcGIS Enterprise](./media/arcgisenterprise-tutorial/configure3.png)

5. W sekcji **Set Identity Provider** (Ustawianie dostawcy tożsamości) wykonaj następujące działania:

    ![Konfiguracja aplikacji ArcGIS Enterprise](./media/arcgisenterprise-tutorial/configure4.png)

    a. W polu tekstowym **Name** (Nazwa) podaj nazwę, na przykład **Test usługi Azure Active Directory**.

    b. W polu tekstowym **URL** (Adres URL) wklej wartość **adresu URL metadanych federacyjnych aplikacji**, którą skopiowano z witryny Azure Portal.

    c. Kliknij pozycję **Show advanced settings** (Pokaż ustawienia zaawansowane) i skopiuj wartość z pola **Entity ID** (Identyfikator jednostki), a następnie wklej ją w polu tekstowym **Identyfikator** w sekcji **Domena i adresy URL aplikacji ArcGIS Enterprise** w witrynie Azure Portal.
    
    ![Konfiguracja aplikacji ArcGIS Enterprise](./media/arcgisenterprise-tutorial/configure5.png)

    d. Kliknij przycisk **UPDATE IDENTITY PROVIDER** (ZAKTUALIZUJ DOSTAWCĘ TOŻSAMOŚCI).

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

W tej sekcji włączysz dla użytkownika Britta Simon możliwość korzystania z logowania jednokrotnego platformy Azure, udzielając dostępu do aplikacji ArcGIS Enterprise.

1. W witrynie Azure Portal wybierz pozycję **Aplikacje dla przedsiębiorstw**, pozycję **Wszystkie aplikacje**, a następnie pozycję **ArcGIS Enterprise**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wpisz i wybierz opcję **ArcGIS Enterprise**.

    ![Link do aplikacji ArcGIS Enterprise na liście Aplikacje](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-arcgis-enterprise-test-user"></a>Tworzenie użytkownika testowego aplikacji ArcGIS Enterprise

W tej sekcji w aplikacji ArcGIS Enterprise jest tworzony użytkownik o nazwie Britta Simon. Aplikacja ArcGIS Enterprise obsługuje aprowizację użytkowników just in time, która jest domyślnie włączona. W tej sekcji nie musisz niczego robić. Jeśli użytkownik jeszcze nie istnieje w aplikacji ArcGIS Enterprise, zostanie utworzony po uwierzytelnieniu.

> [!Note]
> Jeśli konieczne jest ręczne utworzenie użytkownika, skontaktuj się z  [zespołem pomocy technicznej aplikacji ArcGIS Enterprise](mailto:support@esri.com).

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka ArcGIS Enterprise w panelu dostępu powinno nastąpić automatyczne zalogowanie do aplikacji ArcGIS Enterprise, dla której skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

