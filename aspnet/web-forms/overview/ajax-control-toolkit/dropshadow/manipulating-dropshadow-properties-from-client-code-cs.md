---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Změna vlastností ovládacího prvku DropShadow klientským kódem (C#) | Dokumentace Microsoftu
author: wenz
description: Přizpůsobení rozhraní pro úpravy prvku DataList
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e3166b9da97a0f4097566b62ba52b6d672eab78f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377282"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="a79b2-103">Změna vlastností ovládacího prvku DropShadow klientským kódem (C#)</span><span class="sxs-lookup"><span data-stu-id="a79b2-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="a79b2-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a79b2-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a79b2-105">[Stáhněte si kód](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a79b2-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="a79b2-106">Sada nástrojů AJAX Control Toolkit ovládacího prvku DropShadow rozšiřuje panel s vrhá stín.</span><span class="sxs-lookup"><span data-stu-id="a79b2-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a79b2-107">Vlastnosti zařízení extender můžete také změnit pomocí kód JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="a79b2-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="a79b2-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="a79b2-108">Overview</span></span>

<span data-ttu-id="a79b2-109">Sada nástrojů AJAX Control Toolkit ovládacího prvku DropShadow rozšiřuje panel s vrhá stín.</span><span class="sxs-lookup"><span data-stu-id="a79b2-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a79b2-110">Vlastnosti zařízení extender můžete také změnit pomocí kód JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="a79b2-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a79b2-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="a79b2-111">Steps</span></span>

<span data-ttu-id="a79b2-112">Kód spustí se panel, který obsahuje několik řádků textu:</span><span class="sxs-lookup"><span data-stu-id="a79b2-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="a79b2-113">Přidružené třídy šablony stylů CSS poskytuje nice pozadí panelu:</span><span class="sxs-lookup"><span data-stu-id="a79b2-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="a79b2-114">`DropShadowExtender` Se přidá k rozšíření na panelu s efektem stínu, krytí nastavena na 50 %:</span><span class="sxs-lookup"><span data-stu-id="a79b2-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="a79b2-115">Potom technologie ASP.NET AJAX `ScriptManager` ovládací prvek umožňuje Control Toolkit pracovat:</span><span class="sxs-lookup"><span data-stu-id="a79b2-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="a79b2-116">Jiného panelu obsahuje dva odkazy jazyka JavaScript pro nastavení krytí vrhá stín: minus odkaz snižuje krytí má stín, plus odkaz se zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="a79b2-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="a79b2-117">Funkce jazyka JavaScript `changeOpacity()` musí nejdříve vyhledejte `DropShadowExtender` ovládací prvek na stránce.</span><span class="sxs-lookup"><span data-stu-id="a79b2-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="a79b2-118">Definuje technologie ASP.NET AJAX `$find()` metodu pro právě tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="a79b2-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="a79b2-119">Pak, bude `get_Opacity()` metoda načte aktuální krytí `set_Opacity()` metoda ji nastaví.</span><span class="sxs-lookup"><span data-stu-id="a79b2-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="a79b2-120">Kód jazyka JavaScript pak vloží aktuální hodnota neprůhlednosti `<label>` element:</span><span class="sxs-lookup"><span data-stu-id="a79b2-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="a79b2-121">[![Neprůhlednost se změní na straně klienta](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a79b2-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="a79b2-122">Neprůhlednost se změní na straně klienta ([kliknutím ji zobrazíte obrázek v plné velikosti](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a79b2-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a79b2-123">[Předchozí](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [další](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a79b2-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
