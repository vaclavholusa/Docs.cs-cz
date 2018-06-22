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
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="ef1cc-103">Ochrana dat v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef1cc-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="ef1cc-104">Úvod do ochrany dat</span><span class="sxs-lookup"><span data-stu-id="ef1cc-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="ef1cc-105">Začínáme s rozhraními API na ochranu dat</span><span class="sxs-lookup"><span data-stu-id="ef1cc-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="ef1cc-106">Rozhraní API příjemců</span><span class="sxs-lookup"><span data-stu-id="ef1cc-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="ef1cc-107">Přehled rozhraní API příjemců</span><span class="sxs-lookup"><span data-stu-id="ef1cc-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="ef1cc-108">Účelové řetězce</span><span class="sxs-lookup"><span data-stu-id="ef1cc-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="ef1cc-109">Hierarchie účelů a víceklientská architektura</span><span class="sxs-lookup"><span data-stu-id="ef1cc-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="ef1cc-110">Určení hodnoty hash hesel</span><span class="sxs-lookup"><span data-stu-id="ef1cc-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="ef1cc-111">Omezení životnosti chráněných datových částí</span><span class="sxs-lookup"><span data-stu-id="ef1cc-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="ef1cc-112">Zrušení ochrany datových částí s odvolanými klíči</span><span class="sxs-lookup"><span data-stu-id="ef1cc-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="ef1cc-113">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="ef1cc-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="ef1cc-114">Konfigurovat ochranu dat ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef1cc-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="ef1cc-115">Výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="ef1cc-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="ef1cc-116">Zásady pro celý počítač</span><span class="sxs-lookup"><span data-stu-id="ef1cc-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="ef1cc-117">Scénáře využívající DI</span><span class="sxs-lookup"><span data-stu-id="ef1cc-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="ef1cc-118">Rozšiřující rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef1cc-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="ef1cc-119">Rozšiřitelnost základní kryptografie</span><span class="sxs-lookup"><span data-stu-id="ef1cc-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="ef1cc-120">Rozšiřitelnost správy klíčů</span><span class="sxs-lookup"><span data-stu-id="ef1cc-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="ef1cc-121">Ostatní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef1cc-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="ef1cc-122">Implementace</span><span class="sxs-lookup"><span data-stu-id="ef1cc-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="ef1cc-123">Podrobnosti ověřeného šifrování</span><span class="sxs-lookup"><span data-stu-id="ef1cc-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="ef1cc-124">Odvozování podklíčů a ověřené šifrování</span><span class="sxs-lookup"><span data-stu-id="ef1cc-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="ef1cc-125">Kontextová záhlaví</span><span class="sxs-lookup"><span data-stu-id="ef1cc-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="ef1cc-126">Správa klíčů</span><span class="sxs-lookup"><span data-stu-id="ef1cc-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="ef1cc-127">Zprostředkovatelé úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="ef1cc-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="ef1cc-128">Šifrování klíčů v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="ef1cc-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="ef1cc-129">Neměnnost klíče a nastavení</span><span class="sxs-lookup"><span data-stu-id="ef1cc-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="ef1cc-130">Formát ukládání klíčů</span><span class="sxs-lookup"><span data-stu-id="ef1cc-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="ef1cc-131">Zprostředkovatelé dočasné ochrany dat</span><span class="sxs-lookup"><span data-stu-id="ef1cc-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="ef1cc-132">Kompatibilita</span><span class="sxs-lookup"><span data-stu-id="ef1cc-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="ef1cc-133">Nahrazení ASP.NET <machineKey> v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef1cc-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
