---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 01/23/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: cd97c61e7493249785293ae331713ba1a98efee3
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67183786"
---
## <a name="run-image-from-registry"></a>Uruchamianie obrazu z rejestru

Teraz można ściągnąć i uruchomić obraz kontenera `hello-world:v1` z rejestru kontenerów za pomocą polecenia [docker run][docker-run]:

```
docker run <acrLoginServer>/hello-world:v1  
```

Przykładowe dane wyjściowe: 

```
Unable to find image 'mycontainerregistry007.azurecr.io/hello-world:v1' locally
v1: Pulling from hello-world
Digest: sha256:662dd8e65ef7ccf13f417962c2f77567d3b132f12c95909de6c85ac3c326a345
Status: Downloaded newer image for mycontainerregistry007.azurecr.io/hello-world:v1

Hello from Docker!
This message shows that your installation appears to be working correctly.

[...]
```

<!-- LINKS - External -->
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
