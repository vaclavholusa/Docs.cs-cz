---
title: "Migrace obslužné rutiny HTTP a moduly, které middleware ASP.NET Core"
author: rick-anderson
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: 8aac6c649b22dc8f6cfc916aa78d56efad7821a0
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a>Migrace obslužné rutiny HTTP a moduly, které middleware ASP.NET Core 

Podle [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

Tento článek ukazuje, jak migrovat existující ASP.NET [modulů HTTP a obslužné rutiny z system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) na jádro ASP.NET [middleware](xref:fundamentals/middleware/index).

## <a name="modules-and-handlers-revisited"></a>Moduly a obslužné rutiny kdykoli znovu spustit.

Před pokračováním ASP.NET Core middleware, můžeme nejprve recap jak fungují moduly HTTP a obslužné rutiny:

![Obslužná rutina moduly](http-modules/_static/moduleshandlers.png)

**Obslužné rutiny jsou:**

   * Třídy implementující [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)

   * Umožňuje zpracování požadavků daného názvu souboru nebo rozšíření, jako například *.report*

   * [Nakonfigurované](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) v *souboru Web.config*

**Moduly jsou:**

   * Třídy implementující [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)

   * Volá se pro každý požadavek

   * Může krátká smyčka (zastaví další zpracování požadavku)

   * Možné přidat do odpovědi HTTP ani vytvářet své vlastní

   * [Nakonfigurované](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) v *souboru Web.config*

**Pořadí modulů zpracovat příchozí žádosti, ve kterém je určený:**

   1. [Životního cyklu aplikace](https://msdn.microsoft.com/library/ms227673.aspx), což je řady událostí, aktivováno technologií ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest)atd. Každý modul můžete vytvořit obslužnou rutinu pro jeden nebo více událostí.

   2. Pro stejnou událost, pořadí, ve které jste nakonfigurovali v *Web.config*.

Kromě moduly, obslužné rutiny pro události životního cyklu, které přidáte vaše *Global.asax.cs* souboru. V těchto obslužných rutinách spustit po obslužné rutiny v nakonfigurované moduly.

## <a name="from-handlers-and-modules-to-middleware"></a>Z obslužné rutiny a moduly, které middleware

**Middleware jsou jednodušší než vytváření modulů HTTP a obslužné rutiny:**

   * Moduly, obslužné rutiny, *Global.asax.cs*, *Web.config* (s výjimkou konfigurace služby IIS) a životního cyklu aplikace jsou pryč

   * Role modulů a obslužné rutiny byly převzaty middlewaru.

   * Middleware jsou konfigurováni pomocí kódu spíše než v *souboru Web.config*

   * [Vytvoření větve kanálu](xref:fundamentals/middleware/index#middleware-run-map-use) umožňuje odesílat žádosti do konkrétní middleware, podle ne jenom adresu URL, ale i na hlavičky požadavku, řetězce dotazu, atd.

**Middleware jsou velmi podobné moduly:**

   * Volá v zásadě pro každý požadavek

   * Může krátká smyčka žádost, pomocí [není předání požadavku do dalšího middlewaru](#http-modules-shortcircuiting-middleware)

   * K vytvoření vlastních odpovědi HTTP

**Middleware a moduly jsou zpracovány v jiném pořadí:**

   * Pořadí middlewaru je založeno na pořadí, ve kterém jsou vkládána do kanálu požadavku, zatímco především podle pořadí modulů [životního cyklu aplikace](https://msdn.microsoft.com/library/ms227673.aspx) události

   * Pořadí middleware pro odpovědi je zpětného od pro požadavky, zatímco pořadí modulů je stejný pro požadavky a odpovědi

   * V tématu [vytváření middlewaru v řadě s IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)

![Middleware](http-modules/_static/middleware.png)

Všimněte si, jak na předchozím obrázku middleware ověřování short-circuited žádosti.

## <a name="migrating-module-code-to-middleware"></a>Migrace modulu kód pro middleware

Existující modul HTTP bude vypadat podobně jako tento:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Jak je znázorněno v [Middleware](xref:fundamentals/middleware/index) stránka, middlewaru ASP.NET Core je třída, která zveřejňuje `Invoke` metoda pořízení `HttpContext` a vrácení `Task`. Vaše nové middleware bude vypadat takto:

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

Předchozí middleware šablony pořízení z části [zápis middleware](xref:fundamentals/middleware/index#middleware-writing-middleware).

*MyMiddlewareExtensions* pomocná třída usnadňuje konfiguraci vlastního middlewaru v vaší `Startup` třídy. `UseMyMiddleware` Metoda přidá do kanálu žádost o třídě middleware. V konstruktoru middlewaru získat vložit služeb požadovaných middleware.

<a name="http-modules-shortcircuiting-middleware"></a>

Modul může ukončit žádost, například pokud uživatel nemá oprávnění:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

Middleware zpracovává tento není voláním `Invoke` na další middleware v kanálu. Mějte na paměti, že to není ukončit plně požadavek, protože předchozí middlewares bude stále vyvolán při odpovědi díky zpět skrze kanálu.

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

Při migraci vaší modulu funkce do vaší nové middleware, můžete zjistit, že kód není kompilovat, protože `HttpContext` v ASP.NET Core výrazně změnila třída. [Později na](#migrating-to-the-new-httpcontext), uvidíte, jak migrovat do nového HttpContext základní technologie ASP.NET.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>Migrace modulu vložení do kanálu požadavku

Vytváření modulů HTTP jsou obvykle přidány do žádosti o kanálu pomocí *Web.config*:

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

Převést tím, že [přidání vlastního nové middlewaru](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder) na kanále vaší `Startup` – třída:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

Přesnou pozici v kanálu, kde vložit vaše nové middleware závisí na událost, která ji zpracovává jako modul (`BeginRequest`, `EndRequest`atd) a jeho pořadí v seznamu modulů v *Web.config*.

Jako dříve jsme si říkali, neexistuje žádný životního cyklu aplikace v ASP.NET Core a pořadí, ve kterém jsou zpracovány odpovědí middlewarem se liší od pořadí použít moduly. To může se rozhodnout řazení další náročné.

Řazení přestane být problém, může rozdělit modul do více součástí middleware, které lze provést řazení nezávisle.

## <a name="migrating-handler-code-to-middleware"></a>Migrace kód obslužné rutiny pro middleware

Obslužné rutiny HTTP vypadá přibližně takto:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

V projektu ASP.NET Core by to nepřeloží na middleware podobná této:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

Tento middleware je velmi podobný middleware odpovídající moduly. Pouze skutečné rozdílem je, že zde není žádná volání `_next.Invoke(context)`. To dává smysl, protože rutina je na konci kanálu požadavku, takže neexistují žádné další middleware má být vyvolána.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>Migrace obslužná rutina vložení do kanálu požadavku

Konfigurace obslužné rutiny HTTP se provádí v *Web.config* a vypadá takto:

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

To může převést přidáním vlastního middlewaru novou obslužnou rutinu do kanálu požadavku ve vaší `Startup` třída, podobně jako middleware převést z modulů. Problém s tohoto přístupu je, že ho do vaší nové middleware obslužnou rutinu by odesílat všechny požadavky. Ale chcete jenom žádosti s příponou dané k dosažení vlastního middlewaru. Který, získáte stejným funkcím, jaké jste měli s vaší obslužné rutiny HTTP.

Jedno řešení je větev kanálu pro požadavky s danou příponu, pomocí `MapWhen` metoda rozšíření. To uděláte ve stejné `Configure` metoda, kde můžete přidat další middleware:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen`mají tyto parametry:

1. Lambda, která přebírá `HttpContext` a vrátí `true` Pokud požadavek by měl přejděte dolů větev. To znamená, že můžete větev požadavky nejen založené na jejich rozšíření, ale také na hlavičky požadavku, parametrů řetězce dotazu, atd.

2. Lambda, který přebírá `IApplicationBuilder` a přidá všechny middleware pro větev. To znamená, že můžete přidat další middleware do větve před vlastního middlewaru obslužné rutiny.

Middleware přidán do kanálu, než větev, který bude vyvolán u všech požadavků; větev nebude mít vliv na ně.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Možnosti middlewaru pomocí vzoru možnosti načítání

Některé moduly a obslužné rutiny mají možnosti konfigurace, které jsou uložené v *Web.config*. V ASP.NET Core nový model konfigurace se ale používá místě *Web.config*.

Nové [konfigurační systém](xref:fundamentals/configuration/index) vám dává tyto možnosti, chcete-li tento problém vyřešit:

* Přímo vložit možnosti do middleware, jak je znázorněno [další části](#loading-middleware-options-through-direct-injection).

* Použití [možnosti vzor](xref:fundamentals/configuration/options):

1.  Vytvoření třídy k umístění middleware možnosti, například:

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  Uložení možnost hodnot

    Konfigurace systému umožňuje ukládat hodnoty možnosti kdekoli chcete. Ale většina lokality použijte *appSettings.JSON určený*, takže provedeme tohoto přístupu:

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    *MyMiddlewareOptionsSection* následuje název oddílu. Nemusí to být stejný jako název třídy vaše možnosti.

3. Hodnoty možnosti přidružit možnosti – třída

    Vzor možnosti používá framework vkládání závislostí ASP.NET Core k přidružení typu možnosti (například `MyMiddlewareOptions`) s `MyMiddlewareOptions` objekt, který obsahuje skutečné možnosti.

    Aktualizace vašeho `Startup` třídy:

    1.  Pokud používáte *appSettings.JSON určený*, přidejte ho do Tvůrce konfigurace v `Startup` konstruktor:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  Konfigurovat službu možnosti:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  Možnosti přidružte třídě možnosti:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  Vložit možnosti do vaší konstruktor middlewaru. Toto je podobná vložení možnosti do kontroleru.

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  [UseMiddleware](#http-modules-usemiddleware) metody rozšíření, která přidá vaše middlewaru, který má `IApplicationBuilder` postará vkládání závislostí.

  Tato akce není omezen na `IOptions` objekty. Tímto způsobem může vložit jakýkoliv jiný objekt, který vyžaduje vlastního middlewaru.

## <a name="loading-middleware-options-through-direct-injection"></a>Možnosti middlewaru prostřednictvím přímé vkládání načítání

Vzor možnosti má výhodu, který vytvoří přijít spojení mezi hodnotami možnosti a jejich příjemce. Po třída možností jste přidružené hodnoty skutečné možnosti, jiná třída lze získat přístup k možnostem prostřednictvím rozhraní vkládání závislostí. Není nutné předat kolem hodnoty možnosti.

To rozpis ale pokud chcete použít stejné middleware dvakrát, s různými možnostmi. Například autorizaci middleware použitý v různých pobočkách, která umožňuje různé role. Pomocí třídy jeden možnosti nelze přidružit dva objekty různé možnosti.

Řešení je získat možnosti objekty s hodnotami skutečné možnosti ve vaší `Startup` třídy a předejte těch, které přímo ke každé instanci vlastního middlewaru.

1.  Přidat druhý klíč do *appSettings.JSON určený*

    Chcete-li přidat druhý sadu možností a *appSettings.JSON určený* soubor, použijte nový klíč k jeho jednoznačné identifikaci:

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  Načíst hodnoty možností a předejte middleware. `Use...` – Metoda rozšíření (která vlastního middlewaru přidá do kanálu) je logické místo, kde můžete předat hodnoty možnosti: 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  Povolí middleware má provést parametrem možnosti. Zadejte přetížení `Use...` metoda rozšíření (která přebírá parametr možnosti a předává jej do `UseMiddleware`). Když `UseMiddleware` je volána s parametry, předává parametry do vaší konstruktor middlewaru při vytvoření instance objektu middleware.

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    Všimněte si, jak to zabalí objekt možnosti v `OptionsWrapper` objektu. To implementuje `IOptions`, podle očekávání tím konstruktor middlewaru.

## <a name="migrating-to-the-new-httpcontext"></a>Migrace na nové HttpContext

Už jste viděli dříve, `Invoke` metoda v vlastního middlewaru přebírá parametr typu `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext`výrazně se změnilo v ASP.NET Core. V této části ukazuje, jak převede nejčastěji používané vlastnosti [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) do nového `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items** přeloží na:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**ID jedinečné žádosti (System.Web.HttpContext protějšek)**

Poskytuje jedinečné id pro každý požadavek. Velmi užitečné zahrnout do protokolů.

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** přeloží na:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** a **HttpContext.Request.RawUrl** nepřeloží na:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies** přeloží na:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form** přeloží na:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> Čtení hodnot formuláře, pouze v případě, že je typ obsahu dílčí *x--www-form-urlencoded* nebo *data formuláře*.

**HttpContext.Request.InputStream** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Tento kód používejte pouze v middleware typ obslužné rutiny, na konci kanálu.
>
>Nezpracovaná text si můžete přečíst jako v příkladu nahoře jen jednou na základě požadavku. Při pokusu o čtení textu po první čtení middleware budou číst prázdným textem zprávy.
>
>To neplatí pro čtení formuláře, jako je uvedené výše, vzhledem k tomu, které se provádí z vyrovnávací paměti.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** a **HttpContext.Response.StatusDescription** nepřeloží na:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** a **HttpContext.Response.ContentType** nepřeloží na:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** na svůj vlastní také překládá do:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** přeloží na:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Obsluhující si soubor popsané [zde](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

Odeslání hlaviček odpovědí ztěžuje skutečnost, že pokud jste nastavili, je po nic byla zapsána na text odpovědi, se nebude odeslána.

Řešení je nastavena metoda zpětného volání, která bude volána vpravo před zápisem na spustí odpověď. To se nejlépe provádí na začátku `Invoke` metoda v vlastního middlewaru. Je tato metoda zpětného volání, která nastavuje vaše hlavičky odpovědi.

Následující kód nastaví zpětné volání metodu s názvem `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders` Metoda zpětného volání bude vypadat takto:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Soubory cookie dostavit do prohlížeče v *Set-Cookie* hlavičky odpovědi. V důsledku toho odesílání souborů cookie vyžaduje stejné zpětného volání jako používané pro odeslání hlaviček odpovědí:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetCookies` Metoda zpětného volání by vypadat třeba takto:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>Další zdroje

* [Přehled modulů HTTP a obslužné rutiny HTTP](/iis/configuration/system.webserver/)
* [Konfigurace](xref:fundamentals/configuration/index)
* [Spuštění aplikace](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware/index)
