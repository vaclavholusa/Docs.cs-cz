---
title: Základy ASP.NET Core
author: rick-anderson
description: Zjistit základní koncepty pro vytváření aplikací ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: d5b74e213828d1a1f7e09810e5cc72773a821dab
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="860f3-103">Základy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="860f3-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="860f3-104">Aplikace ASP.NET Core je konzolovou aplikaci, která vytvoří webovým serverem v jeho `Main` metoda:</span><span class="sxs-lookup"><span data-stu-id="860f3-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="860f3-105">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="860f3-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="860f3-106">`Main` Vyvolá metoda `WebHost.CreateDefaultBuilder`, který odpovídá vzorci Tvůrce vytvořit hostitel webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="860f3-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="860f3-107">Tvůrce obsahuje metody, které definují webového serveru (například `UseKestrel`) a třída při spuštění (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="860f3-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="860f3-108">V předchozím příkladu [Kestrel](xref:fundamentals/servers/kestrel) webový server je automaticky přidělen.</span><span class="sxs-lookup"><span data-stu-id="860f3-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="860f3-109">ASP.NET Core webového hostitele se pokusí o spuštění ve službě IIS, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="860f3-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="860f3-110">Další webové servery, jako například [HTTP.sys](xref:fundamentals/servers/httpsys), mohou být využívána volání metody odpovídající rozšíření.</span><span class="sxs-lookup"><span data-stu-id="860f3-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="860f3-111">`UseStartup` je vysvětleno v následující části Další.</span><span class="sxs-lookup"><span data-stu-id="860f3-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="860f3-112">`IWebHostBuilder`, návratový typ `WebHost.CreateDefaultBuilder` volání, nabízí mnoho způsobů volitelné.</span><span class="sxs-lookup"><span data-stu-id="860f3-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="860f3-113">Některé z těchto metod zahrnují `UseHttpSys` pro hostování aplikace v HTTP.sys a `UseContentRoot` pro zadání kořenový adresář s obsahem.</span><span class="sxs-lookup"><span data-stu-id="860f3-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="860f3-114">`Build` a `Run` metody sestavení `IWebHost` objekt, který je hostitelem aplikace a začne naslouchat požadavkům HTTP.</span><span class="sxs-lookup"><span data-stu-id="860f3-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="860f3-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="860f3-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="860f3-116">`Main` Používá metoda `WebHostBuilder`, který odpovídá vzorci Tvůrce vytvořit hostitel webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="860f3-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="860f3-117">Tvůrce obsahuje metody, které definují webového serveru (například `UseKestrel`) a třída při spuštění (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="860f3-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="860f3-118">V předchozím příkladu [Kestrel](xref:fundamentals/servers/kestrel) se používá webový server.</span><span class="sxs-lookup"><span data-stu-id="860f3-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="860f3-119">Další webové servery, jako například [WebListener](xref:fundamentals/servers/weblistener), mohou být využívána volání metody odpovídající rozšíření.</span><span class="sxs-lookup"><span data-stu-id="860f3-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="860f3-120">`UseStartup` je vysvětleno v následující části Další.</span><span class="sxs-lookup"><span data-stu-id="860f3-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="860f3-121">`WebHostBuilder` nabízí mnoho volitelné způsobů, včetně `UseIISIntegration` pro hostování v IIS a služby IIS Express a `UseContentRoot` pro zadání kořenový adresář s obsahem.</span><span class="sxs-lookup"><span data-stu-id="860f3-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="860f3-122">`Build` a `Run` metody sestavení `IWebHost` objekt, který je hostitelem aplikace a začne naslouchat požadavkům HTTP.</span><span class="sxs-lookup"><span data-stu-id="860f3-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

* * *
## <a name="startup"></a><span data-ttu-id="860f3-123">Spuštění</span><span class="sxs-lookup"><span data-stu-id="860f3-123">Startup</span></span>

<span data-ttu-id="860f3-124">`UseStartup` Metodu `WebHostBuilder` Určuje `Startup` třídy pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="860f3-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="860f3-125">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="860f3-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="860f3-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="860f3-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

* * *
<span data-ttu-id="860f3-127">`Startup` Je třída, kde můžete definovat v kanálu zpracování požadavků a které jsou nakonfigurované všechny služby potřebné pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="860f3-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="860f3-128">`Startup` Třída musí být veřejné a musí obsahovat následující metody:</span><span class="sxs-lookup"><span data-stu-id="860f3-128">The `Startup` class must be public and contain the following methods:</span></span>

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

<span data-ttu-id="860f3-129">`ConfigureServices` definuje [služby](#dependency-injection-services) používané vaší aplikace (například Identity Entity Framework Core, základní rozhraní ASP.NET MVC).</span><span class="sxs-lookup"><span data-stu-id="860f3-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="860f3-130">`Configure` definuje [middleware](xref:fundamentals/middleware/index) pro kanál požadavku.</span><span class="sxs-lookup"><span data-stu-id="860f3-130">`Configure` defines the [middleware](xref:fundamentals/middleware/index) for the request pipeline.</span></span>

<span data-ttu-id="860f3-131">Další informace najdete v tématu [spuštění aplikace](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="860f3-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="860f3-132">Obsah kořenové</span><span class="sxs-lookup"><span data-stu-id="860f3-132">Content root</span></span>

<span data-ttu-id="860f3-133">Kořenu obsahu je základní cesta k žádný obsah používané aplikace, jako je například zobrazení, [stránky Razor](xref:mvc/razor-pages/index)a statické prostředky.</span><span class="sxs-lookup"><span data-stu-id="860f3-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="860f3-134">Ve výchozím nastavení kořenu obsahu je stejný jako základní cesta aplikace pro spustitelný soubor, který je hostitelem aplikace.</span><span class="sxs-lookup"><span data-stu-id="860f3-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="860f3-135">Kořenový web</span><span class="sxs-lookup"><span data-stu-id="860f3-135">Web root</span></span>

<span data-ttu-id="860f3-136">Kořenové webové aplikace je adresář v projektu obsahující veřejný, statické prostředky, například šablon stylů CSS, JavaScript a soubory obrázků.</span><span class="sxs-lookup"><span data-stu-id="860f3-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="860f3-137">Vkládání závislostí (služby)</span><span class="sxs-lookup"><span data-stu-id="860f3-137">Dependency injection (services)</span></span>

<span data-ttu-id="860f3-138">Služba je komponenta, která je určená pro běžné používání v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="860f3-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="860f3-139">Služby jsou dostupné prostřednictvím [vkládání závislostí (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="860f3-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="860f3-140">ASP.NET Core zahrnuje nativní **I**nPro **o**f **C**kontejneru obrazit řídicí (IoC), který podporuje [konstruktor vkládání](xref:mvc/controllers/dependency-injection#constructor-injection) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="860f3-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="860f3-141">Výchozí kontejner nativní můžete nahradit, pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="860f3-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="860f3-142">Kromě jeho výhody volné párování DI umožňuje služby k dispozici v celé vaší aplikace (například [protokolování](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="860f3-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="860f3-143">Další informace najdete v tématu [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="860f3-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="860f3-144">Middleware</span><span class="sxs-lookup"><span data-stu-id="860f3-144">Middleware</span></span>

<span data-ttu-id="860f3-145">V ASP.NET Core, vytvoříte pomocí kanálu požadavku [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="860f3-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="860f3-146">ASP.NET Core middleware provede asynchronní logiku `HttpContext` a vyvolá další middleware v pořadí, nebo přímo ukončí žádosti.</span><span class="sxs-lookup"><span data-stu-id="860f3-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="860f3-147">Přidá middleware komponenty s názvem "XYZ" se při vyvolání `UseXYZ` metoda rozšíření v `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="860f3-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="860f3-148">ASP.NET Core obsahuje bohatou sadu předdefinovaných middleware:</span><span class="sxs-lookup"><span data-stu-id="860f3-148">ASP.NET Core includes a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="860f3-149">Statické soubory</span><span class="sxs-lookup"><span data-stu-id="860f3-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="860f3-150">Směrování</span><span class="sxs-lookup"><span data-stu-id="860f3-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="860f3-151">Ověřování</span><span class="sxs-lookup"><span data-stu-id="860f3-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="860f3-152">Middleware pro kompresi odpovědí</span><span class="sxs-lookup"><span data-stu-id="860f3-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="860f3-153">Middleware pro přepis adres URL</span><span class="sxs-lookup"><span data-stu-id="860f3-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="860f3-154">[OWIN](http://owin.org)– na základě middlewaru je k dispozici pro aplikace ASP.NET Core a můžete napsat vlastní vlastní middleware.</span><span class="sxs-lookup"><span data-stu-id="860f3-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="860f3-155">Další informace najdete v tématu [Middleware](xref:fundamentals/middleware/index) a [Open Web Interface pro .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="860f3-155">For more information, see [Middleware](xref:fundamentals/middleware/index) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="initiate-http-requests"></a><span data-ttu-id="860f3-156">Inicializace požadavků HTTP</span><span class="sxs-lookup"><span data-stu-id="860f3-156">Initiate HTTP requests</span></span>

<span data-ttu-id="860f3-157">Informace o používání `IHttpClientFactory` pro přístup k `HttpClient` instancí, aby požadavky HTTP, najdete v části [požadavky HTTP zahájit](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="860f3-157">For information about using `IHttpClientFactory` to access `HttpClient` instances to make HTTP requests, see [Initiate HTTP requests](xref:fundamentals/http-requests).</span></span>

## <a name="environments"></a><span data-ttu-id="860f3-158">Prostředí</span><span class="sxs-lookup"><span data-stu-id="860f3-158">Environments</span></span>

<span data-ttu-id="860f3-159">Prostředí, jako je například "Vývoj" a "Provozní", jsou hodnoty první třídy v ASP.NET Core a se dá nastavit pomocí proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="860f3-159">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="860f3-160">Další informace najdete v tématu [pracovat s několika prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="860f3-160">For more information, see [Work with multiple environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="860f3-161">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="860f3-161">Configuration</span></span>

<span data-ttu-id="860f3-162">Jádro ASP.NET používá model konfigurace na základě párů název hodnota.</span><span class="sxs-lookup"><span data-stu-id="860f3-162">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="860f3-163">Konfigurační model není založen na `System.Configuration` nebo *web.config*. Konfigurace obsahuje nastavení ze seřazené sadu poskytovatelů konfigurace.</span><span class="sxs-lookup"><span data-stu-id="860f3-163">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="860f3-164">Poskytovatelé předdefinované konfigurace podporují celou řadu formáty souborů (XML, JSON, INI) a proměnné prostředí umožňující konfiguraci na základě prostředí.</span><span class="sxs-lookup"><span data-stu-id="860f3-164">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="860f3-165">Také můžete napsat vlastního zprostředkovatele vlastní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="860f3-165">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="860f3-166">Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="860f3-166">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="860f3-167">protokolování</span><span class="sxs-lookup"><span data-stu-id="860f3-167">Logging</span></span>

<span data-ttu-id="860f3-168">Jádro ASP.NET podporuje protokolování rozhraní API, která funguje s různými zprostředkovatelů protokolování.</span><span class="sxs-lookup"><span data-stu-id="860f3-168">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="860f3-169">Předdefinované zprostředkovatele podporovat odesílání protokoly na jeden nebo více míst.</span><span class="sxs-lookup"><span data-stu-id="860f3-169">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="860f3-170">Můžete použít rozhraní protokolování třetích stran.</span><span class="sxs-lookup"><span data-stu-id="860f3-170">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="860f3-171">Protokolování</span><span class="sxs-lookup"><span data-stu-id="860f3-171">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="860f3-172">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="860f3-172">Error handling</span></span>

<span data-ttu-id="860f3-173">ASP.NET Core obsahuje integrované funkce pro zpracování chyb v aplikacích, včetně stránky výjimka developer, vlastní chybové stránky, statické stav znakové stránky a spuštění zpracování výjimek.</span><span class="sxs-lookup"><span data-stu-id="860f3-173">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="860f3-174">Další informace najdete v tématu [způsob zpracování chyb](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="860f3-174">For more information, see [how to handle errors](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="860f3-175">Směrování</span><span class="sxs-lookup"><span data-stu-id="860f3-175">Routing</span></span>

<span data-ttu-id="860f3-176">ASP.NET Core nabízí funkce pro směrování požadavků app na obslužné rutiny trasy.</span><span class="sxs-lookup"><span data-stu-id="860f3-176">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="860f3-177">Další informace najdete v tématu [směrování](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="860f3-177">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="860f3-178">Zprostředkovatelé souboru</span><span class="sxs-lookup"><span data-stu-id="860f3-178">File providers</span></span>

<span data-ttu-id="860f3-179">ASP.NET Core abstrahuje přístupu k systému souborů prostřednictvím poskytovatelů souboru, který nabízí společné rozhraní pro práci se soubory napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="860f3-179">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="860f3-180">Další informace najdete v tématu [souboru zprostředkovatelé](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="860f3-180">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="860f3-181">Statické soubory</span><span class="sxs-lookup"><span data-stu-id="860f3-181">Static files</span></span>

<span data-ttu-id="860f3-182">Statické soubory middleware poskytuje statické soubory, jako je například HTML, CSS, image a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="860f3-182">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="860f3-183">Další informace najdete v tématu [pracovat s statické soubory](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="860f3-183">For more information, see [Work with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="860f3-184">Hostování</span><span class="sxs-lookup"><span data-stu-id="860f3-184">Hosting</span></span>

<span data-ttu-id="860f3-185">Aplikace ASP.NET Core nakonfigurovat a spustit *hostitele*, která je zodpovědná za spuštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="860f3-185">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="860f3-186">Další informace najdete v tématu [hostitelský](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="860f3-186">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="860f3-187">Stav relace a aplikace</span><span class="sxs-lookup"><span data-stu-id="860f3-187">Session and application state</span></span>

<span data-ttu-id="860f3-188">Stav relace je funkce v ASP.NET Core, který můžete použít k ukládání a uchovávání dat uživatele, když uživatel prohlíží vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="860f3-188">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="860f3-189">Další informace najdete v tématu [relace a stav aplikace](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="860f3-189">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="860f3-190">Servery</span><span class="sxs-lookup"><span data-stu-id="860f3-190">Servers</span></span>

<span data-ttu-id="860f3-191">Model hostování základní technologie ASP.NET není přímo naslouchat požadavkům.</span><span class="sxs-lookup"><span data-stu-id="860f3-191">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="860f3-192">Model hostování závisí na implementaci serveru HTTP k předání požadavku do aplikace.</span><span class="sxs-lookup"><span data-stu-id="860f3-192">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="860f3-193">Předaný požadavek je zabalená jako sada objektů funkce, které je přístupné prostřednictvím rozhraní.</span><span class="sxs-lookup"><span data-stu-id="860f3-193">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="860f3-194">ASP.NET Core zahrnuje spravovaná, a platformy webového serveru, nazývá [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="860f3-194">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="860f3-195">Kestrel je často spouštět za produkční webového serveru, jako například [IIS](https://www.iis.net/) nebo [Nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="860f3-195">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="860f3-196">Kestrel můžete spouštět jako hraniční server.</span><span class="sxs-lookup"><span data-stu-id="860f3-196">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="860f3-197">Další informace najdete v tématu [servery](xref:fundamentals/servers/index) a v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="860f3-197">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="860f3-198">Kestrel</span><span class="sxs-lookup"><span data-stu-id="860f3-198">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="860f3-199">Modul ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="860f3-199">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="860f3-200">[Ovladač HTTP.sys](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="860f3-200">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="860f3-201">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="860f3-201">Globalization and localization</span></span>

<span data-ttu-id="860f3-202">Vytvoření vícejazyčné webu pomocí ASP.NET Core umožňuje vaší lokality k dosažení širší cílovou skupinu.</span><span class="sxs-lookup"><span data-stu-id="860f3-202">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="860f3-203">Základní technologie ASP.NET poskytuje služby a middleware pro lokalizace do různých jazyků a kultur.</span><span class="sxs-lookup"><span data-stu-id="860f3-203">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="860f3-204">Další informace najdete v tématu [globalizace a lokalizace](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="860f3-204">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="860f3-205">Požadavky na funkce</span><span class="sxs-lookup"><span data-stu-id="860f3-205">Request features</span></span>

<span data-ttu-id="860f3-206">Související podrobnosti implementace webového serveru na požadavky HTTP a odpovědí jsou definovány v rozhraní.</span><span class="sxs-lookup"><span data-stu-id="860f3-206">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="860f3-207">Tato rozhraní jsou používány implementací serveru a middleware, vytvářet a upravovat hostování kanálu aplikace.</span><span class="sxs-lookup"><span data-stu-id="860f3-207">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="860f3-208">Další informace najdete v tématu [žádosti o funkce](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="860f3-208">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="background-tasks"></a><span data-ttu-id="860f3-209">Úlohy na pozadí</span><span class="sxs-lookup"><span data-stu-id="860f3-209">Background tasks</span></span>

<span data-ttu-id="860f3-210">Úlohy na pozadí jsou implementované jako *hostovaných služeb*.</span><span class="sxs-lookup"><span data-stu-id="860f3-210">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="860f3-211">Hostovaná služba je třída s pozadí úloh logiky, která implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="860f3-211">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span>

<span data-ttu-id="860f3-212">Další informace najdete v tématu [pozadí úlohy s hostované služby](xref:fundamentals/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="860f3-212">For more information, see [Background tasks with hosted services](xref:fundamentals/hosted-services).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="860f3-213">Spustit nástroj webové rozhraní pro platformu .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="860f3-213">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="860f3-214">Jádro ASP.NET podporuje Open Web Interface pro .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="860f3-214">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="860f3-215">OWIN umožňuje webových aplikací pro být odděleno od webové servery.</span><span class="sxs-lookup"><span data-stu-id="860f3-215">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="860f3-216">Další informace najdete v tématu [Open Web Interface pro .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="860f3-216">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="860f3-217">Technologie WebSockets</span><span class="sxs-lookup"><span data-stu-id="860f3-217">WebSockets</span></span>

<span data-ttu-id="860f3-218">[Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="860f3-218">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="860f3-219">Použije se pro aplikace, jako je například chat, kurzů akcií, hry a kdekoli požadavky v reálném čase funkce ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="860f3-219">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="860f3-220">Jádro ASP.NET podporuje webových funkcí produktu soketu.</span><span class="sxs-lookup"><span data-stu-id="860f3-220">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="860f3-221">Další informace najdete v tématu [Websocket](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="860f3-221">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="860f3-222">Microsoft.AspNetCore.All metapackage</span><span class="sxs-lookup"><span data-stu-id="860f3-222">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="860f3-223">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage pro ASP.NET Core zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="860f3-223">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="860f3-224">Všechny podporované balíčky tým ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="860f3-224">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="860f3-225">Všechny podporované balíčků základní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="860f3-225">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="860f3-226">Interní a 3. stran závislosti používané ASP.NET Core a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="860f3-226">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="860f3-227">Další informace najdete v tématu [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="860f3-227">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="860f3-228">Modul runtime rozhraní .NET core a rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="860f3-228">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="860f3-229">Aplikace ASP.NET Core, můžete vybrat runtime .NET Core nebo rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="860f3-229">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="860f3-230">Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="860f3-230">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="860f3-231">Zvolte mezi ASP.NET Core a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="860f3-231">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="860f3-232">Další informace o výběru mezi ASP.NET Core a ASP.NET naleznete v tématu [zvolte mezi ASP.NET Core a ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="860f3-232">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
