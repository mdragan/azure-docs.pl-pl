---
title: 'Szybki start: Niestandardowe głosu pierwszego wirtualnego Asystenta ustawień (wersja zapoznawcza) języka Java (Android) — usług przetwarzania mowy'
titleSuffix: Azure Cognitive Services
description: Dowiedz się, jak utworzyć aplikację Asystenta wirtualnych pierwszej głosu w języku Java w systemie Android przy użyciu zestawu SDK rozpoznawania mowy
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 5/24/2019
ms.author: travisw
ms.openlocfilehash: 7e82b2ef9500defe0d08351da1e3487e4671155f
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467046"
---
# <a name="quickstart-create-a-voice-first-virtual-assistant-in-java-on-android-by-using-the-speech-sdk"></a>Szybki start: Utwórz Asystenta ustawień wirtualnego pierwszy głosu w języku Java w systemie Android przy użyciu zestawu SDK rozpoznawania mowy

Szybki Start jest również dostępna dla [mowy na tekst](quickstart-java-android.md).

W tym artykule, będziesz tworzyć głosu pierwszego wirtualnego Asystenta ustawień za pomocą języka Java dla systemu Android przy użyciu [zestaw SDK rozpoznawania mowy](speech-sdk.md). Ta aplikacja połączy się botów, które zostały już utworzone i skonfigurowane za pomocą [kanału Direct mowy wiersza](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech). Następnie wyśle żądanie głosu do robota i przedstawia działanie odpowiedzi z obsługą głosu.

Ta aplikacja jest tworzone za pomocą narzędzia Maven zestaw SDK rozpoznawania mowy pakiet i program Android Studio 3.3. Zestaw Speech SDK jest obecnie zgodny z urządzeniami z systemem Android oraz 32 lub 64-bitowymi procesorami ARM i procesorami x86 lub x64 firmy Intel.

> [!NOTE]
> Aby uzyskać informacje dotyczące zestawu Speech Devices SDK oraz urządzenia Roobo, zobacz [Speech Devices SDK](speech-devices-sdk.md).

## <a name="prerequisites"></a>Wymagania wstępne

* Klucz subskrypcji platformy Azure dla usług przetwarzania mowy. [Bezpłatnie Uzyskaj](get-started.md) lub utwórz go na [witryny Azure portal](https://portal.azure.com).
* Robota utworzonej wcześniej skonfigurowano [kanału Direct mowy wiersza](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)
* [Program android Studio](https://developer.android.com/studio/) v3.3 lub nowszej

    > [!NOTE]
    > Bezpośrednie wiersza Speech (wersja zapoznawcza) jest obecnie dostępna w regionach usług przetwarzania mowy przez. Zapoznaj się [listę obsługiwanych regionów dla wirtualnych asystentów głosowych pierwszy](regions.md#Voice-first virtual assistants) i upewnij się, Twoje zasoby są wdrażane w jednym z tych regionów.

## <a name="create-and-configure-a-project"></a>Tworzenie i konfigurowanie projektu

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-android-create-proj.md)]

## <a name="create-user-interface"></a>Tworzenie interfejsu użytkownika

W tej sekcji utworzymy podstawowy interfejs użytkownika (UI) dla aplikacji. Zacznijmy, otwierając w głównym działaniu: `activity_main.xml`. Podstawowy szablon zawiera pasek tytułu o nazwie aplikacji i `TextView` komunikat "Hello world!".

Następnie zastąp zawartość `activity_main.xml` następującym kodem:

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:onClick="onBotButtonClicked"
        android:text="Talk to your bot" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Recognition Data"
        android:textSize="18dp"
        android:textStyle="bold" />

    <TextView
        android:id="@+id/recoText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="  \n(Recognition goes here)\n" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Activity Data"
        android:textSize="18dp"
        android:textStyle="bold" />

    <TextView
        android:id="@+id/activityText"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scrollbars="vertical"
        android:text="  \n(Activities go here)\n" />

   </LinearLayout>
   ```

Plik XML definiuje prosty interfejs użytkownika do interakcji z botem.

* `button` Element inicjuje interakcji i wywołuje `onBotButtonClicked` metoda po kliknięciu.
* `recoText` Elementu wyświetli wyniki mowy na tekst zwróć się do bota.
* `activityText` Element będzie wyświetlany ładunek JSON dla ostatnią aktywność platformy Bot Framework z botem.

Tekst i graficzna reprezentacja interfejsu użytkownika powinny teraz wyglądać następująco:

![](media/sdk/qs-java-android-assistant-designer-ui.png)

## <a name="add-sample-code"></a>Dodawanie przykładowego kodu

1. Otwórz `MainActivity.java`i zastąp jego zawartość następującym kodem:

   ```java
    package samples.speech.cognitiveservices.microsoft.com;

    import android.media.AudioFormat;
    import android.media.AudioManager;
    import android.media.AudioTrack;
    import android.support.v4.app.ActivityCompat;
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import android.text.method.ScrollingMovementMethod;
    import android.view.View;
    import android.widget.TextView;

    import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
    import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;
    import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConfig;
    import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConnector;

    import org.json.JSONException;
    import org.json.JSONObject;

    import static android.Manifest.permission.*;

    public class MainActivity extends AppCompatActivity {
        // Replace below with your bot's own Direct Line Speech channel secret
        private static String channelSecret = "YourChannelSecret";
        // Replace below with your own speech subscription key
        private static String speechSubscriptionKey = "YourSpeechSubscriptionKey";
        // Replace below with your own speech service region (note: only a subset of regions are currently supported)
        private static String serviceRegion = "YourSpeechServiceRegion";

        private DialogServiceConnector connector;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            TextView recoText = (TextView) this.findViewById(R.id.recoText);
            TextView activityText = (TextView) this.findViewById(R.id.activityText);
            recoText.setMovementMethod(new ScrollingMovementMethod());
            activityText.setMovementMethod(new ScrollingMovementMethod());

            // Note: we need to request permissions for audio input and network access
            int requestCode = 5; // unique code for the permission request
            ActivityCompat.requestPermissions(MainActivity.this, new String[]{RECORD_AUDIO, INTERNET}, requestCode);
        }

        public void onBotButtonClicked(View v) {
            // Recreate the DialogServiceConnector on each button press, ensuring that the existing one is closed
            if (connector != null) {
                connector.close();
                connector = null;
            }

            // Create the DialogServiceConnector from the channel and speech subscription information
            DialogServiceConfig config = DialogServiceConfig.fromBotSecret(channelSecret, speechSubscriptionKey, serviceRegion);
            connector = new DialogServiceConnector(config, AudioConfig.fromDefaultMicrophoneInput());

            // Optional step: preemptively connect to reduce first interaction latency
            connector.connectAsync();

            // Register the DialogServiceConnector's event listeners
            registerEventListeners();

            // Begin sending audio to your bot
            connector.listenOnceAsync();
        }

        private void registerEventListeners() {
            TextView recoText = (TextView) this.findViewById(R.id.recoText); // 'recoText' is the ID of your text view
            TextView activityText = (TextView) this.findViewById(R.id.activityText); // 'activityText' is the ID of your text view

            // Recognizing will provide the intermediate recognized text while an audio stream is being processed
            connector.recognizing.addEventListener((o, recoArgs) -> {
                recoText.setText("  Recognizing: " + recoArgs.getResult().getText());
            });

            // Recognized will provide the final recognized text once audio capture is completed
            connector.recognized.addEventListener((o, recoArgs) -> {
                recoText.setText("  Recognized: " + recoArgs.getResult().getText());
            });

            // SessionStarted will notify when audio begins flowing to the service for a turn
            connector.sessionStarted.addEventListener((o, sessionArgs) -> {
                recoText.setText("Listening...");
            });

            // SessionStopped will notify when a turn is complete and it's safe to begin listening again
            connector.sessionStopped.addEventListener((o, sessionArgs) -> {
            });

            // Canceled will be signaled when a turn is aborted or experiences an error condition
            connector.canceled.addEventListener((o, canceledArgs) -> {
                recoText.setText("Canceled (" + canceledArgs.getReason().toString() + ") error details: {}" + canceledArgs.getErrorDetails());
                connector.disconnectAsync();
            });

            // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities.
            connector.activityReceived.addEventListener((o, activityArgs) -> {
                try {
                    // Here we use JSONObject only to "pretty print" the condensed Activity JSON
                    String rawActivity = activityArgs.getActivity().serialize();
                    String formattedActivity = new JSONObject(rawActivity).toString(2);
                    activityText.setText(formattedActivity);
                } catch (JSONException e) {
                    activityText.setText("Couldn't format activity text: " + e.getMessage());
                }

                if (activityArgs.hasAudio()) {
                    // Text-to-speech audio associated with the activity is 16 kHz 16-bit mono PCM data
                    final int sampleRate = 16000;
                    int bufferSize = AudioTrack.getMinBufferSize(sampleRate, AudioFormat.CHANNEL_OUT_MONO, AudioFormat.ENCODING_PCM_16BIT);

                    AudioTrack track = new AudioTrack(
                            AudioManager.STREAM_MUSIC,
                            sampleRate,
                            AudioFormat.CHANNEL_OUT_MONO,
                            AudioFormat.ENCODING_PCM_16BIT,
                            bufferSize,
                            AudioTrack.MODE_STREAM);

                    track.play();

                    PullAudioOutputStream stream = activityArgs.getAudio();

                    // Audio is streamed as it becomes available. Play it as it arrives.
                    byte[] buffer = new byte[bufferSize];
                    long bytesRead = 0;

                    do {
                        bytesRead = stream.read(buffer);
                        track.write(buffer, 0, (int) bytesRead);
                    } while (bytesRead == bufferSize);

                    track.release();
                }
            });
        }
    }
   ```

   * `onCreate` Zawiera kod, który żąda uprawnienia do mikrofonu i internet.

   * Metoda `onBotButtonClicked` oznacza, jak wspomniano wcześniej, procedurę obsługi naciskania przycisku. Naciśnięcie przycisku wyzwala pojedynczej interakcji ("Włącz") z botem.

   * `registerEventListeners` Metoda pokazuje zdarzenia używane przez `DialogServiceConnector` i obsługa podstawowych działań przychodzących.

1. W tym samym pliku Zastąp ciągi konfiguracji, aby dopasować swoje zasoby:

    * Zastąp `YourChannelSecret` z kluczem tajnym kanału Direct mowy wiersza dla bota.

    * Zastąp element `YourSpeechSubscriptionKey` kluczem subskrypcji.

    * Zastąp `YourServiceRegion` z [region](regions.md) skojarzonych z Twoją subskrypcją tylko podzbiór usług przetwarzania mowy regiony są obecnie obsługiwane za pomocą bezpośredniego mowy wiersza. Aby uzyskać więcej informacji, zobacz [regionów](regions.md#voice-first-virtual-assistants).

## <a name="build-and-run-the-app"></a>Kompilowanie i uruchamianie aplikacji

1. Połącz urządzenie z systemem Android do komputera projektowego. Upewnij się, że na urządzeniu włączono [tryb projektowania i debugowanie USB](https://developer.android.com/studio/debug/dev-options).

1. Aby skompilować aplikację, naciśnij klawisze Ctrl + F9 lub wybierz na pasku menu pozycje **Build** > **Make Project** (Kompilacja) (Utwórz projekt).

1. Aby uruchomić aplikację, naciśnij klawisze Shift + F10 lub wybierz pozycje **Run** > **Run 'app'** (Uruchom) (Uruchom „aplikację”).

1. W oknie docelowym wdrożenia, które zostanie wyświetlone, wybierz urządzenie Android.

   ![Zrzut ekranu okna Deployment Target (Cel wdrożenia)](media/sdk/qs-java-android-12-deploy.png)

Po wprowadzili aplikacji i jej działanie, kliknij przycisk aby rozpocząć, rozmawiając z botem. Uzyskanego tekstu pojawi się jako użytkownik głosu i ostatnią aktywność miał odebranych z usługi bot będzie wyświetlana po odebraniu. Jeśli Twój bot jest skonfigurowany do zapewniania prowadzone odpowiedzi, automatycznie odtwarza zamiany mowy na tekst.

![Zrzut ekranu aplikacji Android](media/sdk/qs-java-android-assistant-completed-turn.png)

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Tworzenie i wdrażanie podstawowy bot](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)

## <a name="see-also"></a>Zobacz także
- [Temat wirtualnych asystentów głosowych pierwszy](voice-first-virtual-assistants.md)
- [Bezpłatnie Uzyskaj klucz subskrypcji usług przetwarzania mowy](get-started.md)
- [Wyrazy niestandardowego](speech-devices-sdk-create-kws.md)
- [Nawiązywanie bezpośredniego mowy wiersza bota](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)
- [Zapoznaj się z przykładami dla języka Java w usłudze GitHub](https://aka.ms/csspeech/samples)
