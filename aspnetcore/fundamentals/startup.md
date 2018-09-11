---
title: Spuštění aplikace v ASP.NET Core
author: ardalis
description: Zjistěte, jak třídu pro spuštění v ASP.NET Core konfiguruje služby a kanál žádosti o aplikace.
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: 923d17be9c2bb1a9d338599d1cdc4c34302cddab
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040092"
---
# <a name="application-startup-in-aspnet-core"></a>Spuštění aplikace v ASP.NET Core

Podle [Steve Smith](https://ardalis.com), [Petr Dykstra](https://github.com/tdykstra), a [Luke Latham](https://github.com/guardrex)

Třída `Startup` konfiguruje služby a kanál zpracování požadavků aplikace.

## <a name="the-startup-class"></a>Třída Startup

Aplikace ASP.NET Core používají třídu `Startup`, která je konvenčně pojmenována `Startup`. Třída `Startup`:

* Může volitelně obsahovat metodu [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices), která konfiguruje služby aplikace.
* Musí obsahovat metodu [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) k vytvoření kanálu zpracování požadavků aplikace.

`ConfigureServices` a `Configure` jsou volány modulem runtime při spuštění aplikace:

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

Třídu `Startup` můžete specifikovat pomocí rozšiřující metody [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_):

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Hostitel webové aplikace poskytuje některé služby, které jsou k dispozici konstruktoru třídy `Startup`. Aplikace přidává další služby prostřednictvím metody `ConfigureServices`. Služby hostitele i aplikace jsou pak k dispozici v metodě `Configure` a v celé aplikaci.

Ve třídě `Startup` se běžně používá [vkládání závislostí](xref:fundamentals/dependency-injection) pro vložení:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) pro konfiguraci služeb prostřednictvím prostředí.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pro načtení konfigurace.
* [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) pro vytváření protokolovacího nástroje v `Startup.ConfigureServices`.

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

Alternativou ke vložení `IHostingEnvironment` je použití přístupu založeného na konvencích. Aplikace může definovat oddělenou `Startup` třídu pro různá prostředí (například `StartupDevelopment`). Odpovídající `Startup` třída je vybrána v době běhu. Třída, jejíž název má příponu odpovídající aktuálnímu prostředí, je upřednostněna. Pokud aplikace běží ve vývojovém prostředí a obsahuje třídu `Startup` i třídu `StartupDevelopment`, použije se třída `StartupDevelopment` . Další informace naleznete v tématu [Používání více prostředí](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Další informace o `WebHostBuilder` naleznete ve článku [Hosting](xref:fundamentals/host/index). Informace o zpracování chyb během spuštění naleznete v tématu [Zpracování výjimek při spuštění](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Metoda ConfigureServices

Metoda [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) je:

* volitelná,
* volána hostitelem webové aplikace před voláním metody `Configure`, aby nakonfigurovala příslušné služby,
* metoda, ve které jsou [možnosti konfigurace](xref:fundamentals/configuration/index) nastaveny podle konvence.

Po přidání služeb do kontejneru jsou tyto služby k dispozici v celé aplikaci a v rámci metody `Configure`. Služby jsou zpřístupňovány prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection) nebo [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

Hostitel webové aplikace může konfigurovat některé služby před voláním metod ve třídě `Startup`. Podrobnosti naleznete v tématu [Hostitel v ASP.NET Core](xref:fundamentals/host/index).

Pro funkce, které vyžadují složitější konfiguraci, existují rozšiřující metody `Add[Service]` nad rozhraním [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Typická webová aplikace může registrovat například služby pro Entity Framework, Identity a MVC:

Pro funkce, které vyžadují složitější konfiguraci, existují rozšiřující metody `Add[Service]` nad rozhraním [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Typická webová aplikace může registrovat například služby pro Entity Framework, Identity a MVC:

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="the-configure-method"></a>Metoda Configure

Metoda [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) se používá k upřesnění způsobu, jakým má aplikace reagovat na HTTP požadavky. Kanál zpracování požadavků se konfiguruje přidáváním [middlewarů](xref:fundamentals/middleware/index) k instanci builderu [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder` je dostupný metodě `Configure`, není však registrován v kontejneru služeb. Hosting vytváří `IApplicationBuilder` a předává jej přímo metodě `Configure` ([referenční zdrojový kód](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

[Šablona ASP.NET Core](/dotnet/core/tools/dotnet-new) konfigurující kanál s podporou stránky pro diagnostiku výjimky, [BrowserLink](http://vswebessentials.com/features/browserlink), chybové stránky, statické soubory a ASP.NET Core MVC:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Každá rozšiřující metoda `Use` přidává middleware do kanálu zpracování požadavků. Rozšiřující metoda `UseMvc` například přidává [směrovací middleware](xref:fundamentals/routing) do kanálu zpracování požadavku a konfiguruje [MVC](xref:mvc/overview) jako výchozí obslužnou rutinu.

Každá middlewarová komponenta v kanálu zpracování požadavků zodpovídá za vyvolání další komponenty v kanálu, případně může provést předčasné ukončení řetězce volání. Pokud nedojde k předčasnému ukončení řetězce volání během zpracování požadavku, může libovolný middleware požadavek zpracovat ještě podruhé, než je odeslán klientovi.

Také doplňující služby, jako jsou například `IHostingEnvironment` a `ILoggerFactory`, mohou být specifikovány v signatuře metody. Jsou-li specifikovány, vkládají se do metody za předpokladu jejich dostupnosti.

Další informace o použití `IApplicationBuilder` a pořadí při zpracování požadavků v middlewarech naleznete v tématu [Middleware](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Usnadňující metody

Namísto specifikace třídy `Startup` lze použít usnadňující metody [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) a [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure). Při vícenásobném volání metody `ConfigureServices` se přidají služby ze všech volání. Při vícenásobném volání metody `Configure` se použije poslední volání metody.

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Startup filtry

Použijte [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) pro konfiguraci middlewaru volaného na začátku nebo na konci kanálu middlewaru [Configure](#the-configure-method) aplikace. `IStartupFilter` je užitečný k zajištění toho, aby byl daný middleware spuštěn před nebo po spuštění middlewarů přidaných knihovnami na začátku nebo konci kanálu zpracování požadavků aplikace.

`IStartupFilter` implementuje jedinou metodu [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), která očekává a vrací `Action<IApplicationBuilder>`. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) definuje třídu pro konfiguraci kanálu zpracování požadavků Vaší aplikace. Další informace naleznete v tématu [Vytvoření kanálu middlewaru s IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Každý `IStartupFilter` implementuje jeden nebo více middlewarů v kanálu zpracování požadavků. Filtry jsou volány v pořadí, ve kterém byly přidány do kontejneru služeb. Filtry mohou přidávat middleware před nebo po předání řízení dalšímu filtru, tedy připojují se na začátek nebo konec kanálu aplikace.

[Ukázková aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample)) ukazuje, jak zaregistrovat middleware s pomocí `IStartupFilter`. Ukázková aplikace obsahuje middleware, který nastavuje hodnoty možností z parametru řetězce dotazu (query string parameter):

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` je nakonfigurovaný ve třídě `RequestSetOptionsStartupFilter`:

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` je zaregistrovaný v kontejneru služeb v `ConfigureServices`:

[!code-csharp[](startup/sample/Program.cs?name=snippet1&highlight=4-5)]

Pokud je určena hodnota parametru řetězce dotazu `option`, middleware zpracuje danou hodnotu předtím, než MVC middleware vykreslí odpověď:

![Okno prohlížeče zobrazující vykreslenou stránku Index. Hodnota Option je vykreslena jako 'From Middleware' na základě parametru řetězce dotazu 'option', jehož hodnota je nastavena na 'From Middleware'.](startup/_static/index.png)

Pořadí spuštění middlewarů je nastaveno podle pořadí registrace `IStartupFilter`:

* Několik různých implementací `IStartupFilter` může operovat se stejnými objekty. Pokud je pro Vás důležité pořadí, seřaďte jednotlivé registrace služeb `IStartupFilter` tak, aby odpovídaly pořadí, ve kterém mají být jejich middlewary spuštěny.
* Knihovny mohou přidávat middlewary s jednou nebo více implementacemi `IStartupFilter`, které se spuští před nebo po spuštění ostatních middlewarů aplikace zaregistrovaných pomocí `IStartupFilter`. K vyvolání `IStartupFilter` middlewaru před `IStartupFilter` middlewarem přidaného knihovnou, umístěte registraci Vaší služby před registrací knihovny do kontejneru služeb. Pro opačné pořadí umístěte registraci služby za přidání knihovny.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a> Přidání konfigurace při spuštění z externího sestavení

Implementace [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) umožňuje doplnit konfiguraci při spuštění třídou `Startup` umístěnou mimo aplikaci v externím sestavení (assembly). Další informace naleznete v tématu [Vylepšení aplikace z externího sestavení](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="additional-resources"></a>Další zdroje

* <xref:fundamentals/host/index>
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
