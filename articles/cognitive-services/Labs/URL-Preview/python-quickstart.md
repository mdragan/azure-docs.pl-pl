---
title: 'Szybki start: Projekt Podgląd adresu URL, Python'
titlesuffix: Azure Cognitive Services
description: Przykładowy skrypt umożliwiający szybkie rozpoczęcie pracy z laboratorium Project URL Preview w języku Python.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: quickstart
ms.date: 03/29/2018
ms.author: rosh
ms.openlocfilehash: afb64b799e826abc3e6aad661a97211b43304dc6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60462728"
---
# <a name="quickstart-url-preview-with-python"></a>Szybki start: Podgląd adresu URL za pomocą języka Python

Poniższy przykład w języku Python tworzy podgląd adresu URL witryny internetowej SwiftKey: https://swiftkey.com/en.

## <a name="prerequisites"></a>Wymagania wstępne

Pobierz klucz dostępu dla bezpłatnej wersji próbnej zestawu [Cognitive Services Labs](https://aka.ms/answersearchsubscription).

W tym przykładzie użyto języka Python 3.6.

## <a name="code-scenario"></a>Scenariusz kodu 

Poniższy kod tworzy podgląd adresu URL.
W celu zaimplementowania kodu wykonaj następujące kroki:
1. Zadeklaruj zmienne, aby określić punkt końcowy na podstawie hosta i ścieżki.
2. Określ adres URL zapytania, dla którego ma zostać wyświetlony podgląd, a następnie dodaj parametr zapytania.  
3. Ustaw parametr zapytania.
4. Zdefiniuj funkcję wyszukiwania, która tworzy żądanie i dodaje nagłówek *Ocp-Apim-Subscription-Key*.
5. Ustaw nagłówek *Ocp-Apim-Subscription-Key*. 
6. Nawiąż połączenie i wyślij żądanie.
7. Wydrukuj wyniki w formacie JSON.

Pełny kod tej wersji demonstracyjnej wygląda następująco:

```
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'your-subscription-key'

host = 'api.labs.cognitive.microsoft.com'
path = '/urlpreview/v7.0/search'

query = 'https://SwiftKey.com'

params = '?q=' + urllib.parse.quote (query)

def get_preview ():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_preview ()
print (json.dumps(json.loads(result), indent=4))
```
## <a name="next-steps"></a>Kolejne kroki
- [Przewodnik Szybki start dla języka C#](csharp.md)
- [Przewodnik Szybki start dla języka Java](java-quickstart.md)
- [Przewodnik Szybki start dla języka JavaScript](javascript.md)
- [Przewodnik Szybki start dla języka Node.js](node-quickstart.md)
