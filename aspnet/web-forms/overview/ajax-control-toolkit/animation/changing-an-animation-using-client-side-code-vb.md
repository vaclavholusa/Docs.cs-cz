---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Změna animace klientským kódem (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze také...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: cc8ca2c962c5ebe5e0c45d5b575031ada3e64acd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386744"
---
<a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="61b2c-104">Změna animace klientským kódem (VB)</span><span class="sxs-lookup"><span data-stu-id="61b2c-104">Changing an Animation Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="61b2c-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="61b2c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="61b2c-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="61b2c-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="61b2c-107">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="61b2c-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="61b2c-108">Animace lze také změnit pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="61b2c-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="61b2c-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="61b2c-109">Overview</span></span>

<span data-ttu-id="61b2c-110">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="61b2c-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="61b2c-111">Animace lze také změnit pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="61b2c-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="61b2c-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="61b2c-112">Steps</span></span>

<span data-ttu-id="61b2c-113">Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="61b2c-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="61b2c-114">Animace se použijí pro panel text, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="61b2c-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="61b2c-115">V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:</span><span class="sxs-lookup"><span data-stu-id="61b2c-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="61b2c-116">Skutečné animace spustí HTML tlačítko:</span><span class="sxs-lookup"><span data-stu-id="61b2c-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="61b2c-117">Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="61b2c-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="61b2c-118">Všimněte si, že neexistuje žádná `<Animations>` uzlu v rámci `AnimationExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="61b2c-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="61b2c-119">Vlastní kód jazyka JavaScript se používá k poskytování animace, který se má použít s ovládacím prvkem.</span><span class="sxs-lookup"><span data-stu-id="61b2c-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="61b2c-120">Stejně jako u serveru rozhraní API `AnimationExtender`, neexistuje žádný snadný způsob, jak přiřadit animace zařízení extender ještě.</span><span class="sxs-lookup"><span data-stu-id="61b2c-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="61b2c-121">Ale zařízení extender zpřístupní ke čtení a zápisu animací několik metod zaregistrovaného na různé události (`OnClick`, `OnLoad`, a tak dále).</span><span class="sxs-lookup"><span data-stu-id="61b2c-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="61b2c-122">Následuje několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="61b2c-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="61b2c-123">Formát vrácené hodnoty `get_*()` funkce a formát argumentu pro `set_*()` funkce je řetězec formátu JSON, poskytuje reprezentaci objektu toho, co by být kód XML.</span><span class="sxs-lookup"><span data-stu-id="61b2c-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="61b2c-124">V současné době neexistuje žádný způsob předat objekt, ale je možné číst objekt z dané animace (`get_OnXXXBehavior()` metody).</span><span class="sxs-lookup"><span data-stu-id="61b2c-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="61b2c-125">Tady je řetězec formátu JSON (bez oddělovací uvozovky a hezky formátovaný) představující animace aktivuje tlačítko, ale animace panelu podle jejich velikosti a mizení ve stejnou dobu:</span><span class="sxs-lookup"><span data-stu-id="61b2c-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="61b2c-126">Následující kód jazyka JavaScript přiřadí JSON descripting k `OnClick` animace aktuální zařízení extender a spustí ho:</span><span class="sxs-lookup"><span data-stu-id="61b2c-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


<span data-ttu-id="61b2c-127">[![Animace se spustí okamžitě, bez kliknutí myší (a s velmi málo značky)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="61b2c-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="61b2c-128">Animace se spustí okamžitě, bez kliknutí myší (a s velmi málo značky) ([kliknutím ji zobrazíte obrázek v plné velikosti](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="61b2c-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="61b2c-129">[Předchozí](executing-animations-using-client-side-code-vb.md)
> [další](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="61b2c-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
