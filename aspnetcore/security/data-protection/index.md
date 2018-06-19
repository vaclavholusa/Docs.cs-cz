---
title: Ochrana dat v ASP.NET Core
author: rick-anderson
description: Tento dokument slouží jako obsah v různých oblastech ochrany dat ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071692"
---
# <a name="data-protection-in-aspnet-core"></a>Ochrana dat v ASP.NET Core

* [Úvod do ochrany dat](xref:security/data-protection/introduction)

* [Začínáme s rozhraními API na ochranu dat](xref:security/data-protection/using-data-protection)

* [Rozhraní API příjemců](xref:security/data-protection/consumer-apis/index)

  * [Přehled rozhraní API příjemců](xref:security/data-protection/consumer-apis/overview)

  * [Účelové řetězce](xref:security/data-protection/consumer-apis/purpose-strings)

  * [Hierarchie účelů a víceklientská architektura](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [Hodnota hash hesla](xref:security/data-protection/consumer-apis/password-hashing)

  * [Omezení životnosti chráněných datových částí](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [Zrušení ochrany datových částí s odvolanými klíči](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [Konfigurace](xref:security/data-protection/configuration/index)

  * [Konfigurovat ochranu dat ASP.NET Core](xref:security/data-protection/configuration/overview)

  * [Výchozí nastavení](xref:security/data-protection/configuration/default-settings)

  * [Zásady pro celý počítač](xref:security/data-protection/configuration/machine-wide-policy)

  * [Scénáře využívající DI](xref:security/data-protection/configuration/non-di-scenarios)

* [Rozšiřující rozhraní API](xref:security/data-protection/extensibility/index)

  * [Rozšiřitelnost základní kryptografie](xref:security/data-protection/extensibility/core-crypto)

  * [Rozšiřitelnost správy klíčů](xref:security/data-protection/extensibility/key-management)

  * [Ostatní rozhraní API](xref:security/data-protection/extensibility/misc-apis)

* [Implementace](xref:security/data-protection/implementation/index)

  * [Podrobnosti ověřeného šifrování](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [Odvozování podklíčů a ověřené šifrování](xref:security/data-protection/implementation/subkeyderivation)

  * [Kontextová záhlaví](xref:security/data-protection/implementation/context-headers)

  * [Správa klíčů](xref:security/data-protection/implementation/key-management)

  * [Zprostředkovatelé úložiště klíčů](xref:security/data-protection/implementation/key-storage-providers)

  * [Šifrování klíčů v klidovém stavu](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [Klíče neměnitelnosti a nastavení](xref:security/data-protection/implementation/key-immutability)

  * [Formát ukládání klíčů](xref:security/data-protection/implementation/key-storage-format)

  * [Zprostředkovatelé dočasné ochrany dat](xref:security/data-protection/implementation/key-storage-ephemeral)

* [Kompatibilita](xref:security/data-protection/compatibility/index)

  * [Nahrazení ASP.NET <machineKey> v ASP.NET Core](xref:security/data-protection/compatibility/replacing-machinekey)
