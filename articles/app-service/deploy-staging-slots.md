---
title: Konfigurowanie środowiska tymczasowego dla aplikacji sieci web w usłudze Azure App Service | Dokumentacja firmy Microsoft
description: Dowiedz się, jak korzystać z publikowania etapowego dla aplikacji sieci web w usłudze Azure App Service.
services: app-service
documentationcenter: ''
author: cephalin
writer: cephalin
manager: jpconnoc
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2019
ms.author: cephalin
ms.openlocfilehash: cbf287aef2c1792033a198070da605014a7b6281
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272856"
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Konfigurowanie środowisk przejściowych w usłudze Azure App Service
<a name="Overview"></a>

Podczas wdrażania Twojej aplikacji sieci web, aplikacji sieci web w systemie Linux, zaplecze dla aplikacji mobilnych i aplikacji interfejsu API [usługi App Service](https://go.microsoft.com/fwlink/?LinkId=529714), można wdrożyć do gniazda wdrażane pojedynczo, zamiast domyślnego miejscem produkcyjnym podczas pracy w **standardowa**, **Premium**, lub **izolowany** warstwę planu usługi App Service. Miejsca wdrożenia to rzeczywiście aktywne aplikacje z własnymi nazwami hosta. Elementy zawartości i konfiguracji aplikacji można wymieniać między 2 miejscami wdrożenia, w tym także miejscem produkcyjnym. Wdrażanie aplikacji w miejscu innym niż produkcyjne ma następujące zalety:

* Można sprawdzić poprawność zmian aplikacji w miejscu przejściowym wdrożenia, zanim zamienisz z miejscem produkcyjnym.
* Wdrażanie aplikacji w miejscu pierwszych i zamiana go w środowisku produkcyjnym sprawia, że się upewnić, że wszystkie wystąpienia elementu gniazda są przygotowaniu przed zamianą na środowisko produkcyjne. Pozwala to wyeliminować przestój w przypadku wdrażania aplikacji. Przekierowywanie ruchu jest bezproblemowe i żadne żądania nie są porzucane z powodu operacji wymiany. Ta całego przepływu pracy można zautomatyzować, konfigurując [automatycznej wymiany](#Auto-Swap) podczas wymiany wstępnego sprawdzania poprawności nie jest wymagane.
* Po zamiany gniazda z wcześniej przygotowaną aplikacji ma teraz poprzedniej aplikacji produkcyjnej. Zmiany zamienione w gnieździe produkcyjnym nie jest zgodnie z oczekiwaniami, należy wykonać tego samego obszaru wymiany od razu, aby uzyskać "ostatniej znanej dobrej witryny" ponownie.

Każda warstwa planu usługi App Service obsługuje szereg różnych miejsc wdrożenia, a nie ma dodatkowych opłat dotyczące używania miejsc wdrożenia. Aby dowiedzieć się, liczba gniazd obsługuje warstwy aplikacji, zobacz [limity usługi App](https://docs.microsoft.com/azure/azure-subscription-service-limits#app-service-limits). Aby przeskalować swoją aplikację do innej warstwy, warstwy docelowej musi obsługiwać liczbę miejsc, w których aplikacja używa już. Na przykład, jeśli aplikacja ma więcej niż pięć gniazda, nie będzie można skalować w dół, aby **standardowa** warstwy, ponieważ **standardowa** warstwy obsługuje tylko pięć miejsc wdrożenia. 

<a name="Add"></a>

## <a name="add-slot"></a>Dodaj miejsce
Aplikacja musi być uruchomiona w **standardowa**, **Premium**, lub **izolowany** warstwy w kolejności, aby włączyć wielu miejsc wdrożenia.

1. W [witryny Azure portal](https://portal.azure.com/), otwórz aplikacji [strony zasobu](../azure-resource-manager/manage-resources-portal.md#manage-resources).

2. Na lewym pasku nawigacyjnym wybierz **miejsc wdrożenia** opcji, a następnie kliknij przycisk **Dodaj miejsce**.
   
    ![Dodaj nowe miejsce wdrożenia](./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png)
   
   > [!NOTE]
   > Jeśli aplikacja nie jest jeszcze w **standardowa**, **Premium**, lub **izolowany** warstwy, pojawi się komunikat wskazujący, obsługiwanych warstw, umożliwiające publikowanie etapowe. W tym momencie masz możliwość dokonania wyboru **uaktualnienia** i przejdź do **skalowania** kartę aplikacji przed kontynuowaniem.
   > 

3. W **Dodaj miejsce** okno dialogowe, nazwij gniazda, a następnie wybierz, czy chcesz sklonować konfiguracji aplikacji z innego istniejącego miejsca wdrożenia. Kliknij przycisk **Dodaj** aby kontynuować.
   
    ![Źródło konfiguracji](./media/web-sites-staged-publishing/ConfigurationSource1.png)
   
    Można sklonować konfiguracji z dowolnego istniejącego miejsca. Ustawienia, które mogą być klonowane obejmują ustawienia aplikacji, parametry połączenia, framework w wersji języka, gniazda sieci web, wersja protokołu HTTP i liczbę bitów platformy.

4. Po dodaniu gniazda kliknij **Zamknij** aby zamknąć okno dialogowe. Nowe gniazdo są teraz wyświetlane w **miejsc wdrożenia** strony. Domyślnie **% ruchu** jest ustawiona na 0 w przypadku nowego miejsca przy użyciu cały ruch klienta kierowane do miejsca produkcji.

5. Kliknij przycisk nowe miejsce wdrożenia, aby otworzyć stronę zasobów tego gniazda.
   
    ![Tytuł miejsca wdrożenia](./media/web-sites-staged-publishing/StagingTitle.png)

    Miejsce przejściowe jest strona zarządzania tak samo jak każdą inną aplikację usługi App Service. Możesz zmienić konfigurację tego gniazda. Nazwa gniazda jest wyświetlany w górnej części strony, aby przypomnieć Ci wyświetlasz miejsce wdrożenia.

6. Kliknij adres URL aplikacji na stronie zasobów tego gniazda. Miejsce wdrożenia ma własną nazwę hosta i jest również działającej aplikacji. Aby ograniczyć dostęp publiczny do miejsca wdrożenia, zobacz [ograniczenia adresów IP usługi aplikacji Azure](app-service-ip-restrictions.md).

Nowe miejsce wdrożenia nie ma zawartości, nawet jeśli klonowanie ustawień z innego miejsca. Można na przykład [publikowania do tego miejsca przy użyciu narzędzia git](app-service-deploy-local-git.md). Można wdrożyć do gniazda z gałęzi innym repozytorium lub innym repozytorium. 

<a name="AboutConfiguration"></a>

## <a name="what-happens-during-swap"></a>Co się dzieje podczas wymiany

[Kroki operacji wymiany](#swap-operation-steps)
[ustawienia, które zostały zamienione?](#which-settings-are-swapped)

### <a name="swap-operation-steps"></a>Kroki operacji wymiany

Podczas zamiany dwa gniazda (zazwyczaj z miejsca przejściowego do miejsca produkcji) usługi App Service wykonuje następujące czynności, aby upewnić się, że w docelowym gnieździe nie doświadczają przestoju:

1. Zastosuj następujące ustawienia z miejsca docelowego (np. miejscem produkcyjnym) do wszystkich wystąpień w miejscu źródłowym: 
    - [Specyficzne dla miejsca](#which-settings-are-swapped) ustawień aplikacji i parametry połączenia, jeśli ma to zastosowanie.
    - [Ciągłe wdrażanie](deploy-continuous-deployment.md) ustawienia, jeśli włączona.
    - [Uwierzytelnianie usługi App Service](overview-authentication-authorization.md) ustawienia, jeśli włączona.
    Wszystkie wystąpienia w gnieździe źródła, aby ponownie uruchomić dowolne z powyższych przypadkach wyzwala. Podczas [wymiany z podglądem](#Multi-Phase), to oznacza koniec pierwszej fazy, gdzie Operacja zamiany zostało wstrzymane, i możesz sprawdzić, czy w miejscu źródłowym działa poprawnie przy użyciu ustawień w docelowym gnieździe.

1. Poczekaj, aż każde wystąpienie w w miejscu źródłowym, aby zakończyć jego ponowne uruchomienie. Jeśli dowolne wystąpienie nie powiedzie się ponownie uruchomić, operacja zamiany anuluje wszystkie zmiany w miejscu źródłowym i przerywa operację.

1. Jeśli [lokalnej pamięci podręcznej](overview-local-cache.md) jest włączona, wyzwolić inicjowania lokalnej pamięci podręcznej, dokonując HTTP żądania do katalogu głównego aplikacji ("/") w każdym wystąpieniu w miejscu źródłowym i zaczekaj, aż każde wystąpienie zwraca odpowiedzi HTTP. Inicjowanie lokalnej pamięci podręcznej powoduje, że kolejne ponowne uruchomienie w każdym wystąpieniu.

1. Jeśli [automatycznej wymiany](#Auto-Swap) włączono [niestandardowe rozgrzewania](#custom-warm-up), wyzwalacz [inicjowania aplikacji](https://docs.microsoft.com/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) poprzez wysłanie żądania HTTP do katalogu głównego aplikacji ("/") w każdym wystąpieniu źródła gniazda. Jeśli wystąpienie zwraca odpowiedzi HTTP, uważa ma być przygotowaniu.

    Jeśli nie `applicationInitialization` jest określony, wyzwalacza żądania HTTP do katalogu głównego aplikacji w miejscu źródłowym na każdym wystąpieniu. Jeśli wystąpienie zwraca odpowiedzi HTTP, uważa ma być przygotowaniu.

1. Jeśli wszystkie wystąpienia w miejscu źródłowym są przygotowaniu pomyślnie, należy zamienić dwa gniazda, przełączając reguły routingu dla dwóch miejsc. Po wykonaniu tego kroku miejsca docelowego (np. miejscem produkcyjnym) ma aplikację, która wcześniej przygotowaniu w miejscu źródłowym.

1. Skoro miejsca źródłowego ma aplikacji przed wymiany wcześniej w docelowym gnieździe, wykonaj tę samą operację przy zastosowaniu wszystkich ustawień i ponowne uruchamianie wystąpienia.

W dowolnym momencie Operacja zamiany całą pracę inicjowania wymienione aplikacje odbywa się w miejscu źródłowym. Docelowym gnieździe pozostaje w trybie online podczas miejsca źródłowego przygotowany i przygotowaniu w górę, niezależnie od tego, gdzie wymiany zakończy się pomyślnie lub nie powiedzie się. Można zamienić miejsca przejściowego z miejscem produkcyjnym, upewnij się, że miejscem produkcyjnym jest zawsze docelowym gnieździe. W ten sposób aplikację w środowisku produkcyjnym nie ma wpływu na operację wymiany.

### <a name="which-settings-are-swapped"></a>Ustawienia, które zostały zamienione?
Podczas klonowania konfiguracji z innego miejsca wdrożenia sklonowanego konfiguracji jest edytowalny. Ponadto niektóre elementy konfiguracji postępuj zgodnie z zawartości między swap (nie gniazdo określonych), podczas gdy inne elementy konfiguracji, pozostają w tym samym miejscu po swap (gniazdo określone). Poniższej przedstawiono ustawienia, które zmieniają się podczas zamiany miejsca.

**Ustawienia, które zostały zamienione**:

* Ustawienia ogólne — takich jak gniazda sieci Web 32/64-bitowych wersji framework
* Ustawienia aplikacji (można skonfigurować w celu przypisane do miejsca)
* Parametry połączenia (można skonfigurować w celu przypisane do miejsca)
* Mapowania obsługi
* Ustawienia monitorowania i diagnostyki
* Certyfikaty publiczne
* Zawartość usługi WebJobs
* Połączenia hybrydowe *
* Integracja z siecią wirtualną *
* Punkty końcowe usługi *
* Sieć CDN systemu Azure *

Funkcje oznaczone atrybutem * planowane są dokonywane umocowany do gniazda. 

**Ustawienia, które nie są zamienione**:

* Publikowanie punktów końcowych
* Niestandardowe nazwy domen
* Certyfikaty prywatne i powiązania SSL
* Ustawienia skalowania
* Planiści zadań Webjob
* Ograniczenia adresów IP
* Zawsze włączone
* Ustawienia protokołu (HTTP**S**, wersji protokołu TLS, certyfikaty klienta)
* Ustawienia dziennika diagnostycznego
* CORS

<!-- VNET and hybrid connections not yet sticky to slot -->

Aby skonfigurować aplikację ustawienie lub parametrów połączenia do przypisane do określonego miejsca (nie zamienione), przejdź do **konfiguracji** strony dla tego miejsca, dodać lub edytować ustawienia, a następnie wybierz **ustawienie miejsca wdrożenia**pole. Zaznaczenie tego pola wyboru informuje usługę App Service to ustawienie nie są swappable. 

![Ustawienie miejsca](./media/web-sites-staged-publishing/SlotSetting.png)

<a name="Swap"></a>

## <a name="swap-two-slots"></a>Dwa gniazda wymiany 
Można zamienić miejsc wdrożenia w swojej aplikacji **miejsc wdrożenia** strony i **Przegląd** strony. Aby uzyskać szczegółowe informacje techniczne na zamiany gniazda, zobacz [co się dzieje podczas wymiany](#what-happens-during-swap)

> [!IMPORTANT]
> Przed możesz zamienić aplikację na podstawie miejsce wdrożenia do środowiska produkcyjnego, upewnij się, czy produkcyjnym jest Twojego miejsca docelowego, a wszystkie ustawienia w miejscu źródłowym są skonfigurowane dokładnie tak, jak chcesz go w środowisku produkcyjnym.
> 
> 

Można zamienić miejsc wdrożenia, wykonaj następujące kroki:

1. Przejdź do swojej aplikacji **miejsc wdrożenia** strony, a następnie kliknij przycisk **wymiany**.
   
    ![Przycisk wymiany](./media/web-sites-staged-publishing/SwapButtonBar.png)

    **Wymiany** okna dialogowego pokazuje ustawienia w wybranych źródłowym i docelowym gnieździe, które zostaną zmienione.

2. Wybierz żądany **źródła** i **docelowej** miejsc. Zazwyczaj obiekt docelowy jest miejscem produkcyjnym. Ponadto, kliknij przycisk **zmian źródła** i **zmiany docelowego** karty i sprawdź oczekuje zmiany konfiguracji. Po zakończeniu będzie można zamienić gniazd od razu, klikając **wymiany**.

    ![Ukończ zamianę](./media/web-sites-staged-publishing/SwapImmediately.png)

    Aby zobaczyć, jak Twojego miejsca docelowego będzie uruchamiane z nowych ustawień, przed wymiany rzeczywiście się dzieje, nie klikaj **wymiany**, ale postępuj zgodnie z instrukcjami w [wymiany z podglądem](#Multi-Phase).

3. Gdy skończysz, zamknij okno dialogowe, klikając **Zamknij**.

Jeśli napotkasz problemy, zobacz [Rozwiązywanie problemów z zamiany](#troubleshoot-swaps).

<a name="Multi-Phase"></a>

### <a name="swap-with-preview-multi-phase-swap"></a>Zamiana z podglądem (wielofazowych wymiany)

> [!NOTE]
> Wymiany z podglądem nie jest obsługiwane w aplikacjach sieci web w systemie Linux.

Przed zamianą na środowisko produkcyjne jako Gniazdo docelowe, sprawdź poprawność uruchomień aplikacji z ustawieniami zamieniono przed wymiany się dzieje. W miejscu źródłowym jest również przygotowaniu przed ukończeniem wymiany, czyli pożądane dla aplikacji o kluczowym znaczeniu.

Podczas wykonywania wymiany z podglądem usługi App Service wykonuje takie same [Operacja zamiany miejsc](#what-happens-during-swap) ale wstrzymuje po pierwszym krokiem. Następnie należy sprawdzić wynik w miejscu przejściowym przed ukończeniem wymiany. 

Anulowanie wymiany usługi App Service przywrócenie elementów konfiguracji w miejscu źródłowym w miejscu źródłowym.

Aby zamienić z podglądem, wykonaj następujące kroki.

1. Postępuj zgodnie z instrukcjami w [zamiana miejsc wdrożenia](#Swap) ale wybierz **wykonaj wymiany z podglądem**.

    ![Zamień z podglądem](./media/web-sites-staged-publishing/SwapWithPreview.png)

    Okno dialogowe dowiesz się, jak zmiany konfiguracji w miejscu źródłowym w fazie 1 i jak zmienić miejsce źródłowe i docelowe w fazie 2.

2. Po wykonaniu tych czynności można uruchomić wymiany kliknij **Start wymiany**.

    Po zakończeniu fazy 1, otrzymasz powiadomienie w oknie dialogowym. W wersji zapoznawczej wymiany w miejscu źródłowym, przechodząc do `https://<app_name>-<source-slot-name>.azurewebsites.net`. 

3. Po wykonaniu tych czynności do wykonania wymiany oczekujące wybierz **Ukończ zamianę** w **Akcja zamiany** i kliknij przycisk **Ukończ zamianę**.

    Aby anulować oczekujące wymiany, zaznacz **anulowanie wymiany** zamiast i kliknij przycisk **anulowanie wymiany**.

4. Gdy skończysz, zamknij okno dialogowe, klikając **Zamknij**.

Jeśli napotkasz problemy, zobacz [Rozwiązywanie problemów z zamiany](#troubleshoot-swaps).

Aby zautomatyzować wymiany wielofazowych, zobacz Automatyzacja przy użyciu programu PowerShell.

<a name="Rollback"></a>

## <a name="roll-back-swap"></a>Wycofywanie wymiany
Jeśli wystąpią błędy w miejscu docelowym (na przykład z miejscem produkcyjnym) po zamiany przywrócić gniazd ich stany wstępne wymiany przez zamianę tego samego dwa gniazda natychmiast.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Konfigurowanie automatycznej wymiany

> [!NOTE]
> Automatycznej wymiany nie jest obsługiwane w aplikacjach sieci web w systemie Linux.

Automatycznej wymiany usprawnia scenariuszy DevOps, w której chcesz wdrożyć aplikację, stale przy zerowej zimnego startu i braku przestojów dla klientów końcowych w aplikacji. Podczas automatycznej wymiany zostały włączone w gnieździe w środowisku produkcyjnym, każdym wypchnięciu zmiany kodu w tym miejscu usługi App Service automatycznie [zamienia aplikacji w środowisku produkcyjnym](#swap-operation-steps) po jego przygotowaniu, w miejscu źródłowym.

   > [!NOTE]
   > Przed rozpoczęciem konfigurowania automatycznej wymiany do miejsca produkcji, należy rozważyć najpierw testowania automatycznej wymiany w gnieździe docelowym nieprodukcyjnych.
   > 

Aby skonfigurować automatycznej wymiany, wykonaj następujące kroki:

1. Przejdź do strony zasobów aplikacji. Wybierz **miejsc wdrożenia** >  *\<miejsca źródłowego żądaną >*  > **konfiguracji**  >  **Ustawienia ogólne**.
   
2. W **automatycznej wymiany włączone**, wybierz opcję **na**, następnie wybierz gniazdo wybraną docelową w **gniazda wdrożenia wymiany automatycznej**i kliknij przycisk **Zapisz** w pasek poleceń. 
   
    ![](./media/web-sites-staged-publishing/AutoSwap02.png)

3. Wykonaj powiadomienie wypychane kodu w miejscu źródłowym. Automatycznej wymiany się dzieje po pewnym czasie, a aktualizacja jest odzwierciedlona pod adresem URL Twojej docelowym gnieździe.

Jeśli napotkasz problemy, zobacz [Rozwiązywanie problemów z zamiany](#troubleshoot-swaps).

<a name="Warm-up"></a>

## <a name="custom-warm-up"></a>Niestandardowe rozgrzewania
Korzystając z [Auto-Swap](#Auto-Swap), niektóre aplikacje mogą wymagać akcje niestandardowe rozgrzewania przed wymiany. `applicationInitialization` Element konfiguracji w pliku web.config pozwala określić niestandardową inicjalizację akcje do wykonania. [Operacja zamiany miejsc](#what-happens-during-swap) czeka, aż ten niestandardowe rozgrzewania, które należy wykonać przed zamianą z miejscem docelowym. Poniżej przedstawiono przykładowe fragment pliku web.config.

    <system.webServer>
        <applicationInitialization>
            <add initializationPage="/" hostName="[app hostname]" />
            <add initializationPage="/Home/About" hostName="[app hostname]" />
        </applicationInitialization>
    </system.webServer>

Aby uzyskać więcej informacji na temat dostosowywania `applicationInitialization` elementu, zobacz [najbardziej typowe błędy zamiany miejsca wdrożenia i jak je rozwiązać](https://ruslany.net/2017/11/most-common-deployment-slot-swap-failures-and-how-to-fix-them/).

Można również dostosować zachowanie rozgrzewania z co najmniej jeden z następujących [ustawienia aplikacji](configure-common.md):

- `WEBSITE_SWAP_WARMUP_PING_PATH`: Ścieżka do polecenia ping rozgrzanie witryny. Dodaj ustawienie aplikacji, określając niestandardową ścieżkę, która rozpoczyna się z ukośnika jako wartość. Na przykład `/statuscheck`. Wartość domyślna to `/`. 
- `WEBSITE_SWAP_WARMUP_PING_STATUSES`: Prawidłowe kody odpowiedzi HTTP dla operacji rozgrzewania. Dodaj tego ustawienia aplikacji zawierającego rozdzielaną przecinkami listę kody HTTP. Na przykład: `200,202` . Jeśli zwrócony kod stanu nie jest na liście, operacje rozgrzewania i wymiany są zatrzymywane. Domyślnie wszystkie kody odpowiedzi są prawidłowe.

Jeśli napotkasz problemy, zobacz [Rozwiązywanie problemów z zamiany](#troubleshoot-swaps).

## <a name="monitor-swap"></a>Monitor wymiany

Jeśli [Operacja zamiany miejsc](#what-happens-during-swap) zajmuje dużo czasu, można uzyskać informacji na temat operacji wymiany [dziennika aktywności](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Na stronie zasobów aplikacji w portalu w obszarze nawigacji po lewej stronie, wybierz **dziennika aktywności**.

Operacja zamiany pojawia się w dzienniku kwerendy w postaci `Swap Web App Slots`. Można rozwinąć i wybierz jedną z podrzędnych operacji lub błędy, aby wyświetlić szczegóły.

## <a name="route-traffic"></a>Kierowanie ruchem

Domyślnie, wszystkie żądania klientów do aplikacji Produkcja — adres URL (`http://<app_name>.azurewebsites.net`) są kierowane do miejsca produkcji. Można kierować część ruchu do innego miejsca. Ta funkcja jest przydatna, jeśli potrzebujesz opinie użytkowników nowych aktualizacji, ale nie jesteś gotowy do wydania go do środowiska produkcyjnego.

### <a name="route-production-traffic-automatically"></a>Automatycznie kierować ruch produkcyjny

Aby automatycznie kierować ruch produkcyjny, wykonaj następujące kroki:

1. Przejdź do strony zasobów aplikacji i wybierz **miejsc wdrożenia**.

2. W **% ruchu** kolumny gniazda, którą chcesz rozesłać do, wartość procentowa (od 0 do 100) do reprezentowania ilości całkowitego ruchu, które chcesz rozesłać. Kliknij pozycję **Zapisz**.

    ![](./media/web-sites-staged-publishing/RouteTraffic.png)

Po zapisaniu ustawienia określonego odsetka klientów losowo odbywa się w miejscu innym niż produkcyjne. 

Gdy klient automatycznie odbywa się w określonym miejscu jego "przypiętych" do tego miejsca dla cyklu życia tej sesji klienta. W przeglądarce klienta widać miejsca, które sesja jest przypięta do, analizując `x-ms-routing-name` plików cookie w nagłówkach HTTP. Żądanie, które odbywa się w miejscu "tymczasową" zawiera plik cookie `x-ms-routing-name=staging`. Żądanie, które jest kierowany do miejsca produkcji ma plik cookie `x-ms-routing-name=self`.

### <a name="route-production-traffic-manually"></a>Ręcznie przekierować ruch produkcji

Oprócz routingu ruchu automatyczne usługi App Service może kierować żądania do określonego miejsca. Jest to przydatne, jeśli chcesz, aby użytkownicy mogli zoptymalizowany pod kątem do lub zrezygnować z aplikacji w wersji beta. Aby ręcznie przekierować ruch produkcyjny, należy użyć `x-ms-routing-name` parametr zapytania.

Aby umożliwić użytkownikom zrezygnować z aplikacji w wersji beta, na przykład, można umieścić tego linku na stronie sieci web:

```HTML
<a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>
```

Ciąg `x-ms-routing-name=self` określa miejscem produkcyjnym. Gdy przeglądarka klienta uzyskuje dostęp do łącza, nie tylko jest przekierowywany do miejsca produkcji, ale każde kolejne żądanie ma `x-ms-routing-name=self` pliku cookie, który Przypina sesji do miejsca produkcji.

Aby umożliwić użytkownikom korzystania z aplikacji w wersji beta, należy ustawić ten sam parametr zapytania na nazwę miejsca nieprodukcyjne, na przykład:

```
<webappname>.azurewebsites.net/?x-ms-routing-name=staging
```

Domyślnie nowe miejsc otrzymują reguły routingu `0%`, jak pokazano w kolorze szarym. Poprzez jawne ustawienie tej wartości `0%` (pokazano czarny tekst), użytkownicy mogą uzyskiwać dostęp miejsca przejściowego ręcznie przy użyciu `x-ms-routing-name` parametr zapytania, ale będzie nie być kierowane do miejsca automatycznie ponieważ procent routingu jest równa 0. Jest to zaawansowany scenariusz, w którym można "ukryć" Twojego miejsca przejściowego publicznie, pozwalając zespołom wewnętrznym przetestować zmiany w gnieździe.

<a name="Delete"></a>

## <a name="delete-slot"></a>Usuń gniazdo

Przejdź do strony zasobów aplikacji. Wybierz **miejsc wdrożenia** >  *\<miejsca, aby usunąć >*  > **Przegląd**. Kliknij przycisk **Usuń** na pasku poleceń.  

![Usuń miejsce wdrożenia](./media/web-sites-staged-publishing/DeleteStagingSiteButton.png)

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-powershell"></a>Automatyzacja przy użyciu programu PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure PowerShell to moduł udostępniający polecenia cmdlet do zarządzania platformy Azure za pomocą programu Windows PowerShell, łącznie z obsługą zarządzania miejscami wdrożenia w usłudze Azure App Service.

Aby uzyskać informacje na temat instalowania i konfigurowania programu Azure PowerShell, na temat uwierzytelniania programu Azure PowerShell z subskrypcją platformy Azure, zobacz [jak zainstalować i skonfigurować program Microsoft Azure PowerShell](/powershell/azure/overview).  

---
### <a name="create-web-app"></a>Tworzenie aplikacji internetowej
```powershell
New-AzWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

---
### <a name="create-slot"></a>Utwórz miejsce
```powershell
New-AzWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

---
### <a name="initiate-swap-with-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a>Zainicjuj wymiany z podglądem (obszar wymiany wielofazowych) i Zastosuj konfigurację miejsca docelowego do miejsca źródłowego
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

---
### <a name="cancel-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Anuluj oczekujące obszaru wymiany (obszar wymiany z przeglądem) oraz przywracanie konfiguracji miejsca źródłowego
```powershell
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

---
### <a name="swap-deployment-slots"></a>Zamiana miejsc wdrożenia
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Monitorowanie wymiany zdarzeń w dzienniku aktywności
```powershell
Get-AzLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

---
### <a name="delete-slot"></a>Usuń gniazdo
```powershell
Remove-AzResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

---
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-cli"></a>Automatyzacja zadań za pomocą interfejsu wiersza polecenia

Aby uzyskać [wiersza polecenia platformy Azure](https://github.com/Azure/azure-cli) poleceń dla miejsc wdrożenia, zobacz [miejsce wdrożenia aplikacji sieci Web az](/cli/azure/webapp/deployment/slot).

## <a name="troubleshoot-swaps"></a>Rozwiązywanie problemów z zamiany

Jeśli jakikolwiek błąd wystąpi podczas [zamiany](#what-happens-during-swap), są rejestrowane w *D:\home\LogFiles\eventlog.xml*, a także dziennik błędów specyficznych dla aplikacji.

Poniżej przedstawiono niektóre typowe błędy wymiany:

- Jest upłynął limit czasu żądania HTTP do katalogu głównego aplikacji. Operacja zamiany czeka 90 sekund dla każdego żądania HTTP i ponownych prób do 5 razy. Jeśli są Przekroczono limit czasu ponawiania prób, operacja zamiany zostało przerwane.

- Inicjowanie lokalnej pamięci podręcznej może zakończyć się niepowodzeniem, gdy zawartość aplikacji przekracza limit przydziału dysku lokalnego, określony dla lokalnej pamięci podręcznej. Aby uzyskać więcej informacji, zobacz [omówienie lokalnej pamięci podręcznej](overview-local-cache.md).

- Podczas [niestandardowe rozgrzewania](#custom-warm-up)żądania HTTP są wykonywane wewnętrznie (bez pośrednictwa zewnętrzny adres URL) i może zakończyć się niepowodzeniem z określonych adresów URL reguły ponownego zapisywania w *Web.config*. Na przykład reguły przekierowywania nazwy domeny lub Wymuszanie protokołu HTTPS uniemożliwia żądania rozgrzewania docieranie do kodu aplikacji w ogóle. Aby obejść ten problem, zmodyfikuj swoje reguły ponownego zapisywania, dodając następujące dwa warunki:

    ```xml
    <conditions>
      <add input="{WARMUP_REQUEST}" pattern="1" negate="true" />
      <add input="{REMOTE_ADDR}" pattern="^100?\." negate="true" />
      ...
    </conditions>
    ```
- Bez niestandardowych rozgrzewania żądań HTTP mogą nadal zostać wstrzymane do czasu przez reguły ponownego zapisywania adresu URL. Aby obejść ten problem, zmodyfikuj swoje reguły ponownego zapisywania, dodając następujący warunek:

    ```xml
    <conditions>
      <add input="{REMOTE_ADDR}" pattern="^100?\." negate="true" />
      ...
    </conditions>
    ```
- Niektóre [zasady ograniczeń adresów IP](app-service-ip-restrictions.md) może uniemożliwić wysyłania żądań HTTP do swojej aplikacji operacji wymiany. Zakresy adresów IPv4 rozpoczynające się od `10.` i `100.` są wewnętrzne dla danego wdrożenia, a powinien być dozwolony, aby nawiązać połączenie z aplikacją.

## <a name="next-steps"></a>Kolejne kroki
[Blokuj dostęp do gniazda nieprodukcyjnych](app-service-ip-restrictions.md)
