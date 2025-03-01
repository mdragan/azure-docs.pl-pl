---
title: Samouczek — udziału Azure przestrzenne zakotwiczenia między sesjami i urządzeniami za pomocą usługi Azure Cosmos DB do zaplecza | Dokumentacja firmy Microsoft
description: W tym samouczku dowiesz się, jak udostępniać identyfikatory kotwic przestrzenne platformy Azure na urządzeniach z systemem Android/iOS na platformie Unity za pomocą usługi zaplecza i Azure Cosmos DB.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: fc172b5327d72687fea7d13ddb706ecc7ab630b6
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135349"
---
# <a name="tutorial-share-azure-spatial-anchors-across-sessions-and-devices-with-an-azure-cosmos-db-back-end"></a>Samouczek: Zaplecze udziału Azure przestrzenne zakotwiczenia między sesjami i urządzeniami za pomocą usługi Azure Cosmos DB

W tym samouczku dowiesz się, jak używać [kotwic przestrzenne Azure](../overview.md) można tworzyć kotwic podczas jednej sesji, a następnie zlokalizuj je w innej sesji, w tym samym urządzeniu lub na inną. Na przykład drugi sesja może być na różne dni. Również znajdować się te sam kotwic przez wiele urządzeń, w tym samym miejscu, a w tym samym czasie.

![Obraz GIF ilustrujące trwałość obiektów](./media/persistence.gif)

[Azure kotwic przestrzenne](../overview.md) to usługa programistycznych dla wielu platform, która umożliwia tworzenie środowisk rzeczywistość mieszana z obiektami, które ulegają zmianie lokalizacji urządzenia wraz z upływem czasu. Gdy skończysz, będziesz mieć aplikację, którą można wdrożyć na dwóch urządzeniach lub więcej. Przestrzenne kotwic utworzone przez jedno wystąpienie będzie udostępniać ich identyfikatorów innych za pomocą usługi Azure Cosmos DB.

Omawiane tematy:

> [!div class="checklist"]
> * Wdrażanie aplikacji internetowej platformy ASP.NET Core na platformie Azure, który może służyć do udostępniania zakotwiczenia, przechowując je w usłudze Azure Cosmos DB.
> * Skonfiguruj sceny AzureSpatialAnchorsLocalSharedDemo w próbce Unity z pakiety Azure quickstarts, aby móc korzystać z aplikacji sieci Web do udostępniania zakotwiczenia.
> * Wdrażanie aplikacji na co najmniej jedno urządzenie, a następnie uruchom go.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Warto zauważyć, że jeśli będziesz korzystać z aparatu Unity i Azure Cosmos DB w ramach tego samouczka, jest po prostu zapewnienie przykładem sposobu udostępniania identyfikatory zakotwiczeń przestrzenne między urządzeniami. Do osiągniecia tego celu można użyć innych języków i technologii zaplecza. Aplikacja internetowa ASP.NET Core, które są używane w tym samouczku wymaga również, .NET Core 2.2 SDK. Działa on poprawnie na Windows dla aplikacji sieci Web, ale obecnie nie będzie działać w usłudze Web Apps dla systemu Linux.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Kopiuj `Connection String` ponieważ będziesz ich potrzebować.

## <a name="download-the-unity-sample-project"></a>Pobierz przykładowy projekt aparatu Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-the-sharing-anchors-service"></a>Wdrażanie usługi udostępniania zakotwiczenia

Otwórz projekt w programie Visual Studio i Otwórz `Sharing\SharingServiceSample` folderu.

### <a name="configure-the-service-to-use-your-azure-cosmos-db-database"></a>Skonfiguruj usługę do używania bazy danych Azure Cosmos DB

W **Eksploratora rozwiązań**, otwórz `SharingService\Startup.cs`.

Znajdź `#define INMEMORY_DEMO` w górnej części pliku a komentarz wiersz się. Zapisz plik.

W **Eksploratora rozwiązań**, otwórz `SharingService\appsettings.json`.

Znajdź `StorageConnectionString` właściwości, a następnie ustaw wartość taka sama jak `Connection String` wartości, który został skopiowany w [tworzenie krok konta bazy danych](#create-a-database-account). Zapisz plik.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku użyto usługi Azure Cosmos DB do udostępnienia identyfikatorów kotwic między urządzeniami. Aby dowiedzieć się więcej o sposobie używania Azure przestrzenne kotwice w nowej aplikacji platformy Unity HoloLens, przejdź do następnego samouczka.

> [!div class="nextstepaction"]
> [Uruchamianie nowej aplikacji systemu Android](./tutorial-new-unity-hololens-app.md)