---
title: "Odkaz na konfiguraci základní modul ASP.NET"
author: guardrex
description: "Postup konfigurace modulu jádra ASP.NET pro hostování aplikací ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 7bb7e5b9c821f87e73763f5f5c4f9fbcd751235f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>Odkaz na konfiguraci základní modul ASP.NET

Podle [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), a [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Tento dokument obsahuje podrobnosti o tom, jak nakonfigurovat modul jádro ASP.NET pro hostování aplikací ASP.NET Core. Úvod do modulu jádra ASP.NET a pokyny k instalaci, najdete v článku [ASP.NET Core modulu přehled](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-via-webconfig"></a>Konfigurace pomocí souboru web.config

Základní modul ASP.NET je nakonfigurovat přes web nebo aplikaci *web.config* souborů a má svou vlastní `aspNetCore` konfigurační oddíl v rámci `system.webServer`. Tady je příklad *web.config* souboru, který `Microsoft.NET.Sdk.Web` SDK bude poskytovat, když na projekt je publikována pro [nasazení závislé na framework](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) se zástupnými symboly pro `processPath` a `arguments`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

*Web.config* následující příklad je pro [samostatná nasazení](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) k [Azure App Service](https://azure.microsoft.com/services/app-service/). Další informace najdete v tématu [hostitele v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index). V tématu [konfiguraci dílčí aplikací](xref:host-and-deploy/iis/index#configuration-of-sub-applications) pro důležitá poznámka týkající se konfigurace *web.config* soubory v dílčí aplikace.

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a>Atributy elementu aspNetCore

| Atribut | Popis |
| --- | --- |
| processPath | <p>Požadovaný atribut typu string.</p><p>Cesta ke spustitelnému souboru, který se spustí proces naslouchání požadavkům HTTP. Jsou podporovány relativní cesty. Pokud cesta začíná '.', cesta se považuje za relativní vůči kořenovému adresáři webu.</p><p>Výchozí hodnota neexistuje.</p> |
| argumenty | <p>Volitelný řetězec atributu.</p><p>Argumenty pro spustitelný soubor určený v **processPath**.</p><p>Výchozí hodnota je prázdný řetězec.</p> |
| startupTimeLimit | <p>Atribut volitelné celé číslo.</p><p>Doba v sekundách, které vyčká, modul pro spuštění procesu naslouchání na portu spustitelný soubor. Pokud je tento časový limit překročen, modul se ukončit proces. Modul se pokusí znovu spusťte proces, při přijetí nového požadavku a bude dále pokoušet pro restartování procesu na následné příchozí žádosti, pokud aplikace se nepodaří spustit **rapidFailsPerMinute** číslo kolikrát za poslední minutu postupného.</p><p>Výchozí hodnota je 120.</p> |
| shutdownTimeLimit | <p>Atribut volitelné celé číslo.</p><p>Doba v sekundách, pro které modul vyčká pro spustitelný soubor řádně vypnutí při *app_offline.htm* je detekován soubor.</p><p>Výchozí hodnota je 10.</p> |
| rapidFailsPerMinute | <p>Atribut volitelné celé číslo.</p><p>Určuje, kolikrát proces zadaný v **processPath** je dovoleno havárií za minutu. Pokud je tento limit překročen, modul se zastaví spuštění procesu pro zbytek minutu.</p><p>Výchozí hodnota je 10.</p> |
| RequestTimeout | <p>Atribut volitelné časový interval.</p><p>Určuje dobu, pro který modul ASP.NET Core bude čekat na odpověď z procesu naslouchání na ASPNETCORE_PORT %.</p><p>Výchozí hodnota je "00: 02:00".</p><p>`requestTimeout` Musí být zadaná v celé minut, v opačném případě výchozí hodnota 2 minuty.</p> |
| stdoutLogEnabled | <p>Volitelný logický atribut.</p><p>V případě hodnoty true **stdout** a **stderr** pro proces zadaný v **processPath** bude přesměrován na soubor zadaný v **stdoutLogFile**.</p><p>Výchozí hodnota je False.</p> |
| stdoutLogFile | <p>Volitelný řetězec atributu.</p><p>Určuje cestu k souboru relativní nebo absolutní, pro kterou **stdout** a **stderr** z procesu zadaný v **processPath** bude do protokolu. Relativní cesty jsou relativní vůči kořenovému adresáři webu. Jakoukoli cestu, počínaje '. ", budou relativní vůči kořenovému adresáři webu a všechny ostatní cesty bude považována za absolutní cesty. Všechny složky zadaná v cestě musí existovat v pořadí pro modul pro vytvoření souboru protokolu. ID procesu je časové razítko (*yyyyMdhms*) a příponu souboru (*.log*) s podtržítka oddělovače přidají na poslední segment **stdoutLogFile** zadat.</p><p>Výchozí hodnota je `aspnetcore-stdout`.</p> |
| forwardWindowsAuthToken | Hodnota true nebo false</p><p>V případě hodnoty true token se předají do podřízeného procesu naslouchání na ASPNETCORE_PORT % jako hlavičku, MS-ASPNETCORE-WINAUTHTOKEN' každý požadavek. Je zodpovědností procesu pro volání funkce CloseHandle na tento token na základě požadavku.</p><p>Výchozí hodnota je true.</p> |
| disableStartUpErrorPage | Hodnota true nebo false</p><p>V případě hodnoty true **502.5 - selhání procesu** potlačeno stránky a znaková stránka 502 stav konfigurované v vaše *web.config* bude mít přednost.</p><p>Výchozí hodnota je False.</p> |

### <a name="setting-environment-variables"></a>Nastavení proměnných prostředí

Základní modul ASP.NET umožňuje zadat proměnné prostředí pro proces zadaný v `processPath` atribut zadáním v jednom nebo více `environmentVariable` podřízených elementů `environmentVariables` kolekce element v části `aspNetCore` elementu. Nastavení v této části proměnné prostředí mají přednost před systému proměnných prostředí pro proces.

Následující příklad nastaví dvou proměnných prostředí. `ASPNETCORE_ENVIRONMENT`nakonfiguruje prostředí aplikace `Development`. Vývojář může dočasně nastavit, že tato hodnota *web.config* souboru, aby bylo možné vynutit [vývojáře výjimka stránky](xref:fundamentals/error-handling) načíst při ladění výjimku aplikace. `CONFIG_DIR`je příkladem proměnnou uživatelské prostředí, kde byl vývojář zápis kód, který bude číst hodnotu na spuštění a aby bylo možné načíst konfigurační soubor aplikace vytvořit cestu.

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

## <a name="appofflinehtm"></a>app_offline.htm

Pokud umístit soubor s názvem *app_offline.htm* v kořenovém adresáři adresář webové aplikace, bude modul základní technologie ASP.NET pokusí řádně vypnutí aplikace a zastavit zpracování příchozích požadavků. Pokud aplikace stále běží `shutdownTimeLimit` počet sekund, bude modul ASP.NET Core kill běžící proces.

Při *app_offline.htm* je soubor k dispozici, modul ASP.NET Core bude odpovídat na požadavky odesláním zpět obsah *app_offline.htm* souboru. Jednou *app_offline.htm* soubor bude odstraněn, další požadavek načte aplikaci, které pak reaguje na požadavky.

## <a name="start-up-error-page"></a>Spuštění chybová stránka

Pokud modul jádro ASP.NET se nepodaří spustit proces back-end nebo spustí proces back-end ale selže tak, aby naslouchala na konfigurovaném portu, zobrazí se na stránku kód stavu HTTP 502.5. Chcete-li potlačit tuto stránku a vrátit na stránku výchozího stavu služby IIS 502 kódu, použijte `disableStartUpErrorPage` atribut. Další informace o konfiguraci vlastních chybových zpráv najdete v tématu [chyby protokolu HTTP `<httpErrors>` ](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).

![502 stavové stránce](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Vytvoření protokolu a přesměrování

Základní modul ASP.NET přesměruje `stdout` a `stderr` protokoly na disk, pokud jste nastavili `stdoutLogEnabled` a `stdoutLogFile` atributy `aspNetCore` element. Všechny složky aplikace `stdoutLogFile` cesta, musí existovat v pořadí pro modul pro vytvoření souboru protokolu. Časové razítko a soubor rozšíření se automaticky přidá, když se vytvoří soubor protokolu. Protokoly nejsou otáčet, pokud dojde k recyklování procesů nebo restartování. Je zodpovědností hostitel k omezení místa na disku, které využívají protokol. Pomocí `stdout` protokolu se doporučuje jenom pro řešení potíží při spuštění aplikace a ne pro účely protokolování obecné aplikace.

Název souboru protokolu se skládá připojením ID procesu (PID), časové razítko (*yyyyMdhms*) a příponu souboru (*.log*) na poslední segment `stdoutLogFile` cesta (obvykle *stdout* ) oddělená podtržítka. Například pokud `stdoutLogFile` cesta končí *stdout*, má název souboru protokolu pro aplikace pomocí PID 10652 vytvořen v 8/10/2017 v 12:05:02 *stdout_10652_20178101252.log*.

Zde je ukázka `aspNetCore` element, který konfiguruje `stdout` protokolování. `stdoutLogFile` Cesta ukazuje příklad je vhodný pro Azure App Service. Místní cestu nebo cestu sdílené síťové složky je přijatelné pro místní protokolování. Zkontrolujte, jestli uživatel identita fondu aplikací má oprávnění k zápisu do zadané cestě.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```
V tématu [konfigurace prostřednictvím web.config](#configuration-via-webconfig) příklad `aspNetCore` element v *web.config* souboru.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Modul ASP.NET Core s službu IIS sdílenou konfiguraci

Základní modul ASP.NET instalační program spustí s oprávněními **systému** účtu. Vzhledem k tomu, že místní systémový účet nemá změnit oprávnění pro cestu ke sdílené složce, kterou používá sdílené konfiguraci IIS, instalační program, se setkají chybu při pokusu o konfiguraci nastavení modulu v odepření přístupu  *applicationHost.config* ve sdílené složce.

Nepodporované řešením je zakázat sdílené konfiguraci IIS, spusťte instalační program, exportovat aktualizovaný *applicationHost.config* souborů do sdílené složky a znovu povolit sdílenou konfiguraci IIS.

## <a name="module-schema-and-configuration-file-locations"></a>Umístění souborů modulu, schémat a konfigurace

### <a name="module"></a>Modul

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schéma

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Konfigurace

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

Můžete vyhledat *aspnetcore.dll* v *applicationHost.config* souboru. Pro službu IIS Express *applicationHost.config* soubor nebude existovat ve výchozím nastavení. Soubor je vytvořen v *{kořenový adresář aplikace}\.vs\config* při spuštění jakékoli projekt webové aplikace v řešení sady Visual Studio.
