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
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="2b37c-103">Ochrana dat v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b37c-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="2b37c-104">Úvod do ochrany dat</span><span class="sxs-lookup"><span data-stu-id="2b37c-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="2b37c-105">Začínáme s rozhraními API na ochranu dat</span><span class="sxs-lookup"><span data-stu-id="2b37c-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="2b37c-106">Rozhraní API příjemců</span><span class="sxs-lookup"><span data-stu-id="2b37c-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="2b37c-107">Přehled rozhraní API příjemců</span><span class="sxs-lookup"><span data-stu-id="2b37c-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="2b37c-108">Účelové řetězce</span><span class="sxs-lookup"><span data-stu-id="2b37c-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="2b37c-109">Hierarchie účelů a víceklientská architektura</span><span class="sxs-lookup"><span data-stu-id="2b37c-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="2b37c-110">Hodnota hash hesla</span><span class="sxs-lookup"><span data-stu-id="2b37c-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="2b37c-111">Omezení životnosti chráněných datových částí</span><span class="sxs-lookup"><span data-stu-id="2b37c-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="2b37c-112">Zrušení ochrany datových částí s odvolanými klíči</span><span class="sxs-lookup"><span data-stu-id="2b37c-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="2b37c-113">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="2b37c-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="2b37c-114">Konfigurovat ochranu dat ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b37c-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="2b37c-115">Výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="2b37c-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="2b37c-116">Zásady pro celý počítač</span><span class="sxs-lookup"><span data-stu-id="2b37c-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="2b37c-117">Scénáře využívající DI</span><span class="sxs-lookup"><span data-stu-id="2b37c-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="2b37c-118">Rozšiřující rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2b37c-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="2b37c-119">Rozšiřitelnost základní kryptografie</span><span class="sxs-lookup"><span data-stu-id="2b37c-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="2b37c-120">Rozšiřitelnost správy klíčů</span><span class="sxs-lookup"><span data-stu-id="2b37c-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="2b37c-121">Ostatní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2b37c-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="2b37c-122">Implementace</span><span class="sxs-lookup"><span data-stu-id="2b37c-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="2b37c-123">Podrobnosti ověřeného šifrování</span><span class="sxs-lookup"><span data-stu-id="2b37c-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="2b37c-124">Odvozování podklíčů a ověřené šifrování</span><span class="sxs-lookup"><span data-stu-id="2b37c-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="2b37c-125">Kontextová záhlaví</span><span class="sxs-lookup"><span data-stu-id="2b37c-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="2b37c-126">Správa klíčů</span><span class="sxs-lookup"><span data-stu-id="2b37c-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="2b37c-127">Zprostředkovatelé úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="2b37c-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="2b37c-128">Šifrování klíčů v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="2b37c-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="2b37c-129">Klíče neměnitelnosti a nastavení</span><span class="sxs-lookup"><span data-stu-id="2b37c-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="2b37c-130">Formát ukládání klíčů</span><span class="sxs-lookup"><span data-stu-id="2b37c-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="2b37c-131">Zprostředkovatelé dočasné ochrany dat</span><span class="sxs-lookup"><span data-stu-id="2b37c-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="2b37c-132">Kompatibilita</span><span class="sxs-lookup"><span data-stu-id="2b37c-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="2b37c-133">Nahrazení ASP.NET <machineKey> v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b37c-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
