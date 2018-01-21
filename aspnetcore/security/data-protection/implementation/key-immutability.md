---
title: "Klíče neměnitelnosti a změna nastavení"
author: rick-anderson
description: "Tento dokument popisuje podrobnosti implementace Komponenta ASP.NET Core data protection klíče neměnitelnosti rozhraní API."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 8e46e634266fa5f082c47f3be306009eb54bcbcc
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="42f6f-103">Klíč neměnitelnosti a změna nastavení</span><span class="sxs-lookup"><span data-stu-id="42f6f-103">Key Immutability and changing settings</span></span>

<span data-ttu-id="42f6f-104">Jakmile je objekt trvalý k záložnímu úložišti, její reprezentace navždy vyřešen.</span><span class="sxs-lookup"><span data-stu-id="42f6f-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="42f6f-105">Nová data mohou být přidány do úložiště zálohování, ale může být mutovat nikdy stávající data.</span><span class="sxs-lookup"><span data-stu-id="42f6f-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="42f6f-106">Primárním účelem toto chování je zabránit poškození dat.</span><span class="sxs-lookup"><span data-stu-id="42f6f-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="42f6f-107">Jedním z důsledků toto chování je, že jakmile klíč je zapsán do úložiště zálohování, se nedá změnit.</span><span class="sxs-lookup"><span data-stu-id="42f6f-107">One consequence of this behavior is that once a key is written to the backing store, it is immutable.</span></span> <span data-ttu-id="42f6f-108">Jeho vytvoření, aktivace a datum vypršení platnosti nesmí nikdy změnit, i když můžete odvolat pomocí `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="42f6f-109">Kromě toho jeho základní algoritmické informace, jejich obsah a šifrování na rest vlastnosti jsou také neměnné.</span><span class="sxs-lookup"><span data-stu-id="42f6f-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="42f6f-110">Pokud se vývojáři změní všechna nastavení, která ovlivňuje klíče trvalost, tyto změny se projeví až po příštím klíče se vygeneruje, buď prostřednictvím explicitní volání `IKeyManager.CreateNewKey` nebo přes data protection systému vlastní [automatické klíč generování](key-management.md#data-protection-implementation-key-management) chování.</span><span class="sxs-lookup"><span data-stu-id="42f6f-110">If the developer changes any setting that affects key persistence, those changes will not go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="42f6f-111">Nastavení, které ovlivňují klíče trvalost jsou následující:</span><span class="sxs-lookup"><span data-stu-id="42f6f-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="42f6f-112">Výchozí doba života klíče</span><span class="sxs-lookup"><span data-stu-id="42f6f-112">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="42f6f-113">Šifrování klíče v mechanismus rest</span><span class="sxs-lookup"><span data-stu-id="42f6f-113">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="42f6f-114">Algoritmické informace obsažené v rámci klíč</span><span class="sxs-lookup"><span data-stu-id="42f6f-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="42f6f-115">Pokud potřebujete těchto nastavení se nové starší než další klíč automatické vrácení čas, zvažte provedení explicitní volání `IKeyManager.CreateNewKey` k vytvoření nového klíče.</span><span class="sxs-lookup"><span data-stu-id="42f6f-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="42f6f-116">Nezapomeňte zadat datum explicitní aktivace ({nyní + 2 dny} je obvykle umožňující čas změna rozšířit) a datum vypršení platnosti ve volání.</span><span class="sxs-lookup"><span data-stu-id="42f6f-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="42f6f-117">Všechny aplikace dotykové ovládání úložiště by měl určovat stejným nastavením `IDataProtectionBuilder` rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="42f6f-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="42f6f-118">Vlastnosti trvalou klíč, jinak budou závislé na konkrétní aplikaci, která volá rutiny generování klíče.</span><span class="sxs-lookup"><span data-stu-id="42f6f-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
