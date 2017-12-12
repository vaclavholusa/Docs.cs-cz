---
title: "Základy ASP.NET Core"
author: rick-anderson
description: "Zjistit základní koncepty pro vytváření aplikací ASP.NET Core."
keywords: "ASP.NET Core, základy, – přehled"
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 83bed4676be3ca752442da3fe560f1f2a4d728a1
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="3346b-104">Základy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3346b-104">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="3346b-105">Aplikace ASP.NET Core je konzolovou aplikaci, která vytvoří webovým serverem v jeho `Main` metoda:</span><span class="sxs-lookup"><span data-stu-id="3346b-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3346b-106">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3346b-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="3346b-107">`Main` Vyvolá metoda `WebHost.CreateDefaultBuilder`, který odpovídá vzorci Tvůrce vytvořit hostitel webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3346b-107">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="3346b-108">Tvůrce obsahuje metody, které definují webového serveru (například `UseKestrel`) a třída při spuštění (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="3346b-108">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="3346b-109">V předchozím příkladu [Kestrel](xref:fundamentals/servers/kestrel) webový server je automaticky přidělen.</span><span class="sxs-lookup"><span data-stu-id="3346b-109">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="3346b-110">ASP.NET Core webového hostitele se pokusí o spuštění ve službě IIS, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3346b-110">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="3346b-111">Další webové servery, jako například [HTTP.sys](xref:fundamentals/servers/httpsys), mohou být využívána volání metody odpovídající rozšíření.</span><span class="sxs-lookup"><span data-stu-id="3346b-111">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="3346b-112">`UseStartup`je vysvětleno v následující části Další.</span><span class="sxs-lookup"><span data-stu-id="3346b-112">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="3346b-113">`IWebHostBuilder`, návratový typ `WebHost.CreateDefaultBuilder` volání, nabízí mnoho způsobů volitelné.</span><span class="sxs-lookup"><span data-stu-id="3346b-113">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="3346b-114">Některé z těchto metod zahrnují `UseHttpSys` pro hostování aplikace v HTTP.sys a `UseContentRoot` pro zadání kořenový adresář s obsahem.</span><span class="sxs-lookup"><span data-stu-id="3346b-114">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="3346b-115">`Build` a `Run` metody sestavení `IWebHost` objekt, který je hostitelem aplikace a začne naslouchat požadavkům HTTP.</span><span class="sxs-lookup"><span data-stu-id="3346b-115">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3346b-116">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3346b-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="3346b-117">`Main` Používá metoda `WebHostBuilder`, který odpovídá vzorci Tvůrce vytvořit hostitel webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3346b-117">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="3346b-118">Tvůrce obsahuje metody, které definují webového serveru (například `UseKestrel`) a třída při spuštění (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="3346b-118">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="3346b-119">V předchozím příkladu [Kestrel](xref:fundamentals/servers/kestrel) se používá webový server.</span><span class="sxs-lookup"><span data-stu-id="3346b-119">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="3346b-120">Další webové servery, jako například [WebListener](xref:fundamentals/servers/weblistener), mohou být využívána volání metody odpovídající rozšíření.</span><span class="sxs-lookup"><span data-stu-id="3346b-120">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="3346b-121">`UseStartup`je vysvětleno v následující části Další.</span><span class="sxs-lookup"><span data-stu-id="3346b-121">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="3346b-122">`WebHostBuilder`nabízí mnoho volitelné způsobů, včetně `UseIISIntegration` pro hostování v IIS a služby IIS Express a `UseContentRoot` pro zadání kořenový adresář s obsahem.</span><span class="sxs-lookup"><span data-stu-id="3346b-122">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="3346b-123">`Build` a `Run` metody sestavení `IWebHost` objekt, který je hostitelem aplikace a začne naslouchat požadavkům HTTP.</span><span class="sxs-lookup"><span data-stu-id="3346b-123">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="3346b-124">Spuštění</span><span class="sxs-lookup"><span data-stu-id="3346b-124">Startup</span></span>

<span data-ttu-id="3346b-125">`UseStartup` Metodu `WebHostBuilder` Určuje `Startup` třídy pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="3346b-125">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3346b-126">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3346b-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3346b-127">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3346b-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="3346b-128">`Startup` Je třída, kde můžete definovat v kanálu zpracování požadavků a které jsou nakonfigurované všechny služby potřebné pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3346b-128">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="3346b-129">`Startup` Třída musí být veřejné a musí obsahovat následující metody:</span><span class="sxs-lookup"><span data-stu-id="3346b-129">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

<span data-ttu-id="3346b-130">`ConfigureServices`definuje [služby](#dependency-injection-services) používané vaší aplikace (například Identity Entity Framework Core, základní rozhraní ASP.NET MVC).</span><span class="sxs-lookup"><span data-stu-id="3346b-130">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="3346b-131">`Configure`definuje [middleware](xref:fundamentals/middleware) pro kanál požadavku.</span><span class="sxs-lookup"><span data-stu-id="3346b-131">`Configure` defines the [middleware](xref:fundamentals/middleware) for the request pipeline.</span></span>

<span data-ttu-id="3346b-132">Další informace najdete v tématu [spuštění aplikace](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="3346b-132">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="3346b-133">Obsah kořenové</span><span class="sxs-lookup"><span data-stu-id="3346b-133">Content root</span></span>

<span data-ttu-id="3346b-134">Kořenu obsahu je základní cesta k žádný obsah používané aplikace, jako je například zobrazení, [stránky Razor](xref:mvc/razor-pages/index)a statické prostředky.</span><span class="sxs-lookup"><span data-stu-id="3346b-134">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="3346b-135">Ve výchozím nastavení kořenu obsahu je stejný jako základní cesta aplikace pro spustitelný soubor, který je hostitelem aplikace.</span><span class="sxs-lookup"><span data-stu-id="3346b-135">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="3346b-136">Kořenový web</span><span class="sxs-lookup"><span data-stu-id="3346b-136">Web root</span></span>

<span data-ttu-id="3346b-137">Kořenové webové aplikace je adresář v projektu obsahující veřejný, statické prostředky, například šablon stylů CSS, JavaScript a soubory obrázků.</span><span class="sxs-lookup"><span data-stu-id="3346b-137">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="3346b-138">Vkládání závislostí (služby)</span><span class="sxs-lookup"><span data-stu-id="3346b-138">Dependency Injection (Services)</span></span>

<span data-ttu-id="3346b-139">Služba je komponenta, která je určená pro běžné používání v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3346b-139">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="3346b-140">Služby jsou dostupné prostřednictvím [vkládání závislostí (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3346b-140">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3346b-141">ASP.NET Core zahrnuje nativní **I**nPro **o**f **C**kontejneru obrazit řídicí (IoC), který podporuje [konstruktor vkládání](xref:mvc/controllers/dependency-injection#constructor-injection) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3346b-141">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="3346b-142">Výchozí kontejner nativní můžete nahradit, pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="3346b-142">You can replace the default native container if you wish.</span></span> <span data-ttu-id="3346b-143">Kromě jeho výhody volné párování DI umožňuje služby k dispozici v celé vaší aplikace (například [protokolování](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="3346b-143">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="3346b-144">Další informace najdete v tématu [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3346b-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="3346b-145">Middleware</span><span class="sxs-lookup"><span data-stu-id="3346b-145">Middleware</span></span>

<span data-ttu-id="3346b-146">V ASP.NET Core, vytvoříte pomocí kanálu požadavku [middleware](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="3346b-146">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="3346b-147">ASP.NET Core middleware provede asynchronní logiku `HttpContext` a vyvolá další middleware v pořadí, nebo přímo ukončí žádosti.</span><span class="sxs-lookup"><span data-stu-id="3346b-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="3346b-148">Přidá middleware komponenty s názvem "XYZ" se při vyvolání `UseXYZ` metoda rozšíření v `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="3346b-148">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="3346b-149">ASP.NET Core se dodává s bohatou sadu předdefinovaných middleware:</span><span class="sxs-lookup"><span data-stu-id="3346b-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="3346b-150">Statické soubory</span><span class="sxs-lookup"><span data-stu-id="3346b-150">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="3346b-151">Směrování</span><span class="sxs-lookup"><span data-stu-id="3346b-151">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="3346b-152">Ověřování</span><span class="sxs-lookup"><span data-stu-id="3346b-152">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="3346b-153">Middleware komprese odpovědi</span><span class="sxs-lookup"><span data-stu-id="3346b-153">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="3346b-154">Adresa URL přepisování middlewaru</span><span class="sxs-lookup"><span data-stu-id="3346b-154">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="3346b-155">[OWIN](http://owin.org)– na základě middlewaru je k dispozici pro aplikace ASP.NET Core a můžete napsat vlastní vlastní middleware.</span><span class="sxs-lookup"><span data-stu-id="3346b-155">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="3346b-156">Další informace najdete v tématu [Middleware](xref:fundamentals/middleware) a [Open Web Interface pro .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="3346b-156">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="environments"></a><span data-ttu-id="3346b-157">Prostředí</span><span class="sxs-lookup"><span data-stu-id="3346b-157">Environments</span></span>

<span data-ttu-id="3346b-158">Prostředí, jako je například "Vývoj" a "Provozní", jsou hodnoty první třídy v ASP.NET Core a se dá nastavit pomocí proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3346b-158">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="3346b-159">Další informace najdete v tématu [práce s několika prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="3346b-159">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="3346b-160">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="3346b-160">Configuration</span></span>

<span data-ttu-id="3346b-161">Jádro ASP.NET používá model konfigurace na základě párů název hodnota.</span><span class="sxs-lookup"><span data-stu-id="3346b-161">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="3346b-162">Konfigurační model není založen na `System.Configuration` nebo *web.config*. Konfigurace obsahuje nastavení ze seřazené sadu poskytovatelů konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3346b-162">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="3346b-163">Poskytovatelé předdefinované konfigurace podporují celou řadu formáty souborů (XML, JSON, INI) a proměnné prostředí umožňující konfiguraci na základě prostředí.</span><span class="sxs-lookup"><span data-stu-id="3346b-163">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="3346b-164">Také můžete napsat vlastního zprostředkovatele vlastní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3346b-164">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="3346b-165">Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3346b-165">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="3346b-166">protokolování</span><span class="sxs-lookup"><span data-stu-id="3346b-166">Logging</span></span>

<span data-ttu-id="3346b-167">Jádro ASP.NET podporuje protokolování rozhraní API, která funguje s různými zprostředkovatelů protokolování.</span><span class="sxs-lookup"><span data-stu-id="3346b-167">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="3346b-168">Předdefinované zprostředkovatele podporovat odesílání protokoly na jeden nebo více míst.</span><span class="sxs-lookup"><span data-stu-id="3346b-168">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="3346b-169">Můžete použít rozhraní protokolování třetích stran.</span><span class="sxs-lookup"><span data-stu-id="3346b-169">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="3346b-170">Protokolování</span><span class="sxs-lookup"><span data-stu-id="3346b-170">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="3346b-171">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="3346b-171">Error handling</span></span>

<span data-ttu-id="3346b-172">ASP.NET Core obsahuje integrované funkce pro zpracování chyb v aplikacích, včetně stránky výjimka developer, vlastní chybové stránky, statické stav znakové stránky a spuštění zpracování výjimek.</span><span class="sxs-lookup"><span data-stu-id="3346b-172">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="3346b-173">Další informace najdete v tématu [zpracování chyb](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="3346b-173">For more information, see [Error Handling](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="3346b-174">Směrování</span><span class="sxs-lookup"><span data-stu-id="3346b-174">Routing</span></span>

<span data-ttu-id="3346b-175">ASP.NET Core nabízí funkce pro směrování požadavků app na obslužné rutiny trasy.</span><span class="sxs-lookup"><span data-stu-id="3346b-175">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="3346b-176">Další informace najdete v tématu [směrování](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="3346b-176">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="3346b-177">Zprostředkovatelé souboru</span><span class="sxs-lookup"><span data-stu-id="3346b-177">File providers</span></span>

<span data-ttu-id="3346b-178">ASP.NET Core abstrahuje přístupu k systému souborů prostřednictvím poskytovatelů souboru, který nabízí společné rozhraní pro práci se soubory napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="3346b-178">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="3346b-179">Další informace najdete v tématu [souboru zprostředkovatelé](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="3346b-179">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="3346b-180">Statické soubory</span><span class="sxs-lookup"><span data-stu-id="3346b-180">Static files</span></span>

<span data-ttu-id="3346b-181">Statické soubory middleware poskytuje statické soubory, jako je například HTML, CSS, image a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3346b-181">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="3346b-182">Další informace najdete v tématu [práce s statické soubory](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="3346b-182">For more information, see [Working with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="3346b-183">Hostování</span><span class="sxs-lookup"><span data-stu-id="3346b-183">Hosting</span></span>

<span data-ttu-id="3346b-184">Aplikace ASP.NET Core nakonfigurovat a spustit *hostitele*, která je zodpovědná za spuštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="3346b-184">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="3346b-185">Další informace najdete v tématu [hostitelský](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="3346b-185">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="3346b-186">Stav relace a aplikace</span><span class="sxs-lookup"><span data-stu-id="3346b-186">Session and application state</span></span>

<span data-ttu-id="3346b-187">Stav relace je funkce v ASP.NET Core, který můžete použít k ukládání a uchovávání dat uživatele, když uživatel prohlíží vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3346b-187">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="3346b-188">Další informace najdete v tématu [relace a stav aplikace](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="3346b-188">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="3346b-189">Servery</span><span class="sxs-lookup"><span data-stu-id="3346b-189">Servers</span></span>

<span data-ttu-id="3346b-190">Model hostování základní technologie ASP.NET není přímo naslouchat požadavkům.</span><span class="sxs-lookup"><span data-stu-id="3346b-190">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="3346b-191">Model hostování závisí na implementaci serveru HTTP k předání požadavku do aplikace.</span><span class="sxs-lookup"><span data-stu-id="3346b-191">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="3346b-192">Předaný požadavek je zabalená jako sada objektů funkce, které je přístupné prostřednictvím rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3346b-192">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="3346b-193">ASP.NET Core zahrnuje spravovaná, a platformy webového serveru, nazývá [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="3346b-193">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="3346b-194">Kestrel je často spouštět za produkční webového serveru, jako například [IIS](https://www.iis.net/) nebo [nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="3346b-194">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [nginx](http://nginx.org).</span></span> <span data-ttu-id="3346b-195">Kestrel můžete spouštět jako hraniční server.</span><span class="sxs-lookup"><span data-stu-id="3346b-195">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="3346b-196">Další informace najdete v tématu [servery](xref:fundamentals/servers/index) a v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="3346b-196">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="3346b-197">Kestrel</span><span class="sxs-lookup"><span data-stu-id="3346b-197">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="3346b-198">Modul ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3346b-198">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="3346b-199">[Ovladač HTTP.sys](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="3346b-199">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="3346b-200">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="3346b-200">Globalization and localization</span></span>

<span data-ttu-id="3346b-201">Vytvoření vícejazyčné webu pomocí ASP.NET Core umožňuje vaší lokality k dosažení širší cílovou skupinu.</span><span class="sxs-lookup"><span data-stu-id="3346b-201">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="3346b-202">Základní technologie ASP.NET poskytuje služby a middleware pro lokalizace do různých jazyků a kultur.</span><span class="sxs-lookup"><span data-stu-id="3346b-202">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="3346b-203">Další informace najdete v tématu [globalizace a lokalizace](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="3346b-203">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="3346b-204">Požadavky na funkce</span><span class="sxs-lookup"><span data-stu-id="3346b-204">Request features</span></span>

<span data-ttu-id="3346b-205">Související podrobnosti implementace webového serveru na požadavky HTTP a odpovědí jsou definovány v rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3346b-205">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="3346b-206">Tato rozhraní jsou používány implementací serveru a middleware, vytvářet a upravovat hostování kanálu aplikace.</span><span class="sxs-lookup"><span data-stu-id="3346b-206">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="3346b-207">Další informace najdete v tématu [žádosti o funkce](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="3346b-207">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="3346b-208">Spustit nástroj webové rozhraní pro platformu .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="3346b-208">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="3346b-209">Jádro ASP.NET podporuje Open Web Interface pro .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="3346b-209">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="3346b-210">OWIN umožňuje webových aplikací pro být odděleno od webové servery.</span><span class="sxs-lookup"><span data-stu-id="3346b-210">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="3346b-211">Další informace najdete v tématu [Open Web Interface pro .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="3346b-211">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="3346b-212">Technologie WebSockets</span><span class="sxs-lookup"><span data-stu-id="3346b-212">WebSockets</span></span>

<span data-ttu-id="3346b-213">[Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="3346b-213">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="3346b-214">Použije se pro aplikace, jako je například chat, kurzů akcií, hry a kdekoli požadavky v reálném čase funkce ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3346b-214">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="3346b-215">Jádro ASP.NET podporuje webových funkcí produktu soketu.</span><span class="sxs-lookup"><span data-stu-id="3346b-215">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="3346b-216">Další informace najdete v tématu [Websocket](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="3346b-216">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="3346b-217">Microsoft.AspNetCore.All metapackage</span><span class="sxs-lookup"><span data-stu-id="3346b-217">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="3346b-218">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage pro ASP.NET Core zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="3346b-218">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="3346b-219">Všechny podporované balíčky tým ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3346b-219">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="3346b-220">Všechny podporované balíčků základní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3346b-220">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="3346b-221">Interní a 3. stran závislosti používané ASP.NET Core a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3346b-221">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="3346b-222">Další informace najdete v tématu [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="3346b-222">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="3346b-223">Modul runtime rozhraní .NET core a rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="3346b-223">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="3346b-224">Aplikace ASP.NET Core, můžete vybrat runtime .NET Core nebo rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3346b-224">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="3346b-225">Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="3346b-225">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="3346b-226">Zvolte mezi ASP.NET Core a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3346b-226">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="3346b-227">Další informace o výběru mezi ASP.NET Core a ASP.NET naleznete v tématu [zvolte mezi ASP.NET Core a ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="3346b-227">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
