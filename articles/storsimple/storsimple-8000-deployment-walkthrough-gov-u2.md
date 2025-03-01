---
title: Wdrażanie urządzenia StorSimple 8000 series w portalu dla instytucji rządowych | Dokumentacja firmy Microsoft
description: W tym artykule opisano kroki i najlepsze rozwiązania dotyczące wdrażania urządzenia StorSimple 8000 series systemem Update 3 lub nowszą oraz usługi w portalu Azure dla instytucji rządowych.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: alkohli
ms.openlocfilehash: 4f093251ce3c711cc060369abb27770157024d91
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61488960"
---
# <a name="deploy-your-on-premises-storsimple-device-in-the-government-portal"></a>Wdrażanie lokalnego urządzenia StorSimple w portalu dla instytucji rządowych

## <a name="overview"></a>Omówienie
Witamy w procesie wdrażania urządzenia Microsoft Azure StorSimple. Te samouczki wdrażania dotyczą serii StorSimple 8000, oprogramowaniem aktualizacji 3 lub później w portalu Azure dla instytucji rządowych. Ta seria samouczków obejmuje listy kontrolnej dotyczącej konfiguracji, listę wymagania wstępne dotyczące konfiguracji oraz szczegółowe opisy kroków konfiguracji dla urządzenia StorSimple.

Informacje podane w tych samouczkach mają zastosowanie po zapoznaniu się ze środkami ostrożności i dopiero wówczas, gdy urządzenie StorSimple zostało rozpakowane i zamontowane w stojaku oraz podłączono do niego kable. Jeśli nadal trzeba wykonać te zadania, należy rozpocząć od sprawdzenia [środków ostrożności](storsimple-safety.md). Wykonaj instrukcje dotyczące rozpakowania określonego urządzenia, zamontowania go w stojaku oraz podłączania do niego kabli.

* [Rozpakowywanie, montowanie w stojaku i podłączanie kabli do urządzenia 8100](storsimple-8100-hardware-installation.md)
* [Rozpakowywanie, montowanie w stojaku i podłączanie kabli do urządzenia 8600](storsimple-8600-hardware-installation.md)

Do ukończenia procesu instalacji i konfiguracji niezbędne są uprawnienia administratora. Przed rozpoczęciem warto przejrzeć listę kontrolną dotyczącą konfiguracji. Proces wdrażania i konfiguracji może potrwać pewien czas.

> [!NOTE]
> Informacje na temat wdrażania urządzenia StorSimple opublikowane w witrynie platformy Microsoft Azure w sieci Web dotyczą tylko urządzeń z serii StorSimple 8000. Aby uzyskać pełne informacje dotyczące urządzeń z serii 7000, przejdź do: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Informacje dotyczące wdrażania urządzeń z serii 7000 można znaleźć w [przewodniku Szybki start do systemu StorSimple](http://onlinehelp.storsimple.com/111_Appliance/).


## <a name="deployment-steps"></a>Kroki wdrażania
Wykonaj wymagane kroki, aby skonfigurować urządzenie StorSimple i połączyć je z usługą Menedżer urządzeń StorSimple. Oprócz kroków wymaganych istnieją opcjonalne kroki i procedury, które należy wykonać podczas wdrażania. W szczegółowych instrukcjach dotyczących wdrażania wskazano, w którym momencie należy wykonać poszczególne kroki opcjonalne.

| Krok | Opis |
| --- | --- |
| **WYMAGANIA WSTĘPNE** |Te czynności należy wykonać w ramach przygotowań do przyszłego wdrożenia. |
| [Lista kontrolna dotycząca konfiguracji wdrożenia](#deployment-configuration-checklist) |Ta lista kontrolna umożliwia zbieranie i rejestrowanie informacji przed wdrożeniem i w jego trakcie. |
| [Wymagania wstępne dotyczące wdrożenia](#deployment-prerequisites) |Te zweryfikować, że środowisko jest gotowe do wdrożenia. |
|  | |
| **WDROŻENIE KROK PO KROKU** |Te kroki są wymagane do wdrożenia urządzenia StorSimple w środowisku produkcyjnym. |
| [Krok 1. Utwórz nową usługę](#step-1-create-a-new-service) |Skonfiguruj magazyn i zarządzanie chmurą dla danego urządzenia StorSimple. *Jeśli masz istniejącą usługę dla innych urządzeń StorSimple, pomiń ten krok*. |
| [Krok 2. Pobierz klucz rejestracji usługi](#step-2-get-the-service-registration-key) |Użyj tego klucza rejestru i połączyć urządzenia StorSimple z usługą zarządzania. |
| [Krok 3. Konfigurowanie i rejestrowanie urządzenia za pomocą programu Windows PowerShell dla usługi StorSimple](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |Połącz urządzenie z siecią i zarejestruj je przy użyciu platformy Azure, aby ukończyć instalację za pomocą usługi zarządzania. |
| [Krok 4. Przeprowadzenie minimalnej konfiguracji urządzenia](#step-4-complete-minimum-device-setup) </br>Opcjonalnie: Aktualizacja urządzenia StorSimple. |Skorzystaj z usługi zarządzania, aby skonfigurować urządzenie i umożliwić przechowywanie w nim danych. |
| [Krok 5. Tworzenie kontenera woluminów](#step-5-create-a-volume-container) |Utwórz kontener, aby umożliwić inicjowanie obsługi woluminów. Kontener woluminów obejmuje ustawienia konta magazynu, przepustowości i szyfrowania wszystkich woluminów, które zawiera. |
| [Krok 6: Tworzenie woluminu](#step-6-create-a-volume) |Zainicjuj obsługę woluminów magazynu na urządzeniu StorSimple na potrzeby serwerów. |
| [Krok 7: Instalowanie, inicjowanie i formatowanie woluminu](#step-7-mount-initialize-and-format-a-volume) </br>Opcjonalnie: Konfigurowanie wielościeżkowego wejścia/wyjścia. |Połącz serwery z magazynem iSCSI udostępnianym przez urządzenie. Opcjonalnie skonfigurować wielościeżkowe wejście/wyjście, aby upewnić się, że serwery tolerują błędy linków, sieci i interfejsu. |
| [Krok 8: Tworzenie kopii zapasowej](#step-8-take-a-backup) |Skonfiguruj zasady tworzenia kopii zapasowej, aby chronić dane. |
|  | |
| **INNE PROCEDURY** |Te procedury mogą okazać się potrzebne podczas wdrażania rozwiązania. |
| [Konfigurowanie nowego konta magazynu dla usługi](#configure-a-new-storage-account-for-the-service) | |
| [Łączenie z konsolą szeregową urządzenia przy użyciu programu PuTTY](#use-putty-to-connect-to-the-device-serial-console) | |
| [Wyszukiwanie i stosowanie aktualizacji](#scan-for-and-apply-updates) | |
| [Pobieranie nazwy IQN hosta z systemem Windows Server](#get-the-iqn-of-a-windows-server-host) | |
| [Ręczne tworzenie kopii zapasowej](#create-a-manual-backup) | |


## <a name="deployment-configuration-checklist"></a>Lista kontrolna dotycząca konfiguracji wdrożenia
Przed przystąpieniem do wdrażania urządzenia StorSimple, należy zebrać informacje w celu skonfigurowania oprogramowania na urządzeniu. Wcześniejsze przygotowanie niektórych z tych informacji usprawni proces wdrażania urządzenia StorSimple w środowisku użytkownika. Pobierz i zanotuj szczegóły konfiguracji, jak wdrożyć urządzenie za pomocą tej listy kontrolnej.

[Pobieranie listy kontrolnej dotyczącej konfiguracji wdrożenia urządzenia StorSimple](https://www.microsoft.com/download/details.aspx?id=49159)

## <a name="deployment-prerequisites"></a>Wymagania wstępne dotyczące wdrażania
Poniższe sekcje zawierają opis wymagań wstępnych dotyczących konfiguracji usługi Menedżer urządzeń StorSimple oraz urządzenia StorSimple.

### <a name="for-the-storsimple-device-manager-service"></a>Usługa Menedżer urządzeń StorSimple
Przed rozpoczęciem upewnij się, że:

* Masz konto Microsoft z poświadczeniami dostępu.
* Masz konto magazynu platformy Microsoft Azure z poświadczeniami dostępu.
* Subskrypcja platformy Microsoft Azure dla usługi Menedżer urządzeń StorSimple została włączona. Subskrypcję należy zakupić w ramach umowy [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/).
* Masz dostęp do oprogramowania służącego do emulacji terminala, takiego jak PuTTY.

### <a name="for-the-device-in-the-datacenter"></a>Urządzenie w centrum danych
Przed skonfigurowaniem urządzenia upewnij się, że:

* Urządzenie zostało całkowicie rozpakowane, zamontowane w stojaku i podłączono wszystkie kable umożliwiające dostęp do zasilania, sieci oraz dostęp szeregowy, zgodnie z opisem w części:
  
  * [Rozpakowywanie, montowanie w stojaku i podłączanie kabli do urządzenia 8100](storsimple-8100-hardware-installation.md)
  * [Rozpakowywanie, montowanie w stojaku i podłączanie kabli do urządzenia 8600](storsimple-8600-hardware-installation.md)

### <a name="for-the-network-in-the-datacenter"></a>Sieć w centrum danych
Przed rozpoczęciem upewnij się, że:

* Porty w zaporze centrum danych zostały otwarte w celu zezwolenia na ruch związany z interfejsem iSCSI i chmurą, zgodnie z opisem w temacie [Networking requirements for your StorSimple device](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device) (Wymagania dotyczące pracy w sieci dla urządzenia StorSimple).

## <a name="step-by-step-deployment"></a>Wdrożenie krok po kroku
Poniższe instrukcje krok po kroku dotyczą wdrażania urządzenia StorSimple w centrum danych.

## <a name="step-1-create-a-new-service"></a>Krok 1: Tworzenie nowej usługi
Usługa Menedżer urządzeń StorSimple umożliwia zarządzanie wieloma urządzeniami StorSimple. Wykonaj poniższe kroki, aby utworzyć nowe wystąpienie usługi Menedżer urządzeń StorSimple.

[!INCLUDE [storsimple-8000-create-new-service-gov](../../includes/storsimple-8000-create-new-service-gov.md)]

> [!IMPORTANT]
> Jeśli nie włączono automatycznego tworzenia konta magazynu przy użyciu usługi, po pomyślnym utworzeniu usługi musisz utworzyć co najmniej jedno konto magazynu. To konto magazynu będzie używane podczas tworzenia kontenera woluminu.
> 
> * Jeśli nie utworzono automatycznie konta magazynu, przejdź do kroku [Konfigurowanie nowego konta magazynu dla usługi](#configure-a-new-storage-account-for-the-service) w celu uzyskania szczegółowych informacji.
> * Jeśli włączono automatyczne tworzenie konta magazynu, przejdź do strony [krok 2: Pobierz klucz rejestracji usługi](#step-2-get-the-service-registration-key).


## <a name="step-2-get-the-service-registration-key"></a>Krok 2: Pobieranie klucza rejestracji usługi
Po skonfigurowaniu i uruchomieniu usługi Menedżer urządzeń StorSimple musisz pobrać klucz rejestracji usługi. Ten klucz jest używany do rejestrowania i połączenia urządzenia StorSimple z usługą.

Wykonaj następujące czynności w portalu dla instytucji rządowych.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Krok 3: Konfigurowanie i rejestrowanie urządzenia za pomocą programu Windows PowerShell dla usługi StorSimple
Użyj programu Windows PowerShell dla urządzenia StorSimple do przeprowadzenia konfiguracji początkowej urządzenia StorSimple, jak opisano w poniższej procedurze. Do wykonania tego kroku niezbędne jest użycie oprogramowania do emulacji terminala. Aby uzyskać więcej informacji, zobacz część [Łączenie z konsolą szeregową urządzenia przy użyciu programu PuTTY](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-8000-configure-and-register-device-gov](../../includes/storsimple-8000-configure-and-register-device-gov-u2.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Krok 4: Przeprowadzenie minimalnej konfiguracji urządzenia
Do przeprowadzenia minimalnej konfiguracji urządzenia StorSimple konieczne jest:

* Podaj przyjazną nazwę dla urządzenia.
* Ustaw strefę czasową urządzenia.
* Przypisanie stałych adresów IP do obu kontrolerów.

Wykonaj następujące czynności w portalu Azure dla instytucji rządowych, aby przeprowadzenie minimalnej konfiguracji urządzenia.

[!INCLUDE [storsimple-8000-complete-minimum-device-setup-u2](../../includes/storsimple-8000-complete-minimum-device-setup-u2.md)]

## <a name="step-5-create-a-volume-container"></a>Krok 5. Tworzenie kontenera woluminów
Kontener woluminów obejmuje ustawienia konta magazynu, przepustowości i szyfrowania wszystkich woluminów, które zawiera. Kontener woluminów należy utworzyć przed zainicjowaniem woluminów na urządzeniu StorSimple.

Wykonaj następujące czynności w portalu dla instytucji rządowych, aby utworzyć kontener woluminów.

[!INCLUDE [storsimple-8000-create-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Krok 6: Tworzenie woluminu
Po utworzeniu kontenera woluminów możesz zainicjować obsługę woluminu magazynu w urządzeniu StorSimple dla serwerów. Wykonaj następujące czynności w portalu dla instytucji rządowych, aby utworzyć wolumin.

> [!IMPORTANT]
> Menedżer urządzeń StorSimple można utworzyć tylko woluminy alokowane elastycznie.  Nie można jednak tworzyć woluminów inicjowanych częściowo.

[!INCLUDE [storsimple-8000-create-volume](../../includes/storsimple-8000-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Krok 7: Instalowanie, inicjowanie i formatowanie woluminu
Wykonaj następujące czynności na hoście z systemem Windows Server.

> [!IMPORTANT]
> * W celu zapewnienia dużej dostępności rozwiązania StorSimple warto przed skonfigurowaniem usługi iSCSI skonfigurować wielościeżkowe wejście/wyjście (MPIO) na serwerach hosta (opcjonalnie). Dzięki skonfigurowaniu wielościeżkowego wejścia/wyjścia (MPIO) na serwerach hosta można mieć pewność, że serwery tolerują błędy linków, sieci lub interfejsu.
> * Instrukcje dotyczące instalowania i konfigurowania usługi iSCSI i wielościeżkowego wejścia/wyjścia (MPIO) na hoście systemu Windows Server można znaleźć w temacie [Configure MPIO for your StorSimple device](storsimple-configure-mpio-windows-server.md) (Konfigurowanie wielościeżkowego wejścia/wyjścia dla urządzenia StorSimple). Informacje te dotyczą również kroków opisujących instalowanie, inicjowanie i formatowanie woluminów StorSimple.
> * Instrukcje dotyczące instalowania i konfigurowania usługi iSCSI i wielościeżkowego wejścia/wyjścia (MPIO) na hoście systemu Linux można znaleźć w temacie [Configure MPIO for your StorSimple Linux host](storsimple-configure-mpio-on-linux.md) (Konfigurowanie wielościeżkowego wejścia/wyjścia dla hosta z systemem Linux urządzenia StorSimple).

Jeśli nie chcesz konfigurować wielościeżkowego wejścia/wyjścia (MPIO), wykonaj poniższe kroki, aby zainstalować, zainicjować i sformatować woluminy StorSimple na hoście systemu Windows Server.

[!INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Krok 8: Tworzenie kopii zapasowej
Kopie zapasowe oferują ochronę woluminów w określonym momencie i udoskonalają możliwości odzyskiwania przy równoczesnym zminimalizowaniu czasów przywracania. W urządzeniu StorSimple można wykonywać dwa typy kopii zapasowych: migawki lokalne i migawki w chmurze. Kopie obu typów można tworzyć jako **zaplanowane** lub **ręczne**.

Wykonaj następujące czynności w portalu dla instytucji rządowych, aby utworzyć zaplanowaną kopię zapasową.

[!INCLUDE [storsimple-8000-take-backup](../../includes/storsimple-8000-take-backup.md)]

Kopię zapasową można utworzyć ręcznie w dowolnym momencie. Informacje na temat procedur można znaleźć w części [Ręczne tworzenie kopii zapasowej](#create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-the-service"></a>Konfigurowanie nowego konta magazynu dla usługi
Jest to krok opcjonalny, który należy wykonać tylko, jeśli nie włączono automatycznego tworzenia konta magazynu przy użyciu usługi. Do utworzenia kontenera woluminów StorSimple wymagane jest konto magazynu platformy Microsoft Azure.

Instrukcje krok po kroku dotyczące tworzenia konta usługi Azure Storage w innym regionie można znaleźć w temacie [About Azure Storage Accounts](../storage/common/storage-create-storage-account.md) (Informacje o kontach usługi Azure Storage).

Wykonaj następujące kroki w portalu dla instytucji rządowych na **usługi Menedżer urządzeń StorSimple** strony.

[!INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Łączenie z konsolą szeregową urządzenia przy użyciu programu PuTTY
Aby nawiązać połączenie z programem Windows PowerShell dla urządzenia StorSimple, należy użyć oprogramowania do emulacji terminala, takiego jak PuTTY. Programu PuTTY można używać w przypadku uzyskiwania dostępu do urządzenia bezpośrednio za pośrednictwem konsoli szeregowej lub przez otwarcie sesji telnet z komputera zdalnego.

[!INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Wyszukiwanie aktualizacji i ich stosowanie
Aktualizowanie urządzenia może potrwać kilka godzin. Aby uzyskać szczegółowe instrukcje dotyczące sposobu instalowania najnowszej aktualizacji, przejdź do artykułu [Instalowanie aktualizacji Update 4](storsimple-8000-install-update-4.md).

## <a name="get-the-iqn-of-a-windows-server-host"></a>Pobieranie nazwy IQN hosta z systemem Windows Server
Wykonaj poniższe kroki, aby pobrać kwalifikowaną nazwę iSCSI (IQN) hosta z systemem Windows, na którym uruchomiono system Windows Server® 2012.

[!INCLUDE [Get IQN of your Windows Server host](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Ręczne tworzenie kopii zapasowej
Wykonaj następujące czynności w portalu dla instytucji rządowych, aby na żądanie ręcznie utworzyć kopię zapasową pojedynczego woluminu w urządzeniu StorSimple.

[!INCLUDE [Create a manual backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Kolejne kroki
* Skonfiguruj [urządzenie wirtualne](storsimple-8000-cloud-appliance-u2.md).
* Użyj [usługi Menedżer urządzeń StorSimple](storsimple-8000-manager-service-administration.md) do zarządzania urządzeniem StorSimple.

