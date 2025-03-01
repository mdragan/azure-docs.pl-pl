---
title: Co to jest usługa Azure Active Directory? — Usługa azure Active Directory | Dokumentacja firmy Microsoft
description: Przegląd i informacje o pojęciach dotyczących usługi Azure Active Directory, w tym terminologii, jakie licencje są dostępne i listę funkcji skojarzone z łącza do dodatkowych informacji.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.topic: overview
ms.date: 05/08/2019
ms.author: lizross
ms.custom: it-pro, seodec18, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd4c0bbe4e4ef1cfbd4d9da92d6fcfac509f45ab
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67110419"
---
# <a name="what-is-azure-active-directory"></a>Co to jest usługa Azure Active Directory?

Azure Active Directory (Azure AD) jest oparta na chmurze tożsamość i dostęp do usługi zarządzania, która pomaga pracownikom Zaloguj się i uzyskać dostęp do zasobów firmy Microsoft:

- Zasoby zewnętrzne, takie jak usługa Microsoft Office 365, witryna Azure Portal i tysiące innych aplikacji SaaS.

- Zasoby wewnętrzne, takie jak aplikacje w sieci firmowej i intranecie, a także aplikacje w chmurze opracowane przez organizację.

Możesz skorzystać z różnych plakatów z [serii Platforma Microsoft Cloud dla architektów w przedsiębiorstwach](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources#identity), aby lepiej zrozumieć podstawowe usługi tożsamości na platformie Azure oraz w usługach Azure AD i Office 365.

## <a name="who-uses-azure-ad"></a>Kto używa usługi Azure AD?

Usługa Azure AD jest przeznaczona dla następujących użytkowników:

- **Administratorzy IT.** Jako administrator IT możesz używać usługi Azure AD, aby kontrolować dostęp do aplikacji i ich zasobów, zależnie od wymagań biznesowych. Możesz na przykład użyć usługi Azure AD, aby wymagać uwierzytelniania wieloskładnikowego podczas uzyskiwania dostępu do ważnych zasobów organizacji. Usługa Azure AD pozwala też zautomatyzować aprowizację użytkowników między istniejącą usługą AD systemu Windows Server i aplikacjami w chmurze, włącznie z usługą Office 365. Ponadto usługa Azure AD zapewnia zaawansowane narzędzia, które automatycznie ułatwiają ochronę tożsamości użytkowników i poświadczeń zgodnie z wymaganiami nadzoru nad dostępem. Aby rozpocząć, utwórz konto [bezpłatnej 30-dniowej wersji próbnej usługi Azure Active Directory w wersji Premium](https://azure.microsoft.com/trial/get-started-active-directory/).

- **Deweloperzy aplikacji.** Dla deweloperów aplikacji usługa Azure AD oferuje opartą na standardach metodę dodawania logowania jednokrotnego (SSO) do aplikacji, umożliwiając posługiwanie się istniejącymi poświadczeniami użytkowników. Usługa Azure AD znajdują się również interfejsy API, które mogą pomóc Ci wrażeń spersonalizowanych aplikacji przy użyciu istniejących danych organizacji. Aby rozpocząć, utwórz konto [bezpłatnej 30-dniowej wersji próbnej usługi Azure Active Directory w wersji Premium](https://azure.microsoft.com/trial/get-started-active-directory/). Aby uzyskać więcej informacji, możesz też przejść do strony [Usługa Azure Active Directory dla deweloperów](../develop/index.yml).

- **Subskrybenci produktów Microsoft 365, Office 365, Azure lub Dynamics CRM Online.** Jako subskrybent korzystasz już z usługi Azure AD. Każda dzierżawa produktów Microsoft 365, Office 365, Azure i Dynamics CRM Online staje się automatycznie dzierżawą usługi Azure AD. Możesz od razu zacząć zarządzać dostępem do zintegrowanych aplikacji w chmurze.

## <a name="what-are-the-azure-ad-licenses"></a>Co to są licencje usługi Azure AD?

Usługi biznesowe online firmy Microsoft, takie jak Office 365 lub Microsoft Azure, wymagają usługi Azure AD, aby obsługiwać logowanie i ułatwić ochronę tożsamości. Jeśli subskrybujesz do wszystkich usług firmy Microsoft Online, możesz automatycznie uzyskują usługi Azure AD, dzięki dostępowi do bezpłatnych funkcji.

Aby zwiększyć możliwości implementacji usługi Azure AD, możesz także dodać płatne funkcje, podnosząc poziom planu do licencji Podstawowa, Premium P1 lub Premium P2 usługi Azure Active Directory. Usługi Azure AD z płatnej licencji są tworzone na podstawie istniejącego katalogu bezpłatnej, zapewnienie samoobsługi, rozszerzone monitorowanie, raporty o zabezpieczeniach i bezpieczny dostęp dla użytkowników mobilnych.

>[!Note]
>Aby zapoznać się z cennikami tych licencji, zobacz [Azure Active Directory — cennik](https://azure.microsoft.com/pricing/details/active-directory/).
>
>Usługi Azure Active Directory Premium P1, Premium P2 — oraz Azure Active Directory — wersja Podstawowa nie są obecnie obsługiwane w Chinach. Aby uzyskać więcej informacji na temat cen usługi Azure AD, skontaktuj się z pomocą [Forum usługi Azure Active Directory](https://azure.microsoft.com/support/community/?product=active-directory).

- **Azure Active Directory — wersja Bezpłatna.** Oferuje zarządzanie użytkownikami i grupami, synchronizację katalogów lokalnych, podstawowe raporty oraz logowanie jednokrotne na platformie Azure, w usłudze Office 365 i w wielu popularnych aplikacjach SaaS.

- **Azure Active Directory — wersja Podstawowa.** Oprócz funkcji wersji Bezpłatna, wersja Podstawowa zapewnia też dostęp do aplikacji w chmurze, zarządzanie dostępem oparte na grupach, samoobsługowe resetowanie haseł dla aplikacji w chmurze oraz serwer proxy aplikacji usługi Azure AD, który umożliwia publikowanie aplikacji internetowych w środowisku lokalnym przy użyciu usługi Azure AD.

- **Azure Active Directory — wersja Premium P1.** Oprócz funkcji wersji Bezpłatna i Podstawowa, wersja P1 umożliwia też użytkownikom hybrydowym dostęp zarówno do zasobów lokalnych, jak i w chmurze. Obsługuje ona również zaawansowaną administrację, taką jak grupy dynamiczne, samoobsługowe zarządzanie grupami, usługa Microsoft Identity Manager (lokalny pakiet do zarządzania tożsamościami i dostępem) oraz funkcje zapisu zwrotnego, które umożliwiają samoobsługowe resetowanie haseł przez użytkowników lokalnych.

- **Azure Active Directory — wersja Premium P2.** Oprócz funkcji bezpłatna, podstawowa i P1, P2 oferuje [usługi Azure Active Directory Identity Protection](../identity-protection/enable.md) w celu uzyskania dostępu warunkowego do aplikacji i kluczowych danych firmy na podstawie ryzyka i [Privileged Identity Zarządzanie](../privileged-identity-management/pim-getting-started.md) ułatwiające odkrywanie, ograniczanie i monitorowanie administratorów oraz ich dostępu do zasobów i zapewnić dostęp just in time w razie.

- **Licencje funkcji „Płatność zgodnie z rzeczywistym użyciem”.** Istnieje też możliwość uzyskania licencji dodatkowych funkcji, takich jak usługa Azure Active Directory Business-to-Customer (B2C). Usługa B2C może ułatwić zapewnianie rozwiązań do zarządzania tożsamościami i dostępem dla aplikacji udostępnianych klientom. Aby uzyskać więcej informacji, zobacz [dokumentację usługi Azure Active Directory B2C](../../active-directory-b2c/index.yml).

Aby uzyskać więcej informacji o kojarzeniu subskrypcji platformy Azure z usługą Azure AD, zobacz [How to: Associate or add an Azure subscription to Azure Active Directory](active-directory-how-subscriptions-associated-directory.md) (Instrukcje: kojarzenie lub dodawanie subskrypcji platformy Azure do usługi Azure Active Directory). Aby uzyskać więcej informacji o przypisywaniu licencji do użytkowników, zobacz [How to: Assign or remove Azure Active Directory licenses](license-users-groups.md) (Instrukcje: przypisywanie lub usuwanie licencji usługi Azure Active Directory).

## <a name="terminology"></a>Terminologia

Aby lepiej poznać usługę Azure AD i jego dokumentacji firma Microsoft zalecamy zapoznanie się z następujących warunków.

|Termin lub pojęcie|Opis|
|---------------|-----------|
|Subskrypcja platformy Azure| Używana do płacenia za usługi platformy Azure w chmurze. Możesz mieć wiele subskrypcji. Są one połączone z kartą kredytową.|
|Dzierżawa platformy Azure| Dedykowane, zaufane wystąpienie usługi Azure AD, które jest automatycznie tworzone, gdy organizacja rejestruje się w subskrypcji usługi w chmurze firmy Microsoft, takiej jak Microsoft Azure, Microsoft Intune lub Office 365. Dzierżawa usługi Azure reprezentuje jedną organizację.|
|Pojedyncza dzierżawa| Dzierżawy platformy Azure, które uzyskują dostęp do innych usług w dedykowanym środowisku, są uznawane za jedną dzierżawę.|
|Wiele dzierżaw| Dzierżawy platformy Azure, które uzyskują dostęp do innych usług w środowisku współużytkowanym przez wiele organizacji, są uznawane za wiele dzierżaw.|
|Katalog usługi Azure AD|Każda dzierżawa platformy Azure ma dedykowany, zaufany katalog usługi Azure AD. Katalog usługi Azure AD zawiera użytkowników, grupy i aplikacje dzierżawy oraz jest używany do obsługi funkcji zarządzania tożsamościami i dostępem dla zasobów dzierżawy.|
|Konto Azure AD | Tożsamość utworzona za pośrednictwem usługi Azure AD lub innej usługi firmy Microsoft w chmurze, takiej jak Office 365. Tożsamości są przechowywane w usłudze Azure AD i dostępne dla subskrypcji usług w chmurze organizacji. To konto jest też czasami nazywane kontem służbowym.|
|Domena niestandardowa|Każdy nowy katalog usługi Azure AD jest dostarczany z początkową nazwą domeny nazwa_domeny.onmicrosoft.com. Oprócz początkowej nazwy, do listy można dodać nazwy domen organizacji, włącznie z nazwami używanymi do prowadzenia działalności firmy i uzyskiwania dostępu do zasobów organizacji przez użytkowników. Dodanie niestandardowych nazw domen ułatwia tworzenie nazw użytkowników takich jak alain@contoso.com, które są znajome dla użytkowników.|
|Administrator konta|Ta klasyczna rola administratora subskrypcji określa właściciela subskrypcji odpowiedzialnego za rozliczenia. Ta rola ma dostęp do [Centrum konta platformy Azure](https://account.azure.com/Subscriptions) i pozwala zarządzać wszystkimi subskrypcjami na koncie. Aby uzyskać więcej informacji, zobacz [ról administratora subskrypcji klasyczne, ról kontroli dostępu opartej na rolach platformy Azure i role administratora usługi Azure AD](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Administrator usługi|Ta klasyczna rola administratora subskrypcji umożliwia zarządzanie wszystkimi zasobami platformy Azure, włącznie z dostępem. Ta rola ma takie same uprawnienia dostępu co użytkownik, któremu przypisano rolę właściciela w zakresie subskrypcji. Aby uzyskać więcej informacji, zobacz [Role klasycznego administratora subskrypcji, role kontroli na podstawie ról (RBAC) platformy Azure i role administratora usługi Azure AD](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Właściciel|Ta rola ułatwia zarządzanie wszystkimi zasobami platformy Azure, włącznie z dostępem. Ta rola bazuje na nowszym systemie autoryzacji, nazywanym kontrolą dostępu opartą na rolach (RBAC), który umożliwia szczegółowe zarządzanie dostępem do zasobów platformy Azure. Aby uzyskać więcej informacji, zobacz [Role klasycznego administratora subskrypcji, role kontroli na podstawie ról (RBAC) platformy Azure i role administratora usługi Azure AD](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Administrator globalny usługi Azure AD|Ta rola administratora jest automatycznie przypisywana osobie, która utworzyła dzierżawę usługi Azure AD. Administratorzy globalni tworzyć wszystkich funkcji administracyjnych w usłudze Azure AD i wszystkich usług, które Federację z usługą Azure AD, takie jak Exchange Online, SharePoint Online i Skype dla firm Online. Może istnieć wielu administratorów globalnych, ale tylko administratorzy globalni mogą przypisywać użytkownikom role administratora (włącznie z przypisywaniem innych administratorów globalnych).<br><br>**Uwaga**<br>Ta rola administratora jest nazywana administratorem globalnym w witrynie Azure Portal, ale ma nazwę **administrator firmy** w interfejsie API programu Microsoft Graph, interfejsie API programu Graph w usłudze Azure AD oraz w usłudze Azure AD PowerShell.<br><br>Aby uzyskać więcej informacji o różnych rolach administratorów, zobacz [Administrator role permissions in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md) (Uprawnienia ról administratorów w usłudze Azure Active Directory).|
|Konto Microsoft (nazywane też kontem MSA)|Konta osobiste, które zapewniają dostęp do produktów i usług w chmurze firmy Microsoft przeznaczonych dla konsumentów, takich jak Outlook, OneDrive, Xbox LIVE i Office 365. Konto Microsoft jest tworzone i przechowywane w systemie kont tożsamości konsumentów firmy Microsoft.|

## <a name="which-features-work-in-azure-ad"></a>Które funkcje działają w usłudze Azure AD?

Po wybraniu licencja usługi Azure AD, będziesz mieć dostęp do niektórych lub wszystkich następujące funkcje dla Twojej organizacji:

|Category|Opis|
|-------|-----------|
|Zarządzanie aplikacjami|Zarządzanie aplikacjami w chmurze i lokalnymi przy użyciu serwera proxy aplikacji, logowania jednokrotnego, portalu Moje aplikacje (nazywanego też panelem dostępu) i aplikacji typu oprogramowanie jako usługa (SaaS). Aby uzyskać więcej informacji, zobacz temat [Jak zapewnić bezpieczny, zdalny dostęp do aplikacji lokalnych](../manage-apps/application-proxy.md) i [dokumentację zarządzania aplikacjami](../manage-apps/index.yml).|
|Authentication|Zarządzanie samoobsługowym resetowaniem haseł, uwierzytelnianiem wieloskładnikowym, niestandardowymi listami zakazanych haseł i inteligentną blokadą usługi Azure Active Directory. Aby uzyskać więcej informacji, zobacz [dokumentację uwierzytelniania w usłudze Azure AD](../authentication/index.yml).|
|Działania między firmami (B2B)|Zarządzanie użytkownikami-gośćmi i partnerami zewnętrznymi przy zachowaniu kontroli nad danymi firmowymi. Aby uzyskać więcej informacji, zobacz [dokumentację usługi Azure Active Directory B2B](../b2b/index.yml).|
|Działania między firmami i klientami (B2C)|Dostosowywanie i kontrolowanie sposobu tworzenia kont, logowania się i zarządzania profilami przez użytkowników podczas korzystania z aplikacji. Aby uzyskać więcej informacji, zobacz [dokumentację usługi Azure Active Directory B2C](../../active-directory-b2c/index.yml).|
|Dostęp warunkowy|Zarządzanie dostępem do aplikacji w chmurze. Aby uzyskać więcej informacji, zobacz [dokumentację dostępu warunkowego w usłudze Azure AD](../conditional-access/index.yml).|
|Usługa Azure Active Directory dla deweloperów|Tworzenie aplikacji, które obsługują logowanie za pomocą wszystkich tożsamości firmy Microsoft oraz uzyskują tokeny w celu wywoływania programu Microsoft Graph, innych interfejsów API firmy Microsoft lub niestandardowych interfejsów API. Aby uzyskać więcej informacji, zobacz [Platforma tożsamości firmy Microsoft (Azure Active Directory dla deweloperów)](../develop/index.yml).|
|Zarządzanie urządzeniami|Zarządzanie sposobem uzyskiwania dostępu do danych firmowych przez urządzenia w chmurze lub lokalne. Aby uzyskać więcej informacji, zobacz [dokumentację zarządzania urządzeniami w usłudze Azure AD](../devices/index.yml).|
|Usługi domenowe|Przyłączanie maszyn wirtualnych platformy Azure do domeny bez używania kontrolerów domeny. Aby uzyskać więcej informacji, zobacz [dokumentację usług Azure AD Domain Services](../../active-directory-domain-services/index.yml).|
|Użytkownicy korporacyjni|Zarządzanie przypisywaniem licencji i dostępem do aplikacji oraz konfigurowanie delegatów za pomocą grup i ról administratora. Aby uzyskać więcej informacji, zobacz [dokumentację zarządzania użytkownikami usługi Azure Active Directory](../users-groups-roles/index.yml).|
|Tożsamość hybrydowa|Używanie programów Azure Active Directory Connect i Connect Health w celu udostępniania pojedynczej tożsamości użytkownika na potrzeby uwierzytelniania i autoryzacji we wszystkich zasobach, niezależnie od lokalizacji (w chmurze lub lokalnie). Aby uzyskać więcej informacji, zobacz [dokumentację tożsamości hybrydowej](../hybrid/index.yml).|
|Nadzór nad tożsamościami|Zarządzanie tożsamością w organizacji za pośrednictwem mechanizmów kontroli dostępu pracowników, partnerów biznesowych, dostawców, usług i aplikacji. Można też przeprowadzać przeglądy dostępu. Aby uzyskać więcej informacji, zobacz [dokumentację zarządzania tożsamościami w usłudze Azure AD](../governance/identity-governance-overview.md) i temat [Przeglądy dostępu w usłudze Azure AD](../governance/access-reviews-overview.md).|
|Ochrona tożsamości|Wykrywanie potencjalnych luk w zabezpieczeniach wpływających na tożsamości w organizacji, konfigurowanie zasad w celu reagowania na podejrzane działania oraz podejmowanie odpowiednich działań w celu rozwiązywania problemów. Aby uzyskać więcej informacji, zobacz [Usługa Azure AD Identity Protection](../identity-protection/index.yml).|
|Tożsamości zarządzane dla zasobów platformy Azure|Zapewnia usługom platformy Azure automatycznie zarządzaną tożsamość w usłudze Azure AD, która umożliwia uwierzytelnianie dowolnych usług uwierzytelniania obsługiwanych przez usługę Azure AD, włącznie z usługą Key Vault. Aby uzyskać więcej informacji, zobacz [Czym są tożsamości zarządzane dla zasobów platformy Azure?](../managed-identities-azure-resources/overview.md).|
|Privileged Identity Management (PIM)|Kontrolowanie i monitorowanie dostępu w organizacji oraz zarządzanie nim. Ta funkcja obejmuje dostęp do zasobów w usłudze Azure AD i platformy Azure i innych Microsoft Online Services, takich jak usługi Office 365 lub Intune. Aby uzyskać więcej informacji, zobacz [Usługa Azure AD Privileged Identity Management](../privileged-identity-management/index.yml).|
|Raporty i monitorowanie|Uzyskiwanie szczegółowych informacji dotyczących zabezpieczeń i wzorców użycia w danym środowisku. Aby uzyskać więcej informacji, zobacz [Raporty i monitorowanie w usłudze Azure Active Directory](../reports-monitoring/index.yml).|

## <a name="next-steps"></a>Kolejne kroki

- [Rejestrowanie w usłudze Azure Active Directory — wersja Premium](active-directory-get-started-premium.md)

- [Kojarzenie subskrypcji platformy Azure z usługą Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- [Uzyskiwanie dostępu do usługi Azure Active Directory i tworzenie nowej dzierżawy](active-directory-access-create-new-tenant.md)

- [Lista kontrolna dotycząca wdrażania funkcji usługi Azure Active Directory Premium P2](active-directory-deployment-checklist-p2.md)
