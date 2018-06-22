---
title: Návratové typy akce kontroleru v webového rozhraní API ASP.NET Core
author: scottaddie
description: Další informace o použití různých řadiče akce metoda návratové typy v rozhraní ASP.NET Core Web API.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/21/2018
uid: web-api/action-return-types
ms.openlocfilehash: 422db97da222fb5e742e1d8e6ae410edc90dbc18
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273553"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>Návratové typy akce kontroleru v webového rozhraní API ASP.NET Core

Podle [Scott Addie](https://github.com/scottaddie)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

ASP.NET Core nabízí že následující možnosti pro akce kontroleru webového rozhraní API návratové typy:

::: moniker range="<= aspnetcore-2.0"
* [Specifický typ](#specific-type)
* [IActionResult](#iactionresult-type)
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
* [Specifický typ](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T >](#actionresultt-type)
::: moniker-end

Tento dokument popisuje, když je nejvhodnější používat každý návratový typ.

## <a name="specific-type"></a>Specifický typ

Nejjednodušší akce vrátí primitivních nebo složitých datový typ (například `string` nebo typ vlastního objektu). Vezměte v úvahu následující akce, která vrátí kolekci vlastní `Product` objekty:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Vrácení konkrétního typu může stačit bez známých podmínky k ochraně proti během provádění akce. Předchozí akce přijme žádné parametry, tak parametr omezení ověření není potřeba.

Když známé podmínky vyžadují zvláštní pozornost pro v akci, jsou zavedené návratový více cest. V takovém případě je běžné kombinovat [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) návratový typ s návratovým typem primitivních nebo složitých. Buď [IActionResult](#iactionresult-type) nebo [ActionResult\<T >](#actionresultt-type) jsou nezbytné pro přizpůsobení tento typ akce.

## <a name="iactionresult-type"></a>Typ IActionResult

[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) návratový typ je vhodné, když více [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) vrátit typy jsou možné v akci. `ActionResult` Typy představují různé stavové kódy HTTP. Jsou některé běžné návratové typy spadající do této kategorie [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), a [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).

Vzhledem k tomu, že existuje více návratové typy a použití cesty v akci, Volná [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) atribut je nezbytné. Tento atribut vytváří popisnější odpovědi podrobnosti stránky nápovědy rozhraní API generovaných nástroje, například [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger). `[ProducesResponseType]` označuje známé typy a stavové kódy HTTP, který má být vrácen akce.

### <a name="synchronous-action"></a>Synchronní akce

Vezměte v úvahu následující synchronní akce, ve kterém jsou dvě možné návratové typy:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

V předchozí akce, je vrácena 404 stavový kód, pokud produkt reprezentována `id` v příslušné úložiště dat neexistuje. [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) metodu helper je vyvolána jako zástupce `return new NotFoundResult();`. Pokud produkt neexistuje, `Product` objekt představující datové části je vrácen s 200 stavový kód. [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) metodu helper je vyvolána jako sdružená formu `return new OkObjectResult(product);`.

### <a name="asynchronous-action"></a>Asynchronní akce

Vezměte v úvahu následující asynchronní akce, ve kterém jsou dvě možné návratové typy:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

V předchozí akce, je vrácena 400 stavový kód, pokud selže ověření modelu a [struktura BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) Pomocná metoda je volána. Například následující modelu označuje, že musí poskytnout požadavky `Name` vlastnosti a hodnotu. Proto nejsou-li správnou `Name` v požadavku způsobí selhání ověření modelu.

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

Předchozí akce jiných známým návratový kód je 201, což je generován [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) metodu helper. V této cestě `Product` se vrátí objekt.

::: moniker range=">= aspnetcore-2.1"
## <a name="actionresultt-type"></a>ActionResult\<T > typ

Zavádí ASP.NET Core 2.1 [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) návratový typ pro akce kontroleru webového rozhraní API. Umožní vám vrátit typ odvozování z [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) , nebo může vracet [konkrétního typu](#specific-type). `ActionResult<T>` nabízí následující výhody přes [IActionResult typ](#iactionresult-type):

* [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) atributu `Type` vlastnost můžete vyloučit.
* [Operátory přetypování implicitní](/dotnet/csharp/language-reference/keywords/implicit) podporují převod obou `T` a `ActionResult` k `ActionResult<T>`. `T` převede na [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), což znamená `return new ObjectResult(T);` je zjednodušenou `return T;`.

Většinu akcí mít konkrétní návratový typ. Neočekávané podmínek může dojít během provádění akce, ve kterém případ specifický typ nevrátí. Vstupní parametr akce může například nezdaří ověření modelu. V takovém případě je běžné vrátit odpovídající `ActionResult` typ místo konkrétního typu.

### <a name="synchronous-action"></a>Synchronní akce

Vezměte v úvahu synchronní akce, ve kterém jsou dvě možné návratové typy:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

Předchozí kód 404 stavový kód je vrácena produktu v databázi neexistuje. Pokud produkt neexistuje, odpovídající `Product` se vrátí objekt. Před ASP.NET Core 2.1 `return product;` řádku by byl `return Ok(product);`.

> [!TIP]
> Od verze 2.1 jádro ASP.NET, je při třídy kontroleru je upraven pomocí povolená akce parametr vazby zdroje odvození `[ApiController]` atribut. Název parametru odpovídající názvu v šabloně trasy, je automaticky vytvořena pomocí data trasy na žádost. V důsledku toho předchozí akce `id` parametr není opatřen poznámkou explicitně [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) atribut.

### <a name="asynchronous-action"></a>Asynchronní akce

Vezměte v úvahu asynchronní akce, ve kterém jsou dvě možné návratové typy:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

Pokud selže ověření modelu [struktura BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) metoda je volána vrátit 400 stavový kód. [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) je předán vlastnost obsahující chyby konkrétní ověření. Pokud model ověření úspěšné, vytvoří se v databázi produktu. Vrácen 201 stavový kód.

> [!TIP]
> Od verze 2.1 jádro ASP.NET, je při třídy kontroleru je upraven pomocí povolená akce parametr vazby zdroje odvození `[ApiController]` atribut. Komplexní typ parametry jsou automaticky svázán pomocí textu požadavku. V důsledku toho předchozí akce `product` parametr není opatřen poznámkou explicitně [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atribut.
::: moniker-end

## <a name="additional-resources"></a>Další zdroje

* [Akce kontroleru](xref:mvc/controllers/actions)
* [Ověření modelu](xref:mvc/models/validation)
* [Pomocí stránky nápovědy webové rozhraní API Swaggeru](xref:tutorials/web-api-help-pages-using-swagger)
