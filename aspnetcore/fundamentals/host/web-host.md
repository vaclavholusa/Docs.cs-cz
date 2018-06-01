---
title: ASP.NET Core webového hostitele
author: guardrex
description: Další informace o webového hostitele v ASP.NET Core, která je zodpovědná za spuštění a životního cyklu správy aplikací.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/web-host
ms.openlocfilehash: ce95599ec8e940635ca63c3bf9a3c28784a3f371
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687487"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="1b6fe-103">ASP.NET Core webového hostitele</span><span class="sxs-lookup"><span data-stu-id="1b6fe-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="1b6fe-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1b6fe-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1b6fe-105">Aplikace ASP.NET Core nakonfigurovat a spustit *hostitele*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="1b6fe-106">Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1b6fe-107">Minimálně hostitele nakonfiguruje server a kanálu zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="1b6fe-108">Toto téma popisuje ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), což je užitečné pro hostování webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="1b6fe-109">Pro pokrytí obecné hostitele .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), najdete v článku [obecné hostitele](xref:fundamentals/host/generic-host) tématu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="1b6fe-110">Nastavení hostitele</span><span class="sxs-lookup"><span data-stu-id="1b6fe-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-111">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="1b6fe-112">Vytvořit pomocí instance hostitele [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="1b6fe-113">To se obvykle provádí v vstupní bod aplikace, `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="1b6fe-114">V rámci šablon projektu `Main` se nachází v *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="1b6fe-115">Typické *Program.cs* volání [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zahájíte nastavení hostitele:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="1b6fe-116">`CreateDefaultBuilder` provádí následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="1b6fe-117">Nakonfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako webový server a nakonfiguruje server pomocí poskytovatelů hostingu konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="1b6fe-118">Výchozí možnosti Kestrel najdete v tématu [Kestrel možnosti části Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="1b6fe-119">Nastaví obsahu kořenovou cestu vrácený [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="1b6fe-120">Načítání volitelné [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) z:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-120">Loads optional [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) from:</span></span>
  * <span data-ttu-id="1b6fe-121">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-121">*appsettings.json*.</span></span>
  * <span data-ttu-id="1b6fe-122">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-122">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="1b6fe-123">[Tajné klíče uživatele](xref:security/app-secrets) při spuštění aplikace `Development` prostředí pomocí položky sestavení.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-123">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="1b6fe-124">Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-124">Environment variables.</span></span>
  * <span data-ttu-id="1b6fe-125">Argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-125">Command-line arguments.</span></span>
* <span data-ttu-id="1b6fe-126">Nakonfiguruje [protokolování](xref:fundamentals/logging/index) konzoly a ladění výstupu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-126">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="1b6fe-127">Zahrnuje protokolování [filtrování protokolu](xref:fundamentals/logging/index#log-filtering) pravidla stanovená v části Konfigurace protokolování *appSettings.JSON určený* nebo *appsettings. { Prostředí} .json* souboru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-127">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="1b6fe-128">Když spustíte za služby IIS, umožňuje [integrační služby IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-128">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="1b6fe-129">Konfiguruje základní cesta a portu server naslouchá na při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-129">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="1b6fe-130">Modul vytvoří reverzní proxy server mezi službou IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-130">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="1b6fe-131">Nakonfiguruje aplikaci taky [zaznamenat chyby při spuštění](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-131">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="1b6fe-132">Výchozí možnosti služby IIS najdete v tématu [IIS možnosti oddílu hostitele ASP.NET Core v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-132">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="1b6fe-133">Nastaví [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) k `true` Pokud prostředí aplikace je vývoj.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-133">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="1b6fe-134">Další informace najdete v tématu [obor ověření](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-134">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="1b6fe-135">*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-135">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="1b6fe-136">Když se aplikace spustí z kořenové složky projektu, projektu kořenové složky se používá jako kořenu obsahu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-136">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="1b6fe-137">Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-137">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="1b6fe-138">Další informace o konfiguraci aplikace, najdete v části [konfigurace v ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-138">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="1b6fe-139">Jako alternativu k použití statických `CreateDefaultBuilder` metody vytvoření hostitele z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) je podporované přístup pomocí ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-139">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="1b6fe-140">Další informace najdete v tématu kartě 1.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-140">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="1b6fe-142">Vytvořit pomocí instance hostitele [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-142">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="1b6fe-143">Vytvoření hostitele se obvykle provádí v vstupní bod aplikace, `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-143">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="1b6fe-144">V rámci šablon projektu `Main` se nachází v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-144">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="1b6fe-145">`WebHostBuilder` vyžaduje [serveru, který implementuje IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-145">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="1b6fe-146">Jsou předdefinované servery [Kestrel](xref:fundamentals/servers/kestrel) a [HTTP.sys](xref:fundamentals/servers/httpsys) (před verzí technologie ASP.NET 2.0 jádra, ovladač HTTP.sys volala [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-146">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="1b6fe-147">V tomto příkladu [UseKestrel rozšíření metoda](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) určuje Kestrel server.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-147">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="1b6fe-148">*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="1b6fe-149">Výchozí kořen obsahu se získávají pro `UseContentRoot` podle [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-149">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="1b6fe-150">Když se aplikace spustí z kořenové složky projektu, projektu kořenové složky se používá jako kořenu obsahu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="1b6fe-151">Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="1b6fe-152">Chcete-li použít jako reverzní proxy server služby IIS, volejte [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) jako součást sestavení hostitele.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-152">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="1b6fe-153">`UseIISIntegration` neprovede konfiguraci *server*, například [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-153">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="1b6fe-154">`UseIISIntegration` Konfiguruje základní cesta a portu server naslouchá na při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) vytvořit reverzní proxy server mezi Kestrel a služby IIS.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-154">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="1b6fe-155">Použití služby IIS s ASP.NET Core `UseKestrel` a `UseIISIntegration` musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-155">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="1b6fe-156">`UseIISIntegration` aktivuje pouze při spuštění za služby IIS nebo IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-156">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="1b6fe-157">Další informace najdete v tématu [Úvod k modulu jádra ASP.NET](xref:fundamentals/servers/aspnet-core-module) a [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-157">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="1b6fe-158">Minimální implementace, které konfiguruje hostitele (a aplikace ASP.NET Core) zahrnuje určení serveru a konfiguraci kanálu žádostí aplikace:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-158">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="1b6fe-159">Při nastavování hostitele, [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metody lze zadat.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="1b6fe-160">Pokud `Startup` je zadána třída, musíte definovat `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="1b6fe-161">Další informace najdete v tématu [spuštění aplikace v ASP.NET Core](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-161">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="1b6fe-162">Více volá, aby se `ConfigureServices` připojit k sobě.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="1b6fe-163">Více volá, aby se `Configure` nebo `UseStartup` na `WebHostBuilder` nahradit předchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="1b6fe-164">Hodnoty konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="1b6fe-164">Host configuration values</span></span>

<span data-ttu-id="1b6fe-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) závisí na následujících dvou přístupů k nastavení hostitelské hodnoty konfigurace:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="1b6fe-166">Konfigurace hostitele tvůrce, který zahrnuje proměnné prostředí ve formátu `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="1b6fe-167">Například `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="1b6fe-168">Explicitní metody, jako například [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-168">Explicit methods, such as [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span></span>
* <span data-ttu-id="1b6fe-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) a přidružené klíče.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="1b6fe-170">Při nastavení hodnoty s `UseSetting`, je hodnota nastavena jako bez ohledu na typ řetězec.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="1b6fe-171">Hostitel používá. obě tyto možnosti nastaví hodnotu poslední.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="1b6fe-172">Další informace najdete v tématu [konfigurace přepisování](#override-configuration) v další části.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="1b6fe-173">Zaznamenat chyby při spuštění</span><span class="sxs-lookup"><span data-stu-id="1b6fe-173">Capture Startup Errors</span></span>

<span data-ttu-id="1b6fe-174">Toto nastavení řídí zaznamenávání chyby při spuštění.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-174">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="1b6fe-175">**Klíč**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="1b6fe-175">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="1b6fe-176">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="1b6fe-176">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1b6fe-177">**Výchozí**: použije se výchozí hodnota `false` Pokud aplikace běží s Kestrel za služby IIS, kde výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-177">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="1b6fe-178">**Nastavit pomocí**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-178">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="1b6fe-179">**Proměnné prostředí**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-179">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="1b6fe-180">Když `false`, chyb během spuštění výsledek v hostiteli. operace bude ukončena.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-180">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="1b6fe-181">Když `true`, hostitel zachytí výjimky během spouštění a pokusí o spuštění serveru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-181">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-182">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="1b6fe-184">Obsah kořenové</span><span class="sxs-lookup"><span data-stu-id="1b6fe-184">Content Root</span></span>

<span data-ttu-id="1b6fe-185">Toto nastavení určuje, kde začíná ASP.NET Core hledání obsahu souborů, jako je například zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-185">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="1b6fe-186">**Klíč**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="1b6fe-186">**Key**: contentRoot</span></span>  
<span data-ttu-id="1b6fe-187">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="1b6fe-187">**Type**: *string*</span></span>  
<span data-ttu-id="1b6fe-188">**Výchozí**: výchozí složce, kde se nachází sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-188">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="1b6fe-189">**Nastavit pomocí**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-189">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="1b6fe-190">**Proměnné prostředí**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-190">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="1b6fe-191">Kořenu obsahu se používá jako základní cesta pro [kořenový Web nastavení](#web-root).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-191">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="1b6fe-192">Pokud cesta neexistuje, hostitel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-192">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-193">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-193">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="1b6fe-195">Podrobné chyby</span><span class="sxs-lookup"><span data-stu-id="1b6fe-195">Detailed Errors</span></span>

<span data-ttu-id="1b6fe-196">Určuje, zda podrobné chyby, které mají být zaznamenány.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-196">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="1b6fe-197">**Klíč**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="1b6fe-197">**Key**: detailedErrors</span></span>  
<span data-ttu-id="1b6fe-198">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="1b6fe-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1b6fe-199">**Výchozí**: false</span><span class="sxs-lookup"><span data-stu-id="1b6fe-199">**Default**: false</span></span>  
<span data-ttu-id="1b6fe-200">**Nastavit pomocí**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-200">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1b6fe-201">**Proměnné prostředí**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-201">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="1b6fe-202">Když povolené (nebo když <a href="#environment">prostředí</a> je nastaven na `Development`), aplikace zaznamená podrobné výjimky.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-202">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-203">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="1b6fe-205">Prostředí</span><span class="sxs-lookup"><span data-stu-id="1b6fe-205">Environment</span></span>

<span data-ttu-id="1b6fe-206">Nastaví prostředí aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-206">Sets the app's environment.</span></span>

<span data-ttu-id="1b6fe-207">**Klíč**: prostředí</span><span class="sxs-lookup"><span data-stu-id="1b6fe-207">**Key**: environment</span></span>  
<span data-ttu-id="1b6fe-208">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="1b6fe-208">**Type**: *string*</span></span>  
<span data-ttu-id="1b6fe-209">**Výchozí**: produkční</span><span class="sxs-lookup"><span data-stu-id="1b6fe-209">**Default**: Production</span></span>  
<span data-ttu-id="1b6fe-210">**Nastavit pomocí**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-210">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="1b6fe-211">**Proměnné prostředí**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-211">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="1b6fe-212">Prostředí můžete nastavit na jakoukoli hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-212">The environment can be set to any value.</span></span> <span data-ttu-id="1b6fe-213">Framework definované hodnoty zahrnují `Development`, `Staging`, a `Production`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-213">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="1b6fe-214">Hodnoty nejsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-214">Values aren't case sensitive.</span></span> <span data-ttu-id="1b6fe-215">Ve výchozím nastavení *prostředí* je pro čtení z `ASPNETCORE_ENVIRONMENT` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-215">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="1b6fe-216">Při použití [Visual Studio](https://www.visualstudio.com/), proměnné prostředí může být nastavena v *launchSettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-216">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="1b6fe-217">Další informace najdete v tématu [použijte prostředí s více](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-217">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-218">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="1b6fe-220">Hostování spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="1b6fe-220">Hosting Startup Assemblies</span></span>

<span data-ttu-id="1b6fe-221">Nastaví hostování spuštění sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-221">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="1b6fe-222">**Klíč**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="1b6fe-222">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="1b6fe-223">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="1b6fe-223">**Type**: *string*</span></span>  
<span data-ttu-id="1b6fe-224">**Výchozí**: prázdný řetězec</span><span class="sxs-lookup"><span data-stu-id="1b6fe-224">**Default**: Empty string</span></span>  
<span data-ttu-id="1b6fe-225">**Nastavit pomocí**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-225">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1b6fe-226">**Proměnné prostředí**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-226">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="1b6fe-227">Řetězec oddělený středníkem hostování spuštění sestavení načíst při spuštění.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-227">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="1b6fe-228">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-228">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="1b6fe-229">I když výchozí hodnota konfigurace je řetězec prázdný, hostování spuštění sestavení, vždy zahrňte sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="1b6fe-230">Při hostování spuštění sestavení jsou k dispozici, se přidají do sestavení aplikace pro načtení, když aplikace sestavení jeho společných služeb během spouštění.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-231">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-231">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-232">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b6fe-233">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-233">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="1b6fe-234">Dáváte přednost hostování adresy URL</span><span class="sxs-lookup"><span data-stu-id="1b6fe-234">Prefer Hosting URLs</span></span>

<span data-ttu-id="1b6fe-235">Určuje, zda hostitel naslouchat požadavkům na adresy URL nakonfigurované `WebHostBuilder` místo nastavení nakonfigurovaného pomocí `IServer` implementace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-235">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="1b6fe-236">**Klíč**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="1b6fe-236">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="1b6fe-237">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="1b6fe-237">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1b6fe-238">**Výchozí**: true</span><span class="sxs-lookup"><span data-stu-id="1b6fe-238">**Default**: true</span></span>  
<span data-ttu-id="1b6fe-239">**Nastavit pomocí**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-239">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="1b6fe-240">**Proměnné prostředí**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-240">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="1b6fe-241">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-241">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-242">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-242">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-243">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-243">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b6fe-244">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-244">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="1b6fe-245">Zabránit spuštění hostování</span><span class="sxs-lookup"><span data-stu-id="1b6fe-245">Prevent Hosting Startup</span></span>

<span data-ttu-id="1b6fe-246">Brání automatické načítání hostování spuštění sestavení, a to včetně hostování spuštění sestavení nakonfiguroval sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-246">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="1b6fe-247">V tématu [vylepšení aplikace z externí sestavení s IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) Další informace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-247">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="1b6fe-248">**Klíč**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="1b6fe-248">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="1b6fe-249">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="1b6fe-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1b6fe-250">**Výchozí**: false</span><span class="sxs-lookup"><span data-stu-id="1b6fe-250">**Default**: false</span></span>  
<span data-ttu-id="1b6fe-251">**Nastavit pomocí**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-251">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1b6fe-252">**Proměnné prostředí**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-252">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="1b6fe-253">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-253">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-254">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-255">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-255">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b6fe-256">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-256">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="1b6fe-257">Adresy URL serveru</span><span class="sxs-lookup"><span data-stu-id="1b6fe-257">Server URLs</span></span>

<span data-ttu-id="1b6fe-258">Určuje IP adres nebo adres hostitele s porty a protokoly, které server naslouchat požadavkům na požadavky.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-258">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="1b6fe-259">**Klíč**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="1b6fe-259">**Key**: urls</span></span>  
<span data-ttu-id="1b6fe-260">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="1b6fe-260">**Type**: *string*</span></span>  
<span data-ttu-id="1b6fe-261">**Výchozí**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="1b6fe-261">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="1b6fe-262">**Nastavit pomocí**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-262">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="1b6fe-263">**Proměnné prostředí**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-263">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="1b6fe-264">Nastavte na oddělených středníkem (;) seznam URL předpony serveru by měl odpovídat.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-264">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="1b6fe-265">Například `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-265">For example, `http://localhost:123`.</span></span> <span data-ttu-id="1b6fe-266">Použití "\*" k označení, že by měl server přijímat požadavky na všechny IP adresy nebo názvu hostitele pomocí zadaný port a protokol (například `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-266">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="1b6fe-267">Protokol (`http://` nebo `https://`) musí být součástí každou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-267">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="1b6fe-268">Podporované formáty liší mezi servery.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-268">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-269">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-269">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="1b6fe-270">Kestrel má svůj vlastní koncový bod rozhraní API konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="1b6fe-271">Další informace najdete v tématu [Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-271">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-272">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-272">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="1b6fe-273">Časový limit vypnutí</span><span class="sxs-lookup"><span data-stu-id="1b6fe-273">Shutdown Timeout</span></span>

<span data-ttu-id="1b6fe-274">Určuje dobu čekání na webového hostitele a ukončí se.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-274">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="1b6fe-275">**Klíč**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="1b6fe-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="1b6fe-276">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="1b6fe-276">**Type**: *int*</span></span>  
<span data-ttu-id="1b6fe-277">**Výchozí**: 5</span><span class="sxs-lookup"><span data-stu-id="1b6fe-277">**Default**: 5</span></span>  
<span data-ttu-id="1b6fe-278">**Nastavit pomocí**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="1b6fe-279">**Proměnné prostředí**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="1b6fe-280">I když přijme klíč *int* s `UseSetting` (například `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) rozšíření metoda přebírá [časový interval](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="1b6fe-281">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-281">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="1b6fe-282">Během časového limitu období, hostování:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-282">During the timeout period, hosting:</span></span>

* <span data-ttu-id="1b6fe-283">Aktivační události [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-283">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="1b6fe-284">Pokusy o zastavení hostované služby, protokolování pro služby, které se nepodařilo zastavit všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-284">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="1b6fe-285">Pokud vyprší časový limit před všechny zastavení hostovaných služeb, jsou zastaveny všechny zbývající služby active při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-285">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="1b6fe-286">Zastavení služeb i v případě, že se nedokončilo zpracování.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-286">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="1b6fe-287">Pokud služby vyžadují další čas ukončení, zvýšit časový limit.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-287">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-288">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-288">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-289">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-289">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b6fe-290">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-290">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="1b6fe-291">Spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="1b6fe-291">Startup Assembly</span></span>

<span data-ttu-id="1b6fe-292">Určuje sestavení pro vyhledávání `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-292">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="1b6fe-293">**Klíč**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="1b6fe-293">**Key**: startupAssembly</span></span>  
<span data-ttu-id="1b6fe-294">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="1b6fe-294">**Type**: *string*</span></span>  
<span data-ttu-id="1b6fe-295">**Výchozí**: sestavení aplikace</span><span class="sxs-lookup"><span data-stu-id="1b6fe-295">**Default**: The app's assembly</span></span>  
<span data-ttu-id="1b6fe-296">**Nastavit pomocí**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-296">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="1b6fe-297">**Proměnné prostředí**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-297">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="1b6fe-298">Sestavení podle názvu (`string`) nebo typ (`TStartup`) může být odkaz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-298">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="1b6fe-299">Pokud je to více `UseStartup` metody jsou volány, poslední změny mají přednost.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-299">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-300">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="1b6fe-302">Kořenový web</span><span class="sxs-lookup"><span data-stu-id="1b6fe-302">Web Root</span></span>

<span data-ttu-id="1b6fe-303">Nastaví relativní cestu na statické prostředky aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-303">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="1b6fe-304">**Klíč**: webroot</span><span class="sxs-lookup"><span data-stu-id="1b6fe-304">**Key**: webroot</span></span>  
<span data-ttu-id="1b6fe-305">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="1b6fe-305">**Type**: *string*</span></span>  
<span data-ttu-id="1b6fe-306">**Výchozí**: Pokud není zadáno, výchozí hodnota je "(Content Root)/wwwroot", pokud cesta existuje.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-306">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="1b6fe-307">Pokud cesta neexistuje, je použít soubor no-op zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-307">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="1b6fe-308">**Nastavit pomocí**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-308">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="1b6fe-309">**Proměnné prostředí**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-309">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-310">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="1b6fe-312">Přepsat konfiguraci</span><span class="sxs-lookup"><span data-stu-id="1b6fe-312">Override configuration</span></span>

<span data-ttu-id="1b6fe-313">Použití [konfigurace](xref:fundamentals/configuration/index) konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-313">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="1b6fe-314">V následujícím příkladu je konfigurace hostitele volitelně specifikován v *hosting.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-314">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="1b6fe-315">Všechny konfigurace načtena z *hosting.json* soubor může být přepsána argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-315">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="1b6fe-316">Integrovaný konfigurace (v `config`) se používá ke konfiguraci hostitele s `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-316">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-317">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-317">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1b6fe-318">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-318">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="1b6fe-319">Přepsání zadaná podle konfigurace `UseUrls` s *hosting.json* konfigurace příkazového řádku, první argument konfigurace druhý:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-319">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-320">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b6fe-321">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-321">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="1b6fe-322">Přepsání zadaná podle konfigurace `UseUrls` s *hosting.json* konfigurace příkazového řádku, první argument konfigurace druhý:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-322">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="1b6fe-323">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) metoda rozšíření není aktuálně schopen analyzovat konfigurační oddíl vrácený `GetSection` (například `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-323">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="1b6fe-324">`GetSection` Metoda filtry konfigurace klíče do části požadovaný, ale ponechá název oddílu na klíče (například `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-324">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="1b6fe-325">`UseConfiguration` Metoda očekává klíče tak, aby odpovídala `WebHostBuilder` klíče (například `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-325">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="1b6fe-326">Přítomnost názvu oddílu na klíče zabrání hodnoty v části Konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-326">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="1b6fe-327">Tento problém bude vyřešen v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-327">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="1b6fe-328">Další informace a řešení, najdete v části [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration používá úplné klíče](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-328">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="1b6fe-329">Pokud chcete zadat hostiteli spustit na konkrétní adresu URL, požadovanou hodnotu lze předat ve z příkazového řádku při provádění [dotnet spustit](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-329">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="1b6fe-330">Přepíše argument příkazového řádku `urls` z hodnoty *hosting.json* souboru a server naslouchá na portu 8080:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-330">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="1b6fe-331">Spravovat hostitele</span><span class="sxs-lookup"><span data-stu-id="1b6fe-331">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b6fe-332">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1b6fe-333">**Spustit**</span><span class="sxs-lookup"><span data-stu-id="1b6fe-333">**Run**</span></span>

<span data-ttu-id="1b6fe-334">`Run` Metoda spustí webovou aplikaci a blokuje volající vlákno, dokud se vypne hostitele:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-334">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="1b6fe-335">**Start**</span><span class="sxs-lookup"><span data-stu-id="1b6fe-335">**Start**</span></span>

<span data-ttu-id="1b6fe-336">Spuštění hostitele způsobem neblokující voláním jeho `Start` metoda:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-336">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="1b6fe-337">Pokud je předán seznam adres URL, `Start` metoda, naslouchá na zadané adresy URL:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-337">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="1b6fe-338">Aplikace můžete inicializaci a spuštění nové hostitele pomocí předem nakonfigurovaných výchozích nastavení `CreateDefaultBuilder` pomocí jiné metody statické pohodlí.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-338">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="1b6fe-339">Tyto metody start pro server bez výstup konzoly a s [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) počkejte zalomení (Ctrl-C nebo sigint – nebo SIGTERM –):</span><span class="sxs-lookup"><span data-stu-id="1b6fe-339">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="1b6fe-340">**Spuštění (RequestDelegate aplikace)**</span><span class="sxs-lookup"><span data-stu-id="1b6fe-340">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="1b6fe-341">Začněte `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-341">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1b6fe-342">Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="1b6fe-342">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="1b6fe-343">`WaitForShutdown` bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-343">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1b6fe-344">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-344">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1b6fe-345">**Spuštění (řetězec adresy url, RequestDelegate aplikace)**</span><span class="sxs-lookup"><span data-stu-id="1b6fe-345">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="1b6fe-346">Spustit s adresou URL a `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-346">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1b6fe-347">Stejný výsledek jako **spuštění (RequestDelegate aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-347">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="1b6fe-348">**Spuštění (akce&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="1b6fe-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="1b6fe-349">Použít instanci `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) používat směrování middleware:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-349">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="1b6fe-350">Použijte následující požadavky na prohlížeč s příkladu:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-350">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="1b6fe-351">Požadavek</span><span class="sxs-lookup"><span data-stu-id="1b6fe-351">Request</span></span>                                    | <span data-ttu-id="1b6fe-352">Odpověď</span><span class="sxs-lookup"><span data-stu-id="1b6fe-352">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="1b6fe-353">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="1b6fe-353">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="1b6fe-354">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="1b6fe-354">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="1b6fe-355">Vyvolá výjimku řetězcem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="1b6fe-355">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="1b6fe-356">Vyvolá výjimku řetězcem "Uh jejda!"</span><span class="sxs-lookup"><span data-stu-id="1b6fe-356">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="1b6fe-357">Santé, kevina!</span><span class="sxs-lookup"><span data-stu-id="1b6fe-357">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="1b6fe-358">Ahoj světe!</span><span class="sxs-lookup"><span data-stu-id="1b6fe-358">Hello World!</span></span>                             |

<span data-ttu-id="1b6fe-359">`WaitForShutdown` bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-359">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1b6fe-360">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-360">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1b6fe-361">**Spuštění (řetězce adresy url, akce&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="1b6fe-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="1b6fe-362">Použijte adresu URL a instance `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-362">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="1b6fe-363">Stejný výsledek jako **spuštění (akce&lt;IRouteBuilder&gt; routeBuilder)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-363">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="1b6fe-364">**StartWith (akce&lt;IApplicationBuilder&gt; aplikace)**</span><span class="sxs-lookup"><span data-stu-id="1b6fe-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="1b6fe-365">Zadejte delegát pro konfiguraci `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-365">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="1b6fe-366">Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="1b6fe-366">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="1b6fe-367">`WaitForShutdown` bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-367">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1b6fe-368">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-368">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1b6fe-369">**StartWith (řetězce adresy url, akce&lt;IApplicationBuilder&gt; aplikace)**</span><span class="sxs-lookup"><span data-stu-id="1b6fe-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="1b6fe-370">Zadejte adresu URL a delegát pro konfiguraci `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-370">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="1b6fe-371">Stejný výsledek jako **StartWith (akce&lt;IApplicationBuilder&gt; aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-371">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b6fe-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b6fe-373">**Spustit**</span><span class="sxs-lookup"><span data-stu-id="1b6fe-373">**Run**</span></span>

<span data-ttu-id="1b6fe-374">`Run` Metoda spustí webovou aplikaci a blokuje volající vlákno, dokud se vypne hostitele:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-374">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="1b6fe-375">**Start**</span><span class="sxs-lookup"><span data-stu-id="1b6fe-375">**Start**</span></span>

<span data-ttu-id="1b6fe-376">Spuštění hostitele způsobem neblokující voláním jeho `Start` metoda:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-376">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="1b6fe-377">Pokud je předán seznam adres URL, `Start` metoda, naslouchá na zadané adresy URL:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-377">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="1b6fe-378">IHostingEnvironment rozhraní</span><span class="sxs-lookup"><span data-stu-id="1b6fe-378">IHostingEnvironment interface</span></span>

<span data-ttu-id="1b6fe-379">[IHostingEnvironment rozhraní](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) poskytuje informace o hostování prostředí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-379">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="1b6fe-380">Použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-380">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="1b6fe-381">A [založené na konvenci přístup](xref:fundamentals/environments#startup-conventions) můžete použít ke konfiguraci aplikace při spuštění založených na prostředí.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-381">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="1b6fe-382">Můžete také vložit `IHostingEnvironment` do `Startup` konstruktor pro použití v `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-382">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="1b6fe-383">Kromě `IsDevelopment` metoda rozšíření `IHostingEnvironment` nabízí `IsStaging`, `IsProduction`, a `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-383">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="1b6fe-384">V tématu [použijte prostředí s více](xref:fundamentals/environments) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-384">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="1b6fe-385">`IHostingEnvironment` Služby může také vložit přímo do `Configure` metoda pro nastavení zpracování kanálu:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-385">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="1b6fe-386">`IHostingEnvironment` můžete vložit do `Invoke` metoda při vytváření vlastní [middleware](xref:fundamentals/middleware/index#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="1b6fe-386">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="1b6fe-387">IApplicationLifetime rozhraní</span><span class="sxs-lookup"><span data-stu-id="1b6fe-387">IApplicationLifetime interface</span></span>

<span data-ttu-id="1b6fe-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umožňuje po spuštění a vypnutí aktivity.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="1b6fe-389">Zrušení tokenů použitá pro zaregistrování jsou tři vlastnosti na rozhraní `Action` metody, které definují události spuštění a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-389">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="1b6fe-390">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="1b6fe-390">Cancellation Token</span></span>    | <span data-ttu-id="1b6fe-391">Při aktivaci&#8230;</span><span class="sxs-lookup"><span data-stu-id="1b6fe-391">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="1b6fe-392">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="1b6fe-392">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="1b6fe-393">Hostitele plně byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-393">The host has fully started.</span></span> |
| [<span data-ttu-id="1b6fe-394">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="1b6fe-394">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="1b6fe-395">Hostitel je dokončení řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="1b6fe-396">Všechny požadavky, měla by být zpracována.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-396">All requests should be processed.</span></span> <span data-ttu-id="1b6fe-397">Vypnutí bloky až po dokončení této události.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-397">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="1b6fe-398">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="1b6fe-398">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="1b6fe-399">Hostitel provádí řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-399">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="1b6fe-400">Může být stále aktivní žádosti.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-400">Requests may still be processing.</span></span> <span data-ttu-id="1b6fe-401">Vypnutí bloky až po dokončení této události.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-401">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="1b6fe-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) požadavky ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="1b6fe-403">Následující třídy používá `StopApplication` korektně vypnout aplikaci při třídy `Shutdown` metoda je volána:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-403">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="1b6fe-404">Ověření oboru</span><span class="sxs-lookup"><span data-stu-id="1b6fe-404">Scope validation</span></span>

<span data-ttu-id="1b6fe-405">V technologii ASP.NET Core 2.0 nebo novější [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) nastaví [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) k `true` Pokud prostředí aplikace je vývoj.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-405">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="1b6fe-406">Když `ValidateScopes` je nastaven na `true`, výchozím zprostředkovatelem služeb provádí kontroly, aby ověřte, že:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-406">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="1b6fe-407">Vymezené služby nejsou přímo nebo nepřímo přeložit od kořenové poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-407">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="1b6fe-408">Vymezená služby nejsou přímo nebo nepřímo vložit do jednotlivých prvků.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-408">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="1b6fe-409">Kořenového poskytovatele služby se vytvoří při [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) je volána.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-409">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="1b6fe-410">Doba platnosti poskytovatele služeb kořenové odpovídá na aplikační nebo server životního cyklu, pokud zprostředkovatel začíná aplikace a uvolnění při vypnutí aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-410">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="1b6fe-411">Vymezená služby jsou zapomenuty kontejnerem, který je vytvořil.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-411">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="1b6fe-412">Pokud vymezené služby se vytvoří v kořenovém kontejneru, životnost služby je efektivně povýšen na singleton, protože jejich pouze likvidace podle Kořenový kontejner při ukončení aplikace nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-412">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="1b6fe-413">Ověřování služby Obory zachytí těchto situacích při `BuildServiceProvider` je volána.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-413">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="1b6fe-414">Vždy ověření obory, včetně v provozním prostředí, nakonfigurujte [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) s [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) na tvůrce hostitele:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-414">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="1b6fe-415">Řešení potíží s System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="1b6fe-415">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="1b6fe-416">**Platí pro pouze ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="1b6fe-416">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="1b6fe-417">Hostitel může být sestaven vložením `IStartup` přímo do kontejneru pro vkládání závislosti místo volání `UseStartup` nebo `Configure`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-417">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="1b6fe-418">Pokud hostitel je integrovaný tímto způsobem, může dojít k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-418">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="1b6fe-419">K tomu dochází, protože [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (aktuální sestavení) je nutná ke kontrole pro `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-419">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="1b6fe-420">Pokud aplikace ručně vloží `IStartup` do kontejneru pro vkládání závislosti, přidejte následující volání `WebHostBuilder` se zadaným názvem sestavení:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-420">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="1b6fe-421">Můžete taky přidat fiktivní `Configure` k `WebHostBuilder`, která nastaví `applicationName`(`ApplicationKey`) automaticky:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-421">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="1b6fe-422">**Poznámka:**: Toto je pouze požadované verze technologie ASP.NET 2.0 jádra a pouze pokud aplikace nemá volání `UseStartup` nebo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-422">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="1b6fe-423">Další informace najdete v tématu [oznámení: Microsoft.Extensions.PlatformAbstractions byl odebrán (komentář)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) a [StartupInjection ukázka](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-423">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1b6fe-424">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1b6fe-424">Additional resources</span></span>

* [<span data-ttu-id="1b6fe-425">Hostování ve Windows se službou IIS</span><span class="sxs-lookup"><span data-stu-id="1b6fe-425">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="1b6fe-426">Hostování v Linuxu na serveru Nginx</span><span class="sxs-lookup"><span data-stu-id="1b6fe-426">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="1b6fe-427">Hostování v Linuxu na serveru Apache</span><span class="sxs-lookup"><span data-stu-id="1b6fe-427">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="1b6fe-428">Hostitele ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="1b6fe-428">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
