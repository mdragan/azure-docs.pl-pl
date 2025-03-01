---
title: Usługa Azure Functions, skalowanie i hosting | Dokumentacja firmy Microsoft
description: Dowiedz się, jak dokonać wyboru między planu usługi Azure Functions zużycie i plan w warstwie Premium.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, funkcje, planu zużycie, plan w warstwie premium, przetwarzanie zdarzeń, elementów webhook, obliczanie dynamiczne, architektury bezserwerowej
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 03/27/2019
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3253cc7e379ae63880d533f14bc76e7af5a4425a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050549"
---
# <a name="azure-functions-scale-and-hosting"></a>Funkcje Azure podlegają skalowaniu i hosting

Po utworzeniu aplikacji funkcji na platformie Azure, musisz wybrać plan hostingu aplikacji. Dostępne są trzy plany hostingu dla usługi Azure Functions: [Plan zużycie](#consumption-plan), [plan w warstwie Premium](#premium-plan), i [planu usługi App Service](#app-service-plan).

Plan hostingu, które wybierzesz decyduje o następujące zachowania:

* Jak jest skalowana aplikację funkcji.
* Zasoby dostępne dla każdego wystąpienia aplikacji funkcji.
* Obsługa zaawansowanych funkcji, takich jak połączenie między SIECIAMI.

Plany Premium i użycie automatycznie dodają moc obliczeniową, gdy kod jest uruchomiony. Twoja aplikacja jest skalowana w poziomie, gdy trzeba obsłużyć obciążenie i skalowane w dół, gdy kod przestanie działać. Dla tego planu zużycie również nie trzeba płacić za bezczynnych maszyn wirtualnych lub zarezerwować pojemności z wyprzedzeniem.  

Plan w warstwie Premium zapewnia dodatkowe funkcje, takie jak premium wystąpień obliczeniowych, możliwość przechowywania bez wyłączania zasilania wystąpień na czas nieokreślony oraz połączenie między sieciami.

Plan usługi App Service umożliwia korzystanie z zalet dedykowana infrastruktura, którym zarządzasz. Aplikacja funkcji skalowania na podstawie zdarzeń, co oznacza, że nigdy nie jest skalowana do zera. (Wymaga, aby [zawsze na](#always-on) jest włączony.)

> [!NOTE]
> Możesz przełączać się między planów zużycia i Premium, zmieniając właściwości planu zasobu aplikacji funkcji.

## <a name="hosting-plan-support"></a>Obsługa plan hostingu

Obsługa funkcji znajduje się na dwie następujące kategorie:

* _Ogólnie dostępna (GA)_ : w pełni obsługiwane i zatwierdzone do użycia w środowisku produkcyjnym.
* _Podgląd_: nie zostały jeszcze w pełni obsługiwane i zatwierdzone do użycia w środowisku produkcyjnym.

Poniższa tabela wskazuje bieżący poziom obsługi trzy plany hostingu, podczas uruchamiania w systemie Windows lub Linux:

| | Plan Zużycie | Plan Premium | Dedykowanego planu |
|-|:----------------:|:------------:|:----------------:|
| Windows | Ogólna dostępność | wersja zapoznawcza | Ogólna dostępność |
| Linux | wersja zapoznawcza | ND | Ogólna dostępność |

## <a name="consumption-plan"></a>Plan Zużycie

Podczas korzystania z planu zużycie wystąpień hosta usługi Azure Functions są dynamicznie dodawane i usuwane na podstawie liczby zdarzeń przychodzących. Ten plan bez użycia serwera skaluje się automatycznie, a opłaty są naliczane za zasoby obliczeniowe tylko wtedy, gdy funkcje są uruchomione. W ramach planu zużycie wykonywania funkcji upłynie limit czasu po upływie czasu określonego można konfigurować.

Rozliczanie jest na podstawie liczby wykonań, czas wykonywania i używanej pamięci. Opłaty są agregowane w obrębie wszystkich funkcji w ramach aplikacji funkcji. Aby uzyskać więcej informacji, zobacz [usługi Azure Functions stronę z cennikiem](https://azure.microsoft.com/pricing/details/functions/).

Plan zużycie jest domyślny plan hostingu i zapewnia następujące korzyści:

* Płać tylko, gdy funkcje są uruchomione
* Skalować automatycznie, nawet w okresach wysokiego obciążenia

Aplikacje funkcji, w tym samym regionie można przypisać do tego samego planu zużycie. Brak wadą i wpływu na posiadanie wielu aplikacji w ten sam plan zużycie. Przypisywanie wielu aplikacji do tego samego planu zużycie nie ma wpływu na odporności, skalowalności ani niezawodności każdej aplikacji.

## <a name="premium-plan"></a>Plan w warstwie Premium (wersja zapoznawcza)

Podczas korzystania z planu Premium wystąpień hosta usługi Azure Functions są dodawane i usuwane na podstawie liczby zdarzeń przychodzących, podobnie jak w planie zużycie.  Plan w warstwie Premium obsługuje następujące funkcje:

* Bezterminowo ciepło wystąpienia, aby uniknąć wszelkich zimny start
* Połączenie między sieciami
* Czas trwania wykonywania bez ograniczeń
* Rozmiary wystąpień — wersja Premium (jeden rdzeń, dwa podstawowe i cztery podstawowego wystąpienia)
* Bardziej przewidywalne ceny
* Alokacja aplikacji o wysokiej gęstości w przypadku planów w wielu aplikacjach — funkcja

Informacje, w jaki sposób można skonfigurować te opcje można znaleźć w [dokument planu premium usługi Azure Functions](functions-premium-plan.md).

Zamiast rozliczeń za wykonywanie i zużywanej pamięci, opłata plan w warstwie Premium opiera się na liczbie sekundy pracy rdzenia, czas wykonywania i pamięć używana w wymaganych i zarezerwowanych wystąpieniach.  Co najmniej jedno wystąpienie musi być ciepło at cały czas. Oznacza to, że istnieje stały koszt miesięczny aktywnego planu, niezależnie od liczby wykonań.

Należy wziąć pod uwagę plan w warstwie premium usługi Azure Functions w następujących sytuacjach:

* Twoje aplikacje funkcji Uruchom stale albo w praktycznie w sposób ciągły.
* Potrzebujesz więcej opcji procesora CPU lub pamięci niż dostarczanych przez planu zużycie.
* Twój kod musi zostać uruchomiony dłużej niż [maksymalny dozwolony czas wykonywania](#timeout) w planu zużycie.
* Wymagane funkcje, które są dostępne tylko na plan w warstwie Premium, takich jak łączność w sieci Wirtualnej/sieci VPN.

> [!NOTE]
> Podgląd planu premium obecnie obsługuje tylko usługi Azure Functions na Windows.

Podczas uruchamiania funkcji języka JavaScript w ramach planu Premium, należy wybrać wystąpienie, który ma mniejszą liczbę procesorów wirtualnych. Aby uzyskać więcej informacji, zobacz [wybierz plany Premium jednordzeniowy](functions-reference-node.md#considerations-for-javascript-functions).  

## <a name="app-service-plan"></a>Planowanie dedykowanej (usługi App Service)

Twoje aplikacje funkcji można również uruchomić na tym samym dedykowanych maszyn wirtualnych jako inne aplikacje usługi App Service (Basic, Standard, Premium i jednostek SKU izolowanej).

Należy wziąć pod uwagę plan usługi App Service w następujących sytuacjach:

* Masz istniejące niedostatecznie używanych maszyn wirtualnych, które zostały już uruchomione inne wystąpienia usługi App Service.
* Chcesz zapewnić niestandardowego obrazu, na którym chcesz uruchamiać swoje funkcje.

Płaci się takie same dla aplikacji funkcji w planie usługi App Service tak jak w przypadku innych zasobów usługi App Service, takich jak aplikacje sieci web. Aby uzyskać szczegółowe informacje dotyczące sposobu działania planu usługi App Service, zobacz [szczegółowe omówienie planów usługi Azure App Service](../app-service/overview-hosting-plans.md).

Za pomocą planu usługi App Service można ręcznie skalować w poziomie, dodając kolejne wystąpienia maszyn wirtualnych. Można również włączyć Skalowanie automatyczne. Aby uzyskać więcej informacji, zobacz [ręczne lub automatyczne skalowanie liczby wystąpień](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). Możesz również skalować w górę, wybierając inny plan usługi App Service. Aby uzyskać więcej informacji, zobacz [skalowanie aplikacji na platformie Azure](../app-service/web-sites-scale.md). 

Podczas uruchamiania funkcji JavaScript na plan usługi App Service, należy wybrać plan, który ma mniejszą liczbę procesorów wirtualnych. Aby uzyskać więcej informacji, zobacz [wybierz plany usługi App Service jednordzeniowy](functions-reference-node.md#choose-single-vcpu-app-service-plans). 
<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 

### <a name="always-on"></a> Zawsze włączone

Jeśli zostanie uruchomione na plan usługi App Service, należy włączyć **zawsze na** ustawienie, aby aplikacja funkcji zostanie uruchomiona poprawnie. Plan usługi App Service środowisko uruchomieniowe usługi functions zacznie bezczynności, po kilku minutach braku aktywności, dzięki czemu tylko wyzwalaczy HTTP "wzbudzania" funkcji. Zawsze na jest dostępna tylko na plan usługi App Service. W ramach planu zużycie platformy aktywuje automatycznie aplikacji funkcji.

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]


Nawet w przypadku zawsze na włączone, limit czasu wykonywania poszczególnych funkcji jest kontrolowana przez `functionTimeout` w [host.json](functions-host-json.md#functiontimeout) pliku projektu.

## <a name="determine-the-hosting-plan-of-an-existing-application"></a>Określić plan hostingu istniejącej aplikacji

Aby określić plan hostingu używane przez aplikację funkcji, zobacz **planu usługi App Service / warstwa cenowa** w **Przegląd** karty dla aplikacji funkcji w [witryny Azure portal](https://portal.azure.com). Dla planów usługi App Service wskazane jest także warstwy cenowej.

![Wyświetl plan skalowania w portalu](./media/functions-scale/function-app-overview-portal.png)

Interfejs wiersza polecenia platformy Azure umożliwia również określić plan, w następujący sposób:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

Gdy dane wyjściowe tego polecenia jest `dynamic`, aplikacja funkcji znajduje się w planie zużycie. Gdy dane wyjściowe tego polecenia jest `ElasticPremium`, aplikacja funkcji jest plan w warstwie Premium. Wszystkie pozostałe wartości wskazują różne warstwy plan usługi App Service.

## <a name="storage-account-requirements"></a>Wymagania konta magazynu

W dowolnym planie aplikacja funkcji wymaga konto usługi Azure Storage ogólnego, który obsługuje usługi Azure Blob, kolejki, pliki i Table storage. Jest to spowodowane funkcje dla operacji, takich jak Zarządzanie wyzwalaczami i rejestrowanie wykonań funkcji polegać na usłudze Azure Storage, ale niektóre konta magazynu nie obsługują kolejek i tabel. Te konta, które obejmują kont usługi storage tylko do obiektów blob (w tym usługi premium storage) i kont magazynu ogólnego przeznaczenia z replikacją magazynu strefowo nadmiarowego, są filtrowane w poziomie z istniejących **konta magazynu** wybrane opcje, po utworzeniu aplikacji funkcji.

<!-- JH: Does using a Premium Storage account improve perf? -->

Aby dowiedzieć się więcej na temat typów kont magazynu, zobacz [wprowadzenie do usługi Azure Storage](../storage/common/storage-introduction.md#azure-storage-services).

## <a name="how-the-consumption-and-premium-plans-work"></a>Jak działają planów zużycia i premium

W przypadku użycia i plany premium infrastruktury usługi Azure Functions umożliwia skalowanie zasobów Procesora i pamięci przez dodanie dodatkowych wystąpień hosta funkcji, w oparciu o liczbę zdarzeń, które funkcje są wyzwalane na. Każde wystąpienie hosta funkcji w planie zużycie jest ograniczona do 1,5 GB pamięci i procesora CPU w jednym.  Wystąpienie hosta jest aplikacja całą funkcję, co oznacza wszystkich funkcji w ramach funkcji zasób udziału dla aplikacji w ramach wystąpienia i skalowania, w tym samym czasie. Aplikacje funkcji, które współużytkują ten sam plan zużycie są skalowane niezależnie.  W planie premium rozmiar planu określają ilość dostępnej pamięci i procesora CPU dla wszystkich aplikacji w tym planie, w tym wystąpieniu.  

Pliki kodu funkcji są przechowywane w udziałach plików platformy Azure przez funkcję głównego konta magazynu. Po usunięciu głównego konta magazynu aplikacji funkcji w plikach kodu funkcji zostaną usunięte i nie można odzyskać.

> [!NOTE]
> Podczas korzystania z wyzwalacz obiektu blob w ramach planu zużycie, może istnieć maksymalnie 10 minut opóźnienia w przetwarzaniu nowe obiekty BLOB. To opóźnienie występuje, gdy aplikacja funkcji przeszedł bezczynności. Po uruchomieniu aplikacji funkcji, obiekty BLOB są przetwarzane od razu. Aby uniknąć tego opóźnienia zimnego startu, przy użyciu planu w warstwie Premium lub [wyzwalacza usługi Event Grid](functions-bindings-event-grid.md). Aby uzyskać więcej informacji, zobacz [artykuł odwołanie do obiektu blob wyzwalacza powiązania](functions-bindings-storage-blob.md#trigger).

### <a name="runtime-scaling"></a>Skalowanie środowiska uruchomieniowego

Usługa Azure Functions używa składnik o nazwie *kontrolera skalowania* monitorować współczynnik zdarzeń i ustalenia, czy należy skalować w poziomie lub w. Kontroler skalowania korzysta z algorytmów heurystycznych dla każdego typu wyzwalacza. Na przykład podczas korzystania z wyzwalacza usługi Azure Queue storage, zostanie przeprowadzone skalowanie na podstawie długości kolejki i wiek najstarsze komunikatu w kolejce.

Jednostka skalowania dla usługi Azure Functions jest aplikacja funkcji. Gdy aplikacja funkcji jest skalowana w poziomie, dodatkowe zasoby są przydzielane do uruchamiania wielu wystąpień hosta usługi Azure Functions. Z drugiej strony jak moc obliczeniowa, czy żądanie jest krótszy, kontroler skalowania usuwa wystąpień hosta funkcji. Liczba wystąpień jest ostatecznie skalować w dół od zera. gdy żadne funkcje nie są uruchomione w ramach aplikacji funkcji.

![Kontroler skalowania zdarzenia dotyczące monitorowania i tworzenia wystąpień](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Opis zachowania skalowania

Skalowanie może się różnić od szeregu czynników, a inaczej w zależności od języka wybranego i wyzwalaczy skalowania. Istnieje kilka niewymagającego skalowania zachowań pod uwagę:

* Pojedynczą aplikację funkcji można skalować w górę do maksymalnie 200 wystąpień. Pojedyncze wystąpienie może przetwarzać więcej niż jednego komunikatu lub żądania naraz, więc nie ma ustawiony limit na liczbę współbieżnych wykonań.
* Wyzwalaczy HTTP nowe wystąpienia zostaną tylko przydzielone co najwyżej raz na 1 sekundę.
* Wyzwalaczy protokołu HTTP nowe wystąpienia zostaną tylko przydzielone co najwyżej raz na 30 sekund.

Różnymi wyzwalaczami mogą także mieć różne, w ograniczonym zakresie, a także udokumentowanego poniżej:

* [Centrum zdarzeń](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>Najlepsze rozwiązania i wzorce skalowalne aplikacje

Istnieje wiele aspektów aplikacji funkcji, które mają wpływ na sposób również zostanie przeprowadzone skalowanie, łącznie z konfiguracją hosta, obciążenie środowiska uruchomieniowego i wydajności zasobów.  Aby uzyskać więcej informacji, zobacz [skalowalności w dalszej części artykułu zagadnienia dotyczące wydajności](functions-best-practices.md#scalability-best-practices). Należy także pamiętać o zachowanie połączenia miarę skalowania aplikacji funkcji. Aby uzyskać więcej informacji, zobacz [sposób zarządzania połączeniami w usłudze Azure Functions](manage-connections.md).

### <a name="billing-model"></a>Model rozliczania

Rozliczenia dla różnych planów został szczegółowo opisany na [usługi Azure Functions stronę z cennikiem](https://azure.microsoft.com/pricing/details/functions/). Sposób użycia są agregowane na poziomie aplikacji funkcji i zlicza czas, który jest wykonywany kod funkcji. Jednostki do rozliczeń są następujące:

* **Użycie zasobów w gigabajtosekundach (GB-s)** . Obliczona jako rozmiar pamięci i czas wykonywania dla wszystkich funkcji w ramach aplikacji funkcji. 
* **Liczba wykonań**. Zliczane każdorazowo, gdy funkcja jest wykonywana w odpowiedzi na wyzwalacz zdarzenia.

Przydatne zapytania i uzyskać informacje na temat opis zawartości rachunku użycia można znaleźć [na często zadawane pytania rozliczeń](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ).

[Azure Functions pricing page]: https://azure.microsoft.com/pricing/details/functions

## <a name="service-limits"></a>Limity usługi

Poniższa tabela wskazuje ograniczeń, które mają zastosowanie do aplikacji funkcji, podczas pracy w różnych planów hostingu:

[!INCLUDE [functions-limits](../../includes/functions-limits.md)]
