---
title: 'Samouczek: Integracja usługi Azure Active Directory za pomocą NetDocuments | Dokumentacja firmy Microsoft'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługi Azure Active Directory i NetDocuments.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: jeedes
ms.openlocfilehash: 929d5d7a8e2b45aeb4ef71e4599cfcf23be83088
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67096601"
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Samouczek: Integracja usługi Azure Active Directory za pomocą NetDocuments

W tym samouczku dowiesz się, jak zintegrować NetDocuments w usłudze Azure Active Directory (Azure AD).
Integrowanie NetDocuments z usługą Azure AD zapewnia następujące korzyści:

* Możesz kontrolować, czy w usłudze Azure AD, kto ma dostęp do NetDocuments.
* Aby umożliwić użytkownikom można automatycznie zalogowany do NetDocuments (logowanie jednokrotne) przy użyciu konta usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD za pomocą NetDocuments, potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* NetDocuments pojedynczego logowania jednokrotnego włączonych subskrypcji

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Obsługuje NetDocuments **SP** jednokrotne logowanie inicjowane przez

## <a name="adding-netdocuments-from-the-gallery"></a>Dodawanie NetDocuments z galerii

Aby skonfigurować integrację NetDocuments w usłudze Azure AD, należy dodać NetDocuments z galerii z listą zarządzanych aplikacji SaaS.

**Aby dodać NetDocuments z galerii, wykonaj następujące czynności:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **NetDocuments**, wybierz opcję **NetDocuments** z panelu wynik kliknięcie **Dodaj** przycisk, aby dodać aplikację.

     ![NetDocuments na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji, konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne za pomocą NetDocuments w oparciu o użytkownika testu o nazwie **Britta Simon**.
Dla logowania jednokrotnego do pracy relację łącza między użytkownika usługi Azure AD i powiązanego użytkownika w NetDocuments musi zostać ustanowione.

Aby skonfigurować i testowanie usługi Azure AD logowanie jednokrotne za pomocą NetDocuments, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie NetDocuments logowania jednokrotnego](#configure-netdocuments-single-sign-on)**  — Aby skonfigurować ustawienia logowania jednokrotnego na stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego NetDocuments](#create-netdocuments-test-user)**  — aby odpowiednikiem Britta Simon w NetDocuments połączonego z usługi Azure AD reprezentacja użytkownika.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować usługę Azure AD logowanie jednokrotne z NetDocuments, wykonaj następujące czynności:

1. W [witryny Azure portal](https://portal.azure.com/)na **NetDocuments** strona integracji aplikacji, wybierz opcję **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. W sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące czynności:

    ![NetDocuments domena i adresy URL pojedynczego logowania jednokrotnego informacji](common/sp-reply.png)

    a. W polu tekstowym **Adres URL logowania** wpisz adres URL, używając następującego wzorca: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<Repository ID>`

    b. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<Repository ID>`

    > [!NOTE]
    > Te wartości nie są prawdziwe. Rzeczywisty symbol dla adresu URL i adresu URL odpowiedzi, należy zaktualizować te wartości. Identyfikator repozytorium jest wartością, począwszy od **CA -** następuje skojarzony z repozytorium NetDocuments 8 kod znaku. Możesz sprawdzić [dokument pomocy technicznej tożsamości federacyjnych NetDocuments](https://support.netdocuments.com/hc/en-us/articles/205220410-Federated-Identity-Login) Aby uzyskać więcej informacji. Alternatywnie możesz skontaktować się ze [zespołem pomocy technicznej klienta NetDocuments](https://support.netdocuments.com/hc/) do uzyskania tych wartości, jeśli masz trudności Konfigurowanie, korzystając z informacji podanych powyżej. Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

5. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **kod XML metadanych federacji** na podstawie podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/metadataxml.png)

6. Na **Konfigurowanie NetDocuments** sekcji, skopiuj odpowiednie adresy URL, zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    c. Adres URL wylogowywania

### <a name="configure-netdocuments-single-sign-on"></a>Konfigurowanie NetDocuments logowanie jednokrotne

1. W oknie przeglądarki internetowej innej Zaloguj się do witryny firmy NetDocuments, jako administrator.

2. Przejdź do **administratora**.

3. Kliknij przycisk **Dodawanie i usuwanie użytkowników i grup**.
   
    ![Repozytorium](./media/netdocuments-tutorial/ic795047.png "repozytorium")

4. Kliknij przycisk **skonfiguruj zaawansowane opcje uwierzytelniania**.
    
    ![Skonfiguruj zaawansowane opcje uwierzytelniania](./media/netdocuments-tutorial/ic795048.png "skonfiguruj zaawansowane opcje uwierzytelniania")

5. Na **tożsamości federacyjnych** okno dialogowe, należy wykonać następujące czynności:
   
    ![Federacyjna Identitty](./media/netdocuments-tutorial/ic795049.png "federacyjnych Identitty")
   
    a. Jako **typ serwera tożsamości federacyjnych**, wybierz opcję **Active Directory Federation Services**.
   
    b. Kliknij przycisk **Choose file**, aby przekazać plik metadanych pobrany, który został pobrany z witryny Azure portal.
   
    c. Kliknij przycisk **OK**.

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

W tej sekcji możesz włączyć Britta Simon do używania usługi Azure logowanie jednokrotne za udzielanie dostępu do NetDocuments.

1. W witrynie Azure portal wybierz **aplikacje dla przedsiębiorstw**, wybierz opcję **wszystkie aplikacje**, a następnie wybierz **NetDocuments**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz **NetDocuments**.

    ![Link NetDocuments na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-netdocuments-test-user"></a>Tworzenie użytkownika testowego NetDocuments

Aby umożliwić użytkownikom usługi Azure AD, zaloguj się do NetDocuments, musi być obsługiwana w NetDocuments.  
W przypadku NetDocuments Inicjowanie obsługi administracyjnej jest zadanie ręczne.

**Aby aprowizować konto użytkownika, wykonaj następujące kroki:**

1. SING do Twojej **NetDocuments** witryny firmy jako administrator.

2. W menu u góry kliknij pozycję **Admin** (Administrator).
   
    ![Administrator](./media/netdocuments-tutorial/ic795051.png "Administrator")

3. Kliknij przycisk **Dodawanie i usuwanie użytkowników i grup**.
   
    ![Repozytorium](./media/netdocuments-tutorial/ic795047.png "repozytorium")

4. W **adres E-mail** polu tekstowym wpisz adres e-mail prawidłowego konta usługi Azure Active Directory do aprowizowania, a następnie kliknij przycisk **Dodaj użytkownika**.
   
    ![Adres e-mail](./media/netdocuments-tutorial/ic795053.png "adres E-mail")
   
    >[!NOTE]
    >Właściciel konta usługi Azure Active Directory otrzyma wiadomość e-mail zawierającą link do potwierdzenia konta, zanim stanie się aktywny. Można użyć jakichkolwiek innych NetDocuments użytkownika konta tworzenie narzędzi lub interfejsów API dostarczonych przez NetDocuments do świadczenia usługi Azure Active Directory kont użytkowników.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka NetDocuments w panelu dostępu, powinien zostać automatycznie zarejestrowaniu w usłudze NetDocuments, dla którego skonfigurować logowanie Jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

