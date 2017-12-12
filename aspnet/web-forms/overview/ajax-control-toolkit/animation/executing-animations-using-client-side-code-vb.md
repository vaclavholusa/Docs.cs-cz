---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: "Provádění animace pomocí kódu na straně klienta (VB) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Spuštění animace..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c97ce87addd566ed1ba63ccdb81206c6449f49a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="ee226-104">Provádění animace pomocí kódu na straně klienta (VB)</span><span class="sxs-lookup"><span data-stu-id="ee226-104">Executing Animations Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="ee226-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ee226-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ee226-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ee226-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="ee226-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="ee226-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ee226-108">Spuštění animace mohou být vyvolány také pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ee226-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="ee226-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="ee226-109">Overview</span></span>

<span data-ttu-id="ee226-110">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="ee226-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ee226-111">Spuštění animace mohou být vyvolány také pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ee226-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="ee226-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="ee226-112">Steps</span></span>

<span data-ttu-id="ee226-113">První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="ee226-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="ee226-114">Animace se použijí na panel textu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="ee226-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="ee226-115">Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:</span><span class="sxs-lookup"><span data-stu-id="ee226-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="ee226-116">Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="ee226-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="ee226-117">V rámci `<Animations>` uzlu, použijte `<OnClick>` spuštění animací jednou uživatel klikne na panelu.</span><span class="sxs-lookup"><span data-stu-id="ee226-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="ee226-118">Přidejte dva animací parallelly spouštění:</span><span class="sxs-lookup"><span data-stu-id="ee226-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="ee226-119">Z důvodu ukázku Tato animace (a všechny ostatní animace vytvořili pomocí sady nástrojů pro ovládací prvek) se spustí pomocí kódu jazyka JavaScript, po spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="ee226-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="ee226-120">Nejprve všech budeme potřebovat přístup ke `AnimationExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="ee226-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="ee226-121">Poskytuje knihovnu ASP.NET AJAX `$find()` funkce pro tuto úlohu:</span><span class="sxs-lookup"><span data-stu-id="ee226-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="ee226-122">`AnimationExtender` Ovládací prvek poskytuje bohaté rozhraní API, včetně metody s názvy identické obslužné rutiny událostí používá v kódu XML: `OnClick()`, `OnLoad()`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="ee226-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="ee226-123">Volání pro instanci systému `OnClick()` metoda provádí animace v rámci `<OnClick>` element `AnimationExtender` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="ee226-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="ee226-124">Tady je dokončení kód JavaScript na straně klienta, který emuluje klikněte na panelu, jakmile byla úplným načtením stránky Všimněte si, že `pageLoad()` se používá název funkce, kterému se říká pomocí prvku ASP.NET AJAX jednou stránky a zahrnuty všechny byly knihovny JavaScript načíst.</span><span class="sxs-lookup"><span data-stu-id="ee226-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


<span data-ttu-id="ee226-125">[![Animace se spustí okamžitě, bez kliknutí myší](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ee226-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="ee226-126">Animace se spustí okamžitě, bez klikněte na tlačítko myši ([Kliknutím zobrazit obrázek v plné velikosti](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ee226-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ee226-127">[Předchozí](modifying-animations-from-the-server-side-vb.md)
[další](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ee226-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
