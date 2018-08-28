---
title: Middleware ASP.NET Core
author: rick-anderson
description: Další informace o ASP.NET Core middleware a kanál žádosti.
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: e6dc76b7cb80e0dfda102df5aefb5d9ce9b821ed
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055803"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="31453-103">Middleware ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31453-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="31453-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="31453-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="31453-105">Middleware je software, který je sestaven do kanálu služby aplikace pro zpracování požadavků a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="31453-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="31453-106">Jednotlivé komponenty:</span><span class="sxs-lookup"><span data-stu-id="31453-106">Each component:</span></span>

* <span data-ttu-id="31453-107">Zvolí, zda se má předat požadavky na další komponenta v kanálu.</span><span class="sxs-lookup"><span data-stu-id="31453-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="31453-108">Práci můžete provádět, před a po zavolání další komponenta v kanálu.</span><span class="sxs-lookup"><span data-stu-id="31453-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="31453-109">Delegáti požadavku se používají k vytvoření kanálu požadavku.</span><span class="sxs-lookup"><span data-stu-id="31453-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="31453-110">Delegáti žádost o zpracování konkrétního požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="31453-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="31453-111">Požádat o Delegáti jsou nakonfigurováni pomocí <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, a <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="31453-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="31453-112">Delegát jednotlivých požadavků může být zadaný řádek jako anonymní metody (označované jako middlewaru vložených), nebo může být definován ve třídě opakovaně použitelné.</span><span class="sxs-lookup"><span data-stu-id="31453-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="31453-113">Tyto opakovaně použitelné třídy a v řádku anonymní metody jsou *middleware*, označované také jako *middlewarových komponent*.</span><span class="sxs-lookup"><span data-stu-id="31453-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="31453-114">Jednotlivé komponenty middleware v kanálu požadavku zodpovídá za vyvolání další komponenta v kanálu nebo zkrácenou kanálu.</span><span class="sxs-lookup"><span data-stu-id="31453-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="31453-115"><xref:migration/http-modules> Vysvětluje rozdíl mezi požadavek kanály v ASP.NET Core a ASP.NET 4.x a poskytuje další ukázky middlewaru.</span><span class="sxs-lookup"><span data-stu-id="31453-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="31453-116">Vytvoření kanálu middlewaru s IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="31453-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="31453-117">Kanál žádosti ASP.NET Core se skládá z posloupnost požadavek delegáty, volá se jedna po druhé.</span><span class="sxs-lookup"><span data-stu-id="31453-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="31453-118">Následující diagram ukazuje koncept.</span><span class="sxs-lookup"><span data-stu-id="31453-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="31453-119">Vlákno provádění postupuje černé šipky.</span><span class="sxs-lookup"><span data-stu-id="31453-119">The thread of execution follows the black arrows.</span></span>

![Vzor zpracování požadavku zobrazující žádosti přicházející, zpracování až tři middlewares a odpovědi opuštění aplikace.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="31453-123">Všem delegátům můžete provádět operace, před a po dalším delegáta.</span><span class="sxs-lookup"><span data-stu-id="31453-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="31453-124">Také můžete rozhodnout delegáta úspěšná žádost o další delegátem, která se nazývá *zkrácenou kanál žádosti*.</span><span class="sxs-lookup"><span data-stu-id="31453-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="31453-125">Krátký cyklus je často žádoucí, protože tím eliminujete zbytečné práce.</span><span class="sxs-lookup"><span data-stu-id="31453-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="31453-126">Statické soubory Middleware můžete například vrátit žádost o statický soubor a zkrácenou rest z kanálu.</span><span class="sxs-lookup"><span data-stu-id="31453-126">For example, Static Files Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="31453-127">Delegáti zpracování výjimek se nazývají již v rané fázi v kanálu, takže se můžete zachytit výjimky, ke kterým dochází v pozdějších fázích kanálu.</span><span class="sxs-lookup"><span data-stu-id="31453-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="31453-128">Nejjednodušší možný aplikace ASP.NET Core nastaví delegáta jedné žádosti, která zpracovává všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="31453-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="31453-129">Tento případ neobsahuje kanál aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="31453-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="31453-130">Místo toho jednoho anonymní funkce je volána v reakci na každý požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="31453-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="31453-131">První <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegáta ukončí kanálu.</span><span class="sxs-lookup"><span data-stu-id="31453-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="31453-132">Zřetězit více požadavek delegátů spolu s <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="31453-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="31453-133">`next` Parametr představuje další delegáta v kanálu.</span><span class="sxs-lookup"><span data-stu-id="31453-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="31453-134">Můžete zkrácenou kanál pomocí *není* volání *Další* parametru.</span><span class="sxs-lookup"><span data-stu-id="31453-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="31453-135">Můžete obvykle provádět akce před i po další delegáta, jak ukazuje následující příklad:</span><span class="sxs-lookup"><span data-stu-id="31453-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="31453-136">Nevolejte `next.Invoke` po odeslání odpovědi klientovi.</span><span class="sxs-lookup"><span data-stu-id="31453-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="31453-137">Změny <xref:Microsoft.AspNetCore.Http.HttpResponse> po spuštění odpovědi vrátit výjimku.</span><span class="sxs-lookup"><span data-stu-id="31453-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="31453-138">Například změny, jako je nastavení hlaviček a stavovým kódem vyvolat výjimku.</span><span class="sxs-lookup"><span data-stu-id="31453-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="31453-139">Zápis do datové části odpovědi po volání `next`:</span><span class="sxs-lookup"><span data-stu-id="31453-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="31453-140">Může způsobit narušení protokolu.</span><span class="sxs-lookup"><span data-stu-id="31453-140">May cause a protocol violation.</span></span> <span data-ttu-id="31453-141">Například zápis informace než uvedené `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="31453-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="31453-142">Může dojít k poškození formát těla zprávy.</span><span class="sxs-lookup"><span data-stu-id="31453-142">May corrupt the body format.</span></span> <span data-ttu-id="31453-143">HTML zápatí například zápis do souboru šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="31453-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="31453-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> je užitečné nápovědu k označení, pokud byly odeslány hlavičky nebo text byl zapsán do.</span><span class="sxs-lookup"><span data-stu-id="31453-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="31453-145">Pořadí</span><span class="sxs-lookup"><span data-stu-id="31453-145">Order</span></span>

<span data-ttu-id="31453-146">Pořadí, ve kterém middlewarových komponent přidány `Startup.Configure` metoda definuje pořadí, ve kterém jsou vyvolány middlewarových komponent na požadavky a obrácenému pořadí pro odpověď.</span><span class="sxs-lookup"><span data-stu-id="31453-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="31453-147">Pořadí je důležité pro zabezpečení, výkon a funkčnost.</span><span class="sxs-lookup"><span data-stu-id="31453-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="31453-148">Následující `Configure` metoda přidá následující middlewarových komponent:</span><span class="sxs-lookup"><span data-stu-id="31453-148">The following `Configure` method adds the following middleware components:</span></span>

1. <span data-ttu-id="31453-149">Zpracování výjimek a chyb</span><span class="sxs-lookup"><span data-stu-id="31453-149">Exception/error handling</span></span>
2. <span data-ttu-id="31453-150">Statický souborový server</span><span class="sxs-lookup"><span data-stu-id="31453-150">Static file server</span></span>
3. <span data-ttu-id="31453-151">Ověřování</span><span class="sxs-lookup"><span data-stu-id="31453-151">Authentication</span></span>
4. <span data-ttu-id="31453-152">MVC</span><span class="sxs-lookup"><span data-stu-id="31453-152">MVC</span></span>

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

<span data-ttu-id="31453-153">V předchozím příkladu kódu, každá metoda rozšíření middlewaru je vystaven na <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> prostřednictvím <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="31453-153">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="31453-154"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> je první komponenta middlewaru přidané do kanálu.</span><span class="sxs-lookup"><span data-stu-id="31453-154"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="31453-155">Proto Middleware obslužné rutiny výjimek zachytává všechny výjimky, ke kterým dochází v pozdější volání.</span><span class="sxs-lookup"><span data-stu-id="31453-155">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="31453-156">Statické soubory middlewaru je volána v rané fázi kanálu tak, aby mohl zpracovávat požadavky a zkrácenou bez nutnosti kontaktovat zbývající součásti.</span><span class="sxs-lookup"><span data-stu-id="31453-156">Static Files Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="31453-157">Statické soubory Middleware poskytuje **žádné** kontroly autorizace.</span><span class="sxs-lookup"><span data-stu-id="31453-157">The Static Files Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="31453-158">Všechny soubory obsluhuje, včetně těch *wwwroot*, jsou veřejně dostupné.</span><span class="sxs-lookup"><span data-stu-id="31453-158">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="31453-159">Přístup k zabezpečení statické soubory, naleznete v tématu <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="31453-159">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="31453-160">Pokud žádost není zpracovávaných middlewarem statické soubory, je předána Middleware ověřování (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), který provádí ověřování.</span><span class="sxs-lookup"><span data-stu-id="31453-160">If the request isn't handled by the Static Files Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="31453-161">Ověřování není zkrácenou neověřené žádosti.</span><span class="sxs-lookup"><span data-stu-id="31453-161">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="31453-162">I když ověřovací Middleware ověřuje požadavky, autorizace (a odmítnutí) nastane pouze po MVC vybere konkrétní stránky Razor nebo MVC kontroleru a akce.</span><span class="sxs-lookup"><span data-stu-id="31453-162">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="31453-163">Pokud požadavek není zpracováván statické soubory Middleware, je předána Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), který provádí ověřování.</span><span class="sxs-lookup"><span data-stu-id="31453-163">If the request isn't handled by Static Files Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="31453-164">Identita není zkrácenou neověřené žádosti.</span><span class="sxs-lookup"><span data-stu-id="31453-164">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="31453-165">I když žádosti o ověření Identity autorizace (a odmítnutí) nastane pouze po MVC vybere konkrétní kontroleru a akce.</span><span class="sxs-lookup"><span data-stu-id="31453-165">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="31453-166">Následující příklad ukazuje middleware pořadí, ve kterém jsou požadavky pro statické soubory zpracovávány middlewarem statické soubory před Middleware pro kompresi odpovědí.</span><span class="sxs-lookup"><span data-stu-id="31453-166">The following example demonstrates a middleware order where requests for static files are handled by Static Files Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="31453-167">Statické soubory nejsou komprimovaný pomocí tohoto pořadí middlewaru.</span><span class="sxs-lookup"><span data-stu-id="31453-167">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="31453-168">Odpovědi MVC <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> lze komprimovat.</span><span class="sxs-lookup"><span data-stu-id="31453-168">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="31453-169">Použití, spuštění a mapy</span><span class="sxs-lookup"><span data-stu-id="31453-169">Use, Run, and Map</span></span>

<span data-ttu-id="31453-170">Konfigurace kanálu HTTP pomocí `Use`, `Run`, a `Map`.</span><span class="sxs-lookup"><span data-stu-id="31453-170">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="31453-171">`Use` Metoda můžete zkrácenou kanálu (tj. Pokud nebude volat `next` delegáta požadavku).</span><span class="sxs-lookup"><span data-stu-id="31453-171">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="31453-172">`Run` je konvence, a některé middlewarových komponent může vystavit `Run[Middleware]` metody, které běží na konci kanálu.</span><span class="sxs-lookup"><span data-stu-id="31453-172">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="31453-173"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> rozšíření jsou použity jako konvence pro větvení kanálu.</span><span class="sxs-lookup"><span data-stu-id="31453-173"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="31453-174">`Map*` větve kanál požadavku na základě shody zadanou cestu požadavku.</span><span class="sxs-lookup"><span data-stu-id="31453-174">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="31453-175">Pokud cesta požadavku začíná dané cesty, větve je proveden.</span><span class="sxs-lookup"><span data-stu-id="31453-175">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="31453-176">V následující tabulce jsou uvedeny požadavky a odpovědi z `http://localhost:1234` pomocí předchozího kódu.</span><span class="sxs-lookup"><span data-stu-id="31453-176">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="31453-177">Požadavek</span><span class="sxs-lookup"><span data-stu-id="31453-177">Request</span></span>             | <span data-ttu-id="31453-178">Odpověď</span><span class="sxs-lookup"><span data-stu-id="31453-178">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="31453-179">1234</span><span class="sxs-lookup"><span data-stu-id="31453-179">localhost:1234</span></span>      | <span data-ttu-id="31453-180">Dobrý den ze bez mapování delegáta.</span><span class="sxs-lookup"><span data-stu-id="31453-180">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="31453-181">1234 / map1</span><span class="sxs-lookup"><span data-stu-id="31453-181">localhost:1234/map1</span></span> | <span data-ttu-id="31453-182">Mapování Test 1</span><span class="sxs-lookup"><span data-stu-id="31453-182">Map Test 1</span></span>                   |
| <span data-ttu-id="31453-183">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="31453-183">localhost:1234/map2</span></span> | <span data-ttu-id="31453-184">Mapování testů 2</span><span class="sxs-lookup"><span data-stu-id="31453-184">Map Test 2</span></span>                   |
| <span data-ttu-id="31453-185">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="31453-185">localhost:1234/map3</span></span> | <span data-ttu-id="31453-186">Dobrý den ze bez mapování delegáta.</span><span class="sxs-lookup"><span data-stu-id="31453-186">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="31453-187">Když `Map` se používá, segmenty cesty odpovídající jsou odebrány z `HttpRequest.Path` a připojený k `HttpRequest.PathBase` pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="31453-187">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="31453-188">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) větví kanál požadavku na základě výsledku daného predikátu.</span><span class="sxs-lookup"><span data-stu-id="31453-188">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="31453-189">Žádné predikátu typu `Func<HttpContext, bool>` je možné mapovat požadavky na novou větev kanálu.</span><span class="sxs-lookup"><span data-stu-id="31453-189">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="31453-190">V následujícím příkladu, predikát slouží k detekci přítomnosti proměnnou s řetězcem dotazu `branch`:</span><span class="sxs-lookup"><span data-stu-id="31453-190">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="31453-191">V následující tabulce jsou uvedeny požadavky a odpovědi z `http://localhost:1234` pomocí předchozího kódu.</span><span class="sxs-lookup"><span data-stu-id="31453-191">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="31453-192">Požadavek</span><span class="sxs-lookup"><span data-stu-id="31453-192">Request</span></span>                       | <span data-ttu-id="31453-193">Odpověď</span><span class="sxs-lookup"><span data-stu-id="31453-193">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="31453-194">1234</span><span class="sxs-lookup"><span data-stu-id="31453-194">localhost:1234</span></span>                | <span data-ttu-id="31453-195">Dobrý den ze bez mapování delegáta.</span><span class="sxs-lookup"><span data-stu-id="31453-195">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="31453-196">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="31453-196">localhost:1234/?branch=master</span></span> | <span data-ttu-id="31453-197">Větev se použije hlavní =</span><span class="sxs-lookup"><span data-stu-id="31453-197">Branch used = master</span></span>         |

<span data-ttu-id="31453-198">`Map` podporuje vnořené, například:</span><span class="sxs-lookup"><span data-stu-id="31453-198">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="31453-199">`Map` Můžete také porovnat více segmentů najednou:</span><span class="sxs-lookup"><span data-stu-id="31453-199">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="31453-200">Integrované middlewaru</span><span class="sxs-lookup"><span data-stu-id="31453-200">Built-in middleware</span></span>

<span data-ttu-id="31453-201">ASP.NET Core se dodává s následujícími součástmi middlewaru.</span><span class="sxs-lookup"><span data-stu-id="31453-201">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="31453-202">*Pořadí* sloupec obsahuje poznámky k umístění middleware v kanálu požadavku a za jakých podmínek middleware může ukončit žádosti a zabránit dalším middlewarem zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="31453-202">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="31453-203">Middleware</span><span class="sxs-lookup"><span data-stu-id="31453-203">Middleware</span></span> | <span data-ttu-id="31453-204">Popis</span><span class="sxs-lookup"><span data-stu-id="31453-204">Description</span></span> | <span data-ttu-id="31453-205">Pořadí</span><span class="sxs-lookup"><span data-stu-id="31453-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="31453-206">Ověřování</span><span class="sxs-lookup"><span data-stu-id="31453-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="31453-207">Poskytuje podporu ověřování.</span><span class="sxs-lookup"><span data-stu-id="31453-207">Provides authentication support.</span></span> | <span data-ttu-id="31453-208">Před `HttpContext.User` je potřeba.</span><span class="sxs-lookup"><span data-stu-id="31453-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="31453-209">Terminál pro zpětná volání OAuth.</span><span class="sxs-lookup"><span data-stu-id="31453-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="31453-210">CORS</span><span class="sxs-lookup"><span data-stu-id="31453-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="31453-211">Konfiguruje sdílení prostředků různého původu.</span><span class="sxs-lookup"><span data-stu-id="31453-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="31453-212">Před provedením komponent, které používají CORS.</span><span class="sxs-lookup"><span data-stu-id="31453-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="31453-213">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="31453-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="31453-214">Konfiguruje se Diagnostika.</span><span class="sxs-lookup"><span data-stu-id="31453-214">Configures diagnostics.</span></span> | <span data-ttu-id="31453-215">Před provedením komponent, které generují chyby.</span><span class="sxs-lookup"><span data-stu-id="31453-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="31453-216">Přesměrovaná záhlaví</span><span class="sxs-lookup"><span data-stu-id="31453-216">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="31453-217">Předává záhlaví směrovány přes proxy server na aktuální žádost.</span><span class="sxs-lookup"><span data-stu-id="31453-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="31453-218">Před provedením komponent, které využívají aktualizovanými poli (příklady: schéma, hostitele, IP adresu klienta, metoda).</span><span class="sxs-lookup"><span data-stu-id="31453-218">Before components that consume the updated fields (examples: scheme, host, client IP, method).</span></span> |
| [<span data-ttu-id="31453-219">Přepsání metody HTTP</span><span class="sxs-lookup"><span data-stu-id="31453-219">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="31453-220">Umožňuje příchozí požadavek POST k přepsání metody.</span><span class="sxs-lookup"><span data-stu-id="31453-220">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="31453-221">Před provedením komponent, které využívají aktualizované metody.</span><span class="sxs-lookup"><span data-stu-id="31453-221">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="31453-222">Přesměrování protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="31453-222">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="31453-223">Přesměrujte všechny požadavky HTTP na HTTPS (ASP.NET Core 2.1 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="31453-223">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="31453-224">Před provedením komponent, které využívají adresu URL.</span><span class="sxs-lookup"><span data-stu-id="31453-224">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="31453-225">Zabezpečení striktní přenosu HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="31453-225">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="31453-226">Zabezpečení vylepšení middleware, který přidá hlavičku odpovědi speciální (ASP.NET Core 2.1 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="31453-226">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="31453-227">Předtím, než se posílají žádosti a po součásti, které mění žádosti (například předávat záhlaví, přepisování adres URL).</span><span class="sxs-lookup"><span data-stu-id="31453-227">Before responses are sent and after components that modify requests (for example, Forwarded Headers, URL Rewriting).</span></span> |
| [<span data-ttu-id="31453-228">MVC</span><span class="sxs-lookup"><span data-stu-id="31453-228">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="31453-229">Zpracuje žádosti s MVC/Razor Pages (ASP.NET Core 2.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="31453-229">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="31453-230">Terminál, pokud požadavek odpovídá trase.</span><span class="sxs-lookup"><span data-stu-id="31453-230">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="31453-231">OWIN</span><span class="sxs-lookup"><span data-stu-id="31453-231">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="31453-232">Interoperabilita s aplikací OWIN, servery a middlewaru.</span><span class="sxs-lookup"><span data-stu-id="31453-232">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="31453-233">Terminál, pokud middlewaru OWIN, který plně zpracuje požadavek.</span><span class="sxs-lookup"><span data-stu-id="31453-233">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="31453-234">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="31453-234">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="31453-235">Poskytuje podporu pro ukládání do mezipaměti odpovědi.</span><span class="sxs-lookup"><span data-stu-id="31453-235">Provides support for caching responses.</span></span> | <span data-ttu-id="31453-236">Před provedením komponent, které vyžadují ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="31453-236">Before components that require caching.</span></span> |
| [<span data-ttu-id="31453-237">Kompresi odpovědí</span><span class="sxs-lookup"><span data-stu-id="31453-237">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="31453-238">Poskytuje podporu pro kompresi odpovědí.</span><span class="sxs-lookup"><span data-stu-id="31453-238">Provides support for compressing responses.</span></span> | <span data-ttu-id="31453-239">Před provedením komponent, které vyžadují komprese.</span><span class="sxs-lookup"><span data-stu-id="31453-239">Before components that require compression.</span></span> |
| [<span data-ttu-id="31453-240">Žádost o lokalizaci</span><span class="sxs-lookup"><span data-stu-id="31453-240">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="31453-241">Poskytuje podporu lokalizace.</span><span class="sxs-lookup"><span data-stu-id="31453-241">Provides localization support.</span></span> | <span data-ttu-id="31453-242">Před provedením komponent citlivé lokalizace.</span><span class="sxs-lookup"><span data-stu-id="31453-242">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="31453-243">Směrování</span><span class="sxs-lookup"><span data-stu-id="31453-243">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="31453-244">Definuje a omezuje žádosti trasy.</span><span class="sxs-lookup"><span data-stu-id="31453-244">Defines and constrains request routes.</span></span> | <span data-ttu-id="31453-245">Terminál odpovídající trasy.</span><span class="sxs-lookup"><span data-stu-id="31453-245">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="31453-246">Relace</span><span class="sxs-lookup"><span data-stu-id="31453-246">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="31453-247">Poskytuje podporu pro správu uživatelských relací.</span><span class="sxs-lookup"><span data-stu-id="31453-247">Provides support for managing user sessions.</span></span> | <span data-ttu-id="31453-248">Před provedením komponent, které vyžadují relace.</span><span class="sxs-lookup"><span data-stu-id="31453-248">Before components that require Session.</span></span> |
| [<span data-ttu-id="31453-249">Statické soubory</span><span class="sxs-lookup"><span data-stu-id="31453-249">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="31453-250">Poskytuje podporu pro poskytování statických souborů a procházení adresářů.</span><span class="sxs-lookup"><span data-stu-id="31453-250">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="31453-251">Terminál, pokud požadavek odpovídá souboru.</span><span class="sxs-lookup"><span data-stu-id="31453-251">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="31453-252">Přepisování adres URL</span><span class="sxs-lookup"><span data-stu-id="31453-252">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="31453-253">Poskytuje podporu pro přepis adres URL a přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="31453-253">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="31453-254">Před provedením komponent, které využívají adresu URL.</span><span class="sxs-lookup"><span data-stu-id="31453-254">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="31453-255">Webové sokety</span><span class="sxs-lookup"><span data-stu-id="31453-255">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="31453-256">Povolí protokol Websocket.</span><span class="sxs-lookup"><span data-stu-id="31453-256">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="31453-257">Před provedením komponent, které jsou nutné, aby přijímal požadavky protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="31453-257">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="31453-258">Zápis middlewaru</span><span class="sxs-lookup"><span data-stu-id="31453-258">Write middleware</span></span>

<span data-ttu-id="31453-259">Middleware je obecně zapouzdřené v třídě a vystavený s metodou rozšíření.</span><span class="sxs-lookup"><span data-stu-id="31453-259">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="31453-260">Vezměte v úvahu následující middlewaru, který nastaví jazykovou verzi pro aktuální požadavek z řetězce dotazu:</span><span class="sxs-lookup"><span data-stu-id="31453-260">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="31453-261">Předchozí kód ukázkové slouží k předvedení vytváření komponenta middlewaru.</span><span class="sxs-lookup"><span data-stu-id="31453-261">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="31453-262">ASP.NET Core lokalizace integrovanou podporu najdete na webu <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="31453-262">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="31453-263">Middleware můžete otestovat tím, že předáte v jazykové verzi, například `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="31453-263">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="31453-264">Následující kód přesune delegáta middleware pro třídu:</span><span class="sxs-lookup"><span data-stu-id="31453-264">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="31453-265">Middleware `Task` název metody musí být `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="31453-265">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="31453-266">V technologii ASP.NET Core 2.0 nebo novější, název může být buď `Invoke` nebo `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="31453-266">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="31453-267">Poskytuje následující metody rozšíření middleware prostřednictvím <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="31453-267">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="31453-268">Následující kód volá middleware z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="31453-268">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="31453-269">Postupujte podle middleware [explicitní závislosti Princip](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) zveřejněním závislých 've svém konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="31453-269">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="31453-270">Middleware je vytvořen jednou za *dobu životnosti aplikace*.</span><span class="sxs-lookup"><span data-stu-id="31453-270">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="31453-271">Najdete v článku [závislosti na požadavku](#per-request-dependencies) části, pokud je potřeba sdílet s middlewaru v rámci žádost o služby.</span><span class="sxs-lookup"><span data-stu-id="31453-271">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="31453-272">Middlewarových komponent lze vyřešit jejich závislosti z [injektáž závislostí (DI)](xref:fundamentals/dependency-injection) prostřednictvím parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="31453-272">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="31453-273">[UseMiddleware&lt;T&gt; ](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) můžete také přijmout přímo další parametry.</span><span class="sxs-lookup"><span data-stu-id="31453-273">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="31453-274">Závislosti na základě žádosti</span><span class="sxs-lookup"><span data-stu-id="31453-274">Per-request dependencies</span></span>

<span data-ttu-id="31453-275">Protože middlewaru je vytvořen při spuštění aplikace, nikoli jednotlivých žádostí, *obor* služby životního cyklu používat middleware konstruktory nejsou sdíleny s jinými typy vložený závislostí při každém požadavku.</span><span class="sxs-lookup"><span data-stu-id="31453-275">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="31453-276">Pokud musíte sdílet *obor* služby mezi middlewaru a ostatními typy, přidejte tyto služby `Invoke` podpis metody.</span><span class="sxs-lookup"><span data-stu-id="31453-276">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="31453-277">`Invoke` Metoda může přijímat další parametry, které se vyplní podle DI:</span><span class="sxs-lookup"><span data-stu-id="31453-277">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="31453-278">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="31453-278">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
