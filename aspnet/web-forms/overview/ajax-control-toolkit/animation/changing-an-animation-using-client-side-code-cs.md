---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Změna animace klientským kódem (C#) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze také...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 253377ef6019a672680c6e819349357627ef111b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752659"
---
<a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="158f5-104">Změna animace klientským kódem (C#)</span><span class="sxs-lookup"><span data-stu-id="158f5-104">Changing an Animation Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="158f5-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="158f5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="158f5-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="158f5-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="158f5-107">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="158f5-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="158f5-108">Animace lze také změnit pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="158f5-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="158f5-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="158f5-109">Overview</span></span>

<span data-ttu-id="158f5-110">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="158f5-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="158f5-111">Animace lze také změnit pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="158f5-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="158f5-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="158f5-112">Steps</span></span>

<span data-ttu-id="158f5-113">Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="158f5-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="158f5-114">Animace se použijí pro panel text, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="158f5-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="158f5-115">V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:</span><span class="sxs-lookup"><span data-stu-id="158f5-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="158f5-116">Skutečné animace spustí HTML tlačítko:</span><span class="sxs-lookup"><span data-stu-id="158f5-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="158f5-117">Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="158f5-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="158f5-118">Všimněte si, že neexistuje žádná `<Animations>` uzlu v rámci `AnimationExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="158f5-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="158f5-119">Vlastní kód jazyka JavaScript se používá k poskytování animace, který se má použít s ovládacím prvkem.</span><span class="sxs-lookup"><span data-stu-id="158f5-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="158f5-120">Stejně jako u serveru rozhraní API `AnimationExtender`, neexistuje žádný snadný způsob, jak přiřadit animace zařízení extender ještě.</span><span class="sxs-lookup"><span data-stu-id="158f5-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="158f5-121">Ale zařízení extender zpřístupní ke čtení a zápisu animací několik metod zaregistrovaného na různé události (`OnClick`, `OnLoad`, a tak dále).</span><span class="sxs-lookup"><span data-stu-id="158f5-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="158f5-122">Následuje několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="158f5-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="158f5-123">Formát vrácené hodnoty `get_*()` funkce a formát argumentu pro `set_*()` funkce je řetězec formátu JSON, poskytuje reprezentaci objektu toho, co by být kód XML.</span><span class="sxs-lookup"><span data-stu-id="158f5-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="158f5-124">V současné době neexistuje žádný způsob předat objekt, ale je možné číst objekt z dané animace (`get_OnXXXBehavior()` metody).</span><span class="sxs-lookup"><span data-stu-id="158f5-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="158f5-125">Tady je řetězec formátu JSON (bez oddělovací uvozovky a hezky formátovaný) představující animace aktivuje tlačítko, ale animace panelu podle jejich velikosti a mizení ve stejnou dobu:</span><span class="sxs-lookup"><span data-stu-id="158f5-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="158f5-126">Následující kód jazyka JavaScript přiřadí JSON descripting k `OnClick` animace aktuální zařízení extender a spustí ho:</span><span class="sxs-lookup"><span data-stu-id="158f5-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


<span data-ttu-id="158f5-127">[![Animace se spustí okamžitě, bez kliknutí myší (a s velmi málo značky)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="158f5-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="158f5-128">Animace se spustí okamžitě, bez kliknutí myší (a s velmi málo značky) ([kliknutím ji zobrazíte obrázek v plné velikosti](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="158f5-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="158f5-129">[Předchozí](executing-animations-using-client-side-code-cs.md)
> [další](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="158f5-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
