---
title: Middleware ASP.NET Core
author: rick-anderson
description: Další informace o ASP.NET Core middleware a kanál žádosti.
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: 9ba77561ab4f6a8668c480d6e81f2ce7e0193c73
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870944"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="791aa-103">Middleware ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="791aa-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="791aa-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="791aa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="791aa-105">Middleware je software, který je sestaven do kanálu služby aplikace pro zpracování požadavků a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="791aa-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="791aa-106">Jednotlivé komponenty:</span><span class="sxs-lookup"><span data-stu-id="791aa-106">Each component:</span></span>

* <span data-ttu-id="791aa-107">Zvolí, zda se má předat požadavky na další komponenta v kanálu.</span><span class="sxs-lookup"><span data-stu-id="791aa-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="791aa-108">Práci můžete provádět, před a po zavolání další komponenta v kanálu.</span><span class="sxs-lookup"><span data-stu-id="791aa-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="791aa-109">Delegáti požadavku se používají k vytvoření kanálu požadavku.</span><span class="sxs-lookup"><span data-stu-id="791aa-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="791aa-110">Delegáti žádost o zpracování konkrétního požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="791aa-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="791aa-111">Požádat o Delegáti jsou nakonfigurováni pomocí <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, a <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="791aa-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="791aa-112">Delegát jednotlivých požadavků může být zadaný řádek jako anonymní metody (označované jako middlewaru vložených), nebo může být definován ve třídě opakovaně použitelné.</span><span class="sxs-lookup"><span data-stu-id="791aa-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="791aa-113">Tyto opakovaně použitelné třídy a v řádku anonymní metody jsou *middleware*, označované také jako *middlewarových komponent*.</span><span class="sxs-lookup"><span data-stu-id="791aa-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="791aa-114">Jednotlivé komponenty middleware v kanálu požadavku zodpovídá za vyvolání další komponenta v kanálu nebo zkrácenou kanálu.</span><span class="sxs-lookup"><span data-stu-id="791aa-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="791aa-115"><xref:migration/http-modules> Vysvětluje rozdíl mezi požadavek kanály v ASP.NET Core a ASP.NET 4.x a poskytuje další ukázky middlewaru.</span><span class="sxs-lookup"><span data-stu-id="791aa-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="791aa-116">Vytvoření kanálu middlewaru s IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="791aa-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="791aa-117">Kanál žádosti ASP.NET Core se skládá z posloupnost požadavek delegáty, volá se jedna po druhé.</span><span class="sxs-lookup"><span data-stu-id="791aa-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="791aa-118">Následující diagram ukazuje koncept.</span><span class="sxs-lookup"><span data-stu-id="791aa-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="791aa-119">Vlákno provádění postupuje černé šipky.</span><span class="sxs-lookup"><span data-stu-id="791aa-119">The thread of execution follows the black arrows.</span></span>

![Vzor zpracování požadavku zobrazující žádosti přicházející, zpracování až tři middlewares a odpovědi opuštění aplikace.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="791aa-123">Všem delegátům můžete provádět operace, před a po dalším delegáta.</span><span class="sxs-lookup"><span data-stu-id="791aa-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="791aa-124">Také můžete rozhodnout delegáta úspěšná žádost o další delegátem, která se nazývá *zkrácenou kanál žádosti*.</span><span class="sxs-lookup"><span data-stu-id="791aa-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="791aa-125">Krátký cyklus je často žádoucí, protože tím eliminujete zbytečné práce.</span><span class="sxs-lookup"><span data-stu-id="791aa-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="791aa-126">Statické soubory Middleware můžete například vrátit žádost o statický soubor a zkrácenou rest z kanálu.</span><span class="sxs-lookup"><span data-stu-id="791aa-126">For example, Static Files Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="791aa-127">Delegáti zpracování výjimek se nazývají již v rané fázi v kanálu, takže se můžete zachytit výjimky, ke kterým dochází v pozdějších fázích kanálu.</span><span class="sxs-lookup"><span data-stu-id="791aa-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="791aa-128">Nejjednodušší možný aplikace ASP.NET Core nastaví delegáta jedné žádosti, která zpracovává všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="791aa-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="791aa-129">Tento případ neobsahuje kanál aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="791aa-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="791aa-130">Místo toho jednoho anonymní funkce je volána v reakci na každý požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="791aa-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="791aa-131">První <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegáta ukončí kanálu.</span><span class="sxs-lookup"><span data-stu-id="791aa-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="791aa-132">Zřetězit více požadavek delegátů spolu s <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="791aa-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="791aa-133">`next` Parametr představuje další delegáta v kanálu.</span><span class="sxs-lookup"><span data-stu-id="791aa-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="791aa-134">Můžete zkrácenou kanál pomocí *není* volání *Další* parametru.</span><span class="sxs-lookup"><span data-stu-id="791aa-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="791aa-135">Můžete obvykle provádět akce před i po další delegáta, jak ukazuje následující příklad:</span><span class="sxs-lookup"><span data-stu-id="791aa-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="791aa-136">Nevolejte `next.Invoke` po odeslání odpovědi klientovi.</span><span class="sxs-lookup"><span data-stu-id="791aa-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="791aa-137">Změny <xref:Microsoft.AspNetCore.Http.HttpResponse> po spuštění odpovědi vrátit výjimku.</span><span class="sxs-lookup"><span data-stu-id="791aa-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="791aa-138">Například změny, jako je nastavení hlaviček a stavovým kódem vyvolat výjimku.</span><span class="sxs-lookup"><span data-stu-id="791aa-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="791aa-139">Zápis do datové části odpovědi po volání `next`:</span><span class="sxs-lookup"><span data-stu-id="791aa-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="791aa-140">Může způsobit narušení protokolu.</span><span class="sxs-lookup"><span data-stu-id="791aa-140">May cause a protocol violation.</span></span> <span data-ttu-id="791aa-141">Například zápis informace než uvedené `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="791aa-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="791aa-142">Může dojít k poškození formát těla zprávy.</span><span class="sxs-lookup"><span data-stu-id="791aa-142">May corrupt the body format.</span></span> <span data-ttu-id="791aa-143">HTML zápatí například zápis do souboru šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="791aa-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="791aa-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> je užitečné nápovědu k označení, pokud byly odeslány hlavičky nebo text byl zapsán do.</span><span class="sxs-lookup"><span data-stu-id="791aa-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="791aa-145">Pořadí</span><span class="sxs-lookup"><span data-stu-id="791aa-145">Order</span></span>

<span data-ttu-id="791aa-146">Pořadí, ve kterém middlewarových komponent přidány `Startup.Configure` metoda definuje pořadí, ve kterém jsou vyvolány middlewarových komponent na požadavky a obrácenému pořadí pro odpověď.</span><span class="sxs-lookup"><span data-stu-id="791aa-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="791aa-147">Pořadí je důležité pro zabezpečení, výkon a funkčnost.</span><span class="sxs-lookup"><span data-stu-id="791aa-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="791aa-148">Následující `Configure` metoda přidá následující middlewarových komponent:</span><span class="sxs-lookup"><span data-stu-id="791aa-148">The following `Configure` method adds the following middleware components:</span></span>

1. <span data-ttu-id="791aa-149">Zpracování výjimek a chyb</span><span class="sxs-lookup"><span data-stu-id="791aa-149">Exception/error handling</span></span>
2. <span data-ttu-id="791aa-150">Statický souborový server</span><span class="sxs-lookup"><span data-stu-id="791aa-150">Static file server</span></span>
3. <span data-ttu-id="791aa-151">Ověřování</span><span class="sxs-lookup"><span data-stu-id="791aa-151">Authentication</span></span>
4. <span data-ttu-id="791aa-152">MVC</span><span class="sxs-lookup"><span data-stu-id="791aa-152">MVC</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    //   Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="791aa-153">Ve výše uvedeném kódu <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> je první komponenta middlewaru přidané do kanálu.</span><span class="sxs-lookup"><span data-stu-id="791aa-153">In the code above, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="791aa-154">Proto Middleware obslužné rutiny výjimek zachytává všechny výjimky, ke kterým dochází v pozdější volání.</span><span class="sxs-lookup"><span data-stu-id="791aa-154">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="791aa-155">Statické soubory middlewaru je volána v rané fázi kanálu tak, aby mohl zpracovávat požadavky a zkrácenou bez nutnosti kontaktovat zbývající součásti.</span><span class="sxs-lookup"><span data-stu-id="791aa-155">Static Files Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="791aa-156">Statické soubory Middleware poskytuje **žádné** kontroly autorizace.</span><span class="sxs-lookup"><span data-stu-id="791aa-156">The Static Files Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="791aa-157">Všechny soubory obsluhuje, včetně těch *wwwroot*, jsou veřejně dostupné.</span><span class="sxs-lookup"><span data-stu-id="791aa-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="791aa-158">Přístup k zabezpečení statické soubory, naleznete v tématu <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="791aa-158">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="791aa-159">Pokud žádost není zpracovávaných middlewarem statické soubory, je předána Middleware ověřování (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), který provádí ověřování.</span><span class="sxs-lookup"><span data-stu-id="791aa-159">If the request isn't handled by the Static Files Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="791aa-160">Ověřování není zkrácenou neověřené žádosti.</span><span class="sxs-lookup"><span data-stu-id="791aa-160">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="791aa-161">I když ověřovací Middleware ověřuje požadavky, autorizace (a odmítnutí) nastane pouze po MVC vybere konkrétní stránky Razor nebo MVC kontroleru a akce.</span><span class="sxs-lookup"><span data-stu-id="791aa-161">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="791aa-162">Pokud požadavek není zpracováván statické soubory Middleware, je předána Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), který provádí ověřování.</span><span class="sxs-lookup"><span data-stu-id="791aa-162">If the request isn't handled by Static Files Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="791aa-163">Identita není zkrácenou neověřené žádosti.</span><span class="sxs-lookup"><span data-stu-id="791aa-163">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="791aa-164">I když žádosti o ověření Identity autorizace (a odmítnutí) nastane pouze po MVC vybere konkrétní kontroleru a akce.</span><span class="sxs-lookup"><span data-stu-id="791aa-164">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="791aa-165">Následující příklad ukazuje middleware pořadí, ve kterém jsou požadavky pro statické soubory zpracovávány middlewarem statické soubory před Middleware pro kompresi odpovědí.</span><span class="sxs-lookup"><span data-stu-id="791aa-165">The following example demonstrates a middleware order where requests for static files are handled by Static Files Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="791aa-166">Statické soubory nejsou komprimovaný pomocí tohoto pořadí middlewaru.</span><span class="sxs-lookup"><span data-stu-id="791aa-166">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="791aa-167">Odpovědi MVC <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> lze komprimovat.</span><span class="sxs-lookup"><span data-stu-id="791aa-167">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="791aa-168">Použití, spuštění a mapy</span><span class="sxs-lookup"><span data-stu-id="791aa-168">Use, Run, and Map</span></span>

<span data-ttu-id="791aa-169">Konfigurace kanálu HTTP pomocí `Use`, `Run`, a `Map`.</span><span class="sxs-lookup"><span data-stu-id="791aa-169">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="791aa-170">`Use` Metoda můžete zkrácenou kanálu (tj. Pokud nebude volat `next` delegáta požadavku).</span><span class="sxs-lookup"><span data-stu-id="791aa-170">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="791aa-171">`Run` je konvence, a některé middlewarových komponent může vystavit `Run[Middleware]` metody, které běží na konci kanálu.</span><span class="sxs-lookup"><span data-stu-id="791aa-171">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="791aa-172"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> rozšíření jsou použity jako konvence pro větvení kanálu.</span><span class="sxs-lookup"><span data-stu-id="791aa-172"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="791aa-173">`Map*` větve kanál požadavku na základě shody zadanou cestu požadavku.</span><span class="sxs-lookup"><span data-stu-id="791aa-173">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="791aa-174">Pokud cesta požadavku začíná dané cesty, větve je proveden.</span><span class="sxs-lookup"><span data-stu-id="791aa-174">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="791aa-175">V následující tabulce jsou uvedeny požadavky a odpovědi z `http://localhost:1234` pomocí předchozího kódu.</span><span class="sxs-lookup"><span data-stu-id="791aa-175">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="791aa-176">Požadavek</span><span class="sxs-lookup"><span data-stu-id="791aa-176">Request</span></span>             | <span data-ttu-id="791aa-177">Odpověď</span><span class="sxs-lookup"><span data-stu-id="791aa-177">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="791aa-178">1234</span><span class="sxs-lookup"><span data-stu-id="791aa-178">localhost:1234</span></span>      | <span data-ttu-id="791aa-179">Dobrý den ze bez mapování delegáta.</span><span class="sxs-lookup"><span data-stu-id="791aa-179">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="791aa-180">1234 / map1</span><span class="sxs-lookup"><span data-stu-id="791aa-180">localhost:1234/map1</span></span> | <span data-ttu-id="791aa-181">Mapování Test 1</span><span class="sxs-lookup"><span data-stu-id="791aa-181">Map Test 1</span></span>                   |
| <span data-ttu-id="791aa-182">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="791aa-182">localhost:1234/map2</span></span> | <span data-ttu-id="791aa-183">Mapování testů 2</span><span class="sxs-lookup"><span data-stu-id="791aa-183">Map Test 2</span></span>                   |
| <span data-ttu-id="791aa-184">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="791aa-184">localhost:1234/map3</span></span> | <span data-ttu-id="791aa-185">Dobrý den ze bez mapování delegáta.</span><span class="sxs-lookup"><span data-stu-id="791aa-185">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="791aa-186">Když `Map` se používá, segmenty cesty odpovídající jsou odebrány z `HttpRequest.Path` a připojený k `HttpRequest.PathBase` pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="791aa-186">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="791aa-187">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) větví kanál požadavku na základě výsledku daného predikátu.</span><span class="sxs-lookup"><span data-stu-id="791aa-187">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="791aa-188">Žádné predikátu typu `Func<HttpContext, bool>` je možné mapovat požadavky na novou větev kanálu.</span><span class="sxs-lookup"><span data-stu-id="791aa-188">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="791aa-189">V následujícím příkladu, predikát slouží k detekci přítomnosti proměnnou s řetězcem dotazu `branch`:</span><span class="sxs-lookup"><span data-stu-id="791aa-189">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="791aa-190">V následující tabulce jsou uvedeny požadavky a odpovědi z `http://localhost:1234` pomocí předchozího kódu.</span><span class="sxs-lookup"><span data-stu-id="791aa-190">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="791aa-191">Požadavek</span><span class="sxs-lookup"><span data-stu-id="791aa-191">Request</span></span>                       | <span data-ttu-id="791aa-192">Odpověď</span><span class="sxs-lookup"><span data-stu-id="791aa-192">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="791aa-193">1234</span><span class="sxs-lookup"><span data-stu-id="791aa-193">localhost:1234</span></span>                | <span data-ttu-id="791aa-194">Dobrý den ze bez mapování delegáta.</span><span class="sxs-lookup"><span data-stu-id="791aa-194">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="791aa-195">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="791aa-195">localhost:1234/?branch=master</span></span> | <span data-ttu-id="791aa-196">Větev se použije hlavní =</span><span class="sxs-lookup"><span data-stu-id="791aa-196">Branch used = master</span></span>         |

<span data-ttu-id="791aa-197">`Map` podporuje vnořené, například:</span><span class="sxs-lookup"><span data-stu-id="791aa-197">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

<span data-ttu-id="791aa-198">`Map` Můžete také porovnat více segmentů najednou:</span><span class="sxs-lookup"><span data-stu-id="791aa-198">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="791aa-199">Integrované middlewaru</span><span class="sxs-lookup"><span data-stu-id="791aa-199">Built-in middleware</span></span>

<span data-ttu-id="791aa-200">ASP.NET Core se dodává s následujícími součástmi middlewaru.</span><span class="sxs-lookup"><span data-stu-id="791aa-200">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="791aa-201">*Pořadí* sloupec obsahuje poznámky k umístění middleware v kanálu požadavku a za jakých podmínek middleware může ukončit žádosti a zabránit dalším middlewarem zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="791aa-201">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="791aa-202">Middleware</span><span class="sxs-lookup"><span data-stu-id="791aa-202">Middleware</span></span> | <span data-ttu-id="791aa-203">Popis</span><span class="sxs-lookup"><span data-stu-id="791aa-203">Description</span></span> | <span data-ttu-id="791aa-204">Pořadí</span><span class="sxs-lookup"><span data-stu-id="791aa-204">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="791aa-205">Ověřování</span><span class="sxs-lookup"><span data-stu-id="791aa-205">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="791aa-206">Poskytuje podporu ověřování.</span><span class="sxs-lookup"><span data-stu-id="791aa-206">Provides authentication support.</span></span> | <span data-ttu-id="791aa-207">Před `HttpContext.User` je potřeba.</span><span class="sxs-lookup"><span data-stu-id="791aa-207">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="791aa-208">Terminál pro zpětná volání OAuth.</span><span class="sxs-lookup"><span data-stu-id="791aa-208">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="791aa-209">CORS</span><span class="sxs-lookup"><span data-stu-id="791aa-209">CORS</span></span>](xref:security/cors) | <span data-ttu-id="791aa-210">Konfiguruje sdílení prostředků různého původu.</span><span class="sxs-lookup"><span data-stu-id="791aa-210">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="791aa-211">Před provedením komponent, které používají CORS.</span><span class="sxs-lookup"><span data-stu-id="791aa-211">Before components that use CORS.</span></span> |
| [<span data-ttu-id="791aa-212">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="791aa-212">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="791aa-213">Konfiguruje se Diagnostika.</span><span class="sxs-lookup"><span data-stu-id="791aa-213">Configures diagnostics.</span></span> | <span data-ttu-id="791aa-214">Před provedením komponent, které generují chyby.</span><span class="sxs-lookup"><span data-stu-id="791aa-214">Before components that generate errors.</span></span> |
| [<span data-ttu-id="791aa-215">Přesměrovaná záhlaví</span><span class="sxs-lookup"><span data-stu-id="791aa-215">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="791aa-216">Předává záhlaví směrovány přes proxy server na aktuální žádost.</span><span class="sxs-lookup"><span data-stu-id="791aa-216">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="791aa-217">Před provedením komponent, které využívají aktualizovanými poli (příklady: schéma, hostitele, IP adresu klienta, metoda).</span><span class="sxs-lookup"><span data-stu-id="791aa-217">Before components that consume the updated fields (examples: scheme, host, client IP, method).</span></span> |
| [<span data-ttu-id="791aa-218">Přepsání metody HTTP</span><span class="sxs-lookup"><span data-stu-id="791aa-218">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="791aa-219">Umožňuje příchozí požadavek POST k přepsání metody.</span><span class="sxs-lookup"><span data-stu-id="791aa-219">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="791aa-220">Před provedením komponent, které využívají aktualizované metody.</span><span class="sxs-lookup"><span data-stu-id="791aa-220">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="791aa-221">Přesměrování protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="791aa-221">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="791aa-222">Přesměrujte všechny požadavky HTTP na HTTPS (ASP.NET Core 2.1 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="791aa-222">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="791aa-223">Před provedením komponent, které využívají adresu URL.</span><span class="sxs-lookup"><span data-stu-id="791aa-223">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="791aa-224">Zabezpečení striktní přenosu HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="791aa-224">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="791aa-225">Zabezpečení vylepšení middleware, který přidá hlavičku odpovědi speciální (ASP.NET Core 2.1 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="791aa-225">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="791aa-226">Předtím, než se posílají žádosti a po součásti, které mění žádosti (například předávat záhlaví, přepisování adres URL).</span><span class="sxs-lookup"><span data-stu-id="791aa-226">Before responses are sent and after components that modify requests (for example, Forwarded Headers, URL Rewriting).</span></span> |
| [<span data-ttu-id="791aa-227">MVC</span><span class="sxs-lookup"><span data-stu-id="791aa-227">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="791aa-228">Zpracuje žádosti s MVC/Razor Pages (ASP.NET Core 2.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="791aa-228">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="791aa-229">Terminál, pokud požadavek odpovídá trase.</span><span class="sxs-lookup"><span data-stu-id="791aa-229">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="791aa-230">OWIN</span><span class="sxs-lookup"><span data-stu-id="791aa-230">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="791aa-231">Interoperabilita s aplikací OWIN, servery a middlewaru.</span><span class="sxs-lookup"><span data-stu-id="791aa-231">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="791aa-232">Terminál, pokud middlewaru OWIN, který plně zpracuje požadavek.</span><span class="sxs-lookup"><span data-stu-id="791aa-232">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="791aa-233">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="791aa-233">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="791aa-234">Poskytuje podporu pro ukládání do mezipaměti odpovědi.</span><span class="sxs-lookup"><span data-stu-id="791aa-234">Provides support for caching responses.</span></span> | <span data-ttu-id="791aa-235">Před provedením komponent, které vyžadují ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="791aa-235">Before components that require caching.</span></span> |
| [<span data-ttu-id="791aa-236">Kompresi odpovědí</span><span class="sxs-lookup"><span data-stu-id="791aa-236">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="791aa-237">Poskytuje podporu pro kompresi odpovědí.</span><span class="sxs-lookup"><span data-stu-id="791aa-237">Provides support for compressing responses.</span></span> | <span data-ttu-id="791aa-238">Před provedením komponent, které vyžadují komprese.</span><span class="sxs-lookup"><span data-stu-id="791aa-238">Before components that require compression.</span></span> |
| [<span data-ttu-id="791aa-239">Žádost o lokalizaci</span><span class="sxs-lookup"><span data-stu-id="791aa-239">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="791aa-240">Poskytuje podporu lokalizace.</span><span class="sxs-lookup"><span data-stu-id="791aa-240">Provides localization support.</span></span> | <span data-ttu-id="791aa-241">Před provedením komponent citlivé lokalizace.</span><span class="sxs-lookup"><span data-stu-id="791aa-241">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="791aa-242">Směrování</span><span class="sxs-lookup"><span data-stu-id="791aa-242">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="791aa-243">Definuje a omezuje žádosti trasy.</span><span class="sxs-lookup"><span data-stu-id="791aa-243">Defines and constrains request routes.</span></span> | <span data-ttu-id="791aa-244">Terminál odpovídající trasy.</span><span class="sxs-lookup"><span data-stu-id="791aa-244">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="791aa-245">Relace</span><span class="sxs-lookup"><span data-stu-id="791aa-245">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="791aa-246">Poskytuje podporu pro správu uživatelských relací.</span><span class="sxs-lookup"><span data-stu-id="791aa-246">Provides support for managing user sessions.</span></span> | <span data-ttu-id="791aa-247">Před provedením komponent, které vyžadují relace.</span><span class="sxs-lookup"><span data-stu-id="791aa-247">Before components that require Session.</span></span> |
| [<span data-ttu-id="791aa-248">Statické soubory</span><span class="sxs-lookup"><span data-stu-id="791aa-248">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="791aa-249">Poskytuje podporu pro poskytování statických souborů a procházení adresářů.</span><span class="sxs-lookup"><span data-stu-id="791aa-249">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="791aa-250">Terminál, pokud požadavek odpovídá souboru.</span><span class="sxs-lookup"><span data-stu-id="791aa-250">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="791aa-251">Přepisování adres URL</span><span class="sxs-lookup"><span data-stu-id="791aa-251">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="791aa-252">Poskytuje podporu pro přepis adres URL a přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="791aa-252">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="791aa-253">Před provedením komponent, které využívají adresu URL.</span><span class="sxs-lookup"><span data-stu-id="791aa-253">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="791aa-254">Webové sokety</span><span class="sxs-lookup"><span data-stu-id="791aa-254">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="791aa-255">Povolí protokol Websocket.</span><span class="sxs-lookup"><span data-stu-id="791aa-255">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="791aa-256">Před provedením komponent, které jsou nutné, aby přijímal požadavky protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="791aa-256">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="791aa-257">Zápis middlewaru</span><span class="sxs-lookup"><span data-stu-id="791aa-257">Write middleware</span></span>

<span data-ttu-id="791aa-258">Middleware je obecně zapouzdřené v třídě a vystavený s metodou rozšíření.</span><span class="sxs-lookup"><span data-stu-id="791aa-258">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="791aa-259">Vezměte v úvahu následující middlewaru, který nastaví jazykovou verzi pro aktuální požadavek z řetězce dotazu:</span><span class="sxs-lookup"><span data-stu-id="791aa-259">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="791aa-260">Předchozí kód ukázkové slouží k předvedení vytváření komponenta middlewaru.</span><span class="sxs-lookup"><span data-stu-id="791aa-260">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="791aa-261">ASP.NET Core lokalizace integrovanou podporu najdete na webu <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="791aa-261">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="791aa-262">Middleware můžete otestovat tím, že předáte v jazykové verzi, například `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="791aa-262">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="791aa-263">Následující kód přesune delegáta middleware pro třídu:</span><span class="sxs-lookup"><span data-stu-id="791aa-263">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="791aa-264">Middleware `Task` název metody musí být `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="791aa-264">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="791aa-265">V technologii ASP.NET Core 2.0 nebo novější, název může být buď `Invoke` nebo `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="791aa-265">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="791aa-266">Poskytuje následující metody rozšíření middleware prostřednictvím <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="791aa-266">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="791aa-267">Následující kód volá middleware z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="791aa-267">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="791aa-268">Postupujte podle middleware [explicitní závislosti Princip](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) zveřejněním závislých 've svém konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="791aa-268">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="791aa-269">Middleware je vytvořen jednou za *dobu životnosti aplikace*.</span><span class="sxs-lookup"><span data-stu-id="791aa-269">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="791aa-270">Najdete v článku [závislosti na požadavku](#per-request-dependencies) části, pokud je potřeba sdílet s middlewaru v rámci žádost o služby.</span><span class="sxs-lookup"><span data-stu-id="791aa-270">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="791aa-271">Middlewarových komponent lze vyřešit jejich závislosti z [injektáž závislostí (DI)](xref:fundamentals/dependency-injection) prostřednictvím parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="791aa-271">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="791aa-272">[UseMiddleware&lt;T&gt; ](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) můžete také přijmout přímo další parametry.</span><span class="sxs-lookup"><span data-stu-id="791aa-272">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="791aa-273">Závislosti na základě žádosti</span><span class="sxs-lookup"><span data-stu-id="791aa-273">Per-request dependencies</span></span>

<span data-ttu-id="791aa-274">Protože middlewaru je vytvořen při spuštění aplikace, nikoli jednotlivých žádostí, *obor* služby životního cyklu používat middleware konstruktory nejsou sdíleny s jinými typy vložený závislostí při každém požadavku.</span><span class="sxs-lookup"><span data-stu-id="791aa-274">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="791aa-275">Pokud musíte sdílet *obor* služby mezi middlewaru a ostatními typy, přidejte tyto služby `Invoke` podpis metody.</span><span class="sxs-lookup"><span data-stu-id="791aa-275">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="791aa-276">`Invoke` Metoda může přijímat další parametry, které se vyplní podle DI:</span><span class="sxs-lookup"><span data-stu-id="791aa-276">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="791aa-277">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="791aa-277">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
