---
title: Základy ASP.NET Core
author: rick-anderson
description: Seznamte se základními koncepty pro vytváření aplikací ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 68760f179c4d6e806510b727e2284f8c2c4a4ff6
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/20/2018
ms.locfileid: "41754639"
---
# <a name="aspnet-core-fundamentals"></a>Základy ASP.NET Core

Aplikace ASP.NET Core je konzolová aplikace, která vytváří webový server ve své `Main` metodě:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

V metodě `Main` se volá metoda [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), která poskytuje implementaci návrhového vzoru [Builder](https://wikipedia.org/wiki/Builder_pattern) a umožňuje tak sestavení hostitele webové aplikace. Samotný Builder obsahuje metody, které specifikují webový server (například <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>), který se má použít, a třídu pro spuštění (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). Ve výše uvedeném příkladu je automaticky použit webový server [Kestrel](xref:fundamentals/servers/kestrel). ASP.NET Core se zároveň pokusí o spuštění aplikace ve službě IIS, pokud je k dispozici. Voláním patřičné rozšiřující metody je možné aplikaci hostovat i na jiných webových serverech, jako například [HTTP.sys](xref:fundamentals/servers/httpsys). `UseStartup` je vysvětleno v další části.

Typ <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, který je vrácen jako výsledek volání `WebHost.CreateDefaultBuilder`, obsahuje mnoho volitelných metod. Jednou z těchto metod je `UseHttpSys` pro hostování aplikace pomocí HTTP.sys a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pro určení kořenového adresáře s obsahem. Metody <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> slouží k sestavení objektu <xref:Microsoft.AspNetCore.Hosting.IWebHost>, který je hostitelem aplikace, resp. zahájí naslouchání HTTP požadavků.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

V metodě `Main` se používá třída <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, která implementuje návrhový vzor [Builder](https://wikipedia.org/wiki/Builder_pattern) a umožňuje tak sestavení hostitele webové aplikace. Samotný Builder obsahuje metody, které specifikují webový server (například <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>), který se má použít, a třídu pro spuštění (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). Ve výše uvedeném příkladu je použit webový server [Kestrel](xref:fundamentals/servers/kestrel). Voláním patřičné rozšiřující metody je možné aplikaci hostovat i na jiných webových serverech, jako například [WebListener](xref:fundamentals/servers/weblistener).  `UseStartup` je vysvětleno v další části.

Třída `WebHostBuilder` obsahuje mnoho volitelných metod. Jednou z těchto metod je <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> pro hostování aplikace pomocí služby IIS nebo IIS Express a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pro určení kořenového adresáře s obsahem. Metody <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> slouží k sestavení objektu <xref:Microsoft.AspNetCore.Hosting.IWebHost>, který je hostitelem aplikace, resp. zahájí naslouchání HTTP požadavků.

::: moniker-end

## <a name="startup"></a>Třída pro spuštění

Metoda `UseStartup` třídy `WebHostBuilder` určuje spouštěcí třídu `Startup` Vaší aplikace:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

Ve třídě `Startup` můžete definovat kanál zpracování požadavků a nakonfigurovat všechny služby, které aplikace vyžaduje. Třída `Startup` musí být veřejná a musí obsahovat následující metody:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> definuje [služby](#dependency-injection-services) používané v aplikaci (například ASP.NET, Core MVC, Entity Framework Core, Identity). <xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> definuje [middlewary](xref:fundamentals/middleware/index) pro kanál zpracování požadavků.

Další informace naleznete v tématu <xref:fundamentals/startup>.

## <a name="content-root"></a>Kořenové obsahu

Obsahu kořenový adresář je základní cesta k obsahu používat aplikace, jako například [Razor Pages](xref:razor-pages/index), MVC zobrazení a statické prostředky. Ve výchozím nastavení obsahu kořenový adresář je na stejné umístění jako základní cesta aplikace pro spustitelný soubor, který je hostitelem aplikace.

## <a name="web-root"></a>Kořenový adresář webové

Kořenový adresář webové aplikace je adresář, do projektu obsahující veřejné, statické prostředky, jako jsou šablony stylů CSS, JavaScript a soubory obrázků.

## <a name="dependency-injection-services"></a>Injektáž závislostí (služby)

A *služby* je komponenta, která je určená pro běžné používání v aplikaci. Služby jsou k dispozici prostřednictvím [injektáž závislostí (DI)](xref:fundamentals/dependency-injection). ASP.NET Core zahrnuje nativní kontejner řízení IOC (Inversion), který podporuje [konstruktor vkládání](xref:mvc/controllers/dependency-injection#constructor-injection) ve výchozím nastavení. Pokud chcete, můžete nahradit výchozí kontejner. Kromě jeho [volné párování výhodu](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI umožňuje službám, jako například [protokolování](xref:fundamentals/logging/index), která je dostupná v celé aplikaci.

Další informace naleznete v tématu <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Middleware

V ASP.NET Core, vytvoříte pomocí kanálu požadavku [middleware](xref:fundamentals/middleware/index). ASP.NET Core middleware provádí asynchronní operace `HttpContext` a potom vyvolá další middleware v kanálu nebo ukončí požadavku.

Podle konvence, middleware komponenty s názvem "XYZ" se přidá do kanálu vyvoláním `UseXYZ` metody rozšíření v `Configure` metody.

ASP.NET Core obsahuje bohatou sadu integrovaných middleware a můžete napsat vlastního middlewaru. [Otevřete Web Interface pro .NET (OWIN)](xref:fundamentals/owin), který umožňuje webové aplikace k oddělení od webové servery, je podporováno v aplikacích pro ASP.NET Core.

Další informace naleznete v tématu <xref:fundamentals/middleware/index> a <xref:fundamentals/owin>.

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Iniciování požadavků HTTP

<xref:System.Net.Http.IHttpClientFactory> je k dispozici pro přístup k <xref:System.Net.Http.HttpClient> instance požadavky HTTP.

Další informace naleznete v tématu <xref:fundamentals/http-requests>.

::: moniker-end

## <a name="environments"></a>Prostředí

Prostředí, jako například *vývoj* a *produkční*, jsou hodnoty první třídy v ASP.NET Core a je možné nastavit použití proměnné prostředí, soubor s nastavením a argument příkazového řádku.

Další informace naleznete v tématu <xref:fundamentals/environments>.

## <a name="hosting"></a>Hostování

Konfigurace aplikace ASP.NET Core a spouštění *hostitele*, který je zodpovědný za spouštění a životního cyklu správy aplikací.

Další informace naleznete v tématu <xref:fundamentals/host/index>.

## <a name="servers"></a>Servery

ASP.NET Core, model hostingu není přímo naslouchat žádostem. Model hostingu závisí na implementaci serveru HTTP k předání požadavku do aplikace. Předaný požadavek je zabalena jako sada objekty funkce, které budou přístupné prostřednictvím rozhraní. ASP.NET Core zahrnuje spravovaná, napříč platformami webový server, volá [Kestrel](xref:fundamentals/servers/kestrel). Kestrel obvykle běží za produkční webový server, jako například [IIS](https://www.iis.net/) nebo [Nginx](http://nginx.org) konfigurace reverzního proxy serveru. Kestrel můžete spustit také jako hraniční server přímo přístupný z Internetu v ASP.NET Core 2.0 nebo novější.

Další informace naleznete v tématu <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Konfigurace

ASP.NET Core využívá konfigurační model založený na páry název hodnota. Konfigurační model není založen na <xref:System.Configuration> nebo *web.config*. Konfigurace nastavení získá ze seřazené sady poskytovatelů konfigurace. Poskytovatelé konfigurace integrovanou podporu širokou škálu formátů souborů (XML, JSON, INI), proměnné a argumenty příkazového řádku. Můžete taky psát vlastní zprostředkovatele vlastní konfigurace.

Další informace naleznete v tématu <xref:fundamentals/configuration/index>.

## <a name="logging"></a>protokolování

ASP.NET Core podporuje rozhraní API protokolování, které funguje s různými zprostředkovatelů protokolování. Předdefinované zprostředkovatele podpory odesílání protokolů do jednoho nebo více cílů. Můžete použít rozhraní protokolování třetích stran.

Další informace naleznete v tématu <xref:fundamentals/logging/index>.

## <a name="error-handling"></a>Zpracování chyb

ASP.NET Core má integrované scénáře pro zpracování chyb v aplikacích, včetně stránku výjimky pro vývojáře, vlastní chybové stránky, statické stav znakové stránky a zpracování výjimek při spuštění.

Další informace naleznete v tématu <xref:fundamentals/error-handling>.

## <a name="routing"></a>Směrování

ASP.NET Core nabízí scénáře pro směrování žádostí na aplikace obslužné rutině trasy.

Další informace naleznete v tématu <xref:fundamentals/routing>.

## <a name="file-providers"></a>Zprostředkovatelé souborů

ASP.NET Core abstrahuje přístupu k systému souborů prostřednictvím zprostředkovatelé souborů, která nabízí obecné rozhraní pro práci se soubory napříč platformami.

Další informace naleznete v tématu <xref:fundamentals/file-providers>.

## <a name="static-files"></a>Statické soubory

Statické soubory Middleware slouží statické soubory, jako jsou soubory HTML, CSS, image a JavaScript.

Další informace naleznete v tématu <xref:fundamentals/static-files>.

## <a name="session-and-app-state"></a>Stav relace a aplikace

ASP.NET Core nabízí několik způsobů zachovat stav relace a aplikace, zatímco uživatel prochází webové aplikace.

Další informace naleznete v tématu <xref:fundamentals/app-state>.

## <a name="globalization-and-localization"></a>Globalizace a lokalizace

Vytvoření vícejazyčné web pomocí ASP.NET Core umožňuje oslovit větší cílovou skupinu webu. ASP.NET Core poskytuje služby a middleware pro lokalizaci obsahu do různých jazyků a kultur.

Další informace naleznete v tématu <xref:fundamentals/localization>.

## <a name="request-features"></a>Funkce požadavků

Podrobnosti o implementaci webového serveru související s požadavky HTTP a odpovědí jsou definovány v rozhraní. Tato rozhraní jsou používány implementací serveru a middleware k vytvoření a úprava kanálu hostitelské aplikace.

Další informace naleznete v tématu <xref:fundamentals/request-features>.

## <a name="background-tasks"></a>Úlohy na pozadí

Úlohy na pozadí jsou implementovány jako *hostovaných služeb*. Hostovanou službu je třída s atributem logiky úloh na pozadí, který implementuje <xref:Microsoft.Extensions.Hosting.IHostedService> rozhraní.

Další informace naleznete v tématu <xref:fundamentals/host/hosted-services>.

## <a name="access-httpcontext"></a>Přístup k objektu HttpContext

`HttpContext` Při zpracování žádosti se stránkami Razor a technologií MVC je automaticky k dispozici. V případech kdy `HttpContext` není snadno k dispozici, můžete přistupovat `HttpContext` prostřednictvím <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> rozhraní a jeho výchozí implementace <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.

Další informace naleznete v tématu <xref:fundamentals/httpcontext>.

## <a name="websockets"></a>Protokoly Websocket

[Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP. Používá se pro aplikace, například konverzace, akciích, hry a kdekoli vyžadujete funkcí v reálném čase ve webové aplikaci. ASP.NET Core podporuje další scénáře využití webových soketů.

Další informace naleznete v tématu <xref:fundamentals/websockets>.

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a>Microsoft.AspNetCore.App Microsoft.aspnetcore.all

[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) Microsoft.aspnetcore.all zjednodušuje správu balíčků.

Další informace naleznete v tématu <xref:fundamentals/metapackage-app>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a>Metabalíček Microsoft.aspnetcore.all

[Metabalíček](https://www.nuget.org/packages/Microsoft.AspNetCore.All) Microsoft.aspnetcore.all pro ASP.NET Core zahrnuje:

* Všechny podporované balíčky týmem ASP.NET Core.
* Všechny podporované balíčky pomocí Entity Framework Core.
* Interní a 3. stran závislosti používat ASP.NET Core a Entity Framework Core.

Další informace naleznete v tématu <xref:fundamentals/metapackage>.

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a>Modul runtime .NET core oproti .NET Framework

Aplikace ASP.NET Core můžete zaměřují na modul runtime .NET Core nebo .NET Framework.

Další informace najdete v tématu [volba mezi .NET Core a .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Zvolte mezi ASP.NET Core a ASP.NET

Další informace o volbě mezi ASP.NET Core a ASP.NET najdete v tématu <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.
