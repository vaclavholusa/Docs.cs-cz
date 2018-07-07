---
title: Obecný hostitele .NET
author: guardrex
description: Další informace o obecných Host v .NET, který je zodpovědný za spouštění a životního cyklu správy aplikací.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: ce2a540cc7a63f61075c9c01759f67531171e1e1
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889152"
---
# <a name="net-generic-host"></a><span data-ttu-id="77f8c-103">Obecný hostitele .NET</span><span class="sxs-lookup"><span data-stu-id="77f8c-103">.NET Generic Host</span></span>

<span data-ttu-id="77f8c-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="77f8c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="77f8c-105">Konfigurace aplikací .NET a spouštění *hostitele*.</span><span class="sxs-lookup"><span data-stu-id="77f8c-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="77f8c-106">Hostitel je zodpovědný za spouštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="77f8c-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="77f8c-107">Toto téma obsahuje obecný hostitele ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), což je užitečné pro hostování aplikací, které není zpracovávají požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="77f8c-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="77f8c-108">Pro pokrytí webového hostitele ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), najdete v článku [webového hostitele](xref:fundamentals/host/web-host) tématu.</span><span class="sxs-lookup"><span data-stu-id="77f8c-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="77f8c-109">Cílem obecný hostitele je oddělit kanálu HTTP z hostitele webového rozhraní API umožňující širší škálu scénářů hostitele.</span><span class="sxs-lookup"><span data-stu-id="77f8c-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="77f8c-110">Zasílání zpráv, úlohy na pozadí a další úlohy jiným protokolem než HTTP podle obecného hostitele, který je výhodné společné funkce, jako jsou konfigurace, injektáž závislostí (DI) a protokolování.</span><span class="sxs-lookup"><span data-stu-id="77f8c-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="77f8c-111">Obecný hostitele je nového v ASP.NET Core 2.1 a není vhodné pro scénáře hostování webů.</span><span class="sxs-lookup"><span data-stu-id="77f8c-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="77f8c-112">Pro web scénářích hostování, použijte [webového hostitele](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="77f8c-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="77f8c-113">Obecný hostitele je ve vývoji a nahraďte webového hostitele v budoucí verzi fungovat jako primární hostitele rozhraní API scénáře jiným protokolem než HTTP a protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="77f8c-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="77f8c-114">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="77f8c-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="77f8c-115">Při spuštění ukázkové aplikace [Visual Studio Code](https://code.visualstudio.com/), použijte *externí nebo integrovaného terminálu*.</span><span class="sxs-lookup"><span data-stu-id="77f8c-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="77f8c-116">Nejdou spustit v ukázce `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="77f8c-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="77f8c-117">Nastavení konzoly ve Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="77f8c-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="77f8c-118">Otevřít *.vscode/launch.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="77f8c-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="77f8c-119">V **.NET Core spuštění (konzola)** konfigurace, vyhledejte **konzoly** položka.</span><span class="sxs-lookup"><span data-stu-id="77f8c-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="77f8c-120">Nastavte hodnotu na buď `externalTerminal` nebo `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="77f8c-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="77f8c-121">Úvod</span><span class="sxs-lookup"><span data-stu-id="77f8c-121">Introduction</span></span>

<span data-ttu-id="77f8c-122">Je k dispozici v knihovně obecný hostitele [obor názvů Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) a poskytuje [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="77f8c-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="77f8c-123">`Microsoft.Extensions.Hosting` Je součástí balíčku [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="77f8c-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="77f8c-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) je vstupním bodem k provádění kódu.</span><span class="sxs-lookup"><span data-stu-id="77f8c-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="77f8c-125">Každý `IHostedService` implementace provádí v pořadí podle [službu registrace v ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="77f8c-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="77f8c-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) je volána v každé `IHostedService` při spuštění hostitele a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) je volána v pořadí reverzní registrace při řádné vypnutí hostitele.</span><span class="sxs-lookup"><span data-stu-id="77f8c-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="77f8c-127">Nastavení hostitele</span><span class="sxs-lookup"><span data-stu-id="77f8c-127">Set up a host</span></span>

<span data-ttu-id="77f8c-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) hlavní součást, knihovny a aplikace použít k inicializaci, vytvoření a spuštění hostitele:</span><span class="sxs-lookup"><span data-stu-id="77f8c-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="77f8c-129">Konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="77f8c-129">Host configuration</span></span>

<span data-ttu-id="77f8c-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) závisí na následujících dvou přístupů k nastavení hodnoty konfigurace hostitele:</span><span class="sxs-lookup"><span data-stu-id="77f8c-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="77f8c-131">Configuration builder</span><span class="sxs-lookup"><span data-stu-id="77f8c-131">Configuration builder</span></span>
* <span data-ttu-id="77f8c-132">Konfigurace metody rozšíření</span><span class="sxs-lookup"><span data-stu-id="77f8c-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="77f8c-133">Configuration builder</span><span class="sxs-lookup"><span data-stu-id="77f8c-133">Configuration builder</span></span>

<span data-ttu-id="77f8c-134">Konfigurace hostitele Tvůrce je vytvořen zavoláním [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementace.</span><span class="sxs-lookup"><span data-stu-id="77f8c-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="77f8c-135">`ConfigureHostConfiguration` používá [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) k vytvoření [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="77f8c-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="77f8c-136">Configuration builder inicializuje [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) pro použití v procesu sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="77f8c-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="77f8c-137">Konfigurace proměnných prostředí není přidán ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="77f8c-137">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="77f8c-138">Volání [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) na tvůrce hostitele a nakonfigurujte hostitele z proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="77f8c-138">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="77f8c-139">`AddEnvironmentVariables` přijímá volitelný uživatelsky definovanou předponu.</span><span class="sxs-lookup"><span data-stu-id="77f8c-139">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="77f8c-140">Ukázková aplikace používá předponou `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="77f8c-140">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="77f8c-141">Předpona, která se odebere, když jsou proměnné prostředí načteny.</span><span class="sxs-lookup"><span data-stu-id="77f8c-141">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="77f8c-142">Když je ukázková aplikace hostitel nakonfigurovaný, hodnotu proměnné prostředí pro `PREFIX_ENVIRONMENT` stane hodnota konfigurace hostitele `environment` klíč.</span><span class="sxs-lookup"><span data-stu-id="77f8c-142">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="77f8c-143">Během vývoje. při použití [sady Visual Studio](https://www.visualstudio.com/) nebo spuštěním aplikace s `dotnet run`, lze nastavit proměnné prostředí *Properties/launchSettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="77f8c-143">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="77f8c-144">V [Visual Studio Code](https://code.visualstudio.com/), lze nastavit proměnné prostředí *.vscode/launch.json* souboru během vývoje.</span><span class="sxs-lookup"><span data-stu-id="77f8c-144">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="77f8c-145">Další informace najdete v tématu [používání více prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="77f8c-145">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="77f8c-146">`ConfigureHostConfiguration` můžete volat vícekrát s přičítáním výsledky.</span><span class="sxs-lookup"><span data-stu-id="77f8c-146">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="77f8c-147">Hostitel používá kterékoli z těchto možností nastaví hodnotu poslední.</span><span class="sxs-lookup"><span data-stu-id="77f8c-147">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="77f8c-148">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="77f8c-148">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="77f8c-149">Příklad `HostBuilder` konfiguraci pomocí `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="77f8c-149">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="77f8c-150">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) rozšiřující metoda není aktuálně podporuje při analýze oddílu konfigurace vrácený [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (například `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="77f8c-150">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="77f8c-151">`GetSection` Metoda filtry konfigurační klíče do části Požadovaná, ale ponechá název oddílu, který na klíče (například `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="77f8c-151">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="77f8c-152">`AddConfiguration` Metoda očekává klíčů tak, aby odpovídaly `HostBuilder` klíče (například `environment`).</span><span class="sxs-lookup"><span data-stu-id="77f8c-152">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="77f8c-153">Přítomnost název oddílu, který klíčů brání hodnoty v části Konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="77f8c-153">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="77f8c-154">Tento problém bude vyřešen v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="77f8c-154">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="77f8c-155">Další informace a řešení najdete v tématu [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration využívá úplnou klíčů](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="77f8c-155">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="77f8c-156">Konfigurace metody rozšíření</span><span class="sxs-lookup"><span data-stu-id="77f8c-156">Extension method configuration</span></span>

<span data-ttu-id="77f8c-157">Metody rozšíření jsou volány v `IHostBuilder` implementace nakonfigurujte obsahu kořenové certifikáty a prostředí.</span><span class="sxs-lookup"><span data-stu-id="77f8c-157">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="77f8c-158">Obsah kořenové</span><span class="sxs-lookup"><span data-stu-id="77f8c-158">Content Root</span></span>

<span data-ttu-id="77f8c-159">Toto nastavení určuje, kde začíná hostitele vyhledávání obsahu souborů.</span><span class="sxs-lookup"><span data-stu-id="77f8c-159">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="77f8c-160">**Klíč**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="77f8c-160">**Key**: contentRoot</span></span>  
<span data-ttu-id="77f8c-161">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="77f8c-161">**Type**: *string*</span></span>  
<span data-ttu-id="77f8c-162">**Výchozí**: výchozí hodnota je složka, ve které se nachází sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="77f8c-162">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="77f8c-163">**Sada s použitím**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="77f8c-163">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="77f8c-164">**Proměnná prostředí**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` je [volitelné a uživatelem definovanými](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="77f8c-164">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="77f8c-165">Pokud cesta neexistuje, hostitel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="77f8c-165">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="77f8c-166">Prostředí</span><span class="sxs-lookup"><span data-stu-id="77f8c-166">Environment</span></span>

<span data-ttu-id="77f8c-167">Nastaví aplikace [prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="77f8c-167">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="77f8c-168">**Klíč**: prostředí</span><span class="sxs-lookup"><span data-stu-id="77f8c-168">**Key**: environment</span></span>  
<span data-ttu-id="77f8c-169">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="77f8c-169">**Type**: *string*</span></span>  
<span data-ttu-id="77f8c-170">**Výchozí**: produkčního prostředí</span><span class="sxs-lookup"><span data-stu-id="77f8c-170">**Default**: Production</span></span>  
<span data-ttu-id="77f8c-171">**Sada s použitím**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="77f8c-171">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="77f8c-172">**Proměnná prostředí**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` je [volitelné a uživatelem definovanými](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="77f8c-172">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="77f8c-173">Prostředí můžete nastavit na libovolnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="77f8c-173">The environment can be set to any value.</span></span> <span data-ttu-id="77f8c-174">Hodnoty definované v rámci rozhraní zahrnují `Development`, `Staging`, a `Production`.</span><span class="sxs-lookup"><span data-stu-id="77f8c-174">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="77f8c-175">Hodnoty se velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="77f8c-175">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="77f8c-176">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="77f8c-176">ConfigureAppConfiguration</span></span>

<span data-ttu-id="77f8c-177">Konfigurace Tvůrce aplikací je vytvořen zavoláním [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementace.</span><span class="sxs-lookup"><span data-stu-id="77f8c-177">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="77f8c-178">`ConfigureAppConfiguration` používá [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) k vytvoření [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="77f8c-178">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="77f8c-179">`ConfigureAppConfiguration` můžete volat vícekrát s přičítáním výsledky.</span><span class="sxs-lookup"><span data-stu-id="77f8c-179">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="77f8c-180">Aplikace používá, podle toho, která možnost poslední nastaví hodnotu.</span><span class="sxs-lookup"><span data-stu-id="77f8c-180">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="77f8c-181">Konfigurace vytvořil `ConfigureAppConfiguration` je k dispozici na [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) pro následné operace a v [služby](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="77f8c-181">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="77f8c-182">Příklad aplikace konfigurace pomocí `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="77f8c-182">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="77f8c-183">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="77f8c-183">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="77f8c-184">*appSettings. Development.JSON*:</span><span class="sxs-lookup"><span data-stu-id="77f8c-184">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="77f8c-185">*appSettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="77f8c-185">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="77f8c-186">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) rozšiřující metoda není aktuálně podporuje při analýze oddílu konfigurace vrácený [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (například `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="77f8c-186">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="77f8c-187">`GetSection` Metoda filtry konfigurační klíče do části Požadovaná, ale ponechá název oddílu, který na klíče (například `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="77f8c-187">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="77f8c-188">`AddConfiguration` Metoda očekává se přesně shodují s konfigurační klíče (například `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="77f8c-188">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="77f8c-189">Přítomnost název oddílu, který klíčů brání hodnoty v části Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="77f8c-189">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="77f8c-190">Tento problém bude vyřešen v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="77f8c-190">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="77f8c-191">Další informace a řešení najdete v tématu [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration využívá úplnou klíčů](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="77f8c-191">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="77f8c-192">Přesunutí souborů nastavení do výstupního adresáře, zadejte soubory nastavení jako [položky projektu nástroje MSBuild](/visualstudio/msbuild/common-msbuild-project-items) v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="77f8c-192">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="77f8c-193">Ukázková aplikace přesune své soubory JSON aplikace nastavení a *hostsettings.json* následujícím **&lt;obsahu:&gt;** položky:</span><span class="sxs-lookup"><span data-stu-id="77f8c-193">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="77f8c-194">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="77f8c-194">ConfigureServices</span></span>

<span data-ttu-id="77f8c-195">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) přidá do aplikace služeb [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru.</span><span class="sxs-lookup"><span data-stu-id="77f8c-195">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="77f8c-196">`ConfigureServices` můžete volat vícekrát s přičítáním výsledky.</span><span class="sxs-lookup"><span data-stu-id="77f8c-196">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="77f8c-197">Hostovanou službu je třída s atributem logiky úloh na pozadí, který implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="77f8c-197">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="77f8c-198">Další informace najdete v tématu [s hostovanými službami úloh na pozadí](xref:fundamentals/host/hosted-services) tématu.</span><span class="sxs-lookup"><span data-stu-id="77f8c-198">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="77f8c-199">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) používá `AddHostedService` metodu rozšíření k přidání služby pro události doby života `LifetimeEventsHostedService`a úlohu na pozadí vypršel časový limit `TimedHostedService`, do aplikace:</span><span class="sxs-lookup"><span data-stu-id="77f8c-199">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="77f8c-200">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="77f8c-200">ConfigureLogging</span></span>

<span data-ttu-id="77f8c-201">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) přidá delegáta pro konfiguraci zadaných [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span><span class="sxs-lookup"><span data-stu-id="77f8c-201">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="77f8c-202">`ConfigureLogging` může být volána více než jednou s přičítáním výsledky.</span><span class="sxs-lookup"><span data-stu-id="77f8c-202">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="77f8c-203">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="77f8c-203">UseConsoleLifetime</span></span>

<span data-ttu-id="77f8c-204">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) naslouchá `Ctrl+C`/SIGINT nebo SIGTERM a volání [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) zahájíte proces vypnutí.</span><span class="sxs-lookup"><span data-stu-id="77f8c-204">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="77f8c-205">`UseConsoleLifetime` odblokuje rozšíření, jako [RunAsync](#runasync) a [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="77f8c-205">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="77f8c-206">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) předem registrován jako výchozí implementace životnost.</span><span class="sxs-lookup"><span data-stu-id="77f8c-206">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="77f8c-207">Poslední doba života zaregistrovaný se používá.</span><span class="sxs-lookup"><span data-stu-id="77f8c-207">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="77f8c-208">Konfigurace kontejneru</span><span class="sxs-lookup"><span data-stu-id="77f8c-208">Container configuration</span></span>

<span data-ttu-id="77f8c-209">Pro podporu připojení v ostatních kontejnerech, může přijmout hostitele [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="77f8c-209">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="77f8c-210">Poskytuje objekt pro vytváření, které nejsou součástí registrace kontejnerů DI, ale místo toho je vnitřní hostitel používá k vytvoření kontejneru pro konkrétní DI.</span><span class="sxs-lookup"><span data-stu-id="77f8c-210">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="77f8c-211">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) přepisuje výchozí objekt pro vytváření použitý k vytvoření poskytovatele služby app service.</span><span class="sxs-lookup"><span data-stu-id="77f8c-211">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="77f8c-212">Konfigurace vlastního kontejneru je spravován [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) metody.</span><span class="sxs-lookup"><span data-stu-id="77f8c-212">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="77f8c-213">`ConfigureContainer` poskytuje možnosti silného typu pro konfiguraci kontejneru nad základního hostitele rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="77f8c-213">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="77f8c-214">`ConfigureContainer` můžete volat vícekrát s přičítáním výsledky.</span><span class="sxs-lookup"><span data-stu-id="77f8c-214">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="77f8c-215">Vytvoření služby kontejneru pro aplikace:</span><span class="sxs-lookup"><span data-stu-id="77f8c-215">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="77f8c-216">Poskytuje objekt pro vytváření služby kontejneru:</span><span class="sxs-lookup"><span data-stu-id="77f8c-216">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="77f8c-217">Použijte objekt pro vytváření a konfigurace kontejneru vlastní služby pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="77f8c-217">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="77f8c-218">Rozšiřitelnost</span><span class="sxs-lookup"><span data-stu-id="77f8c-218">Extensibility</span></span>

<span data-ttu-id="77f8c-219">Rozšiřitelnost hostitele se provádí pomocí metody rozšíření na `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="77f8c-219">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="77f8c-220">Následující příklad ukazuje, jak rozšiřující metoda rozšiřuje `IHostBuilder` implementaci [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="77f8c-220">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="77f8c-221">Metoda rozšíření (jinde v aplikaci) zaregistruje RabbitMQ `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="77f8c-221">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="77f8c-222">Spravovat hostitele</span><span class="sxs-lookup"><span data-stu-id="77f8c-222">Manage the host</span></span>

<span data-ttu-id="77f8c-223">[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementace je zodpovědný za spouštění a zastavování `IHostedService` implementace, které jsou registrovány v kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="77f8c-223">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="77f8c-224">Spustit</span><span class="sxs-lookup"><span data-stu-id="77f8c-224">Run</span></span>

<span data-ttu-id="77f8c-225">[Spustit](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) aplikace spouští a blokuje volající vlákno, dokud nebude ukončen hostitele:</span><span class="sxs-lookup"><span data-stu-id="77f8c-225">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="77f8c-226">RunAsync</span><span class="sxs-lookup"><span data-stu-id="77f8c-226">RunAsync</span></span>

<span data-ttu-id="77f8c-227">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) aplikace spouští a vrátí `Task` , která se dokončí, když se aktivuje token zrušení nebo vypnutí:</span><span class="sxs-lookup"><span data-stu-id="77f8c-227">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="77f8c-228">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="77f8c-228">RunConsoleAsync</span></span>

<span data-ttu-id="77f8c-229">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) umožňuje podporu konzoly, sestaví a spustí hostitele a čeká na `Ctrl+C`/SIGINT nebo SIGTERM vypnout.</span><span class="sxs-lookup"><span data-stu-id="77f8c-229">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="77f8c-230">Počáteční a StopAsync</span><span class="sxs-lookup"><span data-stu-id="77f8c-230">Start and StopAsync</span></span>

<span data-ttu-id="77f8c-231">[Spustit](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) spustí hostitele synchronně.</span><span class="sxs-lookup"><span data-stu-id="77f8c-231">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="77f8c-232">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) limitu pokusí zastavit hostitele v rámci zadaného časového limitu.</span><span class="sxs-lookup"><span data-stu-id="77f8c-232">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="77f8c-233">StartAsync a StopAsync</span><span class="sxs-lookup"><span data-stu-id="77f8c-233">StartAsync and StopAsync</span></span>

<span data-ttu-id="77f8c-234">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) spustí aplikaci.</span><span class="sxs-lookup"><span data-stu-id="77f8c-234">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="77f8c-235">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) ukončí aplikaci.</span><span class="sxs-lookup"><span data-stu-id="77f8c-235">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="77f8c-236">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="77f8c-236">WaitForShutdown</span></span>

<span data-ttu-id="77f8c-237">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) se spouští přes [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), jako například [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (čeká na `Ctrl+C`/SIGINT nebo SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="77f8c-237">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="77f8c-238">`WaitForShutdown` volání [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="77f8c-238">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="77f8c-239">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="77f8c-239">WaitForShutdownAsync</span></span>

<span data-ttu-id="77f8c-240">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) vrátí `Task` , která se dokončí při vypnutí se spouští přes daný token a volání [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="77f8c-240">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="77f8c-241">Vnější ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="77f8c-241">External control</span></span>

<span data-ttu-id="77f8c-242">Vnější ovládací prvek hostitele lze dosáhnout pomocí metody, které je možné volat externě:</span><span class="sxs-lookup"><span data-stu-id="77f8c-242">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="77f8c-243">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) je volána na začátku [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), která čeká, dokud neskončí než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="77f8c-243">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="77f8c-244">To umožňuje zpoždění spuštění, dokud signalizován externí událostí.</span><span class="sxs-lookup"><span data-stu-id="77f8c-244">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="77f8c-245">IHostingEnvironment rozhraní</span><span class="sxs-lookup"><span data-stu-id="77f8c-245">IHostingEnvironment interface</span></span>

<span data-ttu-id="77f8c-246">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) poskytuje informace o aplikaci prvku hostitelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="77f8c-246">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="77f8c-247">Použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:</span><span class="sxs-lookup"><span data-stu-id="77f8c-247">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="77f8c-248">Další informace najdete v tématu [používání více prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="77f8c-248">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="77f8c-249">IApplicationLifetime rozhraní</span><span class="sxs-lookup"><span data-stu-id="77f8c-249">IApplicationLifetime interface</span></span>

<span data-ttu-id="77f8c-250">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) umožňuje po spuštění a vypnutí aktivity, včetně žádostí o řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="77f8c-250">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="77f8c-251">Tři vlastnosti v rozhraní jsou tokeny zrušení použije k registraci `Action` metody, které definují události spuštění a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="77f8c-251">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="77f8c-252">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="77f8c-252">Cancellation Token</span></span> | <span data-ttu-id="77f8c-253">Při aktivaci&#8230;</span><span class="sxs-lookup"><span data-stu-id="77f8c-253">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="77f8c-254">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="77f8c-254">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="77f8c-255">Hostitel plně byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="77f8c-255">The host has fully started.</span></span> |
| [<span data-ttu-id="77f8c-256">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="77f8c-256">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="77f8c-257">Hostitel se dokončuje řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="77f8c-257">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="77f8c-258">By měl zpracovat všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="77f8c-258">All requests should be processed.</span></span> <span data-ttu-id="77f8c-259">Vypnutí blokuje, dokud se tato událost se dokončí.</span><span class="sxs-lookup"><span data-stu-id="77f8c-259">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="77f8c-260">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="77f8c-260">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="77f8c-261">Hostitel provádí řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="77f8c-261">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="77f8c-262">Žádosti se možná ještě zpracovávají.</span><span class="sxs-lookup"><span data-stu-id="77f8c-262">Requests may still be processing.</span></span> <span data-ttu-id="77f8c-263">Vypnutí blokuje, dokud se tato událost se dokončí.</span><span class="sxs-lookup"><span data-stu-id="77f8c-263">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="77f8c-264">Vložit konstruktoru `IApplicationLifetime` service do libovolné třídy.</span><span class="sxs-lookup"><span data-stu-id="77f8c-264">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="77f8c-265">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) používá konstruktor injektáž do `LifetimeEventsHostedService` třídy ( `IHostedService` implementace) k registraci události.</span><span class="sxs-lookup"><span data-stu-id="77f8c-265">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="77f8c-266">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="77f8c-266">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="77f8c-267">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) požaduje ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="77f8c-267">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="77f8c-268">Následující třídy používá `StopApplication` řádně vypnout aplikaci při třídy `Shutdown` volání metody:</span><span class="sxs-lookup"><span data-stu-id="77f8c-268">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="77f8c-269">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="77f8c-269">Additional resources</span></span>

* [<span data-ttu-id="77f8c-270">Úlohy na pozadí s hostovanými službami</span><span class="sxs-lookup"><span data-stu-id="77f8c-270">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="77f8c-271">Hostování úložiště ukázek na Githubu</span><span class="sxs-lookup"><span data-stu-id="77f8c-271">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
