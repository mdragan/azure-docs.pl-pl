---
title: Jak pisać procedury składowane i wyzwalacze przy użyciu interfejsu API zapytań języka JavaScript w usłudze Azure Cosmos DB
description: Dowiedz się, jak pisać procedury składowane i wyzwalacze przy użyciu interfejsu API zapytań języka JavaScript w usłudze Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: f465ac91936b766d2c19ea8efd67b3acc8df6d75
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243942"
---
# <a name="how-to-write-stored-procedures-and-triggers-in-azure-cosmos-db-by-using-the-javascript-query-api"></a>Jak pisać procedury składowane i wyzwalacze przy użyciu interfejsu API zapytań języka JavaScript w usłudze Azure Cosmos DB

Usługa Azure Cosmos DB umożliwia wykonywanie zapytań zoptymalizowanych przy użyciu płynnego interfejsu języka JavaScript bez konieczności znajomości języka SQL, co może służyć do pisania procedur składowanych i wyzwalaczy. Aby dowiedzieć się więcej na temat obsługi interfejsu API zapytań języka JavaScript w usłudze Azure Cosmos DB, zobacz artykuł [Praca ze zintegrowanym interfejsem API zapytań języka JavaScript w usłudze Azure Cosmos DB](javascript-query-api.md).

## <a id="stored-procedures"></a>Procedura składowana używająca interfejsu API zapytań języka JavaScript

Poniższy przykładowy kod pokazuje użycie interfejsu API zapytań języka JavaScript w kontekście procedury składowanej. Procedura składowana wstawia element usługi Azure Cosmos DB, który jest określony przez parametr wejściowy, i aktualizuje dokument metadanych przy użyciu metody `__.filter()` z właściwościami minSize, maxSize i totalSize określonymi na podstawie właściwości rozmiaru elementu wejściowego.

> [!NOTE]
> `__` (podwójne podkreślenie) jest aliasem funkcji `getContext().getCollection()` podczas korzystania z interfejsu API zapytań języka JavaScript.

```javascript
/**
 * Insert an item and update metadata doc: minSize, maxSize, totalSize based on item.size.
 */
function insertDocumentAndUpdateMetadata(item) {
  // HTTP error codes sent to our callback function by CosmosDB server.
  var ErrorCode = {
    RETRY_WITH: 449,
  }

  var isAccepted = __.createDocument(__.getSelfLink(), item, {}, function(err, item, options) {
    if (err) throw err;

    // Check the item (ignore items with invalid/zero size and metadata itself) and call updateMetadata.
    if (!item.isMetadata && item.size > 0) {
      // Get the metadata. We keep it in the same container. it's the only item that has .isMetadata = true.
      var result = __.filter(function(x) {
        return x.isMetadata === true
      }, function(err, feed, options) {
        if (err) throw err;

        // We assume that metadata item was pre-created and must exist when this script is called.
        if (!feed || !feed.length) throw new Error("Failed to find the metadata item.");

        // The metadata item.
        var metaItem = feed[0];

        // Update metaDoc.minSize:
        // for 1st document use doc.Size, for all the rest see if it's less than last min.
        if (metaItem.minSize == 0) metaItem.minSize = item.size;
        else metaItem.minSize = Math.min(metaItem.minSize, item.size);

        // Update metaItem.maxSize.
        metaItem.maxSize = Math.max(metaItem.maxSize, item.size);

        // Update metaItem.totalSize.
        metaItem.totalSize += item.size;

        // Update/replace the metadata item in the store.
        var isAccepted = __.replaceDocument(metaItem._self, metaItem, function(err) {
          if (err) throw err;
          // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again
          //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
          //       We have to take care of that on the client side.
        });
        if (!isAccepted) throw new Error("replaceDocument(metaItem) returned false.");
      });
      if (!result.isAccepted) throw new Error("filter for metaItem returned false.");
    }
  });
  if (!isAccepted) throw new Error("createDocument(actual item) returned false.");
}
```

## <a name="next-steps"></a>Kolejne kroki

Zapoznaj się z poniższymi artykułami, aby dowiedzieć się więcej o procedurach składowanych, wyzwalaczach i funkcjach zdefiniowanych przez użytkownika w usłudze Azure Cosmos DB:

* [Jak pracować z procedurami składowanymi, wyzwalaczami i funkcjami zdefiniowanymi przez użytkownika w usłudze Azure Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md)

* [Jak rejestrować procedury składowane i używać ich w usłudze Azure Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md#stored-procedures)

* Jak rejestrować [wyzwalacze wykonywane przed operacją](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) i [wyzwalacze wykonywane po operacji](how-to-use-stored-procedures-triggers-udfs.md#post-triggers) i używać ich w usłudze Azure Cosmos DB

* [Jak rejestrować funkcje zdefiniowane przez użytkownika i używać ich w usłudze Azure Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md#udfs)

* [Synthetic partition keys in Azure Cosmos DB (Syntetyczne klucze partycji w usłudze Azure Cosmos DB)](synthetic-partition-keys.md)
