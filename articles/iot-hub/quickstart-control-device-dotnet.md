---
title: Kontrolowanie urządzenia z poziomu usługi Azure IoT Hub — Szybki start (.NET) | Microsoft Docs
description: W tym przewodniku Szybki start uruchomisz dwie przykładowe aplikacje C#. Jedna z aplikacji to aplikacja zaplecza, która może zdalnie kontrolować urządzenia podłączone do centrum. Druga z aplikacji symuluje urządzenie podłączone do centrum, które można kontrolować zdalnie.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/21/2019
ms.openlocfilehash: 599bc440bed4781d081f41cbc64bfb944f423e94
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330729"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-net"></a>Szybki start: Kontrolowanie urządzenia podłączonego do centrum IoT (.NET)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub to usługa platformy Azure, która umożliwia pozyskiwanie dużych ilości danych telemetrycznych z urządzeń IoT do chmury i zarządzanie urządzeniami z poziomu chmury. W tym przewodniku Szybki start użyjesz *metody bezpośredniej* do kontrolowania urządzenia symulowanego podłączonego do centrum IoT. Możesz użyć metod bezpośrednich, aby zdalnie zmieniać zachowanie urządzenia podłączonego do centrum IoT.

Przewodnik Szybki start używa dwóch wstępnie napisanych aplikacji .NET:

* Aplikacja urządzenia symulowanego, która reaguje na metody bezpośrednie wywoływane z aplikacji zaplecza. Aby odbierać wywołania metod bezpośrednich, ta aplikacja łączy się z punktem końcowym właściwym dla urządzenia w centrum IoT.

* Aplikacja zaplecza, która wywołuje metody bezpośrednie na urządzeniu symulowanym. Aby wywoływać metody bezpośrednie na urządzeniu, ta aplikacja łączy się z punktem końcowym po stronie usługi w centrum IoT.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Wymagania wstępne

Dwie przykładowe aplikacje uruchamiane w tym przewodniku Szybki start zostały napisane w języku C#. Potrzebujesz zestawu SDK .NET Core w wersji 2.1.0 lub nowszej na komputerze deweloperskim.

Możesz pobrać zestaw SDK .NET Core dla wielu platform z repozytorium [.NET](https://www.microsoft.com/net/download/all).

Możesz sprawdzić bieżącą wersję języka C# na komputerze deweloperskim przy użyciu następującego polecenia:

```cmd/sh
dotnet --version
```

Uruchom następujące polecenie, aby dodać rozszerzenia usługi Microsoft Azure IoT dla interfejsu wiersza polecenia platformy Azure do swojego wystąpienia usługi Cloud Shell. Rozszerzenia IOT dodaje określone polecenia usługi IoT Hub, IoT Edge i usługi aprowizacji urządzeń IoT (DPS) do wiersza polecenia platformy Azure.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Jeśli nie zostało to jeszcze zrobione, pobierz przykładowy projekt C# z https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip i wyodrębnij archiwum ZIP.

## <a name="create-an-iot-hub"></a>Tworzenie centrum IoT Hub

Jeśli ukończono poprzedni przewodnik [Szybki start: wysyłanie danych telemetrycznych z urządzenia do centrum IoT](quickstart-send-telemetry-dotnet.md), możesz pominąć ten krok.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Rejestrowanie urządzenia

Jeśli ukończono poprzedni przewodnik [Szybki start: wysyłanie danych telemetrycznych z urządzenia do centrum IoT](quickstart-send-telemetry-dotnet.md), możesz pominąć ten krok.

Zanim urządzenie będzie mogło nawiązać połączenie, należy je najpierw zarejestrować w centrum IoT. W tym przewodniku Szybki start opisano rejestrowanie urządzenia symulowanego przy użyciu usługi Azure Cloud Shell.

1. Uruchom następujące polecenie w usłudze Azure Cloud Shell do tworzenia tożsamości urządzenia.

   **YourIoTHubName**: zamień ten symbol zastępczy poniżej na wybraną nazwę centrum IoT Hub.

   **MyDotnetDevice**: nazwa rejestrowanego urządzenia. Użyj nazwy **MyDotnetDevice** w pokazany sposób. Jeśli wybierzesz inną nazwę dla swojego urządzenia, musisz używać tej nazwy w tym artykule oraz zaktualizować nazwę urządzenia w przykładowych aplikacjach przed ich uruchomieniem.

    ```azurecli-interactive
    az iot hub device-identity create \
      --hub-name YourIoTHubName --device-id MyDotnetDevice
    ```

2. Uruchom następujące polecenia w usłudze Azure Cloud Shell, aby uzyskać _parametry połączenia urządzenia_ dla urządzenia, które właśnie zostało zarejestrowane:

   **YourIoTHubName**: zamień ten symbol zastępczy poniżej na wybraną nazwę centrum IoT Hub.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name YourIoTHubName \
      --device-id MyDotnetDevice 
      --output table
    ```

    Zanotuj parametry połączenia urządzenia, które wyglądają następująco:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Użyjesz tej wartości w dalszej części tego przewodnika Szybki start.

## <a name="retrieve-the-service-connection-string"></a>Pobieranie parametrów połączenia usługi

Potrzebne będą także _parametry połączenia usługi_ centrum IoT, aby umożliwić aplikacji zaplecza nawiązywanie połączenia z centrum i pobieranie komunikatów. Następujące polecenie pobiera parametry połączenia usługi dla centrum IoT:

```azurecli-interactive
az iot hub show-connection-string --name YourIoTHubName --policy-name service --output table
```

Zanotuj parametry połączenia usługi, które wyglądają następująco:

   `HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

Użyjesz tej wartości w dalszej części tego przewodnika Szybki start. Parametry połączenia usługi różnią się od parametrów połączenia urządzenia.  

## <a name="listen-for-direct-method-calls"></a>Nasłuchiwanie wywołań metod bezpośrednich

Aplikacja urządzenia symulowanego łączy się z punktem końcowym właściwym dla urządzenia w centrum IoT, wysyła symulowane dane telemetryczne i nasłuchuje wywołań metod bezpośrednich z centrum. W tym przewodniku Szybki start wywołanie metody bezpośredniej z centrum nakazuje urządzeniu zmienić interwał wysyłania danych telemetrycznych. Urządzenie symulowane wysyła potwierdzenie z powrotem do centrum po wykonaniu metody bezpośredniej.

1. W lokalnym oknie terminalu przejdź do folderu głównego przykładowego projektu C#. Następnie przejdź do folderu **iot-hub\Quickstarts\simulated-device-2**.

2. Otwórz plik **SimulatedDevice.cs** w wybranym edytorze.

    Zastąp wartość zmiennej `s_connectionString` parametrami połączenia urządzenia zanotowanymi w poprzednim kroku. Następnie zapisz zmiany w pliku **SimulatedDevice.cs**.

3. W lokalnym oknie terminalu uruchom następujące polecenia, aby zainstalować wymagane pakiety dla aplikacji urządzenia symulowanego:

    ```cmd/sh
    dotnet restore
    ```

4. W lokalnym oknie terminalu uruchom następujące polecenia, aby utworzyć i uruchomić aplikację urządzenia symulowanego:

    ```cmd/sh
    dotnet run
    ```

    Poniższy zrzut ekranu przedstawia dane wyjściowe w momencie wysyłania przez aplikację urządzenia symulowanego danych telemetrycznych do centrum IoT:

    ![Uruchamianie urządzenia symulowanego](./media/quickstart-control-device-dotnet/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>Wywoływanie metody bezpośredniej

Aplikacja zaplecza łączy się z punktem końcowym po stronie usługi w usłudze IoT Hub. Aplikacja wykonuje wywołania metod bezpośrednich do urządzenia za pośrednictwem centrum IoT oraz nasłuchuje potwierdzeń. Aplikacja zaplecza usługi IoT Hub zwykle działa w chmurze.

1. W innym lokalnym oknie terminalu przejdź do folderu głównego przykładowego projektu C#. Następnie przejdź do folderu **iot-hub\Quickstarts\back-end-application**.

2. Otwórz plik **BackEndApplication.cs** w wybranym edytorze tekstów.

    Zastąp wartość zmiennej `s_connectionString` parametrami połączenia usługi zanotowanymi w poprzednim kroku. Następnie zapisz zmiany w pliku **BackEndApplication.cs**.

3. W lokalnym oknie terminalu uruchom następujące polecenia, aby zainstalować wymagane biblioteki dla aplikacji zaplecza:

    ```cmd/sh
    dotnet restore
    ```

4. W lokalnym oknie terminalu uruchom następujące polecenia, aby utworzyć i uruchomić aplikację zaplecza:

    ```cmd/sh
    dotnet run
    ```

    Poniższy zrzut ekranu przedstawia dane wyjściowe w sytuacji, w której aplikacja wykonuje wywołanie metody bezpośredniej do urządzenia i odbiera potwierdzenie:

    ![Uruchamianie aplikacji zaplecza](./media/quickstart-control-device-dotnet/BackEndApplication.png)

    Po uruchomieniu aplikacji zaplecza zobaczysz komunikat w oknie konsoli uruchomionym na urządzeniu symulowanym, a także zobaczysz, że zmienia się prędkość wysyłania komunikatów:

    ![Zmiana w kliencie symulowanym](./media/quickstart-control-device-dotnet/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Kolejne kroki

W tym przewodniku Szybki start wywołano metodę bezpośrednią na urządzeniu z aplikacji zaplecza oraz odpowiedziano na wywołanie metody bezpośredniej w aplikacji urządzenia symulowanego.

Aby dowiedzieć się, jak przekierowywać komunikaty urządzenie-chmura do innych miejsc docelowych w chmurze, przejdź do następnego samouczka.

> [!div class="nextstepaction"]
> [Samouczek: przekierowywanie danych telemetrycznych do innych punktów końcowych w celu przetwarzania](tutorial-routing.md)