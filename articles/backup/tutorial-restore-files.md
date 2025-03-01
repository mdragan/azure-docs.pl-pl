---
title: Przywracanie plików na maszynę wirtualną przy użyciu usługi Azure Backup
description: Dowiedz się, jak przeprowadzić przywracanie na poziomie plików na maszynie wirtualnej platformy Azure przy użyciu usług Backup i Recovery Services.
services: backup
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 01/31/2019
ms.author: dacurwin
ms.custom: mvc
ms.openlocfilehash: 801d812f44eacb4953ca3df942d65bd77f210478
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273927"
---
# <a name="restore-files-to-a-virtual-machine-in-azure"></a>Przywracanie plików na maszynę wirtualną na platformie Azure
Usługa Azure Backup tworzy punkty odzyskiwania przechowywane w geograficznie nadmiarowych magazynach odzyskiwania. Z punktu odzyskiwania można przywrócić całą maszynę wirtualną lub poszczególne pliki. W tym artykule szczegółowo przedstawiono sposób przywracania poszczególnych plików. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wyświetlanie listy i wybieranie punktów odzyskiwania
> * Łączenie punktu odzyskiwania z maszyną wirtualną
> * Przywracanie plików z punktu odzyskiwania

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten samouczek będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.18 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie interfejsu, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure](/cli/azure/install-azure-cli). 


## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania tego samouczka jest wymagana maszyna wirtualna z systemem Linux, chroniona przy użyciu usługi Azure Backup. Aby zasymulować proces przypadkowego usunięcia pliku i odzyskania go, usuniesz stronę z serwera internetowego. Jeśli potrzebujesz maszyny wirtualnej z systemem Linux obsługującej serwer internetowy i chronionej przy użyciu usługi Azure Backup, zobacz [Tworzenie kopii zapasowej maszyny wirtualnej na platformie Azure za pomocą interfejsu wiersza polecenia](quick-backup-vm-cli.md).


## <a name="backup-overview"></a>Omówienie usługi Backup
Po zainicjowaniu tworzenia kopii zapasowej przez platformę Azure rozszerzenie kopii zapasowej na maszynie wirtualnej tworzy migawkę punktu w czasie. Rozszerzenie kopii zapasowej jest instalowane na maszynie wirtualnej w momencie pierwszego żądania utworzenia kopii zapasowej. Usługa Azure Backup również może utworzyć migawkę bazowego magazynu, jeśli maszyna wirtualna nie jest uruchomiona w momencie tworzenia kopii zapasowej.

Domyślnie usługa Azure Backup tworzy kopię zapasową spójną na poziomie systemu plików. Po utworzeniu migawki w usłudze Azure Backup dane są przesyłane do magazynu usługi Recovery Services. Aby zmaksymalizować wydajność, usługa Azure Backup rozpoznaje i przesyła jedynie te bloki danych, które uległy zmianie od czasu utworzenia poprzedniej kopii zapasowej.

Po ukończeniu przesyłania danych migawka jest usuwana, a utworzony zostaje punkt odzyskiwania.


## <a name="delete-a-file-from-a-vm"></a>Usuwanie pliku z maszyny wirtualnej
Jeśli przypadkowo usuniesz lub zmienisz plik, możesz przywrócić pojedyncze pliki z punktu odzyskiwania. Ten proces umożliwia przeglądanie plików, których kopia zapasowa jest przechowywana w punkcie odzyskiwania, a następnie przywrócenie tylko wybranych plików. W tym przykładzie usuniemy plik z serwera internetowego, aby przedstawić proces odzyskiwania na poziomie pliku.

1. Aby nawiązać połączenie z maszyną wirtualną, uzyskaj jej adres IP przy użyciu polecenia [az vm show](/cli/azure/vm?view=azure-cli-latest#az-vm-show):

     ```azurecli-interactive
     az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
     ```

2. Aby sprawdzić, czy witryna internetowa aktualnie działa, otwórz przeglądarkę internetową i przejdź do publicznego adresu IP maszyny wirtualnej. Pozostaw otwarte okno przeglądarki internetowej.

    ![Domyślna strona internetowa serwera NGINX](./media/tutorial-restore-files/nginx-working.png)

3. Połącz się z maszyną wirtualną przy użyciu protokołu SSH. Zastąp ciąg *publicIpAddress* publicznym adresem IP uzyskanym przy użyciu poprzedniego polecenia:

    ```bash
    ssh publicIpAddress
    ```

4. Usuń domyślną stronę z lokalizacji */var/www/html/index.nginx-debian.html* na serwerze internetowym, jak pokazano poniżej:

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```

5. Odśwież stronę internetową w przeglądarce. Witryna internetowa nie będzie już ładować tej strony, jak pokazano na poniższym przykładzie:

    ![Witryna internetowa NGINX nie ładuje już strony domyślnej](./media/tutorial-restore-files/nginx-broken.png)

6. Zamknij sesję SSH z maszyną wirtualną w następujący sposób:

    ```bash
    exit
    ```


## <a name="generate-file-recovery-script"></a>Generowanie skryptu odzyskiwania plików
Na potrzeby przywracania plików usługa Azure Backup udostępnia skrypt do uruchomienia na maszynie wirtualnej, który łączy ją z punktem odzyskiwania jako dyskiem lokalnym. Możesz przeglądać zawartość tego dysku lokalnego, przywrócić pliki bezpośrednio na maszynę wirtualną, a następnie zakończyć połączenie z punktem odzyskiwania. Kopia zapasowa danych będzie nadal tworzona w usłudze Azure Backup zgodnie z przypisanymi zasadami harmonogramu i przechowywania.

1. Aby wyświetlić listę punktów odzyskiwania dla maszyny wirtualnej, użyj polecenia [az backup recoverypoint list](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-list). W tym przykładzie wybierzemy najnowszy punkt odzyskiwania dla maszyny wirtualnej o nazwie *myVM*, chronionej w magazynie *myRecoveryServicesVault*:

    ```azurecli-interactive
    az backup recoverypoint list \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --query [0].name \
        --output tsv
    ```

2. Aby uzyskać skrypt umożliwiający połączenie z punktem odzyskiwania — innymi słowy, zainstalowanie go — na maszynie wirtualnej, użyj polecenia [az backup restore files mount-rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az-backup-restore-files-mount-rp). Poniższy przykład umożliwia uzyskanie skryptu dla maszyny wirtualnej o nazwie *myVM*, chronionej w magazynie *myRecoveryServicesVault*.

    Zastąp ciąg *myRecoveryPointName* nazwą punktu odzyskiwania uzyskaną przy użyciu poprzedniego polecenia:

    ```azurecli-interactive
    az backup restore files mount-rp \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --rp-name myRecoveryPointName
    ```

    Skrypt zostanie pobrany i zostanie wyświetlone hasło, zgodnie z poniższym przykładem:

    ```
    File downloaded: myVM_we_1571974050985163527.sh. Use password c068a041ce12465
    ```

3. Aby przesłać skrypt do maszyny wirtualnej, użyj narzędzia Secure Copy (SCP). Podaj nazwę pobranego skryptu i zastąp ciąg *publicIpAddress* publicznym adresem IP maszyny wirtualnej. Pamiętaj o uwzględnieniu końcowego znaku `:` na końcu polecenia SCP, jak pokazano poniżej:

    ```bash
    scp myVM_we_1571974050985163527.sh 52.174.241.110:
    ```


## <a name="restore-file-to-your-vm"></a>Przywracanie plików na maszynę wirtualną
Po skopiowaniu skryptu odzyskiwania na maszynę wirtualną możesz nawiązać połączenie z punktem odzyskiwania i przywrócić pliki.

1. Połącz się z maszyną wirtualną przy użyciu protokołu SSH. Zastąp ciąg *publicIpAddress* publicznym adresem IP maszyny wirtualnej, jak pokazano poniżej:

    ```bash
    ssh publicIpAddress
    ```

2. Aby umożliwić prawidłowe wykonanie skryptu, dodaj uprawnienia do wykonywania przy użyciu polecenia **chmod**. Wprowadź nazwę własnego skryptu:

    ```bash
    chmod +x myVM_we_1571974050985163527.sh
    ```

3. Aby zainstalować punkt odzyskiwania, uruchom skrypt. Wprowadź nazwę własnego skryptu:

    ```bash
    ./myVM_we_1571974050985163527.sh
    ```

    Po uruchomieniu skryptu zostanie wyświetlony monit o wprowadzenie hasła dostępu do punktu odzyskiwania. Wprowadź hasło wyświetlone w danych wyjściowych polecenia [az backup restore files mount-rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az-backup-restore-files-mount-rp) użytego wcześniej do wygenerowania skryptu odzyskiwania.

    Dane wyjściowe skryptu zawierają ścieżkę punktu odzyskiwania. Dane wyjściowe w poniższym przykładzie wskazują, że punkt odzyskiwania został zainstalowany w lokalizacji */home/azureuser/myVM-20170919213536/Volume1*:

    ```
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    Please enter the password as shown on the portal to securely connect to the recovery point. : c068a041ce12465
    
    Connecting to recovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of the recovery point to this machine...
    
    ************ Volumes of the recovery point and their mount paths on this machine ************
    
    Sr.No.  |  Disk  |  Volume  |  MountPath
    
    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170919213536/Volume1
    
    ************ Open File Explorer to browse for files. ************
    ```

4. Użyj polecenia **cp**, aby skopiować domyślną stronę internetową serwera NGINX z zainstalowanego punktu odzyskiwania z powrotem do pierwotnej lokalizacji pliku. Zastąp lokalizację instalacji */home/azureuser/myVM-20170919213536/Volume1* własną lokalizacją:

    ```bash
    sudo cp /home/azureuser/myVM-20170919213536/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```

6. Odśwież stronę internetową w przeglądarce. Witryna internetowa znów będzie poprawnie ładowana, jak pokazano na poniższym przykładzie:

    ![Witryna internetowa NGINX jest znów poprawnie ładowana](./media/tutorial-restore-files/nginx-restored.png)

7. Zamknij sesję SSH z maszyną wirtualną w następujący sposób:

    ```bash
    exit
    ```

8. Odinstaluj punkt odzyskiwania z maszyny wirtualnej przy użyciu polecenia [az backup restore files unmount-rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az-backup-restore-files-unmount-rp). Poniższy przykład umożliwia odinstalowanie punktu odzyskiwania z maszyny wirtualnej o nazwie *myVM* w magazynie *myRecoveryServicesVault*.

    Zastąp ciąg *myRecoveryPointName* nazwą punktu odzyskiwania uzyskaną przy użyciu poprzednich poleceń:
    
    ```azurecli-interactive
    az backup restore files unmount-rp \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --rp-name myRecoveryPointName
    ```

## <a name="next-steps"></a>Kolejne kroki
Podczas pracy z tym samouczkiem połączono punkt odzyskiwania z maszyną wirtualną i przywrócono pliki serwera internetowego. W tym samouczku omówiono:

> [!div class="checklist"]
> * Wyświetlanie listy i wybieranie punktów odzyskiwania
> * Łączenie punktu odzyskiwania z maszyną wirtualną
> * Przywracanie plików z punktu odzyskiwania

Przejdź do następnego samouczka, aby dowiedzieć się, jak utworzyć kopię zapasową serwera z systemem Windows na platformie Azure.

> [!div class="nextstepaction"]
> [Tworzenie kopii zapasowej serwerów z systemem Windows na platformie Azure](tutorial-backup-windows-server-to-azure.md)

