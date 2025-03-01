---
title: Rozwiązywanie problemów z zarządzaniem uprawnień usługi Azure AD (wersja zapoznawcza) — usługi Azure Active Directory
description: Dowiedz się więcej o niektórych elementów, należy sprawdzić, aby ułatwić rozwiązywanie problemów z zarządzaniem uprawnień usługi Azure Active Directory (wersja zapoznawcza).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/30/2019
ms.author: rolyon
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: c2526ef10c3080dae1b32881a109a9436a0fd390
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66473827"
---
# <a name="troubleshoot-azure-ad-entitlement-management-preview"></a>Rozwiązywanie problemów z zarządzaniem uprawnień usługi Azure AD (wersja zapoznawcza)

> [!IMPORTANT]
> Zarządzanie uprawnieniami w usłudze Azure Active Directory (Azure AD) jest obecnie w publicznej wersji zapoznawczej.
> Ta wersja zapoznawcza nie jest objęta umową dotyczącą poziomu usług i nie zalecamy korzystania z niej w przypadku obciążeń produkcyjnych. Niektóre funkcje mogą być nieobsługiwane lub ograniczone.
> Aby uzyskać więcej informacji, zobacz [Uzupełniające warunki korzystania z wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

W tym artykule opisano niektóre elementy, należy sprawdzić, aby ułatwić rozwiązywanie problemów z zarządzaniem uprawnień usługi Azure Active Directory (Azure AD).

## <a name="checklist-for-entitlement-management-administration"></a>Lista kontrolna dotycząca uprawnieniem administrowanie

* Jeśli otrzymasz komunikat o odmowie podczas konfigurowania Zarządzanie uprawnieniami dostępu, ale jesteś administratorem globalnym, upewnij się, że katalog zawiera [licencji usługi Azure AD Premium P2 (lub EMS E5)](entitlement-management-overview.md#license-requirements).  
* Jeśli otrzymasz komunikat, podczas tworzenia lub wyświetlania dostępu do pakietów o odmowie dostępu, a użytkownik jest członkiem grupy Twórcy katalogu, należy utworzyć katalog przed utworzeniem pierwszego pakietu dostępu.

## <a name="checklist-for-adding-a-resource"></a>Lista kontrolna dotycząca dodawania zasobu

* Dla aplikacji do zasobu w pakiecie dostępu musi mieć co najmniej jedną rolę zasobu, które mogą być przypisane. Role są definiowane przez samą aplikację, a także są zarządzane w usłudze Azure AD. Należy pamiętać, że witryny Azure portal, również mogą być wyświetlane nazwy główne usług dla usług, których nie można wybrać jako aplikacje.  W szczególności **usługi Exchange Online** i **usługi SharePoint Online** usług, a nie w przypadku aplikacji, które mają role zasobów w katalogu, dlatego nie można uwzględnić w pakiecie dostępu.  Zamiast tego należy użyć Licencjonowanie na podstawie grupy można ustanowić odpowiednią licencję dla użytkownika, który musi mieć dostęp do tych usług.

* Grupy zasobów w pakiecie dostępu musi mieć możliwość można modyfikować w usłudze Azure AD.  Nie można przypisać grup, które pochodzą z lokalnej usługi Active Directory jako zasoby, ponieważ nie można zmienić ich właścicielem lub atrybuty elementu członkowskiego w usłudze Azure AD.  

* Nie można dodać bibliotekach dokumentów SharePoint Online i poszczególnych dokumentów jako zasoby.  Zamiast tego utwórz grupę zabezpieczeń usługi Azure AD, objąć pakiet dostęp do tej grupy i roli lokacji i w usłudze SharePoint Online należy użyć tej grupy do kontrolowania dostępu do biblioteki dokumentów lub dokumentu.

* W przypadku użytkowników, którzy mają już przypisany do zasobu, który chcesz zarządzać za pomocą pakietu dostępu należy się upewnić, że użytkownicy są przypisane do dostępu do pakietu, odpowiednie zasady. Na przykład możesz chcieć dołączyć grupę w pakiecie dostępu, który już użytkowników w grupie. Jeśli tych użytkowników w ramach wymagają grupy stały dostęp, muszą one mieć odpowiednie zasady dla pakietów dostępu, tak, aby ich nie utracić dostępu do tej grupy. Można przypisać pakietu dostęp, prosząc użytkowników, aby zażądać dostępu do pakietu, zawierająca ten zasób lub przez bezpośrednie przypisywanie ich do dostępu do pakietu. Aby uzyskać więcej informacji, zobacz [edytowanie i zarządzanie nią istniejący pakiet dostępu](entitlement-management-access-package-edit.md).

## <a name="checklist-for-providing-external-users-access"></a>Lista kontrolna dotycząca zapewnienie dostępu użytkowników zewnętrznych

* W przypadku B2B [listy dozwolonych](../b2b/allow-deny-list.md), użytkowników, których katalogi nie są dozwolone, nie będzie mógł poprosić o dostęp.

* Upewnij się, że nie istnieją żadne [zasady dostępu warunkowego](../conditional-access/require-managed-devices.md) które uniemożliwiłyby żądające dostępu lub będzie mógł korzystać z aplikacji w pakietach dostępu użytkowników zewnętrznych.

## <a name="checklist-for-request-issues"></a>Lista kontrolna dotycząca problemów z żądania

* Po użytkownik chce, aby zażądać dostępu do pakietów programu access, należy się upewnić, że będą używać **link do portalu Moje dostępu** dostępu do pakietu. Aby uzyskać więcej informacji, zobacz [link do portalu dostęp Moja kopia](entitlement-management-access-package-edit.md#copy-my-access-portal-link).

* Gdy użytkownik loguje się do portalu Moje dostępu, aby zażądać dostępu do pakietu, upewnij się, że podczas uwierzytelniania przy użyciu konta organizacji. Konto organizacyjne można albo konto, w tym katalogu zasobów lub w katalogu, który znajduje się w jednej z zasad dostępu do pakietu. Jeśli konto użytkownika nie jest kontem organizacyjnym, lub katalog nie znajduje się w zasadach, następnie użytkownik nie zobaczy pakiet dostępu. Aby uzyskać więcej informacji, zobacz [żądanie dostępu do pakietu dostępu](entitlement-management-request-access.md).

* Jeśli użytkownik jest zablokowany logowanie do katalogu zasobów, nie będą w możliwość żądania dostępu w portalu Moje dostępu. Zanim użytkownik może zażądać dostępu, należy usunąć blok logowania z profilu użytkownika. Aby usunąć blok logowania w witrynie Azure portal, kliknij **usługi Azure Active Directory**, kliknij przycisk **użytkowników**, kliknij nazwę użytkownika, a następnie kliknij **profilu**. Edytuj **ustawienia** sekcji i zmień **Blokuj logowanie** do **nie**. Aby uzyskać więcej informacji, zobacz [Dodaj lub zaktualizuj informacje o profilu użytkownika przy użyciu usługi Azure Active Directory](../fundamentals/active-directory-users-profile-azure-portal.md).  Możesz również sprawdzić, jeśli użytkownik został zablokowany ze względu na [zasady ochrony tożsamości](../identity-protection/howto-unblock-user.md).

* W portalu Moje dostęp, jeśli użytkownik należy zarówno obiekt żądający, jak i osoba zatwierdzająca nie zobaczy ich żądanie dostępu do pakietu na **zatwierdzenia** strony. To zachowanie jest celowe — użytkownik nie może zatwierdzić swoje własne żądanie. Upewnij się, że pakiet dostępu, których aplikacja żąda ma dodatkowe osoby zatwierdzające skonfigurowane w zasadach. Aby uzyskać więcej informacji, zobacz [Edytuj istniejące zasady](entitlement-management-access-package-edit.md#edit-an-existing-policy).

* Jeśli nowego użytkownika zewnętrznego, który nie wcześniej zalogował się w katalogu, odbierze pakietu dostępu, włącznie z witryny usługi SharePoint Online, ich dostęp do pakietu zostaną wyświetlone, nie są w pełni dostarczonych do momentu aprowizacji konta w usłudze SharePoint Online.

## <a name="next-steps"></a>Kolejne kroki

- [Wyświetlanie raportów sposobu uzyskania dostępu użytkowników w przystawce Zarządzanie uprawnieniem](entitlement-management-reports.md)
