---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 16ccd89fe6eaad3fd6c2704b2f324f486eee45e1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67183209"
---
# <a name="common-errors-during-classic-to-azure-resource-manager-migration"></a>Typowe błędy występujące podczas migracji z modelu klasycznego do modelu opartego na usłudze Azure Resource Manager
W tym artykule skatalogowano najbardziej typowe błędy i środki zaradcze w trakcie migracji zasobów IaaS z klasycznego modelu wdrażania platformy Azure do stosu usługi Azure Resource Manager.

[!INCLUDE [updated-for-az](./updated-for-az.md)]

## <a name="list-of-errors"></a>Lista błędów

| Ciąg błędu | Środki zaradcze |
| --- | --- |
| Wewnętrzny błąd serwera |W niektórych przypadkach jest to błąd przejściowy, który znika przy ponownej próbie. Jeśli ten błąd będzie się powtarzać, [skontaktuj się z pomocą techniczną platformy Azure](../articles/azure-supportability/how-to-create-azure-support-request.md), ponieważ wymaga to badania dzienników platformy. <br><br> **UWAGA:** Gdy zdarzenie jest śledzone przez zespół pomocy technicznej, nie należy podejmować żadnych samodzielnych ponieważ może to mieć niezamierzone konsekwencje w używanym środowisku. |
| Migracja wdrożenia {nazwa_wdrożenia} w usłudze hostowanej {nazwa_usługi_hostowanej} nie jest obsługiwana, ponieważ jest to wdrożenie PaaS (sieć Web/proces roboczy). |Ma to miejsce w przypadku, gdy wdrożenie zawiera rolę sieci Web/procesu roboczego. Ponieważ migracja jest obsługiwana tylko dla maszyn wirtualnych, usuń rolę sieci Web/procesu roboczego z wdrożenia i ponownie spróbuj przeprowadzić migrację. |
| Wdrożenie szablonu {nazwa_szablonu} nie powiodło się. CorrelationId={guid} |W zapleczu usługi migracji używamy szablonów usługi Azure Resource Manager do tworzenia zasobów w stosie usługi Azure Resource Manager. Ponieważ szablony są idempotentne, zwykle możesz bezpiecznie ponowić próbę operacji migracji, aby pokonać ten błąd. Jeśli ten błąd nie ustąpi, [skontaktuj się z pomocą techniczną platformy Azure](../articles/azure-supportability/how-to-create-azure-support-request.md) i podaj im wartość CorrelationId. <br><br> **UWAGA:** Gdy zdarzenie jest śledzone przez zespół pomocy technicznej, nie należy podejmować żadnych samodzielnych ponieważ może to mieć niezamierzone konsekwencje w używanym środowisku. |
| Sieć wirtualna {nazwa_sieci_wirtualnej} nie istnieje. |Może to się zdarzyć, jeśli sieć wirtualna została utworzona w nowej witrynie Azure Portal. Rzeczywista nazwa sieci wirtualnej jest zgodny ze wzorcem "grupa * \<nazwa sieci Wirtualnej >" |
| Maszyna wirtualna {nazwa_maszyny_wirtualnej} w usłudze hostowanej {nazwa_usługi_hostowanej} zawiera rozszerzenie {nazwa_rozszerzenia}, które nie jest obsługiwane w usłudze Azure Resource Manager. Zaleca się odinstalowanie go z maszyny wirtualnej przed kontynuowaniem migracji. |Rozszerzenia XML, takie jak BGInfo 1. \* nie są obsługiwane w usłudze Azure Resource Manager. W związku z tym nie można migrować tych rozszerzeń. Jeśli te rozszerzenia pozostaną zainstalowane na maszynie wirtualnej, zostaną automatycznie odinstalowane przed ukończeniem migracji. |
| Maszyna wirtualna {nazwa_maszyny_wirtualnej} w usłudze hostowanej {nazwa_usługi_hostowanej} zawiera rozszerzenie VMSnapshot/VMSnapshotLinux, które nie jest obecnie obsługiwane dla migracji. Odinstaluj je z maszyny wirtualnej i dodaj ponownie za pomocą usługi Azure Resource Manager po zakończeniu migracji |Jest to scenariusz, w którym maszyna wirtualna jest skonfigurowana na potrzeby usługi Azure Backup. Ponieważ obecnie jest to nieobsługiwany scenariusz, wykonaj obejście opisane w https://aka.ms/vmbackupmigration |
| Maszyna wirtualna {nazwa_maszyny_wirtualnej} w usłudze hostowanej {nazwa_usługi_hostowanej} zawiera rozszerzenie {nazwa_rozszerzenia}, którego stan nie jest zgłaszany z maszyny wirtualnej. Z tego powodu nie można migrować maszyny wirtualnej. Upewnij się, że stan rozszerzenia jest zgłaszany, lub odinstaluj rozszerzenie z maszyny wirtualnej i ponów próbę migracji. <br><br> Maszyna wirtualna {nazwa_maszyny_wirtualnej} w usłudze hostowanej {nazwa_usługi_hostowanej} zawiera rozszerzenie {nazwa_rozszerzenia} zgłaszające stan procedury obsługi: {stan_procedury_obsługi}. Z tego powodu nie można migrować maszyny wirtualnej. Upewnij się, że zgłaszanym stanem procedury obsługi rozszerzenia jest {stan_procedury_obsługi} lub odinstaluj rozszerzenie z maszyny wirtualnej i ponów próbę migracji. <br><br> Agent maszyny wirtualnej dla maszyny wirtualnej {nazwa_maszyny_wirtualnej} w usłudze hostowanej {nazwa_usługi_hostowanej} zgłasza ogólny stan agenta jako Niegotowy. Z tego powodu nie można migrować maszyny wirtualnej, jeśli ma ona rozszerzenie, które można migrować. Upewnij się, że agent maszyny wirtualnej zgłasza ogólny stan agenta jako Gotowy. Zapoznaj się https://aka.ms/classiciaasmigrationfaqs. |Agent gościa platformy Azure i rozszerzenia maszyny wirtualnej potrzebują wychodzącego dostępu do Internetu dla konta magazynu maszyny wirtualnej w celu wypełnienia ich stanu. Typowe przyczyny niepowodzenia stanu obejmują następujące sytuacje: <li> grupa zabezpieczeń sieci blokuje wychodzący dostęp do Internetu, <li> Jeśli sieć wirtualna ma lokalnych serwerów DNS i DNS połączenie zostało utracone <br><br> Jeśli nieobsługiwany stan jest nadal wyświetlany, możesz odinstalować rozszerzenia, aby pominąć to sprawdzanie i kontynuować migrację. |
| Migracja wdrożenia {nazwa_wdrożenia} w usłudze hostowanej {nazwa_usługi_hostowanej} nie jest obsługiwana, ponieważ zawiera ono wiele zestawów dostępności. |Obecnie można migrować tylko usługi hostowane, które mają nie więcej niż 1 zestaw dostępności. Aby obejść ten problem, przenieś dodatkowe zestawy dostępności i maszyny wirtualne w tych zestawach dostępności do innej usługi hostowanej. |
| Migracja wdrożenia {nazwa_wdrożenia} w usłudze hostowanej {nazwa_usługi_hostowanej} nie jest obsługiwana, ponieważ zawiera ono maszyny wirtualne niebędące częścią zestawu dostępności, choć usługa hostowana zawiera taki zestaw. |Obejście problemu w przypadku tego scenariusza polega na przeniesieniu wszystkich maszyn wirtualnych do jednego zestawu dostępności lub usunięciu wszystkich maszyn wirtualnych z zestawu dostępności w usłudze hostowanej. |
| Konto magazynu/usługa hostowana/sieć wirtualna {nazwa_sieci_wirtualnej} jest w trakcie migracji i dlatego nie można jej zmienić |Ten błąd występuje w przypadku, gdy po zakończeniu operacji migracji „Prepare” dla zasobu wyzwalana jest operacja, która spowodowałaby wprowadzenie zmiany w zasobie. Z powodu blokady na płaszczyźnie zarządzania po wykonaniu operacji „Prepare” wszelkie zmiany zasobu są blokowane. Aby odblokować płaszczyznę zarządzania, można uruchomić operację migracji „Commit” w celu ukończenia migracji lub operację migracji „Abort” w celu wycofania operacji „Prepare”. |
| Migracja nie jest dozwolona dla usługi hostowanej {hostowane nazwa_usługi_hostowanej}, ponieważ ma ona maszynę Wirtualną {nazwa_maszyny_wirtualnej} w stanie: RoleStateUnknown. Migracja jest dozwolona tylko wtedy, gdy maszyna wirtualna jest w jednym z następujących stanów: Uruchomiona, Zatrzymana, Zatrzymana bez alokacji. |Maszyna wirtualna może przechodzić zmianę stanu, co zazwyczaj ma miejsce podczas operacji aktualizacji w usłudze hostowanej, na przykład ponownego uruchomienia komputera, instalacji rozszerzenia itp. Zaleca się zakończenie operacji aktualizacji w usłudze hostowanej przed podjęciem próby migracji. |
| Wdrożenie {nazwa_wdrożenia} w usłudze hostowanej {nazwa_usługi_hostowanej} zawiera maszynę wirtualną {nazwa_maszyny_wirtualnej} z dyskiem danych {nazwa_dysku_danych}. Rozmiar fizyczny obiektu blob ({rozmiar_obiektu_blob_pliku_vhd__dysku_danych} B) tego dysku nie jest zgodny z rozmiarem logicznym dysku danych maszyny wirtualnej ({rozmiar_dysku_danych_określony_w_API_maszyny_wirtualnej} B). Migracja będzie kontynuowana bez określania rozmiaru dysku danych maszyny wirtualnej usługi Azure Resource Manager. | Ten błąd występuje, jeśli rozmiar obiektu blob dysku VHD został zmieniony bez zaktualizowania rozmiaru w modelu interfejsu API maszyny wirtualnej. Szczegółowe kroki zaradcze przedstawiono [poniżej](#vm-with-data-disk-whose-physical-blob-size-bytes-does-not-match-the-vm-data-disk-logical-size-bytes).|
| Wystąpił wyjątek magazynu podczas weryfikowania dysku danych {nazwa_dysku_danych} z linkiem multimediów {identyfikator_URI_dysku_danych} dla maszyny wirtualnej {nazwa_maszyny_wirtualnej} w usłudze w chmurze {nazwa_usługi_w_chmurze}. Upewnij się, że link multimediów wirtualnego dysku twardego jest dostępny dla tej maszyny wirtualnej | Ten błąd może wystąpić, jeśli dyski maszyny wirtualnej zostały usunięte lub nie są już dostępne. Upewnij się, że dyski maszyny wirtualnej istnieją.|
| Maszyna wirtualna {nazwa_maszyny_wirtualnej} w usłudze hostowanej {nazwa_usługi_w_chmurze} zawiera dysk z linkiem multimediów {identyfikator_URI_pliku_vhd} mającym nazwę obiektu blob {nazwa_obiektu_blob_pliku_vhd}, który nie jest obsługiwany w usłudze Azure Resource Manager. | Ten błąd występuje, gdy nazwa obiektu blob zawiera znak „/”, który nie jest aktualnie obsługiwany przez dostawcę zasobów obliczeniowych. |
| Migracja wdrożenia {nazwa_wdrożenia} w usłudze hostowanej {nazwa_usługi_w_chmurze} nie jest dozwolona, ponieważ nie znajduje się ono w zakresie regionalnym. Zob. https:\//aka.ms/regionalscope przenoszenia to wdrożenie do zakresu regionalnego. | W 2014 r. zespół platformy Azure zapowiedział, że zasoby sieciowe zostaną przeniesione z zakresu poziomu klastra do zakresu regionalnego. Zobacz [ https://aka.ms/regionalscope ](https://aka.ms/regionalscope) Aby uzyskać więcej informacji. Ten błąd zdarza się, gdy migrowane wdrożenie nie zostało poddane operacji aktualizacji, która automatycznie przenosi je do zakresu regionalnego. Najlepszym obejściem tego problemu jest dodanie punktu końcowego lub dysku danych do maszyny wirtualnej, a następnie ponowienie próby przeprowadzenia migracji. <br> Zobacz [How to set up endpoints on a classic Windows virtual machine in Azure](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints#create-an-endpoint) (Jak skonfigurować punkty końcowe na klasycznej maszynie wirtualnej z systemem Windows na platformie Azure) lub [Dołączanie dysku danych do maszyny wirtualnej systemu Windows przy użyciu klasycznego modelu wdrażania](../articles/virtual-machines/windows/classic/attach-disk.md)|
| Migracja nie jest obsługiwana dla sieci wirtualnej {nazwa sieci wirtualnej}, ponieważ zawiera ona wdrożenia PaaS bez bram. | Ten błąd występuje w przypadku wdrożeń typu PaaS bez bram, takich jak usługi bramy aplikacji lub usługi API Management, które są podłączone do sieci wirtualnej.|


## <a name="detailed-mitigations"></a>Szczegółowe środki zaradcze

### <a name="vm-with-data-disk-whose-physical-blob-size-bytes-does-not-match-the-vm-data-disk-logical-size-bytes"></a>Maszyna wirtualna, której rozmiar fizyczny obiektu blob (w bajtach) nie jest zgodny z rozmiarem logicznym dysku danych maszyny wirtualnej (w bajtach).

Dzieje się tak, gdy rozmiar logiczny dysku danych nie jest zsynchronizowany z rzeczywistym rozmiarem obiektu blob wirtualnego dysku twardego. Można to łatwo sprawdzić przy użyciu następujących poleceń:

#### <a name="verifying-the-issue"></a>Weryfikowanie problemu

```powershell
# Store the VM details in the VM object
$vm = Get-AzureVM -ServiceName $servicename -Name $vmname

# Display the data disk properties
# NOTE the data disk LogicalDiskSizeInGB below which is 11GB. Also note the MediaLink Uri of the VHD blob as we'll use this in the next step
$vm.VM.DataVirtualHardDisks


HostCaching         : None
DiskLabel           : 
DiskName            : coreosvm-coreosvm-0-201611230636240687
Lun                 : 0
LogicalDiskSizeInGB : 11
MediaLink           : https://contosostorage.blob.core.windows.net/vhds/coreosvm-dd1.vhd
SourceMediaLink     : 
IOType              : Standard
ExtensionData       : 

# Now get the properties of the blob backing the data disk above
# NOTE the size of the blob is about 15 GB which is different from LogicalDiskSizeInGB above
$blob = Get-AzStorageblob -Blob "coreosvm-dd1.vhd" -Container vhds 

$blob

ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudPageBlob
BlobType          : PageBlob
Length            : 16106127872
ContentType       : application/octet-stream
LastModified      : 11/23/2016 7:16:22 AM +00:00
SnapshotTime      : 
ContinuationToken : 
Context           : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext
Name              : coreosvm-dd1.vhd
```

#### <a name="mitigating-the-issue"></a>Korygowanie problemu

```powershell
# Convert the blob size in bytes to GB into a variable which we'll use later
$newSize = [int]($blob.Length / 1GB)

# See the calculated size in GB
$newSize

15

# Store the disk name of the data disk as we'll use this to identify the disk to be updated
$diskName = $vm.VM.DataVirtualHardDisks[0].DiskName

# Identify the LUN of the data disk to remove
$lunToRemove = $vm.VM.DataVirtualHardDisks[0].Lun

# Now remove the data disk from the VM so that the disk isn't leased by the VM and it's size can be updated
Remove-AzureDataDisk -LUN $lunToRemove -VM $vm | Update-AzureVm -Name $vmname -ServiceName $servicename

OperationDescription OperationId                          OperationStatus
-------------------- -----------                          ---------------
Update-AzureVM       213xx1-b44b-1v6n-23gg-591f2a13cd16   Succeeded  

# Verify we have the right disk that's going to be updated
Get-AzureDisk -DiskName $diskName

AffinityGroup        : 
AttachedTo           : 
IsCorrupted          : False
Label                : 
Location             : East US
DiskSizeInGB         : 11
MediaLink            : https://contosostorage.blob.core.windows.net/vhds/coreosvm-dd1.vhd
DiskName             : coreosvm-coreosvm-0-201611230636240687
SourceImageName      : 
OS                   : 
IOType               : Standard
OperationDescription : Get-AzureDisk
OperationId          : 0c56a2b7-a325-123b-7043-74c27d5a61fd
OperationStatus      : Succeeded

# Now update the disk to the new size
Update-AzureDisk -DiskName $diskName -ResizedSizeInGB $newSize -Label $diskName

OperationDescription OperationId                          OperationStatus
-------------------- -----------                          ---------------
Update-AzureDisk     cv134b65-1b6n-8908-abuo-ce9e395ac3e7 Succeeded 

# Now verify that the "DiskSizeInGB" property of the disk matches the size of the blob 
Get-AzureDisk -DiskName $diskName


AffinityGroup        : 
AttachedTo           : 
IsCorrupted          : False
Label                : coreosvm-coreosvm-0-201611230636240687
Location             : East US
DiskSizeInGB         : 15
MediaLink            : https://contosostorage.blob.core.windows.net/vhds/coreosvm-dd1.vhd
DiskName             : coreosvm-coreosvm-0-201611230636240687
SourceImageName      : 
OS                   : 
IOType               : Standard
OperationDescription : Get-AzureDisk
OperationId          : 1v53bde5-cv56-5621-9078-16b9c8a0bad2
OperationStatus      : Succeeded

# Now we'll add the disk back to the VM as a data disk. First we need to get an updated VM object
$vm = Get-AzureVM -ServiceName $servicename -Name $vmname

Add-AzureDataDisk -Import -DiskName $diskName -LUN 0 -VM $vm -HostCaching ReadWrite | Update-AzureVm -Name $vmname -ServiceName $servicename

OperationDescription OperationId                          OperationStatus
-------------------- -----------                          ---------------
Update-AzureVM       b0ad3d4c-4v68-45vb-xxc1-134fd010d0f8 Succeeded      
```

### <a name="moving-a-vm-to-a-different-subscription-after-completing-migration"></a>Przenoszenie maszyny wirtualnej do innej subskrypcji po zakończeniu migracji

Po zakończeniu procesu migracji można przenieść maszynę wirtualną do innej subskrypcji. Jednak jeśli masz wpis tajny/certyfikat na maszynie wirtualnej, który odwołuje się do zasobu usługi Key Vault, przenoszenie aktualnie nie jest obsługiwane. Poniższe instrukcje umożliwią obejście tego problemu. 

#### <a name="powershell"></a>PowerShell
```powershell
$vm = Get-AzVM -ResourceGroupName "MyRG" -Name "MyVM"
Remove-AzVMSecret -VM $vm
Update-AzVM -ResourceGroupName "MyRG" -VM $vm
```
#### <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

```bash
az vm update -g "myrg" -n "myvm" --set osProfile.Secrets=[]
```
