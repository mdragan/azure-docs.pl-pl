---
title: (PRZESTARZAŁE) Zarządzanie klastrem Azure DC/OS za pomocą interfejsu użytkownika platformy Marathon
description: Wdrażanie kontenerów do klastra usługi kontenera platformy Azure przy użyciu interfejsu użytkownika sieci Web Marathon.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 16c16c0217a796ffbb57e10430f90cb4a7660ac6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61468300"
---
# <a name="deprecated-manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a>(PRZESTARZAŁE) Zarządzanie klastrem usługi Azure Container Service DC/OS za pośrednictwem interfejsu użytkownika sieci web platformy Marathon

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Platforma DC/OS dostarcza środowisko wdrażania i skalowania obciążeń klastrowanych, zapewniając jednocześnie abstrakcyjność sprzętu bazowego. Ponad systemem DC/OS istnieje platforma, która zarządza planowaniem i wykonywaniem obciążeń obliczeniowych.

Platformy są dostępne dla wielu popularnych zadań, w tym dokumencie opisano sposób rozpocząć wdrażanie kontenerów przy użyciu platformy Marathon. 


## <a name="prerequisites"></a>Wymagania wstępne
Przed przystąpieniem do pracy nad tymi przykładami będziesz potrzebować klastra DC/OS skonfigurowanego w usłudze kontenera platformy Azure. Potrzebna będzie także zdalna łączność z tym klastrem. Aby uzyskać więcej informacji na temat tych elementów, zobacz następujące artykuły:

* [Wdrażanie klastra usługi Azure Container Service](container-service-deployment.md)
* [Łączenie z klastrem usługi Azure Container Service](../container-service-connect.md)

> [!NOTE]
> W tym artykule przyjęto założenie, że tunelowanie korzysta się z klastrem DC/OS za pośrednictwem lokalnego portu 80.
>

## <a name="explore-the-dcos-ui"></a>Przegląd interfejsu użytkownika platformy DC/OS
Za pomocą tunelu Secure Shell (SSH) [ustanowione](../container-service-connect.md), przejdź do protokołu http: \/ /localhost /. Spowoduje to załadowanie interfejsu użytkownika sieci Web platformy DC/OS oraz wyświetlenie informacji o klastrze, w tym dotyczących używanych zasobów, aktywnych agentów i uruchomione usługi.

![Interfejs użytkownika platformy DC/OS](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Przegląd interfejsu użytkownika platformy Marathon
Aby wyświetlić interfejs użytkownika platformy Marathon, przejdź do protokołu http: \/ /localhost/platformy marathon. Na tym ekranie możesz uruchomić nowy kontener lub inną aplikację w klastrze DC/OS usługi kontenera platformy Azure. Możesz również sprawdzić informacje dotyczące działających kontenerów i aplikacji.  

![Interfejs użytkownika platformy Marathon](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Wdrażanie kontenera w formacie programu Docker
Aby wdrożyć nowy kontener przy użyciu platformy Marathon, kliknij przycisk **Create Application** (Utwórz aplikację), a następnie wprowadź na kartach formularza następujące informacje:

| Pole | Wartość |
| --- | --- |
| ID |nginx |
| Memory (Pamięć) | 32 |
| Image |nginx |
| Network (Sieć) |Bridged (Pomostowa) |
| Host Port (Port hosta) |80 |
| Protocol (Protokół) |TCP |

![Interfejs użytkownika New Application (Nowa aplikacja) — General (Ogólne)](./media/container-service-mesos-marathon-ui/dcos4.png)

![Interfejs użytkownika New Application (Nowa aplikacja) — Docker Container (Kontener Docker)](./media/container-service-mesos-marathon-ui/dcos5.png)

![Interfejs użytkownika New Application (Nowa aplikacja) — Ports and Service Discovery (Porty i odnajdowanie usług)](./media/container-service-mesos-marathon-ui/dcos6.png)

Jeśli chcesz statycznie mapować port kontenera do portu agenta, musisz użyć trybu JSON. W tym celu przełącz kreatora nowej aplikacji w tryb **JSON Mode** (Tryb JSON) za pomocą przełącznika. Następnie wprowadź następujące ustawienie w sekcji `portMappings` definicji aplikacji. W tym przykładzie port 80 kontenera jest powiązany z portem 80 agenta DC/OS. Po wprowadzeniu tej zmiany możesz wyłączyć tryb JSON w kreatorze.

```none
"hostPort": 80,
```

![Interfejs użytkownika New Application (Nowa aplikacja) — przykładowy port 80](./media/container-service-mesos-marathon-ui/dcos13.png)

Jeśli chcesz włączyć kontrole kondycji, ustaw ścieżkę na karcie **Health Checks** (Kontrole kondycji).

![Interfejs użytkownika New Application (Nowa aplikacja) — kontrole kondycji](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

Klaster DC/OS jest wdrażany z zestawem agentów: prywatnym i publicznym. Aby klaster mógł uzyskiwać dostęp do aplikacji z Internetu, należy wdrożyć aplikacje w agencie publicznym. W tym celu wybierz kartę **Optional** (Opcjonalne) kreatora nowej aplikacji i wprowadź wartość **slave_public** w polu **Accepted Resource Roles** (Zaakceptowane role zasobów).

Następnie kliknij przycisk **Create Application** (Utwórz aplikację).

![Interfejs użytkownika New Application (Nowa aplikacja) — ustawienia agenta publicznego](./media/container-service-mesos-marathon-ui/dcos14.png)

Po powrocie do strony głównej platformy Marathon widoczny jest stan wdrożenia kontenera. Początkowo będzie widoczny stan **Deploying** (Wdrażanie). Po pomyślnym wdrożeniu stan zmieni się na **Running** (Uruchomione).

![Strona główna interfejsu użytkownika platformy Marathon — stan wdrożenia kontenera](./media/container-service-mesos-marathon-ui/dcos7.png)

Po przełączeniu się do platformy DC/OS interfejs użytkownika sieci web (http:\//localhost/), zobaczysz, że zadanie (w tym przypadku kontener w formacie Docker) jest uruchomiony w klastrze DC/OS.

![Interfejs użytkownika sieci Web platformy DC/OS — zadanie uruchomione w klastrze](./media/container-service-mesos-marathon-ui/dcos8.png)

Aby zobaczyć węzeł klastra, w którym zadanie jest uruchomione, kliknij kartę **Nodes** (Węzły).

![Interfejs użytkownika sieci Web platformy DC/OS — węzeł klastra zadania](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-the-container"></a>Dotrzeć do kontenera

W tym przykładzie aplikacja jest uruchomiona w węźle agenta publicznego. Możesz uzyskać dostępu do aplikacji, z Internetu, przechodząc do agenta, nazwy FQDN klastra: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, gdzie:

* **DNSPREFIX** (prefiks_DNS) to prefiks DNS podany podczas wdrażania klastra.
* **REGION** to region, w którym znajduje się grupa zasobów.

    ![Serwer Nginx z Internetu](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a>Kolejne kroki
* [Praca z platformą DC/OS i interfejsem API platformy Marathon](container-service-mesos-marathon-rest.md)

* Pełne wykorzystanie usługi Azure Container Service dzięki rozwiązaniu Mesos

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
