---
title: Aplikacja Angular w języku Node.js korzystająca z interfejsu API bazy danych MongoB (część 1)
titleSuffix: Azure Cosmos DB
description: Dowiedz się, jak utworzyć aplikację bazy danych MongoDB w usłudze Angular i języku Node dla usługi Azure Cosmos DB przy użyciu tych samych interfejsów API, co używane w usłudze MongoDB, oglądając tę serię samouczków opartą na filmach wideo.
author: johnpapa
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: jopapa
ms.custom: seodec18
ms.reviewer: sngun
ms.openlocfilehash: ec88b8e78c2d73ffa4badbc8ceaa468b4e5169e0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60765996"
---
# <a name="create-an-angular-app-with-azure-cosmos-dbs-api-for-mongodb"></a>Tworzenie aplikacji Angular przy użyciu interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB

Ten wieloczęściowy samouczek pokazuje, jak utworzyć nową aplikację napisaną w środowisku Node.js za pomocą programu Express i platformy Angular, a następnie podłączyć ją do [konta usługi Cosmos skonfigurowanego przy użyciu interfejsu API usługi Cosmos DB dla bazy danych MongoDB](mongodb-introduction.md).

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Cosmos DB można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

W tym wieloczęściowym samouczku opisano następujące zadania:

> [!div class="checklist"]
> * [Tworzenie aplikacji Node.js Express za pomocą interfejsu wiersza polecenia usługi Angular](tutorial-develop-mongodb-nodejs-part2.md)
> * [Tworzenie interfejsu użytkownika przy użyciu usługi Angular](tutorial-develop-mongodb-nodejs-part3.md)
> * [Tworzenie konta usługi Azure Cosmos DB przy użyciu interfejsu wiersza polecenia platformy Azure](tutorial-develop-mongodb-nodejs-part4.md) 
> * [Łączenie z usługą Azure Cosmos DB przy użyciu wtyczki Mongoose](tutorial-develop-mongodb-nodejs-part5.md)
> * [Dodawanie funkcji Post, Put i Delete do aplikacji](tutorial-develop-mongodb-nodejs-part6.md)

Chcesz skompilować tę samą aplikację na platformie React? Zobacz [samouczek platformy React w postaci serii filmów wideo](tutorial-develop-mongodb-react.md).

## <a name="video-walkthrough"></a>Przewodnik wideo

> [!VIDEO https://www.youtube.com/embed/vlZRP0mDabM]

## <a name="finished-project"></a>Zakończony projekt 

Ten samouczek przedstawia instrukcje tworzenia aplikacji krok po kroku. Jeśli chcesz pobrać gotowy projekt, ukończoną aplikację znajdziesz w [repozytorium angular-cosmosdb](https://github.com/Azure-Samples/angular-cosmosdb) w witrynie GitHub.

## <a name="next-steps"></a>Następne kroki

W tej części samouczka zostały wykonane następujące zadania:

> [!div class="checklist"]
> * Zapoznanie się z omówieniem kroków wymaganych do utworzenia aplikacji MEAN.js przy użyciu usługi Azure Cosmos DB. 

Możesz przejść do następnej części samouczka, aby utworzyć aplikację Node.js Express.

> [!div class="nextstepaction"]
> [Tworzenie aplikacji Node.js Express za pomocą interfejsu wiersza polecenia usługi Angular](tutorial-develop-mongodb-nodejs-part2.md)
