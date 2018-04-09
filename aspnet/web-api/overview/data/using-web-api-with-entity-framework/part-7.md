---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Vytvoření zobrazení (UI) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 5052d7cca4a5c12a9ea56eb929d4794b19e82603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="create-the-view-ui"></a><span data-ttu-id="df979-102">Vytvoření zobrazení (UI)</span><span class="sxs-lookup"><span data-stu-id="df979-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="df979-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="df979-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="df979-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="df979-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="df979-105">V této části začnete definovat HTML pro aplikaci a přidat vazby dat mezi HTML a zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="df979-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="df979-106">Otevřete soubor Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="df979-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="df979-107">Celý obsah souboru nahraďte následujícím textem.</span><span class="sxs-lookup"><span data-stu-id="df979-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="df979-108">Většina `div` elementy jsou na [Bootstrap](http://getbootstrap.com/) styly.</span><span class="sxs-lookup"><span data-stu-id="df979-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="df979-109">Důležité elementy, jsou ty, s `data-bind` atributy.</span><span class="sxs-lookup"><span data-stu-id="df979-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="df979-110">Tento atribut odkazů HTML do modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="df979-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="df979-111">Příklad:</span><span class="sxs-lookup"><span data-stu-id="df979-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="df979-112">V tomto příkladu &quot; `text` &quot; vazby příčiny `<p>` elementu, který chcete zobrazit hodnotu `error` vlastnost z modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="df979-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="df979-113">Odvolat, `error` byla deklarována jako `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="df979-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="df979-114">Vždy, když je přiřazen novou hodnotu `error`, Knockout text v aktualizovat `<p>` elementu.</span><span class="sxs-lookup"><span data-stu-id="df979-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="df979-115">`foreach` Vazby informuje Knockout můžete procházet obsah `books` pole.</span><span class="sxs-lookup"><span data-stu-id="df979-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="df979-116">Pro každou položku v poli, vytvoří novou Knockout &lt;li&gt; elementu.</span><span class="sxs-lookup"><span data-stu-id="df979-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="df979-117">Vazby v kontextu `foreach` odkazovat na vlastnosti položky pole.</span><span class="sxs-lookup"><span data-stu-id="df979-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="df979-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="df979-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="df979-119">Zde `text` vazby čte vlastnost Autor každé adresáře.</span><span class="sxs-lookup"><span data-stu-id="df979-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="df979-120">Pokud nyní aplikaci spustíte, by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="df979-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="df979-121">Seznam seznamů, načte asynchronně, po načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="df979-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="df979-122">Právě teď &quot;podrobnosti&quot; odkazy nejsou funkční.</span><span class="sxs-lookup"><span data-stu-id="df979-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="df979-123">Tato funkce přidáme v další části.</span><span class="sxs-lookup"><span data-stu-id="df979-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="df979-124">[Předchozí](part-6.md)
> [další](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="df979-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
