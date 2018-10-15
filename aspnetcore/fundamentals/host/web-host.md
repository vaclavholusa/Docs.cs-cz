---
title: Webového hostitele ASP.NET Core
author: guardrex
description: Další informace o webového hostitele v ASP.NET Core, který je zodpovědný za spouštění a životního cyklu správy aplikací.
ms.author: riande
ms.custom: mvc
ms.date: 09/01/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 8b6517b009a289d6b93e2cc1bea60ecace61a3c6
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326157"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="9c2c6-103">Webového hostitele ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9c2c6-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="9c2c6-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9c2c6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9c2c6-105">Konfigurace aplikace ASP.NET Core a spouštění *hostitele*.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="9c2c6-106">Hostitel je zodpovědný za spouštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="9c2c6-107">Minimálně hostitele nakonfiguruje server a kanálu zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="9c2c6-108">Toto téma popisuje webového hostitele ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), což je užitečné pro hostování webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="9c2c6-109">Pro pokrytí obecný hostitele .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), najdete v článku <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="9c2c6-110">Nastavení hostitele</span><span class="sxs-lookup"><span data-stu-id="9c2c6-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9c2c6-111">Vytvoření hostitele pomocí instance [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="9c2c6-112">To se obvykle provádí v vstupní bod aplikace, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="9c2c6-113">V šablonách projektů `Main` se nachází v *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="9c2c6-114">Typické *Program.cs* volání [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) spustit nastavení hostitele:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="9c2c6-115">`CreateDefaultBuilder` provádí následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="9c2c6-116">Nakonfiguruje [Kestrel](xref:fundamentals/servers/kestrel) webového serveru a nakonfiguruje server pomocí aplikace poskytovatelů konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="9c2c6-117">Výchozí možnosti Kestrel najdete v tématu <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="9c2c6-118">Nastaví obsahu kořenovou cesta vrácená procedurou [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="9c2c6-119">Načtení [konfigurace hostitele](#host-configuration-values) od:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="9c2c6-120">Proměnné prostředí s předponou `ASPNETCORE_` (například `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="9c2c6-121">Argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-121">Command-line arguments.</span></span>
* <span data-ttu-id="9c2c6-122">Načte konfiguraci aplikací v uvedeném pořadí od:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-122">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="9c2c6-123">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="9c2c6-124">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="9c2c6-125">[Tajný klíč správce](xref:security/app-secrets) při spuštění aplikace `Development` prostředí s využitím vstupní sestavení.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-125">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="9c2c6-126">Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-126">Environment variables.</span></span>
  * <span data-ttu-id="9c2c6-127">Argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-127">Command-line arguments.</span></span>
* <span data-ttu-id="9c2c6-128">Nakonfiguruje [protokolování](xref:fundamentals/logging/index) pro výstup konzoly a ladění.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="9c2c6-129">Protokolování zahrnuje [filtrování protokolu](xref:fundamentals/logging/index#log-filtering) pravidel specifikovaných v části Konfigurace protokolování *appsettings.json* nebo *appsettings. { Prostředí} .json* souboru.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="9c2c6-130">Při spuštění za služby IIS umožňuje [integrace služby IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="9c2c6-131">Nastaví základní cesta a portu server naslouchá na při použití [modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="9c2c6-132">Modul vytváří reverzní proxy server mezi službou IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="9c2c6-133">Také nakonfiguruje aplikaci [zachycení chyb při spuštění](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="9c2c6-134">Výchozí možnosti služby IIS najdete v tématu <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="9c2c6-135">Nastaví [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) k `true` jestli vývojové prostředí aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="9c2c6-136">Další informace najdete v tématu [oboru ověření](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="9c2c6-137">Konfigurace určené `CreateDefaultBuilder` můžete přepsat a rozšířen o [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)a jiné metody a metody rozšíření [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="9c2c6-138">Následuje několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-138">A few examples follow:</span></span>

* <span data-ttu-id="9c2c6-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) slouží k určení dalších `IConfiguration` pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="9c2c6-140">Následující `ConfigureAppConfiguration` volání přidá delegáta a nezahrnují konfiguraci aplikace v *appsettings.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="9c2c6-141">`ConfigureAppConfiguration` může být volána více než jednou.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="9c2c6-142">Všimněte si, že tato konfigurace se nevztahuje na hostitele (například adresy URL serveru nebo prostředí).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="9c2c6-143">Najdete v článku [hostovat konfigurační hodnoty](#host-configuration-values) oddílu.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="9c2c6-144">Následující `ConfigureLogging` volání přidá delegáta konfigurace minimální protokolování úrovně ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) k [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="9c2c6-145">Toto nastavení přepíše nastavení v *appsettings. Development.JSON* (`LogLevel.Debug`) a *appsettings. Production.JSON* (`LogLevel.Error`) nakonfiguroval `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="9c2c6-146">`ConfigureLogging` může být volána více než jednou.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="9c2c6-147">Následující volání `ConfigureKestrel` přepíše výchozí [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 bajtů navázat, když Kestrel nakonfigurovaly `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-147">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

* <span data-ttu-id="9c2c6-148">Následující volání [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) přepíše výchozí [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 bajtů navázat, když Kestrel nakonfigurovaly `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9c2c6-149">*Obsahu kořenové* Určuje, kde hostitele vyhledává soubory obsahu, jako jsou například soubory zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="9c2c6-150">Při spuštění aplikace z kořenové složky projektu, kořenové složky projektu slouží jako kořenový adresář obsahu.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="9c2c6-151">Toto je výchozí hodnotu použitou v [sady Visual Studio](https://www.visualstudio.com/) a [nové šablony pro dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="9c2c6-152">Další informace o konfiguraci aplikací najdete v tématu <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-152">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="9c2c6-153">Jako alternativu k použití statické `CreateDefaultBuilder` metoda vytvoření hostitele z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) je podporované přístup pomocí ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="9c2c6-154">Další informace najdete v tématu na kartě ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9c2c6-155">Vytvoření hostitele pomocí instance [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-155">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="9c2c6-156">Vytvoření hostitele se obvykle provádí v vstupní bod aplikace, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-156">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="9c2c6-157">V šablonách projektů `Main` se nachází v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-157">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="9c2c6-158">`WebHostBuilder` vyžaduje [serveru, který implementuje IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-158">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="9c2c6-159">Integrované servery jsou [Kestrel](xref:fundamentals/servers/kestrel) a [HTTP.sys](xref:fundamentals/servers/httpsys) (před verzí technologie ASP.NET Core 2.0, byla volána HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-159">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="9c2c6-160">V tomto příkladu [UseKestrel rozšiřující metoda](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) určuje Kestrel server.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-160">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="9c2c6-161">*Obsahu kořenové* Určuje, kde hostitele vyhledává soubory obsahu, jako jsou například soubory zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-161">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="9c2c6-162">Výchozí kořen obsahu je získána pro `UseContentRoot` podle [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-162">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="9c2c6-163">Při spuštění aplikace z kořenové složky projektu, kořenové složky projektu slouží jako kořenový adresář obsahu.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-163">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="9c2c6-164">Toto je výchozí hodnotu použitou v [sady Visual Studio](https://www.visualstudio.com/) a [nové šablony pro dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-164">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="9c2c6-165">Chcete-li použít jako reverzní proxy server služby IIS, zavolejte [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) jako součást vytváření hostitele.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-165">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="9c2c6-166">`UseIISIntegration` neprovede konfiguraci *server*, třeba [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-166">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="9c2c6-167">`UseIISIntegration` Nastaví základní cesta a portu server naslouchá na při použití [modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) vytvořit reverzního proxy serveru mezi Kestrel a služby IIS.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-167">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="9c2c6-168">Použití služby IIS s ASP.NET Core, `UseKestrel` a `UseIISIntegration` musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-168">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="9c2c6-169">`UseIISIntegration` aktivuje pouze při spuštění za služby IIS nebo IIS Express.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-169">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="9c2c6-170">Další informace naleznete v tématu <xref:fundamentals/servers/aspnet-core-module> a <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-170">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="9c2c6-171">Minimální implementaci, který konfiguruje hostitele (a aplikace ASP.NET Core) součástí serveru a na konfiguraci aplikace požadavku kanálu:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-171">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

::: moniker-end

<span data-ttu-id="9c2c6-172">Při nastavování hostitele, [konfigurovat](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metody lze zadat.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-172">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="9c2c6-173">Pokud `Startup` Zadaná třída, musíte definovat `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-173">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="9c2c6-174">Další informace naleznete v tématu <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-174">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="9c2c6-175">Při vícenásobném volání metody `ConfigureServices` se přidají služby ze všech volání.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-175">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="9c2c6-176">Více volání `Configure` nebo `UseStartup` na `WebHostBuilder` nahradit předchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-176">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="9c2c6-177">Hodnoty konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="9c2c6-177">Host configuration values</span></span>

<span data-ttu-id="9c2c6-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) závisí na následujících dvou přístupů k nastavení hodnoty konfigurace hostitele:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="9c2c6-179">Tvůrce konfiguraci hostitele, která zahrnuje proměnné prostředí s formátem `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-179">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="9c2c6-180">Například `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-180">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="9c2c6-181">Rozšíření, jako [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) a [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (viz [přepsání konfigurace](#override-configuration) části).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-181">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="9c2c6-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) a přidružený klíč.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="9c2c6-183">Při nastavování hodnoty s `UseSetting`, je hodnota nastavena jako řetězec bez ohledu na typ.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-183">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="9c2c6-184">Hostitel používá kterékoli z těchto možností nastaví hodnotu poslední.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-184">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="9c2c6-185">Další informace najdete v tématu [konfigurace přepisování](#override-configuration) v další části.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-185">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="9c2c6-186">Klíč aplikace (název)</span><span class="sxs-lookup"><span data-stu-id="9c2c6-186">Application Key (Name)</span></span>

<span data-ttu-id="9c2c6-187">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) vlastnost je nastavena automaticky při [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) nebo [konfigurovat](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) je volána v průběhu vytváření hostitele.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-187">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="9c2c6-188">Je hodnota nastavena na název sestavení obsahující vstupní bod aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-188">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="9c2c6-189">Pokud chcete explicitně nastavit hodnotu, použijte [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="9c2c6-189">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="9c2c6-190">**Klíč**: applicationName</span><span class="sxs-lookup"><span data-stu-id="9c2c6-190">**Key**: applicationName</span></span>  
<span data-ttu-id="9c2c6-191">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="9c2c6-191">**Type**: *string*</span></span>  
<span data-ttu-id="9c2c6-192">**Výchozí**: název sestavení obsahující vstupní bod aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-192">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="9c2c6-193">**Sada s použitím**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-193">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="9c2c6-194">**Proměnná prostředí**: `ASPNETCORE_APPLICATIONKEY`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-194">**Environment variable**: `ASPNETCORE_APPLICATIONKEY`</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a><span data-ttu-id="9c2c6-195">Zachycení chyb při spuštění</span><span class="sxs-lookup"><span data-stu-id="9c2c6-195">Capture Startup Errors</span></span>

<span data-ttu-id="9c2c6-196">Toto nastavení řídí zaznamenávání chyb při spuštění.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-196">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="9c2c6-197">**Klíč**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="9c2c6-197">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="9c2c6-198">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="9c2c6-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9c2c6-199">**Výchozí**: výchozí hodnota je `false` Pokud aplikace běží s Kestrel za služby IIS, kde je jako výchozí `true`.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-199">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="9c2c6-200">**Sada s použitím**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-200">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="9c2c6-201">**Proměnná prostředí**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-201">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="9c2c6-202">Když `false`, chyby během spuštění výsledku na hostiteli. operace bude ukončena.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-202">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="9c2c6-203">Když `true`, zachytí výjimky během spouštění hostitele a se pokusí spustit na serveru.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-203">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a><span data-ttu-id="9c2c6-204">Obsah kořenové</span><span class="sxs-lookup"><span data-stu-id="9c2c6-204">Content Root</span></span>

<span data-ttu-id="9c2c6-205">Toto nastavení určuje, kde ASP.NET Core začne vyhledávat soubory obsahu, například zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-205">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="9c2c6-206">**Klíč**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="9c2c6-206">**Key**: contentRoot</span></span>  
<span data-ttu-id="9c2c6-207">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="9c2c6-207">**Type**: *string*</span></span>  
<span data-ttu-id="9c2c6-208">**Výchozí**: výchozí hodnota je složka, ve které se nachází sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-208">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="9c2c6-209">**Sada s použitím**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-209">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="9c2c6-210">**Proměnná prostředí**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-210">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="9c2c6-211">Obsah kořenové slouží také jako základní cesta pro [kořenový adresář webové nastavení](#web-root).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-211">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="9c2c6-212">Pokud cesta neexistuje, hostitel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-212">If the path doesn't exist, the host fails to start.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a><span data-ttu-id="9c2c6-213">Podrobné chyby</span><span class="sxs-lookup"><span data-stu-id="9c2c6-213">Detailed Errors</span></span>

<span data-ttu-id="9c2c6-214">Určuje, zda podrobné chyby mají být zaznamenány.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-214">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="9c2c6-215">**Klíč**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="9c2c6-215">**Key**: detailedErrors</span></span>  
<span data-ttu-id="9c2c6-216">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="9c2c6-216">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9c2c6-217">**Výchozí**: false</span><span class="sxs-lookup"><span data-stu-id="9c2c6-217">**Default**: false</span></span>  
<span data-ttu-id="9c2c6-218">**Sada s použitím**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-218">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="9c2c6-219">**Proměnná prostředí**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-219">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="9c2c6-220">Při povolené (nebo když <a href="#environment">prostředí</a> je nastavena na `Development`), aplikace zaznamenává podrobné výjimky.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-220">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a><span data-ttu-id="9c2c6-221">Prostředí</span><span class="sxs-lookup"><span data-stu-id="9c2c6-221">Environment</span></span>

<span data-ttu-id="9c2c6-222">Nastaví prostředí aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-222">Sets the app's environment.</span></span>

<span data-ttu-id="9c2c6-223">**Klíč**: prostředí</span><span class="sxs-lookup"><span data-stu-id="9c2c6-223">**Key**: environment</span></span>  
<span data-ttu-id="9c2c6-224">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="9c2c6-224">**Type**: *string*</span></span>  
<span data-ttu-id="9c2c6-225">**Výchozí**: produkčního prostředí</span><span class="sxs-lookup"><span data-stu-id="9c2c6-225">**Default**: Production</span></span>  
<span data-ttu-id="9c2c6-226">**Sada s použitím**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-226">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="9c2c6-227">**Proměnná prostředí**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-227">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="9c2c6-228">Prostředí můžete nastavit na libovolnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-228">The environment can be set to any value.</span></span> <span data-ttu-id="9c2c6-229">Hodnoty definované v rámci rozhraní zahrnují `Development`, `Staging`, a `Production`.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-229">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="9c2c6-230">Hodnoty se velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-230">Values aren't case sensitive.</span></span> <span data-ttu-id="9c2c6-231">Ve výchozím nastavení *prostředí* číst z `ASPNETCORE_ENVIRONMENT` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-231">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="9c2c6-232">Při použití [sady Visual Studio](https://www.visualstudio.com/), lze nastavit proměnné prostředí *launchSettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-232">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="9c2c6-233">Další informace naleznete v tématu <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-233">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="9c2c6-234">Hostování při spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="9c2c6-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="9c2c6-235">Nastaví hostování sestavení po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="9c2c6-236">**Klíč**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="9c2c6-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="9c2c6-237">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="9c2c6-237">**Type**: *string*</span></span>  
<span data-ttu-id="9c2c6-238">**Výchozí**: prázdný řetězec</span><span class="sxs-lookup"><span data-stu-id="9c2c6-238">**Default**: Empty string</span></span>  
<span data-ttu-id="9c2c6-239">**Sada s použitím**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="9c2c6-240">**Proměnná prostředí**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="9c2c6-241">Středníkem oddělený řetězec hostování načíst při spuštění po spuštění sestavení.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="9c2c6-242">I když výchozí hodnota konfigurace je prázdný řetězec, hostování při spuštění sestavení vždy zahrnovat sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-242">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="9c2c6-243">Při hostování při spuštění sestavení jsou k dispozici, se přidají do sestavení aplikace pro načtení, když aplikace je sestavena své běžné služby během spouštění.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-243">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a><span data-ttu-id="9c2c6-244">HTTPS Port</span><span class="sxs-lookup"><span data-stu-id="9c2c6-244">HTTPS Port</span></span>

<span data-ttu-id="9c2c6-245">Nastavení přesměrování portu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-245">Set the HTTPS redirect port.</span></span> <span data-ttu-id="9c2c6-246">Použít v [vynucování HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-246">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="9c2c6-247">**Klíč**: https_port **typ**: *řetězec*
**výchozí**: výchozí hodnota není nastavená.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-247">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="9c2c6-248">**Sada s použitím**: `UseSetting` 
 **proměnnou prostředí**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-248">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="9c2c6-249">Hostování sestavení vyloučit při spuštění</span><span class="sxs-lookup"><span data-stu-id="9c2c6-249">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="9c2c6-250">POPIS</span><span class="sxs-lookup"><span data-stu-id="9c2c6-250">DESCRIPTION</span></span>

<span data-ttu-id="9c2c6-251">**Klíč**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="9c2c6-251">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="9c2c6-252">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="9c2c6-252">**Type**: *string*</span></span>  
<span data-ttu-id="9c2c6-253">**Výchozí**: prázdný řetězec</span><span class="sxs-lookup"><span data-stu-id="9c2c6-253">**Default**: Empty string</span></span>  
<span data-ttu-id="9c2c6-254">**Sada s použitím**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-254">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="9c2c6-255">**Proměnná prostředí**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-255">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="9c2c6-256">Řetězec oddělený středníkem při spuštění sestavení mají vyloučit při spuštění hostování.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-256">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="9c2c6-257">Dáváte přednost, který je hostitelem adresy URL</span><span class="sxs-lookup"><span data-stu-id="9c2c6-257">Prefer Hosting URLs</span></span>

<span data-ttu-id="9c2c6-258">Určuje, zda hostitel naslouchat požadavkům na adresy URL nakonfigurované `WebHostBuilder` místo nastavení nakonfigurovaného pomocí `IServer` implementace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-258">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="9c2c6-259">**Klíč**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="9c2c6-259">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="9c2c6-260">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="9c2c6-260">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9c2c6-261">**Výchozí**: true</span><span class="sxs-lookup"><span data-stu-id="9c2c6-261">**Default**: true</span></span>  
<span data-ttu-id="9c2c6-262">**Sada s použitím**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-262">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="9c2c6-263">**Proměnná prostředí**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-263">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="9c2c6-264">Zabránit spuštění hostování</span><span class="sxs-lookup"><span data-stu-id="9c2c6-264">Prevent Hosting Startup</span></span>

<span data-ttu-id="9c2c6-265">Brání automatické načítání hostování při spuštění sestavení, včetně hostování při spuštění sestavení nakonfiguroval sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-265">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="9c2c6-266">Další informace naleznete v tématu <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-266">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="9c2c6-267">**Klíč**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="9c2c6-267">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="9c2c6-268">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="9c2c6-268">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9c2c6-269">**Výchozí**: false</span><span class="sxs-lookup"><span data-stu-id="9c2c6-269">**Default**: false</span></span>  
<span data-ttu-id="9c2c6-270">**Sada s použitím**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-270">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="9c2c6-271">**Proměnná prostředí**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-271">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="9c2c6-272">Serverové adresy URL</span><span class="sxs-lookup"><span data-stu-id="9c2c6-272">Server URLs</span></span>

<span data-ttu-id="9c2c6-273">Určuje IP adresy nebo adresy hostitele s porty a protokoly, které server naslouchat požadavkům na požadavky.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-273">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="9c2c6-274">**Klíč**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="9c2c6-274">**Key**: urls</span></span>  
<span data-ttu-id="9c2c6-275">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="9c2c6-275">**Type**: *string*</span></span>  
<span data-ttu-id="9c2c6-276">**Výchozí**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="9c2c6-276">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="9c2c6-277">**Sada s použitím**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-277">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="9c2c6-278">**Proměnná prostředí**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-278">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="9c2c6-279">Nastavte na oddělený středníkem (;) na server by měl odpovídat předpony adres seznam adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-279">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="9c2c6-280">Například `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-280">For example, `http://localhost:123`.</span></span> <span data-ttu-id="9c2c6-281">Použití "\*" k označení, že by měl server naslouchat požadavkům na IP adresu nebo název hostitele zadaný port a protokol (například `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-281">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="9c2c6-282">Protokol (`http://` nebo `https://`) musí být součástí jednotlivé adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-282">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="9c2c6-283">Podporované formáty lišit mezi servery.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-283">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="9c2c6-284">Kestrel má svůj vlastní konfigurace koncového bodu rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-284">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="9c2c6-285">Další informace naleznete v tématu <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-285">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="9c2c6-286">Časový limit vypnutí</span><span class="sxs-lookup"><span data-stu-id="9c2c6-286">Shutdown Timeout</span></span>

<span data-ttu-id="9c2c6-287">Určuje dobu čekání webového hostitele pro vypnutí.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-287">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="9c2c6-288">**Klíč**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="9c2c6-288">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="9c2c6-289">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="9c2c6-289">**Type**: *int*</span></span>  
<span data-ttu-id="9c2c6-290">**Výchozí**: 5</span><span class="sxs-lookup"><span data-stu-id="9c2c6-290">**Default**: 5</span></span>  
<span data-ttu-id="9c2c6-291">**Sada s použitím**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-291">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="9c2c6-292">**Proměnná prostředí**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-292">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="9c2c6-293">I když se přijme klíč *int* s `UseSetting` (například `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) rozšiřující metoda přijímá [časový interval](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-293">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="9c2c6-294">Během časového limitu, který je hostitelem:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-294">During the timeout period, hosting:</span></span>

* <span data-ttu-id="9c2c6-295">Aktivační události [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-295">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="9c2c6-296">Pokusy o zastavení hostovaných služeb, protokolování všech chyb pro služby, které se nepodařilo zastavit.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-296">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="9c2c6-297">Pokud časový limit vyprší před všechny zarážky hostovaných služeb, zastaví se všechny zbývající služby active při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-297">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="9c2c6-298">Zastavení služeb i v případě, že se nedokončilo zpracování.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-298">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="9c2c6-299">Pokud služby vyžadují další čas ukončení, zvýšit časový limit.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-299">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="9c2c6-300">Po spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="9c2c6-300">Startup Assembly</span></span>

<span data-ttu-id="9c2c6-301">Určuje sestavení, které chcete vyhledat `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-301">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="9c2c6-302">**Klíč**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="9c2c6-302">**Key**: startupAssembly</span></span>  
<span data-ttu-id="9c2c6-303">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="9c2c6-303">**Type**: *string*</span></span>  
<span data-ttu-id="9c2c6-304">**Výchozí**: sestavení aplikace</span><span class="sxs-lookup"><span data-stu-id="9c2c6-304">**Default**: The app's assembly</span></span>  
<span data-ttu-id="9c2c6-305">**Sada s použitím**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-305">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="9c2c6-306">**Proměnná prostředí**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-306">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="9c2c6-307">Sestavení podle názvu (`string`) nebo typ (`TStartup`) může být odkazováno.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-307">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="9c2c6-308">Pokud je položek víc `UseStartup` metody jsou volány, má přednost před poslední z nich.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-308">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a><span data-ttu-id="9c2c6-309">Kořenový adresář webové</span><span class="sxs-lookup"><span data-stu-id="9c2c6-309">Web Root</span></span>

<span data-ttu-id="9c2c6-310">Nastaví relativní cestu k statických prostředků aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-310">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="9c2c6-311">**Klíč**: webroot</span><span class="sxs-lookup"><span data-stu-id="9c2c6-311">**Key**: webroot</span></span>  
<span data-ttu-id="9c2c6-312">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="9c2c6-312">**Type**: *string*</span></span>  
<span data-ttu-id="9c2c6-313">**Výchozí**: Pokud se nezadá, výchozí hodnota je "(Content Root)/wwwroot", pokud cesta existuje.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-313">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="9c2c6-314">Pokud cesta neexistuje, no-op soubor zprostředkovatele slouží.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-314">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="9c2c6-315">**Sada s použitím**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-315">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="9c2c6-316">**Proměnná prostředí**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-316">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a><span data-ttu-id="9c2c6-317">Přepsání konfigurace</span><span class="sxs-lookup"><span data-stu-id="9c2c6-317">Override configuration</span></span>

<span data-ttu-id="9c2c6-318">Použití [konfigurace](xref:fundamentals/configuration/index) ke konfiguraci webového hostitele.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-318">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="9c2c6-319">V následujícím příkladu je konfigurace hostitele volitelně podle *hostsettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-319">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="9c2c6-320">Všechny konfigurace načtena z *hostsettings.json* soubor lze přepsat pomocí argumentů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-320">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="9c2c6-321">Konfigurace sestavení (v `config`) se používá ke konfiguraci hostitele s [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-321">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="9c2c6-322">`IWebHostBuilder` konfigurace je přidaný na konfiguraci aplikace, ale neplatí první&mdash; `ConfigureAppConfiguration` nemá vliv `IWebHostBuilder` konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-322">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9c2c6-323">Přepsání konfigurace poskytované `UseUrls` s *hostsettings.json* config argument příkazového řádku, první config druhý:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-323">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="9c2c6-324">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-324">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9c2c6-325">Přepsání konfigurace poskytované `UseUrls` s *hostsettings.json* config argument příkazového řádku, první config druhý:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-325">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="9c2c6-326">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-326">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="9c2c6-327">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) rozšiřující metoda není aktuálně podporuje při analýze oddílu konfigurace vrácený `GetSection` (například `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-327">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="9c2c6-328">`GetSection` Metoda filtry konfigurační klíče do části Požadovaná, ale ponechá název oddílu, který na klíče (například `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-328">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="9c2c6-329">`UseConfiguration` Metoda očekává klíčů tak, aby odpovídaly `WebHostBuilder` klíče (například `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-329">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="9c2c6-330">Přítomnost název oddílu, který klíčů brání hodnoty v části Konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-330">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="9c2c6-331">Tento problém bude vyřešen v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-331">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="9c2c6-332">Další informace a řešení najdete v tématu [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration využívá úplnou klíčů](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-332">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="9c2c6-333">`UseConfiguration` zkopíruje jen klíče ze zadaného `IConfiguration` Tvůrce konfiguraci hostitele.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-333">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="9c2c6-334">Proto nastavení `reloadOnChange: true` soubory XML, JSON a INI nastavení nemá žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-334">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="9c2c6-335">K určení hostitele spouštět na určitou adresu URL, na požadovanou hodnotu lze předat v příkazovém řádku při provádění [dotnet spustit](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-335">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="9c2c6-336">Argument příkazového řádku přepíše `urls` hodnotu *hostsettings.json* souboru a server naslouchá na portu 8080:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-336">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="9c2c6-337">Spravovat hostitele</span><span class="sxs-lookup"><span data-stu-id="9c2c6-337">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9c2c6-338">**Spuštění**</span><span class="sxs-lookup"><span data-stu-id="9c2c6-338">**Run**</span></span>

<span data-ttu-id="9c2c6-339">`Run` Metoda spustí webovou aplikaci a blokuje volající vlákno, dokud nebude ukončen hostitele:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-339">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="9c2c6-340">**Start**</span><span class="sxs-lookup"><span data-stu-id="9c2c6-340">**Start**</span></span>

<span data-ttu-id="9c2c6-341">Spustí hostitele způsobem neblokující zavoláním jeho `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-341">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="9c2c6-342">Pokud je předán seznam adres URL `Start` metody naslouchá na zadaných adresách URL:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-342">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="9c2c6-343">Aplikaci můžete inicializaci a spuštění nového hostitele pomocí předem nakonfigurovaných výchozích nastavení `CreateDefaultBuilder` pomocí pohodlí statické metody.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-343">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="9c2c6-344">Tyto metody spuštění serveru bez výstup na konzole a s [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) počkejte přerušení (Ctrl-C/SIGINT nebo SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="9c2c6-344">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="9c2c6-345">**Spuštění (RequestDelegate aplikace)**</span><span class="sxs-lookup"><span data-stu-id="9c2c6-345">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="9c2c6-346">Začněte `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-346">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="9c2c6-347">Vytvořit žádost o v prohlížeči `http://localhost:5000` trvá příjem odpovědi "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="9c2c6-347">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="9c2c6-348">`WaitForShutdown` blokuje, dokud se vydává přerušení (Ctrl-C/SIGINT nebo SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="9c2c6-349">Aplikace zobrazí `Console.WriteLine` zprávu a čeká na stisknutí klávesy ukončete.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="9c2c6-350">**Spustit (řetězec adresu url, RequestDelegate aplikace)**</span><span class="sxs-lookup"><span data-stu-id="9c2c6-350">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="9c2c6-351">Začněte s adresou URL a `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-351">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="9c2c6-352">Vytvoří stejný výsledek jako **Start (RequestDelegate aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-352">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="9c2c6-353">**Spustit (akce&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="9c2c6-353">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="9c2c6-354">Použijte instanci `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) používat middleware směrování:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-354">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="9c2c6-355">Použijte následující požadavky na prohlížeč s příkladem:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-355">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="9c2c6-356">Požadavek</span><span class="sxs-lookup"><span data-stu-id="9c2c6-356">Request</span></span>                                    | <span data-ttu-id="9c2c6-357">Odpověď</span><span class="sxs-lookup"><span data-stu-id="9c2c6-357">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="9c2c6-358">Dobrý den, Martin!</span><span class="sxs-lookup"><span data-stu-id="9c2c6-358">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="9c2c6-359">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="9c2c6-359">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="9c2c6-360">Vyvolá výjimku s řetězcem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="9c2c6-360">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="9c2c6-361">Vyvolá výjimku s řetězcem "Uh ale!"</span><span class="sxs-lookup"><span data-stu-id="9c2c6-361">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="9c2c6-362">Santé, Kevin!</span><span class="sxs-lookup"><span data-stu-id="9c2c6-362">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="9c2c6-363">Ahoj světe!</span><span class="sxs-lookup"><span data-stu-id="9c2c6-363">Hello World!</span></span>                             |

<span data-ttu-id="9c2c6-364">`WaitForShutdown` blokuje, dokud se vydává přerušení (Ctrl-C/SIGINT nebo SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-364">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="9c2c6-365">Aplikace zobrazí `Console.WriteLine` zprávu a čeká na stisknutí klávesy ukončete.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-365">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="9c2c6-366">**Spustit (řetězec adresy url, akce&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="9c2c6-366">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="9c2c6-367">Použijte adresu URL a instance `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-367">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="9c2c6-368">Vytvoří stejný výsledek jako **spuštění (akce&lt;IRouteBuilder&gt; routeBuilder)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-368">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="9c2c6-369">**StartWith (akce&lt;IApplicationBuilder&gt; aplikace)**</span><span class="sxs-lookup"><span data-stu-id="9c2c6-369">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="9c2c6-370">Zadejte delegát pro konfiguraci `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-370">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="9c2c6-371">Vytvořit žádost o v prohlížeči `http://localhost:5000` trvá příjem odpovědi "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="9c2c6-371">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="9c2c6-372">`WaitForShutdown` blokuje, dokud se vydává přerušení (Ctrl-C/SIGINT nebo SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-372">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="9c2c6-373">Aplikace zobrazí `Console.WriteLine` zprávu a čeká na stisknutí klávesy ukončete.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-373">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="9c2c6-374">**StartWith (řetězec adresy url, akce&lt;IApplicationBuilder&gt; aplikace)**</span><span class="sxs-lookup"><span data-stu-id="9c2c6-374">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="9c2c6-375">Zadejte adresu URL a delegát pro konfiguraci `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-375">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="9c2c6-376">Vytvoří stejný výsledek jako **StartWith (akce&lt;IApplicationBuilder&gt; aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-376">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9c2c6-377">**Spuštění**</span><span class="sxs-lookup"><span data-stu-id="9c2c6-377">**Run**</span></span>

<span data-ttu-id="9c2c6-378">`Run` Metoda spustí webovou aplikaci a blokuje volající vlákno, dokud nebude ukončen hostitele:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-378">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="9c2c6-379">**Start**</span><span class="sxs-lookup"><span data-stu-id="9c2c6-379">**Start**</span></span>

<span data-ttu-id="9c2c6-380">Spustí hostitele způsobem neblokující zavoláním jeho `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-380">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="9c2c6-381">Pokud je předán seznam adres URL `Start` metody naslouchá na zadaných adresách URL:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-381">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

::: moniker-end

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="9c2c6-382">IHostingEnvironment rozhraní</span><span class="sxs-lookup"><span data-stu-id="9c2c6-382">IHostingEnvironment interface</span></span>

<span data-ttu-id="9c2c6-383">[IHostingEnvironment rozhraní](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) poskytuje informace o hostování prostředí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-383">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="9c2c6-384">Použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-384">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="9c2c6-385">A [přístup podle úmluvy](xref:fundamentals/environments#environment-based-startup-class-and-methods) slouží ke konfiguraci aplikace při spuštění na základě prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-385">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="9c2c6-386">Můžete také vložit `IHostingEnvironment` do `Startup` konstruktoru pro použití v `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-386">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="9c2c6-387">Kromě `IsDevelopment` rozšiřující metoda `IHostingEnvironment` nabízí `IsStaging`, `IsProduction`, a `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-387">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="9c2c6-388">Další informace naleznete v tématu <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-388">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="9c2c6-389">`IHostingEnvironment` Service můžete také vloženy přímo do `Configure` metodu pro vytvoření kanálu zpracování:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-389">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="9c2c6-390">`IHostingEnvironment` mohou být vloženy do `Invoke` metoda při vytváření vlastní [middleware](xref:fundamentals/middleware/index#write-middleware):</span><span class="sxs-lookup"><span data-stu-id="9c2c6-390">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="9c2c6-391">IApplicationLifetime rozhraní</span><span class="sxs-lookup"><span data-stu-id="9c2c6-391">IApplicationLifetime interface</span></span>

<span data-ttu-id="9c2c6-392">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umožňuje po spuštění a vypnutí aktivity.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-392">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="9c2c6-393">Tři vlastnosti v rozhraní jsou tokeny zrušení použije k registraci `Action` metody, které definují události spuštění a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-393">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="9c2c6-394">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="9c2c6-394">Cancellation Token</span></span>    | <span data-ttu-id="9c2c6-395">Při aktivaci&#8230;</span><span class="sxs-lookup"><span data-stu-id="9c2c6-395">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="9c2c6-396">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="9c2c6-396">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="9c2c6-397">Hostitel plně byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-397">The host has fully started.</span></span> |
| [<span data-ttu-id="9c2c6-398">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="9c2c6-398">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="9c2c6-399">Hostitel se dokončuje řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-399">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="9c2c6-400">By měl zpracovat všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-400">All requests should be processed.</span></span> <span data-ttu-id="9c2c6-401">Vypnutí blokuje, dokud se tato událost se dokončí.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-401">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="9c2c6-402">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="9c2c6-402">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="9c2c6-403">Hostitel provádí řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-403">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="9c2c6-404">Žádosti se možná ještě zpracovávají.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-404">Requests may still be processing.</span></span> <span data-ttu-id="9c2c6-405">Vypnutí blokuje, dokud se tato událost se dokončí.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-405">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="9c2c6-406">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) požaduje ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-406">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="9c2c6-407">Následující třídy používá `StopApplication` řádně vypnout aplikaci při třídy `Shutdown` volání metody:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-407">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="9c2c6-408">Rozsah ověřování</span><span class="sxs-lookup"><span data-stu-id="9c2c6-408">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9c2c6-409">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) nastaví [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) k `true` jestli vývojové prostředí aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-409">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="9c2c6-410">Když `ValidateScopes` je nastavena na `true`, poskytovatele služeb výchozí provádí kontroly pro ověření, že:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-410">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="9c2c6-411">Vymezené služby nejsou přímo nebo nepřímo vyřešit z kořenové poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-411">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="9c2c6-412">Vymezené služby nejsou přímo nebo nepřímo vloženy do jednotlivých prvků.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-412">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="9c2c6-413">Poskytovatel služeb root je vytvořen, když [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) je volána.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-413">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="9c2c6-414">Doba života poskytovatele služeb kořenový odpovídá životnost aplikace/serveru při zprostředkovatel začíná aplikace a je uvolněna při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-414">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="9c2c6-415">Vymezené služby jsou uvolněna pomocí kontejneru, který je vytvořil.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-415">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="9c2c6-416">Pokud vymezené služby se vytvoří v kořenovém kontejneru, životnost služby je efektivně povýšen na jednotlivý prvek, protože to je jenom uvolněn pomocí Kořenový kontejner při vypnutí serveru/aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-416">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="9c2c6-417">Ověřování služby Obory zachytí tyto situace při `BuildServiceProvider` je volána.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-417">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="9c2c6-418">Vždy ověřovaly obory, včetně produkčního prostředí, nakonfigurujte [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) s [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) na tvůrce hostitele:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-418">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="9c2c6-419">Řešení potíží s System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="9c2c6-419">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="9c2c6-420">**Toto platí jenom pro aplikace ASP.NET Core 2.0 při aplikaci nevolá `UseStartup` nebo `Configure`.**</span><span class="sxs-lookup"><span data-stu-id="9c2c6-420">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="9c2c6-421">Hostitel může být sestavena v vkládá `IStartup` přímo do kontejneru pro vkládání závislostí místo volání `UseStartup` nebo `Configure`:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-421">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="9c2c6-422">Pokud hostitel je vytvořena tímto způsobem, může dojít k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-422">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="9c2c6-423">K tomu dochází, název aplikace (název aktuální sestavení) je nutná ke kontrole pro `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-423">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="9c2c6-424">Pokud aplikaci ručně vkládá `IStartup` do kontejneru pro vkládání závislostí, přidejte následující volání `WebHostBuilder` pomocí zadaného názvu sestavení:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-424">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="9c2c6-425">Můžete také přidat fiktivní `Configure` k `WebHostBuilder`, který automaticky nastaví název aplikace:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-425">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="9c2c6-426">Další informace najdete v tématu [oznámení: Microsoft.Extensions.PlatformAbstractions byla odebrána (komentář)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) a [StartupInjection ukázka](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="9c2c6-426">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9c2c6-427">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9c2c6-427">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
