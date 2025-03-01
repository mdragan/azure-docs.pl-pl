---
title: Łączenie z systemami SAP — Azure Logic Apps
description: Dostęp do zasobów i zarządzanie nimi SAP dzięki automatyzacji przepływów pracy z usługą Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
ms.date: 05/09/2019
tags: connectors
ms.openlocfilehash: bccefec80ef3afd6d312bb1048d3be5d8e708728
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66258147"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Łączenie z systemami SAP z usługi Azure Logic Apps

Ten artykuł pokazuje, jak można pobrać w lokalnych zasobach SAP z wewnątrz aplikacji logiki za pomocą łącznika SAP. Łącznik współdziała z klasycznymi wersjami rozwiązania SAP, takimi jak systemy lokalne R/3 i ECC. Łącznik również umożliwia integrację z nowszych systemów SAP HANA na podstawie SAP takich jak S/4 HANA, wszędzie tam, gdzie one są hostowane — lokalnie lub w chmurze. Łącznik SAP obsługuje integrację wiadomości lub danych do i z systemów opartych na oprogramowanie SAP NetWeaver przez pośredniego dokumentu (IDoc) lub interfejsu programowania aplikacji biznesowych (BAPI) lub zdalnego wywołania funkcji (RFC).

Łącznik SAP używa [Biblioteka łącznika systemu SAP .NET (NCo)](https://support.sap.com/en/product/connectors/msnet.html) i udostępnia te operacje lub akcje:

* **Wyślij do SAP**: Wysłania IDoc tRFC, wywoływać funkcje BAPI za pośrednictwem RFC lub wywołania RFC/tRFC systemów SAP.
* **Otrzymywać od firmy SAP**: Odbieranie IDoc za pośrednictwem tRFC, wywoływać funkcje BAPI za pośrednictwem tRFC lub wywołania RFC/tRFC systemów SAP.
* **Generowanie schematów**: Generowanie schematów dla artefaktów SAP IDoc, BAPI lub RFC.

We wszystkich powyższych operacjach łącznik SAP obsługuje podstawowe uwierzytelnianie za pomocą nazwy użytkownika i hasła. Łącznik obsługuje dodatkowo funkcje [zabezpieczenia sieci komunikacji (SNC)](https://help.sap.com/doc/saphelp_nw70/7.0.31/e6/56f466e99a11d1a5b00000e835363f/content.htm?no_cache=true), które mogą służyć do SAP NetWeaver logowania jednokrotnego lub możliwości zapewnienia dodatkowych zabezpieczeń oferowane przez to produkt zewnętrzny zabezpieczeń.

Łącznik SAP integruje się z lokalnymi systemami SAP za pośrednictwem [lokalnej bramy danych](../logic-apps/logic-apps-gateway-connection.md). W scenariuszach wysyłania na przykład podczas wysyłania wiadomości z aplikacji logiki do systemu SAP brama danych działa jako klient specyfikacji RFC i przekazuje żądaniami otrzymanymi od logiki aplikacji SAP.
Podobnie w scenariuszach Receive brama danych działa jako serwer RFC, która odbiera żądania od firmy SAP i przekazuje do aplikacji logiki.

W tym artykule pokazano, jak utworzyć przykład aplikacji logiki, które integrują się z SAP podczas obejmujące scenariusze integracji opisany wcześniej.

<a name="pre-reqs"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Aby skorzystać z tego artykułu, potrzebne są następujące elementy:

* Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure [Załóż bezpłatne konto platformy Azure](https://azure.microsoft.com/free/).

* Aplikacja logiki, z którym chcesz uzyskać dostęp do systemu SAP i wyzwalacza uruchamiającego przepływ pracy aplikacji logiki. Jeśli dopiero zaczynasz pracę z usługi logic apps, zapoznaj się z [co to jest Azure Logic Apps](../logic-apps/logic-apps-overview.md) i [Szybki Start: Utwórz swoją pierwszą aplikację logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Twoje [serwera aplikacji SAP](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) lub [SAP Message Server](https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm)

* Pobierz i zainstaluj najnowszą wersję [lokalnej bramy danych](https://www.microsoft.com/download/details.aspx?id=53127) na każdym komputerze w środowisku lokalnym. Upewnij się, że możesz skonfigurować bramę w witrynie Azure portal przed kontynuowaniem. Brama ułatwia bezpiecznego dostępu do danych lokalnych i zasobów. Aby uzyskać więcej informacji, zobacz [instalacji lokalnej bramy danych usługi Azure Logic Apps](../logic-apps/logic-apps-gateway-install.md).

* Jeśli używasz SNC za pomocą pojedynczego logowania jednokrotnego (SSO), upewnij się, że brama działa jako użytkownik, który jest mapowany użytkownika SAP. Aby zmienić konto domyślne, wybierz **zmienić konto**, a następnie wprowadź poświadczenia użytkownika.

  ![Zmień konto bramy](./media/logic-apps-using-sap-connector/gateway-account.png)

* Jeśli włączasz SNC z produktem zabezpieczenia zewnętrzne, skopiuj Biblioteka SNC lub plików na tym samym komputerze, w którym zainstalowano bramę. Niektóre przykłady produktów SNC [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), protokołu Kerberos, NTLM, i tak dalej.

* Pobierz i zainstaluj najnowsze biblioteki klienta SAP, która jest obecnie [łącznika systemu SAP (NCo) 3.0.21.0 dla programu Microsoft .NET Framework 4.0 i Windows 64-bitowy (x64)](https://softwaredownloads.sap.com/file/0020000001865512018), w tym samym komputerze co lokalna brama danych. Zainstaluj tę wersję lub nowszym z tych powodów:

  * Wcześniejszych wersjach SAP NCo mogą stać się zakleszczone więcej niż jeden IDoc komunikaty wysłane w tym samym czasie. Ten warunek blokuje wszystkie nowsze wiadomości, które są wysyłane do miejsca docelowego SAP, powodując wiadomości, które przekraczają limit czasu.

  * Lokalna brama danych działa tylko w systemach 64-bitowych. W przeciwnym razie otrzymasz błąd "Nieprawidłowy obraz" ponieważ usługa hosta bramy danych nie obsługuje zestawów 32-bitowych.

  * Zarówno usługa host bramy danych, jak i Microsoft SAP Adapter korzystają z .NET Framework 4.5. NCo SAP dla programu .NET Framework 4.0 współpracuje z procesami, które używają środowiska uruchomieniowego .NET 4.0 i 4.7.1. NCo SAP dla platformy .NET Framework 2.0 współpracuje z procesami, które używają środowiska uruchomieniowego .NET 2.0 i 3.5 i nie będzie działać z najnowszą lokalnej bramy danych.

* Zawartość wiadomości wysyłanych do serwera SAP, np. przykładowego pliku IDoc. Ta zawartość musi być w formacie XML i zawierać przestrzeń nazw dla akcji SAP, którego chcesz użyć.

<a name="add-trigger"></a>

## <a name="send-to-sap"></a>Wyślij do SAP

W tym przykładzie użyto aplikacji logiki, który można wywoływać za pomocą żądania HTTP. Aplikacja logiki wysyła pośrednie dokument (IDoc) do serwera SAP, a następnie zwraca odpowiedź do osoby zgłaszającej żądanie, która wywołuje aplikację logiki. 

### <a name="add-http-request-trigger"></a>Dodawanie wyzwalacza żądania HTTP

W usłudze Azure Logic Apps, każda aplikacja logiki musi rozpoczynać się [wyzwalacza](../logic-apps/logic-apps-overview.md#logic-app-concepts), który jest aktywowany wystąpienia, gdy zajdzie określone zdarzenie lub po spełnieniu określonego warunku. Każdym aktywowaniu wyzwalacza aparat usługi Logic Apps tworzy wystąpienie aplikacji logiki i uruchamiania przepływu pracy Twojej aplikacji.

W tym przykładzie utworzysz aplikację logiki z punktem końcowym usługi na platformie Azure tak, aby można było wysyłać *żądania HTTP POST* do aplikacji logiki. Gdy Twoja aplikacja logiki odbiera tych żądań HTTP, aktywuje się i uruchamia następny krok w przepływie pracy.

1. W [witryny Azure portal](https://portal.azure.com), tworzenie pustej aplikacji logiki, który zostanie otwarty projektant aplikacji logiki.

1. W polu wyszukiwania wpisz "żądanie http" jako filtr. Z listy wyzwalaczy wybierz następujący wyzwalacz: **Po odebraniu żądania HTTP**

   ![Dodawanie wyzwalacza żądania HTTP](./media/logic-apps-using-sap-connector/add-trigger.png)

1. Teraz Zapisz aplikację logiki, dzięki czemu można wygenerować adresu URL punktu końcowego dla aplikacji logiki. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**.

   Punkt końcowy adres URL jest teraz wyświetlany w wyzwalacza, na przykład:

   ![Generowanie adresu URL punktu końcowego](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

### <a name="add-sap-action"></a>Dodawanie akcji SAP

W usłudze Azure Logic Apps [akcji](../logic-apps/logic-apps-overview.md#logic-app-concepts) jest to krok w przepływie pracy, występującego wyzwalacza lub innej akcji. Jeśli nie zostały jeszcze dodane wyzwalacza do aplikacji logiki, a chcesz kontynuować ten przykład [Dodaj wyzwalacz, opisane w tej sekcji](#add-trigger).

1. W Projektancie aplikacji logiki w obszarze wyzwalacza wybierz **nowy krok**.

   ![Wybierz pozycję "Nowy krok"](./media/logic-apps-using-sap-connector/add-action.png)

1. W polu wyszukiwania wprowadź "sap" jako filtr. Z listy akcji wybierz następującą akcję: **Wyślij wiadomość do SAP**
  
   ![Wybieranie akcji Wyślij SAP](media/logic-apps-using-sap-connector/select-sap-send-action.png)

   Alternatywnie zamiast wyszukiwania, wybierz **Enterprise** kartę, a następnie wybierz akcję SAP.

   ![Wybieranie akcji Wyślij SAP na karcie przedsiębiorstwa](media/logic-apps-using-sap-connector/select-sap-send-action-ent-tab.png)

1. Jeśli zostanie wyświetlony monit, aby uzyskać informacje dotyczące połączenia, należy utworzyć połączenie SAP teraz. W przeciwnym razie istnieje już połączenie, przejdź do następnego kroku, dzięki czemu możesz skonfigurować akcję SAP.

   **Utwórz połączenie SAP w środowisku lokalnym**

   1. Podaj informacje o połączeniu dla serwera SAP. Dla **bramy Data Gateway** właściwości, wybierz bramę danych utworzone w witrynie Azure portal dla Twojej instalacji bramy.

      Jeśli **typ logowania** właściwość jest ustawiona na **serwera aplikacji**, te właściwości, które zwykle znajdują się opcjonalne, są wymagane:

      ![Utwórz połączenie z serwerem aplikacji SAP](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      Jeśli **typ logowania** właściwość jest ustawiona na **grupy**, te właściwości, które zwykle znajdują się opcjonalne, są wymagane:

      ![Utwórz połączenie z serwerem SAP wiadomości](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

      Domyślnie silne wpisywanie służy pod kątem nieprawidłowych wartości, wykonując sprawdzanie poprawności kodu XML względem schematu. To zachowanie może pomóc w wykrywaniu wcześniej. **Bezpieczne wpisując** opcja jest dostępna dla zgodności z poprzednimi wersjami i sprawdza tylko długość ciągu. Dowiedz się więcej o [ **bezpieczne wpisując** opcji](#safe-typing).

   1. Gdy wszystko będzie gotowe, wybierz pozycję **Utwórz**.

      Usługa Logic Apps, konfiguruje i testuje połączenie, upewniając się, że połączenie działa poprawnie.

1. Teraz Znajdź i wybierz akcję z serwera SAP.

   1. W **akcji SAP** , wybierz ikonę folderu. Z listy Znajdź i zaznacz wiadomość SAP, którą chcesz użyć. Aby nawigować po liście, użyj strzałek.

      W tym przykładzie wybiera IDoc z **zamówienia** typu.

      ![Znajdowanie i wybieranie akcji IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      Jeśli nie możesz znaleźć akcję, którą chcesz, możesz ręcznie wprowadzić ścieżkę, na przykład:

      ![Ręcznie Podaj ścieżkę do akcji IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      > [!TIP]
      > Podaj wartość dla akcji SAP za pomocą edytora wyrażeń. W ten sposób można użyć tej samej akcji dla różnych typów komunikatów.

      Aby uzyskać więcej informacji na temat operacji IDoc zobacz [komunikatu schematów dla operacji IDOC](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

   1. Kliknij wewnątrz **komunikat wejściowy** pola tak, aby wyświetlić listę zawartości dynamicznej. Z tej listy w obszarze **zostanie odebrane żądanie po HTTP**, wybierz opcję **treści** pola.

      Ten krok obejmuje treść z wyzwalacza żądania HTTP i wysyła go do serwera SAP.

      ![Zaznacz pole "Treść"](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      Gdy skończysz, Twoja Akcja SAP będzie wyglądać następująco:

      ![Zakończenie akcji SAP](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

1. Zapisz aplikację logiki. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**.

<a name="add-response"></a>

### <a name="add-http-response-action"></a>Dodawanie akcji odpowiedzi HTTP

Teraz Dodaj akcję odpowiedzi do przepływu pracy aplikacji logiki i zawierają dane wyjściowe z akcji SAP. W ten sposób Twoja aplikacja logiki zwraca wyniki z serwera SAP do oryginalnego obiektu żądającego.

1. W Projektancie aplikacji logiki w obszarze akcji SAP wybierz **nowy krok**.

1. W polu wyszukiwania wprowadź "response" jako filtr. Z listy akcji wybierz następującą akcję: **Odpowiedź**

1. Kliknij wewnątrz **treści** pola tak, aby wyświetlić listę zawartości dynamicznej. Z tej listy w obszarze **wysyła komunikat do SAP**, wybierz opcję **treści** pola.

   ![Zakończenie akcji SAP](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

1. Zapisz aplikację logiki.

### <a name="test-your-logic-app"></a>Przetestuj aplikację logiki

1. Jeśli Twoja aplikacja logiki nie zostało jeszcze włączone, w menu aplikacji logiki, wybierz opcję **Przegląd**. Na pasku narzędzi wybierz **Włącz**.

1. Na pasku narzędzi Projektanta aplikacji logiki wybierz **Uruchom**. Ten krok spowoduje ręczne uruchomienie aplikacji logiki.

1. Wyzwolenie aplikacji logiki, wysyłając żądanie HTTP POST do adresu URL w wyzwalaczu żądania HTTP, a zawierają wiadomości zawartości z żądaniem. Wyślij żądanie, można użyć narzędzia takie jak [Postman](https://www.getpostman.com/apps).

   W tym artykule żądania wysyła plik IDoc, który musi być w formacie XML i zawierać przestrzeń nazw dla akcji SAP, którego używasz, na przykład:

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/2/ORDERS05//720/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

1. Po wysłaniu żądania HTTP, oczekiwania na odpowiedź z aplikacji logiki.

   > [!NOTE]
   > Twoja aplikacja logiki może upłynąć limit czasu, jeśli nie zostają ukończone wszystkie kroki, które są wymagane dla odpowiedzi w ramach [limit czasu żądania](./logic-apps-limits-and-config.md). Jeśli ten stan występuje, zablokowane żądania. Aby ułatwić diagnozowanie problemów, Dowiedz się, jak [Sprawdź i monitorowanie aplikacji logiki](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Gratulacje, utworzono aplikację logiki, który może komunikować się z serwerem SAP. Teraz, gdy skonfigurowano połączenie usługi SAP dla aplikacji logiki, możesz eksplorować inne dostępne akcje SAP, takie jak BAPI i RFC.

## <a name="receive-from-sap"></a>Otrzymywać od firmy SAP

W tym przykładzie użyto aplikację logiki, która wyzwala podczas odbierania komunikatu z systemu SAP.

### <a name="add-sap-trigger"></a>Dodawanie wyzwalacza SAP

1. W witrynie Azure portal utworzyć pustej aplikacji logiki, który zostanie otwarty projektant aplikacji logiki.

1. W polu wyszukiwania wprowadź "sap" jako filtr. Z listy wyzwalaczy wybierz następujący wyzwalacz: **Gdy wiadomość zostaje odebrana od firmy SAP**

   ![Dodawanie wyzwalacza SAP](./media/logic-apps-using-sap-connector/add-sap-trigger.png)

   Alternatywnie, można przejść do **Enterprise** kartę, a następnie wybierz wyzwalacz:

   ![Dodawanie wyzwalacza SAP na karcie przedsiębiorstwa](./media/logic-apps-using-sap-connector/add-sap-trigger-ent-tab.png)

1. Jeśli zostanie wyświetlony monit, aby uzyskać informacje dotyczące połączenia, należy utworzyć połączenie SAP teraz. W przeciwnym razie istnieje już połączenie, przejdź do następnego kroku, dzięki czemu możesz skonfigurować akcję SAP.

   **Utwórz połączenie SAP w środowisku lokalnym**

   1. Podaj informacje o połączeniu dla serwera SAP. Dla **bramy Data Gateway** właściwości, wybierz bramę danych utworzone w witrynie Azure portal dla Twojej instalacji bramy.

      Jeśli **typ logowania** właściwość jest ustawiona na **serwera aplikacji**, te właściwości, które zwykle znajdują się opcjonalne, są wymagane:

      ![Utwórz połączenie z serwerem aplikacji SAP](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      Jeśli **typ logowania** właściwość jest ustawiona na **grupy**, te właściwości, które zwykle znajdują się opcjonalne, są wymagane:

      ![Utwórz połączenie z serwerem SAP wiadomości](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

      Domyślnie silne wpisywanie służy pod kątem nieprawidłowych wartości, wykonując sprawdzanie poprawności kodu XML względem schematu. To zachowanie może pomóc w wykrywaniu wcześniej. **Bezpieczne wpisując** opcja jest dostępna dla zgodności z poprzednimi wersjami i sprawdza tylko długość ciągu. Dowiedz się więcej o [ **bezpieczne wpisując** opcji](#safe-typing).

1. Podaj wymagane parametry zgodnie z konfiguracją systemu SAP.

   Opcjonalnie możesz podać co najmniej jedną akcję SAP. Ta lista akcji określa komunikaty odbierane przez wyzwalacz z serwera SAP za pośrednictwem bramy danych. Pusta lista określa, że wyzwalacz odbiera wszystkie komunikaty. Jeśli lista zawiera więcej niż jeden komunikat, wyzwalacz odbiera tylko komunikaty, które są określone na liście. Inne komunikaty wysyłane z serwera SAP są odrzucane przez bramę.

   Możesz wybrać akcję SAP z selektora plików:

   ![Wybierz akcję SAP](media/logic-apps-using-sap-connector/select-SAP-action-trigger.png)  

   Lub można ręcznie określić akcję:

   ![Ręcznie wprowadź akcji SAP](media/logic-apps-using-sap-connector/manual-enter-SAP-action-trigger.png) 

   Oto przykład pokazujący, jak akcja pojawia się podczas konfigurowania wyzwalacza, aby otrzymać więcej niż jeden komunikat.

   ![Przykładowy wyzwalacz](media/logic-apps-using-sap-connector/example-trigger.png)  

   Aby uzyskać więcej informacji na temat akcji SAP, zobacz [komunikatu schematów dla operacji IDOC](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

1. Teraz Zapisz aplikację logiki, aby można było zacząć otrzymywać wiadomości od systemu SAP.
Na pasku narzędzi projektanta wybierz pozycję **Zapisz**.

Twoja aplikacja logiki jest teraz gotowa do odbierania komunikatów z systemu SAP.

> [!NOTE]
> Wyzwalacz SAP nie jest wyzwalaczem sondowania, ale wyzwalacz oparte na elementach webhook zamiast tego. Wyzwalacz jest wywoływana z bramy tylko wtedy, gdy istnieje komunikat, dzięki czemu nie sondowania.

### <a name="test-your-logic-app"></a>Przetestuj aplikację logiki

1. Aby wyzwolić aplikację logiki, Wyślij wiadomość z systemu SAP.

1. W menu aplikacji logiki, wybierz **Przegląd**i przejrzyj **Historia przebiegów** dla żadnych nowych uruchomień aplikacji logiki.

1. Otwórz ostatniego przebiegu, pokazuje komunikat wysyłane z systemu SAP w sekcji danych wyjściowych wyzwalacza.

## <a name="generate-schemas-for-artifacts-in-sap"></a>Generowanie schematów dla artefaktów w systemie SAP

W tym przykładzie użyto aplikacji logiki, który można wywoływać za pomocą żądania HTTP. Akcja SAP wysyła żądanie do systemu SAP mają być generowanie schematów dla określonego dokumentu pośredni (IDoc) i BAPI. Schematy, które zwracają w odpowiedzi są przekazywane do konta integracji przy użyciu łącznika usługi Azure Resource Manager.

### <a name="add-http-request-trigger"></a>Dodawanie wyzwalacza żądania HTTP

1. W witrynie Azure portal utworzyć pustej aplikacji logiki, który zostanie otwarty projektant aplikacji logiki.

1. W polu wyszukiwania wpisz "żądanie http" jako filtr. Z listy wyzwalaczy wybierz następujący wyzwalacz: **Po odebraniu żądania HTTP**

   ![Dodawanie wyzwalacza żądania HTTP](./media/logic-apps-using-sap-connector/add-trigger.png)

1. Teraz Zapisz aplikację logiki, dzięki czemu można wygenerować adresu URL punktu końcowego dla aplikacji logiki.
Na pasku narzędzi projektanta wybierz pozycję **Zapisz**.

   Punkt końcowy adres URL jest teraz wyświetlany w wyzwalacza, na przykład:

   ![Generowanie adresu URL punktu końcowego](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

### <a name="add-sap-action-to-generate-schemas"></a>Dodawanie akcji SAP mają być generowanie schematów

1. W Projektancie aplikacji logiki w obszarze wyzwalacza wybierz **nowy krok**.

   ![Wybierz pozycję "Nowy krok"](./media/logic-apps-using-sap-connector/add-action.png)

1. W polu wyszukiwania wprowadź "sap" jako filtr. Z listy akcji wybierz następującą akcję: **Generowanie schematów**
  
   ![Wybieranie akcji Wyślij SAP](media/logic-apps-using-sap-connector/select-sap-schema-generator-action.png)

   Alternatywnie można również wybrać **Enterprise** kartę, a następnie wybierz akcję SAP.

   ![Wybieranie akcji Wyślij SAP na karcie przedsiębiorstwa](media/logic-apps-using-sap-connector/select-sap-schema-generator-ent-tab.png)

1. Jeśli zostanie wyświetlony monit, aby uzyskać informacje dotyczące połączenia, należy utworzyć połączenie SAP teraz. W przeciwnym razie istnieje już połączenie, przejdź do następnego kroku, dzięki czemu możesz skonfigurować akcję SAP.

   **Utwórz połączenie SAP w środowisku lokalnym**

   1. Podaj informacje o połączeniu dla serwera SAP. Dla **bramy Data Gateway** właściwości, wybierz bramę danych utworzone w witrynie Azure portal dla Twojej instalacji bramy.

      Jeśli **typ logowania** właściwość jest ustawiona na **serwera aplikacji**, te właściwości, które zwykle znajdują się opcjonalne, są wymagane:

      ![Utwórz połączenie z serwerem aplikacji SAP](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      Jeśli **typ logowania** właściwość jest ustawiona na **grupy**, te właściwości, które zwykle znajdują się opcjonalne, są wymagane:

      ![Utwórz połączenie z serwerem SAP wiadomości](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

      Domyślnie silne wpisywanie służy pod kątem nieprawidłowych wartości, wykonując sprawdzanie poprawności kodu XML względem schematu. To zachowanie może pomóc w wykrywaniu wcześniej. **Bezpieczne wpisując** opcja jest dostępna dla zgodności z poprzednimi wersjami i sprawdza tylko długość ciągu. Dowiedz się więcej o [ **bezpieczne wpisując** opcji](#safe-typing).

   1. Gdy wszystko będzie gotowe, wybierz pozycję **Utwórz**. 
   
      Usługa Logic Apps, konfiguruje i testuje połączenie, upewniając się, że połączenie działa poprawnie.

1. Podaj ścieżkę do artefaktu, dla którego chcesz wygenerować schemat.

   Możesz wybrać akcję SAP z selektora plików:

   ![Wybierz akcję SAP](media/logic-apps-using-sap-connector/select-SAP-action-schema-generator.png)  

   Alternatywnie możesz ręcznie wprowadzić akcji:

   ![Ręcznie wprowadź akcji SAP](media/logic-apps-using-sap-connector/manual-enter-SAP-action-schema-generator.png)

   Mają być generowanie schematów dla więcej niż jeden artefakt, na przykład Podaj szczegóły akcji SAP dla każdego artefaktu:

   ![Wybierz opcję Dodaj nowy element.](media/logic-apps-using-sap-connector/schema-generator-array-pick.png)

   ![Pokaż dwóch elementów](media/logic-apps-using-sap-connector/schema-generator-example.png)

   Aby uzyskać więcej informacji na temat akcji SAP, zobacz [komunikatu schematów dla operacji IDOC](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

1. Zapisz aplikację logiki. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**.

### <a name="test-your-logic-app"></a>Przetestuj aplikację logiki

1. Na pasku narzędzi Projektanta wybierz **Uruchom** aby wyzwolić uruchomienie aplikacji logiki.

1. Otwórz przebieg, a następnie sprawdź dane wyjściowe dla **generowanie schematów** akcji.

   Dane wyjściowe pokazują wygenerowanego schematów dla określonej listy komunikatów.

### <a name="upload-schemas-to-integration-account"></a>Przekazywanie schematów do konta integracji

Opcjonalnie można pobrać lub zapisać wygenerowanego schematów w repozytoriów — takiej jak obiektów blob, magazynu lub konta integracji. Konta integracji zapewnia pierwszorzędne środowisko z innymi akcjami XML, w tym przykładzie pokazano, jak przekazywanie schematów do konta integracji dla tej samej aplikacji logiki za pomocą łącznika usługi Azure Resource Manager.

1. W Projektancie aplikacji logiki, w obszarze wyzwalacza wybierz **nowy krok**.

1. W polu wyszukiwania wprowadź "Menedżera zasobów" jako filtr. Wybierz tę akcję: **Utwórz lub zaktualizuj zasób**

   ![Wybierz akcję usługi Azure Resource Manager](media/logic-apps-using-sap-connector/select-azure-resource-manager-action.png)

1. Wprowadź szczegóły akcji, łącznie z Twojej subskrypcji platformy Azure, grupę zasobów platformy Azure i konto integracji. Aby dodać tokeny SAP do pól, kliknij wewnątrz pola dla tych pól, a następnie wybierz z listy zawartości dynamicznej, która pojawia się.

   1. Otwórz **dodano nowy parametr** listy i wybierz **lokalizacji** i **właściwości** pola.

   1. Należy podać szczegóły dotyczące tych nowych pól, jak pokazano w poniższym przykładzie.

      ![Wprowadź szczegóły akcji usługi Azure Resource Manager](media/logic-apps-using-sap-connector/azure-resource-manager-action.png)

   SAP **generowanie schematów** akcji generuje schematów jako kolekcja, dzięki czemu Projektant automatycznie dodaje **dla każdego** pętlę do akcji. Oto przykład pokazujący sposób wyświetlania tej akcji:

   ![Azure akcję usługi Resource Manager przy użyciu "for each" pętli](media/logic-apps-using-sap-connector/azure-resource-manager-action-foreach.png)  

   > [!NOTE]
   > Schematy Użyj algorytmem base64. Aby przekazać schematów z kontem integracji, musi być dekodowana przy użyciu `base64ToString()` funkcji. Oto przykład, który zawiera kod `"properties"` elementu:
   >
   > ```json
   > "properties": {
   >    "Content": "@base64ToString(items('For_each')?['Content'])",
   >    "ContentType": "application/xml",
   >    "SchemaType": "Xml"
   > }
   > ```

1. Zapisz aplikację logiki. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**.

### <a name="test-your-logic-app"></a>Przetestuj aplikację logiki

1. Na pasku narzędzi Projektanta wybierz **Uruchom** do ręcznego wyzwalania aplikacji logiki.

1. Po pomyślnym przeprowadzeniu uruchomienia, przejdź na stronę konto integracji i sprawdź, czy istnieją schematy wygenerowany.

## <a name="enable-secure-network-communications-snc"></a>Włącz komunikację sieciową bezpieczne (SNC)

Przed rozpoczęciem upewnij się, że zostały spełnione wymienione wcześniej [wymagania wstępne](#pre-reqs):

* Lokalna brama danych jest zainstalowany na komputerze, na którym znajduje się w tej samej sieci w systemie SAP.

* Dla rejestracji jednokrotnej, brama działa jako użytkownik, który jest zamapowana na użytkownika SAP.

* Biblioteka SNC, która zapewnia funkcje zabezpieczeń został zainstalowany na tym samym komputerze co brama danych. Przykłady to między innymi [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), protokołu Kerberos, NTLM, i tak dalej.

Aby włączyć SNC dla żądań do lub z systemem SAP, wybierz **SNC użyj** pole wyboru, w związku z SAP i dostarczenie tych właściwości:

   ![Konfigurowanie SAP SNC w ramach połączenia](media/logic-apps-using-sap-connector/configure-sapsnc.png)

   | Właściwość | Opis |
   |----------| ------------|
   | **Biblioteka SNC** | Nazwa biblioteki SNC lub ścieżką względną wobec lokalizacji instalacji NCo lub ścieżkę absolutną. Na przykład `sapsnc.dll` lub `.\security\sapsnc.dll` lub `c:\security\sapsnc.dll` |
   | **USŁUGA REJESTRACJI JEDNOKROTNEJ SNC** | Po nawiązaniu połączenia za pośrednictwem SNC tożsamości SNC jest zazwyczaj używany do uwierzytelniania obiektu wywołującego. Innym rozwiązaniem jest zastąpienie tak, aby informacje użytkownika i hasło mogą służyć do uwierzytelniania obiektu wywołującego, ale nadal szyfrowany wiersza. |
   | **Moja nazwa SNC** | W większości przypadków można pominąć tę właściwość. Zainstalowane rozwiązanie SNC wie, zwykle własną nazwę SNC. Tylko w przypadku rozwiązań Obsługa "wiele tożsamości" może być konieczne określ tożsamość ma być używany dla tego konkretnego docelowego lub serwer. |
   | **Nazwa partnera SNC** | Nazwa wewnętrznej bazy danych SNC |
   | **Jakość SNC ochrony** | Jakość usługi ma być używany do komunikacji SNC tego konkretnego docelowego/serwera. Wartość domyślna jest definiowany przez system wewnętrznej bazy danych. Wartość maksymalna jest definiowany przez produkt zabezpieczeń używane dla SNC. |
   |||

   > [!NOTE]
   > Zmienne środowiskowe zmienna środowiskowa SNC_LIB i SNC_LIB_64 powinna nie można ustawić na komputerze gdzie mają bramą danych i Biblioteka SNC. Jeśli ustawiona, ich pierwszeństwo miałoby na wartość Biblioteka SNC przekazywane za pośrednictwem łącznika.

<a name="safe-typing"></a>

## <a name="safe-typing"></a>Wpisywanie bezpieczne

Domyślnie podczas tworzenia połączenia SAP silne wpisywanie służy pod kątem nieprawidłowych wartości, wykonując sprawdzanie poprawności kodu XML względem schematu. To zachowanie może pomóc w wykrywaniu wcześniej. **Bezpieczne wpisując** opcja jest dostępna dla zgodności z poprzednimi wersjami i sprawdza tylko długość ciągu. Jeśli wybierzesz **bezpieczne wpisując**, typ DATS i wpisz TIMS SAP są traktowane jako ciągi, a nie jako odpowiedniki XML `xs:date` i `xs:time` gdzie `xmlns:xs="http://www.w3.org/2001/XMLSchema"`. Bezpieczne wpisując ma wpływ na zachowanie dla wszystkich generowanie schematu, Wyślij wiadomość dla zarówno ładunku "została wysłana" i "Odebrano" odpowiedzi i wyzwalacza. 

Gdy jest używane silne wpisywanie (**bezpieczne wpisując** nie włączone), schemat mapowania typów DATS i TIMS prostsze typów XML:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true" type="xs:date"/>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true" type="xs:time"/>
```

Podczas wysyłania komunikatów przy użyciu silne wpisywanie, do dopasowania formatu typu XML jest zgodny DATS i TIMS odpowiedzi:

```xml
<DATE>9999-12-31</DATE>
<TIME>23:59:59</TIME>
```

Gdy **bezpieczne wpisując** jest włączona, schemat mapuje DATS i TIMS typy XML pola ciągu, za pomocą ograniczenia długości, na przykład:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="8" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="6" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
```

Podczas wysyłania komunikatów przy użyciu **bezpieczne wpisując** włączone, DATS i TIMS odpowiedź wygląda następująco:

```xml
<DATE>99991231</DATE>
<TIME>235959</TIME>
```


## <a name="known-issues-and-limitations"></a>Znane problemy i ograniczenia

W tym miejscu są obecnie znane problemy i ograniczenia dotyczące łącznika SAP:

* Pojedynczy wysyłania do wywołania SAP lub komunikat w programach tRFC. Wzorzec zatwierdzenia interfejsu programowania aplikacji biznesowych (BAPI), takich jak wykonywanie wielu wywołań tRFC w tej samej sesji nie jest obsługiwane.

* Wyzwalacz SAP nie obsługuje odbieranie dokumentów Idoc usługi batch z systemu SAP. Ta akcja może spowodować błąd połączenia RFC między systemem SAP i bramy danych.

* Wyzwalacz SAP nie obsługuje klastry bramy danych. W niektórych przypadkach trybu failover węzła bramy danych, który komunikuje się z systemu SAP mogą się różnić od aktywnego węzła, spowodować nieoczekiwane zachowanie. W przypadku scenariuszy wysyłania klastrów bramy danych są obsługiwane.

* Łącznik SAP nie obsługuje obecnie SAP routera ciągów. Lokalna brama danych musi istnieć w tej samej sieci lokalnej, jak system SAP, którą chcesz się połączyć.

## <a name="connector-reference"></a>Dokumentacja łączników

Szczegółowe informacje techniczne dotyczące wyzwalaczy, akcje i limity, który opisano przez łącznika interfejsu OpenAPI (dawniej Swagger) opis, przejrzyj [strona referencyjna łącznika](/connectors/sap/).

## <a name="next-steps"></a>Kolejne kroki

* [Nawiązać połączenie z lokalnymi systemami](../logic-apps/logic-apps-gateway-connection.md) z aplikacji logiki
* Dowiedz się, jak sprawdzanie poprawności, przekształcania i inne operacje dotyczące wiadomości z [pakiet integracyjny dla przedsiębiorstw](../logic-apps/logic-apps-enterprise-integration-overview.md)
* Dowiedz się więcej o innych [łączników Logic Apps](../connectors/apis-list.md)
