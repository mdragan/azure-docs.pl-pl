---
title: Warstwy cenowe dla usługi Azure Database dla serwera MariaDB
description: W tym artykule opisano warstw cenowych dla usługi Azure Database dla serwera MariaDB.
author: jan-eng
ms.author: janeng
ms.service: mariadb
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 7a52d05c77d0aeb8ebeba196df60e59f0647fea9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66233919"
---
# <a name="azure-database-for-mariadb-pricing-tiers"></a>Azure Database dla serwera MariaDB warstw cenowych

W jednym z trzech różnych warstw cenowych, można utworzyć usługi Azure Database dla serwera MariaDB: Podstawowa, Ogólnego przeznaczenia i Zoptymalizowane pod kątem pamięci. Warstwy cenowe są zróżnicowane według ilości zasobów obliczeniowych w rdzeni wirtualnych, które mogą być udostępniane, ilość pamięci na rdzeń wirtualny i technologia magazynu używany do przechowywania danych. Wszystkie zasoby są aprowizowane na poziomie serwera MariaDB. Serwer może zawierać jeden lub wiele baz danych.

|    | **Podstawowa** | **Ogólnego przeznaczenia** | **Zoptymalizowane pod kątem pamięci** |
|:---|:----------|:--------------------|:---------------------|
| Generacja obliczeń | 5\. generacja |5\. generacja | 5\. generacja |
| rdzenie wirtualne | 1, 2 | 2, 4, 8, 16, 32, 64 |2, 4, 8, 16, 32 |
| Ilość pamięci na rdzeń wirtualny | 2 GB | 5 GB | 10 GB |
| Rozmiar magazynu | 5 GB do 1 TB | 5 GB do 4 TB | 5 GB do 4 TB |
| Typ magazynu | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| Okres przechowywania kopii zapasowej bazy danych | 7 – 35 dni | 7 – 35 dni | 7 – 35 dni |

Aby wybrać warstwę cenową, skorzystaj z poniższej tabeli jako punktu wyjścia.

| Warstwa cenowa | Docelowe obciążenia |
|:-------------|:-----------------|
| Podstawowa | Obciążenia wymagające niewielkiej obliczeniowej i wydajności we/wy. Na przykład serwery używane do tworzenia i testowania lub niewielkie, rzadko używane aplikacje. |
| Ogólne zastosowanie | Większości obciążeń biznesowych wymagających zrównoważonych zasobów obliczeniowych i pamięci, ze skalowalną przepływnością wejścia/wyjścia. Przykłady obejmują serwery do hostowania w sieci web i aplikacje mobilne i inne aplikacje dla przedsiębiorstw.|
| Pamięć | Obciążeń bazy danych o wysokiej wydajności, wymagających wydajności wewnątrzpamięciowej szybsze przetwarzanie transakcji oraz zapewnienie większej współbieżności. Na przykład serwery przetwarzania danych w czasie rzeczywistym oraz wysokowydajne aplikacje transakcyjne i analityczne.|

Po utworzeniu serwera, liczby rdzeni wirtualnych i warstwę cenową (z wyjątkiem do i z Basic) można zmienić w górę lub w dół w ciągu kilku sekund. Możesz również niezależnie dostosować wielkość pamięci masowej w górę oraz okres przechowywania kopii zapasowej w górę lub w dół bez przestojów aplikacji. Nie można zmienić typu magazynu kopii zapasowych, po utworzeniu serwera. Aby uzyskać więcej informacji, zobacz [skalować zasoby](#scale-resources) sekcji.

## <a name="compute-generations-and-vcores"></a>Generacji zasobów obliczeniowych i rdzeni wirtualnych

Obliczenia, że zasoby są dostarczane jako rdzeni wirtualnych, który reprezentuje logiczny procesor CPU bazowego sprzętu. Logiczne procesory CPU 5 generacji wykorzystują procesory Intel E5-2673 v4 (broadwell z zegarem) 2.3 GHz procesorów.

## <a name="storage"></a>Magazyn

Magazyn, który możesz aprowizować to pojemność magazynu jest dostępne dla usługi Azure Database dla serwera MariaDB. Magazyn jest używany dla plików bazy danych, pliki tymczasowe, dzienników transakcji i serwera MariaDB dzienniki. Łączną ilość pamięci masowej, które możesz aprowizować definiuje również wydajność We/Wy, dostępne na serwerze.

|    | **Podstawowa** | **Ogólnego przeznaczenia** | **Zoptymalizowane pod kątem pamięci** |
|:---|:----------|:--------------------|:---------------------|
| Typ magazynu | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| Rozmiar magazynu | 5 GB do 1 TB | 5 GB do 4 TB | 5 GB do 4 TB |
| Rozmiar przyrost magazynu | 1 GB | 1 GB | 1 GB |
| Operacje wejścia/wyjścia | Zmienna |3 IOPS/GB<br/>Min 100 IOPS<br/>6000 maks. IOPS | 3 IOPS/GB<br/>Min 100 IOPS<br/>6000 maks. IOPS |

Dodanie pojemności dodatkowego miejsca do magazynowania, podczas i po utworzeniu serwera i umożliwić systemu w celu zwiększenia magazynu automatycznie w zależności od użycia magazynu obciążenia. Warstwa podstawowa nie zapewnia gwarancję operacji We/Wy. Ogólnego przeznaczenia i zoptymalizowana pod kątem pamięci, warstw cenowych operacje We/Wy skalowanie o rozmiarze aprowizowanego magazynu współczynnik 3:1.

Można monitorować swoje użycie operacji We/Wy w witrynie Azure portal lub za pomocą poleceń interfejsu wiersza polecenia platformy Azure. Istotne metryki do monitorowania są [limit przestrzeni dyskowej, procent użycia magazynu, magazynu i procent We/Wy](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>Przekroczony limit magazynu

Serwery z mniej niż 100 GB zainicjowano obsługę administracyjną magazynu są oznaczone tylko do odczytu, jeśli ilość wolnego miejsca jest mniejsza niż 512MB lub 5% rozmiaru aprowizowanego magazynu. Serwery z więcej niż 100 GB zainicjowano obsługę administracyjną magazynu są oznaczone odczytu, tylko wtedy, gdy ilość wolnego miejsca jest mniejsza niż 5 GB.

Przykładowo po aprowizowaniu 110 GB miejsca do magazynowania i rzeczywiste użycie przekroczą 105 GB danych, serwer jest oznaczony jako tylko do odczytu. Alternatywnie, jeśli aprowizowaniu 5 GB miejsca do magazynowania, serwer zostanie oznaczony tylko do odczytu po przekraczać 512 MB wolnego miejsca.

Podczas gdy usługa próbuje przełączyć serwer w tryb tylko do odczytu, wszystkie nowe żądania transakcji zapisu są blokowane, a istniejące aktywne transakcje nadal są wykonywane. Gdy serwer zostanie przełączony w tryb tylko do odczytu, wszystkie kolejne zatwierdzenia transakcji i operacji zapisu zakończą się niepowodzeniem. Zapytania odczytu będą działać bez żadnych przerw. Gdy zwiększysz zaaprowizowaną pojemność magazynu, serwer będzie ponownie gotowy do akceptowania transakcji zapisu.

Zaleca się włączenie funkcji magazynu automatyczne powiększanie lub skonfigurować alert, aby otrzymywać powiadomienia, gdy magazyn serwera zbliża się do progu więc można uniknąć, jakim jest w stanie tylko do odczytu. Aby uzyskać więcej informacji, zobacz dokumentację na [sposobu ustawiania alertu](howto-alert-metric.md).

### <a name="storage-auto-grow"></a>Magazyn automatyczne powiększanie

Jeśli rosnąć automatycznie magazynu jest włączona, Magazyn automatycznie rozszerza się bez wywierania wpływu na obciążenie pracą. W przypadku serwerów z mniej niż 100 GB zainicjowano obsługę administracyjną magazynu rozmiar aprowizowanego magazynu jest zwiększana o 5 GB, jak ilość wolnego miejsca jest poniżej większą niż 1 GB lub 10% aprowizowanego magazynu. W przypadku serwerów z więcej niż 100 GB magazynu aprowizowanego rozmiaru aprowizowanego magazynu zwiększa się o 5% gdy ilość wolnego miejsca jest poniżej 5% rozmiaru aprowizowanego magazynu. Limity pamięci masowej, jak określono powyżej mają zastosowanie.

Przykładowo po aprowizowaniu 1000 GB miejsca do magazynowania i rzeczywiste użycie przekroczą 950 GB, rozmiar magazynu serwera zostaje zwiększona do 1050 GB. Alternatywnie Jeśli aprowizowaniu 10 GB miejsca do magazynowania, rozmiar magazynu jest wzrost do 15 GB podczas kosztuje mniej niż 1 GB pamięci.

## <a name="backup"></a>Backup

Usługa automatycznie wykonuje kopie zapasowe serwera. Minimalny okres przechowywania kopii zapasowych wynosi siedem dni. Można ustawić okresu przechowywania maksymalnie 35 dni. Okres przechowywania można zmienić w dowolnym momencie podczas okresu istnienia serwera. Możesz wybrać między lokalnie nadmiarowy i geograficznie nadmiarowych kopii zapasowych. Geograficznie nadmiarowe kopie zapasowe są również przechowywane w [regionu sparowanego geograficznie](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) regionu, w którym serwer zostanie utworzona. Tę nadmiarowość zapewnia poziom ochrony w przypadku awarii. Możesz również uzyskać możliwość przywracania serwera do innego regionu platformy Azure, w którym ta usługa jest dostępna za pomocą geograficznie nadmiarowych kopii zapasowych. Nie jest możliwa zmiana pomiędzy dwiema opcjami magazynu kopii zapasowej po utworzeniu serwera.

## <a name="scale-resources"></a>Skalowanie zasobów

Po utworzeniu serwera, możesz zmienić niezależnie rdzeni wirtualnych, warstwa cenowa (z wyjątkiem do i z Basic), ilości miejsca do magazynowania i okres przechowywania kopii zapasowej. Nie można zmienić typu magazynu kopii zapasowych, po utworzeniu serwera. Liczba rdzeni wirtualnych można skalować w górę lub w dół. Okres przechowywania kopii zapasowej można skalować w górę lub w dół od 7 do 35 dni. Tylko można zwiększyć rozmiar magazynu. Skalowanie zasobów może odbywać się za pośrednictwem portalu lub wiersza polecenia platformy Azure. 

<!--For an example of scaling by using Azure CLI, see [Monitor and scale an Azure Database for MariaDB server by using Azure CLI](scripts/sample-scale-server.md).-->

Jeśli zmienisz liczbę rdzeni wirtualnych lub warstwy cenowej, kopię oryginalny serwer jest tworzony z nowych przydzielić pamięci obliczeniowej. Po skonfigurowaniu i uruchomieniu nowego serwera połączeń są przełączono na nowym serwerze. W chwili, gdy system przełącza do nowego serwera nawiązywanie nowych połączeń nie może zostać nawiązana i wycofać wszystkie niezatwierdzone transakcje. To okno zmienia się, ale w większości przypadków jest mniej niż minutę.

Skalowanie magazynu i zmiana okresu przechowywania kopii zapasowej są prawdziwe operacje online. Brak przestojów, a nie ma wpływu na aplikację. Operacje We/Wy skalowania o rozmiarze aprowizowanego magazynu, możesz zwiększyć operacje We/Wy dostępne na serwerze, skalując w górę magazynu.

## <a name="pricing"></a>Cennik

Aby uzyskać najbardziej aktualne informacje o cenach, zobacz usługę [stronę z cennikiem](https://azure.microsoft.com/pricing/details/mariadb/). Aby zobaczyć kosztów dla konfiguracji, należy [witryny Azure portal](https://portal.azure.com/#create/Microsoft.MariaDBServer) Wyświetla miesięczny koszt w **warstwa cenowa** kartę oparciu o wybrane opcje. Jeśli nie masz subskrypcji platformy Azure, można użyć Kalkulatora cen platformy Azure można pobrać szacowana cena. Na [kalkulatora cen platformy Azure](https://azure.microsoft.com/pricing/calculator/) witryny sieci Web, wybierz opcję **Dodaj elementy**, rozwiń węzeł **baz danych** kategorii i wybierz polecenie **— Azure Database dla serwera MariaDB**Aby dostosować opcje.

## <a name="next-steps"></a>Kolejne kroki
- Dowiedz się więcej o [usługi ograniczenia](concepts-limits.md).
- Dowiedz się, jak [Tworzenie serwera MariaDB w witrynie Azure portal](quickstart-create-mariadb-server-database-using-azure-portal.md).

<!--
- Learn how to [monitor and scale an Azure Database for MariaDB server by using Azure CLI](scripts/sample-scale-server.md).-->
