---
title: Informacje o wersji bazy danych usługi Azure SQL | Dokumentacja firmy Microsoft
description: Dowiedz się więcej o nowe funkcje i ulepszenia w usłudze Azure SQL Database, a w dokumentacji usługi Azure SQL Database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: sstein
ms.openlocfilehash: 04c19c9a8603fee46914eb94b50c4f97e34e9c42
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67070211"
---
# <a name="sql-database-release-notes"></a>Informacje o wersji bazy danych SQL

Ten artykuł zawiera listę nowych funkcji i ulepszeń, w ramach usługi SQL Database i w dokumentacji usługi SQL Database. Ulepszeń usługi SQL Database, zobacz też [aktualizacji usługi SQL Database](https://azure.microsoft.com/updates/?product=sql-database). Ulepszenia do innych usług platformy Azure, można zobaczyć [aktualizowaniem usługi](https://azure.microsoft.com/updates).

## <a name="features-in-public-preview"></a>Funkcje w wersji zapoznawczej

| Cecha | Szczegóły |
| ---| --- |
| Odzyskiwanie bazy danych jej jako przyspieszonej za pomocą pojedynczych baz danych i pul elastycznych | Aby uzyskać informacje, zobacz [przyspieszone odzyskiwanie bazy danych](sql-database-accelerated-database-recovery.md).|
|Przybliżona liczba unikatowych|Aby uzyskać informacje, zobacz [przybliżony Count Distinct](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#approximate-query-processing).|
|Tryb partii na magazynu wierszy (równy podanemu poziomowi zgodności 150)|Aby uzyskać informacje, zobacz [tryb usługi Batch na magazynu wierszy](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#batch-mode-on-rowstore).|
| Odnajdowanie i klasyfikacja danych  |Aby uzyskać informacje, zobacz [usługi Azure SQL Database i SQL Data Warehouse odnajdywanie i klasyfikacja danych](sql-database-data-discovery-and-classification.md).|
| Zadania elastycznych baz danych | Aby uzyskać informacje, zobacz [tworzenie, konfigurowanie i zarządzanie nimi zadań elastycznych](elastic-jobs-overview.md). |
| Zapytania elastyczne | Aby uzyskać informacje, zobacz [omówienie zapytania elastycznego](sql-database-elastic-query-overview.md). |
| Transakcje elastyczne | [Transakcje rozproszone w bazach danych w chmurze](sql-database-elastic-transactions-overview.md). |
|Zwrotna przydziału pamięci (w trybie wiersza) (równy podanemu poziomowi zgodności 150)|Aby uzyskać informacje, zobacz [zwrotna przydziału pamięci (w trybie wiersza)](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback).|
| Edytor zapytań w witrynie Azure portal |Aby uzyskać informacje, zobacz [używać edytora zapytań SQL w witrynie Azure portal do nawiązywania połączeń i wykonywanie zapytań dotyczących danych](sql-database-connect-query-portal.md).|
| R services / usługi machine learning za pomocą pojedynczych baz danych i pul elastycznych |Aby uzyskać informacje, zobacz [usługi Machine Learning w usłudze Azure SQL Database](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#machine-learning-services-in-azure-sql-database).|
| Utwórz ponownie usuwanej bazy danych z wystąpienia zarządzanego |Aby uzyskać informacje, zobacz [ponownie utworzyć usunięte bazy danych w wystąpieniu zarządzanym usługi SQL Azure](https://medium.com/azure-sqldb-managed-instance/re-create-dropped-databases-in-azure-sql-managed-instance-dc369ed60266).|
| Replikacja za pomocą wystąpienia zarządzanego |Aby uzyskać informacje, zobacz [Konfigurowanie replikacji w bazie danych wystąpienia zarządzanego usługi Azure SQL Database](replication-with-sql-database-managed-instance.md).|
| Warstwa bezserwerowych usług obliczeniowych | Aby uzyskać informacje, zobacz [bez użycia serwera bazy danych SQL Database (wersja zapoznawcza)](sql-database-serverless.md).|
|SQL Analytics|Aby uzyskać informacje, zobacz [usługi Azure SQL Analytics](../azure-monitor/insights/azure-sql.md).|
|Tabela zmiennych odroczone kompilacji (na poziomie zgodności 150)|Aby uzyskać informacje, zobacz [kompilacji odroczone w zmiennej tabeli](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#table-variable-deferred-compilation).|
| Wykrywanie zagrożeń za pomocą wystąpienia zarządzanego |Aby uzyskać informacje, zobacz [Konfigurowanie wykrywania zagrożeń w usłudze Azure SQL Database, wystąpienia zarządzanego](sql-database-managed-instance-threat-detection.md).|
| Przezroczyste szyfrowanie danych (TDE) przy użyciu Bring Your Own Key (BYOK) z wystąpienia zarządzanego |Aby uzyskać informacje, zobacz [Azure przezroczyste szyfrowanie danych SQL za pomocą kluczy zarządzanych przez klienta w usłudze Azure Key Vault: Bring Your Own Key obsługi](transparent-data-encryption-byok-azure-sql.md).|
| &nbsp; |

## <a name="june-2019"></a>2019 czerwca

### <a name="service-improvements"></a>Udoskonalenia usługi

| Udoskonalenia usługi | Szczegóły |
| --- | --- |
| 4 rdzenie wirtualne wystąpienia zarządzanego na generacja sprzętu do 5. generacji| Aby uzyskać więcej informacji, zobacz https://aka.ms/four-cores-sql-mi-update. |
| Wystąpienie zarządzane — Obsługa subskrypcji przy użyciu platformy Azure, miesięczne środki dla subskrybentów programu Visual Studio | Aby uzyskać więcej informacji, zobacz https://aka.ms/sql-mi-visual-studio-subscribers. |
|Większe limity regionalne dla wystąpienia zarządzanego| Aby uzyskać więcej informacji, zobacz [ograniczenia zasobów regionalnych](sql-database-managed-instance-resource-limits.md#regional-resource-limitations).
| &nbsp; |


## <a name="may-2019"></a>Maja 2019 r

### <a name="service-improvements"></a>Udoskonalenia usługi

| Udoskonalenia usługi | Szczegóły |
| --- | --- |
|Warstwy usługi w Hiperskali, teraz ogólnie dostępna| Aby uzyskać więcej informacji, zobacz [warstwy usług w Hiperskali dla maksymalnie 100 TB](sql-database-service-tier-hyperscale.md) i [Pobierz o wysokiej wydajności, skalowanie na potrzeby obciążeń bazy danych Azure przy użyciu Hiperskali](https://azure.microsoft.com/blog/get-high-performance-scaling-for-your-azure-database-workloads-with-hyperscale/).|
|Warstwa bezserwerowe środowisko obliczeniowe, wydana w publicznej wersji zapoznawczej | Aby uzyskać więcej informacji, zobacz [bez użycia serwera bazy danych SQL Database (wersja zapoznawcza)](sql-database-serverless.md).|
|Obsługa strefy czasowej dla wystąpienia zarządzanego, teraz ogólnie dostępna |Aby uzyskać więcej informacji, zobacz [strefę czasową w wystąpieniu zarządzanym usługi Azure SQL Database](sql-database-managed-instance-timezone.md).|
|Sortowania wystąpienia za pomocą wystąpienia zarządzanego, teraz ogólnie dostępna |Aby uzyskać informacje, zobacz [Użyj programu PowerShell przy użyciu szablonu usługi Azure Resource Manager, aby utworzyć wystąpienie zarządzane usługi Azure SQL Database](./scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md).|
| Dla baz danych przy użyciu modelu zakupu opartego na rdzeniach wirtualnych zwiększona stawki dziennika transakcji oraz docelowy operacji We/Wy| Aby uzyskać więcej informacji, zobacz [zasobów limitów dla pojedynczych baz danych przy użyciu modelu zakupu opartego na rdzeniach wirtualnych](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases) i [zasobów limitów dla pojedynczych baz danych przy użyciu modelu zakupu opartego na jednostkach DTU](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-single-databases).
| &nbsp; |

### <a name="documentation-improvements"></a>Udoskonalenia dokumentacji

| Udoskonalenia dokumentacji | Szczegóły |
| --- | --- |
| Dokumentacja warstwy z usługi w Hiperskali zaktualizowane w wersji ogólnie dostępnej wersji| Aby uzyskać więcej informacji, zobacz [warstwy usług w Hiperskali dla maksymalnie 100 TB](sql-database-service-tier-hyperscale.md).|
|Dokumentacja warstwy bezserwerowe środowisko obliczeniowe wydana publiczna wersja zapoznawcza| Aby uzyskać więcej informacji, zobacz [bez użycia serwera bazy danych SQL Database (wersja zapoznawcza)](sql-database-serverless.md).|
| &nbsp; |

## <a name="april-2019"></a>2019 kwietnia

### <a name="service-improvements"></a>Udoskonalenia usługi

| Udoskonalenia usługi | Szczegóły |
| --- | --- |
| Publiczne punkty końcowe dla wystąpienia zarządzanego w publicznej wersji zapoznawczej| Aby uzyskać więcej informacji, zobacz [przy użyciu usługi Azure SQL Database managed wystąpienie bezpiecznie z publicznym punktem końcowym](sql-database-managed-instance-public-endpoint-securely.md).|
| Obsługa strefy czasowej dla zarządzanego wystąpienia w publicznej wersji zapoznawczej| Aby uzyskać więcej informacji, zobacz [strefę czasową w wystąpieniu zarządzanym usługi Azure SQL Database](sql-database-managed-instance-timezone.md).|
| Zabezpieczanie bazy danych SQL Azure z zarządzanych tożsamości w drugiej publicznej wersji zapoznawczej wersji| Zobacz [zabezpieczanie Azure SQL Database za pomocą tożsamości zarządzanej właśnie stało się łatwiejsze](https://azure.microsoft.com/blog/securing-azure-sql-databases-with-managed-identities-just-got-easier/).|
| &nbsp; |

### <a name="documentation-improvements"></a>Udoskonalenia dokumentacji

| Udoskonalenia dokumentacji | Szczegóły |
| --- | --- |
| Publiczne punkty końcowe dla wystąpienia zarządzanego w publicznej wersji zapoznawczej| Aby uzyskać więcej informacji, zobacz [przy użyciu usługi Azure SQL Database managed wystąpienie bezpiecznie z publicznym punktem końcowym](sql-database-managed-instance-public-endpoint-securely.md).|
| Obsługa strefy czasowej dla zarządzanego wystąpienia w publicznej wersji zapoznawczej| Aby uzyskać więcej informacji, zobacz [strefę czasową w wystąpieniu zarządzanym usługi Azure SQL Database](sql-database-managed-instance-timezone.md). |
| Zarządzanie zasobami w usłudze Azure SQL Database | Aby uzyskać więcej informacji, zobacz [nadzór nad zasobami w usłudze Azure SQL Database](https://azure.microsoft.com/blog/resource-governance-in-azure-sql-database/). || &nbsp; |

## <a name="march-2019"></a>Marca 2019 r

### <a name="service-improvements"></a>Udoskonalenia usługi

| Udoskonalenia usługi | Szczegóły |
| --- | --- |
| Obsługa skalowalnego w poziomie odczytu dla usługi Azure SQL Database teraz ogólnie dostępna | Aby uzyskać więcej informacji, zobacz [odczytu skalowalnego w poziomie](sql-database-read-scale-out.md).|
| &nbsp; |

### <a name="documentation-improvements"></a>Udoskonalenia dokumentacji

| Udoskonalenia dokumentacji | Szczegóły |
| --- | --- |
| Dodano dziennika limity dla pojedynczych baz danych|Aby uzyskać więcej informacji, zobacz [pojedynczy limity zasobów na rdzeniach wirtualnych bazy danych](sql-database-vcore-resource-limits-single-databases.md).|
| Dodano dziennika limity dla pul elastycznych i baz danych w puli|Aby uzyskać więcej informacji, zobacz [vCore zasobów limity pul elastycznych](sql-database-vcore-resource-limits-elastic-pools.md).|
| Dodano nadzoru współczynnik dziennika transakcji| Dodano nową zawartość dotyczącą [nadzoru współczynnik dziennika transakcji](sql-database-resource-limits-database-server.md#transaction-log-rate-governance).|
| Zaktualizowano przykłady programu PowerShell dla pojedynczych baz danych i pul elastycznych, aby użyć modułu az.sql | Aby uzyskać więcej informacji, zobacz [przykłady programu PowerShell dla pojedynczych baz danych i pul elastycznych](sql-database-powershell-samples.md#single-database-and-elastic-pools).|
| &nbsp; |

## <a name="february-2019"></a>Lutego 2019 r

### <a name="service-improvements"></a>Udoskonalenia usługi

| Udoskonalenia usługi | Szczegóły |
| --- | --- |
|Tworzenie z możliwością wznowienia indeksu w trybie online jest teraz ogólnie dostępna| Aby uzyskać więcej informacji, zobacz [Create Index](https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql).|
|Wystąpienie zarządzane Obsługa tabel tras, ulepszone| Aby uzyskać więcej informacji, zobacz [wymagania dotyczące sieciowej](sql-database-managed-instance-connectivity-architecture.md#network-requirements).|
|Zmień nazwę bazy danych obsługiwane w wystąpieniu zarządzanym | Aby uzyskać więcej informacji, zobacz [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) i [sp_rename](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-rename-transact-sql) składni.|
|Baza danych SQL jako źródła danych referencyjnych dla usługi Stream Analytics. | Aby uzyskać więcej informacji, zobacz [usługi Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) i [usługi Azure Stream Analytics obsługuje teraz usługi Azure SQL Database jako wejściowych danych referencyjnych](https://azure.microsoft.com/blog/azure-stream-analytics-now-supports-azure-sql-database-as-reference-data-input/).|
|Data Migration Assistant dodaje obsługę dla wystąpienia zarządzanego. |Aby uzyskać więcej informacji, zobacz [What's new in DMA](https://docs.microsoft.com/sql/dma/dma-whatsnew).|
|SQL Server Migration Assistant alokowanej oceny gotowości docelowego dla wystąpienia zarządzanego. | Aby uzyskać więcej informacji, zobacz [Asystenta migracji programu SQL Server](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant).
|Usługa migracji danych obsługuje migrację z usług RDS Amazon do wystąpienia zarządzanego | Aby uzyskać więcej informacji, zobacz [Samouczek: Migrowanie usług pulpitu zdalnego programu SQL Server do usługi Azure SQL Database lub wystąpienia zarządzanego usługi Azure SQL Database i online przy użyciu usługi DMS](../dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online.md).|
| &nbsp; |

### <a name="documentation-improvements"></a>Udoskonalenia dokumentacji

| Udoskonalenia dokumentacji | Szczegóły |
| --- | --- |
|Dodawanie zarządzanych wyjaśnienia opcji wdrożenia wystąpienia|Zaktualizowano wiele artykułów, aby wyjaśnić, ma zastosowanie do pojedynczej bazy danych, pul elastycznych i opcji wdrożenia wystąpienia zarządzanego. |
|Zaktualizowano bazę danych tempdb rozmiary modelu zakupu opartego na jednostkach DTU | Aby uzyskać więcej informacji, zobacz [bazy danych Tempdb w bazie danych SQL](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).|
|Zaktualizowano importowanie i eksportowanie z pliku bacpac do obsługi wystąpienia zarządzanego| Aby uzyskać więcej informacji, zobacz [Importuj z pliku BACPAC](sql-database-import.md) i [wyeksportować do pliku BACPAC](sql-database-export.md). |
| &nbsp; |

## <a name="january-2019"></a>Styczeń 2019

### <a name="service-improvements"></a>Udoskonalenia usługi

| Udoskonalenia usługi | Szczegóły |
| --- | --- |
| Opcje dodatkowe stopień szczegółowości za zasoby obliczeniowe | Ogólnego przeznaczenia i krytyczne dla działania usługi warstwy [pojedyncze bazy danych](sql-database-vcore-resource-limits-single-databases.md) i [pul elastycznych](sql-database-vcore-resource-limits-elastic-pools.md) mają teraz więcej opcji obliczeniowych szczegółowych.|
| Wyświetlanie rekordów inspekcji dla wystąpienia zarządzanego w witrynie Azure portal | Wyświetlanie [rekordy dla wystąpienia zarządzanego inspekcji](sql-database-managed-instance-auditing.md) w witrynie Azure portal jest teraz obsługiwane.|
| Funkcja wykrywania zagrożeń wcześniejszym zmieniona na zaawansowane zabezpieczenia danych | Funkcja wykrywania zagrożeń wcześniejszym zmieniona na [zaawansowanych danych](sql-advanced-threat-protection.md) dla pojedynczych baz danych, pul elastycznych i wystąpienia zarządzanego. |
| &nbsp; |

### <a name="documentation-improvements"></a>Udoskonalenia dokumentacji

| Udoskonalenia dokumentacji | Szczegóły |
| --- | --- |
| Wystąpienia zarządzanego i replikacji transakcyjnej | Dodano artykuł na temat przy użyciu [replikacji transakcyjnej przy użyciu zarządzanych wystąpień przez](replication-with-sql-database-managed-instance.md). |
| Dodano usługi Azure AD z samouczkiem wystąpienia zarządzanego | To [usługi Azure AD za pomocą wystąpienia zarządzanego](sql-database-managed-instance-aad-security-tutorial.md) samouczku przedstawiono, które trzeba skonfigurować i przetestować zarządzane wystąpienia zabezpieczeń za pomocą logowania do usługi Azure AD. |
| Zaktualizowano zawartość dla zadań usługi automation przy użyciu skryptów języka Transact-SQL | Zaktualizowane i wyjaśniono zawartości przy użyciu [zadań usługi automation przy użyciu skryptów języka Transact-SQL](sql-database-job-automation-overview.md) dla pojedynczych baz danych, pul elastycznych i wystąpienia zarządzanego. |
| Zawartość dotycząca zabezpieczeń dla zarządzanego wystąpienia zaktualizowane | Zaktualizowane i wyjaśniono zawartość [model zabezpieczeń dla zarządzanego wystąpienia](sql-database-security-overview.md)i odróżniające się przy użyciu modelu zabezpieczeń dla pojedynczych baz danych i pul elastycznych. |
| Odświeżane wszystkie przewodników Szybki Start i samouczki | Wszystkie przewodników Szybki Start i samouczków w [dokumentacji](https://docs.microsoft.com/azure/sql-database) zostały zaktualizowane i odświeżyć, aby dopasować zmiany w witrynie Azure portal. |
| Przewodniki Przegląd dodano szybkiego startu | Dodano omówienie Przewodnik Szybki Start dla [pojedyncze bazy danych](sql-database-quickstart-guide.md) i [wystąpienia zarządzane](sql-database-managed-instance-quickstart-guide.md). |
| Dodano bazę danych SQL słownik pojęć | To [słownik terminów](sql-database-glossary-terms.md) artykuł zawiera listę ostatecznych terminów bazy danych SQL i łącza do głównej strony koncepcyjnej wyjaśnia pojęcie w kontekście. |
| &nbsp; |

## <a name="contribute-to-content-improvement"></a>Przyczynia się do poprawy jakości zawartości

Zestaw dokumentacji usługi Azure SQL jest "open source". Praca w środowisku otwartym oferuje następujące korzyści:

- Planowanie repozytoriów typu open source w środowisku otwartym, aby uzyskać opinie na temat najbardziej potrzebnych dokumentów, jakie.
- "Open source" Przeglądanie repozytoriów w środowisku otwartym w celu publikowania najbardziej użytecznej zawartości w naszej pierwszej wersji.
- Aktualizowanie repozytoriów typu open source w środowisku otwartym w celu ułatwienia stałego udoskonalania zawartości.

Aby współtworzyć zawartość dokumentacji usługi Azure SQL Database, zobacz [przewodnik współautora Microsoft Docs — omówienie](https://docs.microsoft.com/contribute/). Środowisko użytkownika w [docs.microsoft.com](https://docs.microsoft.com/) integruje [GitHub](https://github.com/) przepływy pracy, bezpośrednio po to, aby jeszcze bardziej ułatwiał. Rozpocznij od [edytowania przeglądanego dokumentu](https://docs.microsoft.com/contribute/#quick-edits-to-existing-documents). Lub Pomóż [recenzować nowe tematy](https://docs.microsoft.com/contribute/#review-open-prs), lub [tworzenie problemów z jakością](https://docs.microsoft.com/contribute/#create-quality-issues).
