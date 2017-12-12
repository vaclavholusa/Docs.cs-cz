---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: "Manipulace s DropShadow vlastnosti z kódu klienta (VB) | Microsoft Docs"
author: wenz
description: "DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu. Vlastnosti této rozšiřujícího objektu se změní taky pomocí klienta JavaScrip..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 706ccb5a95873aad4c1b9e0942ab06cf4162710a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="122a3-104">Manipulace s DropShadow vlastnosti z kódu klienta (VB)</span><span class="sxs-lookup"><span data-stu-id="122a3-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="122a3-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="122a3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="122a3-106">[Stáhněte si kód](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="122a3-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="122a3-107">DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu.</span><span class="sxs-lookup"><span data-stu-id="122a3-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="122a3-108">Vlastnosti této rozšiřujícího objektu můžete změnit také pomocí kód JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="122a3-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="122a3-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="122a3-109">Overview</span></span>

<span data-ttu-id="122a3-110">DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu.</span><span class="sxs-lookup"><span data-stu-id="122a3-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="122a3-111">Vlastnosti této rozšiřujícího objektu můžete změnit také pomocí kód JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="122a3-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="122a3-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="122a3-112">Steps</span></span>

<span data-ttu-id="122a3-113">Kód začíná panelu obsahující některé řádky textu:</span><span class="sxs-lookup"><span data-stu-id="122a3-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="122a3-114">Třída CSS, která přidružené dává panelu barvu pozadí dobrý:</span><span class="sxs-lookup"><span data-stu-id="122a3-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="122a3-115">`DropShadowExtender` Se přidá do rozšířit panel s efekt stínu rozevírací krytí nastavena na 50 %:</span><span class="sxs-lookup"><span data-stu-id="122a3-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="122a3-116">Potom prvku ASP.NET AJAX `ScriptManager` řízení umožňuje Toolkitu postup:</span><span class="sxs-lookup"><span data-stu-id="122a3-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="122a3-117">Jiný panel obsahuje dva odkazy JavaScript pro nastavení krytí stín: odkaz znaménkem minus snižuje krytí bude stín, plus odkaz se zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="122a3-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="122a3-118">Funkce JavaScript, která `changeOpacity()` musí nejprve vyhledejte `DropShadowExtender` ovládací prvek na stránce.</span><span class="sxs-lookup"><span data-stu-id="122a3-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="122a3-119">Definuje prvku ASP.NET AJAX `$find()` metoda pro přesně tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="122a3-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="122a3-120">Potom, `get_Opacity()` metoda načte aktuální krytí `set_Opacity()` metoda ji nastaví.</span><span class="sxs-lookup"><span data-stu-id="122a3-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="122a3-121">Kód jazyka JavaScript pak vloží aktuální hodnota neprůhlednosti `<label>` element:</span><span class="sxs-lookup"><span data-stu-id="122a3-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="122a3-122">[![Krytí se změní na straně klienta](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="122a3-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="122a3-123">Krytí se změní na straně klienta ([Kliknutím zobrazit obrázek v plné velikosti](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="122a3-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="122a3-124">Předchozí</span><span class="sxs-lookup"><span data-stu-id="122a3-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
