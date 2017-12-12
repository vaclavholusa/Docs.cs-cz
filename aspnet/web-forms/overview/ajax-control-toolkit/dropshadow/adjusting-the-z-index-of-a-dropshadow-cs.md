---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: "Úprava Z-Index DropShadow (C#) | Microsoft Docs"
author: wenz
description: "DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu. Ale tento stínové někdy je v konfliktu s další ovládací prvky pro Nainstalo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 161f02daa5dd1f0e21853c1b7c1a65c1a9aa5d03
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="a0fdf-104">Úprava Z-Index DropShadow (C#)</span><span class="sxs-lookup"><span data-stu-id="a0fdf-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>
====================
<span data-ttu-id="a0fdf-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a0fdf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a0fdf-106">[Stáhněte si kód](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a0fdf-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="a0fdf-107">DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu.</span><span class="sxs-lookup"><span data-stu-id="a0fdf-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a0fdf-108">Ale tento stínové někdy je v konfliktu s další ovládací prvky pro instanci ovládacího prvku Menu technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a0fdf-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="a0fdf-109">Pokud položku nabídky se zobrazí, se objeví za stín.</span><span class="sxs-lookup"><span data-stu-id="a0fdf-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="a0fdf-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="a0fdf-110">Overview</span></span>

<span data-ttu-id="a0fdf-111">DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu.</span><span class="sxs-lookup"><span data-stu-id="a0fdf-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a0fdf-112">Ale tento stínové někdy je v konfliktu s další ovládací prvky pro instanci ovládacího prvku Menu technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a0fdf-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="a0fdf-113">Pokud položku nabídky se zobrazí, se objeví za stín.</span><span class="sxs-lookup"><span data-stu-id="a0fdf-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="a0fdf-114">Kroky</span><span class="sxs-lookup"><span data-stu-id="a0fdf-114">Steps</span></span>

<span data-ttu-id="a0fdf-115">Kód zahájí s panelem samostatně, obsahující dostatek text tak, aby panelu obsahuje dostatek text pro účinek viditelné:</span><span class="sxs-lookup"><span data-stu-id="a0fdf-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="a0fdf-116">Jiný panel je umístěno přímo před `panelShadow` panelu.</span><span class="sxs-lookup"><span data-stu-id="a0fdf-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="a0fdf-117">Obsahuje nabídku s vodorovné orientaci tak, aby položky nabídky se zobrazují nad (nebo spíše: v části) `dropShadow` panely):</span><span class="sxs-lookup"><span data-stu-id="a0fdf-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="a0fdf-118">Potom, `DropShadowExtender` se přidá do rozšířit `panelShadow` panelu s efekt stínu rozevírací:</span><span class="sxs-lookup"><span data-stu-id="a0fdf-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="a0fdf-119">Nakonec prvku ASP.NET AJAX `ScriptManager` řízení umožňuje Toolkitu postup:</span><span class="sxs-lookup"><span data-stu-id="a0fdf-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="a0fdf-120">Po spuštění tohoto skriptu položky nabídky se zobrazí pod panelu.</span><span class="sxs-lookup"><span data-stu-id="a0fdf-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="a0fdf-121">Ale v nabídce používá třídu CSS `panel` kde právě musíte definovat dvě věci, aby elementy, které se zobrazují před jiné panelu:</span><span class="sxs-lookup"><span data-stu-id="a0fdf-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="a0fdf-122">Relativní umístění</span><span class="sxs-lookup"><span data-stu-id="a0fdf-122">Relative positioning</span></span>
- <span data-ttu-id="a0fdf-123">Kladné z-index</span><span class="sxs-lookup"><span data-stu-id="a0fdf-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="a0fdf-124">Potom, `DropShadowExtender` ovládací prvek není v konfliktu už pomocí ovládacího prvku nabídky.</span><span class="sxs-lookup"><span data-stu-id="a0fdf-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="a0fdf-125">[![Před: Položka nabídky není viditelná](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a0fdf-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="a0fdf-126">Předtím: Položka nabídky není viditelné ([Kliknutím zobrazit obrázek v plné velikosti](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a0fdf-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>


<span data-ttu-id="a0fdf-127">[![Po: Zobrazí se položka nabídky](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="a0fdf-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="a0fdf-128">Po: Zobrazí se položka nabídky ([Kliknutím zobrazit obrázek v plné velikosti](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a0fdf-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="a0fdf-129">Další</span><span class="sxs-lookup"><span data-stu-id="a0fdf-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
