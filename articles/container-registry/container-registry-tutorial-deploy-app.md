---
title: Samouczek — wdrażanie aplikacji z rejestru platformy Docker z replikacją geograficzną na platformie Azure
description: Wdrażanie aplikacji internetowej opartej na systemie Linux w dwóch różnych regionach platformy Azure przy użyciu obrazu kontenera z rejestru kontenerów platformy Azure z replikacją geograficzną. Druga część trzyczęściowej serii.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: tutorial
ms.date: 08/20/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: e5a38e2b6550d763f30c2462944b154f76bbe92c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60508256"
---
# <a name="tutorial-deploy-a-web-app-from-a-geo-replicated-azure-container-registry"></a>Samouczek: Wdrażanie aplikacji internetowej z rejestru kontenerów platformy Azure z replikacją geograficzną

Niniejszy samouczek jest drugą częścią trzyczęściowej serii. W [pierwszej części](container-registry-tutorial-prepare-registry.md) został utworzony prywatny rejestr kontenerów z replikacją geograficzną, a na podstawie źródła został utworzony i wypchnięty do rejestru obraz kontenera. W tym artykule wykorzystasz aspekt bliskości sieci rejestru z replikacją geograficzną przez wdrożenie kontenera w wystąpieniach aplikacji internetowej w dwóch różnych regionach platformy Azure. Następnie każde wystąpienie pobiera obraz kontenera z najbliższego rejestru.

Z tego samouczka, który jest drugą częścią serii, nauczysz się wykonywać następujące czynności:

> [!div class="checklist"]
> * Wdrażanie obrazu kontenera w dwóch wystąpieniach usługi *Web Apps for Containers*
> * Weryfikowanie wdrożonej aplikacji

Jeśli rejestr z replikacją geograficzną nie został jeszcze utworzony, a obraz konteneryzowanej aplikacji przykładowej nie został wypchnięty do rejestru, wróć do poprzedniego samouczka z tej serii zatytułowanego [Przygotowanie rejestru kontenerów platformy Azure z replikacją geograficzną](container-registry-tutorial-prepare-registry.md).

W następnym artykule z tej serii zaktualizujesz aplikację, a następnie wypchniesz zaktualizowany obraz kontenera do rejestru. Na koniec przejdziesz do każdego uruchomionego wystąpienia aplikacji internetowej, aby zobaczyć zmianę automatycznie odzwierciedloną w obu wystąpieniach, która pokazuje elementy webhook i replikację geograficzną usługi Azure Container Registry w działaniu.

## <a name="automatic-deployment-to-web-apps-for-containers"></a>Automatyczne wdrażanie w usłudze Web Apps for Containers

Usługa Azure Container Registry zapewnia obsługę wdrażania konteneryzowanych aplikacji bezpośrednio w usłudze [Web Apps for Containers](../app-service/containers/index.yml). W tym samouczku za pomocą witryny Azure Portal wdrożysz obraz kontenera, który został utworzony w poprzednim samouczku, w dwóch planach aplikacji internetowej znajdujących się w różnych regionach platformy Azure.

Gdy wdrażasz aplikację internetową za pomocą obrazu kontenera w rejestrze, a rejestr z replikacją geograficzną znajduje się w tym samym regionie, usługa Azure Container Registry tworzy za Ciebie element [webhook](container-registry-webhook.md) wdrożenia obrazu. Gdy wypchniesz nowy obraz do repozytorium kontenerów, element webhook przejmie zmianę i automatycznie wdroży nowy obraz kontenera w Twojej aplikacji internetowej.

## <a name="deploy-a-web-app-for-containers-instance"></a>Wdrażanie wystąpienia usługi Web App for Containers

W tym kroku utworzysz wystąpienie usługi Web App for Containers w regionie *Zachodnie stany USA*.

Zaloguj się do witryny [Azure Portal](https://portal.azure.com) i przejdź do rejestru utworzonego w poprzednim samouczku.

Wybierz pozycję **Repozytoria** > **acr-helloworld**, a następnie kliknij prawym przyciskiem myszy tag **v1** w obszarze **Tagi** i wybierz pozycję **Wdróż do aplikacji internetowej**:

![Wdrażanie w usłudze App Service w witrynie Azure Portal][deploy-app-portal-01]

Jeśli polecenie „Wdróż do aplikacji internetowej” jest wyłączone, może to oznaczać, że nie włączono konta administratora rejestru zgodnie ze wskazówkami zawartymi w sekcji [Tworzenie rejestru kontenerów](container-registry-tutorial-prepare-registry.md#create-a-container-registry) w pierwszym samouczku. Można je włączyć w obszarze **Ustawienia** > **Klucze dostępu** w witrynie Azure Portal.

W oknie usługi **Web App for Containers** wyświetlonym po wybraniu polecenia „Wdróż do aplikacji internetowej” podaj następujące wartości dla każdego ustawienia:

| Ustawienie | Wartość |
|---|---|
| **Nazwa witryny** | Globalnie unikatowa nazwa aplikacji internetowej. W tym przykładzie używamy formatu `<acrName>-westus`, aby łatwo zidentyfikować rejestr i region, z których jest wdrażana aplikacja internetowa. |
| **Grupa zasobów** | **Użyj istniejącej** > `myResourceGroup` |
| **Plan usługi App Service/lokalizacja** | Utwórz nowy plan o nazwie `plan-westus` w regionie **Zachodnie stany USA**. |
| **Obraz** | `acr-helloworld:v1`

Wybierz pozycję **Utwórz**, aby aprowizować aplikację internetową w regionie *Zachodnie stany USA*.

![Aplikacja internetowa w konfiguracji systemu Linux w witrynie Azure Portal][deploy-app-portal-02]

## <a name="view-the-deployed-web-app"></a>Wyświetlanie wdrożonej aplikacji internetowej

Po zakończeniu wdrażania możesz wyświetlić uruchomioną aplikację, przechodząc do jej adresu URL w przeglądarce.

W portalu wybierz pozycję **App Services**, a następnie aplikację internetową aprowizowaną w poprzednim kroku. W tym przykładzie aplikacja internetowa nosi nazwę *uniqueregistryname-westus*.

Wybierz adres URL aplikacji internetowej z hiperlinku w prawym górnym rogu okna przeglądu **App Service**, aby wyświetlić uruchomioną aplikację w przeglądarce.

![Aplikacja internetowa w konfiguracji systemu Linux w witrynie Azure Portal][deploy-app-portal-04]

Po wdrożeniu obrazu platformy Docker z rejestru kontenerów z replikacją geograficzną w witrynie będzie wyświetlany obraz przedstawiający region świadczenia usługi Azure hostujący rejestr kontenerów.

![Wdrożona aplikacja internetowa wyświetlona w przeglądarce][deployed-app-westus]

## <a name="deploy-second-web-app-for-containers-instance"></a>Wdrażanie drugiego wystąpienia usługi Web App for Containers

Wdróż drugą aplikację internetową w regionie *Wschodnie stany USA*, używając procedury opisanej w poprzedniej sekcji. W usłudze **Web App for Containers** określ następujące wartości:

| Ustawienie | Wartość |
|---|---|
| **Nazwa witryny** | Globalnie unikatowa nazwa aplikacji internetowej. W tym przykładzie używamy formatu `<acrName>-eastus`, aby łatwo zidentyfikować rejestr i region, z których jest wdrażana aplikacja internetowa. |
| **Grupa zasobów** | **Użyj istniejącej** > `myResourceGroup` |
| **Plan usługi App Service/lokalizacja** | Utwórz nowy plan o nazwie `plan-eastus` w regionie **Wschodnie stany USA**. |
| **Obraz** | `acr-helloworld:v1`

Wybierz pozycję **Utwórz**, aby aprowizować aplikację internetową w regionie *Wschodnie stany USA*.

![Aplikacja internetowa w konfiguracji systemu Linux w witrynie Azure Portal][deploy-app-portal-06]

## <a name="view-the-deployed-web-app"></a>Wyświetlanie wdrożonej aplikacji internetowej

Jak poprzednio, możesz wyświetlić uruchomioną aplikację, przechodząc do jej adresu URL w przeglądarce.

W portalu wybierz pozycję **App Services**, a następnie aplikację internetową aprowizowaną w poprzednim kroku. W tym przykładzie aplikacja internetowa nosi nazwę *uniqueregistryname-eastus*.

Wybierz adres URL aplikacji internetowej z hiperlinku w prawym górnym rogu okna przeglądu **App Service**, aby wyświetlić uruchomioną aplikację w przeglądarce.

![Aplikacja internetowa w konfiguracji systemu Linux w witrynie Azure Portal][deploy-app-portal-07]

Po wdrożeniu obrazu platformy Docker z rejestru kontenerów z replikacją geograficzną w witrynie będzie wyświetlany obraz przedstawiający region świadczenia usługi Azure hostujący rejestr kontenerów.

![Wdrożona aplikacja internetowa wyświetlona w przeglądarce][deployed-app-eastus]

## <a name="next-steps"></a>Kolejne kroki

W ramach tego samouczka zostały wdrożone dwa wystąpienia usługi Web App for Containers z rejestru kontenerów platformy Azure z replikacją geograficzną.

Przejdź do następnego samouczka, aby zaktualizować oraz wdrożyć nowy obraz kontenera w rejestrze kontenerów, a następnie sprawdzić, czy aplikacje internetowe działające w obu regionach zostały automatycznie zaktualizowane.

> [!div class="nextstepaction"]
> [Wdrażanie aktualizacji w obrazie kontenera z replikacją geograficzną](./container-registry-tutorial-deploy-update.md)

<!-- IMAGES -->
[deploy-app-portal-01]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-01.png
[deploy-app-portal-02]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-02.png
[deploy-app-portal-03]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-03.png
[deploy-app-portal-04]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-04.png
[deploy-app-portal-05]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-05.png
[deploy-app-portal-06]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-06.png
[deploy-app-portal-07]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-07.png
[deployed-app-westus]: ./media/container-registry-tutorial-deploy-app/deployed-app-westus.png
[deployed-app-eastus]: ./media/container-registry-tutorial-deploy-app/deployed-app-eastus.png