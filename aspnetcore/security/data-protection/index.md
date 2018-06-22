---
title: Ochrana dat v ASP.NET Core
author: rick-anderson
description: Tento dokument slouží jako obsah v různých oblastech ochrany dat ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277121"
---
# <a name="data-protection-in-aspnet-core"></a>Ochrana dat v ASP.NET Core

* [Úvod do ochrany dat](xref:security/data-protection/introduction)

* [Začínáme s rozhraními API na ochranu dat](xref:security/data-protection/using-data-protection)

* [Rozhraní API příjemců](xref:security/data-protection/consumer-apis/index)

  * [Přehled rozhraní API příjemců](xref:security/data-protection/consumer-apis/overview)

  * [Účelové řetězce](xref:security/data-protection/consumer-apis/purpose-strings)

  * [Hierarchie účelů a víceklientská architektura](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [Určení hodnoty hash hesel](xref:security/data-protection/consumer-apis/password-hashing)

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

  * [Neměnnost klíče a nastavení](xref:security/data-protection/implementation/key-immutability)

  * [Formát ukládání klíčů](xref:security/data-protection/implementation/key-storage-format)

  * [Zprostředkovatelé dočasné ochrany dat](xref:security/data-protection/implementation/key-storage-ephemeral)

* [Kompatibilita](xref:security/data-protection/compatibility/index)

  * [Nahrazení ASP.NET <machineKey> v ASP.NET Core](xref:security/data-protection/compatibility/replacing-machinekey)
