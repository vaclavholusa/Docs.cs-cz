---
title: "Hostování v ASP.NET Core"
author: guardrex
description: "Další informace o webového hostitele v ASP.NET Core, která je zodpovědná za spuštění a životního cyklu správy aplikací."
keywords: Web ASP.NET Core hostitele, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: dfec2a67112d40b528b97c847da3dda8ef1e63bd
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="hosting-in-aspnet-core"></a>Hostování v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Aplikace ASP.NET Core nakonfigurovat a spustit *hostitele*, která je zodpovědná za spuštění a životního cyklu správy aplikací. Minimálně hostitele nakonfiguruje server a kanálu zpracování požadavků.

## <a name="setting-up-a-host"></a>Nastavení hostitele

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Vytvořit pomocí instance hostitele [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). To se obvykle provádí ve vstupním bodě vaší aplikace, `Main` metoda. V rámci šablon projektu `Main` se nachází v *Program.cs*. Typické *Program.cs* volání [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zahájíte nastavení hostitele:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder`provádí následující úlohy:

* Nakonfiguruje [Kestrel](servers/kestrel.md) jako webový server. Výchozí možnosti Kestrel najdete v tématu [Kestrel možnosti části Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).
* Nastaví kořenu obsahu [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Načítání z volitelné konfigurace:
  * *appSettings.JSON určený*.
  * *appSettings. {Prostředí} .json*.
  * [Tajné klíče uživatele](xref:security/app-secrets) při spuštění aplikace `Development` prostředí.
  * Proměnné prostředí.
  * Argumenty příkazového řádku.
* Nakonfiguruje [protokolování](xref:fundamentals/logging/index) konzoly a ladění výstupu s [filtrování protokolu](xref:fundamentals/logging/index#log-filtering) pravidla stanovená v části Konfigurace protokolování *appSettings.JSON určený* nebo *appsettings. {Prostředí} .json* souboru.
* Při spuštění za služby IIS, umožňuje [integrační služby IIS](xref:publishing/iis) nakonfigurováním základní cesta a port serveru naslouchat požadavkům na při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module). Modul vytvoří proxy zpětného mezi službou IIS a Kestrel. Nakonfiguruje aplikaci taky [zaznamenat chyby při spuštění](#capture-startup-errors). Výchozí možnosti služby IIS najdete v tématu [IIS možnosti oddílu hostitele ASP.NET Core v systému Windows pomocí služby IIS](xref:publishing/iis#iis-options).

*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC. Výchozí kořen obsahu je [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory). Výsledkem je použití webového projektu kořenové složky jako kořenu obsahu při spuštění aplikace z kořenové složky (například volání [dotnet spustit](/dotnet/core/tools/dotnet-run) ze složky projektu). Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).

V tématu [konfigurace v ASP.NET Core](xref:fundamentals/configuration/index) pro další informace o konfiguraci aplikace.

> [!NOTE]
> Jako alternativu k použití statických `CreateDefaultBuilder` metody vytvoření hostitele z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) je podporované přístup pomocí ASP.NET Core 2.x. V tématu kartě ASP.NET Core 1.x pro další informace.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Vytvořit pomocí instance hostitele [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). To se obvykle provádí ve vstupním bodě vaší aplikace, `Main` metoda. V rámci šablon projektu `Main` se nachází v *Program.cs*. Následující *Program.cs* ukazuje, jak používat `WebHostBuilder` k sestavení hostitele:

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder`vyžaduje [serveru, který implementuje IServer](servers/index.md). Jsou předdefinované servery [Kestrel](servers/kestrel.md) a [HTTP.sys](servers/httpsys.md) (před verzí technologie ASP.NET 2.0 jádra, ovladač HTTP.sys volala [WebListener](xref:fundamentals/servers/weblistener)). V tomto příkladu [UseKestrel rozšíření metoda](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) určuje Kestrel server.

*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC. Zadaný výchozí kořen obsahu do `UseContentRoot` je [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Výsledkem je použití webového projektu kořenové složky jako kořenu obsahu při spuštění aplikace z kořenové složky (například volání [dotnet spustit](/dotnet/core/tools/dotnet-run) ze složky projektu). Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).

Chcete-li použít jako reverzní proxy server služby IIS, volejte [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) jako součást sestavení hostitele. `UseIISIntegration`neprovede konfiguraci *server*, například [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) nepodporuje. `UseIISIntegration`Konfiguruje základní cesta a portu server musí naslouchat na portu při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) vytvořit proxy zpětného mezi Kestrel a služby IIS. Pokud chcete používat IIS s ASP.NET Core, musíte zadat oba `UseKestrel` a `UseIISIntegration`. `UseIISIntegration`aktivuje pouze při spuštění za služby IIS nebo IIS Express. Další informace najdete v tématu [Úvod k modulu jádra ASP.NET](xref:fundamentals/servers/aspnet-core-module) a [odkazu na modul jádro ASP.NET konfigurace](xref:hosting/aspnet-core-module).

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

Při nastavení na hostitele, je možné poskytnout [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metody. Pokud zadáte `Startup` třídu, musí definovat `Configure` metoda. Další informace najdete v tématu [spuštění aplikace v ASP.NET Core](startup.md). Více volá, aby se `ConfigureServices` připojit k sobě. Více volá, aby se `Configure` nebo `UseStartup` na `WebHostBuilder` nahradit předchozí nastavení.

## <a name="host-configuration-values"></a>Hodnoty konfigurace hostitele

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) poskytuje metody pro nastavení většinu hodnoty dostupné konfigurace pro hostitele, které lze nastavit také přímo s [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) a přidružené klíče. Při nastavení hodnoty s `UseSetting`, je hodnota nastavena jako bez ohledu na typ řetězec (v uvozovkách).

### <a name="capture-startup-errors"></a>Zaznamenat chyby při spuštění

Toto nastavení řídí zaznamenávání chyby při spuštění.

**Klíč**: captureStartupErrors  
**Typ**: *bool* (`true` nebo `1`)  
**Výchozí**: použije se výchozí hodnota `false` Pokud aplikace běží s Kestrel za služby IIS, kde výchozí hodnota je `true`.  
**Nastavit pomocí**:`CaptureStartupErrors`

Když `false`, chyb během spuštění výsledek v hostiteli. operace bude ukončena. Když `true`, hostitel zachytí výjimky během spouštění a pokusí o spuštění serveru.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>Obsah kořenové

Toto nastavení určuje, kde začíná ASP.NET Core hledání obsahu souborů, jako je například zobrazení MVC. 

**Klíč**: contentRoot  
**Typ**: *řetězec*  
**Výchozí**: výchozí složce, kde se nachází sestavení aplikace.  
**Nastavit pomocí**:`UseContentRoot`

Kořenu obsahu se používá jako základní cesta pro [kořenový Web nastavení](#web-root). Pokud cesta neexistuje, hostitel se nepodaří spustit.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>Podrobné chyby

Určuje, zda podrobné chyby, které mají být zaznamenány.

**Klíč**: detailedErrors  
**Typ**: *bool* (`true` nebo `1`)  
**Výchozí**: false  
**Nastavit pomocí**:`UseSetting`

Když povolené (nebo když <a href="#environment">prostředí</a> je nastaven na `Development`), aplikace zaznamená podrobné výjimky.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>Prostředí

Nastaví prostředí aplikace.

**Klíč**: prostředí  
**Typ**: *řetězec*  
**Výchozí**: produkční  
**Nastavit pomocí**:`UseEnvironment`

Můžete nastavit *prostředí* na jakoukoli hodnotu. Framework definované hodnoty zahrnují `Development`, `Staging`, a `Production`. Hodnoty nejsou velká a malá písmena. Ve výchozím nastavení *prostředí* je pro čtení z `ASPNETCORE_ENVIRONMENT` proměnné prostředí. Při použití [Visual Studio](https://www.visualstudio.com/), proměnné prostředí může být nastavena v *launchSettings.json* souboru. Další informace najdete v tématu [práce s několika prostředí](xref:fundamentals/environments).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>Hostování spuštění sestavení

Nastaví hostování spuštění sestavení aplikace.

**Klíč**: hostingStartupAssemblies  
**Typ**: *řetězec*  
**Výchozí**: prázdný řetězec  
**Nastavit pomocí**:`UseSetting`

Řetězec oddělený středníkem hostování spuštění sestavení načíst při spuštění. Tato funkce je nového v technologii ASP.NET 2.0 jádra.

I když výchozí hodnota konfigurace je řetězec prázdný, hostování spuštění sestavení, vždy zahrňte sestavení aplikace. Když zadáte hostování sestavení spuštění, se přidají do sestavení aplikace pro načtení, když aplikace sestavení jeho společných služeb během spouštění.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Tato funkce není k dispozici v ASP.NET Core 1.x.

---

### <a name="prefer-hosting-urls"></a>Dáváte přednost hostování adresy URL

Určuje, zda hostitel naslouchat požadavkům na adresy URL nakonfigurované `WebHostBuilder` místo nastavení nakonfigurovaného pomocí `IServer` implementace.

**Klíč**: preferHostingUrls  
**Typ**: *bool* (`true` nebo `1`)  
**Výchozí**: true  
**Nastavit pomocí**:`PreferHostingUrls`

Tato funkce je nového v technologii ASP.NET 2.0 jádra.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Tato funkce není k dispozici v ASP.NET Core 1.x.

---

### <a name="prevent-hosting-startup"></a>Zabránit spuštění hostování

Brání automatické načítání hostování spuštění sestavení, a to včetně hostování spuštění sestavení nakonfiguroval sestavení aplikace. V tématu [přidat funkce aplikací z externí sestavení pomocí IHostingStartup](xref:hosting/ihostingstartup) Další informace.

**Klíč**: preventHostingStartup  
**Typ**: *bool* (`true` nebo `1`)  
**Výchozí**: false  
**Nastavit pomocí**:`UseSetting`

Tato funkce je nového v technologii ASP.NET 2.0 jádra.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Tato funkce není k dispozici v ASP.NET Core 1.x.

---

### <a name="server-urls"></a>Adresy URL serveru

Určuje IP adres nebo adres hostitele s porty a protokoly, které server naslouchat požadavkům na požadavky.

**Klíč**: adresy URL  
**Typ**: *řetězec*  
**Výchozí**: http://localhost: 5000  
**Nastavit pomocí**:`UseUrls`

Nastavte na oddělených středníkem (;) seznam URL předpony serveru by měl odpovídat. Například `http://localhost:123`. Použití "\*" k označení, že by měl server přijímat požadavky na všechny IP adresy nebo názvu hostitele pomocí zadaný port a protokol (například `http://*:5000`). Protokol (`http://` nebo `https://`) musí být součástí každou adresu URL. Podporované formáty liší mezi servery.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel má svůj vlastní koncový bod rozhraní API konfigurace. Další informace najdete v tématu [Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>Časový limit vypnutí

Určuje dobu čekání na webového hostitele vypnutí.

**Klíč**: shutdownTimeoutSeconds  
**Typ**: *int*  
**Výchozí**: 5  
**Nastavit pomocí**:`UseShutdownTimeout`

I když přijme klíč *int* s `UseSetting` (například `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` rozšíření metoda přebírá `TimeSpan`. Tato funkce je nového v technologii ASP.NET 2.0 jádra.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Tato funkce není k dispozici v ASP.NET Core 1.x.

---

### <a name="startup-assembly"></a>Spuštění sestavení

Určuje sestavení pro vyhledávání `Startup` třídy.

**Klíč**: startupAssembly  
**Typ**: *řetězec*  
**Výchozí**: sestavení aplikace  
**Nastavit pomocí**:`UseStartup`

Sestavení, můžete odkazovat podle názvu (`string`) nebo typ (`TStartup`). Pokud je to více `UseStartup` metody jsou volány, poslední změny mají přednost.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a>Kořenový web

Nastaví relativní cestu na statické prostředky aplikace.

**Klíč**: webroot  
**Typ**: *řetězec*  
**Výchozí**: Pokud není zadáno, výchozí hodnota je "(Content Root)/wwwroot", pokud cesta existuje. Pokud cesta neexistuje, je použít soubor no-op zprostředkovatele.  
**Nastavit pomocí**:`UseWebRoot`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>Přepsání konfigurace

Použití [konfigurace](xref:fundamentals/configuration/index) konfigurace hostitele. V následujícím příkladu je konfigurace hostitele volitelně specifikován v *hosting.json* souboru. Všechny konfigurace načtena z *hosting.json* soubor může být přepsána argumenty příkazového řádku. Integrovaný konfigurace (v `config`) se používá ke konfiguraci hostitele s `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

*Hosting.JSON*:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

*Hosting.JSON*:

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
> `UseConfiguration` Metoda rozšíření není aktuálně schopen analyzovat konfigurační oddíl vrácený `GetSection` (například `.UseConfiguration(Configuration.GetSection("section"))`. `GetSection` Metoda filtry konfigurace klíče do části požadovaný, ale ponechá název oddílu na klíče (například `section:urls`, `section:environment`). `UseConfiguration` Metoda očekává klíče tak, aby odpovídala `WebHostBuilder` klíče (například `urls`, `environment`). Přítomnost názvu oddílu na klíče zabrání hodnoty v části Konfigurace hostitele. Tento problém bude vyřešen v příští verzi. Další informace a řešení, najdete v části [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration používá úplné klíče](https://github.com/aspnet/Hosting/issues/839).

Pokud chcete zadat hostiteli spustit na konkrétní adresu URL, můžete předat požadovanou hodnotu z příkazového řádku při provádění `dotnet run`. Přepíše argument příkazového řádku `urls` z hodnoty *hosting.json* souboru a server naslouchá na portu 8080:

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a>Řazení důležitostí

Některé `WebHostBuilder` nastavení jsou nejdřív přečíst z proměnné prostředí, pokud nastavení. Tyto proměnné prostředí, použijte formát `ASPNETCORE_{configurationKey}`. Adresy URL, které server naslouchá na ve výchozím nastavení, můžete nastavit `ASPNETCORE_URLS`.

Některou z těchto hodnot proměnných prostředí můžete přepsat zadáním konfigurace (pomocí `UseConfiguration`) nebo nastavením hodnoty explicitně (pomocí `UseSetting` nebo jednu z metod explicitní rozšíření, jako například `UseUrls`). Hostitel používá. obě tyto možnosti nastaví hodnotu poslední. Pokud chcete prostřednictvím kódu programu nastavena výchozí adresy URL na jednu hodnotu, ale aby ji bylo možné přepsat s konfigurací, můžete příkazového řádku konfigurace po nastavení adresy URL. V tématu [konfigurace přepíše](#overriding-configuration).

## <a name="starting-the-host"></a>Od hostitele

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

**Spustit**

`Run` Metoda spuštění webové aplikace a blokuje volající vlákno, dokud hostitel vypnutí:

```csharp
host.Run();
```

**Start**

Můžete spustit hostitele způsobem neblokující voláním jeho `Start` metoda:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Pokud předáte seznam adres URL `Start` metoda, naslouchá na zadané adresy URL:

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

Můžete inicializaci a spuštění nové hostitele pomocí předem nakonfigurovaných výchozích nastavení `CreateDefaultBuilder` pomocí jiné metody statické pohodlí. Tyto metody start pro server bez výstup konzoly a s [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) počkejte zalomení (Ctrl-C nebo sigint – nebo SIGTERM –):

**Spuštění (RequestDelegate aplikace)**

Začněte `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!" `WaitForShutdown`bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –). Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.

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

**Spuštění (akce<IRouteBuilder> routeBuilder)**

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

`WaitForShutdown`bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –). Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.

**Spuštění (řetězce adresy url, akce<IRouteBuilder> routeBuilder)**

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Stejný výsledek jako **spuštění (akce<IRouteBuilder> routeBuilder)**, s výjimkou aplikace reaguje na `http://localhost:8080`.

**StartWith (akce<IApplicationBuilder> aplikace)**

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!" `WaitForShutdown`bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –). Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.

**StartWith (řetězce adresy url, akce<IApplicationBuilder> aplikace)**

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Stejný výsledek jako **StartWith (akce<IApplicationBuilder> aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

**Spustit**

`Run` Metoda spuštění webové aplikace a blokuje volající vlákno, dokud hostitel vypnutí:

```csharp
host.Run();
```

**Start**

Můžete spustit hostitele způsobem neblokující voláním jeho `Start` metoda:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Pokud předáte seznam adres URL `Start` metoda, naslouchá na zadané adresy URL:


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

[IHostingEnvironment rozhraní](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) poskytuje informace o hostování prostředí webové aplikace. Můžete použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:

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

Můžete použít [založené na konvenci přístup](xref:fundamentals/environments#startup-conventions) ke konfiguraci vaší aplikace při spuštění založených na prostředí. Alternativně můžete vložit `IHostingEnvironment` do `Startup` konstruktor pro použití v `ConfigureServices`:

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
> Kromě `IsDevelopment` metoda rozšíření `IHostingEnvironment` nabízí `IsStaging`, `IsProduction`, a `IsEnvironment(string environmentName)` metody. V tématu [práce s několika prostředí](xref:fundamentals/environments) podrobnosti.

`IHostingEnvironment` Služby může také vložit přímo do `Configure` metoda pro nastavení vašeho kanálu zpracování:

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

Můžete vložit `IHostingEnvironment` do `Invoke` metoda při vytváření vlastní [middleware](xref:fundamentals/middleware#writing-middleware):

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

[IApplicationLifetime rozhraní](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umožňuje provádět po spuštění a vypnutí aktivity. Zrušení tokenů, které můžete zaregistrovat s jsou tři vlastnosti na rozhraní `Action` metody k definování události spuštění a vypnutí. K dispozici je také `StopApplication` metoda.

| Token zrušení    | Aktivuje, když &#8230; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | Hostitele plně byla spuštěna. |
| `ApplicationStopping` | Hostitel provádí řádné vypnutí. Může být stále aktivní žádosti. Vypnutí bloky až po dokončení této události. |
| `ApplicationStopped`  | Hostitel je dokončení řádné vypnutí. Všechny požadavky, měla by být zcela zpracována. Vypnutí bloky až po dokončení této události. |

| Metoda            | Akce                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | Žádosti o ukončení aktuální aplikace. |

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

## <a name="troubleshooting-systemargumentexception"></a>Řešení potíží s System.ArgumentException

**Platí pro pouze ASP.NET Core 2.0**

Pokud vytvoříte hostitele vložením `IStartup` přímo do kontejneru pro vkládání závislosti místo volání `UseStartup` nebo `Configure`, může dojít k následující chybě: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.

K tomu dochází, protože [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (aktuální sestavení) je nutná ke kontrole pro `HostingStartupAttributes`. Pokud ručně vložit `IStartup` do kontejneru pro vkládání závislosti, přidejte následující volání vaší `WebHostBuilder` se zadaným názvem sestavení:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

Můžete taky přidat fiktivní `Configure` k vaší `WebHostBuilder`, která nastaví `applicationName`(`ApplicationKey`) automaticky:

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**Poznámka:**: Toto je pouze požadované verze technologie ASP.NET 2.0 jádra a pouze pokud nemůžete volat `UseStartup` nebo `Configure`.

Další informace najdete v tématu [oznámení: Microsoft.Extensions.PlatformAbstractions byl odebrán (komentář)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) a [StartupInjection ukázka](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

## <a name="additional-resources"></a>Další zdroje

* [Publikovat do systému Windows pomocí služby IIS](../publishing/iis.md)
* [Publikování do systému Linux pomocí Nginx](../publishing/linuxproduction.md)
* [Publikování do systému Linux pomocí Apache](../publishing/apache-proxy.md)
* [Hostitele ve službě Windows](xref:hosting/windows-service)
