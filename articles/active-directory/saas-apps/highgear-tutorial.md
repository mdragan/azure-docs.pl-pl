---
title: 'Samouczek: integracja usługi Azure Active Directory z aplikacją HighGear | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory i aplikacją HighGear.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 55dcd2fb-96b7-46ec-9e69-eee71c535773
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ad7aedf90ead4b21bab661bdaf85316cd648fd75
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67101271"
---
# <a name="tutorial-azure-active-directory-integration-with-highgear"></a>Samouczek: integracja usługi Azure Active Directory z aplikacją HighGear

Z tego samouczka dowiesz się, jak zintegrować aplikację HighGear z usługą Azure Active Directory (Azure AD).
Zintegrowanie aplikacji HighGear z usługą Azure AD zapewnia następujące korzyści:

* W usłudze Azure AD możesz kontrolować, kto ma dostęp do aplikacji HighGear.
* Możesz zezwolić swoim użytkownikom na automatyczne logowanie do aplikacji HighGear (logowanie jednokrotne) przy użyciu kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z aplikacją HighGear, potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* System HighGear z licencją Enterprise lub Unlimited

## <a name="scenario-description"></a>Opis scenariusza

Z tego samouczka dowiesz się, jak skonfigurować i przetestować logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Aplikacja HighGear obsługuje logowanie jednokrotne inicjowane przez **dostawcę usługi i dostawcę tożsamości**

## <a name="adding-highgear-from-the-gallery"></a>Dodawanie aplikacji HighGear z galerii

Aby skonfigurować integrację aplikacji HighGear z usługą Azure AD, musisz dodać aplikację HighGear z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać aplikację HighGear z galerii, wykonaj następujące kroki:**

1. W witrynie **[Azure Portal](https://portal.azure.com)** w panelu nawigacyjnym po lewej stronie kliknij ikonę **Azure Active Directory**.

    ![Przycisk usługi Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![W bloku aplikacji przedsiębiorstwa](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij przycisk **Nowa aplikacja** w górnej części okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **HighGear**, wybierz pozycję **HighGear** z panelu wyników i kliknij przycisk **Dodaj**, aby dodać aplikację.

     ![Aplikacja HighGear na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

Z tej sekcji dowiesz się, jak skonfigurować i przetestować logowanie jednokrotne usługi Azure AD w systemie HighGear, korzystając z danych testowego użytkownika **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację połączenia między użytkownikiem usługi Azure AD i powiązanym użytkownikiem systemu HighGear.

Aby skonfigurować i przetestować logowanie jednokrotne usługi Azure AD z systemem HighGear, należy wykonać czynności opisane w poniższych blokach konstrukcyjnych:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w aplikacji HighGear](#configure-highgear-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji HighGear.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego aplikacji HighGear](#create-highgear-test-user)**  — aby zapewnić odpowiednik użytkownika Britta Simon w aplikacji HighGear powiązany z reprezentacją użytkownika usługi Azure AD. 
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD logowania jednokrotnego

Z tej sekcji dowiesz się, jak włączyć logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD w systemie HighGear, wykonaj następujące kroki:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **HighGear** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą języka SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja języka SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. W sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące czynności:

    ![Informacje o domenie i adresach URL pojedynczego logowania aplikacji HighGear](common/idp-intiated.png)

    a. W polu tekstowym **Identyfikator** wklej wartość **identyfikatora jednostki usługodawcy**, która znajduje się na stronie ustawień logowania jednokrotnego w systemie HighGear.

    ![Pole Identyfikator jednostki usługodawcy](media/highgear-tutorial/service-provider-entity-id-field.png)
    
    > [!NOTE]
    > Musisz zalogować się do systemu HighGear, aby uzyskać dostęp do strony ustawień logowania jednokrotnego. Po zalogowaniu przesuń wskaźnik myszy nad kartę administracji w aplikacji HighGear, a następnie kliknij element menu Ustawienia logowania jednokrotnego.
    
    ![Element menu Ustawienia logowania jednokrotnego](media/highgear-tutorial/single-sign-on-settings-menu-item.png)

    b. W polu tekstowym **Adres URL odpowiedzi** wklej wartość **Adres URL usługi Assertion Consumer Service (ACS)** ze strony ustawień logowania jednokrotnego w systemie HighGear.

    ![Pole adresu URL usługi Assertion Consumer Service (ACS)](media/highgear-tutorial/assertion-consumer-service-url-field.png)

    c. Kliknij przycisk **Ustaw dodatkowe adresy URL** i wykonaj następujący krok, jeśli chcesz skonfigurować aplikację w trybie inicjowania przez **dostawcę usług**:

     ![Informacje o domenie i adresach URL pojedynczego logowania aplikacji HighGear](common/metadata-upload-additional-signon.png)

     W polu tekstowym **Adres URL logowania** wklej wartość **identyfikatora jednostki usługodawcy**, która znajduje się na stronie ustawień logowania jednokrotnego w systemie HighGear. (Ten identyfikator jednostki jest również podstawowym adresem URL systemu HighGear, który ma być używany w przypadku logowania jednokrotnego zainicjowanego przez usługodawcę).

    ![Pole Identyfikator jednostki usługodawcy](media/highgear-tutorial/service-provider-entity-id-field.png)

    > [!NOTE]
    > Te wartości nie są prawdziwe. Zaktualizuj te wartości przy użyciu rzeczywistego identyfikatora, adresu URL odpowiedzi i adresu URL logowania jednokrotnego ze strony **Ustawienia logowania jednokrotnego** w systemie HighGear. Jeśli potrzebujesz pomocy, skontaktuj się z [zespołem pomocy technicznej aplikacji HighGear](mailto:support@highgear.com).

4. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **Certyfikat (Base64)** i zapisać go na komputerze. Będziesz go potrzebować w kolejnym kroku konfiguracji logowania jednokrotnego.

    ![Link pobierania certyfikatu](common/certificatebase64.png)

6. W sekcji **Konfigurowanie aplikacji HighGear** zanotuj lokalizację następujących adresów URL.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania. Ta wartość będzie potrzebna w kroku 2 w poniższej sekcji **Konfigurowanie logowania jednokrotnego w aplikacji HighGear**.

    b. Identyfikator usługi Azure AD. Ta wartość będzie potrzebna w kroku 3 w poniższej sekcji **Konfigurowanie logowania jednokrotnego w aplikacji HighGear**.

    c. Adres URL wylogowywania. Ta wartość będzie potrzebna w kroku 4 w poniższej sekcji **Konfigurowanie logowania jednokrotnego w aplikacji HighGear**.

### <a name="configure-highgear-single-sign-on"></a>Konfigurowanie logowania jednokrotnego w aplikacji HighGear

Aby skonfigurować aplikację HighGear na potrzeby logowania jednokrotnego, zaloguj się do systemu HighGear. Po zalogowaniu przesuń wskaźnik myszy nad kartę administracji w aplikacji HighGear, a następnie kliknij element menu Ustawienia logowania jednokrotnego.

![Element menu Ustawienia logowania jednokrotnego](media/highgear-tutorial/single-sign-on-settings-menu-item.png)

1. W obszarze **Nazwa dostawcy tożsamości** wpisz krótki opis, który będzie wyświetlany na przycisku logowania jednokrotnego w aplikacji HighGear na stronie logowania. Na przykład: Azure AD

2. W polu **Adres URL logowania jednokrotnego** aplikacji HighGear wklej wartość z pola **Adres URL logowania**, które znajduje się w sekcji **Konfigurowanie aplikacji HighGear** na platformie Azure.

3. W polu **Identyfikator jednostki usługodawcy** aplikacji HighGear wklej wartość z pola **Identyfikator usługi Azure AD**, które znajduje się w sekcji **Konfigurowanie aplikacji HighGear** na platformie Azure.

4. W polu **Adres URL wylogowania jednokrotnego** aplikacji HighGear wklej wartość z pola **Adres URL wylogowywania**, które znajduje się w sekcji **Konfigurowanie aplikacji HighGear** na platformie Azure.

5. W Notatniku otwórz certyfikat pobrany z sekcji **Certyfikat podpisywania SAML** na platformie Azure. Pobrany certyfikat powinien mieć format **Certyfikat (Base64)** . Skopiuj zawartość certyfikatu z Notatnika i wklej go w polu **Certyfikat dostawcy tożsamości** w aplikacji HighGear.

6. Wyślij wiadomość e-mail do [zespołu pomocy technicznej aplikacji HighGear](mailto:support@highgear.com), aby zażądać certyfikatu aplikacji HighGear. Postępuj zgodnie z instrukcjami otrzymanymi od tego zespołu, aby wypełnić pola **Certyfikat aplikacji HighGear** i **Hasło certyfikatu aplikacji HighGear**.

7. Kliknij przycisk **Zapisz**, aby zapisać konfigurację logowania jednokrotnego w aplikacji HighGear.

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

    c. Zaznacz pole wyboru **Pokaż hasło** i zanotuj wartość wyświetlaną w polu Hasło.

    d. Kliknij pozycję **Utwórz**.

### <a name="assign-the-azure-ad-test-user"></a>Przypisywanie użytkownika testowego usługi Azure AD

W tej sekcji włączysz dla użytkownika Britta Simon możliwość korzystania z logowania jednokrotnego platformy Azure, udzielając dostępu do aplikacji HighGear.

1. W witrynie Azure Portal wybierz pozycję **Aplikacje dla przedsiębiorstw**, wybierz pozycję **Wszystkie aplikacje**, a następnie wybierz pozycję **HighGear**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz pozycję **HighGear**.

    ![Link aplikacji HighGear na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-highgear-test-user"></a>Tworzenie użytkownika testowego aplikacji HighGear

Aby utworzyć użytkownika testowego aplikacji HighGear w celu przetestowania konfiguracji logowania jednokrotnego, zaloguj się do systemu HighGear.

1. Kliknij przycisk **Utwórz nowy kontakt**.

    ![Przycisk Utwórz nowy kontakt](media/highgear-tutorial/create-new-contact-button.png)

    Zostanie wyświetlone menu umożliwiające wybranie rodzaju tworzonego kontaktu.

2. Kliknij element menu **Osoba**, aby utworzyć aplikację użytkownika HighGear.

    Po prawej stronie zostanie wysunięte okienko, w którym można wpisać informacje dotyczące nowego użytkownika.  
    ![Formularz Nowy kontakt](media/highgear-tutorial/new-contact-form.png)

3. W polu **Nazwa** wpisz nazwę kontaktu. Na przykład: Britta Simon

4. Kliknij menu **Więcej opcji**, a następnie wybierz element menu **Informacje o koncie**.

    ![Klikanie elementu menu Informacje o koncie](media/highgear-tutorial/account-info-menu-item.png)

5. Ustaw pole **Można się zalogować** na wartość Tak.

    Pole **Włącz logowanie jednokrotne** zostanie również automatycznie ustawione na wartość Tak.

6. W polu **Identyfikator użytkownika logowania jednokrotnego** wpisz identyfikator użytkownika. Na przykład: BrittaSimon@contoso.com

    Sekcja informacji o koncie powinna teraz wyglądać mniej więcej tak:  
    ![Kompletna sekcja informacji o koncie](media/highgear-tutorial/finished-account-info-section.png)

7. Aby zapisać kontakt, kliknij przycisk **Zapisz** w dolnej części okienka.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka HighGear w panelu dostępu powinno nastąpić automatyczne zalogowanie do aplikacji HighGear, dla której skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

