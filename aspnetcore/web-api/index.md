---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core
author: scottaddie
description: Další informace o funkcích, které jsou k dispozici pro vytváření webových rozhraní API v ASP.NET Core a kdy je vhodné používat jednotlivé funkce.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: 84e4a51a8a8ab031752ef054cba834bd202a4927
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894212"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="41613-103">Vytvoření webového rozhraní API pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41613-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="41613-104">Podle [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="41613-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="41613-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="41613-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="41613-106">Tento dokument popisuje, jak vytvořit webové rozhraní API v ASP.NET Core a je nejvhodnější používat jednotlivé funkce.</span><span class="sxs-lookup"><span data-stu-id="41613-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="41613-107">Odvodit třídu z ControllerBase</span><span class="sxs-lookup"><span data-stu-id="41613-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="41613-108">Dědí [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) třídy v kontroleru, který má sloužit jako webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="41613-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="41613-109">Příklad:</span><span class="sxs-lookup"><span data-stu-id="41613-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="41613-110">`ControllerBase` Třídě poskytuje přístup k několika vlastností a metod.</span><span class="sxs-lookup"><span data-stu-id="41613-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="41613-111">V předchozím kódu mezi příklady patří [chybného požadavku](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) a [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="41613-111">In the preceding code, examples include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="41613-112">Tyto metody jsou volány v rámci metod akce vrátit kód HTTP 400 a 201 stavové kódy, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="41613-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="41613-113">[ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) vlastnost také poskytuje `ControllerBase`, přistupuje ke zpracování žádosti o ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="41613-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="41613-114">Třída s atributem ApiControllerAttribute opatřit poznámkami</span><span class="sxs-lookup"><span data-stu-id="41613-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="41613-115">ASP.NET Core 2.1 přináší [[objektu ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atribut k označení webového rozhraní API třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="41613-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="41613-116">Příklad:</span><span class="sxs-lookup"><span data-stu-id="41613-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="41613-117">Kompatibilita verze 2.1 nebo novější, nastavené přes [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), je potřeba použít tento atribut.</span><span class="sxs-lookup"><span data-stu-id="41613-117">A compatibility version of 2.1 or later, set via [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), is required to use this attribute.</span></span> <span data-ttu-id="41613-118">Například zvýrazněný kód do *Startup.ConfigureServices* nastaví příznak 2.1 kompatibility:</span><span class="sxs-lookup"><span data-stu-id="41613-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="41613-119">`[ApiController]` Atribut běžně doplňuje `ControllerBase` povolit chování specifické pro REST pro řadiče.</span><span class="sxs-lookup"><span data-stu-id="41613-119">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="41613-120">`ControllerBase` poskytuje přístup k metodám například [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) a [souboru](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="41613-120">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="41613-121">Další možností je vytvořit třídu vlastní základní kontroler opatřen poznámkou `[ApiController]` atribut:</span><span class="sxs-lookup"><span data-stu-id="41613-121">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="41613-122">Následující části popisují užitečných funkcí, které jsou přidány pomocí atributu.</span><span class="sxs-lookup"><span data-stu-id="41613-122">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="41613-123">Automatické odpovědi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="41613-123">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="41613-124">Chyby ověření automaticky aktivuje odpověď HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="41613-124">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="41613-125">Následující kód bude nutná u akcí:</span><span class="sxs-lookup"><span data-stu-id="41613-125">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="41613-126">Výchozí chování je zakázaná. Pokud [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) je nastavena na `true`.</span><span class="sxs-lookup"><span data-stu-id="41613-126">The default behavior is disabled when the [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) property is set to `true`.</span></span> <span data-ttu-id="41613-127">Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="41613-127">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="41613-128">Odvození parametr zdroje vazby</span><span class="sxs-lookup"><span data-stu-id="41613-128">Binding source parameter inference</span></span>

<span data-ttu-id="41613-129">Zdrojový atribut vazby definuje umístění, ve kterém není nalezena hodnota parametru akce.</span><span class="sxs-lookup"><span data-stu-id="41613-129">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="41613-130">Existují následující atributy zdroje vazby:</span><span class="sxs-lookup"><span data-stu-id="41613-130">The following binding source attributes exist:</span></span>

|<span data-ttu-id="41613-131">Atribut</span><span class="sxs-lookup"><span data-stu-id="41613-131">Attribute</span></span>|<span data-ttu-id="41613-132">Zdroje připojení</span><span class="sxs-lookup"><span data-stu-id="41613-132">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="41613-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="41613-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="41613-134">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="41613-134">Request body</span></span> |
|<span data-ttu-id="41613-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="41613-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="41613-136">Data formuláře v textu požadavku</span><span class="sxs-lookup"><span data-stu-id="41613-136">Form data in the request body</span></span> |
|<span data-ttu-id="41613-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="41613-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="41613-138">Hlavička požadavku</span><span class="sxs-lookup"><span data-stu-id="41613-138">Request header</span></span> |
|<span data-ttu-id="41613-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="41613-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="41613-140">Požádat o parametru řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="41613-140">Request query string parameter</span></span> |
|<span data-ttu-id="41613-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="41613-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="41613-142">Data trasy z aktuální žádosti</span><span class="sxs-lookup"><span data-stu-id="41613-142">Route data from the current request</span></span> |
|<span data-ttu-id="41613-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="41613-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="41613-144">Žádost o služby, vložený jako parametru akce</span><span class="sxs-lookup"><span data-stu-id="41613-144">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="41613-145">Nepoužívejte `[FromRoute]` při může obsahovat hodnoty `%2f` (to znamená `/`).</span><span class="sxs-lookup"><span data-stu-id="41613-145">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="41613-146">`%2f` nebude znaků bez řídících k `/`.</span><span class="sxs-lookup"><span data-stu-id="41613-146">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="41613-147">Použití `[FromQuery]` Pokud hodnota může obsahovat `%2f`.</span><span class="sxs-lookup"><span data-stu-id="41613-147">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="41613-148">Bez `[ApiController]` atribut, vytvoření vazby zdroje nejsou explicitně definovány atributy.</span><span class="sxs-lookup"><span data-stu-id="41613-148">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="41613-149">V následujícím příkladu `[FromQuery]` atribut označuje, že `discontinuedOnly` je zadána hodnota parametru v řetězci dotazu v adrese URL požadavku:</span><span class="sxs-lookup"><span data-stu-id="41613-149">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="41613-150">Odvozená pravidla se použijí pro zdroje dat výchozí parametry akce.</span><span class="sxs-lookup"><span data-stu-id="41613-150">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="41613-151">Tato pravidla konfigurace zdroje připojení, v opačném případě budete pravděpodobně pro ruční použití na parametry akce.</span><span class="sxs-lookup"><span data-stu-id="41613-151">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="41613-152">Atributy zdroje vazby chovají následovně:</span><span class="sxs-lookup"><span data-stu-id="41613-152">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="41613-153">**[FromBody]**  odvodit pro komplexní typ parametrů.</span><span class="sxs-lookup"><span data-stu-id="41613-153">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="41613-154">Výjimkou z tohoto pravidla je libovolný integrované, komplexní typ zvláštní význam, jako například [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) a [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="41613-154">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="41613-155">Zdrojový kód odvození vazby ignoruje tyto speciální typy.</span><span class="sxs-lookup"><span data-stu-id="41613-155">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="41613-156">Pokud má akci explicitně zadán více než jeden parametr (prostřednictvím `[FromBody]`) nebo odvozený jako vázaný z textu požadavku, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="41613-156">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="41613-157">Například následující akce podpisy způsobit výjimku:</span><span class="sxs-lookup"><span data-stu-id="41613-157">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="41613-158">**[FromForm]**  odvodit pro parametry akce typu [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) a [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="41613-158">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="41613-159">Není odvodit pro jednoduché nebo uživatelem definované typy.</span><span class="sxs-lookup"><span data-stu-id="41613-159">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="41613-160">**[FromRoute]**  odvodit pro název parametru žádné akce odpovídající parametr v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="41613-160">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="41613-161">Když víc tras odpovídá parametru akce, je považován za libovolnou hodnotu trasy `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="41613-161">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="41613-162">**[FromQuery]**  odvodit pro všechny ostatní parametry akce.</span><span class="sxs-lookup"><span data-stu-id="41613-162">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="41613-163">Výchozí pravidla pro odvození jsou zakázané. Pokud [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) je nastavena na `true`.</span><span class="sxs-lookup"><span data-stu-id="41613-163">The default inference rules are disabled when the [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) property is set to `true`.</span></span> <span data-ttu-id="41613-164">Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="41613-164">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="41613-165">Odvození multipart/formulář data požadavku</span><span class="sxs-lookup"><span data-stu-id="41613-165">Multipart/form-data request inference</span></span>

<span data-ttu-id="41613-166">Když parametr akce je opatřen poznámkou [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) atribut, `multipart/form-data` žádosti je odvozený typ obsahu.</span><span class="sxs-lookup"><span data-stu-id="41613-166">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="41613-167">Výchozí chování je zakázaná. Pokud [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) je nastavena na `true`.</span><span class="sxs-lookup"><span data-stu-id="41613-167">The default behavior is disabled when the [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) property is set to `true`.</span></span> <span data-ttu-id="41613-168">Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="41613-168">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="41613-169">Atribut směrování požadavků</span><span class="sxs-lookup"><span data-stu-id="41613-169">Attribute routing requirement</span></span>

<span data-ttu-id="41613-170">Směrování atributů se změní na požadavek.</span><span class="sxs-lookup"><span data-stu-id="41613-170">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="41613-171">Příklad:</span><span class="sxs-lookup"><span data-stu-id="41613-171">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="41613-172">Akce jsou nedostupné přes [trasy konvenční](xref:mvc/controllers/routing#conventional-routing) definované v [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) nebo [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) v *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="41613-172">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="41613-173">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="41613-173">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
