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
ms.openlocfilehash: 14e48adf5671a41ad6e135caeb4a87fdf7292aa6
ms.sourcegitcommit: 5834afb87e4262b9b88e60e3fe6c735e61a1e08d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/20/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="212aa-104">Hostování v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="212aa-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="212aa-105">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="212aa-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="212aa-106">Aplikace ASP.NET Core nakonfigurovat a spustit *hostitele*.</span><span class="sxs-lookup"><span data-stu-id="212aa-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="212aa-107">Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="212aa-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="212aa-108">Minimálně hostitele nakonfiguruje server a kanálu zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="212aa-108">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="212aa-109">Nastavení hostitele</span><span class="sxs-lookup"><span data-stu-id="212aa-109">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-110">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="212aa-111">Vytvořit pomocí instance hostitele [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="212aa-111">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="212aa-112">To se obvykle provádí ve vstupním bodě vaší aplikace, `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="212aa-112">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="212aa-113">V rámci šablon projektu `Main` se nachází v *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="212aa-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="212aa-114">Typické *Program.cs* volání [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zahájíte nastavení hostitele:</span><span class="sxs-lookup"><span data-stu-id="212aa-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="212aa-115">`CreateDefaultBuilder`provádí následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="212aa-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="212aa-116">Nakonfiguruje [Kestrel](servers/kestrel.md) jako webový server.</span><span class="sxs-lookup"><span data-stu-id="212aa-116">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="212aa-117">Výchozí možnosti Kestrel najdete v tématu [Kestrel možnosti části Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="212aa-117">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="212aa-118">Nastaví kořenu obsahu [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="212aa-118">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="212aa-119">Načítání z volitelné konfigurace:</span><span class="sxs-lookup"><span data-stu-id="212aa-119">Loads optional configuration from:</span></span>
  * <span data-ttu-id="212aa-120">*appSettings.JSON určený*.</span><span class="sxs-lookup"><span data-stu-id="212aa-120">*appsettings.json*.</span></span>
  * <span data-ttu-id="212aa-121">*appSettings. {Prostředí} .json*.</span><span class="sxs-lookup"><span data-stu-id="212aa-121">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="212aa-122">[Tajné klíče uživatele](xref:security/app-secrets) při spuštění aplikace `Development` prostředí.</span><span class="sxs-lookup"><span data-stu-id="212aa-122">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="212aa-123">Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="212aa-123">Environment variables.</span></span>
  * <span data-ttu-id="212aa-124">Argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="212aa-124">Command-line arguments.</span></span>
* <span data-ttu-id="212aa-125">Nakonfiguruje [protokolování](xref:fundamentals/logging/index) konzoly a ladění výstupu.</span><span class="sxs-lookup"><span data-stu-id="212aa-125">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="212aa-126">Zahrnuje protokolování [filtrování protokolu](xref:fundamentals/logging/index#log-filtering) pravidla stanovená v části Konfigurace protokolování *appSettings.JSON určený* nebo *appsettings. { Prostředí} .json* souboru.</span><span class="sxs-lookup"><span data-stu-id="212aa-126">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="212aa-127">Při spuštění za služby IIS, umožňuje [integrační služby IIS](xref:publishing/iis) nakonfigurováním základní cesta a port serveru naslouchat požadavkům na při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="212aa-127">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="212aa-128">Modul vytvoří proxy zpětného mezi službou IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="212aa-128">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="212aa-129">Nakonfiguruje aplikaci taky [zaznamenat chyby při spuštění](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="212aa-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="212aa-130">Výchozí možnosti služby IIS najdete v tématu [IIS možnosti oddílu hostitele ASP.NET Core v systému Windows pomocí služby IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="212aa-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="212aa-131">*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="212aa-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="212aa-132">Výchozí kořen obsahu je [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="212aa-132">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="212aa-133">Výchozí obsahu kořen (`Directory.GetCurrentDirectory`) výsledkem použití webového projektu kořenové složky jako kořenu obsahu při spuštění aplikace z kořenové složky (například volání [dotnet spustit](/dotnet/core/tools/dotnet-run) ze složky projektu).</span><span class="sxs-lookup"><span data-stu-id="212aa-133">The default content root (`Directory.GetCurrentDirectory`) results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="212aa-134">Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="212aa-134">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="212aa-135">Další informace o konfiguraci aplikace, najdete v části [konfigurace v ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="212aa-135">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="212aa-136">Jako alternativu k použití statických `CreateDefaultBuilder` metody vytvoření hostitele z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) je podporované přístup pomocí ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="212aa-136">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="212aa-137">Další informace najdete v tématu kartě 1.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="212aa-137">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-138">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="212aa-139">Vytvořit pomocí instance hostitele [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="212aa-139">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="212aa-140">Vytvoření hostitele se obvykle provádí v vstupní bod aplikace, `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="212aa-140">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="212aa-141">V rámci šablon projektu `Main` se nachází v *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="212aa-141">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="212aa-142">Typické *Program.cs* volání [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zahájíte nastavení hostitele:</span><span class="sxs-lookup"><span data-stu-id="212aa-142">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="212aa-143">`WebHostBuilder`vyžaduje [serveru, který implementuje IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="212aa-143">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="212aa-144">Jsou předdefinované servery [Kestrel](servers/kestrel.md) a [HTTP.sys](servers/httpsys.md) (před verzí technologie ASP.NET 2.0 jádra, ovladač HTTP.sys volala [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="212aa-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="212aa-145">V tomto příkladu [UseKestrel rozšíření metoda](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) určuje Kestrel server.</span><span class="sxs-lookup"><span data-stu-id="212aa-145">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="212aa-146">*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="212aa-146">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="212aa-147">Zadaný výchozí kořen obsahu do `UseContentRoot` je [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="212aa-147">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="212aa-148">Výsledkem je použití webového projektu kořenové složky jako kořenu obsahu při spuštění aplikace z kořenové složky (například volání [dotnet spustit](/dotnet/core/tools/dotnet-run) ze složky projektu).</span><span class="sxs-lookup"><span data-stu-id="212aa-148">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="212aa-149">Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="212aa-149">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="212aa-150">Chcete-li použít jako reverzní proxy server služby IIS, volejte [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) jako součást sestavení hostitele.</span><span class="sxs-lookup"><span data-stu-id="212aa-150">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="212aa-151">`UseIISIntegration`neprovede konfiguraci *server*, například [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="212aa-151">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="212aa-152">`UseIISIntegration`Konfiguruje základní cesta a portu server musí naslouchat na portu při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) vytvořit proxy zpětného mezi Kestrel a služby IIS.</span><span class="sxs-lookup"><span data-stu-id="212aa-152">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="212aa-153">Pokud chcete používat IIS s ASP.NET Core, musíte zadat oba `UseKestrel` a `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="212aa-153">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="212aa-154">`UseIISIntegration`aktivuje pouze při spuštění za služby IIS nebo IIS Express.</span><span class="sxs-lookup"><span data-stu-id="212aa-154">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="212aa-155">Další informace najdete v tématu [Úvod k modulu jádra ASP.NET](xref:fundamentals/servers/aspnet-core-module) a [odkazu na modul jádro ASP.NET konfigurace](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="212aa-155">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="212aa-156">Minimální implementace, které konfiguruje hostitele (a aplikace ASP.NET Core) zahrnuje určení serveru a konfiguraci kanálu žádostí aplikace:</span><span class="sxs-lookup"><span data-stu-id="212aa-156">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="212aa-157">Při nastavení na hostitele, je možné poskytnout [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metody.</span><span class="sxs-lookup"><span data-stu-id="212aa-157">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="212aa-158">Pokud zadáte `Startup` třídu, musí definovat `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="212aa-158">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="212aa-159">Další informace najdete v tématu [spuštění aplikace v ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="212aa-159">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="212aa-160">Více volá, aby se `ConfigureServices` připojit k sobě.</span><span class="sxs-lookup"><span data-stu-id="212aa-160">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="212aa-161">Více volá, aby se `Configure` nebo `UseStartup` na `WebHostBuilder` nahradit předchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="212aa-161">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="212aa-162">Hodnoty konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="212aa-162">Host configuration values</span></span>

<span data-ttu-id="212aa-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) poskytuje následujících dvou přístupů pro většinu hodnoty dostupné konfigurace nastavení pro hostitele:</span><span class="sxs-lookup"><span data-stu-id="212aa-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides the following approaches for setting most of the available configuration values for the host:</span></span>

* <span data-ttu-id="212aa-164">Proměnné prostředí ve formátu `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="212aa-164">Environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="212aa-165">Například `ASPNETCORE_DETAILEDERRORS`.</span><span class="sxs-lookup"><span data-stu-id="212aa-165">For example, `ASPNETCORE_DETAILEDERRORS`.</span></span>
* <span data-ttu-id="212aa-166">Explicitní metody, jako například `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="212aa-166">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="212aa-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) a přidružené klíče.</span><span class="sxs-lookup"><span data-stu-id="212aa-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="212aa-168">Při nastavení hodnoty s `UseSetting`, je hodnota nastavena jako bez ohledu na typ řetězec.</span><span class="sxs-lookup"><span data-stu-id="212aa-168">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="212aa-169">Zaznamenat chyby při spuštění</span><span class="sxs-lookup"><span data-stu-id="212aa-169">Capture Startup Errors</span></span>

<span data-ttu-id="212aa-170">Toto nastavení řídí zaznamenávání chyby při spuštění.</span><span class="sxs-lookup"><span data-stu-id="212aa-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="212aa-171">**Klíč**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="212aa-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="212aa-172">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="212aa-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="212aa-173">**Výchozí**: použije se výchozí hodnota `false` Pokud aplikace běží s Kestrel za služby IIS, kde výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="212aa-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="212aa-174">**Nastavit pomocí**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="212aa-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="212aa-175">**Proměnné prostředí**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="212aa-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="212aa-176">Když `false`, chyb během spuštění výsledek v hostiteli. operace bude ukončena.</span><span class="sxs-lookup"><span data-stu-id="212aa-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="212aa-177">Když `true`, hostitel zachytí výjimky během spouštění a pokusí o spuštění serveru.</span><span class="sxs-lookup"><span data-stu-id="212aa-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-178">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-179">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="212aa-180">Obsah kořenové</span><span class="sxs-lookup"><span data-stu-id="212aa-180">Content Root</span></span>

<span data-ttu-id="212aa-181">Toto nastavení určuje, kde začíná ASP.NET Core hledání obsahu souborů, jako je například zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="212aa-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="212aa-182">**Klíč**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="212aa-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="212aa-183">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="212aa-183">**Type**: *string*</span></span>  
<span data-ttu-id="212aa-184">**Výchozí**: výchozí složce, kde se nachází sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="212aa-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="212aa-185">**Nastavit pomocí**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="212aa-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="212aa-186">**Proměnné prostředí**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="212aa-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="212aa-187">Kořenu obsahu se používá jako základní cesta pro [kořenový Web nastavení](#web-root).</span><span class="sxs-lookup"><span data-stu-id="212aa-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="212aa-188">Pokud cesta neexistuje, hostitel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="212aa-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-189">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-190">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="212aa-191">Podrobné chyby</span><span class="sxs-lookup"><span data-stu-id="212aa-191">Detailed Errors</span></span>

<span data-ttu-id="212aa-192">Určuje, zda podrobné chyby, které mají být zaznamenány.</span><span class="sxs-lookup"><span data-stu-id="212aa-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="212aa-193">**Klíč**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="212aa-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="212aa-194">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="212aa-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="212aa-195">**Výchozí**: false</span><span class="sxs-lookup"><span data-stu-id="212aa-195">**Default**: false</span></span>  
<span data-ttu-id="212aa-196">**Nastavit pomocí**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="212aa-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="212aa-197">**Proměnné prostředí**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="212aa-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="212aa-198">Když povolené (nebo když <a href="#environment">prostředí</a> je nastaven na `Development`), aplikace zaznamená podrobné výjimky.</span><span class="sxs-lookup"><span data-stu-id="212aa-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-199">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-200">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="212aa-201">Prostředí</span><span class="sxs-lookup"><span data-stu-id="212aa-201">Environment</span></span>

<span data-ttu-id="212aa-202">Nastaví prostředí aplikace.</span><span class="sxs-lookup"><span data-stu-id="212aa-202">Sets the app's environment.</span></span>

<span data-ttu-id="212aa-203">**Klíč**: prostředí</span><span class="sxs-lookup"><span data-stu-id="212aa-203">**Key**: environment</span></span>  
<span data-ttu-id="212aa-204">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="212aa-204">**Type**: *string*</span></span>  
<span data-ttu-id="212aa-205">**Výchozí**: produkční</span><span class="sxs-lookup"><span data-stu-id="212aa-205">**Default**: Production</span></span>  
<span data-ttu-id="212aa-206">**Nastavit pomocí**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="212aa-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="212aa-207">**Proměnné prostředí**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="212aa-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="212aa-208">Můžete nastavit *prostředí* na jakoukoli hodnotu.</span><span class="sxs-lookup"><span data-stu-id="212aa-208">You can set the *Environment* to any value.</span></span> <span data-ttu-id="212aa-209">Framework definované hodnoty zahrnují `Development`, `Staging`, a `Production`.</span><span class="sxs-lookup"><span data-stu-id="212aa-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="212aa-210">Hodnoty nejsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="212aa-210">Values aren't case sensitive.</span></span> <span data-ttu-id="212aa-211">Ve výchozím nastavení *prostředí* je pro čtení z `ASPNETCORE_ENVIRONMENT` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="212aa-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="212aa-212">Při použití [Visual Studio](https://www.visualstudio.com/), proměnné prostředí může být nastavena v *launchSettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="212aa-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="212aa-213">Další informace najdete v tématu [práce s několika prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="212aa-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-214">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-215">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="212aa-216">Hostování spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="212aa-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="212aa-217">Nastaví hostování spuštění sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="212aa-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="212aa-218">**Klíč**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="212aa-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="212aa-219">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="212aa-219">**Type**: *string*</span></span>  
<span data-ttu-id="212aa-220">**Výchozí**: prázdný řetězec</span><span class="sxs-lookup"><span data-stu-id="212aa-220">**Default**: Empty string</span></span>  
<span data-ttu-id="212aa-221">**Nastavit pomocí**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="212aa-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="212aa-222">**Proměnné prostředí**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="212aa-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="212aa-223">Řetězec oddělený středníkem hostování spuštění sestavení načíst při spuštění.</span><span class="sxs-lookup"><span data-stu-id="212aa-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="212aa-224">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="212aa-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="212aa-225">I když výchozí hodnota konfigurace je řetězec prázdný, hostování spuštění sestavení, vždy zahrňte sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="212aa-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="212aa-226">Když zadáte hostování sestavení spuštění, se přidají do sestavení aplikace pro načtení, když aplikace sestavení jeho společných služeb během spouštění.</span><span class="sxs-lookup"><span data-stu-id="212aa-226">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-227">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-228">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="212aa-229">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="212aa-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="212aa-230">Dáváte přednost hostování adresy URL</span><span class="sxs-lookup"><span data-stu-id="212aa-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="212aa-231">Určuje, zda hostitel naslouchat požadavkům na adresy URL nakonfigurované `WebHostBuilder` místo nastavení nakonfigurovaného pomocí `IServer` implementace.</span><span class="sxs-lookup"><span data-stu-id="212aa-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="212aa-232">**Klíč**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="212aa-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="212aa-233">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="212aa-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="212aa-234">**Výchozí**: true</span><span class="sxs-lookup"><span data-stu-id="212aa-234">**Default**: true</span></span>  
<span data-ttu-id="212aa-235">**Nastavit pomocí**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="212aa-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="212aa-236">**Proměnné prostředí**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="212aa-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="212aa-237">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="212aa-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-238">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-239">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="212aa-240">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="212aa-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="212aa-241">Zabránit spuštění hostování</span><span class="sxs-lookup"><span data-stu-id="212aa-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="212aa-242">Brání automatické načítání hostování spuštění sestavení, a to včetně hostování spuštění sestavení nakonfiguroval sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="212aa-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="212aa-243">V tématu [přidat funkce aplikací z externí sestavení pomocí IHostingStartup](xref:hosting/ihostingstartup) Další informace.</span><span class="sxs-lookup"><span data-stu-id="212aa-243">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="212aa-244">**Klíč**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="212aa-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="212aa-245">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="212aa-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="212aa-246">**Výchozí**: false</span><span class="sxs-lookup"><span data-stu-id="212aa-246">**Default**: false</span></span>  
<span data-ttu-id="212aa-247">**Nastavit pomocí**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="212aa-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="212aa-248">**Proměnné prostředí**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="212aa-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="212aa-249">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="212aa-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-250">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-251">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="212aa-252">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="212aa-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="212aa-253">Adresy URL serveru</span><span class="sxs-lookup"><span data-stu-id="212aa-253">Server URLs</span></span>

<span data-ttu-id="212aa-254">Určuje IP adres nebo adres hostitele s porty a protokoly, které server naslouchat požadavkům na požadavky.</span><span class="sxs-lookup"><span data-stu-id="212aa-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="212aa-255">**Klíč**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="212aa-255">**Key**: urls</span></span>  
<span data-ttu-id="212aa-256">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="212aa-256">**Type**: *string*</span></span>  
<span data-ttu-id="212aa-257">**Výchozí**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="212aa-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="212aa-258">**Nastavit pomocí**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="212aa-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="212aa-259">**Proměnné prostředí**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="212aa-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="212aa-260">Nastavte na oddělených středníkem (;) seznam URL předpony serveru by měl odpovídat.</span><span class="sxs-lookup"><span data-stu-id="212aa-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="212aa-261">Například `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="212aa-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="212aa-262">Použití "\*" k označení, že by měl server přijímat požadavky na všechny IP adresy nebo názvu hostitele pomocí zadaný port a protokol (například `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="212aa-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="212aa-263">Protokol (`http://` nebo `https://`) musí být součástí každou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="212aa-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="212aa-264">Podporované formáty liší mezi servery.</span><span class="sxs-lookup"><span data-stu-id="212aa-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-265">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="212aa-266">Kestrel má svůj vlastní koncový bod rozhraní API konfigurace.</span><span class="sxs-lookup"><span data-stu-id="212aa-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="212aa-267">Další informace najdete v tématu [Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="212aa-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-268">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="212aa-269">Časový limit vypnutí</span><span class="sxs-lookup"><span data-stu-id="212aa-269">Shutdown Timeout</span></span>

<span data-ttu-id="212aa-270">Určuje dobu čekání na webového hostitele vypnutí.</span><span class="sxs-lookup"><span data-stu-id="212aa-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="212aa-271">**Klíč**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="212aa-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="212aa-272">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="212aa-272">**Type**: *int*</span></span>  
<span data-ttu-id="212aa-273">**Výchozí**: 5</span><span class="sxs-lookup"><span data-stu-id="212aa-273">**Default**: 5</span></span>  
<span data-ttu-id="212aa-274">**Nastavit pomocí**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="212aa-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="212aa-275">**Proměnné prostředí**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="212aa-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="212aa-276">I když přijme klíč *int* s `UseSetting` (například `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` rozšíření metoda přebírá `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="212aa-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="212aa-277">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="212aa-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-278">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-279">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="212aa-280">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="212aa-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="212aa-281">Spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="212aa-281">Startup Assembly</span></span>

<span data-ttu-id="212aa-282">Určuje sestavení pro vyhledávání `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="212aa-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="212aa-283">**Klíč**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="212aa-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="212aa-284">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="212aa-284">**Type**: *string*</span></span>  
<span data-ttu-id="212aa-285">**Výchozí**: sestavení aplikace</span><span class="sxs-lookup"><span data-stu-id="212aa-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="212aa-286">**Nastavit pomocí**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="212aa-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="212aa-287">**Proměnné prostředí**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="212aa-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="212aa-288">Sestavení, můžete odkazovat podle názvu (`string`) nebo typ (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="212aa-288">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="212aa-289">Pokud je to více `UseStartup` metody jsou volány, poslední změny mají přednost.</span><span class="sxs-lookup"><span data-stu-id="212aa-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-290">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-291">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="212aa-292">Kořenový web</span><span class="sxs-lookup"><span data-stu-id="212aa-292">Web Root</span></span>

<span data-ttu-id="212aa-293">Nastaví relativní cestu na statické prostředky aplikace.</span><span class="sxs-lookup"><span data-stu-id="212aa-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="212aa-294">**Klíč**: webroot</span><span class="sxs-lookup"><span data-stu-id="212aa-294">**Key**: webroot</span></span>  
<span data-ttu-id="212aa-295">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="212aa-295">**Type**: *string*</span></span>  
<span data-ttu-id="212aa-296">**Výchozí**: Pokud není zadáno, výchozí hodnota je "(Content Root)/wwwroot", pokud cesta existuje.</span><span class="sxs-lookup"><span data-stu-id="212aa-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="212aa-297">Pokud cesta neexistuje, je použít soubor no-op zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="212aa-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="212aa-298">**Nastavit pomocí**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="212aa-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="212aa-299">**Proměnné prostředí**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="212aa-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-300">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-301">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="212aa-302">Přepsání konfigurace</span><span class="sxs-lookup"><span data-stu-id="212aa-302">Overriding configuration</span></span>

<span data-ttu-id="212aa-303">Použití [konfigurace](xref:fundamentals/configuration/index) konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="212aa-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="212aa-304">V následujícím příkladu je konfigurace hostitele volitelně specifikován v *hosting.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="212aa-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="212aa-305">Všechny konfigurace načtena z *hosting.json* soubor může být přepsána argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="212aa-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="212aa-306">Integrovaný konfigurace (v `config`) se používá ke konfiguraci hostitele s `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="212aa-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-307">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="212aa-308">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="212aa-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="212aa-309">Přepsání zadaná podle konfigurace `UseUrls` s *hosting.json* konfigurace příkazového řádku, první argument konfigurace druhý:</span><span class="sxs-lookup"><span data-stu-id="212aa-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-310">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="212aa-311">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="212aa-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="212aa-312">Přepsání zadaná podle konfigurace `UseUrls` s *hosting.json* konfigurace příkazového řádku, první argument konfigurace druhý:</span><span class="sxs-lookup"><span data-stu-id="212aa-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="212aa-313">`UseConfiguration` Metoda rozšíření není aktuálně schopen analyzovat konfigurační oddíl vrácený `GetSection` (například `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="212aa-313">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="212aa-314">`GetSection` Metoda filtry konfigurace klíče do části požadovaný, ale ponechá název oddílu na klíče (například `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="212aa-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="212aa-315">`UseConfiguration` Metoda očekává klíče tak, aby odpovídala `WebHostBuilder` klíče (například `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="212aa-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="212aa-316">Přítomnost názvu oddílu na klíče zabrání hodnoty v části Konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="212aa-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="212aa-317">Tento problém bude vyřešen v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="212aa-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="212aa-318">Další informace a řešení, najdete v části [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration používá úplné klíče](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="212aa-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="212aa-319">Pokud chcete zadat hostiteli spustit na konkrétní adresu URL, můžete předat požadovanou hodnotu z příkazového řádku při provádění `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="212aa-319">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="212aa-320">Přepíše argument příkazového řádku `urls` z hodnoty *hosting.json* souboru a server naslouchá na portu 8080:</span><span class="sxs-lookup"><span data-stu-id="212aa-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="212aa-321">Řazení důležitostí</span><span class="sxs-lookup"><span data-stu-id="212aa-321">Ordering importance</span></span>

<span data-ttu-id="212aa-322">Některé `WebHostBuilder` nastavení jsou nejdřív přečíst z proměnné prostředí, pokud nastavení.</span><span class="sxs-lookup"><span data-stu-id="212aa-322">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="212aa-323">Tyto proměnné prostředí, použijte formát `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="212aa-323">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="212aa-324">Adresy URL, které server naslouchá na ve výchozím nastavení, můžete nastavit `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="212aa-324">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="212aa-325">Některou z těchto hodnot proměnných prostředí můžete přepsat zadáním konfigurace (pomocí `UseConfiguration`) nebo nastavením hodnoty explicitně (pomocí `UseSetting` nebo jednu z metod explicitní rozšíření, jako například `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="212aa-325">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="212aa-326">Hostitel používá. obě tyto možnosti nastaví hodnotu poslední.</span><span class="sxs-lookup"><span data-stu-id="212aa-326">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="212aa-327">Pokud chcete prostřednictvím kódu programu nastavena výchozí adresy URL na jednu hodnotu, ale aby ji bylo možné přepsat s konfigurací, můžete příkazového řádku konfigurace po nastavení adresy URL.</span><span class="sxs-lookup"><span data-stu-id="212aa-327">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="212aa-328">V tématu [konfigurace přepíše](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="212aa-328">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="212aa-329">Od hostitele</span><span class="sxs-lookup"><span data-stu-id="212aa-329">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="212aa-330">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="212aa-330">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="212aa-331">**Spustit**</span><span class="sxs-lookup"><span data-stu-id="212aa-331">**Run**</span></span>

<span data-ttu-id="212aa-332">`Run` Metoda spuštění webové aplikace a blokuje volající vlákno, dokud hostitel vypnutí:</span><span class="sxs-lookup"><span data-stu-id="212aa-332">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="212aa-333">**Start**</span><span class="sxs-lookup"><span data-stu-id="212aa-333">**Start**</span></span>

<span data-ttu-id="212aa-334">Můžete spustit hostitele způsobem neblokující voláním jeho `Start` metoda:</span><span class="sxs-lookup"><span data-stu-id="212aa-334">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="212aa-335">Pokud předáte seznam adres URL `Start` metoda, naslouchá na zadané adresy URL:</span><span class="sxs-lookup"><span data-stu-id="212aa-335">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="212aa-336">Můžete inicializaci a spuštění nové hostitele pomocí předem nakonfigurovaných výchozích nastavení `CreateDefaultBuilder` pomocí jiné metody statické pohodlí.</span><span class="sxs-lookup"><span data-stu-id="212aa-336">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="212aa-337">Tyto metody start pro server bez výstup konzoly a s [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) počkejte zalomení (Ctrl-C nebo sigint – nebo SIGTERM –):</span><span class="sxs-lookup"><span data-stu-id="212aa-337">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="212aa-338">**Spuštění (RequestDelegate aplikace)**</span><span class="sxs-lookup"><span data-stu-id="212aa-338">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="212aa-339">Začněte `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="212aa-339">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="212aa-340">Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="212aa-340">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="212aa-341">`WaitForShutdown`bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="212aa-341">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="212aa-342">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="212aa-342">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="212aa-343">**Spuštění (řetězec adresy url, RequestDelegate aplikace)**</span><span class="sxs-lookup"><span data-stu-id="212aa-343">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="212aa-344">Spustit s adresou URL a `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="212aa-344">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="212aa-345">Stejný výsledek jako **spuštění (RequestDelegate aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="212aa-345">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="212aa-346">**Spuštění (akce<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="212aa-346">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="212aa-347">Použít instanci `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) používat směrování middleware:</span><span class="sxs-lookup"><span data-stu-id="212aa-347">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="212aa-348">Použijte následující požadavky na prohlížeč s příkladu:</span><span class="sxs-lookup"><span data-stu-id="212aa-348">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="212aa-349">Požadavek</span><span class="sxs-lookup"><span data-stu-id="212aa-349">Request</span></span>                                    | <span data-ttu-id="212aa-350">Odpověď</span><span class="sxs-lookup"><span data-stu-id="212aa-350">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="212aa-351">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="212aa-351">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="212aa-352">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="212aa-352">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="212aa-353">Vyvolá výjimku řetězcem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="212aa-353">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="212aa-354">Vyvolá výjimku řetězcem "Uh jejda!"</span><span class="sxs-lookup"><span data-stu-id="212aa-354">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="212aa-355">Santé, kevina!</span><span class="sxs-lookup"><span data-stu-id="212aa-355">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="212aa-356">Ahoj světe!</span><span class="sxs-lookup"><span data-stu-id="212aa-356">Hello World!</span></span>                             |

<span data-ttu-id="212aa-357">`WaitForShutdown`bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="212aa-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="212aa-358">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="212aa-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="212aa-359">**Spuštění (řetězce adresy url, akce<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="212aa-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="212aa-360">Použijte adresu URL a instance `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="212aa-360">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="212aa-361">Stejný výsledek jako **spuštění (akce<IRouteBuilder> routeBuilder)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="212aa-361">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="212aa-362">**StartWith (akce<IApplicationBuilder> aplikace)**</span><span class="sxs-lookup"><span data-stu-id="212aa-362">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="212aa-363">Zadejte delegát pro konfiguraci `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="212aa-363">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="212aa-364">Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="212aa-364">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="212aa-365">`WaitForShutdown`bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="212aa-365">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="212aa-366">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="212aa-366">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="212aa-367">**StartWith (řetězce adresy url, akce<IApplicationBuilder> aplikace)**</span><span class="sxs-lookup"><span data-stu-id="212aa-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="212aa-368">Zadejte adresu URL a delegát pro konfiguraci `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="212aa-368">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="212aa-369">Stejný výsledek jako **StartWith (akce<IApplicationBuilder> aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="212aa-369">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="212aa-370">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="212aa-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="212aa-371">**Spustit**</span><span class="sxs-lookup"><span data-stu-id="212aa-371">**Run**</span></span>

<span data-ttu-id="212aa-372">`Run` Metoda spustí webovou aplikaci a blokuje volající vlákno, dokud se vypne hostitele:</span><span class="sxs-lookup"><span data-stu-id="212aa-372">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="212aa-373">**Start**</span><span class="sxs-lookup"><span data-stu-id="212aa-373">**Start**</span></span>

<span data-ttu-id="212aa-374">Můžete spustit hostitele způsobem neblokující voláním jeho `Start` metoda:</span><span class="sxs-lookup"><span data-stu-id="212aa-374">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="212aa-375">Pokud předáte seznam adres URL `Start` metoda, naslouchá na zadané adresy URL:</span><span class="sxs-lookup"><span data-stu-id="212aa-375">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="212aa-376">IHostingEnvironment rozhraní</span><span class="sxs-lookup"><span data-stu-id="212aa-376">IHostingEnvironment interface</span></span>

<span data-ttu-id="212aa-377">[IHostingEnvironment rozhraní](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) poskytuje informace o hostování prostředí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="212aa-377">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="212aa-378">Můžete použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:</span><span class="sxs-lookup"><span data-stu-id="212aa-378">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="212aa-379">Můžete použít [založené na konvenci přístup](xref:fundamentals/environments#startup-conventions) ke konfiguraci vaší aplikace při spuštění založených na prostředí.</span><span class="sxs-lookup"><span data-stu-id="212aa-379">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="212aa-380">Alternativně můžete vložit `IHostingEnvironment` do `Startup` konstruktor pro použití v `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="212aa-380">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="212aa-381">Kromě `IsDevelopment` metoda rozšíření `IHostingEnvironment` nabízí `IsStaging`, `IsProduction`, a `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="212aa-381">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="212aa-382">V tématu [práce s několika prostředí](xref:fundamentals/environments) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="212aa-382">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="212aa-383">`IHostingEnvironment` Služby může také vložit přímo do `Configure` metoda pro nastavení vašeho kanálu zpracování:</span><span class="sxs-lookup"><span data-stu-id="212aa-383">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="212aa-384">Můžete vložit `IHostingEnvironment` do `Invoke` metoda při vytváření vlastní [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="212aa-384">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="212aa-385">IApplicationLifetime rozhraní</span><span class="sxs-lookup"><span data-stu-id="212aa-385">IApplicationLifetime interface</span></span>

<span data-ttu-id="212aa-386">[IApplicationLifetime rozhraní](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umožňuje provádět po spuštění a vypnutí aktivity.</span><span class="sxs-lookup"><span data-stu-id="212aa-386">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="212aa-387">Zrušení tokenů, které můžete zaregistrovat s jsou tři vlastnosti na rozhraní `Action` metody k definování události spuštění a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="212aa-387">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="212aa-388">K dispozici je také `StopApplication` metoda.</span><span class="sxs-lookup"><span data-stu-id="212aa-388">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="212aa-389">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="212aa-389">Cancellation Token</span></span>    | <span data-ttu-id="212aa-390">Aktivuje, když &#8230;</span><span class="sxs-lookup"><span data-stu-id="212aa-390">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="212aa-391">Hostitele plně byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="212aa-391">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="212aa-392">Hostitel provádí řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="212aa-392">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="212aa-393">Může být stále aktivní žádosti.</span><span class="sxs-lookup"><span data-stu-id="212aa-393">Requests may still be processing.</span></span> <span data-ttu-id="212aa-394">Vypnutí bloky až po dokončení této události.</span><span class="sxs-lookup"><span data-stu-id="212aa-394">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="212aa-395">Hostitel je dokončení řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="212aa-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="212aa-396">Všechny požadavky, měla by být zpracována.</span><span class="sxs-lookup"><span data-stu-id="212aa-396">All requests should be processed.</span></span> <span data-ttu-id="212aa-397">Vypnutí bloky až po dokončení této události.</span><span class="sxs-lookup"><span data-stu-id="212aa-397">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="212aa-398">Metoda</span><span class="sxs-lookup"><span data-stu-id="212aa-398">Method</span></span>            | <span data-ttu-id="212aa-399">Akce</span><span class="sxs-lookup"><span data-stu-id="212aa-399">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="212aa-400">Žádosti o ukončení aktuální aplikace.</span><span class="sxs-lookup"><span data-stu-id="212aa-400">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="212aa-401">Řešení potíží s System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="212aa-401">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="212aa-402">**Platí pro pouze ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="212aa-402">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="212aa-403">Pokud vytvoříte hostitele vložením `IStartup` přímo do kontejneru pro vkládání závislosti místo volání `UseStartup` nebo `Configure`, může dojít k následující chybě: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="212aa-403">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="212aa-404">K tomu dochází, protože [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (aktuální sestavení) je nutná ke kontrole pro `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="212aa-404">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="212aa-405">Pokud ručně vložit `IStartup` do kontejneru pro vkládání závislosti, přidejte následující volání vaší `WebHostBuilder` se zadaným názvem sestavení:</span><span class="sxs-lookup"><span data-stu-id="212aa-405">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="212aa-406">Můžete taky přidat fiktivní `Configure` k vaší `WebHostBuilder`, která nastaví `applicationName`(`ApplicationKey`) automaticky:</span><span class="sxs-lookup"><span data-stu-id="212aa-406">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="212aa-407">**Poznámka:**: Toto je pouze požadované verze technologie ASP.NET 2.0 jádra a pouze pokud nemůžete volat `UseStartup` nebo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="212aa-407">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="212aa-408">Další informace najdete v tématu [oznámení: Microsoft.Extensions.PlatformAbstractions byl odebrán (komentář)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) a [StartupInjection ukázka](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="212aa-408">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="212aa-409">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="212aa-409">Additional resources</span></span>

* [<span data-ttu-id="212aa-410">Publikovat do systému Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="212aa-410">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="212aa-411">Publikování do systému Linux pomocí Nginx</span><span class="sxs-lookup"><span data-stu-id="212aa-411">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="212aa-412">Publikování do systému Linux pomocí Apache</span><span class="sxs-lookup"><span data-stu-id="212aa-412">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="212aa-413">Hostitele ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="212aa-413">Host in a Windows Service</span></span>](xref:hosting/windows-service)
