---
title: 'Samouczek: Tworzenie aplikacji do tłumaczenia przy użyciu platformy WPF i języka C# — interfejs API tłumaczenia tekstu w usłudze Translator'
titleSuffix: Azure Cognitive Services
description: W tym samouczku utworzysz aplikację platformy Windows Presentation Foundation (WPF), która korzysta z interfejsu API usług Cognitive Services na potrzeby tłumaczenia tekstu, wykrywania języka i sprawdzania pisowni, przy użyciu klucza pojedynczej subskrypcji. W tym ćwiczeniu dowiesz się, jak korzystać z funkcji interfejsu API tłumaczenia tekstu w usłudze Translator i interfejsu API sprawdzania pisowni Bing.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: tutorial
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: b300c40b4a9c832a0df87f7cfc6e6a9558d766f6
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448227"
---
# <a name="tutorial-create-a-translation-app-with-wpf"></a>Samouczek: Tworzenie aplikacji do tłumaczenia przy użyciu platformy WPF

W tym samouczku utworzysz aplikację platformy [Windows Presentation Foundation (WPF)](https://docs.microsoft.com/visualstudio/designers/getting-started-with-wpf?view=vs-2017), która korzysta z usług Azure Cognitive Services na potrzeby tłumaczenia tekstu, wykrywania języka i sprawdzania pisowni, przy użyciu klucza pojedynczej subskrypcji. W szczególności aplikacja będzie wywoływać interfejsy API tłumaczenia tekstu w usłudze Translator i [sprawdzania pisowni Bing](https://azure.microsoft.com/services/cognitive-services/spell-check/).

Co to jest platforma WPF? Jest struktura interfejsu użytkownika służąca do tworzenia aplikacji klienckich dla komputerów. Platforma programistyczna WPF obsługuje szeroką gamę funkcji tworzenia aplikacji, w tym model aplikacji, zasoby, kontrolki, grafiki, układ, powiązanie danych, dokumenty i zabezpieczenia. Jest podzbiorem programu .NET Framework, więc jeśli wcześniej tworzono aplikacje przy użyciu programu .NET Framework za pomocą platformy ASP.NET lub Windows Forms, środowisko programowania powinno być znajome. Platforma WPF przy użyciu języka Extensible Application Markup Language (XAML) zapewnia model deklaratywny na potrzeby programowania aplikacji, który zostanie omówiony w następnych sekcjach.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie projektu WPF w programie Visual Studio
> * Dodawanie pakietów NuGet do projektu
> * Tworzenie interfejsu użytkownika aplikacji przy użyciu języka XAML
> * Pobieranie języków, tłumaczenie tekstu i wykrywanie języka źródłowego przy użyciu interfejsu API tłumaczenia tekstu w usłudze Translator
> * Weryfikowanie danych wejściowych i zwiększanie dokładności tłumaczeń przy użyciu interfejsu API sprawdzania pisowni Bing
> * Uruchamianie aplikacji WPF

### <a name="cognitive-services-used-in-this-tutorial"></a>Usługi Cognitive Services używane w tym samouczku

Ta lista zawiera usługi Cognitive Services używane w tym samouczku. Użyj linku, aby przejrzeć dokumentację interfejsu API dla każdej funkcji.

| Usługa | Cecha | Opis |
|---------|---------|-------------|
| Tekst usługi Translator | [Pobieranie języków](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages) | Pobierz pełną listę obsługiwanych języków na potrzeby tłumaczenia tekstu. |
| Tekst usługi Translator | [Tłumaczenie](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate) | Przetłumacz tekst na ponad 60 języków. |
| Tekst usługi Translator | [Wykrywanie](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-detect) | Wykryj język tekstu wejściowego. Obejmuje współczynnik ufności dla wykrywania. |
| Sprawdzanie pisowni Bing | [Sprawdzanie pisowni](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference) | Popraw błędy pisowni w celu zwiększenia dokładności tłumaczenia. |

## <a name="prerequisites"></a>Wymagania wstępne

Przed kontynuowaniem potrzebne będą następujące elementy:

* Subskrypcja usług Azure Cognitive Services. [Uzyskaj klucz usług Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#multi-service-subscription).
* Maszyna z systemem Windows
* [Program Visual Studio 2017](https://www.visualstudio.com/downloads/) — Community lub Enterprise

> [!NOTE]
> Na potrzeby tego samouczka zalecamy utworzenie subskrypcji w regionie Zachodnie stany USA. W przeciwnym podczas wykonywania ćwiczenia konieczna będzie zmiana punktów końcowych i regionów w kodzie.  

## <a name="create-a-wpf-app-in-visual-studio"></a>Tworzenie aplikacji WPF w programie Visual Studio

Pierwszą rzeczą, jaką należy wykonać, jest skonfigurowanie projektu w programie Visual Studio.

1. Otwórz program Visual Studio. Następnie wybierz pozycję **Plik > Nowy > Projekt**.
2. W panelu po lewej stronie znajdź i wybierz pozycję **Visual C#** . Następnie wybierz pozycję **Aplikacja WPF (.NET Framework)** w środkowym panelu.
   ![Tworzenie aplikacji WPF w programie Visual Studio](media/create-wpf-project-visual-studio.png)
3. Nazwij swój projekt, ustaw wersję platformy na **.NET Framework 4.5.2 lub nowszą**, a następnie kliknij przycisk **OK**.
4. Projekt został utworzony. Zauważysz, że otwarte są dwie karty: `MainWindow.xaml` i `MainWindow.xaml.cs`. W tym samouczku będziemy dodawać kod do tych dwóch plików. Pierwszy dotyczy interfejsu użytkownika aplikacji; drugi dotyczy wywołań tłumaczenia tekstu w usłudze Translator i sprawdzania pisowni Bing.
   ![Przeglądanie środowiska](media/blank-wpf-project.png)

W następnej sekcji dodamy do projektu zestawy i pakiet NuGet na potrzeby dodatkowych funkcji takich jak analiza JSON.

## <a name="add-references-and-nuget-packages-to-your-project"></a>Dodawanie odwołań i pakietów NuGet do projektu

Nasz projekt wymaga kilku zestawów .NET Framework i pakietu NewtonSoft.Json, które zainstalujemy przy użyciu menedżera pakietów NuGet.

### <a name="add-net-framework-assemblies"></a>Dodawanie zestawów .NET Framework

Dodajmy do naszego projektu zestawy na potrzeby serializacji i deserializacji obiektów oraz zarządzania żądaniami i odpowiedziami HTTP.

1. Znajdź swój projekt w Eksploratorze rozwiązań programu Visual Studio (panel po prawej stronie). Kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj > Odwołanie**, co spowoduje otwarcie **Menedżera odwołań**.
   ![Dodawanie odwołań do zestawów](media/add-assemblies-sample.png)
2. Karta zestawów zawiera listę wszystkich zestawów .NET Framework, do których można się odwoływać. Użyj paska wyszukiwania w prawym górnym rogu ekranu, aby wyszukać te odwołania i dodać je do projektu:
   * [System.Runtime.Serialization](https://docs.microsoft.com/dotnet/api/system.runtime.serialization)
   * [System.Web](https://docs.microsoft.com/dotnet/api/system.web)
   * [System.Web.Extensions](https://docs.microsoft.com/dotnet/api/system.web)
3. Po dodaniu tych odwołań do projektu możesz kliknąć przycisk **OK**, aby zamknąć **Menedżera odwołań**.

> [!NOTE]
> Jeśli chcesz dowiedzieć się więcej na temat odwołań do zestawów, zobacz [Instrukcje: dodawanie lub usuwanie odwołań za pomocą Menedżera odwołań](https://docs.microsoft.com/visualstudio/ide/how-to-add-or-remove-references-by-using-the-reference-manager?view=vs-2017).

### <a name="install-newtonsoftjson"></a>Instalowanie pakietu Newtonsoft.Json

Nasza aplikacja zdeserializuje obiekty JSON przy użyciu pakietu NewtonSoft.Json. Wykonaj te instrukcje, aby zainstalować pakiet.

1. Znajdź swój projekt w Eksploratorze rozwiązań programu Visual Studio i kliknij go prawym przyciskiem myszy. Wybierz pozycję **Zarządzaj pakietami NuGet...**
2. Znajdź i wybierz kartę **Przeglądaj**.
3. Wpisz [NewtonSoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) na pasku wyszukiwania.
   ![Znajdowanie i instalowanie pakietu NewtonSoft.Json](media/add-nuget-packages.png)
4. Zaznacz pakiet i kliknij pozycję **Instaluj**.
5. Po zakończeniu instalacji zamknij kartę.

## <a name="create-a-wpf-form-using-xaml"></a>Tworzenie formularza WPF przy użyciu języka XAML

Aby móc korzystać z aplikacji, potrzebny będzie interfejs użytkownika. Przy użyciu języka XAML utworzymy formularz, który umożliwi użytkownikom wybieranie języków wejściowych i języków tłumaczenia, wprowadzanie tekstu do tłumaczenia oraz wyświetlanie danych wyjściowych tłumaczenia.

Zobaczmy, co będziemy tworzyć.

![Interfejs użytkownika WPF XAML](media/translator-text-csharp-xaml.png)

Interfejs użytkownika obejmuje następujące składniki:

| Name (Nazwa) | Typ | Opis |
|------|------|-------------|
| `FromLanguageComboBox` | ComboBox | Wyświetla listę języków tłumaczenia tekstu obsługiwanych przez usługę Microsoft Translator. Użytkownik wybiera źródłowy język tłumaczenia. |
| `ToLanguageComboBox` | ComboBox | Wyświetla tę samą listę języków co kontrolka `FromComboBox`, ale służy do wybierania docelowego języka tłumaczenia. |
| `TextToTranslate` | TextBox | Umożliwia użytkownikowi wprowadzanie tekstu do przetłumaczenia. |
| `TranslateButton` | Button | Użyj tego przycisku, aby przetłumaczyć tekst. |
| `TranslatedTextLabel` | Label | Wyświetla tłumaczenie. |
| `DetectedLanguageLabel` | Label | Wyświetla wykryty język tekstu do przetłumaczenia (`TextToTranslate`). |

> [!NOTE]
> Tworzymy ten formularz za pomocą kodu źródłowego XAML, jednak formularz możesz także utworzyć przy użyciu edytora w programie Visual Studio.

Dodajmy kod do naszego projektu.

1. W programie Visual Studio wybierz kartę `MainWindow.xaml`.
2. Skopiuj ten kod do projektu i zapisz.
   ```xaml
   <Window x:Class="MSTranslatorTextDemo.MainWindow"
           xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
           xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
           xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
           xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
           xmlns:local="clr-namespace:MSTranslatorTextDemo"
           mc:Ignorable="d"
           Title="Microsoft Translator" Height="400" Width="700" BorderThickness="0">
       <Grid>
           <Label x:Name="label" Content="Microsoft Translator" HorizontalAlignment="Left" Margin="39,6,0,0" VerticalAlignment="Top" Height="49" FontSize="26.667"/>
           <TextBox x:Name="TextToTranslate" HorizontalAlignment="Left" Height="23" Margin="42,160,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="600" FontSize="14" TabIndex="3"/>
           <Label x:Name="EnterTextLabel" Content="Text to translate:" HorizontalAlignment="Left" Margin="40,129,0,0" VerticalAlignment="Top" FontSize="14"/>
           <Label x:Name="toLabel" Content="Translate to:" HorizontalAlignment="Left" Margin="304,58,0,0" VerticalAlignment="Top" FontSize="14"/>

           <Button x:Name="TranslateButton" Content="Translate" HorizontalAlignment="Left" Margin="39,206,0,0" VerticalAlignment="Top" Width="114" Height="31" Click="TranslateButton_Click" FontSize="14" TabIndex="4" IsDefault="True"/>
           <ComboBox x:Name="ToLanguageComboBox"
                   HorizontalAlignment="Left"
                   Margin="306,88,0,0"
                   VerticalAlignment="Top"
                   Width="175" FontSize="14" TabIndex="2">

           </ComboBox>
           <Label x:Name="fromLabel" Content="Translate from:" HorizontalAlignment="Left" Margin="40,58,0,0" VerticalAlignment="Top" FontSize="14"/>
           <ComboBox x:Name="FromLanguageComboBox"
               HorizontalAlignment="Left"
               Margin="42,88,0,0"
               VerticalAlignment="Top"
               Width="175" FontSize="14" TabIndex="1"/>
           <Label x:Name="TranslatedTextLabel" Content="Translation is displayed here." HorizontalAlignment="Left" Margin="39,255,0,0" VerticalAlignment="Top" Width="620" FontSize="14" Height="85" BorderThickness="0"/>
           <Label x:Name="DetectedLanguageLabel" Content="Autodetected language is displayed here." HorizontalAlignment="Left" Margin="39,288,0,0" VerticalAlignment="Top" Width="620" FontSize="14" Height="84" BorderThickness="0"/>
       </Grid>
   </Window>
   ```
3. Powinien zostać teraz wyświetlony podgląd interfejsu użytkownika aplikacji w programie Visual Studio. Podgląd powinien wyglądać mniej więcej tak jak na obrazie powyżej.

To wszystko. Formularz jest gotowy. Teraz napiszmy kod, który umożliwi korzystanie z tłumaczenia tekstu i sprawdzania pisowni Bing.

> [!NOTE]
> Możesz dostosować ten formularz lub utworzyć własny.

## <a name="create-your-app"></a>Tworzenie aplikacji

Plik `MainWindow.xaml.cs` zawiera kod, który kontroluje naszą aplikację. W kolejnych sekcjach dodamy kod, przy użyciu którego wypełnimy menu rozwijane oraz wywołamy kilka interfejsów API uwidocznionych przez tłumaczenie tekstu w usłudze Translator i sprawdzanie pisowni Bing.

* Po uruchomieniu programu i utworzeniu wystąpienia `MainWindow` metoda `Languages` interfejsu API tłumaczenia tekstu w usłudze Translator jest wywoływana w celu pobrania i wypełnienia menu rozwijanych wyboru języka. Odbywa się to raz na początku każdej sesji.
* Po kliknięciu przycisku **Translate** (Tłumacz) pobierany jest tekst i wybrany język użytkownika, przeprowadzane jest sprawdzenie pisowni na danych wejściowych, a następnie użytkownikowi jest wyświetlane tłumaczenie i wykryty język.
  * Metoda `Translate` interfejsu API tłumaczenia tekstu w usłudze Translator jest wywoływana w celu przetłumaczenia tekstu z elementu `TextToTranslate`. To wywołanie obejmuje także języki `to` i `from` wybrane przy użyciu menu rozwijanych.
  * Metoda `Detect` interfejsu API tłumaczenia tekstu w usłudze Translator jest wywoływana w celu określenia języka `TextToTranslate`.
  * Sprawdzanie pisowni Bing służy do weryfikowania elementu `TextToTranslate` i dopasowania błędów pisowni.

Cały nasz projekt jest hermetyzowany w klasie `MainWindow : Window`. Zacznijmy od dodania kodu, który ustawi klucz subskrypcji, zadeklaruje punkty końcowe dla tłumaczenia tekstu w usłudze Translator i sprawdzania pisowni Bing oraz zainicjuje aplikację.

1. W programie Visual Studio wybierz kartę `MainWindow.xaml.cs`.
2. Zastąp wstępnie wypełnione instrukcje `using` następującym kodem.  
   ```csharp
   using System;
   using System.Windows;
   using System.Net;
   using System.Net.Http;
   using System.IO;
   using System.Collections.Generic;
   using System.Linq;
   using System.Text;
   using Newtonsoft.Json;
   ```
3. Znajdź klasę `MainWindow : Window` i zastąp ją następującym kodem:
   ```csharp
   {
       // This sample uses the Cognitive Services subscription key for all services. To learn more about
       // authentication options, see: https://docs.microsoft.com/azure/cognitive-services/authentication.
       const string COGNITIVE_SERVICES_KEY = "YOUR_COG_SERVICES_KEY";
       // Endpoints for Translator Text and Bing Spell Check
       public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api- version=3.0";
       const string BING_SPELL_CHECK_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/bing/v7.0/spellcheck/";
       // An array of language codes
       private string[] languageCodes;

       // Dictionary to map language codes from friendly name (sorted case-insensitively on language name)
       private SortedDictionary<string, string> languageCodesAndTitles =
           new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

       // Global exception handler to display error message and exit
       private static void HandleExceptions(object sender, UnhandledExceptionEventArgs args)
       {
           Exception e = (Exception)args.ExceptionObject;
           MessageBox.Show("Caught " + e.Message, "Error", MessageBoxButton.OK, MessageBoxImage.Error);
           System.Windows.app.Current.Shutdown();
       }
       // MainWindow constructor
       public MainWindow()
       {
           // Display a message if unexpected error is encountered
           AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

           if (COGNITIVE_SERVICES_KEY.Length != 32)
           {
               MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                   "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                   "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
               System.Windows.app.Current.Shutdown();
           }
           else
           {
               // Start GUI
               InitializeComponent();
               // Get languages for drop-downs
               GetLanguagesForTranslate();
               // Populate drop-downs with values from GetLanguagesForTranslate
               PopulateLanguageMenus();
           }
       }
   // NOTE:
   // In the following sections, we'll add code below this.
   }
   ```
   1. Dodaj klucz subskrypcji usług Cognitive Services i zapisz.

W tym bloku kodu zadeklarowaliśmy dwie zmienne składowe, które zawierają informacje o dostępnych językach na potrzeby tłumaczenia:

| Zmienna | Type | Opis |
|----------|------|-------------|
|`languageCodes` | Tablica ciągów |Buforuje kody języków. Usługa Translator identyfikuje języki za pomocą krótkich kodów, takich jak `en` dla języka angielskiego. |
|`languageCodesAndTitles` | Posortowany słownik | Mapuje „przyjazne” nazwy w interfejsie użytkownika z powrotem na krótkie kody używane w interfejsie API. Jest posortowana alfabetycznie, bez uwzględniania wielkości liter. |

Następnie, w ramach `MainWindow` konstruktora, dodaliśmy obsługę błędów przy użyciu polecenia `HandleExceptions`. Daje to gwarancję, że jeśli wyjątek nie jest obsługiwany, zostanie zgłoszony alert. Następnie zostanie uruchomiony test w celu potwierdzenia, że podany klucz subskrypcji ma długość 32 znaków. Jeśli klucz jest krótszy lub dłuższy niż 32 znaki, zostanie zwrócony błąd.

Jeśli istnieją klucze o poprawnej długości, wywołanie funkcji `InitializeComponent()` uruchamia interfejs użytkownika przez zlokalizowanie, załadowanie i utworzenie wystąpienia opisu XAML dla głównego okna aplikacji.

Na koniec dodaliśmy kod wywołujący metody pobierające języki do tłumaczenia i wypełniający menu rozwijane w interfejsie użytkownika aplikacji. Nie martw się — kod odpowiedzialny za te wywołania omówimy już za chwilę.

## <a name="get-supported-languages"></a>Uzyskiwanie obsługiwanych języków

Interfejs API tłumaczenia tekstu w usłudze Translator obecnie obsługuje ponad 60 języków. Ponieważ w miarę upływu czasu będzie dodawana obsługa nowych języków, zalecamy wywołanie zasobu Languages uwidocznionego przez tłumaczenie tekstu w usłudze Translator zamiast trwałego zakodowania listy języków w aplikacji.

W tej sekcji utworzymy żądanie `GET` do zasobu Languages określające, że chcemy uzyskać listę języków dostępnych do tłumaczenia.

> [!NOTE]
> Zasób Languages umożliwia filtrowanie obsługi języków przy użyciu następujących parametrów zapytań: transliteracja, słownik i tłumaczenie. Aby uzyskać więcej informacji, zobacz [dokumentację interfejsu API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages).

Zanim przejdziemy dalej, przyjrzyjmy się przykładowym danym wyjściowym wywołania zasobu Languages:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    }
    // Additional languages are provided in the full JSON output.
}
```

Z tych danych wyjściowych możemy wyodrębnić kod języka i właściwość `name` określonego języka. Nasza aplikacja deserializuje obiekt JSON ([`JsonConvert.DeserializeObject`](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonConvert_DeserializeObject__1.htm)) przy użyciu pakietu NewtonSoft.Json.

Wróćmy teraz do miejsca, w który przerwaliśmy pracę w poprzedniej sekcji, i dodajmy metodę pobierającą obsługiwane języki do naszej aplikacji.

1. W programie Visual Studio otwórz kartę `MainWindow.xaml.cs`.
2. Dodaj następujący kod do projektu:
   ```csharp
   // ***** GET TRANSLATABLE LANGUAGE CODES
   private void GetLanguagesForTranslate()
   {
       // Send request to get supported language codes
       string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
       WebRequest WebRequest = WebRequest.Create(uri);
       WebRequest.Headers.Add("Accept-Language", "en");
       WebResponse response = null;
       // Read and parse the JSON response
       response = WebRequest.GetResponse();
       using (var reader = new StreamReader(response.GetResponseStream(), UnicodeEncoding.UTF8))
       {
           var result = JsonConvert.DeserializeObject<Dictionary<string, Dictionary<string, Dictionary<string, string>>>>(reader.ReadToEnd());
           var languages = result["translation"];

           languageCodes = languages.Keys.ToArray();
           foreach (var kv in languages)
           {
               languageCodesAndTitles.Add(kv.Value["name"], kv.Key);
           }
       }
   }
   // NOTE:
   // In the following sections, we'll add code below this.
   ```

Metoda `GetLanguagesForTranslate()` tworzy żądanie HTTP GET i przy użyciu parametru ciągu zapytania `scope=translation` ogranicza zakres żądania do języków obsługiwanych na potrzeby tłumaczenia. Dodawany jest nagłówek `Accept-Language` z wartością `en`, aby obsługiwane języki były zwracane w języku angielskim.

Odpowiedź w formacie JSON jest analizowana i konwertowana do słownika. Następnie kody języków są dodawane do zmiennej składowej `languageCodes`. Pary klucz/wartość, które zawierają kody języków i przyjazne nazwy języków, są przetwarzane w pętli i dodawane do zmiennej składowej `languageCodesAndTitles`. Menu rozwijane w formularzu zawierają przyjazne nazwy, ale do żądania tłumaczeń są potrzebne kody.

## <a name="populate-language-drop-down-menus"></a>Wypełnianie menu rozwijanych języków

Interfejs użytkownika jest zdefiniowany w kodzie XAML, dzięki czemu skonfigurowanie go nie wymaga wielu operacji oprócz wywołania funkcji `InitializeComponent()`. Jedyną operacją, którą należy wykonać, jest dodanie przyjaznych nazw języków do list rozwijanych **Tłumacz z** i **Tłumacz na**, co jest wykonywane przy użyciu metody `PopulateLanguageMenus()`.

1. W programie Visual Studio otwórz kartę `MainWindow.xaml.cs`.
2. Dodaj następujący kod do projektu poniżej metody `GetLanguagesForTranslate()`:
   ```csharp
   private void PopulateLanguageMenus()
   {
       // Add option to automatically detect the source language
       FromLanguageComboBox.Items.Add("Detect");

       int count = languageCodesAndTitles.Count;
       foreach (string menuItem in languageCodesAndTitles.Keys)
       {
           FromLanguageComboBox.Items.Add(menuItem);
           ToLanguageComboBox.Items.Add(menuItem);
       }

       // Set default languages
       FromLanguageComboBox.SelectedItem = "Detect";
       ToLanguageComboBox.SelectedItem = "English";
   }
   // NOTE:
   // In the following sections, we'll add code below this.
   ```

Ta metoda iteruje przez słownik `languageCodesAndTitles` i dodaje każdy klucz do obu menu. Gdy menu zostaną wypełnione, domyślne języki źródłowy i docelowy są ustawiane odpowiednio na wartość **Detect** (Wykryj) i **English** (Angielski).

> [!TIP]
> Bez domyślnie wybranych opcji menu użytkownik mógłby kliknąć przycisk **Translate** bez uprzedniego wybrania języka docelowego lub źródłowego. Ustawienia domyślne eliminują konieczność obsługi tego problemu.

Po zainicjowaniu klasy `MainWindow` i utworzeniu interfejsu użytkownika kod nie zostanie uruchomiony, dopóki przycisk **Translate** (Tłumacz) nie zostanie kliknięty.

## <a name="detect-language-of-source-text"></a>Wykrywanie języka tekstu źródłowego

Teraz utworzymy metodę wykrywającą język tekstu źródłowego (tekstu wprowadzonego do pola tekstowego) przy użyciu interfejsu API tłumaczenia tekstu w usłudze Translator. Wartość zwrócona przez to żądanie zostanie później użyta w żądaniu tłumaczenia.

1. W programie Visual Studio otwórz kartę `MainWindow.xaml.cs`.
2. Dodaj następujący kod do projektu poniżej metody `PopulateLanguageMenus()`:
   ```csharp
   // ***** DETECT LANGUAGE OF TEXT TO BE TRANSLATED
   private string DetectLanguage(string text)
   {
       string detectUri = string.Format(TEXT_TRANSLATION_API_ENDPOINT ,"detect");

       // Create request to Detect languages with Translator Text
       HttpWebRequest detectLanguageWebRequest = (HttpWebRequest)WebRequest.Create(detectUri);
       detectLanguageWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", COGNITIVE_SERVICES_KEY);
       detectLanguageWebRequest.Headers.Add("Ocp-Apim-Subscription-Region", "westus");
       detectLanguageWebRequest.ContentType = "application/json; charset=utf-8";
       detectLanguageWebRequest.Method = "POST";

       // Send request
       var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
       string jsonText = serializer.Serialize(text);

       string body = "[{ \"Text\": " + jsonText + " }]";
       byte[] data = Encoding.UTF8.GetBytes(body);

       detectLanguageWebRequest.ContentLength = data.Length;

       using (var requestStream = detectLanguageWebRequest.GetRequestStream())
           requestStream.Write(data, 0, data.Length);

       HttpWebResponse response = (HttpWebResponse)detectLanguageWebRequest.GetResponse();

       // Read and parse JSON response
       var responseStream = response.GetResponseStream();
       var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
       dynamic jsonResponse = serializer.DeserializeObject(jsonString);

       // Fish out the detected language code
       var languageInfo = jsonResponse[0];
       if (languageInfo["score"] > (decimal)0.5)
       {
           DetectedLanguageLabel.Content = languageInfo["language"];
           return languageInfo["language"];
       }
       else
           return "Unable to confidently detect input language.";
   }
   // NOTE:
   // In the following sections, we'll add code below this.
   ```

Ta metoda tworzy żądanie HTTP `POST` do zasobu Detect. Przyjmuje ona pojedynczy argument `text`, który jest przekazywany jako treść żądania. Później, gdy utworzymy żądanie tłumaczenia, tekst wprowadzony w interfejsie użytkownika zostanie przekazany do tej metody na potrzeby wykrywania języka.

Ponadto ta metoda oblicza współczynnik ufności odpowiedzi. Jeśli wynik jest większy niż `0.5`, wykryty język jest wyświetlany w interfejsie użytkownika.

## <a name="spell-check-the-source-text"></a>Sprawdzanie pisowni w tekście źródłowym

Teraz utworzymy metodę sprawdzającą pisownię w tekście źródłowym przy użyciu interfejsu API sprawdzania pisowni Bing. Daje to gwarancję, że otrzymamy dokładne tłumaczenia z interfejsu API tłumaczenia tekstu w usłudze Translator. Wszelkie poprawki w tekście źródłowym są przekazywane w żądaniu tłumaczenia po kliknięciu przycisku **Translate** (Tłumacz).

1. W programie Visual Studio otwórz kartę `MainWindow.xaml.cs`.
2. Dodaj następujący kod do projektu poniżej metody `DetectLanguage()`:

```csharp
// ***** CORRECT SPELLING OF TEXT TO BE TRANSLATED
private string CorrectSpelling(string text)
{
    string uri = BING_SPELL_CHECK_API_ENDPOINT + "?mode=spell&mkt=en-US";

    // Create a request to Bing Spell Check API
    HttpWebRequest spellCheckWebRequest = (HttpWebRequest)WebRequest.Create(uri);
    spellCheckWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", COGNITIVE_SERVICES_KEY);
    spellCheckWebRequest.Method = "POST";
    spellCheckWebRequest.ContentType = "application/x-www-form-urlencoded"; // doesn't work without this

    // Create and send the request
    string body = "text=" + System.Web.HttpUtility.UrlEncode(text);
    byte[] data = Encoding.UTF8.GetBytes(body);
    spellCheckWebRequest.ContentLength = data.Length;
    using (var requestStream = spellCheckWebRequest.GetRequestStream())
        requestStream.Write(data, 0, data.Length);
    HttpWebResponse response = (HttpWebResponse)spellCheckWebRequest.GetResponse();

    // Read and parse the JSON response; get spelling corrections
    var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
    var responseStream = response.GetResponseStream();
    var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
    dynamic jsonResponse = serializer.DeserializeObject(jsonString);
    var flaggedTokens = jsonResponse["flaggedTokens"];

    // Construct sorted dictionary of corrections in reverse order (right to left)
    // This ensures that changes don't impact later indexes
    var corrections = new SortedDictionary<int, string[]>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
    for (int i = 0; i < flaggedTokens.Length; i++)
    {
        var correction = flaggedTokens[i];
        var suggestion = correction["suggestions"][0];  // Consider only first suggestion
        if (suggestion["score"] > (decimal)0.7)         // Take it only if highly confident
            corrections[(int)correction["offset"]] = new string[]   // dict key   = offset
                { correction["token"], suggestion["suggestion"] };  // dict value = {error, correction}
    }

    // Apply spelling corrections, in order, from right to left
    foreach (int i in corrections.Keys)
    {
        var oldtext = corrections[i][0];
        var newtext = corrections[i][1];

        // Apply capitalization from original text to correction - all caps or initial caps
        if (text.Substring(i, oldtext.Length).All(char.IsUpper)) newtext = newtext.ToUpper();
        else if (char.IsUpper(text[i])) newtext = newtext[0].ToString().ToUpper() + newtext.Substring(1);

        text = text.Substring(0, i) + newtext + text.Substring(i + oldtext.Length);
    }
    return text;
}
// NOTE:
// In the following sections, we'll add code below this.
```

## <a name="translate-text-on-click"></a>Tłumaczenie tekstu po kliknięciu

Ostatnią czynnością do wykonania jest utworzenie metody wywoływanej po kliknięciu przycisku **Translate** (Tłumacz) w interfejsie użytkownika.

1. W programie Visual Studio otwórz kartę `MainWindow.xaml.cs`.
2. Dodaj następujący kod do projektu poniżej metody `CorrectSpelling()` i zapisz:  
   ```csharp
   // ***** PERFORM TRANSLATION ON BUTTON CLICK
   private async void TranslateButton_Click(object sender, EventArgs e)
   {
       string textToTranslate = TextToTranslate.Text.Trim();

       string fromLanguage = FromLanguageComboBox.SelectedValue.ToString();
       string fromLanguageCode;

       // auto-detect source language if requested
       if (fromLanguage == "Detect")
       {
           fromLanguageCode = DetectLanguage(textToTranslate);
           if (!languageCodes.Contains(fromLanguageCode))
           {
               MessageBox.Show("The source language could not be detected automatically " +
                   "or is not supported for translation.", "Language detection failed",
                   MessageBoxButton.OK, MessageBoxImage.Error);
               return;
           }
       }
       else
           fromLanguageCode = languageCodesAndTitles[fromLanguage];

       string toLanguageCode = languageCodesAndTitles[ToLanguageComboBox.SelectedValue.ToString()];

       // spell-check the source text if the source language is English
       if (fromLanguageCode == "en")
       {
           if (textToTranslate.StartsWith("-"))    // don't spell check in this case
               textToTranslate = textToTranslate.Substring(1);
           else
           {
               textToTranslate = CorrectSpelling(textToTranslate);
               TextToTranslate.Text = textToTranslate;     // put corrected text into input field
           }
       }
       // handle null operations: no text or same source/target languages
       if (textToTranslate == "" || fromLanguageCode == toLanguageCode)
       {
           TranslatedTextLabel.Content = textToTranslate;
           return;
       }

       // send HTTP request to perform the translation
       string endpoint = string.Format(TEXT_TRANSLATION_API_ENDPOINT, "translate");
       string uri = string.Format(endpoint + "&from={0}&to={1}", fromLanguageCode, toLanguageCode);

       System.Object[] body = new System.Object[] { new { Text = textToTranslate } };
       var requestBody = JsonConvert.SerializeObject(body);

       using (var client = new HttpClient())
       using (var request = new HttpRequestMessage())
       {
           request.Method = HttpMethod.Post;
           request.RequestUri = new Uri(uri);
           request.Content = new StringContent(requestBody, Encoding.UTF8, "app/json");
           request.Headers.Add("Ocp-Apim-Subscription-Key", COGNITIVE_SERVICES_KEY);
           request.Headers.Add("Ocp-Apim-Subscription-Region", "westus");
           request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

           var response = await client.SendAsync(request);
           var responseBody = await response.Content.ReadAsStringAsync();

           var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
           var translation = result[0]["translations"][0]["text"];

           // Update the translation field
           TranslatedTextLabel.Content = translation;
       }
   }
   ```

Pierwszym krokiem jest pobranie języków źródłowego i docelowego oraz tekstu wprowadzonego przez użytkownika w formularzu. Jeśli dla języka źródłowego jest ustawiona opcja **Detect** (Wykryj), wywoływana jest funkcja `DetectLanguage()` w celu ustalenia języka tekstu źródłowego. Tekst może być w języku, który nie jest obsługiwany przez interfejs API usługi Translator. W takim przypadku jest wyświetlany komunikat z informacją dla użytkownika i następuje powrót bez tłumaczenia tekstu.

Jeśli językiem źródłowym jest angielski (określony lub wykryty), sprawdzana jest pisownia tekstu za pomocą funkcji `CorrectSpelling()` i stosowane są wszelkie poprawki. Poprawiony tekst jest dodawany z powrotem do obszaru tekstu, aby użytkownik widział, że wprowadzono poprawki.

Kod wykonujący tłumaczenie tekstu powinien wyglądać znajomo: utworzenie identyfikatora URI, utworzenie żądania, wysłanie go i przeanalizowanie odpowiedzi. Tablica JSON może zawierać więcej niż jeden obiekt do tłumaczenia, jednak nasza aplikacja wymaga tylko jednego.

Po pomyślnym zakończeniu żądania element `TranslatedTextLabel.Content` jest zastępowany elementem `translation`, który aktualizuje interfejs użytkownika, aby wyświetlał przetłumaczony tekst.

## <a name="run-your-wpf-app"></a>Uruchamianie aplikacji WPF

To wszystko — masz teraz działającą aplikację do tłumaczenia utworzoną przy użyciu platformy WPF. Aby uruchomić aplikację, kliknij przycisk **Uruchom** w programie Visual Studio.

## <a name="source-code"></a>Kod źródłowy

Kod źródłowy tego projektu jest dostępny w serwisie GitHub.

* [Zapoznaj się z kodem źródłowym](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-C-Sharp-Tutorial)

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Dokumentacja interfejsu API tłumaczenia tekstu w usłudze Microsoft Translator](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference)
