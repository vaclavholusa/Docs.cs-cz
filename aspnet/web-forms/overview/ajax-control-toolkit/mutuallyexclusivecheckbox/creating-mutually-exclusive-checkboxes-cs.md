---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Vytváření vzájemně se vylučuje zaškrtávací políčka (C#) | Microsoft Docs
author: wenz
description: 'Když může vybrat pouze jeden ze sady možností, přepínače se obvykle používají. Nevýhodou, když je: Jakmile jeden ve skupině přepínače,...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: c3a5abe7d02ace16f4aaad8d4adfbd0cba8e84ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871802"
---
<a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="ae4e8-104">Vytváření vzájemně se vylučuje zaškrtávací políčka (C#)</span><span class="sxs-lookup"><span data-stu-id="ae4e8-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>
====================
<span data-ttu-id="ae4e8-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ae4e8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ae4e8-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ae4e8-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="ae4e8-107">Když může vybrat pouze jeden ze sady možností, přepínače se obvykle používají.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="ae4e8-108">Nevýhodou, když je: Jakmile jeden ve skupině přepínače, není možné zrušte zaškrtnutí všech přepínačů.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="ae4e8-109">Zaškrtávací políčka může být kdykoli není zaškrtnuto, ale nejsou vzájemně vylučují.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="ae4e8-110">V tomto kurzu poskytuje nejlepší z obou přístupů: zaškrtávací políčka, které se vzájemně vylučují.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="ae4e8-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="ae4e8-111">Overview</span></span>

<span data-ttu-id="ae4e8-112">Když může vybrat pouze jeden ze sady možností, přepínače se obvykle používají.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="ae4e8-113">Nevýhodou, když je: Jakmile jeden ve skupině přepínače, není možné zrušte zaškrtnutí všech přepínačů.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="ae4e8-114">Zaškrtávací políčka může být kdykoli není zaškrtnuto, ale nejsou vzájemně vylučují.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="ae4e8-115">V tomto kurzu poskytuje nejlepší z obou přístupů: zaškrtávací políčka, které se vzájemně vylučují.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="ae4e8-116">Kroky</span><span class="sxs-lookup"><span data-stu-id="ae4e8-116">Steps</span></span>

<span data-ttu-id="ae4e8-117">Ovládací prvek ASP.NET AJAX Toolkit obsahuje MutuallyExclusiveCheckBox rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="ae4e8-118">To umožňuje programátorům přiřadit název skupiny libovolný zaškrtávací políčko (`Key` atributu).</span><span class="sxs-lookup"><span data-stu-id="ae4e8-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="ae4e8-119">Z všechna zaškrtávací políčka v rámci stejné skupiny může být pouze jeden vybraný najednou.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="ae4e8-120">Začněme ukládání dvě zaškrtávací políčka na novou stránku ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="ae4e8-121">Může být více, ale dvě z nich stačí k předvedení se zásadou:</span><span class="sxs-lookup"><span data-stu-id="ae4e8-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="ae4e8-122">Pro zaškrtnutí obou políček musíte být na stránce umístit MutuallyExclusiveCheckBoxExtender ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="ae4e8-123">Oba atributy klíč musí mít stejnou hodnotu, stejně jako hodnota, kterou atributy prvků HTML přepínač tlačítko musí být identické a označují skupinu, ke které patří.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="ae4e8-124">Vlastnost TargetControlID body rozšiřujícího objektu ID zaškrtávacího políčka.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="ae4e8-125">Nakonec zahrnout ASP.NET AJAX `ScriptManager` které je požadované pro všechny elementy sady nástrojů ovládacího prvku ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="ae4e8-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="ae4e8-126">Uložte a spuštění stránky: můžete zkontrolovat a zrušit zaškrtnutí obou políček, ale současně lze obě políčka zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="ae4e8-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="ae4e8-127">[![Současně lze zkontrolovat pouze jedno zaškrtávací políčko.](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ae4e8-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="ae4e8-128">Může být pouze jeden zaškrtnuto současně ([Kliknutím zobrazit obrázek v plné velikosti](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ae4e8-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ae4e8-129">Next</span><span class="sxs-lookup"><span data-stu-id="ae4e8-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
