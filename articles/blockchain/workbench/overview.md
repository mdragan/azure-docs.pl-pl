---
title: Omówienie usługi Azure Blockchain Workbench
description: Omówienie usługi Azure Blockchain Workbench i jej możliwości.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/20/2019
ms.topic: overview
ms.service: azure-blockchain
ms.reviewer: brendal
manager: femila
ms.openlocfilehash: 16e2497f4e26d16c008161477fd7d573e59e84db
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65955205"
---
# <a name="what-is-azure-blockchain-workbench"></a>Czym jest usługa Azure Blockchain Workbench?

Azure Blockchain Workbench to zbiór usług i funkcji platformy Azure ułatwiających tworzenie i wdrażanie aplikacji łańcucha bloków w celu udostępniania procesów biznesowych i danych innym organizacjom. Usługa Azure Blockchain Workbench udostępnia szkielet infrastruktury do tworzenia aplikacji łańcucha bloków, umożliwiając deweloperom skoncentrowanie się na tworzeniu logiki biznesowej i kontraktów inteligentnych. Ułatwia również tworzenie aplikacji łańcucha bloków dzięki integracji kilku usług i funkcji platformy Azure w celu zautomatyzowania typowych zadań projektowych.

## <a name="create-blockchain-applications"></a>Tworzenie aplikacji łańcucha bloków

Usługa Blockchain Workbench umożliwia definiowanie aplikacji łańcucha bloków przy użyciu konfiguracji oraz pisanie kodu kontraktu inteligentnego. Zamiast tworzenia szkieletu i konfigurowania usług pomocniczych, można od razu rozpocząć tworzenie aplikacji łańcucha bloków i skoncentrować się na definiowaniu umowy i pisaniu logiki biznesowej.

## <a name="manage-applications-and-users"></a>Zarządzanie aplikacjami i użytkownikami

Usługa Azure Blockchain Workbench udostępnia aplikację internetową i interfejsy API REST do zarządzania aplikacjami i użytkownikami łańcucha bloków. Administratorzy usługi Blockchain Workbench mogą zarządzać dostępem do aplikacji i przypisywać użytkowników do ról aplikacji. Użytkownicy usługi Azure AD są automatycznie mapowani na członków w aplikacji.

## <a name="integrate-blockchain-with-applications"></a>Integracja łańcucha bloków z aplikacjami

Interfejsy API REST usługi Blockchain Workbench i interfejsy API oparte na komunikatach umożliwiają integrację z istniejącymi systemami. Interfejsy API zapewniają interfejs umożliwiający zastępowanie lub korzystanie z wielu technologii rejestru rozproszonego, magazynu i ofert baz danych.

Usługa Blockchain Workbench umożliwia przekształcanie komunikatów wysłanych do jej interfejsu API opartego na komunikatach w celu tworzenia transakcji w formacie oczekiwanym przez natywny interfejs API tego łańcucha bloków.  Usługa Workbench umożliwia rejestrowanie transakcji i kierowanie ich do odpowiedniego łańcucha bloków. 

Usługa Workbench automatycznie dostarcza zdarzenia do usług Service Bus i Event Grid w celu wysyłania wiadomości do użytkowników podrzędnych. Deweloperzy mogą zintegrować usługę z jednym z tych systemów obsługi wiadomości, aby realizować transakcje i przyglądać się wynikom.

## <a name="deploy-a-blockchain-network"></a>Wdrażanie sieci łańcucha bloków

Usługa Azure Blockchain Workbench, jako wstępnie skonfigurowane rozwiązanie z szablonem rozwiązania Azure Resource Manager, upraszcza konfigurację sieci łańcucha bloków konsorcjum. Ten szablon obejmuje uproszczone wdrażanie, które powoduje wdrożenie wszystkich składników potrzebnych do uruchomienia konsorcjum. Obecnie usługa Blockchain Workbench obsługuje platformę Ethereum.

## <a name="use-active-directory"></a>Użyj usługi Active Directory

W przypadku istniejących protokołów łańcucha bloków tożsamości łańcucha bloków są reprezentowane jako adres w sieci. W usłudze Azure Blockchain Workbench nie trzeba troszczyć się o tożsamość łańcucha bloków, ponieważ jest ona skojarzona z tożsamością usługi Active Directory, co ułatwia tworzenie aplikacji przedsiębiorstwa za pomocą tożsamości usługi Active Directory.

## <a name="synchronize-on-chain-data-with-off-chain-storage"></a>Synchronizowanie danych łańcucha z magazynem poza łańcuchem

Usługa Azure Blockchain Workbench ułatwia analizowanie zdarzeń i danych łańcucha bloków dzięki automatycznemu synchronizowaniu danych łańcucha bloków z magazynem poza łańcuchem. Zamiast wyodrębniać dane bezpośrednio z łańcucha bloków, można wysyłać zapytania do systemów baz danych poza łańcuchem, takich jak SQL Server. Doświadczenie łańcucha bloków nie jest wymagana dla użytkowników końcowych, którzy robią zadania analizy danych.

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Architektura usługi Azure Blockchain Workbench](architecture.md)
