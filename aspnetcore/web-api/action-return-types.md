---
title: Návratové typy akcí kontroleru v rozhraní Web API ASP.NET Core
author: scottaddie
description: Další informace o použití různé metody návratové typy akcí kontroleru v ASP.NET Core Web API.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: web-api/action-return-types
ms.openlocfilehash: 179a3e23ebc13a40b8e2d955b6adcc23d9a0f323
ms.sourcegitcommit: 8268cc67beb1bb1ca470abb0e28b15a7a71b8204
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44126719"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="8b75f-103">Návratové typy akcí kontroleru v rozhraní Web API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b75f-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="8b75f-104">Podle [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="8b75f-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="8b75f-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8b75f-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8b75f-106">ASP.NET Core nabízí že následující možnosti pro akce kontroleru webového rozhraní API vrací typy:</span><span class="sxs-lookup"><span data-stu-id="8b75f-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="8b75f-107">Konkrétní typ</span><span class="sxs-lookup"><span data-stu-id="8b75f-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="8b75f-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="8b75f-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="8b75f-109">ActionResult\<T ></span><span class="sxs-lookup"><span data-stu-id="8b75f-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="8b75f-110">Konkrétní typ</span><span class="sxs-lookup"><span data-stu-id="8b75f-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="8b75f-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="8b75f-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="8b75f-112">Tento dokument vysvětluje, kdy je nejvhodnější použít každý návratový typ.</span><span class="sxs-lookup"><span data-stu-id="8b75f-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="8b75f-113">Konkrétní typ</span><span class="sxs-lookup"><span data-stu-id="8b75f-113">Specific type</span></span>

<span data-ttu-id="8b75f-114">Nejjednodušší akce vrátí primitivních nebo složitých datový typ (například `string` nebo zadejte vlastní objekt).</span><span class="sxs-lookup"><span data-stu-id="8b75f-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="8b75f-115">Vezměte v úvahu následující akce, která vrátí kolekci vlastní `Product` objekty:</span><span class="sxs-lookup"><span data-stu-id="8b75f-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="8b75f-116">Vrácení specifického typu může stačit bez známých podmínky k ochraně proti během provádění akce.</span><span class="sxs-lookup"><span data-stu-id="8b75f-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="8b75f-117">Předchozí akci nepřijímá žádné parametry, takže není potřeba omezení ověření parametru.</span><span class="sxs-lookup"><span data-stu-id="8b75f-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="8b75f-118">Když známé podmínky musí být zahrnuté pro jedná o smluvní jednání, jsou zavedeny více návratový cesty.</span><span class="sxs-lookup"><span data-stu-id="8b75f-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="8b75f-119">V takovém případě je běžné kombinovat [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) návratový typ s návratovým typem primitivních nebo složitých.</span><span class="sxs-lookup"><span data-stu-id="8b75f-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="8b75f-120">Buď [IActionResult](#iactionresult-type) nebo [ActionResult\<T >](#actionresultt-type) jsou nezbytné pro tento typ akce.</span><span class="sxs-lookup"><span data-stu-id="8b75f-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="8b75f-121">Typ IActionResult</span><span class="sxs-lookup"><span data-stu-id="8b75f-121">IActionResult type</span></span>

<span data-ttu-id="8b75f-122">[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) návratový typ je vhodné, když více [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) vrátí typy je možné v akci.</span><span class="sxs-lookup"><span data-stu-id="8b75f-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="8b75f-123">`ActionResult` Typy představují různé stavových kódů HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b75f-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="8b75f-124">Jsou některé běžné návratové typy spadající do této kategorie [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400) [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), a [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span><span class="sxs-lookup"><span data-stu-id="8b75f-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="8b75f-125">Protože jsou více návratových typů a cesty v akci, liberální použití [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) je atribut nezbytný.</span><span class="sxs-lookup"><span data-stu-id="8b75f-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="8b75f-126">Tento atribut vytváří více popisné podrobnosti o odpovědi pro stránky nápovědy rozhraní API generovaných nástrojů, jako je [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="8b75f-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="8b75f-127">`[ProducesResponseType]` označuje známé typy a stavové kódy HTTP, který se má vrátit akce.</span><span class="sxs-lookup"><span data-stu-id="8b75f-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="8b75f-128">Synchronní akce</span><span class="sxs-lookup"><span data-stu-id="8b75f-128">Synchronous action</span></span>

<span data-ttu-id="8b75f-129">Vezměte v úvahu následující synchronní akce, ve kterém jsou dvě možné návratové typy:</span><span class="sxs-lookup"><span data-stu-id="8b75f-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="8b75f-130">V předchozí akci, se vrátí stavový kód 404 při produktu reprezentována `id` podkladových dat v úložišti neexistuje.</span><span class="sxs-lookup"><span data-stu-id="8b75f-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="8b75f-131">[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) Pomocná metoda, je vyvolána jako zástupce `return new NotFoundResult();`.</span><span class="sxs-lookup"><span data-stu-id="8b75f-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="8b75f-132">Pokud produkt existuje, `Product` objekt představující datové části se vrátil s kódem stavový kód 200.</span><span class="sxs-lookup"><span data-stu-id="8b75f-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="8b75f-133">[Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) Pomocná metoda, je vyvolána jako zkrácený zápis `return new OkObjectResult(product);`.</span><span class="sxs-lookup"><span data-stu-id="8b75f-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="8b75f-134">Asynchronní akce</span><span class="sxs-lookup"><span data-stu-id="8b75f-134">Asynchronous action</span></span>

<span data-ttu-id="8b75f-135">Vezměte v úvahu následující asynchronní akce, ve kterém jsou dvě možné návratové typy:</span><span class="sxs-lookup"><span data-stu-id="8b75f-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="8b75f-136">V předchozí akci, se vrátí stavový kód 400, pokud selže ověření modelu a [chybného požadavku](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) vyvolání Pomocná metoda.</span><span class="sxs-lookup"><span data-stu-id="8b75f-136">In the preceding action, a 400 status code is returned when model validation fails and the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) helper method is invoked.</span></span> <span data-ttu-id="8b75f-137">Například následující model označuje, že musíte zadat požadavky `Name` vlastnosti a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8b75f-137">For example, the following model indicates that requests must provide the `Name` property and a value.</span></span> <span data-ttu-id="8b75f-138">Proto selhání zajistit správnou `Name` v požadavku způsobí selhání ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="8b75f-138">Therefore, failure to provide a proper `Name` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

<span data-ttu-id="8b75f-139">Předchozí akce další známé návratový kód je 201, který je generován [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) Pomocná metoda.</span><span class="sxs-lookup"><span data-stu-id="8b75f-139">The preceding action's other known return code is a 201, which is generated by the [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) helper method.</span></span> <span data-ttu-id="8b75f-140">V této cestě `Product` je vrácen objekt.</span><span class="sxs-lookup"><span data-stu-id="8b75f-140">In this path, the `Product` object is returned.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="actionresultt-type"></a><span data-ttu-id="8b75f-141">ActionResult\<T > typ</span><span class="sxs-lookup"><span data-stu-id="8b75f-141">ActionResult\<T> type</span></span>

<span data-ttu-id="8b75f-142">ASP.NET Core 2.1 přináší [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) návratový typ pro akce kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8b75f-142">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="8b75f-143">Umožňuje návratový typ odvozený od [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) nebo se vraťte [konkrétní typ](#specific-type).</span><span class="sxs-lookup"><span data-stu-id="8b75f-143">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="8b75f-144">`ActionResult<T>` nabízí následující výhody oproti [IActionResult typ](#iactionresult-type):</span><span class="sxs-lookup"><span data-stu-id="8b75f-144">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="8b75f-145">[[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) atributu `Type` vlastnost je možné vyloučit z replikace.</span><span class="sxs-lookup"><span data-stu-id="8b75f-145">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="8b75f-146">Například `[ProducesResponseType(200, Type = typeof(Product))]` je zjednodušenou `[ProducesResponseType(200)]`.</span><span class="sxs-lookup"><span data-stu-id="8b75f-146">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="8b75f-147">Očekávaný návratový typ je odvozen namísto z akce `T` v `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="8b75f-147">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="8b75f-148">[Implicitní přetypování operátory](/dotnet/csharp/language-reference/keywords/implicit) podporu převodu obou `T` a `ActionResult` k `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="8b75f-148">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="8b75f-149">`T` převede na [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), což znamená, že `return new ObjectResult(T);` je zjednodušenou `return T;`.</span><span class="sxs-lookup"><span data-stu-id="8b75f-149">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="8b75f-150">C# nepodporuje operátory implicitní přetypování na rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8b75f-150">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="8b75f-151">V důsledku toho je potřeba použít převod rozhraní do konkrétního typu implementujícího typ `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="8b75f-151">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="8b75f-152">Například použití `IEnumerable` nebude fungovat v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="8b75f-152">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

<span data-ttu-id="8b75f-153">Jedna možnost, chcete-li vyřešit předchozí kód je vrátit `_repository.GetProducts().ToList();`.</span><span class="sxs-lookup"><span data-stu-id="8b75f-153">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="8b75f-154">Většinu akcí mít návratový typ konkrétní.</span><span class="sxs-lookup"><span data-stu-id="8b75f-154">Most actions have a specific return type.</span></span> <span data-ttu-id="8b75f-155">Neočekávané podmínky může dojít během provádění akce, ve kterém případ konkrétní typ nevrátí.</span><span class="sxs-lookup"><span data-stu-id="8b75f-155">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="8b75f-156">Vstupní parametr akce může například selhat ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="8b75f-156">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="8b75f-157">V takovém případě je běžné vrátí odpovídající `ActionResult` typ místo konkrétního typu.</span><span class="sxs-lookup"><span data-stu-id="8b75f-157">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="8b75f-158">Synchronní akce</span><span class="sxs-lookup"><span data-stu-id="8b75f-158">Synchronous action</span></span>

<span data-ttu-id="8b75f-159">Vezměte v úvahu synchronní akce, ve kterém jsou dvě možné návratové typy:</span><span class="sxs-lookup"><span data-stu-id="8b75f-159">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="8b75f-160">V předchozím kódu je vrátil stavový kód 404 při produktu v databázi neexistuje.</span><span class="sxs-lookup"><span data-stu-id="8b75f-160">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="8b75f-161">Pokud produkt neexistuje, odpovídající `Product` je vrácen objekt.</span><span class="sxs-lookup"><span data-stu-id="8b75f-161">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="8b75f-162">Před ASP.NET Core 2.1 `return product;` řádku by byl `return Ok(product);`.</span><span class="sxs-lookup"><span data-stu-id="8b75f-162">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="8b75f-163">K ASP.NET Core 2.1 je povolená odvození zdroj vazby parametrů akce, když je doplněn třídu kontroleru `[ApiController]` atribut.</span><span class="sxs-lookup"><span data-stu-id="8b75f-163">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="8b75f-164">Název parametru názvu v šabloně trasy automaticky svázán pomocí žádosti o data trasy.</span><span class="sxs-lookup"><span data-stu-id="8b75f-164">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="8b75f-165">V důsledku toho předchozí akce `id` parametr není explicitně opatřen poznámkou [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="8b75f-165">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="8b75f-166">Asynchronní akce</span><span class="sxs-lookup"><span data-stu-id="8b75f-166">Asynchronous action</span></span>

<span data-ttu-id="8b75f-167">Vezměte v úvahu asynchronní akce, ve kterém jsou dvě možné návratové typy:</span><span class="sxs-lookup"><span data-stu-id="8b75f-167">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="8b75f-168">Pokud selže ověření modelu [chybného požadavku](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) vyvolána metoda vrátit stavový kód 400.</span><span class="sxs-lookup"><span data-stu-id="8b75f-168">If model validation fails, the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) method is invoked to return a 400 status code.</span></span> <span data-ttu-id="8b75f-169">[ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) vlastnost obsahující chyby ověřování podle je předán do něj.</span><span class="sxs-lookup"><span data-stu-id="8b75f-169">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property containing the specific validation errors is passed to it.</span></span> <span data-ttu-id="8b75f-170">V případě úspěšného ověření modelu se vytvoří produktu v databázi.</span><span class="sxs-lookup"><span data-stu-id="8b75f-170">If model validation succeeds, the product is created in the database.</span></span> <span data-ttu-id="8b75f-171">Se vrátí 201 stavový kód.</span><span class="sxs-lookup"><span data-stu-id="8b75f-171">A 201 status code is returned.</span></span>

> [!TIP]
> <span data-ttu-id="8b75f-172">K ASP.NET Core 2.1 je povolená odvození zdroj vazby parametrů akce, když je doplněn třídu kontroleru `[ApiController]` atribut.</span><span class="sxs-lookup"><span data-stu-id="8b75f-172">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="8b75f-173">Komplexní typ parametry jsou automaticky svázán pomocí textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="8b75f-173">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="8b75f-174">V důsledku toho předchozí akce `product` parametr není explicitně opatřen poznámkou [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="8b75f-174">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="8b75f-175">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8b75f-175">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
