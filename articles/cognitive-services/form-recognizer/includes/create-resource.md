---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: d322aa27c7c9dcf4ad365ec22f8074aeacc85d3b
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502847"
---
Po nadaniu prawa dostępu do używania rozpoznawania formularza otrzymasz Witaj poczty e-mail przy użyciu kilka linków i zasobów. Użyj linku "Portal platformy Azure" w tej wiadomości, aby Otwórz witrynę Azure portal i Utwórz zasób rozpoznawania formularza. W **Utwórz** okienku, podaj następujące informacje:

|    |    |
|--|--|
| **Nazwa** | Opisowa nazwa zasobu bazy danych. Firma Microsoft zaleca używanie nazwę opisową, na przykład *MyNameFormRecognizer*. |
| **Subskrypcja** | Wybierz subskrypcję platformy Azure, którym udzielono dostępu. |
| **Location** | Lokalizacja wystąpienia usługi cognitive Services. Różne lokalizacje może wprowadzić opóźnienie, ale nie mają wpływu na dostępność Twojego zasobu w czasie wykonywania. |
| **Warstwa cenowa** | Koszt zasobu zależy od warstwy cenowej, który wybierzesz i użycie. Aby uzyskać więcej informacji, zobacz omówienie interfejsu API [cennik](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Grupa zasobów** | [Grupy zasobów platformy Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) który będzie zawierał zasobu. Można utworzyć nową grupę lub dodaj je do wcześniej istniejącej grupy. |

> [!IMPORTANT]
> Zwykle podczas tworzenia zasobu usługi cognitive Services w witrynie Azure portal, masz możliwość utworzenia klucz subskrypcji z wieloma usługami (używane w wielu usługach cognitive) lub klucz subskrypcji jednousługowa (używane tylko z określonej usługi cognitive service). Jednak ponieważ rozpoznawania formularza jest wersja zapoznawcza, nie znajduje się w ramach wielu usług subskrypcji i nie można utworzyć subskrypcji jednousługowa, chyba że za pomocą linku podanego w powitalnej wiadomości e-mail.

Po zakończeniu wdrażania zasobu rozpoznawania formularza Znajdź i wybierz go z **wszystkie zasoby** listy w portalu. Następnie wybierz pozycję **klucze** kartę, aby wyświetlić klucze subskrypcji. Żadnego z nich umożliwia uzyskanie dostępu do zasobu. Skopiuj wartość **klucz 1**. Zostanie użyty w następnej sekcji.