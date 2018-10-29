---
title: Základy ASP.NET Core
author: rick-anderson
description: Seznamte se základními koncepty pro vytváření aplikací ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/index
ms.openlocfilehash: ab140051648c1640b3c4f382bfd8201c5c0c2039
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207469"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="66ba0-103">Základy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66ba0-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="66ba0-104">Aplikace ASP.NET Core je konzolová aplikace, která vytvoří webovým serverem v jeho `Program.Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="66ba0-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="66ba0-105">`Main` Aplikace je metoda *spravované vstupní bod*:</span><span class="sxs-lookup"><span data-stu-id="66ba0-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="66ba0-106">.NET Core hostitele:</span><span class="sxs-lookup"><span data-stu-id="66ba0-106">The .NET Core Host:</span></span>

* <span data-ttu-id="66ba0-107">Načtení [.NET Core runtime](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="66ba0-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="66ba0-108">První argument příkazového řádku používá jako cestu pro spravované binární soubor, který obsahuje vstupní bod (`Main`) a zahájí provádění kódu.</span><span class="sxs-lookup"><span data-stu-id="66ba0-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="66ba0-109">V metodě `Main` se volá metoda [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), která poskytuje implementaci návrhového vzoru [Builder](https://wikipedia.org/wiki/Builder_pattern) a umožňuje tak sestavení hostitele webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="66ba0-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="66ba0-110">Samotný Builder obsahuje metody, které specifikují webový server (například <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>), který se má použít, a třídu pro spuštění (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="66ba0-110">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="66ba0-111">Ve výše uvedeném příkladu je automaticky použit webový server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="66ba0-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="66ba0-112">ASP.NET Core se zároveň pokusí o spuštění aplikace ve službě IIS, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="66ba0-112">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="66ba0-113">Voláním patřičné rozšiřující metody je možné aplikaci hostovat i na jiných webových serverech, jako například [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="66ba0-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="66ba0-114">`UseStartup` je vysvětleno v další části.</span><span class="sxs-lookup"><span data-stu-id="66ba0-114">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="66ba0-115">Typ <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, který je vrácen jako výsledek volání `WebHost.CreateDefaultBuilder`, obsahuje mnoho volitelných metod.</span><span class="sxs-lookup"><span data-stu-id="66ba0-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="66ba0-116">Jednou z těchto metod je `UseHttpSys` pro hostování aplikace pomocí HTTP.sys a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pro určení kořenového adresáře s obsahem.</span><span class="sxs-lookup"><span data-stu-id="66ba0-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="66ba0-117">Metody <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> slouží k sestavení objektu <xref:Microsoft.AspNetCore.Hosting.IWebHost>, který je hostitelem aplikace, resp. zahájí naslouchání HTTP požadavků.</span><span class="sxs-lookup"><span data-stu-id="66ba0-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="66ba0-118">.NET Core hostitele:</span><span class="sxs-lookup"><span data-stu-id="66ba0-118">The .NET Core Host:</span></span>

* <span data-ttu-id="66ba0-119">Načtení [.NET Core runtime](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="66ba0-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="66ba0-120">První argument příkazového řádku používá jako cestu pro spravované binární soubor, který obsahuje vstupní bod (`Main`) a zahájí provádění kódu.</span><span class="sxs-lookup"><span data-stu-id="66ba0-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="66ba0-121">V metodě `Main` se používá třída <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, která implementuje návrhový vzor [Builder](https://wikipedia.org/wiki/Builder_pattern) a umožňuje tak sestavení hostitele webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="66ba0-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="66ba0-122">Samotný Builder obsahuje metody, které specifikují webový server (například <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>), který se má použít, a třídu pro spuštění (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="66ba0-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="66ba0-123">Ve výše uvedeném příkladu je použit webový server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="66ba0-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="66ba0-124">Voláním patřičné rozšiřující metody je možné aplikaci hostovat i na jiných webových serverech, jako například [WebListener](xref:fundamentals/servers/weblistener). </span><span class="sxs-lookup"><span data-stu-id="66ba0-124">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="66ba0-125">`UseStartup` je vysvětleno v další části.</span><span class="sxs-lookup"><span data-stu-id="66ba0-125">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="66ba0-126">Třída `WebHostBuilder` obsahuje mnoho volitelných metod. Jednou z těchto metod je <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> pro hostování aplikace pomocí služby IIS nebo IIS Express a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pro určení kořenového adresáře s obsahem.</span><span class="sxs-lookup"><span data-stu-id="66ba0-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="66ba0-127">Metody <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> slouží k sestavení objektu <xref:Microsoft.AspNetCore.Hosting.IWebHost>, který je hostitelem aplikace, resp. zahájí naslouchání HTTP požadavků.</span><span class="sxs-lookup"><span data-stu-id="66ba0-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="66ba0-128">Třída pro spuštění</span><span class="sxs-lookup"><span data-stu-id="66ba0-128">Startup</span></span>

<span data-ttu-id="66ba0-129">Metoda `UseStartup` třídy `WebHostBuilder` určuje spouštěcí třídu `Startup` Vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="66ba0-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="66ba0-130">Ve třídě `Startup` můžete definovat kanál zpracování požadavků a nakonfigurovat všechny služby, které aplikace vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="66ba0-130">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="66ba0-131">Třída `Startup` musí být veřejná a musí obsahovat následující metody:</span><span class="sxs-lookup"><span data-stu-id="66ba0-131">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="66ba0-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> definuje [služby](#dependency-injection-services) používané v aplikaci (například ASP.NET, Core MVC, Entity Framework Core, Identity).</span><span class="sxs-lookup"><span data-stu-id="66ba0-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="66ba0-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> definuje [middlewary](xref:fundamentals/middleware/index) pro kanál zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="66ba0-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="66ba0-134">Další informace naleznete v tématu <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-134">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="66ba0-135">Kořen obsahu</span><span class="sxs-lookup"><span data-stu-id="66ba0-135">Content root</span></span>

<span data-ttu-id="66ba0-136">Kořen obsahu (Content Root) je bázová cesta k libovolnému obsahu využívaného aplikací, jako jsou například pohledy, [Stránky Razor](xref:razor-pages/index) a statický obsah (obrázky apod.).</span><span class="sxs-lookup"><span data-stu-id="66ba0-136">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="66ba0-137">Ve výchozím nastavení je kořenu obsahu stejný jako bázová cesta aplikace ke spustitelnému souboru, který je hostitelem aplikace.</span><span class="sxs-lookup"><span data-stu-id="66ba0-137">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="66ba0-138">Kořenový adresář webové (webroot)</span><span class="sxs-lookup"><span data-stu-id="66ba0-138">Web root (webroot)</span></span>

<span data-ttu-id="66ba0-139">Webroot aplikace je adresář, do projektu obsahující veřejné, statické prostředky, jako jsou šablony stylů CSS, JavaScript a soubory obrázků.</span><span class="sxs-lookup"><span data-stu-id="66ba0-139">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="66ba0-140">Ve výchozím nastavení *wwwroot* je webroot.</span><span class="sxs-lookup"><span data-stu-id="66ba0-140">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="66ba0-141">Pro syntaxi Razor (*.cshtml*) soubory tilda lomítky `~/` odkazuje webroot.</span><span class="sxs-lookup"><span data-stu-id="66ba0-141">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="66ba0-142">Cesty začínající `~/` se označují jako virtuální cesty.</span><span class="sxs-lookup"><span data-stu-id="66ba0-142">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="66ba0-143">Vkládání závislostí (služby)</span><span class="sxs-lookup"><span data-stu-id="66ba0-143">Dependency injection (services)</span></span>

<span data-ttu-id="66ba0-144">*Služba* je komponenta, která je určená pro společné používání v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="66ba0-144">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="66ba0-145">Služby jsou zpřístupněny prostřednictvím [vkládání závislosti](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="66ba0-145">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="66ba0-146">ASP.NET Core zahrnuje nativní kontejner pro inverzi závislostí, který podporuje [vkládání závislostí pomocí konstruktoru](xref:mvc/controllers/dependency-injection#constructor-injection).</span><span class="sxs-lookup"><span data-stu-id="66ba0-146">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="66ba0-147">Nativní kontejner je možné nahradit jiným.</span><span class="sxs-lookup"><span data-stu-id="66ba0-147">You can replace the default container if you wish.</span></span> <span data-ttu-id="66ba0-148">Výhoda použití vkládání závislostí spočívá ve [volnějším propojení jednotlivých komponent](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) a zpřístupnění služeb (jako je třeba [protokolování](xref:fundamentals/logging/index)) všude v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="66ba0-148">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="66ba0-149">Další informace naleznete v tématu <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-149">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="66ba0-150">Middleware</span><span class="sxs-lookup"><span data-stu-id="66ba0-150">Middleware</span></span>

<span data-ttu-id="66ba0-151">V ASP.NET Core budujete kanál zpracování požadavků pomocí [middlewarů](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="66ba0-151">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="66ba0-152">ASP.NET Core middleware nejprve provádí asynchronní operace nad kontextem `HttpContext`, a následně může předat řízení dalšímu middlewaru v pořadí nebo okamžitě ukončit zpracování HTTP požadavku.</span><span class="sxs-lookup"><span data-stu-id="66ba0-152">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="66ba0-153">Middlewarová komponenta s názvem "XYZ" se přidá pomocí volání rozšiřující metody `UseXYZ` uvnitř metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="66ba0-153">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="66ba0-154">ASP.NET Core obsahuje pestrý výběr vestavěných middlewarů, nebo můžete použít vlastní middleware.</span><span class="sxs-lookup"><span data-stu-id="66ba0-154">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="66ba0-155">[Otevřete Web Interface pro .NET (OWIN)](xref:fundamentals/owin), který umožňuje webové aplikace k oddělení od webové servery, je podporováno v aplikacích pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="66ba0-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="66ba0-156">Další informace naleznete v tématu <xref:fundamentals/middleware/index> a <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-156">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="66ba0-157">Iniciování HTTP požadavků</span><span class="sxs-lookup"><span data-stu-id="66ba0-157">Initiate HTTP requests</span></span>

<span data-ttu-id="66ba0-158"><xref:System.Net.Http.IHttpClientFactory> je k dispozici pro přístup k <xref:System.Net.Http.HttpClient> instance požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="66ba0-158"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="66ba0-159">Další informace naleznete v tématu <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-159">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="66ba0-160">Prostředí</span><span class="sxs-lookup"><span data-stu-id="66ba0-160">Environments</span></span>

<span data-ttu-id="66ba0-161">ASP.NET Core umožňuje rozlišení vývojového *Development* a produkčního *Production* prostředí, konkrétní prostředí lze nastavit pomocí proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="66ba0-161">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="66ba0-162">Další informace naleznete v tématu <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-162">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="66ba0-163">Hostování</span><span class="sxs-lookup"><span data-stu-id="66ba0-163">Hosting</span></span>

<span data-ttu-id="66ba0-164">Aplikace ASP.NET Core konfiguruje a spouští *hostitele*, který je zodpovědný za správu životního cyklu aplikace.</span><span class="sxs-lookup"><span data-stu-id="66ba0-164">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="66ba0-165">Další informace naleznete v tématu <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-165">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="66ba0-166">Servery</span><span class="sxs-lookup"><span data-stu-id="66ba0-166">Servers</span></span>

<span data-ttu-id="66ba0-167">ASP.NET Core nenaslouchá přímo HTTP požadavkům.</span><span class="sxs-lookup"><span data-stu-id="66ba0-167">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="66ba0-168">Spoléhá na HTTP server, který aplikaci předává jednotlivé požadavky.</span><span class="sxs-lookup"><span data-stu-id="66ba0-168">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="66ba0-169">Předaný požadavek je zabalen do objektů, ke kterým je možné přistupovat skrz rozhraní.</span><span class="sxs-lookup"><span data-stu-id="66ba0-169">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="66ba0-170">ASP.NET Core obsahuje zabudovaný multiplatformní webový server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="66ba0-170">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="66ba0-171">Kestrel může běžet v pozadí produkčního webového serveru, jako je například [IIS](https://www.iis.net/) nebo [Nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="66ba0-171">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="66ba0-172">Kestrel můžete také spustit jako veřejnou hraniční server přístup přímo k Internetu v ASP.NET Core 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="66ba0-172">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="66ba0-173">Další informace naleznete v tématu <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-173">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="66ba0-174">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="66ba0-174">Configuration</span></span>

<span data-ttu-id="66ba0-175">ASP.NET Core využívá konfigurační model založený na dvojicích název-hodnota.</span><span class="sxs-lookup"><span data-stu-id="66ba0-175">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="66ba0-176">Konfigurační model již není založen na <xref:System.Configuration> nebo *web.config*. Hodnoty konfigurace jsou získávány z uspořádané množiny zprostředkovatelů konfigurace.</span><span class="sxs-lookup"><span data-stu-id="66ba0-176">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="66ba0-177">Vestavění zprostředkovatelé konfigurace podporují množství standardních formátů (XML, JSON, INI) nebo konfiguraci pomocí proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="66ba0-177">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="66ba0-178">Můžete vytvořit i vlastního zprostředkovatele konfigurace.</span><span class="sxs-lookup"><span data-stu-id="66ba0-178">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="66ba0-179">Další informace naleznete v tématu <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-179">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="66ba0-180">Protokolování</span><span class="sxs-lookup"><span data-stu-id="66ba0-180">Logging</span></span>

<span data-ttu-id="66ba0-181">ASP.NET Core poskytuje obecné API pro protokolování, které spolupracuje s různými zprostředkovateli protokolování.</span><span class="sxs-lookup"><span data-stu-id="66ba0-181">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="66ba0-182">Předdefinovaní zprostředkovatelé protokolování podporují odesílání záznamů do jednoho i více míst zároveň.</span><span class="sxs-lookup"><span data-stu-id="66ba0-182">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="66ba0-183">Použít je možné i zprostředkovatele třetích stran.</span><span class="sxs-lookup"><span data-stu-id="66ba0-183">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="66ba0-184">Další informace naleznete v tématu <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-184">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="66ba0-185">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="66ba0-185">Error handling</span></span>

<span data-ttu-id="66ba0-186">ASP.NET Core má integrované funkce pro zpracování chyb v aplikacích, vč. stránky pro diagnostiku výjimek, vlastní chybové stránky a zpracování výjimek při spuštění.</span><span class="sxs-lookup"><span data-stu-id="66ba0-186">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="66ba0-187">Další informace naleznete v tématu <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-187">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="66ba0-188">Směrování</span><span class="sxs-lookup"><span data-stu-id="66ba0-188">Routing</span></span>

<span data-ttu-id="66ba0-189">ASP.NET Core nabízí funkce pro směrování požadavků do obslužné rutiny směrovače.</span><span class="sxs-lookup"><span data-stu-id="66ba0-189">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="66ba0-190">Další informace naleznete v tématu <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-190">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="66ba0-191">Úlohy na pozadí</span><span class="sxs-lookup"><span data-stu-id="66ba0-191">Background tasks</span></span>

<span data-ttu-id="66ba0-192">Úlohy na pozadí jsou implementovány jako *hostované služby*.</span><span class="sxs-lookup"><span data-stu-id="66ba0-192">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="66ba0-193">Hostovaná služba je třída s logikou úlohy na pozadí a implementuje rozhraní <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-193">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="66ba0-194">Další informace naleznete v tématu <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-194">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="66ba0-195">Přístup k objektu HttpContext</span><span class="sxs-lookup"><span data-stu-id="66ba0-195">Access HttpContext</span></span>

<span data-ttu-id="66ba0-196">`HttpContext` Při zpracování žádosti se stránkami Razor a technologií MVC je automaticky k dispozici.</span><span class="sxs-lookup"><span data-stu-id="66ba0-196">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="66ba0-197">V případech kdy `HttpContext` není snadno k dispozici, můžete přistupovat `HttpContext` prostřednictvím <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> rozhraní a jeho výchozí implementace <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-197">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="66ba0-198">Další informace naleznete v tématu <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="66ba0-198">For more information, see <xref:fundamentals/httpcontext>.</span></span>
