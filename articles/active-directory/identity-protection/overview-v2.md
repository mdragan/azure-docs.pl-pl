---
title: Co to jest Azure Active Directory Identity Protection (odświeżane)? | Microsoft Docs
description: Co to jest Azure Active Directory Identity Protection (odświeżane)?
services: active-directory
keywords: Usługa Azure active directory identity protection odnajdywania aplikacji w chmurze, zarządzanie aplikacji, zabezpieczenia, ryzyka, poziom ryzyka, luk w zabezpieczeniach, zasady zabezpieczeń
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2b4f8caf03aad339cea3c3fcc732fc1af6086ea7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108884"
---
# <a name="what-is-azure-active-directory-identity-protection-refreshed"></a>Co to jest Azure Active Directory Identity Protection (odświeżane)?

Środowisko Identity Protection zostały odświeżone lepiej chronić tożsamości w organizacji. Zapewnia to odświeżone środowisko:

- Środowisko przeprojektowana administratora, który dotyczy tego ryzyka najbardziej odpowiednią dla ryzyka związanego z użytkownikiem you¬ i ryzyko logowania

- Odpytywanie zaawansowane środowisko o obsługę filtrowania, sortowania i inteligentne pliki do pobrania

- Udoskonalone obliczanie ryzyka użytkownika, aby pomóc w ustaleniu priorytetów swoich działań wobec użytkowników, które z największym prawdopodobieństwem mogą być narażone na ataki

- Nowy interfejs API obsługuje umożliwiające programowy dostęp do danych dotyczących ryzyka

- Proces opinii uproszczone administratora, który umożliwia od razu chronić użytkowników

- Nowe nadzorowanego uczenia maszynowego, aby poprawić dokładność ocena ryzyka



Zabezpieczenia są szczególnie ważne dla organizacji, już dziś. Większość zabezpieczeń, które wykonać naruszeń miejsce, gdy osoby atakujące uzyskują dostęp do środowiska, kradzież tożsamości użytkownika. W ciągu lat osoby atakujące stały się coraz bardziej efektywne w wykorzystaniu naruszeń innych firm i za pomocą zaawansowanych wyłudzanie informacji. Jak najszybciej napastnicy uzyskają dostęp do nawet użytkownika niski uprzywilejowanych kont, jest stosunkowo łatwe uzyskanie dostępu do zasobów firmy ważne za pośrednictwem ruchu poprzecznego. 

Aby odpowiedzieć na te zagrożenia, Azure AD Identity Protection umożliwia: 

- Proaktywnie uniemożliwić ze złamanymi zabezpieczeniami tożsamości są użyte 

- Automatycznie ograniczenia ryzyka po wykryciu podejrzanego działania 

- Badanie ryzykownych użytkowników i potencjalnych luk w zabezpieczeniach adres logowania  

- Alerty, gdy ryzyka związanego z użytkownikiem osiągnie określoną wartość progową 

 

Usługa Azure AD Identity Protection to funkcja usługi Azure Active Directory Premium P2, która umożliwia skonfigurowanie zasad, automatycznie odpowiadać, jeśli zostanie naruszony tożsamości użytkownika lub gdy ktoś jest inne niż właściciel konta próbuje zalogować się przy użyciu ich tożsamość. Te zasady, oprócz innych kontrolek dostępu warunkowego, dostarczone przez usługę Azure AD, albo można automatycznie Blokuj dostęp lub działania łagodzące inicjowania, takie jak resetowanie haseł lub wymuszanie uwierzytelniania wieloskładnikowego. Ponadto Identity Protection udostępnia możliwości monitorowania i raportowania w celu uzyskania bardziej szczegółowych informacji o podwyższonym ryzyku i potencjalnych kompromisów w Twojej organizacji. 

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RWsS6Q]


## <a name="risk-events"></a>Zdarzenia o podwyższonym ryzyku

Usługa Azure AD Identity Protection wykrywa następujące zdarzenia o podwyższonym ryzyku: 

 

| Typ zdarzenia o podwyższonym ryzyku | Opis | Typ wykrywania |
| ---             | ---         | ---            |
| Nietypowa podróży | Zaloguj się z nietypowych lokalizacji oparte na ostatnich operacji logowania użytkownika. | Offline |
| Anonimowego adresu IP | Zaloguj się za pomocą anonimowego adresu IP (na przykład: Tor przeglądarki, anonymizer sieci VPN). | W czasie rzeczywistym |
| Nieznane właściwości logowania | Zaloguj się przy użyciu właściwości, które nie okazały z ostatnio dla danego użytkownika. | W czasie rzeczywistym |
| Złośliwe oprogramowanie połączone adresu IP | Zaloguj się za pomocą adresu IP połączone złośliwego oprogramowania | Offline |
| Ujawnione poświadczenia | To zdarzenie o podwyższonym ryzyku oznacza, że prawidłowe poświadczenia użytkownika zostały ujawnione | Offline |





## <a name="types-of-risk"></a>Typy ryzyka 

Ochrona tożsamości opiera się na dwa rodzaje ryzyka:

- Ryzyko logowania

- Ryzyko związane z użytkownikiem

### <a name="sign-in-risk"></a>Ryzyko logowania

Ryzyko logowania reprezentuje prawdopodobieństwo, że dany uwierzytelniania nie Autoryzowano żądania przez właściciela tożsamości.

Istnieją dwa oceny ryzyka logowania: 

- **Ryzyko logowania (w czasie rzeczywistym)** -ryzyka logowania (w czasie rzeczywistym) opiera się na wszystkie wykrycia w czasie rzeczywistym, które mogą powodować podczas przetwarzania logowania.  

- **Ryzyko logowania (agregacji)** -ryzyka logowania (agregacji) jest to łączna liczba ryzyko logowania. Jest ona obliczana przez model, który uwzględnia uczenia maszynowego:

    - Wykrywanie w czasie rzeczywistym (opisane powyżej)
    
    - Wykrywanie w trybie offline, (które są aktywowane, po zalogowaniu miało miejsce) 
    
    - Wszystkie inne funkcje logowania


### <a name="user-risk"></a>Ryzyko związane z użytkownikiem

Ryzyko związane z użytkownikiem odpowiada prawdopodobieństwo złamane podanej tożsamości. 

Ryzyko związane z użytkownikiem jest obliczana, biorąc pod uwagę wszystkie ryzyka, które są skojarzone z użytkownikiem:

- Wszystkie ryzykowne logowania
- Wszystkie zdarzenia o podwyższonym ryzyku niepołączone z zalogowaniem się
- Bieżący ryzyka związanego z użytkownikiem
- Wszelkie ryzyko korygowania lub zwolnienia akcje wykonywane dla użytkownika do daty



## <a name="how-identity-protection-detects-risk"></a>Jak Identity Protection wykrywa zagrożenie  

Usługa Azure AD używa uczenia maszynowego w celu wykrycia anomalii i podejrzanych działań, za pomocą obu sygnałów wykryte w w czasie rzeczywistym podczas logowania oraz jak sygnały rzeczywistemu inne niż dotyczące użytkowników i ich działań logowania. Przy użyciu tych danych, Identity Protection oblicza w czasie rzeczywistym ryzyka logowania w każdym razem, gdy użytkownik jest uwierzytelniany, a także określenie ogólny poziom ryzyka użytkownika dla użytkownika w zasięgu. Identity Protection umożliwia automatyczne podejmowanie akcji na wykrywane odmiany ryzyko przez skonfigurowanie ryzyka związanego z użytkownikiem Identity Protection i zasad ryzyka logowania.  

 

Aby dowiedzieć się, jak Identity Protection wykrywa zagrożenie, istnieją dwie ważne pojęcia dotyczące: ryzyka związanego z użytkownikiem i ryzyko logowania. Ryzyko logowania odzwierciedla prawdopodobieństwo, że dany uwierzytelniania nie Autoryzowano żądania przez właściciela tożsamości. Istnieją dwa typy ryzyka logowania: w czasie rzeczywistym i całkowitego. W czasie rzeczywistym ryzyka logowania zostanie wykryty w czasie danego próby logowania (na przykład logowania z anonimowych adresów IP). Łączna liczba ryzyka logowania jest sumą wykryte w czasie rzeczywistym logowania ryzyko związane z również jako kolejne ryzyka się w czasie rzeczywistym zdarzeń skojarzonych z użytkownikiem logowania (na przykład niemożliwa zmiana lokalizacji). Ryzyko związane z użytkownikiem odzwierciedla ogólny prawdopodobieństwo, że nieuprawnione złamał podanej tożsamości. Ryzyko związane z użytkownikiem zawiera wszystkie działania o podwyższonym ryzyku dla danego użytkownika, w tym:

- W czasie rzeczywistym ryzyka logowania
- Kolejne ryzyka logowania
- Wykrywanie ryzykownego użytkownika.   

 

 
 ![Ruch](./media/overview-v2/01.png)
 

 

Przepływ punktu odniesienia dla wykrywania ryzyka Identity Protection i odpowiedzi dla dowolnej podanej logowania jest podsumowywane na powyższym rysunku.  

 

 

 

## <a name="common-scenarios"></a>Typowe scenariusze 

Spójrzmy na przykład pracownik firmy Contoso. 

1. Pracownik próbuje zalogować się do usługi Exchange Online z poziomu przeglądarki sieci Tor. Podczas logowania usługa Azure AD wykrywa zdarzenia ryzyka w czasie rzeczywistym. 

2. Usługa Azure AD wykryje, że pracownik loguje się z anonimowego adresu IP, wyzwalając poziomu średniego ryzyka logowania. 

3. Pracownik jest kwestionowana przez wiersz MFA, ponieważ administrator IT firmy Contoso skonfigurowany Identity Protection ryzyka logowania zasady dostępu warunkowego. Zasady wymagają usługi MFA na ryzyko logowania o średnim lub nowszej. 

4. Pracownik przekazuje wiersza MFA i uzyskuje dostęp do usługi Exchange Online, a ich poziom ryzyka użytkownika nie jest zmieniany. 

Co się stało się w tle? Próby logowania za pomocą przeglądarki Tor wyzwalane w czasie rzeczywistym ryzyka logowania w usłudze Azure AD dla anonimowego adresu IP. Usługa Azure AD przetworzeniu żądania stosowane ryzyka logowania zasady skonfigurowane w Identity Protection, ponieważ poziom ryzyka logowania pracownika spełnione próg (średnia liczba godzin). Ponieważ pracownik miał wcześniej zarejestrowane dla usługi MFA, byli w stanie odpowiedzieć i przekazać żądanie uwierzytelniania MFA. Możliwość pomyślnie przekazać żądanie uwierzytelniania MFA sygnalizowane do usługi Azure AD, że prawdopodobnie zostały uzasadnione tożsamość właściciela, a nie zwiększa ich poziom ryzyka użytkownika. 


Ale co zrobić, jeśli pracownik nie był jeden próba zalogowania? 

1. Złośliwy aktora przy użyciu poświadczeń pracownika próbuje zalogować się do swojego konta usługi Exchange Online z przeglądarki Tor, ponieważ chcą ukryć adresy IP. 

2. Usługa Azure AD wykryje, że próba logowania z anonimowego adresu IP, wyzwalania w czasie rzeczywistym ryzyka logowania. 

3. Złośliwe aktora jest kwestionowana przez wiersz MFA, ponieważ administrator IT firmy Contoso skonfigurowane zasady tożsamości ochrony logowania ryzyka dostępu warunkowego, aby wymagać uwierzytelniania Wieloskładnikowego w przypadku, gdy ryzyka logowania jest średnim lub nowszej. 

4. Złośliwy aktora kończy się niepowodzeniem żądania uwierzytelniania MFA i nie można uzyskać dostępu konta usługi Exchange Online pracownika. 

5. Nie powiodło się uwierzytelnianie wieloskładnikowe wiersza Wyzwolone zdarzenie o podwyższonym ryzyku, ma zostać zarejestrowany, wywoływanie ich ryzyka związanego z użytkownikiem dla przyszłych operacji logowania. 

Teraz, gdy złośliwe aktora próbował uzyskać dostęp do konta przez Sarah, zobaczmy, co się stanie po następnym pracownika próbuje zalogować. 

1. Pracownik próbuje zalogować się do usługi Exchange Online z programu Outlook. Podczas logowania usługa Azure AD wykrywa zdarzenia ryzyka w czasie rzeczywistym, jak również wszelkie ryzyko związane z użytkownikiem poprzedniego. 

2. Usługa Azure AD nie wykrywa w czasie rzeczywistym ryzyka logowania, ale wykrywa ryzyka związanego z użytkownikiem wysokiej ze względu na ostatnie ryzykowne działania w poprzednich scenariuszach.  

3. Pracownik jest kwestionowana, monit resetowania hasła, ponieważ Contoso administratora IT skonfigurowany przez Wymagaj zmiany hasła podczas logowania użytkownika przy użyciu wysokiego ryzyka zasad ryzyka użytkownika ochrony tożsamości. 

4. Ponieważ pracownik jest zarejestrowany do samoobsługowego resetowania HASEŁ i uwierzytelniania Wieloskładnikowego, pomyślnie zresetuje hasło. 

5. Poprzez zresetowanie hasła, pracownika poświadczenia nie są już naruszenia zabezpieczeń i ich tożsamości powraca do stanu bezpiecznego. 

6. Pracownika poprzednie zdarzenia o podwyższonym ryzyku są rozwiązywane i jej poziom ryzyka użytkownika jest automatycznie ustawiany w odpowiedzi na łagodzenia poświadczenia pozbyć się tych kompromisów. 

## <a name="how-do-i-configure-identity-protection"></a>Jak skonfigurować ochronę tożsamości? 

Aby rozpocząć pracę z usługą Identity Protection, należy najpierw skonfigurować zasad ryzyka dla użytkownika i zasad ryzyka logowania. Gdy te zasady są konfigurowane i stosowane w grupie testowej, można symulować zdarzeń o podwyższonym ryzyku, aby zrozumieć, jak Identity Protection będą odpowiadać w danym środowisku. Poniżej Szybki Start przewodniki zawierają wskazówki dotyczące sposobu konfigurowania zasad wyżej i przetestować w środowisku. 

 

Identity Protection obsługuje 3 role w usłudze Azure AD do zrównoważenia działań z zakresu zarządzania całym wdrożeniu: 

| Rola | Można zrobić | Nie można wykonać |
| --- | --- | --- |
| Administrator globalny | Pełny dostęp do usługi Identity Protection dołączanie Identity Protection | |
| Administrator zabezpieczeń | Pełny dostęp do usługi Identity Protection | Dołączanie Identity Protection, resetować hasła dla użytkownika |
| Czytelnik zabezpieczeń | Dostęp tylko do odczytu do usługi Identity Protection | Korygowanie użytkowników dołączanie Identity Protection, konfigurowanie zasad, resetowania haseł| 

Aby uzyskać więcej informacji, zobacz [przypisywanie ról administratorów w usłudze Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)

 
## <a name="licensing"></a>Licencjonowanie

>[!NOTE]
> W publicznej wersji zapoznawczej usługi Identity Protection (odświeżane) tylko usługi Azure AD Premium P2 klienci będą mieli dostęp do raportów ryzykownych użytkowników i raport dotyczący ryzykownych logowań.



| Możliwości | Usługa Azure AD — warstwa Premium P2 | Usługa Azure AD — warstwa Premium P1 | Podstawowa/bezpłatne usługi Azure AD |
| --- | --- | --- | --- |
| Zasady dotyczące ryzyka związanego z użytkownika | Tak | Nie | Nie |
| Zasady ryzyka logowania | Tak | Nie | Nie |
| Raport ryzykownych użytkowników | Pełny dostęp | Ograniczone informacje | Ograniczone informacje |
| Raport dotyczący ryzykownych logowań | Pełny dostęp | Ograniczone informacje | Ograniczone informacje |
| Zasady rejestracji usługi MFA | Tak | Nie | Nie |







## <a name="next-steps"></a>Kolejne kroki 

Aby rozpocząć pracę z usługą Identity Protection, zobacz [Konfigurowanie zasad ryzyka logowania](quickstart-sign-in-risk-policy.md). 






