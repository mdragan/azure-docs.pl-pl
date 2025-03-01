---
title: Przewodnik Szybki start — Tworzenie klastra usługi Azure Kubernetes Service (AKS) w portalu
description: Dowiedz się, jak szybko utworzyć klaster Kubernetes, wdrożyć aplikację i monitorować wydajność w usłudze Azure Kubernetes Service (AKS) przy użyciu witryny Azure Portal.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: quickstart
ms.date: 5/31/2019
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 67ea040294e669859881dfd90533eba942105dec
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303356"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-using-the-azure-portal"></a>Szybki start: wdrażanie klastra usługi Azure Kubernetes Service (AKS) przy użyciu witryny Azure Portal

Azure Kubernetes Service (AKS) to zarządzana usługa platformy Kubernetes, która umożliwia szybkie wdrażanie klastrów i zarządzanie nimi. W tym przewodniku Szybki start wdrażany jest klaster AKS przy użyciu witryny Azure Portal. W klastrze jest uruchamiana aplikacja obsługująca wiele kontenerów, która składa się z frontonu internetowego i wystąpienia pamięci podręcznej Redis. Następnie zobaczysz, jak monitorować kondycję klastra i zasobników, w których działa Twoja aplikacja.

![Obraz przedstawiający przechodzenie do przykładowej aplikacji do głosowania na platformie Azure](media/container-service-kubernetes-walkthrough/azure-vote.png)

W tym przewodniku Szybki start założono, że masz podstawową wiedzę na temat pojęć związanych z rozwiązaniem Kubernetes. Aby uzyskać więcej informacji, zobacz [Kubernetes podstawowe pojęcia dotyczące usługi Azure Kubernetes Service (AKS)][kubernetes-concepts].

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się do witryny Azure Portal pod adresem https://portal.azure.com.

## <a name="create-an-aks-cluster"></a>Tworzenie klastra AKS

W lewym górnym rogu witryny Azure portal wybierz **+ Utwórz zasób** > **kontenery** >  **usługi Kubernetes**.

Aby utworzyć klaster usługi AKS, wykonaj następujące czynności:

1. Na **podstawy** strony, skonfiguruj następujące opcje:
   - *SZCZEGÓŁY PROJEKTU*: Wybierz subskrypcję platformy Azure, a następnie wybierz lub utwórz grupę zasobów platformy Azure, taką jak *myResourceGroup*. Wprowadź **nazwę klastra Kubernetes**, taką jak *myAKSCluster*.
   - *SZCZEGÓŁY KLASTRA*: Wybierz region, wersję platformy Kubernetes i prefiks nazwy DNS dla klastra usługi AKS.
   - **PODSTAWOWYM PULI węzeł**: Wybierz rozmiar maszyny Wirtualnej dla węzłów AKS. Rozmiar maszyny wirtualnej **nie może** zostać zmieniony po wdrożeniu klastra AKS. 
       - Wybierz liczbę węzłów do wdrożenia w klastrze. Na potrzeby tego przewodnika Szybki start ustaw pozycję **Liczba węzłów** na *1*. Liczbę węzłów **można** dostosować po wdrożeniu klastra.
    
     ![Tworzenie klastra AKS — podaj podstawowe informacje](media/kubernetes-walkthrough-portal/create-cluster-basics.png)

     Wybierz opcję **Dalej: Skala** po zakończeniu.

2. Na **skalowania** Zachowaj wartości domyślne. W dolnej części ekranu kliknij **dalej: uwierzytelnianie**.
3. Na **uwierzytelniania** strony, skonfiguruj następujące opcje:
   - Utwórz nową jednostkę usługi, pozostawiając **nazwy głównej usługi** pole **(nowy) domyślna jednostka usługi**. Lub możesz wybrać *nazwy głównej usługi Konfiguruj* można użyć istniejącego. Jeśli używasz istniejącej, konieczne będzie dostarczenie klientowi nazwę SPN, identyfikator i klucz tajny.
   - Włącz opcję dla funkcji kontroli dostępu opartej na rolach (RBAC) w rozwiązaniu Kubernetes. Zapewni to bardziej precyzyjną kontrolę nad dostępem do zasobów platformy Kubernetes, wdrożonych w klastrze AKS.

    Domyślnie używana jest sieć *Podstawowa* i włączona jest usługa Azure Monitor dla kontenerów. Kliknij przycisk **przeglądu + Utwórz** i następnie **Utwórz** po zakończeniu weryfikacji.

Trwa kilka minut, aby utworzyć klaster usługi AKS. Po zakończeniu wdrożenia kliknij przycisk **przejdź do zasobu**, lub przejdź do grupy zasobów klastra AKS, takich jak *myResourceGroup*i wybierz zasób usługi AKS, taką jak *myAKSCluster*. Pulpit nawigacyjny klastra usługi AKS pokazano, jak w poniższym przykładzie:

![Przykładowy pulpit nawigacyjny usługi AKS w witrynie Azure Portal](media/kubernetes-walkthrough-portal/aks-portal-dashboard.png)

## <a name="connect-to-the-cluster"></a>Łączenie z klastrem

Aby zarządzać klastrem Kubernetes, należy użyć [kubectl][kubectl], czyli klienta wiersza polecenia usługi Kubernetes. Klient `kubectl` jest instalowany wstępnie wraz z usługą Azure Cloud Shell.

Otwórz za pomocą usługi Cloud Shell `>_` przycisku w górnej części witryny Azure portal.

![Otwieranie usługi Azure Cloud Shell w portalu](media/kubernetes-walkthrough-portal/aks-cloud-shell.png)

Aby skonfigurować narzędzie `kubectl` w celu nawiązania połączenia z klastrem Kubernetes, użyj polecenia [az aks get-credentials][az-aks-get-credentials]. To polecenie powoduje pobranie poświadczeń i zastosowanie ich w konfiguracji interfejsu wiersza polecenia Kubernetes. Poniższy przykład umożliwia pobranie poświadczeń dla nazwy klastra *myAKSCluster* w grupie zasobów *myResourceGroup*:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Aby sprawdzić połączenie z klastrem, użyj polecenia [kubectl get][kubectl-get], aby powrócić do listy węzłów klastra.

```azurecli-interactive
kubectl get nodes
```

Poniższe przykładowe dane wyjściowe zawierają jeden węzeł utworzony w poprzednich krokach. Upewnij się, że stan węzła to *Gotowy*:

```
NAME                       STATUS    ROLES     AGE       VERSION
aks-agentpool-14693408-0   Ready     agent     15m       v1.11.5
```

## <a name="run-the-application"></a>Uruchamianie aplikacji

Plik manifestu platformy Kubernetes definiuje żądany stan klastra, w tym informacje o obrazach kontenera do uruchomienia. W tym przewodniku Szybki start manifest służy do tworzenia wszystkich obiektów potrzebnych do uruchomienia aplikacji Azure Vote. Ten manifest zawiera dwie [wdrożenia rozwiązania Kubernetes][kubernetes-deployment] - one for the sample Azure Vote Python applications, and the other for a Redis instance. Two [Kubernetes Services][kubernetes-service] są również tworzone — wewnętrzna usługa dla wystąpienia usługi Redis i zewnętrzna usługa dostępu do aplikacji Azure Vote z Internetu.

> [!TIP]
> W tym przewodniku Szybki start ręcznie utworzysz i wdrożysz manifesty aplikacji w klastrze AKS. W rzeczywistych scenariuszy, można użyć [miejsca do magazynowania Azure Dev][azure-dev-spaces] szybko iterować i debugowania kodu bezpośrednio w klastrze AKS. Obszarów Dev Spaces można używać na różnych platformach systemów operacyjnych i w środowiskach deweloperskich, aby pracować razem z innymi członkami zespołu.

W usłudze cloud shell za pomocą `nano` lub `vi` do utworzenia pliku o nazwie `azure-vote.yaml` i skopiuj poniższą definicję kodu YAML:

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

Wdrażanie aplikacji przy użyciu [zastosować kubectl][kubectl-apply] polecenia i podaj nazwę manifeście YAML:

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

![Obraz przedstawiający przechodzenie do przykładowej aplikacji do głosowania na platformie Azure](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="monitor-health-and-logs"></a>Monitorowanie kondycji i dzienników

Podczas tworzenia klastra włączono usługę Azure Monitor dla kontenerów. Ta funkcja monitorowania udostępnia metryki dotyczące kondycji zasobników i klastra usługi AKS działających w klastrze.

Zebranie tych danych w witrynie Azure Portal może zająć kilka minut. Aby wyświetlić bieżący stan, czas pracy i użycie zasobów na potrzeby zasobników aplikacji Azure Vote, wróć do zasobu usługi AKS, takiego jak *myAKSCluster*, w witrynie Azure Portal. Możesz uzyskać dostęp do stanu kondycji w następujący sposób:

1. W obszarze **Monitorowanie** po lewej stronie wybierz pozycję **Insights**
1. U góry wybierz pozycję **+ Dodaj filtr**
1. Wybierz wartość *Namespace* (Przestrzeń nazw) jako właściwość, a następnie wybierz pozycję *\<All but kube-system\>* (Wszystko oprócz kube-system)
1. Wybierz pozycję **Kontenery**, aby ją wyświetlić.

Zostaną wyświetlone kontenery *azure-vote-back* i *azure-vote-front*, jak pokazano w poniższym przykładzie:

![Wyświetlanie kondycji uruchomionych kontenerów w usłudze AKS](media/kubernetes-walkthrough-portal/monitor-containers.png)

Aby wyświetlić dzienniki dla zasobnika `azure-vote-front`, wybierz link **Wyświetl dzienniki kontenerów** po prawej stronie listy kontenerów. Dzienniki te obejmują strumienie *stdout* i *stderr* z kontenera.

![Wyświetlanie dzienników kontenerów w usłudze AKS](media/kubernetes-walkthrough-portal/monitor-container-logs.png)

## <a name="delete-cluster"></a>Usuwanie klastra

Gdy klaster nie będzie już potrzebny, usuń zasób klastra, co spowoduje usunięcie wszystkich skojarzonych zasobów. Tę operację można wykonać w witrynie Azure Portal, wybierając przycisk **Usuń** na pulpicie nawigacyjnym klastra usługi AKS. Alternatywnie [Usuń az aks][az-aks-delete] polecenia mogą być używane w usłudze Cloud Shell:

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster --no-wait
```

> [!NOTE]
> Po usunięciu klastra jednostka usługi Azure Active Directory używana przez klaster usługi AKS nie jest usuwana. Aby uzyskać instrukcje dotyczące sposobu usuwania nazwy głównej usługi, zobacz [uwagi podmiotu zabezpieczeń i usuwania usługi AKS][sp-delete].

## <a name="get-the-code"></a>Uzyskiwanie kodu

W tym przewodniku Szybki start utworzono wdrożenie platformy Kubernetes za pomocą utworzonych wcześniej obrazów kontenerów. Powiązany kod aplikacji, plik Dockerfile i plik manifestu rozwiązania Kubernetes są dostępne w serwisie GitHub.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wdrożono klaster Kubernetes oraz wdrożono w nim aplikację obsługującą wiele kontenerów.

Aby dowiedzieć się więcej o usłudze AKS i poznać dokładnie proces od kompletnego kodu do wdrożenia, przejdź do samouczka dotyczącego klastra Kubernetes.

> [!div class="nextstepaction"]
> [Samouczek dotyczący usługi AKS][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-delete]: /cli/azure/aks#az-aks-delete
[aks-monitor]: ../monitoring/monitoring-container-health.md
[aks-network]: ./concepts-network.md
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[http-routing]: ./http-application-routing.md
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services