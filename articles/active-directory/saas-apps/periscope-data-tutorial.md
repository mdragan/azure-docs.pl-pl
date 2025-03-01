---
title: 'Samouczek: integracja usługi Azure Active Directory z aplikacją Periscope Data | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory i aplikacją Periscope Data.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3f378edb-9ac9-494d-a84a-03357b923ee1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: efde1f1dafc62576398c5225ad1c652438fc0c31
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67094497"
---
# <a name="tutorial-azure-active-directory-integration-with-periscope-data"></a>Samouczek: integracja usługi Azure Active Directory z aplikacją Periscope Data

Z tego samouczka dowiesz się, jak zintegrować aplikację Periscope Data z usługą Azure Active Directory (Azure AD).
Integracja aplikacji Periscope Data z usługą Azure AD zapewnia następujące korzyści:

* Możesz kontrolować, kto ma dostęp do aplikacji Periscope Data, w usłudze Azure AD.
* Możesz zezwolić swoim użytkownikom na automatyczne logowanie do aplikacji Periscope Data (logowanie jednokrotne) przy użyciu kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Do skonfigurowania integracji usługi Azure AD z aplikacją Periscope Data potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja aplikacji Periscope Data z obsługą logowania jednokrotnego

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Aplikacja Periscope Data obsługuje logowanie jednokrotne zainicjowane przez **dostawcę usługi**

## <a name="adding-periscope-data-from-the-gallery"></a>Dodawanie aplikacji Periscope Data z galerii

Aby skonfigurować integrację aplikacji Periscope Data z usługą Azure AD, musisz dodać tę aplikację z galerii do swojej listy zarządzanych aplikacji SaaS.

**Aby dodać aplikację Periscope Data z galerii, wykonaj następujące czynności:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **Periscope Data**, wybierz pozycję **Periscope Data** z panelu wyników, a następnie kliknij przycisk **Dodaj**, aby dodać aplikację.

     ![Aplikacja Periscope Data na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD z aplikacją Periscope Data, korzystając z danych użytkownika testowego o nazwie **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację połączenia między użytkownikiem usługi Azure AD i powiązanym użytkownikiem aplikacji Periscope Data.

Aby skonfigurować i przetestować logowanie jednokrotne usługi Azure AD z aplikacją Periscope Data, należy wykonać czynności opisane w poniższych blokach konstrukcyjnych:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w aplikacji Periscope Data](#configure-periscope-data-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego w aplikacji Periscope Data](#create-periscope-data-test-user)** — aby w aplikacji Periscope Data utworzyć odpowiednik użytkownika Britta Simon połączony z reprezentacją użytkownika w usłudze Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD z aplikacją Periscope Data, wykonaj następujące kroki:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **Periscope Data** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. W sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące czynności:

    ![Domena i adresy URL aplikacji Periscope Data — informacje dotyczące logowania jednokrotnego](common/sp-identifier.png)

    a. W polu tekstowym **Adres URL logowania** wpisz jeden z następujących adresów URL:
    
    | |
    |--|
    | `https://app.periscopedata.com/` |
    | `https://app.periscopedata.com/app/<SITENAME>` |

    b. W polu tekstowym **Identyfikator (identyfikator jednostki)** wpisz adres URL, używając następującego wzorca: `https://app.periscopedata.com/<SITENAME>/sso`

    > [!NOTE]
    > Wartość adresu URL logowania nie jest prawdziwa. Zastąp wartości rzeczywistymi wartościami adresu URL logowania. Skontaktuj się z [zespołem obsługi klienta aplikacji Periscope Data](mailto:support@periscopedata.com), aby uzyskać tę wartość, a wartość identyfikatora uzyskasz w sekcji **Konfigurowanie logowania jednokrotnego w aplikacji Periscope Data** w dalszej części tego samouczka. Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

5. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij przycisk kopiowania, aby skopiować **adres URL metadanych federacji aplikacji** i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/copy-metadataurl.png)

### <a name="configure-periscope-data-single-sign-on"></a>Konfigurowanie logowania jednokrotnego w aplikacji Periscope Data

1. W innym oknie przeglądarki internetowej zaloguj się do aplikacji Periscope Data jako administrator.

2. Otwórz menu koła zębatego w lewym dolnym rogu, a następnie otwórz menu **Billing** (Rozliczenia)  >  **Security** (Zabezpieczenia) i wykonaj następujące kroki. Tylko administratorzy mają dostęp do tych ustawień.

    ![Informacje na temat konfigurowania aplikacji Periscope Data](./media/periscope-data-tutorial/configure01.png)

    a. Skopiuj **adres URL metadanych federacji aplikacji** z kroku 5. **Certyfikat podpisywania SAML** i otwórz go w przeglądarce. Spowoduje to otwarcie dokumentu XML.

    b. W polu tekstowym **Logowanie jednokrotne** wybierz pozycję **Azure Active Directory**.

    c. Znajdź tag **SingleSignOnService** i wklej wartość pola**Location** (Lokalizacja) w polu tekstowym **SSO URL** (Adres URL logowania jednokrotnego).

    d. Znajdź tag **SingleLogoutService** i wklej wartość pola**Location** w polu tekstowym **SLO URL** (Adres URL wylogowania jednokrotnego).

    e. Kopiuj **identyfikator** wartości dla swojego wystąpienia, a następnie wklej je **identyfikator jednostki** pole tekstowe z **podstawową konfigurację protokołu SAML** sekcji w witrynie Azure portal.

    f. Znajdź pierwszy znacznik pliku w formacie XML, skopiuj wartość **entityID** i wklej go w **wystawcy** pola tekstowego.

    g. Znajdź tag **IDPSSODescriptor** przy użyciu protokołu SAML. W tej sekcji znajdź tag **KeyDescriptor** przy użyciu polecenia **use=signing**. skopiuj wartość **X509Certificate** i wklej ją w polu tekstowym **Certyfikat**.

    h. W witrynach z wieloma przestrzeniami można wybrać domyślną przestrzeń w menu rozwijanym **Default Space** (Domyślna przestrzeń). Jest to przestrzeń, do której dodawani są nowy użytkownicy podczas pierwszego logowania w aplikacji Periscope Data i aprowizacji za pomocą logowania jednokrotnego w usłudze Active Directory.

    i. Na koniec kliknij polecenie **Save** (Zapisz) i **potwierdź** zmianę ustawień logowania jednokrotnego, wpisując **Logout** (Wyloguj).

    ![Informacje na temat konfigurowania aplikacji Periscope Data](./media/periscope-data-tutorial/configure02.png)

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

W tej sekcji włączysz dla użytkownika Britta Simon możliwość korzystania z logowania jednokrotnego na platformie Azure, udzielając dostępu do aplikacji Periscope Data.

1. W witrynie Azure Portal wybierz kolejno pozycje **Aplikacje dla przedsiębiorstw**, **Wszystkie aplikacje** oraz **Periscope Data**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz aplikację **Periscope Data**.

    ![Link do aplikacji Periscope Data na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-periscope-data-test-user"></a>Tworzenie użytkownika testowego w aplikacji Periscope Data

Użytkownicy usługi Azure AD mogą logować się w aplikacji Periscope Data po aprowizowaniu w usłudze Periscope Data. W aplikacji Periscope Data aprowizowanie wykonywane jest ręcznie.

**Aby aprowizować konto użytkownika, wykonaj następujące kroki:**

1. Zaloguj się w aplikacji Periscope Data jako administrator.

2. Kliknij ikonę **Settings** (Ustawienia) w lewym dolnym rogu menu i przejdź do pozycji **Permissions** (Uprawnienia).

    ![Informacje na temat konfigurowania aplikacji Periscope Data](./media/periscope-data-tutorial/configure03.png)

3. Kliknij pozycję **ADD USER** (Dodaj użytkownika) i wykonaj następujące czynności:

      ![Informacje na temat konfigurowania aplikacji Periscope Data](./media/periscope-data-tutorial/configure04.png)

    a. W polu tekstowym **First Name** (Imię) wprowadź imię użytkownika, na przykład **Britta**.

    b. W polu tekstowym **Last name** (Nazwisko) wprowadź nazwisko użytkownika, na przykład **Simon**.

    c. W **E-mail** tekstu wprowadź adres e-mail użytkownika, takich jak **brittasimon\@contoso.com**.

    d. Kliknij pozycję **ADD** (Dodaj).

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka Periscope Data w panelu dostępu powinno nastąpić automatyczne zalogowanie do aplikacji Periscope Data, dla której skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

