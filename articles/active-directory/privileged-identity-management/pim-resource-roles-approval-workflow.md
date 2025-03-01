---
title: Zatwierdź lub Odrzuć żądania dla ról zasobów platformy Azure w usłudze PIM — usługi Azure Active Directory | Dokumentacja firmy Microsoft
description: Dowiedz się, jak akceptują lub odrzucają żądania dla ról zasobów platformy Azure w usłudze Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f645b7077ef43dc7eb4d70261b6b601b5e4af1b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60288477"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-pim"></a>Zatwierdź lub Odrzuć żądania dla ról zasobów platformy Azure w usłudze PIM

Za pomocą usługi Azure Active Directory (Azure AD) Privileged Identity Management (PIM), można skonfigurować role, aby wymagały zatwierdzenia aktywacji i wybierz jednego lub wielu użytkowników lub grup w ramach delegowanego osób zatwierdzających. Osoby zatwierdzające delegowanego mają 24 godziny do żądań zatwierdzenia. Jeśli żądanie nie zostanie zatwierdzone w ciągu 24 godzin, w uprawniony użytkownik musi ponownie przesłać nowe żądanie. Nie konfiguruje się w danym przedziale czasu zatwierdzania 24-godzinnym.

Wykonaj kroki opisane w tym artykule, akceptują lub odrzucają żądania dla ról zasobów platformy Azure.

## <a name="view-pending-requests"></a>Wyświetlanie oczekujących żądań

Jako delegowany osoby zatwierdzającej otrzymasz wiadomość e-mail z powiadomieniem po żądanie roli zasobów platformy Azure oczekują na zatwierdzenie. Możesz wyświetlić te oczekujących żądań w usłudze PIM.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

1. Otwórz **usługi Azure AD Privileged Identity Management**.

1. Kliknij przycisk **zatwierdzanie żądań**.

    ![Zasoby platformy Azure — zatwierdzanie żądań](./media/pim-resource-roles-approval-workflow/resources-approve-requests.png)

    W **żądania aktywacji roli** sekcji zobaczysz listę żądań oczekujących na zatwierdzenie.

## <a name="approve-requests"></a>Zatwierdzanie żądań

1. Znajdź i kliknij żądanie, które chcesz zatwierdzić. Zostanie wyświetlone okienko zatwierdzenia.

    ![Zatwierdzanie żądań okienko](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. W **uzasadnienie** wpisz przyczynę.

1. Kliknij przycisk **zatwierdzić**.

    Pojawi się powiadomienie o zatwierdzenie.

    ![Zatwierdzenie powiadomienia](./media/pim-resource-roles-approval-workflow/resources-approve-notification.png)

## <a name="deny-requests"></a>Odmów żądań

1. Znajdź i kliknij żądanie, które chcesz odmówić. Zostanie wyświetlone okienko zatwierdzenia.

    ![Zatwierdzanie żądań okienko](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. W **uzasadnienie** wpisz przyczynę.

1. Kliknij przycisk **Odmów**.

    Pojawi się powiadomienie z usługi typu "odmowa".

## <a name="workflow-notifications"></a>Powiadomienia dotyczące przepływu pracy

Poniżej przedstawiono niektóre informacje na temat powiadomienia dotyczące przepływu pracy:

- Wszyscy członkowie listy Osoba zatwierdzająca powiadomienie za pośrednictwem poczty e-mail, gdy żądanie dla roli oczekuje na ich przeglądu. Powiadomienia e-mail zawierają bezpośredni link do żądania, gdy osoba zatwierdzająca mogli zatwierdzać lub odrzucać.
- Żądania są rozpoznawane przez pierwszego elementu członkowskiego listy, która zatwierdza lub odrzuca.
- Gdy osoba zatwierdzająca odpowiada na żądanie, wszyscy członkowie listy Osoba zatwierdzająca powiadomienie o akcji.
- Administratorom zasobów są powiadamiani, gdy zatwierdzonego elementu członkowskiego staje się aktywny w danej roli.

>[!Note]
>Administrator zasobu, który uważa, że zatwierdzonego elementu członkowskiego nie powinien być aktywny, można usunąć przypisania roli active w usłudze PIM. Mimo że administratorom zasobów nie są powiadamiani o oczekujących żądań, chyba że są one członków listy osoby zatwierdzającej, mogą wyświetlać i anulować oczekujące żądania wszystkim użytkownikom wyświetlanie oczekujących żądań w usłudze PIM. 

## <a name="next-steps"></a>Kolejne kroki

- [Rozszerzanie lub odnawianie ról zasobów platformy Azure w usłudze PIM](pim-resource-roles-renew-extend.md)
- [Powiadomienia e-mail w usłudze PIM](pim-email-notifications.md)
- [Zatwierdź lub Odrzuć żądania dla ról usługi Azure AD w usłudze PIM](azure-ad-pim-approval-workflow.md)
