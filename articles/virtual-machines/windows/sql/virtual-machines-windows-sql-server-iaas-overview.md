---
title: Omówienie programu SQL Server na maszynach wirtualnych platformy Azure z systemem Windows | Microsoft Docs
description: Dowiedz się, jak uruchomić pełne wersje programu SQL Server na maszynach wirtualnych platformy Azure.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.assetid: c505089e-6bbf-4d14-af0e-dd39a1872767
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/12/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 99c4f0f99af61196cf1a12f2f68a7d10d8b2e6c7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61477165"
---
# <a name="what-is-sql-server-on-azure-virtual-machines-windows"></a>Co to jest program SQL Server na maszynach wirtualnych platformy Azure? (Windows)

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-overview.md)
> * [Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md)

[Program SQL Server na maszynach wirtualnych platformy Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/) pozwala na używanie pełnych wersji programu SQL Server w chmurze bez konieczności zarządzania sprzętem lokalnym. Maszyny wirtualne z programem SQL Server upraszczają również określanie kosztów licencjonowania w przypadku płatności zgodnie z rzeczywistym użyciem.

Maszyny wirtualne platformy Azure działają w wielu różnych [regionach geograficznych](https://azure.microsoft.com/regions/) na świecie. Oferują one także szereg różnych [rozmiarów maszyn](../sizes.md). Galeria obrazów maszyn wirtualnych umożliwia utworzenie maszyny wirtualnej z programem SQL Server we właściwej wersji i edycji oraz z odpowiednim systemem operacyjnym. Dzięki temu maszyny wirtualne to dobry wybór w przypadku wielu różnych obciążeń programu SQL Server.

## <a name="automated-updates"></a>Aktualizacje automatyczne

Maszyny wirtualne platformy Azure z programem SQL Server umożliwiają korzystanie z funkcji [Automatyczne stosowanie poprawek](virtual-machines-windows-sql-automated-patching.md) w celu zaplanowania okna obsługi, w którym mają być automatycznie instalowane ważne aktualizacje systemu Windows i programu SQL Server.

## <a name="automated-backups"></a>Automatyczne kopie zapasowe

Maszyny wirtualne Azure z programem SQL Server umożliwiają korzystanie z funkcji [Automatyczne kopie zapasowe](virtual-machines-windows-sql-automated-backup-v2.md), która regularnie tworzy kopie zapasowe bazy danych w usłudze Blob Storage. Tej techniki można również używać ręcznie. Aby uzyskać więcej informacji, zobacz [Use Azure Storage for SQL Server Backup and Restore](virtual-machines-windows-use-storage-sql-server-backup-restore.md) (Używanie usługi Azure Storage do tworzenia kopii zapasowych programu SQL Server i ich przywracania).

## <a name="high-availability"></a>Wysoka dostępność

Jeśli potrzebujesz wysokiej dostępności, rozważ skonfigurowanie grup dostępności programu SQL Server. Obejmuje to wiele maszyn wirtualnych Azure z programem SQL Server w sieci wirtualnej. Rozwiązanie o wysokiej dostępności możesz skonfigurować ręcznie lub zastosować jeden z szablonów w witrynie Azure Portal, aby zapewnić automatyczną konfigurację. Omówienie wszystkich opcji wysokiej dostępności można znaleźć w temacie [Wysoka dostępność i odzyskiwanie awaryjne na potrzeby programu SQL Server na maszynach wirtualnych platformy Azure](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="performance"></a>Wydajność

Maszyny wirtualne platformy Azure mają różne rozmiary dopasowane do różnych wymagań dotyczących obciążenia. Maszyny wirtualne SQL zapewniają również automatyczną konfigurację usługi Storage zoptymalizowaną pod kątem wymagań dotyczących wydajności. Aby uzyskać więcej informacji na temat konfigurowania usługi Storage na potrzeby maszyn wirtualnych SQL, zobacz [Konfiguracja usługi Storage dla maszyn wirtualnych programu SQL Server](virtual-machines-windows-sql-server-storage-configuration.md). Aby uzyskać informacje dotyczące dostrajania wydajności, zobacz [Najlepsze rozwiązania w zakresie wydajności dla programu SQL Server w usłudze Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

## <a name="get-started-with-sql-vms"></a>Wprowadzenie do maszyn wirtualnych SQL

Aby rozpocząć, wybierz obraz maszyny wirtualnej z programem SQL Server w wymaganej wersji i edycji oraz z odpowiednim systemem operacyjnym. Poniższe sekcje zawierają bezpośrednie linki do obrazów maszyn wirtualnych programu SQL Server w galerii witryny Azure Portal.

> [!TIP]
> Aby uzyskać więcej informacji umożliwiających zrozumienie cennika obrazów SQL, zobacz [Pricing guidance for SQL Server Azure VMs (Wskazówki dotyczące cen maszyn wirtualnych platformy Azure z programem SQL Server)](virtual-machines-windows-sql-server-pricing-guidance.md). 

### <a id="payasyougo"></a> Płatność zgodnie z rzeczywistym użyciem
Poniższa tabela zawiera macierz obrazów z programem SQL Server korzystających z modelu płatności zgodnie z rzeczywistym użyciem.

| Version | System operacyjny | Wersja |
| --- | --- | --- |
| **SQL Server 2017** |Windows Server 2016 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016) |
| **SQL Server 2016 SP2** |Windows Server 2016 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016SP2EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016SP2StandardWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016SP2WebWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP2ExpressWindowsServer2016), [Developer](https://portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP2DeveloperWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2ExpressWindowsServer2012R2) |
| **SQL Server 2012 SP4** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP4EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP4StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP4WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP4ExpressWindowsServer2012R2) |
| **SQL Server 2008 R2 SP3** |Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2008R2) |

Aby wyświetlić dostępne obrazy maszyn wirtualnych z programem SQL Server dla systemu Linux, zobacz [Overview of SQL Server on Azure Virtual Machines (Linux) (Omówienie programu SQL Server na maszynach wirtualnych platformy Azure — system Linux)](../../linux/sql/sql-server-linux-virtual-machines-overview.md).

> [!NOTE]
> Teraz można zmienić model licencjonowania maszyny wirtualnej z programem SQL Server opłacanej za użycie na własną licencję. Aby uzyskać więcej informacji, zobacz [Jak zmienić model licencjonowania dla maszyny wirtualnej SQL](virtual-machines-windows-sql-ahb.md). 

### <a id="BYOL"></a> Bring your own license
Możesz również skorzystać z modelu dostarczania własnej licencji (Bring Your Own License, BYOL). W tym scenariuszu płacisz wyłącznie za maszynę wirtualną i nie ponosisz żadnych dodatkowych kosztów licencjonowania programu SQL Server.  Użycie własnej licencji może w dłuższym okresie przynieść oszczędności w przypadku ciągłych obciążeń produkcyjnych. Wymagania dotyczące korzystania z tej opcji zawiera artykuł [Pricing guidance for SQL Server Azure VMs (Wskazówki dotyczące cen maszyn wirtualnych platformy Azure z programem SQL Server)](virtual-machines-windows-sql-server-pricing-guidance.md#byol).

Aby dostarczyć własną licencję, możesz przekonwertować istniejącą maszynę wirtualną SQL opłacaną za użycie lub wdrożyć obraz **{BYOL}** poprzedzony prefiksem. Aby uzyskać więcej informacji na temat przełączania modelu licencjonowania między płatnością za użycie i modelem BYOL, zobacz [Jak zmienić model licencjonowania dla maszyny wirtualnej SQL](virtual-machines-windows-sql-ahb.md). 

| Version | System operacyjny | Wersja |
| --- | --- | --- |
| **SQL Server 2017** |Windows Server 2016 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017EnterpriseWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017StandardonWindowsServer2016) |
| **SQL Server 2016 SP2** |Windows Server 2016 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP2EnterpriseWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP2StandardWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2StandardWindowsServer2012R2) |
| **SQL Server 2012 SP4** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP4EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP4StandardWindowsServer2012R2) |

Jest możliwe wdrożenie starszego obrazu programu SQL Server, który nie jest dostępny w witrynie Azure Portal przy użyciu programu PowerShell. Aby wyświetlić wszystkie dostępne obrazy przy użyciu programu Powershell, użyj następującego polecenia:

  ```powershell
  Get-AzVMImageOffer -Location $Location -Publisher 'MicrosoftSQLServer'
  ```

Aby uzyskać więcej informacji o wdrażaniu maszyn wirtualnych programu SQL Server przy użyciu programu PowerShell, zobacz [How to provision SQL Server virtual machines with Azure PowerShell (Jak aprowizować maszyny wirtualne programu SQL Server przy użyciu programu Azure PowerShell)](virtual-machines-windows-ps-sql-create.md).


### <a name="connect-to-the-vm"></a>Łączenie z maszyną wirtualną
Po utworzeniu maszyny wirtualnej z programem SQL Server nawiąż z nią połączenie z poziomu aplikacji lub narzędzia, np. SQL Server Management Studio (SSMS). Instrukcje można znaleźć w artykule [Połączenie z maszyną wirtualną z programem SQL Server na platformie Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migrowanie danych
Jeśli masz istniejącą bazę danych, najprawdopodobniej będziesz ją przenosić do nowo aprowizowanej maszyny wirtualnej z programem SQL. Aby uzyskać listę opcji migracji i wskazówki, zobacz [Migrating a Database to SQL Server on an Azure VM](virtual-machines-windows-migrate-sql.md) (Migrowanie bazy danych do programu SQL Server na maszynie wirtualnej portalu Azure).

## <a id="lifecycle"></a> Zasady odświeżania obrazu maszyny wirtualnej SQL
Platforma Azure utrzymuje tylko jeden obraz maszyny wirtualnej dla każdego połączenia obsługiwanego systemu operacyjnego, wersji i wydania. Oznacza to, że z czasem obrazy będą odświeżane, a ich starsze wersje usuwane. Aby uzyskać więcej informacji, zobacz sekcję **Obrazy** w temacie [Często zadawane pytania dotyczące maszyn wirtualnych programu SQL Server](virtual-machines-windows-sql-server-iaas-faq.md#images).

## <a name="customer-experience-improvement-program-ceip"></a>Program poprawy jakości obsługi klienta
Program poprawy jakości obsługi klienta jest domyślnie włączony. Wysyła on okresowo raporty do firmy Microsoft w celu umożliwienia doskonalenia programu SQL Server. Program poprawy jakości obsługi klienta nie wymaga przeprowadzenia żadnych czynności związanych z zarządzaniem. Wyjątek stanowi sytuacja, gdy ma on zostać wyłączony po ukończeniu aprowizacji. Program poprawy jakości obsługi klienta możesz dostosować lub wyłączyć, łącząc się z maszyną wirtualną za pomocą pulpitu zdalnego. Następnie uruchom narzędzie **SQL Server Error and Usage Reporting**. Postępuj zgodnie z instrukcjami, aby wyłączyć raportowanie. Aby uzyskać więcej informacji na temat zbierania danych, zobacz [Zasady zachowania poufności informacji programu SQL Server](https://docs.microsoft.com/sql/getting-started/microsoft-sql-server-privacy-statement).

## <a name="related-products-and-services"></a>Powiązane produkty i usługi
### <a name="windows-virtual-machines"></a>Maszyny wirtualne z systemem Windows
* [Omówienie maszyn wirtualnych](../overview.md)

### <a name="storage"></a>Magazyn
* [Wprowadzenie do usługi Microsoft Azure Storage](../../../storage/common/storage-introduction.md)

### <a name="networking"></a>Networking
* [Omówienie usługi Virtual Network](../../../virtual-network/virtual-networks-overview.md)
* [Adresy IP na platformie Azure](../../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Tworzenie w pełni kwalifikowanej nazwy domeny w witrynie Azure Portal](../portal-create-fqdn.md)

### <a name="sql"></a>SQL
* [SQL Server documentation (Dokumentacja programu SQL Server)](https://docs.microsoft.com/sql/index)
* [Porównanie usługi Azure SQL Database](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md)

## <a name="next-steps"></a>Kolejne kroki

Wprowadzenie do programu SQL Server na maszynach wirtualnych platformy Azure:

* [Tworzenie maszyny wirtualnej z programem SQL Server w witrynie Azure Portal](quickstart-sql-vm-create-portal.md)

Uzyskaj odpowiedzi na często zadawane pytania dotyczące maszyn wirtualnych SQL:

* [Często zadawane pytania dotyczące programu SQL Server w usłudze Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-faq.md)
