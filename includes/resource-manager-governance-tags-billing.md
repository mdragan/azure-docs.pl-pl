---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: e7fcb72cecbfad2de80b844ed5281267d5e4c0c3
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67183163"
---
Jeśli do zasobów zostaną zastosowane tagi, za pomocą tych tagów można wyświetlić koszty zasobów. Aby w analizie kosztów było wyświetlane najnowsze użycie, musi upłynąć trochę czasu, dlatego koszty mogą nie być jeszcze widoczne. Gdy koszty będą już dostępne, można wyświetlić koszty zasobów między grupami zasobów w ramach subskrypcji. Aby móc zobaczyć koszty, użytkownicy muszą mieć [dostęp do informacji o rozliczeniach na poziomie subskrypcji](../articles/billing/billing-manage-access.md).

Aby w portalu wyświetlić koszty według tagów, wybierz subskrypcję, a następnie wybierz pozycję **Analiza kosztów**.

![Analiza kosztów](./media/resource-manager-governance-tags-billing/select-cost-analysis.png)

Następnie ustaw filtrowanie według wartości tagu i wybierz pozycję **Zastosuj**.

![Wyświetlanie kosztów według tagów](./media/resource-manager-governance-tags-billing/view-costs-by-tag.png)

Można również użyć [interfejsów API rozliczeń platformy Azure](../articles/billing/billing-usage-rate-card-overview.md), aby programowo wyświetlić koszty.
