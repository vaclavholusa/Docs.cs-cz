---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core
author: scottaddie
description: Další informace o funkcích, které jsou k dispozici pro vytváření webových rozhraní API v ASP.NET Core a kdy je vhodné používat jednotlivé funkce.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: d410f28ff7fda3bf33f73c06b3e626dfd4ee7dd8
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/22/2018
ms.locfileid: "41822137"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="45b06-103">Vytvoření webového rozhraní API pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="45b06-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="45b06-104">Podle [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="45b06-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="45b06-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="45b06-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="45b06-106">Tento dokument popisuje, jak vytvořit webové rozhraní API v ASP.NET Core a je nejvhodnější používat jednotlivé funkce.</span><span class="sxs-lookup"><span data-stu-id="45b06-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="45b06-107">Odvodit třídu z ControllerBase</span><span class="sxs-lookup"><span data-stu-id="45b06-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="45b06-108">Dědí <xref:Microsoft.AspNetCore.Mvc.ControllerBase> třídy v kontroleru, který má sloužit jako webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="45b06-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="45b06-109">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45b06-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="45b06-110">`ControllerBase` Třídě poskytuje přístup k několika vlastností a metod.</span><span class="sxs-lookup"><span data-stu-id="45b06-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="45b06-111">V předchozím kódu mezi příklady patří <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> a <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="45b06-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="45b06-112">Tyto metody jsou volány v rámci metod akce vrátit kód HTTP 400 a 201 stavové kódy, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="45b06-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="45b06-113"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> Vlastnost také poskytuje `ControllerBase`, přistupuje ke zpracování žádosti o ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="45b06-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="45b06-114">Třída s atributem ApiControllerAttribute opatřit poznámkami</span><span class="sxs-lookup"><span data-stu-id="45b06-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="45b06-115">ASP.NET Core 2.1 přináší [[objektu ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atribut k označení webového rozhraní API třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="45b06-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="45b06-116">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45b06-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="45b06-117">Kompatibilita verze 2.1 nebo novější, nastavené přes <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, je potřeba použít tento atribut.</span><span class="sxs-lookup"><span data-stu-id="45b06-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="45b06-118">Například zvýrazněný kód do *Startup.ConfigureServices* nastaví příznak 2.1 kompatibility:</span><span class="sxs-lookup"><span data-stu-id="45b06-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="45b06-119">Další informace naleznete v tématu <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="45b06-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="45b06-120">`[ApiController]` Atribut běžně doplňuje `ControllerBase` povolit chování specifické pro REST pro řadiče.</span><span class="sxs-lookup"><span data-stu-id="45b06-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="45b06-121">`ControllerBase` poskytuje přístup k metodám například <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> a <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="45b06-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="45b06-122">Další možností je vytvořit třídu vlastní základní kontroler opatřen poznámkou `[ApiController]` atribut:</span><span class="sxs-lookup"><span data-stu-id="45b06-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="45b06-123">Následující části popisují užitečných funkcí, které jsou přidány pomocí atributu.</span><span class="sxs-lookup"><span data-stu-id="45b06-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="45b06-124">Automatické odpovědi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="45b06-124">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="45b06-125">Chyby ověření automaticky aktivuje odpověď HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="45b06-125">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="45b06-126">Následující kód bude nutná u akcí:</span><span class="sxs-lookup"><span data-stu-id="45b06-126">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="45b06-127">Výchozí chování je zakázaná. Pokud <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> je nastavena na `true`.</span><span class="sxs-lookup"><span data-stu-id="45b06-127">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="45b06-128">Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="45b06-128">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="45b06-129">Odvození parametr zdroje vazby</span><span class="sxs-lookup"><span data-stu-id="45b06-129">Binding source parameter inference</span></span>

<span data-ttu-id="45b06-130">Zdrojový atribut vazby definuje umístění, ve kterém není nalezena hodnota parametru akce.</span><span class="sxs-lookup"><span data-stu-id="45b06-130">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="45b06-131">Existují následující atributy zdroje vazby:</span><span class="sxs-lookup"><span data-stu-id="45b06-131">The following binding source attributes exist:</span></span>

|<span data-ttu-id="45b06-132">Atribut</span><span class="sxs-lookup"><span data-stu-id="45b06-132">Attribute</span></span>|<span data-ttu-id="45b06-133">Zdroje připojení</span><span class="sxs-lookup"><span data-stu-id="45b06-133">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="45b06-134">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="45b06-134">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="45b06-135">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="45b06-135">Request body</span></span> |
|<span data-ttu-id="45b06-136">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="45b06-136">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="45b06-137">Data formuláře v textu požadavku</span><span class="sxs-lookup"><span data-stu-id="45b06-137">Form data in the request body</span></span> |
|<span data-ttu-id="45b06-138">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="45b06-138">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="45b06-139">Hlavička požadavku</span><span class="sxs-lookup"><span data-stu-id="45b06-139">Request header</span></span> |
|<span data-ttu-id="45b06-140">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="45b06-140">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="45b06-141">Požádat o parametru řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="45b06-141">Request query string parameter</span></span> |
|<span data-ttu-id="45b06-142">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="45b06-142">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="45b06-143">Data trasy z aktuální žádosti</span><span class="sxs-lookup"><span data-stu-id="45b06-143">Route data from the current request</span></span> |
|<span data-ttu-id="45b06-144">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="45b06-144">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="45b06-145">Žádost o služby, vložený jako parametru akce</span><span class="sxs-lookup"><span data-stu-id="45b06-145">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="45b06-146">Nepoužívejte `[FromRoute]` při může obsahovat hodnoty `%2f` (to znamená `/`).</span><span class="sxs-lookup"><span data-stu-id="45b06-146">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="45b06-147">`%2f` nebude znaků bez řídících k `/`.</span><span class="sxs-lookup"><span data-stu-id="45b06-147">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="45b06-148">Použití `[FromQuery]` Pokud hodnota může obsahovat `%2f`.</span><span class="sxs-lookup"><span data-stu-id="45b06-148">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="45b06-149">Bez `[ApiController]` atribut, vytvoření vazby zdroje nejsou explicitně definovány atributy.</span><span class="sxs-lookup"><span data-stu-id="45b06-149">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="45b06-150">V následujícím příkladu `[FromQuery]` atribut označuje, že `discontinuedOnly` je zadána hodnota parametru v řetězci dotazu v adrese URL požadavku:</span><span class="sxs-lookup"><span data-stu-id="45b06-150">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="45b06-151">Odvozená pravidla se použijí pro zdroje dat výchozí parametry akce.</span><span class="sxs-lookup"><span data-stu-id="45b06-151">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="45b06-152">Tato pravidla konfigurace zdroje připojení, v opačném případě budete pravděpodobně pro ruční použití na parametry akce.</span><span class="sxs-lookup"><span data-stu-id="45b06-152">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="45b06-153">Atributy zdroje vazby chovají následovně:</span><span class="sxs-lookup"><span data-stu-id="45b06-153">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="45b06-154">**[FromBody]**  odvodit pro komplexní typ parametrů.</span><span class="sxs-lookup"><span data-stu-id="45b06-154">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="45b06-155">Výjimkou z tohoto pravidla je libovolný integrované, komplexní typ zvláštní význam, jako například <xref:Microsoft.AspNetCore.Http.IFormCollection> a <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="45b06-155">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="45b06-156">Zdrojový kód odvození vazby ignoruje tyto speciální typy.</span><span class="sxs-lookup"><span data-stu-id="45b06-156">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="45b06-157">`[FromBody]` není odvodit pro jednoduché typy, jako například `string` nebo `int`.</span><span class="sxs-lookup"><span data-stu-id="45b06-157">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="45b06-158">Proto `[FromBody]` atribut by měl použít pro jednoduché typy, které tuto funkci potřebujete.</span><span class="sxs-lookup"><span data-stu-id="45b06-158">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="45b06-159">Pokud má akci explicitně zadán více než jeden parametr (prostřednictvím `[FromBody]`) nebo odvozený jako vázaný z textu požadavku, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="45b06-159">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="45b06-160">Například následující akce podpisy způsobit výjimku:</span><span class="sxs-lookup"><span data-stu-id="45b06-160">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="45b06-161">**[FromForm]**  odvodit pro parametry akce typu <xref:Microsoft.AspNetCore.Http.IFormFile> a <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="45b06-161">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="45b06-162">Není odvodit pro jednoduché nebo uživatelem definované typy.</span><span class="sxs-lookup"><span data-stu-id="45b06-162">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="45b06-163">**[FromRoute]**  odvodit pro název parametru žádné akce odpovídající parametr v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="45b06-163">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="45b06-164">Když víc tras odpovídá parametru akce, je považován za libovolnou hodnotu trasy `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="45b06-164">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="45b06-165">**[FromQuery]**  odvodit pro všechny ostatní parametry akce.</span><span class="sxs-lookup"><span data-stu-id="45b06-165">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="45b06-166">Výchozí pravidla pro odvození jsou zakázané. Pokud <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> je nastavena na `true`.</span><span class="sxs-lookup"><span data-stu-id="45b06-166">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="45b06-167">Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="45b06-167">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="45b06-168">Odvození multipart/formulář data požadavku</span><span class="sxs-lookup"><span data-stu-id="45b06-168">Multipart/form-data request inference</span></span>

<span data-ttu-id="45b06-169">Když parametr akce je opatřen poznámkou [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) atribut, `multipart/form-data` žádosti je odvozený typ obsahu.</span><span class="sxs-lookup"><span data-stu-id="45b06-169">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="45b06-170">Výchozí chování je zakázaná. Pokud <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> je nastavena na `true`.</span><span class="sxs-lookup"><span data-stu-id="45b06-170">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="45b06-171">Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="45b06-171">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="45b06-172">Atribut směrování požadavků</span><span class="sxs-lookup"><span data-stu-id="45b06-172">Attribute routing requirement</span></span>

<span data-ttu-id="45b06-173">Směrování atributů se změní na požadavek.</span><span class="sxs-lookup"><span data-stu-id="45b06-173">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="45b06-174">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45b06-174">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="45b06-175">Akce jsou nedostupné přes [trasy konvenční](xref:mvc/controllers/routing#conventional-routing) definované v <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> nebo <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> v *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="45b06-175">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="45b06-176">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="45b06-176">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
