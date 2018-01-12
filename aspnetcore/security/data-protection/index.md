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
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="7eb52-104">Ochrana dat v ASP.NET Core: rozhraní API pro příjemce, konfiguraci, rozšiřitelnost rozhraní API a implementace</span><span class="sxs-lookup"><span data-stu-id="7eb52-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="7eb52-105">Úvod do ochrany dat</span><span class="sxs-lookup"><span data-stu-id="7eb52-105">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="7eb52-106">Začínáme s rozhraními API ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="7eb52-106">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="7eb52-107">Rozhraní API příjemců</span><span class="sxs-lookup"><span data-stu-id="7eb52-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="7eb52-108">Přehled rozhraní API příjemce</span><span class="sxs-lookup"><span data-stu-id="7eb52-108">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="7eb52-109">Účel řetězce</span><span class="sxs-lookup"><span data-stu-id="7eb52-109">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="7eb52-110">Hierarchie účelů a víceklientská architektura</span><span class="sxs-lookup"><span data-stu-id="7eb52-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="7eb52-111">Hashování hesel</span><span class="sxs-lookup"><span data-stu-id="7eb52-111">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="7eb52-112">Omezení životnosti chráněných datových částí</span><span class="sxs-lookup"><span data-stu-id="7eb52-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="7eb52-113">Zrušení ochrany datových částí s odvolanými klíči</span><span class="sxs-lookup"><span data-stu-id="7eb52-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="7eb52-114">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="7eb52-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="7eb52-115">Konfigurace ochrany dat</span><span class="sxs-lookup"><span data-stu-id="7eb52-115">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="7eb52-116">Výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="7eb52-116">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="7eb52-117">Zásady pro celý počítač</span><span class="sxs-lookup"><span data-stu-id="7eb52-117">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="7eb52-118">Scénáře využívající DI</span><span class="sxs-lookup"><span data-stu-id="7eb52-118">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="7eb52-119">Rozšiřující rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7eb52-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="7eb52-120">Rozšiřitelnost základní kryptografie</span><span class="sxs-lookup"><span data-stu-id="7eb52-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="7eb52-121">Rozšiřitelnost správy klíčů</span><span class="sxs-lookup"><span data-stu-id="7eb52-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="7eb52-122">Ostatní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7eb52-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="7eb52-123">Implementace</span><span class="sxs-lookup"><span data-stu-id="7eb52-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="7eb52-124">Podrobnosti ověřeného šifrování</span><span class="sxs-lookup"><span data-stu-id="7eb52-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="7eb52-125">Odvození podklíčů a ověřené šifrování</span><span class="sxs-lookup"><span data-stu-id="7eb52-125">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="7eb52-126">Kontextová záhlaví</span><span class="sxs-lookup"><span data-stu-id="7eb52-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="7eb52-127">Správa klíčů</span><span class="sxs-lookup"><span data-stu-id="7eb52-127">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="7eb52-128">Poskytovatelé úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="7eb52-128">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="7eb52-129">Klíče šifrování v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="7eb52-129">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="7eb52-130">Klíče neměnitelnosti a změna nastavení</span><span class="sxs-lookup"><span data-stu-id="7eb52-130">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="7eb52-131">Formát úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="7eb52-131">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="7eb52-132">Zprostředkovatelé dočasné ochrany dat</span><span class="sxs-lookup"><span data-stu-id="7eb52-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="7eb52-133">Kompatibilita</span><span class="sxs-lookup"><span data-stu-id="7eb52-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="7eb52-134">Sdílení souborů cookie mezi aplikacemi</span><span class="sxs-lookup"><span data-stu-id="7eb52-134">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="7eb52-135">Nahrazení <machineKey> v ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7eb52-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
