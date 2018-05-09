---
title: Základy ASP.NET Core
author: rick-anderson
description: Zjistit základní koncepty pro vytváření aplikací ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: 223b1906ef9941084e18e0698f007d9564e81f09
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="aspnet-core-fundamentals"></a>Základy ASP.NET Core

Aplikace ASP.NET Core je konzolovou aplikaci, která vytvoří webovým serverem v jeho `Main` metoda:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)
[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

`Main` Vyvolá metoda `WebHost.CreateDefaultBuilder`, který odpovídá vzorci Tvůrce vytvořit hostitel webové aplikace. Tvůrce obsahuje metody, které definují webového serveru (například `UseKestrel`) a třída při spuštění (`UseStartup`). V předchozím příkladu [Kestrel](xref:fundamentals/servers/kestrel) webový server je automaticky přidělen. ASP.NET Core webového hostitele se pokusí o spuštění ve službě IIS, pokud je k dispozici. Další webové servery, jako například [HTTP.sys](xref:fundamentals/servers/httpsys), mohou být využívána volání metody odpovídající rozšíření. `UseStartup` je vysvětleno v následující části Další.

`IWebHostBuilder`, návratový typ `WebHost.CreateDefaultBuilder` volání, nabízí mnoho způsobů volitelné. Některé z těchto metod zahrnují `UseHttpSys` pro hostování aplikace v HTTP.sys a `UseContentRoot` pro zadání kořenový adresář s obsahem. `Build` a `Run` metody sestavení `IWebHost` objekt, který je hostitelem aplikace a začne naslouchat požadavkům HTTP.

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

`Main` Používá metoda `WebHostBuilder`, který odpovídá vzorci Tvůrce vytvořit hostitel webové aplikace. Tvůrce obsahuje metody, které definují webového serveru (například `UseKestrel`) a třída při spuštění (`UseStartup`). V předchozím příkladu [Kestrel](xref:fundamentals/servers/kestrel) se používá webový server. Další webové servery, jako například [WebListener](xref:fundamentals/servers/weblistener), mohou být využívána volání metody odpovídající rozšíření. `UseStartup` je vysvětleno v následující části Další.

`WebHostBuilder` nabízí mnoho volitelné způsobů, včetně `UseIISIntegration` pro hostování v IIS a služby IIS Express a `UseContentRoot` pro zadání kořenový adresář s obsahem. `Build` a `Run` metody sestavení `IWebHost` objekt, který je hostitelem aplikace a začne naslouchat požadavkům HTTP.

* * *
## <a name="startup"></a>Spuštění

`UseStartup` Metodu `WebHostBuilder` Určuje `Startup` třídy pro aplikaci:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)
[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

* * *
`Startup` Je třída, kde můžete definovat v kanálu zpracování požadavků a které jsou nakonfigurované všechny služby potřebné pro aplikaci. `Startup` Třída musí být veřejné a musí obsahovat následující metody:

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

`ConfigureServices` definuje [služby](#dependency-injection-services) používané vaší aplikace (například Identity Entity Framework Core, základní rozhraní ASP.NET MVC). `Configure` definuje [middleware](xref:fundamentals/middleware/index) pro kanál požadavku.

Další informace najdete v tématu [spuštění aplikace](xref:fundamentals/startup).

## <a name="content-root"></a>Obsah kořenové

Kořenu obsahu je základní cesta k žádný obsah používané aplikace, jako je například zobrazení, [stránky Razor](xref:mvc/razor-pages/index)a statické prostředky. Ve výchozím nastavení kořenu obsahu je stejný jako základní cesta aplikace pro spustitelný soubor, který je hostitelem aplikace.

## <a name="web-root"></a>Kořenový web

Kořenové webové aplikace je adresář v projektu obsahující veřejný, statické prostředky, například šablon stylů CSS, JavaScript a soubory obrázků.

## <a name="dependency-injection-services"></a>Vkládání závislostí (služby)

Služba je komponenta, která je určená pro běžné používání v aplikaci. Služby jsou dostupné prostřednictvím [vkládání závislostí (DI)](xref:fundamentals/dependency-injection). ASP.NET Core zahrnuje nativní **I**nPro **o**f **C**kontejneru obrazit řídicí (IoC), který podporuje [konstruktor vkládání](xref:mvc/controllers/dependency-injection#constructor-injection) ve výchozím nastavení. Výchozí kontejner nativní můžete nahradit, pokud chcete. Kromě jeho výhody volné párování DI umožňuje služby k dispozici v celé vaší aplikace (například [protokolování](xref:fundamentals/logging/index)).

Další informace najdete v tématu [vkládání závislostí](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Middleware

V ASP.NET Core, vytvoříte pomocí kanálu požadavku [middleware](xref:fundamentals/middleware/index). ASP.NET Core middleware provede asynchronní logiku `HttpContext` a vyvolá další middleware v pořadí, nebo přímo ukončí žádosti. Přidá middleware komponenty s názvem "XYZ" se při vyvolání `UseXYZ` metoda rozšíření v `Configure` metoda.

ASP.NET Core obsahuje bohatou sadu předdefinovaných middleware:

* [Statické soubory](xref:fundamentals/static-files)
* [Směrování](xref:fundamentals/routing)
* [Ověřování](xref:security/authentication/index)
* [Middleware pro kompresi odpovědí](xref:performance/response-compression)
* [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting)

[OWIN](http://owin.org)– na základě middlewaru je k dispozici pro aplikace ASP.NET Core a můžete napsat vlastní vlastní middleware.

Další informace najdete v tématu [Middleware](xref:fundamentals/middleware/index) a [Open Web Interface pro .NET (OWIN)](xref:fundamentals/owin).

## <a name="initiate-http-requests"></a>Inicializace požadavků HTTP

Informace o používání `IHttpClientFactory` pro přístup k `HttpClient` instancí, aby požadavky HTTP, najdete v části [požadavky HTTP zahájit](xref:fundamentals/http-requests).

## <a name="environments"></a>Prostředí

Prostředí, jako je například "Vývoj" a "Provozní", jsou hodnoty první třídy v ASP.NET Core a se dá nastavit pomocí proměnné prostředí.

Další informace najdete v tématu [použijte prostředí s více](xref:fundamentals/environments).

## <a name="configuration"></a>Konfigurace

Jádro ASP.NET používá model konfigurace na základě párů název hodnota. Konfigurační model není založen na `System.Configuration` nebo *web.config*. Konfigurace obsahuje nastavení ze seřazené sadu poskytovatelů konfigurace. Poskytovatelé předdefinované konfigurace podporují celou řadu formáty souborů (XML, JSON, INI) a proměnné prostředí umožňující konfiguraci na základě prostředí. Také můžete napsat vlastního zprostředkovatele vlastní konfigurace.

Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).

## <a name="logging"></a>protokolování

Jádro ASP.NET podporuje protokolování rozhraní API, která funguje s různými zprostředkovatelů protokolování. Předdefinované zprostředkovatele podporovat odesílání protokoly na jeden nebo více míst. Můžete použít rozhraní protokolování třetích stran.

[Protokolování](xref:fundamentals/logging/index)

## <a name="error-handling"></a>Zpracování chyb

ASP.NET Core obsahuje integrované funkce pro zpracování chyb v aplikacích, včetně stránky výjimka developer, vlastní chybové stránky, statické stav znakové stránky a spuštění zpracování výjimek.

Další informace najdete v tématu [způsob zpracování chyb](xref:fundamentals/error-handling).

## <a name="routing"></a>Směrování

ASP.NET Core nabízí funkce pro směrování požadavků app na obslužné rutiny trasy.

Další informace najdete v tématu [směrování](xref:fundamentals/routing).

## <a name="file-providers"></a>Zprostředkovatelé souboru

ASP.NET Core abstrahuje přístupu k systému souborů prostřednictvím poskytovatelů souboru, který nabízí společné rozhraní pro práci se soubory napříč platformami.

Další informace najdete v tématu [souboru zprostředkovatelé](xref:fundamentals/file-providers).

## <a name="static-files"></a>Statické soubory

Statické soubory middleware poskytuje statické soubory, jako je například HTML, CSS, image a JavaScript.

Další informace najdete v tématu [statické soubory](xref:fundamentals/static-files).

## <a name="hosting"></a>Hostování

Aplikace ASP.NET Core nakonfigurovat a spustit *hostitele*, která je zodpovědná za spuštění a životního cyklu správy aplikací.

Další informace najdete v tématu [hostitelský](xref:fundamentals/hosting).

## <a name="session-and-application-state"></a>Stav relace a aplikace

Stav relace je funkce v ASP.NET Core, který můžete použít k ukládání a uchovávání dat uživatele, když uživatel prohlíží vaší webové aplikace.

Další informace najdete v tématu [relace a stav aplikace](xref:fundamentals/app-state).

## <a name="servers"></a>Servery

Model hostování základní technologie ASP.NET není přímo naslouchat požadavkům. Model hostování závisí na implementaci serveru HTTP k předání požadavku do aplikace. Předaný požadavek je zabalená jako sada objektů funkce, které je přístupné prostřednictvím rozhraní. ASP.NET Core zahrnuje spravovaná, a platformy webového serveru, nazývá [Kestrel](xref:fundamentals/servers/kestrel). Kestrel je často spouštět za produkční webového serveru, jako například [IIS](https://www.iis.net/) nebo [Nginx](http://nginx.org). Kestrel můžete spouštět jako hraniční server.

Další informace najdete v tématu [servery](xref:fundamentals/servers/index) a v následujících tématech:

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Ovladač HTTP.sys](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>Globalizace a lokalizace

Vytvoření vícejazyčné webu pomocí ASP.NET Core umožňuje vaší lokality k dosažení širší cílovou skupinu. Základní technologie ASP.NET poskytuje služby a middleware pro lokalizace do různých jazyků a kultur.

Další informace najdete v tématu [globalizace a lokalizace](xref:fundamentals/localization).

## <a name="request-features"></a>Požadavky na funkce

Související podrobnosti implementace webového serveru na požadavky HTTP a odpovědí jsou definovány v rozhraní. Tato rozhraní jsou používány implementací serveru a middleware, vytvářet a upravovat hostování kanálu aplikace.

Další informace najdete v tématu [žádosti o funkce](xref:fundamentals/request-features).

## <a name="background-tasks"></a>Úlohy na pozadí

Úlohy na pozadí jsou implementované jako *hostovaných služeb*. Hostovaná služba je třída s pozadí úloh logiky, která implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní.

Další informace najdete v tématu [pozadí úlohy s hostované služby](xref:fundamentals/hosted-services).

## <a name="open-web-interface-for-net-owin"></a>Spustit nástroj webové rozhraní pro platformu .NET (OWIN)

Jádro ASP.NET podporuje Open Web Interface pro .NET (OWIN). OWIN umožňuje webových aplikací pro být odděleno od webové servery.

Další informace najdete v tématu [Open Web Interface pro .NET (OWIN)](xref:fundamentals/owin).

## <a name="websockets"></a>Technologie WebSockets

[Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP. Použije se pro aplikace, jako je například chat, kurzů akcií, hry a kdekoli požadavky v reálném čase funkce ve webové aplikaci. Jádro ASP.NET podporuje webových funkcí produktu soketu.

Další informace najdete v tématu [Websocket](xref:fundamentals/websockets).

## <a name="microsoftaspnetcoreall-metapackage"></a>Microsoft.AspNetCore.All metapackage

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage pro ASP.NET Core zahrnuje:

* Všechny podporované balíčky tým ASP.NET Core.
* Všechny podporované balíčků základní Entity Framework. 
* Interní a 3. stran závislosti používané ASP.NET Core a Entity Framework Core.

Další informace najdete v tématu [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

## <a name="net-core-vs-net-framework-runtime"></a>Modul runtime rozhraní .NET core a rozhraní .NET Framework

Aplikace ASP.NET Core, můžete vybrat runtime .NET Core nebo rozhraní .NET Framework.

Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Zvolte mezi ASP.NET Core a ASP.NET

Další informace o výběru mezi ASP.NET Core a ASP.NET naleznete v tématu [zvolte mezi ASP.NET Core a ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).
