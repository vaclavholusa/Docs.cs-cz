---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Vytvoření zobrazení (uživatelského rozhraní) | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: e9ebe60f88ecbf65a6f8d04de9a23d72a72fda83
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364978"
---
<a name="create-the-view-ui"></a><span data-ttu-id="9e6db-102">Vytvoření zobrazení (uživatelského rozhraní)</span><span class="sxs-lookup"><span data-stu-id="9e6db-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="9e6db-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9e6db-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9e6db-104">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="9e6db-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="9e6db-105">V této části se začnou definovat kód HTML pro aplikace a přidání datové vazby mezi HTML a model zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9e6db-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="9e6db-106">Otevřete soubor Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="9e6db-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="9e6db-107">Celý obsah tohoto souboru nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="9e6db-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="9e6db-108">Většina `div` prvky jsou pro [Bootstrap](http://getbootstrap.com/) práce se styly.</span><span class="sxs-lookup"><span data-stu-id="9e6db-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="9e6db-109">Důležité prvky jsou ty, které se `data-bind` atributy.</span><span class="sxs-lookup"><span data-stu-id="9e6db-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="9e6db-110">Tento atribut odkazuje na model zobrazení HTML.</span><span class="sxs-lookup"><span data-stu-id="9e6db-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="9e6db-111">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9e6db-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="9e6db-112">V tomto příkladu &quot; `text` &quot; vazby způsobí, že `<p>` prvek, který chcete zobrazit hodnotu `error` vlastnost z modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9e6db-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="9e6db-113">Vzpomeňte si, že `error` byl deklarován jako `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="9e6db-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="9e6db-114">Vždy, když je nová hodnota přiřazená `error`, Knockout aktualizace textu `<p>` element.</span><span class="sxs-lookup"><span data-stu-id="9e6db-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="9e6db-115">`foreach` Vazby říká Knockout pro cyklický průchod obsah `books` pole.</span><span class="sxs-lookup"><span data-stu-id="9e6db-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="9e6db-116">Pro každou položku v poli, vytvoří novou Knockout &lt;li&gt; elementu.</span><span class="sxs-lookup"><span data-stu-id="9e6db-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="9e6db-117">Vazby v kontextu `foreach` odkazují na vlastnosti pro položku pole.</span><span class="sxs-lookup"><span data-stu-id="9e6db-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="9e6db-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9e6db-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="9e6db-119">Tady `text` vazby načte vlastnost Autor jednotlivé knihy.</span><span class="sxs-lookup"><span data-stu-id="9e6db-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="9e6db-120">Pokud nyní aplikaci spustíte, by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="9e6db-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="9e6db-121">Seznam knihy načte asynchronně, po načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="9e6db-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="9e6db-122">Právě teď &quot;podrobnosti&quot; odkazy nejsou funkční.</span><span class="sxs-lookup"><span data-stu-id="9e6db-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="9e6db-123">Tato funkce přidáme v další části.</span><span class="sxs-lookup"><span data-stu-id="9e6db-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9e6db-124">[Předchozí](part-6.md)
> [další](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="9e6db-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
