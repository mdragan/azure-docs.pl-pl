---
title: Szybki start — tworzenie klastra usługi Azure Kubernetes Service (AKS)
description: Dowiedz się, jak szybko utworzyć klaster usługi Kubernetes przy użyciu szablonu usługi Azure Resource Manager i wdrażanie aplikacji w usłudze Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: quickstart
ms.date: 04/19/2019
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 524eb97a2c865a14800cf503edd7f506151521bb
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64920207"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-using-an-azure-resource-manager-template"></a>Szybki start: Wdrażanie klastra usługi Azure Kubernetes Service (AKS) przy użyciu szablonu usługi Azure Resource Manager

Azure Kubernetes Service (AKS) to zarządzana usługa platformy Kubernetes, która umożliwia szybkie wdrażanie klastrów i zarządzanie nimi. W tym przewodniku Szybki Start wdrożono klaster usługi AKS przy użyciu szablonu usługi Azure Resource Manager. W klastrze jest uruchamiana aplikacja obsługująca wiele kontenerów, która składa się z frontonu internetowego i wystąpienia pamięci podręcznej Redis.

![Obraz przedstawiający przechodzenie do aplikacji Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

W tym przewodniku Szybki start założono, że masz podstawową wiedzę na temat pojęć związanych z rozwiązaniem Kubernetes. Aby uzyskać więcej informacji, zobacz temat [Kubernetes core concepts for Azure Kubernetes Service (AKS)][kubernetes-concepts] (Kubernetes — podstawowe pojęcia dotyczące usługi Azure Kubernetes Service (AKS)).

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować i korzystać z interfejsu wiersza polecenia lokalnie, ten przewodnik Szybki Start wymaga, czy korzystasz z wiersza polecenia platformy Azure w wersji 2.0.61 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure][azure-cli-install].

## <a name="prerequisites"></a>Wymagania wstępne

Aby utworzyć klaster usługi AKS przy użyciu szablonu usługi Resource Manager, możesz podać klucz publiczny SSH i jednostkę usługi Azure Active Directory. Jeśli potrzebujesz jednej z tych zasobów, zobacz następującą sekcję; w przeciwnym razie przejdź do [Tworzenie klastra AKS](#create-an-aks-cluster) sekcji.

### <a name="create-an-ssh-key-pair"></a>Tworzenie pary kluczy SSH

Aby uzyskać dostęp do węzłów AKS, należy połączyć za pomocą pary kluczy SSH. Użyj `ssh-keygen` polecenie, aby wygenerować SSH pliki publicznych i prywatnych kluczy. Domyślnie te pliki są tworzone w *~/.ssh* katalogu. Jeśli pary kluczy SSH o takiej samej nazwie istnieje w danej lokalizacji, te pliki zostaną zastąpione.

Następujące polecenie tworzy parę kluczy SSH przy użyciu szyfrowania RSA i długość w bitach 2048:

```azurecli-interactive
ssh-keygen -t rsa -b 2048
```

Aby uzyskać więcej informacji na temat tworzenia kluczy SSH, zobacz [tworzenie i zarządzanie kluczami SSH do uwierzytelniania na platformie Azure][ssh-keys].

### <a name="create-a-service-principal"></a>Tworzenie nazwy głównej usługi

Jednostka usługi Azure Active Directory zezwala klastrowi usługi AKS na interakcje z innymi zasobami platformy Azure. Tworzenie jednostki usługi przy użyciu [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] polecenia. Parametr `--skip-assignment` umożliwia ograniczenie przypisywania dodatkowych uprawnień. Domyślnie ta jednostka usługi jest ważna przez rok.

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Dane wyjściowe są podobne do poniższego przykładu:

```json
{
  "appId": "8b1ede42-d407-46c2-a1bc-6b213b04295f",
  "displayName": "azure-cli-2019-04-19-21-42-11",
  "name": "http://azure-cli-2019-04-19-21-42-11",
  "password": "27e5ac58-81b0-46c1-bd87-85b4ef622682",
  "tenant": "73f978cf-87f2-41bf-92ab-2e7ce012db57"
}
```

Zwróć uwagę na wartości *appId* i *password*. Te wartości są używane w kolejnych krokach.

## <a name="create-an-aks-cluster"></a>Tworzenie klastra AKS

Do szablonu używanego w tym przewodniku Szybki Start jest [wdrażanie klastra usługi Azure Kubernetes Service](https://azure.microsoft.com/resources/templates/101-aks/). Aby uzyskać więcej przykładów AKS, zobacz [szablony szybkiego startu usługi AKS] [ aks-quickstart-templates] lokacji.

1. Wybierz poniższy obraz, aby zalogować się na platformie Azure i otworzyć szablon.

    [![Wdrażanie na platformie Azure](./media/kubernetes-walkthrough-rm-template/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-aks%2Fazuredeploy.json)

2. Wybierz lub wprowadź następujące wartości.  

    W tym przewodniku Szybki Start pozostaw wartości domyślne dla *GB rozmiar dysku systemu operacyjnego*, *liczba agentów*, *rozmiar maszyny Wirtualnej agenta*, *typ systemu operacyjnego*i *Wersję rozwiązania Kubernetes*. Należy podać własne wartości dla następujących parametrów szablonu:

    * **Subskrypcja**: Wybierz subskrypcję platformy Azure.
    * **Grupa zasobów**: Wybierz pozycję**Utwórz nowy**. Wprowadź unikatową nazwę grupy zasobów, takich jak *myResourceGroup*, następnie wybierz **OK**.
    * **Lokalizacja**: Wybierz lokalizację, taką jak **wschodnie stany USA**.
    * **Nazwa klastra**: Wprowadź unikatową nazwę klastra AKS, taką jak *myAKSCluster*.
    * **Prefiks DNS**: Wprowadź unikatowy prefiks DNS dla klastra, takie jak *myakscluster*.
    * **Nazwa użytkownika administratora systemu Linux**: Wprowadź nazwę użytkownika, aby nawiązać połączenie przy użyciu protokołu SSH, takich jak *azureuser*.
    * **SSH klucza publicznego RSA**: Skopiuj i Wklej *publicznych* częścią pary kluczy SSH (domyślnie zawartość *~/.ssh/id_rsa.pub*).
    * **Identyfikator klienta jednostki usługi**: Skopiuj i Wklej *appId* jednostki z usługi `az ad sp create-for-rbac` polecenia.
    * **Klucz tajny klienta jednostki usługi**: Skopiuj i Wklej *hasło* jednostki z usługi `az ad sp create-for-rbac` polecenia.
    * **Wyrażam zgodę na powyższe warunki i postanowienia**: Zaznacz to pole, aby zaakceptować.

    ![Szablon usługi Resource Manager, aby utworzyć klaster usługi Azure Kubernetes Service w portalu](./media/kubernetes-walkthrough-rm-template/create-aks-cluster-using-template-portal.png)

3. Wybierz pozycję **Kup**.

Trwa kilka minut, aby utworzyć klaster usługi AKS. Poczekaj, aby klaster mógł zostać pomyślnie wdrożona przed przejściem do następnego kroku.

## <a name="connect-to-the-cluster"></a>Łączenie z klastrem

Aby zarządzać klastrem Kubernetes, należy użyć klienta wiersza polecenia usługi Kubernetes — narzędzia [kubectl][kubectl]. Jeśli korzystasz z usługi Azure Cloud Shell, narzędzie `kubectl` jest już zainstalowane. Aby zainstalować narzędzie `kubectl` lokalnie, użyj polecenia [az aks install-cli][az-aks-install-cli]:

```azurecli
az aks install-cli
```

Aby skonfigurować narzędzie `kubectl` w celu nawiązania połączenia z klastrem Kubernetes, użyj polecenia [az aks get-credentials][az-aks-get-credentials]. To polecenie powoduje pobranie poświadczeń i skonfigurowanie interfejsu wiersza polecenia Kubernetes do ich użycia.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Aby zweryfikować połączenie z klastrem, użyj polecenia [kubectl get][kubectl-get] w celu zwrócenia listy węzłów klastra.

```azurecli-interactive
kubectl get nodes
```

Następujące przykładowe dane wyjściowe pokazuje węzły, utworzony w poprzednich krokach. Upewnij się, że jest w stanie dla wszystkich węzłów *gotowe*:

```
NAME                       STATUS   ROLES   AGE     VERSION
aks-agentpool-41324942-0   Ready    agent   6m44s   v1.12.6
aks-agentpool-41324942-1   Ready    agent   6m46s   v1.12.6
aks-agentpool-41324942-2   Ready    agent   6m45s   v1.12.6
```

## <a name="run-the-application"></a>Uruchamianie aplikacji

Plik manifestu platformy Kubernetes definiuje żądany stan klastra, w tym informacje o obrazach kontenera do uruchomienia. W tym przewodniku Szybki start manifest służy do tworzenia wszystkich obiektów potrzebnych do uruchomienia aplikacji Azure Vote. Ten manifest obejmuje dwa [wdrożenia platformy Kubernetes][kubernetes-deployment] — jedno dla aplikacji Azure Vote w języku Python i jedno dla wystąpienia usługi Redis. Tworzone są również dwie [usługi platformy Kubernetes][kubernetes-service] — wewnętrzna usługa dla wystąpienia usługi Redis i zewnętrzna usługa służąca do uzyskiwania dostępu do aplikacji Azure Vote z Internetu.

> [!TIP]
> W tym przewodniku Szybki start ręcznie utworzysz i wdrożysz manifesty aplikacji w klastrze AKS. W bardziej rzeczywistych scenariuszach można użyć obszarów [Azure Dev Spaces][azure-dev-spaces], aby szybko iterować i debugować kod bezpośrednio w klastrze AKS. Obszarów Dev Spaces można używać na różnych platformach systemów operacyjnych i w środowiskach deweloperskich, aby pracować razem z innymi członkami zespołu.

Utwórz plik o nazwie `azure-vote.yaml` i skopiuj go do poniższej definicji YAML. Jeśli używasz usługi Azure Cloud Shell, ten plik można utworzyć przy użyciu programu `vi` lub `nano`, tak jak podczas pracy w systemie wirtualnym lub fizycznym:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Wdróż aplikację przy użyciu polecenia [kubectl apply][kubectl-apply] i podaj nazwę manifestu YAML:

```azurecli-interactive
kubectl apply -f azure-vote.yaml
```

Następujące przykładowe dane wyjściowe przedstawiają pomyślnie utworzone wdrożenia i usługi:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Testowanie aplikacji

Podczas uruchamiania aplikacji usługa Kubernetes uwidacznia fronton aplikacji w Internecie. Ten proces może potrwać kilka minut.

Aby monitorować postęp, użyj polecenia [kubectl get-service][kubectl-get] z argumentem `--watch`.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Początkowo adres *EXTERNAL-IP* dla usługi *azure-vote-front* jest wyświetlany jako *oczekujący*.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Gdy dla adresu *EXTERNAL-IP* wartość *oczekujący* zmieni się na rzeczywisty publiczny adres IP, naciśnij klawisze `CTRL-C`, aby zatrzymać proces śledzenia narzędzia `kubectl`. Następujące przykładowe dane wyjściowe przedstawiają prawidłowy publiczny adres IP przypisany do usługi:

```
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Aby wyświetlić działającą aplikację Azure Vote, otwórz zewnętrzny adres IP usługi w przeglądarce internetowej.

![Obraz przedstawiający przechodzenie do aplikacji Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>Usuwanie klastra

Gdy klaster nie będzie już potrzebny, usuń grupę zasobów, usługę kontenera i wszystkie pokrewne zasoby za pomocą polecenia [az group delete][az-group-delete].

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Po usunięciu klastra jednostka usługi Azure Active Directory używana przez klaster usługi AKS nie jest usuwana. Aby sprawdzić, jak usunąć jednostkę usługi, zobacz [AKS service principal considerations and deletion (Uwagi dotyczące jednostki usługi AKS i jej usuwanie)][sp-delete].

## <a name="get-the-code"></a>Uzyskiwanie kodu

W tym przewodniku Szybki start utworzono wdrożenie platformy Kubernetes za pomocą utworzonych wcześniej obrazów kontenerów. Powiązany kod aplikacji, plik Dockerfile i plik manifestu rozwiązania Kubernetes są dostępne w serwisie GitHub.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wdrożono klaster Kubernetes oraz wdrożono w nim aplikację obsługującą wiele kontenerów. [Dostęp do pulpitu nawigacyjnego sieci web rozwiązania Kubernetes] [ kubernetes-dashboard] dla utworzonego klastra.

Aby dowiedzieć się więcej o usłudze AKS i poznać dokładnie proces od kompletnego kodu do wdrożenia, przejdź do samouczka dotyczącego klastra Kubernetes.

> [!div class="nextstepaction"]
> [Samouczek usługi AKS][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[azure-dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[aks-quickstart-templates]: https://azure.microsoft.com/resources/templates/?term=Azure+Kubernetes+Service

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
[kubernetes-dashboard]: kubernetes-dashboard.md
[ssh-keys]: ../virtual-machines/linux/create-ssh-keys-detailed.md
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
