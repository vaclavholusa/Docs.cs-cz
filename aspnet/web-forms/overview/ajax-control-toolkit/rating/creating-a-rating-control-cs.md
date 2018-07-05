---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Vytvoření ovládacího prvku Rating (C#) | Dokumentace Microsoftu
author: wenz
description: Mnoho webů z elektronického obchodování na komunitní weby, nabízí svým uživatelům míra články nebo položky. To obvykle vyžaduje některé kódování úsilí, ale pracujeme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7be954a73c6c08bca9992aacf6ad529bc61c9247
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389583"
---
<a name="creating-a-rating-control-c"></a><span data-ttu-id="0ead1-104">Vytvoření ovládacího prvku Rating (C#)</span><span class="sxs-lookup"><span data-stu-id="0ead1-104">Creating a Rating Control (C#)</span></span>
====================
<span data-ttu-id="0ead1-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0ead1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0ead1-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0ead1-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="0ead1-107">Mnoho webů z elektronického obchodování na komunitní weby, nabízí svým uživatelům míra články nebo položky.</span><span class="sxs-lookup"><span data-stu-id="0ead1-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="0ead1-108">To obvykle vyžaduje některé kódování úsilí, ale musíme Control Toolkit naše vyřazení.</span><span class="sxs-lookup"><span data-stu-id="0ead1-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="0ead1-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="0ead1-109">Overview</span></span>

<span data-ttu-id="0ead1-110">Mnoho webů z elektronického obchodování na komunitní weby, nabízí svým uživatelům míra články nebo položky.</span><span class="sxs-lookup"><span data-stu-id="0ead1-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="0ead1-111">To obvykle vyžaduje některé kódování úsilí, ale musíme Control Toolkit naše vyřazení.</span><span class="sxs-lookup"><span data-stu-id="0ead1-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="0ead1-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="0ead1-112">Steps</span></span>

<span data-ttu-id="0ead1-113">Za prvé, budete potřebovat (minimálně) dva druhy bitové kopie: jednu pro plného hodnocení položky a z nich je pro položku prázdný hodnocení.</span><span class="sxs-lookup"><span data-stu-id="0ead1-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="0ead1-114">Hodnocení položky je obvykle hvězdička nebo ikonu Veselého.</span><span class="sxs-lookup"><span data-stu-id="0ead1-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="0ead1-115">V tomto scénáři najít tři soubory, smiley.png a empty.png a obličej done.png jako součást stažené soubory zdrojového kódu pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0ead1-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="0ead1-116">Potom vytvořte nový soubor ASP.NET a začít s přidáváním `ScriptManager` ovládacího prvku do ní:</span><span class="sxs-lookup"><span data-stu-id="0ead1-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="0ead1-117">Pak přidejte `Rating` ovládacího prvku z technologie ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="0ead1-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="0ead1-118">Následující atributy je potřeba nastavit v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="0ead1-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="0ead1-119">`CurrentRating` Počáteční hodnocení, který se má použít</span><span class="sxs-lookup"><span data-stu-id="0ead1-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="0ead1-120">`MaxRating` maximální hodnocení</span><span class="sxs-lookup"><span data-stu-id="0ead1-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="0ead1-121">`EmptyStarCssClass` nic neobsahuje třídu šablony stylů CSS pro použití při hodnocení položky (star)</span><span class="sxs-lookup"><span data-stu-id="0ead1-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="0ead1-122">`FilledStarCssClass` vyplnění třídu šablony stylů CSS pro použití při hodnocení položky (star)</span><span class="sxs-lookup"><span data-stu-id="0ead1-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="0ead1-123">`StarCssClass` Třída CSS použitá pro viditelné stat</span><span class="sxs-lookup"><span data-stu-id="0ead1-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="0ead1-124">`WaitingStarCssClass` Při hodnocení hvězdičkami budou odeslána zpět do serveru, použijte třídu šablony stylů CSS</span><span class="sxs-lookup"><span data-stu-id="0ead1-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="0ead1-125">A tady je kód, který vytvoří ovládacího prvku rating s pěti položek (Veselé obličeje), z nichž žádná vyplnění zpočátku:</span><span class="sxs-lookup"><span data-stu-id="0ead1-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="0ead1-126">Tři odkazované třídy šablony stylů CSS teď třeba zobrazit příslušné obrázkové soubory, které lze snadno udělat pomocí šablon stylů CSS:</span><span class="sxs-lookup"><span data-stu-id="0ead1-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="0ead1-127">Ujistěte se, že zadáte šířku a výšku tři Image, jinak může vypadat trochu messed do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0ead1-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="0ead1-128">Nakonec výsledek hodnocení by měl být uživateli zobrazí (nebo aspoň uložený v databázi).</span><span class="sxs-lookup"><span data-stu-id="0ead1-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="0ead1-129">Proto přidejte popisek pro výstup textovou zprávu a tlačítka Odeslat zpět post formuláře hodnocení k serveru:</span><span class="sxs-lookup"><span data-stu-id="0ead1-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="0ead1-130">V kódu na straně serveru, přístup k ovládacímu prvku hodnocení prostřednictvím jeho `ID` a přejděte k jeho `CurrentRating` vlastnost, která je počet položek vybraných hodnocení, v našem příkladu hodnotu od 0 do 5.</span><span class="sxs-lookup"><span data-stu-id="0ead1-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="0ead1-131">Uložit na stránku a jejich načtení do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0ead1-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="0ead1-132">Když najedete myší položky (původně prázdným) hodnocení, JavaScript účinek se vyskytuje: změny hodnocení.</span><span class="sxs-lookup"><span data-stu-id="0ead1-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="0ead1-133">Když kliknete na sadu hvězdiček, se uchovávají aktuální hodnocení.</span><span class="sxs-lookup"><span data-stu-id="0ead1-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="0ead1-134">Nakonec po odeslání formuláře kód na straně serveru vypíše vybrané hodnocení.</span><span class="sxs-lookup"><span data-stu-id="0ead1-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="0ead1-135">[![Vytvoření systému hodnocení s minimem kódování](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0ead1-135">[![Creating a rating system with minimal code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="0ead1-136">Vytvoření systému hodnocení s minimem kódu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0ead1-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0ead1-137">Next</span><span class="sxs-lookup"><span data-stu-id="0ead1-137">Next</span></span>](creating-a-rating-control-vb.md)
