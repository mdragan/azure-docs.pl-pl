---
title: Wprowadzenie do żądań HTTP połączeń hybrydowych usługi Azure Relay na platformie .NET | Microsoft Docs
description: Napisz aplikację konsolową w języku C# do obsługi żądań HTTP połączeń hybrydowych usługi Azure Relay na platformie .NET.
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/01/2018
ms.author: spelluru
ms.openlocfilehash: 37227b7d0ea1b3630a3c2ce991a61543e6a1503d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428248"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-net"></a>Wprowadzenie do żądań HTTP połączeń hybrydowych usługi Relay na platformie .NET
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

W tym przewodniku Szybki start utworzysz aplikacje nadawcy i odbiorcy w środowisku .NET umożliwiające wysyłanie i odbieranie komunikatów przy użyciu protokołu HTTP. Aplikacje korzystają z funkcji połączeń hybrydowych usługi Azure Relay. Aby uzyskać więcej ogólnych informacji o usłudze Azure Relay, zobacz [Azure Relay](relay-what-is-it.md). 

W tym przewodniku Szybki start wykonasz następujące kroki:

1. Utworzenie przestrzeni nazw usługi Relay za pomocą witryny Azure Portal.
2. Utworzenie połączenia hybrydowego w tej przestrzeni nazw za pomocą witryny Azure Portal.
3. Napisanie aplikacji konsolowej serwera (odbiornika) służącej do odbierania komunikatów.
4. Napisanie aplikacji konsolowej klienta (nadawcy) służącej do wysyłania komunikatów.
5. Uruchamianie aplikacji. 

## <a name="prerequisites"></a>Wymagania wstępne

Do wykonania kroków tego samouczka niezbędne jest spełnienie następujących wymagań wstępnych:

* [Program Visual Studio 2015 lub nowszy](https://www.visualstudio.com). W przykładach znajdujących się w tym samouczku używany jest program Visual Studio 2017.
* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="create-a-namespace"></a>Tworzenie przestrzeni nazw
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection"></a>Tworzenie połączenia hybrydowego
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Tworzenie aplikacji serwera (odbiornika)
W programie Visual Studio napisz aplikację konsoli w języku C#, aby nasłuchiwać i odbierać komunikaty z usługi Relay.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-server](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>Tworzenie aplikacji klienta (nadawcy)
W programie Visual Studio napisz aplikację konsoli w języku C#, aby wysyłać komunikaty do usługi Relay.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-client](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-client.md)]

## <a name="run-the-applications"></a>Uruchamianie aplikacji
1. Uruchom aplikację serwera. W oknie konsoli zobaczysz następujący tekst:

    ```
    Online
    Server listening
    ```
1. Uruchom aplikację kliencką. W oknie klienta zobaczysz ciąg `hello!`. Klient wysłał żądanie HTTP do serwera, a serwer zwrócił ciąg `hello!`. 
3. Teraz, aby zamknąć okna konsoli, naciśnij klawisz **ENTER** w obu oknach konsoli. 

Gratulacje, utworzono kompletna aplikacja połączeń hybrydowych.

## <a name="next-steps"></a>Kolejne kroki

W tym przewodniku Szybki start utworzono aplikacje klienta i serwera w środowisku .NET służące do wysyłania i odbierania komunikatów za pomocą protokołu HTTP. Funkcja połączeń hybrydowych usługi Azure Relay obsługuje również wysyłanie i odbieranie komunikatów przy użyciu obiektów WebSocket. Aby dowiedzieć się, jak używać obiektów WebSocket z funkcją połączeń hybrydowych usługi Azure Relay, zobacz [Przewodnik Szybki start dotyczący obiektów WebSocket](relay-hybrid-connections-dotnet-get-started.md).

W tym przewodniku Szybki start za pomocą programu .NET Framework utworzono aplikacje klienta i serwera. Aby dowiedzieć się, jak pisać aplikacje klienta i serwera w środowisku Node.js, zapoznaj się z [przewodnikiem Szybki start dotyczącym obiektów WebSocket w środowisku Node.js](relay-hybrid-connections-node-get-started.md) lub [przewodnikiem Szybki start dotyczącym protokołu HTTP w środowisku Node.js](relay-hybrid-connections-http-requests-dotnet-get-started.md).
