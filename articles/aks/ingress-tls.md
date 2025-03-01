---
title: Tworzenie ruchu przychodzącego protokołu HTTPS za pomocą klastra Azure Kubernetes Service (AKS)
description: Dowiedz się, jak zainstalować i skonfigurować kontroler ruch przychodzący serwera NGINX, który używa teraz szyfrowania do automatycznego generowania certyfikatu TLS w klastrze usługi Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/24/2019
ms.author: iainfou
ms.openlocfilehash: c858d1ac56da5f04346b3cd84402d4eeeb7fd975
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66430978"
---
# <a name="create-an-https-ingress-controller-on-azure-kubernetes-service-aks"></a>Tworzenie kontrolera danych przychodzących HTTPS w usłudze Azure Kubernetes Service (AKS)

Kontroler ruchu przychodzącego to element oprogramowania dostarczający odwrotny serwer proxy, konfigurowalne trasowanie ruchu oraz zakończenie protokołu TLS dla usług Kubernetes. Zasoby ruchu przychodzącego usług Kubernetes są używane do skonfigurowania zasad ruchu przychodzącego oraz tras dla poszczególnych usług Kubernetes. Dzięki korzystaniu z kontrolera ruchu przychodzącego oraz zasad ruchu przychodzącego można użyć jednego adresu IP do trasowania ruchu w wielu usługach w klastrze Kubernetes.

W tym artykule przedstawiono sposób wdrażania [kontrolera danych przychodzących NGINX] [ nginx-ingress] w klastrze usługi Azure Kubernetes Service (AKS). [Menedżera certyfikatów] [ cert-manager] projekt jest używana do automatycznego generowania i skonfigurować [umożliwia szyfrowanie] [ lets-encrypt] certyfikatów. Na koniec dwie aplikacje są uruchamiane w klastrze AKS, z których każdy jest dostępny za pośrednictwem pojedynczego adresu IP.

Możesz również wykonać następujące czynności:

- [Tworzenie kontrolera podstawowego transferu danych przychodzących za pomocą połączenia z siecią zewnętrzną][aks-ingress-basic]
- [Włączyć dodatek routing aplikacji protokołu HTTP][aks-http-app-routing]
- [Tworzenie kontrolera danych przychodzących, korzystającą z sieci prywatne, wewnętrzne i adres IP][aks-ingress-internal]
- [Tworzenie kontrolera transferu danych przychodzących, która korzysta z certyfikatów protokołu TLS][aks-ingress-own-tls]
- [Tworzenie kontrolera danych przychodzących, używającej umożliwia szyfrowanie można automatycznie wygenerować certyfikaty protokołu TLS z statyczny publiczny adres IP][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Przed rozpoczęciem

W tym artykule założono, że masz istniejący klaster usługi AKS. Jeśli potrzebujesz klastra AKS, zobacz Przewodnik Szybki Start usługi AKS [przy użyciu wiersza polecenia platformy Azure] [ aks-quickstart-cli] lub [przy użyciu witryny Azure portal][aks-quickstart-portal].

W tym artykule używa narzędzia Helm do zainstalowania serwera NGINX kontrolera danych przychodzących, Menedżer certyfikatów i przykładową aplikację sieci web. Musisz mieć narzędzia Helm inicjowane w obrębie klastra usługi AKS i przy użyciu konta usługi dla Tiller. Upewnij się, że używasz najnowszej wersji narzędzia Helm. Aby uzyskać instrukcje uaktualniania, zobacz [Helm zainstalować docs][helm-install]. Aby uzyskać więcej informacji na temat konfigurowania i używania narzędzia Helm, zobacz [instalowanie aplikacji za pomocą narzędzia Helm w usłudze Azure Kubernetes Service (AKS)][use-helm].

W tym artykule wymaga również, czy korzystasz z wiersza polecenia platformy Azure w wersji 2.0.64 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Tworzenie kontrolera danych przychodzących

Aby utworzyć kontroler danych przychodzących, użyj `Helm` zainstalował *ruch przychodzący serwera nginx*. Dodano nadmiarowość dwóch replik kontrolerów ruch przychodzący serwera NGINX są wdrażane przy użyciu `--set controller.replicaCount` parametru. Aby w pełni korzystać z systemem replik kontrolera danych przychodzących, upewnij się, że istnieje więcej niż jeden węzeł w klastrze AKS.

Kontroler danych przychodzących musi odbywać się w węźle systemu Linux. Węzły systemu Windows Server (obecnie dostępna w wersji zapoznawczej w usłudze AKS) nie należy uruchamiać kontrolera danych przychodzących. Selektor węzła jest określony, przy użyciu `--set nodeSelector` parametru, aby poinformować harmonogram Kubernetes, aby uruchomić kontroler danych przychodzących NGINX w węźle opartych na systemie Linux.

> [!TIP]
> Poniższy przykład obejmuje tworzenie przestrzeni nazw Kubernetes, transferu danych przychodzących zasobów o nazwie *basic ruch przychodzący*. Określ obszar nazw dla Twojego środowiska, zgodnie z potrzebami. Jeśli klaster AKS nie jest włączone RBAC, Dodaj `--set rbac.create=false` polecenia narzędzia Helm.

> [!TIP]
> Jeśli chcesz umożliwić [klienta źródłowego adresu IP zachowania] [ client-source-ip] dla żądań kierowanych do kontenerów w klastrze, należy dodać `--set controller.service.externalTrafficPolicy=Local` do narzędzia Helm polecenie instalacji. Źródło klienta IP znajduje się w nagłówku żądania, w obszarze *X-Forwarded-dla*. Jeśli kontroler danych przychodzących z klienta zachowania IP dla źródła włączona, przekazywanego SSL nie będzie działać.

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Use Helm to deploy an NGINX ingress controller
helm install stable/nginx-ingress \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Podczas instalacji Azure publiczny adres IP jest tworzona dla kontrolera danych przychodzących. Ten publiczny adres IP to statyczny dla żywotności kontrolera danych przychodzących. Jeśli usuniesz kontrolera danych przychodzących, przypisanie publicznego adresu IP zostanie utracony. Jeśli następnie utworzysz kontroler dodatkowy ruch przychodzący, nowy publiczny adres IP zostanie przypisany. Jeśli chcesz zachować użycie publicznego adresu IP, możesz zamiast tego [utworzyć kontroler danych przychodzących z statyczny publiczny adres IP][aks-ingress-static-tls].

Aby uzyskać publiczny adres IP, użyj `kubectl get service` polecenia. Trwa kilka minut, aż adres IP, który można przypisać do usługi.

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
billowing-kitten-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
billowing-kitten-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Utworzono jeszcze żadnych reguł ruchu przychodzącego. Po przejściu do publicznego adresu IP, zostanie wyświetlona strona 404 domyślna serwera NGINX kontrolera danych przychodzących.

## <a name="configure-a-dns-name"></a>Konfigurowanie nazwy DNS

W przypadku certyfikaty HTTPS działała prawidłowo należy skonfigurować nazwy FQDN dla adresu IP kontrolera danych przychodzących. Zaktualizuj poniższy skrypt adres IP kontrolera danych przychodzących i unikatową nazwę, której chcesz użyć dla nazwy FQDN:

```azurecli-interactive
#!/bin/bash

# Public IP address of your ingress controller
IP="51.145.155.210"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with DNS name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

Kontroler danych przychodzących jest teraz dostępna za pośrednictwem nazwy FQDN.

## <a name="install-cert-manager"></a>Zainstaluj Menedżera certyfikatów

Kontroler danych przychodzących NGINX obsługuje kończenie żądań protokołu TLS. Istnieje kilka sposobów, aby pobrać i skonfigurować certyfikaty dla protokołu HTTPS. W tym artykule pokazano sposób użycia [Menedżera certyfikatów][cert-manager], która umożliwia automatyczne [umożliwia szyfrowanie] [ lets-encrypt] Generowanie certyfikatu i Funkcje zarządzania.

> [!NOTE]
> W tym artykule wykorzystano `staging` środowiska umożliwia szyfrowanie. W przypadku wdrożeń produkcyjnych, użyj `letsencrypt-prod` i `https://acme-v02.api.letsencrypt.org/directory` w definicjach zasobów, a podczas instalowania wykresu Helm.

Aby zainstalować kontroler Menedżera certyfikatów w klastrze z włączoną funkcją RBAC, należy użyć następującego `helm install` polecenia:

```console
# Install the CustomResourceDefinition resources separately
kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.8/deploy/manifests/00-crds.yaml

# Create the namespace for cert-manager
kubectl create namespace cert-manager

# Label the cert-manager namespace to disable resource validation
kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=true

# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io

# Update your local Helm chart repository cache
helm repo update

# Install the cert-manager Helm chart
helm install \
  --name cert-manager \
  --namespace cert-manager \
  --version v0.8.0 \
  jetstack/cert-manager
```

Aby uzyskać więcej informacji na temat konfiguracji Menedżera certyfikatów, zobacz [projektu Menedżera certyfikatów][cert-manager].

## <a name="create-a-ca-cluster-issuer"></a>Utwórz wystawcę klastra urzędu certyfikacji

Zanim certyfikaty mogą być wystawiane, Menedżer certyfikatów wymaga [wystawcy] [ cert-manager-issuer] lub [ClusterIssuer] [ cert-manager-cluster-issuer] zasobów. Te zasoby platformy Kubernetes są identyczne w działaniu, jednak `Issuer` działa w jednej przestrzeni nazw i `ClusterIssuer` działa we wszystkich przestrzeni nazw. Aby uzyskać więcej informacji, zobacz [wystawcy certyfikatu Menedżera] [ cert-manager-issuer] dokumentacji.

Tworzenie klastra wystawcy, takich jak `cluster-issuer.yaml`, za pomocą następujących manifest przykładu. Przy użyciu prawidłowego adresu z Twojej organizacji, należy zaktualizować adres e-mail:

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
  namespace: ingress-basic
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-staging
    http01: {}
```

Aby utworzyć wystawcy, użyj `kubectl apply -f cluster-issuer.yaml` polecenia.

```
$ kubectl apply -f cluster-issuer.yaml

clusterissuer.certmanager.k8s.io/letsencrypt-staging created
```

## <a name="run-demo-applications"></a>Uruchamianie aplikacji demonstracyjnych

Skonfigurowano kontroler danych przychodzących i rozwiązania do zarządzania certyfikatu. Teraz sklonujemy wykonywania dwóch pokaz aplikacji w klastrze AKS. W tym przykładzie Helm służy do wdrażania dwa wystąpienia aplikacji proste "Hello world".

Przed zainstalowaniem wykresów rozwiązania Helm przykładowe Dodaj repozytorium przykładów dla platformy Azure w środowisku Helm w następujący sposób:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Tworzenie pierwszej aplikacji demonstracyjnej z planu narzędzia Helm, za pomocą następującego polecenia:

```console
helm install azure-samples/aks-helloworld --namespace ingress-basic
```

Teraz zainstalować drugie wystąpienie aplikacji pokazowej. Dla drugiego wystąpienia należy określić nowy tytuł, aby wizualnie różniące się od dwóch aplikacji. Możesz również określić unikatową nazwę usługi:

```console
helm install azure-samples/aks-helloworld \
    --namespace ingress-basic \
    --set title="AKS Ingress Demo" \
    --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Utwórz trasę w protokole transferu danych przychodzących

Obie aplikacje są uruchomione w klastrze Kubernetes, ale są one konfigurowane przy użyciu usługi typu `ClusterIP`. Jako takie aplikacje nie są dostępne z Internetu. Aby udostępnić je publicznie, utwórz zasób transferu danych przychodzących rozwiązania Kubernetes. Zasób ruch przychodzący konfiguruje zasady, które kierują ruch do jednego z dwóch aplikacji.

W poniższym przykładzie ruch do adresu `https://demo-aks-ingress.eastus.cloudapp.azure.com/` jest kierowany do usługi o nazwie `aks-helloworld`. Ruch do adresu `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` jest kierowany do `ingress-demo` usługi. Aktualizacja *hosty* i *hosta* na nazwę DNS utworzonego w poprzednim kroku.

Utwórz plik o nazwie `hello-world-ingress.yaml` i skopiuj poniższy przykład kodu YAML.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  namespace: ingress-basic
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-staging
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /(.*)
      - backend:
          serviceName: ingress-demo
          servicePort: 80
        path: /hello-world-two(/|$)(.*)
```

Tworzenie przy użyciu zasobów ruch przychodzący `kubectl apply -f hello-world-ingress.yaml` polecenia.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="create-a-certificate-object"></a>Utwórz obiekt certyfikatu

Następnie można utworzyć zasobu certyfikatu. Zasób certyfikatu definiuje żądany certyfikat X.509. Aby uzyskać więcej informacji, zobacz [certyfikaty Menedżera certyfikatów][cert-manager-certificates].

Menedżer certyfikatów prawdopodobnie automatycznie utworzył obiekt certyfikatu przy użyciu transferu danych przychodzących — podkładki, który jest automatycznie wdrażane za pomocą Menedżera certyfikatów od v0.2.2. Aby uzyskać więcej informacji, zobacz [dokumentacji podkładki ruch przychodzący][ingress-shim].

Aby sprawdzić, czy certyfikat został pomyślnie utworzony, użyj `kubectl describe certificate tls-secret --namespace ingress-basic` polecenia.

Jeśli certyfikat został wystawiony, zobaczysz dane wyjściowe podobne do następujących:
```
Type    Reason          Age   From          Message
----    ------          ----  ----          -------
  Normal  CreateOrder     11m   cert-manager  Created new ACME order, attempting validation...
  Normal  DomainVerified  10m   cert-manager  Domain "demo-aks-ingress.eastus.cloudapp.azure.com" verified with "http-01" validation
  Normal  IssueCert       10m   cert-manager  Issuing certificate...
  Normal  CertObtained    10m   cert-manager  Obtained certificate from ACME server
  Normal  CertIssued      10m   cert-manager  Certificate issued successfully
```

Jeśli musisz utworzyć zasób dodatkowy certyfikat, możesz to zrobić z manifestem poniższy przykład. Aktualizacja *dnsNames* i *domen* na nazwę DNS utworzonego w poprzednim kroku. Jeśli używasz kontrolera danych przychodzących wyłącznie wewnętrznym, należy określić wewnętrzna nazwa DNS usługi.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: tls-secret
  namespace: ingress-basic
spec:
  secretName: tls-secret-staging
  dnsNames:
  - demo-aks-ingress.eastus.cloudapp.azure.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - demo-aks-ingress.eastus.cloudapp.azure.com
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
```

Aby utworzyć zasób certyfikatu, użyj `kubectl apply -f certificates.yaml` polecenia.

```
$ kubectl apply -f certificates.yaml

certificate.certmanager.k8s.io/tls-secret-staging created
```

## <a name="test-the-ingress-configuration"></a>Testowanie konfiguracji transferu danych przychodzących

Otwórz przeglądarkę internetową nazwę FQDN kontrolera danych przychodzących rozwiązania Kubernetes, takich jak *https://demo-aks-ingress.eastus.cloudapp.azure.com* .

Do używania w tych przykładach `letsencrypt-staging`, wystawionego certyfikatu SSL nie jest zaufany przez przeglądarkę. Zaakceptuj monit ostrzegawczy, aby przejść do aplikacji. Przedstawia informacje o certyfikacie, to *sfałszowana LE pośrednich X1* certyfikat wystawiony przez umożliwia szyfrowanie. Ten certyfikat fałszywych wskazuje `cert-manager` poprawnie przetwarzał żądanie i otrzymała certyfikat od dostawcy:

![Umożliwia szyfrowanie przemieszczania certyfikatu](media/ingress/staging-certificate.png)

Po zmianie umożliwia szyfrowanie do użycia `prod` zamiast `staging`, zaufany certyfikat wystawiony przez umożliwia szyfrowanie jest używany, jak pokazano w poniższym przykładzie:

![Umożliwia szyfrowanie certyfikatu](media/ingress/certificate.png)

Aplikacji demonstracyjnej jest wyświetlany w przeglądarce sieci web:

![Przykładowa aplikacja, jeden](media/ingress/app-one.png)

Teraz Dodaj */hello-world-two* ścieżkę do pełni kwalifikowaną nazwę domeny, takich jak *https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two* . Pokazano drugiej aplikacji demonstracyjnej z tytułem niestandardowe:

![Przykładowa aplikacja dwóch](media/ingress/app-two.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

W tym artykule używane narzędzia Helm do zainstalowania składników transferu danych przychodzących, certyfikaty i przykładowe aplikacje. Podczas wdrażania wykresu Helm tworzonych wiele zasobów Kubernetes. Te zasoby obejmują zasobników, wdrożenia i usług. Aby wyczyścić te zasoby, albo można usunąć przestrzeni nazw cały przykładowy lub poszczególnych zasobów.

### <a name="delete-the-sample-namespace-and-all-resources"></a>Usuń przestrzeń nazw przykładowy i wszystkie zasoby

Aby usunąć cały przykładowy przestrzeni nazw, należy użyć `kubectl delete` polecenia i podaj nazwę swojej przestrzeni nazw. Zostaną usunięte wszystkie zasoby w przestrzeni nazw.

```console
kubectl delete namespace ingress-basic
```

Następnie usuń repozytorium narzędzia Helm dla usługi AKS hello world aplikacji:

```console
helm repo remove azure-samples
```

### <a name="delete-resources-individually"></a>Aby usunąć zasoby pojedynczo

Alternatywnie bardziej szczegółowego podejścia polega na usunięciu poszczególnych zasobów, które są tworzone. Najpierw należy usunąć zasoby certyfikatu:

```console
kubectl delete -f certificates.yaml
kubectl delete -f cluster-issuer.yaml
```

Teraz wyświetlić listę wersji narzędzia Helm przy użyciu `helm list` polecenia. Wyszukaj wykresy o nazwie *ruch przychodzący serwera nginx*, *Menedżera certyfikatów*, i *aks-helloworld*, jak pokazano w następujących przykładowych danych wyjściowych:

```
$ helm list

NAME                    REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
billowing-kitten        1           Wed Mar  6 19:37:43 2019    DEPLOYED    nginx-ingress-1.3.1     0.22.0      kube-system
loitering-waterbuffalo  1           Wed Mar  6 20:25:01 2019    DEPLOYED    cert-manager-v0.6.6     v0.6.2      kube-system
flabby-deer             1           Wed Mar  6 20:27:54 2019    DEPLOYED    aks-helloworld-0.1.0                default
linting-echidna         1           Wed Mar  6 20:27:59 2019    DEPLOYED    aks-helloworld-0.1.0                default
```

Usuń wersjach z `helm delete` polecenia. Poniższy przykład usuwa wdrożenia ruch przychodzący serwera NGINX, Menedżer certyfikatów i dwie przykładowe AKS Witaj świecie aplikacje.

```
$ helm delete billowing-kitten loitering-waterbuffalo flabby-deer linting-echidna

release "billowing-kitten" deleted
release "loitering-waterbuffalo" deleted
release "flabby-deer" deleted
release "linting-echidna" deleted
```

Następnie usuń repozytorium narzędzia Helm dla usługi AKS hello world aplikacji:

```console
helm repo remove azure-samples
```

Usunięcie samej przestrzeni nazw. Użyj `kubectl delete` polecenia i podaj nazwę swojej przestrzeni nazw:

```console
kubectl delete namespace ingress-basic
```

Na koniec usunąć trasę ruchu przychodzącego, który kierowany ruch do aplikacji przykładowej:

```console
kubectl delete -f hello-world-ingress.yaml
```

## <a name="next-steps"></a>Kolejne kroki

W tym artykule uwzględnione niektóre składniki zewnętrzne w usłudze AKS. Aby dowiedzieć się więcej na temat tych składników, zobacz następujące strony projektu:

- [Interfejs wiersza polecenia narzędzia Helm][helm-cli]
- [Kontroler danych przychodzących serwera NGINX][nginx-ingress]
- [cert-manager][cert-manager]

Możesz również wykonać następujące czynności:

- [Tworzenie kontrolera podstawowego transferu danych przychodzących za pomocą połączenia z siecią zewnętrzną][aks-ingress-basic]
- [Włączyć dodatek routing aplikacji protokołu HTTP][aks-http-app-routing]
- [Tworzenie kontrolera danych przychodzących, korzystającą z sieci prywatne, wewnętrzne i adres IP][aks-ingress-internal]
- [Tworzenie kontrolera transferu danych przychodzących, która korzysta z certyfikatów protokołu TLS][aks-ingress-own-tls]
- [Tworzenie kontrolera danych przychodzących, używającej umożliwia szyfrowanie można automatycznie wygenerować certyfikaty protokołu TLS z statyczny publiczny adres IP][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[ingress-shim]: https://docs.cert-manager.io/en/latest/tasks/issuing-certificates/ingress-shim.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[client-source-ip]: concepts-network.md#ingress-controllers
[install-azure-cli]: /cli/azure/install-azure-cli