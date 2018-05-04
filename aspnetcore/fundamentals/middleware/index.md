---
title: Middleware ASP.NET Core
author: rick-anderson
description: Další informace o ASP.NET Core middleware a kanál požadavku.
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 4c44063fb3385fc625c35c8a3cf06a35b5b0afb7
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="380c2-103">Middleware ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="380c2-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="380c2-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="380c2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="380c2-105">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="380c2-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="380c2-106">Co je middleware?</span><span class="sxs-lookup"><span data-stu-id="380c2-106">What is middleware?</span></span>

<span data-ttu-id="380c2-107">Middleware je software, který je sestavit do kanálu určité aplikace pro zpracování požadavků a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="380c2-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="380c2-108">Jednotlivé komponenty:</span><span class="sxs-lookup"><span data-stu-id="380c2-108">Each component:</span></span>

* <span data-ttu-id="380c2-109">Zvolí, jestli se k předání požadavku na další komponenta v kanálu.</span><span class="sxs-lookup"><span data-stu-id="380c2-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="380c2-110">Můžete práci před a po vyvolání další komponenta v kanálu.</span><span class="sxs-lookup"><span data-stu-id="380c2-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="380c2-111">Delegáti požadavku se používají k vytvoření kanálu požadavku.</span><span class="sxs-lookup"><span data-stu-id="380c2-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="380c2-112">Delegáti požadavek zpracovat každý požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="380c2-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="380c2-113">Žádosti o Delegáti jsou konfigurováni pomocí [spustit](/dotnet/api/microsoft.aspnetcore.builder.runextensions), [mapy](/dotnet/api/microsoft.aspnetcore.builder.mapextensions), a [použití](/dotnet/api/microsoft.aspnetcore.builder.useextensions) rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="380c2-113">Request delegates are configured using [Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions), [Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions), and [Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="380c2-114">Delegáta individuální žádosti může být zadaný v řádku jako anonymní metody (nazývané v řádku middleware), nebo může být definováno v třídě opakovaně použitelné.</span><span class="sxs-lookup"><span data-stu-id="380c2-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="380c2-115">Tyto opakovaně použitelné třídy a metody anonymní v řádku jsou *middleware*, nebo *komponenty middlewaru*.</span><span class="sxs-lookup"><span data-stu-id="380c2-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="380c2-116">Jednotlivé komponenty middleware v kanálu požadavku zodpovídá za vyvolání další komponenta v kanálu nebo krátká smyčka řetězu v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="380c2-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="380c2-117">[Migrovat vytváření modulů HTTP v middlewaru](xref:migration/http-modules) najdete vysvětlení rozdílu mezi požadavek kanály v ASP.NET Core a ASP.NET 4.x a poskytuje další middleware ukázky.</span><span class="sxs-lookup"><span data-stu-id="380c2-117">[Migrate HTTP Modules to Middleware](xref:migration/http-modules) explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="380c2-118">Vytvoření kanálu middlewaru s IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="380c2-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="380c2-119">Kanál požadavku ASP.NET Core se skládá z posloupnost delegáti požadavku, říká jeden za druhým, tento diagram zobrazuje (vlákno provádění způsobem černé šipky):</span><span class="sxs-lookup"><span data-stu-id="380c2-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Žádost o zpracování vzoru zobrazující žádost přicházejících, zpracování prostřednictvím tři middlewares a odpověď je aplikaci.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="380c2-123">Každý delegát provádět operace před a po další delegáta.</span><span class="sxs-lookup"><span data-stu-id="380c2-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="380c2-124">Delegát můžete také rozhodnout není předat požadavek na další delegáta, který se nazývá krátká smyčka kanál požadavku.</span><span class="sxs-lookup"><span data-stu-id="380c2-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="380c2-125">Krátká smyčka je často žádoucí, protože při ní nedochází nepotřebné práci.</span><span class="sxs-lookup"><span data-stu-id="380c2-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="380c2-126">Middleware se statickými soubory můžete například vrátit požadavek na statický soubor a krátká smyčka zbytek kanálu.</span><span class="sxs-lookup"><span data-stu-id="380c2-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="380c2-127">Zpracování výjimek delegáti muset volat již v rané fázi v kanálu, takže se můžete zachytit výjimky, ke kterým došlo v novější fázemi kanálu.</span><span class="sxs-lookup"><span data-stu-id="380c2-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="380c2-128">Nejjednodušší možné aplikace ASP.NET Core nastaví delegáta jedné žádosti, která zpracovává všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="380c2-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="380c2-129">Tento případ neobsahuje kanál skutečné požadavku.</span><span class="sxs-lookup"><span data-stu-id="380c2-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="380c2-130">Místo toho jedné anonymní funkce je volána v reakci na každý požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="380c2-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/sample/Middleware/Startup.cs)]

<span data-ttu-id="380c2-131">První [aplikace. Spustit](/dotnet/api/microsoft.aspnetcore.builder.runextensions) delegáta ukončí kanálu.</span><span class="sxs-lookup"><span data-stu-id="380c2-131">The first [app.Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="380c2-132">Můžete řetězu více delegátů žádost společně s [aplikace. Použití](/dotnet/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="380c2-132">You can chain multiple request delegates together with [app.Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="380c2-133">`next` Parametr představuje další delegát v kanálu.</span><span class="sxs-lookup"><span data-stu-id="380c2-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="380c2-134">(Mějte na paměti, že může krátká smyčka kanálu pomocí *není* volání *Další* parametru.) Můžete obvykle provádět akce před i po další delegáta, jak ukazuje tento příklad:</span><span class="sxs-lookup"><span data-stu-id="380c2-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="380c2-135">Nemůžete volat `next.Invoke` po odeslání odpovědi klientovi.</span><span class="sxs-lookup"><span data-stu-id="380c2-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="380c2-136">Změny `HttpResponse` po spuštění odpovědi vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="380c2-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="380c2-137">Například změny, jako je třeba nastavení hlavičky, stavový kód atd., vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="380c2-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="380c2-138">Zápis do text odpovědi po volání `next`:</span><span class="sxs-lookup"><span data-stu-id="380c2-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="380c2-139">Může způsobit narušení protokolu.</span><span class="sxs-lookup"><span data-stu-id="380c2-139">May cause a protocol violation.</span></span> <span data-ttu-id="380c2-140">Například zápis více než deklarovaným `content-length`.</span><span class="sxs-lookup"><span data-stu-id="380c2-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="380c2-141">Může dojít k poškození formátu textu.</span><span class="sxs-lookup"><span data-stu-id="380c2-141">May corrupt the body format.</span></span> <span data-ttu-id="380c2-142">Například zápatí HTML zápis do souboru CSS.</span><span class="sxs-lookup"><span data-stu-id="380c2-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="380c2-143">[HttpResponse.HasStarted](/dotnet/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) je užitečné nápovědu k označení, pokud byly odeslány hlavičky nebo text byl proveden zápis.</span><span class="sxs-lookup"><span data-stu-id="380c2-143">[HttpResponse.HasStarted](/dotnet/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="380c2-144">Řazení</span><span class="sxs-lookup"><span data-stu-id="380c2-144">Ordering</span></span>

<span data-ttu-id="380c2-145">Middleware součásti jsou přidány v pořadí `Configure` metoda definuje pořadí, ve kterém se volá na požadavky a pro odpověď v obráceném pořadí.</span><span class="sxs-lookup"><span data-stu-id="380c2-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="380c2-146">Toto pořadí je důležité pro zabezpečení, výkonu a funkce.</span><span class="sxs-lookup"><span data-stu-id="380c2-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="380c2-147">Metodu konfigurace (viz dole) přidá middleware následující součásti:</span><span class="sxs-lookup"><span data-stu-id="380c2-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="380c2-148">Zpracování výjimek/chyb</span><span class="sxs-lookup"><span data-stu-id="380c2-148">Exception/error handling</span></span>
2. <span data-ttu-id="380c2-149">Statické souborového serveru</span><span class="sxs-lookup"><span data-stu-id="380c2-149">Static file server</span></span>
3. <span data-ttu-id="380c2-150">Ověřování</span><span class="sxs-lookup"><span data-stu-id="380c2-150">Authentication</span></span>
4. <span data-ttu-id="380c2-151">MVC</span><span class="sxs-lookup"><span data-stu-id="380c2-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="380c2-152">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="380c2-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="380c2-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="380c2-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="380c2-154">Ve výše, kódu `UseExceptionHandler` je první komponenta middlewaru přidané do kanálu – tedy zachytávalo jakékoli výjimky, které se vyskytují v pozdější volání.</span><span class="sxs-lookup"><span data-stu-id="380c2-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="380c2-155">Middleware se statickými soubory se nazývá již v rané fázi v kanálu, aby mohl zpracovávat požadavky a krátká smyčka – bez průchodu přes zbývající součásti.</span><span class="sxs-lookup"><span data-stu-id="380c2-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="380c2-156">Poskytuje middleware se statickými soubory **žádné** kontroly autorizace.</span><span class="sxs-lookup"><span data-stu-id="380c2-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="380c2-157">Všechny soubory obsluhovat, včetně těch na *wwwroot*, jsou veřejně dostupné.</span><span class="sxs-lookup"><span data-stu-id="380c2-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="380c2-158">V tématu [pracovat s statické soubory](xref:fundamentals/static-files) pro přístup k zabezpečení statické soubory.</span><span class="sxs-lookup"><span data-stu-id="380c2-158">See [Work with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="380c2-159">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="380c2-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="380c2-160">Pokud požadavek není zpracováván middleware se statickými soubory, je předána Identity middleware (`app.UseAuthentication`), který provádí ověřování.</span><span class="sxs-lookup"><span data-stu-id="380c2-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="380c2-161">Identita není krátká smyčka neověřené požadavky.</span><span class="sxs-lookup"><span data-stu-id="380c2-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="380c2-162">I když žádosti o ověření Identity autorizace (a odmítání) nastane pouze po MVC vybere konkrétní stránky Razor nebo kontroleru a akce.</span><span class="sxs-lookup"><span data-stu-id="380c2-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="380c2-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="380c2-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="380c2-164">Pokud požadavek není zpracováván middleware se statickými soubory, je předána Identity middleware (`app.UseIdentity`), který provádí ověřování.</span><span class="sxs-lookup"><span data-stu-id="380c2-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="380c2-165">Identita není krátká smyčka neověřené požadavky.</span><span class="sxs-lookup"><span data-stu-id="380c2-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="380c2-166">I když žádosti o ověření Identity autorizace (a odmítání) nastane pouze po MVC vybere konkrétní kontroleru a akce.</span><span class="sxs-lookup"><span data-stu-id="380c2-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="380c2-167">Následující příklad ukazuje, kde zpracovává požadavky pro statické soubory middleware se statickými soubory před middleware komprese odpovědi řazení middleware.</span><span class="sxs-lookup"><span data-stu-id="380c2-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="380c2-168">Statické soubory nejsou komprimované s toto uspořádání middleware.</span><span class="sxs-lookup"><span data-stu-id="380c2-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="380c2-169">Z odpovědi MVC [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) lze komprimovat.</span><span class="sxs-lookup"><span data-stu-id="380c2-169">The MVC responses from [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

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

### <a name="use-run-and-map"></a><span data-ttu-id="380c2-170">Použití, spuštění a mapy</span><span class="sxs-lookup"><span data-stu-id="380c2-170">Use, Run, and Map</span></span>

<span data-ttu-id="380c2-171">Můžete nakonfigurovat pomocí kanálu HTTP `Use`, `Run`, a `Map`.</span><span class="sxs-lookup"><span data-stu-id="380c2-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="380c2-172">`Use` Metoda může krátká smyčka kanálu (tj. Pokud není volání `next` delegáta požadavek).</span><span class="sxs-lookup"><span data-stu-id="380c2-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="380c2-173">`Run` je konvence, a může vystavit některé komponenty middlewaru `Run[Middleware]` metody, které běží na konci kanálu.</span><span class="sxs-lookup"><span data-stu-id="380c2-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="380c2-174">`Map*` rozšíření jsou použity jako konvence pro vytvoření větve kanálu.</span><span class="sxs-lookup"><span data-stu-id="380c2-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="380c2-175">[Mapa](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) větví kanálu požadavku podle odpovídá zadanou cestu požadavku.</span><span class="sxs-lookup"><span data-stu-id="380c2-175">[Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="380c2-176">Pokud cesta požadavku začíná zadané cestě, je proveden větev.</span><span class="sxs-lookup"><span data-stu-id="380c2-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="380c2-177">V následující tabulce jsou uvedeny požadavky a odpovědi z `http://localhost:1234` pomocí předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="380c2-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="380c2-178">Požadavek</span><span class="sxs-lookup"><span data-stu-id="380c2-178">Request</span></span> | <span data-ttu-id="380c2-179">Odpověď</span><span class="sxs-lookup"><span data-stu-id="380c2-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="380c2-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="380c2-180">localhost:1234</span></span> | <span data-ttu-id="380c2-181">Hello z jiných mapy delegáta.</span><span class="sxs-lookup"><span data-stu-id="380c2-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="380c2-182">localhost:1234 / map1</span><span class="sxs-lookup"><span data-stu-id="380c2-182">localhost:1234/map1</span></span> | <span data-ttu-id="380c2-183">Mapa Test 1</span><span class="sxs-lookup"><span data-stu-id="380c2-183">Map Test 1</span></span> |
| <span data-ttu-id="380c2-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="380c2-184">localhost:1234/map2</span></span> | <span data-ttu-id="380c2-185">Mapa testu 2</span><span class="sxs-lookup"><span data-stu-id="380c2-185">Map Test 2</span></span> |
| <span data-ttu-id="380c2-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="380c2-186">localhost:1234/map3</span></span> | <span data-ttu-id="380c2-187">Hello z jiných mapy delegáta.</span><span class="sxs-lookup"><span data-stu-id="380c2-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="380c2-188">Když `Map` se používá, segment(s) odpovídající cesta se odeberou z `HttpRequest.Path` a připojením k `HttpRequest.PathBase` pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="380c2-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="380c2-189">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) větví kanál požadavku na základě výsledku daného predikátu.</span><span class="sxs-lookup"><span data-stu-id="380c2-189">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="380c2-190">Všechny predikát typu `Func<HttpContext, bool>` lze použít k mapování požadavků na nové větve kanálu.</span><span class="sxs-lookup"><span data-stu-id="380c2-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="380c2-191">V následujícím příkladu predikát slouží k detekci přítomnosti proměnné řetězce dotazu `branch`:</span><span class="sxs-lookup"><span data-stu-id="380c2-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="380c2-192">V následující tabulce jsou uvedeny požadavky a odpovědi z `http://localhost:1234` pomocí předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="380c2-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="380c2-193">Požadavek</span><span class="sxs-lookup"><span data-stu-id="380c2-193">Request</span></span> | <span data-ttu-id="380c2-194">Odpověď</span><span class="sxs-lookup"><span data-stu-id="380c2-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="380c2-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="380c2-195">localhost:1234</span></span> | <span data-ttu-id="380c2-196">Hello z jiných mapy delegáta.</span><span class="sxs-lookup"><span data-stu-id="380c2-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="380c2-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="380c2-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="380c2-198">Větev použít = hlavní</span><span class="sxs-lookup"><span data-stu-id="380c2-198">Branch used = master</span></span>|

<span data-ttu-id="380c2-199">`Map` podporuje vnoření, například:</span><span class="sxs-lookup"><span data-stu-id="380c2-199">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="380c2-200">`Map` Můžete také shoda s více segmenty najednou, například:</span><span class="sxs-lookup"><span data-stu-id="380c2-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="380c2-201">Předdefinované middlewaru</span><span class="sxs-lookup"><span data-stu-id="380c2-201">Built-in middleware</span></span>

<span data-ttu-id="380c2-202">ASP.NET Core se dodává s následující komponenty middlewaru, jakož i popis pořadí, ve kterém se má přidat:</span><span class="sxs-lookup"><span data-stu-id="380c2-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="380c2-203">Middleware</span><span class="sxs-lookup"><span data-stu-id="380c2-203">Middleware</span></span> | <span data-ttu-id="380c2-204">Popis</span><span class="sxs-lookup"><span data-stu-id="380c2-204">Description</span></span> | <span data-ttu-id="380c2-205">Pořadí</span><span class="sxs-lookup"><span data-stu-id="380c2-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="380c2-206">Ověřování</span><span class="sxs-lookup"><span data-stu-id="380c2-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="380c2-207">Poskytuje podporu ověřování.</span><span class="sxs-lookup"><span data-stu-id="380c2-207">Provides authentication support.</span></span> | <span data-ttu-id="380c2-208">Před `HttpContext.User` je potřeba.</span><span class="sxs-lookup"><span data-stu-id="380c2-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="380c2-209">Terminálu zpětná volání OAuth.</span><span class="sxs-lookup"><span data-stu-id="380c2-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="380c2-210">CORS</span><span class="sxs-lookup"><span data-stu-id="380c2-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="380c2-211">Konfiguruje sdílení prostředků různého původu.</span><span class="sxs-lookup"><span data-stu-id="380c2-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="380c2-212">Před provedením komponent, které používají CORS.</span><span class="sxs-lookup"><span data-stu-id="380c2-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="380c2-213">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="380c2-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="380c2-214">Nakonfiguruje diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="380c2-214">Configures diagnostics.</span></span> | <span data-ttu-id="380c2-215">Před provedením komponent, které generují chyby.</span><span class="sxs-lookup"><span data-stu-id="380c2-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="380c2-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="380c2-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="380c2-217">Předává směrovány přes proxy server hlavičky do aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="380c2-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="380c2-218">Před provedením komponent, které využívají aktualizovaná pole (příklady: schéma, hostitele, když, metoda).</span><span class="sxs-lookup"><span data-stu-id="380c2-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="380c2-219">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="380c2-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="380c2-220">Poskytuje podporu pro ukládání do mezipaměti odpovědi.</span><span class="sxs-lookup"><span data-stu-id="380c2-220">Provides support for caching responses.</span></span> | <span data-ttu-id="380c2-221">Před provedením komponent, které vyžadují ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="380c2-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="380c2-222">Komprese odpovědi</span><span class="sxs-lookup"><span data-stu-id="380c2-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="380c2-223">Poskytuje podporu pro kompresi odpovědí.</span><span class="sxs-lookup"><span data-stu-id="380c2-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="380c2-224">Před provedením komponent, které vyžadují komprese.</span><span class="sxs-lookup"><span data-stu-id="380c2-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="380c2-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="380c2-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="380c2-226">Poskytuje podporu lokalizace.</span><span class="sxs-lookup"><span data-stu-id="380c2-226">Provides localization support.</span></span> | <span data-ttu-id="380c2-227">Před provedením komponent citlivé lokalizace.</span><span class="sxs-lookup"><span data-stu-id="380c2-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="380c2-228">Směrování</span><span class="sxs-lookup"><span data-stu-id="380c2-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="380c2-229">Definuje a omezí požadavek trasy.</span><span class="sxs-lookup"><span data-stu-id="380c2-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="380c2-230">Terminálu odpovídající trasy.</span><span class="sxs-lookup"><span data-stu-id="380c2-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="380c2-231">Relace</span><span class="sxs-lookup"><span data-stu-id="380c2-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="380c2-232">Poskytuje podporu pro správu uživatelských relací.</span><span class="sxs-lookup"><span data-stu-id="380c2-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="380c2-233">Před provedením komponent, které vyžadují relace.</span><span class="sxs-lookup"><span data-stu-id="380c2-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="380c2-234">Statické soubory</span><span class="sxs-lookup"><span data-stu-id="380c2-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="380c2-235">Poskytuje podporu pro obsluhující statické soubory a procházení adresářů.</span><span class="sxs-lookup"><span data-stu-id="380c2-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="380c2-236">Terminál, pokud požadavek odpovídá soubory.</span><span class="sxs-lookup"><span data-stu-id="380c2-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="380c2-237">Přepisování adres URL </span><span class="sxs-lookup"><span data-stu-id="380c2-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="380c2-238">Poskytuje podporu pro přepisování adres URL a přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="380c2-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="380c2-239">Před provedením komponent, které využívají adresu URL.</span><span class="sxs-lookup"><span data-stu-id="380c2-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="380c2-240">Webové sokety</span><span class="sxs-lookup"><span data-stu-id="380c2-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="380c2-241">Umožňuje protokol Websocket.</span><span class="sxs-lookup"><span data-stu-id="380c2-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="380c2-242">Před provedením komponent, které jsou nutné k přijímání požadavků protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="380c2-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="380c2-243">Zápis middlewaru</span><span class="sxs-lookup"><span data-stu-id="380c2-243">Writing middleware</span></span>

<span data-ttu-id="380c2-244">Middleware je obecně zapouzdřené v třídě a vystavené pomocí metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="380c2-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="380c2-245">Vezměte v úvahu následující middlewaru, který nastaví jazykovou verzi pro aktuální požadavek z řetězce dotazu:</span><span class="sxs-lookup"><span data-stu-id="380c2-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="380c2-246">Poznámka: Výše uvedeném ukázkovém kódu se používá k předvedení vytváření komponenta middlewaru.</span><span class="sxs-lookup"><span data-stu-id="380c2-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="380c2-247">V tématu [ globalizace a lokalizace](xref:fundamentals/localization) pro podporu předdefinované lokalizace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="380c2-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="380c2-248">Middleware můžete otestovat pomocí předání v jazykovou verzi, například `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="380c2-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="380c2-249">Následující kód přesune delegáta middleware na třídu:</span><span class="sxs-lookup"><span data-stu-id="380c2-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

> [!NOTE]
> <span data-ttu-id="380c2-250">V ASP.NET Core 1.x, middleware `Task` název metody musí být `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="380c2-250">In ASP.NET Core 1.x, the middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="380c2-251">V technologii ASP.NET Core 2.0 nebo novější, název může být buď `Invoke` nebo `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="380c2-251">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

<span data-ttu-id="380c2-252">Zpřístupní metodu rozšíření middleware prostřednictvím [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="380c2-252">The following extension method exposes the middleware through [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="380c2-253">Následující kód volá middleware z `Configure`:</span><span class="sxs-lookup"><span data-stu-id="380c2-253">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="380c2-254">Postupujte podle middleware [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/) díky zpřístupnění jeho závislé součásti v jeho konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="380c2-254">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="380c2-255">Middleware je vytvořený jednou za *životního cyklu aplikace*.</span><span class="sxs-lookup"><span data-stu-id="380c2-255">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="380c2-256">V tématu *požadavků závislosti* níže v případě, je potřeba sdílet s middlewaru v rámci žádost o služby.</span><span class="sxs-lookup"><span data-stu-id="380c2-256">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="380c2-257">Komponenty middlewaru lze vyřešit závislé z vkládání závislostí prostřednictvím parametrů konstruktor.</span><span class="sxs-lookup"><span data-stu-id="380c2-257">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="380c2-258">[`UseMiddleware<T>`](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) Můžete také přijímat další parametry, přímo.</span><span class="sxs-lookup"><span data-stu-id="380c2-258">[`UseMiddleware<T>`](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="380c2-259">Závislosti na žádost</span><span class="sxs-lookup"><span data-stu-id="380c2-259">Per-request dependencies</span></span>

<span data-ttu-id="380c2-260">Protože middlewaru je vytvořený při spuštění aplikace, není požadavků, *obor* služby životního cyklu, které middleware konstruktory používá nejsou sdílené s jinými typy vložit závislostí při každé žádosti.</span><span class="sxs-lookup"><span data-stu-id="380c2-260">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="380c2-261">Pokud je nutné sdílet *obor* služby mezi vlastního middlewaru a jinými typy, přidejte tyto služby `Invoke` podpis metody.</span><span class="sxs-lookup"><span data-stu-id="380c2-261">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="380c2-262">`Invoke` Metoda může přijímat další parametry, které jsou naplněny pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="380c2-262">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="380c2-263">Příklad:</span><span class="sxs-lookup"><span data-stu-id="380c2-263">For example:</span></span>

```csharp
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

## <a name="additional-resources"></a><span data-ttu-id="380c2-264">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="380c2-264">Additional resources</span></span>

* [<span data-ttu-id="380c2-265">Migrovat vytváření modulů HTTP v middlewaru.</span><span class="sxs-lookup"><span data-stu-id="380c2-265">Migrate HTTP Modules to Middleware</span></span>](xref:migration/http-modules)
* [<span data-ttu-id="380c2-266">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="380c2-266">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="380c2-267">Funkce požadavků</span><span class="sxs-lookup"><span data-stu-id="380c2-267">Request Features</span></span>](xref:fundamentals/request-features)
* [<span data-ttu-id="380c2-268">Aktivace na základě Factory middlewaru</span><span class="sxs-lookup"><span data-stu-id="380c2-268">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="380c2-269">Middleware aktivaci s použitím kontejner třetích stran</span><span class="sxs-lookup"><span data-stu-id="380c2-269">Middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
