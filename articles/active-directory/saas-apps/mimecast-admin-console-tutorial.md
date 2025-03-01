---
title: 'Samouczek: Integracja usługi Azure Active Directory z konsolą Mimecast Admin Console | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługami Azure Active Directory i konsolą Mimecast Admin Console.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0bbbd73d1856ba5d3dc19873c56fce622b272939
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67097340"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Samouczek: Integracja usługi Azure Active Directory z konsolą Mimecast Admin Console

Z tego samouczka dowiesz się, jak zintegrować konsolę Mimecast Admin Console z usługą Azure Active Directory (Azure AD).
Zintegrowanie konsoli Mimecast Admin Console z usługą Azure AD zapewnia następujące korzyści:

* Możesz kontrolować w usłudze Azure AD, kto ma dostęp do konsoli Mimecast Admin Console.
* Możesz zezwolić swoim użytkownikom na automatyczne logowanie do konsoli Mimecast Admin Console (logowanie jednokrotne) przy użyciu kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z konsolą Mimecast Admin Console, potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja konsoli Mimecast Admin Console z obsługą logowania jednokrotnego

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Konsola Mimecast Admin Console obsługuje logowanie jednokrotne inicjowane przez **dostawcę usług**

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Dodawanie konsoli Mimecast Admin Console z galerii

Aby skonfigurować integrację konsoli Mimecast Admin Console z usługą Azure AD, należy dodać konsolę Mimecast Admin Console z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać konsolę Mimecast Admin Console z galerii, wykonaj następujące kroki:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **Mimecast Admin Console**, wybierz pozycję **Mimecast Admin Console** z panelu wyników, a następnie kliknij przycisk **Dodaj**, aby dodać aplikację.

     ![Konsola Mimecast Admin Console na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD z konsolą Mimecast Admin Console, korzystając z danych użytkownika testowego **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację połączenia między użytkownikiem usługi Azure AD i powiązanym użytkownikiem portalu Mimecast Admin Console.

Aby skonfigurować i przetestować logowanie jednokrotne usługi Azure AD z konsolą Mimecast Admin Console, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w konsoli Mimecast Admin Console](#configure-mimecast-admin-console-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego konsoli Mimecast Admin Console](#create-mimecast-admin-console-test-user)** — aby mieć w konsoli Mimecast Admin Console odpowiednik użytkownika Britta Simon, który jest połączony z reprezentacją użytkownika w usłudze Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD za pomocą konsoli Mimecast Admin Console, wykonaj następujące kroki:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **Mimecast Admin Console** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. W sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące czynności:

    ![Informacje o domenie i adresach URL logowania jednokrotnego konsoli Mimecast Admin Console](common/sp-signonurl.png)

    W polu tekstowym **Adres URL logowania** wpisz adres URL:
    
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > Adres URL logowania zależy od regionu.

4. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **certyfikat (Base64)** z podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/certificatebase64.png)

6. W sekcji **Konfigurowanie konsoli Mimecast Admin Console** skopiuj odpowiednie adresy URL zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    d. Adres URL wylogowywania

### <a name="configure-mimecast-admin-console-single-sign-on"></a>Konfigurowanie logowania jednokrotnego w konsoli Mimecast Admin Console

1. W innym oknie przeglądarki internetowej zaloguj się do swojej konsoli Mimecast Admin Console jako administrator.

2. Przejdź do strony **Usługi\>Aplikacja**.

    ![Usługi](./media/mimecast-admin-console-tutorial/ic794998.png "Usługi")

3. Kliknij pozycję **Profile uwierzytelniania**.

    ![Profile uwierzytelniania](./media/mimecast-admin-console-tutorial/ic794999.png "Profile uwierzytelniania")
    
4. Kliknij pozycję **Nowy profil uwierzytelniania**.

    ![Nowe profile uwierzytelniania](./media/mimecast-admin-console-tutorial/ic795000.png "Nowe profile uwierzytelniania")

5. W sekcji **Profil uwierzytelniania** wykonaj następujące kroki:

    ![Profil uwierzytelniania](./media/mimecast-admin-console-tutorial/ic795015.png "Profil uwierzytelniania")
    
    a. W polu tekstowym **Opis** wpisz nazwę konfiguracji.
    
    b. Wybierz pozycję **Wymuś uwierzytelnianie SAML dla konsoli Mimecast Admin Console**.
    
    c. W polu **Dostawca** wybierz opcję **Azure Active Directory**.
    
    d. W polu tekstowym **Adres URL wystawcy** wklej wartość **identyfikatora usługi Azure AD** skopiowaną z witryny Azure Portal.
    
    e. W polu tekstowym **Adres URL logowania** wklej wartość **adresu URL logowania** skopiowaną z witryny Azure Portal.

    f. W polu tekstowym **Adres URL wylogowywania** wklej wartość **adresu URL logowania** skopiowaną z witryny Azure Portal.
    
    >[!NOTE]
    >Wartość adresu URL logowania i wartość adresu URL wylogowywania są takie same dla konsoli Mimecast Admin Console.
    
    g. Otwórz w Notatniku swój certyfikat zakodowany w formacie base-64 pobrany z witryny Azure Portal, usuń pierwszy wiersz („ *--* ”) and the last line („ *--* ”), skopiuj pozostałą zawartość do schowka, a następnie wklej ją w polu tekstowym **Certyfikat dostawcy tożsamości (metadane)** .
    
    h. Wybierz pozycję **Zezwalaj na logowanie jednokrotne**.
    
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

W tej sekcji włączysz dla użytkownika Britta Simon możliwość korzystania z logowania jednokrotnego platformy Azure, udzielając dostępu do konsoli Mimecast Admin Console.

1. W witrynie Azure Portal wybierz pozycję **Aplikacje dla przedsiębiorstw**, wybierz pozycję **Wszystkie aplikacje**, a następnie wybierz pozycję **Mimecast Admin Console**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wpisz **Mimecast Admin Console** i wybierz odpowiednią pozycję.

    ![Link konsoli Mimecast Admin Console na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-mimecast-admin-console-test-user"></a>Tworzenie użytkownika testowego konsoli Mimecast Admin Console

Aby umożliwić użytkownikom usługi Azure AD logowanie do konsoli Mimecast Admin Console, należy ich aprowizować w konsoli Mimecast Admin Console. W przypadku konsoli Mimecast Admin Console aprowizowanie to zadanie wykonywane ręczne.

* Należy zarejestrować domenę, aby można było utworzyć użytkowników.

**Aby skonfigurować aprowizację użytkowników, wykonaj następujące czynności:**

1. Zaloguj się do konsoli **Mimecast Admin Console** jako administrator.

2. Przejdź do pozycji **Directories (Katalogi) \> Internal (Wewnętrzny)** .
   
    ![Katalogi](./media/mimecast-admin-console-tutorial/ic795003.png "Katalogi")

3. Kliknij pozycję **Register New Domain** (Rejestruj nową domenę).
   
    ![Rejestruj nową domenę](./media/mimecast-admin-console-tutorial/ic795004.png "Rejestruj nową domenę")

4. Po utworzeniu nowej domeny kliknij pozycję **New Address** (Nowy adres).
   
    ![Nowy adres](./media/mimecast-admin-console-tutorial/ic795005.png "Nowy adres")

5. W oknie dialogowym New Address (Nowy adres) wykonaj następujące kroki:
   
    ![Zapisz](./media/mimecast-admin-console-tutorial/ic795006.png "Zapisz")
   
    a. W polach tekstowych **Email Address** (Adres e-mail), **Global Name** (Nazwa globalna), **Password** (Hasło) i **Confirm Password** (Potwierdź hasło) wpisz wartości prawidłowego konta usługi Azure Active Directory, które chcesz aprowizować.

    b. Kliknij pozycję **Zapisz**.

>[!NOTE]
>Do aprowizowania kont użytkowników usługi Azure AD można użyć dowolnych innych narzędzi do tworzenia kont użytkowników konsoli Mimecast Admin Console udostępnianych przez tę konsolę. 

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka Mimecast Admin Console na panelu dostępu powinno nastąpić automatyczne zalogowanie do konsoli Mimecast Admin Console, dla której skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

