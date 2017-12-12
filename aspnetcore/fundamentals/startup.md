---
title: "Spuštění aplikace v ASP.NET Core"
author: ardalis
description: "Zjistit, jak třída při spuštění v ASP.NET Core nakonfiguruje služby a aplikace požadavku kanálu."
keywords: "ASP.NET Core, spuštění, metoda konfigurace, ConfigureServices – metoda"
ms.author: tdykstra
manager: wpickett
ms.date: 02/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 83b2647df8beec1feae33400224dacf9823be9b4
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="application-startup-in-aspnet-core"></a>Spuštění aplikace v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/) a [tní Dykstra](https://github.com/tdykstra/)

`Startup` Třída nakonfiguruje služby a aplikace požadavku kanálu.

## <a name="the-startup-class"></a>Třída při spuštění

Vyžadují aplikace ASP.NET Core `Startup` třídou, která se nazývá `Startup` podle konvence. Zadejte název třídy spuštění v `Main` programu [WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [ `UseStartup<TStartup>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) metoda. V tématu [hostitelský](xref:fundamentals/hosting) Další informace o `WebHostBuilder`, která se spouští před `Startup`.

Můžete definovat samostatné `Startup` třídy pro různá prostředí a odpovídající bude být vybrána jedna za běhu. Pokud zadáte `startupAssembly` v [konfigurace webového hostitele](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host) nebo možnosti hostování bude načtení tohoto sestavení spuštění a vyhledejte `Startup` nebo `Startup[Environment]` typu. Třídu, jejíž název odpovídá příponu bude možné v pořadí stavět aktuální prostředí, takže když aplikace běží v *vývoj* prostředí a zahrnuje i `Startup` a `StartupDevelopment` třídy, `StartupDevelopment` bude – třída použít. V tématu [FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs) v `StartupLoader` a [práce s několika prostředí](environments.md#startup-conventions).

Alternativně můžete definovat pevná `Startup` třídu, která se použije bez ohledu na prostředí voláním `UseStartup<TStartup>`. Toto je doporučený postup.

`Startup` Konstruktoru třídy může přijmout závislosti, které jsou k dispozici prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection). Běžně je použití `IHostingEnvironment` nastavit [konfigurace](xref:fundamentals/configuration/index) zdroje.

`Startup` Musí obsahovat třídu `Configure` metoda a může volitelně obsahovat `ConfigureServices` metody, které se označují jako při spuštění aplikace. Třída může také obsahovat [prostředí specifické verze těchto metod](xref:fundamentals/environments#startup-conventions). `ConfigureServices`, pokud existuje, je volána před provedením `Configure`.

Další informace o [zpracování výjimek během spuštění aplikace](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Metoda ConfigureServices

[ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_) metoda je volitelné; ale pokud se používá, je volána před provedením `Configure` metoda webového hostitele. Webového hostitele může konfigurovat některé služby před ``Startup`` metody jsou volány (viz [hostování](xref:fundamentals/hosting)). Podle konvence [možnosti konfigurace](xref:fundamentals/configuration/index) jsou nastaveny v této metodě.

Funkce, které vyžadují významné instalace existují `Add[Service]` rozšiřující metody na [IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection). Tento příklad z výchozí šablony webové stránky konfiguruje aplikaci, aby používala services pro rozhraní Entity Framework, Identity a MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

Přidání služeb do kontejneru služby jsou k dispozici v rámci vaší aplikace prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection).

## <a name="services-available-in-startup"></a>K dispozici v spuštění služby

Vkládání závislostí ASP.NET Core poskytuje služby při spuštění aplikace. Tyto služby můžete požádat o, včetně příslušné rozhraní jako parametr vaše `Startup` konstruktoru třídy nebo jeho `Configure` metoda. `ConfigureServices` Metoda přijímá pouze `IServiceCollection` parametr (ale všechny registrované služby může načíst z této kolekce tak další parametry nejsou nutné).

Níže jsou uvedeny některé služby obvykle požadoval `Startup` metody:

* V konstruktoru: `IHostingEnvironment`,`ILogger<Startup>`
* V `ConfigureServices`:`IServiceCollection`
* V `Configure`: `IApplicationBuilder`, `IHostingEnvironment`,`ILoggerFactory`

Žádné služby přidal ``WebHostBuilder`` ``ConfigureServices`` metoda může být požadoval ``Startup`` konstruktoru třídy nebo jeho ``Configure`` metoda. Použití `WebHostBuilder` zajistit žádné služby, je třeba během `Startup` metody.

## <a name="the-configure-method"></a>Metoda konfigurace

`Configure` Metoda se používá k určení, jak bude aplikace ASP.NET reagovat na požadavky HTTP. Kanál požadavku je nakonfigurována tak, že přidáte [middleware](middleware.md) součásti, které `IApplicationBuilder` instance, které poskytuje vkládání závislostí.

V následujícím příkladu z výchozí šablony webové stránky, několik rozšiřující metody slouží ke konfiguraci kanálu s podporou [BrowserLink](http://vswebessentials.com/features/browserlink), chybové stránky, statické soubory, ASP.NET MVC a Identity.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Každý `Use` přidá metody rozšíření [middleware](xref:fundamentals/middleware) součásti požadavku kanálu. Například `UseMvc` přidá metody rozšíření [směrování](routing.md) middleware kanál požadavku a nakonfiguruje [MVC](xref:mvc/overview) jako výchozí obslužnou rutinu.

Další informace o tom, jak používat `IApplicationBuilder`, najdete v části [Middleware](xref:fundamentals/middleware).

Další služby, jako například `IHostingEnvironment` a `ILoggerFactory` můžete také zadat v podpis metody v takovém případě budou tyto služby [vložit](dependency-injection.md) Pokud jsou k dispozici. 

## <a name="additional-resources"></a>Další prostředky

* [Práce s několika prostředí](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware)
* [Protokolování](xref:fundamentals/logging/index)
* [Konfigurace](xref:fundamentals/configuration/index)
