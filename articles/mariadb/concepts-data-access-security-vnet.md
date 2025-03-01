---
title: Azure Database dla serwera MariaDB w sieci wirtualnej usługi punktu końcowego — omówienie | Dokumentacja firmy Microsoft
description: W tym artykule opisano, jak działają punkty końcowe usługi sieci wirtualnej dla usługi Azure Database dla serwera MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 02/26/2019
ms.openlocfilehash: 5a4e6819eeff2a2c8efaf3807c38cc06f7c35002
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60332867"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-database-for-mariadb"></a>Użyj reguł i punktów końcowych usługi sieci wirtualnej dla usługi Azure Database dla serwera MariaDB

*Reguły sieci wirtualnej* są jedną funkcję zabezpieczeń zapory, która kontroluje, czy usługi Azure Database dla serwera MariaDB akceptuje łączności, które są wysyłane z określonej podsieci w sieciach wirtualnych. W tym artykule opisano, dlaczego funkcja reguły sieci wirtualnej jest czasami najlepszym rozwiązaniem dla bezpiecznego zezwolenie na komunikację z usługi Azure Database dla serwera MariaDB.

Aby utworzyć regułę sieci wirtualnej musi najpierw mieć [sieci wirtualnej] [ vm-virtual-network-overview] (VNet) i [punkt końcowy usługi sieci wirtualnej] [ vm-virtual-network-service-endpoints-overview-649d] dla reguły do odwołania. Poniższej ilustracji przedstawiono, jak działa punkt końcowy usługi sieci wirtualnej z usługą Azure Database dla serwera MariaDB:

![Przykład działania punkt końcowy usługi sieci wirtualnej](media/concepts-data-access-security-vnet/vnet-concept.png)

> [!NOTE]
> Ta funkcja jest dostępna we wszystkich regionach platformy Azure wdrożonym — Azure Database dla serwera MariaDB ogólnego przeznaczenia i zoptymalizowana pod kątem pamięci serwera.

<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminologia i opis

**Sieć wirtualna:** Może mieć sieci wirtualne, skojarzony z subskrypcją platformy Azure.

**Podsieć:** Sieć wirtualna zawiera **podsieci**. Żadnych maszyn wirtualnych (VM), do których masz są przypisywane do podsieci. W jednej podsieci może zawierać wiele maszyn wirtualnych lub innych węzłów obliczeniowych. Obliczenia, że węzły, które znajdują się poza siecią wirtualną nie może uzyskiwać dostęp do sieci wirtualnej, chyba, że konfigurowanie zabezpieczeń w taki sposób, aby zezwolić na dostęp.

**Punkt końcowy usługi wirtualne sieci:** A [punkt końcowy usługi sieci wirtualnej] [ vm-virtual-network-service-endpoints-overview-649d] jest podsiecią, w których wartości właściwości zawierają jedną lub więcej nazw typu formalnego usługi platformy Azure. W tym artykule jesteśmy zainteresowani nazwę typu **Microsoft.Sql**, która odnosi się do usługi platformy Azure o nazwie bazy danych SQL. Ten tag usługi dotyczy również do usługi Azure Database dla MariaDB, MySQL i postgresql w warstwie usług. Ważne jest, aby pamiętać podczas stosowania **Microsoft.Sql** tag usługi punktu końcowego usługi sieci wirtualnej zostanie skonfigurowany ruchu w ramach punktu końcowego usługi dla usługi Azure SQL Database, Azure Database dla serwera MariaDB, usługi Azure Database for MySQL i Azure Baza danych postgresql w warstwie serwerów w podsieci.

**Reguła sieci wirtualnej:** Reguły sieci wirtualnej dla usługi Azure Database dla serwera MariaDB jest usługi Azure Database dla serwera MariaDB podsieci, który znajduje się na liście kontroli dostępu (ACL). Aby być na liście kontroli dostępu dla usługi Azure Database dla serwera MariaDB, musi zawierać podsieci **Microsoft.Sql** nazwy typu.

Reguły sieci wirtualnej informuje usługi Azure Database dla serwera MariaDB do akceptowania komunikacji z każdego węzła, który znajduje się w podsieci.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Zalety reguły sieci wirtualnej

Do momentu podejmowanie akcji maszyny wirtualne w podsieci nie może komunikować się za pomocą usługi Azure Database dla serwera MariaDB. Jedna akcja, która ustanawia komunikacji jest tworzenie reguły sieci wirtualnej. Racjonalne uzasadnienie wyboru podejście reguły sieci wirtualnej wymaga to dyskusji porównania i kontrast, obejmujące konkurencyjnych opcje zabezpieczeń oferowanych przez zaporę.

### <a name="a-allow-access-to-azure-services"></a>A. Zezwalaj na dostęp do usług platformy Azure

Okienko zabezpieczeń połączenia ma **włączyć/wyłączyć** przycisku, która jest oznaczona etykietą **zezwolić na dostęp do usług platformy Azure**. **ON** ustawienie umożliwia komunikację ze wszystkich adresów IP platformy Azure i wszystkie podsieci platformy Azure. Te adresy IP platformy Azure lub podsieci może nie być własnością użytkownika. To **ON** ustawienie jest prawdopodobnie mniej rygorystyczne niż usługi Azure Database for MariaDB bazę danych jako. Funkcja reguły sieci wirtualnej oferuje znacznie bardziej szczegółową kontrolę.

### <a name="b-ip-rules"></a>B. Reguły IP

Azure Database for MariaDB zapora zezwala na określ zakresy adresów IP, z których łączności są akceptowane do usługi Azure Database dla serwera MariaDB. To podejście jest odpowiednie dla stabilne adresów IP, które są spoza sieci prywatnej platformy Azure. Jednak wiele węzłów w sieci prywatnej platformy Azure są skonfigurowane przy użyciu *dynamiczne* adresów IP. Dynamiczne adresy IP mogą zmienić, np. po ponownym uruchomieniu maszyny Wirtualnej. Byłoby folly, aby określić dynamiczny adres IP dla reguły zapory w środowisku produkcyjnym.

Opcja IP można odzyskana, uzyskując *statyczne* adresu IP dla maszyny Wirtualnej. Aby uzyskać więcej informacji, zobacz [skonfigurować prywatnych adresów IP dla maszyny wirtualnej przy użyciu witryny Azure portal][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Jednak podejście statycznego adresu IP może być trudne do zarządzania i jest kosztowny w przypadku na dużą skalę. Reguły sieci wirtualnej są łatwiejsze ustalenie, jak i do zarządzania.

### <a name="c-cannot-yet-have-azure-database-for-mariadb-on-a-subnet-without-defining-a-service-endpoint"></a>C. Nie można jeszcze — Azure Database dla serwera MariaDB w podsieci bez definiowania punktu końcowego usługi

Jeśli Twoje **Microsoft.Sql** serwer był węzłem w podsieci w sieci wirtualnej, wszystkie węzły w sieci wirtualnej można nawiązać komunikacji z usługi Azure Database dla serwera MariaDB. W tym przypadku maszyn wirtualnych można nawiązać komunikacji z usługą Azure Database dla serwera MariaDB bez żadnych reguł sieci wirtualnej lub reguły IP.

Jednak począwszy od sierpnia 2018 r. Usługa Azure Database for MariaDB nie jest jeszcze między usługami, które można przypisać bezpośrednio do podsieci.

<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Szczegóły dotyczące reguł sieci wirtualnej

W tej sekcji opisano kilka szczegóły dotyczące reguł sieci wirtualnej.

### <a name="only-one-geographic-region"></a>Tylko jeden region geograficzny

Każdy punkt końcowy usługi sieci wirtualnej mają zastosowanie do tylko jednego regionu platformy Azure. Punkt końcowy nie uwzględnia innych regionach akceptował komunikację z podsieci.

Reguły sieci wirtualnej jest ograniczona do regionu, który dotyczy podstawowym punktem końcowym.

### <a name="server-level-not-database-level"></a>Poziom serwera, nie poziom bazy danych

Każda reguła sieci wirtualnej ma zastosowanie do całej bazy danych Azure dla serwera MariaDB, nie tylko do jednej konkretnej bazy danych na serwerze. Innymi słowy sieć wirtualną reguła ma zastosowanie na poziomie serwera, nie na poziomie bazy danych.

### <a name="security-administration-roles"></a>Ról administracyjnych zabezpieczeń

Brak separacji ról zabezpieczeń w administracji punkty końcowe usługi sieci wirtualnej. Akcja jest wymagana z każdej z następujących ról:

- **Administrator sieci:** &nbsp; Włącz punkt końcowy.
- **Administrator bazy danych:** &nbsp; Aktualizowanie listy kontroli dostępu (ACL), aby dodać danej podsieci do usługi Azure Database dla serwera MariaDB.

*Alternatywa RBAC:*

Role bazy danych i administratora sieci mają więcej możliwości niż są wymagane do zarządzania reguł sieci wirtualnej. Potrzebny jest tylko podzbiór ich możliwości.

Masz możliwość korzystania z [kontroli dostępu opartej na rolach (RBAC)] [ rbac-what-is-813s] na platformie Azure, aby utworzyć pojedynczy rolę niestandardową, która zawiera tylko niezbędne podzbiór funkcji. Rola niestandardowa, można użyć zamiast obejmujące administratora sieci lub administrator bazy danych. Obszar powierzchni usługi ekspozycji zabezpieczeń jest mniejszy, jeśli Dodawanie użytkownika do roli niestandardowej, a dodanie użytkownika do dwóch innych ról administratora głównych.

> [!NOTE]
> W niektórych przypadkach usługi Azure Database for MariaDB i podsieciami sieci wirtualnej znajdują się w różnych subskrypcjach. W takich sytuacjach należy zapewnić następujące konfiguracje:
> - Obie subskrypcje muszą być w tej samej dzierżawie usługi Azure Active Directory.
> - Użytkownik ma uprawnienia wymagane do zainicjowania operacje, takie jak włączenie punktów końcowych usługi i dodawanie podsieci sieci wirtualnej do danego serwera.

## <a name="limitations"></a>Ograniczenia

Dla usługi Azure Database dla serwera MariaDB funkcja reguł sieci wirtualnej ma następujące ograniczenia:

- Aplikacja sieci Web mogą być mapowane na prywatny adres IP w sieci wirtualnej/podsieci. Nawet wtedy, gdy punkty końcowe usługi są włączone w danej sieci wirtualnej/podsieci, połączeń z serwerem z aplikacji sieci Web będzie źródło IP publicznej platformy Azure nie źródło sieci wirtualnej/podsieci. Aby włączyć łączność z aplikacji sieci Web na serwerze, który ma reguły zapory sieci wirtualnej, musisz usług systemu Azure umożliwia dostęp do serwera na serwerze.

- W zaporze dla usługi Azure Database dla serwera MariaDB każdą regułę sieci wirtualnej odwołuje się do podsieci. Te odwołania podsieci muszą być hostowane w tym samym regionie geograficznym, który jest hostem usługi Azure Database dla serwera MariaDB.

- Każdy — Azure Database dla serwera MariaDB może mieć maksymalnie 128 wpisy listy ACL dla dowolnego danej sieci wirtualnej.

- Reguły sieci wirtualnej mają zastosowanie tylko do sieci wirtualnej usługi Azure Resource Manager. i nie [klasycznego modelu wdrażania] [ resource-manager-deployment-model-568f] sieci.

- Włączenie w sieci wirtualnej punkty końcowe usługi bazą danych Azure przy użyciu MariaDB **Microsoft.Sql** tag usługi umożliwia również punkty końcowe dla wszystkich usług platformy Azure, bazy danych: Azure Database dla serwera MariaDB, Azure Database for MySQL, Azure Database for PostgreSQL, usługi Azure SQL Database i Azure SQL Data Warehouse.

- Obsługa punktów końcowych usługi sieci wirtualnej jest tylko w przypadku serwerów ogólnego przeznaczenia i zoptymalizowana pod kątem pamięci.

- Na zaporze zakresów adresów IP są stosowane do następujących elementów sieci, ale nie obsługują reguł sieci wirtualnej:
    - [Wirtualnej sieci prywatnej (VPN) lokacja-lokacja (S2S)][vpn-gateway-indexmd-608y]
    - W środowisku lokalnym za pośrednictwem [usługi ExpressRoute][expressroute-indexmd-744v]

## <a name="expressroute"></a>ExpressRoute

Jeśli sieć jest połączony z siecią platformy Azure za pośrednictwem [ExpressRoute][expressroute-indexmd-744v], każdy obwód jest skonfigurowany przy użyciu dwóch publicznych adresów IP w Microsoft Edge. Dwa adresy IP są używane połączyć się programem Microsoft Services, takich jak do usługi Azure Storage, korzystając z publicznej komunikacji równorzędnej Azure.

Aby zezwalać na komunikację z obwodu do usługi Azure Database dla serwera MariaDB, należy utworzyć zasady sieci IP publiczne adresy IP obwodów usługi. Aby można było znaleźć publiczne adresy IP obwodów usługi ExpressRoute, należy otworzyć bilet pomocy technicznej przy użyciu usługi ExpressRoute za pomocą witryny Azure portal.

## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>Dodawanie reguły zapory sieci Wirtualnej do serwera bez włączania na sieć Wirtualną punktów końcowych usługi

Tylko ustawienie reguły zapory nie pomaga zabezpieczyć serwer do sieci wirtualnej. Należy również włączyć punkty końcowe usługi sieci wirtualnej **na** zabezpieczeń, które zostały wprowadzone. Po włączeniu punktów końcowych usługi **na**, podsieciami sieci wirtualnej, wystąpi przestój przed zakończeniem tego procesu przejścia z **poza** do **na**. Jest to szczególnie istotne w kontekście dużych sieciach wirtualnych. Możesz użyć **IgnoreMissingServiceEndpoint** flagę, aby zredukować lub całkowicie wyeliminować przestój podczas przejścia.

Możesz ustawić **IgnoreMissingServiceEndpoint** flagi przy użyciu wiersza polecenia platformy Azure lub portalu.

## <a name="related-articles"></a>Pokrewne artykuły:
- [Sieci wirtualne platformy Azure][vm-virtual-network-overview]
- [Punkty końcowe usługi sieci wirtualnej platformy Azure][vm-virtual-network-service-endpoints-overview-649d]

## <a name="next-steps"></a>Kolejne kroki
W przypadku artykułów na temat tworzenia reguł sieci wirtualnej zobacz:
- [Tworzenie i zarządzanie nimi — Azure Database dla reguł sieci wirtualnej MariaDB przy użyciu witryny Azure portal](howto-manage-vnet-portal.md)
 
<!--
- [Create and manage Azure Database for MariaDB VNet rules using Azure CLI](howto-manage-vnet-using-cli.md)
-->

<!-- Link references, to text, Within this same GitHub repo. -->
[resource-manager-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[vm-virtual-network-overview]: ../virtual-network/virtual-networks-overview.md

[vm-virtual-network-service-endpoints-overview-649d]: ../virtual-network/virtual-network-service-endpoints-overview.md

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[rbac-what-is-813s]: ../active-directory/role-based-access-control-what-is.md

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.yml

[expressroute-indexmd-744v]: ../expressroute/index.yml
