---
title: 'Samouczek: Integracja usługi Azure Active Directory z aplikacją etouches | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory i aplikacją etouches.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee28d5a9ec3a8df694cd1971be8a73ff8e976cc3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67103157"
---
# <a name="tutorial-azure-active-directory-integration-with-etouches"></a>Samouczek: Integracja usługi Azure Active Directory z aplikacją etouches

Z tego samouczka dowiesz się, jak zintegrować aplikację etouches z usługą Azure Active Directory (Azure AD).
Integracja aplikacji etouches z usługą Azure AD oferuje następujące korzyści:

* Możesz kontrolować w usłudze Azure AD, kto ma dostęp do aplikacji etouches.
* Możesz zezwolić użytkownikom na automatyczne logowanie do aplikacji etouches (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Do skonfigurowania integracji usługi Azure AD z aplikacją etouches potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja aplikacji etouches z obsługą logowania jednokrotnego

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Aplikacja etouches obsługuje logowanie jednokrotne inicjowane przez **dostawcę usług**

## <a name="adding-etouches-from-the-gallery"></a>Dodawanie aplikacji etouches z galerii

Aby skonfigurować integrację aplikacji etouches w usłudze Azure AD, należy dodać aplikację etouches z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać aplikację etouches z galerii, wykonaj następujące kroki:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **etouches**, wybierz pozycję **etouches** z panelu wyników i kliknij przycisk **Dodaj**, aby dodać aplikację.

     ![Aplikacja etouches na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD z aplikacją etouches, korzystając z danych użytkownika testowego **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację połączenia między użytkownikiem usługi Azure AD i powiązanym użytkownikiem aplikacji etouches.

Aby skonfigurować i przetestować logowanie jednokrotne usługi Azure AD w aplikacji etouches, należy wykonać kroki opisane w poniższych blokach konstrukcyjnych:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w aplikacji etouches](#configure-etouches-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego w aplikacji etouches](#create-etouches-test-user)** — aby w aplikacji etouches utworzyć odpowiednik użytkownika Britta Simon połączony z reprezentacją użytkownika w usłudze Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD w aplikacji etouches, wykonaj następujące kroki:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **etouches** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. W sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące czynności:

    ![Informacje o domenie i adresach URL logowania jednokrotnego aplikacji etouches](common/sp-identifier.png)

    a. W polu tekstowym **Adres URL logowania** wpisz adres URL, używając następującego wzorca: `https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`

    b. W polu tekstowym **Identyfikator (identyfikator jednostki)** wpisz adres URL, używając następującego wzorca: `https://www.eiseverywhere.com/<instance name>`

    > [!NOTE] 
    > Te wartości nie są prawdziwe. Zaktualizuj te wartości, używając rzeczywistego identyfikatora i adresu URL logowania, co zostało opisane w dalszej części tego samouczka.
    > 

5. Aplikacja etouches oczekuje asercji SAML w określonym formacie, który wymaga dodania mapowań atrybutów niestandardowych do konfiguracji atrybutów tokenów języka SAML. Poniższy zrzut ekranu przedstawia listę atrybutów domyślnych. Kliknij ikonę **Edytuj**, aby dodać atrybuty.

    ![image](common/edit-attribute.png)

6. Oprócz powyższych aplikacja etouches oczekuje jeszcze kilku atrybutów, które powinny zostać przekazane w odpowiedzi SAML. W sekcji **Oświadczenia użytkownika** w oknie dialogowym **Atrybuty użytkownika** wykonaj następujące czynności, aby dodać atrybut tokenu SAML, jak pokazano w poniższej tabeli:

    | Name (Nazwa) | Atrybut źródłowy|
    | ------------------- | -------------------- |
    | Email | user.mail | 

    a. Kliknij przycisk **Dodaj nowe oświadczenie**, aby otworzyć okno dialogowe **Zarządzanie oświadczeniami użytkownika**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. W polu tekstowym **Nazwa** wpisz nazwę atrybutu pokazaną dla tego wiersza.

    d. Pozostaw pole **Przestrzeń nazw** puste.

    d. Dla opcji Źródło wybierz wartość **Atrybut**.

    e. Na liście **Atrybut źródłowy** wpisz wartość atrybutu pokazaną dla tego wiersza.

    f. Kliknij przycisk **OK**.

    g. Kliknij pozycję **Zapisz**.

7. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **kod XML metadanych federacji** na podstawie podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/metadataxml.png)

8. W sekcji **Konfigurowanie aplikacji etouches** skopiuj odpowiednie adresy URL zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    d. Adres URL wylogowywania

### <a name="configure-etouches-single-sign-on"></a>Konfigurowanie logowania jednokrotnego w aplikacji etouches

1. Aby skonfigurować dla aplikacji logowanie jednokrotne, wykonaj następujące kroki w aplikacji etouches: 

    ![Konfigurowanie aplikacji etouches](./media/etouches-tutorial/tutorial_etouches_06.png) 

    a. Zaloguj się w aplikacji **etouches** przy użyciu uprawnień administratora.
   
    b. Przejdź do konfiguracji **SAML**.

    c. W sekcji **General Settings** (Ustawienie ogólne) otwórz w Notatniku certyfikat pobrany z witryny Azure Portal, skopiuj zawartość, a następnie wklej ją w polu tekstowym metadanych IDP. 

    d. Kliknij przycisk **Save & Stay** (Zapisz i pozostań).
  
    e. Kliknij przycisk **Update Metadata** (Aktualizuj metadane) w sekcji metadanych SAML. 

    f. Spowoduje to otwarcie strony i wykonanie logowania jednokrotnego. Gdy logowanie jednokrotne będzie już działać, będzie można skonfigurować nazwę użytkownika.

    g. W polu Username (Nazwa użytkownika) wybierz pozycję **emailaddress**, jak pokazano na poniższej ilustracji. 

    h. Skopiuj wartość **SP entity ID** (Identyfikator jednostki dostawcy usług) i wklej ją w polu tekstowym **Identyfikator** w sekcji **Podstawowa konfiguracja protokołu SAML** witryny Azure Portal.

    i. Skopiuj wartość **SSO URL / ACS** i wklej ją w polu tekstowym **Adres URL logowania** w sekcji **Podstawowa konfiguracja protokołu SAML** witryny Azure Portal.
   
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

W tej sekcji włączysz dla użytkownika Britta Simon możliwość korzystania z logowania jednokrotnego platformy Azure, udzielając dostępu do aplikacji etouches.

1. W witrynie Azure Portal wybierz pozycję **Aplikacje dla przedsiębiorstw**, pozycję **Wszystkie aplikacje**, a następnie pozycję **etouches**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz pozycję **etouches**.

    ![Link do aplikacji etouches na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-etouches-test-user"></a>Tworzenie użytkownika testowego aplikacji etouches

W tej sekcji utworzysz użytkownika Britta Simon w aplikacji etouches. Skontaktuj się z [zespołem pomocy technicznej aplikacji etouches](https://www.etouches.com/event-software/support/customer-support/) w celu dodania użytkowników na platformie etouches.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka etouches w panelu dostępu powinno nastąpić automatyczne zalogowanie do aplikacji etouches, dla której skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

