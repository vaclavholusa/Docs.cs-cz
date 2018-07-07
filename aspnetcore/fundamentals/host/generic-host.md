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
# <a name="net-generic-host"></a>Obecný hostitele .NET

Podle [Luke Latham](https://github.com/guardrex)

Konfigurace aplikací .NET a spouštění *hostitele*. Hostitel je zodpovědný za spouštění a životního cyklu správy aplikací. Toto téma obsahuje obecný hostitele ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), což je užitečné pro hostování aplikací, které není zpracovávají požadavky HTTP. Pro pokrytí webového hostitele ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), najdete v článku [webového hostitele](xref:fundamentals/host/web-host) tématu.

Cílem obecný hostitele je oddělit kanálu HTTP z hostitele webového rozhraní API umožňující širší škálu scénářů hostitele. Zasílání zpráv, úlohy na pozadí a další úlohy jiným protokolem než HTTP podle obecného hostitele, který je výhodné společné funkce, jako jsou konfigurace, injektáž závislostí (DI) a protokolování.

Obecný hostitele je nového v ASP.NET Core 2.1 a není vhodné pro scénáře hostování webů. Pro web scénářích hostování, použijte [webového hostitele](xref:fundamentals/host/web-host). Obecný hostitele je ve vývoji a nahraďte webového hostitele v budoucí verzi fungovat jako primární hostitele rozhraní API scénáře jiným protokolem než HTTP a protokolu HTTP.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Při spuštění ukázkové aplikace [Visual Studio Code](https://code.visualstudio.com/), použijte *externí nebo integrovaného terminálu*. Nejdou spustit v ukázce `internalConsole`.

Nastavení konzoly ve Visual Studio Code:

1. Otevřít *.vscode/launch.json* souboru.
1. V **.NET Core spuštění (konzola)** konfigurace, vyhledejte **konzoly** položka. Nastavte hodnotu na buď `externalTerminal` nebo `integratedTerminal`.

## <a name="introduction"></a>Úvod

Je k dispozici v knihovně obecný hostitele [obor názvů Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) a poskytuje [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) balíčku. `Microsoft.Extensions.Hosting` Je součástí balíčku [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 nebo novější).

[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) je vstupním bodem k provádění kódu. Každý `IHostedService` implementace provádí v pořadí podle [službu registrace v ConfigureServices](#configureservices). [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) je volána v každé `IHostedService` při spuštění hostitele a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) je volána v pořadí reverzní registrace při řádné vypnutí hostitele.

## <a name="set-up-a-host"></a>Nastavení hostitele

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) hlavní součást, knihovny a aplikace použít k inicializaci, vytvoření a spuštění hostitele:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a>Konfigurace hostitele

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) závisí na následujících dvou přístupů k nastavení hodnoty konfigurace hostitele:

* Configuration builder
* Konfigurace metody rozšíření

### <a name="configuration-builder"></a>Configuration builder

Konfigurace hostitele Tvůrce je vytvořen zavoláním [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementace. `ConfigureHostConfiguration` používá [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) k vytvoření [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pro hostitele. Configuration builder inicializuje [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) pro použití v procesu sestavení aplikace.

Konfigurace proměnných prostředí není přidán ve výchozím nastavení. Volání [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) na tvůrce hostitele a nakonfigurujte hostitele z proměnných prostředí. `AddEnvironmentVariables` přijímá volitelný uživatelsky definovanou předponu. Ukázková aplikace používá předponou `PREFIX_`. Předpona, která se odebere, když jsou proměnné prostředí načteny. Když je ukázková aplikace hostitel nakonfigurovaný, hodnotu proměnné prostředí pro `PREFIX_ENVIRONMENT` stane hodnota konfigurace hostitele `environment` klíč.

Během vývoje. při použití [sady Visual Studio](https://www.visualstudio.com/) nebo spuštěním aplikace s `dotnet run`, lze nastavit proměnné prostředí *Properties/launchSettings.json* souboru. V [Visual Studio Code](https://code.visualstudio.com/), lze nastavit proměnné prostředí *.vscode/launch.json* souboru během vývoje. Další informace najdete v tématu [používání více prostředí](xref:fundamentals/environments).

`ConfigureHostConfiguration` můžete volat vícekrát s přičítáním výsledky. Hostitel používá kterékoli z těchto možností nastaví hodnotu poslední.

*hostsettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Příklad `HostBuilder` konfiguraci pomocí `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) rozšiřující metoda není aktuálně podporuje při analýze oddílu konfigurace vrácený [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (například `.AddConfiguration(Configuration.GetSection("section"))`. `GetSection` Metoda filtry konfigurační klíče do části Požadovaná, ale ponechá název oddílu, který na klíče (například `section:environment`). `AddConfiguration` Metoda očekává klíčů tak, aby odpovídaly `HostBuilder` klíče (například `environment`). Přítomnost název oddílu, který klíčů brání hodnoty v části Konfigurace hostitele. Tento problém bude vyřešen v příští verzi. Další informace a řešení najdete v tématu [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration využívá úplnou klíčů](https://github.com/aspnet/Hosting/issues/839).

### <a name="extension-method-configuration"></a>Konfigurace metody rozšíření

Metody rozšíření jsou volány v `IHostBuilder` implementace nakonfigurujte obsahu kořenové certifikáty a prostředí.

#### <a name="content-root"></a>Obsah kořenové

Toto nastavení určuje, kde začíná hostitele vyhledávání obsahu souborů.

**Klíč**: contentRoot  
**Typ**: *řetězec*  
**Výchozí**: výchozí hodnota je složka, ve které se nachází sestavení aplikace.  
**Sada s použitím**: `UseContentRoot`  
**Proměnná prostředí**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` je [volitelné a uživatelem definovanými](#configuration-builder))

Pokud cesta neexistuje, hostitel se nepodaří spustit.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>Prostředí

Nastaví aplikace [prostředí](xref:fundamentals/environments).

**Klíč**: prostředí  
**Typ**: *řetězec*  
**Výchozí**: produkčního prostředí  
**Sada s použitím**: `UseEnvironment`  
**Proměnná prostředí**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` je [volitelné a uživatelem definovanými](#configuration-builder))

Prostředí můžete nastavit na libovolnou hodnotu. Hodnoty definované v rámci rozhraní zahrnují `Development`, `Staging`, a `Production`. Hodnoty se velká a malá písmena.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Konfigurace Tvůrce aplikací je vytvořen zavoláním [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementace. `ConfigureAppConfiguration` používá [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) k vytvoření [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pro aplikaci. `ConfigureAppConfiguration` můžete volat vícekrát s přičítáním výsledky. Aplikace používá, podle toho, která možnost poslední nastaví hodnotu. Konfigurace vytvořil `ConfigureAppConfiguration` je k dispozici na [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) pro následné operace a v [služby](/dotnet/api/microsoft.extensions.hosting.ihost.services).

Příklad aplikace konfigurace pomocí `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appSettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appSettings. Development.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appSettings. Production.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) rozšiřující metoda není aktuálně podporuje při analýze oddílu konfigurace vrácený [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (například `.AddConfiguration(Configuration.GetSection("section"))`. `GetSection` Metoda filtry konfigurační klíče do části Požadovaná, ale ponechá název oddílu, který na klíče (například `section:Logging:LogLevel:Default`). `AddConfiguration` Metoda očekává se přesně shodují s konfigurační klíče (například `Logging:LogLevel:Default`). Přítomnost název oddílu, který klíčů brání hodnoty v části Konfigurace aplikace. Tento problém bude vyřešen v příští verzi. Další informace a řešení najdete v tématu [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration využívá úplnou klíčů](https://github.com/aspnet/Hosting/issues/839).

Přesunutí souborů nastavení do výstupního adresáře, zadejte soubory nastavení jako [položky projektu nástroje MSBuild](/visualstudio/msbuild/common-msbuild-project-items) v souboru projektu. Ukázková aplikace přesune své soubory JSON aplikace nastavení a *hostsettings.json* následujícím **&lt;obsahu:&gt;** položky:

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a>ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) přidá do aplikace služeb [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru. `ConfigureServices` můžete volat vícekrát s přičítáním výsledky.

Hostovanou službu je třída s atributem logiky úloh na pozadí, který implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní. Další informace najdete v tématu [s hostovanými službami úloh na pozadí](xref:fundamentals/host/hosted-services) tématu.

[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) používá `AddHostedService` metodu rozšíření k přidání služby pro události doby života `LifetimeEventsHostedService`a úlohu na pozadí vypršel časový limit `TimedHostedService`, do aplikace:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) přidá delegáta pro konfiguraci zadaných [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder). `ConfigureLogging` může být volána více než jednou s přičítáním výsledky.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) naslouchá `Ctrl+C`/SIGINT nebo SIGTERM a volání [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) zahájíte proces vypnutí. `UseConsoleLifetime` odblokuje rozšíření, jako [RunAsync](#runasync) a [WaitForShutdownAsync](#waitforshutdownasync). [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) předem registrován jako výchozí implementace životnost. Poslední doba života zaregistrovaný se používá.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Konfigurace kontejneru

Pro podporu připojení v ostatních kontejnerech, může přijmout hostitele [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1). Poskytuje objekt pro vytváření, které nejsou součástí registrace kontejnerů DI, ale místo toho je vnitřní hostitel používá k vytvoření kontejneru pro konkrétní DI. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) přepisuje výchozí objekt pro vytváření použitý k vytvoření poskytovatele služby app service.

Konfigurace vlastního kontejneru je spravován [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) metody. `ConfigureContainer` poskytuje možnosti silného typu pro konfiguraci kontejneru nad základního hostitele rozhraní API. `ConfigureContainer` můžete volat vícekrát s přičítáním výsledky.

Vytvoření služby kontejneru pro aplikace:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Poskytuje objekt pro vytváření služby kontejneru:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Použijte objekt pro vytváření a konfigurace kontejneru vlastní služby pro aplikaci:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Rozšiřitelnost

Rozšiřitelnost hostitele se provádí pomocí metody rozšíření na `IHostBuilder`. Následující příklad ukazuje, jak rozšiřující metoda rozšiřuje `IHostBuilder` implementaci [RabbitMQ](https://www.rabbitmq.com/). Metoda rozšíření (jinde v aplikaci) zaregistruje RabbitMQ `IHostedService`:

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a>Spravovat hostitele

[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementace je zodpovědný za spouštění a zastavování `IHostedService` implementace, které jsou registrovány v kontejneru služby.

### <a name="run"></a>Spustit

[Spustit](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) aplikace spouští a blokuje volající vlákno, dokud nebude ukončen hostitele:

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

### <a name="runasync"></a>RunAsync

[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) aplikace spouští a vrátí `Task` , která se dokončí, když se aktivuje token zrušení nebo vypnutí:

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

### <a name="runconsoleasync"></a>RunConsoleAsync

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) umožňuje podporu konzoly, sestaví a spustí hostitele a čeká na `Ctrl+C`/SIGINT nebo SIGTERM vypnout.

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

### <a name="start-and-stopasync"></a>Počáteční a StopAsync

[Spustit](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) spustí hostitele synchronně.

[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) limitu pokusí zastavit hostitele v rámci zadaného časového limitu.

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

### <a name="startasync-and-stopasync"></a>StartAsync a StopAsync

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) spustí aplikaci.

[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) ukončí aplikaci.

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

### <a name="waitforshutdown"></a>WaitForShutdown

[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) se spouští přes [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), jako například [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (čeká na `Ctrl+C`/SIGINT nebo SIGTERM). `WaitForShutdown` volání [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) vrátí `Task` , která se dokončí při vypnutí se spouští přes daný token a volání [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

### <a name="external-control"></a>Vnější ovládací prvek

Vnější ovládací prvek hostitele lze dosáhnout pomocí metody, které je možné volat externě:

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

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) je volána na začátku [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), která čeká, dokud neskončí než budete pokračovat. To umožňuje zpoždění spuštění, dokud signalizován externí událostí.

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment rozhraní

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) poskytuje informace o aplikaci prvku hostitelské prostředí. Použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:

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

Další informace najdete v tématu [používání více prostředí](xref:fundamentals/environments).

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime rozhraní

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) umožňuje po spuštění a vypnutí aktivity, včetně žádostí o řádné vypnutí. Tři vlastnosti v rozhraní jsou tokeny zrušení použije k registraci `Action` metody, které definují události spuštění a vypnutí.

| Token zrušení | Při aktivaci&#8230; |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | Hostitel plně byla spuštěna. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | Hostitel se dokončuje řádné vypnutí. By měl zpracovat všechny požadavky. Vypnutí blokuje, dokud se tato událost se dokončí. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | Hostitel provádí řádné vypnutí. Žádosti se možná ještě zpracovávají. Vypnutí blokuje, dokud se tato událost se dokončí. |

Vložit konstruktoru `IApplicationLifetime` service do libovolné třídy. [Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) používá konstruktor injektáž do `LifetimeEventsHostedService` třídy ( `IHostedService` implementace) k registraci události.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) požaduje ukončení aplikace. Následující třídy používá `StopApplication` řádně vypnout aplikaci při třídy `Shutdown` volání metody:

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

## <a name="additional-resources"></a>Další zdroje

* [Úlohy na pozadí s hostovanými službami](xref:fundamentals/host/hosted-services)
* [Hostování úložiště ukázek na Githubu](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
