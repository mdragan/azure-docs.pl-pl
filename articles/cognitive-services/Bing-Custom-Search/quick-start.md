---
title: 'Szybki start: tworzenie pierwszego wystąpienia usługi wyszukiwania niestandardowego Bing | Microsoft Docs'
titlesuffix: Azure Cognitive Services
description: Korzystając z tego artykułu, możesz utworzyć niestandardowe wystąpienie usługi Bing, które umożliwi przeszukiwanie zdefiniowanych przez Ciebie domen i stron internetowych.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 06/18/2019
ms.author: aahi
ms.openlocfilehash: 6949824f598194456837544526223b823dcfc3e5
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273349"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>Szybki start: tworzenie pierwszego wystąpienia usługi wyszukiwania niestandardowego Bing

Aby móc skorzystać z usługi wyszukiwania niestandardowego Bing, należy utworzyć wystąpienie wyszukiwania niestandardowego definiujące widok lub wycinek sieci Internet. To wystąpienie zawiera domeny publiczne, witryny i strony internetowe, które mają być wyszukiwane, oraz wszelkie dostosowania klasyfikacji, których możesz potrzebować. 

Aby utworzyć wystąpienie, użyj [portalu usługi wyszukiwania niestandardowego Bing](https://customsearch.ai). 

![Obraz portalu usługi wyszukiwania niestandardowego Bing](media/blockedCustomSrch.png)

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-a-custom-search-instance"></a>Tworzenie wystąpienia wyszukiwania niestandardowego

Aby utworzyć wystąpienie wyszukiwania niestandardowego Bing:

1. Kliknij pozycję **Wprowadzenie** na stronie internetowej [portalu usługi wyszukiwania niestandardowego Bing](https://customsearch.ai) i zaloguj się przy użyciu konta Microsoft.

2. Kliknij pozycję **Nowe wystąpienie** i wprowadź opisową nazwę. Nazwę wystąpienia można w dowolnym momencie zmienić.
 
3. Na karcie **Active** (Aktywne) w obszarze **Search Experience** (Funkcja wyszukiwania) wprowadź adres URL jednej lub większej liczby witryn internetowych, które mają zostać uwzględnione w wyszukiwaniu. 

    > [!NOTE]
    > Wystąpienia usługi wyszukiwania niestandardowego Bing będą zwracały tylko wyniki dla domen i stron internetowych, które są publiczne i zostały zindeksowane przez usługę Bing.

4. Po prawej stronie portalu usługi wyszukiwania niestandardowego Bing możesz wprowadzić zapytanie i sprawdzić wyniki zwrócone przez wystąpienie usługi wyszukiwania. Jeśli nie są zwracane żadne wyniki, spróbuj wprowadzić inny adres URL.  

5. Kliknij pozycję **Publikuj**, aby opublikować zmiany w środowisku produkcyjnym i zaktualizować punkty końcowe wystąpienia.

6.  Kliknij **produkcji** karcie **punktów końcowych**, a następnie skopiuj usługi **niestandardowy identyfikator konfiguracji**. Ten identyfikator jest potrzebny do wywołania interfejsu API wyszukiwania niestandardowego. Dodaje się go do parametru zapytania `customconfig=` w wywołaniach.


## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Szybki start: wywoływanie punktu końcowego wyszukiwania niestandardowego Bing](./call-endpoint-csharp.md)
