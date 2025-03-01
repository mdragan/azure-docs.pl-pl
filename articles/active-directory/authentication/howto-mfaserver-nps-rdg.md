---
title: Brama usług pulpitu zdalnego i serwer Azure MFA przy użyciu usługi RADIUS — usługi Azure Active Directory
description: Jest to strona poświęcona usłudze Azure Multi-Factor Authentication i zawiera informacje pomocne we wdrażaniu bramy usług pulpitu zdalnego (Remote Desktop, RD) i serwera Azure Multi-Factor Authentication przy użyciu usługi RADIUS.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: a129030e8071dc590562ca5ca203d8d735f0449e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052553"
---
# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Brama usług pulpitu zdalnego i serwer Azure Multi-Factor Authentication korzystające z usługi RADIUS

Często bramy usług pulpitu zdalnego (RD) używa lokalnej [usług zasad sieciowych (NPS)](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide#BKMK_optionalfeatures) do uwierzytelniania użytkowników. Ten artykuł zawiera informacje dotyczące określenia trasy żądań usługi RADIUS z bramy usług pulpitu zdalnego (za pośrednictwem lokalnych usług NPS) do serwera Multi-Factor Authentication. Połączenie usługi Azure MFA i bramy usług pulpitu zdalnego pozwala użytkownikom uzyskiwać dostęp do środowisk pracy z dowolnego miejsca i korzystać z silnego uwierzytelniania.

Ponieważ uwierzytelnianie systemu Windows dla usług terminalowych nie jest obsługiwane w przypadku systemu Windows Server 2012 R2, należy użyć bramy usług pulpitu zdalnego i usługi RADIUS do integracji z serwerem MFA.

Serwer Azure Multi-Factor Authentication należy zainstalować na osobnym serwerze, który przekaże żądanie usługi RADIUS z powrotem do serwera NPS na serwerze bramy usług pulpitu zdalnego. Po zweryfikowaniu nazwy użytkownika i hasła serwer NPS zwróci odpowiedź do serwera Multi-Factor Authentication. Następnie serwer MFA zrealizuje drugi składnik uwierzytelniania przed zwróceniem wyników do bramy.

> [!IMPORTANT]
> Począwszy od 1 lipca 2019 firma Microsoft będzie oferować już serwer MFA w przypadku nowych wdrożeń. Nowi klienci, którzy chcesz wymagać uwierzytelniania wieloskładnikowego od użytkowników należy używać oparte na chmurze usługi Azure Multi-Factor Authentication. Istniejący klienci, którzy aktywowali usługę MFA Server przed 1 lipca będzie można pobrać najnowszą wersję, a przyszłe aktualizacje i Generuj poświadczenia aktywacji w zwykły sposób.

## <a name="prerequisites"></a>Wymagania wstępne

- Przyłączony do domeny serwer Azure MFA. Jeśli nie jest jeszcze zainstalowany, postępuj zgodnie z instrukcjami przedstawionymi w artykule [Wprowadzenie do serwera Azure Multi-Factor Authentication](howto-mfaserver-deploy.md).
- Istniejące skonfigurowane serwera NPS.
- Brama usług pulpitu zdalnego uwierzytelniana za pomocą usług Network Policy Services.

> [!NOTE]
> W tym artykule należy używać z tylko wdrożenia serwera usługi MFA nie usługi Azure MFA (oparte na chmurze).

## <a name="configure-the-remote-desktop-gateway"></a>Konfigurowanie bramy pulpitu zdalnego

Należy skonfigurować bramę usług pulpitu zdalnego w celu wysyłania uwierzytelniania usługi RADIUS do serwera Azure Multi-Factor Authentication.

1. W menedżerze bramy usług pulpitu zdalnego kliknij prawym przyciskiem myszy nazwę serwera i wybierz pozycję **Właściwości**.
2. Przejdź na kartę **Magazyn zasad autoryzacji połączeń usług pulpitu zdalnego**, a następnie wybierz pozycję **Centralny serwer, na którym działają usługi NPS**.
3. Dodaj co najmniej jeden serwer Azure Multi-Factor Authentication jako serwer usługi RADIUS, wprowadzając nazwę lub adres IP poszczególnych serwerów.
4. Utwórz wspólny klucz tajny dla każdego serwera.

## <a name="configure-nps"></a>Konfigurowanie serwera NPS

Brama usług pulpitu zdalnego używa serwera NPS do wysyłania żądań usługi RADIUS do usługi Azure Multi-Factor Authentication. Aby skonfigurować usługi NPS, najpierw należy zmienić ustawienia limitu czasu, aby zapobiec przekraczaniu limitu czasu przez bramę usług pulpitu zdalnego przed ukończeniem weryfikacji dwuetapowej. Następnie należy zaktualizować usługi NPS na potrzeby odbierania uwierzytelnień usługi RADIUS z serwera MFA. Postępuj zgodnie z następującą procedurą, aby skonfigurować usługi NPS:

### <a name="modify-the-timeout-policy"></a>Modyfikowanie zasad limitu czasu

1. W usługach NPS otwórz menu **Klienci i serwery usługi RADIUS** w lewej kolumnie, a następnie wybierz pozycję **Grupy zdalnych serwerów usługi RADIUS**.
2. Wybierz pozycję **GRUPA SERWERA BRAMY USŁUG TERMINALOWYCH**.
3. Przejdź na kartę **Równoważenie obciążenia**.
4. Zmień ustawienia **Liczba sekund bez odpowiedzi zanim żądanie zostanie uznane za porzucone** i **Liczba sekund między żądaniami, gdy serwer jest zidentyfikowany jako niedostępny**, wybierając wartości między 30 a 60 sekundami. Jeśli okaże się, że nadal ma miejsce przekraczanie limitu czasu przez serwer podczas uwierzytelniania, możesz tu wrócić i zwiększyć liczbę sekund.
5. Przejdź na kartę **Uwierzytelnianie/konto** i sprawdź, czy określone porty usługi RADIUS są zgodne z portami, na których będzie nasłuchiwać serwer Multi-Factor Authentication.

### <a name="prepare-nps-to-receive-authentications-from-the-mfa-server"></a>Przygotowywanie usług NPS do odbierania uwierzytelnień z serwera MFA

1. Kliknij prawym przyciskiem myszy pozycję **Klienci RADIUS** w obszarze Klienci i serwery usługi RADIUS w lewej kolumnie, a następnie wybierz pozycję **Nowy**.
2. Dodaj serwer Azure Multi-Factor Authentication jako klienta usługi RADIUS. Wybierz przyjazną nazwę i określ wspólny klucz tajny.
3. Otwórz menu **Zasady** w lewej kolumnie, a następnie wybierz pozycję **Zasady żądań połączeń**. Powinny zostać wyświetlone zasady o nazwie ZASADY AUTORYZACJI BRAMY USŁUG TERMINALOWYCH, które zostały utworzone podczas konfigurowania bramy usług pulpitu zdalnego. Te zasady powodują przesyłanie żądań usługi RADIUS do serwera Multi-Factor Authentication.
4. Kliknij prawym przyciskiem pozycję **ZASADY AUTORYZACJI BRAMY USŁUG TERMINALOWYCH**, a następnie wybierz pozycję **Duplikuj zasady**.
5. Otwórz nowe zasady i przejdź na kartę **Warunki**.
6. Dodaj warunek, który dopasowuje przyjazną nazwę klienta do przyjaznej nazwy ustawionej w kroku 2 dla klienta usługi RADIUS serwera Azure Multi-Factor Authentication.
7. Przejdź na kartę **Ustawienia**, a następnie wybierz pozycję **Uwierzytelnianie**.
8. Zmień opcję dostawcy uwierzytelniania na wartość **Uwierzytelniaj żądania na tym serwerze**. Te zasady zapewniają, że po odebraniu przez usługi NPS żądania usługi RADIUS z serwera Azure MFA uwierzytelnianie odbywa się lokalnie, a żądania usługi RADIUS nie są wysyłane ponownie do serwera Azure Multi-Factor Authentication, co mogłoby spowodować zapętlenie.
9. Aby zapobiec zapętleniu, upewnij się, że nowe zasady są POWYŻEJ oryginalnych zasad w okienku **Zasady żądań połączeń**.

## <a name="configure-azure-multi-factor-authentication"></a>Konfigurowanie usługi Azure Multi-Factor Authentication

Serwer Azure Multi-Factor Authentication jest konfigurowany jako serwer proxy usługi RADIUS pomiędzy bramą usług pulpitu zdalnego a serwerem NPS.  Powinien zostać zainstalowany na serwerze przyłączonym do domeny, który jest oddzielony od serwera bramy usług pulpitu zdalnego. Poniższa procedura umożliwia skonfigurowanie serwera Azure Multi-Factor Authentication.

1. Otwórz serwer Azure Multi-Factor Authentication i wybierz ikonę uwierzytelniania usługi RADIUS.
2. Zaznacz pole wyboru **Włącz uwierzytelnianie usługi RADIUS**.
3. Na karcie Klienci upewnij się, że porty są zgodne z portami skonfigurowanymi w usługach NPS, a następnie wybierz pozycję **Dodaj**.
4. Dodaj adres IP serwera bramy usług pulpitu zdalnego, nazwę aplikacji (opcjonalnie) i wspólny wpis tajny. Wspólny wpis tajny musi być taki sam dla serwera usługi Azure Multi-Factor Authentication i bramy usług pulpitu zdalnego.
3. Kliknij kartę **Cel**, a następnie wybierz przycisk radiowy **Serwery RADIUS**.
4. Wybierz pozycję **Dodaj**, a następnie wprowadź adres IP, wspólny wpis tajny i porty serwera usług NPS. Jeśli nie jest używany centralny serwer usług NPS, klient usługi RADIUS i cel usługi RADIUS będą takie same. Wspólny wpis tajny musi być zgodny z kluczem skonfigurowanym w sekcji klienta usługi RADIUS serwera NPS.

![Uwierzytelnianie usługi RADIUS na serwerze MFA](./media/howto-mfaserver-nps-rdg/radius.png)

## <a name="next-steps"></a>Kolejne kroki

- Zintegruj usługę Azure MFA z [aplikacjami internetowymi usługi IIS](howto-mfaserver-iis.md)

- Uzyskaj odpowiedzi z artykułu [Często zadawane pytania dotyczące usługi Azure Multi-Factor Authentication](multi-factor-authentication-faq.md)
