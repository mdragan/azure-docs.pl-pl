---
ms.openlocfilehash: 52dfbfca5f79a7f92848ea39eddc00aa10f05ff1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67182947"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Znajdź kotwica przestrzenne chmury

Możliwość zlokalizować kotwica przestrzenne wcześniej przekazane chmury jest jednym z podstawowe przyczyny przy użyciu biblioteki Azure przestrzenne kotwic. Aby zlokalizować przestrzenne kotwic chmury, musisz znać ich identyfikatorów. Identyfikatory zakotwiczeń mogą być przechowywane w usłudze zaplecza aplikacji i jest dostępny dla wszystkich urządzeń, którzy mogą poprawnie uwierzytelnić się do niego. Przykładem tego zobacz [samouczka: Udostępniaj przestrzenne zakotwiczenia między urządzeniami](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Utwórz wystąpienie `AnchorLocateCriteria` obiektu, należy ustawić identyfikatorów, czego szukasz i wywoływać `CreateWatcher` metody w sesji, zapewniając Twojej `AnchorLocateCriteria`.
