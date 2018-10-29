---
title: Obecný hostitele .NET
author: guardrex
description: Další informace o obecných Host v .NET, který je zodpovědný za spouštění a životního cyklu správy aplikací.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: e5f91ed64b7f8402dfe938f0fa8a0d94755d15c6
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207716"
---
# <a name="net-generic-host"></a><span data-ttu-id="340b0-103">Obecný hostitele .NET</span><span class="sxs-lookup"><span data-stu-id="340b0-103">.NET Generic Host</span></span>

<span data-ttu-id="340b0-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="340b0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="340b0-105">Konfigurace aplikace .NET core a spouštění *hostitele*.</span><span class="sxs-lookup"><span data-stu-id="340b0-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="340b0-106">Hostitel je zodpovědný za spouštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="340b0-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="340b0-107">Toto téma obsahuje obecný hostitele ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), což je užitečné pro hostování aplikací, které není zpracovávají požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="340b0-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="340b0-108">Pro pokrytí webového hostitele ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), najdete v článku <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="340b0-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="340b0-109">Cílem obecný hostitele je oddělit kanálu HTTP z hostitele webového rozhraní API umožňující širší škálu scénářů hostitele.</span><span class="sxs-lookup"><span data-stu-id="340b0-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="340b0-110">Zasílání zpráv, úlohy na pozadí a další úlohy jiným protokolem než HTTP podle obecného hostitele, který je výhodné společné funkce, jako jsou konfigurace, injektáž závislostí (DI) a protokolování.</span><span class="sxs-lookup"><span data-stu-id="340b0-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="340b0-111">Obecný hostitele je nového v ASP.NET Core 2.1 a není vhodné pro scénáře hostování webů.</span><span class="sxs-lookup"><span data-stu-id="340b0-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="340b0-112">Pro web scénářích hostování, použijte [webového hostitele](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="340b0-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="340b0-113">Obecný hostitele je ve vývoji a nahraďte webového hostitele v budoucí verzi fungovat jako primární hostitele rozhraní API scénáře jiným protokolem než HTTP a protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="340b0-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="340b0-114">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([stažení](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="340b0-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="340b0-115">Při spuštění ukázkové aplikace [Visual Studio Code](https://code.visualstudio.com/), použijte *externí nebo integrovaného terminálu*.</span><span class="sxs-lookup"><span data-stu-id="340b0-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="340b0-116">Nejdou spustit v ukázce `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="340b0-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="340b0-117">Nastavení konzoly ve Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="340b0-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="340b0-118">Otevřít *.vscode/launch.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="340b0-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="340b0-119">V **.NET Core spuštění (konzola)** konfigurace, vyhledejte **konzoly** položka.</span><span class="sxs-lookup"><span data-stu-id="340b0-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="340b0-120">Nastavte hodnotu na buď `externalTerminal` nebo `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="340b0-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="340b0-121">Úvod</span><span class="sxs-lookup"><span data-stu-id="340b0-121">Introduction</span></span>

<span data-ttu-id="340b0-122">Je k dispozici v knihovně obecný hostitele [obor názvů Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) a poskytuje [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="340b0-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="340b0-123">`Microsoft.Extensions.Hosting` Je součástí balíčku [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="340b0-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="340b0-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) je vstupním bodem k provádění kódu.</span><span class="sxs-lookup"><span data-stu-id="340b0-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="340b0-125">Každý `IHostedService` implementace provádí v pořadí podle [službu registrace v ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="340b0-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="340b0-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) je volána v každé `IHostedService` při spuštění hostitele a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) je volána v pořadí reverzní registrace při řádné vypnutí hostitele.</span><span class="sxs-lookup"><span data-stu-id="340b0-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="340b0-127">Nastavení hostitele</span><span class="sxs-lookup"><span data-stu-id="340b0-127">Set up a host</span></span>

<span data-ttu-id="340b0-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) hlavní součást, knihovny a aplikace použít k inicializaci, vytvoření a spuštění hostitele:</span><span class="sxs-lookup"><span data-stu-id="340b0-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a><span data-ttu-id="340b0-129">Výchozí služby</span><span class="sxs-lookup"><span data-stu-id="340b0-129">Default services</span></span>

<span data-ttu-id="340b0-130">Během inicializace hostitele jsou registrovány následující služby:</span><span class="sxs-lookup"><span data-stu-id="340b0-130">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="340b0-131">[Prostředí](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="340b0-131">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="340b0-132">[Konfigurace](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="340b0-132">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="340b0-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="340b0-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="340b0-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="340b0-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="340b0-135">[Možnosti](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="340b0-135">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="340b0-136">[Protokolování](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="340b0-136">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="340b0-137">Konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="340b0-137">Host configuration</span></span>

<span data-ttu-id="340b0-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) závisí na následujících dvou přístupů k nastavení hodnoty konfigurace hostitele:</span><span class="sxs-lookup"><span data-stu-id="340b0-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="340b0-139">Configuration builder</span><span class="sxs-lookup"><span data-stu-id="340b0-139">Configuration builder</span></span>
* <span data-ttu-id="340b0-140">Konfigurace metody rozšíření</span><span class="sxs-lookup"><span data-stu-id="340b0-140">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="340b0-141">Configuration builder</span><span class="sxs-lookup"><span data-stu-id="340b0-141">Configuration builder</span></span>

<span data-ttu-id="340b0-142">Konfigurace hostitele Tvůrce je vytvořen zavoláním [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementace.</span><span class="sxs-lookup"><span data-stu-id="340b0-142">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="340b0-143">`ConfigureHostConfiguration` používá [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) k vytvoření [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="340b0-143">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="340b0-144">Configuration builder inicializuje [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) pro použití v procesu sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="340b0-144">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="340b0-145">Konfigurace proměnných prostředí není přidán ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="340b0-145">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="340b0-146">Volání [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) na tvůrce hostitele a nakonfigurujte hostitele z proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="340b0-146">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="340b0-147">`AddEnvironmentVariables` přijímá volitelný uživatelsky definovanou předponu.</span><span class="sxs-lookup"><span data-stu-id="340b0-147">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="340b0-148">Ukázková aplikace používá předponou `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="340b0-148">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="340b0-149">Předpona, která se odebere, když jsou proměnné prostředí načteny.</span><span class="sxs-lookup"><span data-stu-id="340b0-149">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="340b0-150">Když je ukázková aplikace hostitel nakonfigurovaný, hodnotu proměnné prostředí pro `PREFIX_ENVIRONMENT` stane hodnota konfigurace hostitele `environment` klíč.</span><span class="sxs-lookup"><span data-stu-id="340b0-150">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="340b0-151">Během vývoje. při použití [sady Visual Studio](https://www.visualstudio.com/) nebo spuštěním aplikace s `dotnet run`, lze nastavit proměnné prostředí *Properties/launchSettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="340b0-151">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="340b0-152">V [Visual Studio Code](https://code.visualstudio.com/), lze nastavit proměnné prostředí *.vscode/launch.json* souboru během vývoje.</span><span class="sxs-lookup"><span data-stu-id="340b0-152">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="340b0-153">Další informace naleznete v tématu <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="340b0-153">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="340b0-154">`ConfigureHostConfiguration` můžete volat vícekrát s přičítáním výsledky.</span><span class="sxs-lookup"><span data-stu-id="340b0-154">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="340b0-155">Hostitel používá kterékoli z těchto možností nastaví hodnotu poslední.</span><span class="sxs-lookup"><span data-stu-id="340b0-155">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="340b0-156">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="340b0-156">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="340b0-157">Příklad `HostBuilder` konfiguraci pomocí `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="340b0-157">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="340b0-158">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) rozšiřující metoda není aktuálně podporuje při analýze oddílu konfigurace vrácený [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (například `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="340b0-158">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="340b0-159">`GetSection` Metoda filtry konfigurační klíče do části Požadovaná, ale ponechá název oddílu, který na klíče (například `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="340b0-159">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="340b0-160">`AddConfiguration` Metoda očekává klíčů tak, aby odpovídaly `HostBuilder` klíče (například `environment`).</span><span class="sxs-lookup"><span data-stu-id="340b0-160">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="340b0-161">Přítomnost název oddílu, který klíčů brání hodnoty v části Konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="340b0-161">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="340b0-162">Tento problém bude vyřešen v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="340b0-162">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="340b0-163">Další informace a řešení najdete v tématu [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration využívá úplnou klíčů](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="340b0-163">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="340b0-164">Konfigurace metody rozšíření</span><span class="sxs-lookup"><span data-stu-id="340b0-164">Extension method configuration</span></span>

<span data-ttu-id="340b0-165">Metody rozšíření jsou volány v `IHostBuilder` implementace nakonfigurujte obsahu kořenové certifikáty a prostředí.</span><span class="sxs-lookup"><span data-stu-id="340b0-165">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="application-key-name"></a><span data-ttu-id="340b0-166">Klíč aplikace (název)</span><span class="sxs-lookup"><span data-stu-id="340b0-166">Application Key (Name)</span></span>

<span data-ttu-id="340b0-167">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) vlastnost nastaven z konfigurace hostitele během vytváření hostitele.</span><span class="sxs-lookup"><span data-stu-id="340b0-167">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is set from host configuration during host construction.</span></span> <span data-ttu-id="340b0-168">Pokud chcete explicitně nastavit hodnotu, použijte [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="340b0-168">To set the value explicitly, use the [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span></span>

<span data-ttu-id="340b0-169">**Klíč**: applicationName</span><span class="sxs-lookup"><span data-stu-id="340b0-169">**Key**: applicationName</span></span>  
<span data-ttu-id="340b0-170">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="340b0-170">**Type**: *string*</span></span>  
<span data-ttu-id="340b0-171">**Výchozí**: název sestavení obsahující vstupní bod aplikace.</span><span class="sxs-lookup"><span data-stu-id="340b0-171">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="340b0-172">**Sada s použitím**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="340b0-172">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="340b0-173">**Proměnná prostředí**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` je [volitelné a uživatelem definovanými](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="340b0-173">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a><span data-ttu-id="340b0-174">Obsah kořenové</span><span class="sxs-lookup"><span data-stu-id="340b0-174">Content Root</span></span>

<span data-ttu-id="340b0-175">Toto nastavení určuje, kde začíná hostitele vyhledávání obsahu souborů.</span><span class="sxs-lookup"><span data-stu-id="340b0-175">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="340b0-176">**Klíč**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="340b0-176">**Key**: contentRoot</span></span>  
<span data-ttu-id="340b0-177">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="340b0-177">**Type**: *string*</span></span>  
<span data-ttu-id="340b0-178">**Výchozí**: výchozí hodnota je složka, ve které se nachází sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="340b0-178">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="340b0-179">**Sada s použitím**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="340b0-179">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="340b0-180">**Proměnná prostředí**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` je [volitelné a uživatelem definovanými](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="340b0-180">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="340b0-181">Pokud cesta neexistuje, hostitel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="340b0-181">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="340b0-182">Prostředí</span><span class="sxs-lookup"><span data-stu-id="340b0-182">Environment</span></span>

<span data-ttu-id="340b0-183">Nastaví aplikace [prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="340b0-183">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="340b0-184">**Klíč**: prostředí</span><span class="sxs-lookup"><span data-stu-id="340b0-184">**Key**: environment</span></span>  
<span data-ttu-id="340b0-185">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="340b0-185">**Type**: *string*</span></span>  
<span data-ttu-id="340b0-186">**Výchozí**: produkčního prostředí</span><span class="sxs-lookup"><span data-stu-id="340b0-186">**Default**: Production</span></span>  
<span data-ttu-id="340b0-187">**Sada s použitím**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="340b0-187">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="340b0-188">**Proměnná prostředí**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` je [volitelné a uživatelem definovanými](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="340b0-188">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="340b0-189">Prostředí můžete nastavit na libovolnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="340b0-189">The environment can be set to any value.</span></span> <span data-ttu-id="340b0-190">Hodnoty definované v rámci rozhraní zahrnují `Development`, `Staging`, a `Production`.</span><span class="sxs-lookup"><span data-stu-id="340b0-190">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="340b0-191">Hodnoty se velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="340b0-191">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="340b0-192">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="340b0-192">ConfigureAppConfiguration</span></span>

<span data-ttu-id="340b0-193">Konfigurace Tvůrce aplikací je vytvořen zavoláním [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementace.</span><span class="sxs-lookup"><span data-stu-id="340b0-193">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="340b0-194">`ConfigureAppConfiguration` používá [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) k vytvoření [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="340b0-194">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="340b0-195">`ConfigureAppConfiguration` můžete volat vícekrát s přičítáním výsledky.</span><span class="sxs-lookup"><span data-stu-id="340b0-195">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="340b0-196">Aplikace používá, podle toho, která možnost poslední nastaví hodnotu.</span><span class="sxs-lookup"><span data-stu-id="340b0-196">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="340b0-197">Konfigurace vytvořil `ConfigureAppConfiguration` je k dispozici na [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) pro následné operace a v [služby](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="340b0-197">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="340b0-198">Příklad aplikace konfigurace pomocí `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="340b0-198">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="340b0-199">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="340b0-199">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="340b0-200">*appSettings. Development.JSON*:</span><span class="sxs-lookup"><span data-stu-id="340b0-200">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="340b0-201">*appSettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="340b0-201">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="340b0-202">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) rozšiřující metoda není aktuálně podporuje při analýze oddílu konfigurace vrácený [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (například `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="340b0-202">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="340b0-203">`GetSection` Metoda filtry konfigurační klíče do části Požadovaná, ale ponechá název oddílu, který na klíče (například `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="340b0-203">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="340b0-204">`AddConfiguration` Metoda očekává se přesně shodují s konfigurační klíče (například `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="340b0-204">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="340b0-205">Přítomnost název oddílu, který klíčů brání hodnoty v části Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="340b0-205">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="340b0-206">Tento problém bude vyřešen v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="340b0-206">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="340b0-207">Další informace a řešení najdete v tématu [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration využívá úplnou klíčů](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="340b0-207">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="340b0-208">Přesunutí souborů nastavení do výstupního adresáře, zadejte soubory nastavení jako [položky projektu nástroje MSBuild](/visualstudio/msbuild/common-msbuild-project-items) v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="340b0-208">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="340b0-209">Ukázková aplikace přesune své soubory JSON aplikace nastavení a *hostsettings.json* následujícím **&lt;obsahu:&gt;** položky:</span><span class="sxs-lookup"><span data-stu-id="340b0-209">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="340b0-210">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="340b0-210">ConfigureServices</span></span>

<span data-ttu-id="340b0-211">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) přidá do aplikace služeb [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru.</span><span class="sxs-lookup"><span data-stu-id="340b0-211">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="340b0-212">`ConfigureServices` můžete volat vícekrát s přičítáním výsledky.</span><span class="sxs-lookup"><span data-stu-id="340b0-212">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="340b0-213">Hostovanou službu je třída s atributem logiky úloh na pozadí, který implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="340b0-213">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="340b0-214">Další informace naleznete v tématu <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="340b0-214">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="340b0-215">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) používá `AddHostedService` metodu rozšíření k přidání služby pro události doby života `LifetimeEventsHostedService`a úlohu na pozadí vypršel časový limit `TimedHostedService`, do aplikace:</span><span class="sxs-lookup"><span data-stu-id="340b0-215">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="340b0-216">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="340b0-216">ConfigureLogging</span></span>

<span data-ttu-id="340b0-217">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) přidá delegáta pro konfiguraci zadaných [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span><span class="sxs-lookup"><span data-stu-id="340b0-217">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="340b0-218">`ConfigureLogging` může být volána více než jednou s přičítáním výsledky.</span><span class="sxs-lookup"><span data-stu-id="340b0-218">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="340b0-219">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="340b0-219">UseConsoleLifetime</span></span>

<span data-ttu-id="340b0-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) naslouchá `Ctrl+C`/SIGINT nebo SIGTERM a volání [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) zahájíte proces vypnutí.</span><span class="sxs-lookup"><span data-stu-id="340b0-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="340b0-221">`UseConsoleLifetime` odblokuje rozšíření, jako [RunAsync](#runasync) a [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="340b0-221">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="340b0-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) předem registrován jako výchozí implementace životnost.</span><span class="sxs-lookup"><span data-stu-id="340b0-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="340b0-223">Poslední doba života zaregistrovaný se používá.</span><span class="sxs-lookup"><span data-stu-id="340b0-223">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="340b0-224">Konfigurace kontejneru</span><span class="sxs-lookup"><span data-stu-id="340b0-224">Container configuration</span></span>

<span data-ttu-id="340b0-225">Pro podporu připojení v ostatních kontejnerech, může přijmout hostitele [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="340b0-225">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="340b0-226">Poskytuje objekt pro vytváření, které nejsou součástí registrace kontejnerů DI, ale místo toho je vnitřní hostitel používá k vytvoření kontejneru pro konkrétní DI.</span><span class="sxs-lookup"><span data-stu-id="340b0-226">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="340b0-227">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) přepisuje výchozí objekt pro vytváření použitý k vytvoření poskytovatele služby app service.</span><span class="sxs-lookup"><span data-stu-id="340b0-227">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="340b0-228">Konfigurace vlastního kontejneru je spravován [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) metody.</span><span class="sxs-lookup"><span data-stu-id="340b0-228">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="340b0-229">`ConfigureContainer` poskytuje možnosti silného typu pro konfiguraci kontejneru nad základního hostitele rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="340b0-229">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="340b0-230">`ConfigureContainer` můžete volat vícekrát s přičítáním výsledky.</span><span class="sxs-lookup"><span data-stu-id="340b0-230">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="340b0-231">Vytvoření služby kontejneru pro aplikace:</span><span class="sxs-lookup"><span data-stu-id="340b0-231">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="340b0-232">Poskytuje objekt pro vytváření služby kontejneru:</span><span class="sxs-lookup"><span data-stu-id="340b0-232">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="340b0-233">Použijte objekt pro vytváření a konfigurace kontejneru vlastní služby pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="340b0-233">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="340b0-234">Rozšiřitelnost</span><span class="sxs-lookup"><span data-stu-id="340b0-234">Extensibility</span></span>

<span data-ttu-id="340b0-235">Rozšiřitelnost hostitele se provádí pomocí metody rozšíření na `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="340b0-235">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="340b0-236">Následující příklad ukazuje, jak rozšiřující metoda rozšiřuje `IHostBuilder` implementaci [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) příklad jsme vám ukázali v <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="340b0-236">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="340b0-237">Vytvoří aplikaci `UseHostedService` metodu rozšíření k registraci hostované služby předaný `T`:</span><span class="sxs-lookup"><span data-stu-id="340b0-237">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a><span data-ttu-id="340b0-238">Spravovat hostitele</span><span class="sxs-lookup"><span data-stu-id="340b0-238">Manage the host</span></span>

<span data-ttu-id="340b0-239">[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementace je zodpovědný za spouštění a zastavování `IHostedService` implementace, které jsou registrovány v kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="340b0-239">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="340b0-240">Spustit</span><span class="sxs-lookup"><span data-stu-id="340b0-240">Run</span></span>

<span data-ttu-id="340b0-241">[Spustit](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) aplikace spouští a blokuje volající vlákno, dokud nebude ukončen hostitele:</span><span class="sxs-lookup"><span data-stu-id="340b0-241">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a><span data-ttu-id="340b0-242">RunAsync</span><span class="sxs-lookup"><span data-stu-id="340b0-242">RunAsync</span></span>

<span data-ttu-id="340b0-243">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) aplikace spouští a vrátí `Task` , která se dokončí, když se aktivuje token zrušení nebo vypnutí:</span><span class="sxs-lookup"><span data-stu-id="340b0-243">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a><span data-ttu-id="340b0-244">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="340b0-244">RunConsoleAsync</span></span>

<span data-ttu-id="340b0-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) umožňuje podporu konzoly, sestaví a spustí hostitele a čeká na `Ctrl+C`/SIGINT nebo SIGTERM vypnout.</span><span class="sxs-lookup"><span data-stu-id="340b0-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="340b0-246">Počáteční a StopAsync</span><span class="sxs-lookup"><span data-stu-id="340b0-246">Start and StopAsync</span></span>

<span data-ttu-id="340b0-247">[Spustit](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) spustí hostitele synchronně.</span><span class="sxs-lookup"><span data-stu-id="340b0-247">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="340b0-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) limitu pokusí zastavit hostitele v rámci zadaného časového limitu.</span><span class="sxs-lookup"><span data-stu-id="340b0-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a><span data-ttu-id="340b0-249">StartAsync a StopAsync</span><span class="sxs-lookup"><span data-stu-id="340b0-249">StartAsync and StopAsync</span></span>

<span data-ttu-id="340b0-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) spustí aplikaci.</span><span class="sxs-lookup"><span data-stu-id="340b0-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="340b0-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) ukončí aplikaci.</span><span class="sxs-lookup"><span data-stu-id="340b0-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a><span data-ttu-id="340b0-252">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="340b0-252">WaitForShutdown</span></span>

<span data-ttu-id="340b0-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) se spouští přes [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), jako například [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (čeká na `Ctrl+C`/SIGINT nebo SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="340b0-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="340b0-254">`WaitForShutdown` volání [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="340b0-254">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a><span data-ttu-id="340b0-255">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="340b0-255">WaitForShutdownAsync</span></span>

<span data-ttu-id="340b0-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) vrátí `Task` , která se dokončí při vypnutí se spouští přes daný token a volání [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="340b0-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a><span data-ttu-id="340b0-257">Vnější ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="340b0-257">External control</span></span>

<span data-ttu-id="340b0-258">Vnější ovládací prvek hostitele lze dosáhnout pomocí metody, které je možné volat externě:</span><span class="sxs-lookup"><span data-stu-id="340b0-258">External control of the host can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<span data-ttu-id="340b0-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) je volána na začátku [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), která čeká, dokud neskončí než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="340b0-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="340b0-260">To umožňuje zpoždění spuštění, dokud signalizován externí událostí.</span><span class="sxs-lookup"><span data-stu-id="340b0-260">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="340b0-261">IHostingEnvironment rozhraní</span><span class="sxs-lookup"><span data-stu-id="340b0-261">IHostingEnvironment interface</span></span>

<span data-ttu-id="340b0-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) poskytuje informace o aplikaci prvku hostitelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="340b0-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="340b0-263">Použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:</span><span class="sxs-lookup"><span data-stu-id="340b0-263">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

<span data-ttu-id="340b0-264">Další informace naleznete v tématu <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="340b0-264">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="340b0-265">IApplicationLifetime rozhraní</span><span class="sxs-lookup"><span data-stu-id="340b0-265">IApplicationLifetime interface</span></span>

<span data-ttu-id="340b0-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) umožňuje po spuštění a vypnutí aktivity, včetně žádostí o řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="340b0-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="340b0-267">Tři vlastnosti v rozhraní jsou tokeny zrušení použije k registraci `Action` metody, které definují události spuštění a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="340b0-267">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="340b0-268">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="340b0-268">Cancellation Token</span></span> | <span data-ttu-id="340b0-269">Při aktivaci&#8230;</span><span class="sxs-lookup"><span data-stu-id="340b0-269">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="340b0-270">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="340b0-270">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="340b0-271">Hostitel plně byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="340b0-271">The host has fully started.</span></span> |
| [<span data-ttu-id="340b0-272">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="340b0-272">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="340b0-273">Hostitel se dokončuje řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="340b0-273">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="340b0-274">By měl zpracovat všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="340b0-274">All requests should be processed.</span></span> <span data-ttu-id="340b0-275">Vypnutí blokuje, dokud se tato událost se dokončí.</span><span class="sxs-lookup"><span data-stu-id="340b0-275">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="340b0-276">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="340b0-276">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="340b0-277">Hostitel provádí řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="340b0-277">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="340b0-278">Žádosti se možná ještě zpracovávají.</span><span class="sxs-lookup"><span data-stu-id="340b0-278">Requests may still be processing.</span></span> <span data-ttu-id="340b0-279">Vypnutí blokuje, dokud se tato událost se dokončí.</span><span class="sxs-lookup"><span data-stu-id="340b0-279">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="340b0-280">Vložit konstruktoru `IApplicationLifetime` service do libovolné třídy.</span><span class="sxs-lookup"><span data-stu-id="340b0-280">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="340b0-281">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) používá konstruktor injektáž do `LifetimeEventsHostedService` třídy ( `IHostedService` implementace) k registraci události.</span><span class="sxs-lookup"><span data-stu-id="340b0-281">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="340b0-282">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="340b0-282">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="340b0-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) požaduje ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="340b0-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="340b0-284">Následující třídy používá `StopApplication` řádně vypnout aplikaci při třídy `Shutdown` volání metody:</span><span class="sxs-lookup"><span data-stu-id="340b0-284">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="340b0-285">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="340b0-285">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="340b0-286">Hostování úložiště ukázek na Githubu</span><span class="sxs-lookup"><span data-stu-id="340b0-286">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
