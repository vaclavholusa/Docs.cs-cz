---
title: Pomocná rutina částečné značky v ASP.NET Core
author: scottaddie
description: Zjišťování ASP.NET Core částečné pomocné rutiny značky a role, každá z jeho atributy přehrávání ve vykreslení částečného zobrazení.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 2272b2ecdd6f2b0a759356b1f03dd5c495ea1c91
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889100"
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="19bca-103">Pomocná rutina částečné značky v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19bca-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="19bca-104">Podle [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="19bca-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="19bca-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="19bca-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="19bca-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="19bca-106">Overview</span></span>

<span data-ttu-id="19bca-107">Pomocná rutina částečné značky se používá pro vykreslování [částečné zobrazení](xref:mvc/views/partial) v aplikacích pro Razor Pages a MVC.</span><span class="sxs-lookup"><span data-stu-id="19bca-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="19bca-108">Vezměte v úvahu, že:</span><span class="sxs-lookup"><span data-stu-id="19bca-108">Consider that it:</span></span>

* <span data-ttu-id="19bca-109">Vyžaduje ASP.NET Core 2.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="19bca-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="19bca-110">Představuje alternativu k [pomocné rutiny HTML syntaxe](xref:mvc/views/partial#reference-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="19bca-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#reference-a-partial-view).</span></span>
* <span data-ttu-id="19bca-111">Vykreslí částečné zobrazení asynchronně.</span><span class="sxs-lookup"><span data-stu-id="19bca-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="19bca-112">Možnosti pomocné rutiny HTML pro vykreslení částečného zobrazení:</span><span class="sxs-lookup"><span data-stu-id="19bca-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="19bca-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="19bca-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="19bca-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="19bca-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="19bca-115">*Produktu* model se používá v ukázky v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="19bca-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="19bca-116">Následuje seznam atributů pomocná rutina částečné značky.</span><span class="sxs-lookup"><span data-stu-id="19bca-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="19bca-117">name</span><span class="sxs-lookup"><span data-stu-id="19bca-117">name</span></span>

<span data-ttu-id="19bca-118">`name` Atribut je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="19bca-118">The `name` attribute is required.</span></span> <span data-ttu-id="19bca-119">Označuje název nebo cesta k vykreslení částečného zobrazení.</span><span class="sxs-lookup"><span data-stu-id="19bca-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="19bca-120">Když je zadaný název částečného zobrazení, [zobrazení zjišťování](xref:mvc/views/overview#view-discovery) zahájit proces.</span><span class="sxs-lookup"><span data-stu-id="19bca-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="19bca-121">Tento proces je obejít, když je k dispozici explicitní cestu.</span><span class="sxs-lookup"><span data-stu-id="19bca-121">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="19bca-122">Následující kód používá cestu k explicitní, což indikuje, že *_ProductPartial.cshtml* má být načtena z *Shared* složky.</span><span class="sxs-lookup"><span data-stu-id="19bca-122">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="19bca-123">Použití [pro](#for) atribut, modelu je předán do částečné zobrazení pro vazbu.</span><span class="sxs-lookup"><span data-stu-id="19bca-123">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="19bca-124">pro</span><span class="sxs-lookup"><span data-stu-id="19bca-124">for</span></span>

<span data-ttu-id="19bca-125">`for` Atribut přiřadí [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) má být porovnán s aktuální model.</span><span class="sxs-lookup"><span data-stu-id="19bca-125">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="19bca-126">A `ModelExpression` odvodí, `@Model.` syntaxe.</span><span class="sxs-lookup"><span data-stu-id="19bca-126">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="19bca-127">Například `for="Product"` lze použít místo `for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="19bca-127">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="19bca-128">Toto výchozí chování odvození přepsán pomocí `@` symbolů k definování vložený výraz.</span><span class="sxs-lookup"><span data-stu-id="19bca-128">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="19bca-129">`for` Atributu nelze použít s [modelu](#model) atribut.</span><span class="sxs-lookup"><span data-stu-id="19bca-129">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="19bca-130">Následující kód načte *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="19bca-130">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="19bca-131">Částečné zobrazení je vázán na příslušné stránce modelu `Product` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="19bca-131">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="19bca-132">model</span><span class="sxs-lookup"><span data-stu-id="19bca-132">model</span></span>

<span data-ttu-id="19bca-133">`model` Atribut přiřadí instance modelu má být předána do částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="19bca-133">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="19bca-134">`model` Atributu nelze použít s [pro](#for) atribut.</span><span class="sxs-lookup"><span data-stu-id="19bca-134">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="19bca-135">V následujícím kódu novou `Product` objektu je vytvořena instance a předat `model` atribut pro vazbu:</span><span class="sxs-lookup"><span data-stu-id="19bca-135">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="19bca-136">view-data</span><span class="sxs-lookup"><span data-stu-id="19bca-136">view-data</span></span>

<span data-ttu-id="19bca-137">`view-data` Atribut přiřadí [objektu ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) k předání do částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="19bca-137">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="19bca-138">Následující kód zpřístupňuje celou kolekci slovníku ViewData částečného zobrazení:</span><span class="sxs-lookup"><span data-stu-id="19bca-138">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="19bca-139">V předchozím kódu `IsNumberReadOnly` nastavena na hodnotu klíče `true` a přidat do kolekce slovníku ViewData.</span><span class="sxs-lookup"><span data-stu-id="19bca-139">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="19bca-140">V důsledku toho `ViewData["IsNumberReadOnly"]` zpřístupněna v rámci následující částečné zobrazení:</span><span class="sxs-lookup"><span data-stu-id="19bca-140">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="19bca-141">V tomto příkladu hodnota `ViewData["IsNumberReadOnly"]` Určuje, zda *číslo* zobrazí pole jako jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="19bca-141">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="migrate-from-an-html-helper"></a><span data-ttu-id="19bca-142">Migrace ze pomocné rutiny HTML</span><span class="sxs-lookup"><span data-stu-id="19bca-142">Migrate from an HTML Helper</span></span>

<span data-ttu-id="19bca-143">Zvažte následující příklad asynchronní pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="19bca-143">Consider the following asynchronous HTML Helper example.</span></span> <span data-ttu-id="19bca-144">Kolekce produktů provést iteraci a zobrazit.</span><span class="sxs-lookup"><span data-stu-id="19bca-144">A collection of products is iterated and displayed.</span></span> <span data-ttu-id="19bca-145">Za `PartialAsync` první parametr metody, *_ProductPartial.cshtml* načtení částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="19bca-145">Per the `PartialAsync` method's first parameter, the *_ProductPartial.cshtml* partial view is loaded.</span></span> <span data-ttu-id="19bca-146">Instance `Product` model částečného zobrazení předán pro vazbu.</span><span class="sxs-lookup"><span data-stu-id="19bca-146">An instance of the `Product` model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_HtmlHelper&highlight=3)]

<span data-ttu-id="19bca-147">Následující pomocná rutina částečné značky dosáhne stejné chování jako asynchronní vykreslování `PartialAsync` pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="19bca-147">The following Partial Tag Helper achieves the same asynchronous rendering behavior as the `PartialAsync` HTML Helper.</span></span> <span data-ttu-id="19bca-148">`model` Atributu je přiřazena `Product` instanci modelu pro vazbu na částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="19bca-148">The `model` attribute is assigned a `Product` model instance for binding to the partial view.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_TagHelper&highlight=3)]

## <a name="additional-resources"></a><span data-ttu-id="19bca-149">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="19bca-149">Additional resources</span></span>

* <xref:mvc/views/partial>
* <xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag>