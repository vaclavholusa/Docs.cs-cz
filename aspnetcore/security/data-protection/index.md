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
ms.openlocfilehash: b846fb7cb28eeceb8c0bdb47135e1cf014ae08a7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="26d63-103">Ochrana dat v ASP.NET Core: rozhraní API pro příjemce, konfiguraci, rozšiřitelnost rozhraní API a implementace</span><span class="sxs-lookup"><span data-stu-id="26d63-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="26d63-104">Úvod do ochrany dat</span><span class="sxs-lookup"><span data-stu-id="26d63-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="26d63-105">Začínáme s rozhraními API na ochranu dat</span><span class="sxs-lookup"><span data-stu-id="26d63-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="26d63-106">Rozhraní API příjemců</span><span class="sxs-lookup"><span data-stu-id="26d63-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="26d63-107">Přehled rozhraní API příjemců</span><span class="sxs-lookup"><span data-stu-id="26d63-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="26d63-108">Účelové řetězce</span><span class="sxs-lookup"><span data-stu-id="26d63-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="26d63-109">Hierarchie účelů a víceklientská architektura</span><span class="sxs-lookup"><span data-stu-id="26d63-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="26d63-110">Použití funkce hash u hesla</span><span class="sxs-lookup"><span data-stu-id="26d63-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="26d63-111">Omezení životnosti chráněných datových částí</span><span class="sxs-lookup"><span data-stu-id="26d63-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="26d63-112">Zrušení ochrany datových částí s odvolanými klíči</span><span class="sxs-lookup"><span data-stu-id="26d63-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="26d63-113">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="26d63-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="26d63-114">Konfigurace ochrany dat</span><span class="sxs-lookup"><span data-stu-id="26d63-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="26d63-115">Výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="26d63-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="26d63-116">Zásady pro celý počítač</span><span class="sxs-lookup"><span data-stu-id="26d63-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="26d63-117">Scénáře využívající DI</span><span class="sxs-lookup"><span data-stu-id="26d63-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="26d63-118">Rozšiřující rozhraní API</span><span class="sxs-lookup"><span data-stu-id="26d63-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="26d63-119">Rozšiřitelnost základní kryptografie</span><span class="sxs-lookup"><span data-stu-id="26d63-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="26d63-120">Rozšiřitelnost správy klíčů</span><span class="sxs-lookup"><span data-stu-id="26d63-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="26d63-121">Ostatní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="26d63-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="26d63-122">Implementace</span><span class="sxs-lookup"><span data-stu-id="26d63-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="26d63-123">Podrobnosti ověřeného šifrování</span><span class="sxs-lookup"><span data-stu-id="26d63-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="26d63-124">Odvozování podklíčů a ověřené šifrování</span><span class="sxs-lookup"><span data-stu-id="26d63-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="26d63-125">Kontextová záhlaví</span><span class="sxs-lookup"><span data-stu-id="26d63-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="26d63-126">Správa klíčů</span><span class="sxs-lookup"><span data-stu-id="26d63-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="26d63-127">Zprostředkovatelé úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="26d63-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="26d63-128">Šifrování klíčů v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="26d63-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="26d63-129">Neměnnost klíče a změna nastavení</span><span class="sxs-lookup"><span data-stu-id="26d63-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="26d63-130">Formát ukládání klíčů</span><span class="sxs-lookup"><span data-stu-id="26d63-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="26d63-131">Zprostředkovatelé dočasné ochrany dat</span><span class="sxs-lookup"><span data-stu-id="26d63-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="26d63-132">Kompatibilita</span><span class="sxs-lookup"><span data-stu-id="26d63-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="26d63-133">Sdílení souborů cookie mezi aplikací</span><span class="sxs-lookup"><span data-stu-id="26d63-133">Sharing cookies among apps</span></span>](xref:security/data-protection/compatibility/cookie-sharing)

  * [<span data-ttu-id="26d63-134">Nahrazení <machineKey> v ASP.NET</span><span class="sxs-lookup"><span data-stu-id="26d63-134">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
