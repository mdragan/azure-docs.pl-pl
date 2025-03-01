---
title: Samouczek usługi Azure Security Center — Reagowanie na zdarzenia związane z zabezpieczeniami | Microsoft Docs
description: Samouczek usługi Azure Security Center — Reagowanie na zdarzenia związane z zabezpieczeniami
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 181e3695-cbb8-4b4e-96e9-c4396754862f
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2018
ms.author: rkarlin
ms.openlocfilehash: d726006d3ecce69f129b1576c7c6d12833582873
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60612711"
---
# <a name="tutorial-respond-to-security-incidents"></a>Samouczek: Reagowanie na zdarzenia związane z zabezpieczeniami
Usługa Security Center w sposób ciągły analizuje obciążenia chmury hybrydowej i ostrzega o złośliwych działaniach, używając zaawansowanych funkcji analitycznych oraz funkcji analizy zagrożeń. Ponadto można integrować alerty z innymi produktami i usługami zabezpieczeń w usłudze Security Center oraz tworzyć niestandardowe alerty bazujące na własnych wskaźnikach lub źródłach analiz. Po wygenerowaniu alertu potrzebne jest sprawne działanie w celu zbadania i skorygowania jego przyczyny. Niniejszy samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Klasyfikacja alertów zabezpieczeń
> * Dalsze badanie w celu ustalenia głównej przyczyny i zakresu zdarzenia zabezpieczeń
> * Wyszukiwanie danych zabezpieczeń pomocnych w badaniu

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne
Aby wypróbować funkcje omówione w tym samouczku, musisz korzystać ze standardowej warstwy cenowej usługi Security Center. Warstwę Standardowa usługi Security Center możesz wypróbować bezpłatnie. Aby dowiedzieć się więcej, zobacz [stronę z cennikiem](https://azure.microsoft.com/pricing/details/security-center/). Przewodnik szybkiego startu [Dołączanie subskrypcji platformy Azure do standardowej usługi Security Center](security-center-get-started.md) przeprowadzi Cię przez procedurę uaktualniania do warstwy standardowej.

## <a name="triage-security-alerts"></a>Klasyfikacja alertów zabezpieczeń
Usługa Security Center zapewnia spójny widok wszystkich alertów zabezpieczeń. Alerty zabezpieczeń są klasyfikowane na podstawie ich ważności i możliwości połączenia powiązanych alertów w zdarzenia zabezpieczeń. Klasyfikując alerty i zdarzenia, wykonaj następujące czynności:

- Odrzuć alerty, w przypadku których nie jest wymagane żadne dodatkowe działanie, na przykład gdy alert jest fałszywie dodatni
- Wykonaj czynności korygujące znane ataki, na przykład blokując ruch sieciowy ze złośliwego adresu IP
- Określ alerty, które wymagają dalszych badań


1. W menu głównym usługi Security Center w sekcji **WYKRYWANIE** wybierz pozycję **Alerty zabezpieczeń**:

   ![Alerty zabezpieczeń](./media/tutorial-security-incident/tutorial-security-incident-fig1.png)  

2. Na liście alertów kliknij zdarzenie zabezpieczeń będące kolekcją alertów, aby dowiedzieć się więcej na temat tego zdarzenia. Zostanie otwarte okno **Wykryto zdarzenie zabezpieczeń**.

   ![Zdarzenie naruszenia zabezpieczeń](./media/tutorial-security-incident/tutorial-security-incident-fig2.png)

3. U góry tego ekranu znajduje się opis zdarzenia zabezpieczeń, a poniżej lista alertów będących częścią tego zdarzenia. Kliknij alert, który chcesz zbadać dokładniej, aby uzyskać więcej informacji.

   ![Zdarzenie naruszenia zabezpieczeń](./media/tutorial-security-incident/tutorial-security-incident-fig3.png)

   Typy alertów mogą być różne. Aby uzyskać więcej szczegółowych informacji na temat typu alertu i możliwych czynności korygujących, przeczytaj [Informacje o alertach zabezpieczeń w usłudze Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). W przypadku alertów, które można bezpiecznie odrzucić, możesz kliknąć alert prawym przyciskiem myszy i wybrać pozycję **Odrzuć**:

   ![Alerty](./media/tutorial-security-incident/tutorial-security-incident-fig4.png)

4. Jeśli główna przyczyna i zakres złośliwych działań są nieznane, przejdź do następnego kroku, aby zbadać problem dokładniej.

## <a name="investigate-an-alert-or-incident"></a>Badanie alertu lub zdarzenia
1. Na stronie **Alert zabezpieczeń** kliknij przycisk **Rozpocznij badanie** (jeśli badanie jest już rozpoczęte, nazwa przycisku zmienia się na **Kontynuuj badanie**).

   ![Badanie](./media/tutorial-security-incident/tutorial-security-incident-fig5.png)

   Mapa badania jest graficzną reprezentacją obiektów związanych z tym alertem lub zdarzeniem zabezpieczeń. Kliknięcie obiektu na mapie powoduje, że w informacjach o tym obiekcie są wyświetlane nowe obiekty, a mapy są rozwijane. Właściwości obiektu zaznaczonego na mapie są wyróżnione w okienku po prawej stronie. Informacje dostępne na poszczególnych kartach różnią się w zależności od wybranego obiektu. W procesie badania przejrzyj wszystkie istotne informacje, aby lepiej zrozumieć poczynania osoby atakującej.

2. Jeśli potrzebujesz więcej dowodów lub musisz dokładniej zbadać obiekty, które zostały znalezione w trakcie badania, przejdź do następnego kroku.

## <a name="search-data-for-investigation"></a>Wyszukiwanie danych do badania

Aby znaleźć więcej dowodów na naruszenie zabezpieczeń systemów i uzyskać więcej informacji na temat obiektów, które są częścią dochodzenia, możesz skorzystać z funkcji wyszukiwania w usłudze Security Center.

Aby przeprowadzić wyszukiwanie, otwórz pulpit nawigacyjny usługi **Security Center**, kliknij pozycję **Wyszukaj** w lewym okienku nawigacji, wybierz obszar roboczy zawierający obiekty, które chcesz wyszukać, wpisz zapytanie wyszukiwania i kliknij przycisk wyszukiwania.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Inne przewodniki szybkiego startu i samouczki w tej kolekcji bazują na tym przewodniku. Jeśli zamierzasz kontynuować pracę z kolejnymi przewodnikami szybkiego startu i samouczkami, w dalszym ciągu korzystaj z warstwy standardowej i zachowaj włączoną automatyczną aprowizację. Jeśli nie zamierzasz kontynuować lub chcesz wrócić do warstwy bezpłatnej:

1. Wróć do menu głównego usługi Security Center i wybierz pozycję **Zasady zabezpieczeń**.
2. Wybierz subskrypcję lub zasady, którym chcesz przywrócić warstwę bezpłatną. Zostanie otwarte okno **Zasady zabezpieczeń**.
3. W obszarze **SKŁADNIKI ZASAD** wybierz pozycję **Warstwa cenowa**.
4. Wybierz pozycję **Bezpłatna**, aby zmienić subskrypcję z warstwy standardowej na warstwę bezpłatną.
5. Wybierz pozycję **Zapisz**.

Jeśli chcesz wyłączyć automatyczną aprowizację:

1. Wróć do menu głównego usługi Security Center i wybierz pozycję **Zasady zabezpieczeń**.
2. Wybierz subskrypcję, dla której chcesz wyłączyć automatyczną aprowizację.
3. W sekcji **Zasady zabezpieczeń — zbieranie danych** wybierz pozycję **Wyłącz** w obszarze **Dołączanie**, aby wyłączyć automatyczną aprowizację.
4. Wybierz pozycję **Zapisz**.

>[!NOTE]
> Wyłączenie automatycznej aprowizacji nie powoduje usunięcia programu Microsoft Monitoring Agent z maszyn wirtualnych platformy Azure, na których aprowizowano agenta. Wyłączenie automatycznej aprowizacji powoduje ograniczenie monitorowania zabezpieczeń dla zasobów.
>

## <a name="next-steps"></a>Następne kroki
W tym samouczku przedstawiono funkcje usługi Security Center, z których należy korzystać, podczas reagowania na zdarzenia zabezpieczeń, takie jak:

> [!div class="checklist"]
> * Zdarzenie zabezpieczeń będące agregacją powiązanych alertów dla zasobu
> * Mapa badania, która jest graficzną reprezentacją obiektów związanych z alertem lub zdarzeniem zabezpieczeń
> * Funkcje wyszukiwania umożliwiające znajdowanie dodatkowych dowodów na naruszenie zabezpieczeń systemów

Aby dowiedzieć się więcej na temat funkcji badania w usłudze Security Center, zobacz:

> [!div class="nextstepaction"]
> [Badanie zdarzeń i alertów](security-center-investigation.md)
