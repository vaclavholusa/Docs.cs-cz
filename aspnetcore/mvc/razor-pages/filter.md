---
title: Filtr metody pro stránky Razor v ASP.NET Core
author: Rick-Anderson
description: Naučte se vytvořit filtr metody pro stránky Razor v ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/5/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/filter
ms.openlocfilehash: b04253b9240cb88c4f0d3824a4b9fda947d6da08
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/19/2018
ms.locfileid: "31585189"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a><span data-ttu-id="f15e8-103">Filtr metody pro stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f15e8-103">Filter methods for Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f15e8-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f15e8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f15e8-105">Filtry stránky Razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) a [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) povolit stránky Razor spustit kód před a po spuštění obslužnou rutinu stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="f15e8-105">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="f15e8-106">Filtry Razor stránce jsou podobná [filtrů Akce ASP.NET Core MVC](xref:mvc/controllers/filters#action-filters), s výjimkou je nelze použít s obslužnými rutinami jednotlivých stránek.</span><span class="sxs-lookup"><span data-stu-id="f15e8-106">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span> 

<span data-ttu-id="f15e8-107">Filtry Razor stránky:</span><span class="sxs-lookup"><span data-stu-id="f15e8-107">Razor Page filters:</span></span>

* <span data-ttu-id="f15e8-108">Spuštění kódu po nebyla vybrána metoda obslužná rutina, ale předtím, než dojde k vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="f15e8-108">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="f15e8-109">Kód spusťte před spuštěním metody obslužné rutiny, po dokončení vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="f15e8-109">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="f15e8-110">Po provedení metody obslužná rutina spusťte kód.</span><span class="sxs-lookup"><span data-stu-id="f15e8-110">Run code after the handler method executes.</span></span>
* <span data-ttu-id="f15e8-111">Může být implementováno na stránce nebo globálně.</span><span class="sxs-lookup"><span data-stu-id="f15e8-111">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="f15e8-112">Nelze použít s obslužnými rutinami konkrétní stránky.</span><span class="sxs-lookup"><span data-stu-id="f15e8-112">Cannot be applied to specific page handler methods.</span></span>

<span data-ttu-id="f15e8-113">Kód můžete spustit, než obslužná rutina metoda provádí pomocí konstruktoru stránky nebo middleware, ale pouze filtry stránky Razor mít přístup k [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span><span class="sxs-lookup"><span data-stu-id="f15e8-113">Code can be run before a handler method executes using the page constructor or middleware, but only Razor Page filters have access to [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span></span> <span data-ttu-id="f15e8-114">Mít filtry [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) odvozené parametr, který poskytuje přístup k `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="f15e8-114">Filters have a [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="f15e8-115">Například [implementovat atribut filtru](#ifa) ukázka přidá hlavičku odpovědi, něco, co nelze provést pomocí konstruktorů nebo middleware.</span><span class="sxs-lookup"><span data-stu-id="f15e8-115">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="f15e8-116">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/live/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f15e8-116">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f15e8-117">Stránka filtry Razor poskytují následující metody, které lze použít globálně nebo na úrovni stránky:</span><span class="sxs-lookup"><span data-stu-id="f15e8-117">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="f15e8-118">Synchronních metod:</span><span class="sxs-lookup"><span data-stu-id="f15e8-118">Synchronous methods:</span></span>

    * <span data-ttu-id="f15e8-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : volá se po nebyla vybrána metoda obslužná rutina, ale model, než dojde k vazby.</span><span class="sxs-lookup"><span data-stu-id="f15e8-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
    * <span data-ttu-id="f15e8-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : voláno před provedením metody obslužné rutiny, po dokončení vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="f15e8-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
    * <span data-ttu-id="f15e8-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : voláno po provedení metody obslužné rutiny, před výsledku akce.</span><span class="sxs-lookup"><span data-stu-id="f15e8-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="f15e8-122">Asynchronní metody:</span><span class="sxs-lookup"><span data-stu-id="f15e8-122">Asynchronous methods:</span></span>

    * <span data-ttu-id="f15e8-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : volána asynchronně po nebyla vybrána metoda obslužná rutina, ale model, než dojde k vazby.</span><span class="sxs-lookup"><span data-stu-id="f15e8-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
    * <span data-ttu-id="f15e8-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : volaná asynchronně před vyvoláním metody obslužné rutiny, po dokončení vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="f15e8-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

> [!NOTE]
> <span data-ttu-id="f15e8-125">Implementace **buď** synchronní nebo asynchronní verzi rozhraní filtru, nikoli oba dva.</span><span class="sxs-lookup"><span data-stu-id="f15e8-125">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="f15e8-126">Rozhraní framework zkontroluje nejprve Pokud filtr implementuje rozhraní asynchronní, a pokud ano, který volá.</span><span class="sxs-lookup"><span data-stu-id="f15e8-126">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="f15e8-127">Pokud tomu tak není, volá metodu nebo metody rozhraní synchronní.</span><span class="sxs-lookup"><span data-stu-id="f15e8-127">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="f15e8-128">Pokud jsou obě rozhraní implementované, se nazývají asynchronní metody.</span><span class="sxs-lookup"><span data-stu-id="f15e8-128">If both interfaces are implemented, only the async methods are be called.</span></span> <span data-ttu-id="f15e8-129">Stejné pravidlo platí pro přepsání v stránky, implementovat synchronní nebo asynchronní verzi přepsání, nikoli oba dva.</span><span class="sxs-lookup"><span data-stu-id="f15e8-129">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="f15e8-130">Globálně implementace filtrů stránky Razor</span><span class="sxs-lookup"><span data-stu-id="f15e8-130">Implement Razor Page filters globally</span></span>

<span data-ttu-id="f15e8-131">Následující kód implementuje `IAsyncPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="f15e8-131">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="f15e8-132">V předchozí kód [objektu ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) se nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="f15e8-132">In the preceding code, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) is not required.</span></span> <span data-ttu-id="f15e8-133">V ukázce slouží k zadání informací o trasování pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="f15e8-133">It's used in the sample to provide trace information for the application.</span></span>

<span data-ttu-id="f15e8-134">Následující kód umožňuje `SampleAsyncPageFilter` v `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="f15e8-134">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

<span data-ttu-id="f15e8-135">Následující kód ukazuje kompletní `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="f15e8-135">The following code shows the complete `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

<span data-ttu-id="f15e8-136">Následující kód volání `AddFolderApplicationModelConvention` pro použití `SampleAsyncPageFilter` na pouze stránky v */subFolder*:</span><span class="sxs-lookup"><span data-stu-id="f15e8-136">The following code calls `AddFolderApplicationModelConvention` to apply the `SampleAsyncPageFilter` to only pages in */subFolder*:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="f15e8-137">Následující kód implementuje synchronní `IPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="f15e8-137">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="f15e8-138">Následující kód umožňuje `SamplePageFilter`:</span><span class="sxs-lookup"><span data-stu-id="f15e8-138">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"
## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="f15e8-139">Implementace filtru stránek Razor přepsáním metody filtru</span><span class="sxs-lookup"><span data-stu-id="f15e8-139">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="f15e8-140">Následující kód přepíše synchronní filtry Razor stránky:</span><span class="sxs-lookup"><span data-stu-id="f15e8-140">The following code overrides the synchronous Razor Page filters:</span></span>

<span data-ttu-id="f15e8-141">[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="f15e8-141">[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]</span></span>

::: moniker-end

<a name="ifa"></a>
## <a name="implement-a-filter-attribute"></a><span data-ttu-id="f15e8-142">Implementace atribut filtru</span><span class="sxs-lookup"><span data-stu-id="f15e8-142">Implement a filter attribute</span></span>

<span data-ttu-id="f15e8-143">Integrovaný filtr na základě atributů [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) může být rozčlenění filtru.</span><span class="sxs-lookup"><span data-stu-id="f15e8-143">The built-in attribute-based filter [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filter can be subclassed.</span></span> <span data-ttu-id="f15e8-144">Filtr přidá do odpovědi hlavičku:</span><span class="sxs-lookup"><span data-stu-id="f15e8-144">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="f15e8-145">Následující kód se vztahuje `AddHeader` atribut:</span><span class="sxs-lookup"><span data-stu-id="f15e8-145">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

<span data-ttu-id="f15e8-146">V tématu [přepsání výchozí pořadí](xref:mvc/controllers/filters#overriding-the-default-order) pokyny k přepsání pořadí.</span><span class="sxs-lookup"><span data-stu-id="f15e8-146">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="f15e8-147">V tématu [zrušení a krátký circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) pokyny týkající se krátká smyčka kanálu filtru z filtru.</span><span class="sxs-lookup"><span data-stu-id="f15e8-147">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span> 

<a name="auth"></a>
## <a name="authorize-filter-attribute"></a><span data-ttu-id="f15e8-148">Autorizovat atribut filtru</span><span class="sxs-lookup"><span data-stu-id="f15e8-148">Authorize filter attribute</span></span>

<span data-ttu-id="f15e8-149">[Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) atribut lze použít k `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="f15e8-149">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
