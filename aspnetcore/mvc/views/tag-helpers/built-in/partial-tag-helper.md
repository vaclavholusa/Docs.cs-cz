---
title: Pomocník částečné značky ASP.NET Core
author: scottaddie
description: Zjistit pomocná částečné značky ASP.NET Core a roli každý z jeho atributy hrát v vykreslení částečného zobrazení.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 4573409720ccba524c0d5d05696e0fb3468ff338
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/03/2018
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="11cf5-103">Pomocník částečné značky ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="11cf5-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="11cf5-104">Podle [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="11cf5-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="11cf5-105">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="11cf5-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="11cf5-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="11cf5-106">Overview</span></span>

<span data-ttu-id="11cf5-107">Částečné pomocná značky se používá pro vykreslování [částečné zobrazení](xref:mvc/views/partial) v aplikacích pro stránky Razor a MVC.</span><span class="sxs-lookup"><span data-stu-id="11cf5-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="11cf5-108">Zvažte to:</span><span class="sxs-lookup"><span data-stu-id="11cf5-108">Consider that it:</span></span>

* <span data-ttu-id="11cf5-109">Vyžaduje ASP.NET Core 2.1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="11cf5-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="11cf5-110">Představuje alternativu k [pomocné rutiny HTML syntaxe](xref:mvc/views/partial#referencing-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="11cf5-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#referencing-a-partial-view).</span></span>
* <span data-ttu-id="11cf5-111">Asynchronně vykreslí částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="11cf5-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="11cf5-112">Možnosti pomocné rutiny HTML pro vykreslení částečného zobrazení:</span><span class="sxs-lookup"><span data-stu-id="11cf5-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="11cf5-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="11cf5-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="11cf5-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="11cf5-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="11cf5-115">*Produktu* model se používá v ukázky v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="11cf5-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="11cf5-116">Následuje inventář atributy částečné pomocná značky.</span><span class="sxs-lookup"><span data-stu-id="11cf5-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="11cf5-117">name</span><span class="sxs-lookup"><span data-stu-id="11cf5-117">name</span></span>

<span data-ttu-id="11cf5-118">`name` Atribut je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="11cf5-118">The `name` attribute is required.</span></span> <span data-ttu-id="11cf5-119">Označuje název nebo cesta částečné zobrazení k vykreslení.</span><span class="sxs-lookup"><span data-stu-id="11cf5-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="11cf5-120">Pokud je zadaný název částečného zobrazení, [zobrazení zjišťování](xref:mvc/views/overview#view-discovery) proces je zahájen.</span><span class="sxs-lookup"><span data-stu-id="11cf5-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="11cf5-121">Pokud je k dispozici explicitní cestu přeskočí tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="11cf5-121">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="11cf5-122">Následující kód používá explicitní cestu, která znamená, že *_ProductPartial.cshtml* je třeba načíst z *sdílené* složky.</span><span class="sxs-lookup"><span data-stu-id="11cf5-122">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="11cf5-123">Pomocí [asp-pro](#asp-for) atribut model je předán částečné zobrazení pro vazbu.</span><span class="sxs-lookup"><span data-stu-id="11cf5-123">Using the [asp-for](#asp-for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="asp-for"></a><span data-ttu-id="11cf5-124">ASP-pro</span><span class="sxs-lookup"><span data-stu-id="11cf5-124">asp-for</span></span>

<span data-ttu-id="11cf5-125">`asp-for` Atribut přiřadí [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) má být porovnán s aktuální model.</span><span class="sxs-lookup"><span data-stu-id="11cf5-125">The `asp-for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="11cf5-126">A `ModelExpression` odvodí, že `@Model.` syntaxe.</span><span class="sxs-lookup"><span data-stu-id="11cf5-126">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="11cf5-127">Například `asp-for="Product"` lze použít místo `asp-for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="11cf5-127">For example, `asp-for="Product"` can be used instead of `asp-for="@Model.Product"`.</span></span> <span data-ttu-id="11cf5-128">Toto výchozí chování odvození je přepsat pomocí `@` symbol definovat výraz vložené.</span><span class="sxs-lookup"><span data-stu-id="11cf5-128">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span>

<span data-ttu-id="11cf5-129">Následující kód načte *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="11cf5-129">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_AspFor)]

<span data-ttu-id="11cf5-130">Částečné zobrazení je vázán k modelu přidružené stránky `Product` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="11cf5-130">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="view-data"></a><span data-ttu-id="11cf5-131">view-data</span><span class="sxs-lookup"><span data-stu-id="11cf5-131">view-data</span></span>

<span data-ttu-id="11cf5-132">`view-data` Atribut přiřadí [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) mají být předána do částečného zobrazení.</span><span class="sxs-lookup"><span data-stu-id="11cf5-132">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="11cf5-133">Následující kód, zpřístupní se celou kolekci ViewData částečného zobrazení:</span><span class="sxs-lookup"><span data-stu-id="11cf5-133">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="11cf5-134">V předchozí kód `IsNumberReadOnly` nastavena na hodnotu klíče `true` a přidat do kolekce ViewData.</span><span class="sxs-lookup"><span data-stu-id="11cf5-134">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="11cf5-135">V důsledku toho `ViewData["IsNumberReadOnly"]` zpřístupněna v rámci následující částečné zobrazení:</span><span class="sxs-lookup"><span data-stu-id="11cf5-135">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="11cf5-136">V tomto příkladu hodnota `ViewData["IsNumberReadOnly"]` Určuje, zda *číslo* pole se zobrazí jako jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="11cf5-136">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11cf5-137">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="11cf5-137">Additional resources</span></span>

* [<span data-ttu-id="11cf5-138">Částečná zobrazení</span><span class="sxs-lookup"><span data-stu-id="11cf5-138">Partial views</span></span>](xref:mvc/views/partial)
* [<span data-ttu-id="11cf5-139">Slabě typovaná data (ViewData a ViewBag)</span><span class="sxs-lookup"><span data-stu-id="11cf5-139">Weakly typed data (ViewData and ViewBag)</span></span>](xref:mvc/views/overview#weakly-typed-data-viewdata-and-viewbag)
