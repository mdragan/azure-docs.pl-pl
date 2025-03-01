---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 10/15/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: ecdd419331c88e712644851f9213861f882cf0f6
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67183391"
---
## <a name="prepare-your-web-app"></a>Przygotowywanie aplikacji internetowej

Aby powiązać niestandardowy certyfikat protokołu SSL (certyfikat innej firmy lub certyfikat usługi App Service) z Twoją aplikacją internetową, Twój [Plan usługi App Service](https://azure.microsoft.com/pricing/details/app-service/) musi znajdować się w warstwie **Podstawowa**, **Standardowa**, **Premium** lub **Izolowana**. W tym kroku musisz się upewnić, że Twoja aplikacja internetowa jest w obsługiwanej warstwie cenowej.

### <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Otwórz [portal Azure](https://portal.azure.com).

### <a name="navigate-to-your-web-app"></a>Przejdź do swojej aplikacji internetowej

W menu po lewej stronie kliknij pozycję **App Services**, a następnie kliknij nazwę swojej aplikacji internetowej.

![Wybieranie aplikacji internetowej](./media/app-service-ssl-prepare-app/select-app.png)

Znajdujesz się na stronie zarządzania Twojej aplikacji internetowej.  

### <a name="check-the-pricing-tier"></a>Sprawdzanie warstwy cenowej

W lewym obszarze nawigacji na stronie Twojej aplikacji internetowej przewiń do sekcji **Ustawienia** i wybierz pozycję **Skaluj w górę (plan usługi App Service)** .

![Menu skalowania w górę](./media/app-service-ssl-prepare-app/scale-up-menu.png)

Upewnij się, że Twoja aplikacja internetowa nie znajduje się w warstwie **F1** ani **D1**. Bieżąca warstwa Twojej aplikacji internetowej jest wyróżniona ciemnoniebieskim polem.

![Sprawdzanie warstwy cenowej](./media/app-service-ssl-prepare-app/check-pricing-tier.png)

Niestandardowy protokół SSL nie jest obsługiwany w warstwie **F1** ani **D1**. Jeśli musisz skalować w górę, wykonaj kroki opisane w następnej sekcji. W przeciwnym razie zamknij stronę **Skalowanie w górę** i przejdź do sekcji [Skalowanie w górę planu usługi App Service](#scale-up-your-app-service-plan).

### <a name="scale-up-your-app-service-plan"></a>Skalowanie w górę planu usługi App Service

Wybierz jedną z płatnych warstw (**B1**, **B2**, **B3** lub dowolną warstwę z kategorii **Produkcja**). Aby uzyskać dodatkowe opcje, kliknij pozycję **Wyświetl dodatkowe opcje**.

Kliknij przycisk **Zastosuj**.

![Wybieranie warstwy cenowej](./media/app-service-ssl-prepare-app/choose-pricing-tier.png)

Wyświetlenie następującego powiadomienia oznacza zakończenie operacji skalowania.

![Powiadomienie o skalowaniu w górę](./media/app-service-ssl-prepare-app/scale-notification.png)

