---
title: Klíč nastavení neměnitelnosti a klíč v ASP.NET Core
author: rick-anderson
description: Další podrobnosti implementace klíče neměnitelnosti ochranu dat ASP.NET jádra rozhraní API.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: e918b00562aca9821de87c38f10242177517d8a5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a><span data-ttu-id="a21ac-103">Klíč nastavení neměnitelnosti a klíč v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a21ac-103">Key immutability and key settings in ASP.NET Core</span></span>

<span data-ttu-id="a21ac-104">Jakmile je objekt trvalý k záložnímu úložišti, její reprezentace navždy vyřešen.</span><span class="sxs-lookup"><span data-stu-id="a21ac-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="a21ac-105">Nová data mohou být přidány do úložiště zálohování, ale může být mutovat nikdy stávající data.</span><span class="sxs-lookup"><span data-stu-id="a21ac-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="a21ac-106">Primárním účelem toto chování je zabránit poškození dat.</span><span class="sxs-lookup"><span data-stu-id="a21ac-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="a21ac-107">Jedním z důsledků toto chování je, že jakmile klíč je zapsán do úložiště zálohování, se nedá změnit.</span><span class="sxs-lookup"><span data-stu-id="a21ac-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="a21ac-108">Jeho vytvoření, aktivace a datum vypršení platnosti nesmí nikdy změnit, i když můžete odvolat pomocí `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="a21ac-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="a21ac-109">Kromě toho jeho základní algoritmické informace, jejich obsah a šifrování na rest vlastnosti jsou také neměnné.</span><span class="sxs-lookup"><span data-stu-id="a21ac-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="a21ac-110">Pokud se vývojáři změní všechna nastavení, která ovlivňuje klíče trvalost, tyto změny se projeví až po příštím klíče se vygeneruje, buď prostřednictvím explicitní volání `IKeyManager.CreateNewKey` nebo přes data protection systému vlastní [automatické klíč generování](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) chování.</span><span class="sxs-lookup"><span data-stu-id="a21ac-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="a21ac-111">Nastavení, které ovlivňují klíče trvalost jsou následující:</span><span class="sxs-lookup"><span data-stu-id="a21ac-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="a21ac-112">Výchozí doba života klíče</span><span class="sxs-lookup"><span data-stu-id="a21ac-112">The default key lifetime</span></span>](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [<span data-ttu-id="a21ac-113">Šifrování klíče v mechanismus rest</span><span class="sxs-lookup"><span data-stu-id="a21ac-113">The key encryption at rest mechanism</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="a21ac-114">Algoritmické informace obsažené v rámci klíč</span><span class="sxs-lookup"><span data-stu-id="a21ac-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="a21ac-115">Pokud potřebujete těchto nastavení se nové starší než další klíč automatické vrácení čas, zvažte provedení explicitní volání `IKeyManager.CreateNewKey` k vytvoření nového klíče.</span><span class="sxs-lookup"><span data-stu-id="a21ac-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="a21ac-116">Nezapomeňte zadat datum explicitní aktivace ({nyní + 2 dny} je obvykle umožňující čas změna rozšířit) a datum vypršení platnosti ve volání.</span><span class="sxs-lookup"><span data-stu-id="a21ac-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="a21ac-117">Všechny aplikace dotykové ovládání úložiště by měl určovat stejným nastavením `IDataProtectionBuilder` rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="a21ac-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="a21ac-118">Vlastnosti trvalou klíč, jinak budou závislé na konkrétní aplikaci, která volá rutiny generování klíče.</span><span class="sxs-lookup"><span data-stu-id="a21ac-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
