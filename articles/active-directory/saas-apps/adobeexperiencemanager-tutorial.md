---
title: 'Samouczek: integracja usługi Azure Active Directory z programem Adobe Experience Manager | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory i programem Adobe Experience Manager.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 88a95bb5-c17c-474f-bb92-1f80f5344b5a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81032fbad21b18b0b7ca2e7662b0c4b4b6c10901
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107285"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Samouczek: integracja usługi Azure Active Directory z programem Adobe Experience Manager

Z tego samouczka dowiesz się, jak zintegrować program Adobe Experience Manager z usługą Azure Active Directory (Azure AD).
Integrowanie programu Adobe Experience Manager z usługą Azure AD zapewnia następujące korzyści:

* W usłudze Azure AD możesz kontrolować, kto ma dostęp do programu Adobe Experience Manager.
* Twoi użytkownicy mogą być automatycznie logowani do programu Adobe Experience Manager (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD.
* Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Do skonfigurowania integracji usługi Azure AD z programem Adobe Experience Manager potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja programu Adobe Experience Manager z obsługą logowania jednokrotnego

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Program Adobe Experience Manager obsługuje jednokrotne logowanie inicjowane przez **dostawcę usług i dostawcę tożsamości**

* Adobe Experience Manager obsługuje aprowizację użytkowników typu **Just In Time**

## <a name="adding-adobe-experience-manager-from-the-gallery"></a>Dodawanie programu Adobe Experience Manager z galerii

Aby skonfigurować integrację programu Adobe Experience Manager z usługą Azure AD, musisz dodać program Adobe Experience Manager z galerii do swojej listy zarządzanych aplikacji SaaS.

**Aby dodać program Adobe Experience Manager z galerii, wykonaj następujące kroki:**

1. W **[witryny Azure portal](https://portal.azure.com)** , w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji](common/add-new-app.png)

4. W polu wyszukiwania wpisz **Adobe Experience Manager**, z panelu wyników wybierz pozycję **Adobe Experience Manager**, a następnie kliknij przycisk **Dodaj**, aby dodać aplikację.

     ![Program Adobe Experience Manager na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD logowania jednokrotnego

W tej sekcji skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD z aplikacją [Application name] w oparciu o użytkownika testowego o nazwie **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację łącza między użytkownikiem usługi Azure AD i powiązanym użytkownikiem aplikacji [Application name].

Aby skonfigurować i przetestować logowanie jednokrotne usługi Azure AD z aplikacją [Application name], należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie logowania jednokrotnego w programie Adobe Experience Manager](#configure-adobe-experience-manager-single-sign-on)** — aby skonfigurować ustawienia logowania jednokrotnego po stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Tworzenie użytkownika testowego programu Adobe Experience Manager](#create-adobe-experience-manager-test-user)** — aby udostępnić w programie Adobe Experience Manager odpowiednik użytkownika Britta Simon połączony z reprezentacją użytkownika w usłudze Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD z aplikacją [Application name], wykonaj następujące czynności:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **Adobe Experience Manager** wybierz pozycję **Logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. Jeśli chcesz skonfigurować aplikację w trybie inicjowanym przez **dostawcę tożsamości**, w sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące kroki:

    ![Informacje o domenie i adresach URL logowania jednokrotnego programu Adobe Experience Manager](common/idp-intiated.png)

    a. W polu tekstowym **Identyfikator** wpisz unikatową wartość, która została również zdefiniowana na Twoim serwerze AEM.

    b. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca: `https://<AEM Server Url>/saml_login`

    > [!NOTE]
    > Wartość adresu URL odpowiedzi nie jest prawdziwa. Zaktualizuj wartość adresu URL odpowiedzi przy użyciu rzeczywistego adresu URL odpowiedzi. Aby uzyskać tę wartość, skontaktuj się z [zespołem pomocy technicznej dla klienta programu Adobe Experience Manager](https://helpx.adobe.com/support/experience-manager.html). Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

5. Kliknij przycisk **Ustaw dodatkowe adresy URL** i wykonaj następujący krok, jeśli chcesz skonfigurować aplikację w trybie inicjowania przez **dostawcę usług**:

    ![Informacje o domenie i adresach URL logowania jednokrotnego programu Adobe Experience Manager](common/metadata-upload-additional-signon.png)

    W polu tekstowym **Adres URL logowania** wpisz adres URL swojego serwera programu Adobe Experience Manager.

6. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **certyfikat (Base64)** z podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link pobierania certyfikatu](common/certificatebase64.png)

7. W sekcji **Konfiguracja programu Adobe Experience Manager** skopiuj odpowiednie adresy URL zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    d. Adres URL wylogowywania

### <a name="configure-adobe-experience-manager-single-sign-on"></a>Konfigurowanie logowania jednokrotnego w programie Adobe Experience Manager

1. W innym oknie przeglądarki otwórz portal administracyjny programu **Adobe Experience Manager**.

2. Wybierz pozycje **Ustawienia** > **Zabezpieczenia** > **Użytkownicy**.

    ![Konfigurowanie przycisku Zapisz logowania jednokrotnego](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

3. Wybierz **administratora** lub dowolnego innego odpowiedniego użytkownika.

    ![Konfigurowanie przycisku Zapisz logowania jednokrotnego](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

4. Wybierz pozycje **Ustawienia konta** > **Zarządzanie magazynem TrustStore**.

    ![Konfigurowanie przycisku Zapisz logowania jednokrotnego](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

5. W obszarze **Dodaj certyfikat z pliku CER** kliknij pozycję **Wybierz plik certyfikatu**. Wyszukaj i wybierz plik certyfikatu, który został już pobrany z witryny Azure Portal.

    ![Konfigurowanie przycisku Zapisz logowania jednokrotnego](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

6. Certyfikat jest dodawany do magazynu TrustStore. Zapisz alias certyfikatu.

    ![Konfigurowanie przycisku Zapisz logowania jednokrotnego](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

7. Na stronie **Użytkownicy** wybierz pozycję **authentication-service**.

    ![Konfigurowanie przycisku Zapisz logowania jednokrotnego](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

8. Wybierz pozycję **Ustawienia konta** > **Tworzenie/zarządzanie magazynem kluczy**. Utwórz magazyn kluczy, podając hasło.

    ![Konfigurowanie przycisku Zapisz logowania jednokrotnego](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

9. Wróć do ekranu administratora. Następnie wybierz pozycję **Ustawienia** > **Operacje** > **Konsola internetowa**.

    ![Konfigurowanie przycisku Zapisz logowania jednokrotnego](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

    Spowoduje to otwarcie strony konfiguracji.

    ![Konfigurowanie przycisku Zapisz logowania jednokrotnego](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

10. Znajdź pozycję **Program obsługi uwierzytelniania Adobe Granite SAML 2.0**. Następnie wybierz ikonę **Dodaj**.

    ![Konfigurowanie przycisku Zapisz logowania jednokrotnego](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

11. Na tej stronie wykonaj następujące czynności.

    ![Konfigurowanie przycisku Zapisz logowania jednokrotnego](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. W polu **Ścieżka** wprowadź **/** .

    b. W polu **Adres URL dostawcy tożsamości** wprowadź wartość **adresu URL logowania** skopiowaną z witryny Azure Portal.

    c. W polu **Alias certyfikatu dostawcy tożsamości** wprowadź wartość **aliasu certyfikatu**, która została dodana w magazynie TrustStore.

    d. W polu **Identyfikator jednostki podanej w zabezpieczeniach** wprowadź wartość unikatowego **identyfikatora usługi Azure Ad**, która została skonfigurowana w witrynie Azure Portal.

    e. W polu **Adres URL usługi Assertion Consumer Service** wprowadź wartość **adresu URL odpowiedzi**, która została skonfigurowana w witrynie Azure Portal.

    f. W polu **Hasło magazynu kluczy** wprowadź **hasło** ustawione w magazynie kluczy.

    g. W polu **Identyfikator atrybutu użytkownika** wprowadź **identyfikator nazwy** lub inny identyfikator użytkownika, który jest odpowiedni w Twoim przypadku.

    h. Wybierz pozycję **Automatycznie utwórz użytkowników CRX**.

    i. W polu **Adres URL wylogowywania** wprowadź wartość unikatowego **adresu URL wylogowywania**, która została uzyskana z witryny Azure Portal.

    j. Wybierz pozycję **Zapisz**.

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

W tej sekcji włączysz dla użytkownika Britta Simon możliwość korzystania z logowania jednokrotnego platformy Azure, udzielając dostępu do programu Adobe Experience Manager.

1. W witrynie Azure Portal wybierz pozycję **Aplikacje dla przedsiębiorstw**, wybierz pozycję **Wszystkie aplikacje**, a następnie wybierz pozycję **Adobe Experience Manager**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz pozycję **Adobe Experience Manager**.

    ![Link programu Adobe Experience Manager na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz wartości roli w asercji SAML, w oknie dialogowym **Wybieranie roli** wybierz z listy odpowiednią rolę dla użytkownika, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-adobe-experience-manager-test-user"></a>Tworzenie użytkownika testowego programu Adobe Experience Manager

W tej sekcji utworzysz w programie Adobe Experience Manager użytkownika o nazwie Britta Simon. W przypadku wybrania opcji **Automatycznie utwórz użytkowników CRX** użytkownicy są tworzeni automatycznie po pomyślnym uwierzytelnieniu.

Jeśli chcesz ręcznie tworzyć użytkowników, nawiąż współpracę z [zespołem pomocy technicznej programu Adobe Experience Manager](https://helpx.adobe.com/support/experience-manager.html) , aby dodać użytkowników na platformie programu Adobe Experience Manager.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka Adobe Experience Manager w panelu dostępu powinno nastąpić automatyczne zalogowanie do programu Adobe Experience Manager, dla którego skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Wprowadzenie do panelu dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
