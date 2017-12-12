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
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="5e62c-104">Ochrana dat v ASP.NET Core: rozhraní API pro příjemce, konfiguraci, rozšiřitelnost rozhraní API a implementace</span><span class="sxs-lookup"><span data-stu-id="5e62c-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="5e62c-105">Úvod do ochrany dat</span><span class="sxs-lookup"><span data-stu-id="5e62c-105">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="5e62c-106">Začínáme s rozhraními API ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="5e62c-106">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="5e62c-107">Rozhraní API příjemce</span><span class="sxs-lookup"><span data-stu-id="5e62c-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="5e62c-108">Přehled rozhraní API příjemce</span><span class="sxs-lookup"><span data-stu-id="5e62c-108">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="5e62c-109">Účel řetězce</span><span class="sxs-lookup"><span data-stu-id="5e62c-109">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="5e62c-110">Účel hierarchie a víceklientské prostředí</span><span class="sxs-lookup"><span data-stu-id="5e62c-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="5e62c-111">Hashování hesel</span><span class="sxs-lookup"><span data-stu-id="5e62c-111">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="5e62c-112">Omezení délky trvání chráněných datových částí</span><span class="sxs-lookup"><span data-stu-id="5e62c-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="5e62c-113">Probíhá rušení ochrany datových částí, které byly odvolány jejíž klíče</span><span class="sxs-lookup"><span data-stu-id="5e62c-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="5e62c-114">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="5e62c-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="5e62c-115">Konfigurace ochrany dat</span><span class="sxs-lookup"><span data-stu-id="5e62c-115">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="5e62c-116">Výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="5e62c-116">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="5e62c-117">Široké zásady počítače</span><span class="sxs-lookup"><span data-stu-id="5e62c-117">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="5e62c-118">Scénáře využívající bez DI</span><span class="sxs-lookup"><span data-stu-id="5e62c-118">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="5e62c-119">Rozšiřitelnost rozhraní API</span><span class="sxs-lookup"><span data-stu-id="5e62c-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="5e62c-120">Rozšiřitelnost kryptografie jádra</span><span class="sxs-lookup"><span data-stu-id="5e62c-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="5e62c-121">Rozšíření správy klíčů</span><span class="sxs-lookup"><span data-stu-id="5e62c-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="5e62c-122">Ostatní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="5e62c-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="5e62c-123">Implementace</span><span class="sxs-lookup"><span data-stu-id="5e62c-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="5e62c-124">Podrobnosti o ověřené šifrování</span><span class="sxs-lookup"><span data-stu-id="5e62c-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="5e62c-125">Odvození podklíčů a ověřené šifrování</span><span class="sxs-lookup"><span data-stu-id="5e62c-125">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="5e62c-126">Kontext hlavičky</span><span class="sxs-lookup"><span data-stu-id="5e62c-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="5e62c-127">Správa klíčů</span><span class="sxs-lookup"><span data-stu-id="5e62c-127">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="5e62c-128">Poskytovatelé úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="5e62c-128">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="5e62c-129">Klíče šifrování v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="5e62c-129">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="5e62c-130">Klíče neměnitelnosti a změna nastavení</span><span class="sxs-lookup"><span data-stu-id="5e62c-130">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="5e62c-131">Formát úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="5e62c-131">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="5e62c-132">Zprostředkovatelé ochrany dočasných dat</span><span class="sxs-lookup"><span data-stu-id="5e62c-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="5e62c-133">Kompatibilita</span><span class="sxs-lookup"><span data-stu-id="5e62c-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="5e62c-134">Sdílení souborů cookie mezi aplikacemi</span><span class="sxs-lookup"><span data-stu-id="5e62c-134">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="5e62c-135">Nahrazení <machineKey> technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5e62c-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
