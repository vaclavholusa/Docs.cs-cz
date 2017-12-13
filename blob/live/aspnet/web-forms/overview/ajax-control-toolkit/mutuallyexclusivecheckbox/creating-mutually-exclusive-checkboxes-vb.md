---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: "Vytváření vzájemně se vylučuje políček (VB) | Microsoft Docs"
author: wenz
description: "Když může vybrat pouze jeden ze sady možností, přepínače se obvykle používají. Nevýhodou, když je: Jakmile jeden ve skupině přepínače,..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 023ca0b145de8147a98e78f4dba20846dc344f06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="7d17d-104">Vytváření vzájemně se vylučuje políček (VB)</span><span class="sxs-lookup"><span data-stu-id="7d17d-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>
====================
<span data-ttu-id="7d17d-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7d17d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7d17d-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7d17d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="7d17d-107">Když může vybrat pouze jeden ze sady možností, přepínače se obvykle používají.</span><span class="sxs-lookup"><span data-stu-id="7d17d-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="7d17d-108">Nevýhodou, když je: Jakmile jeden ve skupině přepínače, není možné zrušte zaškrtnutí všech přepínačů.</span><span class="sxs-lookup"><span data-stu-id="7d17d-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="7d17d-109">Zaškrtávací políčka může být kdykoli není zaškrtnuto, ale nejsou vzájemně vylučují.</span><span class="sxs-lookup"><span data-stu-id="7d17d-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="7d17d-110">V tomto kurzu poskytuje nejlepší z obou přístupů: zaškrtávací políčka, které se vzájemně vylučují.</span><span class="sxs-lookup"><span data-stu-id="7d17d-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="7d17d-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="7d17d-111">Overview</span></span>

<span data-ttu-id="7d17d-112">Když může vybrat pouze jeden ze sady možností, přepínače se obvykle používají.</span><span class="sxs-lookup"><span data-stu-id="7d17d-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="7d17d-113">Nevýhodou, když je: Jakmile jeden ve skupině přepínače, není možné zrušte zaškrtnutí všech přepínačů.</span><span class="sxs-lookup"><span data-stu-id="7d17d-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="7d17d-114">Zaškrtávací políčka může být kdykoli není zaškrtnuto, ale nejsou vzájemně vylučují.</span><span class="sxs-lookup"><span data-stu-id="7d17d-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="7d17d-115">V tomto kurzu poskytuje nejlepší z obou přístupů: zaškrtávací políčka, které se vzájemně vylučují.</span><span class="sxs-lookup"><span data-stu-id="7d17d-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="7d17d-116">Kroky</span><span class="sxs-lookup"><span data-stu-id="7d17d-116">Steps</span></span>

<span data-ttu-id="7d17d-117">Ovládací prvek ASP.NET AJAX Toolkit obsahuje MutuallyExclusiveCheckBox rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="7d17d-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="7d17d-118">To umožňuje programátorům přiřadit název skupiny libovolný zaškrtávací políčko (`Key` atributu).</span><span class="sxs-lookup"><span data-stu-id="7d17d-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="7d17d-119">Z všechna zaškrtávací políčka v rámci stejné skupiny může být pouze jeden vybraný najednou.</span><span class="sxs-lookup"><span data-stu-id="7d17d-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="7d17d-120">Začněme ukládání dvě zaškrtávací políčka na novou stránku ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7d17d-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="7d17d-121">Může být více, ale dvě z nich stačí k předvedení se zásadou:</span><span class="sxs-lookup"><span data-stu-id="7d17d-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="7d17d-122">Pro zaškrtnutí obou políček musíte být na stránce umístit MutuallyExclusiveCheckBoxExtender ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="7d17d-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="7d17d-123">Oba atributy klíč musí mít stejnou hodnotu, stejně jako hodnota, kterou atributy prvků HTML přepínač tlačítko musí být identické a označují skupinu, ke které patří.</span><span class="sxs-lookup"><span data-stu-id="7d17d-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="7d17d-124">Vlastnost TargetControlID body rozšiřujícího objektu ID zaškrtávacího políčka.</span><span class="sxs-lookup"><span data-stu-id="7d17d-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="7d17d-125">Nakonec zahrnout ASP.NET AJAX `ScriptManager` které je požadované pro všechny elementy sady nástrojů ovládacího prvku ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="7d17d-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="7d17d-126">Uložte a spuštění stránky: můžete zkontrolovat a zrušit zaškrtnutí obou políček, ale současně lze obě políčka zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="7d17d-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="7d17d-127">[![Současně lze zkontrolovat pouze jedno zaškrtávací políčko.](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7d17d-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="7d17d-128">Může být pouze jeden zaškrtnuto současně ([Kliknutím zobrazit obrázek v plné velikosti](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7d17d-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7d17d-129">Předchozí</span><span class="sxs-lookup"><span data-stu-id="7d17d-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
