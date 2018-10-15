---
title: Základy ASP.NET Core
author: rick-anderson
description: Seznamte se základními koncepty pro vytváření aplikací ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 83dfb5707700da01c45bae3c0c00e67ca397d402
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325468"
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

## <a name="content-root"></a>Kořen obsahu

Kořen obsahu (Content Root) je bázová cesta k libovolnému obsahu využívaného aplikací, jako jsou například pohledy, [Stránky Razor](xref:razor-pages/index) a statický obsah (obrázky apod.). Ve výchozím nastavení je kořenu obsahu stejný jako bázová cesta aplikace ke spustitelnému souboru, který je hostitelem aplikace.

## <a name="web-root"></a>Kořen webu

Kořen webu (Web Root) je adresář projektu, který obsahuje veřejné, statické prostředky, jako jsou např. kaskádové styly, JavaScript a obrázky.

## <a name="dependency-injection-services"></a>Vkládání závislostí (služby)

*Služba* je komponenta, která je určená pro společné používání v rámci aplikace. Služby jsou zpřístupněny prostřednictvím [vkládání závislosti](xref:fundamentals/dependency-injection). ASP.NET Core zahrnuje nativní kontejner pro inverzi závislostí, který podporuje [vkládání závislostí pomocí konstruktoru](xref:mvc/controllers/dependency-injection#constructor-injection). Nativní kontejner je možné nahradit jiným. Výhoda použití vkládání závislostí spočívá ve [volnějším propojení jednotlivých komponent](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) a zpřístupnění služeb (jako je třeba [protokolování](xref:fundamentals/logging/index)) všude v aplikaci.

Další informace naleznete v tématu <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Middleware

V ASP.NET Core budujete kanál zpracování požadavků pomocí [middlewarů](xref:fundamentals/middleware/index). ASP.NET Core middleware nejprve provádí asynchronní operace nad kontextem `HttpContext`, a následně může předat řízení dalšímu middlewaru v pořadí nebo okamžitě ukončit zpracování HTTP požadavku.

Middlewarová komponenta s názvem "XYZ" se přidá pomocí volání rozšiřující metody `UseXYZ` uvnitř metody `Configure`.

ASP.NET Core obsahuje pestrý výběr vestavěných middlewarů, nebo můžete použít vlastní middleware. [Otevřete Web Interface pro .NET (OWIN)](xref:fundamentals/owin), který umožňuje webové aplikace k oddělení od webové servery, je podporováno v aplikacích pro ASP.NET Core.

Další informace naleznete v tématu <xref:fundamentals/middleware/index> a <xref:fundamentals/owin>.

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Iniciování HTTP požadavků

<xref:System.Net.Http.IHttpClientFactory> je k dispozici pro přístup k <xref:System.Net.Http.HttpClient> instance požadavky HTTP.

Další informace naleznete v tématu <xref:fundamentals/http-requests>.

::: moniker-end

## <a name="environments"></a>Prostředí

ASP.NET Core umožňuje rozlišení vývojového *Development* a produkčního *Production* prostředí, konkrétní prostředí lze nastavit pomocí proměnných prostředí.

Další informace naleznete v tématu <xref:fundamentals/environments>.

## <a name="hosting"></a>Hostování

Aplikace ASP.NET Core konfiguruje a spouští *hostitele*, který je zodpovědný za správu životního cyklu aplikace.

Další informace naleznete v tématu <xref:fundamentals/host/index>.

## <a name="servers"></a>Servery

ASP.NET Core nenaslouchá přímo HTTP požadavkům. Spoléhá na HTTP server, který aplikaci předává jednotlivé požadavky. Předaný požadavek je zabalen do objektů, ke kterým je možné přistupovat skrz rozhraní. ASP.NET Core obsahuje zabudovaný multiplatformní webový server [Kestrel](xref:fundamentals/servers/kestrel). Kestrel může běžet v pozadí produkčního webového serveru, jako je například [IIS](https://www.iis.net/) nebo [Nginx](http://nginx.org). Kestrel můžete také spustit jako veřejnou hraniční server přístup přímo k Internetu v ASP.NET Core 2.0 nebo novější.

Další informace naleznete v tématu <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Konfigurace

ASP.NET Core využívá konfigurační model založený na dvojicích název-hodnota. Konfigurační model již není založen na <xref:System.Configuration> nebo *web.config*. Hodnoty konfigurace jsou získávány z uspořádané množiny zprostředkovatelů konfigurace. Vestavění zprostředkovatelé konfigurace podporují množství standardních formátů (XML, JSON, INI) nebo konfiguraci pomocí proměnných prostředí. Můžete vytvořit i vlastního zprostředkovatele konfigurace.

Další informace naleznete v tématu <xref:fundamentals/configuration/index>.

## <a name="logging"></a>Protokolování

ASP.NET Core poskytuje obecné API pro protokolování, které spolupracuje s různými zprostředkovateli protokolování. Předdefinovaní zprostředkovatelé protokolování podporují odesílání záznamů do jednoho i více míst zároveň. Použít je možné i zprostředkovatele třetích stran.

Další informace naleznete v tématu <xref:fundamentals/logging/index>.

## <a name="error-handling"></a>Zpracování chyb

ASP.NET Core má integrované funkce pro zpracování chyb v aplikacích, vč. stránky pro diagnostiku výjimek, vlastní chybové stránky a zpracování výjimek při spuštění.

Další informace naleznete v tématu <xref:fundamentals/error-handling>.

## <a name="routing"></a>Směrování

ASP.NET Core nabízí funkce pro směrování požadavků do obslužné rutiny směrovače.

Další informace naleznete v tématu <xref:fundamentals/routing>.

## <a name="file-providers"></a>Zprostředkovatelé souborů

ASP.NET Core abstrahuje přístup k systému souborů prostřednictvím zprostředkovatelů souborů, nabízí společné rozhraní pro práci se soubory napříč platformami.

Další informace naleznete v tématu <xref:fundamentals/file-providers>.

## <a name="static-files"></a>Statické soubory

Middleware pro statické soubory poskytuje přístup ke statickým souborům, jako například HTML, kaskádovým stylům, obrázkům a JavaScriptu.

Další informace naleznete v tématu <xref:fundamentals/static-files>.

## <a name="session-and-app-state"></a>Stav aplikace a relace

ASP.NET Core nabízí několik způsobů pro uchování stavu aplikace a relace v čase procházení webové aplikace uživatelem.

Další informace naleznete v tématu <xref:fundamentals/app-state>.

## <a name="globalization-and-localization"></a>Globalizace a lokalizace

Vytvoření vícejazyčného webu pomocí ASP.NET Core umožňuje oslovit větší počet návštěvníků. ASP.NET Core poskytuje služby a middleware pro usnadnění lokalizace do různých jazyků a kultur.

Další informace naleznete v tématu <xref:fundamentals/localization>.

## <a name="request-features"></a>Funkce požadavků

Implementační detaily webového serveru související s HTTP požadavky a odpověďmi jsou definovány v rozhraních. Tato rozhraní jsou používána implementacemi serveru a middlewarem k vytvoření a modifikaci kanálu hostingu.

Další informace naleznete v tématu <xref:fundamentals/request-features>.

## <a name="background-tasks"></a>Úlohy na pozadí

Úlohy na pozadí jsou implementovány jako *hostované služby*. Hostovaná služba je třída s logikou úlohy na pozadí a implementuje rozhraní <xref:Microsoft.Extensions.Hosting.IHostedService>.

Další informace naleznete v tématu <xref:fundamentals/host/hosted-services>.

## <a name="access-httpcontext"></a>Přístup k objektu HttpContext

`HttpContext` Při zpracování žádosti se stránkami Razor a technologií MVC je automaticky k dispozici. V případech kdy `HttpContext` není snadno k dispozici, můžete přistupovat `HttpContext` prostřednictvím <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> rozhraní a jeho výchozí implementace <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.

Další informace naleznete v tématu <xref:fundamentals/httpcontext>.

## <a name="websockets"></a>WebSocket

[WebSocket](https://wikipedia.org/wiki/WebSocket) je protokol, který umožňuje vytvoření trvalých obousměrných komunikačních kanálů přes TCP připojení. Používá se např. pro komunikační aplikace, hry a kdekoli je vyžadována funkcionalita prováděná v reálném čase. ASP.NET Core podporuje funkce webových soketů.

Další informace naleznete v tématu <xref:fundamentals/websockets>.

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a>Metabalíček Microsoft.AspNetCore.App

Metabalíček [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) zjednodušuje správu balíčků.

Další informace naleznete v tématu <xref:fundamentals/metapackage-app>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a>Metabalíček Microsoft.AspNetCore.All

[Metabalíček](https://www.nuget.org/packages/Microsoft.AspNetCore.All) Microsoft.AspNetCore.All pro ASP.NET Core zahrnuje:

* Všechny podporované balíčky vytvořené týmem ASP.NET Core.
* Všechny podporované balíčky Entity Framework Core.
* Interní závislosti a závislosti třetích stran používané v rámci ASP.NET Core a Entity Framework Core.

Další informace naleznete v tématu <xref:fundamentals/metapackage>.

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a>.NET Core proti .NET Framework

Aplikace ASP.NET Core lze při překladu cílit jak na běhové prostředí .NET Core, tak na běhové prostředí .NET Framework.

Další informace naleznete v tématu [Volba mezi .NET Core a .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Volba mezi ASP.NET Core a ASP.NET

Další informace o volbě mezi ASP.NET Core a ASP.NET najdete v tématu <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.
