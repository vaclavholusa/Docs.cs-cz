---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: "Vytvoření ovládacího prvku hodnocení (C#) | Microsoft Docs"
author: wenz
description: "Mnoho webů, z elektronického obchodování do lokalit komunity, nabízí svým uživatelům míra články nebo položek. Většinou je potřeba některé kódování úsilí, ale máme..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c004522ac72b848e42320862d77bced0c11ca15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-rating-control-c"></a><span data-ttu-id="baae2-104">Vytvoření ovládacího prvku hodnocení (C#)</span><span class="sxs-lookup"><span data-stu-id="baae2-104">Creating a Rating Control (C#)</span></span>
====================
<span data-ttu-id="baae2-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="baae2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="baae2-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="baae2-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="baae2-107">Mnoho webů, z elektronického obchodování do lokalit komunity, nabízí svým uživatelům míra články nebo položek.</span><span class="sxs-lookup"><span data-stu-id="baae2-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="baae2-108">Většinou je potřeba některé kódování úsilí, ale musíme Toolkitu naše uvolnění.</span><span class="sxs-lookup"><span data-stu-id="baae2-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="baae2-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="baae2-109">Overview</span></span>

<span data-ttu-id="baae2-110">Mnoho webů, z elektronického obchodování do lokalit komunity, nabízí svým uživatelům míra články nebo položek.</span><span class="sxs-lookup"><span data-stu-id="baae2-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="baae2-111">Většinou je potřeba některé kódování úsilí, ale musíme Toolkitu naše uvolnění.</span><span class="sxs-lookup"><span data-stu-id="baae2-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="baae2-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="baae2-112">Steps</span></span>

<span data-ttu-id="baae2-113">Je třeba nejprve všech, (aspoň) dva druhy bitové kopie: jednu pro vyplnění položku hodnocení a jeden pro položku prázdný hodnocení.</span><span class="sxs-lookup"><span data-stu-id="baae2-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="baae2-114">Položku hodnocení je obvykle hvězdu nebo emotikona.</span><span class="sxs-lookup"><span data-stu-id="baae2-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="baae2-115">V tomto scénáři zjistíte tři soubory, smiley.png a empty.png a emotikona done.png v rámci stahování zdrojového kódu pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="baae2-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="baae2-116">Pak vytvořte nový soubor ASP.NET a začněte přidáte `ScriptManager` řízení k němu:</span><span class="sxs-lookup"><span data-stu-id="baae2-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="baae2-117">Poté, přidejte `Rating` řízení z ovládacího prvku ASP.NET AJAX Toolkit.</span><span class="sxs-lookup"><span data-stu-id="baae2-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="baae2-118">Následující atributy je nutné nastavit v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="baae2-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="baae2-119">`CurrentRating`Počáteční hodnocení, který se má použít</span><span class="sxs-lookup"><span data-stu-id="baae2-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="baae2-120">`MaxRating`maximální hodnocení</span><span class="sxs-lookup"><span data-stu-id="baae2-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="baae2-121">`EmptyStarCssClass`Třída šablon stylů CSS pro použití při hodnocení položky (hvězdička) je prázdný</span><span class="sxs-lookup"><span data-stu-id="baae2-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="baae2-122">`FilledStarCssClass`třídy šablon stylů CSS použité při vyplňování položku hodnocení (hvězdička)</span><span class="sxs-lookup"><span data-stu-id="baae2-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="baae2-123">`StarCssClass`Třída šablon stylů CSS pro viditelné stat</span><span class="sxs-lookup"><span data-stu-id="baae2-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="baae2-124">`WaitingStarCssClass`třídy šablon stylů CSS použité při hodnocení hvězdičkami budou odeslána zpět do serveru</span><span class="sxs-lookup"><span data-stu-id="baae2-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="baae2-125">A zde je kód, který vytvoří ovládacího prvku hodnocení s pěti položek (Veselé obličeje), ve kterých žádný vyplní se původně:</span><span class="sxs-lookup"><span data-stu-id="baae2-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="baae2-126">Tři odkazované tříd CSS nyní třeba zobrazit soubory příslušné bitové kopie, což je snadno pomocí šablon stylů CSS:</span><span class="sxs-lookup"><span data-stu-id="baae2-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="baae2-127">Ujistěte se, zadejte šířku a výšku tři bitových kopií, v opačném případě bude vypadat zobrazení trochu messed nahoru.</span><span class="sxs-lookup"><span data-stu-id="baae2-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="baae2-128">Nakonec výsledek hodnocení by měl být uživateli zobrazí (nebo alespoň uloženy v databázi).</span><span class="sxs-lookup"><span data-stu-id="baae2-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="baae2-129">Proto přidáte popisek pro výstup zprávu SMS a tlačítko odeslání odeslat zpět hodnocení formuláře k serveru:</span><span class="sxs-lookup"><span data-stu-id="baae2-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="baae2-130">V kódu na straně serveru přístup prostřednictvím ovládacího prvku hodnocení jeho `ID` a pro přístup k jeho `CurrentRating` vlastnost, která je počet položek vybraných hodnocení, v našem příkladu hodnotu v rozmezí 0 až 5.</span><span class="sxs-lookup"><span data-stu-id="baae2-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="baae2-131">Uložit stránky a načíst do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="baae2-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="baae2-132">Po přesunutí ukazatele myši položky (původně prázdné) hodnocení, dojde k vliv JavaScript: změny hodnocení.</span><span class="sxs-lookup"><span data-stu-id="baae2-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="baae2-133">Když kliknete na sadu hvězdiček, se zachová aktuální hodnocení.</span><span class="sxs-lookup"><span data-stu-id="baae2-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="baae2-134">Nakonec při odesílání formuláře kódu na straně serveru výstupy vybrané hodnocení.</span><span class="sxs-lookup"><span data-stu-id="baae2-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="baae2-135">[![Vytváření hodnocení systému s minimálním kódu](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="baae2-135">[![Creating a rating system with minimal code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="baae2-136">Vytváření hodnocení systému s minimálním kódu ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="baae2-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="baae2-137">Další</span><span class="sxs-lookup"><span data-stu-id="baae2-137">Next</span></span>](creating-a-rating-control-vb.md)
