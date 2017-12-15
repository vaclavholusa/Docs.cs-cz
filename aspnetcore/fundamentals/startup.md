---
title: "Spuštění aplikace v ASP.NET Core"
author: ardalis
description: "Zjistit, jak třída při spuštění v ASP.NET Core nakonfiguruje služby a aplikace požadavku kanálu."
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 8adb96c7261a2e7b1556f0daddcf6f135862b53a
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="application-startup-in-aspnet-core"></a>Spuštění aplikace v ASP.NET Core

Podle [Steve Smith](https://ardalis.com), [tní Dykstra](https://github.com/tdykstra), a [Luke Latham](https://github.com/guardrex)

`Startup` Třída nakonfiguruje služby a aplikace požadavku kanálu.

## <a name="the-startup-class"></a>Třída při spuštění

Použití aplikace ASP.NET Core `Startup` třídou, která se nazývá `Startup` podle konvence. `Startup` Třídy:

* Může volitelně obsahovat [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) metoda konfigurace služby pro aplikace.
* Musí zahrnovat [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) metodu pro vytvoření kanálu zpracování žádostí aplikace.

`ConfigureServices`a `Configure` se nazývají modulem runtime při spuštění aplikace:

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

Zadejte `Startup` třídy s [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) metoda:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

`Startup` Konstruktoru třídy přijímá závislosti definované hostitele. Běžně se používají [vkládání závislostí](xref:fundamentals/dependency-injection) do `Startup` třída je vložení [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) konfigurace služby pro prostředí:

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

Alternativu k vložení `IHostingStartup` , je použít přístup na základě konvence. Aplikace můžete definovat samostatné `Startup` třídy pro různá prostředí (například `StartupDevelopment`), a v době běhu je vybraná třída odpovídající při spuštění. Třída, jehož příponu názvu odpovídá aktuální prostředí prioritu. Pokud aplikace běží ve vývojovém prostředí a zahrnuje i `Startup` třídy a `StartupDevelopment` třídy, `StartupDevelopment` třída se používá. Další informace najdete v části [práce s několika prostředí](xref:fundamentals/environments#startup-conventions).

Další informace o `WebHostBuilder`, najdete v článku [hostitelský](xref:fundamentals/hosting) tématu. Informace o zpracování chyb během spouštění najdete v tématu [spuštění zpracování výjimek](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Metoda ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) metoda je:

* Volitelné.
* Je voláno hostitelem webové před `Configure` metoda konfigurace služby pro aplikace.
* Kde [možnosti konfigurace](xref:fundamentals/configuration/index) jsou nastaveny podle konvence.

Přidání služeb do kontejneru služby jsou dostupné v rámci aplikace a v `Configure` metoda. Služby jsou vyřešeny prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection) nebo z [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

Webového hostitele může konfigurovat některé služby před `Startup` metody jsou volány. Podrobnosti najdete v [hostitelský](xref:fundamentals/hosting) tématu. 

Pro funkce, které vyžadují významné instalace, jsou `Add[Service]` rozšiřující metody na [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Typické webové aplikace se zaregistruje services pro rozhraní Entity Framework, Identity a MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>K dispozici v spuštění služby

Webový hostitel poskytuje některé služby, které jsou k dispozici `Startup` konstruktoru třídy. Aplikace přidá další služby prostřednictvím `ConfigureServices`. Hostitele a aplikaci služby jsou pak dostupné v `Configure` a v celé aplikaci.

## <a name="the-configure-method"></a>Metoda konfigurace

[Konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) metoda se používá k určení, jak se aplikace reaguje na požadavky HTTP. Kanál požadavku je nakonfigurována tak, že přidáte [middleware](xref:fundamentals/middleware) součásti, které [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instance. `IApplicationBuilder`je k dispozici `Configure` metoda, ale není registrován v kontejneru služby. Hostování vytvoří `IApplicationBuilder` a předává je přímo na `Configure` ([odkaz na zdroj](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

[Šablony ASP.NET Core](/dotnet/core/tools/dotnet-new) konfiguraci kanálu s podporou pro vývojáře výjimka stránku, [BrowserLink](http://vswebessentials.com/features/browserlink), chybové stránky, statické soubory a architektura ASP.NET MVC:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Každý `Use` metoda rozšíření přidá do kanálu požadavku komponenta middlewaru. Například `UseMvc` přidá metody rozšíření [směrování middleware](xref:fundamentals/routing) do kanálu požadavku a nakonfiguruje [MVC](xref:mvc/overview) jako výchozí obslužnou rutinu.

Další služby, jako například `IHostingEnvironment` a `ILoggerFactory`, můžete také zadat v podpis metody. -Li zadána, jsou vložit další služby, pokud jsou k dispozici.

Další informace o tom, jak používat `IApplicationBuilder`, najdete v části [Middleware](xref:fundamentals/middleware).

## <a name="convenience-methods"></a>Usnadňující metody

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) a [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) usnadňující metody lze použít místo zadávání `Startup` třídy. Více volá, aby se `ConfigureServices` připojit k sobě. Vícenásobné volání `Configure` použijte posledního volání metody.

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=16,20)]

## <a name="startup-filters"></a>Spuštění filtrů

Použití [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) pro konfiguraci middlewaru na začátku nebo na konci aplikace [konfigurace](#the-configure-method) middlewaru v řadě. `IStartupFilter`je užitečné k zajištění, že middleware běží před nebo po middleware přidal knihovny na začátku nebo na konci kanálu zpracování žádostí aplikace.

`IStartupFilter`implementuje jedné metody [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), který obdrží a vrátí `Action<IApplicationBuilder>`. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) definuje třída ke konfiguraci aplikace požadavku kanálu. Další informace najdete v tématu [vytváření middlewaru v řadě s IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).

Každý `IStartupFilter` implementuje jeden nebo více middlewares v kanálu požadavku. Filtry je vyvolán v pořadí, ve kterém byly přidány do kontejneru služby. Filtry může přidat middleware před nebo po úspěšném řízení další filtru, proto se připojit k začátku nebo na konci kanálu aplikace.

[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample)) ukazuje, jak zaregistrovat middleware s `IStartupFilter`. Ukázková aplikace obsahuje middleware, která nastavuje hodnoty možností z parametr řetězce dotazu:

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` Je nakonfigurovaný v `RequestSetOptionsStartupFilter` třídy:

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` Je zaregistrován v kontejneru služby `ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Když parametr řetězce dotazu pro `option` je zadané middleware zpracovává přiřazení hodnoty před MVC middleware vykreslí odpovědi:

![Okno prohlížeče zobrazující na vykreslené stránce Index. Hodnota možnosti je vykreslen jako z middlewaru založené na vyžádání stránky se parametr řetězce dotazu a hodnotou možnost nastavena na hodnotu z middlewaru.](startup/_static/index.png)

Pořadí spuštění middlewaru je nastavena podle pořadí podle `IStartupFilter` registrace:

* Více `IStartupFilter` implementace může komunikovat s objekty stejné. Pokud pořadí je důležité, pořadí jejich `IStartupFilter` služby registrace tak, aby odpovídala pořadí, ve kterém se má spustit jejich middlewares.
* Knihovny může přidávat middlewaru s jedním nebo více `IStartupFilter` implementací, které spustit před nebo po dalším middlewarem aplikace zaregistrována `IStartupFilter`. K vyvolání `IStartupFilter` middleware před middleware přidal knihovny `IStartupFilter`, umístěte registrace služby před knihovny je přidat do kontejneru služby. K vyvolání následně, umístěte registrace služby po přidání knihovny.

## <a name="additional-resources"></a>Další zdroje

* [Hostování](xref:fundamentals/hosting)
* [Práce s několika prostředí](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware)
* [Protokolování](xref:fundamentals/logging/index)
* [Konfigurace](xref:fundamentals/configuration/index)
* [Třída StartupLoader: metoda FindStartupType (referenční dokumentace zdroje)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116))
