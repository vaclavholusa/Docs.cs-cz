---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core
author: scottaddie
description: Další informace o funkcích, které jsou k dispozici pro vytváření webových rozhraní API v ASP.NET Core a kdy je vhodné používat jednotlivé funkce.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/04/2018
ms.locfileid: "36274963"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="73699-103">Vytvoření webového rozhraní API pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73699-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="73699-104">Podle [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="73699-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="73699-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="73699-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="73699-106">Tento dokument popisuje, jak vytvořit webové rozhraní API v ASP.NET Core a je nejvhodnější používat jednotlivé funkce.</span><span class="sxs-lookup"><span data-stu-id="73699-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="73699-107">Odvodit třídu z ControllerBase</span><span class="sxs-lookup"><span data-stu-id="73699-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="73699-108">Dědí [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) třídy v kontroleru, který má sloužit jako webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="73699-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="73699-109">Příklad:</span><span class="sxs-lookup"><span data-stu-id="73699-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

<span data-ttu-id="73699-110">`ControllerBase` Třídě poskytuje přístup k mnoha vlastností a metod.</span><span class="sxs-lookup"><span data-stu-id="73699-110">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="73699-111">V předchozím příkladu, některé tyto metody zahrnují [chybného požadavku](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) a [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="73699-111">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="73699-112">Tyto metody jsou vyvolány v rámci metod akce vrátit kód HTTP 400 a 201 stavové kódy, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="73699-112">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="73699-113">[ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) vlastnost také poskytuje `ControllerBase`, přistupuje k provedení žádosti o ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="73699-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="73699-114">Třída s atributem ApiControllerAttribute opatřit poznámkami</span><span class="sxs-lookup"><span data-stu-id="73699-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="73699-115">ASP.NET Core 2.1 přináší [[objektu ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atribut k označení webového rozhraní API třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="73699-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="73699-116">Příklad:</span><span class="sxs-lookup"><span data-stu-id="73699-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="73699-117">Tento atribut je obvykle párována s `ControllerBase` k získání přístupu k užitečných metod a vlastností.</span><span class="sxs-lookup"><span data-stu-id="73699-117">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="73699-118">`ControllerBase` poskytuje přístup k metodám například [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) a [souboru](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="73699-118">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="73699-119">Další možností je vytvořit třídu vlastní základní kontroler opatřen poznámkou `[ApiController]` atribut:</span><span class="sxs-lookup"><span data-stu-id="73699-119">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="73699-120">Následující části popisují užitečných funkcí, které jsou přidány pomocí atributu.</span><span class="sxs-lookup"><span data-stu-id="73699-120">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="73699-121">Automatické odpovědi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="73699-121">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="73699-122">Chyby ověření automaticky aktivuje odpověď HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="73699-122">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="73699-123">Následující kód bude nutná u akcí:</span><span class="sxs-lookup"><span data-stu-id="73699-123">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="73699-124">Toto výchozí chování je zakázaná pomocí následujícího kódu v *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="73699-124">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="73699-125">Odvození parametr zdroje vazby</span><span class="sxs-lookup"><span data-stu-id="73699-125">Binding source parameter inference</span></span>

<span data-ttu-id="73699-126">Zdrojový atribut vazby definuje umístění, ve kterém není nalezena hodnota parametru akce.</span><span class="sxs-lookup"><span data-stu-id="73699-126">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="73699-127">Existují následující atributy zdroje vazby:</span><span class="sxs-lookup"><span data-stu-id="73699-127">The following binding source attributes exist:</span></span>

|<span data-ttu-id="73699-128">Atribut</span><span class="sxs-lookup"><span data-stu-id="73699-128">Attribute</span></span>|<span data-ttu-id="73699-129">Zdroje připojení</span><span class="sxs-lookup"><span data-stu-id="73699-129">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="73699-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="73699-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="73699-131">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="73699-131">Request body</span></span> |
|<span data-ttu-id="73699-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="73699-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="73699-133">Data formuláře v textu požadavku</span><span class="sxs-lookup"><span data-stu-id="73699-133">Form data in the request body</span></span> |
|<span data-ttu-id="73699-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="73699-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="73699-135">Hlavička požadavku</span><span class="sxs-lookup"><span data-stu-id="73699-135">Request header</span></span> |
|<span data-ttu-id="73699-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="73699-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="73699-137">Požádat o parametru řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="73699-137">Request query string parameter</span></span> |
|<span data-ttu-id="73699-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="73699-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="73699-139">Data trasy z aktuální žádosti</span><span class="sxs-lookup"><span data-stu-id="73699-139">Route data from the current request</span></span> |
|<span data-ttu-id="73699-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="73699-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="73699-141">Žádost o služby, vložený jako parametru akce</span><span class="sxs-lookup"><span data-stu-id="73699-141">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="73699-142">Proveďte **není** použít `[FromRoute]` při může obsahovat hodnoty `%2f` (to znamená `/`) vzhledem k tomu, `%2f` nebude znaků bez řídících k `/`.</span><span class="sxs-lookup"><span data-stu-id="73699-142">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="73699-143">Použití `[FromQuery]` Pokud hodnota může obsahovat `%2f`.</span><span class="sxs-lookup"><span data-stu-id="73699-143">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="73699-144">Bez `[ApiController]` atribut, vytvoření vazby zdroje nejsou explicitně definovány atributy.</span><span class="sxs-lookup"><span data-stu-id="73699-144">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="73699-145">V následujícím příkladu `[FromQuery]` atribut označuje, že `discontinuedOnly` je zadána hodnota parametru v řetězci dotazu v adrese URL požadavku:</span><span class="sxs-lookup"><span data-stu-id="73699-145">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="73699-146">Odvozená pravidla se použijí pro zdroje dat výchozí parametry akce.</span><span class="sxs-lookup"><span data-stu-id="73699-146">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="73699-147">Tato pravidla konfigurace zdroje připojení, v opačném případě budete pravděpodobně pro ruční použití na parametry akce.</span><span class="sxs-lookup"><span data-stu-id="73699-147">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="73699-148">Atributy zdroje vazby chovají následovně:</span><span class="sxs-lookup"><span data-stu-id="73699-148">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="73699-149">**[FromBody]**  odvodit pro komplexní typ parametrů.</span><span class="sxs-lookup"><span data-stu-id="73699-149">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="73699-150">Výjimkou z tohoto pravidla je libovolný integrované, komplexní typ zvláštní význam, jako například [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) a [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="73699-150">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="73699-151">Zdrojový kód odvození vazby ignoruje tyto speciální typy.</span><span class="sxs-lookup"><span data-stu-id="73699-151">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="73699-152">Pokud má akci explicitně zadán více než jeden parametr (prostřednictvím `[FromBody]`) nebo odvozený jako vázaný z textu požadavku, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="73699-152">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="73699-153">Například následující akce podpisy způsobit výjimku:</span><span class="sxs-lookup"><span data-stu-id="73699-153">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="73699-154">**[FromForm]**  odvodit pro parametry akce typu [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) a [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="73699-154">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="73699-155">Není odvodit pro jednoduché nebo uživatelem definované typy.</span><span class="sxs-lookup"><span data-stu-id="73699-155">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="73699-156">**[FromRoute]**  odvodit pro název parametru žádné akce odpovídající parametr v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="73699-156">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="73699-157">Pokud několik tras shodovat s parametrem akce, je považován za libovolnou hodnotu trasy `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="73699-157">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="73699-158">**[FromQuery]**  odvodit pro všechny ostatní parametry akce.</span><span class="sxs-lookup"><span data-stu-id="73699-158">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="73699-159">Výchozí pravidla pro odvození jsou zakázaná pomocí následujícího kódu v *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="73699-159">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="73699-160">Odvození multipart/formulář data požadavku</span><span class="sxs-lookup"><span data-stu-id="73699-160">Multipart/form-data request inference</span></span>

<span data-ttu-id="73699-161">Když parametr akce je opatřen poznámkou [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) atribut, `multipart/form-data` žádosti je odvozený typ obsahu.</span><span class="sxs-lookup"><span data-stu-id="73699-161">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="73699-162">Výchozí chování je zakázaná pomocí následujícího kódu v *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="73699-162">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="73699-163">Atribut směrování požadavků</span><span class="sxs-lookup"><span data-stu-id="73699-163">Attribute routing requirement</span></span>

<span data-ttu-id="73699-164">Směrování atributů se změní na požadavek.</span><span class="sxs-lookup"><span data-stu-id="73699-164">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="73699-165">Příklad:</span><span class="sxs-lookup"><span data-stu-id="73699-165">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="73699-166">Akce jsou nedostupné přes [trasy konvenční](xref:mvc/controllers/routing#conventional-routing) definované v [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) nebo [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) v *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="73699-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="73699-167">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="73699-167">Additional resources</span></span>

* [<span data-ttu-id="73699-168">Návratové typy akcí kontroleru</span><span class="sxs-lookup"><span data-stu-id="73699-168">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="73699-169">Vlastní formátovací moduly</span><span class="sxs-lookup"><span data-stu-id="73699-169">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="73699-170">Formátování dat odpovědi</span><span class="sxs-lookup"><span data-stu-id="73699-170">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="73699-171">Stránky nápovědy používající Swagger</span><span class="sxs-lookup"><span data-stu-id="73699-171">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="73699-172">Směrování na akce kontroleru</span><span class="sxs-lookup"><span data-stu-id="73699-172">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
