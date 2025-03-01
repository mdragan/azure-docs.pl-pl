---
title: 'Szybki start: usługa Microsoft Azure Data Box Disk | Microsoft Docs'
description: Skorzystaj z tego przewodnika Szybki start, aby przeprowadzić szybkie wdrożenie usługi Azure Data Box Disk w witrynie Azure Portal
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: quickstart
ms.date: 02/26/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to quickly deploy Data Box Disk so as to import data into Azure.
ms.openlocfilehash: a01da3218b07307faa8e94acab1473c82bd86c41
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60306210"
---
# <a name="quickstart-deploy-azure-data-box-disk-using-the-azure-portal"></a>Szybki start: wdrażanie usługi Azure Data Box Disk za pomocą witryny Azure Portal

W tym przewodniku Szybki start opisano sposób wdrażania usługi Azure Data Box Disk przy użyciu witryny Azure Portal. Opisano czynności szybkiego tworzenia zamówienia, odbioru dysków, ich rozpakowywania i podłączania oraz kopiowania danych na dyski w celu przekazania na platformę Azure.

Aby zyskać szczegółowe instrukcje dotyczące wdrażania i śledzenia krok po kroku, zobacz [Samouczek: zamawianie urządzenia Azure Data Box Disk](data-box-disk-deploy-ordered.md). 

Jeśli nie masz subskrypcji platformy Azure, utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem:

- Upewnij się, że w ramach Twojej subskrypcji włączono usługę Azure Data Box. Aby włączyć tę usługę w subskrypcji, [zarejestruj się](https://aka.ms/azuredataboxfromdiskdocs).

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się do witryny Azure Portal pod adresem [https://aka.ms/azuredataboxfromdiskdocs](https://aka.ms/azuredataboxfromdiskdocs).

## <a name="order"></a>Zamówienie

Ten krok zajmuje około 5 minut.

1. Utwórz nowy zasób usługi Azure Data Box w witrynie Azure Portal. 
2. Wybierz subskrypcję, w której włączono tę usługę, a następnie wybierz typ transferu **Import**. Podaj **Kraj źródłowy**, w którym są przechowywane dane, oraz **Docelowy region platformy Azure**, do którego mają zostać przekazane dane.
3. Wybierz pozycję **Data Box Disk**. Maksymalna pojemność tego rozwiązania to 35 TB. W przypadku większych ilości danych możesz utworzyć większą liczbę zamówień.  
4. Wprowadź szczegóły zamówienia i informacje dotyczące wysyłki. Jeśli ta usługa jest dostępna w Twoim regionie, podaj adresy e-mail na potrzeby powiadomień, zapoznaj się z podsumowaniem i utwórz zamówienie.

Po utworzeniu zamówienia dyski zostaną przygotowane do wysłania.

## <a name="unpack"></a>Rozpakowywanie

Ten krok zajmuje około 5 minut.

Urządzenia Data Box Disk są wysyłane w opakowaniu UPS Express Box. Otwórz opakowanie i sprawdź, czy zawiera:

- od 1 do 5 dysków USB zapakowanych w folię bąbelkową,
- jeden kabel połączeniowy dla każdego dysku,
- etykietę wysyłkową na potrzeby wysyłki zwrotnej.

## <a name="connect-and-unlock"></a>Podłączanie i odblokowywanie

Ten krok zajmuje około 5 minut.

1. Podłącz dysk do komputera z obsługiwaną wersją systemu Windows/Linux za pomocą dołączonego kabla. Aby uzyskać więcej informacji na temat obsługiwanych wersji systemu operacyjnego, zapoznaj się z [wymaganiami systemowymi rozwiązania Azure Data Box Disk](data-box-disk-system-requirements.md). 
2. Aby odblokować dysk:

    1. W witrynie Azure Portal przejdź do pozycji **Ogólne > Szczegóły urządzenia** i uzyskaj klucz dostępu.
    2. Pobierz i wyodrębnij przeznaczone dla danego systemu operacyjnego narzędzie do odblokowywania urządzenia Data Box Disk na komputerze, którego używasz do skopiowania danych na dyski. 
    3. Uruchom narzędzie do odblokowywania urządzenia Data Box Disk i podaj klucz dostępu. W przypadku ponownego użycia dysku uruchom ponownie narzędzie do odblokowywania i podaj klucz dostępu. **Do odblokowania dysku nie należy używać okna dialogowego funkcji BitLocker ani klucza funkcji BitLocker.** Aby uzyskać więcej informacji na temat sposobu odblokowywania dysków, zobacz [Odblokowywanie dysków na komputerze klienckim z systemem Windows](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client) lub [Odblokowywanie dysków na komputerze klienckim z systemem Linux](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client).
    4. W narzędziu jest wyświetlana litera dysku przypisana do danego dysku. Zanotuj tę literę dysku. Będzie używana w kolejnych krokach.

## <a name="copy-data-and-validate"></a>Kopiowanie danych i weryfikacja

Czas wymagany do ukończenia tej operacji zależy od rozmiaru danych.

1. Dysk zawiera *PageBlob*, *BlockBlob*, *AzureFile*, *ManagedDisk*, i *DataBoxDiskImport* folderów. Przeciągnij i upuść dane, które mają zostać zaimportowane jako blokowe obiekty blob, do folderu *BlockBlob*. Podobnie, przeciągnij i upuść dane, takie jak dysk VHD/VHDX do *PageBlob* folder i odpowiednie dane do *AzureFile*. Kopiowanie wirtualnych dysków twardych, które chcesz przekazać jako dysków zarządzanych do folderu, w obszarze *ManagedDisk*.

    Dla każdego podfolderu w folderach *BlockBlob* i *PageBlob* zostanie utworzony kontener na koncie usługi Azure Storage. Udział plików jest tworzona dla podfolderu w obszarze *AzureFile*.

    Wszystkie pliki w folderach *BlockBlob* i *PageBlob* zostaną skopiowane do domyślnego kontenera `$root` na koncie usługi Azure Storage. Skopiuj pliki do folderu, w ramach *AzureFile*. Wszystkie pliki kopiowana bezpośrednio do *AzureFile* folderu zakończy się niepowodzeniem i są przekazywane jako blokowe obiekty BLOB.

    > [!NOTE]
    > - Wszystkie kontenery, obiekty BLOB i plików powinny być zgodne z [konwencjami nazewnictwa platformy Azure](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions). W przypadku niezgodności z tymi regułami przekazywanie danych na platformę Azure zakończy się niepowodzeniem.
    > - Upewnij się, że pliki nie przekraczają ~4.75 TiB dla blokowych obiektów blob, ~ 8 TiB dla stronicowych obiektów blob i OK. 1 TiB dla usługi Azure Files.

2. **(Opcjonalne, ale zalecane)**  Po zakończeniu kopiowania, zdecydowanie zalecamy co najmniej uruchomienie `DataBoxDiskValidation.cmd` podawany *DataBoxDiskImport* folder, a następnie wybierz opcję 1, aby zweryfikować pliki. Zalecamy również tego pozwalające czasu za pomocą opcji 2 również generowanie sum kontrolnych dla sprawdzania poprawności (może potrwać w zależności od rozmiaru danych). Te kroki zminimalizować prawdopodobieństwo wszelkie błędy, podczas przekazywania danych na platformie Azure.
3. Bezpiecznie usunąć dysk.

## <a name="ship-to-azure"></a>Wysyłka do platformy Azure

Ten krok zajmuje około 5–7 minut.

1. Umieść wszystkie dyski w oryginalnym opakowaniu. Użyj dołączonej etykiety wysyłkowej. W przypadku uszkodzenia lub utraty etykiety pobierz etykietę z portalu. W sekcji **Przegląd** kliknij pozycję **Pobierz etykietę wysyłkową** na pasku poleceń.
2. Oddaj zapieczętowaną paczkę firmie przewozowej.  

Usługa Data Box Disk wysyła powiadomienie e-mail i aktualizuje stan zamówienia w witrynie Azure Portal.

## <a name="verify-your-data"></a>Weryfikowanie danych

Czas wymagany do ukończenia tej operacji zależy od rozmiaru danych.

1. Gdy urządzenie Data Box Disk zostanie podłączone do sieci w centrum danych platformy Azure, automatycznie rozpocznie się przekazywanie danych na platformę Azure.
2. Usługa Azure Data Box powiadomi Cię o zakończeniu kopiowania danych w witrynie Azure Portal.
    
    1. Sprawdź dzienniki błędów pod kątem ewentualnych niepowodzeń i podejmij odpowiednie działania.
    2. Sprawdź, czy dane znajdują się na kontach magazynu, zanim usuniesz je ze źródła.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Ten krok zajmuje około 2–3 minut.

Aby wyczyścić zasoby, możesz anulować zamówienie urządzenia Data Box i usunąć je.

- Możesz anulować zamówienie urządzenia Data Box w witrynie Azure Portal przed jego przetworzeniem. Po przetworzeniu zamówienia nie można go anulować. Realizacja zamówienia będzie kontynuowana aż do etapu ukończenia.

    Aby anulować zamówienie, w sekcji **Przegląd** kliknij pozycję **Anuluj** na pasku poleceń.  

- Możesz usunąć zamówienie, gdy jego stan w witrynie Azure Portal to **Ukończono** lub **Anulowano**.

    Aby usunąć zamówienie, w sekcji **Przegląd** kliknij pozycję **Usuń** na pasku poleceń.

## <a name="next-steps"></a>Kolejne kroki

Podczas pracy z tym przewodnikiem Szybki start wdrożono usługę Azure Data Box Disk w celu zaimportowania danych na platformę Azure. Aby dowiedzieć się więcej na temat zarządzania usługą Azure Data Box Disk, przejdź do kolejnego samouczka:

> [!div class="nextstepaction"]
> [Use the Azure portal to administer Data Box Disk (Administrowanie usługą Data Box Disk w witrynie Azure Portal)](data-box-portal-ui-admin.md)
