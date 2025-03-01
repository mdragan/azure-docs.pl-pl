---
title: Instalowanie usługi Azure IoT Edge w systemie Linux | Dokumentacja firmy Microsoft
description: Instrukcje instalacji usługi Azure IoT Edge na urządzeniach z systemem Linux AMD64 systemem Ubuntu
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 86ca3080229f2a286e8aa4725fe13c40e2a38549
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67054277"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Instalowanie środowiska uruchomieniowego usługi Azure IoT Edge w systemie Linux (x64)

Środowisko uruchomieniowe usługi Azure IoT Edge to, co jest przekształcany urządzenia urządzenia usługi IoT Edge. Środowisko uruchomieniowe można wdrożyć na urządzeniach jako małej, jak Raspberry Pi lub tak duże jak serwer przemysłowe. Gdy urządzenie zostanie skonfigurowany ze środowiskiem uruchomieniowym usługi IoT Edge, możesz rozpocząć wdrażanie logikę biznesową w chmurze.

Aby dowiedzieć się więcej, zobacz [zrozumieć środowisko uruchomieniowe usługi Azure IoT Edge oraz jej architektury](iot-edge-runtime.md).

W tym artykule wymieniono kroki, aby zainstalować środowisko uruchomieniowe usługi Azure IoT Edge w systemie Linux x64 (Intel/AMD) Ubuntu urządzenie usługi IoT Edge. Zapoznaj się [usługi Azure IoT Edge obsługiwane systemy](support.md#operating-systems) Aby uzyskać listę obsługiwanych systemów operacyjnych AMD64.

> [!NOTE]
> Są pakiety w repozytoriach oprogramowania systemu Linux, z zastrzeżeniem postanowień licencyjnych, znajduje się w każdym pakiecie (/ usr/udostępnianie/docs/*nazwy pakietu*). Przeczytaj postanowienia licencyjne przed przy użyciu pakietu. Twoja instalacja i używanie pakietu stanowi zaakceptowania przez korzystającego tych warunków. Jeśli nie zgadzasz się z warunkami licencji, nie należy używać pakietu.

## <a name="register-microsoft-key-and-software-repository-feed"></a>Zarejestruj kanał repozytorium klucza i oprogramowania firmy Microsoft

Przygotuj urządzenie usługi IoT Edge instalacji środowiska uruchomieniowego.


Zainstaluj Konfiguracja repozytorium. Wybierz opcję **16.04** lub **18.04** fragment kodu odpowiednie dla danej wersji systemu Ubuntu:

> [!NOTE]
> Upewnij się, że wybrano fragment kodu z usługi box poprawny kod dla danej wersji systemu Ubuntu.

* Aby uzyskać **Ubuntu 16.04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   ```

* Aby uzyskać **Ubuntu 18.04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
   ```
   
Kopiowanie wygenerowanej listy.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

Zainstaluj klucz publiczny GPG firmy Microsoft

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

## <a name="install-the-container-runtime"></a>Zainstaluj środowisko uruchomieniowe kontenera

Usługa Azure IoT Edge opiera się na [zgodnego z OCI](https://www.opencontainers.org/) kontener środowiska uruchomieniowego. Na potrzeby scenariuszy produkcyjnych, firma Microsoft zaleca się używanie [na podstawie Moby](https://mobyproject.org/) aparatu przedstawione poniżej. Jest aparat tylko kontenera oficjalnie obsługiwana dzięki usłudze Azure IoT Edge. Obrazy usługi docker CE/EE kontenerów są zgodne ze środowiskiem uruchomieniowym Moby.

Należy wykonać aktualizację apt.

   ```bash
   sudo apt-get update
   ```

Zainstaluj aparat Moby.

   ```bash
   sudo apt-get install moby-engine
   ```

Zainstaluj Moby interfejsu wiersza polecenia (CLI). Interfejs wiersza polecenia jest przydatne do tworzenia aplikacji, ale opcjonalny w przypadku wdrożeń produkcyjnych.

   ```bash
   sudo apt-get install moby-cli
   ```

### <a name="verify-your-linux-kernel-for-moby-compatibility"></a>Sprawdź swoje jądra systemu Linux w celu Moby zgodności

Wielu producentów urządzeń osadzonych dostarczane w obrazach urządzeń, które zawierają niestandardowe jądra systemu Linux, które może brakować funkcje wymagane przez kontener środowiska uruchomieniowego zgodności. Jeśli napotkasz problemy podczas instalowania zalecanych [Moby](https://github.com/moby/moby) kontener środowiska uruchomieniowego, można rozwiązywać problemy z systemem Linux jądra konfiguracji przy użyciu [wyboru config](https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh) skryptu podane w Oficjalna [repozytorium Moby Github](https://github.com/moby/moby) , uruchamiając następujące polecenia na urządzeniu.

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

Zapewni to szczegółowych danych wyjściowych, który zawiera stan funkcji jądra, które są używane przez środowisko uruchomieniowe Moby. Należy upewnić się, że wszystkie elementy w obszarze `Generally Necessary` i `Network Drivers` są włączone, aby upewnić się, że Twoje jądra jest w pełni zgodna ze środowiskiem uruchomieniowym Moby.  Po zidentyfikowaniu dowolnej brakujących funkcji mogą umożliwiać ich ponowne tworzenie usługi jądra ze źródła i wybierając skojarzone modułów do włączenia w .config jądra odpowiedni.  Podobnie jeśli używasz jądra generatora konfiguracji, takich jak defconfig lub menuconfig należy znaleźć i włączyć odpowiednie funkcje i odpowiednio odbudować usługi jądra.  Po wdrożeniu nowo zmodyfikowanych jądra, należy uruchomić skrypt konfiguracji wyboru ponownie, aby sprawdzić, czy określonych funkcji została pomyślnie włączona.

## <a name="install-the-azure-iot-edge-security-daemon"></a>Instalowanie demona zabezpieczeń usługi Azure IoT Edge

**Demona zabezpieczeń usługi IoT Edge** dostarcza i utrzymuje standardów zabezpieczeń na urządzeniu usługi IoT Edge. Demon jest uruchamiany w każdym rozruchu i używa do ładowania urządzenia przez uruchomienie pozostałych środowiska uruchomieniowego usługi IoT Edge.

Polecenie instalacji instaluje również standardowej wersji **iothsmlib** Jeśli nie są już zainstalowane.

Należy wykonać aktualizację apt.

   ```bash
   sudo apt-get update
   ```

Instalowanie demona zabezpieczeń. Pakiet jest zainstalowany na `/etc/iotedge/`.

   ```bash
   sudo apt-get install iotedge
   ```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Skonfiguruj demona zabezpieczeń usługi Azure IoT Edge

Konfigurowanie środowiska uruchomieniowego usługi IoT Edge, aby połączyć urządzenia fizycznego przy użyciu tożsamości urządzenia, która znajduje się w usłudze Azure IoT hub.

Demon można skonfigurować przy użyciu pliku konfiguracji na `/etc/iotedge/config.yaml`. Plik jest zabezpieczony przed zapisem domyślnie, może być konieczne podwyższonym poziomem uprawnień, aby go edytować.

Pojedyncze urządzenie usługi IoT Edge mogą być udostępniane ręcznie przy użyciu parametrów połączenia urządzenia dostarczane przez usługę IoT Hub. Alternatywnie można użyć usługi Device Provisioning Service, aby automatycznie aprowizować urządzenia, co jest przydatne, jeśli masz wiele urządzeń do aprowizowania. W zależności od wybranego inicjowania obsługi administracyjnej wybierz skrypt instalacyjny odpowiedniego.

### <a name="option-1-manual-provisioning"></a>Opcja 1: Aprowizacja ręczna

Aby ręcznie zainicjować obsługę urządzeń, należy podać go [parametry połączenia urządzenia](how-to-register-device-portal.md) , możesz utworzyć przez zarejestrowanie nowego urządzenia w usłudze IoT hub.

Otwórz plik konfiguracji.

```bash
sudo nano /etc/iotedge/config.yaml
```

Znajdź inicjowania obsługi administracyjnej części pliku. Usuń znaczniki komentarza **ręczne** tryb obsługi administracyjnej i upewnij się, punktu dystrybucji, tryb obsługi administracyjnej jest opatrzona komentarzem. Zaktualizuj wartość **device_connection_string** przy użyciu parametrów połączenia z urządzenia usługi IoT Edge.

   ```yaml
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # provisioning:
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   registration_id: "{registration_id}"
   ```

Zapisz i zamknij plik.

   `CTRL + X`, `Y`, `Enter`

Po wprowadzeniu informacji o udostępnianiu w pliku konfiguracji, należy ponownie uruchomić demona:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Opcja 2: Automatyczna aprowizacja

Aby automatycznie aprowizować urządzenia [Konfigurowanie usługi Device Provisioning i pobrania swój identyfikator rejestracji urządzenia](how-to-auto-provision-simulated-device-linux.md). Automatyczna aprowizacja działa tylko z urządzeń, które mają mikroukładu Trusted Platform Module (TPM). Na przykład urządzenia Raspberry Pi nie pochodzą z modułem TPM domyślnie.

Otwórz plik konfiguracji.

```bash
sudo nano /etc/iotedge/config.yaml
```

Znajdź inicjowania obsługi administracyjnej części pliku. Usuń znaczniki komentarza **dps** inicjowania obsługi trybu ujmij w komentarz ręczne części. Zaktualizuj wartości **scope_id** i **registration_id** wartościami z usługi IoT Hub Device Provisioning Service i urządzenia usługi IoT Edge przy użyciu modułu TPM.

   ```yaml
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   provisioning:
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     registration_id: "{registration_id}"
   ```

Zapisz i zamknij plik.

   `CTRL + X`, `Y`, `Enter`

Po wprowadzeniu informacji o udostępnianiu w pliku konfiguracji, należy ponownie uruchomić demona:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Sprawdź pomyślnej instalacji

Jeśli użyto **ręcznej konfiguracji** kroki opisane w poprzedniej sekcji, środowisko uruchomieniowe usługi IoT Edge należy pomyślnie aprowizacja i uruchomienie na urządzeniu. Jeśli użyto **automatycznej konfiguracji** kroki należy wykonać kilka dodatkowych kroków, środowisko uruchomieniowe można zarejestrować urządzenia za pomocą usługi IoT hub w Twoim imieniu. Dalsze czynności, zobacz [tworzenie i aprowizowanie symulowanego urządzenia TPM IoT Edge na maszynie wirtualnej systemu Linux](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

Możesz sprawdzić stan, IoT Edge demona przy użyciu:

```bash
systemctl status iotedge
```

Sprawdź dzienniki demona przy użyciu:

```bash
journalctl -u iotedge --no-pager --no-full
```

I uruchomione moduły z listy:

```bash
sudo iotedge list
```

## <a name="tips-and-suggestions"></a>Porady i sugestie

Uruchomienie poleceń `iotedge` wymaga podniesionych uprawnień. Po zainstalowaniu środowiska uruchomieniowego, wyloguj się z komputera, a następnie zaloguj się ponownie na automatyczne aktualizowanie uprawnień. W międzyczasie użyj **"sudo"** przed dowolnego `iotedge` polecenia.

Na urządzeniach z zasobu ograniczone, zdecydowanie zaleca się ustawienie *OptimizeForPerformance* zmiennej środowiskowej, aby *false* zgodnie z instrukcjami w [przewodnik rozwiązywania problemów ](troubleshoot.md).

Jeśli sieci, na którym działa serwer proxy, wykonaj kroki opisane w [skonfigurować urządzenie usługi IoT Edge do komunikowania się za pośrednictwem serwera proxy](how-to-configure-proxy-support.md).

## <a name="uninstall-iot-edge"></a>Odinstalowywanie usługi IoT Edge

Jeśli chcesz usunąć instalację usługi IoT Edge na urządzeniu z systemem Linux, użyj następujących poleceń w wierszu polecenia.

Usuń środowisko uruchomieniowe usługi IoT Edge.

```bash
sudo apt-get remove --purge iotedge
```

Po usunięciu środowiska uruchomieniowego usługi IoT Edge kontenera, do którego utworzona zostaną zatrzymane, ale nadal istnieje na urządzeniu. Wyświetl wszystkie kontenery, aby zobaczyć, które pozostają.

```bash
sudo docker ps -a
```

Usuwanie kontenerów na urządzeniu, w tym dwóch kontenerów środowiska wykonawczego.

```bash
sudo docker rm -f <container name>
```

Na koniec usunąć kontener środowiska uruchomieniowego na urządzeniu.

```bash
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>Kolejne kroki

Teraz, gdy masz urządzenia usługi IoT Edge zaopatrzony zainstalowanego środowiska uruchomieniowego, można [wdrożyć moduły usługi IoT Edge](how-to-deploy-modules-portal.md).

Jeśli problemy ze środowiskiem uruchomieniowym usługi IoT Edge instalacji prawidłowo, zapoznaj się z [Rozwiązywanie problemów z](troubleshoot.md) strony.

Aby zaktualizować istniejącą instalację do najnowszej wersji usługi IoT Edge, zobacz [aktualizacji demona zabezpieczeń usługi IoT Edge i środowisko uruchomieniowe](how-to-update-iot-edge.md).
