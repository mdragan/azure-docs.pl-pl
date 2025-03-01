---
title: 'Samouczek: Integracja usługi Azure Active Directory z TOPdesk - Public | Dokumentacja firmy Microsoft'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługi Azure Active Directory i TOPdesk — publiczny.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: jeedes
ms.openlocfilehash: a2a0ffd670a03aeaaa262b83127a385be9efc978
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67088473"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Samouczek: Integracja usługi Azure Active Directory za pomocą TOPdesk — publiczny

W tym samouczku dowiesz się, jak zintegrować TOPdesk - publicznej za pomocą usługi Azure Active Directory (Azure AD).
Integrowanie TOPdesk - publicznej z usługą Azure AD zapewnia następujące korzyści:

* Możesz kontrolować, czy w usłudze Azure AD, kto ma dostęp do TOPdesk - publicznego.
* Aby umożliwić użytkownikom można automatycznie zalogowany do TOPdesk — publiczny (logowanie jednokrotne) przy użyciu konta usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Do konfigurowania integracji z usługą Azure AD z TOPdesk - publiczny, potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* TOPdesk — publiczny logowanie jednokrotne włączone subskrypcji

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Obsługuje publicznego TOPdesk - **SP** jednokrotne logowanie inicjowane przez

## <a name="adding-topdesk---public-from-the-gallery"></a>Dodawanie TOPdesk - publicznego z galerii

Aby skonfigurować integrację TOPdesk - publicznego w usłudze Azure AD, należy dodać TOPdesk - publicznego z galerii z listą zarządzanych aplikacji SaaS.

**Aby dodać TOPdesk - publicznego z galerii, wykonaj następujące czynności:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **TOPdesk — publiczny**, wybierz opcję **TOPdesk — publiczny** z panelu wynik kliknięcie **Dodaj** przycisk, aby dodać aplikację.

     ![TOPdesk - publiczny, na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji możesz skonfigurować i przetestować usługi Azure AD logowanie jednokrotne za pomocą TOPdesk — publiczny w oparciu o nazwie użytkownika testowego **Britta Simon**.
Dla logowania jednokrotnego do pracy, relację łącza między użytkownika usługi Azure AD i powiązanego użytkownika w TOPdesk — publicznego musi zostać ustanowione.

Do konfigurowania i testowania usługi Azure AD logowanie jednokrotne za pomocą TOPdesk - publiczny, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie TOPdesk — publiczny logowania jednokrotnego](#configure-topdesk---public-single-sign-on)**  — Aby skonfigurować ustawienia logowania jednokrotnego na stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Utwórz TOPdesk - użytkownika testowego publicznych](#create-topdesk---public-test-user)**  — aby odpowiednikiem Britta Simon w TOPdesk - publiczny, połączonego z usługi Azure AD reprezentacja użytkownika.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować usługę Azure AD logowanie jednokrotne z TOPdesk — publiczna, wykonaj następujące czynności:

1. W [witryny Azure portal](https://portal.azure.com/)na **TOPdesk - Public** strona integracji aplikacji, wybierz opcję **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4.  W sekcji **Podstawowa konfiguracja protokołu SAML**, jeśli masz **plik metadanych dostawcy usługi**, wykonaj następujące kroki:

    >[!NOTE]
    >Zostanie wyświetlony **plik metadanych dostawcy usług** z **TOPdesk skonfigurować — publiczny logowania jednokrotnego** sekcję, co zostało wyjaśnione w dalszej części tego samouczka.

    a. Kliknij pozycję **Przekaż plik metadanych**.
    
    ![Przekazywanie pliku metadanych](common/upload-metadata.png)

    b. Kliknij **logo folderu**, aby wybrać plik metadanych, a następnie kliknij pozycję **Przekaż**.

    ![wybierz plik metadanych](common/browse-upload-metadata.png)

    c. Po pomyślnym przekazaniu pliku metadanych **identyfikator** i **adres URL odpowiedzi** wartości Uzyskaj automatycznie wypełnione w sekcji podstawową konfigurację protokołu SAML.

    ![TOPdesk — domeny publicznej i adresów URL pojedynczy informacje logowania jednokrotnego](common/sp-identifier-reply.png)

    d. W polu tekstowym **Adres URL logowania** wpisz adres URL, korzystając z następującego wzorca: `https://<companyname>.topdesk.net`

    e. W **identyfikator** pole tekstowe, wpisz adres URL przy użyciu następującego wzorca: `https://<companyname>.topdesk.net/tas/public/login/verify`

    > [!NOTE] 
    > Jeśli **identyfikator** i **adres URL odpowiedzi** wartości nie są automatycznie wypełniane, należy wprowadzić je ręcznie. Dla identyfikatora, postępuj zgodnie ze wzorcem, jak wspomniano powyżej, a adres URL odpowiedzi korzyści płynących z **TOPdesk skonfigurować — publiczny logowania jednokrotnego** sekcję, co zostało wyjaśnione w dalszej części tego samouczka. **Adres URL logowania** wartość nie jest prawdziwe, więc należy zaktualizować wartości za pomocą adresu URL logowania rzeczywiste. Skontaktuj się z pomocą [TOPdesk - zespołem pomocy technicznej publicznych klienta](https://help.topdesk.com/saas/enterprise/user/) można uzyskać wartość. Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

5. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **kod XML metadanych federacji** na podstawie podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/metadataxml.png)

6. Na **Konfigurowanie TOPdesk - Public** sekcji, skopiuj odpowiednie adresy URL, zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    c. Adres URL wylogowywania

### <a name="configure-topdesk---public-single-sign-on"></a>Konfigurowanie TOPdesk — publiczny logowania jednokrotnego

1. Zaloguj się na swoje **TOPdesk - Public** witryny firmy jako administrator.

2. W menu **TOPdesk** kliknij pozycję **Settings** (Ustawienia).
   
    ![Ustawienia](./media/topdesk-public-tutorial/ic790598.png "Ustawienia")

3. Kliknij pozycję **Login Settings** (Ustawienia logowania).
   
    ![Ustawienia logowania](./media/topdesk-public-tutorial/ic790599.png "Ustawienia logowania")

4. Rozwiń menu **Login Settings** (Ustawienia logowania), a następnie kliknij pozycję **General** (Ogólne).
   
    ![Ogólne](./media/topdesk-public-tutorial/ic790600.png "Ogólne")

5. W **publicznych** części **logowania języka SAML** konfiguracji sekcji, wykonaj następujące czynności:
   
    ![Ustawienia techniczne](./media/topdesk-public-tutorial/ic790601.png "Ustawienia techniczne")
   
    a. Kliknij pozycję **Download** (Pobierz), aby pobrać publiczny plik metadanych, a następnie zapisz go lokalnie na komputerze.
   
    b. Otwórz plik metadanych pobrany, a następnie zlokalizuj **AssertionConsumerService** węzła.

    ![AssertionConsumerService](./media/topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Kopiuj **AssertionConsumerService** wartość, wklej tę wartość w **adres URL odpowiedzi** polu tekstowym w **podstawową konfigurację protokołu SAML** sekcji.      
   
6. Aby utworzyć plik certyfikatu, wykonaj następujące kroki:
    
    ![Certyfikat](./media/topdesk-public-tutorial/ic790606.png "Certyfikat")
    
    a. Otwórz plik metadanych pobrany w witrynie Azure Portal.
    
    b. Rozwiń węzeł **RoleDescriptor**, w którym element **xsi:type** ma wartość **fed:ApplicationServiceType**.
    
    c. Skopiuj wartość węzła**X509Certificate**.
    
    d. Zapisz skopiowaną wartość **X509Certificate** lokalnie na komputerze w pliku.

7. W sekcji **Public** (Publiczne) kliknij przycisk **Add** (Dodaj).
    
    ![Logowanie SAML](./media/topdesk-public-tutorial/ic790625.png "Logowanie SAML")

8. W oknie dialogowym **SAML configuration assistant** (Asystent konfiguracji SAML) wykonaj następujące kroki:
    
    ![Asystent konfiguracji SAML](./media/topdesk-public-tutorial/ic790608.png "Asystent konfiguracji SAML")
    
    a. Aby przekazać plik metadanych pobrany w witrynie Azure Portal, w obszarze **Federation Metadata** (Metadane federacji) kliknij przycisk **Browse** (Przeglądaj).

    b. Aby przekazać plik certyfikatu, w obszarze **Certificate (RSA)** (Certyfikat — RSA) kliknij przycisk **Browse** (Przeglądaj).

    c. Aby przekazać plik logo uzyskany od zespołu pomocy technicznej TOPdesk, w obszarze **Logo icon** (Ikona logo) kliknij przycisk **Browse** (Przeglądaj).

    d. W polu tekstowym **User name attribute** (Atrybut nazwy użytkownika) wpisz `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. W polu tekstowym **Display name** (Nazwa wyświetlana) wpisz nazwę konfiguracji.

    f. Kliknij pozycję **Zapisz**.

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD 

W tej sekcji w witrynie Azure Portal utworzysz użytkownika testowego o nazwie Britta Simon.

1. W witrynie Azure Portal w okienku po lewej stronie wybierz pozycję **Azure Active Directory**, wybierz opcję **Użytkownicy**, a następnie wybierz pozycję **Wszyscy użytkownicy**.

    ![Linki „Użytkownicy i grupy” i „Wszyscy użytkownicy”](common/users.png)

2. Wybierz przycisk **Nowy użytkownik** w górnej części ekranu.

    ![Przycisk Nowy użytkownik](common/new-user.png)

3. We właściwościach użytkownika wykonaj następujące kroki.

    ![Okno dialogowe Użytkownik](common/user-properties.png)

    a. W polu **Nazwa** wprowadź **BrittaSimon**.
  
    b. W **nazwa_użytkownika** typ pola brittasimon@yourcompanydomain.extension. Na przykład: BrittaSimon@contoso.com

    d. Zaznacz pole wyboru **Pokaż hasło** i zanotuj wartość wyświetlaną w polu Hasło.

    d. Kliknij pozycję **Utwórz**.

### <a name="assign-the-azure-ad-test-user"></a>Przypisywanie użytkownika testowego usługi Azure AD

W tej sekcji możesz włączyć Britta Simon do używania platformy Azure logowanie jednokrotne za udzielanie dostępu do TOPdesk - publicznego.

1. W witrynie Azure portal wybierz **aplikacje dla przedsiębiorstw**, wybierz opcję **wszystkie aplikacje**, a następnie wybierz **TOPdesk - Public**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz **TOPdesk - Public**.

    ![TOPdesk - publicznego linku na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-topdesk---public-test-user"></a>Utwórz TOPdesk - użytkownika testowego publiczne

Aby umożliwić użytkownikom usługi Azure AD zalogować się do TOPdesk - publiczny, musi być obsługiwana w TOPdesk - publicznego. W przypadku TOPdesk - publiczny, inicjowanie obsługi administracyjnej jest zadanie ręczne.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować aprowizację użytkowników, wykonaj następujące kroki:

1. Zaloguj się na swoje **TOPdesk - Public** witryny firmy jako administrator.

2. W menu u góry kliknij **TOPdesk \> New \> pliki obsługi \> osoby**.
   
    ![Osoby](./media/topdesk-public-tutorial/ic790628.png "osoby")

3. W oknie dialogowym nowej osoby wykonaj następujące czynności:
   
    ![Nową osobę](./media/topdesk-public-tutorial/ic790629.png "nowej osoby")
   
    a. Kliknij kartę Ogólne.

    b. W **nazwisko** polu tekstowym wpisz nazwisko użytkownika, takich jak Simon
 
    c. Wybierz **witryny** dla konta.
 
    d. Kliknij pozycję **Zapisz**.

> [!NOTE]
> Możesz użyć wszelkie inne TOPdesk — narzędzia do tworzenia konta użytkownika publicznego lub interfejsów API dostarczonych przez TOPdesk — publiczny można uaktywniać ich konta usługi Azure AD.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu TOPdesk - Public kafelka w panelu dostępu, powinien zostać automatycznie zarejestrowaniu w usłudze TOPdesk - publiczny, dla którego skonfigurować logowanie Jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
