---
title: Middleware ASP.NET Core
author: rick-anderson
description: "Další informace o ASP.NET Core middleware a kanál požadavku."
keywords: "ASP.NET Core, Middleware, kanál, delegát"
ms.author: riande
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: db9a86ab-46c2-40e0-baed-86e38c16af1f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: ad8d207b1e6de396f16d098fb07ddc89bea2c520
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="0399e-104">Základy Middleware ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0399e-104">ASP.NET Core Middleware Fundamentals</span></span>

<a name="fundamentals-middleware"></a>

<span data-ttu-id="0399e-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0399e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0399e-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0399e-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="0399e-107">Co je middleware</span><span class="sxs-lookup"><span data-stu-id="0399e-107">What is middleware</span></span>

<span data-ttu-id="0399e-108">Middleware je software, který je sestavit do kanálu určité aplikace pro zpracování požadavků a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="0399e-108">Middleware is software that is assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="0399e-109">Jednotlivé komponenty:</span><span class="sxs-lookup"><span data-stu-id="0399e-109">Each component:</span></span>

* <span data-ttu-id="0399e-110">Zvolí, jestli se k předání požadavku na další komponenta v kanálu.</span><span class="sxs-lookup"><span data-stu-id="0399e-110">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="0399e-111">Můžete práci před a po vyvolání další komponenta v kanálu.</span><span class="sxs-lookup"><span data-stu-id="0399e-111">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="0399e-112">Delegáti požadavku se používají k vytvoření kanálu požadavku.</span><span class="sxs-lookup"><span data-stu-id="0399e-112">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="0399e-113">Delegáti požadavek zpracovat každý požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="0399e-113">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="0399e-114">Žádosti o Delegáti jsou konfigurováni pomocí [spustit](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [mapy](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), a [použití](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="0399e-114">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="0399e-115">Delegáta individuální žádosti může být zadaný v řádku jako anonymní metody (nazývané v řádku middleware), nebo může být definováno v třídě opakovaně použitelné.</span><span class="sxs-lookup"><span data-stu-id="0399e-115">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="0399e-116">Tyto opakovaně použitelné třídy a metody anonymní v řádku jsou *middleware*, nebo *komponenty middlewaru*.</span><span class="sxs-lookup"><span data-stu-id="0399e-116">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="0399e-117">Jednotlivé komponenty middleware v kanálu požadavku zodpovídá za vyvolání další komponenta v kanálu nebo krátká smyčka řetězu v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="0399e-117">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="0399e-118">[Migrace modulů HTTP s Middlewarem](../migration/http-modules.md) najdete vysvětlení rozdílu mezi požadavek kanály v ASP.NET Core a předchozí verze a poskytuje další middleware ukázky.</span><span class="sxs-lookup"><span data-stu-id="0399e-118">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="0399e-119">Vytvoření kanálu middlewaru s IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="0399e-119">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="0399e-120">Kanál požadavku ASP.NET Core se skládá z posloupnost delegáti požadavku, říká jeden za druhým, tento diagram zobrazuje (vlákno provádění způsobem černé šipky):</span><span class="sxs-lookup"><span data-stu-id="0399e-120">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Žádost o zpracování vzoru zobrazující žádost přicházejících, zpracování prostřednictvím tři middlewares a odpověď je aplikaci.](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="0399e-124">Každý delegát provádět operace před a po další delegáta.</span><span class="sxs-lookup"><span data-stu-id="0399e-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="0399e-125">Delegát můžete také rozhodnout není předat požadavek na další delegáta, který se nazývá krátká smyčka kanál požadavku.</span><span class="sxs-lookup"><span data-stu-id="0399e-125">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="0399e-126">Krátká smyčka je často žádoucí, protože při ní nedochází nepotřebné práci.</span><span class="sxs-lookup"><span data-stu-id="0399e-126">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="0399e-127">Middleware se statickými soubory můžete například vrátit požadavek na statický soubor a krátká smyčka zbytek kanálu.</span><span class="sxs-lookup"><span data-stu-id="0399e-127">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="0399e-128">Zpracování výjimek delegáti muset volat již v rané fázi v kanálu, takže se můžete zachytit výjimky, ke kterým došlo v novější fázemi kanálu.</span><span class="sxs-lookup"><span data-stu-id="0399e-128">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="0399e-129">Nejjednodušší možné aplikace ASP.NET Core nastaví delegáta jedné žádosti, která zpracovává všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="0399e-129">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="0399e-130">Tento případ neobsahuje kanál skutečné požadavku.</span><span class="sxs-lookup"><span data-stu-id="0399e-130">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="0399e-131">Místo toho jedné anonymní funkce je volána v reakci na každý požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="0399e-131">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="0399e-132">První [aplikace. Spustit](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegáta ukončí kanálu.</span><span class="sxs-lookup"><span data-stu-id="0399e-132">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="0399e-133">Můžete řetězu více delegátů žádost společně s [aplikace. Použití](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="0399e-133">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="0399e-134">`next` Parametr představuje další delegát v kanálu.</span><span class="sxs-lookup"><span data-stu-id="0399e-134">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="0399e-135">(Mějte na paměti, že může krátká smyčka kanálu pomocí *není* volání *Další* parametru.) Můžete obvykle provádět akce před i po další delegáta, jak ukazuje tento příklad:</span><span class="sxs-lookup"><span data-stu-id="0399e-135">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="0399e-136">Nevolejte `next.Invoke` po odeslání odpovědi klientovi.</span><span class="sxs-lookup"><span data-stu-id="0399e-136">Do not call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="0399e-137">Změny `HttpResponse` po spuštění odpovědi vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="0399e-137">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="0399e-138">Například změny, jako je třeba nastavení hlavičky, stavový kód atd., vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="0399e-138">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="0399e-139">Zápis do text odpovědi po volání `next`:</span><span class="sxs-lookup"><span data-stu-id="0399e-139">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="0399e-140">Může způsobit narušení protokolu.</span><span class="sxs-lookup"><span data-stu-id="0399e-140">May cause a protocol violation.</span></span> <span data-ttu-id="0399e-141">Například zápis více než deklarovaným `content-length`.</span><span class="sxs-lookup"><span data-stu-id="0399e-141">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="0399e-142">Může dojít k poškození formátu textu.</span><span class="sxs-lookup"><span data-stu-id="0399e-142">May corrupt the body format.</span></span> <span data-ttu-id="0399e-143">Například zápatí HTML zápis do souboru CSS.</span><span class="sxs-lookup"><span data-stu-id="0399e-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="0399e-144">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) je užitečné nápovědu k označení, pokud byly odeslány hlavičky nebo text byl proveden zápis.</span><span class="sxs-lookup"><span data-stu-id="0399e-144">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="0399e-145">Řazení</span><span class="sxs-lookup"><span data-stu-id="0399e-145">Ordering</span></span>

<span data-ttu-id="0399e-146">Middleware součásti jsou přidány v pořadí `Configure` metoda definuje pořadí, ve kterém jsou vyvolány na požadavky a pro odpověď v obráceném pořadí.</span><span class="sxs-lookup"><span data-stu-id="0399e-146">The order that middleware components are added in the `Configure` method defines the order in which they are invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="0399e-147">Toto pořadí je důležité pro zabezpečení, výkonu a funkce.</span><span class="sxs-lookup"><span data-stu-id="0399e-147">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="0399e-148">Metodu konfigurace (viz dole) přidá middleware následující součásti:</span><span class="sxs-lookup"><span data-stu-id="0399e-148">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="0399e-149">Zpracování výjimek/chyb</span><span class="sxs-lookup"><span data-stu-id="0399e-149">Exception/error handling</span></span>
2. <span data-ttu-id="0399e-150">Statické souborového serveru</span><span class="sxs-lookup"><span data-stu-id="0399e-150">Static file server</span></span>
3. <span data-ttu-id="0399e-151">Ověřování</span><span class="sxs-lookup"><span data-stu-id="0399e-151">Authentication</span></span>
4. <span data-ttu-id="0399e-152">MVC</span><span class="sxs-lookup"><span data-stu-id="0399e-152">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0399e-153">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="0399e-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0399e-154">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="0399e-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="0399e-155">Ve výše, kódu `UseExceptionHandler` je první komponenta middlewaru přidané do kanálu – tedy zachytávalo jakékoli výjimky, které se vyskytují v pozdější volání.</span><span class="sxs-lookup"><span data-stu-id="0399e-155">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="0399e-156">Middleware se statickými soubory se nazývá již v rané fázi v kanálu, aby mohl zpracovávat požadavky a krátká smyčka – bez průchodu přes zbývající součásti.</span><span class="sxs-lookup"><span data-stu-id="0399e-156">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="0399e-157">Poskytuje middleware se statickými soubory **žádné** kontroly autorizace.</span><span class="sxs-lookup"><span data-stu-id="0399e-157">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="0399e-158">Všechny soubory obsluhovat, včetně těch na *wwwroot*, jsou veřejně dostupné.</span><span class="sxs-lookup"><span data-stu-id="0399e-158">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="0399e-159">V tématu [práce s statické soubory](xref:fundamentals/static-files) pro přístup k zabezpečení statické soubory.</span><span class="sxs-lookup"><span data-stu-id="0399e-159">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0399e-160">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="0399e-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="0399e-161">Pokud požadavek není zpracováván middleware se statickými soubory, je předána Identity middleware (`app.UseAuthentication`), který provádí ověřování.</span><span class="sxs-lookup"><span data-stu-id="0399e-161">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="0399e-162">Identita není krátká smyčka neověřené požadavky.</span><span class="sxs-lookup"><span data-stu-id="0399e-162">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="0399e-163">I když žádosti o ověření Identity autorizace (a odmítání) nastane pouze po MVC vybere konkrétní stránky Razor nebo kontroleru a akce.</span><span class="sxs-lookup"><span data-stu-id="0399e-163">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0399e-164">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="0399e-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0399e-165">Pokud požadavek není zpracováván middleware se statickými soubory, je předána Identity middleware (`app.UseIdentity`), který provádí ověřování.</span><span class="sxs-lookup"><span data-stu-id="0399e-165">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="0399e-166">Identita není krátká smyčka neověřené požadavky.</span><span class="sxs-lookup"><span data-stu-id="0399e-166">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="0399e-167">I když žádosti o ověření Identity autorizace (a odmítání) nastane pouze po MVC vybere konkrétní kontroleru a akce.</span><span class="sxs-lookup"><span data-stu-id="0399e-167">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="0399e-168">Následující příklad ukazuje, kde zpracovává požadavky pro statické soubory middleware se statickými soubory před middleware komprese odpovědi řazení middleware.</span><span class="sxs-lookup"><span data-stu-id="0399e-168">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="0399e-169">Statické soubory nejsou komprimované s toto uspořádání middleware.</span><span class="sxs-lookup"><span data-stu-id="0399e-169">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="0399e-170">Z odpovědi MVC [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) lze komprimovat.</span><span class="sxs-lookup"><span data-stu-id="0399e-170">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="0399e-171">Použití, spuštění a mapy</span><span class="sxs-lookup"><span data-stu-id="0399e-171">Use, Run, and Map</span></span>

<span data-ttu-id="0399e-172">Můžete nakonfigurovat pomocí kanálu HTTP `Use`, `Run`, a `Map`.</span><span class="sxs-lookup"><span data-stu-id="0399e-172">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="0399e-173">`Use` Metoda může krátká smyčka kanálu (tj. Pokud ho nevyvolá `next` delegáta požadavek).</span><span class="sxs-lookup"><span data-stu-id="0399e-173">The `Use` method can short-circuit the pipeline (that is, if it does not call a `next` request delegate).</span></span> <span data-ttu-id="0399e-174">`Run`je konvence, a může vystavit některé komponenty middlewaru `Run[Middleware]` metody, které běží na konci kanálu.</span><span class="sxs-lookup"><span data-stu-id="0399e-174">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="0399e-175">`Map*`rozšíření jsou použity jako konvence pro vytvoření větve kanálu.</span><span class="sxs-lookup"><span data-stu-id="0399e-175">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="0399e-176">[Mapa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) větví kanálu požadavku podle odpovídá zadanou cestu požadavku.</span><span class="sxs-lookup"><span data-stu-id="0399e-176">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="0399e-177">Pokud cesta požadavku začíná zadané cestě, je proveden větev.</span><span class="sxs-lookup"><span data-stu-id="0399e-177">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="0399e-178">V následující tabulce jsou uvedeny požadavky a odpovědi z `http://localhost:1234` pomocí předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="0399e-178">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="0399e-179">Požadavek</span><span class="sxs-lookup"><span data-stu-id="0399e-179">Request</span></span> | <span data-ttu-id="0399e-180">Odpověď</span><span class="sxs-lookup"><span data-stu-id="0399e-180">Response</span></span> |
| --- | --- |
| <span data-ttu-id="0399e-181">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="0399e-181">localhost:1234</span></span> | <span data-ttu-id="0399e-182">Hello z jiných mapy delegáta.</span><span class="sxs-lookup"><span data-stu-id="0399e-182">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="0399e-183">localhost:1234 / map1</span><span class="sxs-lookup"><span data-stu-id="0399e-183">localhost:1234/map1</span></span> | <span data-ttu-id="0399e-184">Mapa Test 1</span><span class="sxs-lookup"><span data-stu-id="0399e-184">Map Test 1</span></span> |
| <span data-ttu-id="0399e-185">localhost:1234 / map2 –</span><span class="sxs-lookup"><span data-stu-id="0399e-185">localhost:1234/map2</span></span> | <span data-ttu-id="0399e-186">Mapa testu 2</span><span class="sxs-lookup"><span data-stu-id="0399e-186">Map Test 2</span></span> |
| <span data-ttu-id="0399e-187">localhost:1234 / map3 –</span><span class="sxs-lookup"><span data-stu-id="0399e-187">localhost:1234/map3</span></span> | <span data-ttu-id="0399e-188">Hello z jiných mapy delegáta.</span><span class="sxs-lookup"><span data-stu-id="0399e-188">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="0399e-189">Když `Map` se používá, segment(s) odpovídající cesta se odeberou z `HttpRequest.Path` a připojením k `HttpRequest.PathBase` pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="0399e-189">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="0399e-190">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) větví kanál požadavku na základě výsledku daného predikátu.</span><span class="sxs-lookup"><span data-stu-id="0399e-190">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="0399e-191">Všechny predikát typu `Func<HttpContext, bool>` lze použít k mapování požadavků na nové větve kanálu.</span><span class="sxs-lookup"><span data-stu-id="0399e-191">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="0399e-192">V následujícím příkladu predikát slouží k detekci přítomnosti proměnné řetězce dotazu `branch`:</span><span class="sxs-lookup"><span data-stu-id="0399e-192">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="0399e-193">V následující tabulce jsou uvedeny požadavky a odpovědi z `http://localhost:1234` pomocí předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="0399e-193">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="0399e-194">Požadavek</span><span class="sxs-lookup"><span data-stu-id="0399e-194">Request</span></span> | <span data-ttu-id="0399e-195">Odpověď</span><span class="sxs-lookup"><span data-stu-id="0399e-195">Response</span></span> |
| --- | --- |
| <span data-ttu-id="0399e-196">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="0399e-196">localhost:1234</span></span> | <span data-ttu-id="0399e-197">Hello z jiných mapy delegáta.</span><span class="sxs-lookup"><span data-stu-id="0399e-197">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="0399e-198">localhost:1234 /? větve = hlavní</span><span class="sxs-lookup"><span data-stu-id="0399e-198">localhost:1234/?branch=master</span></span> | <span data-ttu-id="0399e-199">Větev použít = hlavní</span><span class="sxs-lookup"><span data-stu-id="0399e-199">Branch used = master</span></span>|

<span data-ttu-id="0399e-200">`Map`podporuje vnoření, například:</span><span class="sxs-lookup"><span data-stu-id="0399e-200">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="0399e-201">`Map`Můžete také shoda s více segmenty najednou, například:</span><span class="sxs-lookup"><span data-stu-id="0399e-201">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="0399e-202">Předdefinované middlewaru</span><span class="sxs-lookup"><span data-stu-id="0399e-202">Built-in middleware</span></span>

<span data-ttu-id="0399e-203">ASP.NET Core se dodává s následujícími součástmi middleware:</span><span class="sxs-lookup"><span data-stu-id="0399e-203">ASP.NET Core ships with the following middleware components:</span></span>

| <span data-ttu-id="0399e-204">Middleware</span><span class="sxs-lookup"><span data-stu-id="0399e-204">Middleware</span></span> | <span data-ttu-id="0399e-205">Popis</span><span class="sxs-lookup"><span data-stu-id="0399e-205">Description</span></span> |
| ----- | ------- |
| [<span data-ttu-id="0399e-206">Ověřování</span><span class="sxs-lookup"><span data-stu-id="0399e-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="0399e-207">Poskytuje podporu ověřování.</span><span class="sxs-lookup"><span data-stu-id="0399e-207">Provides authentication support.</span></span> |
| [<span data-ttu-id="0399e-208">CORS</span><span class="sxs-lookup"><span data-stu-id="0399e-208">CORS</span></span>](xref:security/cors) | <span data-ttu-id="0399e-209">Konfiguruje sdílení prostředků různého původu.</span><span class="sxs-lookup"><span data-stu-id="0399e-209">Configures Cross-Origin Resource Sharing.</span></span> |
| [<span data-ttu-id="0399e-210">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="0399e-210">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="0399e-211">Poskytuje podporu pro ukládání do mezipaměti odpovědi.</span><span class="sxs-lookup"><span data-stu-id="0399e-211">Provides support for caching responses.</span></span> |
| [<span data-ttu-id="0399e-212">Komprese odpovědi</span><span class="sxs-lookup"><span data-stu-id="0399e-212">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="0399e-213">Poskytuje podporu pro kompresi odpovědí.</span><span class="sxs-lookup"><span data-stu-id="0399e-213">Provides support for compressing responses.</span></span> |
| [<span data-ttu-id="0399e-214">Směrování</span><span class="sxs-lookup"><span data-stu-id="0399e-214">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="0399e-215">Definuje a omezí požadavek trasy.</span><span class="sxs-lookup"><span data-stu-id="0399e-215">Defines and constrains request routes.</span></span> |
| [<span data-ttu-id="0399e-216">Relace</span><span class="sxs-lookup"><span data-stu-id="0399e-216">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="0399e-217">Poskytuje podporu pro správu uživatelských relací.</span><span class="sxs-lookup"><span data-stu-id="0399e-217">Provides support for managing user sessions.</span></span> |
| [<span data-ttu-id="0399e-218">Statické soubory</span><span class="sxs-lookup"><span data-stu-id="0399e-218">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="0399e-219">Poskytuje podporu pro obsluhující statické soubory a procházení adresářů.</span><span class="sxs-lookup"><span data-stu-id="0399e-219">Provides support for serving static files and directory browsing.</span></span> |
| [<span data-ttu-id="0399e-220">Adresa URL přepisování middlewaru</span><span class="sxs-lookup"><span data-stu-id="0399e-220">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="0399e-221">Poskytuje podporu pro přepisování adres URL a přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="0399e-221">Provides support for rewriting URLs and redirecting requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="0399e-222">Zápis middlewaru</span><span class="sxs-lookup"><span data-stu-id="0399e-222">Writing middleware</span></span>

<span data-ttu-id="0399e-223">Middleware je obecně zapouzdřené v třídě a vystavené pomocí metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="0399e-223">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="0399e-224">Vezměte v úvahu následující middlewaru, který nastaví jazykovou verzi pro aktuální požadavek z řetězce dotazu:</span><span class="sxs-lookup"><span data-stu-id="0399e-224">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="0399e-225">Poznámka: Výše uvedeném ukázkovém kódu se používá k předvedení vytváření komponenta middlewaru.</span><span class="sxs-lookup"><span data-stu-id="0399e-225">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="0399e-226">V tématu [ globalizace a lokalizace](xref:fundamentals/localization) pro podporu předdefinované lokalizace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0399e-226">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="0399e-227">Middleware můžete otestovat pomocí předání v jazykovou verzi, například `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="0399e-227">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="0399e-228">Následující kód přesune delegáta middleware na třídu:</span><span class="sxs-lookup"><span data-stu-id="0399e-228">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="0399e-229">Zpřístupní metodu rozšíření middleware prostřednictvím [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="0399e-229">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="0399e-230">Následující kód volá middleware z `Configure`:</span><span class="sxs-lookup"><span data-stu-id="0399e-230">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="0399e-231">Postupujte podle middleware [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/) díky zpřístupnění jeho závislé součásti v jeho konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="0399e-231">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="0399e-232">Middleware je vytvořený jednou za *životního cyklu aplikace*.</span><span class="sxs-lookup"><span data-stu-id="0399e-232">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="0399e-233">V tématu *požadavků závislosti* níže v případě, je potřeba sdílet s middlewaru v rámci žádost o služby.</span><span class="sxs-lookup"><span data-stu-id="0399e-233">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="0399e-234">Komponenty middlewaru lze vyřešit závislé z vkládání závislostí prostřednictvím parametrů konstruktor.</span><span class="sxs-lookup"><span data-stu-id="0399e-234">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="0399e-235">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)Můžete také přijímat další parametry, přímo.</span><span class="sxs-lookup"><span data-stu-id="0399e-235">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="0399e-236">Závislosti na žádost</span><span class="sxs-lookup"><span data-stu-id="0399e-236">Per-request dependencies</span></span>

<span data-ttu-id="0399e-237">Protože middlewaru je vytvořený při spuštění aplikace, není požadavků, *obor* služby životního cyklu, které middleware konstruktory používá nejsou sdílené s jinými typy vložit závislostí při každé žádosti.</span><span class="sxs-lookup"><span data-stu-id="0399e-237">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="0399e-238">Pokud je nutné sdílet *obor* služby mezi vlastního middlewaru a jinými typy, přidejte tyto služby `Invoke` podpis metody.</span><span class="sxs-lookup"><span data-stu-id="0399e-238">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="0399e-239">`Invoke` Metoda může přijímat další parametry, které jsou naplněny pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="0399e-239">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="0399e-240">Příklad:</span><span class="sxs-lookup"><span data-stu-id="0399e-240">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a><span data-ttu-id="0399e-241">Prostředky</span><span class="sxs-lookup"><span data-stu-id="0399e-241">Resources</span></span>

* [<span data-ttu-id="0399e-242">Ukázkový kód používá v této dokumentace</span><span class="sxs-lookup"><span data-stu-id="0399e-242">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="0399e-243">Migrace modulů HTTP k middlewaru.</span><span class="sxs-lookup"><span data-stu-id="0399e-243">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="0399e-244">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="0399e-244">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="0399e-245">Požadavky na funkce</span><span class="sxs-lookup"><span data-stu-id="0399e-245">Request Features</span></span>](request-features.md)
