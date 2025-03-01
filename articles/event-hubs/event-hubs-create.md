---
title: Przewodnik Szybki start platformy Azure — tworzenie centrum zdarzeń przy użyciu witryny Azure Portal | Microsoft Docs
description: Z tego przewodnika Szybki start dowiesz się, jak utworzyć centrum zdarzeń platformy Azure przy użyciu witryny Azure Portal oraz wysyłać i odbierać zdarzenia za pomocą zestawu .NET Standard SDK.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/23/2019
ms.author: shvija
ms.openlocfilehash: 15b2b5863f5b78bd24ffd8cbd55b9914f52e32a1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072792"
---
# <a name="quickstart-create-an-event-hub-using-azure-portal"></a>Szybki start: tworzenie centrum zdarzeń za pomocą witryny Azure Portal
Azure Event Hubs to platforma do pozyskiwania i strumieniowego przesyłania danych, która umożliwia odbieranie i przetwarzanie milionów zdarzeń na sekundę. Usługa Event Hubs pozwala przetwarzać i przechowywać zdarzenia, dane lub dane telemetryczne generowane przez rozproszone oprogramowanie i urządzenia. Dane wysłane do centrum zdarzeń mogą zostać przekształcone i zmagazynowane przy użyciu dowolnego dostawcy analityki czasu rzeczywistego lub adapterów przetwarzania wsadowego/magazynowania. Aby zapoznać się ze szczegółowym omówieniem usługi Event Hubs, zobacz [Omówienie usługi Event Hubs](event-hubs-about.md) i [Funkcje usługi Event Hubs](event-hubs-features.md).

W tym przewodniku Szybki start utworzysz centrum zdarzeń za pomocą witryny [Azure Portal](https://portal.azure.com).

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik Szybki start, upewnij się, że dysponujesz następującymi elementami:

- Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).
- [Visual Studio 2019)](https://www.visualstudio.com/vs) lub nowszej.
- [Zestaw .NET Standard SDK](https://www.microsoft.com/net/download/windows) w wersji 2.0 lub nowszej.

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Grupa zasobów to logiczna kolekcja zasobów platformy Azure. Wszystkie zasoby są wdrażane i zarządzane w ramach grupy zasobów. Aby utworzyć grupę zasobów:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W lewym obszarze nawigacji kliknij pozycję **Grupy zasobów**. Następnie kliknij pozycję **Dodaj**.

   ![Grupy zasobów — przycisk Dodaj](./media/event-hubs-quickstart-portal/resource-groups1.png)

2. W polu **Subskrypcja** wybierz nazwę subskrypcji platformy Azure, w której chcesz utworzyć grupę zasobów.
3. Wpisz unikatową **nazwę grupy zasobów**. System natychmiast sprawdzi, czy nazwa jest dostępna w aktualnie wybranej subskrypcji platformy Azure.
4. Wybierz **region** dla grupy zasobów.
5. Wybierz pozycję **Przegląd + utwórz**.

   ![Grupa zasobów — tworzenie](./media/event-hubs-quickstart-portal/resource-groups2.png)
6. Na stronie **Przeglądanie + tworzenie** wybierz pozycję **Utwórz**. 

## <a name="create-an-event-hubs-namespace"></a>Tworzenie przestrzeni nazw usługi Event Hubs

Przestrzeń nazw usługi Event Hubs udostępnia unikatowy kontener zakresu przywoływany przy użyciu jego w pełni kwalifikowanej nazwy domeny, w którym można utworzyć jedno lub wiele centrów zdarzeń. Aby utworzyć przestrzeń nazw w grupie zasobów przy użyciu portalu, wykonaj następujące akcje:

1. W witrynie Azure Portal kliknij pozycję **Utwórz zasób** w lewym górnym rogu ekranu.
2. Wybierz pozycję **Wszystkie usługi** w menu po lewej stronie, a następnie wybierz **gwiazdkę (`*`)** obok pozycji **Event Hubs** w kategorii **Analiza**. Upewnij się, że usługa **Event Hubs** została dodana do kategorii **ULUBIONE** w menu nawigacji po lewej stronie. 
    
   ![Wyszukiwanie usługi Event Hubs](./media/event-hubs-quickstart-portal/select-event-hubs-menu.png)
3. Wybierz pozycję **Event Hubs** w obszarze **ULUBIONE** w menu nawigacji po lewej stronie, a następnie wybierz pozycję **Dodaj** na pasku narzędzi.

   ![Przycisk dodawania](./media/event-hubs-quickstart-portal/event-hubs-add-toolbar.png)
4. Na stronie **Tworzenie przestrzeni nazw** wykonaj następujące czynności:
    1. Wprowadź nazwę dla przestrzeni nazw. System od razu sprawdza, czy nazwa jest dostępna.
    2. Wybierz warstwę cenową (podstawowa lub standardowa).
    3. Wybierz **subskrypcję**, w ramach której chcesz utworzyć przestrzeń nazw.
    4. Wybierz **lokalizację** dla przestrzeni nazw.
    5. Wybierz pozycję **Utwórz**. Może być konieczne odczekanie kilku minut, aby system przeprowadził pełną aprowizację zasobów.

       ![Tworzenie przestrzeni nazw centrum zdarzeń](./media/event-hubs-quickstart-portal/create-event-hub1.png)
5. Odśwież stronę usługi **Event Hubs**, aby wyświetlić przestrzeń nazw centrum zdarzeń. Możesz sprawdzić stan tworzenia centrum zdarzeń w alertach. 

    ![Tworzenie przestrzeni nazw centrum zdarzeń](./media/event-hubs-quickstart-portal/event-hubs-refresh.png)
6. Wybierz przestrzeń nazw. W portalu zostanie wyświetlona strona główna **przestrzeni nazw usługi Event Hubs**. 

   ![Strona główna przestrzeń nazw](./media/event-hubs-quickstart-portal/namespace-home-page.png)
    
## <a name="create-an-event-hub"></a>Tworzenie centrum zdarzeń

Aby utworzyć centrum zdarzeń w przestrzeni nazw, wykonaj następujące akcje:

1. Na stronie przestrzeni nazw usługi Event Hubs wybierz pozycję **Event Hubs** w menu po lewej stronie.
1. W górnej części okna kliknij pozycję **+ Centrum zdarzeń**.
   
    ![Dodaj centrum zdarzeń — przycisk](./media/event-hubs-quickstart-portal/create-event-hub4.png)
1. Wpisz nazwę centrum zdarzeń, a następnie kliknij pozycję **Utwórz**.
   
    ![Tworzenie centrum zdarzeń](./media/event-hubs-quickstart-portal/create-event-hub5.png)
4. Możesz sprawdzić stan tworzenia centrum zdarzeń w alertach. Po utworzeniu centrum zdarzeń będzie ono widoczne na liście centrów zdarzeń, jak pokazano na poniższej ilustracji:

    ![Centrum zdarzeń zostało utworzone](./media/event-hubs-quickstart-portal/event-hub-created.png)

Gratulacje! Za pomocą portalu utworzono przestrzeń nazw usługi Event Hubs i centrum zdarzeń w ramach tej przestrzeni nazw. 

## <a name="next-steps"></a>Kolejne kroki

W tym artykule utworzono grupę zasobów, przestrzeń nazw usługi Event Hubs i centrum zdarzeń. Aby uzyskać instrukcje krok po kroku wysyłania zdarzeń do (lub) odbieranie zdarzeń z Centrum zdarzeń, zobacz **wysyłania i odbierania zdarzeń** samouczków: 

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [Node.js](event-hubs-node-get-started-send.md)
- [Go](event-hubs-go-get-started-send.md)
- [C (tylko Wyślij)](event-hubs-c-getstarted-send.md)
- [Apache Storm (tylko reecive)](event-hubs-storm-getstarted-receive.md)


[Azure portal]: https://portal.azure.com/
[3]: ./media/event-hubs-quickstart-portal/sender1.png
[4]: ./media/event-hubs-quickstart-portal/receiver1.png
