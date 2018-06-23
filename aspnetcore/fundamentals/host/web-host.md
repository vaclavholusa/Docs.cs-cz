---
title: ASP.NET Core webového hostitele
author: guardrex
description: Další informace o webového hostitele v ASP.NET Core, která je zodpovědná za spuštění a životního cyklu správy aplikací.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 98070f49c98919e7ebff41ecc69c953249977dcc
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314146"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="3bb8d-103">ASP.NET Core webového hostitele</span><span class="sxs-lookup"><span data-stu-id="3bb8d-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="3bb8d-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3bb8d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3bb8d-105">Aplikace ASP.NET Core nakonfigurovat a spustit *hostitele*.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="3bb8d-106">Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="3bb8d-107">Minimálně hostitele nakonfiguruje server a kanálu zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="3bb8d-108">Toto téma popisuje ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), což je užitečné pro hostování webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="3bb8d-109">Pro pokrytí obecné hostitele .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), najdete v článku [obecné hostitele](xref:fundamentals/host/generic-host) tématu.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="3bb8d-110">Nastavení hostitele</span><span class="sxs-lookup"><span data-stu-id="3bb8d-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-111">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="3bb8d-112">Vytvořit pomocí instance hostitele [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="3bb8d-113">To se obvykle provádí v vstupní bod aplikace, `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="3bb8d-114">V rámci šablon projektu `Main` se nachází v *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="3bb8d-115">Typické *Program.cs* volání [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zahájíte nastavení hostitele:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="3bb8d-116">`CreateDefaultBuilder` provádí následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="3bb8d-117">Nakonfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako webový server a nakonfiguruje server pomocí poskytovatelů hostingu konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="3bb8d-118">Výchozí možnosti Kestrel najdete v tématu [Kestrel možnosti části Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="3bb8d-119">Nastaví obsahu kořenovou cestu vrácený [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="3bb8d-120">Načítání [konfigurace hostitele](#host-configuration-values) z:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-120">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="3bb8d-121">Proměnné prostředí s předponou `ASPNETCORE_` (například `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-121">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="3bb8d-122">Argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-122">Command-line arguments.</span></span>
* <span data-ttu-id="3bb8d-123">Načítání z konfigurace aplikací:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-123">Loads app configuration from:</span></span>
  * <span data-ttu-id="3bb8d-124">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-124">*appsettings.json*.</span></span>
  * <span data-ttu-id="3bb8d-125">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-125">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="3bb8d-126">[Tajné klíče uživatele](xref:security/app-secrets) při spuštění aplikace `Development` prostředí pomocí položky sestavení.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-126">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="3bb8d-127">Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-127">Environment variables.</span></span>
  * <span data-ttu-id="3bb8d-128">Argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-128">Command-line arguments.</span></span>
* <span data-ttu-id="3bb8d-129">Nakonfiguruje [protokolování](xref:fundamentals/logging/index) konzoly a ladění výstupu.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-129">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="3bb8d-130">Zahrnuje protokolování [filtrování protokolu](xref:fundamentals/logging/index#log-filtering) pravidla stanovená v části Konfigurace protokolování *appSettings.JSON určený* nebo *appsettings. { Prostředí} .json* souboru.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-130">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="3bb8d-131">Když spustíte za služby IIS, umožňuje [integrační služby IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-131">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="3bb8d-132">Konfiguruje základní cesta a portu server naslouchá na při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-132">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="3bb8d-133">Modul vytvoří reverzní proxy server mezi službou IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-133">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="3bb8d-134">Nakonfiguruje aplikaci taky [zaznamenat chyby při spuštění](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-134">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="3bb8d-135">Výchozí možnosti služby IIS najdete v tématu [IIS možnosti oddílu hostitele ASP.NET Core v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-135">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="3bb8d-136">Nastaví [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) k `true` Pokud prostředí aplikace je vývoj.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-136">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="3bb8d-137">Další informace najdete v tématu [obor ověření](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-137">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="3bb8d-138">Konfigurace definované `CreateDefaultBuilder` můžete přepsat a rozšířen o [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)a jiné metody a metodami rozšíření [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-138">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="3bb8d-139">Následuje několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-139">A few examples follow:</span></span>

* <span data-ttu-id="3bb8d-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) slouží k určení dalších `IConfiguration` pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="3bb8d-141">Následující `ConfigureAppConfiguration` volání přidá delegáta, kterého patří konfigurace aplikací v *appsettings.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-141">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="3bb8d-142">`ConfigureAppConfiguration` může volat vícekrát.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-142">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="3bb8d-143">Všimněte si, že tato konfigurace se nevztahuje na hostitele (například adresy URL serveru nebo prostředí).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-143">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="3bb8d-144">Najdete v článku [hostitele hodnoty konfigurace](#host-configuration-values) části.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-144">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="3bb8d-145">Následující `ConfigureLogging` volání přidá delegát pro konfiguraci úroveň protokolování minimální ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) k [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-145">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="3bb8d-146">Toto nastavení potlačí nastavení v *appsettings. Development.JSON* (`LogLevel.Debug`) a *appsettings. Production.JSON* (`LogLevel.Error`) nakonfiguroval `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-146">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="3bb8d-147">`ConfigureLogging` může volat vícekrát.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-147">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

* <span data-ttu-id="3bb8d-148">Následující volání do [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) přepíše výchozí [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 bajtů navázat, když Kestrel nakonfigurovaly `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

<span data-ttu-id="3bb8d-149">*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="3bb8d-150">Když se aplikace spustí z kořenové složky projektu, projektu kořenové složky se používá jako kořenu obsahu.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="3bb8d-151">Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="3bb8d-152">Další informace o konfiguraci aplikace, najdete v části [konfigurace v ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-152">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="3bb8d-153">Jako alternativu k použití statických `CreateDefaultBuilder` metody vytvoření hostitele z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) je podporované přístup pomocí ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="3bb8d-154">Další informace najdete v tématu kartě 1.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-155">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-155">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="3bb8d-156">Vytvořit pomocí instance hostitele [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-156">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="3bb8d-157">Vytvoření hostitele se obvykle provádí v vstupní bod aplikace, `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-157">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="3bb8d-158">V rámci šablon projektu `Main` se nachází v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-158">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="3bb8d-159">`WebHostBuilder` vyžaduje [serveru, který implementuje IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-159">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="3bb8d-160">Jsou předdefinované servery [Kestrel](xref:fundamentals/servers/kestrel) a [HTTP.sys](xref:fundamentals/servers/httpsys) (před verzí technologie ASP.NET 2.0 jádra, ovladač HTTP.sys volala [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-160">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="3bb8d-161">V tomto příkladu [UseKestrel rozšíření metoda](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) určuje Kestrel server.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-161">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="3bb8d-162">*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-162">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="3bb8d-163">Výchozí kořen obsahu se získávají pro `UseContentRoot` podle [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-163">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="3bb8d-164">Když se aplikace spustí z kořenové složky projektu, projektu kořenové složky se používá jako kořenu obsahu.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-164">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="3bb8d-165">Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-165">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="3bb8d-166">Chcete-li použít jako reverzní proxy server služby IIS, volejte [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) jako součást sestavení hostitele.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-166">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="3bb8d-167">`UseIISIntegration` neprovede konfiguraci *server*, například [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-167">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="3bb8d-168">`UseIISIntegration` Konfiguruje základní cesta a portu server naslouchá na při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) vytvořit reverzní proxy server mezi Kestrel a služby IIS.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-168">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="3bb8d-169">Použití služby IIS s ASP.NET Core `UseKestrel` a `UseIISIntegration` musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-169">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="3bb8d-170">`UseIISIntegration` aktivuje pouze při spuštění za služby IIS nebo IIS Express.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-170">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="3bb8d-171">Další informace najdete v tématu [Úvod k modulu jádra ASP.NET](xref:fundamentals/servers/aspnet-core-module) a [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-171">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="3bb8d-172">Minimální implementace, které konfiguruje hostitele (a aplikace ASP.NET Core) zahrnuje určení serveru a konfiguraci kanálu žádostí aplikace:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-172">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="3bb8d-173">Při nastavování hostitele, [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metody lze zadat.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-173">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="3bb8d-174">Pokud `Startup` je zadána třída, musíte definovat `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-174">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="3bb8d-175">Další informace najdete v tématu [spuštění aplikace v ASP.NET Core](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-175">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="3bb8d-176">Více volá, aby se `ConfigureServices` připojit k sobě.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-176">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="3bb8d-177">Více volá, aby se `Configure` nebo `UseStartup` na `WebHostBuilder` nahradit předchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-177">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="3bb8d-178">Hodnoty konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="3bb8d-178">Host configuration values</span></span>

<span data-ttu-id="3bb8d-179">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) závisí na následujících dvou přístupů k nastavení hostitelské hodnoty konfigurace:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-179">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="3bb8d-180">Konfigurace hostitele tvůrce, který zahrnuje proměnné prostředí ve formátu `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-180">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="3bb8d-181">Například `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-181">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="3bb8d-182">Rozšíření, jako [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) a [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (najdete v článku [konfigurace přepisování](#override-configuration) části).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-182">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="3bb8d-183">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) a přidružené klíče.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-183">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="3bb8d-184">Při nastavení hodnoty s `UseSetting`, je hodnota nastavena jako bez ohledu na typ řetězec.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-184">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="3bb8d-185">Hostitel používá. obě tyto možnosti nastaví hodnotu poslední.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-185">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="3bb8d-186">Další informace najdete v tématu [konfigurace přepisování](#override-configuration) v další části.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-186">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="3bb8d-187">Zaznamenat chyby při spuštění</span><span class="sxs-lookup"><span data-stu-id="3bb8d-187">Capture Startup Errors</span></span>

<span data-ttu-id="3bb8d-188">Toto nastavení řídí zaznamenávání chyby při spuštění.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-188">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="3bb8d-189">**Klíč**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="3bb8d-189">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="3bb8d-190">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="3bb8d-190">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3bb8d-191">**Výchozí**: použije se výchozí hodnota `false` Pokud aplikace běží s Kestrel za služby IIS, kde výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-191">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="3bb8d-192">**Nastavit pomocí**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-192">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="3bb8d-193">**Proměnné prostředí**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-193">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="3bb8d-194">Když `false`, chyb během spuštění výsledek v hostiteli. operace bude ukončena.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-194">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="3bb8d-195">Když `true`, hostitel zachytí výjimky během spouštění a pokusí o spuštění serveru.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-195">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-196">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-197">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-197">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="3bb8d-198">Obsah kořenové</span><span class="sxs-lookup"><span data-stu-id="3bb8d-198">Content Root</span></span>

<span data-ttu-id="3bb8d-199">Toto nastavení určuje, kde začíná ASP.NET Core hledání obsahu souborů, jako je například zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-199">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="3bb8d-200">**Klíč**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="3bb8d-200">**Key**: contentRoot</span></span>  
<span data-ttu-id="3bb8d-201">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="3bb8d-201">**Type**: *string*</span></span>  
<span data-ttu-id="3bb8d-202">**Výchozí**: výchozí složce, kde se nachází sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-202">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="3bb8d-203">**Nastavit pomocí**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-203">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="3bb8d-204">**Proměnné prostředí**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-204">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="3bb8d-205">Kořenu obsahu se používá jako základní cesta pro [kořenový Web nastavení](#web-root).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-205">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="3bb8d-206">Pokud cesta neexistuje, hostitel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-206">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-207">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-207">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-208">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-208">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="3bb8d-209">Podrobné chyby</span><span class="sxs-lookup"><span data-stu-id="3bb8d-209">Detailed Errors</span></span>

<span data-ttu-id="3bb8d-210">Určuje, zda podrobné chyby, které mají být zaznamenány.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-210">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="3bb8d-211">**Klíč**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="3bb8d-211">**Key**: detailedErrors</span></span>  
<span data-ttu-id="3bb8d-212">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="3bb8d-212">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3bb8d-213">**Výchozí**: false</span><span class="sxs-lookup"><span data-stu-id="3bb8d-213">**Default**: false</span></span>  
<span data-ttu-id="3bb8d-214">**Nastavit pomocí**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-214">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="3bb8d-215">**Proměnné prostředí**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-215">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="3bb8d-216">Když povolené (nebo když <a href="#environment">prostředí</a> je nastaven na `Development`), aplikace zaznamená podrobné výjimky.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-216">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-217">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-218">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-218">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="3bb8d-219">Prostředí</span><span class="sxs-lookup"><span data-stu-id="3bb8d-219">Environment</span></span>

<span data-ttu-id="3bb8d-220">Nastaví prostředí aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-220">Sets the app's environment.</span></span>

<span data-ttu-id="3bb8d-221">**Klíč**: prostředí</span><span class="sxs-lookup"><span data-stu-id="3bb8d-221">**Key**: environment</span></span>  
<span data-ttu-id="3bb8d-222">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="3bb8d-222">**Type**: *string*</span></span>  
<span data-ttu-id="3bb8d-223">**Výchozí**: produkční</span><span class="sxs-lookup"><span data-stu-id="3bb8d-223">**Default**: Production</span></span>  
<span data-ttu-id="3bb8d-224">**Nastavit pomocí**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-224">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="3bb8d-225">**Proměnné prostředí**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-225">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="3bb8d-226">Prostředí můžete nastavit na jakoukoli hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-226">The environment can be set to any value.</span></span> <span data-ttu-id="3bb8d-227">Framework definované hodnoty zahrnují `Development`, `Staging`, a `Production`.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-227">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="3bb8d-228">Hodnoty nejsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-228">Values aren't case sensitive.</span></span> <span data-ttu-id="3bb8d-229">Ve výchozím nastavení *prostředí* je pro čtení z `ASPNETCORE_ENVIRONMENT` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-229">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="3bb8d-230">Při použití [Visual Studio](https://www.visualstudio.com/), proměnné prostředí může být nastavena v *launchSettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-230">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="3bb8d-231">Další informace najdete v tématu [použijte prostředí s více](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-231">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-232">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-232">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-233">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-233">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="3bb8d-234">Hostování spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="3bb8d-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="3bb8d-235">Nastaví hostování spuštění sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="3bb8d-236">**Klíč**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="3bb8d-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="3bb8d-237">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="3bb8d-237">**Type**: *string*</span></span>  
<span data-ttu-id="3bb8d-238">**Výchozí**: prázdný řetězec</span><span class="sxs-lookup"><span data-stu-id="3bb8d-238">**Default**: Empty string</span></span>  
<span data-ttu-id="3bb8d-239">**Nastavit pomocí**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="3bb8d-240">**Proměnné prostředí**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="3bb8d-241">Řetězec oddělený středníkem hostování spuštění sestavení načíst při spuštění.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="3bb8d-242">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-242">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="3bb8d-243">I když výchozí hodnota konfigurace je řetězec prázdný, hostování spuštění sestavení, vždy zahrňte sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-243">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="3bb8d-244">Při hostování spuštění sestavení jsou k dispozici, se přidají do sestavení aplikace pro načtení, když aplikace sestavení jeho společných služeb během spouštění.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-244">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-245">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-245">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-246">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-246">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3bb8d-247">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-247">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="3bb8d-248">Dáváte přednost hostování adresy URL</span><span class="sxs-lookup"><span data-stu-id="3bb8d-248">Prefer Hosting URLs</span></span>

<span data-ttu-id="3bb8d-249">Určuje, zda hostitel naslouchat požadavkům na adresy URL nakonfigurované `WebHostBuilder` místo nastavení nakonfigurovaného pomocí `IServer` implementace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-249">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="3bb8d-250">**Klíč**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="3bb8d-250">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="3bb8d-251">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="3bb8d-251">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3bb8d-252">**Výchozí**: true</span><span class="sxs-lookup"><span data-stu-id="3bb8d-252">**Default**: true</span></span>  
<span data-ttu-id="3bb8d-253">**Nastavit pomocí**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-253">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="3bb8d-254">**Proměnné prostředí**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-254">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="3bb8d-255">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-255">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-256">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-256">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3bb8d-258">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-258">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="3bb8d-259">Zabránit spuštění hostování</span><span class="sxs-lookup"><span data-stu-id="3bb8d-259">Prevent Hosting Startup</span></span>

<span data-ttu-id="3bb8d-260">Brání automatické načítání hostování spuštění sestavení, a to včetně hostování spuštění sestavení nakonfiguroval sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-260">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="3bb8d-261">V tématu [vylepšení aplikace z externí sestavení s IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) Další informace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-261">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="3bb8d-262">**Klíč**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="3bb8d-262">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="3bb8d-263">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="3bb8d-263">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3bb8d-264">**Výchozí**: false</span><span class="sxs-lookup"><span data-stu-id="3bb8d-264">**Default**: false</span></span>  
<span data-ttu-id="3bb8d-265">**Nastavit pomocí**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-265">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="3bb8d-266">**Proměnné prostředí**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-266">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="3bb8d-267">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-267">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-268">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-268">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-269">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3bb8d-270">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-270">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="3bb8d-271">Adresy URL serveru</span><span class="sxs-lookup"><span data-stu-id="3bb8d-271">Server URLs</span></span>

<span data-ttu-id="3bb8d-272">Určuje IP adres nebo adres hostitele s porty a protokoly, které server naslouchat požadavkům na požadavky.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-272">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="3bb8d-273">**Klíč**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="3bb8d-273">**Key**: urls</span></span>  
<span data-ttu-id="3bb8d-274">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="3bb8d-274">**Type**: *string*</span></span>  
<span data-ttu-id="3bb8d-275">**Výchozí**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="3bb8d-275">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="3bb8d-276">**Nastavit pomocí**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-276">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="3bb8d-277">**Proměnné prostředí**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-277">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="3bb8d-278">Nastavte na oddělených středníkem (;) seznam URL předpony serveru by měl odpovídat.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-278">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="3bb8d-279">Například `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-279">For example, `http://localhost:123`.</span></span> <span data-ttu-id="3bb8d-280">Použití "\*" k označení, že by měl server přijímat požadavky na všechny IP adresy nebo názvu hostitele pomocí zadaný port a protokol (například `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-280">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="3bb8d-281">Protokol (`http://` nebo `https://`) musí být součástí každou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-281">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="3bb8d-282">Podporované formáty liší mezi servery.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-282">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-283">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-283">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="3bb8d-284">Kestrel má svůj vlastní koncový bod rozhraní API konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-284">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="3bb8d-285">Další informace najdete v tématu [Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-285">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-286">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-286">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="3bb8d-287">Časový limit vypnutí</span><span class="sxs-lookup"><span data-stu-id="3bb8d-287">Shutdown Timeout</span></span>

<span data-ttu-id="3bb8d-288">Určuje dobu čekání na webového hostitele a ukončí se.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-288">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="3bb8d-289">**Klíč**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="3bb8d-289">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="3bb8d-290">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="3bb8d-290">**Type**: *int*</span></span>  
<span data-ttu-id="3bb8d-291">**Výchozí**: 5</span><span class="sxs-lookup"><span data-stu-id="3bb8d-291">**Default**: 5</span></span>  
<span data-ttu-id="3bb8d-292">**Nastavit pomocí**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-292">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="3bb8d-293">**Proměnné prostředí**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-293">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="3bb8d-294">I když přijme klíč *int* s `UseSetting` (například `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) rozšíření metoda přebírá [časový interval](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-294">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="3bb8d-295">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-295">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="3bb8d-296">Během časového limitu období, hostování:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-296">During the timeout period, hosting:</span></span>

* <span data-ttu-id="3bb8d-297">Aktivační události [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-297">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="3bb8d-298">Pokusy o zastavení hostované služby, protokolování pro služby, které se nepodařilo zastavit všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-298">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="3bb8d-299">Pokud vyprší časový limit před všechny zastavení hostovaných služeb, jsou zastaveny všechny zbývající služby active při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-299">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="3bb8d-300">Zastavení služeb i v případě, že se nedokončilo zpracování.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-300">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="3bb8d-301">Pokud služby vyžadují další čas ukončení, zvýšit časový limit.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-301">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-302">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-302">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-303">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-303">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3bb8d-304">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-304">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="3bb8d-305">Spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="3bb8d-305">Startup Assembly</span></span>

<span data-ttu-id="3bb8d-306">Určuje sestavení pro vyhledávání `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-306">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="3bb8d-307">**Klíč**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="3bb8d-307">**Key**: startupAssembly</span></span>  
<span data-ttu-id="3bb8d-308">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="3bb8d-308">**Type**: *string*</span></span>  
<span data-ttu-id="3bb8d-309">**Výchozí**: sestavení aplikace</span><span class="sxs-lookup"><span data-stu-id="3bb8d-309">**Default**: The app's assembly</span></span>  
<span data-ttu-id="3bb8d-310">**Nastavit pomocí**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-310">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="3bb8d-311">**Proměnné prostředí**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-311">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="3bb8d-312">Sestavení podle názvu (`string`) nebo typ (`TStartup`) může být odkaz.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-312">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="3bb8d-313">Pokud je to více `UseStartup` metody jsou volány, poslední změny mají přednost.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-313">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-314">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="3bb8d-316">Kořenový web</span><span class="sxs-lookup"><span data-stu-id="3bb8d-316">Web Root</span></span>

<span data-ttu-id="3bb8d-317">Nastaví relativní cestu na statické prostředky aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-317">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="3bb8d-318">**Klíč**: webroot</span><span class="sxs-lookup"><span data-stu-id="3bb8d-318">**Key**: webroot</span></span>  
<span data-ttu-id="3bb8d-319">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="3bb8d-319">**Type**: *string*</span></span>  
<span data-ttu-id="3bb8d-320">**Výchozí**: Pokud není zadáno, výchozí hodnota je "(Content Root)/wwwroot", pokud cesta existuje.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-320">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="3bb8d-321">Pokud cesta neexistuje, je použít soubor no-op zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-321">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="3bb8d-322">**Nastavit pomocí**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-322">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="3bb8d-323">**Proměnné prostředí**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="3bb8d-323">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-324">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-324">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="3bb8d-326">Přepsat konfiguraci</span><span class="sxs-lookup"><span data-stu-id="3bb8d-326">Override configuration</span></span>

<span data-ttu-id="3bb8d-327">Použití [konfigurace](xref:fundamentals/configuration/index) konfigurace webového hostitele.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-327">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="3bb8d-328">V následujícím příkladu je konfigurace hostitele volitelně specifikován v *hostsettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-328">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="3bb8d-329">Všechny konfigurace načtena z *hostsettings.json* soubor může být přepsána argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-329">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="3bb8d-330">Integrovaný konfigurace (v `config`) se používá ke konfiguraci hostitele s [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-330">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="3bb8d-331">`IWebHostBuilder` konfigurace je přidaný do konfigurace aplikace, ale konverzace není true&mdash; `ConfigureAppConfiguration` nemá vliv `IWebHostBuilder` konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-331">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-332">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3bb8d-333">Přepsání zadaná podle konfigurace `UseUrls` s *hostsettings.json* konfigurace příkazového řádku, první argument konfigurace druhý:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-333">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="3bb8d-334">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-334">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-335">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-335">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3bb8d-336">Přepsání zadaná podle konfigurace `UseUrls` s *hostsettings.json* konfigurace příkazového řádku, první argument konfigurace druhý:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-336">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="3bb8d-337">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-337">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

---

> [!NOTE]
> <span data-ttu-id="3bb8d-338">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) metoda rozšíření není aktuálně schopen analyzovat konfigurační oddíl vrácený `GetSection` (například `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-338">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="3bb8d-339">`GetSection` Metoda filtry konfigurace klíče do části požadovaný, ale ponechá název oddílu na klíče (například `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-339">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="3bb8d-340">`UseConfiguration` Metoda očekává klíče tak, aby odpovídala `WebHostBuilder` klíče (například `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-340">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="3bb8d-341">Přítomnost názvu oddílu na klíče zabrání hodnoty v části Konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-341">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="3bb8d-342">Tento problém bude vyřešen v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-342">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="3bb8d-343">Další informace a řešení, najdete v části [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration používá úplné klíče](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-343">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="3bb8d-344">`UseConfiguration` zkopíruje jen klíče ze zadaných `IConfiguration` Tvůrce konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-344">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="3bb8d-345">Proto nastavení `reloadOnChange: true` pro soubory XML, JSON a INI nastavení nemá žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-345">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="3bb8d-346">Pokud chcete zadat hostiteli spustit na konkrétní adresu URL, požadovanou hodnotu lze předat ve z příkazového řádku při provádění [dotnet spustit](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-346">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="3bb8d-347">Přepíše argument příkazového řádku `urls` z hodnoty *hostsettings.json* souboru a server naslouchá na portu 8080:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-347">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="3bb8d-348">Spravovat hostitele</span><span class="sxs-lookup"><span data-stu-id="3bb8d-348">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bb8d-349">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-349">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3bb8d-350">**Spustit**</span><span class="sxs-lookup"><span data-stu-id="3bb8d-350">**Run**</span></span>

<span data-ttu-id="3bb8d-351">`Run` Metoda spustí webovou aplikaci a blokuje volající vlákno, dokud se vypne hostitele:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-351">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="3bb8d-352">**Start**</span><span class="sxs-lookup"><span data-stu-id="3bb8d-352">**Start**</span></span>

<span data-ttu-id="3bb8d-353">Spuštění hostitele způsobem neblokující voláním jeho `Start` metoda:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-353">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="3bb8d-354">Pokud je předán seznam adres URL, `Start` metoda, naslouchá na zadané adresy URL:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-354">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="3bb8d-355">Aplikace můžete inicializaci a spuštění nové hostitele pomocí předem nakonfigurovaných výchozích nastavení `CreateDefaultBuilder` pomocí jiné metody statické pohodlí.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-355">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="3bb8d-356">Tyto metody start pro server bez výstup konzoly a s [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) počkejte zalomení (Ctrl-C nebo sigint – nebo SIGTERM –):</span><span class="sxs-lookup"><span data-stu-id="3bb8d-356">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="3bb8d-357">**Spuštění (RequestDelegate aplikace)**</span><span class="sxs-lookup"><span data-stu-id="3bb8d-357">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="3bb8d-358">Začněte `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-358">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3bb8d-359">Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="3bb8d-359">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="3bb8d-360">`WaitForShutdown` bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-360">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="3bb8d-361">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-361">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="3bb8d-362">**Spuštění (řetězec adresy url, RequestDelegate aplikace)**</span><span class="sxs-lookup"><span data-stu-id="3bb8d-362">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="3bb8d-363">Spustit s adresou URL a `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-363">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3bb8d-364">Stejný výsledek jako **spuštění (RequestDelegate aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-364">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="3bb8d-365">**Spuštění (akce&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="3bb8d-365">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="3bb8d-366">Použít instanci `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) používat směrování middleware:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-366">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="3bb8d-367">Použijte následující požadavky na prohlížeč s příkladu:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-367">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="3bb8d-368">Požadavek</span><span class="sxs-lookup"><span data-stu-id="3bb8d-368">Request</span></span>                                    | <span data-ttu-id="3bb8d-369">Odpověď</span><span class="sxs-lookup"><span data-stu-id="3bb8d-369">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="3bb8d-370">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="3bb8d-370">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="3bb8d-371">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="3bb8d-371">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="3bb8d-372">Vyvolá výjimku řetězcem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="3bb8d-372">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="3bb8d-373">Vyvolá výjimku řetězcem "Uh jejda!"</span><span class="sxs-lookup"><span data-stu-id="3bb8d-373">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="3bb8d-374">Santé, kevina!</span><span class="sxs-lookup"><span data-stu-id="3bb8d-374">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="3bb8d-375">Ahoj světe!</span><span class="sxs-lookup"><span data-stu-id="3bb8d-375">Hello World!</span></span>                             |

<span data-ttu-id="3bb8d-376">`WaitForShutdown` bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-376">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="3bb8d-377">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-377">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="3bb8d-378">**Spuštění (řetězce adresy url, akce&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="3bb8d-378">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="3bb8d-379">Použijte adresu URL a instance `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-379">Use a URL and an instance of `IRouteBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3bb8d-380">Stejný výsledek jako **spuštění (akce&lt;IRouteBuilder&gt; routeBuilder)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-380">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="3bb8d-381">**StartWith (akce&lt;IApplicationBuilder&gt; aplikace)**</span><span class="sxs-lookup"><span data-stu-id="3bb8d-381">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="3bb8d-382">Zadejte delegát pro konfiguraci `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-382">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3bb8d-383">Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="3bb8d-383">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="3bb8d-384">`WaitForShutdown` bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-384">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="3bb8d-385">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-385">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="3bb8d-386">**StartWith (řetězce adresy url, akce&lt;IApplicationBuilder&gt; aplikace)**</span><span class="sxs-lookup"><span data-stu-id="3bb8d-386">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="3bb8d-387">Zadejte adresu URL a delegát pro konfiguraci `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-387">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3bb8d-388">Stejný výsledek jako **StartWith (akce&lt;IApplicationBuilder&gt; aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-388">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bb8d-389">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bb8d-389">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3bb8d-390">**Spustit**</span><span class="sxs-lookup"><span data-stu-id="3bb8d-390">**Run**</span></span>

<span data-ttu-id="3bb8d-391">`Run` Metoda spustí webovou aplikaci a blokuje volající vlákno, dokud se vypne hostitele:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-391">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="3bb8d-392">**Start**</span><span class="sxs-lookup"><span data-stu-id="3bb8d-392">**Start**</span></span>

<span data-ttu-id="3bb8d-393">Spuštění hostitele způsobem neblokující voláním jeho `Start` metoda:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-393">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="3bb8d-394">Pokud je předán seznam adres URL, `Start` metoda, naslouchá na zadané adresy URL:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-394">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="3bb8d-395">IHostingEnvironment rozhraní</span><span class="sxs-lookup"><span data-stu-id="3bb8d-395">IHostingEnvironment interface</span></span>

<span data-ttu-id="3bb8d-396">[IHostingEnvironment rozhraní](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) poskytuje informace o hostování prostředí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-396">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="3bb8d-397">Použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-397">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="3bb8d-398">A [založené na konvenci přístup](xref:fundamentals/environments#environment-based-startup-class-and-methods) můžete použít ke konfiguraci aplikace při spuštění založených na prostředí.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-398">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="3bb8d-399">Můžete také vložit `IHostingEnvironment` do `Startup` konstruktor pro použití v `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-399">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="3bb8d-400">Kromě `IsDevelopment` metoda rozšíření `IHostingEnvironment` nabízí `IsStaging`, `IsProduction`, a `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-400">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="3bb8d-401">V tématu [použijte prostředí s více](xref:fundamentals/environments) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-401">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="3bb8d-402">`IHostingEnvironment` Služby může také vložit přímo do `Configure` metoda pro nastavení zpracování kanálu:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-402">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="3bb8d-403">`IHostingEnvironment` můžete vložit do `Invoke` metoda při vytváření vlastní [middleware](xref:fundamentals/middleware/index#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="3bb8d-403">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="3bb8d-404">IApplicationLifetime rozhraní</span><span class="sxs-lookup"><span data-stu-id="3bb8d-404">IApplicationLifetime interface</span></span>

<span data-ttu-id="3bb8d-405">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umožňuje po spuštění a vypnutí aktivity.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-405">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="3bb8d-406">Zrušení tokenů použitá pro zaregistrování jsou tři vlastnosti na rozhraní `Action` metody, které definují události spuštění a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-406">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="3bb8d-407">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="3bb8d-407">Cancellation Token</span></span>    | <span data-ttu-id="3bb8d-408">Při aktivaci&#8230;</span><span class="sxs-lookup"><span data-stu-id="3bb8d-408">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="3bb8d-409">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="3bb8d-409">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="3bb8d-410">Hostitele plně byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-410">The host has fully started.</span></span> |
| [<span data-ttu-id="3bb8d-411">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="3bb8d-411">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="3bb8d-412">Hostitel je dokončení řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-412">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="3bb8d-413">Všechny požadavky, měla by být zpracována.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-413">All requests should be processed.</span></span> <span data-ttu-id="3bb8d-414">Vypnutí bloky až po dokončení této události.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-414">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="3bb8d-415">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="3bb8d-415">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="3bb8d-416">Hostitel provádí řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-416">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="3bb8d-417">Může být stále aktivní žádosti.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-417">Requests may still be processing.</span></span> <span data-ttu-id="3bb8d-418">Vypnutí bloky až po dokončení této události.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-418">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="3bb8d-419">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) požadavky ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-419">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="3bb8d-420">Následující třídy používá `StopApplication` korektně vypnout aplikaci při třídy `Shutdown` metoda je volána:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-420">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a><span data-ttu-id="3bb8d-421">Ověření oboru</span><span class="sxs-lookup"><span data-stu-id="3bb8d-421">Scope validation</span></span>

<span data-ttu-id="3bb8d-422">V technologii ASP.NET Core 2.0 nebo novější [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) nastaví [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) k `true` Pokud prostředí aplikace je vývoj.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-422">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="3bb8d-423">Když `ValidateScopes` je nastaven na `true`, výchozím zprostředkovatelem služeb provádí kontroly, aby ověřte, že:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-423">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="3bb8d-424">Vymezené služby nejsou přímo nebo nepřímo přeložit od kořenové poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-424">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="3bb8d-425">Vymezená služby nejsou přímo nebo nepřímo vložit do jednotlivých prvků.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-425">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="3bb8d-426">Kořenového poskytovatele služby se vytvoří při [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) je volána.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-426">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="3bb8d-427">Doba platnosti poskytovatele služeb kořenové odpovídá na aplikační nebo server životního cyklu, pokud zprostředkovatel začíná aplikace a uvolnění při vypnutí aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-427">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="3bb8d-428">Vymezená služby jsou zapomenuty kontejnerem, který je vytvořil.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-428">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="3bb8d-429">Pokud vymezené služby se vytvoří v kořenovém kontejneru, životnost služby je efektivně povýšen na singleton, protože jejich pouze likvidace podle Kořenový kontejner při ukončení aplikace nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-429">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="3bb8d-430">Ověřování služby Obory zachytí těchto situacích při `BuildServiceProvider` je volána.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-430">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="3bb8d-431">Vždy ověření obory, včetně v provozním prostředí, nakonfigurujte [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) s [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) na tvůrce hostitele:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-431">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="3bb8d-432">Řešení potíží s System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="3bb8d-432">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="3bb8d-433">**Platí pro pouze ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="3bb8d-433">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="3bb8d-434">Hostitel může být sestaven vložením `IStartup` přímo do kontejneru pro vkládání závislosti místo volání `UseStartup` nebo `Configure`:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-434">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="3bb8d-435">Pokud hostitel je integrovaný tímto způsobem, může dojít k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-435">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="3bb8d-436">K tomu dochází, protože [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (aktuální sestavení) je nutná ke kontrole pro `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-436">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="3bb8d-437">Pokud aplikace ručně vloží `IStartup` do kontejneru pro vkládání závislosti, přidejte následující volání `WebHostBuilder` se zadaným názvem sestavení:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-437">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="3bb8d-438">Můžete taky přidat fiktivní `Configure` k `WebHostBuilder`, která nastaví `applicationName`(`ApplicationKey`) automaticky:</span><span class="sxs-lookup"><span data-stu-id="3bb8d-438">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="3bb8d-439">**Poznámka:**: Toto je pouze požadované verze technologie ASP.NET 2.0 jádra a pouze pokud aplikace nemá volání `UseStartup` nebo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="3bb8d-439">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="3bb8d-440">Další informace najdete v tématu [oznámení: Microsoft.Extensions.PlatformAbstractions byl odebrán (komentář)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) a [StartupInjection ukázka](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="3bb8d-440">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3bb8d-441">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3bb8d-441">Additional resources</span></span>

* [<span data-ttu-id="3bb8d-442">Hostování ve Windows se službou IIS</span><span class="sxs-lookup"><span data-stu-id="3bb8d-442">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="3bb8d-443">Hostování v Linuxu na serveru Nginx</span><span class="sxs-lookup"><span data-stu-id="3bb8d-443">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="3bb8d-444">Hostování v Linuxu na serveru Apache</span><span class="sxs-lookup"><span data-stu-id="3bb8d-444">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="3bb8d-445">Hostitele ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="3bb8d-445">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
