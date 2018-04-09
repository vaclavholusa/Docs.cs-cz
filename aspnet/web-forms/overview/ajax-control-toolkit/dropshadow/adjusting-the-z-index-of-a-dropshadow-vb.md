---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Úprava Z-Index DropShadow (VB) | Microsoft Docs
author: wenz
description: DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu. Ale tento stínové někdy je v konfliktu s další ovládací prvky pro Nainstalo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: b484dc6bfa6f67bd6b70f7c36c2eb2ec7143edaf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="f50a3-104">Úprava Z-Index DropShadow (VB)</span><span class="sxs-lookup"><span data-stu-id="f50a3-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="f50a3-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f50a3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f50a3-106">[Stáhněte si kód](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f50a3-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="f50a3-107">DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu.</span><span class="sxs-lookup"><span data-stu-id="f50a3-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="f50a3-108">Ale tento stínové někdy je v konfliktu s další ovládací prvky pro instanci ovládacího prvku Menu technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f50a3-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="f50a3-109">Pokud položku nabídky se zobrazí, se objeví za stín.</span><span class="sxs-lookup"><span data-stu-id="f50a3-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="f50a3-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="f50a3-110">Overview</span></span>

<span data-ttu-id="f50a3-111">DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu.</span><span class="sxs-lookup"><span data-stu-id="f50a3-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="f50a3-112">Ale tento stínové někdy je v konfliktu s další ovládací prvky pro instanci ovládacího prvku Menu technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f50a3-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="f50a3-113">Pokud položku nabídky se zobrazí, se objeví za stín.</span><span class="sxs-lookup"><span data-stu-id="f50a3-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="f50a3-114">Kroky</span><span class="sxs-lookup"><span data-stu-id="f50a3-114">Steps</span></span>

<span data-ttu-id="f50a3-115">Kód zahájí s panelem samostatně, obsahující dostatek text tak, aby panelu obsahuje dostatek text pro účinek viditelné:</span><span class="sxs-lookup"><span data-stu-id="f50a3-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="f50a3-116">Jiný panel je umístěno přímo před `panelShadow` panelu.</span><span class="sxs-lookup"><span data-stu-id="f50a3-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="f50a3-117">Obsahuje nabídku s vodorovné orientaci tak, aby položky nabídky se zobrazují nad (nebo spíše: v části) `dropShadow` panely):</span><span class="sxs-lookup"><span data-stu-id="f50a3-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="f50a3-118">Potom, `DropShadowExtender` se přidá do rozšířit `panelShadow` panelu s efekt stínu rozevírací:</span><span class="sxs-lookup"><span data-stu-id="f50a3-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="f50a3-119">Nakonec prvku ASP.NET AJAX `ScriptManager` řízení umožňuje Toolkitu postup:</span><span class="sxs-lookup"><span data-stu-id="f50a3-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="f50a3-120">Po spuštění tohoto skriptu položky nabídky se zobrazí pod panelu.</span><span class="sxs-lookup"><span data-stu-id="f50a3-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="f50a3-121">Ale v nabídce používá třídu CSS `panel` kde právě musíte definovat dvě věci, aby elementy, které se zobrazují před jiné panelu:</span><span class="sxs-lookup"><span data-stu-id="f50a3-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="f50a3-122">Relativní umístění</span><span class="sxs-lookup"><span data-stu-id="f50a3-122">Relative positioning</span></span>
- <span data-ttu-id="f50a3-123">Kladné z-index</span><span class="sxs-lookup"><span data-stu-id="f50a3-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="f50a3-124">Potom, `DropShadowExtender` ovládací prvek není v konfliktu už pomocí ovládacího prvku nabídky.</span><span class="sxs-lookup"><span data-stu-id="f50a3-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="f50a3-125">[![Před: Položka nabídky není viditelná](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f50a3-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="f50a3-126">Předtím: Položka nabídky není viditelné ([Kliknutím zobrazit obrázek v plné velikosti](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f50a3-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="f50a3-127">[![Po: Zobrazí se položka nabídky](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f50a3-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="f50a3-128">Po: Zobrazí se položka nabídky ([Kliknutím zobrazit obrázek v plné velikosti](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f50a3-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f50a3-129">[Předchozí](manipulating-dropshadow-properties-from-client-code-cs.md)
> [další](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f50a3-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
