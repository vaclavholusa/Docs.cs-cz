---
title: Základy ASP.NET Core
author: rick-anderson
description: Seznamte se základními koncepty pro vytváření aplikací ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 68760f179c4d6e806510b727e2284f8c2c4a4ff6
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/20/2018
ms.locfileid: "41754639"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="c8476-103">Základy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8476-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="c8476-104">Aplikace ASP.NET Core je konzolová aplikace, která vytváří webový server ve své `Main` metodě:</span><span class="sxs-lookup"><span data-stu-id="c8476-104">An ASP.NET Core app is a console app that creates a web server in its `Main` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="c8476-105">V metodě `Main` se volá metoda [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), která poskytuje implementaci návrhového vzoru [Builder](https://wikipedia.org/wiki/Builder_pattern) a umožňuje tak sestavení hostitele webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8476-105">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="c8476-106">Samotný Builder obsahuje metody, které specifikují webový server (například <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>), který se má použít, a třídu pro spuštění (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="c8476-106">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="c8476-107">Ve výše uvedeném příkladu je automaticky použit webový server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="c8476-107">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="c8476-108">ASP.NET Core se zároveň pokusí o spuštění aplikace ve službě IIS, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c8476-108">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="c8476-109">Voláním patřičné rozšiřující metody je možné aplikaci hostovat i na jiných webových serverech, jako například [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="c8476-109">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="c8476-110">`UseStartup` je vysvětleno v další části.</span><span class="sxs-lookup"><span data-stu-id="c8476-110">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="c8476-111">Typ <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, který je vrácen jako výsledek volání `WebHost.CreateDefaultBuilder`, obsahuje mnoho volitelných metod.</span><span class="sxs-lookup"><span data-stu-id="c8476-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="c8476-112">Jednou z těchto metod je `UseHttpSys` pro hostování aplikace pomocí HTTP.sys a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pro určení kořenového adresáře s obsahem.</span><span class="sxs-lookup"><span data-stu-id="c8476-112">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="c8476-113">Metody <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> slouží k sestavení objektu <xref:Microsoft.AspNetCore.Hosting.IWebHost>, který je hostitelem aplikace, resp. zahájí naslouchání HTTP požadavků.</span><span class="sxs-lookup"><span data-stu-id="c8476-113">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="c8476-114">V metodě `Main` se používá třída <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, která implementuje návrhový vzor [Builder](https://wikipedia.org/wiki/Builder_pattern) a umožňuje tak sestavení hostitele webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8476-114">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="c8476-115">Samotný Builder obsahuje metody, které specifikují webový server (například <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>), který se má použít, a třídu pro spuštění (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="c8476-115">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="c8476-116">Ve výše uvedeném příkladu je použit webový server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="c8476-116">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="c8476-117">Voláním patřičné rozšiřující metody je možné aplikaci hostovat i na jiných webových serverech, jako například [WebListener](xref:fundamentals/servers/weblistener). </span><span class="sxs-lookup"><span data-stu-id="c8476-117">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="c8476-118">`UseStartup` je vysvětleno v další části.</span><span class="sxs-lookup"><span data-stu-id="c8476-118">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="c8476-119">Třída `WebHostBuilder` obsahuje mnoho volitelných metod. Jednou z těchto metod je <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> pro hostování aplikace pomocí služby IIS nebo IIS Express a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pro určení kořenového adresáře s obsahem.</span><span class="sxs-lookup"><span data-stu-id="c8476-119">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="c8476-120">Metody <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> slouží k sestavení objektu <xref:Microsoft.AspNetCore.Hosting.IWebHost>, který je hostitelem aplikace, resp. zahájí naslouchání HTTP požadavků.</span><span class="sxs-lookup"><span data-stu-id="c8476-120">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="c8476-121">Třída pro spuštění</span><span class="sxs-lookup"><span data-stu-id="c8476-121">Startup</span></span>

<span data-ttu-id="c8476-122">Metoda `UseStartup` třídy `WebHostBuilder` určuje spouštěcí třídu `Startup` Vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="c8476-122">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="c8476-123">Ve třídě `Startup` můžete definovat kanál zpracování požadavků a nakonfigurovat všechny služby, které aplikace vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="c8476-123">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="c8476-124">Třída `Startup` musí být veřejná a musí obsahovat následující metody:</span><span class="sxs-lookup"><span data-stu-id="c8476-124">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="c8476-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> definuje [služby](#dependency-injection-services) používané v aplikaci (například ASP.NET, Core MVC, Entity Framework Core, Identity).</span><span class="sxs-lookup"><span data-stu-id="c8476-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="c8476-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> definuje [middlewary](xref:fundamentals/middleware/index) pro kanál zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="c8476-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="c8476-127">Další informace naleznete v tématu <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="c8476-127">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="c8476-128">Kořen obsahu</span><span class="sxs-lookup"><span data-stu-id="c8476-128">Content root</span></span>

<span data-ttu-id="c8476-129">Kořen obsahu (Content Root) je bázová cesta k libovolnému obsahu využívaného aplikací, jako jsou například pohledy, [Stránky Razor](xref:razor-pages/index) a statický obsah (obrázky apod.).</span><span class="sxs-lookup"><span data-stu-id="c8476-129">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="c8476-130">Ve výchozím nastavení je kořenu obsahu stejný jako bázová cesta aplikace ke spustitelnému souboru, který je hostitelem aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8476-130">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="c8476-131">Kořen webu</span><span class="sxs-lookup"><span data-stu-id="c8476-131">Web root</span></span>

<span data-ttu-id="c8476-132">Kořen webu (Web Root) je adresář projektu, který obsahuje veřejné, statické prostředky, jako jsou např. kaskádové styly, JavaScript a obrázky.</span><span class="sxs-lookup"><span data-stu-id="c8476-132">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="c8476-133">Vkládání závislostí (služby)</span><span class="sxs-lookup"><span data-stu-id="c8476-133">Dependency injection (services)</span></span>

<span data-ttu-id="c8476-134">*Služba* je komponenta, která je určená pro společné používání v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8476-134">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="c8476-135">Služby jsou zpřístupněny prostřednictvím [vkládání závislosti](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c8476-135">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c8476-136">ASP.NET Core zahrnuje nativní kontejner pro inverzi závislostí, který podporuje [vkládání závislostí pomocí konstruktoru](xref:mvc/controllers/dependency-injection#constructor-injection).</span><span class="sxs-lookup"><span data-stu-id="c8476-136">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="c8476-137">Nativní kontejner je možné nahradit jiným.</span><span class="sxs-lookup"><span data-stu-id="c8476-137">You can replace the default container if you wish.</span></span> <span data-ttu-id="c8476-138">Výhoda použití vkládání závislostí spočívá ve [volnějším propojení jednotlivých komponent](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) a zpřístupnění služeb (jako je třeba [protokolování](xref:fundamentals/logging/index)) všude v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c8476-138">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="c8476-139">Další informace naleznete v tématu <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="c8476-139">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="c8476-140">Middleware</span><span class="sxs-lookup"><span data-stu-id="c8476-140">Middleware</span></span>

<span data-ttu-id="c8476-141">V ASP.NET Core budujete kanál zpracování požadavků pomocí [middlewarů](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="c8476-141">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="c8476-142">ASP.NET Core middleware nejprve provádí asynchronní operace nad kontextem `HttpContext`, a následně může předat řízení dalšímu middlewaru v pořadí nebo okamžitě ukončit zpracování HTTP požadavku.</span><span class="sxs-lookup"><span data-stu-id="c8476-142">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="c8476-143">Middlewarová komponenta s názvem "XYZ" se přidá pomocí volání rozšiřující metody `UseXYZ` uvnitř metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c8476-143">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="c8476-144">ASP.NET Core obsahuje pestrý výběr vestavěných middlewarů, nebo můžete použít vlastní middleware.</span><span class="sxs-lookup"><span data-stu-id="c8476-144">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="c8476-145">[Otevřete Web Interface pro .NET (OWIN)](xref:fundamentals/owin), který umožňuje webové aplikace k oddělení od webové servery, je podporováno v aplikacích pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c8476-145">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="c8476-146">Další informace naleznete v tématu <xref:fundamentals/middleware/index> a <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="c8476-146">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="c8476-147">Iniciování HTTP požadavků</span><span class="sxs-lookup"><span data-stu-id="c8476-147">Initiate HTTP requests</span></span>

<span data-ttu-id="c8476-148"><xref:System.Net.Http.IHttpClientFactory> je k dispozici pro přístup k <xref:System.Net.Http.HttpClient> instance požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="c8476-148"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="c8476-149">Další informace naleznete v tématu <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="c8476-149">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="c8476-150">Prostředí</span><span class="sxs-lookup"><span data-stu-id="c8476-150">Environments</span></span>

<span data-ttu-id="c8476-151">ASP.NET Core umožňuje rozlišení vývojového *Development* a produkčního *Production* prostředí, konkrétní prostředí lze nastavit pomocí proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8476-151">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="c8476-152">Další informace naleznete v tématu <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="c8476-152">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="c8476-153">Hostování</span><span class="sxs-lookup"><span data-stu-id="c8476-153">Hosting</span></span>

<span data-ttu-id="c8476-154">Aplikace ASP.NET Core konfiguruje a spouští *hostitele*, který je zodpovědný za správu životního cyklu aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8476-154">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="c8476-155">Další informace naleznete v tématu <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="c8476-155">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="c8476-156">Servery</span><span class="sxs-lookup"><span data-stu-id="c8476-156">Servers</span></span>

<span data-ttu-id="c8476-157">ASP.NET Core nenaslouchá přímo HTTP požadavkům.</span><span class="sxs-lookup"><span data-stu-id="c8476-157">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="c8476-158">Spoléhá na HTTP server, který aplikaci předává jednotlivé požadavky.</span><span class="sxs-lookup"><span data-stu-id="c8476-158">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="c8476-159">Předaný požadavek je zabalen do objektů, ke kterým je možné přistupovat skrz rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c8476-159">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="c8476-160">ASP.NET Core obsahuje zabudovaný multiplatformní webový server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="c8476-160">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="c8476-161">Kestrel může běžet v pozadí produkčního webového serveru, jako je například [IIS](https://www.iis.net/) nebo [Nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="c8476-161">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="c8476-162">Kestrel můžete spustit také jako hraniční server přímo přístupný z Internetu v ASP.NET Core 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c8476-162">Kestrel can also be run as an edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="c8476-163">Další informace naleznete v tématu <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="c8476-163">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="c8476-164">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="c8476-164">Configuration</span></span>

<span data-ttu-id="c8476-165">ASP.NET Core využívá konfigurační model založený na dvojicích název-hodnota.</span><span class="sxs-lookup"><span data-stu-id="c8476-165">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="c8476-166">Konfigurační model již není založen na <xref:System.Configuration> nebo *web.config*. Hodnoty konfigurace jsou získávány z uspořádané množiny zprostředkovatelů konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c8476-166">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="c8476-167">Vestavění zprostředkovatelé konfigurace podporují množství standardních formátů (XML, JSON, INI) nebo konfiguraci pomocí proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8476-167">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="c8476-168">Můžete vytvořit i vlastního zprostředkovatele konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c8476-168">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="c8476-169">Další informace naleznete v tématu <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c8476-169">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="c8476-170">Protokolování</span><span class="sxs-lookup"><span data-stu-id="c8476-170">Logging</span></span>

<span data-ttu-id="c8476-171">ASP.NET Core poskytuje obecné API pro protokolování, které spolupracuje s různými zprostředkovateli protokolování.</span><span class="sxs-lookup"><span data-stu-id="c8476-171">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="c8476-172">Předdefinovaní zprostředkovatelé protokolování podporují odesílání záznamů do jednoho i více míst zároveň.</span><span class="sxs-lookup"><span data-stu-id="c8476-172">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="c8476-173">Použít je možné i zprostředkovatele třetích stran.</span><span class="sxs-lookup"><span data-stu-id="c8476-173">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="c8476-174">Další informace naleznete v tématu <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="c8476-174">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="c8476-175">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="c8476-175">Error handling</span></span>

<span data-ttu-id="c8476-176">ASP.NET Core má integrované funkce pro zpracování chyb v aplikacích, vč. stránky pro diagnostiku výjimek, vlastní chybové stránky a zpracování výjimek při spuštění.</span><span class="sxs-lookup"><span data-stu-id="c8476-176">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="c8476-177">Další informace naleznete v tématu <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="c8476-177">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="c8476-178">Směrování</span><span class="sxs-lookup"><span data-stu-id="c8476-178">Routing</span></span>

<span data-ttu-id="c8476-179">ASP.NET Core nabízí funkce pro směrování požadavků do obslužné rutiny směrovače.</span><span class="sxs-lookup"><span data-stu-id="c8476-179">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="c8476-180">Další informace naleznete v tématu <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="c8476-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="file-providers"></a><span data-ttu-id="c8476-181">Zprostředkovatelé souborů</span><span class="sxs-lookup"><span data-stu-id="c8476-181">File Providers</span></span>

<span data-ttu-id="c8476-182">ASP.NET Core abstrahuje přístup k systému souborů prostřednictvím zprostředkovatelů souborů, nabízí společné rozhraní pro práci se soubory napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="c8476-182">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="c8476-183">Další informace naleznete v tématu <xref:fundamentals/file-providers>.</span><span class="sxs-lookup"><span data-stu-id="c8476-183">For more information, see <xref:fundamentals/file-providers>.</span></span>

## <a name="static-files"></a><span data-ttu-id="c8476-184">Statické soubory</span><span class="sxs-lookup"><span data-stu-id="c8476-184">Static files</span></span>

<span data-ttu-id="c8476-185">Middleware pro statické soubory poskytuje přístup ke statickým souborům, jako například HTML, kaskádovým stylům, obrázkům a JavaScriptu.</span><span class="sxs-lookup"><span data-stu-id="c8476-185">Static Files Middleware serves static files, such as HTML, CSS, image, and JavaScript files.</span></span>

<span data-ttu-id="c8476-186">Další informace naleznete v tématu <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="c8476-186">For more information, see <xref:fundamentals/static-files>.</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="c8476-187">Stav aplikace a relace</span><span class="sxs-lookup"><span data-stu-id="c8476-187">Session and app state</span></span>

<span data-ttu-id="c8476-188">ASP.NET Core nabízí několik způsobů pro uchování stavu aplikace a relace v čase procházení webové aplikace uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c8476-188">ASP.NET Core offers several approaches to preserve session and app state while a user browses a web app.</span></span>

<span data-ttu-id="c8476-189">Další informace naleznete v tématu <xref:fundamentals/app-state>.</span><span class="sxs-lookup"><span data-stu-id="c8476-189">For more information, see <xref:fundamentals/app-state>.</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="c8476-190">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="c8476-190">Globalization and localization</span></span>

<span data-ttu-id="c8476-191">Vytvoření vícejazyčného webu pomocí ASP.NET Core umožňuje oslovit větší počet návštěvníků.</span><span class="sxs-lookup"><span data-stu-id="c8476-191">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="c8476-192">ASP.NET Core poskytuje služby a middleware pro usnadnění lokalizace do různých jazyků a kultur.</span><span class="sxs-lookup"><span data-stu-id="c8476-192">ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>

<span data-ttu-id="c8476-193">Další informace naleznete v tématu <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="c8476-193">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="request-features"></a><span data-ttu-id="c8476-194">Funkce požadavků</span><span class="sxs-lookup"><span data-stu-id="c8476-194">Request features</span></span>

<span data-ttu-id="c8476-195">Implementační detaily webového serveru související s HTTP požadavky a odpověďmi jsou definovány v rozhraních.</span><span class="sxs-lookup"><span data-stu-id="c8476-195">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="c8476-196">Tato rozhraní jsou používána implementacemi serveru a middlewarem k vytvoření a modifikaci kanálu hostingu.</span><span class="sxs-lookup"><span data-stu-id="c8476-196">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="c8476-197">Další informace naleznete v tématu <xref:fundamentals/request-features>.</span><span class="sxs-lookup"><span data-stu-id="c8476-197">For more information, see <xref:fundamentals/request-features>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="c8476-198">Úlohy na pozadí</span><span class="sxs-lookup"><span data-stu-id="c8476-198">Background tasks</span></span>

<span data-ttu-id="c8476-199">Úlohy na pozadí jsou implementovány jako *hostované služby*.</span><span class="sxs-lookup"><span data-stu-id="c8476-199">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="c8476-200">Hostovaná služba je třída s logikou úlohy na pozadí a implementuje rozhraní <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="c8476-200">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="c8476-201">Další informace naleznete v tématu <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="c8476-201">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="c8476-202">Přístup k objektu HttpContext</span><span class="sxs-lookup"><span data-stu-id="c8476-202">Access HttpContext</span></span>

<span data-ttu-id="c8476-203">`HttpContext` Při zpracování žádosti se stránkami Razor a technologií MVC je automaticky k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c8476-203">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="c8476-204">V případech kdy `HttpContext` není snadno k dispozici, můžete přistupovat `HttpContext` prostřednictvím <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> rozhraní a jeho výchozí implementace <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="c8476-204">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="c8476-205">Další informace naleznete v tématu <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="c8476-205">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="websockets"></a><span data-ttu-id="c8476-206">WebSocket</span><span class="sxs-lookup"><span data-stu-id="c8476-206">WebSockets</span></span>

<span data-ttu-id="c8476-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) je protokol, který umožňuje vytvoření trvalých obousměrných komunikačních kanálů přes TCP připojení.</span><span class="sxs-lookup"><span data-stu-id="c8476-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="c8476-208">Používá se např. pro komunikační aplikace, hry a kdekoli je vyžadována funkcionalita prováděná v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="c8476-208">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="c8476-209">ASP.NET Core podporuje funkce webových soketů.</span><span class="sxs-lookup"><span data-stu-id="c8476-209">ASP.NET Core supports web socket scenarios.</span></span>

<span data-ttu-id="c8476-210">Další informace naleznete v tématu <xref:fundamentals/websockets>.</span><span class="sxs-lookup"><span data-stu-id="c8476-210">For more information, see <xref:fundamentals/websockets>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="c8476-211">Metabalíček Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="c8476-211">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="c8476-212">Metabalíček [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) zjednodušuje správu balíčků.</span><span class="sxs-lookup"><span data-stu-id="c8476-212">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span>

<span data-ttu-id="c8476-213">Další informace naleznete v tématu <xref:fundamentals/metapackage-app>.</span><span class="sxs-lookup"><span data-stu-id="c8476-213">For more information, see <xref:fundamentals/metapackage-app>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="c8476-214">Metabalíček Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="c8476-214">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="c8476-215">[Metabalíček](https://www.nuget.org/packages/Microsoft.AspNetCore.All) Microsoft.AspNetCore.All pro ASP.NET Core zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="c8476-215">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="c8476-216">Všechny podporované balíčky vytvořené týmem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c8476-216">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="c8476-217">Všechny podporované balíčky Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c8476-217">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="c8476-218">Interní závislosti a závislosti třetích stran používané v rámci ASP.NET Core a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c8476-218">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="c8476-219">Další informace naleznete v tématu <xref:fundamentals/metapackage>.</span><span class="sxs-lookup"><span data-stu-id="c8476-219">For more information, see <xref:fundamentals/metapackage>.</span></span>

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="c8476-220">.NET Core proti .NET Framework</span><span class="sxs-lookup"><span data-stu-id="c8476-220">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="c8476-221">Aplikace ASP.NET Core lze při překladu cílit jak na běhové prostředí .NET Core, tak na běhové prostředí .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c8476-221">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="c8476-222">Další informace naleznete v tématu [Volba mezi .NET Core a .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="c8476-222">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="c8476-223">Volba mezi ASP.NET Core a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c8476-223">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="c8476-224">Další informace o volbě mezi ASP.NET Core a ASP.NET najdete v tématu <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span><span class="sxs-lookup"><span data-stu-id="c8476-224">For more information on choosing between ASP.NET Core and ASP.NET, see <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span></span>
