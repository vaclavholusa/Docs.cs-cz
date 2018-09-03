---
title: Základy ASP.NET Core
author: rick-anderson
description: Seznamte se základními koncepty pro vytváření aplikací ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 07/02/2018
uid: fundamentals/index
ms.openlocfilehash: 8e0198e2975192e6522c4821741aacc7a844000b
ms.sourcegitcommit: 571d76fbbff05e84406b6d909c8fe9cbea2c8ff1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/01/2018
ms.locfileid: "39410088"
---
# <a name="aspnet-core-fundamentals"></a>Základy ASP.NET Core

Aplikace ASP.NET Core je konzolová aplikace, která vytváří webový server ve své `Main` metodě:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

V metodě `Main` se volá metoda `WebHost.CreateDefaultBuilder`, která poskytuje implementaci návrhového vzoru Builder a umožňuje tak sestavení hostitele webové aplikace. Samotný Builder obsahuje metody, které specifikují webový server (například `UseKestrel`), který se má použít, a třídu pro spuštění (`UseStartup`). Ve výše uvedeném příkladu je automaticky použit webový server [Kestrel](xref:fundamentals/servers/kestrel). ASP.NET Core se zároveň pokusí o spuštění aplikace ve službě IIS, pokud je k dispozici. Voláním patřičné rozšiřující metody je možné aplikaci hostovat i na jiných webových serverech, jako například [HTTP.sys](xref:fundamentals/servers/httpsys). `UseStartup` je vysvětleno v další části.

Typ `IWebHostBuilder`, který je vrácen jako výsledek volání `WebHost.CreateDefaultBuilder`, obsahuje mnoho volitelných metod. Jednou z těchto metod je `UseHttpSys` pro hostování aplikace pomocí HTTP.sys a `UseContentRoot` pro určení kořenového adresáře s obsahem. Metody `Build` a `Run` slouží k sestavení objektu `IWebHost`, který je hostitelem aplikace, resp. zahájí naslouchání HTTP požadavků.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

V metodě `Main` se používá třída `WebHostBuilder`, která implementuje návrhový vzor Builder a umožňuje tak sestavení hostitele webové aplikace. Samotný Builder obsahuje metody, které specifikují webový server (například `UseKestrel`), který se má použít, a třídu pro spuštění (`UseStartup`). Ve výše uvedeném příkladu je použit webový server [Kestrel](xref:fundamentals/servers/kestrel). Voláním patřičné rozšiřující metody je možné aplikaci hostovat i na jiných webových serverech, jako například [WebListener](xref:fundamentals/servers/weblistener). `UseStartup` je vysvětleno v další části.

Třída `WebHostBuilder` obsahuje mnoho volitelných metod. Jednou z těchto metod je `UseIISIntegration` pro hostování aplikace pomocí služby IIS nebo IIS Express a `UseContentRoot` pro určení kořenového adresáře s obsahem. Metody `Build` a `Run` slouží k sestavení objektu `IWebHost`, který je hostitelem aplikace, resp. zahájí naslouchání HTTP požadavků.

---

## <a name="startup"></a>Třída pro spuštění

Metoda `UseStartup` třídy `WebHostBuilder` určuje spouštěcí třídu `Startup` Vaší aplikace:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

Ve třídě `Startup` můžete definovat kanál zpracování požadavků a nakonfigurovat všechny služby, které aplikace vyžaduje. Třída `Startup` musí být veřejná a musí obsahovat následující metody:

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

`ConfigureServices` definuje [služby](#dependency-injection-services) používané v aplikaci (například ASP.NET, Core MVC, Entity Framework Core, Identity). `Configure` definuje [middlewary](xref:fundamentals/middleware/index) pro kanál zpracování požadavků.

Další informace naleznete v tématu [Spuštění aplikace](xref:fundamentals/startup).

## <a name="content-root"></a>Kořen obsahu

Kořen obsahu (Content Root) je bázová cesta k libovolnému obsahu využívaného aplikací, jako jsou například pohledy, [Stránky Razor](xref:razor-pages/index) a statický obsah (obrázky apod.). Ve výchozím nastavení je kořenu obsahu stejný jako bázová cesta aplikace ke spustitelnému souboru, který je hostitelem aplikace.

## <a name="web-root"></a>Kořen webu

Kořen webu (Web Root) je adresář projektu, který obsahuje veřejné, statické prostředky, jako jsou např. kaskádové styly, JavaScript a obrázky.

## <a name="dependency-injection-services"></a>Vkládání závislostí (služby)

Služba je komponenta, která je určená pro společné používání v rámci aplikace. Služby jsou zpřístupněny prostřednictvím [vkládání závislosti](xref:fundamentals/dependency-injection). ASP.NET Core zahrnuje nativní kontejner pro inverzi závislostí (**I**nversion **o**f **C**ontrol, IOC), který podporuje [vkládání závislostí pomocí konstruktoru](xref:mvc/controllers/dependency-injection#constructor-injection). Nativní kontejner je možné nahradit jiným. Výhoda použití vkládání závislostí spočívá ve volnějším propojení jednotlivých komponent a zpřístupnění služeb (jako je třeba [protokolování](xref:fundamentals/logging/index)) všude v aplikaci.

Další informace najdete v tématu [Vkládání závislostí](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Middleware

V ASP.NET Core budujete kanál zpracování požadavků pomocí [middlewarů](xref:fundamentals/middleware/index). ASP.NET Core middleware nejprve provádí asynchronní operace nad kontextem `HttpContext`, a následně může předat řízení dalšímu middlewaru v pořadí nebo okamžitě ukončit zpracování HTTP požadavku. Middlewarová komponenta s názvem "XYZ" se přidá pomocí volání rozšiřující metody `UseXYZ` uvnitř metody `Configure`.

ASP.NET Core obsahuje pestrý výběr vestavěných middlewarů:

* [Statické soubory](xref:fundamentals/static-files)
* [Směrování](xref:fundamentals/routing)
* [Ověřování](xref:security/authentication/index)
* [Middleware pro kompresi odpovědí](xref:performance/response-compression)
* [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting)

Možné je také použít middleware založený na [OWIN](http://owin.org) nebo vlastní middleware.

Další informace najdete v tématu [Middleware](xref:fundamentals/middleware/index) a [Open Web Interface pro .NET (OWIN)](xref:fundamentals/owin).

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Iniciování HTTP požadavků

Informace o používání `IHttpClientFactory` pro přístup k instancím třídy `HttpClient` za účelem zasílání HTTP požadavků naleznete v tématu [Iniciování HTTP požadavků](xref:fundamentals/http-requests).

::: moniker-end

## <a name="environments"></a>Prostředí

ASP.NET Core umožňuje rozlišení vývojového ("Development") a produkčního ("Production") prostředí, konkrétní prostředí lze nastavit pomocí proměnných prostředí.

Další informace najdete v tématu [používání více prostředí](xref:fundamentals/environments).

## <a name="configuration"></a>Konfigurace

ASP.NET Core využívá konfigurační model založený na dvojicích název-hodnota. Konfigurační model již není založen na `System.Configuration` nebo *web.config*. Hodnoty konfigurace jsou získávány z uspořádané množiny zprostředkovatelů konfigurace. Vestavění zprostředkovatelé konfigurace podporují množství standardních formátů (XML, JSON, INI) nebo konfiguraci pomocí proměnných prostředí. Můžete vytvořit i vlastního zprostředkovatele konfigurace.

Další informace najdete v tématu [Konfigurace](xref:fundamentals/configuration/index).

## <a name="logging"></a>Protokolování

ASP.NET Core poskytuje obecné API pro protokolování, které spolupracuje s různými zprostředkovateli protokolování. Předdefinovaní zprostředkovatelé protokolování podporují odesílání záznamů do jednoho i více míst zároveň. Použít je možné i zprostředkovatele třetích stran.

Další informace najdete v tématu [Protokolování](xref:fundamentals/logging/index)

## <a name="error-handling"></a>Zpracování chyb

ASP.NET Core má integrované funkce pro zpracování chyb v aplikacích, vč. stránky pro diagnostiku výjimek, vlastní chybové stránky a zpracování výjimek při spuštění.

Další informace najdete v tématu [Zpracování chyb](xref:fundamentals/error-handling).

## <a name="routing"></a>Směrování

ASP.NET Core nabízí funkce pro směrování požadavků do  obslužné rutiny směrovače.

Další informace najdete v tématu [Směrování](xref:fundamentals/routing).

## <a name="file-providers"></a>Zprostředkovatelé souborů

ASP.NET Core abstrahuje přístup k systému souborů prostřednictvím zprostředkovatelů souborů, nabízí společné rozhraní pro práci se soubory napříč platformami.

Další informace najdete v tématu [Zprostředkovatelé souborů](xref:fundamentals/file-providers).

## <a name="static-files"></a>Statické soubory

Middleware pro statické soubory poskytuje přístup ke statickým souborům, jako například HTML, kaskádovým stylům, obrázkům a JavaScriptu.

Další informace najdete v tématu [Statické soubory](xref:fundamentals/static-files).

## <a name="hosting"></a>Hostování

Aplikace ASP.NET Core konfiguruje a spouští *hostitele*, který je zodpovědný za správu životního cyklu aplikace.

Další informace najdete v tématu [Hostování v ASP.NET Core](xref:fundamentals/host/index).

## <a name="session-and-app-state"></a>Stav aplikace a relace

ASP.NET Core nabízí několik způsobů pro uchování stavu aplikace a relace v čase procházení webové aplikace uživatelem.

Další informace najdete v tématu [Stav aplikace a relace](xref:fundamentals/app-state).

## <a name="servers"></a>Servery

ASP.NET Core nenaslouchá přímo HTTP požadavkům. Spoléhá na HTTP server, který aplikaci předává jednotlivé požadavky. Předaný požadavek je zabalen do objektů, ke kterým je možné přistupovat skrz rozhraní. ASP.NET Core obsahuje zabudovaný multiplatformní webový server [Kestrel](xref:fundamentals/servers/kestrel). Kestrel může běžet v pozadí produkčního webového serveru, jako je například [IIS](https://www.iis.net/) nebo [Nginx](http://nginx.org). Kestrel může běžet i na pozadí hraničního serveru.

Další informace najdete v tématu [Servery](xref:fundamentals/servers/index) a v následujících tématech:

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Ovladač HTTP.sys](xref:fundamentals/servers/httpsys) (dříve označovaný jako [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>Globalizace a lokalizace

Vytvoření vícejazyčného webu pomocí ASP.NET Core umožňuje oslovit větší počet návštěvníků. ASP.NET Core poskytuje služby a middleware pro usnadnění lokalizace do různých jazyků a kultur.

Další informace najdete v tématu [Globalizace a lokalizace](xref:fundamentals/localization).

## <a name="request-features"></a>Funkce požadavků

Implementační detaily webového serveru související s HTTP požadavky a odpověďmi jsou definovány v rozhraních. Tato rozhraní jsou používána implementacemi serveru a middlewarem k vytvoření a modifikaci kanálu hostingu.

Další informace najdete v tématu [Funkce požadavků](xref:fundamentals/request-features).

## <a name="background-tasks"></a>Úlohy na pozadí

Úlohy na pozadí jsou implementovány jako *hostované služby*. Hostovaná služba je třída s logikou úlohy na pozadí a implementuje rozhraní [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).

Další informace najdete v tématu [Úlohy na pozadí s hostovanými službami](xref:fundamentals/host/hosted-services).

## <a name="access-httpcontext"></a>Přístup k objektu HttpContext

Přistupujte k objektu `HttpContext` prostřednictvím rozhraní [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) a jeho výchozí implementace [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).

Další informace naleznete v tématu <xref:fundamentals/httpcontext>.

## <a name="open-web-interface-for-net-owin"></a>Open Web Interface pro .NET (OWIN)

ASP.NET Core podporuje Open Web Interface pro .NET (OWIN). OWIN umožňuje odstínění webových aplikací od implementačních detailů webových serverů.

Další informace najdete v tématu [Open Web Interface pro .NET (OWIN)](xref:fundamentals/owin).

## <a name="websockets"></a>WebSocket

[WebSocket](https://wikipedia.org/wiki/WebSocket) je protokol, který umožňuje vytvoření trvalých obousměrných komunikačních kanálů přes TCP připojení. Používá se např. pro komunikační aplikace, hry a kdekoli je vyžadována funkcionalita prováděná v reálném čase. ASP.NET Core podporuje funkce webových soketů.

Další informace najdete v tématu [WebSocket](xref:fundamentals/websockets).

::: moniker range=">= aspnetcore-2.1"
## <a name="microsoftaspnetcoreapp-metapackage"></a>Metabalíček Microsoft.AspNetCore.App

Metabalíček [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) zjednodušuje správu balíčků. Další informace najdete v tématu [Metabalíček Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end
::: moniker range="= aspnetcore-2.0"
## <a name="microsoftaspnetcoreall-metapackage"></a>Metabalíček Microsoft.AspNetCore.All

[Metabalíček](https://www.nuget.org/packages/Microsoft.AspNetCore.All) Microsoft.AspNetCore.All pro ASP.NET Core zahrnuje:

* Všechny podporované balíčky vytvořené týmem ASP.NET Core.
* Všechny podporované balíčky Entity Framework Core.
* Interní závislosti a závislosti třetích stran používané v rámci ASP.NET Core a Entity Framework Core.

Další informace najdete v tématu [Metabalíček Microsoft.AspNetCore.All](xref:fundamentals/metapackage).
::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a>.NET Core proti .NET Framework

Aplikace ASP.NET Core lze při překladu cílit jak na běhové prostředí .NET Core, tak na běhové prostředí .NET Framework.

Další informace najdete v tématu [Volba mezi .NET Core a .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Volba mezi ASP.NET Core a ASP.NET

Další informace o volbě mezi ASP.NET Core a ASP.NET najdete v tématu [Volba mezi ASP.NET Core a ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).
