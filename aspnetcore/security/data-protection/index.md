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
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>Ochrana dat v ASP.NET Core: rozhraní API pro příjemce, konfiguraci, rozšiřitelnost rozhraní API a implementace

* [Úvod do ochrany dat](introduction.md)

* [Začínáme s rozhraními API ochrany dat.](using-data-protection.md)

* [Rozhraní API příjemců](consumer-apis/index.md)

  * [Přehled rozhraní API příjemce](consumer-apis/overview.md)

  * [Účel řetězce](consumer-apis/purpose-strings.md)

  * [Hierarchie účelů a víceklientská architektura](consumer-apis/purpose-strings-multitenancy.md)

  * [Hashování hesel](consumer-apis/password-hashing.md)

  * [Omezení životnosti chráněných datových částí](consumer-apis/limited-lifetime-payloads.md)

  * [Zrušení ochrany datových částí s odvolanými klíči](consumer-apis/dangerous-unprotect.md)

* [Konfigurace](configuration/index.md)

  * [Konfigurace ochrany dat](configuration/overview.md)

  * [Výchozí nastavení](configuration/default-settings.md)

  * [Zásady pro celý počítač](configuration/machine-wide-policy.md)

  * [Scénáře využívající DI](configuration/non-di-scenarios.md)

* [Rozšiřující rozhraní API](extensibility/index.md)

  * [Rozšiřitelnost základní kryptografie](extensibility/core-crypto.md)

  * [Rozšiřitelnost správy klíčů](extensibility/key-management.md)

  * [Ostatní rozhraní API](extensibility/misc-apis.md)

* [Implementace](implementation/index.md)

  * [Podrobnosti ověřeného šifrování](implementation/authenticated-encryption-details.md)

  * [Odvození podklíčů a ověřené šifrování](implementation/subkeyderivation.md)

  * [Kontextová záhlaví](implementation/context-headers.md)

  * [Správa klíčů](implementation/key-management.md)

  * [Poskytovatelé úložiště klíčů](implementation/key-storage-providers.md)

  * [Klíče šifrování v klidovém stavu](implementation/key-encryption-at-rest.md)

  * [Klíče neměnitelnosti a změna nastavení](implementation/key-immutability.md)

  * [Formát úložiště klíčů](implementation/key-storage-format.md)

  * [Zprostředkovatelé dočasné ochrany dat](implementation/key-storage-ephemeral.md)

* [Kompatibilita](compatibility/index.md)

  * [Sdílení souborů cookie mezi aplikacemi](compatibility/cookie-sharing.md)

  * [Nahrazení <machineKey> v ASP.NET](compatibility/replacing-machinekey.md)
