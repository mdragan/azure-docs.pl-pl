---
title: Konfigurowanie alertów metryk dla usługi Azure Database dla serwera MariaDB w witrynie Azure portal
description: Tym artykule opisano sposób konfigurowania i alertów dotyczących metryk dostępu dla usługi Azure Database dla serwera MariaDB w witrynie Azure portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 01/07/2019
ms.openlocfilehash: ce9804a4ecad4cd0f2e797038dc10c71b50de9bf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61041091"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mariadb"></a>Konfigurowanie alertów dotyczących metryk usługi Azure Database dla serwera MariaDB za pomocą witryny Azure portal

W tym artykule przedstawiono sposób konfigurowania usługi Azure Database dla alertów MariaDB przy użyciu witryny Azure portal. Otrzymasz alert na podstawie monitorowania metryk usług platformy Azure.

Alertu powoduje wyzwolenie, gdy wartość określonej metryki przekracza wartość progową, w których zostanie przypisany. Alert wyzwalacze zarówno, gdy warunek zostanie spełniony, a następnie później podczas warunku nie jest już trwa spełniany.

Można skonfigurować alert, aby po jego wyzwoleniu, wykonaj następujące czynności:
* wysyłanie powiadomień e-mail do administratora usługi i współadministratorzy
* Wyślij wiadomość e-mail do dodatkowe adresy e-mail, które określisz.
* Wywołanie elementu webhook

Można skonfigurować i uzyskać informacje na temat reguł alertów za pomocą:
* [Azure Portal](../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [Interfejs wiersza polecenia platformy Azure](../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [Interfejs API REST usługi Azure Monitor](https://docs.microsoft.com/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric"></a>Tworzenie reguły alertu na metrykę
1. W [witryny Azure portal](https://portal.azure.com/), wybierz bazę danych Azure dla serwera MariaDB, którą chcesz monitorować.

2. W obszarze **monitorowanie** części paska bocznego wybierz **alerty** jak pokazano:

   ![Wybierz reguły alertów](./media/howto-alert-metric/2-alert-rules.png)

3. Wybierz **Dodaj alert dotyczący metryki** (ikony +).

4. **Utwórz regułę** zostanie otwarta strona, jak pokazano poniżej. Wprowadź wymagane informacje:

   ![Dodaj formularz alertu metryki](./media/howto-alert-metric/4-add-rule-form.png)

5. W ramach **warunek** zaznacz **Dodaj warunek**.

6. Wybierz metrykę, z listy sygnały, aby otrzymywać alerty na. W tym przykładzie wybierz "Magazynu percent".
   
   ![Wybierz metrykę](./media/howto-alert-metric/6-configure-signal-logic.png)

7. Konfigurowanie alert logic, w tym **warunek** (np.) "Większe niż"), **próg** (np.) 85 procent), **Agregacja czasu**, **okres** czasu reguła metryki muszą być spełnione przed wyzwalaczy alertu (np.) "W ciągu ostatnich 30 minut") a **częstotliwość**.
   
   Wybierz **gotowe** po zakończeniu.

   ![Wybierz metrykę](./media/howto-alert-metric/7-set-threshold-time.png)

8. W ramach **grup akcji** zaznacz **Utwórz nowy** Aby utworzyć nową grupę, aby otrzymywać powiadomienia o alercie.

9. Wypełnij formularz "Dodaj grupę akcji" przy użyciu nazwy krótkiej nazwy, subskrypcji i grupie zasobów.

10. Konfigurowanie **poczty E-mail/SMS/wypychania/rejestr** typ akcji.
    
    Wybierz "Wiadomości E-mail Azure zasobu Manager rolę" Aby wybrać subskrypcję właściciele, współautorzy i czytelnicy otrzymywać powiadomienia.
   
    Opcjonalnie możesz podać prawidłowy identyfikator URI w **elementu Webhook** jeśli ma ona wywoływana, gdy zostanie wyzwolony alert.

    Wybierz **OK** po zakończeniu.

    ![grupy akcji](./media/howto-alert-metric/10-action-group-type.png)

11. Określ nazwę reguły alertu, opis i ważności.

    ![grupy akcji](./media/howto-alert-metric/11-name-description-severity.png) 

12. Wybierz **Utwórz regułę alertu** do utworzenia alertu.

    W ciągu kilku minut ten alert jest aktywny i wyzwala w sposób opisany wcześniej.

## <a name="manage-your-alerts"></a>Zarządzanie alertami
Po utworzeniu alertu, można ją zaznaczyć i wykonaj następujące czynności:

* Wyświetl wykres przedstawiający próg metryki i rzeczywiste wartości z poprzedniego dnia związanych z tym alertem.
* **Edytuj** lub **Usuń** reguły alertu.
* **Wyłącz** lub **Włącz** alert, jeśli chcesz tymczasowo zatrzymać lub wznowić odbieranie powiadomień.


## <a name="next-steps"></a>Kolejne kroki
* Dowiedz się więcej o [konfigurowania elementów webhook w alertach](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Pobierz [omówienie zbierania metryk](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) się upewnić, że usługa jest dostępna i działa prawidłowo.
