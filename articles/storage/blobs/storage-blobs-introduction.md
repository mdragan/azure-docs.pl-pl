---
title: Wprowadzenie do magazynu obiektów Blob — magazyn obiektów na platformie Azure
description: Magazyn obiektów Blob Azure przechowuje olbrzymie ilości danych obiektów niestrukturalnych, takich jak dane tekstowe lub binarne. Magazyn obiektów Blob Azure jest wysoce skalowalny i dostępny. Klienci mają dostęp do obiektów danych w magazynie obiektów Blob za pomocą programu PowerShell lub interfejsu wiersza polecenia platformy Azure, w sposób programistyczny za pomocą bibliotek klienta magazynu Azure lub przy użyciu interfejsu REST.
services: storage
author: tamram
ms.service: storage
ms.topic: overview
ms.date: 05/24/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: e216503cac2db55115bd4c1b5bf0e2f6e50355fc
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190856"
---
# <a name="introduction-to-azure-blob-storage"></a>Wprowadzenie do usługi Azure Blob Storage

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

## <a name="blob-storage-resources"></a>Zasoby magazynu obiektów blob

Magazyn obiektów blob oferuje trzy typy zasobów:

- **Konto magazynu** 
- **Kontener** na koncie magazynu
- **Obiekt blob** w kontenerze 

Na poniższym diagramie przedstawiono relacje między tymi zasobami.

![Relacja między konto Blob i zasobu kontenera](./media/storage-blob-introduction/blob1.png)

### <a name="storage-accounts"></a>Konta magazynu

Konto magazynu zapewnia unikatową przestrzeń nazw na platformie Azure dla danych użytkownika. Każdy obiekt przechowywany w usłudze Azure Storage ma adres, który zawiera unikatową nazwę konta. Kombinacja nazwy konta i punktu końcowego obiektu blob usługi Azure Storage stanowi adres podstawowy dla obiektów w ramach konta magazynu.

Jeśli na przykład konto magazynu ma nazwę *mystorageaccount*, domyślnym punktem końcowym magazynu obiektów blob jest:

```
http://mystorageaccount.blob.core.windows.net 
```

Aby utworzyć konto magazynu, zobacz temat [Tworzenie konta magazynu](../common/storage-quickstart-create-account.md). Aby dowiedzieć się więcej o kontach magazynu, zobacz [Omówienie konta usługi Azure Storage](../common/storage-account-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="containers"></a>Containers

Kontener porządkuje zestaw obiektów blob, pełniąc funkcję podobną do katalogu w systemie plików. Konto magazynu może zawierać nieograniczoną liczbę kontenerów, a każdy kontener może zawierać nieograniczoną liczbę obiektów blob. 

  > [!NOTE]
  > Wszystkie litery w nazwie kontenera muszą być małymi literami. Aby uzyskać dodatkowe informacje o nazewnictwie kontenerów, zobacz [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) (Nazewnictwo i odwołania do kontenerów, obiektów blob i metadanych).

### <a name="blobs"></a>Obiekty blob
 
Usługa Azure Storage obsługuje trzy typy obiektów blob:

* **Blokowe obiekty blob** mogą zawierać dane tekstowe i binarne o rozmiarze do około 4,7 TB. Blokowe obiekty blob składają się z bloków danych, którymi można zarządzać indywidualnie.
* **Uzupełnialne obiekty blob** również składają się z bloków, podobnie jak blokowe obiekty blob, lecz są zoptymalizowane pod kątem operacji dołączania. Uzupełnialne obiekty blob doskonale sprawdzają się w sytuacjach takich jak zapisywanie danych dzienników z maszyn wirtualnych.
* **Stronicowe obiekty blob** umożliwiają przechowywanie plików o dostępie swobodnym o rozmiarze do 8 TB. Stronicowe obiekty BLOB przechowywane pliki wirtualnych dysków twardych (VHD) i służyć jako dyski maszyn wirtualnych platformy Azure. Aby uzyskać więcej informacji na temat stronicowych obiektów blob, zobacz [Overview of Azure page blobs (Omówienie stronicowych obiektów blob platformy Azure)](storage-blob-pageblob-overview.md)

Aby uzyskać więcej informacji na temat różnych typów obiektów blob, zobacz temat [Understanding Block Blobs, Append Blobs, and Page Blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) (Omówienie blokowych, uzupełnialnych i stronicowych obiektów blob).

## <a name="move-data-to-blob-storage"></a>Przenoszenie danych do magazynu obiektów blob

Istnieje wiele rozwiązań do migrowania istniejących danych do magazynu obiektów blob:

- **AzCopy** to łatwe w użyciu narzędzie wiersza polecenia dla systemu Windows i Linux, które służy do kopiowania danych do i z magazynu obiektów blob, między kontenerami oraz między kontami magazynu. Aby uzyskać więcej informacji na temat narzędzia AzCopy, zobacz temat [Transfer data with the AzCopy v10 (Preview)](../common/storage-use-azcopy-v10.md) (Przenoszenie danych przy użyciu narzędzia AzCopy w wersji 10 [wersja zapoznawcza]). 
- **Biblioteka przenoszenia danych usługi Azure Storage** jest biblioteką .NET służącą do przenoszenia danych między usługami Azure Storage. Narzędzie AzCopy zostało utworzone przy użyciu biblioteki przenoszenia danych. Więcej informacji można znaleźć w [dokumentacji referencyjnej](/dotnet/api/microsoft.azure.storage.datamovement) biblioteki przenoszenia danych. 
- **Usługa Azure Data Factory** obsługuje kopiowanie danych do i z magazynu obiektów blob przy użyciu klucza konta, sygnatury dostępu współdzielonego, jednostki usługi lub tożsamości zarządzanych do uwierzytelnienia zasobów platformy Azure. Aby uzyskać więcej informacji, zobacz temat [Copy data to or from Azure Blob storage by using Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Kopiowanie danych do i z usługi Azure Blob Storage za pomocą usługi Azure Data Factory). 
- **Blobfuse** jest sterownikiem wirtualnego systemu plików usługi Azure Blob Storage. Sterownik blobfuse umożliwia dostęp do istniejących danych blokowych obiektów blob na koncie magazynu za pomocą systemu plików w systemie Linux. Aby uzyskać więcej informacji, zobacz temat [How to mount Blob storage as a file system with blobfuse](storage-how-to-mount-container-linux.md) (Jak zainstalować magazyn obiektów blob jako system plików przy użyciu sterownika blobfuse).
- **Urządzenie Azure Data Box** usługa jest dostępna na przesyłanie danych lokalnych do usługi Blob storage, po dużych zestawów danych lub sieci ograniczenia przekazywania danych przez sieć określenie. W zależności od rozmiar danych, możesz poprosić o [dysku Azure Data Box](../../databox/data-box-disk-overview.md), [urządzenia Azure Data Box](../../databox/data-box-overview.md), lub [Azure Data Box mocno](../../databox/data-box-heavy-overview.md) urządzeń firmy Microsoft. Możesz następnie skopiować dane do tych urządzeń i odeślemy je do firmy Microsoft do przekazania do magazynu obiektów Blob.
- **Usługa Azure Import/Export** zapewniono możliwość importowania lub eksportowania dużych ilości danych do i z konta magazynu przy użyciu dysków twardych, które należy podać. Aby uzyskać więcej informacji, zobacz [Use the Microsoft Azure Import/Export service to transfer data to Blob Storage](../common/storage-import-export-service.md) (Przesyłanie danych do usługi Blob Storage za pomocą usługi Microsoft Azure Import/Export).

## <a name="next-steps"></a>Kolejne kroki

* [Tworzenie konta magazynu](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Azure Storage scalability and performance targets](../common/storage-scalability-targets.md) (Cele dotyczące skalowalności i wydajności usługi Azure Storage)
