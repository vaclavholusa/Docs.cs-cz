---
title: Návratové typy akcí kontroleru v rozhraní Web API ASP.NET Core
author: scottaddie
description: Další informace o použití různé metody návratové typy akcí kontroleru v ASP.NET Core Web API.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/21/2018
uid: web-api/action-return-types
ms.openlocfilehash: 422db97da222fb5e742e1d8e6ae410edc90dbc18
ms.sourcegitcommit: ee2b26c7d08b38c908c668522554b52ab8efa221
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2018
ms.locfileid: "36273553"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>Návratové typy akcí kontroleru v rozhraní Web API ASP.NET Core

Podle [Scott Addie](https://github.com/scottaddie)

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

ASP.NET Core nabízí že následující možnosti pro akce kontroleru webového rozhraní API vrací typy:

::: moniker range="<= aspnetcore-2.0"
* [Konkrétní typ](#specific-type)
* [IActionResult](#iactionresult-type)
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
* [Konkrétní typ](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T >](#actionresultt-type)
::: moniker-end

Tento dokument vysvětluje, kdy je nejvhodnější použít každý návratový typ.

## <a name="specific-type"></a>Konkrétní typ

Nejjednodušší akce vrátí primitivních nebo složitých datový typ (například `string` nebo zadejte vlastní objekt). Vezměte v úvahu následující akce, která vrátí kolekci vlastní `Product` objekty:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Vrácení specifického typu může stačit bez známých podmínky k ochraně proti během provádění akce. Předchozí akci nepřijímá žádné parametry, takže není potřeba omezení ověření parametru.

Když známé podmínky musí být zahrnuté pro jedná o smluvní jednání, jsou zavedeny více návratový cesty. V takovém případě je běžné kombinovat [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) návratový typ s návratovým typem primitivních nebo složitých. Buď [IActionResult](#iactionresult-type) nebo [ActionResult\<T >](#actionresultt-type) jsou nezbytné pro tento typ akce.

## <a name="iactionresult-type"></a>Typ IActionResult

[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) návratový typ je vhodné, když více [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) vrátí typy je možné v akci. `ActionResult` Typy představují různé stavových kódů HTTP. Jsou některé běžné návratové typy spadající do této kategorie [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400) [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), a [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).

Protože jsou více návratových typů a cesty v akci, liberální použití [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) je atribut nezbytný. Tento atribut vytváří více popisné podrobnosti o odpovědi pro stránky nápovědy rozhraní API generovaných nástrojů, jako je [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger). `[ProducesResponseType]` označuje známé typy a stavové kódy HTTP, který se má vrátit akce.

### <a name="synchronous-action"></a>Synchronní akce

Vezměte v úvahu následující synchronní akce, ve kterém jsou dvě možné návratové typy:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

V předchozí akci, se vrátí stavový kód 404 při produktu reprezentována `id` podkladových dat v úložišti neexistuje. [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) Pomocná metoda, je vyvolána jako zástupce `return new NotFoundResult();`. Pokud produkt existuje, `Product` objekt představující datové části se vrátil s kódem stavový kód 200. [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) Pomocná metoda, je vyvolána jako zkrácený zápis `return new OkObjectResult(product);`.

### <a name="asynchronous-action"></a>Asynchronní akce

Vezměte v úvahu následující asynchronní akce, ve kterém jsou dvě možné návratové typy:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

V předchozí akci, se vrátí stavový kód 400, pokud selže ověření modelu a [chybného požadavku](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) vyvolání Pomocná metoda. Například následující model označuje, že musíte zadat požadavky `Name` vlastnosti a hodnotu. Proto selhání zajistit správnou `Name` v požadavku způsobí selhání ověření modelu.

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

Předchozí akce další známé návratový kód je 201, který je generován [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) Pomocná metoda. V této cestě `Product` je vrácen objekt.

::: moniker range=">= aspnetcore-2.1"
## <a name="actionresultt-type"></a>ActionResult\<T > typ

ASP.NET Core 2.1 přináší [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) návratový typ pro akce kontroleru webového rozhraní API. Umožňuje návratový typ odvozený od [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) nebo se vraťte [konkrétní typ](#specific-type). `ActionResult<T>` nabízí následující výhody oproti [IActionResult typ](#iactionresult-type):

* [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) atributu `Type` vlastnost je možné vyloučit z replikace.
* [Implicitní přetypování operátory](/dotnet/csharp/language-reference/keywords/implicit) podporu převodu obou `T` a `ActionResult` k `ActionResult<T>`. `T` převede na [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), což znamená, že `return new ObjectResult(T);` je zjednodušenou `return T;`.

Většinu akcí mít návratový typ konkrétní. Neočekávané podmínky může dojít během provádění akce, ve kterém případ konkrétní typ nevrátí. Vstupní parametr akce může například selhat ověření modelu. V takovém případě je běžné vrátí odpovídající `ActionResult` typ místo konkrétního typu.

### <a name="synchronous-action"></a>Synchronní akce

Vezměte v úvahu synchronní akce, ve kterém jsou dvě možné návratové typy:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

V předchozím kódu je vrátil stavový kód 404 při produktu v databázi neexistuje. Pokud produkt neexistuje, odpovídající `Product` je vrácen objekt. Před ASP.NET Core 2.1 `return product;` řádku by byl `return Ok(product);`.

> [!TIP]
> K ASP.NET Core 2.1 je povolená odvození zdroj vazby parametrů akce, když je doplněn třídu kontroleru `[ApiController]` atribut. Název parametru názvu v šabloně trasy automaticky svázán pomocí žádosti o data trasy. V důsledku toho předchozí akce `id` parametr není explicitně opatřen poznámkou [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) atribut.

### <a name="asynchronous-action"></a>Asynchronní akce

Vezměte v úvahu asynchronní akce, ve kterém jsou dvě možné návratové typy:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

Pokud selže ověření modelu [chybného požadavku](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) vyvolána metoda vrátit stavový kód 400. [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) vlastnost obsahující chyby ověřování podle je předán do něj. V případě úspěšného ověření modelu se vytvoří produktu v databázi. Se vrátí 201 stavový kód.

> [!TIP]
> K ASP.NET Core 2.1 je povolená odvození zdroj vazby parametrů akce, když je doplněn třídu kontroleru `[ApiController]` atribut. Komplexní typ parametry jsou automaticky svázán pomocí textu požadavku. V důsledku toho předchozí akce `product` parametr není explicitně opatřen poznámkou [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atribut.
::: moniker-end

## <a name="additional-resources"></a>Další zdroje

* [Akce kontroleru](xref:mvc/controllers/actions)
* [Ověření modelu](xref:mvc/models/validation)
* [Stránek nápovědy webového rozhraní API pomocí Swaggeru](xref:tutorials/web-api-help-pages-using-swagger)
