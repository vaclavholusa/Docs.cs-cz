---
title: Spuštění aplikace v ASP.NET Core
author: ardalis
description: Zjistěte, jak třídu pro spuštění v ASP.NET Core konfiguruje služby a kanál žádosti o aplikace.
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: 228719863400f30643cd3007df5291ee807cffba
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870890"
---
# <a name="application-startup-in-aspnet-core"></a>Spuštění aplikace v ASP.NET Core

Podle [Steve Smith](https://ardalis.com), [Petr Dykstra](https://github.com/tdykstra), a [Luke Latham](https://github.com/guardrex)

`Startup` Třída nakonfiguruje služby a kanál žádosti o aplikace.

## <a name="the-startup-class"></a>Třída při spuštění

Použití aplikace ASP.NET Core `Startup` třídy, který se nazývá `Startup` konvencí. `Startup` Třídy:

* Může volitelně zahrnovat [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) metoda konfigurace služby pro aplikace.
* Musí obsahovat [konfigurovat](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) metodu pro vytvoření kanálu zpracování žádosti o aplikace.

`ConfigureServices` a `Configure` jsou volány modulem runtime při spuštění aplikace:

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

Zadejte `Startup` třídy s [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) metody:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Webového hostitele obsahuje některé služby, které jsou k dispozici na `Startup` konstruktoru třídy. Aplikace přidá další služby prostřednictvím `ConfigureServices`. Hostitelské i aplikaci služby jsou pak k dispozici v `Configure` a v celé aplikaci.

Běžně [injektáž závislostí](xref:fundamentals/dependency-injection) do `Startup` třída je vložení:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) konfigurace služby pro prostředí.
* [Parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) načíst konfiguraci.
* [Implementaci třídy ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) pro vytváření protokolovacího nástroje v `Startup.ConfigureServices`.

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

Alternativa k vkládání `IHostingEnvironment` je chcete použít přístup na základě konvence. Aplikace můžete definovat zvláštní `Startup` třídy pro různá prostředí (například `StartupDevelopment`) a odpovídající `Startup` třídy je vybrané v době běhu. Třídy, jejichž přípona názvu odpovídá aktuální prostředí je nastaveno jako prioritní. Pokud aplikace běží ve vývojovém prostředí a zahrnuje i `Startup` třídy a `StartupDevelopment` třídy, `StartupDevelopment` třída se používá. Další informace najdete v tématu [používání více prostředí](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Další informace o `WebHostBuilder`, najdete v článku [Hosting](xref:fundamentals/host/index) tématu. Informace o zpracování chyb během spouštění najdete v tématu [zpracování výjimek při spuštění](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>ConfigureServices – metoda

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) metoda je:

* Nepovinné
* Je voláno hostitelem webové před `Configure` metoda konfigurace služby pro aplikace.
* Kde [možnosti konfigurace](xref:fundamentals/configuration/index) jsou nastaveny podle konvence.

Přidání služeb do kontejneru služby je k dispozici v aplikaci a `Configure` metody. Služby jsou vyřešeny prostřednictvím [injektáž závislostí](xref:fundamentals/dependency-injection) nebo z [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

Hostitel webové nakonfigurovat některé služby před `Startup` metody jsou volány. Podrobnosti najdete v [hostitele v ASP.NET Core](xref:fundamentals/host/index) tématu.

Funkce, které vyžadují značné instalační program, existují `Add[Service]` rozšiřující metody na [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Běžné webové aplikace registruje služby pro Entity Framework, Identity a MVC:

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="the-configure-method"></a>Konfigurovat – metoda

[Konfigurovat](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) metoda se používá k určení, jak aplikace reaguje na požadavky HTTP. Kanál žádosti je nakonfigurovaný tak, že přidáte [middleware](xref:fundamentals/middleware/index) součástí [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instance. `IApplicationBuilder` je k dispozici na `Configure` metody, ale není registrován v kontejneru služby. Hostování vytvoří `IApplicationBuilder` a předává je přímo na `Configure` ([zdroj odkazu](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

[Šablony ASP.NET Core](/dotnet/core/tools/dotnet-new) konfiguraci kanálu s podporou pro stránku výjimky pro vývojáře [BrowserLink](http://vswebessentials.com/features/browserlink), chybové stránky, statické soubory a ASP.NET Core MVC:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Každý `Use` rozšiřující metoda přidá do kanálu požadavku komponenta middlewaru. Například `UseMvc` přidá metody rozšíření [směrování Middleware](xref:fundamentals/routing) do kanálu požadavku a nakonfiguruje [MVC](xref:mvc/overview) jako výchozího popisovače.

Jednotlivé komponenty middleware v kanálu požadavku zodpovídá za vyvolání další komponenta v kanálu nebo zkrácenou řetězci, pokud je to vhodné. Pokud zkrácenou nedojde, middleware řetězce, má každý middleware druhou šanci na zpracování žádosti. před odesláním do klienta.

Další služby, jako například `IHostingEnvironment` a `ILoggerFactory`, může také uvést v signatuře metody. -Li zadána, se vkládají další služby, pokud jsou k dispozici.

Další informace o tom, jak používat `IApplicationBuilder` a pořadí zpracování middleware, naleznete v tématu [Middleware](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Vhodné metody

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) a [konfigurovat](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) vhodné metody je možné použít místo zadávání `Startup` třídy. Více volání `ConfigureServices` připojit k sobě navzájem. Více volání `Configure` použít poslední volání metody.

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Po spuštění filtry

Použití [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) pro konfiguraci middlewaru na začátku nebo na konci vaší aplikace [konfigurovat](#the-configure-method) middleware kanálu. `IStartupFilter` je užitečný k zajištění toho, že middleware běží před nebo po middleware přidal knihovny na začátku nebo konci kanál pro zpracování požadavku aplikace.

`IStartupFilter` implementuje jedinou metodu [konfigurovat](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), která přijímá a vrací `Action<IApplicationBuilder>`. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) definuje třídu ke konfiguraci kanálu požadavku vaší aplikace. Další informace najdete v tématu [vytvoření kanálu middlewaru s IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Každý `IStartupFilter` implementuje jednu nebo více middlewares v kanálu požadavku. Filtry jsou vyvolány v pořadí, ve kterém byly přidány do kontejneru služby. Filtry mohou přidat middleware před nebo po předá řízení dalšímu filtru, proto se připojit k začátku nebo konci kanálu aplikací.

[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample)) ukazuje, jak se zaregistrovat middleware s `IStartupFilter`. Ukázkové aplikace zahrnují middleware, který nastavuje hodnoty možností z parametru řetězce dotazu:

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` Je nakonfigurovaný v `RequestSetOptionsStartupFilter` třídy:

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` Je zaregistrovaný v kontejneru služby `ConfigureServices`:

[!code-csharp[](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Když parametr řetězce dotazu pro `option` je k dispozici, middleware zpracovává přiřazení hodnoty před MVC middleware vykreslí odpovědi:

![Okno prohlížeče na vykreslené stránce Index. Hodnota možnosti je vykreslen jako z middlewaru podle požadujícím příslušnou stránku, pomocí parametru řetězce dotazu a hodnota možnosti z middlewaru.](startup/_static/index.png)

Pořadí spuštění middlewaru je nastavena podle pořadí podle `IStartupFilter` registrace:

* Více `IStartupFilter` implementace může komunikovat se stejnými objekty. Pokud pořadí je důležité, pořadí jejich `IStartupFilter` služby registrace tak, aby odpovídala pořadí, ve kterém jejich middlewares by měly být spuštěny.
* Knihovny přidat middlewaru s jedním nebo více `IStartupFilter` implementace, které spustit před nebo po dalším middlewarem aplikace zaregistrované `IStartupFilter`. K vyvolání `IStartupFilter` middleware před middleware přidal knihovny `IStartupFilter`, umístěte registrace služby před knihovny se přidá do kontejneru služby. K vyvolání později, umístěte registrace služby po přidání knihovny.

## <a name="adding-configuration-at-startup-from-an-external-assembly"></a>Přidává se konfigurace při spuštění z externího sestavení

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementace umožňuje přidání vylepšení do aplikace při spuštění z externího sestavení mimo aplikaci prvku `Startup` třídy. Další informace najdete v tématu [vylepšení aplikace z externího sestavení](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="additional-resources"></a>Další zdroje

* <xref:fundamentals/host/index>
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
* [Třída StartupLoader: metoda FindStartupType (odkaz na zdroj)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
