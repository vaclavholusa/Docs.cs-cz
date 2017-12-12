---
title: Ochrana dat v ASP.NET Core
author: rick-anderson
description: "Tento dokument slouží jako obsah v různých oblastech ochrany dat ASP.NET Core."
keywords: ASP.NET Core, ochrany dat
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>Ochrana dat v ASP.NET Core: rozhraní API pro příjemce, konfiguraci, rozšiřitelnost rozhraní API a implementace

* [Úvod do ochrany dat](introduction.md)

* [Začínáme s rozhraními API ochrany dat.](using-data-protection.md)

* [Rozhraní API příjemce](consumer-apis/index.md)

  * [Přehled rozhraní API příjemce](consumer-apis/overview.md)

  * [Účel řetězce](consumer-apis/purpose-strings.md)

  * [Účel hierarchie a víceklientské prostředí](consumer-apis/purpose-strings-multitenancy.md)

  * [Hashování hesel](consumer-apis/password-hashing.md)

  * [Omezení délky trvání chráněných datových částí](consumer-apis/limited-lifetime-payloads.md)

  * [Probíhá rušení ochrany datových částí, které byly odvolány jejíž klíče](consumer-apis/dangerous-unprotect.md)

* [Konfigurace](configuration/index.md)

  * [Konfigurace ochrany dat](configuration/overview.md)

  * [Výchozí nastavení](configuration/default-settings.md)

  * [Široké zásady počítače](configuration/machine-wide-policy.md)

  * [Scénáře využívající bez DI](configuration/non-di-scenarios.md)

* [Rozšiřitelnost rozhraní API](extensibility/index.md)

  * [Rozšiřitelnost kryptografie jádra](extensibility/core-crypto.md)

  * [Rozšíření správy klíčů](extensibility/key-management.md)

  * [Ostatní rozhraní API](extensibility/misc-apis.md)

* [Implementace](implementation/index.md)

  * [Podrobnosti o ověřené šifrování](implementation/authenticated-encryption-details.md)

  * [Odvození podklíčů a ověřené šifrování](implementation/subkeyderivation.md)

  * [Kontext hlavičky](implementation/context-headers.md)

  * [Správa klíčů](implementation/key-management.md)

  * [Poskytovatelé úložiště klíčů](implementation/key-storage-providers.md)

  * [Klíče šifrování v klidovém stavu](implementation/key-encryption-at-rest.md)

  * [Klíče neměnitelnosti a změna nastavení](implementation/key-immutability.md)

  * [Formát úložiště klíčů](implementation/key-storage-format.md)

  * [Zprostředkovatelé ochrany dočasných dat](implementation/key-storage-ephemeral.md)

* [Kompatibilita](compatibility/index.md)

  * [Sdílení souborů cookie mezi aplikacemi](compatibility/cookie-sharing.md)

  * [Nahrazení <machineKey> technologie ASP.NET](compatibility/replacing-machinekey.md)
