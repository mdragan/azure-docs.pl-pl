---
title: Rozwiązywanie problemów z błędami dołączanie rozwiązania Update Management, śledzenia zmian i spisu
description: Dowiedz się, jak rozwiązywać problemy z błędami dołączania przy użyciu rozwiązania Update Management, śledzenia zmian i spis rozwiązań
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/22/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 012cf227bea7dc24eb85fcd9179a7b1025327dcf
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342147"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Rozwiązywanie problemów z błędami podczas dołączania do rozwiązania

Mogą wystąpić błędy podczas dołączania rozwiązań, takich jak zarządzanie aktualizacjami lub śledzenia zmian i spisu. W tym artykule opisano różne błędy, które mogą występować i ich rozwiązania.

## <a name="general-errors"></a>Ogólne błędy

### <a name="missing-write-permissions"></a>Scenariusz: Dołączanie zakończy się niepowodzeniem z komunikatem — nie można włączyć rozwiązania

#### <a name="issue"></a>Problem

Podczas próby dołączenia maszyny wirtualnej do rozwiązania wyświetlany jest jeden z następujących komunikatów:

```error
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

```error
The solution cannot be enabled on this VM because the permission to read the workspace is missing
```

#### <a name="cause"></a>Przyczyna

Ten błąd jest spowodowany przez niepoprawne lub brakujące uprawnienia na maszynie wirtualnej w obszarze roboczym, czy dla użytkownika.

#### <a name="resolution"></a>Rozwiązanie

Upewnij się, że masz odpowiednie uprawnienia do dołączenia maszyny wirtualnej. Przegląd [uprawnienia wymagane do dołączania maszyn](../automation-role-based-access-control.md#onboarding) , a następnie spróbuj dołączyć rozwiązanie ponownie. Jeśli zostanie wyświetlony błąd `The solution cannot be enabled on this VM because the permission to read the workspace is missing`, upewnij się, że `Microsoft.OperationalInsights/workspaces/read` uprawnień, aby można było znaleźć, jeśli maszyna wirtualna jest dołączona do obszaru roboczego.

### <a name="diagnostic-logging"></a>Scenariusz: Dołączanie kończy się niepowodzeniem z komunikatem — nie można skonfigurować konto automatyzacji dla rejestrowania diagnostycznego

#### <a name="issue"></a>Problem

Pojawi się następujący komunikat o błędzie podczas próby dołączenia maszyny wirtualnej do rozwiązania:

```error
Failed to configure automation account for diagnostic logging
```

#### <a name="cause"></a>Przyczyna

Ten błąd może być spowodowany, jeśli warstwa cenowa nie jest zgodny model rozliczeń dla subskrypcji. Aby uzyskać więcej informacji, zobacz [monitorowanie użycia i szacowanych kosztów w usłudze Azure Monitor](https://aka.ms/PricingTierWarning).

#### <a name="resolution"></a>Rozwiązanie

Ręczne tworzenie obszaru roboczego usługi Log Analytics i powtórz proces dołączania, aby wybrać obszar roboczy utworzony.

### <a name="computer-group-query-format-error"></a>Scenariusz: ComputerGroupQueryFormatError

#### <a name="issue"></a>Problem

Tego kodu błędu oznacza, że zapytanie grupy komputera zapisanego kryterium wyszukiwania używane pod kątem rozwiązania został nieprawidłowo sformatowany. 

#### <a name="cause"></a>Przyczyna

Zmienić kwerendę lub zostały zmienione przez system.

#### <a name="resolution"></a>Rozwiązanie

Można usunąć zapytania dla tego rozwiązania, a reonboard rozwiązanie, polegające na zapytanie. Zapytanie znajduje się w obszarze roboczym, w obszarze **zapisane wyszukiwania**. Nazwa zapytania jest **MicrosoftDefaultComputerGroup**, a kategorię zapytanie to nazwa rozwiązania skojarzoną z tym zapytaniem. Jeśli wiele rozwiązań są włączone, **MicrosoftDefaultComputerGroup** zawiera wiele razy w obszarze **zapisane wyszukiwania**.

### <a name="policy-violation"></a>Scenariusz: PolicyViolation

#### <a name="issue"></a>Problem

Ten kod błędu oznacza, że wdrożenie nie powiodło się z powodu naruszenia co najmniej jedne zasady.

#### <a name="cause"></a>Przyczyna 

Zasady zostaną spełnione, która blokuje ukończenie operacji.

#### <a name="resolution"></a>Rozwiązanie

Aby pomyślnie wdrożyć to rozwiązanie, należy wziąć pod uwagę zmianę wskazanych zasad. Jak wiele różnych typów zasad, które mogą być definiowane, zmian wymagane są zależne od zasady, które zostanie naruszona. Na przykład zdefiniowano zasad z grupy zasobów, której odmówiono uprawnień do zmiany zawartości niektórych rodzajów zasobów w tej grupie zasobów, można na przykład wykonaniu dowolnej z następujących czynności:

* Całkowicie usunąć zasady.
* Spróbuj przejść do innej grupy zasobów.
* Popraw zasady, na przykład:
  * Ponownie celem zasad do określonego zasobu (np. na określonym koncie usługi Automation).
  * Zmiana zestawu zasobów tej zasady został skonfigurowany do odmowy.

Sprawdź powiadomienia w prawym górnym rogu witryny Azure Portal lub przejdź do grupy zasobów, który zawiera Twoje konto usługi automation i wybierz **wdrożeń** w obszarze **ustawienia** wyświetlić nieudane wdrożenie. Aby dowiedzieć się więcej o zasadach usługi Azure, odwiedź stronę: [Omówienie usługi Azure Policy](../../governance/policy/overview.md?toc=%2fazure%2fautomation%2ftoc.json).

### <a name="unlink"></a>Scenariusz: Błędy podczas próby odłączanie obszaru roboczego

#### <a name="issue"></a>Problem

Pojawi się następujący błąd podczas próby odłączanie obszaru roboczego:

```error
The link cannot be updated or deleted because it is linked to Update Management and/or ChangeTracking Solutions.
```

#### <a name="cause"></a>Przyczyna

Ten błąd występuje, gdy nadal masz rozwiązania active w obszarze roboczym usługi Log Analytics, które są zależne od obszaru roboczego konto usługi Automation i analizy dzienników jest połączona.

### <a name="resolution"></a>Rozwiązanie

Aby rozwiązać ten problem, musisz usunąć następujące rozwiązania z obszaru roboczego, korzystając z nich:

* Zarządzanie aktualizacjami
* Śledzenie zmian
* Uruchamianie lub zatrzymywanie maszyn wirtualnych po godzinach pracy

Po usunięciu rozwiązania można odłączyć obszaru roboczego. Jest ważne oczyścić wszelkie istniejące artefakty z tych rozwiązań także z poziomu obszaru roboczego i konta usługi Automation.  

* Zarządzanie aktualizacjami
  * Usuwanie wdrożenia aktualizacji (harmonogramy) na koncie usługi Automation
* Uruchamianie lub zatrzymywanie maszyn wirtualnych po godzinach pracy
  * Usuń wszystkie blokady na składniki rozwiązania na koncie usługi Automation w ramach **ustawienia** > **blokad**.
  * Aby uzyskać dodatkowe kroki w celu usunięcia uruchamianie/zatrzymywanie maszyn wirtualnych podczas szczytu rozwiązania znajduje się [Usuń uruchamianie/zatrzymywanie maszyny Wirtualnej podczas szczytu rozwiązania](../automation-solution-vm-management.md##remove-the-solution).

## <a name="mma-extension-failures"></a>Błędy rozszerzenia programu MMA

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

W przypadku wdrażania rozwiązania, szereg powiązane zasoby są wdrażane. Jednym z tych zasobów jest agenta Microsoft Monitoring Agent rozszerzenia lub usługi Log Analytics dla systemu Linux. Są one zainstalowanych przez agenta gościa maszyny wirtualnej, który jest odpowiedzialny za podczas komunikowania się z skonfigurowany obszar roboczy usługi Log Analytics, na potrzeby koordynacji nowsze pobierania plików binarnych rozszerzeń maszyny wirtualnej i inne pliki rozwiązanie jesteś dołączania są zależne od po jej rozpoczęciu wykonywania.
Użytkownik zwykle najpierw zostaną powiadomieni o agenta MMA lub usługi Log Analytics dla systemu Linux błędy instalacji dla powiadomienia w Centrum powiadomień. Kliknięcie powiadomienia zawiera dalsze informacje na temat określonego błędu. Nawigacja do zasobów grupy zasobów, a następnie do elementu wdrożenia w niej również zawiera szczegółowe informacje dotyczące niepowodzeń wdrażania, które wystąpiły.
Instalacja agenta MMA lub usługi Log Analytics dla systemu Linux może się nie powieść z różnych powodów i kroki do wykonania, aby rozwiązać te błędy różnią się, w zależności od problemu. Postępuj zgodnie z określonym kroki rozwiązywania problemów.

W poniższej sekcji opisano różne problemy, które mogą pochodzić między podczas dołączania, spowodować awarię we wdrożeniu rozszerzenia programu MMA.

### <a name="webclient-exception"></a>Scenariusz: Wystąpił wyjątek podczas wykonywania żądania WebClient

Rozszerzenie programu MMA na maszynie wirtualnej nie może komunikować się z zasobami zewnętrznymi i wdrożenie kończy się niepowodzeniem.

#### <a name="issue"></a>Problem

Poniżej przedstawiono przykłady komunikaty o błędach, które są zwracane:

```error
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```error
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Przyczyna

Niektóre z możliwych przyczyn tego błędu to:

* Jest skonfigurowane na maszynie Wirtualnej, która pozwala tylko na określone porty serwera proxy.

* Ustawienie zapory został zablokowany dostęp do wymaganych portów i adresów.

#### <a name="resolution"></a>Rozwiązanie

Upewnij się, że masz odpowiednie porty i adresy otworzyć dla komunikacji. Aby uzyskać listę portów i adresów, zobacz [planowania sieci](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Scenariusz: Instalacja nie powiodła się z powodu problemów środowiska przejściowe

Instalacja rozszerzenia Microsoft Monitoring Agent nie powiodło się podczas wdrażania, ponieważ inna instalacja lub Akcja blokowania instalacji

#### <a name="issue"></a>Problem

Poniżej przedstawiono przykłady komunikaty o błędach, które mogą być zwrócone:

```error
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Przyczyna

Niektóre z możliwych przyczyn tego błędu to:

* Inna instalacja jest w toku
* System jest wyzwalany, ponowne uruchomienie podczas wdrażania szablonu

#### <a name="resolution"></a>Rozwiązanie

Ten błąd jest przejściowy błąd charakter. Ponów próbę wdrożenia, aby zainstalować rozszerzenie.

### <a name="installation-timeout"></a>Scenariusz: Limit czasu instalacji

Instalacja rozszerzenia MMA nie została ukończona z powodu przekroczenia limitu czasu.

#### <a name="issue"></a>Problem

Poniższy przykład jest komunikat o błędzie mogą być zwrócone:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Przyczyna

Ten błąd występuje, ponieważ maszyna wirtualna jest mocno obciążony podczas instalacji.

### <a name="resolution"></a>Rozwiązanie

Podjęto próbę można zainstalować rozszerzenia programu MMA, gdy maszyna wirtualna ma pod obciążeniem niższe.

## <a name="next-steps"></a>Kolejne kroki

Jeśli nie był widoczny problemu lub są w stanie rozwiązać problemu, odwiedź jedną z następujących kanałów obsługi więcej:

* Uzyskaj odpowiedzi od ekspertów w zakresie platformy Azure na [forach dotyczących platformy Azure](https://azure.microsoft.com/support/forums/)
* Połącz się z kontem [@AzureSupport](https://twitter.com/azuresupport) — oficjalnym kontem platformy Microsoft Azure utworzonym w celu podniesienia jakości obsługi klientów przez połączenie społeczności platformy Azure z odpowiednimi zasobami: odpowiedziami, pomocą techniczną i ekspertami.
* Jeśli potrzebujesz więcej pomocy, mogą zgłaszać zdarzenia pomocy technicznej platformy Azure. Przejdź do [witryny pomocy technicznej platformy Azure](https://azure.microsoft.com/support/options/) i wybierz **uzyskiwanie pomocy technicznej**.
