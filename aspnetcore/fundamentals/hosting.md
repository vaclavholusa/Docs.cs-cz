---
title: "Hostování v ASP.NET Core"
author: guardrex
description: "Další informace o webového hostitele v ASP.NET Core, která je zodpovědná za spuštění a životního cyklu správy aplikací."
keywords: Web ASP.NET Core hostitele, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: dfec2a67112d40b528b97c847da3dda8ef1e63bd
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="db6f1-104">Hostování v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db6f1-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="db6f1-105">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="db6f1-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="db6f1-106">Aplikace ASP.NET Core nakonfigurovat a spustit *hostitele*, která je zodpovědná za spuštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="db6f1-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="db6f1-107">Minimálně hostitele nakonfiguruje server a kanálu zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="db6f1-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="db6f1-108">Nastavení hostitele</span><span class="sxs-lookup"><span data-stu-id="db6f1-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-109">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="db6f1-110">Vytvořit pomocí instance hostitele [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="db6f1-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="db6f1-111">To se obvykle provádí ve vstupním bodě vaší aplikace, `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="db6f1-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="db6f1-112">V rámci šablon projektu `Main` se nachází v *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="db6f1-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="db6f1-113">Typické *Program.cs* volání [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zahájíte nastavení hostitele:</span><span class="sxs-lookup"><span data-stu-id="db6f1-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="db6f1-114">`CreateDefaultBuilder`provádí následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="db6f1-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="db6f1-115">Nakonfiguruje [Kestrel](servers/kestrel.md) jako webový server.</span><span class="sxs-lookup"><span data-stu-id="db6f1-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="db6f1-116">Výchozí možnosti Kestrel najdete v tématu [Kestrel možnosti části Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="db6f1-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="db6f1-117">Nastaví kořenu obsahu [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="db6f1-117">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="db6f1-118">Načítání z volitelné konfigurace:</span><span class="sxs-lookup"><span data-stu-id="db6f1-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="db6f1-119">*appSettings.JSON určený*.</span><span class="sxs-lookup"><span data-stu-id="db6f1-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="db6f1-120">*appSettings. {Prostředí} .json*.</span><span class="sxs-lookup"><span data-stu-id="db6f1-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="db6f1-121">[Tajné klíče uživatele](xref:security/app-secrets) při spuštění aplikace `Development` prostředí.</span><span class="sxs-lookup"><span data-stu-id="db6f1-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="db6f1-122">Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="db6f1-122">Environment variables.</span></span>
  * <span data-ttu-id="db6f1-123">Argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="db6f1-123">Command-line arguments.</span></span>
* <span data-ttu-id="db6f1-124">Nakonfiguruje [protokolování](xref:fundamentals/logging/index) konzoly a ladění výstupu s [filtrování protokolu](xref:fundamentals/logging/index#log-filtering) pravidla stanovená v části Konfigurace protokolování *appSettings.JSON určený* nebo *appsettings. {Prostředí} .json* souboru.</span><span class="sxs-lookup"><span data-stu-id="db6f1-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output with [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="db6f1-125">Při spuštění za služby IIS, umožňuje [integrační služby IIS](xref:publishing/iis) nakonfigurováním základní cesta a port serveru naslouchat požadavkům na při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="db6f1-125">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="db6f1-126">Modul vytvoří proxy zpětného mezi službou IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="db6f1-126">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="db6f1-127">Nakonfiguruje aplikaci taky [zaznamenat chyby při spuštění](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="db6f1-127">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="db6f1-128">Výchozí možnosti služby IIS najdete v tématu [IIS možnosti oddílu hostitele ASP.NET Core v systému Windows pomocí služby IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="db6f1-128">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="db6f1-129">*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="db6f1-129">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="db6f1-130">Výchozí kořen obsahu je [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="db6f1-130">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="db6f1-131">Výsledkem je použití webového projektu kořenové složky jako kořenu obsahu při spuštění aplikace z kořenové složky (například volání [dotnet spustit](/dotnet/core/tools/dotnet-run) ze složky projektu).</span><span class="sxs-lookup"><span data-stu-id="db6f1-131">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="db6f1-132">Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="db6f1-132">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="db6f1-133">V tématu [konfigurace v ASP.NET Core](xref:fundamentals/configuration/index) pro další informace o konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-133">See [Configuration in ASP.NET Core](xref:fundamentals/configuration/index) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="db6f1-134">Jako alternativu k použití statických `CreateDefaultBuilder` metody vytvoření hostitele z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) je podporované přístup pomocí ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="db6f1-134">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="db6f1-135">V tématu kartě ASP.NET Core 1.x pro další informace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-135">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-136">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="db6f1-137">Vytvořit pomocí instance hostitele [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="db6f1-137">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="db6f1-138">To se obvykle provádí ve vstupním bodě vaší aplikace, `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="db6f1-138">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="db6f1-139">V rámci šablon projektu `Main` se nachází v *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="db6f1-139">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="db6f1-140">Následující *Program.cs* ukazuje, jak používat `WebHostBuilder` k sestavení hostitele:</span><span class="sxs-lookup"><span data-stu-id="db6f1-140">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="db6f1-141">`WebHostBuilder`vyžaduje [serveru, který implementuje IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="db6f1-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="db6f1-142">Jsou předdefinované servery [Kestrel](servers/kestrel.md) a [HTTP.sys](servers/httpsys.md) (před verzí technologie ASP.NET 2.0 jádra, ovladač HTTP.sys volala [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="db6f1-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="db6f1-143">V tomto příkladu [UseKestrel rozšíření metoda](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) určuje Kestrel server.</span><span class="sxs-lookup"><span data-stu-id="db6f1-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="db6f1-144">*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="db6f1-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="db6f1-145">Zadaný výchozí kořen obsahu do `UseContentRoot` je [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="db6f1-145">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="db6f1-146">Výsledkem je použití webového projektu kořenové složky jako kořenu obsahu při spuštění aplikace z kořenové složky (například volání [dotnet spustit](/dotnet/core/tools/dotnet-run) ze složky projektu).</span><span class="sxs-lookup"><span data-stu-id="db6f1-146">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="db6f1-147">Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="db6f1-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="db6f1-148">Chcete-li použít jako reverzní proxy server služby IIS, volejte [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) jako součást sestavení hostitele.</span><span class="sxs-lookup"><span data-stu-id="db6f1-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="db6f1-149">`UseIISIntegration`neprovede konfiguraci *server*, například [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="db6f1-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="db6f1-150">`UseIISIntegration`Konfiguruje základní cesta a portu server musí naslouchat na portu při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) vytvořit proxy zpětného mezi Kestrel a služby IIS.</span><span class="sxs-lookup"><span data-stu-id="db6f1-150">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="db6f1-151">Pokud chcete používat IIS s ASP.NET Core, musíte zadat oba `UseKestrel` a `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-151">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="db6f1-152">`UseIISIntegration`aktivuje pouze při spuštění za služby IIS nebo IIS Express.</span><span class="sxs-lookup"><span data-stu-id="db6f1-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="db6f1-153">Další informace najdete v tématu [Úvod k modulu jádra ASP.NET](xref:fundamentals/servers/aspnet-core-module) a [odkazu na modul jádro ASP.NET konfigurace](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="db6f1-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="db6f1-154">Minimální implementace, které konfiguruje hostitele (a aplikace ASP.NET Core) zahrnuje určení serveru a konfiguraci kanálu žádostí aplikace:</span><span class="sxs-lookup"><span data-stu-id="db6f1-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

<span data-ttu-id="db6f1-155">Při nastavení na hostitele, je možné poskytnout [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metody.</span><span class="sxs-lookup"><span data-stu-id="db6f1-155">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="db6f1-156">Pokud zadáte `Startup` třídu, musí definovat `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="db6f1-156">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="db6f1-157">Další informace najdete v tématu [spuštění aplikace v ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="db6f1-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="db6f1-158">Více volá, aby se `ConfigureServices` připojit k sobě.</span><span class="sxs-lookup"><span data-stu-id="db6f1-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="db6f1-159">Více volá, aby se `Configure` nebo `UseStartup` na `WebHostBuilder` nahradit předchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="db6f1-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="db6f1-160">Hodnoty konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="db6f1-160">Host configuration values</span></span>

<span data-ttu-id="db6f1-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) poskytuje metody pro nastavení většinu hodnoty dostupné konfigurace pro hostitele, které lze nastavit také přímo s [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) a přidružené klíče.</span><span class="sxs-lookup"><span data-stu-id="db6f1-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="db6f1-162">Při nastavení hodnoty s `UseSetting`, je hodnota nastavena jako bez ohledu na typ řetězec (v uvozovkách).</span><span class="sxs-lookup"><span data-stu-id="db6f1-162">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="db6f1-163">Zaznamenat chyby při spuštění</span><span class="sxs-lookup"><span data-stu-id="db6f1-163">Capture Startup Errors</span></span>

<span data-ttu-id="db6f1-164">Toto nastavení řídí zaznamenávání chyby při spuštění.</span><span class="sxs-lookup"><span data-stu-id="db6f1-164">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="db6f1-165">**Klíč**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="db6f1-165">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="db6f1-166">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="db6f1-166">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="db6f1-167">**Výchozí**: použije se výchozí hodnota `false` Pokud aplikace běží s Kestrel za služby IIS, kde výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-167">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="db6f1-168">**Nastavit pomocí**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="db6f1-168">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="db6f1-169">Když `false`, chyb během spuštění výsledek v hostiteli. operace bude ukončena.</span><span class="sxs-lookup"><span data-stu-id="db6f1-169">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="db6f1-170">Když `true`, hostitel zachytí výjimky během spouštění a pokusí o spuštění serveru.</span><span class="sxs-lookup"><span data-stu-id="db6f1-170">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-171">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-172">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="db6f1-173">Obsah kořenové</span><span class="sxs-lookup"><span data-stu-id="db6f1-173">Content Root</span></span>

<span data-ttu-id="db6f1-174">Toto nastavení určuje, kde začíná ASP.NET Core hledání obsahu souborů, jako je například zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="db6f1-174">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="db6f1-175">**Klíč**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="db6f1-175">**Key**: contentRoot</span></span>  
<span data-ttu-id="db6f1-176">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="db6f1-176">**Type**: *string*</span></span>  
<span data-ttu-id="db6f1-177">**Výchozí**: výchozí složce, kde se nachází sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-177">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="db6f1-178">**Nastavit pomocí**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="db6f1-178">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="db6f1-179">Kořenu obsahu se používá jako základní cesta pro [kořenový Web nastavení](#web-root).</span><span class="sxs-lookup"><span data-stu-id="db6f1-179">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="db6f1-180">Pokud cesta neexistuje, hostitel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="db6f1-180">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-181">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-182">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="db6f1-183">Podrobné chyby</span><span class="sxs-lookup"><span data-stu-id="db6f1-183">Detailed Errors</span></span>

<span data-ttu-id="db6f1-184">Určuje, zda podrobné chyby, které mají být zaznamenány.</span><span class="sxs-lookup"><span data-stu-id="db6f1-184">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="db6f1-185">**Klíč**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="db6f1-185">**Key**: detailedErrors</span></span>  
<span data-ttu-id="db6f1-186">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="db6f1-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="db6f1-187">**Výchozí**: false</span><span class="sxs-lookup"><span data-stu-id="db6f1-187">**Default**: false</span></span>  
<span data-ttu-id="db6f1-188">**Nastavit pomocí**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="db6f1-188">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="db6f1-189">Když povolené (nebo když <a href="#environment">prostředí</a> je nastaven na `Development`), aplikace zaznamená podrobné výjimky.</span><span class="sxs-lookup"><span data-stu-id="db6f1-189">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-190">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-191">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="db6f1-192">Prostředí</span><span class="sxs-lookup"><span data-stu-id="db6f1-192">Environment</span></span>

<span data-ttu-id="db6f1-193">Nastaví prostředí aplikace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-193">Sets the app's environment.</span></span>

<span data-ttu-id="db6f1-194">**Klíč**: prostředí</span><span class="sxs-lookup"><span data-stu-id="db6f1-194">**Key**: environment</span></span>  
<span data-ttu-id="db6f1-195">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="db6f1-195">**Type**: *string*</span></span>  
<span data-ttu-id="db6f1-196">**Výchozí**: produkční</span><span class="sxs-lookup"><span data-stu-id="db6f1-196">**Default**: Production</span></span>  
<span data-ttu-id="db6f1-197">**Nastavit pomocí**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="db6f1-197">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="db6f1-198">Můžete nastavit *prostředí* na jakoukoli hodnotu.</span><span class="sxs-lookup"><span data-stu-id="db6f1-198">You can set the *Environment* to any value.</span></span> <span data-ttu-id="db6f1-199">Framework definované hodnoty zahrnují `Development`, `Staging`, a `Production`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-199">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="db6f1-200">Hodnoty nejsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="db6f1-200">Values aren't case sensitive.</span></span> <span data-ttu-id="db6f1-201">Ve výchozím nastavení *prostředí* je pro čtení z `ASPNETCORE_ENVIRONMENT` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="db6f1-201">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="db6f1-202">Při použití [Visual Studio](https://www.visualstudio.com/), proměnné prostředí může být nastavena v *launchSettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="db6f1-202">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="db6f1-203">Další informace najdete v tématu [práce s několika prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="db6f1-203">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-204">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-205">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="db6f1-206">Hostování spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="db6f1-206">Hosting Startup Assemblies</span></span>

<span data-ttu-id="db6f1-207">Nastaví hostování spuštění sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-207">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="db6f1-208">**Klíč**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="db6f1-208">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="db6f1-209">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="db6f1-209">**Type**: *string*</span></span>  
<span data-ttu-id="db6f1-210">**Výchozí**: prázdný řetězec</span><span class="sxs-lookup"><span data-stu-id="db6f1-210">**Default**: Empty string</span></span>  
<span data-ttu-id="db6f1-211">**Nastavit pomocí**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="db6f1-211">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="db6f1-212">Řetězec oddělený středníkem hostování spuštění sestavení načíst při spuštění.</span><span class="sxs-lookup"><span data-stu-id="db6f1-212">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="db6f1-213">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="db6f1-213">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="db6f1-214">I když výchozí hodnota konfigurace je řetězec prázdný, hostování spuštění sestavení, vždy zahrňte sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-214">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="db6f1-215">Když zadáte hostování sestavení spuštění, se přidají do sestavení aplikace pro načtení, když aplikace sestavení jeho společných služeb během spouštění.</span><span class="sxs-lookup"><span data-stu-id="db6f1-215">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-216">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-217">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="db6f1-218">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="db6f1-218">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="db6f1-219">Dáváte přednost hostování adresy URL</span><span class="sxs-lookup"><span data-stu-id="db6f1-219">Prefer Hosting URLs</span></span>

<span data-ttu-id="db6f1-220">Určuje, zda hostitel naslouchat požadavkům na adresy URL nakonfigurované `WebHostBuilder` místo nastavení nakonfigurovaného pomocí `IServer` implementace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-220">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="db6f1-221">**Klíč**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="db6f1-221">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="db6f1-222">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="db6f1-222">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="db6f1-223">**Výchozí**: true</span><span class="sxs-lookup"><span data-stu-id="db6f1-223">**Default**: true</span></span>  
<span data-ttu-id="db6f1-224">**Nastavit pomocí**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="db6f1-224">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="db6f1-225">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="db6f1-225">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-226">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-227">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="db6f1-228">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="db6f1-228">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="db6f1-229">Zabránit spuštění hostování</span><span class="sxs-lookup"><span data-stu-id="db6f1-229">Prevent Hosting Startup</span></span>

<span data-ttu-id="db6f1-230">Brání automatické načítání hostování spuštění sestavení, a to včetně hostování spuštění sestavení nakonfiguroval sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-230">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="db6f1-231">V tématu [přidat funkce aplikací z externí sestavení pomocí IHostingStartup](xref:hosting/ihostingstartup) Další informace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-231">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="db6f1-232">**Klíč**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="db6f1-232">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="db6f1-233">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="db6f1-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="db6f1-234">**Výchozí**: false</span><span class="sxs-lookup"><span data-stu-id="db6f1-234">**Default**: false</span></span>  
<span data-ttu-id="db6f1-235">**Nastavit pomocí**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="db6f1-235">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="db6f1-236">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="db6f1-236">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-237">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-237">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-238">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-238">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="db6f1-239">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="db6f1-239">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="db6f1-240">Adresy URL serveru</span><span class="sxs-lookup"><span data-stu-id="db6f1-240">Server URLs</span></span>

<span data-ttu-id="db6f1-241">Určuje IP adres nebo adres hostitele s porty a protokoly, které server naslouchat požadavkům na požadavky.</span><span class="sxs-lookup"><span data-stu-id="db6f1-241">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="db6f1-242">**Klíč**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="db6f1-242">**Key**: urls</span></span>  
<span data-ttu-id="db6f1-243">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="db6f1-243">**Type**: *string*</span></span>  
<span data-ttu-id="db6f1-244">**Výchozí**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="db6f1-244">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="db6f1-245">**Nastavit pomocí**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="db6f1-245">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="db6f1-246">Nastavte na oddělených středníkem (;) seznam URL předpony serveru by měl odpovídat.</span><span class="sxs-lookup"><span data-stu-id="db6f1-246">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="db6f1-247">Například `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-247">For example, `http://localhost:123`.</span></span> <span data-ttu-id="db6f1-248">Použití "\*" k označení, že by měl server přijímat požadavky na všechny IP adresy nebo názvu hostitele pomocí zadaný port a protokol (například `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="db6f1-248">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="db6f1-249">Protokol (`http://` nebo `https://`) musí být součástí každou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="db6f1-249">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="db6f1-250">Podporované formáty liší mezi servery.</span><span class="sxs-lookup"><span data-stu-id="db6f1-250">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-251">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="db6f1-252">Kestrel má svůj vlastní koncový bod rozhraní API konfigurace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-252">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="db6f1-253">Další informace najdete v tématu [Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="db6f1-253">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-254">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-254">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="db6f1-255">Časový limit vypnutí</span><span class="sxs-lookup"><span data-stu-id="db6f1-255">Shutdown Timeout</span></span>

<span data-ttu-id="db6f1-256">Určuje dobu čekání na webového hostitele vypnutí.</span><span class="sxs-lookup"><span data-stu-id="db6f1-256">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="db6f1-257">**Klíč**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="db6f1-257">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="db6f1-258">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="db6f1-258">**Type**: *int*</span></span>  
<span data-ttu-id="db6f1-259">**Výchozí**: 5</span><span class="sxs-lookup"><span data-stu-id="db6f1-259">**Default**: 5</span></span>  
<span data-ttu-id="db6f1-260">**Nastavit pomocí**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="db6f1-260">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="db6f1-261">I když přijme klíč *int* s `UseSetting` (například `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` rozšíření metoda přebírá `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-261">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="db6f1-262">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="db6f1-262">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-263">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-263">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-264">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-264">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="db6f1-265">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="db6f1-265">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="db6f1-266">Spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="db6f1-266">Startup Assembly</span></span>

<span data-ttu-id="db6f1-267">Určuje sestavení pro vyhledávání `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="db6f1-267">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="db6f1-268">**Klíč**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="db6f1-268">**Key**: startupAssembly</span></span>  
<span data-ttu-id="db6f1-269">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="db6f1-269">**Type**: *string*</span></span>  
<span data-ttu-id="db6f1-270">**Výchozí**: sestavení aplikace</span><span class="sxs-lookup"><span data-stu-id="db6f1-270">**Default**: The app's assembly</span></span>  
<span data-ttu-id="db6f1-271">**Nastavit pomocí**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="db6f1-271">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="db6f1-272">Sestavení, můžete odkazovat podle názvu (`string`) nebo typ (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="db6f1-272">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="db6f1-273">Pokud je to více `UseStartup` metody jsou volány, poslední změny mají přednost.</span><span class="sxs-lookup"><span data-stu-id="db6f1-273">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-274">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-274">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-275">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-275">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="db6f1-276">Kořenový web</span><span class="sxs-lookup"><span data-stu-id="db6f1-276">Web Root</span></span>

<span data-ttu-id="db6f1-277">Nastaví relativní cestu na statické prostředky aplikace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-277">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="db6f1-278">**Klíč**: webroot</span><span class="sxs-lookup"><span data-stu-id="db6f1-278">**Key**: webroot</span></span>  
<span data-ttu-id="db6f1-279">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="db6f1-279">**Type**: *string*</span></span>  
<span data-ttu-id="db6f1-280">**Výchozí**: Pokud není zadáno, výchozí hodnota je "(Content Root)/wwwroot", pokud cesta existuje.</span><span class="sxs-lookup"><span data-stu-id="db6f1-280">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="db6f1-281">Pokud cesta neexistuje, je použít soubor no-op zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="db6f1-281">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="db6f1-282">**Nastavit pomocí**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="db6f1-282">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-283">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-283">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-284">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-284">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="db6f1-285">Přepsání konfigurace</span><span class="sxs-lookup"><span data-stu-id="db6f1-285">Overriding configuration</span></span>

<span data-ttu-id="db6f1-286">Použití [konfigurace](xref:fundamentals/configuration/index) konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="db6f1-286">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="db6f1-287">V následujícím příkladu je konfigurace hostitele volitelně specifikován v *hosting.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="db6f1-287">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="db6f1-288">Všechny konfigurace načtena z *hosting.json* soubor může být přepsána argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="db6f1-288">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="db6f1-289">Integrovaný konfigurace (v `config`) se používá ke konfiguraci hostitele s `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-289">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-290">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="db6f1-291">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="db6f1-291">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="db6f1-292">Přepsání zadaná podle konfigurace `UseUrls` s *hosting.json* konfigurace příkazového řádku, první argument konfigurace druhý:</span><span class="sxs-lookup"><span data-stu-id="db6f1-292">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-293">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-293">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="db6f1-294">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="db6f1-294">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="db6f1-295">Přepsání zadaná podle konfigurace `UseUrls` s *hosting.json* konfigurace příkazového řádku, první argument konfigurace druhý:</span><span class="sxs-lookup"><span data-stu-id="db6f1-295">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> <span data-ttu-id="db6f1-296">`UseConfiguration` Metoda rozšíření není aktuálně schopen analyzovat konfigurační oddíl vrácený `GetSection` (například `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-296">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="db6f1-297">`GetSection` Metoda filtry konfigurace klíče do části požadovaný, ale ponechá název oddílu na klíče (například `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="db6f1-297">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="db6f1-298">`UseConfiguration` Metoda očekává klíče tak, aby odpovídala `WebHostBuilder` klíče (například `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="db6f1-298">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="db6f1-299">Přítomnost názvu oddílu na klíče zabrání hodnoty v části Konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="db6f1-299">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="db6f1-300">Tento problém bude vyřešen v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="db6f1-300">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="db6f1-301">Další informace a řešení, najdete v části [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration používá úplné klíče](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="db6f1-301">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="db6f1-302">Pokud chcete zadat hostiteli spustit na konkrétní adresu URL, můžete předat požadovanou hodnotu z příkazového řádku při provádění `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-302">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="db6f1-303">Přepíše argument příkazového řádku `urls` z hodnoty *hosting.json* souboru a server naslouchá na portu 8080:</span><span class="sxs-lookup"><span data-stu-id="db6f1-303">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="db6f1-304">Řazení důležitostí</span><span class="sxs-lookup"><span data-stu-id="db6f1-304">Ordering importance</span></span>

<span data-ttu-id="db6f1-305">Některé `WebHostBuilder` nastavení jsou nejdřív přečíst z proměnné prostředí, pokud nastavení.</span><span class="sxs-lookup"><span data-stu-id="db6f1-305">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="db6f1-306">Tyto proměnné prostředí, použijte formát `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-306">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="db6f1-307">Adresy URL, které server naslouchá na ve výchozím nastavení, můžete nastavit `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-307">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="db6f1-308">Některou z těchto hodnot proměnných prostředí můžete přepsat zadáním konfigurace (pomocí `UseConfiguration`) nebo nastavením hodnoty explicitně (pomocí `UseSetting` nebo jednu z metod explicitní rozšíření, jako například `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="db6f1-308">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="db6f1-309">Hostitel používá. obě tyto možnosti nastaví hodnotu poslední.</span><span class="sxs-lookup"><span data-stu-id="db6f1-309">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="db6f1-310">Pokud chcete prostřednictvím kódu programu nastavena výchozí adresy URL na jednu hodnotu, ale aby ji bylo možné přepsat s konfigurací, můžete příkazového řádku konfigurace po nastavení adresy URL.</span><span class="sxs-lookup"><span data-stu-id="db6f1-310">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="db6f1-311">V tématu [konfigurace přepíše](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="db6f1-311">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="db6f1-312">Od hostitele</span><span class="sxs-lookup"><span data-stu-id="db6f1-312">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="db6f1-313">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-313">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="db6f1-314">**Spustit**</span><span class="sxs-lookup"><span data-stu-id="db6f1-314">**Run**</span></span>

<span data-ttu-id="db6f1-315">`Run` Metoda spuštění webové aplikace a blokuje volající vlákno, dokud hostitel vypnutí:</span><span class="sxs-lookup"><span data-stu-id="db6f1-315">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="db6f1-316">**Start**</span><span class="sxs-lookup"><span data-stu-id="db6f1-316">**Start**</span></span>

<span data-ttu-id="db6f1-317">Můžete spustit hostitele způsobem neblokující voláním jeho `Start` metoda:</span><span class="sxs-lookup"><span data-stu-id="db6f1-317">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="db6f1-318">Pokud předáte seznam adres URL `Start` metoda, naslouchá na zadané adresy URL:</span><span class="sxs-lookup"><span data-stu-id="db6f1-318">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="db6f1-319">Můžete inicializaci a spuštění nové hostitele pomocí předem nakonfigurovaných výchozích nastavení `CreateDefaultBuilder` pomocí jiné metody statické pohodlí.</span><span class="sxs-lookup"><span data-stu-id="db6f1-319">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="db6f1-320">Tyto metody start pro server bez výstup konzoly a s [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) počkejte zalomení (Ctrl-C nebo sigint – nebo SIGTERM –):</span><span class="sxs-lookup"><span data-stu-id="db6f1-320">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="db6f1-321">**Spuštění (RequestDelegate aplikace)**</span><span class="sxs-lookup"><span data-stu-id="db6f1-321">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="db6f1-322">Začněte `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="db6f1-322">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="db6f1-323">Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="db6f1-323">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="db6f1-324">`WaitForShutdown`bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="db6f1-324">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="db6f1-325">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="db6f1-325">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="db6f1-326">**Spuštění (řetězec adresy url, RequestDelegate aplikace)**</span><span class="sxs-lookup"><span data-stu-id="db6f1-326">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="db6f1-327">Spustit s adresou URL a `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="db6f1-327">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="db6f1-328">Stejný výsledek jako **spuštění (RequestDelegate aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-328">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="db6f1-329">**Spuštění (akce<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="db6f1-329">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="db6f1-330">Použít instanci `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) používat směrování middleware:</span><span class="sxs-lookup"><span data-stu-id="db6f1-330">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="db6f1-331">Použijte následující požadavky na prohlížeč s příkladu:</span><span class="sxs-lookup"><span data-stu-id="db6f1-331">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="db6f1-332">Požadavek</span><span class="sxs-lookup"><span data-stu-id="db6f1-332">Request</span></span>                                    | <span data-ttu-id="db6f1-333">Odpověď</span><span class="sxs-lookup"><span data-stu-id="db6f1-333">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="db6f1-334">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="db6f1-334">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="db6f1-335">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="db6f1-335">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="db6f1-336">Vyvolá výjimku řetězcem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="db6f1-336">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="db6f1-337">Vyvolá výjimku řetězcem "Uh jejda!"</span><span class="sxs-lookup"><span data-stu-id="db6f1-337">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="db6f1-338">Santé, kevina!</span><span class="sxs-lookup"><span data-stu-id="db6f1-338">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="db6f1-339">Ahoj světe!</span><span class="sxs-lookup"><span data-stu-id="db6f1-339">Hello World!</span></span>                             |

<span data-ttu-id="db6f1-340">`WaitForShutdown`bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="db6f1-340">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="db6f1-341">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="db6f1-341">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="db6f1-342">**Spuštění (řetězce adresy url, akce<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="db6f1-342">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="db6f1-343">Použijte adresu URL a instance `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="db6f1-343">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="db6f1-344">Stejný výsledek jako **spuštění (akce<IRouteBuilder> routeBuilder)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-344">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="db6f1-345">**StartWith (akce<IApplicationBuilder> aplikace)**</span><span class="sxs-lookup"><span data-stu-id="db6f1-345">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="db6f1-346">Zadejte delegát pro konfiguraci `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="db6f1-346">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="db6f1-347">Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="db6f1-347">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="db6f1-348">`WaitForShutdown`bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="db6f1-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="db6f1-349">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="db6f1-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="db6f1-350">**StartWith (řetězce adresy url, akce<IApplicationBuilder> aplikace)**</span><span class="sxs-lookup"><span data-stu-id="db6f1-350">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="db6f1-351">Zadejte adresu URL a delegát pro konfiguraci `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="db6f1-351">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="db6f1-352">Stejný výsledek jako **StartWith (akce<IApplicationBuilder> aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-352">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="db6f1-353">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="db6f1-353">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="db6f1-354">**Spustit**</span><span class="sxs-lookup"><span data-stu-id="db6f1-354">**Run**</span></span>

<span data-ttu-id="db6f1-355">`Run` Metoda spuštění webové aplikace a blokuje volající vlákno, dokud hostitel vypnutí:</span><span class="sxs-lookup"><span data-stu-id="db6f1-355">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="db6f1-356">**Start**</span><span class="sxs-lookup"><span data-stu-id="db6f1-356">**Start**</span></span>

<span data-ttu-id="db6f1-357">Můžete spustit hostitele způsobem neblokující voláním jeho `Start` metoda:</span><span class="sxs-lookup"><span data-stu-id="db6f1-357">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="db6f1-358">Pokud předáte seznam adres URL `Start` metoda, naslouchá na zadané adresy URL:</span><span class="sxs-lookup"><span data-stu-id="db6f1-358">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="db6f1-359">IHostingEnvironment rozhraní</span><span class="sxs-lookup"><span data-stu-id="db6f1-359">IHostingEnvironment interface</span></span>

<span data-ttu-id="db6f1-360">[IHostingEnvironment rozhraní](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) poskytuje informace o hostování prostředí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-360">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="db6f1-361">Můžete použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:</span><span class="sxs-lookup"><span data-stu-id="db6f1-361">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="db6f1-362">Můžete použít [založené na konvenci přístup](xref:fundamentals/environments#startup-conventions) ke konfiguraci vaší aplikace při spuštění založených na prostředí.</span><span class="sxs-lookup"><span data-stu-id="db6f1-362">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="db6f1-363">Alternativně můžete vložit `IHostingEnvironment` do `Startup` konstruktor pro použití v `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="db6f1-363">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="db6f1-364">Kromě `IsDevelopment` metoda rozšíření `IHostingEnvironment` nabízí `IsStaging`, `IsProduction`, a `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="db6f1-364">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="db6f1-365">V tématu [práce s několika prostředí](xref:fundamentals/environments) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="db6f1-365">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="db6f1-366">`IHostingEnvironment` Služby může také vložit přímo do `Configure` metoda pro nastavení vašeho kanálu zpracování:</span><span class="sxs-lookup"><span data-stu-id="db6f1-366">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="db6f1-367">Můžete vložit `IHostingEnvironment` do `Invoke` metoda při vytváření vlastní [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="db6f1-367">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="db6f1-368">IApplicationLifetime rozhraní</span><span class="sxs-lookup"><span data-stu-id="db6f1-368">IApplicationLifetime interface</span></span>

<span data-ttu-id="db6f1-369">[IApplicationLifetime rozhraní](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umožňuje provádět po spuštění a vypnutí aktivity.</span><span class="sxs-lookup"><span data-stu-id="db6f1-369">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="db6f1-370">Zrušení tokenů, které můžete zaregistrovat s jsou tři vlastnosti na rozhraní `Action` metody k definování události spuštění a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="db6f1-370">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="db6f1-371">K dispozici je také `StopApplication` metoda.</span><span class="sxs-lookup"><span data-stu-id="db6f1-371">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="db6f1-372">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="db6f1-372">Cancellation Token</span></span>    | <span data-ttu-id="db6f1-373">Aktivuje, když &#8230;</span><span class="sxs-lookup"><span data-stu-id="db6f1-373">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="db6f1-374">Hostitele plně byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="db6f1-374">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="db6f1-375">Hostitel provádí řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="db6f1-375">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="db6f1-376">Může být stále aktivní žádosti.</span><span class="sxs-lookup"><span data-stu-id="db6f1-376">Requests may still be processing.</span></span> <span data-ttu-id="db6f1-377">Vypnutí bloky až po dokončení této události.</span><span class="sxs-lookup"><span data-stu-id="db6f1-377">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="db6f1-378">Hostitel je dokončení řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="db6f1-378">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="db6f1-379">Všechny požadavky, měla by být zcela zpracována.</span><span class="sxs-lookup"><span data-stu-id="db6f1-379">All requests should be completely processed.</span></span> <span data-ttu-id="db6f1-380">Vypnutí bloky až po dokončení této události.</span><span class="sxs-lookup"><span data-stu-id="db6f1-380">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="db6f1-381">Metoda</span><span class="sxs-lookup"><span data-stu-id="db6f1-381">Method</span></span>            | <span data-ttu-id="db6f1-382">Akce</span><span class="sxs-lookup"><span data-stu-id="db6f1-382">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="db6f1-383">Žádosti o ukončení aktuální aplikace.</span><span class="sxs-lookup"><span data-stu-id="db6f1-383">Requests termination of the current application.</span></span> |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="db6f1-384">Řešení potíží s System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="db6f1-384">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="db6f1-385">**Platí pro pouze ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="db6f1-385">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="db6f1-386">Pokud vytvoříte hostitele vložením `IStartup` přímo do kontejneru pro vkládání závislosti místo volání `UseStartup` nebo `Configure`, může dojít k následující chybě: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-386">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="db6f1-387">K tomu dochází, protože [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (aktuální sestavení) je nutná ke kontrole pro `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-387">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="db6f1-388">Pokud ručně vložit `IStartup` do kontejneru pro vkládání závislosti, přidejte následující volání vaší `WebHostBuilder` se zadaným názvem sestavení:</span><span class="sxs-lookup"><span data-stu-id="db6f1-388">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="db6f1-389">Můžete taky přidat fiktivní `Configure` k vaší `WebHostBuilder`, která nastaví `applicationName`(`ApplicationKey`) automaticky:</span><span class="sxs-lookup"><span data-stu-id="db6f1-389">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="db6f1-390">**Poznámka:**: Toto je pouze požadované verze technologie ASP.NET 2.0 jádra a pouze pokud nemůžete volat `UseStartup` nebo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="db6f1-390">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="db6f1-391">Další informace najdete v tématu [oznámení: Microsoft.Extensions.PlatformAbstractions byl odebrán (komentář)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) a [StartupInjection ukázka](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="db6f1-391">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db6f1-392">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="db6f1-392">Additional resources</span></span>

* [<span data-ttu-id="db6f1-393">Publikovat do systému Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="db6f1-393">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="db6f1-394">Publikování do systému Linux pomocí Nginx</span><span class="sxs-lookup"><span data-stu-id="db6f1-394">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="db6f1-395">Publikování do systému Linux pomocí Apache</span><span class="sxs-lookup"><span data-stu-id="db6f1-395">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="db6f1-396">Hostitele ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="db6f1-396">Host in a Windows Service</span></span>](xref:hosting/windows-service)
