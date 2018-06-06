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
# <a name="net-generic-host"></a>Obecné hostitele rozhraní .NET

Podle [Luke Latham](https://github.com/guardrex)

Konfigurace aplikace .NET a spusťte *hostitele*. Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací. Toto téma obsahuje obecné hostitele ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), což je užitečné pro hostování aplikací, které nejsou zpracovávají požadavky HTTP. Pro pokrytí webového hostitele ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), najdete v článku [webového hostitele](xref:fundamentals/host/web-host) tématu.

Cílem obecné hostitele je oddělit kanál protokolu HTTP z hostitele webového rozhraní API umožňující širší škálu scénářů hostitele. Zasílání zpráv, úlohy na pozadí a další úlohy jiným protokolem než HTTP podle výhody obecné hostitele z vyjímání mezi funkce, jako je například konfigurace, vkládání závislostí (DI) a protokolování.

Obecné hostitele je nového v technologii ASP.NET Core 2.1 a není vhodná pro webového hostingu scénáře. Pro scénáře hostování, web, použijte [webového hostitele](xref:fundamentals/host/web-host). Obecné hostitel je ve vývoji a nahraďte webového hostitele v budoucí verzi fungovat jako hostitel primární rozhraní API v protokolu HTTP a scénáře jiným protokolem než HTTP.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Při spuštění ukázkové aplikace [Visual Studio Code](https://code.visualstudio.com/), použijte *externí nebo integrované Terminálové*. Nelze spustit ukázku v `internalConsole`.

Nastavení konzole ve Visual Studio Code:

1. Otevřete *.vscode/launch.json* souboru.
1. V **.NET Core spuštění (konzola)** konfigurace, vyhledejte **konzoly** položku. Nastavte hodnotu na buď `externalTerminal` nebo `integratedTerminal`.

## <a name="introduction"></a>Úvod

Je k dispozici v knihovně obecné hostitele [obor názvů Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) a poskytované [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) balíčku. `Microsoft.Extensions.Hosting` Je součástí balíčku [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 nebo vyšší).

[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) se vstupním bodem k provádění kódu. Každý `IHostedService` implementace je provést v pořadí podle [služby registrace v ConfigureServices](#configureservices). [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) je volána v každém `IHostedService` po spuštění hostitele a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) je volána v pořadí zpětné registrace, když řádně vypne hostitele.

## <a name="set-up-a-host"></a>Nastavení hostitele

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) hlavní součást, knihovny a aplikace použít k inicializaci, vytvoření a spuštění hostitele:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a>Konfigurace hostitele

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) závisí na následujících dvou přístupů k nastavení hostitelské hodnoty konfigurace:

* Tvůrce konfigurace
* Konfigurace metody rozšíření

### <a name="configuration-builder"></a>Tvůrce konfigurace

Konfigurace hostitele tvůrce vytvoří volání [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementace. `ConfigureHostConfiguration` používá [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) vytvořit [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pro hostitele. Inicializuje Tvůrce konfigurace [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) pro použití v procesu sestavení aplikace. `ConfigureHostConfiguration` je možné volat vícekrát s sčítání výsledky. Hostitel používá. obě tyto možnosti nastaví hodnotu poslední.

*hostsettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Příklad `HostBuilder` konfigurace pomocí `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) metoda rozšíření není aktuálně schopen analyzovat konfigurační oddíl vrácený [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (například `.AddConfiguration(Configuration.GetSection("section"))`. `GetSection` Metoda filtry konfigurace klíče do části požadovaný, ale ponechá název oddílu na klíče (například `section:environment`). `AddConfiguration` Metoda očekává klíče tak, aby odpovídala `HostBuilder` klíče (například `environment`). Přítomnost názvu oddílu na klíče zabrání hodnoty v části Konfigurace hostitele. Tento problém bude vyřešen v příští verzi. Další informace a řešení, najdete v části [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration používá úplné klíče](https://github.com/aspnet/Hosting/issues/839).

### <a name="extension-method-configuration"></a>Konfigurace metody rozšíření

Rozšiřující metody jsou volány na `IHostBuilder` implementace konfigurace kořenu obsahu a prostředí.

#### <a name="content-root"></a>Obsah kořenové

Toto nastavení určuje, kde začíná hostitele hledání obsahu souborů.

**Klíč**: contentRoot  
**Typ**: *řetězec*  
**Výchozí**: výchozí složce, kde se nachází sestavení aplikace.  
**Nastavit pomocí**: `UseContentRoot`  
**Proměnné prostředí**: `ASPNETCORE_CONTENTROOT`

Pokud cesta neexistuje, hostitel se nepodaří spustit.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>Prostředí

Nastaví aplikace [prostředí](xref:fundamentals/environments).

**Klíč**: prostředí  
**Typ**: *řetězec*  
**Výchozí**: produkční  
**Nastavit pomocí**: `UseEnvironment`  
**Proměnné prostředí**: `ASPNETCORE_ENVIRONMENT`

Prostředí můžete nastavit na jakoukoli hodnotu. Framework definované hodnoty zahrnují `Development`, `Staging`, a `Production`. Hodnoty nejsou velká a malá písmena. Ve výchozím nastavení *prostředí* je pro čtení z `ASPNETCORE_ENVIRONMENT` proměnné prostředí. Při použití [Visual Studio](https://www.visualstudio.com/), proměnné prostředí může být nastavena v *launchSettings.json* souboru. Další informace najdete v tématu [použijte prostředí s více](xref:fundamentals/environments).

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Konfigurace Tvůrce aplikací je vytvořená voláním [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementace. `ConfigureAppConfiguration` používá [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) vytvořit [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pro aplikaci. `ConfigureAppConfiguration` je možné volat vícekrát s sčítání výsledky. Aplikace používá, podle toho, která možnost nastaví hodnotu poslední. Konfigurace vytvořených `ConfigureAppConfiguration` je k dispozici na [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) pro následující operace a v [služby](/dotnet/api/microsoft.extensions.hosting.ihost.services).

Příklad aplikace konfigurace pomocí `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appSettings.JSON určený*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appSettings. Development.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appSettings. Production.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) metoda rozšíření není aktuálně schopen analyzovat konfigurační oddíl vrácený [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (například `.AddConfiguration(Configuration.GetSection("section"))`. `GetSection` Metoda filtry konfigurace klíče do části požadovaný, ale ponechá název oddílu na klíče (například `section:Logging:LogLevel:Default`). `AddConfiguration` Metoda očekává se přesně shodují s konfigurace klíče (například `Logging:LogLevel:Default`). Přítomnost názvu oddílu na klíče zabrání hodnoty v části Konfigurace aplikace. Tento problém bude vyřešen v příští verzi. Další informace a řešení, najdete v části [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration používá úplné klíče](https://github.com/aspnet/Hosting/issues/839).

## <a name="configureservices"></a>ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) přidá do aplikace služeb [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru. `ConfigureServices` je možné volat vícekrát s sčítání výsledky.

Hostovaná služba je třída s pozadí úloh logiky, která implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní. Další informace najdete v tématu [pozadí úlohy s hostované služby](xref:fundamentals/host/hosted-services) tématu.

[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) používá `AddHostedService` rozšíření metoda pro přidání služby pro události životního cyklu `LifetimeEventsHostedService`a úlohy na pozadí vypršel `TimedHostedService`, do aplikace:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) přidá delegáta pro konfiguraci poskytnutého [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder). `ConfigureLogging` může volat vícekrát s sčítání výsledky.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) naslouchá `Ctrl+C`/SIGINT nebo SIGTERM – a volání [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) ke spuštění procesu vypnutí. `UseConsoleLifetime` například odblokuje rozšíření [RunAsync](#runasync) a [WaitForShutdownAsync](#waitforshutdownasync). [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) je předem zaregistrovaný jako výchozí implementace životního cyklu. Poslední doba platnosti zaregistrovat se používá.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Konfiguraci kontejneru

Pro podporu zapojení do jiných kontejnerů, může přijmout hostitele [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1). Poskytuje objekt pro vytváření není součástí registrace kontejneru DI, ale místo toho je vnitřní hostitele použít k vytvoření kontejneru konkrétní DI. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) přepíše výchozí objekt pro vytváření použitý k vytvoření aplikace poskytovatele služeb.

Spravuje konfiguraci vlastní kontejneru [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) metoda. `ConfigureContainer` pro konfiguraci kontejneru nad základního hostitele rozhraní API, poskytuje možnosti silného typu. `ConfigureContainer` je možné volat vícekrát s sčítání výsledky.

Vytvoření kontejneru služby pro aplikaci:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Zadejte objekt kontejneru služby:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Použití objektu pro vytváření a konfiguraci kontejneru vlastní služby pro aplikaci:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Rozšiřitelnost

Rozšíření hostitele se provádí pomocí metody rozšíření na `IHostBuilder`. Následující příklad ukazuje, jak rozšiřuje metodu rozšíření `IHostBuilder` implementace s [RabbitMQ](https://www.rabbitmq.com/). Registr – metoda rozšíření (jinde v aplikaci) RabbitMQ `IHostedService`:

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a>Spravovat hostitele

[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementace je zodpovědná za spuštění a zastavení `IHostedService` implementace, které jsou zaregistrovány v kontejneru služby.

### <a name="run"></a>Spustit

[Spustit](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) aplikace spouští a blokuje volající vlákno, dokud se vypne hostitele:

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

[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) aplikace spouští a vrátí `Task` která se dokončí při aktivaci token zrušení nebo ukončení:

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

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) umožňuje podporu konzoly, vytvoří a spustí hostitele a čeká na `Ctrl+C`/SIGINT nebo SIGTERM – vypnout.

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

[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) pokusí zastavit hostitele během zadaného časového limitu.

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

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) se spustí aplikace.

[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) zastaví aplikace.

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

[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) aktivaci prostřednictvím [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), jako například [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (čeká na `Ctrl+C`/SIGINT nebo SIGTERM –). `WaitForShutdown` volání [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) vrátí `Task` která se dokončí při vypnutí aktivaci prostřednictvím daný token a volání [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

### <a name="external-control"></a>Externí ovládací prvek

Externí řízení hostitele lze dosáhnout pomocí metod, které je možné volat externě:

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

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) nazývá na začátku [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), který bude čekat, dokud nebude dokončeno než budete pokračovat. Tímto lze zpoždění spuštění, dokud signalizovala externí událostí.

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment rozhraní

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) poskytuje informace o aplikaci hostitelského prostředí. Použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:

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

Další informace najdete v tématu [použijte prostředí s více](xref:fundamentals/environments).

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime rozhraní

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) umožňuje po spuštění a vypnutí aktivity, včetně žádostí řádné vypnutí. Zrušení tokenů použitá pro zaregistrování jsou tři vlastnosti na rozhraní `Action` metody, které definují události spuštění a vypnutí.

| Token zrušení | Při aktivaci&#8230; |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | Hostitele plně byla spuštěna. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | Hostitel je dokončení řádné vypnutí. Všechny požadavky, měla by být zpracována. Vypnutí bloky až po dokončení této události. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | Hostitel provádí řádné vypnutí. Může být stále aktivní žádosti. Vypnutí bloky až po dokončení této události. |

Konstruktor vložit `IApplicationLifetime` služby do jakékoli třídy. [Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) používá contructor vkládání do `LifetimeEventsHostedService` – třída ( `IHostedService` implementace) k registraci události.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) požadavky ukončení aplikace. Následující třídy používá `StopApplication` korektně vypnout aplikaci při třídy `Shutdown` metoda je volána:

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
* [Hostování ukázky úložišti na Githubu](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
