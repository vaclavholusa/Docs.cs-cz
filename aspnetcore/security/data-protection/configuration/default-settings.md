---
title: "Správa klíčů ochrany dat a doba platnosti v ASP.NET Core"
author: rick-anderson
description: "Další informace o ochranu dat správy klíčů a doba platnosti v ASP.NET Core."
keywords: "ASP.NET Core klíče management, rozhraní DPAPI, ochrany dat, doba platnosti klíče"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ef7dad2a-7029-4ae5-8f06-1fbebedccaa4
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 4f5409acf4d934ced828153ccfd945834d0f1718
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a><span data-ttu-id="e3009-104">Správa klíčů ochrany dat a doba platnosti v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3009-104">Data Protection key management and lifetime in ASP.NET Core</span></span>

<span data-ttu-id="e3009-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e3009-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="key-management"></a><span data-ttu-id="e3009-106">Správa klíčů</span><span class="sxs-lookup"><span data-stu-id="e3009-106">Key management</span></span>

<span data-ttu-id="e3009-107">Aplikace se pokusí zjistit jeho provozní prostředí a zpracování konfigurace klíče svoje vlastní.</span><span class="sxs-lookup"><span data-stu-id="e3009-107">The app attempts to detect its operational environment and handle key configuration on its own.</span></span>

1. <span data-ttu-id="e3009-108">Pokud je aplikace hostovaná v [Azure Apps](https://azure.microsoft.com/services/app-service/), klíče jsou nastavené jako trvalé k *%HOME%\ASP.NET\DataProtection-Keys* složky.</span><span class="sxs-lookup"><span data-stu-id="e3009-108">If the app is hosted in [Azure Apps](https://azure.microsoft.com/services/app-service/), keys are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="e3009-109">Tato složka je zálohovaný díky síťového úložiště a se synchronizují napříč všechny počítače, které hostují aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e3009-109">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span>
   * <span data-ttu-id="e3009-110">Klíče nejsou chráněné v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="e3009-110">Keys aren't protected at rest.</span></span>
   * <span data-ttu-id="e3009-111">*DataProtection klíče* složky poskytuje prstenec klíč na všechny instance aplikace v jednom nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="e3009-111">The *DataProtection-Keys* folder supplies the key ring to all instances of an app in a single deployment slot.</span></span>
   * <span data-ttu-id="e3009-112">Samostatné nasazovací sloty, jako je například pracovní a provozní, Nesdílejte prstenec klíč.</span><span class="sxs-lookup"><span data-stu-id="e3009-112">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span> <span data-ttu-id="e3009-113">Při výměně mezi sloty nasazení, například odkládací pracovní do produkčního prostředí nebo použití A / B testování, všech aplikací pomocí funkce Ochrana dat nebude moci dešifrovat uložená data pomocí klíče prstenec uvnitř předchozí pozici.</span><span class="sxs-lookup"><span data-stu-id="e3009-113">When you swap between deployment slots, for example swapping Staging to Production or using A/B testing, any app using Data Protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="e3009-114">To vede k uživatelé protokolována mimo aplikaci, která používá standardní ověřování souborů cookie ASP.NET Core, protože využívá ochranu dat k ochraně jeho soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="e3009-114">This leads to users being logged out of an app that uses the standard ASP.NET Core cookie authentication, as it uses Data Protection to protect its cookies.</span></span> <span data-ttu-id="e3009-115">Vyžadujete, aby okruhy klíč nezávislé na pozici,-li použít zprostředkovatel vnější prstenec klíč, například Azure Blob Storage, Azure Key Vault, úložiště SQL nebo Redis cache.</span><span class="sxs-lookup"><span data-stu-id="e3009-115">If you desire slot-independent key rings, use an external key ring provider, such as Azure Blob Storage, Azure Key Vault, a SQL store, or Redis cache.</span></span>

1. <span data-ttu-id="e3009-116">Pokud profil uživatele je k dispozici, klíče, jsou nastavené jako trvalé k *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* složky.</span><span class="sxs-lookup"><span data-stu-id="e3009-116">If the user profile is available, keys are persisted to the *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="e3009-117">Pokud operační systém Windows, klíče jsou zašifrovaná přinejmenším pomocí rozhraní DPAPI.</span><span class="sxs-lookup"><span data-stu-id="e3009-117">If the operating system is Windows, the keys are encrypted at rest using DPAPI.</span></span>

1. <span data-ttu-id="e3009-118">Pokud je aplikace hostované ve službě IIS, jsou trvalé klíče registru HKLM klíč speciální registru, který je ACLed pouze pro účet pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="e3009-118">If the app is hosted in IIS, keys are persisted to the HKLM registry in a special registry key that is ACLed only to the worker process account.</span></span> <span data-ttu-id="e3009-119">Klíče jsou zašifrovaná přinejmenším pomocí rozhraní DPAPI.</span><span class="sxs-lookup"><span data-stu-id="e3009-119">Keys are encrypted at rest using DPAPI.</span></span>

1. <span data-ttu-id="e3009-120">Pokud žádná z těchto podmínek shodují, klíče nejsou trvalé mimo aktuální proces.</span><span class="sxs-lookup"><span data-stu-id="e3009-120">If none of these conditions match, keys aren't persisted outside of the current process.</span></span> <span data-ttu-id="e3009-121">Když se proces ukončí, všechny vygenerované klíče jsou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="e3009-121">When the process shuts down, all generated keys are lost.</span></span>

<span data-ttu-id="e3009-122">Vývojář je vždy plně pod kontrolou a můžete přepsat, jak a kde jsou uložené klíče.</span><span class="sxs-lookup"><span data-stu-id="e3009-122">The developer is always in full control and can override how and where keys are stored.</span></span> <span data-ttu-id="e3009-123">První tři výše uvedených možností by měl poskytovat dobrý výchozí hodnoty pro většinu aplikací podobná jak ASP.NET  **\<machineKey >** automatické generování rutiny fungovaly v minulosti.</span><span class="sxs-lookup"><span data-stu-id="e3009-123">The first three options above should provide good defaults for most apps similar to how the ASP.NET **\<machineKey>** auto-generation routines worked in the past.</span></span> <span data-ttu-id="e3009-124">Možnost konečné, záložní je jenom scénáře, který vyžaduje vývojáři zadejte [konfigurace](xref:security/data-protection/configuration/overview) předem, pokud chtějí klíče trvalost, ale tento záložní dochází pouze ve výjimečných případech.</span><span class="sxs-lookup"><span data-stu-id="e3009-124">The final, fallback option is the only scenario that requires the developer to specify [configuration](xref:security/data-protection/configuration/overview) upfront if they want key persistence, but this fallback only occurs in rare situations.</span></span>

<span data-ttu-id="e3009-125">Při hostování v kontejner Docker, klíče by měl natrvalo ve složce, kterou je svazek Docker (sdíleného svazku nebo svazku připojené hostitele, který potrvají nad rámec kontejneru životnost) nebo do externího poskytovatele, jako například [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) nebo [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="e3009-125">When hosting in a Docker container, keys should be persisted in a folder that's a Docker volume (a shared volume or a host-mounted volume that persists beyond the container's lifetime) or in an external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span> <span data-ttu-id="e3009-126">Externího poskytovatele je užitečný ve scénářích, webové farmy také pokud aplikace nemá přístup k sdílené síťové svazek (viz [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) informace).</span><span class="sxs-lookup"><span data-stu-id="e3009-126">An external provider is also useful in web farm scenarios if apps can't access a shared network volume (see [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) for more information).</span></span>

> [!WARNING]
> <span data-ttu-id="e3009-127">Pokud vývojář přepsání pravidel uvedených výše a bodů systému ochrany dat na konkrétní úložiště klíčů, je zakázané automatické šifrování klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="e3009-127">If the developer overrides the rules outlined above and points the Data Protection system at a specific key repository, automatic encryption of keys at rest is disabled.</span></span> <span data-ttu-id="e3009-128">Může být znovu zapnout ochranu v rest [konfigurace](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="e3009-128">At-rest protection can be re-enabled via [configuration](xref:security/data-protection/configuration/overview).</span></span>

## <a name="key-lifetime"></a><span data-ttu-id="e3009-129">Doba platnosti klíče</span><span class="sxs-lookup"><span data-stu-id="e3009-129">Key lifetime</span></span>

<span data-ttu-id="e3009-130">Klíče měly životnost-90denní ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e3009-130">Keys have a 90-day lifetime by default.</span></span> <span data-ttu-id="e3009-131">Když vyprší platnost klíče, aplikace se automaticky vygeneruje nový klíč a nastaví nového klíče jako aktivní klíč.</span><span class="sxs-lookup"><span data-stu-id="e3009-131">When a key expires, the app automatically generates a new key and sets the new key as the active key.</span></span> <span data-ttu-id="e3009-132">Tak dlouho, dokud vyřazeno klíče zůstávají v systému, aplikace mohly dešifrovat všechna data chráněná s nimi.</span><span class="sxs-lookup"><span data-stu-id="e3009-132">As long as retired keys remain on the system, your app can decrypt any data protected with them.</span></span> <span data-ttu-id="e3009-133">V tématu [Správa klíčů](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e3009-133">See [key management](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) for more information.</span></span>

## <a name="default-algorithms"></a><span data-ttu-id="e3009-134">Výchozí algoritmy</span><span class="sxs-lookup"><span data-stu-id="e3009-134">Default algorithms</span></span>

<span data-ttu-id="e3009-135">Použitý algoritmus ochrany výchozí datové části pro utajení a HMACSHA256 je AES-256-CBC pro pravosti.</span><span class="sxs-lookup"><span data-stu-id="e3009-135">The default payload protection algorithm used is AES-256-CBC for confidentiality and HMACSHA256 for authenticity.</span></span> <span data-ttu-id="e3009-136">Hlavní klíč 512 bitů, změnit každých 90 dní, slouží k dvě dílčí klíče používané pro tyto algoritmy na základě za datové odvozena.</span><span class="sxs-lookup"><span data-stu-id="e3009-136">A 512-bit master key, changed every 90 days, is used to derive the two sub-keys used for these algorithms on a per-payload basis.</span></span> <span data-ttu-id="e3009-137">V tématu [podklíčů odvození](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e3009-137">See [subkey derivation](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) for more information.</span></span>

## <a name="see-also"></a><span data-ttu-id="e3009-138">Viz také</span><span class="sxs-lookup"><span data-stu-id="e3009-138">See also</span></span>

* [<span data-ttu-id="e3009-139">Rozšíření správy klíčů</span><span class="sxs-lookup"><span data-stu-id="e3009-139">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)