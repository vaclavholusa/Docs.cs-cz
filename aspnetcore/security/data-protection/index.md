---
title: Ochrana dat v ASP.NET Core
author: rick-anderson
description: "Tento dokument slouží jako obsah v různých oblastech ochrany dat ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 151385964d877fc9eadaa219320e5f5a195164e4
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="211a5-103">Ochrana dat v ASP.NET Core: rozhraní API pro příjemce, konfiguraci, rozšiřitelnost rozhraní API a implementace</span><span class="sxs-lookup"><span data-stu-id="211a5-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="211a5-104">Úvod do ochrany dat</span><span class="sxs-lookup"><span data-stu-id="211a5-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="211a5-105">Začínáme s rozhraními API na ochranu dat</span><span class="sxs-lookup"><span data-stu-id="211a5-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="211a5-106">Rozhraní API příjemců</span><span class="sxs-lookup"><span data-stu-id="211a5-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="211a5-107">Přehled rozhraní API příjemců</span><span class="sxs-lookup"><span data-stu-id="211a5-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="211a5-108">Účelové řetězce</span><span class="sxs-lookup"><span data-stu-id="211a5-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="211a5-109">Hierarchie účelů a víceklientská architektura</span><span class="sxs-lookup"><span data-stu-id="211a5-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="211a5-110">Použití funkce hash u hesla</span><span class="sxs-lookup"><span data-stu-id="211a5-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="211a5-111">Omezení životnosti chráněných datových částí</span><span class="sxs-lookup"><span data-stu-id="211a5-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="211a5-112">Zrušení ochrany datových částí s odvolanými klíči</span><span class="sxs-lookup"><span data-stu-id="211a5-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="211a5-113">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="211a5-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="211a5-114">Konfigurace ochrany dat</span><span class="sxs-lookup"><span data-stu-id="211a5-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="211a5-115">Výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="211a5-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="211a5-116">Zásady pro celý počítač</span><span class="sxs-lookup"><span data-stu-id="211a5-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="211a5-117">Scénáře využívající DI</span><span class="sxs-lookup"><span data-stu-id="211a5-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="211a5-118">Rozšiřující rozhraní API</span><span class="sxs-lookup"><span data-stu-id="211a5-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="211a5-119">Rozšiřitelnost základní kryptografie</span><span class="sxs-lookup"><span data-stu-id="211a5-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="211a5-120">Rozšiřitelnost správy klíčů</span><span class="sxs-lookup"><span data-stu-id="211a5-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="211a5-121">Ostatní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="211a5-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="211a5-122">Implementace</span><span class="sxs-lookup"><span data-stu-id="211a5-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="211a5-123">Podrobnosti ověřeného šifrování</span><span class="sxs-lookup"><span data-stu-id="211a5-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="211a5-124">Odvozování podklíčů a ověřené šifrování</span><span class="sxs-lookup"><span data-stu-id="211a5-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="211a5-125">Kontextová záhlaví</span><span class="sxs-lookup"><span data-stu-id="211a5-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="211a5-126">Správa klíčů</span><span class="sxs-lookup"><span data-stu-id="211a5-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="211a5-127">Zprostředkovatelé úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="211a5-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="211a5-128">Šifrování klíčů v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="211a5-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="211a5-129">Neměnnost klíče a změna nastavení</span><span class="sxs-lookup"><span data-stu-id="211a5-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="211a5-130">Formát ukládání klíčů</span><span class="sxs-lookup"><span data-stu-id="211a5-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="211a5-131">Zprostředkovatelé dočasné ochrany dat</span><span class="sxs-lookup"><span data-stu-id="211a5-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="211a5-132">Kompatibilita</span><span class="sxs-lookup"><span data-stu-id="211a5-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="211a5-133">Sdílení souborů cookie mezi aplikací</span><span class="sxs-lookup"><span data-stu-id="211a5-133">Sharing cookies among apps</span></span>](xref:security/data-protection/compatibility/cookie-sharing)

  * [<span data-ttu-id="211a5-134">Nahrazení <machineKey> v ASP.NET</span><span class="sxs-lookup"><span data-stu-id="211a5-134">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
