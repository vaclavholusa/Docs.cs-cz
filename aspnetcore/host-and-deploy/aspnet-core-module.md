---
title: "Odkaz na konfiguraci základní modul ASP.NET"
author: guardrex
description: "Postup konfigurace modulu jádra ASP.NET pro hostování aplikací ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c01abed767a226eae68725c1c53d922eac2f705e
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2018
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="c4ee4-103">Odkaz na konfiguraci základní modul ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c4ee4-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="c4ee4-104">Podle [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), a [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="c4ee4-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="c4ee4-105">Tento dokument obsahuje pokyny ke konfiguraci základní modul ASP.NET pro hostování aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="c4ee4-106">Úvod do modulu jádra ASP.NET a pokyny k instalaci, najdete v článku [ASP.NET Core modulu přehled](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="c4ee4-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="c4ee4-107">Konfigurace se soubor web.config</span><span class="sxs-lookup"><span data-stu-id="c4ee4-107">Configuration with web.config</span></span>

<span data-ttu-id="c4ee4-108">Modul ASP.NET Core nastavena `aspNetCore` části `system.webServer` uzlu na webu *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-108">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="c4ee4-109">Následující *web.config* souboru je publikována pro [nasazení závislé na framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) a nakonfiguruje modul ASP.NET jádra pro zpracování požadavků lokality:</span><span class="sxs-lookup"><span data-stu-id="c4ee4-109">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="c4ee4-110">Následující *web.config* je publikována pro [samostatná nasazení](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="c4ee4-110">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="c4ee4-111">Když je aplikace nasazená na [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` cesta je nastavena na `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-111">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="c4ee4-112">Cesta uloží protokoly stdout *LogFiles* složky, která je umístění automaticky vytvořených službou.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-112">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="c4ee4-113">V tématu [dílčí aplikace konfigurace](xref:host-and-deploy/iis/index#sub-application-configuration) pro důležitá poznámka týkající se konfigurace *web.config* soubory v dílčí aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-113">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="c4ee4-114">Atributy elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="c4ee4-114">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="c4ee4-115">Atribut</span><span class="sxs-lookup"><span data-stu-id="c4ee4-115">Attribute</span></span> | <span data-ttu-id="c4ee4-116">Popis</span><span class="sxs-lookup"><span data-stu-id="c4ee4-116">Description</span></span> | <span data-ttu-id="c4ee4-117">Výchozí</span><span class="sxs-lookup"><span data-stu-id="c4ee4-117">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="c4ee4-118">Volitelný řetězec atributu.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-118">Optional string attribute.</span></span></p><p><span data-ttu-id="c4ee4-119">Argumenty pro spustitelný soubor určený v **processPath**.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-119">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <span data-ttu-id="c4ee4-120">Hodnota true nebo false</span><span class="sxs-lookup"><span data-stu-id="c4ee4-120">true or false.</span></span></p><p><span data-ttu-id="c4ee4-121">V případě hodnoty true **502.5 - selhání procesu** potlačeno stránky a stav 502 znaková stránka konfigurované v *web.config* má přednost před.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-121">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <span data-ttu-id="c4ee4-122">Hodnota true nebo false</span><span class="sxs-lookup"><span data-stu-id="c4ee4-122">true or false.</span></span></p><p><span data-ttu-id="c4ee4-123">V případě hodnoty true token se předají do podřízeného procesu naslouchání na ASPNETCORE_PORT % jako hlavičku, MS-ASPNETCORE-WINAUTHTOKEN' každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-123">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="c4ee4-124">Je zodpovědností procesu pro volání funkce CloseHandle na tento token na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-124">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processPath` | <p><span data-ttu-id="c4ee4-125">Požadovaný atribut typu string.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-125">Required string attribute.</span></span></p><p><span data-ttu-id="c4ee4-126">Cesta ke spustitelnému souboru, který spustí proces naslouchání požadavkům HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-126">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="c4ee4-127">Jsou podporovány relativní cesty.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-127">Relative paths are supported.</span></span> <span data-ttu-id="c4ee4-128">Pokud cesta začíná `.`, cesta se považuje za relativní vůči kořenovému adresáři webu.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-128">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="c4ee4-129">Atribut volitelné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-129">Optional integer attribute.</span></span></p><p><span data-ttu-id="c4ee4-130">Určuje, kolikrát proces zadaný v **processPath** je dovoleno havárií za minutu.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-130">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="c4ee4-131">Pokud je tento limit překročen, modul zastaví spuštění procesu pro zbytek minutu.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-131">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | `10` |
| `requestTimeout` | <p><span data-ttu-id="c4ee4-132">Atribut volitelné časový interval.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-132">Optional timespan attribute.</span></span></p><p><span data-ttu-id="c4ee4-133">Určuje dobu, pro který modul ASP.NET jádra čeká na odpověď z procesu naslouchání na ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-133">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="c4ee4-134">`requestTimeout` Musí být zadaná v celé minut, v opačném případě výchozí hodnota 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-134">The `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | `00:02:00` |
| `shutdownTimeLimit` | <p><span data-ttu-id="c4ee4-135">Atribut volitelné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-135">Optional integer attribute.</span></span></p><p><span data-ttu-id="c4ee4-136">Doba v sekundách, kterou čeká modul pro spustitelný soubor řádně vypnutí při *app_offline.htm* je detekován soubor.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-136">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | `10` |
| `startupTimeLimit` | <p><span data-ttu-id="c4ee4-137">Atribut volitelné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-137">Optional integer attribute.</span></span></p><p><span data-ttu-id="c4ee4-138">Doba v sekundách, kterou čeká modul pro spuštění procesu naslouchání na portu spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-138">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="c4ee4-139">Pokud je tento časový limit překročen, modul ukončí proces.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-139">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="c4ee4-140">Modul se pokusí spustit proces, při přijetí nového požadavku a pokračuje v pokusí restartovat proces na následné příchozí žádosti, pokud se nepodaří spustit aplikaci **rapidFailsPerMinute** počet, kolikrát za posledních vrácení minutu.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-140">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | `120` |
| `stdoutLogEnabled` | <p><span data-ttu-id="c4ee4-141">Volitelný logický atribut.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-141">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="c4ee4-142">V případě hodnoty true **stdout** a **stderr** pro proces zadaný v **processPath** přesměrováni na soubor zadaný v **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-142">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="c4ee4-143">Volitelný řetězec atributu.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-143">Optional string attribute.</span></span></p><p><span data-ttu-id="c4ee4-144">Určuje cestu k souboru relativní nebo absolutní, pro kterou **stdout** a **stderr** z procesu zadaný v **processPath** přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-144">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="c4ee4-145">Relativní cesty jsou relativní vůči kořenovému adresáři webu.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-145">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="c4ee4-146">Jakoukoli cestu, počínaje `.` jsou relativní vzhledem k webu kořenové a všechny ostatní cesty jsou považovány za absolutní cesty.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-146">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="c4ee4-147">Všechny složky zadaná v cestě musí existovat v pořadí pro modul pro vytvoření souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-147">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="c4ee4-148">Pomocí oddělovače podtržítko, časové razítko, ID procesu a přípona souboru (*.log*) jsou přidány na poslední segment **stdoutLogFile** cesta.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-148">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="c4ee4-149">Pokud `.\logs\stdout` je zadaný jako hodnota, je uložený protokol příklad stdout jako *stdout_20180205194132_1934.log* v *protokoly* složky při ukládání na 2/5/2018 na 19:41:32 s ID procesu sady 1934.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-149">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="c4ee4-150">Nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="c4ee4-150">Setting environment variables</span></span>

<span data-ttu-id="c4ee4-151">Pro proces v nástroji lze zadat proměnné prostředí `processPath` atribut.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-151">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="c4ee4-152">Zadejte proměnnou prostředí s `environmentVariable` podřízený element `environmentVariables` prvku kolekce.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-152">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="c4ee4-153">Nastavení v této části proměnné prostředí mají přednost před systémové proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-153">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="c4ee4-154">Následující příklad ilustruje dvou proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-154">The following example sets two environment variables.</span></span> <span data-ttu-id="c4ee4-155">`ASPNETCORE_ENVIRONMENT` nakonfiguruje prostředí aplikace `Development`.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-155">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="c4ee4-156">Vývojář může dočasně nastavit, že tato hodnota *web.config* souboru, aby bylo možné vynutit [vývojáře výjimka stránky](xref:fundamentals/error-handling) načíst při ladění výjimku aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-156">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="c4ee4-157">`CONFIG_DIR` je příkladem proměnné uživatelské prostředí, kde byl vývojář zápis kódu, který čte hodnota při spuštění a vytvořit cestu pro načítání konfiguračního souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-157">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!WARNING]
> <span data-ttu-id="c4ee4-158">Nastavit pouze `ASPNETCORE_ENVIRONMENT` envirnonment proměnnou `Development` na přípravy a testování serverů, které nejsou dostupné k nedůvěryhodným sítím, jako je Internet.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-158">Only set the `ASPNETCORE_ENVIRONMENT` envirnonment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="c4ee4-159">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="c4ee4-159">app_offline.htm</span></span>

<span data-ttu-id="c4ee4-160">Pokud soubor s názvem *app_offline.htm* zjistil v kořenovém adresáři aplikace, modulu základní technologie ASP.NET pokusí korektně vypnout aplikaci a zastavit zpracování příchozích požadavků.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-160">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="c4ee4-161">Pokud aplikace stále běží po uplynutí počtu sekund, které jsou definované v `shutdownTimeLimit`, ASP.NET Core modul ukončí proces spuštěný.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-161">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="c4ee4-162">Když *app_offline.htm* je soubor k dispozici, modul ASP.NET Core reaguje na požadavky odesílání zpět obsah *app_offline.htm* souboru.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-162">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="c4ee4-163">Když *app_offline.htm* soubor bude odstraněn, příští žádosti o spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-163">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="c4ee4-164">Spuštění chybová stránka</span><span class="sxs-lookup"><span data-stu-id="c4ee4-164">Start-up error page</span></span>

<span data-ttu-id="c4ee4-165">Základní modul ASP.NET selže-li spustit proces back-end nebo spustí proces back-end ale selže tak, aby naslouchala na konfigurovaném portu, *502.5 selhání procesu* se zobrazí stav znaková stránka.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-165">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="c4ee4-166">Chcete-li potlačit tuto stránku a vrátit na stránku výchozího stavu služby IIS 502 kódu, použijte `disableStartUpErrorPage` atribut.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-166">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="c4ee4-167">Další informace o konfiguraci vlastních chybových zpráv najdete v tématu [chyby protokolu HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="c4ee4-167">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 proces selhání stav znakové stránky](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="c4ee4-169">Vytvoření protokolu a přesměrování</span><span class="sxs-lookup"><span data-stu-id="c4ee4-169">Log creation and redirection</span></span>

<span data-ttu-id="c4ee4-170">Základní modul ASP.NET přesměruje `stdout` a `stderr` protokoly na disk v případě `stdoutLogEnabled` a `stdoutLogFile` atributy `aspNetCore` element jsou nastavené.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-170">The ASP.NET Core Module redirects `stdout` and `stderr` logs to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="c4ee4-171">Všechny složky aplikace `stdoutLogFile` cesta, musí existovat v pořadí pro modul pro vytvoření souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-171">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="c4ee4-172">Časové razítko a soubor rozšíření se automaticky přidá, když se vytvoří soubor protokolu.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-172">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="c4ee4-173">Protokoly nejsou otáčet, pokud dojde k recyklování procesů nebo restartování.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-173">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="c4ee4-174">Je zodpovědností hostitel k omezení místa na disku, které využívají protokol.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-174">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span> <span data-ttu-id="c4ee4-175">Pomocí `stdout` protokolu doporučujeme pouze pro řešení potíží při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-175">Using the `stdout` log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="c4ee4-176">Nepoužívejte stdout protokolu pro účely protokolování obecné aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-176">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="c4ee4-177">Pro běžné protokolování v aplikaci ASP.NET Core, použijte knihovnu protokolování, který omezuje velikost souboru protokolu a otočí protokoly.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-177">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="c4ee4-178">Další informace najdete v tématu [poskytovatelů třetích stran protokolování](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="c4ee4-178">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="c4ee4-179">Název souboru protokolu se skládá připojením časové razítko, ID procesu a přípona souboru (*.log*) na poslední segment `stdoutLogFile` cesta (obvykle *stdout*) oddělená podtržítka.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-179">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="c4ee4-180">Pokud `stdoutLogFile` cesta končí *stdout*, má název souboru protokolu pro aplikace pomocí PID 1934 na 2/5/2018 vytvořit na 19:42:32 *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-180">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="c4ee4-181">Následující ukázka `aspNetCore` element konfiguruje `stdout` protokolování pro aplikaci hostovanou v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-181">The following sample `aspNetCore` element configures `stdout` logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="c4ee4-182">Místní cestu nebo cestu sdílené síťové složky je přijatelné pro místní protokolování.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-182">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="c4ee4-183">Zkontrolujte, jestli uživatel identita fondu aplikací má oprávnění k zápisu do zadané cestě.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-183">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="c4ee4-184">V tématu [konfigurace s web.config](#configuration-with-webconfig) příklad `aspNetCore` element v *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-184">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="c4ee4-185">Modul ASP.NET Core s službu IIS sdílenou konfiguraci</span><span class="sxs-lookup"><span data-stu-id="c4ee4-185">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="c4ee4-186">Základní modul ASP.NET instalační program spustí s oprávněními **systému** účtu.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-186">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="c4ee4-187">Vzhledem k tomu, že místní systémový účet nemá změnit oprávnění pro cestu ke sdílené složce používá sdílené konfiguraci IIS, instalační program dotkne chybu při pokusu o konfiguraci nastavení modulu v odepření přístupu *applicationHost.config* ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-187">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="c4ee4-188">Pokud používáte sdílenou konfiguraci IIS, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="c4ee4-188">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="c4ee4-189">Zakážete sdílenou konfiguraci IIS.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-189">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="c4ee4-190">Spusťte instalační program.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-190">Run the installer.</span></span>
1. <span data-ttu-id="c4ee4-191">Export aktualizovaný *applicationHost.config* soubor do sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-191">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="c4ee4-192">Znovu povolte sdílenou konfiguraci IIS.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-192">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="c4ee4-193">Verze modulu a hostování sady Instalační protokoly</span><span class="sxs-lookup"><span data-stu-id="c4ee4-193">Module version and hosting bundle installer logs</span></span>

<span data-ttu-id="c4ee4-194">Chcete-li zjistit verzi nainstalovaný modul jádro ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="c4ee4-194">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="c4ee4-195">V hostitelském systému, přejděte na *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-195">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="c4ee4-196">Vyhledejte *aspnetcore.dll* souboru.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-196">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="c4ee4-197">Klikněte pravým tlačítkem na soubor a vyberte **vlastnosti** v kontextové nabídce.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-197">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="c4ee4-198">Vyberte **podrobnosti** kartě. **Verze souboru** a **verze produktu** představují nainstalovaná verze modulu.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-198">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="c4ee4-199">Hostování v systému Windows Server protokoly instalační program sady pro modul se nacházejí v *C:\\uživatelé\\% UserName %\\data aplikací\\místní\\Temp*. Název souboru *dd_DotNetCoreWinSvrHosting__\<časové razítko > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-199">The Windows Server Hosting bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="c4ee4-200">Umístění souborů modulu, schémat a konfigurace</span><span class="sxs-lookup"><span data-stu-id="c4ee4-200">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="c4ee4-201">Modul</span><span class="sxs-lookup"><span data-stu-id="c4ee4-201">Module</span></span>

<span data-ttu-id="c4ee4-202">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="c4ee4-202">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="c4ee4-203">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c4ee4-203">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="c4ee4-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c4ee4-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="c4ee4-205">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="c4ee4-205">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="c4ee4-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c4ee4-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="c4ee4-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c4ee4-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="c4ee4-208">Schéma</span><span class="sxs-lookup"><span data-stu-id="c4ee4-208">Schema</span></span>

<span data-ttu-id="c4ee4-209">**IIS**</span><span class="sxs-lookup"><span data-stu-id="c4ee4-209">**IIS**</span></span>

   * <span data-ttu-id="c4ee4-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="c4ee4-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="c4ee4-211">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="c4ee4-211">**IIS Express**</span></span>

   * <span data-ttu-id="c4ee4-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="c4ee4-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="c4ee4-213">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="c4ee4-213">Configuration</span></span>

<span data-ttu-id="c4ee4-214">**IIS**</span><span class="sxs-lookup"><span data-stu-id="c4ee4-214">**IIS**</span></span>

   * <span data-ttu-id="c4ee4-215">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="c4ee4-215">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="c4ee4-216">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="c4ee4-216">**IIS Express**</span></span>

   * <span data-ttu-id="c4ee4-217">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="c4ee4-217">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="c4ee4-218">Soubory naleznete vyhledáním *aspnetcore.dll* v *applicationHost.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-218">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="c4ee4-219">Pro službu IIS Express *applicationHost.config* soubor nebude existovat ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-219">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="c4ee4-220">Soubor je vytvořen v  *\<application_root >\\neodstraňujte\\konfigurace* při spuštění jakékoli projekt webové aplikace v řešení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4ee4-220">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
