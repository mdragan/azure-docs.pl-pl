---
title: 'Generowanie i eksportowanie certyfikatów for Point-to-Site: W systemie Linux: CLI: Azure | Microsoft Docs'
description: Tworzenie certyfikatu głównego z podpisem własnym, wyeksportować klucz publiczny i wygenerować certyfikaty klienta przy użyciu systemu Linux (strongSwan) interfejsu wiersza polecenia.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/31/2019
ms.author: cherylmc
ms.openlocfilehash: b673be47d4951adab08f04efc56410095f549356
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60766178"
---
# <a name="generate-and-export-certificates"></a>Generowanie i eksportowanie certyfikatów

Połączenia punkt-lokacja używają certyfikatów do uwierzytelniania. W tym artykule pokazano, jak utworzyć certyfikat główny z podpisem własnym i generowania certyfikatów klienta przy użyciu interfejsu wiersza polecenia systemu Linux i strongSwan. Jeśli potrzebujesz innego certyfikatu instrukcje, zobacz [Powershell](vpn-gateway-certificates-point-to-site.md) lub [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) artykułów. Aby uzyskać informacje o sposobie instalowania strongSwan przy użyciu graficznego interfejsu użytkownika, a nie interfejsu wiersza polecenia, zobacz kroki w [konfiguracji klienta](point-to-site-vpn-client-configuration-azure-cert.md#install) artykułu.

## <a name="generate-and-export"></a>Generowanie i eksportowanie
[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>Kolejne kroki

Kontynuuj konfiguracji punkt-lokacja, aby [tworzenie i instalowanie plików konfiguracji klienta sieci VPN](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).
