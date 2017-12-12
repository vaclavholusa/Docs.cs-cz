---
title: "Klíče neměnitelnosti a změna nastavení"
author: rick-anderson
description: "Tento dokument popisuje podrobnosti implementace Komponenta ASP.NET Core data protection klíče neměnitelnosti rozhraní API."
keywords: "ASP.NET Core, ochranu dat klíče neměnitelnosti"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 96860b44b64f241a1bbff2ac8366e0863b1ac10c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="d1e45-104">Klíč neměnitelnosti a změna nastavení</span><span class="sxs-lookup"><span data-stu-id="d1e45-104">Key Immutability and changing settings</span></span>

<span data-ttu-id="d1e45-105">Jakmile je objekt trvalý k záložnímu úložišti, její reprezentace navždy vyřešen.</span><span class="sxs-lookup"><span data-stu-id="d1e45-105">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="d1e45-106">Nová data mohou být přidány do úložiště zálohování, ale může být mutovat nikdy stávající data.</span><span class="sxs-lookup"><span data-stu-id="d1e45-106">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="d1e45-107">Primárním účelem toto chování je zabránit poškození dat.</span><span class="sxs-lookup"><span data-stu-id="d1e45-107">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="d1e45-108">Jedním z důsledků toto chování je, že jakmile klíč je zapsán do úložiště zálohování, se nedá změnit.</span><span class="sxs-lookup"><span data-stu-id="d1e45-108">One consequence of this behavior is that once a key is written to the backing store, it is immutable.</span></span> <span data-ttu-id="d1e45-109">Jeho vytvoření, aktivace a datum vypršení platnosti nesmí nikdy změnit, i když můžete odvolat pomocí `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="d1e45-109">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="d1e45-110">Kromě toho jeho základní algoritmické informace, jejich obsah a šifrování na rest vlastnosti jsou také neměnné.</span><span class="sxs-lookup"><span data-stu-id="d1e45-110">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="d1e45-111">Pokud se vývojáři změní všechna nastavení, která ovlivňuje klíče trvalost, tyto změny se projeví až po příštím klíče se vygeneruje, buď prostřednictvím explicitní volání `IKeyManager.CreateNewKey` nebo přes data protection systému vlastní [automatické klíč generování](key-management.md#data-protection-implementation-key-management) chování.</span><span class="sxs-lookup"><span data-stu-id="d1e45-111">If the developer changes any setting that affects key persistence, those changes will not go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="d1e45-112">Nastavení, které ovlivňují klíče trvalost jsou následující:</span><span class="sxs-lookup"><span data-stu-id="d1e45-112">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="d1e45-113">Výchozí doba života klíče</span><span class="sxs-lookup"><span data-stu-id="d1e45-113">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="d1e45-114">Šifrování klíče v mechanismus rest</span><span class="sxs-lookup"><span data-stu-id="d1e45-114">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="d1e45-115">Algoritmické informace obsažené v rámci klíč</span><span class="sxs-lookup"><span data-stu-id="d1e45-115">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="d1e45-116">Pokud potřebujete těchto nastavení se nové starší než další klíč automatické vrácení čas, zvažte provedení explicitní volání `IKeyManager.CreateNewKey` k vytvoření nového klíče.</span><span class="sxs-lookup"><span data-stu-id="d1e45-116">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="d1e45-117">Nezapomeňte zadat datum explicitní aktivace ({nyní + 2 dny} je obvykle umožňující čas změna rozšířit) a datum vypršení platnosti ve volání.</span><span class="sxs-lookup"><span data-stu-id="d1e45-117">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="d1e45-118">Všechny aplikace dotykové ovládání úložiště by měl určovat stejným nastavením `IDataProtectionBuilder` rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="d1e45-118">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="d1e45-119">Vlastnosti trvalou klíč, jinak budou závislé na konkrétní aplikaci, která volá rutiny generování klíče.</span><span class="sxs-lookup"><span data-stu-id="d1e45-119">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
