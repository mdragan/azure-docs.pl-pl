---
title: Konfigurowanie nazwanych lokalizacji w usłudze Azure Active Directory | Microsoft Docs
description: Dowiedz się, jak konfigurować nazwane lokalizacje.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.subservice: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: d12991813f68a42f9846c1c9c9c31c01d371d1d4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107636"
---
# <a name="quickstart-configure-named-locations-in-azure-active-directory"></a>Szybki start: konfigurowanie nazwanych lokalizacji w usłudze Azure Active Directory

Za pomocą nazwanych lokalizacji możesz oznaczać etykietą zakresy zaufanych adresów IP w Twojej organizacji. Usługa Azure AD używa nazwanych lokalizacji w następujących celach:
- Wykrywanie wyników fałszywie dodatnich w przypadku [zdarzeń o podwyższonym ryzyku](concept-risk-events.md). Logowanie z zaufanej lokalizacji obniża ryzyko użytkownika związane z logowaniem.   
- Konfigurowanie [dostępu warunkowego na podstawie lokalizacji](../conditional-access/location-condition.md).

W tym przewodniku Szybki start dowiesz się, jak skonfigurować nazwane lokalizacje w swoim środowisku.

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik Szybki Start, musisz spełnić następujące warunki:

* Dzierżawa usługi Azure AD. Zarejestruj się, aby skorzystać z [bezpłatnej wersji próbnej](https://azure.microsoft.com/trial/get-started-active-directory/). 
* Użytkownik będący administratorem globalnym dzierżawy.
* Zakres adresów IP, który jest uznany i wiarygodny w Twojej organizacji. Zakres adresów IP musi mieć format **CIDR (Classless Interdomain Routing)** .

## <a name="configure-named-locations"></a>Konfigurowanie nazwanych lokalizacji

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. W okienku po lewej stronie wybierz **usługi Azure Active Directory**, a następnie wybierz **dostępu warunkowego** z **zabezpieczeń** sekcji.

    ![Karta dostęp warunkowy](./media/quickstart-configure-named-locations/entrypoint.png)

3. Na stronie **Dostęp warunkowy** wybierz pozycję **Lokalizacje nazwane**, a następnie **Nowa lokalizacja**.

    ![Nazwana lokalizacja](./media/quickstart-configure-named-locations/namedlocation.png)

6. Wypełnij formularz na nowej stronie. 

   * W polu **Nazwa** wpisz nazwę dla nazwanej lokalizacji.
   * W polu **Zakresy adresów IP** wpisz zakres adresów IP w formacie CIDR.  
   * Kliknij pozycję **Utwórz**.
    
     ![Blok Nowy](./media/quickstart-configure-named-locations/61.png)

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji, zobacz:

- [Dostęp warunkowy usługi Azure AD](../active-directory-conditional-access-azure-portal.md).
- [Warunki lokalizacji w funkcji dostępu warunkowego usługi Azure AD](../conditional-access/location-condition.md)
- [Raport dotyczący ryzykownych logowań](concept-risky-sign-ins.md)  
