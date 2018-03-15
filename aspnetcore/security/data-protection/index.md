---
title: Ochrana dat v ASP.NET Core
author: rick-anderson
description: "Tento dokument slouží jako obsah v různých oblastech ochrany dat ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: e08dea63f012c4a758f2e5561c4930d09cfee0ac
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>Ochrana dat v ASP.NET Core: rozhraní API pro příjemce, konfiguraci, rozšiřitelnost rozhraní API a implementace

* [Úvod do ochrany dat](introduction.md)

* [Začínáme s rozhraními API na ochranu dat](using-data-protection.md)

* [Rozhraní API příjemců](consumer-apis/index.md)

  * [Přehled rozhraní API příjemců](consumer-apis/overview.md)

  * [Účelové řetězce](consumer-apis/purpose-strings.md)

  * [Hierarchie účelů a víceklientská architektura](consumer-apis/purpose-strings-multitenancy.md)

  * [Použití funkce hash u hesla](consumer-apis/password-hashing.md)

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

  * [Odvozování podklíčů a ověřené šifrování](implementation/subkeyderivation.md)

  * [Kontextová záhlaví](implementation/context-headers.md)

  * [Správa klíčů](implementation/key-management.md)

  * [Zprostředkovatelé úložiště klíčů](implementation/key-storage-providers.md)

  * [Šifrování klíčů v klidovém stavu](implementation/key-encryption-at-rest.md)

  * [Neměnnost klíče a změna nastavení](implementation/key-immutability.md)

  * [Formát ukládání klíčů](implementation/key-storage-format.md)

  * [Zprostředkovatelé dočasné ochrany dat](implementation/key-storage-ephemeral.md)

* [Kompatibilita](compatibility/index.md)

  * [Nahrazení <machineKey> v ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
