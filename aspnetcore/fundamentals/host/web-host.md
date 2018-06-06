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
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34687487"
---
# <a name="aspnet-core-web-host"></a>ASP.NET Core webového hostitele

Podle [Luke Latham](https://github.com/guardrex)

Aplikace ASP.NET Core nakonfigurovat a spustit *hostitele*. Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací. Minimálně hostitele nakonfiguruje server a kanálu zpracování požadavků. Toto téma popisuje ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), což je užitečné pro hostování webových aplikací. Pro pokrytí obecné hostitele .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), najdete v článku [obecné hostitele](xref:fundamentals/host/generic-host) tématu.

## <a name="set-up-a-host"></a>Nastavení hostitele

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

Vytvořit pomocí instance hostitele [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). To se obvykle provádí v vstupní bod aplikace, `Main` metoda. V rámci šablon projektu `Main` se nachází v *Program.cs*. Typické *Program.cs* volání [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zahájíte nastavení hostitele:

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

`CreateDefaultBuilder` provádí následující úlohy:

* Nakonfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako webový server a nakonfiguruje server pomocí poskytovatelů hostingu konfigurace aplikace. Výchozí možnosti Kestrel najdete v tématu [Kestrel možnosti části Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).
* Nastaví obsahu kořenovou cestu vrácený [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Načítání volitelné [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) z:
  * *appsettings.json*.
  * *appsettings.{Environment}.json*.
  * [Tajné klíče uživatele](xref:security/app-secrets) při spuštění aplikace `Development` prostředí pomocí položky sestavení.
  * Proměnné prostředí.
  * Argumenty příkazového řádku.
* Nakonfiguruje [protokolování](xref:fundamentals/logging/index) konzoly a ladění výstupu. Zahrnuje protokolování [filtrování protokolu](xref:fundamentals/logging/index#log-filtering) pravidla stanovená v části Konfigurace protokolování *appSettings.JSON určený* nebo *appsettings. { Prostředí} .json* souboru.
* Když spustíte za služby IIS, umožňuje [integrační služby IIS](xref:host-and-deploy/iis/index). Konfiguruje základní cesta a portu server naslouchá na při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module). Modul vytvoří reverzní proxy server mezi službou IIS a Kestrel. Nakonfiguruje aplikaci taky [zaznamenat chyby při spuštění](#capture-startup-errors). Výchozí možnosti služby IIS najdete v tématu [IIS možnosti oddílu hostitele ASP.NET Core v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index#iis-options).
* Nastaví [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) k `true` Pokud prostředí aplikace je vývoj. Další informace najdete v tématu [obor ověření](#scope-validation).

*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC. Když se aplikace spustí z kořenové složky projektu, projektu kořenové složky se používá jako kořenu obsahu. Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).

Další informace o konfiguraci aplikace, najdete v části [konfigurace v ASP.NET Core](xref:fundamentals/configuration/index).

> [!NOTE]
> Jako alternativu k použití statických `CreateDefaultBuilder` metody vytvoření hostitele z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) je podporované přístup pomocí ASP.NET Core 2.x. Další informace najdete v tématu kartě 1.x ASP.NET Core.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Vytvořit pomocí instance hostitele [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Vytvoření hostitele se obvykle provádí v vstupní bod aplikace, `Main` metoda. V rámci šablon projektu `Main` se nachází v *Program.cs*:

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

`WebHostBuilder` vyžaduje [serveru, který implementuje IServer](xref:fundamentals/servers/index). Jsou předdefinované servery [Kestrel](xref:fundamentals/servers/kestrel) a [HTTP.sys](xref:fundamentals/servers/httpsys) (před verzí technologie ASP.NET 2.0 jádra, ovladač HTTP.sys volala [WebListener](xref:fundamentals/servers/weblistener)). V tomto příkladu [UseKestrel rozšíření metoda](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) určuje Kestrel server.

*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC. Výchozí kořen obsahu se získávají pro `UseContentRoot` podle [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Když se aplikace spustí z kořenové složky projektu, projektu kořenové složky se používá jako kořenu obsahu. Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).

Chcete-li použít jako reverzní proxy server služby IIS, volejte [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) jako součást sestavení hostitele. `UseIISIntegration` neprovede konfiguraci *server*, například [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) nepodporuje. `UseIISIntegration` Konfiguruje základní cesta a portu server naslouchá na při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) vytvořit reverzní proxy server mezi Kestrel a služby IIS. Použití služby IIS s ASP.NET Core `UseKestrel` a `UseIISIntegration` musí být zadán. `UseIISIntegration` aktivuje pouze při spuštění za služby IIS nebo IIS Express. Další informace najdete v tématu [Úvod k modulu jádra ASP.NET](xref:fundamentals/servers/aspnet-core-module) a [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module).

Minimální implementace, které konfiguruje hostitele (a aplikace ASP.NET Core) zahrnuje určení serveru a konfiguraci kanálu žádostí aplikace:

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

Při nastavování hostitele, [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metody lze zadat. Pokud `Startup` je zadána třída, musíte definovat `Configure` metoda. Další informace najdete v tématu [spuštění aplikace v ASP.NET Core](xref:fundamentals/startup). Více volá, aby se `ConfigureServices` připojit k sobě. Více volá, aby se `Configure` nebo `UseStartup` na `WebHostBuilder` nahradit předchozí nastavení.

## <a name="host-configuration-values"></a>Hodnoty konfigurace hostitele

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) závisí na následujících dvou přístupů k nastavení hostitelské hodnoty konfigurace:

* Konfigurace hostitele tvůrce, který zahrnuje proměnné prostředí ve formátu `ASPNETCORE_{configurationKey}`. Například `ASPNETCORE_ENVIRONMENT`.
* Explicitní metody, jako například [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) a přidružené klíče. Při nastavení hodnoty s `UseSetting`, je hodnota nastavena jako bez ohledu na typ řetězec.

Hostitel používá. obě tyto možnosti nastaví hodnotu poslední. Další informace najdete v tématu [konfigurace přepisování](#override-configuration) v další části.

### <a name="capture-startup-errors"></a>Zaznamenat chyby při spuštění

Toto nastavení řídí zaznamenávání chyby při spuštění.

**Klíč**: captureStartupErrors  
**Typ**: *bool* (`true` nebo `1`)  
**Výchozí**: použije se výchozí hodnota `false` Pokud aplikace běží s Kestrel za služby IIS, kde výchozí hodnota je `true`.  
**Nastavit pomocí**: `CaptureStartupErrors`  
**Proměnné prostředí**: `ASPNETCORE_CAPTURESTARTUPERRORS`

Když `false`, chyb během spuštění výsledek v hostiteli. operace bude ukončena. Když `true`, hostitel zachytí výjimky během spouštění a pokusí o spuštění serveru.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a>Obsah kořenové

Toto nastavení určuje, kde začíná ASP.NET Core hledání obsahu souborů, jako je například zobrazení MVC. 

**Klíč**: contentRoot  
**Typ**: *řetězec*  
**Výchozí**: výchozí složce, kde se nachází sestavení aplikace.  
**Nastavit pomocí**: `UseContentRoot`  
**Proměnné prostředí**: `ASPNETCORE_CONTENTROOT`

Kořenu obsahu se používá jako základní cesta pro [kořenový Web nastavení](#web-root). Pokud cesta neexistuje, hostitel se nepodaří spustit.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a>Podrobné chyby

Určuje, zda podrobné chyby, které mají být zaznamenány.

**Klíč**: detailedErrors  
**Typ**: *bool* (`true` nebo `1`)  
**Výchozí**: false  
**Nastavit pomocí**: `UseSetting`  
**Proměnné prostředí**: `ASPNETCORE_DETAILEDERRORS`

Když povolené (nebo když <a href="#environment">prostředí</a> je nastaven na `Development`), aplikace zaznamená podrobné výjimky.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a>Prostředí

Nastaví prostředí aplikace.

**Klíč**: prostředí  
**Typ**: *řetězec*  
**Výchozí**: produkční  
**Nastavit pomocí**: `UseEnvironment`  
**Proměnné prostředí**: `ASPNETCORE_ENVIRONMENT`

Prostředí můžete nastavit na jakoukoli hodnotu. Framework definované hodnoty zahrnují `Development`, `Staging`, a `Production`. Hodnoty nejsou velká a malá písmena. Ve výchozím nastavení *prostředí* je pro čtení z `ASPNETCORE_ENVIRONMENT` proměnné prostředí. Při použití [Visual Studio](https://www.visualstudio.com/), proměnné prostředí může být nastavena v *launchSettings.json* souboru. Další informace najdete v tématu [použijte prostředí s více](xref:fundamentals/environments).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a>Hostování spuštění sestavení

Nastaví hostování spuštění sestavení aplikace.

**Klíč**: hostingStartupAssemblies  
**Typ**: *řetězec*  
**Výchozí**: prázdný řetězec  
**Nastavit pomocí**: `UseSetting`  
**Proměnné prostředí**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

Řetězec oddělený středníkem hostování spuštění sestavení načíst při spuštění. Tato funkce je nového v technologii ASP.NET 2.0 jádra.

I když výchozí hodnota konfigurace je řetězec prázdný, hostování spuštění sestavení, vždy zahrňte sestavení aplikace. Při hostování spuštění sestavení jsou k dispozici, se přidají do sestavení aplikace pro načtení, když aplikace sestavení jeho společných služeb během spouštění.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Tato funkce není k dispozici v ASP.NET Core 1.x.

---

### <a name="prefer-hosting-urls"></a>Dáváte přednost hostování adresy URL

Určuje, zda hostitel naslouchat požadavkům na adresy URL nakonfigurované `WebHostBuilder` místo nastavení nakonfigurovaného pomocí `IServer` implementace.

**Klíč**: preferHostingUrls  
**Typ**: *bool* (`true` nebo `1`)  
**Výchozí**: true  
**Nastavit pomocí**: `PreferHostingUrls`  
**Proměnné prostředí**: `ASPNETCORE_PREFERHOSTINGURLS`

Tato funkce je nového v technologii ASP.NET 2.0 jádra.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Tato funkce není k dispozici v ASP.NET Core 1.x.

---

### <a name="prevent-hosting-startup"></a>Zabránit spuštění hostování

Brání automatické načítání hostování spuštění sestavení, a to včetně hostování spuštění sestavení nakonfiguroval sestavení aplikace. V tématu [vylepšení aplikace z externí sestavení s IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) Další informace.

**Klíč**: preventHostingStartup  
**Typ**: *bool* (`true` nebo `1`)  
**Výchozí**: false  
**Nastavit pomocí**: `UseSetting`  
**Proměnné prostředí**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`

Tato funkce je nového v technologii ASP.NET 2.0 jádra.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Tato funkce není k dispozici v ASP.NET Core 1.x.

---

### <a name="server-urls"></a>Adresy URL serveru

Určuje IP adres nebo adres hostitele s porty a protokoly, které server naslouchat požadavkům na požadavky.

**Klíč**: adresy URL  
**Typ**: *řetězec*  
**Výchozí**: http://localhost:5000  
**Nastavit pomocí**: `UseUrls`  
**Proměnné prostředí**: `ASPNETCORE_URLS`

Nastavte na oddělených středníkem (;) seznam URL předpony serveru by měl odpovídat. Například `http://localhost:123`. Použití "\*" k označení, že by měl server přijímat požadavky na všechny IP adresy nebo názvu hostitele pomocí zadaný port a protokol (například `http://*:5000`). Protokol (`http://` nebo `https://`) musí být součástí každou adresu URL. Podporované formáty liší mezi servery.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

Kestrel má svůj vlastní koncový bod rozhraní API konfigurace. Další informace najdete v tématu [Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a>Časový limit vypnutí

Určuje dobu čekání na webového hostitele a ukončí se.

**Klíč**: shutdownTimeoutSeconds  
**Typ**: *int*  
**Výchozí**: 5  
**Nastavit pomocí**: `UseShutdownTimeout`  
**Proměnné prostředí**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

I když přijme klíč *int* s `UseSetting` (například `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) rozšíření metoda přebírá [časový interval](/dotnet/api/system.timespan). Tato funkce je nového v technologii ASP.NET 2.0 jádra.

Během časového limitu období, hostování:

* Aktivační události [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).
* Pokusy o zastavení hostované služby, protokolování pro služby, které se nepodařilo zastavit všechny chyby.

Pokud vyprší časový limit před všechny zastavení hostovaných služeb, jsou zastaveny všechny zbývající služby active při ukončení aplikace. Zastavení služeb i v případě, že se nedokončilo zpracování. Pokud služby vyžadují další čas ukončení, zvýšit časový limit.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Tato funkce není k dispozici v ASP.NET Core 1.x.

---

### <a name="startup-assembly"></a>Spuštění sestavení

Určuje sestavení pro vyhledávání `Startup` třídy.

**Klíč**: startupAssembly  
**Typ**: *řetězec*  
**Výchozí**: sestavení aplikace  
**Nastavit pomocí**: `UseStartup`  
**Proměnné prostředí**: `ASPNETCORE_STARTUPASSEMBLY`

Sestavení podle názvu (`string`) nebo typ (`TStartup`) může být odkaz. Pokud je to více `UseStartup` metody jsou volány, poslední změny mají přednost.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a>Kořenový web

Nastaví relativní cestu na statické prostředky aplikace.

**Klíč**: webroot  
**Typ**: *řetězec*  
**Výchozí**: Pokud není zadáno, výchozí hodnota je "(Content Root)/wwwroot", pokud cesta existuje. Pokud cesta neexistuje, je použít soubor no-op zprostředkovatele.  
**Nastavit pomocí**: `UseWebRoot`  
**Proměnné prostředí**: `ASPNETCORE_WEBROOT`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a>Přepsat konfiguraci

Použití [konfigurace](xref:fundamentals/configuration/index) konfigurace hostitele. V následujícím příkladu je konfigurace hostitele volitelně specifikován v *hosting.json* souboru. Všechny konfigurace načtena z *hosting.json* soubor může být přepsána argumenty příkazového řádku. Integrovaný konfigurace (v `config`) se používá ke konfiguraci hostitele s `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

Přepsání zadaná podle konfigurace `UseUrls` s *hosting.json* konfigurace příkazového řádku, první argument konfigurace druhý:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

Přepsání zadaná podle konfigurace `UseUrls` s *hosting.json* konfigurace příkazového řádku, první argument konfigurace druhý:

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
> [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) metoda rozšíření není aktuálně schopen analyzovat konfigurační oddíl vrácený `GetSection` (například `.UseConfiguration(Configuration.GetSection("section"))`. `GetSection` Metoda filtry konfigurace klíče do části požadovaný, ale ponechá název oddílu na klíče (například `section:urls`, `section:environment`). `UseConfiguration` Metoda očekává klíče tak, aby odpovídala `WebHostBuilder` klíče (například `urls`, `environment`). Přítomnost názvu oddílu na klíče zabrání hodnoty v části Konfigurace hostitele. Tento problém bude vyřešen v příští verzi. Další informace a řešení, najdete v části [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration používá úplné klíče](https://github.com/aspnet/Hosting/issues/839).

Pokud chcete zadat hostiteli spustit na konkrétní adresu URL, požadovanou hodnotu lze předat ve z příkazového řádku při provádění [dotnet spustit](/dotnet/core/tools/dotnet-run). Přepíše argument příkazového řádku `urls` z hodnoty *hosting.json* souboru a server naslouchá na portu 8080:

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a>Spravovat hostitele

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

**Spustit**

`Run` Metoda spustí webovou aplikaci a blokuje volající vlákno, dokud se vypne hostitele:

```csharp
host.Run();
```

**Start**

Spuštění hostitele způsobem neblokující voláním jeho `Start` metoda:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Pokud je předán seznam adres URL, `Start` metoda, naslouchá na zadané adresy URL:

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

Aplikace můžete inicializaci a spuštění nové hostitele pomocí předem nakonfigurovaných výchozích nastavení `CreateDefaultBuilder` pomocí jiné metody statické pohodlí. Tyto metody start pro server bez výstup konzoly a s [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) počkejte zalomení (Ctrl-C nebo sigint – nebo SIGTERM –):

**Spuštění (RequestDelegate aplikace)**

Začněte `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!" `WaitForShutdown` bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –). Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.

**Spuštění (řetězec adresy url, RequestDelegate aplikace)**

Spustit s adresou URL a `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Stejný výsledek jako **spuštění (RequestDelegate aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.

**Spuštění (akce&lt;IRouteBuilder&gt; routeBuilder)**

Použít instanci `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) používat směrování middleware:

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

Použijte následující požadavky na prohlížeč s příkladu:

| Požadavek                                    | Odpověď                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Hello, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | Vyvolá výjimku řetězcem "ooops!" |
| `http://localhost:5000/throw`              | Vyvolá výjimku řetězcem "Uh jejda!" |
| `http://localhost:5000/Sante/Kevin`        | Santé, kevina!                            |
| `http://localhost:5000`                    | Ahoj světe!                             |

`WaitForShutdown` bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –). Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.

**Spuštění (řetězce adresy url, akce&lt;IRouteBuilder&gt; routeBuilder)**

Použijte adresu URL a instance `IRouteBuilder`:

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

Stejný výsledek jako **spuštění (akce&lt;IRouteBuilder&gt; routeBuilder)**, s výjimkou aplikace reaguje na `http://localhost:8080`.

**StartWith (akce&lt;IApplicationBuilder&gt; aplikace)**

Zadejte delegát pro konfiguraci `IApplicationBuilder`:

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

Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!" `WaitForShutdown` bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –). Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.

**StartWith (řetězce adresy url, akce&lt;IApplicationBuilder&gt; aplikace)**

Zadejte adresu URL a delegát pro konfiguraci `IApplicationBuilder`:

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

Stejný výsledek jako **StartWith (akce&lt;IApplicationBuilder&gt; aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**Spustit**

`Run` Metoda spustí webovou aplikaci a blokuje volající vlákno, dokud se vypne hostitele:

```csharp
host.Run();
```

**Start**

Spuštění hostitele způsobem neblokující voláním jeho `Start` metoda:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Pokud je předán seznam adres URL, `Start` metoda, naslouchá na zadané adresy URL:

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

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment rozhraní

[IHostingEnvironment rozhraní](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) poskytuje informace o hostování prostředí webové aplikace. Použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:

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

A [založené na konvenci přístup](xref:fundamentals/environments#startup-conventions) můžete použít ke konfiguraci aplikace při spuštění založených na prostředí. Můžete také vložit `IHostingEnvironment` do `Startup` konstruktor pro použití v `ConfigureServices`:

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
> Kromě `IsDevelopment` metoda rozšíření `IHostingEnvironment` nabízí `IsStaging`, `IsProduction`, a `IsEnvironment(string environmentName)` metody. V tématu [použijte prostředí s více](xref:fundamentals/environments) podrobnosti.

`IHostingEnvironment` Služby může také vložit přímo do `Configure` metoda pro nastavení zpracování kanálu:

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

`IHostingEnvironment` můžete vložit do `Invoke` metoda při vytváření vlastní [middleware](xref:fundamentals/middleware/index#writing-middleware):

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

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime rozhraní

[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umožňuje po spuštění a vypnutí aktivity. Zrušení tokenů použitá pro zaregistrování jsou tři vlastnosti na rozhraní `Action` metody, které definují události spuštění a vypnutí.

| Token zrušení    | Při aktivaci&#8230; |
| --------------------- | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | Hostitele plně byla spuštěna. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | Hostitel je dokončení řádné vypnutí. Všechny požadavky, měla by být zpracována. Vypnutí bloky až po dokončení této události. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | Hostitel provádí řádné vypnutí. Může být stále aktivní žádosti. Vypnutí bloky až po dokončení této události. |

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

[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) požadavky ukončení aplikace. Následující třídy používá `StopApplication` korektně vypnout aplikaci při třídy `Shutdown` metoda je volána:

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

## <a name="scope-validation"></a>Ověření oboru

V technologii ASP.NET Core 2.0 nebo novější [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) nastaví [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) k `true` Pokud prostředí aplikace je vývoj.

Když `ValidateScopes` je nastaven na `true`, výchozím zprostředkovatelem služeb provádí kontroly, aby ověřte, že:

* Vymezené služby nejsou přímo nebo nepřímo přeložit od kořenové poskytovatele služeb.
* Vymezená služby nejsou přímo nebo nepřímo vložit do jednotlivých prvků.

Kořenového poskytovatele služby se vytvoří při [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) je volána. Doba platnosti poskytovatele služeb kořenové odpovídá na aplikační nebo server životního cyklu, pokud zprostředkovatel začíná aplikace a uvolnění při vypnutí aplikace.

Vymezená služby jsou zapomenuty kontejnerem, který je vytvořil. Pokud vymezené služby se vytvoří v kořenovém kontejneru, životnost služby je efektivně povýšen na singleton, protože jejich pouze likvidace podle Kořenový kontejner při ukončení aplikace nebo serveru. Ověřování služby Obory zachytí těchto situacích při `BuildServiceProvider` je volána.

Vždy ověření obory, včetně v provozním prostředí, nakonfigurujte [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) s [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) na tvůrce hostitele:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a>Řešení potíží s System.ArgumentException

**Platí pro pouze ASP.NET Core 2.0**

Hostitel může být sestaven vložením `IStartup` přímo do kontejneru pro vkládání závislosti místo volání `UseStartup` nebo `Configure`:

```csharp
services.AddSingleton<IStartup, Startup>();
```

Pokud hostitel je integrovaný tímto způsobem, může dojít k následující chybě:

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

K tomu dochází, protože [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (aktuální sestavení) je nutná ke kontrole pro `HostingStartupAttributes`. Pokud aplikace ručně vloží `IStartup` do kontejneru pro vkládání závislosti, přidejte následující volání `WebHostBuilder` se zadaným názvem sestavení:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

Můžete taky přidat fiktivní `Configure` k `WebHostBuilder`, která nastaví `applicationName`(`ApplicationKey`) automaticky:

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**Poznámka:**: Toto je pouze požadované verze technologie ASP.NET 2.0 jádra a pouze pokud aplikace nemá volání `UseStartup` nebo `Configure`.

Další informace najdete v tématu [oznámení: Microsoft.Extensions.PlatformAbstractions byl odebrán (komentář)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) a [StartupInjection ukázka](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

## <a name="additional-resources"></a>Další zdroje

* [Hostování ve Windows se službou IIS](xref:host-and-deploy/iis/index)
* [Hostování v Linuxu na serveru Nginx](xref:host-and-deploy/linux-nginx)
* [Hostování v Linuxu na serveru Apache](xref:host-and-deploy/linux-apache)
* [Hostitele ve službě Windows](xref:host-and-deploy/windows-service)
