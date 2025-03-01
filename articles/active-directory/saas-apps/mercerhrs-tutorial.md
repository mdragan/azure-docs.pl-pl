---
title: 'Samouczek: Integracja usługi Azure Active Directory za pomocą BenefitsCentral Mercer (MBC) | Dokumentacja firmy Microsoft'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługi Azure Active Directory i Mercer BenefitsCentral (MBC).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3788b28c-49aa-4208-9acd-630362008e89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/20/2019
ms.author: jeedes
ms.openlocfilehash: 9ae074e8732898524d917f2537023c4b506cff80
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67097471"
---
# <a name="tutorial-azure-active-directory-integration-with-mercer-benefitscentral-mbc"></a>Samouczek: Integracja usługi Azure Active Directory za pomocą BenefitsCentral Mercer (MBC)

W tym samouczku dowiesz się, jak zintegrować Mercer BenefitsCentral (MBC) w usłudze Azure Active Directory (Azure AD).
Integrowanie Mercer BenefitsCentral (MBC) z usługą Azure AD zapewnia następujące korzyści:

* Możesz kontrolować, czy w usłudze Azure AD, kto ma dostęp BenefitsCentral Mercer (MBC).
* Użytkownikom można automatycznie zalogowany do Mercer BenefitsCentral (MBC) (logowanie jednokrotne) można włączyć za pomocą kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD za pomocą BenefitsCentral Mercer (MBC), potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Mercer BenefitsCentral (MBC) logowanie jednokrotne włączone subskrypcji

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Obsługuje mercer BenefitsCentral (MBC) **tożsamości** jednokrotne logowanie inicjowane przez

## <a name="adding-mercer-benefitscentral-mbc-from-the-gallery"></a>Dodawanie Mercer BenefitsCentral (MBC) z galerii

Aby skonfigurować integrację BenefitsCentral Mercer (MBC) w usłudze Azure AD, należy dodać Mercer BenefitsCentral (MBC) z galerii z listą zarządzanych aplikacji SaaS.

**Aby dodać Mercer BenefitsCentral (MBC) z galerii, wykonaj następujące czynności:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **Mercer BenefitsCentral (MBC)** , wybierz opcję **Mercer BenefitsCentral (MBC)** z panelu wynik kliknięcie **Dodaj** przycisk, aby dodać aplikację.

     ![Mercer BenefitsCentral (MBC) na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji, konfigurowania i testowania usługi Azure AD logowanie jednokrotne za pomocą BenefitsCentral Mercer (MBC) w oparciu o nazwie użytkownika testowego **Britta Simon**.
Dla logowania jednokrotnego do pracy relację łącza między użytkownika usługi Azure AD i powiązanych użytkowników BenefitsCentral Mercer (MBC) musi zostać ustanowione.

Aby skonfigurować i testowanie usługi Azure AD logowanie jednokrotne za pomocą BenefitsCentral Mercer (MBC), należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie Mercer BenefitsCentral (MBC) logowanie jednokrotne](#configure-mercer-benefitscentral-mbc-single-sign-on)**  — Aby skonfigurować ustawienia logowania jednokrotnego na stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego Mercer BenefitsCentral (MBC)](#create-mercer-benefitscentral-mbc-test-user)**  — aby odpowiednikiem Britta Simon w Mercer BenefitsCentral (MBC) połączoną usługę Azure AD reprezentacja użytkownika.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować usługi Azure AD logowanie jednokrotne za pomocą BenefitsCentral Mercer (MBC), wykonaj następujące czynności:

1. W [witryny Azure portal](https://portal.azure.com/)na **Mercer BenefitsCentral (MBC)** strona integracji aplikacji, wybierz opcję **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** wykonaj następujące kroki:

    ![Mercer BenefitsCentral (MBC) domena i adresy URL pojedynczego logowania jednokrotnego informacji](common/idp-intiated.png)

    a. W polu tekstowym **Identyfikator** wpisz adres URL: `stg.mercerhrs.com/saml2.0`

    b. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca: `https://ssous-stg.mercerhrs.com/SP2/Saml2AssertionConsumer.aspx`

    > [!NOTE]
    > Wartość adresu URL odpowiedzi nie jest prawdziwa. Zaktualizuj ją, stosując rzeczywisty adres URL odpowiedzi. Skontaktuj się z pomocą [zespołem pomocy technicznej klienta Mercer BenefitsCentral (MBC)](https://www.mercer.com/contact-us.html) aby zyskać tę wartość. Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

5. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **kod XML metadanych federacji** na podstawie podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/metadataxml.png)

6. Na **Ustaw się BenefitsCentral Mercer (MBC)** sekcji, skopiuj odpowiednie adresy URL, zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    d. Adres URL wylogowywania

### <a name="configure-mercer-benefitscentral-mbc-single-sign-on"></a>Konfigurowanie Mercer BenefitsCentral (MBC) logowanie jednokrotne

Aby skonfigurować logowanie jednokrotne na **Mercer BenefitsCentral (MBC)** stronie, musisz wysłać pobrany **XML metadanych Federacji** i odpowiednie skopiowany adresy URL z portalu Azure w celu [Mercer Zespół pomocy technicznej BenefitsCentral (MBC)](https://www.mercer.com/contact-us.html). Ustawiają to ustawienie, aby były prawidłowo po obu stronach połączenia logowania jednokrotnego SAML.

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

W tej sekcji możesz włączyć Britta Simon do używania platformy Azure logowanie jednokrotne za udzielanie dostępu BenefitsCentral Mercer (MBC).

1. W witrynie Azure portal wybierz **aplikacje dla przedsiębiorstw**, wybierz opcję **wszystkie aplikacje**, a następnie wybierz **Mercer BenefitsCentral (MBC)** .

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz **Mercer BenefitsCentral (MBC)** .

    ![Łącze Mercer BenefitsCentral (MBC) na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-mercer-benefitscentral-mbc-test-user"></a>Tworzenie użytkownika testowego Mercer BenefitsCentral (MBC)

W tej sekcji utworzysz użytkownika o nazwie Britta Simon BenefitsCentral Mercer (MBC). Praca z [zespołem pomocy technicznej Mercer BenefitsCentral (MBC)](https://www.mercer.com/contact-us.html) Aby dodać użytkowników na platformie Mercer BenefitsCentral (MBC). Użytkownicy muszą być tworzone i aktywowana, aby używać logowania jednokrotnego.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka Mercer BenefitsCentral (MBC) w panelu dostępu, możesz powinna istnieć możliwość automatycznej rejestracji do Mercer BenefitsCentral (MBC) dla której skonfigurować logowanie Jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

