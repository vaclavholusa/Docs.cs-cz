---
title: Odkaz na konfiguraci základní modul ASP.NET
author: guardrex
description: Postup konfigurace modulu jádra ASP.NET pro hostování aplikací ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 954841a1b1465c80e60d5745ad9e22294a88fdf4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483744"
---
# <a name="aspnet-core-module-configuration-reference"></a>Odkaz na konfiguraci základní modul ASP.NET

Podle [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), a [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Tento dokument obsahuje pokyny ke konfiguraci základní modul ASP.NET pro hostování aplikací ASP.NET Core. Úvod do modulu jádra ASP.NET a pokyny k instalaci, najdete v článku [ASP.NET Core modulu přehled](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-with-webconfig"></a>Konfigurace se soubor web.config

Modul ASP.NET Core nastavena `aspNetCore` části `system.webServer` uzlu na webu *web.config* souboru.

Následující *web.config* souboru je publikována pro [nasazení závislé na framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) a nakonfiguruje modul ASP.NET jádra pro zpracování požadavků lokality:

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

Následující *web.config* je publikována pro [samostatná nasazení](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

Když je aplikace nasazená na [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` cesta je nastavena na `\\?\%home%\LogFiles\stdout`. Cesta uloží protokoly stdout *LogFiles* složky, která je umístění automaticky vytvořených službou.

V tématu [dílčí aplikace konfigurace](xref:host-and-deploy/iis/index#sub-application-configuration) pro důležitá poznámka týkající se konfigurace *web.config* soubory v dílčí aplikace.

### <a name="attributes-of-the-aspnetcore-element"></a>Atributy elementu aspNetCore

::: moniker range="<= aspnetcore-2.0"
| Atribut | Popis | Výchozí |
| --------- | ----------- | :-----: |
| `arguments` | <p>Volitelný řetězec atributu.</p><p>Argumenty pro spustitelný soubor určený v **processPath**.</p>| |
| `disableStartUpErrorPage` | Hodnota true nebo false</p><p>V případě hodnoty true **502.5 - selhání procesu** potlačeno stránky a stav 502 znaková stránka konfigurované v *web.config* má přednost před.</p> | `false` |
| `forwardWindowsAuthToken` | Hodnota true nebo false</p><p>V případě hodnoty true token se předají do podřízeného procesu naslouchání na ASPNETCORE_PORT % jako hlavičku, MS-ASPNETCORE-WINAUTHTOKEN' každý požadavek. Je zodpovědností procesu pro volání funkce CloseHandle na tento token na základě požadavku.</p> | `true` |
| `processPath` | <p>Požadovaný atribut typu string.</p><p>Cesta ke spustitelnému souboru, který spustí proces naslouchání požadavkům HTTP. Jsou podporovány relativní cesty. Pokud cesta začíná `.`, cesta se považuje za relativní vůči kořenovému adresáři webu.</p> | |
| `rapidFailsPerMinute` | <p>Atribut volitelné celé číslo.</p><p>Určuje, kolikrát proces zadaný v **processPath** je dovoleno havárií za minutu. Pokud je tento limit překročen, modul zastaví spuštění procesu pro zbytek minutu.</p> | `10` |
| `requestTimeout` | <p>Atribut volitelné časový interval.</p><p>Určuje dobu, pro který modul ASP.NET jádra čeká na odpověď z procesu naslouchání na ASPNETCORE_PORT %.</p><p>Ve verzích ASP.NET základní modul, který dodává s verzí technologie ASP.NET 2.0 jádra nebo dřívější `requestTimeout` musí být zadaná v celé minut, v opačném případě výchozí hodnota 2 minuty.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Atribut volitelné celé číslo.</p><p>Doba v sekundách, kterou čeká modul pro spustitelný soubor řádně vypnutí při *app_offline.htm* je detekován soubor.</p> | `10` |
| `startupTimeLimit` | <p>Atribut volitelné celé číslo.</p><p>Doba v sekundách, kterou čeká modul pro spuštění procesu naslouchání na portu spustitelný soubor. Pokud je tento časový limit překročen, modul ukončí proces. Modul se pokusí spustit proces, při přijetí nového požadavku a pokračuje v pokusí restartovat proces na následné příchozí žádosti, pokud se nepodaří spustit aplikaci **rapidFailsPerMinute** počet, kolikrát za posledních vrácení minutu.</p> | `120` |
| `stdoutLogEnabled` | <p>Volitelný logický atribut.</p><p>V případě hodnoty true **stdout** a **stderr** pro proces zadaný v **processPath** přesměrováni na soubor zadaný v **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Volitelný řetězec atributu.</p><p>Určuje cestu k souboru relativní nebo absolutní, pro kterou **stdout** a **stderr** z procesu zadaný v **processPath** přihlášeni. Relativní cesty jsou relativní vůči kořenovému adresáři webu. Jakoukoli cestu, počínaje `.` jsou relativní vzhledem k webu kořenové a všechny ostatní cesty jsou považovány za absolutní cesty. Všechny složky zadaná v cestě musí existovat v pořadí pro modul pro vytvoření souboru protokolu. Pomocí oddělovače podtržítko, časové razítko, ID procesu a přípona souboru (*.log*) jsou přidány na poslední segment **stdoutLogFile** cesta. Pokud `.\logs\stdout` je zadaný jako hodnota, je uložený protokol příklad stdout jako *stdout_20180205194132_1934.log* v *protokoly* složky při ukládání na 2/5/2018 na 19:41:32 s ID procesu sady 1934.</p> | `aspnetcore-stdout` |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Atribut | Popis | Výchozí |
| --------- | ----------- | :-----: |
| `arguments` | <p>Volitelný řetězec atributu.</p><p>Argumenty pro spustitelný soubor určený v **processPath**.</p>| |
| `disableStartUpErrorPage` | Hodnota true nebo false</p><p>V případě hodnoty true **502.5 - selhání procesu** potlačeno stránky a stav 502 znaková stránka konfigurované v *web.config* má přednost před.</p> | `false` |
| `forwardWindowsAuthToken` | Hodnota true nebo false</p><p>V případě hodnoty true token se předají do podřízeného procesu naslouchání na ASPNETCORE_PORT % jako hlavičku, MS-ASPNETCORE-WINAUTHTOKEN' každý požadavek. Je zodpovědností procesu pro volání funkce CloseHandle na tento token na základě požadavku.</p> | `true` |
| `processPath` | <p>Požadovaný atribut typu string.</p><p>Cesta ke spustitelnému souboru, který spustí proces naslouchání požadavkům HTTP. Jsou podporovány relativní cesty. Pokud cesta začíná `.`, cesta se považuje za relativní vůči kořenovému adresáři webu.</p> | |
| `rapidFailsPerMinute` | <p>Atribut volitelné celé číslo.</p><p>Určuje, kolikrát proces zadaný v **processPath** je dovoleno havárií za minutu. Pokud je tento limit překročen, modul zastaví spuštění procesu pro zbytek minutu.</p> | `10` |
| `requestTimeout` | <p>Atribut volitelné časový interval.</p><p>Určuje dobu, pro který modul ASP.NET jádra čeká na odpověď z procesu naslouchání na ASPNETCORE_PORT %.</p><p>Ve verzích ASP.NET základní modul, který dodaný ve verzi ASP.NET Core 2.1 nebo novější `requestTimeout` je uveden v hodiny, minuty a sekundy.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Atribut volitelné celé číslo.</p><p>Doba v sekundách, kterou čeká modul pro spustitelný soubor řádně vypnutí při *app_offline.htm* je detekován soubor.</p> | `10` |
| `startupTimeLimit` | <p>Atribut volitelné celé číslo.</p><p>Doba v sekundách, kterou čeká modul pro spuštění procesu naslouchání na portu spustitelný soubor. Pokud je tento časový limit překročen, modul ukončí proces. Modul se pokusí spustit proces, při přijetí nového požadavku a pokračuje v pokusí restartovat proces na následné příchozí žádosti, pokud se nepodaří spustit aplikaci **rapidFailsPerMinute** počet, kolikrát za posledních vrácení minutu.</p> | `120` |
| `stdoutLogEnabled` | <p>Volitelný logický atribut.</p><p>V případě hodnoty true **stdout** a **stderr** pro proces zadaný v **processPath** přesměrováni na soubor zadaný v **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Volitelný řetězec atributu.</p><p>Určuje cestu k souboru relativní nebo absolutní, pro kterou **stdout** a **stderr** z procesu zadaný v **processPath** přihlášeni. Relativní cesty jsou relativní vůči kořenovému adresáři webu. Jakoukoli cestu, počínaje `.` jsou relativní vzhledem k webu kořenové a všechny ostatní cesty jsou považovány za absolutní cesty. Všechny složky zadaná v cestě musí existovat v pořadí pro modul pro vytvoření souboru protokolu. Pomocí oddělovače podtržítko, časové razítko, ID procesu a přípona souboru (*.log*) jsou přidány na poslední segment **stdoutLogFile** cesta. Pokud `.\logs\stdout` je zadaný jako hodnota, je uložený protokol příklad stdout jako *stdout_20180205194132_1934.log* v *protokoly* složky při ukládání na 2/5/2018 na 19:41:32 s ID procesu sady 1934.</p> | `aspnetcore-stdout` |
::: moniker-end

### <a name="setting-environment-variables"></a>Nastavení proměnných prostředí

Pro proces v nástroji lze zadat proměnné prostředí `processPath` atribut. Zadejte proměnnou prostředí s `environmentVariable` podřízený element `environmentVariables` prvku kolekce. Nastavení v této části proměnné prostředí mají přednost před systémové proměnné prostředí.

Následující příklad ilustruje dvou proměnných prostředí. `ASPNETCORE_ENVIRONMENT` nakonfiguruje prostředí aplikace `Development`. Vývojář může dočasně nastavit, že tato hodnota *web.config* souboru, aby bylo možné vynutit [vývojáře výjimka stránky](xref:fundamentals/error-handling) načíst při ladění výjimku aplikace. `CONFIG_DIR` je příkladem proměnné uživatelské prostředí, kde byl vývojář zápis kódu, který čte hodnota při spuštění a vytvořit cestu pro načítání konfiguračního souboru aplikace.

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
> Nastavit pouze `ASPNETCORE_ENVIRONMENT` envirnonment proměnnou `Development` na přípravy a testování serverů, které nejsou dostupné k nedůvěryhodným sítím, jako je Internet.

## <a name="appofflinehtm"></a>app_offline.htm

Pokud soubor s názvem *app_offline.htm* zjistil v kořenovém adresáři aplikace, modulu základní technologie ASP.NET pokusí korektně vypnout aplikaci a zastavit zpracování příchozích požadavků. Pokud aplikace stále běží po uplynutí počtu sekund, které jsou definované v `shutdownTimeLimit`, ASP.NET Core modul ukončí proces spuštěný.

Když *app_offline.htm* je soubor k dispozici, modul ASP.NET Core reaguje na požadavky odesílání zpět obsah *app_offline.htm* souboru. Když *app_offline.htm* soubor bude odstraněn, příští žádosti o spuštění aplikace.

## <a name="start-up-error-page"></a>Spuštění chybová stránka

Základní modul ASP.NET selže-li spustit proces back-end nebo spustí proces back-end ale selže tak, aby naslouchala na konfigurovaném portu, *502.5 selhání procesu* se zobrazí stav znaková stránka. Chcete-li potlačit tuto stránku a vrátit na stránku výchozího stavu služby IIS 502 kódu, použijte `disableStartUpErrorPage` atribut. Další informace o konfiguraci vlastních chybových zpráv najdete v tématu [chyby protokolu HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![502.5 proces selhání stav znakové stránky](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Vytvoření protokolu a přesměrování

Modul ASP.NET Core přesměruje protokolů stdout a stderr na disk, pokud `stdoutLogEnabled` a `stdoutLogFile` atributy `aspNetCore` element jsou nastavené. Všechny složky aplikace `stdoutLogFile` cesta, musí existovat v pořadí pro modul pro vytvoření souboru protokolu. Fond aplikací musí mít oprávnění k zápisu do umístění, kde se protokoly zapisují (použijte `IIS AppPool\<app_pool_name>` poskytnout oprávnění k zápisu).

Protokoly nejsou otáčet, pokud dojde k recyklování procesů nebo restartování. Je zodpovědností hostitel k omezení místa na disku, které využívají protokol.

Použití protokolu stdout se doporučuje jenom pro řešení potíží při spuštění aplikace. Nepoužívejte stdout protokolu pro účely protokolování obecné aplikace. Pro běžné protokolování v aplikaci ASP.NET Core, použijte knihovnu protokolování, který omezuje velikost souboru protokolu a otočí protokoly. Další informace najdete v tématu [poskytovatelů třetích stran protokolování](xref:fundamentals/logging/index#third-party-logging-providers).

Časové razítko a soubor rozšíření se automaticky přidá, když se vytvoří soubor protokolu. Název souboru protokolu se skládá připojením časové razítko, ID procesu a přípona souboru (*.log*) na poslední segment `stdoutLogFile` cesta (obvykle *stdout*) oddělená podtržítka. Pokud `stdoutLogFile` cesta končí *stdout*, má název souboru protokolu pro aplikace pomocí PID 1934 na 2/5/2018 vytvořit na 19:42:32 *stdout_20180205194132_1934.log*.

Následující ukázka `aspNetCore` element konfiguruje stdout protokolování pro aplikaci hostovanou v Azure App Service. Místní cestu nebo cestu sdílené síťové složky je přijatelné pro místní protokolování. Zkontrolujte, jestli uživatel identita fondu aplikací má oprávnění k zápisu do zadané cestě.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

V tématu [konfigurace s web.config](#configuration-with-webconfig) příklad `aspNetCore` element v *web.config* souboru.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfigurace proxy serveru používá protokol HTTP a párovací token

Proxy server mezi ASP.NET Core modulu a Kestrel vytvořen používá protokol HTTP. Pomocí protokolu HTTP je optimalizace výkonu, kde provoz mezi modulu a Kestrel probíhá na adresu zpětné smyčky z síťové rozhraní. Neexistuje žádné riziko odposlouchávání provoz mezi modulu a Kestrel z umístění od serveru.

Token párovací se používá k zaručit, že požadavků přijatých službou Kestrel byly směrovány přes proxy server službou IIS a nebyla pochází z jiného zdroje. Párovací token se vytvoří a nastavení do proměnné prostředí (`ASPNETCORE_TOKEN`) modulem. Také nastavení párovací tokenu do záhlaví (`MSAspNetCoreToken`) u každého požadavku směrovány přes proxy server. Služba IIS Middleware kontroly požadavku že obdrží potvrďte, že párovací hodnota tokenu hlavičky odpovídá hodnotu proměnné prostředí. Pokud hodnoty tokenu se neshodují, žádost je zaznamenána a odmítnut. Proměnnou párovací tokenu prostředí a přenosy dat mezi modulu a Kestrel nejsou dostupné z umístění od serveru. Bez znalosti párovací hodnota tokenu, útočník nemohou odesílat požadavky, které obejít kontrolu v IIS Middleware.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Modul ASP.NET Core s službu IIS sdílenou konfiguraci

Základní modul ASP.NET instalační program spustí s oprávněními **systému** účtu. Vzhledem k tomu, že místní systémový účet nemá změnit oprávnění pro cestu ke sdílené složce používá sdílené konfiguraci IIS, instalační program dotkne chybu při pokusu o konfiguraci nastavení modulu v odepření přístupu *applicationHost.config* ve sdílené složce. Pokud používáte sdílenou konfiguraci IIS, postupujte takto:

1. Zakážete sdílenou konfiguraci IIS.
1. Spusťte instalační program.
1. Export aktualizovaný *applicationHost.config* soubor do sdílené složky.
1. Znovu povolte sdílenou konfiguraci IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Verze modulu a hostování sady Instalační protokoly

Chcete-li zjistit verzi nainstalovaný modul jádro ASP.NET:

1. V hostitelském systému, přejděte na *%windir%\System32\inetsrv*.
1. Vyhledejte *aspnetcore.dll* souboru.
1. Klikněte pravým tlačítkem na soubor a vyberte **vlastnosti** v kontextové nabídce.
1. Vyberte **podrobnosti** kartě. **Verze souboru** a **verze produktu** představují nainstalovaná verze modulu.

Instalační protokoly hostování sady pro modul se nacházejí v *C:\\uživatelé\\% UserName %\\data aplikací\\místní\\Temp*. Název souboru *dd_DotNetCoreWinSvrHosting__\<časové razítko > _000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Umístění souborů modulu, schémat a konfigurace

### <a name="module"></a>Modul

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**Služba IIS Express (x86 nebo amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schéma

**SLUŽBY IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**Služby IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Konfigurace

**SLUŽBY IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**Služby IIS Express**

   * .vs\config\applicationHost.config

Soubory naleznete vyhledáním *aspnetcore.dll* v *applicationHost.config* souboru. Pro službu IIS Express *applicationHost.config* soubor nebude existovat ve výchozím nastavení. Soubor je vytvořen v  *\<application_root >\\neodstraňujte\\konfigurace* při spuštění jakékoli projekt webové aplikace v řešení sady Visual Studio.
