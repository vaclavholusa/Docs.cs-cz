---
title: Základy ASP.NET Core
author: rick-anderson
description: Seznamte se základními koncepty pro vytváření aplikací ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 07/02/2018
uid: fundamentals/index
ms.openlocfilehash: 30c456685ce26522faff9b58fbd2977ad2f2869a
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202624"
---
# <a name="aspnet-core-fundamentals"></a>Základy ASP.NET Core

Aplikace ASP.NET Core je konzolová aplikace, která vytvoří webovým serverem v jeho `Main` metody:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

`Main` Metoda vyvolá `WebHost.CreateDefaultBuilder`, která používá vzor Tvůrce vytvořit hostitel webové aplikace. Tvůrce obsahuje metody, které definují webového serveru (například `UseKestrel`) a třída při spuštění (`UseStartup`). V předchozím příkladu [Kestrel](xref:fundamentals/servers/kestrel) automaticky přiděluje webový server. Webového hostitele ASP.NET Core se pokusí o spuštění ve službě IIS, pokud je k dispozici. Další webové servery, jako například [HTTP.sys](xref:fundamentals/servers/httpsys), je možné vyvoláním metody odpovídající rozšíření. `UseStartup` je vysvětleno v další části.

`IWebHostBuilder`, návratový typ `WebHost.CreateDefaultBuilder` vyvolání, obsahuje mnoho metod volitelné. Tyto metody patří k `UseHttpSys` pro hostování aplikace v souboru HTTP.sys a `UseContentRoot` pro zadání kořenový adresář s obsahem. `Build` a `Run` metody sestavení `IWebHost` objekt, který je hostitelem aplikace a začne naslouchat požadavkům HTTP.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

`Main` Používá metoda `WebHostBuilder`, která používá vzor Tvůrce vytvořit hostitel webové aplikace. Tvůrce obsahuje metody, které definují webového serveru (například `UseKestrel`) a třída při spuštění (`UseStartup`). V předchozím příkladu [Kestrel](xref:fundamentals/servers/kestrel) se používá webový server. Další webové servery, jako například [WebListener](xref:fundamentals/servers/weblistener), je možné vyvoláním metody odpovídající rozšíření. `UseStartup` je vysvětleno v další části.

`WebHostBuilder` poskytuje mnoho volitelné metod, včetně `UseIISIntegration` k hostování ve službě IIS a služby IIS Express a `UseContentRoot` pro zadání kořenový adresář s obsahem. `Build` a `Run` metody sestavení `IWebHost` objekt, který je hostitelem aplikace a začne naslouchat požadavkům HTTP.

---

## <a name="startup"></a>Po spuštění

`UseStartup` Metoda `WebHostBuilder` Určuje `Startup` třídy pro vaši aplikaci:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

`Startup` Třída je tady můžete definovat kanál zpracování požadavků a které jsou nakonfigurované všechny služby vyžaduje aplikaci. `Startup` Třída musí být veřejné a musí obsahovat následující metody:

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

`ConfigureServices` definuje [služby](#dependency-injection-services) používané v aplikaci (například ASP.NET, MVC Core a Entity Framework Core Identity). `Configure` definuje [middleware](xref:fundamentals/middleware/index) pro kanál žádosti.

Další informace najdete v tématu [spuštění aplikace](xref:fundamentals/startup).

## <a name="content-root"></a>Kořenové obsahu

Obsahu kořenový adresář je základní cesta k obsahu používat aplikace, jako je například zobrazení, [Razor Pages](xref:razor-pages/index)a statické prostředky. Ve výchozím nastavení kořenu obsahu je stejný jako základní cesta k aplikaci pro spustitelný soubor, který je hostitelem aplikace.

## <a name="web-root"></a>Kořenový adresář webové

Kořenový adresář webové aplikace je adresář, do projektu obsahující veřejné, statické prostředky, jako jsou šablony stylů CSS, JavaScript a soubory obrázků.

## <a name="dependency-injection-services"></a>Injektáž závislostí (služby)

Služba je komponenta, která je určená pro běžné používání v aplikaci. Služby jsou k dispozici prostřednictvím [injektáž závislostí (DI)](xref:fundamentals/dependency-injection). ASP.NET Core zahrnuje nativní **můžu**nPro **o**f **C**kontejner pro ojů (Inversion), který podporuje [konstruktor vkládání](xref:mvc/controllers/dependency-injection#constructor-injection) ve výchozím nastavení. Pokud chcete, můžete nahradit výchozí kontejner nativní. Kromě jeho výhody volné párování DI umožňuje službám k dispozici v rámci vaší aplikace (například [protokolování](xref:fundamentals/logging/index)).

Další informace najdete v tématu [injektáž závislostí](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Middleware

V ASP.NET Core, vytvoříte pomocí kanálu požadavku [middleware](xref:fundamentals/middleware/index). ASP.NET Core middleware provádí asynchronní logiku `HttpContext` a potom vyvolá další middleware v řadě nebo ukončí žádost přímo. Přidá middleware komponenty s názvem "XYZ" se při volání `UseXYZ` metody rozšíření v `Configure` – metoda.

ASP.NET Core obsahuje bohatou sadu integrovaných middleware:

* [Statické soubory](xref:fundamentals/static-files)
* [Směrování](xref:fundamentals/routing)
* [Ověřování](xref:security/authentication/index)
* [Middleware pro kompresi odpovědí](xref:performance/response-compression)
* [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting)

[OWIN](http://owin.org)– na základě middlewaru je k dispozici pro aplikace ASP.NET Core a můžete napsat vlastního middlewaru.

Další informace najdete v tématu [Middleware](xref:fundamentals/middleware/index) a [Open Web Interface pro .NET (OWIN)](xref:fundamentals/owin).

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Iniciování požadavků HTTP

Informace o používání `IHttpClientFactory` přístup `HttpClient` instance požadavků protokolu HTTP, naleznete v tématu [požadavky HTTP zahájit](xref:fundamentals/http-requests).

::: moniker-end

## <a name="environments"></a>Prostředí

Prostředí, jako je "Vývoj" a "Produkční" jsou hodnoty první třídy v ASP.NET Core a lze nastavit pomocí proměnné prostředí.

Další informace najdete v tématu [používání více prostředí](xref:fundamentals/environments).

## <a name="configuration"></a>Konfigurace

ASP.NET Core využívá konfigurační model založený na páry název hodnota. Konfigurační model není založen na `System.Configuration` nebo *web.config*. Konfigurace nastavení získá ze seřazené sady poskytovatelů konfigurace. Poskytovatelé konfigurace integrovanou podporu širokou škálu formátů souborů (XML, JSON, INI) a proměnných prostředí umožňující konfigurace podle prostředí. Můžete taky psát vlastní zprostředkovatele vlastní konfigurace.

Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).

## <a name="logging"></a>protokolování

ASP.NET Core podporuje protokolování rozhraní API, která funguje s různých poskytovatelů protokolování. Předdefinované zprostředkovatele podpory odesílání protokolů do jednoho nebo více cílů. Můžete použít rozhraní protokolování třetích stran.

Další informace najdete v tématu [protokolování](xref:fundamentals/logging/index)

## <a name="error-handling"></a>Zpracování chyb

ASP.NET Core má integrované funkce pro zpracování chyb v aplikacích, včetně stránku výjimky pro vývojáře, vlastní chybové stránky, statické stav znakové stránky a zpracování výjimek při spuštění.

Další informace najdete v tématu [zpracování chyb](xref:fundamentals/error-handling).

## <a name="routing"></a>Směrování

ASP.NET Core nabízí funkce pro směrování žádostí na aplikace obslužné rutině trasy.

Další informace najdete v tématu [směrování](xref:fundamentals/routing).

## <a name="file-providers"></a>Zprostředkovatelé souborů

ASP.NET Core abstrahuje přístupu k systému souborů prostřednictvím zprostředkovatelé souborů, která nabízí obecné rozhraní pro práci se soubory napříč platformami.

Další informace najdete v tématu [zprostředkovatelé souborů](xref:fundamentals/file-providers).

## <a name="static-files"></a>Statické soubory

Statické soubory middleware slouží statické soubory, jako jsou HTML, CSS, image a JavaScript.

Další informace najdete v tématu [statické soubory](xref:fundamentals/static-files).

## <a name="hosting"></a>Hostování

Konfigurace aplikace ASP.NET Core a spouštění *hostitele*, který je zodpovědný za spouštění a životního cyklu správy aplikací.

Další informace najdete v tématu [hostitele v ASP.NET Core](xref:fundamentals/host/index).

## <a name="session-and-app-state"></a>Stav relace a aplikace

ASP.NET Core nabízí několik způsobů zachovat stav relace a aplikace, zatímco uživatel prochází webové aplikace.

Další informace najdete v tématu [stav relace a aplikace](xref:fundamentals/app-state).

## <a name="servers"></a>Servery

ASP.NET Core, model hostingu není přímo naslouchat žádostem. Model hostingu závisí na implementaci serveru HTTP k předání požadavku do aplikace. Předaný požadavek je zabalena jako sada objekty funkce, které budou přístupné prostřednictvím rozhraní. ASP.NET Core zahrnuje spravovaná, napříč platformami webový server, volá [Kestrel](xref:fundamentals/servers/kestrel). Kestrel je často spuštění za produkční webový server, například [IIS](https://www.iis.net/) nebo [Nginx](http://nginx.org). Kestrel může běžet pod hraniční server.

Další informace najdete v tématu [servery](xref:fundamentals/servers/index) a v následujících tématech:

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Ovladač HTTP.sys](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>Globalizace a lokalizace

Vytvoření vícejazyčné web pomocí ASP.NET Core umožňuje oslovit větší cílovou skupinu webu. ASP.NET Core poskytuje služby a middleware pro lokalizaci do různých jazyků a kultur.

Další informace najdete v tématu [globalizace a lokalizace](xref:fundamentals/localization).

## <a name="request-features"></a>Funkce požadavků

Podrobnosti o implementaci webového serveru související s požadavky HTTP a odpovědí jsou definovány v rozhraní. Tato rozhraní jsou používány implementací serveru a middleware k vytvoření a úprava kanálu hostitelské aplikace.

Další informace najdete v tématu [požádat o funkce](xref:fundamentals/request-features).

## <a name="background-tasks"></a>Úlohy na pozadí

Úlohy na pozadí jsou implementovány jako *hostovaných služeb*. Hostovanou službu je třída s atributem logiky úloh na pozadí, který implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní.

Další informace najdete v tématu [s hostovanými službami úloh na pozadí](xref:fundamentals/host/hosted-services).

## <a name="access-httpcontext"></a>Přístup k objektu HttpContext

Přístup `HttpContext` prostřednictvím [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) rozhraní a jeho výchozí implementace [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).

Další informace naleznete v tématu <xref:fundamentals/httpcontext>.

## <a name="open-web-interface-for-net-owin"></a>Open Web Interface pro .NET (OWIN)

ASP.NET Core podporuje Open Web Interface pro .NET (OWIN). OWIN umožňuje webové aplikace k oddělení od webových serverů.

Další informace najdete v tématu [Open Web Interface pro .NET (OWIN)](xref:fundamentals/owin).

## <a name="websockets"></a>Protokoly Websocket

[Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP. Používá se pro aplikace, například konverzace, akciích, hry a kdekoli vyžadujete funkcí v reálném čase ve webové aplikaci. ASP.NET Core podporuje funkce webových soketů.

Další informace najdete v tématu [objekty Websocket](xref:fundamentals/websockets).

::: moniker range=">= aspnetcore-2.1"
## <a name="microsoftaspnetcoreapp-metapackage"></a>Microsoft.AspNetCore.App Microsoft.aspnetcore.all

[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) Microsoft.aspnetcore.all zjednodušuje správu balíčků. Další informace najdete v tématu [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).

::: moniker-end
::: moniker range="= aspnetcore-2.0"
## <a name="microsoftaspnetcoreall-metapackage"></a>Metabalíček Microsoft.aspnetcore.all

[Metabalíček](https://www.nuget.org/packages/Microsoft.AspNetCore.All) Microsoft.aspnetcore.all pro ASP.NET Core zahrnuje:

* Všechny podporované balíčky týmem ASP.NET Core.
* Všechny podporované balíčky pomocí Entity Framework Core.
* Interní a 3. stran závislosti používat ASP.NET Core a Entity Framework Core.

Další informace najdete v tématu [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage).
::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a>Modul runtime .NET core oproti .NET Framework

Aplikace ASP.NET Core můžete zaměřují na modul runtime .NET Core nebo .NET Framework.

Další informace najdete v tématu [volba mezi .NET Core a .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Zvolte mezi ASP.NET Core a ASP.NET

Další informace o volbě mezi ASP.NET Core a ASP.NET najdete v tématu [zvolte mezi ASP.NET Core a ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).
