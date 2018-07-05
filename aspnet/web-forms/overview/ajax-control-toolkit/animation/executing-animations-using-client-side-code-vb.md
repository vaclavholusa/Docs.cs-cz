---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Spuštění animací klientským kódem (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Spuštění animace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 5408c51c1330a0394b89281e4ddce00df455a418
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371217"
---
<a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="eefb3-104">Spuštění animací klientským kódem (VB)</span><span class="sxs-lookup"><span data-stu-id="eefb3-104">Executing Animations Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="eefb3-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="eefb3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="eefb3-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="eefb3-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="eefb3-107">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="eefb3-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="eefb3-108">Spuštění animace může také aktivovat pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="eefb3-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="eefb3-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="eefb3-109">Overview</span></span>

<span data-ttu-id="eefb3-110">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="eefb3-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="eefb3-111">Spuštění animace může také aktivovat pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="eefb3-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="eefb3-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="eefb3-112">Steps</span></span>

<span data-ttu-id="eefb3-113">Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="eefb3-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="eefb3-114">Animace se použijí pro panel text, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="eefb3-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="eefb3-115">V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:</span><span class="sxs-lookup"><span data-stu-id="eefb3-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="eefb3-116">Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="eefb3-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="eefb3-117">V rámci `<Animations>` uzlu, použijte `<OnClick>` spuštění animací uživatel klikne na tlačítko na panelu.</span><span class="sxs-lookup"><span data-stu-id="eefb3-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="eefb3-118">Přidejte dva animace parallelly provádět:</span><span class="sxs-lookup"><span data-stu-id="eefb3-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="eefb3-119">Představu Tato animace (a jakékoli jiné animace vytvořené pomocí Control Toolkit) je provedeno pomocí kódu jazyka JavaScript, po spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="eefb3-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="eefb3-120">Za prvé jsme potřebují přístup k `AnimationExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="eefb3-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="eefb3-121">Poskytuje knihovna ASP.NET AJAX `$find()` funkce pro tuto úlohu:</span><span class="sxs-lookup"><span data-stu-id="eefb3-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="eefb3-122">`AnimationExtender` Ovládací prvek poskytuje plnohodnotné rozhraní API, včetně metod s názvy shodné s obslužné rutiny událostí používá v kódu XML: `OnClick()`, `OnLoad()`, a tak dále.</span><span class="sxs-lookup"><span data-stu-id="eefb3-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="eefb3-123">Například volání `OnClick()` spuštění animace v rámci metody `<OnClick>` elementu `AnimationExtender` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="eefb3-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="eefb3-124">Tady je kompletní kód JavaScript na straně klienta, který emuluje klikněte na panelu, jakmile úplným načtením stránky, Všimněte si, že `pageLoad()` se používá název funkce, které je voláno rozhraním ASP.NET AJAX jednou na stránce a zahrnuty všechny byly knihoven jazyka JavaScript načíst.</span><span class="sxs-lookup"><span data-stu-id="eefb3-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


<span data-ttu-id="eefb3-125">[![Animace se spustí okamžitě, bez kliknutí myší](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eefb3-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="eefb3-126">Animace se spustí okamžitě, bez kliknutí myší ([kliknutím ji zobrazíte obrázek v plné velikosti](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="eefb3-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="eefb3-127">[Předchozí](modifying-animations-from-the-server-side-vb.md)
> [další](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="eefb3-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
