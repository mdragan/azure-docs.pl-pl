---
title: Wyłącz usługi Azure Active Directory Domain Services | Dokumentacja firmy Microsoft
description: Wyłącz usługi Azure Active Directory Domain Services w witrynie Azure portal
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: mstephen
ms.openlocfilehash: 122caddd73219060689ac8f6e3ab6408ea99a422
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66246227"
---
# <a name="disable-azure-active-directory-domain-services-using-the-azure-portal"></a>Wyłącz usługi Azure Active Directory Domain Services w witrynie Azure portal
W tym artykule pokazano, jak wyłączyć usługi domenowe Azure Active Directory (AD) dla katalogu usługi Azure AD za pomocą witryny Azure portal.

> [!WARNING]
> **Usuwanie jest trwały i nie można cofnąć.**
> Zachowaj przy tym ostrożność! Usunięcie domeny zarządzanej:
>   * Kontrolery domeny dla domeny zarządzanej są cofanie aprowizacji i usuwane z sieci wirtualnej.
>   * Dane w domenie zarządzanej jest trwale usunięte. W tym niestandardowych jednostkach organizacyjnych, obiekty zasad grupy, niestandardowych rekordów DNS, nazwy główne usług, itp. konta Gmsa, które zostały utworzone w domenie zarządzanej.
>   * Komputery przyłączone do domeny zarządzanej utratę ich relacji zaufania z domeną i muszą być odłączony od domeny.
>   * Nie możesz się zarejestrować na tych maszynach przy użyciu firmowych poświadczeń usługi AD. Zamiast tego użyj poświadczeń administratora lokalnego dla maszyny.
> Usuwanie domeny zarządzanej nie usunąć katalog usługi Azure AD lub w przeciwnym razie niekorzystnie wpłynąć na katalog.

Wykonaj poniższe kroki, aby usunąć domenę zarządzaną usług domenowych Azure AD:
1. Przejdź do [rozszerzenia usługi Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) w witrynie Azure portal.
2. Kliknij nazwę swojej domeny zarządzanej.

    ![Wybierz domenę, aby usunąć](./media/getting-started/domain-services-delete-select-domain.png)

3. Na **Przegląd** kliknij **Usuń** przycisku.

    ![Usuwanie domeny](./media/getting-started/domain-services-delete-domain.png)

4. Aby potwierdzić usunięcie, wpisz nazwę domeny DNS z domeny zarządzanej. Kliknij przycisk **Usuń** przycisku po wykonaniu tych czynności.

    ![Domeny potwierdzenie usunięcia](./media/getting-started/domain-services-delete-domain-confirm.png)

Domena zarządzana zostanie usunięty z około 15 – 20 minut.

Należy wziąć pod uwagę [udostępniania opinii](contact-us.md) która pomoże nam zrozumieć, jakie funkcje może pomóc w przyszłości wybrano usług domenowych Azure AD. Ta opinia pomoże nam rozwijać usługę, aby lepiej dopasować się do swoich potrzeb wdrożenia i przypadki użycia.
