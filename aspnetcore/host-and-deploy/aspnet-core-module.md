---
title: Referenční informace o ASP.NET Core modulu Konfigurace
author: guardrex
description: Zjistěte, jak nakonfigurovat modul ASP.NET Core pro hostování aplikací ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 0d167f779f9dcae6b0d946dce5e341793daf43bf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50091012"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="3e4df-103">Referenční informace o ASP.NET Core modulu Konfigurace</span><span class="sxs-lookup"><span data-stu-id="3e4df-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="3e4df-104">Podle [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), a [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="3e4df-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="3e4df-105">Tento dokument obsahuje pokyny o tom, jak nakonfigurovat modul ASP.NET Core pro hostování aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3e4df-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="3e4df-106">Úvod do modul ASP.NET Core a pokyny k instalaci, najdete v článku [modul ASP.NET Core přehled](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3e4df-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="3e4df-107">Model hostingu</span><span class="sxs-lookup"><span data-stu-id="3e4df-107">Hosting model</span></span>

<span data-ttu-id="3e4df-108">Modul pro aplikace běžící na .NET Core 2.2 nebo vyšší, podporuje model hostování v procesu za účelem vylepšení výkonu v porovnání s hostitelem (out-of-process) reverzních proxy serverů.</span><span class="sxs-lookup"><span data-stu-id="3e4df-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="3e4df-109">Další informace naleznete v tématu <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="3e4df-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="3e4df-110">Hostování v procsess je vyjádřit výslovný souhlas pro existující aplikace, ale [dotnet nové](/dotnet/core/tools/dotnet-new) výchozí šablony na model hostingu v procesu pro všechny služby IIS a služby IIS Express.</span><span class="sxs-lookup"><span data-stu-id="3e4df-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="3e4df-111">Jak nakonfigurovat aplikaci pro hostování v procesu, přidejte `<AspNetCoreModuleHostingModel>` vlastnosti do souboru projektu vaší aplikace s hodnotou `inprocess` (hostování mimo proces se nastaví pomocí `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="3e4df-111">To configure an app for in-process hosting, add the `<AspNetCoreModuleHostingModel>` property to the app's project file with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreModuleHostingModel>inprocess</AspNetCoreModuleHostingModel>
</PropertyGroup>
```

<span data-ttu-id="3e4df-112">Při hostování v procesu platí následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="3e4df-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="3e4df-113">[Kestrel server](xref:fundamentals/servers/kestrel) se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="3e4df-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="3e4df-114">Vlastní <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementaci `IISHttpServer` funguje jako server aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e4df-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="3e4df-115">[RequestTimeout atribut](#attributes-of-the-aspnetcore-element) neplatí pro hostování v procesu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="3e4df-116">Sdílení fondem aplikací mezi aplikacemi se nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="3e4df-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="3e4df-117">Použijte jeden fond aplikací na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3e4df-117">Use one app pool per app.</span></span>

* <span data-ttu-id="3e4df-118">Při použití [Webdeploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) nebo ručně uvedení [soubor app_offline.htm v nasazení](xref:host-and-deploy/iis/index#locked-deployment-files), aplikace nemusí vypnout okamžitě při otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="3e4df-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="3e4df-119">Připojení soketu websocket bylo třeba může způsobit prodlevu při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e4df-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="3e4df-120">Architektura (bitové verze) (x64 nebo x86) nainstalovaný modul runtime a aplikace musí odpovídat architektuře fondu aplikací.</span><span class="sxs-lookup"><span data-stu-id="3e4df-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="3e4df-121">Pokud nastavení aplikace hostitele ručně pomocí `WebHostBuilder` (bez použití [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) a spuštění aplikace se nikdy přímo na serveru Kestrel (v místním prostředí), volání `UseKestrel` před voláním `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="3e4df-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="3e4df-122">Pokud je obrácený pořadí, hostitel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="3e4df-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="3e4df-123">Odpojení klienta jsou zjištěny.</span><span class="sxs-lookup"><span data-stu-id="3e4df-123">Client disconnects are detected.</span></span> <span data-ttu-id="3e4df-124">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) token zrušení se zrušila, když se klient odpojí.</span><span class="sxs-lookup"><span data-stu-id="3e4df-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="3e4df-125">Hostování změny modelu</span><span class="sxs-lookup"><span data-stu-id="3e4df-125">Hosting model changes</span></span>

<span data-ttu-id="3e4df-126">Pokud `hostingModel` nastavení se změnilo v *web.config* souboru (podrobně [konfigurace pomocí souboru web.config](#configuration-with-webconfig) části), modul pro službu IIS recykluje pracovní proces.</span><span class="sxs-lookup"><span data-stu-id="3e4df-126">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="3e4df-127">Pro službu IIS Express modul nebude recyklovat pracovní proces, ale místo toho aktivuje řádné vypnutí aktuální proces IIS Express.</span><span class="sxs-lookup"><span data-stu-id="3e4df-127">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="3e4df-128">Další požadavek na aplikaci vytvoří nový proces IIS Express.</span><span class="sxs-lookup"><span data-stu-id="3e4df-128">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="3e4df-129">Název procesu</span><span class="sxs-lookup"><span data-stu-id="3e4df-129">Process name</span></span>

<span data-ttu-id="3e4df-130">`Process.GetCurrentProcess().ProcessName` sestavy buď `w3wp` (v procesu) nebo `dotnet` (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="3e4df-130">`Process.GetCurrentProcess().ProcessName` reports either `w3wp` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="3e4df-131">Konfigurace pomocí souboru web.config</span><span class="sxs-lookup"><span data-stu-id="3e4df-131">Configuration with web.config</span></span>

<span data-ttu-id="3e4df-132">Modul ASP.NET Core, nastavena `aspNetCore` část `system.webServer` uzlu na webu *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="3e4df-132">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="3e4df-133">Následující *web.config* soubor je publikován pro [nasazení závisí na architektuře](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) a konfiguruje modul ASP.NET Core pro zpracování požadavků lokality:</span><span class="sxs-lookup"><span data-stu-id="3e4df-133">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" 
                hostingModel="inprocess" />
  </system.webServer>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="3e4df-134">Následující *web.config* je publikována pro [samostatná nasazení](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="3e4df-134">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" 
                hostingModel="inprocess" />
  </system.webServer>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="3e4df-135">Když je aplikace nasazená na [služby Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` je nastavena cesta `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="3e4df-135">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="3e4df-136">Protokoly stdout a uloží ji *LogFiles* složce, která je na místě automaticky vytvořené službou.</span><span class="sxs-lookup"><span data-stu-id="3e4df-136">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="3e4df-137">Naleznete v tématu [dílčí aplikace konfigurace](xref:host-and-deploy/iis/index#sub-application-configuration) pro důležitou poznámku týkající se konfigurace *web.config* soubory v dílčí aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e4df-137">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="3e4df-138">Atributy elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="3e4df-138">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="3e4df-139">Atribut</span><span class="sxs-lookup"><span data-stu-id="3e4df-139">Attribute</span></span> | <span data-ttu-id="3e4df-140">Popis</span><span class="sxs-lookup"><span data-stu-id="3e4df-140">Description</span></span> | <span data-ttu-id="3e4df-141">Výchozí</span><span class="sxs-lookup"><span data-stu-id="3e4df-141">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="3e4df-142">Volitelný atribut řetězce.</span><span class="sxs-lookup"><span data-stu-id="3e4df-142">Optional string attribute.</span></span></p><p><span data-ttu-id="3e4df-143">Argumenty pro spustitelný soubor určený v **processPath**.</span><span class="sxs-lookup"><span data-stu-id="3e4df-143">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="3e4df-144">Volitelný logický atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-144">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="3e4df-145">Při hodnotě true **502.5 – selhání procesu** Potlačené stránky a znakovou stránku 502 stav nakonfigurované v *web.config* má přednost před.</span><span class="sxs-lookup"><span data-stu-id="3e4df-145">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="3e4df-146">Volitelný logický atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-146">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="3e4df-147">Při hodnotě true se token předá podřízený proces naslouchání na ASPNETCORE_PORT % jako záhlaví "MS-ASPNETCORE-WINAUTHTOKEN" každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="3e4df-147">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="3e4df-148">Je odpovědností tento proces pro volání CloseHandle na tento token každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="3e4df-148">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="3e4df-149">Volitelný atribut řetězce.</span><span class="sxs-lookup"><span data-stu-id="3e4df-149">Optional string attribute.</span></span></p><p><span data-ttu-id="3e4df-150">Určuje model hostování jako v procesu (`inprocess`) nebo out-of-process (`outofprocess`).</span><span class="sxs-lookup"><span data-stu-id="3e4df-150">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="3e4df-151">Volitelný celočíselný atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-151">Optional integer attribute.</span></span></p><p><span data-ttu-id="3e4df-152">Určuje počet instancí procesu zadaný v **processPath** nastavení, které lze se nespředený nahoru na app.</span><span class="sxs-lookup"><span data-stu-id="3e4df-152">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="3e4df-153">&dagger;Hostování v procesu, je omezena na hodnotu `1`.</span><span class="sxs-lookup"><span data-stu-id="3e4df-153">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="3e4df-154">Výchozí hodnota: `1`</span><span class="sxs-lookup"><span data-stu-id="3e4df-154">Default: `1`</span></span><br><span data-ttu-id="3e4df-155">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="3e4df-155">Min: `1`</span></span><br><span data-ttu-id="3e4df-156">Max: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="3e4df-156">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="3e4df-157">Požadovaný atribut typu string.</span><span class="sxs-lookup"><span data-stu-id="3e4df-157">Required string attribute.</span></span></p><p><span data-ttu-id="3e4df-158">Cesta ke spustitelnému souboru, který spustí nějaký proces naslouchání požadavků protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e4df-158">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="3e4df-159">Jsou podporovány relativní cesty.</span><span class="sxs-lookup"><span data-stu-id="3e4df-159">Relative paths are supported.</span></span> <span data-ttu-id="3e4df-160">Pokud cestu začíná `.`, cesta se považuje za kořeni webu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-160">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="3e4df-161">Volitelný celočíselný atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-161">Optional integer attribute.</span></span></p><p><span data-ttu-id="3e4df-162">Určuje, kolikrát podle procesu **processPath** může při selhání za minutu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-162">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="3e4df-163">Pokud je tento limit překročen, modulu zastaví spuštění procesu pro zbytek minuty.</span><span class="sxs-lookup"><span data-stu-id="3e4df-163">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="3e4df-164">Není podporováno s hostitelem v procesu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-164">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="3e4df-165">Výchozí hodnota: `10`</span><span class="sxs-lookup"><span data-stu-id="3e4df-165">Default: `10`</span></span><br><span data-ttu-id="3e4df-166">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="3e4df-166">Min: `0`</span></span><br><span data-ttu-id="3e4df-167">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="3e4df-167">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="3e4df-168">Atribut volitelný časový interval.</span><span class="sxs-lookup"><span data-stu-id="3e4df-168">Optional timespan attribute.</span></span></p><p><span data-ttu-id="3e4df-169">Určuje dobu, pro kterou modul ASP.NET Core čeká na odpověď z procesu naslouchání na ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="3e4df-169">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="3e4df-170">Ve verzích modul ASP.NET Core dodávané s verzí technologie ASP.NET Core 2.1 nebo novější `requestTimeout` je zadán v hodiny, minuty a sekundy.</span><span class="sxs-lookup"><span data-stu-id="3e4df-170">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="3e4df-171">Neplatí pro hostování v procesu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-171">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="3e4df-172">Hostování v procesu, čeká modul app ke zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="3e4df-172">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="3e4df-173">Výchozí hodnota: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="3e4df-173">Default: `00:02:00`</span></span><br><span data-ttu-id="3e4df-174">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="3e4df-174">Min: `00:00:00`</span></span><br><span data-ttu-id="3e4df-175">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="3e4df-175">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="3e4df-176">Volitelný celočíselný atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-176">Optional integer attribute.</span></span></p><p><span data-ttu-id="3e4df-177">Doba trvání v sekundách, které modul čeká na spustitelný soubor na řádné vypnutí při *app_offline.htm* je detekován soubor.</span><span class="sxs-lookup"><span data-stu-id="3e4df-177">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="3e4df-178">Výchozí hodnota: `10`</span><span class="sxs-lookup"><span data-stu-id="3e4df-178">Default: `10`</span></span><br><span data-ttu-id="3e4df-179">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="3e4df-179">Min: `0`</span></span><br><span data-ttu-id="3e4df-180">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="3e4df-180">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="3e4df-181">Volitelný celočíselný atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-181">Optional integer attribute.</span></span></p><p><span data-ttu-id="3e4df-182">Doba trvání v sekundách, které modul čeká na spustitelný soubor ke spuštění procesu naslouchání na portu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-182">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="3e4df-183">Pokud je překročen časový limit, modul ukončí proces.</span><span class="sxs-lookup"><span data-stu-id="3e4df-183">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="3e4df-184">Modul se pokusí znovu spustit proces, když ji dostane novou žádost a pokusit se restartovat proces na dalších příchozích požadavků, pokud se aplikaci nepodaří spustit **rapidFailsPerMinute** málokdy za posledních minuta se zajištěním provozu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-184">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="3e4df-185">Hodnota 0 (nula) je **ne** za neomezený časový limit.</span><span class="sxs-lookup"><span data-stu-id="3e4df-185">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="3e4df-186">Výchozí hodnota: `120`</span><span class="sxs-lookup"><span data-stu-id="3e4df-186">Default: `120`</span></span><br><span data-ttu-id="3e4df-187">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="3e4df-187">Min: `0`</span></span><br><span data-ttu-id="3e4df-188">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="3e4df-188">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="3e4df-189">Volitelný logický atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-189">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="3e4df-190">Při hodnotě true se **stdout** a **stderr** pro proces určený v **processPath** se přesměrují do souboru zadaného v **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="3e4df-190">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="3e4df-191">Volitelný atribut řetězce.</span><span class="sxs-lookup"><span data-stu-id="3e4df-191">Optional string attribute.</span></span></p><p><span data-ttu-id="3e4df-192">Určuje relativní nebo absolutní cestu, pro kterou **stdout** a **stderr** z procesu podle **processPath** přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="3e4df-192">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="3e4df-193">Relativní cesty jsou relativní vzhledem k kořen webu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-193">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="3e4df-194">Libovolnou cestu od `.` jsou relativní vzhledem k webu kořenové a všechny ostatní cesty jsou považovány za absolutní cesty.</span><span class="sxs-lookup"><span data-stu-id="3e4df-194">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="3e4df-195">Všechny složky v cestě k dispozici musí existovat v pořadí pro modul pro vytvoření souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-195">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="3e4df-196">Pomocí oddělovače podtržítko, časové razítko, ID procesu a přípona souboru (*.log*) jsou přidány na poslední segment **stdoutLogFile** cestu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-196">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="3e4df-197">Pokud `.\logs\stdout` je zadaný jako hodnota, je uložen protokolu příklad stdout jako *stdout_20180205194132_1934.log* v *protokoly* složky při uložení 5 2. 2018 v 19:41:32 s ID procesu sady 1934.</span><span class="sxs-lookup"><span data-stu-id="3e4df-197">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="3e4df-198">Atribut</span><span class="sxs-lookup"><span data-stu-id="3e4df-198">Attribute</span></span> | <span data-ttu-id="3e4df-199">Popis</span><span class="sxs-lookup"><span data-stu-id="3e4df-199">Description</span></span> | <span data-ttu-id="3e4df-200">Výchozí</span><span class="sxs-lookup"><span data-stu-id="3e4df-200">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="3e4df-201">Volitelný atribut řetězce.</span><span class="sxs-lookup"><span data-stu-id="3e4df-201">Optional string attribute.</span></span></p><p><span data-ttu-id="3e4df-202">Argumenty pro spustitelný soubor určený v **processPath**.</span><span class="sxs-lookup"><span data-stu-id="3e4df-202">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="3e4df-203">Volitelný logický atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-203">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="3e4df-204">Při hodnotě true **502.5 – selhání procesu** Potlačené stránky a znakovou stránku 502 stav nakonfigurované v *web.config* má přednost před.</span><span class="sxs-lookup"><span data-stu-id="3e4df-204">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="3e4df-205">Volitelný logický atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-205">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="3e4df-206">Při hodnotě true se token předá podřízený proces naslouchání na ASPNETCORE_PORT % jako záhlaví "MS-ASPNETCORE-WINAUTHTOKEN" každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="3e4df-206">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="3e4df-207">Je odpovědností tento proces pro volání CloseHandle na tento token každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="3e4df-207">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="3e4df-208">Volitelný celočíselný atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-208">Optional integer attribute.</span></span></p><p><span data-ttu-id="3e4df-209">Určuje počet instancí procesu zadaný v **processPath** nastavení, které lze se nespředený nahoru na app.</span><span class="sxs-lookup"><span data-stu-id="3e4df-209">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="3e4df-210">Výchozí hodnota: `1`</span><span class="sxs-lookup"><span data-stu-id="3e4df-210">Default: `1`</span></span><br><span data-ttu-id="3e4df-211">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="3e4df-211">Min: `1`</span></span><br><span data-ttu-id="3e4df-212">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="3e4df-212">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="3e4df-213">Požadovaný atribut typu string.</span><span class="sxs-lookup"><span data-stu-id="3e4df-213">Required string attribute.</span></span></p><p><span data-ttu-id="3e4df-214">Cesta ke spustitelnému souboru, který spustí nějaký proces naslouchání požadavků protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e4df-214">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="3e4df-215">Jsou podporovány relativní cesty.</span><span class="sxs-lookup"><span data-stu-id="3e4df-215">Relative paths are supported.</span></span> <span data-ttu-id="3e4df-216">Pokud cestu začíná `.`, cesta se považuje za kořeni webu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-216">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="3e4df-217">Volitelný celočíselný atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-217">Optional integer attribute.</span></span></p><p><span data-ttu-id="3e4df-218">Určuje, kolikrát podle procesu **processPath** může při selhání za minutu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-218">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="3e4df-219">Pokud je tento limit překročen, modulu zastaví spuštění procesu pro zbytek minuty.</span><span class="sxs-lookup"><span data-stu-id="3e4df-219">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="3e4df-220">Výchozí hodnota: `10`</span><span class="sxs-lookup"><span data-stu-id="3e4df-220">Default: `10`</span></span><br><span data-ttu-id="3e4df-221">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="3e4df-221">Min: `0`</span></span><br><span data-ttu-id="3e4df-222">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="3e4df-222">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="3e4df-223">Atribut volitelný časový interval.</span><span class="sxs-lookup"><span data-stu-id="3e4df-223">Optional timespan attribute.</span></span></p><p><span data-ttu-id="3e4df-224">Určuje dobu, pro kterou modul ASP.NET Core čeká na odpověď z procesu naslouchání na ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="3e4df-224">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="3e4df-225">Ve verzích modul ASP.NET Core dodávané s verzí technologie ASP.NET Core 2.1 nebo novější `requestTimeout` je zadán v hodiny, minuty a sekundy.</span><span class="sxs-lookup"><span data-stu-id="3e4df-225">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="3e4df-226">Výchozí hodnota: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="3e4df-226">Default: `00:02:00`</span></span><br><span data-ttu-id="3e4df-227">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="3e4df-227">Min: `00:00:00`</span></span><br><span data-ttu-id="3e4df-228">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="3e4df-228">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="3e4df-229">Volitelný celočíselný atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-229">Optional integer attribute.</span></span></p><p><span data-ttu-id="3e4df-230">Doba trvání v sekundách, které modul čeká na spustitelný soubor na řádné vypnutí při *app_offline.htm* je detekován soubor.</span><span class="sxs-lookup"><span data-stu-id="3e4df-230">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="3e4df-231">Výchozí hodnota: `10`</span><span class="sxs-lookup"><span data-stu-id="3e4df-231">Default: `10`</span></span><br><span data-ttu-id="3e4df-232">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="3e4df-232">Min: `0`</span></span><br><span data-ttu-id="3e4df-233">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="3e4df-233">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="3e4df-234">Volitelný celočíselný atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="3e4df-235">Doba trvání v sekundách, které modul čeká na spustitelný soubor ke spuštění procesu naslouchání na portu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-235">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="3e4df-236">Pokud je překročen časový limit, modul ukončí proces.</span><span class="sxs-lookup"><span data-stu-id="3e4df-236">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="3e4df-237">Modul se pokusí znovu spustit proces, když ji dostane novou žádost a pokusit se restartovat proces na dalších příchozích požadavků, pokud se aplikaci nepodaří spustit **rapidFailsPerMinute** málokdy za posledních minuta se zajištěním provozu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-237">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="3e4df-238">Hodnota 0 (nula) je **ne** za neomezený časový limit.</span><span class="sxs-lookup"><span data-stu-id="3e4df-238">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="3e4df-239">Výchozí hodnota: `120`</span><span class="sxs-lookup"><span data-stu-id="3e4df-239">Default: `120`</span></span><br><span data-ttu-id="3e4df-240">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="3e4df-240">Min: `0`</span></span><br><span data-ttu-id="3e4df-241">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="3e4df-241">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="3e4df-242">Volitelný logický atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-242">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="3e4df-243">Při hodnotě true se **stdout** a **stderr** pro proces určený v **processPath** se přesměrují do souboru zadaného v **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="3e4df-243">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="3e4df-244">Volitelný atribut řetězce.</span><span class="sxs-lookup"><span data-stu-id="3e4df-244">Optional string attribute.</span></span></p><p><span data-ttu-id="3e4df-245">Určuje relativní nebo absolutní cestu, pro kterou **stdout** a **stderr** z procesu podle **processPath** přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="3e4df-245">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="3e4df-246">Relativní cesty jsou relativní vzhledem k kořen webu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-246">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="3e4df-247">Libovolnou cestu od `.` jsou relativní vzhledem k webu kořenové a všechny ostatní cesty jsou považovány za absolutní cesty.</span><span class="sxs-lookup"><span data-stu-id="3e4df-247">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="3e4df-248">Všechny složky v cestě k dispozici musí existovat v pořadí pro modul pro vytvoření souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-248">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="3e4df-249">Pomocí oddělovače podtržítko, časové razítko, ID procesu a přípona souboru (*.log*) jsou přidány na poslední segment **stdoutLogFile** cestu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-249">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="3e4df-250">Pokud `.\logs\stdout` je zadaný jako hodnota, je uložen protokolu příklad stdout jako *stdout_20180205194132_1934.log* v *protokoly* složky při uložení 5 2. 2018 v 19:41:32 s ID procesu sady 1934.</span><span class="sxs-lookup"><span data-stu-id="3e4df-250">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="3e4df-251">Atribut</span><span class="sxs-lookup"><span data-stu-id="3e4df-251">Attribute</span></span> | <span data-ttu-id="3e4df-252">Popis</span><span class="sxs-lookup"><span data-stu-id="3e4df-252">Description</span></span> | <span data-ttu-id="3e4df-253">Výchozí</span><span class="sxs-lookup"><span data-stu-id="3e4df-253">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="3e4df-254">Volitelný atribut řetězce.</span><span class="sxs-lookup"><span data-stu-id="3e4df-254">Optional string attribute.</span></span></p><p><span data-ttu-id="3e4df-255">Argumenty pro spustitelný soubor určený v **processPath**.</span><span class="sxs-lookup"><span data-stu-id="3e4df-255">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="3e4df-256">Volitelný logický atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-256">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="3e4df-257">Při hodnotě true **502.5 – selhání procesu** Potlačené stránky a znakovou stránku 502 stav nakonfigurované v *web.config* má přednost před.</span><span class="sxs-lookup"><span data-stu-id="3e4df-257">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="3e4df-258">Volitelný logický atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-258">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="3e4df-259">Při hodnotě true se token předá podřízený proces naslouchání na ASPNETCORE_PORT % jako záhlaví "MS-ASPNETCORE-WINAUTHTOKEN" každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="3e4df-259">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="3e4df-260">Je odpovědností tento proces pro volání CloseHandle na tento token každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="3e4df-260">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="3e4df-261">Volitelný celočíselný atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-261">Optional integer attribute.</span></span></p><p><span data-ttu-id="3e4df-262">Určuje počet instancí procesu zadaný v **processPath** nastavení, které lze se nespředený nahoru na app.</span><span class="sxs-lookup"><span data-stu-id="3e4df-262">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="3e4df-263">Výchozí hodnota: `1`</span><span class="sxs-lookup"><span data-stu-id="3e4df-263">Default: `1`</span></span><br><span data-ttu-id="3e4df-264">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="3e4df-264">Min: `1`</span></span><br><span data-ttu-id="3e4df-265">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="3e4df-265">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="3e4df-266">Požadovaný atribut typu string.</span><span class="sxs-lookup"><span data-stu-id="3e4df-266">Required string attribute.</span></span></p><p><span data-ttu-id="3e4df-267">Cesta ke spustitelnému souboru, který spustí nějaký proces naslouchání požadavků protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e4df-267">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="3e4df-268">Jsou podporovány relativní cesty.</span><span class="sxs-lookup"><span data-stu-id="3e4df-268">Relative paths are supported.</span></span> <span data-ttu-id="3e4df-269">Pokud cestu začíná `.`, cesta se považuje za kořeni webu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-269">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="3e4df-270">Volitelný celočíselný atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-270">Optional integer attribute.</span></span></p><p><span data-ttu-id="3e4df-271">Určuje, kolikrát podle procesu **processPath** může při selhání za minutu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-271">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="3e4df-272">Pokud je tento limit překročen, modulu zastaví spuštění procesu pro zbytek minuty.</span><span class="sxs-lookup"><span data-stu-id="3e4df-272">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="3e4df-273">Výchozí hodnota: `10`</span><span class="sxs-lookup"><span data-stu-id="3e4df-273">Default: `10`</span></span><br><span data-ttu-id="3e4df-274">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="3e4df-274">Min: `0`</span></span><br><span data-ttu-id="3e4df-275">Max: `100`</span><span class="sxs-lookup"><span data-stu-id="3e4df-275">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="3e4df-276">Atribut volitelný časový interval.</span><span class="sxs-lookup"><span data-stu-id="3e4df-276">Optional timespan attribute.</span></span></p><p><span data-ttu-id="3e4df-277">Určuje dobu, pro kterou modul ASP.NET Core čeká na odpověď z procesu naslouchání na ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="3e4df-277">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="3e4df-278">Ve verzích modul ASP.NET Core dodávané s verzí technologie ASP.NET Core 2.0 nebo starší `requestTimeout` musí být zadán v celých minutách, v opačném případě výchozí hodnota 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="3e4df-278">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="3e4df-279">Výchozí hodnota: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="3e4df-279">Default: `00:02:00`</span></span><br><span data-ttu-id="3e4df-280">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="3e4df-280">Min: `00:00:00`</span></span><br><span data-ttu-id="3e4df-281">Max: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="3e4df-281">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="3e4df-282">Volitelný celočíselný atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-282">Optional integer attribute.</span></span></p><p><span data-ttu-id="3e4df-283">Doba trvání v sekundách, které modul čeká na spustitelný soubor na řádné vypnutí při *app_offline.htm* je detekován soubor.</span><span class="sxs-lookup"><span data-stu-id="3e4df-283">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="3e4df-284">Výchozí hodnota: `10`</span><span class="sxs-lookup"><span data-stu-id="3e4df-284">Default: `10`</span></span><br><span data-ttu-id="3e4df-285">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="3e4df-285">Min: `0`</span></span><br><span data-ttu-id="3e4df-286">Max: `600`</span><span class="sxs-lookup"><span data-stu-id="3e4df-286">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="3e4df-287">Volitelný celočíselný atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-287">Optional integer attribute.</span></span></p><p><span data-ttu-id="3e4df-288">Doba trvání v sekundách, které modul čeká na spustitelný soubor ke spuštění procesu naslouchání na portu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-288">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="3e4df-289">Pokud je překročen časový limit, modul ukončí proces.</span><span class="sxs-lookup"><span data-stu-id="3e4df-289">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="3e4df-290">Modul se pokusí znovu spustit proces, když ji dostane novou žádost a pokusit se restartovat proces na dalších příchozích požadavků, pokud se aplikaci nepodaří spustit **rapidFailsPerMinute** málokdy za posledních minuta se zajištěním provozu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-290">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="3e4df-291">Výchozí hodnota: `120`</span><span class="sxs-lookup"><span data-stu-id="3e4df-291">Default: `120`</span></span><br><span data-ttu-id="3e4df-292">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="3e4df-292">Min: `0`</span></span><br><span data-ttu-id="3e4df-293">Max: `3600`</span><span class="sxs-lookup"><span data-stu-id="3e4df-293">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="3e4df-294">Volitelný logický atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-294">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="3e4df-295">Při hodnotě true se **stdout** a **stderr** pro proces určený v **processPath** se přesměrují do souboru zadaného v **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="3e4df-295">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="3e4df-296">Volitelný atribut řetězce.</span><span class="sxs-lookup"><span data-stu-id="3e4df-296">Optional string attribute.</span></span></p><p><span data-ttu-id="3e4df-297">Určuje relativní nebo absolutní cestu, pro kterou **stdout** a **stderr** z procesu podle **processPath** přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="3e4df-297">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="3e4df-298">Relativní cesty jsou relativní vzhledem k kořen webu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-298">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="3e4df-299">Libovolnou cestu od `.` jsou relativní vzhledem k webu kořenové a všechny ostatní cesty jsou považovány za absolutní cesty.</span><span class="sxs-lookup"><span data-stu-id="3e4df-299">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="3e4df-300">Všechny složky v cestě k dispozici musí existovat v pořadí pro modul pro vytvoření souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-300">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="3e4df-301">Pomocí oddělovače podtržítko, časové razítko, ID procesu a přípona souboru (*.log*) jsou přidány na poslední segment **stdoutLogFile** cestu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-301">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="3e4df-302">Pokud `.\logs\stdout` je zadaný jako hodnota, je uložen protokolu příklad stdout jako *stdout_20180205194132_1934.log* v *protokoly* složky při uložení 5 2. 2018 v 19:41:32 s ID procesu sady 1934.</span><span class="sxs-lookup"><span data-stu-id="3e4df-302">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="3e4df-303">Nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="3e4df-303">Setting environment variables</span></span>

<span data-ttu-id="3e4df-304">Proměnné prostředí se dá nastavit pro proces v `processPath` atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-304">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="3e4df-305">Zadat proměnné prostředí s `environmentVariable` podřízený prvek `environmentVariables` prvek kolekce.</span><span class="sxs-lookup"><span data-stu-id="3e4df-305">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="3e4df-306">Proměnné prostředí nastavené v této části přednost systémové proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e4df-306">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="3e4df-307">Následující příklad nastaví dvou proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e4df-307">The following example sets two environment variables.</span></span> <span data-ttu-id="3e4df-308">`ASPNETCORE_ENVIRONMENT` nakonfiguruje prostředí aplikace tak, aby `Development`.</span><span class="sxs-lookup"><span data-stu-id="3e4df-308">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="3e4df-309">Vývojáři mohou dočasně nastaví tuto hodnotu *web.config* souboru, aby bylo možné vynutit [stránku výjimek pro vývojáře](xref:fundamentals/error-handling) načíst při ladění aplikace výjimky.</span><span class="sxs-lookup"><span data-stu-id="3e4df-309">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="3e4df-310">`CONFIG_DIR` je příkladem proměnné prostředí, kam má vývojář zapisovat kód, který čte hodnoty při spuštění tvoří cestu pro načtení konfiguračního souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e4df-310">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="3e4df-311">Nastavit pouze `ASPNETCORE_ENVIRONMENT` proměnnou prostředí, aby `Development` na přípravy a testování serverů, které nejsou dostupné k nedůvěryhodným sítím, jako je Internet.</span><span class="sxs-lookup"><span data-stu-id="3e4df-311">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="3e4df-312">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="3e4df-312">app_offline.htm</span></span>

<span data-ttu-id="3e4df-313">Pokud soubor s názvem *app_offline.htm* je zjištěna v kořenovém adresáři aplikace, že modul ASP.NET Core se pokusí řádné vypnutí aplikace a zastavit zpracování příchozích požadavků.</span><span class="sxs-lookup"><span data-stu-id="3e4df-313">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="3e4df-314">Pokud aplikace pořád běží po dobu v sekundách podle `shutdownTimeLimit`, že modul ASP.NET Core ukončuje spuštěnému procesu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-314">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="3e4df-315">Zatímco *app_offline.htm* soubor je k dispozici, že modul ASP.NET Core reaguje na požadavky odesílá zpět obsah *app_offline.htm* souboru.</span><span class="sxs-lookup"><span data-stu-id="3e4df-315">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="3e4df-316">Když *app_offline.htm* soubor bude odstraněn, příští žádosti o spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e4df-316">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3e4df-317">Pokud používáte model hostingu mimo proces, aplikace nemusí vypnout okamžitě při otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="3e4df-317">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="3e4df-318">Připojení soketu websocket bylo třeba může způsobit prodlevu při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e4df-318">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="3e4df-319">Spuštění chybovou stránku</span><span class="sxs-lookup"><span data-stu-id="3e4df-319">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3e4df-320">*Platí jenom pro hostování mimo proces.*</span><span class="sxs-lookup"><span data-stu-id="3e4df-320">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="3e4df-321">Pokud se nespustí back-endový proces nebo back-endový proces spustí ale nebude moci poslouchat na konfigurovaném portu, že modul ASP.NET Core *502.5 selhání procesu* se zobrazí stav znakovou stránku.</span><span class="sxs-lookup"><span data-stu-id="3e4df-321">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="3e4df-322">Chcete-li potlačit tuto stránku a vrátit se na výchozí stav služby IIS 502 znakovou stránku, použijte `disableStartUpErrorPage` atribut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-322">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="3e4df-323">Další informace o konfiguraci vlastních chybových zpráv, najdete v části [chyby protokolu HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="3e4df-323">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 proces selhání stav znaková stránka](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="3e4df-325">Vytvoření protokolu a přesměrování</span><span class="sxs-lookup"><span data-stu-id="3e4df-325">Log creation and redirection</span></span>

<span data-ttu-id="3e4df-326">Modul ASP.NET Core přesměruje výstup stdout a stderr console na disk, pokud `stdoutLogEnabled` a `stdoutLogFile` atributy `aspNetCore` element jsou nastavené.</span><span class="sxs-lookup"><span data-stu-id="3e4df-326">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="3e4df-327">Všechny složky v `stdoutLogFile` cesta musí existovat v pořadí pro modul pro vytvoření souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-327">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="3e4df-328">Fond aplikací musí mít oprávnění k zápisu do umístění, ve kterém jsou zapsány protokoly (použijte `IIS AppPool\<app_pool_name>` poskytnout oprávnění k zápisu).</span><span class="sxs-lookup"><span data-stu-id="3e4df-328">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="3e4df-329">Protokoly nejsou střídán, pokud dojde k recyklování procesů/restartování.</span><span class="sxs-lookup"><span data-stu-id="3e4df-329">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="3e4df-330">Je odpovědností hostitel k omezení místa na disku, které využijete v protokolech.</span><span class="sxs-lookup"><span data-stu-id="3e4df-330">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="3e4df-331">Použití protokolu stdout se doporučuje jenom pro řešení potíží při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e4df-331">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="3e4df-332">Nepoužívejte stdout protokolu pro účely protokolování obecné aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e4df-332">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="3e4df-333">Pro rutiny protokolování v aplikaci ASP.NET Core, použijte protokolování knihovnu, která omezuje velikost souboru protokolu a otočí protokoly.</span><span class="sxs-lookup"><span data-stu-id="3e4df-333">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="3e4df-334">Další informace najdete v tématu [zprostředkovatele přihlášení třetí strany](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="3e4df-334">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="3e4df-335">Časové razítko a soubor rozšíření jsou přidány automaticky při vytvoření souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-335">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="3e4df-336">Název souboru protokolu se skládá připojením časové razítko, ID procesu a přípona souboru (*.log*) na poslední segment `stdoutLogFile` cestu (obvykle *stdout*) oddělené podtržítka.</span><span class="sxs-lookup"><span data-stu-id="3e4df-336">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="3e4df-337">Pokud `stdoutLogFile` cesta končí *stdout*, má název souboru protokolu pro aplikaci s PID 1934 vytvořili 5 2. 2018 v 19:42:32 *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="3e4df-337">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="3e4df-338">Následující ukázka `aspNetCore` element konfiguruje stdout protokolování pro aplikace hostované ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3e4df-338">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="3e4df-339">Místní cesta nebo cesta ke sdílené položce je přijatelné pro místní přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3e4df-339">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="3e4df-340">Ověřte, jestli uživatel identita fondu aplikací má oprávnění k zápisu do zadaná výstupní cesta.</span><span class="sxs-lookup"><span data-stu-id="3e4df-340">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="3e4df-341">Rozšířené diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="3e4df-341">Enhanced diagnostic logs</span></span>

<span data-ttu-id="3e4df-342">Poskytuje modul ASP.NET Core je konfigurovat, a poskytují rozšířené diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="3e4df-342">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="3e4df-343">Přidat `<handlerSettings>` elementu `<aspNetCore>` prvek *web.config*. Nastavení `debugLevel` k `TRACE` zpřístupňuje větší věrnost diagnostické informace:</span><span class="sxs-lookup"><span data-stu-id="3e4df-343">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="3e4df-344">Ladit úrovně (`debugLevel`) hodnoty může obsahovat úroveň a umístění.</span><span class="sxs-lookup"><span data-stu-id="3e4df-344">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="3e4df-345">Úrovně (v pořadí od nejméně na nejpodrobnější):</span><span class="sxs-lookup"><span data-stu-id="3e4df-345">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="3e4df-346">CHYBA</span><span class="sxs-lookup"><span data-stu-id="3e4df-346">ERROR</span></span>
* <span data-ttu-id="3e4df-347">UPOZORNĚNÍ</span><span class="sxs-lookup"><span data-stu-id="3e4df-347">WARNING</span></span>
* <span data-ttu-id="3e4df-348">INFORMACE O</span><span class="sxs-lookup"><span data-stu-id="3e4df-348">INFO</span></span>
* <span data-ttu-id="3e4df-349">TRACE</span><span class="sxs-lookup"><span data-stu-id="3e4df-349">TRACE</span></span>

<span data-ttu-id="3e4df-350">Umístění (umístění více jsou povoleny):</span><span class="sxs-lookup"><span data-stu-id="3e4df-350">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="3e4df-351">KONZOLY</span><span class="sxs-lookup"><span data-stu-id="3e4df-351">CONSOLE</span></span>
* <span data-ttu-id="3e4df-352">PROTOKOL UDÁLOSTÍ</span><span class="sxs-lookup"><span data-stu-id="3e4df-352">EVENTLOG</span></span>
* <span data-ttu-id="3e4df-353">SOUBOR</span><span class="sxs-lookup"><span data-stu-id="3e4df-353">FILE</span></span>

<span data-ttu-id="3e4df-354">Obslužná rutina nastavení se dá zadat i prostřednictvím proměnné prostředí:</span><span class="sxs-lookup"><span data-stu-id="3e4df-354">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="3e4df-355">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Cesta k souboru protokolu ladění.</span><span class="sxs-lookup"><span data-stu-id="3e4df-355">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="3e4df-356">(Výchozí: *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="3e4df-356">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="3e4df-357">`ASPNETCORE_MODULE_DEBUG` &ndash; Ladit nastavení úrovně.</span><span class="sxs-lookup"><span data-stu-id="3e4df-357">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="3e4df-358">Proveďte **není** protokolování ladění povoleno v nasazení pro déle, než se požaduje chcete vyřešit nějaký problém.</span><span class="sxs-lookup"><span data-stu-id="3e4df-358">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="3e4df-359">Velikost souboru protokolu není omezený.</span><span class="sxs-lookup"><span data-stu-id="3e4df-359">The size of the log isn't limited.</span></span> <span data-ttu-id="3e4df-360">Opuštění protokol ladění povoleno může vyčerpat dostupné místo na disku a chybách u aplikací na serveru nebo služby app service.</span><span class="sxs-lookup"><span data-stu-id="3e4df-360">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="3e4df-361">Naleznete v tématu [konfigurace pomocí souboru web.config](#configuration-with-webconfig) příklad `aspNetCore` prvek *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="3e4df-361">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="3e4df-362">Konfigurace proxy serveru používá protokol HTTP a token pro párování</span><span class="sxs-lookup"><span data-stu-id="3e4df-362">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3e4df-363">*Platí jenom pro hostování mimo proces.*</span><span class="sxs-lookup"><span data-stu-id="3e4df-363">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="3e4df-364">Proxy server mezi modul ASP.NET Core a Kestrel používá protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e4df-364">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="3e4df-365">Pomocí protokolu HTTP je optimalizace výkonu, kdy přenos dat mezi modulu a Kestrel probíhá na adresu zpětné smyčky ze síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3e4df-365">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="3e4df-366">Neexistuje žádné riziko odposloucháváním provozu mezi modulu a Kestrel z umístění mimo server.</span><span class="sxs-lookup"><span data-stu-id="3e4df-366">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="3e4df-367">Párování token se používá k zajištění, že požadavků přijatých službou Kestrel byly směrovány přes proxy server službou IIS a nebyl dodán z nějakého jiného zdroje.</span><span class="sxs-lookup"><span data-stu-id="3e4df-367">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="3e4df-368">Vytvoření a nastavení do proměnné prostředí párování token (`ASPNETCORE_TOKEN`) modulu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-368">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="3e4df-369">Párování token byl nastavený i do záhlaví (`MSAspNetCoreToken`) na všechny požadavky směrovány přes proxy server.</span><span class="sxs-lookup"><span data-stu-id="3e4df-369">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="3e4df-370">Služba IIS Middleware kontroly požadavku že přijme potvrďte, že odpovídá párování hodnota tokenu hlavičky hodnotu proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e4df-370">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="3e4df-371">Pokud token hodnoty se neshodují, žádost je zaznamenána a odmítnut.</span><span class="sxs-lookup"><span data-stu-id="3e4df-371">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="3e4df-372">Párování proměnná tokenu prostředí a přenos dat mezi modulu a Kestrel nejsou dostupné z umístění mimo server.</span><span class="sxs-lookup"><span data-stu-id="3e4df-372">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="3e4df-373">Nainstalovat bez mého párování hodnota tokenu nelze útočník odesílat požadavky, které obejít kontrolu v IIS middlewaru.</span><span class="sxs-lookup"><span data-stu-id="3e4df-373">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="3e4df-374">Modul ASP.NET Core s službu IIS sdílenou konfiguraci</span><span class="sxs-lookup"><span data-stu-id="3e4df-374">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="3e4df-375">Modul ASP.NET Core instalační program spustí s oprávněními **systému** účtu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-375">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="3e4df-376">Vzhledem k tomu, že místní systémový účet mít nezmění oprávnění pro sdílenou složku cesta používaná systémem sdílené konfiguraci IIS, instalační program narazí na chybu při pokusu o konfiguraci nastavení modulu v odepření přístupu *applicationHost.config* ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="3e4df-376">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="3e4df-377">Pokud používáte sdílenou konfiguraci IIS, postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="3e4df-377">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="3e4df-378">Zakážete sdílenou konfiguraci IIS.</span><span class="sxs-lookup"><span data-stu-id="3e4df-378">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="3e4df-379">Spusťte instalační program.</span><span class="sxs-lookup"><span data-stu-id="3e4df-379">Run the installer.</span></span>
1. <span data-ttu-id="3e4df-380">Export aktualizovaný *applicationHost.config* souboru do sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3e4df-380">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="3e4df-381">Znovu povolte sdílenou konfiguraci IIS.</span><span class="sxs-lookup"><span data-stu-id="3e4df-381">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="3e4df-382">Verze modulu a hostování sady Instalační protokoly</span><span class="sxs-lookup"><span data-stu-id="3e4df-382">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="3e4df-383">Chcete-li zjistit verzi nainstalovaný modul ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3e4df-383">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="3e4df-384">V hostitelském systému, přejděte na *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="3e4df-384">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="3e4df-385">Vyhledejte *aspnetcore.dll* souboru.</span><span class="sxs-lookup"><span data-stu-id="3e4df-385">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="3e4df-386">Klikněte pravým tlačítkem na soubor a vyberte **vlastnosti** z kontextové nabídky.</span><span class="sxs-lookup"><span data-stu-id="3e4df-386">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="3e4df-387">Vyberte **podrobnosti** kartu. **Verze souboru** a **verze produktu** představují nainstalovaná verze modulu.</span><span class="sxs-lookup"><span data-stu-id="3e4df-387">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="3e4df-388">Instalační protokoly hostování sady prostředků modulu se nacházejí v *C:\\uživatelé\\% UserName %\\AppData\\místní\\Temp*. Soubor *dd_DotNetCoreWinSvrHosting__\<časové razítko > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="3e4df-388">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="3e4df-389">Modul, schéma a konfiguraci umístění souborů</span><span class="sxs-lookup"><span data-stu-id="3e4df-389">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="3e4df-390">Modul</span><span class="sxs-lookup"><span data-stu-id="3e4df-390">Module</span></span>

<span data-ttu-id="3e4df-391">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="3e4df-391">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="3e4df-392">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="3e4df-392">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="3e4df-393">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="3e4df-393">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="3e4df-394">**Služba IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="3e4df-394">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="3e4df-395">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="3e4df-395">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="3e4df-396">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="3e4df-396">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="3e4df-397">Schéma</span><span class="sxs-lookup"><span data-stu-id="3e4df-397">Schema</span></span>

<span data-ttu-id="3e4df-398">**SLUŽBA IIS**</span><span class="sxs-lookup"><span data-stu-id="3e4df-398">**IIS**</span></span>

   * <span data-ttu-id="3e4df-399">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="3e4df-399">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="3e4df-400">**Služba IIS Express**</span><span class="sxs-lookup"><span data-stu-id="3e4df-400">**IIS Express**</span></span>

   * <span data-ttu-id="3e4df-401">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="3e4df-401">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="3e4df-402">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="3e4df-402">Configuration</span></span>

<span data-ttu-id="3e4df-403">**SLUŽBA IIS**</span><span class="sxs-lookup"><span data-stu-id="3e4df-403">**IIS**</span></span>

   * <span data-ttu-id="3e4df-404">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="3e4df-404">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="3e4df-405">**Služba IIS Express**</span><span class="sxs-lookup"><span data-stu-id="3e4df-405">**IIS Express**</span></span>

   * <span data-ttu-id="3e4df-406">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="3e4df-406">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="3e4df-407">Soubory můžete najít tak, že *aspnetcore.dll* v *applicationHost.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="3e4df-407">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="3e4df-408">Pro službu IIS Express *applicationHost.config* soubor neexistují ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3e4df-408">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="3e4df-409">Soubor je vytvořen v  *\<application_root >\\.vs\\config* při spouštění projektu žádné webové aplikace v řešení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e4df-409">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
