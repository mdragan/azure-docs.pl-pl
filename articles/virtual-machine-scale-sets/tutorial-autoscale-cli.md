---
title: Samouczek — skalowanie automatyczne zestawu skalowania przy użyciu interfejsu wiersza polecenia platformy Azure | Microsoft Docs
description: Dowiedz się, jak za pomocą interfejsu wiersza polecenia platformy Azure automatycznie skalować zestaw skalowania maszyn wirtualnych w odpowiedzi na wzrosty i spadki zapotrzebowania na procesor CPU
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/18/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 4064816ae932a0f26fd3478420c69f3e8fba8732
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60188908"
---
# <a name="tutorial-automatically-scale-a-virtual-machine-scale-set-with-the-azure-cli"></a>Samouczek: Automatyczne skalowanie zestawu skalowania maszyn wirtualnych przy użyciu interfejsu wiersza polecenia platformy Azure

Podczas tworzenia zestawu skalowania musisz zdefiniować liczbę wystąpień maszyn wirtualnych, które chcesz uruchamiać. W odpowiedzi na zmieniające się zapotrzebowanie aplikacji możesz automatycznie zwiększać lub zmniejszać liczbę wystąpień maszyn wirtualnych. Skalowanie automatyczne pozwala spełniać potrzeby klientów lub reagować na zmiany wydajności aplikacji w całym cyklu jej życia. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Używanie funkcji skalowania automatycznego z zestawem skalowania
> * Tworzenie reguł skalowania automatycznego i korzystanie z nich
> * Testy obciążeniowe wystąpień maszyn wirtualnych i wyzwalanie reguł skalowania automatycznego
> * Skalowanie automatyczne do wewnątrz po zmniejszeniu zapotrzebowania

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten samouczek będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.32 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure]( /cli/azure/install-azure-cli).

## <a name="create-a-scale-set"></a>Tworzenie zestawu skalowania

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group):

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Teraz utwórz zestaw skalowania maszyn wirtualnych przy użyciu polecenia [az vmss create](/cli/azure/vmss). W poniższym przykładzie pokazano tworzenie zestawu skalowania z liczbą wystąpień równą *2* i generowanie kluczy SSH, jeśli nie istnieją:

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --instance-count 2 \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="define-an-autoscale-profile"></a>Definiowanie profilu skalowania automatycznego

Aby włączyć skalowanie automatyczne na zestawie skalowania, najpierw zdefiniuj profil skalowania automatycznego. Ten profil obejmuje definiowanie domyślnej, minimalnej i maksymalnej pojemności zestawu skalowania. Dzięki tym limitom możesz kontrolować koszty, ponieważ wystąpienia maszyn wirtualnych nie są tworzone w sposób ciągły, zaś akceptowalna wydajność jest zrównoważona z minimalną liczbą wystąpień, które pozostają w zdarzeniu skalowania w pionie. Utwórz profil skalowania automatycznego za pomocą polecenia [az monitor autoscale create](/cli/azure/monitor/autoscale#az-monitor-autoscale-create). W poniższym przykładzie ustawiono domyślną, minimalną pojemność — *2* — oraz maksymalną pojemność — *10* wystąpień maszyn wirtualnych:

```azurecli-interactive
az monitor autoscale create \
  --resource-group myResourceGroup \
  --resource myScaleSet \
  --resource-type Microsoft.Compute/virtualMachineScaleSets \
  --name autoscale \
  --min-count 2 \
  --max-count 10 \
  --count 2
```

## <a name="create-a-rule-to-autoscale-out"></a>Tworzenie reguły skalowania automatycznego w poziomie

Wraz ze wzrostem zapotrzebowania aplikacji zwiększa się obciążenie wystąpień maszyn wirtualnych w zestawie skalowania. Jeśli wzrost obciążenia ma cechy stałego trendu, można skonfigurować reguły skalowania automatycznego umożliwiające zwiększenie liczby wystąpień maszyn wirtualnych w zestawie skalowania. Gdy aplikacje zostaną wdrożone, zestaw skalowania rozpoczyna kierowanie ruchu do nowo utworzonych wystąpień maszyn wirtualnych za pośrednictwem modułu równoważenia obciążenia. Możesz określić, jakie metryki mają być monitorowane, na przykład procesor CPU lub dysk, oraz jak długo obciążenie aplikacji musi przekraczać wartość progową, a także ile wystąpień maszyn wirtualnych ma zostać dodanych do zestawu skalowania.

Za pomocą polecenia [az monitor autoscale rule create](/cli/azure/monitor/autoscale/rule#az-monitor-autoscale-rule-create) utworzymy regułę, która zwiększa liczbę wystąpień maszyn wirtualnych w zestawie skalowania, jeśli w okresie 5 minut średnie obciążenie procesora CPU wzrośnie ponad 70%. Wyzwolenie reguły powoduje zwiększenie liczby wystąpień maszyn wirtualnych o trzy.

```azurecli-interactive
az monitor autoscale rule create \
  --resource-group myResourceGroup \
  --autoscale-name autoscale \
  --condition "Percentage CPU > 70 avg 5m" \
  --scale out 3
```

## <a name="create-a-rule-to-autoscale-in"></a>Tworzenie reguły skalowania automatycznego w pionie

Wieczorami lub w weekendy zapotrzebowanie aplikacji może być mniejsze. Jeśli spadek obciążenia ma cechy stałego trendu w danym okresie, można skonfigurować reguły skalowania automatycznego umożliwiające zmniejszenie liczby wystąpień maszyn wirtualnych w zestawie skalowania. Akcja skalowania w pionie ogranicza koszt używania zestawu skalowania, ponieważ jest uruchomionych tylko tyle wystąpień, ile jest wymaganych do zaspokojenia bieżącego zapotrzebowania.

Za pomocą polecenia [az monitor autoscale rule create](/cli/azure/monitor/autoscale/rule#az-monitor-autoscale-rule-create) utwórz inną regułę, która zmniejsza liczbę wystąpień maszyn wirtualnych w zestawie skalowania, jeśli w okresie 5 minut średnie obciążenie procesora CPU spadnie poniżej 30%. W poniższym przykładzie zdefiniowano regułę umożliwiającą skalowanie w pionie liczby wystąpień maszyn wirtualnych o jeden:

```azurecli-interactive
az monitor autoscale rule create \
  --resource-group myResourceGroup \
  --autoscale-name autoscale \
  --condition "Percentage CPU < 30 avg 5m" \
  --scale in 1
```

## <a name="generate-cpu-load-on-scale-set"></a>Generowanie obciążenia procesora CPU w zestawie skalowania

Przetestowanie reguł skalowania automatycznego wymaga wygenerowania obciążenia procesora CPU na wystąpieniach maszyn wirtualnych w zestawie skalowania. Symulowane obciążenie procesora CPU powoduje uruchomienie skalowania w poziomie przez reguły skalowania automatycznego, a w efekcie zwiększenie liczby wystąpień maszyn wirtualnych. Gdy symulowane obciążenie procesora CPU zmniejszy się, nastąpi automatyczne skalowanie w pionie i ograniczenie liczby wystąpień maszyn wirtualnych.

Najpierw za pomocą polecenia [az vmss list-instance-connection-info](/cli/azure/vmss) podaj listę adresów i portów służących do nawiązania połączenia z wystąpieniami maszyn wirtualnych w zestawie skalowania:

```azurecli-interactive
az vmss list-instance-connection-info \
  --resource-group myResourceGroup \
  --name myScaleSet
```

Następujące przykładowe dane wyjściowe zawierają nazwę wystąpienia, publiczny adres IP modułu równoważenia obciążenia oraz numer portu, do którego reguły translatora adresów sieciowych (NAT) kierują ruch:

```json
{
  "instance 1": "13.92.224.66:50001",
  "instance 3": "13.92.224.66:50003"
}
```

Połącz się przez protokół SSH z pierwszym wystąpieniem maszyny wirtualnej. Podaj własny publiczny adres IP i numer portu za pomocą parametru `-p`, jak pokazano w poprzednim poleceniu:

```azurecli-interactive
ssh azureuser@13.92.224.66 -p 50001
```

Po zalogowaniu zainstaluj narzędzie **stress**. Uruchom *10* procesów roboczych narzędzia **stress**, które generują obciążenie procesora CPU. Procesy robocze będą działać przez *420* sekund, co wystarczy do zaimplementowania odpowiedniej akcji przez reguły skalowania automatycznego.

```azurecli-interactive
sudo apt-get -y install stress
sudo stress --cpu 10 --timeout 420 &
```

Kiedy narzędzie **stress** pokazuje dane wyjściowe podobne do następujących: *stress: info: [2688] dispatching hogs: 10 cpu, 0 io, 0 vm, 0 hdd*, naciśnij klawisz *Enter*, aby powrócić do wiersza polecenia.

Aby potwierdzić obecność wygenerowanego obciążenia procesora CPU przez narzędzie **stress**, sprawdź aktywne obciążenie systemu za pomocą narzędzia **top**:

```azurecli-interactive
top
```

Zakończ działanie narzędzia **top**, a następnie zamknij połączenie z wystąpieniem maszyny wirtualnej. Narzędzie **stress** będzie kontynuować działanie na wystąpieniu maszyny wirtualnej.

```azurecli-interactive
Ctrl-c
exit
```

Połącz się z drugim wystąpieniem maszyny wirtualnej przy użyciu numeru portu z poprzedniego polecenia [az vmss list-instance-connection-info](/cli/azure/vmss):

```azurecli-interactive
ssh azureuser@13.92.224.66 -p 50003
```

Zainstaluj i uruchom narzędzie **stress**, a następnie uruchom dziesięć procesów roboczych na drugim wystąpieniu maszyny wirtualnej.

```azurecli-interactive
sudo apt-get -y install stress
sudo stress --cpu 10 --timeout 420 &
```

Ponownie kiedy narzędzie **stress** pokazuje dane wyjściowe podobne do następujących: *stress: info: [2713] dispatching hogs: 10 cpu, 0 io, 0 vm, 0 hdd*, naciśnij klawisz *Enter*, aby powrócić do wiersza polecenia.

Zamknij połączenie z drugim wystąpieniem maszyny wirtualnej. Narzędzie **stress** będzie kontynuować działanie na wystąpieniu maszyny wirtualnej.

```azurecli-interactive
exit
```

## <a name="monitor-the-active-autoscale-rules"></a>Monitorowanie aktywnych reguł skalowania automatycznego

Do monitorowania liczby wystąpień maszyn wirtualnych w zestawie skalowania służy narzędzie **watch**. Po upływie 5 minut reguły skalowania automatycznego rozpoczynają proces skalowania w poziomie w odpowiedzi na obciążenie procesora CPU wygenerowane przez narzędzie **stress** na poszczególnych wystąpieniach maszyn wirtualnych:

```azurecli-interactive
watch az vmss list-instances \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --output table
```

Po osiągnięciu progu procesora CPU reguły skalowania automatycznego zwiększają liczbę wystąpień maszyn wirtualnych w zestawie skalowania. Następujące dane wyjściowe zawierają trzy maszyny wirtualne tworzone w zestawie skalowania podczas automatycznego skalowania w poziomie:

```bash
Every 2.0s: az vmss list-instances --resource-group myResourceGroup --name myScaleSet --output table

  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup    VmId
------------  --------------------  ----------  ------------  -------------------  ---------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            myResourceGroup  4f92f350-2b68-464f-8a01-e5e590557955
           2  True                  eastus      myScaleSet_2  Succeeded            myResourceGroup  d734cd3d-fb38-4302-817c-cfe35655d48e
           4  True                  eastus      myScaleSet_4  Creating             myResourceGroup  061b4c90-0d73-49fc-a066-19eab0b3d95c
           5  True                  eastus      myScaleSet_5  Creating             myResourceGroup  4beff8b9-4e65-40cb-9652-43899309da27
           6  True                  eastus      myScaleSet_6  Creating             myResourceGroup  9e4133dd-2c57-490e-ae45-90513ce3b336
```

Gdy narzędzie **stress** zakończy działanie na początkowych wystąpieniach maszyn wirtualnych, średnie obciążenie procesora CPU powróci do typowej wartości. Po kolejnych pięciu minutach reguły skalowania automatycznego inicjują skalowanie w pionie liczby wystąpień maszyn wirtualnych. Najpierw są usuwane wystąpienia maszyn wirtualnych o najwyższych identyfikatorach. Jeśli zestaw skalowania używa zestawów dostępności lub stref dostępności, akcje skalowania w pionie są dystrybuowane równomiernie na te wystąpienia maszyn wirtualnych. Następujące przykładowe dane wyjściowe przedstawiają usuwanie wystąpienia maszyny wirtualnej z zestawu skalowania podczas skalowania w pionie:

```bash
           6  True                  eastus      myScaleSet_6  Deleting             myResourceGroup  9e4133dd-2c57-490e-ae45-90513ce3b336
```

Zakończ działanie narzędzia *watch* za pomocą klawiszy `Ctrl-c`. Co pięć minut zestaw skalowania będzie powtarzał operację skalowania w pionie, usuwając po jednym wystąpieniu maszyny wirtualnej aż do osiągnięcia minimalnej liczby wystąpień równej dwa.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Aby pozbyć się zestawu skalowania i dodatkowych zasobów, usuń grupę zasobów wraz z całą zawartością za pomocą polecenia [az group delete](/cli/azure/group). Parametr `--no-wait` zwraca kontrolę do wiersza polecenia bez oczekiwania na zakończenie operacji. Parametr `--yes` potwierdza, że chcesz usunąć zasoby bez wyświetlania dodatkowego monitu.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku przedstawiono automatyczne skalowanie zestawu skalowania w pionie lub w poziomie za pomocą interfejsu wiersza polecenia platformy Azure:

> [!div class="checklist"]
> * Używanie funkcji skalowania automatycznego z zestawem skalowania
> * Tworzenie reguł skalowania automatycznego i korzystanie z nich
> * Testy obciążeniowe wystąpień maszyn wirtualnych i wyzwalanie reguł skalowania automatycznego
> * Skalowanie automatyczne do wewnątrz po zmniejszeniu zapotrzebowania

Więcej akcji zestawów skalowania maszyn wirtualnych zawierają następujące przykładowe skrypty interfejsu wiersza polecenia platformy Azure:

> [!div class="nextstepaction"]
> [Scale set script samples for Azure CLI (Przykładowe skrypty zestawu skalowania przeznaczone dla interfejsu wiersza polecenia platformy Azure)](cli-samples.md)
