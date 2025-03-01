---
title: Usługa Azure Table | Portal Azure Marketplace
description: Skonfiguruj zarządzanie potencjalnymi klientami dla tabeli platformy Azure.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: pabutler
ms.openlocfilehash: 08f9d794822dfd7879efc7c4813ecc46f92f6a45
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147896"
---
# <a name="lead-management-instructions-for-azure-table"></a>Instrukcje prowadzić zarządzania dla usługi Azure Table

W tym artykule opisano sposób konfigurowania usługi Azure Table do przechowywania potencjalnych klientów. Tabela platformy Azure umożliwia przechowywanie i Dostosuj informacje o kliencie.


## <a name="how-to-configure-azure-table"></a>Jak skonfigurować usługi Azure Table

1. Jeśli nie masz konta platformy Azure, możesz to zrobić [Utwórz bezpłatne konto próbne](https://azure.microsoft.com/pricing/free-trial/).
2. Po uaktywnieniu konta platformy Azure, zaloguj się do [witryny Azure portal](https://portal.azure.com).
3. W witrynie Azure portal Utwórz konto magazynu, korzystając z następującej procedury.  
    1. Wybierz **+ Utwórz zasób** na pasku menu po lewej stronie.  **New** (bloku) w okienku pojawi się po prawej stronie.
    2. Wybierz **magazynu** w **New** okienka.  A **polecane** po prawej stronie zostanie wyświetlona lista.
    3. Wybierz **konta magazynu** do rozpoczęcia tworzenia konta.  Postępuj zgodnie z instrukcjami w artykule [Tworzenie konta magazynu](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal).

    ![Kroki, aby utworzyć konto magazynu platformy Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

    Aby uzyskać więcej informacji na temat kont magazynu wybierz [samouczek szybkiego startu](https://docs.microsoft.com/azure/storage/).  Aby uzyskać więcej informacji na temat cen magazynu, zobacz [ceny za magazyn](https://azure.microsoft.com/pricing/details/storage/).

4. Zaczekaj, aż aprowizacji konta magazynu, proces, który zazwyczaj zajmuje kilka minut.  Uzyskiwać dostęp do konta usługi storage z **Home** strony w witrynie Azure Portal, wybierając **Zobacz wszystkie Twoje zasoby** lub wybierając **wszystkie zasoby** w lewym obszarze nawigacji pasek menu w witrynie Azure Portal.

    ![Dostęp do konta usługi Azure storage](./media/cloud-partner-portal-lead-management-instructions-azure-table/azure-storage-access.png)

5. W okienku konta magazynu, skopiuj parametry połączenia konta magazynu dla klucza i wklej go w **parametry połączenia konta magazynu** pole w portalu Cloud Partner. Przykładem ciągu połączenia jest:

```sql
DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net
```

  ![Klucz usługi Azure storage](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

Możesz użyć [Eksplorator usługi Azure storage](https://azurestorageexplorer.codeplex.com/) lub innego podobnego narzędzia, aby wyświetlić dane w tabeli magazynu. Można również wyeksportować dane z tabel platformy Azure.


## <a name="use-microsoft-flow-with-an-azure-table-optional"></a>Microsoft Flow za pomocą usługi Azure Table (*opcjonalne*) 

Możesz użyć [Microsoft Flow](https://docs.microsoft.com/flow/) zautomatyzować powiadomienia, za każdym razem, gdy potencjalny klient zostanie dodana do tabeli platformy Azure. Jeśli nie masz konta, możesz to zrobić [Załóż bezpłatne konto](https://flow.microsoft.com/).


### <a name="lead-notification-example"></a>Przykład powiadomienia potencjalnych klientów

Użyj tego przykładu jako przewodnika, aby utworzyć przepływ podstawowy, który automatycznie wysyła wiadomość e-mail z powiadomieniem po dodaniu nowego potencjalnego klienta do tabeli platformy Azure. W tym przykładzie cykl wysyłać informacje o potencjalnym kliencie co godzinę, jeśli magazyn tabel jest aktualizowana.

1. Zaloguj się do konta usługi Microsoft Flow.
2. Na pasku nawigacyjnym po lewej stronie wybierz **moje przepływy**.
3. Na górnym pasku nawigacyjnym, wybierz **+ nowy**.  
4. Na liście rozwijanej wybierz **+ Utwórz z pustego**
5. W obszarze Utwórz przepływ od podstaw, wybierz **Utwórz z pustego**.

   ![Utwórz nowy przepływ od podstaw](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-create-from-blank.png)

6. Na stronie łączniki i wyzwalacze wyszukiwania, wybierz **wyzwalaczy**.
7. W obszarze **wyzwalaczy**, wybierz opcję **cyklu**.
8. W **cyklu** oknie pozostawić domyślne ustawienie 1 **interwał**. Z **częstotliwość** listy rozwijanej wybierz **godzinę**.

   >[!NOTE] 
   >Mimo że w tym przykładzie użyto interwału 1-godzinnego, możesz wybrać interwał i częstotliwość, z którą jest najlepszym rozwiązaniem dla potrzeb biznesowych.

   ![Ustaw częstotliwość cyklu 1 godziny](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-recurrence-dropdown.png)

9. Wybierz **+ nowy krok**.
10. Wyszukaj "Get ostatnich czas", a następnie wybierz pozycję **Pobierz czas w przeszłości** w kolumnie akcje. 

    ![Znajdź i wybierz polecenie Pobierz upłynął czas działania](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-search-getpasttime.png)

11. W **Pobierz czas w przeszłości** oknie **interwał** 1.  Z **jednostkę czasu** listy rozwijanej wybierz **godzinę**.
    >[!IMPORTANT] 
    >Upewnij się, że ta jednostka interwału i czas odpowiada interwał i częstotliwość został skonfigurowany dla cyklu.

    ![Pobierz zestaw ostatnie przedział czasu](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getpast-time.png)

    >[!TIP] 
    >Przepływ można sprawdzić w dowolnym momencie, aby sprawdzić, czy każdy krok został poprawnie skonfigurowany. Aby sprawdzić swój przepływ, zaznacz **sprawdzania przepływ** na pasku menu usługi Flow.

W następnego zestawu kroków połączysz się z tabeli platformy Azure i skonfiguruj logiki przetwarzania do obsługi nowych potencjalnych klientów.

1. Po Get ostatnie krok czasu, wybrać **+ nowy krok**, a następnie wyszukaj "Get jednostki".
2. W obszarze **akcje**, wybierz opcję **pobrać jednostki**, a następnie wybierz pozycję **Pokaż opcje zaawansowane**.
3. W **pobrać jednostki** okna, podaj informacje dla następujących pól:

   - **Tabela** — wprowadź nazwę użytkownika usługi Azure Table Storage. Następny zrzut ekranu przedstawia monit po wprowadzeniu "MarketPlaceLeads" w tym przykładzie. 

     ![Wybierz niestandardową wartość dla nazwy tabeli platformy Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-table-name.png)

   - **Zapytanie filtru** — to pole, a **Pobierz czas w przeszłości** ikona jest wyświetlana w oknie podręcznym. Wybierz **czas w przeszłości** skorzystać z tej jako sygnatura czasowa do filtrowania zapytania. Alternatywnie można wkleić następującą funkcję do pola: CreatedTime `Timestamp gt datetime'@{body('Get_past_time')}'` 

     ![Konfigurowanie funkcji zapytania filtru](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-filterquery.png)

4. Wybierz **nowy krok** Aby dodać warunek do skanowania tabeli platformy Azure dla nowych potencjalnych klientów.

   ![Użyj nowy krok, aby dodać warunek do skanowania tabeli platformy Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-add-filterquery-new-step.png)

5. W **wybierz akcję** wybierz **akcje**, a następnie wybierz pozycję **warunek** kontroli.

     ![Dodaj warunek](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-action-condition-control.png)

6. W **warunek** wybierz **wybierz wartość** , a następnie wybierz opcję **wyrażenie** w oknie podręcznym.
7. Wklej `length(body('Get_entities')?['value'])` do ***fx*** pola. Wybierz **OK** można dodać tę funkcję. Aby zakończyć konfigurowanie warunku:

   - Wybierz opcję "jest większa niż" z listy rozwijanej.
   - Wprowadź wartość 0, jako wartość 

     ![Dodawanie funkcji do warunku](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-fx0.png)

8. Skonfiguruj akcję do wykonania na podstawie wyniku warunku.

     ![Konfigurowanie akcji na podstawie wyników warunku](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-pick-action.png)

9. Jeśli warunek jest rozpoznawany jako **Jeśli**, nie robią niczego. 
10. Jeśli warunek jest rozpoznawany jako **tak**, wyzwalać akcję, która łączy konto usługi Office 365, aby wysłać wiadomość e-mail. Wybierz **Dodaj akcję**.
11. Wybierz **Wyślij wiadomość e-mail**. 
12. W **Wyślij wiadomość e-mail** okna, podaj informacje dla następujących pól:

    - **Aby** — wprowadź adres e-mail dla wszystkich użytkowników, które otrzymają to powiadomienie.
    - **Temat** — Podaj temat wiadomości e-mail. Na przykład: Nowych potencjalnych klientów!
    - **Treść**:   Dodaj tekst, który chcesz uwzględnić w każdej wiadomości e-mail (opcjonalnie), a następnie wklej w treści `body('Get_entities')?['value']` jako funkcję, aby wstawić informacje o potencjalnym kliencie.

      >[!NOTE] 
      >Możesz wstawić dodatkowe dane statyczne lub dynamiczne punkty do treści tej wiadomości e-mail.

      ![Konfigurowanie poczty e-mail dla powiadomień o potencjalnych klientów](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-emailbody-fx.png)

13. Wybierz **Zapisz** Aby zapisać przepływ. Microsoft Flow automatycznie spowoduje przetestowanie przepływu dla błędów. Jeśli nie ma żadnych błędów, przepływ uruchamiania po zapisaniu.

Następnym zrzucie ekranu przedstawiono przykład sposobu końcowego przepływ powinien wyglądać.

[![Sekwencja końcowego przepływu](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end-thmb.png)](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end.png)

(*Kliknij obraz, aby powiększyć.* )


### <a name="manage-your-flow"></a>Zarządzanie przepływu

Zarządzanie przepływ po uruchomiona jest bardzo proste.  Masz pełną kontrolę nad przepływu. Na przykład można zatrzymać go, go edytować, wyświetlać historię wykonywania i umieścić dane analityczne. Następny zrzut ekranu przedstawia opcje, które są dostępne do zarządzania przepływem. 

 ![Zarządzanie przepływem](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-manage-completed.png)

Przepływ nadal będzie działać, dopóki nie można zatrzymać za pomocą **wyłączyć przepływ** opcji.

Jeśli nie otrzymujesz powiadomień e-mail wszelkich potencjalnych klientów, oznacza to, że nowych potencjalnych klientów nie zostały dodane do tabeli platformy Azure. W przypadku wszelkich błędów przepływów, otrzymasz wiadomość e-mail, podobnie jak w przykładzie na następnym zrzucie ekranu.

 ![Powiadomienie e-mail niepowodzenia przepływu](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-failure-note.png)


## <a name="next-steps"></a>Kolejne kroki

[Konfigurowanie potencjalnych klientów](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)
