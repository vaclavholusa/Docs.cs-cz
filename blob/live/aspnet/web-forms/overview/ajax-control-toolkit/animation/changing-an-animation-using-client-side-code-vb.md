---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: "Změna animace pomocí kódu na straně klienta (VB) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Animace může taky..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 83d1a21fba37d8807be467d02b5550dc7d096e6c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="a1ed6-104">Změna animace pomocí kódu na straně klienta (VB)</span><span class="sxs-lookup"><span data-stu-id="a1ed6-104">Changing an Animation Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="a1ed6-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a1ed6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a1ed6-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a1ed6-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="a1ed6-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="a1ed6-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a1ed6-108">Animace se změní taky pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="a1ed6-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="a1ed6-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="a1ed6-109">Overview</span></span>

<span data-ttu-id="a1ed6-110">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="a1ed6-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a1ed6-111">Animace se změní taky pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="a1ed6-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a1ed6-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="a1ed6-112">Steps</span></span>

<span data-ttu-id="a1ed6-113">První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="a1ed6-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="a1ed6-114">Animace se použijí na panel textu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a1ed6-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="a1ed6-115">Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:</span><span class="sxs-lookup"><span data-stu-id="a1ed6-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="a1ed6-116">Skutečné animace je spuštěn HTML tlačítko:</span><span class="sxs-lookup"><span data-stu-id="a1ed6-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="a1ed6-117">Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="a1ed6-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="a1ed6-118">Všimněte si, že neexistuje žádná `<Animations>` uzel v rámci `AnimationExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="a1ed6-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="a1ed6-119">Vlastní kód JavaScript slouží k poskytování animací, který se má použít s ovládacím prvkem.</span><span class="sxs-lookup"><span data-stu-id="a1ed6-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="a1ed6-120">Stejně jako u rozhraní API serveru z `AnimationExtender`, neexistuje žádný snadný způsob, jak přiřadit animace rozšiřujícího objektu ještě.</span><span class="sxs-lookup"><span data-stu-id="a1ed6-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="a1ed6-121">Ale rozšiřujícího objektu vystavit několik metod, jak číst a zapisovat animací zaregistrována různé události (`OnClick`, `OnLoad`a tak dále).</span><span class="sxs-lookup"><span data-stu-id="a1ed6-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="a1ed6-122">Následuje několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="a1ed6-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="a1ed6-123">Formát vrácenou hodnotu `get_*()` funkce a formát argument pro `set_*()` funkce je řetězec formátu JSON, poskytuje reprezentaci objektu z co by být značek XML.</span><span class="sxs-lookup"><span data-stu-id="a1ed6-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="a1ed6-124">V současné době neexistuje žádný způsob, jak předat objekt v, ale je možné načíst objekt z daného animace (`get_OnXXXBehavior()` metody).</span><span class="sxs-lookup"><span data-stu-id="a1ed6-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="a1ed6-125">Tady je řetězec formátu JSON (bez uvozovek rozdělujících a přehledně naformátovaná) představující animace aktivuje tlačítko, ale animace panelu změny velikosti a roztmívání ve stejnou dobu:</span><span class="sxs-lookup"><span data-stu-id="a1ed6-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="a1ed6-126">Následující kód v JavaScriptu přiřadí JSON descripting k `OnClick` animace aktuální rozšiřujícího objektu a spustí ho:</span><span class="sxs-lookup"><span data-stu-id="a1ed6-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


<span data-ttu-id="a1ed6-127">[![Animace se spustí okamžitě, bez kliknutí myši (a s velmi malé značek)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a1ed6-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="a1ed6-128">Animace se spustí okamžitě, bez klikněte na tlačítko myši (a s velmi malé značek) ([Kliknutím zobrazit obrázek v plné velikosti](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a1ed6-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a1ed6-129">[Předchozí](executing-animations-using-client-side-code-vb.md)
[další](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a1ed6-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
