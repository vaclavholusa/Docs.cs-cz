---
title: Obecné hostitele rozhraní .NET
author: guardrex
description: Další informace o obecné hostitele v rozhraní .NET, která je zodpovědná za spuštění a životního cyklu správy aplikací.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/generic-host
ms.openlocfilehash: a851f2faf13792b2c232c124371d07710ae1fce3
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734468"
---
# <a name="net-generic-host"></a><span data-ttu-id="a0b34-103">Obecné hostitele rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="a0b34-103">.NET Generic Host</span></span>

<span data-ttu-id="a0b34-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a0b34-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a0b34-105">Konfigurace aplikace .NET a spusťte *hostitele*.</span><span class="sxs-lookup"><span data-stu-id="a0b34-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="a0b34-106">Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="a0b34-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="a0b34-107">Toto téma obsahuje obecné hostitele ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), což je užitečné pro hostování aplikací, které nejsou zpracovávají požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0b34-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="a0b34-108">Pro pokrytí webového hostitele ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), najdete v článku [webového hostitele](xref:fundamentals/host/web-host) tématu.</span><span class="sxs-lookup"><span data-stu-id="a0b34-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="a0b34-109">Cílem obecné hostitele je oddělit kanál protokolu HTTP z hostitele webového rozhraní API umožňující širší škálu scénářů hostitele.</span><span class="sxs-lookup"><span data-stu-id="a0b34-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="a0b34-110">Zasílání zpráv, úlohy na pozadí a další úlohy jiným protokolem než HTTP podle výhody obecné hostitele z vyjímání mezi funkce, jako je například konfigurace, vkládání závislostí (DI) a protokolování.</span><span class="sxs-lookup"><span data-stu-id="a0b34-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="a0b34-111">Obecné hostitele je nového v technologii ASP.NET Core 2.1 a není vhodná pro webového hostingu scénáře.</span><span class="sxs-lookup"><span data-stu-id="a0b34-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="a0b34-112">Pro scénáře hostování, web, použijte [webového hostitele](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="a0b34-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="a0b34-113">Obecné hostitel je ve vývoji a nahraďte webového hostitele v budoucí verzi fungovat jako hostitel primární rozhraní API v protokolu HTTP a scénáře jiným protokolem než HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0b34-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="a0b34-114">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a0b34-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a0b34-115">Při spuštění ukázkové aplikace [Visual Studio Code](https://code.visualstudio.com/), použijte *externí nebo integrované Terminálové*.</span><span class="sxs-lookup"><span data-stu-id="a0b34-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="a0b34-116">Nelze spustit ukázku v `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="a0b34-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="a0b34-117">Nastavení konzole ve Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="a0b34-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="a0b34-118">Otevřete *.vscode/launch.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="a0b34-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="a0b34-119">V **.NET Core spuštění (konzola)** konfigurace, vyhledejte **konzoly** položku.</span><span class="sxs-lookup"><span data-stu-id="a0b34-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="a0b34-120">Nastavte hodnotu na buď `externalTerminal` nebo `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="a0b34-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="a0b34-121">Úvod</span><span class="sxs-lookup"><span data-stu-id="a0b34-121">Introduction</span></span>

<span data-ttu-id="a0b34-122">Je k dispozici v knihovně obecné hostitele [obor názvů Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) a poskytované [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="a0b34-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="a0b34-123">`Microsoft.Extensions.Hosting` Je součástí balíčku [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="a0b34-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="a0b34-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) se vstupním bodem k provádění kódu.</span><span class="sxs-lookup"><span data-stu-id="a0b34-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="a0b34-125">Každý `IHostedService` implementace je provést v pořadí podle [služby registrace v ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="a0b34-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="a0b34-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) je volána v každém `IHostedService` po spuštění hostitele a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) je volána v pořadí zpětné registrace, když řádně vypne hostitele.</span><span class="sxs-lookup"><span data-stu-id="a0b34-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="a0b34-127">Nastavení hostitele</span><span class="sxs-lookup"><span data-stu-id="a0b34-127">Set up a host</span></span>

<span data-ttu-id="a0b34-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) hlavní součást, knihovny a aplikace použít k inicializaci, vytvoření a spuštění hostitele:</span><span class="sxs-lookup"><span data-stu-id="a0b34-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="a0b34-129">Konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="a0b34-129">Host configuration</span></span>

<span data-ttu-id="a0b34-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) závisí na následujících dvou přístupů k nastavení hostitelské hodnoty konfigurace:</span><span class="sxs-lookup"><span data-stu-id="a0b34-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="a0b34-131">Tvůrce konfigurace</span><span class="sxs-lookup"><span data-stu-id="a0b34-131">Configuration builder</span></span>
* <span data-ttu-id="a0b34-132">Konfigurace metody rozšíření</span><span class="sxs-lookup"><span data-stu-id="a0b34-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="a0b34-133">Tvůrce konfigurace</span><span class="sxs-lookup"><span data-stu-id="a0b34-133">Configuration builder</span></span>

<span data-ttu-id="a0b34-134">Konfigurace hostitele tvůrce vytvoří volání [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementace.</span><span class="sxs-lookup"><span data-stu-id="a0b34-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="a0b34-135">`ConfigureHostConfiguration` používá [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) vytvořit [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="a0b34-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="a0b34-136">Inicializuje Tvůrce konfigurace [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) pro použití v procesu sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0b34-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span> <span data-ttu-id="a0b34-137">`ConfigureHostConfiguration` je možné volat vícekrát s sčítání výsledky.</span><span class="sxs-lookup"><span data-stu-id="a0b34-137">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="a0b34-138">Hostitel používá. obě tyto možnosti nastaví hodnotu poslední.</span><span class="sxs-lookup"><span data-stu-id="a0b34-138">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="a0b34-139">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="a0b34-139">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="a0b34-140">Příklad `HostBuilder` konfigurace pomocí `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="a0b34-140">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="a0b34-141">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) metoda rozšíření není aktuálně schopen analyzovat konfigurační oddíl vrácený [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (například `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="a0b34-141">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="a0b34-142">`GetSection` Metoda filtry konfigurace klíče do části požadovaný, ale ponechá název oddílu na klíče (například `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="a0b34-142">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="a0b34-143">`AddConfiguration` Metoda očekává klíče tak, aby odpovídala `HostBuilder` klíče (například `environment`).</span><span class="sxs-lookup"><span data-stu-id="a0b34-143">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="a0b34-144">Přítomnost názvu oddílu na klíče zabrání hodnoty v části Konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="a0b34-144">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="a0b34-145">Tento problém bude vyřešen v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="a0b34-145">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="a0b34-146">Další informace a řešení, najdete v části [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration používá úplné klíče](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="a0b34-146">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="a0b34-147">Konfigurace metody rozšíření</span><span class="sxs-lookup"><span data-stu-id="a0b34-147">Extension method configuration</span></span>

<span data-ttu-id="a0b34-148">Rozšiřující metody jsou volány na `IHostBuilder` implementace konfigurace kořenu obsahu a prostředí.</span><span class="sxs-lookup"><span data-stu-id="a0b34-148">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="a0b34-149">Obsah kořenové</span><span class="sxs-lookup"><span data-stu-id="a0b34-149">Content Root</span></span>

<span data-ttu-id="a0b34-150">Toto nastavení určuje, kde začíná hostitele hledání obsahu souborů.</span><span class="sxs-lookup"><span data-stu-id="a0b34-150">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="a0b34-151">**Klíč**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="a0b34-151">**Key**: contentRoot</span></span>  
<span data-ttu-id="a0b34-152">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="a0b34-152">**Type**: *string*</span></span>  
<span data-ttu-id="a0b34-153">**Výchozí**: výchozí složce, kde se nachází sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0b34-153">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="a0b34-154">**Nastavit pomocí**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="a0b34-154">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="a0b34-155">**Proměnné prostředí**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="a0b34-155">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="a0b34-156">Pokud cesta neexistuje, hostitel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="a0b34-156">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="a0b34-157">Prostředí</span><span class="sxs-lookup"><span data-stu-id="a0b34-157">Environment</span></span>

<span data-ttu-id="a0b34-158">Nastaví aplikace [prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a0b34-158">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="a0b34-159">**Klíč**: prostředí</span><span class="sxs-lookup"><span data-stu-id="a0b34-159">**Key**: environment</span></span>  
<span data-ttu-id="a0b34-160">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="a0b34-160">**Type**: *string*</span></span>  
<span data-ttu-id="a0b34-161">**Výchozí**: produkční</span><span class="sxs-lookup"><span data-stu-id="a0b34-161">**Default**: Production</span></span>  
<span data-ttu-id="a0b34-162">**Nastavit pomocí**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="a0b34-162">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="a0b34-163">**Proměnné prostředí**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="a0b34-163">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="a0b34-164">Prostředí můžete nastavit na jakoukoli hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a0b34-164">The environment can be set to any value.</span></span> <span data-ttu-id="a0b34-165">Framework definované hodnoty zahrnují `Development`, `Staging`, a `Production`.</span><span class="sxs-lookup"><span data-stu-id="a0b34-165">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="a0b34-166">Hodnoty nejsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="a0b34-166">Values aren't case sensitive.</span></span> <span data-ttu-id="a0b34-167">Ve výchozím nastavení *prostředí* je pro čtení z `ASPNETCORE_ENVIRONMENT` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="a0b34-167">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="a0b34-168">Při použití [Visual Studio](https://www.visualstudio.com/), proměnné prostředí může být nastavena v *launchSettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="a0b34-168">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="a0b34-169">Další informace najdete v tématu [použijte prostředí s více](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a0b34-169">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="a0b34-170">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="a0b34-170">ConfigureAppConfiguration</span></span>

<span data-ttu-id="a0b34-171">Konfigurace Tvůrce aplikací je vytvořená voláním [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementace.</span><span class="sxs-lookup"><span data-stu-id="a0b34-171">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="a0b34-172">`ConfigureAppConfiguration` používá [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) vytvořit [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a0b34-172">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="a0b34-173">`ConfigureAppConfiguration` je možné volat vícekrát s sčítání výsledky.</span><span class="sxs-lookup"><span data-stu-id="a0b34-173">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="a0b34-174">Aplikace používá, podle toho, která možnost nastaví hodnotu poslední.</span><span class="sxs-lookup"><span data-stu-id="a0b34-174">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="a0b34-175">Konfigurace vytvořených `ConfigureAppConfiguration` je k dispozici na [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) pro následující operace a v [služby](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="a0b34-175">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="a0b34-176">Příklad aplikace konfigurace pomocí `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="a0b34-176">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="a0b34-177">*appSettings.JSON určený*:</span><span class="sxs-lookup"><span data-stu-id="a0b34-177">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="a0b34-178">*appSettings. Development.JSON*:</span><span class="sxs-lookup"><span data-stu-id="a0b34-178">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="a0b34-179">*appSettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="a0b34-179">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="a0b34-180">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) metoda rozšíření není aktuálně schopen analyzovat konfigurační oddíl vrácený [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (například `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="a0b34-180">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="a0b34-181">`GetSection` Metoda filtry konfigurace klíče do části požadovaný, ale ponechá název oddílu na klíče (například `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="a0b34-181">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="a0b34-182">`AddConfiguration` Metoda očekává se přesně shodují s konfigurace klíče (například `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="a0b34-182">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="a0b34-183">Přítomnost názvu oddílu na klíče zabrání hodnoty v části Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0b34-183">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="a0b34-184">Tento problém bude vyřešen v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="a0b34-184">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="a0b34-185">Další informace a řešení, najdete v části [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration používá úplné klíče](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="a0b34-185">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

## <a name="configureservices"></a><span data-ttu-id="a0b34-186">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="a0b34-186">ConfigureServices</span></span>

<span data-ttu-id="a0b34-187">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) přidá do aplikace služeb [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a0b34-187">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="a0b34-188">`ConfigureServices` je možné volat vícekrát s sčítání výsledky.</span><span class="sxs-lookup"><span data-stu-id="a0b34-188">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="a0b34-189">Hostovaná služba je třída s pozadí úloh logiky, která implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a0b34-189">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="a0b34-190">Další informace najdete v tématu [pozadí úlohy s hostované služby](xref:fundamentals/host/hosted-services) tématu.</span><span class="sxs-lookup"><span data-stu-id="a0b34-190">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="a0b34-191">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) používá `AddHostedService` rozšíření metoda pro přidání služby pro události životního cyklu `LifetimeEventsHostedService`a úlohy na pozadí vypršel `TimedHostedService`, do aplikace:</span><span class="sxs-lookup"><span data-stu-id="a0b34-191">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="a0b34-192">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="a0b34-192">ConfigureLogging</span></span>

<span data-ttu-id="a0b34-193">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) přidá delegáta pro konfiguraci poskytnutého [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span><span class="sxs-lookup"><span data-stu-id="a0b34-193">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="a0b34-194">`ConfigureLogging` může volat vícekrát s sčítání výsledky.</span><span class="sxs-lookup"><span data-stu-id="a0b34-194">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="a0b34-195">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="a0b34-195">UseConsoleLifetime</span></span>

<span data-ttu-id="a0b34-196">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) naslouchá `Ctrl+C`/SIGINT nebo SIGTERM – a volání [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) ke spuštění procesu vypnutí.</span><span class="sxs-lookup"><span data-stu-id="a0b34-196">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="a0b34-197">`UseConsoleLifetime` například odblokuje rozšíření [RunAsync](#runasync) a [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="a0b34-197">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="a0b34-198">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) je předem zaregistrovaný jako výchozí implementace životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="a0b34-198">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="a0b34-199">Poslední doba platnosti zaregistrovat se používá.</span><span class="sxs-lookup"><span data-stu-id="a0b34-199">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="a0b34-200">Konfiguraci kontejneru</span><span class="sxs-lookup"><span data-stu-id="a0b34-200">Container configuration</span></span>

<span data-ttu-id="a0b34-201">Pro podporu zapojení do jiných kontejnerů, může přijmout hostitele [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="a0b34-201">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="a0b34-202">Poskytuje objekt pro vytváření není součástí registrace kontejneru DI, ale místo toho je vnitřní hostitele použít k vytvoření kontejneru konkrétní DI.</span><span class="sxs-lookup"><span data-stu-id="a0b34-202">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="a0b34-203">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) přepíše výchozí objekt pro vytváření použitý k vytvoření aplikace poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="a0b34-203">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="a0b34-204">Spravuje konfiguraci vlastní kontejneru [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) metoda.</span><span class="sxs-lookup"><span data-stu-id="a0b34-204">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="a0b34-205">`ConfigureContainer` pro konfiguraci kontejneru nad základního hostitele rozhraní API, poskytuje možnosti silného typu.</span><span class="sxs-lookup"><span data-stu-id="a0b34-205">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="a0b34-206">`ConfigureContainer` je možné volat vícekrát s sčítání výsledky.</span><span class="sxs-lookup"><span data-stu-id="a0b34-206">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="a0b34-207">Vytvoření kontejneru služby pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="a0b34-207">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="a0b34-208">Zadejte objekt kontejneru služby:</span><span class="sxs-lookup"><span data-stu-id="a0b34-208">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="a0b34-209">Použití objektu pro vytváření a konfiguraci kontejneru vlastní služby pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="a0b34-209">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="a0b34-210">Rozšiřitelnost</span><span class="sxs-lookup"><span data-stu-id="a0b34-210">Extensibility</span></span>

<span data-ttu-id="a0b34-211">Rozšíření hostitele se provádí pomocí metody rozšíření na `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a0b34-211">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="a0b34-212">Následující příklad ukazuje, jak rozšiřuje metodu rozšíření `IHostBuilder` implementace s [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="a0b34-212">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="a0b34-213">Registr – metoda rozšíření (jinde v aplikaci) RabbitMQ `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a0b34-213">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="a0b34-214">Spravovat hostitele</span><span class="sxs-lookup"><span data-stu-id="a0b34-214">Manage the host</span></span>

<span data-ttu-id="a0b34-215">[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementace je zodpovědná za spuštění a zastavení `IHostedService` implementace, které jsou zaregistrovány v kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="a0b34-215">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="a0b34-216">Spustit</span><span class="sxs-lookup"><span data-stu-id="a0b34-216">Run</span></span>

<span data-ttu-id="a0b34-217">[Spustit](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) aplikace spouští a blokuje volající vlákno, dokud se vypne hostitele:</span><span class="sxs-lookup"><span data-stu-id="a0b34-217">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="a0b34-218">RunAsync</span><span class="sxs-lookup"><span data-stu-id="a0b34-218">RunAsync</span></span>

<span data-ttu-id="a0b34-219">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) aplikace spouští a vrátí `Task` která se dokončí při aktivaci token zrušení nebo ukončení:</span><span class="sxs-lookup"><span data-stu-id="a0b34-219">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="a0b34-220">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="a0b34-220">RunConsoleAsync</span></span>

<span data-ttu-id="a0b34-221">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) umožňuje podporu konzoly, vytvoří a spustí hostitele a čeká na `Ctrl+C`/SIGINT nebo SIGTERM – vypnout.</span><span class="sxs-lookup"><span data-stu-id="a0b34-221">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="a0b34-222">Počáteční a StopAsync</span><span class="sxs-lookup"><span data-stu-id="a0b34-222">Start and StopAsync</span></span>

<span data-ttu-id="a0b34-223">[Spustit](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) spustí hostitele synchronně.</span><span class="sxs-lookup"><span data-stu-id="a0b34-223">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="a0b34-224">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) pokusí zastavit hostitele během zadaného časového limitu.</span><span class="sxs-lookup"><span data-stu-id="a0b34-224">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="a0b34-225">StartAsync a StopAsync</span><span class="sxs-lookup"><span data-stu-id="a0b34-225">StartAsync and StopAsync</span></span>

<span data-ttu-id="a0b34-226">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) se spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0b34-226">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="a0b34-227">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) zastaví aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0b34-227">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="a0b34-228">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="a0b34-228">WaitForShutdown</span></span>

<span data-ttu-id="a0b34-229">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) aktivaci prostřednictvím [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), jako například [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (čeká na `Ctrl+C`/SIGINT nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="a0b34-229">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="a0b34-230">`WaitForShutdown` volání [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="a0b34-230">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="a0b34-231">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="a0b34-231">WaitForShutdownAsync</span></span>

<span data-ttu-id="a0b34-232">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) vrátí `Task` která se dokončí při vypnutí aktivaci prostřednictvím daný token a volání [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="a0b34-232">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="a0b34-233">Externí ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="a0b34-233">External control</span></span>

<span data-ttu-id="a0b34-234">Externí řízení hostitele lze dosáhnout pomocí metod, které je možné volat externě:</span><span class="sxs-lookup"><span data-stu-id="a0b34-234">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="a0b34-235">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) nazývá na začátku [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), který bude čekat, dokud nebude dokončeno než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="a0b34-235">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="a0b34-236">Tímto lze zpoždění spuštění, dokud signalizovala externí událostí.</span><span class="sxs-lookup"><span data-stu-id="a0b34-236">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="a0b34-237">IHostingEnvironment rozhraní</span><span class="sxs-lookup"><span data-stu-id="a0b34-237">IHostingEnvironment interface</span></span>

<span data-ttu-id="a0b34-238">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) poskytuje informace o aplikaci hostitelského prostředí.</span><span class="sxs-lookup"><span data-stu-id="a0b34-238">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="a0b34-239">Použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:</span><span class="sxs-lookup"><span data-stu-id="a0b34-239">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="a0b34-240">Další informace najdete v tématu [použijte prostředí s více](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a0b34-240">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="a0b34-241">IApplicationLifetime rozhraní</span><span class="sxs-lookup"><span data-stu-id="a0b34-241">IApplicationLifetime interface</span></span>

<span data-ttu-id="a0b34-242">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) umožňuje po spuštění a vypnutí aktivity, včetně žádostí řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="a0b34-242">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="a0b34-243">Zrušení tokenů použitá pro zaregistrování jsou tři vlastnosti na rozhraní `Action` metody, které definují události spuštění a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="a0b34-243">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="a0b34-244">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="a0b34-244">Cancellation Token</span></span> | <span data-ttu-id="a0b34-245">Při aktivaci&#8230;</span><span class="sxs-lookup"><span data-stu-id="a0b34-245">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="a0b34-246">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="a0b34-246">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="a0b34-247">Hostitele plně byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="a0b34-247">The host has fully started.</span></span> |
| [<span data-ttu-id="a0b34-248">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="a0b34-248">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="a0b34-249">Hostitel je dokončení řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="a0b34-249">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="a0b34-250">Všechny požadavky, měla by být zpracována.</span><span class="sxs-lookup"><span data-stu-id="a0b34-250">All requests should be processed.</span></span> <span data-ttu-id="a0b34-251">Vypnutí bloky až po dokončení této události.</span><span class="sxs-lookup"><span data-stu-id="a0b34-251">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="a0b34-252">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="a0b34-252">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="a0b34-253">Hostitel provádí řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="a0b34-253">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="a0b34-254">Může být stále aktivní žádosti.</span><span class="sxs-lookup"><span data-stu-id="a0b34-254">Requests may still be processing.</span></span> <span data-ttu-id="a0b34-255">Vypnutí bloky až po dokončení této události.</span><span class="sxs-lookup"><span data-stu-id="a0b34-255">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="a0b34-256">Konstruktor vložit `IApplicationLifetime` služby do jakékoli třídy.</span><span class="sxs-lookup"><span data-stu-id="a0b34-256">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="a0b34-257">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) používá contructor vkládání do `LifetimeEventsHostedService` – třída ( `IHostedService` implementace) k registraci události.</span><span class="sxs-lookup"><span data-stu-id="a0b34-257">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses contructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="a0b34-258">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="a0b34-258">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="a0b34-259">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) požadavky ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0b34-259">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="a0b34-260">Následující třídy používá `StopApplication` korektně vypnout aplikaci při třídy `Shutdown` metoda je volána:</span><span class="sxs-lookup"><span data-stu-id="a0b34-260">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="a0b34-261">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a0b34-261">Additional resources</span></span>

* [<span data-ttu-id="a0b34-262">Úlohy na pozadí s hostovanými službami</span><span class="sxs-lookup"><span data-stu-id="a0b34-262">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="a0b34-263">Hostování ukázky úložišti na Githubu</span><span class="sxs-lookup"><span data-stu-id="a0b34-263">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
