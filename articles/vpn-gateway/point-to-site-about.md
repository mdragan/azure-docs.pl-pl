---
title: Informacje o połączeniach Azure Point-to-Site VPN | Dokumentacja firmy Microsoft
description: Ten artykuł pomoże Ci zrozumieć połączeń punkt-lokacja i pomoże Ci zdecydować, której typ uwierzytelniania bramy sieci VPN P2S do użycia.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: cherylmc
ms.openlocfilehash: f1e014bb14b2b5c1ae924f4371e08aa8bf8698f2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056478"
---
# <a name="about-point-to-site-vpn"></a>Sieci VPN typu punkt lokacja — informacje

Połączenie bramy VPN Gateway typu punkt-lokacja pozwala utworzyć bezpieczne połączenie z siecią wirtualną z indywidualnego komputera klienckiego. Połączenie typu punkt-lokacja jest ustanawiane przez uruchomienie z komputera klienckiego. To rozwiązanie jest przydatne dla osób pracujących zdalnie, które chcą łączyć się z sieciami wirtualnymi platformy Azure z lokalizacji zdalnej, na przykład z domu lub sali konferencyjnej. Połączenie sieci VPN typu punkt-lokacja jest również przydatne zamiast połączenia sieci VPN typu lokacja-lokacja w przypadku niewielkiej liczby klientów, którzy muszą się łączyć z siecią wirtualną. Ten artykuł ma zastosowanie w modelu wdrażania usługi Resource Manager.

## <a name="protocol"></a>Jaki protokół używa P2S?

Sieci VPN typu punkt lokacja można użyć jednej z następujących protokołów:

* **Protokół® OpenVPN**, SSL/TLS, na podstawie protokołu sieci VPN. Rozwiązanie SSL sieci VPN może przechodzić przez zapory, ponieważ większość zapór otwiera port TCP 443 wychodzących, która używa protokołu SSL. OpenVPN może służyć do łączenia z systemem Android, iOS (w wersji 11.0 i nowszych), urządzenia Windows, Linux i Mac (wersji OS x 10.13 i nowsze wersje).

* Zabezpiecz SSTP Socket Tunneling Protocol (), protokołem opartym na protokole SSL sieci VPN. Rozwiązanie SSL sieci VPN może przechodzić przez zapory, ponieważ większość zapór otwiera port TCP 443 wychodzących, która używa protokołu SSL. Protokół SSTP jest obsługiwana tylko na urządzeniach Windows. Platforma Azure obsługuje wszystkich wersji systemu Windows, który ma protokołu SSTP (Windows 7 i nowsze).

* Sieć VPN z protokołem IKEv2 to oparte na standardach rozwiązanie sieci VPN korzystające z protokołu IPsec. Sieci VPN z protokołem IKEv2 można używać do łączenia z urządzeniami Mac (z systemem OSX 10.11 lub nowszym).


>[!NOTE]
>Protokół IKEv2 i OpenVPN dla P2S są dostępne tylko modelu wdrażania usługi Resource Manager. Nie są one dostępne dla klasycznego modelu wdrażania.
>

## <a name="authentication"></a>Sposób uwierzytelniania klientów sieci VPN typu P2S

Przed Azure akceptuje połączenia sieci VPN typu P2S, użytkownik musi najpierw uwierzytelniania. Istnieją dwa mechanizmy, oferowanych przez platformę Azure w celu uwierzytelnienia użytkownik nawiązujący połączenie.

### <a name="authenticate-using-native-azure-certificate-authentication"></a>Uwierzytelnianie przy użyciu uwierzytelniania certyfikatu platformy Azure natywne

Korzystając z uwierzytelniania natywnych certyfikatu platformy Azure, certyfikat klienta, który jest obecny na urządzeniu jest używany do uwierzytelnienia użytkownik nawiązujący połączenie. Certyfikaty klienta są generowane na podstawie zaufanego certyfikatu głównego, a następnie instalowane na każdym komputerze klienckim. Można użyć certyfikatu głównego, który został wygenerowany za pomocą rozwiązania przedsiębiorstwa, lub możesz wygenerować certyfikat z podpisem własnym.

Weryfikacja certyfikatu klienta jest wykonywane przez bramę sieci VPN i będzie się działo podczas ustanawiania połączenia sieci VPN typu P2S. Certyfikat główny jest wymagany dla weryfikacji i musi zostać przekazany na platformę Azure.

### <a name="authenticate-using-active-directory-ad-domain-server"></a>Uwierzytelnianie przy użyciu serwera domeny usługi Active Directory (AD)

Uwierzytelnianie domeny AD umożliwia użytkownikom łączenie z platformą Azure, przy użyciu swoich poświadczeń domeny organizacji. Wymaga serwera usługi RADIUS, która integruje się z serwerem usługi AD. Organizacje także korzystać z ich istniejące wdrożenie usługi RADIUS.   
  
Serwer usługi RADIUS, może być wdrożony lokalnie lub w sieci wirtualnej platformy Azure. Podczas uwierzytelniania usługi Azure VPN Gateway działa jako przekazywania i przekazuje komunikaty uwierzytelniania pomiędzy serwerem usługi RADIUS i łączącego się urządzenia. Dlatego ważne jest, osiągalności bramy do serwera RADIUS. Jeśli serwer RADIUS jest obecny w środowisku lokalnym, połączenie S2S sieci VPN od platformy Azure do lokacji lokalnej jest wymagana dla osiągalności.  
  
Serwer usługi RADIUS można również zintegrować z usługami certyfikatów usługi AD. Dzięki temu można użyć serwera usługi RADIUS i wdrożeniu certyfikatu przedsiębiorstwa P2S uwierzytelniania certyfikatu jako alternatywę dla uwierzytelniania certyfikatu platformy Azure. Zaletą jest to, że nie trzeba przekazać certyfikatów głównych i odwołanych certyfikatów do platformy Azure.

Serwer usługi RADIUS można również zintegrować z innymi systemami tożsamości zewnętrznej. Spowoduje to otwarcie wiele możliwości uwierzytelniania sieci VPN P2S, włącznie z opcjami usługi Multi-Factor Authentication.

>[!NOTE]
>**Protokół® OpenVPN** nie jest obsługiwana przy użyciu uwierzytelniania usługi RADIUS.
>

![punkt lokacja](./media/point-to-site-about/p2s.png "Point-to-Site")

## <a name="what-are-the-client-configuration-requirements"></a>Jakie są wymagania dotyczące konfiguracji klienta?

>[!NOTE]
>W przypadku klientów Windows musi mieć prawa administratora na urządzeniu klienckim, aby zainicjować połączenie sieci VPN na urządzeniu klienckim na platformie Azure.
>

Użytkownicy na użytek natywnych klientów sieci VPN na urządzeniach Windows i Mac P2S. Platforma Azure udostępnia klienta sieci VPN zip konfiguracji zawierający ustawienia wymagane przez tych klientów natywnych do połączenia z platformą Azure.

* W przypadku urządzeń Windows konfiguracji klienta sieci VPN składa się z pakiet instalacyjny instalowanego przez użytkowników na ich urządzeniach.
* W przypadku urządzeń Mac składa się z pliku mobileconfig instalowanego przez użytkowników na ich urządzeniach.

Plik zip zawiera również wartości niektórych ważnych ustawień po stronie platformy Azure, można użyć w celu utworzenia profilu dla tych urządzeń. Niektóre wartości obejmują adres bramy sieci VPN, typów tuneli skonfigurowany, trasy i certyfikatu głównego do sprawdzania poprawności bramy.

>[!NOTE]
>[!INCLUDE [TLS version changes](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="gwsku"></a>Które jednostki SKU bramy są obsługiwane przez sieci VPN P2S?

[!INCLUDE [aggregate throughput sku](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

* Aby uzyskać zalecenia dotyczące jednostki SKU bramy, zobacz [ustawienia bramy sieci VPN — informacje](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

>[!NOTE]
>Podstawowa jednostka SKU nie obsługuje uwierzytelniania za pomocą protokołu IKEv2 ani usługi RADIUS.
>

## <a name="IKE/IPsec policies"></a>Jakie zasady IKE/IPsec są skonfigurowane na bramach sieci VPN dla P2S?


**IKEv2**

|**Szyfrowania** | **Integralność** | **PRF** | **Grupa DH** |
|---        | ---           | ---       | ---   |
|GCM_AES256 |   GCM_AES256  | SHA384    | GROUP_24 |
|GCM_AES256 |   GCM_AES256  | SHA384    | GROUP_14 |
|GCM_AES256 |   GCM_AES256  | SHA384    | GROUP_ECP384 |
|GCM_AES256 |   GCM_AES256  | SHA384    | GROUP_ECP256 |
|GCM_AES256 |   GCM_AES256  | SHA256    | GROUP_24 |
|GCM_AES256 |   GCM_AES256  | SHA256    | GROUP_14 |
|GCM_AES256 |   GCM_AES256  | SHA256    | GROUP_ECP384 |
|GCM_AES256 |   GCM_AES256  | SHA256    | GROUP_ECP256 |
|AES256     |   SHA384      | SHA384    | GROUP_24 |
|AES256     |   SHA384      | SHA384    | GROUP_14 |
|AES256     |   SHA384      | SHA384    | GROUP_ECP384 |
|AES256     |   SHA384      | SHA384    | GROUP_ECP256 |
|AES256     |   SHA256      | SHA256    | GROUP_24 |
|AES256     |   SHA256      | SHA256    | GROUP_14 |
|AES256     |   SHA256      | SHA256    | GROUP_ECP384 |
|AES256     |   SHA256      | SHA256    | GROUP_ECP256 |
|AES256     |   SHA256      | SHA256    | GROUP_2 |

**IPsec**

|**Szyfrowania** | **Integralność** | **Grupa PFS** |
|---        | ---           | ---       |
|GCM_AES256 | GCM_AES256 | GROUP_NONE |
|GCM_AES256 | GCM_AES256 | GROUP_24 |
|GCM_AES256 | GCM_AES256 | GROUP_14 |
|GCM_AES256 | GCM_AES256 | GROUP_ECP384 |
|GCM_AES256 | GCM_AES256 | GROUP_ECP256 |
| AES256    | SHA256 | GROUP_NONE |
| AES256    | SHA256 | GROUP_24 |
| AES256    | SHA256 | GROUP_14 |
| AES256    | SHA256 | GROUP_ECP384 |
| AES256    | SHA256 | GROUP_ECP256 |
| AES256    | SHA1 | GROUP_NONE |

## <a name="TLS policies"></a>Jakie zasady protokołu TLS są skonfigurowane na bramach sieci VPN dla P2S?
**TLS**

|**Zasady** |
|---| 
|TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 |
|TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 |
|TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 |
|TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 |
|TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256 |
|TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384 |
|TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 |
|TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 |
|TLS_RSA_WITH_AES_128_GCM_SHA256 |
|TLS_RSA_WITH_AES_256_GCM_SHA384 |
|TLS_RSA_WITH_AES_128_CBC_SHA256 |
|TLS_RSA_WITH_AES_256_CBC_SHA256 |




## <a name="configure"></a>Jak skonfigurować połączenia P2S?

Konfiguracji P2S wymaga kilku określonych kroków. Następujące artykuły zawierają instrukcje umożliwia przeprowadzenie konfiguracji P2S i linki do skonfigurowanie urządzeń klienckich w sieci VPN:

* [Konfigurowanie połączenia P2S — uwierzytelnianie usługi RADIUS](point-to-site-how-to-radius-ps.md)

* [Konfigurowanie połączenia P2S - Azure natywne Uwierzytelnianie certyfikatów](vpn-gateway-howto-point-to-site-rm-ps.md)

* [Konfigurowanie OpenVPN](vpn-gateway-howto-openvpn.md)

## <a name="how-do-i-remove-the-configuration-of-a-p2s-connection"></a>Jak usunąć konfigurację połączenia P2S?

Można usunąć konfiguracji P2S za pomocą interfejsu wiersza polecenia az oraz następujące polecenie: 

`az network vnet-gateway update --name <gateway-name> --resource-group <resource-group name> --remove "vpnClientConfiguration"`
 
## <a name="faqcert"></a>Często zadawane pytania dotyczące uwierzytelniania certyfikatu platformy Azure natywne

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faqradius"></a>Często zadawane pytania dotyczące uwierzytelniania usługi RADIUS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Następne kroki

* [Konfigurowanie połączenia P2S — uwierzytelnianie usługi RADIUS](point-to-site-how-to-radius-ps.md)

* [Konfigurowanie połączenia P2S - Azure natywne Uwierzytelnianie certyfikatów](vpn-gateway-howto-point-to-site-rm-ps.md)

**"OpenVPN" jest zastrzeżonym znakiem towarowym firmy OpenVPN Inc.**
