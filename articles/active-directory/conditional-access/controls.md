---
title: Co to są kontroli dostępu w usłudze Azure Active Directory dostępu warunkowego? | Microsoft Docs
description: Dowiedz się, jak kontrolę dostępu w usłudze Azure Active Directory dostępu warunkowego pracy.
services: active-directory
keywords: Dostęp warunkowy do aplikacji, dostęp warunkowy w usłudze Azure AD, bezpieczny dostęp do zasobów firmy, zasady dostępu warunkowego
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/15/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 206b5fef3e4e686bd237fe0f45cfb91dccd4626f
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67136507"
---
# <a name="what-are-access-controls-in-azure-active-directory-conditional-access"></a>Co to są kontroli dostępu w usłudze Azure Active Directory dostępu warunkowego?

Za pomocą [dostępu warunkowego usługi Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md), jak autoryzowanego dostępu użytkowników można kontrolować swoje aplikacje w chmurze. W zasadach dostępu warunkowego zdefiniujesz odpowiedzi ("to") można Przyczyna służącą do wyzwalania zasad ("w takim przypadku").

![Formant](./media/controls/10.png)

W kontekście dostępu warunkowego

- "**w takim przypadku**" nosi nazwę **warunków**

- "**To zrobić**" nosi nazwę **kontrole dostępu**

Kombinacja instrukcja warunku, za pomocą formantów reprezentuje zasady dostępu warunkowego.

![Formant](./media/controls/61.png)

Każdy formant jest wymaganie, które muszą zostać spełnione przez osobę lub logowania systemu lub ograniczenia na to, jak użytkownik można zrobić po zarejestrowaniu się.

Istnieją dwa typy formantów:

- **Kontrolki udzielania** — dostęp do bramy

- **Kontrolki sesji** — w celu ograniczenia dostępu w ramach sesji

W tym temacie wyjaśniono różne formanty, które są dostępne w funkcji dostępu warunkowego usługi Azure AD. 

## <a name="grant-controls"></a>Udziel kontroli

Za pomocą Udziel kontroli możesz całkowicie blokowanie dostępu lub zezwolić na dostęp z dodatkowymi wymaganiami, wybierając odpowiednią kontrolki. W przypadku wielu kontrolek możesz wymagać od:

- Wszystkie wybrane formanty, które należy spełnić (*i*)
- Jeden zaznaczony formant do spełnienia (*lub*)

![Formant](./media/controls/18.png)

### <a name="multi-factor-authentication"></a>Uwierzytelnianie wieloskładnikowe

Możesz użyć tego formantu, aby wymagać uwierzytelniania wieloskładnikowego, aby uzyskać dostęp do aplikacji określonej chmury. Ten formant obsługuje następujących dostawców usługi Multi-Factor Authentication:

- Azure Multi-Factor Authentication

- Dostawcę usługi Multi-Factor authentication w środowisku lokalnym, łączyć się z usługi Active Directory Federation Services (AD FS).

Przy użyciu usługi uwierzytelnianie wieloskładnikowe pomaga chronić zasoby przed dostępem nieautoryzowany użytkownik, który może mieć uzyskał dostęp do podstawowych poświadczeń prawidłowego użytkownika.

### <a name="compliant-device"></a>Zgodne urządzenie

Można skonfigurować zasady dostępu warunkowego, które są oparte na urządzeniu. Celem zasad dostępu warunkowego opartego na urządzeniach, które jest tylko udzielić dostępu do aplikacji w wybranej chmurze z [urządzeń zarządzanych przy użyciu](require-managed-devices.md). Wymaganie urządzenie było oznaczone jako zgodne jest jedną z opcji należy ograniczyć dostęp do zarządzanych urządzeń. Urządzenie może zostać oznaczony jako zgodne przez usługę Intune (dla dowolnego systemu operacyjnego urządzenia) lub przez system MDM innej firmy dla urządzeń z systemem Windows 10. Systemy zarządzania urządzeniami Przenośnymi innej firmy dla systemu operacyjnego urządzenia typów innych niż Windows 10 nie są obsługiwane. 

Urządzenie musi być zarejestrowany w usłudze Azure AD przed mogą zostać oznaczone jako zgodne. Aby zarejestrować urządzenie, masz trzy opcje: 

- [Urządzeń zarejestrowanych w usłudze Azure AD](../devices/overview.md#azure-ad-registered-devices)
- [Urządzenia przyłączone do usługi Azure AD](../devices/overview.md#azure-ad-joined-devices)  
- [Urządzenia przyłączone do hybrydowej usługi Azure AD](../devices/overview.md#hybrid-azure-ad-joined-devices)

Aby uzyskać więcej informacji, zobacz [sposób wymagać zarządzanych urządzeń w celu uzyskania dostępu do aplikacji w chmurze przy użyciu dostępu warunkowego](require-managed-devices.md).

### <a name="hybrid-azure-ad-joined-device"></a>Urządzenia przyłączone do hybrydowej usługi Azure AD

Wymaganie hybrydowych dołączonym do urządzenia w usłudze Azure AD inną opcję, należy skonfigurować zasady dostępu warunkowego opartego na urządzeniach. To wymaganie dotyczy komputerów stacjonarnych, laptopów i tabletów przedsiębiorstwa, które są przyłączone do lokalnej usługi Active Directory Windows. Jeśli ta opcja jest zaznaczona, zasady dostępu warunkowego przyznaje dostęp do podjętych z urządzeniami, które są przyłączone do lokalnej usługi Active Directory i usługi Azure Active Directory.  

Aby uzyskać więcej informacji, zobacz [skonfigurować zasady dostępu warunkowego opartego na urządzeniach w usłudze Azure Active Directory](require-managed-devices.md).

### <a name="approved-client-app"></a>Zatwierdzonej aplikacji klienckiej

Pracownicy używają urządzeń przenośnych na potrzeby osobiste i zadania, możesz chcieć mieć możliwość ochrony danych firmowych, uzyskiwać dostęp za pomocą urządzeń, nawet w przypadku, gdy nie są zarządzane przez użytkownika.
Możesz użyć [zasady ochrony aplikacji usługi Intune](https://docs.microsoft.com/intune/app-protection-policy) w celu ochrony danych firmy jest niezależna od wszelkich rozwiązań do zarządzania urządzeniami przenośnymi (MDM).

Przy użyciu zatwierdzonych aplikacji klienckich, możesz wymagać od aplikacji klienckiej, która próbuje uzyskać dostęp do obsługi aplikacji w chmurze [zasady ochrony aplikacji usługi Intune](https://docs.microsoft.com/intune/app-protection-policy). Na przykład można ograniczyć dostęp do usługi Exchange Online do aplikacji Outlook. Zasady dostępu warunkowego, które wymagają zatwierdzonych aplikacji klienckich jest także znana jako [zasad dostępu warunkowego opartego na aplikacji](app-based-conditional-access.md). Aby uzyskać listę obsługiwanych zatwierdzonych aplikacji klienckich, zobacz [zatwierdzone klienta aplikacji wymaganie](technical-reference.md#approved-client-app-requirement).

### <a name="app-protection-policy-preview"></a>Zasady ochrony aplikacji (wersja zapoznawcza)

Pracownicy używają urządzeń przenośnych na potrzeby osobiste i zadania, możesz chcieć mieć możliwość ochrony danych firmowych, uzyskiwać dostęp za pomocą urządzeń, nawet w przypadku, gdy nie są zarządzane przez użytkownika.
Możesz użyć [zasady ochrony aplikacji usługi Intune](https://docs.microsoft.com/intune/app-protection-policy) w celu ochrony danych firmy jest niezależna od wszelkich rozwiązań do zarządzania urządzeniami przenośnymi (MDM).

Za pomocą zasad ochrony aplikacji, można ograniczyć dostęp do aplikacji klienckich, które zostały zgłoszone do usługi Azure AD ma otrzymały [zasady ochrony aplikacji usługi Intune](https://docs.microsoft.com/intune/app-protection-policy). Na przykład można ograniczyć dostęp do usługi Exchange Online do aplikacji Outlook, która zawiera zasady ochrony aplikacji usługi Intune. Zasady dostępu warunkowego, które wymagają zasad ochrony aplikacji jest również nazywany [zasad dostępu warunkowego oparte na ochronę aplikacji](app-protection-based-conditional-access.md). 

Urządzenie musi być zarejestrowana do usługi Azure AD, zanim aplikacja może być oznaczony jako chroniony zasad.

Aby uzyskać listę zasad obsługiwanych klientów chronionych, zobacz [wymaganie dotyczące zasad ochrony aplikacji](technical-reference.md#app-protection-policy-requirement).


### <a name="terms-of-use"></a>Warunki użytkowania

Użytkownik może wymagać w dzierżawie, aby wyrazić zgodę na warunki użytkowania, zanim zostanie im przyznany dostęp do zasobu. Jako administrator można skonfigurować i dostosować warunki użytkowania, przekazywania dokumentów PDF. Jeśli użytkownik znajduje się w zakresie Ta kontrola dostępu do aplikacji, otrzymuje tylko jeśli uzgodniono warunki użytkowania.

### <a name="custom-controls-preview"></a>Formanty niestandardowe (wersja zapoznawcza)

Można dodać niestandardowe formanty dostępu warunkowego, które przekierować użytkowników do usługi zgodne w celu dalszego spełniają wymagania poza usługą Azure Active Directory. Dzięki temu można używać niektórych zewnętrznego uwierzytelniania wieloskładnikowego i dostawców weryfikacji do wymuszania wymagań dotyczących dostępu warunkowego. W celu spełnienia tego formantu, przeglądarki użytkownika jest przekierowywany do zewnętrznej usługi, wykonuje wszelkie wymagane uwierzytelnianie lub sprawdzania poprawności działania i jest następnie przekierowywane do usługi Azure Active Directory. Jeśli użytkownik został pomyślnie uwierzytelniony ani zweryfikowany, użytkownik będzie nadal w przepływ dostępu warunkowego. 

## <a name="custom-controls"></a>Formanty niestandardowe

Kontrolki niestandardowe są możliwości wersji usługi Azure Active Directory Premium P1. Używając niestandardowych formantów, użytkownicy są przekierowywane do zgodnych usługi w celu dalszego spełniają wymagania poza usługą Azure Active Directory. W celu spełnienia tego formantu, przeglądarki użytkownika jest przekierowywany do zewnętrznej usługi, wykonuje wszelkie wymagane uwierzytelnianie lub sprawdzania poprawności działania i jest następnie przekierowywane do usługi Azure Active Directory. Usługa Azure Active Directory weryfikuje odpowiedzi i, jeśli użytkownik został pomyślnie uwierzytelniony ani zweryfikowany, użytkownik będzie kontynuowana w przepływ dostępu warunkowego.

Te kontrolki Zezwalaj na korzystanie z niektórych usług zewnętrznych lub niestandardowe jako kontroli dostępu warunkowego, a rozszerzenie możliwości dostępu warunkowego.

Obejmują obecnie oferującymi usługę zgodnych dostawców:

- [Rozwiązanie Duo umożliwia zabezpieczeń](https://duo.com/docs/azure-ca)
- [Datacard Entrust](https://www.entrustdatacard.com/products/authentication/intellitrust)
- [GSMA](https://mobileconnect.io/azure/)
- [Ping Identity](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_AzureADIntegration.html)
- RSA
- [SecureAuth](https://docs.secureauth.com/pages/viewpage.action?pageId=47238992#)
- [Silverfort](https://www.silverfort.io/company/using-silverfort-mfa-with-azure-active-directory/)
- [Symantec VIP](https://help.symantec.com/home/VIP_Integrate_with_Azure_AD)
- [(Firmy Gemalto) firmy Thales](https://resources.eu.safenetid.com/help/AzureMFA/Azure_Help/Index.htm)
- [Trusona](https://www.trusona.com/docs/azure-ad-integration-guide)

Aby uzyskać więcej informacji na temat tych usług skontaktuj się bezpośrednio z dostawców.

### <a name="creating-custom-controls"></a>Tworzenie niestandardowych formantów

Aby utworzyć formant niestandardowy, najpierw skontaktuj się dostawcą, który chcesz wykorzystać. Każdy dostawca firm innych niż Microsoft ma swój własny proces i wymagania dotyczące Zarejestruj, subskrybowanie lub w przeciwnym razie stają się częścią usługi i wskazują, że chcesz zintegrować z dostępem warunkowym. W tym momencie dostawcę udostępni bloku danych w formacie JSON. Te dane umożliwia dostawcy i dostępu warunkowego współpracują ze sobą w dzierżawie, tworzy nowy formant i definiuje, jak można sprawdzić dostępu warunkowego w przypadku użytkowników zostały wykonane pomyślnie weryfikacji za pomocą dostawcy.

Nie można użyć niestandardowych kontrolek, za pomocą usługi Identity Protection automation wymaganie uwierzytelniania wieloskładnikowego lub podniesienie poziomu ról w Privileged Identity Manager (PIM).

Kopiowanie danych JSON, a następnie wklej go w polu tekstowym powiązane. Nie należy wprowadzać żadnych zmian za pomocą pliku JSON, chyba że jawnie zrozumieć zmiany, które wprowadzasz. Wprowadzania żadnych zmian może przerwać połączenie między dostawcą a firmą Microsoft i potencjalnie zablokować Ty i Twoi użytkownicy spoza Twojego konta.

Trwa opcję, aby utworzyć formant niestandardowy **Zarządzaj** części **dostępu warunkowego** strony.

![Formant](./media/controls/82.png)

Klikając **nowego formantu niestandardowego**, zostanie otwarty blok zawierający pole tekstowe dla danych JSON kontrolki.  

![Formant](./media/controls/81.png)

### <a name="deleting-custom-controls"></a>Usuwanie kontrolek niestandardowych

Aby usunąć formant niestandardowy, należy się najpierw upewnić, nie jest on używany w żadnych zasad dostępu warunkowego. Po zakończeniu:

1. Przejdź do listy formantów niestandardowych

2. Kliknij pozycję...  

3. Wybierz pozycję **Usuń**.

### <a name="editing-custom-controls"></a>Niestandardowe formanty do edycji

Aby edytować kontrolkę niestandardową, należy usunąć bieżącą kontroli i utworzyć nowy formant przy użyciu zaktualizowanych informacji.

## <a name="session-controls"></a>Kontrolki sesji

Kontrolki sesji umożliwiają korzystanie z ograniczonego środowiska w ramach aplikacji w chmurze. Kontrolki sesji są wymuszane przez aplikacje w chmurze i opierają się na dodatkowe informacje podane przez usługę Azure AD do aplikacji o sesji.

![Formant](./media/controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Użyj ograniczeń wymuszonych przez aplikację

Możesz użyć tego formantu, aby wymagają usługi Azure AD do przekazania informacji o urządzeniu do aplikacji w wybranej chmurze. Informacje o urządzeniu umożliwia aplikacji w chmurze należy wiedzieć, czy połączenie jest inicjowane z urządzenia zgodne lub dołączone do domeny. Ten formant obsługuje tylko usługi SharePoint Online i Exchange Online jako aplikacji w wybranej chmurze. Po wybraniu aplikacji w chmurze używa informacji o urządzeniu użytkownikom, w zależności od stanu urządzenia, za pomocą ograniczony lub pełnego środowiska.

Aby dowiedzieć się więcej, zobacz:

- [Włączanie ograniczony dostęp z usługą SharePoint Online](https://aka.ms/spolimitedaccessdocs)

- [Włączanie ograniczony dostęp z usługą Exchange Online](https://aka.ms/owalimitedaccess)

## <a name="next-steps"></a>Kolejne kroki

- Jeśli chcesz wiedzieć, jak skonfigurować zasady dostępu warunkowego, zobacz [wymagają usługi MFA dla określonych aplikacji przy użyciu usługi Azure Active Directory dostępu warunkowego](app-based-mfa.md).

- Jeśli wszystko jest gotowe do skonfigurowania zasad dostępu warunkowego dla danego środowiska, zobacz [najlepsze rozwiązania dotyczące dostępu warunkowego w usłudze Azure Active Directory](best-practices.md).
