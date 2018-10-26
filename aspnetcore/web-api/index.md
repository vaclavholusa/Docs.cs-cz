---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core
author: scottaddie
description: Další informace o funkcích, které jsou k dispozici pro vytváření webových rozhraní API v ASP.NET Core a kdy je vhodné používat jednotlivé funkce.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: 763b95fb8ed3806bc67b7ad199153ea1027efa57
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090417"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="542d1-103">Vytvoření webového rozhraní API pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="542d1-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="542d1-104">Podle [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="542d1-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="542d1-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="542d1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="542d1-106">Tento dokument popisuje, jak vytvořit webové rozhraní API v ASP.NET Core a je nejvhodnější používat jednotlivé funkce.</span><span class="sxs-lookup"><span data-stu-id="542d1-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="542d1-107">Odvodit třídu z ControllerBase</span><span class="sxs-lookup"><span data-stu-id="542d1-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="542d1-108">Dědí <xref:Microsoft.AspNetCore.Mvc.ControllerBase> třídy v kontroleru, který má sloužit jako webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="542d1-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="542d1-109">Příklad:</span><span class="sxs-lookup"><span data-stu-id="542d1-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="542d1-110">`ControllerBase` Třídě poskytuje přístup k několika vlastností a metod.</span><span class="sxs-lookup"><span data-stu-id="542d1-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="542d1-111">V předchozím kódu mezi příklady patří <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> a <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="542d1-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="542d1-112">Tyto metody jsou volány v rámci metod akce vrátit kód HTTP 400 a 201 stavové kódy, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="542d1-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="542d1-113"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> Vlastnost také poskytuje `ControllerBase`, přistupuje ke zpracování žádosti o ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="542d1-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="542d1-114">Třída s atributem ApiControllerAttribute opatřit poznámkami</span><span class="sxs-lookup"><span data-stu-id="542d1-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="542d1-115">ASP.NET Core 2.1 přináší [[objektu ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atribut k označení webového rozhraní API třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="542d1-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="542d1-116">Příklad:</span><span class="sxs-lookup"><span data-stu-id="542d1-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="542d1-117">Kompatibilita verze 2.1 nebo novější, nastavené přes <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, je potřeba použít tento atribut.</span><span class="sxs-lookup"><span data-stu-id="542d1-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="542d1-118">Například zvýrazněný kód do *Startup.ConfigureServices* nastaví příznak 2.2 kompatibility:</span><span class="sxs-lookup"><span data-stu-id="542d1-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.2 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="542d1-119">Další informace naleznete v tématu <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="542d1-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="542d1-120">`[ApiController]` Atribut běžně doplňuje `ControllerBase` povolit chování specifické pro REST pro řadiče.</span><span class="sxs-lookup"><span data-stu-id="542d1-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="542d1-121">`ControllerBase` poskytuje přístup k metodám například <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> a <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="542d1-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="542d1-122">Další možností je vytvořit třídu vlastní základní kontroler opatřen poznámkou `[ApiController]` atribut:</span><span class="sxs-lookup"><span data-stu-id="542d1-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="542d1-123">Následující části popisují užitečných funkcí, které jsou přidány pomocí atributu.</span><span class="sxs-lookup"><span data-stu-id="542d1-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="542d1-124">Problém podrobnosti odpovědi pro stavové kódy chyb</span><span class="sxs-lookup"><span data-stu-id="542d1-124">Problem details responses for error status codes</span></span>

<span data-ttu-id="542d1-125">ASP.NET Core 2.1 nebo novější obsahuje [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), na základě typu [specifikaci RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="542d1-125">ASP.NET Core 2.1 and later includes [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), a type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="542d1-126">`ProblemDetails` Typ poskytuje standardizovaný formát pro předávání počítač čitelné podrobnosti o chybách v odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="542d1-126">The `ProblemDetails` type provides a standardized format for conveying machine readable details of errors in a HTTP response.</span></span>

<span data-ttu-id="542d1-127">V ASP.NET Core 2.2 a novější, MVC transformuje výsledky kódu stavu (stavový kód 400 a vyšší) chyby do výsledku s `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="542d1-127">In ASP.NET Core 2.2 and later, MVC transforms error status code results (status code 400 and higher) to a result with `ProblemDetails`.</span></span> <span data-ttu-id="542d1-128">Vezměte v úvahu následující kód:</span><span class="sxs-lookup"><span data-stu-id="542d1-128">Consider the following code:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

<span data-ttu-id="542d1-129">Odpověď HTTP pro `NotFound` výsledek má stavový kód 404 s `ProblemDetails` podobný následujícímu textu:</span><span class="sxs-lookup"><span data-stu-id="542d1-129">The HTTP response for the `NotFound` result has a 404 status code with a `ProblemDetails` body similar to the following:</span></span>

```js
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="542d1-130">Podrobnosti o problému vyžaduje příznak kompatibility 2.2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="542d1-130">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="542d1-131">Výchozí chování je zakázaná. Pokud [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> je nastavena na `true`.</span><span class="sxs-lookup"><span data-stu-id="542d1-131">The default behavior is disabled when the [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> property is set to `true`.</span></span> <span data-ttu-id="542d1-132">Následující zvýrazněný kód z `Startup.ConfigureServices` zakáže podrobnosti o problému:</span><span class="sxs-lookup"><span data-stu-id="542d1-132">The following highlighted code from `Startup.ConfigureServices` disables problem details:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

<span data-ttu-id="542d1-133">Použití [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> vlastnosti ke konfiguraci obsah `ProblemDetails` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="542d1-133">Use the [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="542d1-134">Například následující kód aktualizace `type` vlastnost obdržíte kód odpovědi 404:</span><span class="sxs-lookup"><span data-stu-id="542d1-134">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a><span data-ttu-id="542d1-135">Automatické odpovědi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="542d1-135">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="542d1-136">Chyby ověření automaticky aktivuje odpověď HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="542d1-136">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="542d1-137">Následující kód bude nutná u akcí:</span><span class="sxs-lookup"><span data-stu-id="542d1-137">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="542d1-138">Použití <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> přizpůsobení výstup výsledné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="542d1-138">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the the resulting response.</span></span>

<span data-ttu-id="542d1-139">Výchozí chování je zakázaná. Pokud <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> je nastavena na `true`.</span><span class="sxs-lookup"><span data-stu-id="542d1-139">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="542d1-140">Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="542d1-140">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

<span data-ttu-id="542d1-141">Pomocí příznaku kompatibility 2.2 nebo novější, je výchozí typ odpovědi vrátí 400 odpovědí <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="542d1-141">With a compatibility flag of 2.2 or later, the default response type returned for 400 responses is a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="542d1-142">Použít [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> vlastnost na používání ASP.NET Core 2.1 Chyba formát.</span><span class="sxs-lookup"><span data-stu-id="542d1-142">Use the Use the [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> property to use the ASP.NET Core 2.1 error format.</span></span>

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="542d1-143">Odvození parametr zdroje vazby</span><span class="sxs-lookup"><span data-stu-id="542d1-143">Binding source parameter inference</span></span>

<span data-ttu-id="542d1-144">Zdrojový atribut vazby definuje umístění, ve kterém není nalezena hodnota parametru akce.</span><span class="sxs-lookup"><span data-stu-id="542d1-144">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="542d1-145">Existují následující atributy zdroje vazby:</span><span class="sxs-lookup"><span data-stu-id="542d1-145">The following binding source attributes exist:</span></span>

|<span data-ttu-id="542d1-146">Atribut</span><span class="sxs-lookup"><span data-stu-id="542d1-146">Attribute</span></span>|<span data-ttu-id="542d1-147">Zdroje připojení</span><span class="sxs-lookup"><span data-stu-id="542d1-147">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="542d1-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="542d1-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="542d1-149">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="542d1-149">Request body</span></span> |
|<span data-ttu-id="542d1-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="542d1-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="542d1-151">Data formuláře v textu požadavku</span><span class="sxs-lookup"><span data-stu-id="542d1-151">Form data in the request body</span></span> |
|<span data-ttu-id="542d1-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="542d1-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="542d1-153">Hlavička požadavku</span><span class="sxs-lookup"><span data-stu-id="542d1-153">Request header</span></span> |
|<span data-ttu-id="542d1-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="542d1-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="542d1-155">Požádat o parametru řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="542d1-155">Request query string parameter</span></span> |
|<span data-ttu-id="542d1-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="542d1-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="542d1-157">Data trasy z aktuální žádosti</span><span class="sxs-lookup"><span data-stu-id="542d1-157">Route data from the current request</span></span> |
|<span data-ttu-id="542d1-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="542d1-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="542d1-159">Žádost o služby, vložený jako parametru akce</span><span class="sxs-lookup"><span data-stu-id="542d1-159">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="542d1-160">Nepoužívejte `[FromRoute]` při může obsahovat hodnoty `%2f` (to znamená `/`).</span><span class="sxs-lookup"><span data-stu-id="542d1-160">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="542d1-161">`%2f` nebude znaků bez řídících k `/`.</span><span class="sxs-lookup"><span data-stu-id="542d1-161">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="542d1-162">Použití `[FromQuery]` Pokud hodnota může obsahovat `%2f`.</span><span class="sxs-lookup"><span data-stu-id="542d1-162">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="542d1-163">Bez `[ApiController]` atribut, vytvoření vazby zdroje nejsou explicitně definovány atributy.</span><span class="sxs-lookup"><span data-stu-id="542d1-163">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="542d1-164">V následujícím příkladu `[FromQuery]` atribut označuje, že `discontinuedOnly` je zadána hodnota parametru v řetězci dotazu v adrese URL požadavku:</span><span class="sxs-lookup"><span data-stu-id="542d1-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="542d1-165">Odvozená pravidla se použijí pro zdroje dat výchozí parametry akce.</span><span class="sxs-lookup"><span data-stu-id="542d1-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="542d1-166">Tato pravidla konfigurace zdroje připojení, v opačném případě budete pravděpodobně pro ruční použití na parametry akce.</span><span class="sxs-lookup"><span data-stu-id="542d1-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="542d1-167">Atributy zdroje vazby chovají následovně:</span><span class="sxs-lookup"><span data-stu-id="542d1-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="542d1-168">**[FromBody]**  odvodit pro komplexní typ parametrů.</span><span class="sxs-lookup"><span data-stu-id="542d1-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="542d1-169">Výjimkou z tohoto pravidla je libovolný integrované, komplexní typ zvláštní význam, jako například <xref:Microsoft.AspNetCore.Http.IFormCollection> a <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="542d1-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="542d1-170">Zdrojový kód odvození vazby ignoruje tyto speciální typy.</span><span class="sxs-lookup"><span data-stu-id="542d1-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="542d1-171">`[FromBody]` není odvodit pro jednoduché typy, jako například `string` nebo `int`.</span><span class="sxs-lookup"><span data-stu-id="542d1-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="542d1-172">Proto `[FromBody]` atribut by měl použít pro jednoduché typy, které tuto funkci potřebujete.</span><span class="sxs-lookup"><span data-stu-id="542d1-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="542d1-173">Pokud má akci explicitně zadán více než jeden parametr (prostřednictvím `[FromBody]`) nebo odvozený jako vázaný z textu požadavku, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="542d1-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="542d1-174">Například následující akce podpisy způsobit výjimku:</span><span class="sxs-lookup"><span data-stu-id="542d1-174">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="542d1-175">**[FromForm]**  odvodit pro parametry akce typu <xref:Microsoft.AspNetCore.Http.IFormFile> a <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="542d1-175">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="542d1-176">Není odvodit pro jednoduché nebo uživatelem definované typy.</span><span class="sxs-lookup"><span data-stu-id="542d1-176">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="542d1-177">**[FromRoute]**  odvodit pro název parametru žádné akce odpovídající parametr v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="542d1-177">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="542d1-178">Když víc tras odpovídá parametru akce, je považován za libovolnou hodnotu trasy `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="542d1-178">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="542d1-179">**[FromQuery]**  odvodit pro všechny ostatní parametry akce.</span><span class="sxs-lookup"><span data-stu-id="542d1-179">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="542d1-180">Výchozí pravidla pro odvození jsou zakázané. Pokud <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> je nastavena na `true`.</span><span class="sxs-lookup"><span data-stu-id="542d1-180">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="542d1-181">Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="542d1-181">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="542d1-182">Odvození multipart/formulář data požadavku</span><span class="sxs-lookup"><span data-stu-id="542d1-182">Multipart/form-data request inference</span></span>

<span data-ttu-id="542d1-183">Když parametr akce je opatřen poznámkou [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) atribut, `multipart/form-data` žádosti je odvozený typ obsahu.</span><span class="sxs-lookup"><span data-stu-id="542d1-183">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="542d1-184">Výchozí chování je zakázaná. Pokud <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> je nastavena na `true`.</span><span class="sxs-lookup"><span data-stu-id="542d1-184">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="542d1-185">Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="542d1-185">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="542d1-186">Atribut směrování požadavků</span><span class="sxs-lookup"><span data-stu-id="542d1-186">Attribute routing requirement</span></span>

<span data-ttu-id="542d1-187">Směrování atributů se změní na požadavek.</span><span class="sxs-lookup"><span data-stu-id="542d1-187">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="542d1-188">Příklad:</span><span class="sxs-lookup"><span data-stu-id="542d1-188">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="542d1-189">Akce jsou nedostupné přes [trasy konvenční](xref:mvc/controllers/routing#conventional-routing) definované v <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> nebo <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> v *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="542d1-189">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="542d1-190">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="542d1-190">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>