---
title: 'Samouczek: Hostowanie statycznej witryny internetowej w usłudze Blob Storage — Azure Storage'
description: Dowiedz się, w jaki sposób skonfigurować konto magazynu w celu hostowania statycznej witryny internetowej i wdrożyć statyczną witrynę internetową w usłudze Azure Storage.
services: storage
author: normesta
ms.service: storage
ms.topic: tutorial
ms.date: 12/31/2018
ms.author: normesta
ms.reviewer: seguler
ms.custom: seodec18
ms.openlocfilehash: e3f2dd74d00b36016549cd33d76b866de241d977
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148480"
---
<!---Customer intent: I want to host files for a static website in Blob storage and access the website from an Azure endpoint.--->

# <a name="tutorial-host-a-static-website-on-blob-storage"></a>Samouczek: Hostowanie statycznej witryny internetowej w usłudze Blob Storage

Niniejszy samouczek jest pierwszą częścią serii. Dzięki niemu dowiesz się, w jaki sposób utworzyć i wdrożyć statyczną witrynę internetową w usłudze Azure Storage. Po zakończeniu będzie gotowa statyczna witryna internetowa, do której użytkownicy będą mieli publiczny dostęp. 

Część pierwsza serii zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie hostowania statycznej witryny internetowej
> * Wdrażanie witryny internetowej Hello world

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

W tym samouczku wykorzystany zostanie program [Visual Studio Code](https://code.visualstudio.com/download), czyli bezpłatne narzędzie dla programistów umożliwiające utworzenie statycznej witryny internetowej i wdrożenie jej na koncie usługi Azure Storage.

Po zainstalowaniu programu Visual Studio Code zainstaluj rozszerzenie wersji zapoznawczej usługi Azure Storage. To rozszerzenie integruje funkcje zarządzania usługą Azure Storage z programem Visual Studio Code. To rozszerzenie zostanie użyte do wdrożenia statycznej witryny internetowej w usłudze Azure Storage. Aby zainstalować rozszerzenie:

1. Uruchom program Visual Studio Code.
2. Na pasku narzędzi kliknij pozycję **Extensions** (Rozszerzenia). Wyszukaj hasło *Azure Storage* i wybierz rozszerzenie **Azure Storage** na liście. Następnie kliknij przycisk **Install** (Zainstaluj), aby zainstalować rozszerzenie.

    ![Instalowanie rozszerzenia usługi Azure Storage w programie VS Code](media/storage-blob-static-website-host/install-extension-vs-code.png)

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/), aby rozpocząć pracę.

## <a name="configure-static-website-hosting"></a>Konfigurowanie hostowania statycznej witryny internetowej

Pierwszym krokiem jest skonfigurowanie konta magazynu na potrzeby hostowania statycznej witryny internetowej w witrynie Azure Portal. Gdy skonfigurujesz konto pod kątem hostowania statycznej witryny internetowej, usługa Azure Storage automatycznie utworzy kontener o nazwie *$web*. Kontener *$web* będzie zawierał pliki statycznej witryny internetowej. 

1. Otwórz witrynę [Azure Portal](https://portal.azure.com/) w przeglądarce internetowej. 
1. Znajdź konto magazynu i wyświetl omówienie konta.
1. Wybierz pozycję **Statyczna witryna internetowa**, aby wyświetlić stronę konfiguracji dla statycznych witryn internetowych.
1. Wybierz pozycję **Włączone**, aby umożliwić hostowanie statycznej witryny internetowej na koncie magazynu.
1. W polu **Nazwa dokumentu indeksu** określ domyślną stronę indeksu *index.html*. Gdy użytkownik przejdzie do głównego elementu statycznej witryny internetowej, wyświetlona zostanie domyślna strona indeksu.  
1. W polu **Ścieżka dokumentu błędu** określ domyślną stronę błędu *404. html*. Gdy użytkownik spróbuje przejść do strony, która nie istnieje w statycznej witrynie internetowej, wyświetlona zostanie domyślna strona błędu.
1. Kliknij pozycję **Zapisz**. W witrynie Azure Portal jest teraz wyświetlany punkt końcowy statycznej witryny internetowej. 

    ![Włączanie hostowania statycznej witryny internetowej na koncie magazynu](media/storage-blob-static-website-host/enable-static-website-hosting.png)

## <a name="deploy-a-hello-world-website"></a>Wdrażanie witryny internetowej Hello world

Następnie utwórz stronę internetową Hello world w programie Visual Studio Code i wdróż ją w statycznej witrynie internetowej hostowanej na koncie usługi Azure Storage.

1. Utwórz pusty folder o nazwie *mywebsite* w lokalnym systemie plików. 
1. Uruchom program Visual Studio Code i otwórz utworzony folder na panelu **Explorer**.

    ![Otwieranie folderu w programie Visual Studio Code](media/storage-blob-static-website-host/open-folder-vs-code.png)

1. Utwórz domyślny plik indeksu w folderze *mywebsite* i nazwij go *index.html*.

    ![Tworzenie domyślnego pliku indeksu w programie Visual Studio Code](media/storage-blob-static-website-host/create-index-file-vs-code.png)

1. Otwórz plik *index.html* w edytorze, wklej następujący tekst do pliku i zapisz go:

    ```
    <h1>Hello World!</h1>
    ```

1. Utwórz domyślny plik błędu i nazwij go *404. html*.
1. Otwórz plik *404.html* w edytorze, wklej następujący tekst do pliku i zapisz go:

    ```
    <h1>404</h1>
    ```

1. Kliknij prawym przyciskiem myszy w obszarze folderu *mywebsite* na panelu **Explorer**, a następnie wybierz pozycję **Deploy to Static Website...** (Wdrażanie w statycznej witrynie internetowej), aby wdrożyć witrynę internetową. Zostanie wyświetlony monit o zalogowanie się na platformie Azure w celu pobrania listy subskrypcji.

1. Wybierz subskrypcję zawierającą konto magazynu, dla którego włączono hostowanie statycznej witryny internetowej. Następnie po wyświetleniu monitu wybierz konto magazynu.

Program Visual Studio Code przekaże pliki do internetowego punktu końcowego i wyświetli pasek stanu powodzenia. Uruchom witrynę internetową, aby ją wyświetlić na platformie Azure.

![Wyświetlanie wdrożenia statycznej witryny internetowej na platformie Azure](media/storage-blob-static-website-host/view-static-website-endpoint.png)

Pomyślnie ukończono pracę z samouczkiem i wdrożono statyczną witrynę internetową na platformie Azure.

## <a name="next-steps"></a>Kolejne kroki

W części pierwszej tego samouczka pokazano, jak skonfigurować konto usługi Azure Storage w celu hostowania statycznej witryny internetowej, oraz jak utworzyć i wdrożyć statyczną witrynę internetową w punkcie końcowym platformy Azure.

Przejdź teraz do części drugiej, w której pokazane zostanie konfigurowanie domeny niestandardowej z protokołem SSL dla statycznej witryny internetowej w usłudze Azure CDN.

> [!div class="nextstepaction"]
> [Korzystanie z usługi Azure CDN w celu włączenia domeny niestandardowej dla statycznej witryny internetowej](storage-blob-static-website-custom-domain.md)
