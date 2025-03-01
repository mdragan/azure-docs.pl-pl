---
title: Tworzenie połączenia lokacja-lokacja z platformą Azure w usłudze Azure Virtual WAN | Microsoft Docs
description: Z tego samouczka dowiesz się, jak utworzyć połączenie lokacja-lokacja VPN z platformą Azure w usłudze Azure Virtual WAN.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 04/23/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my local site to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: e8e251aa5031a8eadd2d567bff2830449c7decc3
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64689511"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan"></a>Samouczek: Utwórz połączenie lokacja-lokacja przy użyciu Azure wirtualnego WAN

W tym samouczku pokazano, w jaki sposób przy użyciu usługi Azure Virtual WAN utworzyć połączenie z zasobami na platformie Azure za pośrednictwem połączenia sieci VPN protokołu IPsec/IKE (IKEv1 i IKEv2). Ten typ połączenia wymaga lokalnego urządzenia sieci VPN z przypisanym publicznym adresem IP dostępnym z zewnątrz. Aby uzyskać więcej informacji na temat usługi Virtual WAN, zobacz [Omówienie usługi Virtual WAN](virtual-wan-about.md).

> [!NOTE]
> Jeśli masz wiele lokacji, skorzystaj z usług [partnera usługi Virtual WAN](https://aka.ms/virtualwan) w celu utworzenia tej konfiguracji. Możesz też utworzyć tę konfigurację samodzielnie, jeśli masz wiedzę z zakresu sieci i doświadczenie w konfigurowaniu urządzeń sieci VPN.
>

![Diagram usługi Virtual WAN](./media/virtual-wan-about/virtualwan.png)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie sieci WAN
> * Tworzenie lokacji
> * Tworzenie koncentratora
> * Łączenie koncentratora z lokacją
> * Tworzenie sieci wirtualnej zgodny, (Jeśli nie masz jeszcze jeden)
> * Łączenie sieci wirtualnej z koncentratorem
> * Pobieranie i stosowanie konfiguracji urządzenia sieci VPN
> * Wyświetlanie wirtualnej sieci WAN
> * Wyświetlanie kondycji zasobu
> * Monitorowanie połączenia

## <a name="before-you-begin"></a>Przed rozpoczęciem

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="openvwan"></a>1. Tworzenie wirtualnej sieci WAN

Przejdź w przeglądarce do witryny [Azure Portal](https://aka.ms/azurevirtualwanpreviewfeatures) i zaloguj się przy użyciu konta platformy Azure.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="site"></a>2. Tworzenie lokacji

Możesz utworzyć dowolną liczbę lokacji, tak aby odpowiadała liczbie lokalizacji fizycznych. Jeśli na przykład masz oddziały w Nowym Jorku, Londynie i Los Angeles, należy utworzyć trzy oddzielne lokacje. W tych lokacjach znajdują się punkty końcowe lokalnych urządzeń sieci VPN. Obecnie można określić tylko jedną prywatną przestrzeń adresową na lokację.

1. Kliknij utworzoną sieć WAN. Na stronie sieci WAN w obszarze **Architektura sieci WAN** kliknij pozycję **Lokacje sieci VPN**, aby otworzyć stronę lokacji sieci VPN.
2. Na stronie **Lokacje sieci VPN** kliknij pozycję **+Utwórz lokację**.
3. Na stronie **Tworzenie lokacji** wypełnij następujące pola:

   * **Nazwa** — nazwę, za pomocą którego do odwoływania się do witryny w środowisku lokalnym.
   * **Publiczny adres IP** — publiczny adres IP urządzenia sieci VPN, który znajduje się w lokacji lokalnej.
   * **Prywatna przestrzeń adresowa** — to przestrzeń adresowa IP znajdująca się w lokacji lokalnej. Ruch do tej przestrzeni adresowej jest kierowany do lokacji lokalnej.
   * **Subskrypcja** — sprawdź, czy wybrano właściwą subskrypcję.
   * **Grupa zasobów** — grupa zasobów, której chcesz używać.
   * **Lokalizacja**
4. Kliknij pozycję **Pokaż zaawansowane**, aby wyświetlić dodatkowe ustawienia. Możesz wybrać **BGP** można włączyć protokołu BGP, która spowoduje włączenie funkcji protokołu BGP dla wszystkich połączeń, utworzone w tej witrynie na platformie Azure. Możesz też wprowadzić **Informacje o urządzeniu** (pola opcjonalne). To może pomóc lepiej zrozumieć środowiska można dodać możliwości optymalizacji dodatkowe w przyszłości lub ułatwiających rozwiązywanie problemów z zespołu platformy Azure.
5. Kliknij pozycję **Potwierdź**.
6. Po kliknięciu pozycji **Potwierdź** wyświetl stan na stronie lokacji sieci VPN. Stan lokacji zmieni się z **Aprowizowanie** na **Aprowizowano**.

## <a name="hub"></a>3. Tworzenie koncentratora

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="associate"></a>4. Kojarzenie lokacji z koncentratorem

Zasadniczo koncentratory powinny być skojarzone z lokacjami znajdującymi się w tym samym regionie, w którym znajduje się sieć wirtualna.

1. Na stronie **Lokacje sieci VPN** wybierz lokację lub lokacje, które chcesz skojarzyć z koncentratorem, a następnie kliknij pozycję **+Nowe skojarzenie koncentratora**.
2. Na stronie **Kojarzenie lokacji z co najmniej jednym koncentratorem** wybierz koncentrator z listy rozwijanej. Możesz skojarzyć lokację z dodatkowymi koncentratorami, klikając pozycję **+Dodaj skojarzenie**.
3. Możesz również dodać w tym miejscu określony **klucz wstępny** lub użyć domyślnego.
4. Kliknij pozycję **Potwierdź**.
5. Możesz wyświetlić stan połączenia na stronie **Lokacje sieci VPN**.

## <a name="vnet"></a>5. Tworzenie sieci wirtualnej

Jeśli nie masz jeszcze sieci wirtualnej, można szybko utworzyć ją przy użyciu programu PowerShell lub witryny Azure portal. Jeśli masz już sieć wirtualną, sprawdź, czy spełnia ona wymagane kryteria i nie ma bramy sieci wirtualnej.

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="vnet"></a>6. Łączenie sieci wirtualnej z koncentratorem

W tym kroku zostanie utworzone połączenie równorzędne pomiędzy koncentratorem i siecią wirtualną. Powtórz te czynności dla każdej sieci wirtualnej, z którą chcesz się połączyć.

1. Na stronie sieci wirtualnej WAN kliknij pozycję **Połączenia sieci wirtualnych**.
2. Na stronie połączenia sieci wirtualnej kliknij polecenie **+ Dodaj połączenie**.
3. Na stronie **Dodaj połączenie** wypełnij następujące pola:

    * **Nazwa połączenia** — nazwij połączenie.
    * **Koncentratory** — wybierz koncentrator, z którym chcesz skojarzyć to połączenie.
    * **Subskrypcja** — sprawdź, czy wybrano właściwą subskrypcję.
    * **Sieć wirtualna** — wybierz sieć wirtualną, którą chcesz połączyć z tym koncentratorem. Sieć wirtualna nie może mieć istniejącej bramy sieci wirtualnej.
4. Kliknij przycisk **OK**, aby utworzyć połączenie równorzędne.

## <a name="device"></a>7. Pobieranie konfiguracji sieci VPN

Skorzystaj z konfiguracji urządzenia sieci VPN, aby skonfigurować swoje lokalne urządzenie sieci VPN.

1. Na stronie usługi Virtual WAN kliknij pozycję **Przegląd**.
2. W górnej części strony Przegląd kliknij pozycję **Pobierz konfigurację sieci VPN**. Platforma Azure utworzy konto magazynu w grupie zasobów „microsoft-network-[lokalizacja]”, gdzie „lokalizacja” to lokalizacja sieci WAN. Po zastosowaniu konfiguracji na urządzeniach sieci VPN możesz usunąć to konto magazynu.
3. Po zakończeniu tworzenia pliku możesz kliknąć link, aby go pobrać.
4. Zastosuj konfigurację na urządzeniu sieci VPN.

### <a name="understanding-the-vpn-device-configuration-file"></a>Opis pliku konfiguracji urządzenia sieci VPN

Plik konfiguracji urządzenia zawiera ustawienia używane podczas konfigurowania lokalnego urządzenia sieci VPN. Podczas przeglądania tego pliku należy zwrócić uwagę na następujące informacje:

* **vpnSiteConfiguration —** ta sekcja zawiera szczegółowe informacje o urządzeniu skonfigurowanym jako lokacja połączona z wirtualną sieci WAN. Zawiera nazwę i publiczny adres IP urządzenia w oddziale.
* **vpnSiteConnections -** ta sekcja zawiera informacje o następujących ustawień:

    * **Przestrzeń adresowa** sieci wirtualnej koncentratora<br>Przykład:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Przestrzeń adresowa** sieci wirtualnych, które są połączone z koncentratorem<br>Przykład:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * **Adresy IP** bramy vpngateway koncentratora wirtualnego. Ponieważ każde połączenie bramy vpngateway składa się z dwóch tuneli w konfiguracji aktywne aktywne, zobaczysz oba adresy IP wymienione w tym pliku. W tym przykładzie są to wartości „Instance0” i „Instance1” dla każdej lokacji.<br>Przykład:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * **Szczegóły konfiguracji połączenia bramy vpngateway**, takie jak protokół BGP, klucz wstępny i tym podobne. PSK to klucz wstępny, który jest generowany automatycznie. Zawsze możesz edytować połączenie na stronie Przegląd, aby użyć niestandardowego klucza wstępnego.
  
### <a name="example-device-configuration-file"></a>Przykładowy plik konfiguracji urządzenia

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.30.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

### <a name="configuring-your-vpn-device"></a>Konfigurowanie urządzenia sieci VPN

>[!NOTE]
> Jeśli używasz rozwiązania partnera usługi Virtual WAN, konfiguracja urządzenia sieci VPN odbywa się automatycznie. Kontroler urządzenia uzyskuje plik konfiguracji z platformy Azure i stosuje go do urządzenia w celu skonfigurowania połączenia z platformą Azure. Oznacza to, że nie musisz wiedzieć, w jaki sposób skonfigurować urządzenie sieci VPN ręcznie.
>

Jeśli chcesz wiedzieć, jak skonfigurować urządzenie, możesz skorzystać z instrukcji na [stronie ze skryptami konfigurowania urządzenia sieci VPN](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts), z następującymi zastrzeżeniami:

* Instrukcje na stronie urządzeń sieci VPN nie zostały napisane dla usługi Virtual WAN, ale możesz skorzystać z wartości z pliku konfiguracji usługi Virtual WAN, aby ręcznie skonfigurować urządzenie sieci VPN. 
* Skrypty konfiguracji urządzenia do pobrania przeznaczone dla usługi VPN Gateway nie działają w przypadku usługi Virtual WAN, ponieważ ich konfiguracja różni się.
* Nowa usługa Virtual WAN może obsługiwać zarówno protokół IKEv1, jak i IKEv2.
* W usłudze Virtual WAN można używać tylko instrukcji i urządzeń sieci VPN opartych na trasach.

## <a name="viewwan"></a>8. Wyświetlanie wirtualnej sieci WAN

1. Przejdź do wirtualnej sieci WAN.
2. Na stronie Przegląd każdy punkt na mapie przedstawia koncentrator. Umieść kursor na dowolnym punkcie, aby wyświetlić podsumowanie kondycji koncentratora.
3. W sekcji dotyczącej koncentratorów i połączeń możesz wyświetlić stan koncentratora, lokację, region, stan połączenia sieci VPN oraz bajty przychodzące i wychodzące.

## <a name="viewhealth"></a>9. Wyświetlanie kondycji zasobów

1. Przejdź do sieci WAN.
2. Na stronie sieci WAN w sekcji **Pomoc techniczna i rozwiązywanie problemów** kliknij pozycję **Kondycja** i wyświetl zasób.

## <a name="connectmon"></a>10. Monitorowanie połączenia

Utwórz połączenie, aby monitorować komunikację pomiędzy maszyną wirtualną platformy Azure a zdalną lokacją. Aby uzyskać informacje dotyczące konfigurowania monitora połączeń, zobacz [Monitorowanie komunikacji sieciowej](~/articles/network-watcher/connection-monitor.md). Pole Źródło zawiera adres IP maszyny wirtualnej na platformie Azure, a docelowy adres IP to adres IP lokacji.

## <a name="cleanup"></a>11. Oczyszczanie zasobów

Jeśli te zasoby nie są już potrzebne, możesz użyć [AzResourceGroup Usuń](/powershell/module/az.resources/remove-azresourcegroup) Aby usunąć grupę zasobów i wszystkie zawarte w niej zasoby. Zastąp wartość „myResourceGroup” nazwą grupy zasobów, a następnie uruchom następujące polecenie programu PowerShell:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Kolejne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie sieci WAN
> * Tworzenie lokacji
> * Tworzenie koncentratora
> * Łączenie koncentratora z lokacją
> * Łączenie sieci wirtualnej z koncentratorem
> * Pobieranie i stosowanie konfiguracji urządzenia sieci VPN
> * Wyświetlanie wirtualnej sieci WAN
> * Wyświetlanie kondycji zasobu
> * Monitorowanie połączenia

Aby uzyskać więcej informacji na temat usługi Virtual WAN, zobacz stronę [Omówienie usługi Virtual WAN](virtual-wan-about.md).
