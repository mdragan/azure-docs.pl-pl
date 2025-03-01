---
title: Zaufanie do zawartości w usłudze Azure Container Registry
description: Dowiedz się, jak włączyć zaufanie do zawartości dla rejestru kontenerów oraz jak wypychać i ściągać podpisane obrazy.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 05/06/2019
ms.author: danlep
ms.openlocfilehash: ca9ef32a830f56edb471256b3b9175ba0fbec51d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/27/2019
ms.locfileid: "65069222"
---
# <a name="content-trust-in-azure-container-registry"></a>Zaufanie do zawartości w usłudze Azure Container Registry

Usługa Azure Container Registry implementuje platformy Docker [zawartości zaufania] [ docker-content-trust] model, umożliwiając wypychanie i ściąganie obrazów z podpisem. Ten artykuł ułatwia pracę, umożliwiając zaufaj zawartości w Twojej rejestry kontenerów.

> [!NOTE]
> Zaufaj zawartości jest funkcją [jednostki SKU Premium](container-registry-skus.md) usługi Azure Container Registry.

## <a name="how-content-trust-works"></a>Jak działa zaufanie do zawartości

W każdym systemie rozproszonym zaprojektowanym z myślą o bezpieczeństwie bardzo ważne jest weryfikowanie zarówno *źródła*, jak i *integralności* danych wprowadzanych do systemu. Użytkownicy danych muszą mieć możliwość zweryfikowania wydawcy (źródła) danych, a także pewność, że dane nie zostały zmodyfikowane po publikacji (integralność). 

Jako wydawcy obrazu zaufanie do zawartości umożliwia Ci **podpisywanie** obrazów wypychanych do rejestru. Użytkownicy obrazów (osoby lub systemy ściągające obrazy z rejestru) mogą skonfigurować swoich klientów do ściągania *tylko* podpisanych obrazów. Gdy użytkownik ściąga podpisany obraz, jego klient platformy Docker weryfikuje integralność obrazu. W tym modelu użytkownicy mają pewność, że podpisane obrazy w Twoim rejestrze zostały faktycznie opublikowane przez Ciebie i że nie zostały one zmodyfikowane od momentu publikacji.

### <a name="trusted-images"></a>Zaufane obrazy

Zaufanie do zawartości współdziała z **tagami** w repozytorium. Repozytoria obrazów mogą zawierać obrazy z zarówno podpisanymi, jak i niepodpisanymi tagami. Przykładowo można podpisać tylko obrazy `myimage:stable` i `myimage:latest`, ale nie `myimage:dev`.

### <a name="signing-keys"></a>Klucze podpisywania

Zaufanie do zawartości jest zarządzane przy użyciu zestawu kryptograficznych kluczy podpisywania. Te klucze są skojarzone z konkretnym repozytorium w rejestrze. Istnieje kilka typów kluczy podpisywania, które są używane przez klientów platformy Docker i rejestr do zarządzania zaufaniem dla tagów w repozytorium. Po włączeniu zaufania do zawartości i zintegrowaniu go z publikowaniem kontenerów oraz potokiem zużycia należy ostrożnie zarządzać tymi kluczami. Aby uzyskać więcej informacji, zobacz sekcję [Zarządzanie kluczami](#key-management) w dalszej części tego artykułu oraz artykuł [Manage keys for content trust][docker-manage-keys] (Zarządzanie kluczami dla zaufania do zawartości) w dokumentacji platformy Docker.

> [!TIP]
> To jest bardzo ogólne omówienie modelu zaufania do zawartości platformy Docker. Szczegółowe informacje na temat zaufania do zawartości znajdziesz w artykule [Content trust in Docker][docker-content-trust] (Zaufanie do zawartości na platformie Docker).

## <a name="enable-registry-content-trust"></a>Włączanie zaufania do zawartości rejestru

Pierwszym krokiem jest włączenie zaufania do zawartości na poziomie rejestru. Gdy włączysz zaufanie do zawartości, klienci (użytkownicy lub usługi) będą mogli wypychać podpisane obrazy do Twojego rejestru. Włączenie zaufania do zawartości w rejestrze nie ogranicza użycia rejestru tylko do użytkowników z włączonym zaufaniem do zawartości. Użytkownicy bez włączonego zaufania do zawartości wciąż będą mogli używać Twojego rejestru w zwykły sposób. Jednak użytkownicy z włączony zaufaniem do zawartości w swoich klientach będą widzieć w Twoim rejestrze *tylko* podpisane obrazy.

Aby włączyć zaufanie do zawartości dla rejestru, najpierw przejdź do rejestru w witrynie Azure Portal. W obszarze **zasady**, wybierz opcję **ufaj zawartości** > **włączone** > **Zapisz**.

![Włączanie zaufania do zawartości dla rejestru w witrynie Azure Portal][content-trust-01-portal]

## <a name="enable-client-content-trust"></a>Włączanie zaufania do zawartości klienta

Aby pracować z zaufanymi obrazami, zarówno wydawcy obrazów, jak i użytkownicy muszą włączyć zaufanie do zawartości dla swoich klientów platformy Docker. Jako wydawca możesz podpisywać obrazy, które wypychasz do rejestru z włączonym zaufaniem do zawartości. Jako użytkownikowi włączenie zaufania do zawartości ogranicza widok rejestru do wyłącznie podpisanych obrazów. Zaufanie do zawartości jest domyślnie wyłączone w klientach platformy Docker, ale można je włączyć przy użyciu sesji powłoki lub polecenia.

Aby włączyć zaufanie do zawartości dla sesji powłoki, ustaw zmienną środowiskową `DOCKER_CONTENT_TRUST` na wartość **1**. Na przykład w powłoce Bash:

```bash
# Enable content trust for shell session
export DOCKER_CONTENT_TRUST=1
```

Jeśli zamiast tego chcesz włączyć lub wyłączyć zaufanie do zawartości dla jednego polecenia, kilka poleceń platformy Docker obsługuje argument `--disable-content-trust`. Aby włączyć zaufanie do zawartości dla pojedynczego polecenia:

```bash
# Enable content trust for single command
docker build --disable-content-trust=false -t myacr.azurecr.io/myimage:v1 .
```

Jeśli włączono zaufanie do zawartości dla sesji powłoki i chcesz je wyłączyć dla pojedynczego polecenia:

```bash
# Disable content trust for single command
docker build --disable-content-trust -t myacr.azurecr.io/myimage:v1 .
```

## <a name="grant-image-signing-permissions"></a>Udzielanie uprawnień do podpisywania obrazów

Tylko użytkownicy lub systemy, którym udzielono uprawnień, mogą wypychać zaufane obrazy do rejestru. Aby udzielić użytkownikowi (lub systemowi korzystającemu z jednostki usługi) uprawnień do wypychania zaufanych obrazów, przydziel jego tożsamości w usłudze Azure Active Directory rolę `AcrImageSigner`. Jest to uzupełnienie `AcrPush` (lub równoważnego) rolę wymaganą dla wypychanie obrazów do rejestru. Aby uzyskać więcej informacji, zobacz [uprawnienia i role usługi Azure Container Registry](container-registry-roles.md).

Szczegóły dotyczące przydzielania roli `AcrImageSigner` w witrynie Azure Portal i interfejsie wiersza polecenia platformy Azure znajdują się poniżej.

### <a name="azure-portal"></a>Azure Portal

Przejdź do rejestru w witrynie Azure Portal, a następnie wybierz pozycję **Kontrola dostępu (IAM)**  > **Dodaj przypisanie roli**. W obszarze **Dodawanie przypisania roli** wybierz pozycję `AcrImageSigner` w obszarze **Rola**, a następnie **wybierz** co najmniej jednego użytkownika lub jedną jednostkę usługi, po czym wybierz pozycję **Zapisz**.

W tym przykładzie przypisano rolę `AcrImageSigner` dwóm jednostkom: jednostce usługi o nazwie „service-principal” i użytkownikowi o nazwie „Azure User”.

![Włączanie zaufania do zawartości dla rejestru w witrynie Azure Portal][content-trust-02-portal]

### <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

Aby udzielić użytkownikowi uprawnień do podpisywania za pomocą interfejsu wiersza polecenia platformy Azure przypisz użytkownikowi rolę `AcrImageSigner` ograniczoną do rejestru. Format polecenia jest następujący:

```azurecli
az role assignment create --scope <registry ID> --role AcrImageSigner --assignee <user name>
```

Aby na przykład przydzielić sobie rolę, możesz uruchomić następujące polecenia w uwierzytelnionej sesji interfejsu wiersza polecenia platformy Azure. Zmodyfikuj wartość `REGISTRY`, aby odzwierciedlić nazwę rejestru kontenerów platformy Azure.

```bash
# Grant signing permissions to authenticated Azure CLI user
REGISTRY=myregistry
USER=$(az account show --query user.name --output tsv)
REGISTRY_ID=$(az acr show --name $REGISTRY --query id --output tsv)

az role assignment create --scope $REGISTRY_ID --role AcrImageSigner --assignee $USER
```

Możesz także udzielić [jednostce usługi](container-registry-auth-service-principal.md) uprawnienia do wypychania zaufanych obrazów do rejestru. Używanie jednostki usługi jest przydatne w przypadku systemów kompilacji oraz innych nienadzorowanych systemów, które mają wypychać zaufane obrazy do rejestru. Format jest podobny jak w przypadku udzielania uprawnień użytkownikowi, ale dla wartości `--assignee` należy określi identyfikator jednostki usługi.

```azurecli
az role assignment create --scope $REGISTRY_ID --role AcrImageSigner --assignee <service principal ID>
```

Identyfikator `<service principal ID>` może być identyfikatorem **appId** lub **objectId** bądź jedną z nazw **servicePrincipalName**. Aby uzyskać więcej informacji na temat pracy z jednostkami usługi i usługą Azure Container Registry, zobacz [Uwierzytelnianie w usłudze Azure Container Registry przy użyciu jednostek usługi](container-registry-auth-service-principal.md).

Po wprowadzeniu zmian dowolnej roli, uruchom `az acr login` do odświeżenia tokenu lokalna tożsamość wiersza polecenia platformy Azure, tak aby nowe role mogą obowiązywać.

## <a name="push-a-trusted-image"></a>Wypychanie zaufanego obrazu

Aby wypchnąć tag zaufanego obrazu do rejestru kontenerów, włącz zaufanie do zawartości i wypchnij obraz za pomocą polecenia `docker push`. Przy pierwszym wypchnięciu podpisanego tagu zostanie wyświetlony monit o utworzenie hasła dla zarówno klucza głównego podpisywania, jak i klucza podpisywania repozytorium. Zarówno klucz główny, jak i klucz repozytorium są generowane i przechowywane lokalnie na maszynie.

```console
$ docker push myregistry.azurecr.io/myimage:v1
The push refers to repository [myregistry.azurecr.io/myimage]
ee83fc5847cb: Pushed
v1: digest: sha256:aca41a608e5eb015f1ec6755f490f3be26b48010b178e78c00eac21ffbe246f1 size: 524
Signing and pushing trust metadata
You are about to create a new root signing key passphrase. This passphrase
will be used to protect the most sensitive key in your signing system. Please
choose a long, complex passphrase and be careful to keep the password and the
key file itself secure and backed up. It is highly recommended that you use a
password manager to generate the passphrase and keep it safe. There will be no
way to recover this key. You can find the key in your config directory.
Enter passphrase for new root key with ID 4c6c56a:
Repeat passphrase for new root key with ID 4c6c56a:
Enter passphrase for new repository key with ID bcd6d98:
Repeat passphrase for new repository key with ID bcd6d98:
Finished initializing "myregistry.azurecr.io/myimage"
Successfully signed myregistry.azurecr.io/myimage:v1
```

Po pierwszym użyciu polecenia `docker push` z włączonym zaufaniem do zawartości klient platformy Docker używa tego samego klucza głównego do kolejnych wypchnięć. Przy każdym kolejnym wypchnięciu do tego samego repozytorium wyświetlany jest tylko monit o podanie klucza repozytorium. Przy każdym wypchnięciu zaufanego obrazu do nowego repozytorium wyświetlany jest monit o podanie hasła dla nowego klucza repozytorium.

## <a name="pull-a-trusted-image"></a>Ściąganie zaufanego obrazu

Aby ściągnąć zaufany obraz, włącz zaufanie do zawartości i uruchom polecenie `docker pull` jak zwykle. Do ściągania obrazów zaufanych `AcrPull` rola jest wystarczająca dla normalni użytkownicy. Nie dodatkowych ról, takich jak `AcrImageSigner` roli są wymagane. Użytkownicy z włączonym zaufaniem do zawartości mogą ściągać tylko obrazy z podpisanymi tagami. Oto przykład ściągania podpisanego tagu:

```console
$ docker pull myregistry.azurecr.io/myimage:signed
Pull (1 of 1): myregistry.azurecr.io/myimage:signed@sha256:0800d17e37fb4f8194495b1a188f121e5b54efb52b5d93dc9e0ed97fce49564b
sha256:0800d17e37fb4f8194495b1a188f121e5b54efb52b5d93dc9e0ed97fce49564b: Pulling from myimage
8e3ba11ec2a2: Pull complete
Digest: sha256:0800d17e37fb4f8194495b1a188f121e5b54efb52b5d93dc9e0ed97fce49564b
Status: Downloaded newer image for myregistry.azurecr.io/myimage@sha256:0800d17e37fb4f8194495b1a188f121e5b54efb52b5d93dc9e0ed97fce49564b
Tagging myregistry.azurecr.io/myimage@sha256:0800d17e37fb4f8194495b1a188f121e5b54efb52b5d93dc9e0ed97fce49564b as myregistry.azurecr.io/myimage:signed
```

Jeśli klient z włączonym zaufaniem do zawartości spróbuje ściągnąć niepodpisany tag, operacja zakończy się niepowodzeniem:

```console
$ docker pull myregistry.azurecr.io/myimage:unsigned
No valid trust data for unsigned
```

### <a name="behind-the-scenes"></a>Za kulisami

Po uruchomieniu polecenia `docker pull` klient platformy Docker używa tej samej biblioteki co w przypadku [interfejsu wiersza polecenia usługi Notary][docker-notary-cli] do zażądania mapowania skrótu tag-do-SHA-256 dla wypychanego tagu. Po zweryfikowaniu podpisów danych o zaufaniu klient nakazuje aparatowi platformy Docker wykonanie „ściągnięcia przez skrót”. Podczas ściągania aparat używa sumy kontrolnej SHA-256 jako adresu zawartości w celu zażądania i zweryfikowania manifestu obrazu z rejestru kontenerów platformy Azure.

## <a name="key-management"></a>Zarządzanie kluczami

Jak określono w danych wyjściowych polecenia `docker push` podczas wypychania pierwszego zaufanego obrazu, klucz główny jest najbardziej wrażliwy. Pamiętaj, aby utworzyć kopię zapasową klucza głównego i przechowywać ją w bezpiecznym miejscu. Domyślnie klient platformy Docker przechowuje klucze podpisywania w następującym katalogu:

```sh
~/.docker/trust/private
```

Utwórz kopię zapasową klucza głównego i klucza repozytorium, kompresując jest w archiwum i przechowując je bezpiecznie w trybie offline (na przykład na urządzeniu magazynującym USB). Na przykład w powłoce Bash:

```bash
umask 077; tar -zcvf docker_private_keys_backup.tar.gz ~/.docker/trust/private; umask 022
```

Wraz z lokalnie wygenerowanym kluczem głównym i kluczem repozytorium generowanych jest kilka innych i są one zapisywane przez usługę Azure Container Registry podczas wypychania zaufanego obrazu. Szczegółowe omówienie różnych kluczy w implementacji zaufania do zawartości platformy Docker, w tym dodatkowe wskazówki dotyczące zarządzania, znajdziesz w artykule [Manage keys for content trust][docker-manage-keys] (Zarządzanie kluczami dla zaufania do zawartości) w dokumentacji platformy Docker.

### <a name="lost-root-key"></a>Utracenie klucza głównego

Jeśli utracisz dostęp do klucza głównego, utracisz dostęp do podpisanych tagów w każdym repozytorium, którego tagi były podpisane przy użyciu tego klucza. Usługa Azure Container Registry nie może przywrócić dostępu do tagów obrazów podpisanych przy użyciu utraconego klucza głównego. Aby usunąć wszystkie dane o zaufaniu (podpisy) dla rejestru, najpierw wyłącz, a następnie ponownie włącz zaufanie do zawartości dla rejestru.

> [!WARNING]
> Wyłączenie i ponowne włączenie zaufania do zawartości w rejestrze spowoduje **usunięcie całości danych o zaufaniu dla wszystkich podpisanych tagów w każdym repozytorium w rejestrze**. Ta akcja jest nieodwracalna — usługa Azure Container Registry nie może odzyskać usuniętych danych o zaufaniu. Wyłączenie zaufania do zawartości nie powoduje usunięcia samych obrazów.

Aby wyłączyć zaufanie do zawartości dla rejestru, przejdź do rejestru w witrynie Azure Portal. W obszarze **zasady**, wybierz opcję **ufaj zawartości** > **wyłączone** > **Zapisz**. Zostanie wyświetlone ostrzeżenie dotyczące utraty wszystkich podpisów w rejestrze. Wybierz przycisk **OK**, aby trwale usunąć wszystkie podpisy w rejestrze.

![Wyłączanie zaufania do zawartości dla rejestru w witrynie Azure Portal][content-trust-03-portal]

## <a name="next-steps"></a>Kolejne kroki

Zobacz [zawartości zaufania na platformie Docker] [ docker-content-trust] dodatkowe informacje o zawartości zaufania. Mimo iż w tym artykule poruszono kilka kluczowych kwestii, zaufanie do zawartości to obszerny temat i omówiono go dogłębniej w dokumentacji platformy Docker.

<!-- IMAGES> -->
[content-trust-01-portal]: ./media/container-registry-content-trust/content-trust-01-portal.png
[content-trust-02-portal]: ./media/container-registry-content-trust/content-trust-02-portal.png
[content-trust-03-portal]: ./media/container-registry-content-trust/content-trust-03-portal.png

<!-- LINKS - external -->
[docker-content-trust]: https://docs.docker.com/engine/security/trust/content_trust
[docker-manage-keys]: https://docs.docker.com/engine/security/trust/trust_key_mng/
[docker-notary-cli]: https://docs.docker.com/notary/getting_started/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[azure-cli]: /cli/azure/install-azure-cli
