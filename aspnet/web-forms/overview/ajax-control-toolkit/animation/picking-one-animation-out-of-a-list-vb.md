---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Výběr jedné animace ze seznamu (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Rozhraní také povolit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 561f05e96888962cfe576963ce3905b171203939
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387738"
---
<a name="picking-one-animation-out-of-a-list-vb"></a><span data-ttu-id="7a42a-104">Výběr jedné animace ze seznamu (VB)</span><span class="sxs-lookup"><span data-stu-id="7a42a-104">Picking One Animation Out Of a List (VB)</span></span>
====================
<span data-ttu-id="7a42a-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7a42a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7a42a-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7a42a-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span></span>

> <span data-ttu-id="7a42a-107">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="7a42a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7a42a-108">Rozhraní framework umožňuje také programátora pro výběr jedné animace ze seznamu animace, v závislosti na hodnocení kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7a42a-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="7a42a-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="7a42a-109">Overview</span></span>

<span data-ttu-id="7a42a-110">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="7a42a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7a42a-111">Rozhraní framework umožňuje také programátora pro výběr jedné animace ze seznamu animace, v závislosti na hodnocení kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7a42a-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="7a42a-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="7a42a-112">Steps</span></span>

<span data-ttu-id="7a42a-113">Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="7a42a-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

<span data-ttu-id="7a42a-114">Animace se použijí pro panel text, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="7a42a-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

<span data-ttu-id="7a42a-115">V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:</span><span class="sxs-lookup"><span data-stu-id="7a42a-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

<span data-ttu-id="7a42a-116">Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="7a42a-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

<span data-ttu-id="7a42a-117">V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile úplným načtením stránky.</span><span class="sxs-lookup"><span data-stu-id="7a42a-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="7a42a-118">Místo jednoho regulární animací `<Case>` element vstupu do play.</span><span class="sxs-lookup"><span data-stu-id="7a42a-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="7a42a-119">Hodnota atributu jeho SelectScript je vyhodnocen; Vrácená hodnota musí být číselná.</span><span class="sxs-lookup"><span data-stu-id="7a42a-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="7a42a-120">V závislosti na toto číslo, jeden z subanimations v rámci &lt;případ&gt; provádí.</span><span class="sxs-lookup"><span data-stu-id="7a42a-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="7a42a-121">Například pokud SelectScript vyhodnotí na 2, Control Toolkit se spustí třetí animace v rámci &lt;případ&gt; (počítání začíná 0).</span><span class="sxs-lookup"><span data-stu-id="7a42a-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="7a42a-122">Následující kód definuje tři subanimations: Změna velikosti šířka, výška změny velikosti a mizení. Kód jazyka JavaScript (`Math.floor(3 * Math.random())`) pak vybere číslo mezi 0 a 2, tak, aby jeden ze tří animace spustit:</span><span class="sxs-lookup"><span data-stu-id="7a42a-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


<span data-ttu-id="7a42a-123">[![Jednou z možných tři animací: získá širší panelu](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7a42a-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span></span>

<span data-ttu-id="7a42a-124">Jednou z možných tři animací: panelu získá širší ([kliknutím ji zobrazíte obrázek v plné velikosti](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7a42a-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7a42a-125">[Předchozí](animation-depending-on-a-condition-vb.md)
> [další](animating-in-response-to-user-interaction-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7a42a-125">[Previous](animation-depending-on-a-condition-vb.md)
[Next](animating-in-response-to-user-interaction-vb.md)</span></span>
