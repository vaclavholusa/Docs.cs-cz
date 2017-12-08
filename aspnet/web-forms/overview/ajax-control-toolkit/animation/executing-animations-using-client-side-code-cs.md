---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: "Provádění animace pomocí kódu na straně klienta (C#) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Spuštění animace..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6b1911686a79aa692ef193430cd0746a2511105a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="executing-animations-using-client-side-code-c"></a><span data-ttu-id="adf6c-104">Provádění animace pomocí kódu na straně klienta (C#)</span><span class="sxs-lookup"><span data-stu-id="adf6c-104">Executing Animations Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="adf6c-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="adf6c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="adf6c-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="adf6c-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span></span>

> <span data-ttu-id="adf6c-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="adf6c-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="adf6c-108">Spuštění animace mohou být vyvolány také pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="adf6c-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="adf6c-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="adf6c-109">Overview</span></span>

<span data-ttu-id="adf6c-110">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="adf6c-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="adf6c-111">Spuštění animace mohou být vyvolány také pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="adf6c-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="adf6c-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="adf6c-112">Steps</span></span>

<span data-ttu-id="adf6c-113">První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="adf6c-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="adf6c-114">Animace se použijí na panel textu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="adf6c-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="adf6c-115">Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:</span><span class="sxs-lookup"><span data-stu-id="adf6c-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="adf6c-116">Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="adf6c-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="adf6c-117">V rámci `<Animations>` uzlu, použijte `<OnClick>` spuštění animací jednou uživatel klikne na panelu.</span><span class="sxs-lookup"><span data-stu-id="adf6c-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="adf6c-118">Přidejte dva animací parallelly spouštění:</span><span class="sxs-lookup"><span data-stu-id="adf6c-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

<span data-ttu-id="adf6c-119">Z důvodu ukázku Tato animace (a všechny ostatní animace vytvořili pomocí sady nástrojů pro ovládací prvek) se spustí pomocí kódu jazyka JavaScript, po spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="adf6c-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="adf6c-120">Nejprve všech budeme potřebovat přístup ke `AnimationExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="adf6c-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="adf6c-121">Poskytuje knihovnu ASP.NET AJAX `$find()` funkce pro tuto úlohu:</span><span class="sxs-lookup"><span data-stu-id="adf6c-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

<span data-ttu-id="adf6c-122">`AnimationExtender` Ovládací prvek poskytuje bohaté rozhraní API, včetně metody s názvy identické obslužné rutiny událostí používá v kódu XML: `OnClick()`, `OnLoad()`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="adf6c-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="adf6c-123">Volání pro instanci systému `OnClick()` metoda provádí animace v rámci `<OnClick>` element `AnimationExtender` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="adf6c-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

<span data-ttu-id="adf6c-124">Tady je dokončení kód JavaScript na straně klienta, který emuluje klikněte na panelu, jakmile byla úplným načtením stránky Všimněte si, že `pageLoad()` se používá název funkce, kterému se říká pomocí prvku ASP.NET AJAX jednou stránky a zahrnuty všechny byly knihovny JavaScript načíst.</span><span class="sxs-lookup"><span data-stu-id="adf6c-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


<span data-ttu-id="adf6c-125">[![Animace se spustí okamžitě, bez kliknutí myší](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="adf6c-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="adf6c-126">Animace se spustí okamžitě, bez klikněte na tlačítko myši ([Kliknutím zobrazit obrázek v plné velikosti](executing-animations-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="adf6c-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="adf6c-127">[Předchozí](modifying-animations-from-the-server-side-cs.md)
[další](changing-an-animation-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="adf6c-127">[Previous](modifying-animations-from-the-server-side-cs.md)
[Next](changing-an-animation-using-client-side-code-cs.md)</span></span>
