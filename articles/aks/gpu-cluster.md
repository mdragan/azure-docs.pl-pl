---
title: Użyj procesorach GPU znajdujących się w usłudze Azure Kubernetes Service (AKS)
description: Dowiedz się, jak używać procesorów GPU na potrzeby obliczenia o wysokiej wydajności lub obciążeń intensywnie korzystających z grafiki na platformie Azure Kubernetes Service (AKS)
services: container-service
author: zr-msft
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/16/2019
ms.author: zarhoads
ms.openlocfilehash: c92762b53b0f5b50ea08f2f78998a3ccecbed990
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67061056"
---
# <a name="use-gpus-for-compute-intensive-workloads-on-azure-kubernetes-service-aks"></a>Użycie procesorów GPU w przypadku obciążeń intensywnie korzystających z obliczeń w usłudze Azure Kubernetes Service (AKS)

Graficzny graficznych (GPU) są często używane dla obciążeń intensywnie korzystających z obliczeń, takich jak obciążenia grafiki i wizualizacji. AKS obsługuje tworzenie pul węzłów z włączonymi procesorami GPU do uruchamiania tych obciążeń intensywnie korzystających z obliczeń w usłudze Kubernetes. Aby uzyskać więcej informacji na temat maszyny wirtualne obsługujące procesor GPU dostępności, zobacz [rozmiarów maszyn wirtualnych na platformie Azure zoptymalizowane pod kątem procesora GPU][gpu-skus]. W przypadku węzłów AKS zalecamy minimalny rozmiar *maszyna wirtualna Standard_NC6*.

> [!NOTE]
> Maszyny wirtualne z włączonymi procesorami GPU zawierają wyspecjalizowanym sprzęcie, który podlega wyższą dostępność ceny i regionu. Aby uzyskać więcej informacji, zobacz [ceny] [ azure-pricing] narzędzia i [dostępność w poszczególnych regionach][azure-availability].

Obecnie używane są pule węzłów z włączonymi procesorami GPU jest dostępna tylko dla pule węzłów systemu Linux.

## <a name="before-you-begin"></a>Przed rozpoczęciem

W tym artykule założono, że masz istniejący klaster AKS z węzłami, które obsługują procesorów GPU. Klastra usługi AKS musi działać Kubernetes, 1,10 lub nowszej. Jeśli potrzebujesz klastra AKS, która spełnia te wymagania, zobacz ten artykuł, aby w pierwszej sekcji [Tworzenie klastra AKS](#create-an-aks-cluster).

Możesz również muszą wiersza polecenia platformy Azure w wersji 2.0.64 lub później zainstalowane i skonfigurowane. Uruchom polecenie  `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie przeprowadzenie instalacji lub uaktualnienia, zobacz  [Instalowanie interfejsu wiersza polecenia platformy Azure][install-azure-cli].

## <a name="create-an-aks-cluster"></a>Tworzenie klastra AKS

Jeśli potrzebujesz klastra AKS, który spełnia minimalne wymagania (węzeł z włączonymi procesorami GPU i Kubernetes wersji 1,10 lub nowszej), wykonaj następujące czynności. Jeśli masz już klaster AKS, która spełnia te wymagania [od razu przejść do następnej sekcji](#confirm-that-gpus-are-schedulable).

Najpierw utwórz grupę zasobów dla klastra przy użyciu [Tworzenie grupy az] [ az-group-create] polecenia. Poniższy przykład tworzy nazwę grupy zasobów *myResourceGroup* w *eastus* regionu:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Teraz Utwórz klaster usługi AKS przy użyciu [tworzenie az aks] [ az-aks-create] polecenia. Poniższy przykład tworzy klaster z jednym węzłem o rozmiarze `Standard_NC6`:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-vm-size Standard_NC6 \
    --node-count 1
```

Uzyskiwanie poświadczeń dla klastra usługi AKS przy użyciu [az aks get-credentials] [ az-aks-get-credentials] polecenia:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="install-nvidia-drivers"></a>Zainstaluj sterowniki nVidia

Przed użyciem procesorów GPU w węzłach, należy wdrożyć DaemonSet dla wtyczki urządzenia firmy NVIDIA. Ta DaemonSet uruchamia zasobnik w każdym węźle, aby zapewnić wymagane sterowniki dla procesorów GPU.

Najpierw należy utworzyć za pomocą nazw [kubectl tworzenie przestrzeni nazw] [ kubectl-create] polecenia, takie jak *zasoby procesora gpu*:

```console
kubectl create namespace gpu-resources
```

Utwórz plik o nazwie *nvidia — urządzenie wtyczka ds.yaml* i wklej następujące manifest YAML. Tego manifestu jest dostarczana jako część [NVIDIA urządzenia wtyczki dla projektu Kubernetes][nvidia-github].

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: nvidia-device-plugin-daemonset
  namespace: kube-system
spec:
  updateStrategy:
    type: RollingUpdate
  template:
    metadata:
      # Mark this pod as a critical add-on; when enabled, the critical add-on scheduler
      # reserves resources for critical add-on pods so that they can be rescheduled after
      # a failure.  This annotation works in tandem with the toleration below.
      annotations:
        scheduler.alpha.kubernetes.io/critical-pod: ""
      labels:
        name: nvidia-device-plugin-ds
    spec:
      tolerations:
      # Allow this pod to be rescheduled while the node is in "critical add-ons only" mode.
      # This, along with the annotation above marks this pod as a critical add-on.
      - key: CriticalAddonsOnly
        operator: Exists
      - key: nvidia.com/gpu
        operator: Exists
        effect: NoSchedule
      containers:
      - image: nvidia/k8s-device-plugin:1.11
        name: nvidia-device-plugin-ctr
        securityContext:
          allowPrivilegeEscalation: false
          capabilities:
            drop: ["ALL"]
        volumeMounts:
          - name: device-plugin
            mountPath: /var/lib/kubelet/device-plugins
      volumes:
        - name: device-plugin
          hostPath:
            path: /var/lib/kubelet/device-plugins
```

Teraz za pomocą [zastosować kubectl] [ kubectl-apply] polecenie, aby utworzyć element DaemonSet i upewnij się, wtyczka urządzenia nVidia został utworzony pomyślnie, jak pokazano w następujących przykładowych danych wyjściowych:

```console
$ kubectl apply -f nvidia-device-plugin-ds.yaml

daemonset "nvidia-device-plugin" created
```

## <a name="confirm-that-gpus-are-schedulable"></a>Upewnij się, że procesorów GPU są ustalonych w harmonogramie

Przy użyciu klastra usługi AKS utworzone Potwierdź ustalonych w harmonogramie w usłudze Kubernetes procesorów GPU. Najpierw Wyświetl listę węzłów w klastrze za pomocą [kubectl get-węzły] [ kubectl-get] polecenia:

```console
$ kubectl get nodes

NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-28993262-0   Ready    agent   13m   v1.12.7
```

Teraz za pomocą [kubectl opisują węzła] [ kubectl-describe] polecenie, aby potwierdzić, czy ustalonych w harmonogramie procesorów GPU. W obszarze *pojemności* sekcji procesora GPU pomysłem jest wystawienie jako `nvidia.com/gpu:  1`.

Poniższego, skróconego przykładu pokazuje, że procesora GPU jest dostępny w węźle, nazwane *aks-nodepool1-18821093-0*:

```console
$ kubectl describe node aks-nodepool1-28993262-0

Name:               aks-nodepool1-28993262-0
Roles:              agent
Labels:             accelerator=nvidia

[...]

Capacity:
 attachable-volumes-azure-disk:  24
 cpu:                            6
 ephemeral-storage:              101584140Ki
 hugepages-1Gi:                  0
 hugepages-2Mi:                  0
 memory:                         57713784Ki
 nvidia.com/gpu:                 1
 pods:                           110
Allocatable:
 attachable-volumes-azure-disk:  24
 cpu:                            5916m
 ephemeral-storage:              93619943269
 hugepages-1Gi:                  0
 hugepages-2Mi:                  0
 memory:                         51702904Ki
 nvidia.com/gpu:                 1
 pods:                           110
System Info:
 Machine ID:                 b0cd6fb49ffe4900b56ac8df2eaa0376
 System UUID:                486A1C08-C459-6F43-AD6B-E9CD0F8AEC17
 Boot ID:                    f134525f-385d-4b4e-89b8-989f3abb490b
 Kernel Version:             4.15.0-1040-azure
 OS Image:                   Ubuntu 16.04.6 LTS
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://1.13.1
 Kubelet Version:            v1.12.7
 Kube-Proxy Version:         v1.12.7
PodCIDR:                     10.244.0.0/24
ProviderID:                  azure:///subscriptions/<guid>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Compute/virtualMachines/aks-nodepool1-28993262-0
Non-terminated Pods:         (9 in total)
  Namespace                  Name                                     CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                                     ------------  ----------  ---------------  -------------  ---
  kube-system                nvidia-device-plugin-daemonset-bbjlq     0 (0%)        0 (0%)      0 (0%)           0 (0%)         2m39s

[...]
```

## <a name="run-a-gpu-enabled-workload"></a>Uruchamianie obciążenia obsługujące procesor GPU

Aby wyświetlić procesora GPU w akcji, należy zaplanować obciążenia obsługujące procesor GPU z żądaniem odpowiedni zasób. W tym przykładzie uruchommy [Tensorflow](https://www.tensorflow.org/versions/r1.1/get_started/mnist/beginners) zadania względem [zestawu danych mnist ręcznie ZAPISANYCH](http://yann.lecun.com/exdb/mnist/).

Utwórz plik o nazwie *przykłady tf-mnist ręcznie zapisanych demo.yaml* i wklej następujące manifest YAML. Następujące manifest zadania obejmuje limit zasobów `nvidia.com/gpu: 1`:

> [!NOTE]
> Jeśli zostanie wyświetlony błąd niezgodności wersji podczas wywoływania do sterowników, takie jak wersja sterownika CUDA jest niewystarczająca dla wersji środowiska uruchomieniowego CUDA, zapoznaj się z wykresu zgodności macierzy sterownika nVidia — [https://docs.nvidia.com/deploy/cuda-compatibility/index.html](https://docs.nvidia.com/deploy/cuda-compatibility/index.html)

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  labels:
    app: samples-tf-mnist-demo
  name: samples-tf-mnist-demo
spec:
  template:
    metadata:
      labels:
        app: samples-tf-mnist-demo
    spec:
      containers:
      - name: samples-tf-mnist-demo
        image: microsoft/samples-tf-mnist-demo:gpu
        args: ["--max_steps", "500"]
        imagePullPolicy: IfNotPresent
        resources:
          limits:
           nvidia.com/gpu: 1
      restartPolicy: OnFailure
```

Użyj [zastosować kubectl] [ kubectl-apply] polecenie, aby uruchomić zadanie. To polecenie analizuje plik manifestu i tworzy zdefiniowane obiekty usługi Kubernetes:

```console
kubectl apply -f samples-tf-mnist-demo.yaml
```

## <a name="view-the-status-and-output-of-the-gpu-enabled-workload"></a>Wyświetl stan i dane wyjściowe obciążenia obsługujące procesor GPU

Monitoruj postęp zadania za pomocą polecenia [kubectl get-zadania] [ kubectl-get] polecenia `--watch` argumentu. Może potrwać kilka minut do ściągnięcia pierwsze obraz i przetworzyć zestawu danych. Gdy *uzupełnienia* kolumna pokazuje *1/1*, zadanie zostało zakończone pomyślnie. Zakończ `kubetctl --watch` polecenia *Ctrl-C*:

```console
$ kubectl get jobs samples-tf-mnist-demo --watch

NAME                    COMPLETIONS   DURATION   AGE

samples-tf-mnist-demo   0/1           3m29s      3m29s
samples-tf-mnist-demo   1/1   3m10s   3m36s
```

Aby wyświetlić dane wyjściowe z włączonymi procesorami GPU obciążenia, należy najpierw uzyskać nazwę zasobnik za pomocą [kubectl get pods-] [ kubectl-get] polecenia:

```console
$ kubectl get pods --selector app=samples-tf-mnist-demo

NAME                          READY   STATUS      RESTARTS   AGE
samples-tf-mnist-demo-mtd44   0/1     Completed   0          4m39s
```

Teraz za pomocą [Dzienniki narzędzia kubectl] [ kubectl-logs] polecenie, aby wyświetlić dzienniki zasobników. Następujące przykładowe dzienniki pod upewnij się, że odpowiednie urządzenie GPU został odnaleziony, `Tesla K80`. Podaj nazwę własnego zasobnika:

```console
$ kubectl logs samples-tf-mnist-demo-smnr6

2019-05-16 16:08:31.258328: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2019-05-16 16:08:31.396846: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties: 
name: Tesla K80 major: 3 minor: 7 memoryClockRate(GHz): 0.8235
pciBusID: 2fd7:00:00.0
totalMemory: 11.17GiB freeMemory: 11.10GiB
2019-05-16 16:08:31.396886: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Tesla K80, pci bus id: 2fd7:00:00.0, compute capability: 3.7)
2019-05-16 16:08:36.076962: I tensorflow/stream_executor/dso_loader.cc:139] successfully opened CUDA library libcupti.so.8.0 locally
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Extracting /tmp/tensorflow/input_data/train-images-idx3-ubyte.gz
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Extracting /tmp/tensorflow/input_data/train-labels-idx1-ubyte.gz
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Extracting /tmp/tensorflow/input_data/t10k-images-idx3-ubyte.gz
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting /tmp/tensorflow/input_data/t10k-labels-idx1-ubyte.gz
Accuracy at step 0: 0.1081
Accuracy at step 10: 0.7457
Accuracy at step 20: 0.8233
Accuracy at step 30: 0.8644
Accuracy at step 40: 0.8848
Accuracy at step 50: 0.8889
Accuracy at step 60: 0.8898
Accuracy at step 70: 0.8979
Accuracy at step 80: 0.9087
Accuracy at step 90: 0.9099
Adding run metadata for 99
Accuracy at step 100: 0.9125
Accuracy at step 110: 0.9184
Accuracy at step 120: 0.922
Accuracy at step 130: 0.9161
Accuracy at step 140: 0.9219
Accuracy at step 150: 0.9151
Accuracy at step 160: 0.9199
Accuracy at step 170: 0.9305
Accuracy at step 180: 0.9251
Accuracy at step 190: 0.9258
Adding run metadata for 199
Accuracy at step 200: 0.9315
Accuracy at step 210: 0.9361
Accuracy at step 220: 0.9357
Accuracy at step 230: 0.9392
Accuracy at step 240: 0.9387
Accuracy at step 250: 0.9401
Accuracy at step 260: 0.9398
Accuracy at step 270: 0.9407
Accuracy at step 280: 0.9434
Accuracy at step 290: 0.9447
Adding run metadata for 299
Accuracy at step 300: 0.9463
Accuracy at step 310: 0.943
Accuracy at step 320: 0.9439
Accuracy at step 330: 0.943
Accuracy at step 340: 0.9457
Accuracy at step 350: 0.9497
Accuracy at step 360: 0.9481
Accuracy at step 370: 0.9466
Accuracy at step 380: 0.9514
Accuracy at step 390: 0.948
Adding run metadata for 399
Accuracy at step 400: 0.9469
Accuracy at step 410: 0.9489
Accuracy at step 420: 0.9529
Accuracy at step 430: 0.9507
Accuracy at step 440: 0.9504
Accuracy at step 450: 0.951
Accuracy at step 460: 0.9512
Accuracy at step 470: 0.9539
Accuracy at step 480: 0.9533
Accuracy at step 490: 0.9494
Adding run metadata for 499
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Aby usunąć skojarzone obiekty usługi Kubernetes, utworzone w tym artykule, należy użyć [kubectl Usuń zadanie] [ kubectl delete] polecenia w następujący sposób:

```console
kubectl delete jobs samples-tf-mnist-demo
```

## <a name="next-steps"></a>Kolejne kroki

Aby uruchamiać zadania Apache Spark, zobacz [zadania uruchamiania Apache Spark w usłudze AKS][aks-spark].

Aby uzyskać więcej informacji na temat obciążeń machine learning (uczenie Maszynowe) na platformie Kubernetes, zobacz [Kubeflow Labs][kubeflow-labs].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubeflow-labs]: https://github.com/Azure/kubeflow-labs
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[azure-pricing]: https://azure.microsoft.com/pricing/
[azure-availability]: https://azure.microsoft.com/global-infrastructure/services/
[nvidia-github]: https://github.com/NVIDIA/k8s-device-plugin

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[aks-spark]: spark-job.md
[gpu-skus]: ../virtual-machines/linux/sizes-gpu.md
[install-azure-cli]: /cli/azure/install-azure-cli
