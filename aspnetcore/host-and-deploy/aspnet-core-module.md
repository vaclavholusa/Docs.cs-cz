---
title: Referenční informace o ASP.NET Core modulu Konfigurace
author: guardrex
description: Zjistěte, jak nakonfigurovat modul ASP.NET Core pro hostování aplikací ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 0ae19b26bc86c9da7a61f3117aaae1844115593a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913278"
---
# <a name="aspnet-core-module-configuration-reference"></a>Referenční informace o ASP.NET Core modulu Konfigurace

Podle [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), a [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Tento dokument obsahuje pokyny o tom, jak nakonfigurovat modul ASP.NET Core pro hostování aplikací ASP.NET Core. Úvod do modul ASP.NET Core a pokyny k instalaci, najdete v článku [modul ASP.NET Core přehled](xref:fundamentals/servers/aspnet-core-module).

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a>Model hostingu

Modul pro aplikace běžící na .NET Core 2.2 nebo vyšší, podporuje model hostování v procesu za účelem vylepšení výkonu v porovnání s hostitelem (out-of-process) reverzních proxy serverů. Další informace naleznete v tématu <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.

Hostování v procsess je vyjádřit výslovný souhlas pro existující aplikace, ale [dotnet nové](/dotnet/core/tools/dotnet-new) výchozí šablony na model hostingu v procesu pro všechny služby IIS a služby IIS Express.

Jak nakonfigurovat aplikaci pro hostování v procesu, přidejte `<AspNetCoreModuleHostingModel>` vlastnosti do souboru projektu vaší aplikace s hodnotou `inprocess` (hostování mimo proces se nastaví pomocí `outofprocess`):

```xml
<PropertyGroup>
  <AspNetCoreModuleHostingModel>inprocess</AspNetCoreModuleHostingModel>
</PropertyGroup>
```

Při hostování v procesu platí následující vlastnosti:

* [Kestrel server](xref:fundamentals/servers/kestrel) se nepoužívá. Vlastní <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementaci `IISHttpServer` funguje jako server aplikace.

* [RequestTimeout atribut](#attributes-of-the-aspnetcore-element) neplatí pro hostování v procesu.

* Sdílení fondem aplikací mezi aplikacemi se nepodporuje. Použijte jeden fond aplikací na aplikaci.

* Při použití [Webdeploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) nebo ručně uvedení [soubor app_offline.htm v nasazení](xref:host-and-deploy/iis/index#locked-deployment-files), aplikace nemusí vypnout okamžitě při otevření připojení. Připojení soketu websocket bylo třeba může způsobit prodlevu při ukončení aplikace.

* Architektura (bitové verze) (x64 nebo x86) nainstalovaný modul runtime a aplikace musí odpovídat architektuře fondu aplikací.

* Pokud nastavení aplikace hostitele ručně pomocí `WebHostBuilder` (bez použití [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) a spuštění aplikace se nikdy přímo na serveru Kestrel (v místním prostředí), volání `UseKestrel` před voláním `UseIISIntegration`. Pokud je obrácený pořadí, hostitel se nepodaří spustit.

### <a name="hosting-model-changes"></a>Hostování změny modelu

Pokud `hostingModel` nastavení se změnilo v *web.config* souboru (podrobně [konfigurace pomocí souboru web.config](#configuration-with-webconfig) části), modul pro službu IIS recykluje pracovní proces.

Pro službu IIS Express modul nebude recyklovat pracovní proces, ale místo toho aktivuje řádné vypnutí aktuální proces IIS Express. Další požadavek na aplikaci vytvoří nový proces IIS Express.

### <a name="process-name"></a>Název procesu

`Process.GetCurrentProcess().ProcessName` sestavy buď `w3wp` (v procesu) nebo `dotnet` (out-of-process).

::: moniker-end

## <a name="configuration-with-webconfig"></a>Konfigurace pomocí souboru web.config

Modul ASP.NET Core, nastavena `aspNetCore` část `system.webServer` uzlu na webu *web.config* souboru.

Následující *web.config* soubor je publikován pro [nasazení závisí na architektuře](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) a konfiguruje modul ASP.NET Core pro zpracování požadavků lokality:

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

Následující *web.config* je publikována pro [samostatná nasazení](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

Když je aplikace nasazená na [služby Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` je nastavena cesta `\\?\%home%\LogFiles\stdout`. Protokoly stdout a uloží ji *LogFiles* složce, která je na místě automaticky vytvořené službou.

Naleznete v tématu [dílčí aplikace konfigurace](xref:host-and-deploy/iis/index#sub-application-configuration) pro důležitou poznámku týkající se konfigurace *web.config* soubory v dílčí aplikace.

### <a name="attributes-of-the-aspnetcore-element"></a>Atributy elementu aspNetCore

::: moniker range=">= aspnetcore-2.2"

| Atribut | Popis | Výchozí |
| --------- | ----------- | :-----: |
| `arguments` | <p>Volitelný atribut řetězce.</p><p>Argumenty pro spustitelný soubor určený v **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>Volitelný logický atribut.</p><p>Při hodnotě true **502.5 – selhání procesu** Potlačené stránky a znakovou stránku 502 stav nakonfigurované v *web.config* má přednost před.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Volitelný logický atribut.</p><p>Pokud je hodnota true, předán token podřízený proces naslouchání na % ASPNETCORE_PORT % jako hlavičku "MS-ASPNETCORE-WINAUTHTOKEN" na základě požadavku. Je odpovědností tento proces pro volání CloseHandle na tento token každý požadavek.</p> | `true` |
| `hostingModel` | <p>Volitelný atribut řetězce.</p><p>Určuje model hostování jako v procesu (`inprocess`) nebo out-of-process (`outofprocess`).</p> | `outofprocess` |
| `processesPerApplication` | <p>Volitelný celočíselný atribut.</p><p>Určuje počet instancí procesu zadaný v **processPath** nastavení, které lze se nespředený nahoru na app.</p><p>&dagger;Hostování v procesu, je omezena na hodnotu `1`.</p> | Výchozí hodnota: `1`<br>Min: `1`<br>Max: `100`&dagger; |
| `processPath` | <p>Požadovaný atribut typu string.</p><p>Cesta ke spustitelnému souboru, který spustí nějaký proces naslouchání požadavků protokolu HTTP. Jsou podporovány relativní cesty. Pokud cestu začíná `.`, cesta se považuje za kořeni webu.</p> | |
| `rapidFailsPerMinute` | <p>Volitelný celočíselný atribut.</p><p>Určuje, kolikrát podle procesu **processPath** může při selhání za minutu. Pokud je tento limit překročen, modulu zastaví spuštění procesu pro zbytek minuty.</p><p>Není podporováno s hostitelem v procesu.</p> | Výchozí hodnota: `10`<br>Min: `0`<br>Max: `100` |
| `requestTimeout` | <p>Atribut volitelný časový interval.</p><p>Určuje dobu trvání, pro které základní modul ASP.NET čeká na odpověď proces, který poslouchá na % ASPNETCORE_PORT %.</p><p>Ve verzích modul ASP.NET Core dodávané s verzí technologie ASP.NET Core 2.1 nebo novější `requestTimeout` je zadán v hodiny, minuty a sekundy.</p><p>Neplatí pro hostování v procesu. Hostování v procesu, čeká modul app ke zpracování požadavku.</p> | Výchozí hodnota: `00:02:00`<br>Min: `00:00:00`<br>Max: `360:00:00` |
| `shutdownTimeLimit` | <p>Volitelný celočíselný atribut.</p><p>Doba v sekundách, po kterou čeká modul pro spustitelný soubor k řádné vypnutí při *app_offline.htm* souboru je zjištěn.</p> | Výchozí hodnota: `10`<br>Min: `0`<br>Max: `600` |
| `startupTimeLimit` | <p>Volitelný celočíselný atribut.</p><p>Doba trvání v sekundách, které modul čeká na spustitelný soubor ke spuštění procesu naslouchání na portu. Pokud je překročen časový limit, modul ukončí proces. Modul se pokusí znovu spustit proces, když ji dostane novou žádost a pokusit se restartovat proces na dalších příchozích požadavků, pokud se aplikaci nepodaří spustit **rapidFailsPerMinute** málokdy za posledních minuta se zajištěním provozu.</p><p>Hodnota 0 (nula) je **ne** za neomezený časový limit.</p> | Výchozí hodnota: `120`<br>Min: `0`<br>Max: `3600` |
| `stdoutLogEnabled` | <p>Volitelný logický atribut.</p><p>Při hodnotě true se **stdout** a **stderr** pro proces určený v **processPath** se přesměrují do souboru zadaného v **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Volitelný atribut řetězce.</p><p>Určuje relativní nebo absolutní cestu, pro kterou **stdout** a **stderr** z procesu podle **processPath** přihlášeni. Relativní cesty jsou relativní vzhledem k kořen webu. Libovolnou cestu od `.` jsou relativní vzhledem k webu kořenové a všechny ostatní cesty jsou považovány za absolutní cesty. Všechny složky v cestě k dispozici musí existovat v pořadí pro modul pro vytvoření souboru protokolu. Pomocí oddělovače podtržítko, časové razítko, ID procesu a přípona souboru (*.log*) jsou přidány na poslední segment **stdoutLogFile** cestu. Pokud `.\logs\stdout` se dodává jako hodnotu protokolu příklad stdout je uložen jako *stdout_20180205194132_1934.log* v *protokoly* složky při ukládání na 2, 5/2018 v 19:41:32 s ID procesu z roku 1934.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| Atribut | Popis | Výchozí |
| --------- | ----------- | :-----: |
| `arguments` | <p>Volitelný atribut řetězce.</p><p>Argumenty pro spustitelný soubor určený v **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>Volitelný logický atribut.</p><p>Při hodnotě true **502.5 – selhání procesu** Potlačené stránky a znakovou stránku 502 stav nakonfigurované v *web.config* má přednost před.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Volitelný logický atribut.</p><p>Pokud je hodnota true, předán token podřízený proces naslouchání na % ASPNETCORE_PORT % jako hlavičku "MS-ASPNETCORE-WINAUTHTOKEN" na základě požadavku. Je odpovědností tento proces pro volání CloseHandle na tento token každý požadavek.</p> | `true` |
| `processesPerApplication` | <p>Volitelný celočíselný atribut.</p><p>Určuje počet instancí procesu zadaný v **processPath** nastavení, které lze se nespředený nahoru na app.</p> | Výchozí hodnota: `1`<br>Min: `1`<br>Max: `100` |
| `processPath` | <p>Požadovaný atribut typu string.</p><p>Cesta ke spustitelnému souboru, který spustí nějaký proces naslouchání požadavků protokolu HTTP. Jsou podporovány relativní cesty. Pokud cestu začíná `.`, cesta se považuje za kořeni webu.</p> | |
| `rapidFailsPerMinute` | <p>Volitelný celočíselný atribut.</p><p>Určuje, kolikrát podle procesu **processPath** může při selhání za minutu. Pokud je tento limit překročen, modulu zastaví spuštění procesu pro zbytek minuty.</p> | Výchozí hodnota: `10`<br>Min: `0`<br>Max: `100` |
| `requestTimeout` | <p>Atribut volitelný časový interval.</p><p>Určuje dobu trvání, pro které základní modul ASP.NET čeká na odpověď proces, který poslouchá na % ASPNETCORE_PORT %.</p><p>Ve verzích modul ASP.NET Core dodávané s verzí technologie ASP.NET Core 2.1 nebo novější `requestTimeout` je zadán v hodiny, minuty a sekundy.</p> | Výchozí hodnota: `00:02:00`<br>Min: `00:00:00`<br>Max: `360:00:00` |
| `shutdownTimeLimit` | <p>Volitelný celočíselný atribut.</p><p>Doba v sekundách, po kterou čeká modul pro spustitelný soubor k řádné vypnutí při *app_offline.htm* souboru je zjištěn.</p> | Výchozí hodnota: `10`<br>Min: `0`<br>Max: `600` |
| `startupTimeLimit` | <p>Volitelný celočíselný atribut.</p><p>Doba trvání v sekundách, které modul čeká na spustitelný soubor ke spuštění procesu naslouchání na portu. Pokud je překročen časový limit, modul ukončí proces. Modul se pokusí znovu spustit proces, když ji dostane novou žádost a pokusit se restartovat proces na dalších příchozích požadavků, pokud se aplikaci nepodaří spustit **rapidFailsPerMinute** málokdy za posledních minuta se zajištěním provozu.</p><p>Hodnota 0 (nula) je **ne** za neomezený časový limit.</p> | Výchozí hodnota: `120`<br>Min: `0`<br>Max: `3600` |
| `stdoutLogEnabled` | <p>Volitelný logický atribut.</p><p>Při hodnotě true se **stdout** a **stderr** pro proces určený v **processPath** se přesměrují do souboru zadaného v **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Volitelný atribut řetězce.</p><p>Určuje relativní nebo absolutní cestu, pro kterou **stdout** a **stderr** z procesu podle **processPath** přihlášeni. Relativní cesty jsou relativní vzhledem k kořen webu. Libovolnou cestu od `.` jsou relativní vzhledem k webu kořenové a všechny ostatní cesty jsou považovány za absolutní cesty. Všechny složky v cestě k dispozici musí existovat v pořadí pro modul pro vytvoření souboru protokolu. Pomocí oddělovače podtržítko, časové razítko, ID procesu a přípona souboru (*.log*) jsou přidány na poslední segment **stdoutLogFile** cestu. Pokud `.\logs\stdout` se dodává jako hodnotu protokolu příklad stdout je uložen jako *stdout_20180205194132_1934.log* v *protokoly* složky při ukládání na 2, 5/2018 v 19:41:32 s ID procesu z roku 1934.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| Atribut | Popis | Výchozí |
| --------- | ----------- | :-----: |
| `arguments` | <p>Volitelný atribut řetězce.</p><p>Argumenty pro spustitelný soubor určený v **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>Volitelný logický atribut.</p><p>Při hodnotě true **502.5 – selhání procesu** Potlačené stránky a znakovou stránku 502 stav nakonfigurované v *web.config* má přednost před.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Volitelný logický atribut.</p><p>Pokud je hodnota true, předán token podřízený proces naslouchání na % ASPNETCORE_PORT % jako hlavičku "MS-ASPNETCORE-WINAUTHTOKEN" na základě požadavku. Je odpovědností tento proces pro volání CloseHandle na tento token každý požadavek.</p> | `true` |
| `processesPerApplication` | <p>Volitelný celočíselný atribut.</p><p>Určuje počet instancí procesu zadaný v **processPath** nastavení, které lze se nespředený nahoru na app.</p> | Výchozí hodnota: `1`<br>Min: `1`<br>Max: `100` |
| `processPath` | <p>Požadovaný atribut typu string.</p><p>Cesta ke spustitelnému souboru, který spustí nějaký proces naslouchání požadavků protokolu HTTP. Jsou podporovány relativní cesty. Pokud cestu začíná `.`, cesta se považuje za kořeni webu.</p> | |
| `rapidFailsPerMinute` | <p>Volitelný celočíselný atribut.</p><p>Určuje, kolikrát podle procesu **processPath** může při selhání za minutu. Pokud je tento limit překročen, modulu zastaví spuštění procesu pro zbytek minuty.</p> | Výchozí hodnota: `10`<br>Min: `0`<br>Max: `100` |
| `requestTimeout` | <p>Atribut volitelný časový interval.</p><p>Určuje dobu trvání, pro které základní modul ASP.NET čeká na odpověď proces, který poslouchá na % ASPNETCORE_PORT %.</p><p>Ve verzích modul ASP.NET Core dodávané s verzí technologie ASP.NET Core 2.0 nebo starší `requestTimeout` musí být zadán v celých minutách, v opačném případě výchozí hodnota 2 minuty.</p> | Výchozí hodnota: `00:02:00`<br>Min: `00:00:00`<br>Max: `360:00:00` |
| `shutdownTimeLimit` | <p>Volitelný celočíselný atribut.</p><p>Doba v sekundách, po kterou čeká modul pro spustitelný soubor k řádné vypnutí při *app_offline.htm* souboru je zjištěn.</p> | Výchozí hodnota: `10`<br>Min: `0`<br>Max: `600` |
| `startupTimeLimit` | <p>Volitelný celočíselný atribut.</p><p>Doba trvání v sekundách, které modul čeká na spustitelný soubor ke spuštění procesu naslouchání na portu. Pokud je překročen časový limit, modul ukončí proces. Modul se pokusí znovu spustit proces, když ji dostane novou žádost a pokusit se restartovat proces na dalších příchozích požadavků, pokud se aplikaci nepodaří spustit **rapidFailsPerMinute** málokdy za posledních minuta se zajištěním provozu.</p> | Výchozí hodnota: `120`<br>Min: `0`<br>Max: `3600` |
| `stdoutLogEnabled` | <p>Volitelný logický atribut.</p><p>Při hodnotě true se **stdout** a **stderr** pro proces určený v **processPath** se přesměrují do souboru zadaného v **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Volitelný atribut řetězce.</p><p>Určuje relativní nebo absolutní cestu, pro kterou **stdout** a **stderr** z procesu podle **processPath** přihlášeni. Relativní cesty jsou relativní vzhledem k kořen webu. Libovolnou cestu od `.` jsou relativní vzhledem k webu kořenové a všechny ostatní cesty jsou považovány za absolutní cesty. Všechny složky v cestě k dispozici musí existovat v pořadí pro modul pro vytvoření souboru protokolu. Pomocí oddělovače podtržítko, časové razítko, ID procesu a přípona souboru (*.log*) jsou přidány na poslední segment **stdoutLogFile** cestu. Pokud `.\logs\stdout` se dodává jako hodnotu protokolu příklad stdout je uložen jako *stdout_20180205194132_1934.log* v *protokoly* složky při ukládání na 2, 5/2018 v 19:41:32 s ID procesu z roku 1934.</p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a>Nastavení proměnných prostředí

Proměnné prostředí se dá nastavit pro proces v `processPath` atribut. Zadat proměnné prostředí s `environmentVariable` podřízený prvek `environmentVariables` prvek kolekce. Proměnné prostředí nastavené v této části přednost systémové proměnné prostředí.

Následující příklad nastaví dvou proměnných prostředí. `ASPNETCORE_ENVIRONMENT` nakonfiguruje prostředí aplikace tak, aby `Development`. Vývojáři mohou dočasně nastaví tuto hodnotu *web.config* souboru, aby bylo možné vynutit [stránku výjimek pro vývojáře](xref:fundamentals/error-handling) načíst při ladění aplikace výjimky. `CONFIG_DIR` je příkladem proměnné prostředí, kam má vývojář zapisovat kód, který čte hodnoty při spuštění tvoří cestu pro načtení konfiguračního souboru aplikace.

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
> Nastavit pouze `ASPNETCORE_ENVIRONMENT` proměnnou prostředí, aby `Development` na přípravy a testování serverů, které nejsou dostupné k nedůvěryhodným sítím, jako je Internet.

## <a name="appofflinehtm"></a>app_offline.htm

Pokud soubor s názvem *app_offline.htm* je zjištěna v kořenovém adresáři aplikace, že modul ASP.NET Core se pokusí řádné vypnutí aplikace a zastavit zpracování příchozích požadavků. Pokud aplikace pořád běží po dobu v sekundách podle `shutdownTimeLimit`, že modul ASP.NET Core ukončuje spuštěnému procesu.

Zatímco *app_offline.htm* soubor je k dispozici, že modul ASP.NET Core reaguje na požadavky odesílá zpět obsah *app_offline.htm* souboru. Když *app_offline.htm* soubor bude odstraněn, příští žádosti o spuštění aplikace.

::: moniker range=">= aspnetcore-2.2"

Pokud používáte model hostingu mimo proces, aplikace nemusí vypnout okamžitě při otevření připojení. Připojení soketu websocket bylo třeba může způsobit prodlevu při ukončení aplikace.

::: moniker-end

## <a name="start-up-error-page"></a>Spuštění chybovou stránku

::: moniker range=">= aspnetcore-2.2"

*Platí jenom pro hostování mimo proces.*

::: moniker-end

Pokud se nespustí back-endový proces nebo back-endový proces spustí ale nebude moci poslouchat na konfigurovaném portu, že modul ASP.NET Core *502.5 selhání procesu* se zobrazí stav znakovou stránku. Chcete-li potlačit tuto stránku a vrátit se na výchozí stav služby IIS 502 znakovou stránku, použijte `disableStartUpErrorPage` atribut. Další informace o konfiguraci vlastních chybových zpráv, najdete v části [chyby protokolu HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![502.5 proces selhání stav znaková stránka](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Vytvoření protokolu a přesměrování

Modul ASP.NET Core přesměruje výstup stdout a stderr console na disk, pokud `stdoutLogEnabled` a `stdoutLogFile` atributy `aspNetCore` element jsou nastavené. Všechny složky v `stdoutLogFile` cesta musí existovat v pořadí pro modul pro vytvoření souboru protokolu. Fond aplikací musí mít oprávnění k zápisu do umístění, ve kterém jsou zapsány protokoly (použijte `IIS AppPool\<app_pool_name>` poskytnout oprávnění k zápisu).

Protokoly nejsou střídán, pokud dojde k recyklování procesů/restartování. Je odpovědností hostitel k omezení místa na disku, které využijete v protokolech.

Použití protokolu stdout se doporučuje jenom pro řešení potíží při spuštění aplikace. Nepoužívejte stdout protokolu pro účely protokolování obecné aplikace. Pro rutiny protokolování v aplikaci ASP.NET Core, použijte protokolování knihovnu, která omezuje velikost souboru protokolu a otočí protokoly. Další informace najdete v tématu [zprostředkovatele přihlášení třetí strany](xref:fundamentals/logging/index#third-party-logging-providers).

Časové razítko a soubor rozšíření jsou přidány automaticky při vytvoření souboru protokolu. Název souboru protokolu se skládá připojením časové razítko, ID procesu a přípona souboru (*.log*) na poslední segment `stdoutLogFile` cestu (obvykle *stdout*) oddělené podtržítka. Pokud `stdoutLogFile` cesta končí *stdout*, má název souboru protokolu pro aplikaci s PID 1934 vytvořili 5 2. 2018 v 19:42:32 *stdout_20180205194132_1934.log*.

Následující ukázka `aspNetCore` element konfiguruje stdout protokolování pro aplikace hostované ve službě Azure App Service. Místní cesta nebo cesta ke sdílené položce je přijatelné pro místní přihlášení. Ověřte, jestli uživatel identita fondu aplikací má oprávnění k zápisu do zadaná výstupní cesta.

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

Naleznete v tématu [konfigurace pomocí souboru web.config](#configuration-with-webconfig) příklad `aspNetCore` prvek *web.config* souboru.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfigurace proxy serveru používá protokol HTTP a token pro párování

::: moniker range=">= aspnetcore-2.2"

*Platí jenom pro hostování mimo proces.*

::: moniker-end

Proxy server mezi modul ASP.NET Core a Kestrel používá protokol HTTP. Pomocí protokolu HTTP je optimalizace výkonu, kdy přenos dat mezi modulu a Kestrel probíhá na adresu zpětné smyčky ze síťového rozhraní. Neexistuje žádné riziko odposloucháváním provozu mezi modulu a Kestrel z umístění mimo server.

Párování token se používá k zajištění, že požadavků přijatých službou Kestrel byly směrovány přes proxy server službou IIS a nebyl dodán z nějakého jiného zdroje. Vytvoření a nastavení do proměnné prostředí párování token (`ASPNETCORE_TOKEN`) modulu. Párování token byl nastavený i do záhlaví (`MSAspNetCoreToken`) na všechny požadavky směrovány přes proxy server. Služba IIS Middleware kontroly požadavku že přijme potvrďte, že odpovídá párování hodnota tokenu hlavičky hodnotu proměnné prostředí. Pokud token hodnoty se neshodují, žádost je zaznamenána a odmítnut. Párování proměnná tokenu prostředí a přenos dat mezi modulu a Kestrel nejsou dostupné z umístění mimo server. Nainstalovat bez mého párování hodnota tokenu nelze útočník odesílat požadavky, které obejít kontrolu v IIS middlewaru.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Modul ASP.NET Core s službu IIS sdílenou konfiguraci

Modul ASP.NET Core instalační program spustí s oprávněními **systému** účtu. Vzhledem k tomu, že místní systémový účet mít nezmění oprávnění pro sdílenou složku cesta používaná systémem sdílené konfiguraci IIS, instalační program narazí na chybu při pokusu o konfiguraci nastavení modulu v odepření přístupu *applicationHost.config* ve sdílené složce. Pokud používáte sdílenou konfiguraci IIS, postupujte podle těchto kroků:

1. Zakážete sdílenou konfiguraci IIS.
1. Spusťte instalační program.
1. Export aktualizovaný *applicationHost.config* souboru do sdílené složky.
1. Znovu povolte sdílenou konfiguraci IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Verze modulu a hostování sady Instalační protokoly

Chcete-li zjistit verzi nainstalovaný modul ASP.NET Core:

1. V hostitelském systému, přejděte na *%windir%\System32\inetsrv*.
1. Vyhledejte *aspnetcore.dll* souboru.
1. Klikněte pravým tlačítkem na soubor a vyberte **vlastnosti** z kontextové nabídky.
1. Vyberte **podrobnosti** kartu. **Verze souboru** a **verze produktu** představují nainstalovaná verze modulu.

Protokoly instalační balík hostování modulu se nacházejí v *C:\\uživatelů\\% UserName %\\AppData\\místní\\Temp*. Soubor se nazývá *dd_DotNetCoreWinSvrHosting__\<razítka > _000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Modul, schéma a konfiguraci umístění souborů

### <a name="module"></a>Modul

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**Služba IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schéma

**SLUŽBA IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**Služba IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Konfigurace

**SLUŽBA IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**Služba IIS Express**

   * .vs\config\applicationHost.config

Soubory můžete najít tak, že *aspnetcore.dll* v *applicationHost.config* souboru. Pro službu IIS Express *applicationHost.config* soubor neexistují ve výchozím nastavení. Soubor je vytvořen v  *\<application_root >\\.vs\\config* při spouštění libovolného webového projektu aplikace Visual Studio řešení.
