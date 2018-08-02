---
title: Základy ASP.NET Core
author: rick-anderson
description: Seznamte se základními koncepty pro vytváření aplikací ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 07/02/2018
uid: fundamentals/index
ms.openlocfilehash: 8e0198e2975192e6522c4821741aacc7a844000b
ms.sourcegitcommit: 571d76fbbff05e84406b6d909c8fe9cbea2c8ff1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/01/2018
ms.locfileid: "39410088"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="a940b-103">Základy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a940b-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="a940b-104">Aplikace ASP.NET Core je konzolová aplikace, která vytvoří webovým serverem v jeho `Main` metody:</span><span class="sxs-lookup"><span data-stu-id="a940b-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a940b-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a940b-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="a940b-106">`Main` Metoda vyvolá `WebHost.CreateDefaultBuilder`, která používá vzor Tvůrce vytvořit hostitel webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a940b-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="a940b-107">Tvůrce obsahuje metody, které definují webového serveru (například `UseKestrel`) a třída při spuštění (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="a940b-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="a940b-108">V předchozím příkladu [Kestrel](xref:fundamentals/servers/kestrel) automaticky přiděluje webový server.</span><span class="sxs-lookup"><span data-stu-id="a940b-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="a940b-109">Webového hostitele ASP.NET Core se pokusí o spuštění ve službě IIS, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a940b-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="a940b-110">Další webové servery, jako například [HTTP.sys](xref:fundamentals/servers/httpsys), je možné vyvoláním metody odpovídající rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a940b-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="a940b-111">`UseStartup` je vysvětleno v další části.</span><span class="sxs-lookup"><span data-stu-id="a940b-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="a940b-112">`IWebHostBuilder`, návratový typ `WebHost.CreateDefaultBuilder` vyvolání, obsahuje mnoho metod volitelné.</span><span class="sxs-lookup"><span data-stu-id="a940b-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="a940b-113">Tyto metody patří k `UseHttpSys` pro hostování aplikace v souboru HTTP.sys a `UseContentRoot` pro zadání kořenový adresář s obsahem.</span><span class="sxs-lookup"><span data-stu-id="a940b-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="a940b-114">`Build` a `Run` metody sestavení `IWebHost` objekt, který je hostitelem aplikace a začne naslouchat požadavkům HTTP.</span><span class="sxs-lookup"><span data-stu-id="a940b-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a940b-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a940b-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="a940b-116">`Main` Používá metoda `WebHostBuilder`, která používá vzor Tvůrce vytvořit hostitel webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a940b-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="a940b-117">Tvůrce obsahuje metody, které definují webového serveru (například `UseKestrel`) a třída při spuštění (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="a940b-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="a940b-118">V předchozím příkladu [Kestrel](xref:fundamentals/servers/kestrel) se používá webový server.</span><span class="sxs-lookup"><span data-stu-id="a940b-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="a940b-119">Další webové servery, jako například [WebListener](xref:fundamentals/servers/weblistener), je možné vyvoláním metody odpovídající rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a940b-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="a940b-120">`UseStartup` je vysvětleno v další části.</span><span class="sxs-lookup"><span data-stu-id="a940b-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="a940b-121">`WebHostBuilder` poskytuje mnoho volitelné metod, včetně `UseIISIntegration` k hostování ve službě IIS a služby IIS Express a `UseContentRoot` pro zadání kořenový adresář s obsahem.</span><span class="sxs-lookup"><span data-stu-id="a940b-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="a940b-122">`Build` a `Run` metody sestavení `IWebHost` objekt, který je hostitelem aplikace a začne naslouchat požadavkům HTTP.</span><span class="sxs-lookup"><span data-stu-id="a940b-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="a940b-123">Po spuštění</span><span class="sxs-lookup"><span data-stu-id="a940b-123">Startup</span></span>

<span data-ttu-id="a940b-124">`UseStartup` Metoda `WebHostBuilder` Určuje `Startup` třídy pro vaši aplikaci:</span><span class="sxs-lookup"><span data-stu-id="a940b-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a940b-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a940b-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a940b-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a940b-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="a940b-127">`Startup` Třída je tady můžete definovat kanál zpracování požadavků a které jsou nakonfigurované všechny služby vyžaduje aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a940b-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="a940b-128">`Startup` Třída musí být veřejné a musí obsahovat následující metody:</span><span class="sxs-lookup"><span data-stu-id="a940b-128">The `Startup` class must be public and contain the following methods:</span></span>

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

<span data-ttu-id="a940b-129">`ConfigureServices` definuje [služby](#dependency-injection-services) používané v aplikaci (například ASP.NET, MVC Core a Entity Framework Core Identity).</span><span class="sxs-lookup"><span data-stu-id="a940b-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="a940b-130">`Configure` definuje [middleware](xref:fundamentals/middleware/index) pro kanál žádosti.</span><span class="sxs-lookup"><span data-stu-id="a940b-130">`Configure` defines the [middleware](xref:fundamentals/middleware/index) for the request pipeline.</span></span>

<span data-ttu-id="a940b-131">Další informace najdete v tématu [spuštění aplikace](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a940b-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="a940b-132">Kořenové obsahu</span><span class="sxs-lookup"><span data-stu-id="a940b-132">Content root</span></span>

<span data-ttu-id="a940b-133">Obsahu kořenový adresář je základní cesta k obsahu používat aplikace, jako je například zobrazení, [Razor Pages](xref:razor-pages/index)a statické prostředky.</span><span class="sxs-lookup"><span data-stu-id="a940b-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:razor-pages/index), and static assets.</span></span> <span data-ttu-id="a940b-134">Ve výchozím nastavení kořenu obsahu je stejný jako základní cesta k aplikaci pro spustitelný soubor, který je hostitelem aplikace.</span><span class="sxs-lookup"><span data-stu-id="a940b-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="a940b-135">Kořenový adresář webové</span><span class="sxs-lookup"><span data-stu-id="a940b-135">Web root</span></span>

<span data-ttu-id="a940b-136">Kořenový adresář webové aplikace je adresář, do projektu obsahující veřejné, statické prostředky, jako jsou šablony stylů CSS, JavaScript a soubory obrázků.</span><span class="sxs-lookup"><span data-stu-id="a940b-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="a940b-137">Injektáž závislostí (služby)</span><span class="sxs-lookup"><span data-stu-id="a940b-137">Dependency injection (services)</span></span>

<span data-ttu-id="a940b-138">Služba je komponenta, která je určená pro běžné používání v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a940b-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="a940b-139">Služby jsou k dispozici prostřednictvím [injektáž závislostí (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a940b-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a940b-140">ASP.NET Core zahrnuje nativní **můžu**nPro **o**f **C**kontejner pro ojů (Inversion), který podporuje [konstruktor vkládání](xref:mvc/controllers/dependency-injection#constructor-injection) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="a940b-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="a940b-141">Pokud chcete, můžete nahradit výchozí kontejner nativní.</span><span class="sxs-lookup"><span data-stu-id="a940b-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="a940b-142">Kromě jeho výhody volné párování DI umožňuje službám k dispozici v rámci vaší aplikace (například [protokolování](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="a940b-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="a940b-143">Další informace najdete v tématu [injektáž závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a940b-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="a940b-144">Middleware</span><span class="sxs-lookup"><span data-stu-id="a940b-144">Middleware</span></span>

<span data-ttu-id="a940b-145">V ASP.NET Core, vytvoříte pomocí kanálu požadavku [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="a940b-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="a940b-146">ASP.NET Core middleware provádí asynchronní logiku `HttpContext` a potom vyvolá další middleware v řadě nebo ukončí žádost přímo.</span><span class="sxs-lookup"><span data-stu-id="a940b-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="a940b-147">Přidá middleware komponenty s názvem "XYZ" se při volání `UseXYZ` metody rozšíření v `Configure` – metoda.</span><span class="sxs-lookup"><span data-stu-id="a940b-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="a940b-148">ASP.NET Core obsahuje bohatou sadu integrovaných middleware:</span><span class="sxs-lookup"><span data-stu-id="a940b-148">ASP.NET Core includes a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="a940b-149">Statické soubory</span><span class="sxs-lookup"><span data-stu-id="a940b-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="a940b-150">Směrování</span><span class="sxs-lookup"><span data-stu-id="a940b-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="a940b-151">Ověřování</span><span class="sxs-lookup"><span data-stu-id="a940b-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="a940b-152">Middleware pro kompresi odpovědí</span><span class="sxs-lookup"><span data-stu-id="a940b-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="a940b-153">Middleware pro přepis adres URL</span><span class="sxs-lookup"><span data-stu-id="a940b-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="a940b-154">[OWIN](http://owin.org)– na základě middlewaru je k dispozici pro aplikace ASP.NET Core a můžete napsat vlastního middlewaru.</span><span class="sxs-lookup"><span data-stu-id="a940b-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="a940b-155">Další informace najdete v tématu [Middleware](xref:fundamentals/middleware/index) a [Open Web Interface pro .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="a940b-155">For more information, see [Middleware](xref:fundamentals/middleware/index) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="a940b-156">Iniciování požadavků HTTP</span><span class="sxs-lookup"><span data-stu-id="a940b-156">Initiate HTTP requests</span></span>

<span data-ttu-id="a940b-157">Informace o používání `IHttpClientFactory` přístup `HttpClient` instance požadavků protokolu HTTP, naleznete v tématu [požadavky HTTP zahájit](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="a940b-157">For information about using `IHttpClientFactory` to access `HttpClient` instances to make HTTP requests, see [Initiate HTTP requests](xref:fundamentals/http-requests).</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="a940b-158">Prostředí</span><span class="sxs-lookup"><span data-stu-id="a940b-158">Environments</span></span>

<span data-ttu-id="a940b-159">Prostředí, jako je "Vývoj" a "Produkční" jsou hodnoty první třídy v ASP.NET Core a lze nastavit pomocí proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="a940b-159">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="a940b-160">Další informace najdete v tématu [používání více prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a940b-160">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="a940b-161">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a940b-161">Configuration</span></span>

<span data-ttu-id="a940b-162">ASP.NET Core využívá konfigurační model založený na páry název hodnota.</span><span class="sxs-lookup"><span data-stu-id="a940b-162">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="a940b-163">Konfigurační model není založen na `System.Configuration` nebo *web.config*. Konfigurace nastavení získá ze seřazené sady poskytovatelů konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a940b-163">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="a940b-164">Poskytovatelé konfigurace integrovanou podporu širokou škálu formátů souborů (XML, JSON, INI) a proměnných prostředí umožňující konfigurace podle prostředí.</span><span class="sxs-lookup"><span data-stu-id="a940b-164">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="a940b-165">Můžete taky psát vlastní zprostředkovatele vlastní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a940b-165">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="a940b-166">Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="a940b-166">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="a940b-167">protokolování</span><span class="sxs-lookup"><span data-stu-id="a940b-167">Logging</span></span>

<span data-ttu-id="a940b-168">ASP.NET Core podporuje protokolování rozhraní API, která funguje s různých poskytovatelů protokolování.</span><span class="sxs-lookup"><span data-stu-id="a940b-168">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="a940b-169">Předdefinované zprostředkovatele podpory odesílání protokolů do jednoho nebo více cílů.</span><span class="sxs-lookup"><span data-stu-id="a940b-169">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="a940b-170">Můžete použít rozhraní protokolování třetích stran.</span><span class="sxs-lookup"><span data-stu-id="a940b-170">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="a940b-171">Další informace najdete v tématu [protokolování](xref:fundamentals/logging/index)</span><span class="sxs-lookup"><span data-stu-id="a940b-171">For more information, see [Logging](xref:fundamentals/logging/index)</span></span>

## <a name="error-handling"></a><span data-ttu-id="a940b-172">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="a940b-172">Error handling</span></span>

<span data-ttu-id="a940b-173">ASP.NET Core má integrované funkce pro zpracování chyb v aplikacích, včetně stránku výjimky pro vývojáře, vlastní chybové stránky, statické stav znakové stránky a zpracování výjimek při spuštění.</span><span class="sxs-lookup"><span data-stu-id="a940b-173">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="a940b-174">Další informace najdete v tématu [zpracování chyb](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="a940b-174">For more information, see [how to handle errors](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="a940b-175">Směrování</span><span class="sxs-lookup"><span data-stu-id="a940b-175">Routing</span></span>

<span data-ttu-id="a940b-176">ASP.NET Core nabízí funkce pro směrování žádostí na aplikace obslužné rutině trasy.</span><span class="sxs-lookup"><span data-stu-id="a940b-176">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="a940b-177">Další informace najdete v tématu [směrování](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="a940b-177">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="a940b-178">Zprostředkovatelé souborů</span><span class="sxs-lookup"><span data-stu-id="a940b-178">File Providers</span></span>

<span data-ttu-id="a940b-179">ASP.NET Core abstrahuje přístupu k systému souborů prostřednictvím zprostředkovatelé souborů, která nabízí obecné rozhraní pro práci se soubory napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="a940b-179">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="a940b-180">Další informace najdete v tématu [zprostředkovatelé souborů](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="a940b-180">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="a940b-181">Statické soubory</span><span class="sxs-lookup"><span data-stu-id="a940b-181">Static files</span></span>

<span data-ttu-id="a940b-182">Statické soubory middleware slouží statické soubory, jako jsou HTML, CSS, image a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a940b-182">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="a940b-183">Další informace najdete v tématu [statické soubory](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="a940b-183">For more information, see [Static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="a940b-184">Hostování</span><span class="sxs-lookup"><span data-stu-id="a940b-184">Hosting</span></span>

<span data-ttu-id="a940b-185">Konfigurace aplikace ASP.NET Core a spouštění *hostitele*, který je zodpovědný za spouštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="a940b-185">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="a940b-186">Další informace najdete v tématu [hostitele v ASP.NET Core](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="a940b-186">For more information, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="a940b-187">Stav relace a aplikace</span><span class="sxs-lookup"><span data-stu-id="a940b-187">Session and app state</span></span>

<span data-ttu-id="a940b-188">ASP.NET Core nabízí několik způsobů zachovat stav relace a aplikace, zatímco uživatel prochází webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a940b-188">ASP.NET Core offers several approaches to preserve session and app state while the user browses a web app.</span></span>

<span data-ttu-id="a940b-189">Další informace najdete v tématu [stav relace a aplikace](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="a940b-189">For more information, see [Session and app state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="a940b-190">Servery</span><span class="sxs-lookup"><span data-stu-id="a940b-190">Servers</span></span>

<span data-ttu-id="a940b-191">ASP.NET Core, model hostingu není přímo naslouchat žádostem.</span><span class="sxs-lookup"><span data-stu-id="a940b-191">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="a940b-192">Model hostingu závisí na implementaci serveru HTTP k předání požadavku do aplikace.</span><span class="sxs-lookup"><span data-stu-id="a940b-192">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="a940b-193">Předaný požadavek je zabalena jako sada objekty funkce, které budou přístupné prostřednictvím rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a940b-193">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="a940b-194">ASP.NET Core zahrnuje spravovaná, napříč platformami webový server, volá [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="a940b-194">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="a940b-195">Kestrel je často spuštění za produkční webový server, například [IIS](https://www.iis.net/) nebo [Nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="a940b-195">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="a940b-196">Kestrel může běžet pod hraniční server.</span><span class="sxs-lookup"><span data-stu-id="a940b-196">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="a940b-197">Další informace najdete v tématu [servery](xref:fundamentals/servers/index) a v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="a940b-197">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="a940b-198">Kestrel</span><span class="sxs-lookup"><span data-stu-id="a940b-198">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="a940b-199">Modul ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a940b-199">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="a940b-200">[Ovladač HTTP.sys](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="a940b-200">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="a940b-201">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="a940b-201">Globalization and localization</span></span>

<span data-ttu-id="a940b-202">Vytvoření vícejazyčné web pomocí ASP.NET Core umožňuje oslovit větší cílovou skupinu webu.</span><span class="sxs-lookup"><span data-stu-id="a940b-202">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="a940b-203">ASP.NET Core poskytuje služby a middleware pro lokalizaci do různých jazyků a kultur.</span><span class="sxs-lookup"><span data-stu-id="a940b-203">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="a940b-204">Další informace najdete v tématu [globalizace a lokalizace](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="a940b-204">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="a940b-205">Funkce požadavků</span><span class="sxs-lookup"><span data-stu-id="a940b-205">Request features</span></span>

<span data-ttu-id="a940b-206">Podrobnosti o implementaci webového serveru související s požadavky HTTP a odpovědí jsou definovány v rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a940b-206">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="a940b-207">Tato rozhraní jsou používány implementací serveru a middleware k vytvoření a úprava kanálu hostitelské aplikace.</span><span class="sxs-lookup"><span data-stu-id="a940b-207">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="a940b-208">Další informace najdete v tématu [požádat o funkce](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="a940b-208">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="background-tasks"></a><span data-ttu-id="a940b-209">Úlohy na pozadí</span><span class="sxs-lookup"><span data-stu-id="a940b-209">Background tasks</span></span>

<span data-ttu-id="a940b-210">Úlohy na pozadí jsou implementovány jako *hostovaných služeb*.</span><span class="sxs-lookup"><span data-stu-id="a940b-210">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="a940b-211">Hostovanou službu je třída s atributem logiky úloh na pozadí, který implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a940b-211">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span>

<span data-ttu-id="a940b-212">Další informace najdete v tématu [s hostovanými službami úloh na pozadí](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="a940b-212">For more information, see [Background tasks with hosted services](xref:fundamentals/host/hosted-services).</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="a940b-213">Přístup k objektu HttpContext</span><span class="sxs-lookup"><span data-stu-id="a940b-213">Access HttpContext</span></span>

<span data-ttu-id="a940b-214">Přístup `HttpContext` prostřednictvím [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) rozhraní a jeho výchozí implementace [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="a940b-214">Access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span>

<span data-ttu-id="a940b-215">Další informace naleznete v tématu <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="a940b-215">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="a940b-216">Open Web Interface pro .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="a940b-216">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="a940b-217">ASP.NET Core podporuje Open Web Interface pro .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="a940b-217">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="a940b-218">OWIN umožňuje webové aplikace k oddělení od webových serverů.</span><span class="sxs-lookup"><span data-stu-id="a940b-218">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="a940b-219">Další informace najdete v tématu [Open Web Interface pro .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="a940b-219">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="a940b-220">Protokoly Websocket</span><span class="sxs-lookup"><span data-stu-id="a940b-220">WebSockets</span></span>

<span data-ttu-id="a940b-221">[Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="a940b-221">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="a940b-222">Používá se pro aplikace, například konverzace, akciích, hry a kdekoli vyžadujete funkcí v reálném čase ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a940b-222">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="a940b-223">ASP.NET Core podporuje funkce webových soketů.</span><span class="sxs-lookup"><span data-stu-id="a940b-223">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="a940b-224">Další informace najdete v tématu [objekty Websocket](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="a940b-224">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="a940b-225">Microsoft.AspNetCore.App Microsoft.aspnetcore.all</span><span class="sxs-lookup"><span data-stu-id="a940b-225">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="a940b-226">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) Microsoft.aspnetcore.all zjednodušuje správu balíčků.</span><span class="sxs-lookup"><span data-stu-id="a940b-226">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span> <span data-ttu-id="a940b-227">Další informace najdete v tématu [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a940b-227">For more information, see [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end
::: moniker range="= aspnetcore-2.0"
## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="a940b-228">Metabalíček Microsoft.aspnetcore.all</span><span class="sxs-lookup"><span data-stu-id="a940b-228">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="a940b-229">[Metabalíček](https://www.nuget.org/packages/Microsoft.AspNetCore.All) Microsoft.aspnetcore.all pro ASP.NET Core zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="a940b-229">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="a940b-230">Všechny podporované balíčky týmem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a940b-230">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="a940b-231">Všechny podporované balíčky pomocí Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a940b-231">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="a940b-232">Interní a 3. stran závislosti používat ASP.NET Core a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a940b-232">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="a940b-233">Další informace najdete v tématu [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="a940b-233">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>
::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="a940b-234">Modul runtime .NET core oproti .NET Framework</span><span class="sxs-lookup"><span data-stu-id="a940b-234">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="a940b-235">Aplikace ASP.NET Core můžete zaměřují na modul runtime .NET Core nebo .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a940b-235">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="a940b-236">Další informace najdete v tématu [volba mezi .NET Core a .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="a940b-236">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="a940b-237">Zvolte mezi ASP.NET Core a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a940b-237">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="a940b-238">Další informace o volbě mezi ASP.NET Core a ASP.NET najdete v tématu [zvolte mezi ASP.NET Core a ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="a940b-238">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
