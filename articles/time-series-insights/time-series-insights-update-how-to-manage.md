---
title: Aprowizowanie i zarządzanie nimi serii czasu Azure w wersji zapoznawczej | Dokumentacja firmy Microsoft
description: Zrozumienie, jak aprowizować i zarządzać nimi Azure czas Series Insights w wersji zapoznawczej.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: 6251df2317ceff9dded92f2d829bfab0503fdf1b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237590"
---
# <a name="provision-and-manage-azure-time-series-insights-preview"></a>Aprowizowanie i zarządzanie nimi Azure czas Series Insights w wersji zapoznawczej

W tym artykule opisano sposób tworzenia i zarządzanie środowiskiem Azure czas Series Insights w wersji zapoznawczej za pomocą [witryny Azure portal](https://portal.azure.com/).

## <a name="overview"></a>Omówienie

Środowiska czasu Series Insights w wersji zapoznawczej platformy Azure są środowisk płatność za rzeczywiste użycie (PAYG).

Podczas aprowizacji środowiska Azure czas Series Insights w wersji zapoznawczej, jest utworzenie dwóch zasobów platformy Azure:

* Środowisko Azure czas Series Insights w wersji zapoznawczej  
* Konta ogólnego przeznaczenia w wersji 1 usługi Azure Storage
  
Dowiedz się, [jak Planowanie środowiska](./time-series-insights-update-plan.md).

>[!IMPORTANT]
> W wersji zapoznawczej, upewnij się, korzystania z platformy Azure magazynu ogólnego przeznaczenia w wersji 1 (GPv1) konta.

Opcjonalnie można skojarzyć każde środowisko Azure czas Series Insights w wersji zapoznawczej z źródła zdarzeń. Aby uzyskać więcej informacji, przeczytaj [dodawania źródła zdarzeń Centrum](./time-series-insights-how-to-add-an-event-source-eventhub.md) i [Dodawanie źródła Centrum IoT](./time-series-insights-how-to-add-an-event-source-iothub.md). W tym kroku możesz podać właściwość Identyfikatora sygnatury czasowej i grupy unikatowych odbiorców. Daje to gwarancję, że środowisko ma dostęp do odpowiedniego zdarzenia.

Po ukończeniu inicjowania obsługi administracyjnej można zmodyfikować zasady dostępu i inne atrybuty środowiska do własnych potrzeb biznesowych.

## <a name="create-the-environment"></a>Utwórz środowisko

W poniższych krokach opisano sposób tworzenia środowiska Azure czas Series Insights w wersji zapoznawczej:

1. Wybierz **PAYG** przycisku w obszarze **jednostki SKU** menu. Podaj nazwę środowiska, a następnie wybierz grupę, do której subskrypcję i grupę zasobów do użycia. Następnie wybierz dla środowiska hostowane w obsługiwanej lokalizacji.

   [![Utwórz wystąpienie usługi Azure Time Series Insights.](media/v2-update-manage/manage_three.PNG)](media/v2-update-manage/manage_three.PNG#lightbox)

1. Wprowadź szeregów czasowych identyfikatora.

    >[!NOTE]
    > * Identyfikator serii czasu jest rozróżniana wielkość liter i niezmienne. (Nie można zmienić po jego ustawieniu.)
    > * Identyfikatory serii czasu może być maksymalnie trzy klucze.
    > * Aby uzyskać więcej informacji o wybieraniu Identyfikatora serii czasu, przeczytaj [wybierz identyfikator serii czasu](./time-series-insights-update-how-to-id.md).

1. Utwórz konto usługi Azure storage, wybierając nazwę konta magazynu i wyznaczanie wybór replikacji. Wykonując to automatycznie tworzy konto usługi Azure Storage ogólnego przeznaczenia w wersji 1. Zostanie utworzony w tym samym regionie, co środowisko Azure czas Series Insights w wersji zapoznawczej, która została wybrana wcześniej.

    [![Utwórz konto magazynu platformy Azure dla swojego wystąpienia](media/v2-update-manage/manage_five.PNG)](media/v2-update-manage/manage_five.PNG#lightbox)

1. Opcjonalnie można dodać źródła zdarzenia.

   * Time Series Insights obsługuje [usługi Azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) i [usługi Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) jako opcje. Mimo że można dodawać tylko źródło pojedynczego zdarzenia w czasie tworzenia środowiska, później można dodać innego źródła zdarzeń. Zaleca się, można utworzyć grupy konsumentów unikatowy, aby upewnić się, że wszystkie zdarzenia są widoczne dla swojego wystąpienia usługi Azure czas Series Insights w wersji zapoznawczej. Można wybrać istniejącą grupę odbiorców lub Utwórz nową grupę odbiorców podczas dodawania źródła zdarzeń.

   * Należy również wybrać odpowiednie właściwości sygnatury czasowej. Domyślnie usługa Azure Time Series Insights używa czas umieszczonych w kolejce komunikatu dla każdego źródła zdarzeń.

     > [!TIP]
     > Czas umieszczonych w kolejce komunikatów może nie być najlepiej skonfigurowane ustawienie do użycia w partii zdarzeń lub danych historycznych, przekazywanie scenariuszy. Upewnij się, że zdecydować, czy użycie lub nie właściwość sygnatury czasowej w takich przypadkach.

     [![Karta źródła zdarzeń](media/v2-update-manage/manage_two.PNG)](media/v2-update-manage/manage_two.PNG#lightbox)

1. Upewnij się, że środowisko został aprowizowany przy użyciu odpowiednich ustawień.

    [![Przeglądanie + Tworzenie karty](media/v2-update-manage/manage_three.PNG)](media/v2-update-manage/manage_three.PNG#lightbox)

## <a name="manage-the-environment"></a>Zarządzać środowiskiem

Środowiska Azure czas Series Insights w wersji zapoznawczej można zarządzać za pomocą witryny Azure portal. Oto najważniejsze różnice w zarządzaniu PAYG Azure czas Series Insights w wersji zapoznawczej środowiska, w przeciwieństwie do S1 lub S2 środowiska, za pomocą witryny Azure portal:

* Witryny Azure portal **Przegląd** bloku są takie same jak w usłudze Azure Time Series Insights, z wyjątkiem w następujący sposób:
  * Pojemność zostanie usunięty, ponieważ nie jest odpowiedni dla środowisk PAYG tę koncepcję.
  * Dodano właściwość ID serii czasu. Ustala, jak Twoje dane są podzielone na partycje.
  * Zestawy danych referencyjnych są usuwane.
  * Wyświetlany adres URL kieruje użytkownika do [Azure czas Series Insights w wersji zapoznawczej Eksplorator](./time-series-insights-update-explorer.md).
  * Znajduje się nazwa konta magazynu platformy Azure.

* Witryny Azure portal **Konfiguruj** bloku zostały usunięte w wersji zapoznawczej Azure czas serii szczegółowe informacje, ponieważ PAYG środowiska nie są konfigurowalne.

* Witryny Azure portal **dane referencyjne** bloku zostały usunięte w wersji zapoznawczej Azure czas serii szczegółowe informacje, ponieważ dane referencyjne nie jest składnikiem PAYG środowisk.

[![Środowisko usługi Time Series Insights w wersji zapoznawczej w witrynie Azure portal](media/v2-update-manage/manage_four.PNG)](media/v2-update-manage/manage_four.PNG#lightbox)

## <a name="next-steps"></a>Kolejne kroki

- Odczyt [Planowanie środowiska](./time-series-insights-update-plan.md).

- Dowiedz się, jak [dodawania źródła zdarzeń Centrum](./time-series-insights-how-to-add-an-event-source-eventhub.md).

- Konfigurowanie [źródłem Centrum IoT](./time-series-insights-how-to-add-an-event-source-iothub.md).