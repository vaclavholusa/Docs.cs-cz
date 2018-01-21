---
title: Stav relace a aplikace v ASP.NET Core
author: rick-anderson
description: "Přístupy k zachování aplikace a stavu uživatele (relace) mezi požadavky."
ms.author: riande
manager: wpickett
ms.date: 11/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 13b4d759ae574cdf9899ca148f0ffd3d9df6f9ae
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>Úvod do stavu relace a aplikace v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), a [Diana LaRose](https://github.com/DianaLaRose)

HTTP je bezstavový. Webový server zpracovává každý požadavek HTTP jako požadavek nezávislé a hodnoty uživatele z předchozích žádostech není zachována. Tento článek popisuje různé způsoby, jak zachovat aplikace a stav relace mezi požadavky. 

## <a name="session-state"></a>Stav relace

Stav relace je funkce v ASP.NET Core, který můžete použít k ukládání a uchovávání dat uživatele, když uživatel prohlíží vaší webové aplikace. Skládající se z tabulky slovník nebo hodnotu hash na serveru, stav relace trvá dat napříč požadavky z prohlížeče. Data relace je zálohovaný díky mezipaměti.

ASP.NET Core Udržovat stav relace tím, že klient soubor cookie, který obsahuje Identifikátor relace, která je odeslána na server s každou žádostí. Tento server využívá ID relace k načtení dat relace. Soubor cookie relace je prohlížeč, a proto nelze sdílet relací mezi prohlížeče. Soubory cookie relace se odstraní pouze při ukončení relace prohlížeče. Pokud není soubor cookie pro relaci s ukončenou platností, je vytvořit novou relaci, který používá stejný soubor cookie relace. 

Server uchovává relace po omezenou dobu od poslední žádosti. Můžete nastavit časový limit relace nebo použijte výchozí hodnotu 20 minut. Je ideální pro ukládání uživatelských dat, která je specifická pro konkrétní relace, ale nemusí být trvale jako trvalý stav relace. Data odstranit z úložiště zálohování buď při volání `Session.Clear` nebo vypršení platnosti relace v úložišti. Server nebude vědět při zavření prohlížeče nebo když se odstraní soubor cookie relace.

> [!WARNING]
> Neukládejte citlivá data v relaci. Klient nemusí zavřete prohlížeč a vymazat souboru cookie relace (a některé prohlížeče zachování souborů cookie relací připojení mezi windows). Navíc nemusí být relaci omezen na jednoho uživatele; Další uživatel může pokračovat ve stejné relaci.

Zprostředkovatel relací v paměti ukládá data relace na místním serveru. Pokud máte v plánu ke spouštění vaší webové aplikace na serverové farmě, musíte použít trvalé relace ke svázání každou relaci k určitému serveru. Platforma Windows Azure webů výchozí trvalé relace (směrování žádostí na aplikace nebo směrování žádostí na aplikace). Trvalé relace však může mít vliv na škálovatelnost a zkomplikovat aktualizace webové aplikace. Lepší možností je používat Redis nebo SQL Server distribuované ukládá do mezipaměti, které nevyžadují trvalé relace. Další informace najdete v tématu [práce s distribuované mezipaměti](xref:performance/caching/distributed). Podrobnosti o nastavení poskytovatelé služeb najdete v tématu [konfigurace relace](#configuring-session) dále v tomto článku.

<a name="temp"></a>
## <a name="tempdata"></a>TempData

ASP.NET MVC základní zpřístupní [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) vlastnost [řadiče](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0). Tato vlastnost ukládá data, dokud je pro čtení. `Keep` a `Peek` metody můžete použít k prozkoumání dat bez odstranění. `TempData`je obzvláště užitečné pro přesměrování, pokud dat je potřeba pro více než jeden požadavek. `TempData`je implementováno modulem TempData poskytovatelů, například pomocí souborů cookie nebo stav relace.

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a>Zprostředkovatelé TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

V technologii ASP.NET Core 2.0 nebo novější TempData zprostředkovatele na základě souborů cookie se používá ve výchozím nastavení pro ukládání TempData v souborech cookie.

Cookie data je zakódovaných pomocí [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0). Vzhledem k tomu, že soubor cookie je šifrovaný a blokové, velikost jednoho souboru cookie limit v ASP.NET Core 1.x doporučení se netýká nalezen. Není komprimována cookie data, protože kompresi šifrovaná data může způsobit problémy se zabezpečením, jako [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) a [porušení](https://wikipedia.org/wiki/BREACH_(security_exploit)) útoky. Další informace o poskytovateli TempData na základě souborů cookie najdete v tématu [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

TempData poskytovatele stavu relace ASP.NET Core 1.0 a 1.1, je výchozí.

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a>Výběr zprostředkovatele TempData

Výběr zprostředkovatele TempData zahrnuje několik důležité informace, například:

1. Používá aplikace již stav relace pro jiné účely? Pokud ano, použití TempData poskytovatele stavu relace má bez dalších nákladů na aplikace (kromě zajištění dostatečného množství dat).
2. Aplikace používá TempData pouze pouze pro relativně malé množství dat (až 500 bajtů)? Pokud ano, přidá zprostředkovatele TempData souboru cookie malé náklady na každý požadavek, který představuje TempData. Pokud ne, může být výhodné, aby se zabránilo odezvy velké množství dat v každé žádosti o, dokud TempData obsazením TempData poskytovatele stavu relace.
3. Spuštění aplikace ve webové farmě (více serverů)? Pokud ano, neexistuje žádná další konfigurace, které jsou potřeba k použití zprostředkovatele TempData souboru cookie.

> [!NOTE]
> Většina webovými klienty (například webové prohlížeče) vynutit omezení pro maximální velikost každého souboru cookie, celkový počet souborů cookie nebo obojí. Proto při použití zprostředkovatele TempData souboru cookie, ověřte, že aplikace nesmí být větší než tato omezení. Vezměte v úvahu celková velikost dat, monitorování účtů pro režie šifrování a rozdělování.

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a>Konfigurace zprostředkovatele TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Ve výchozím nastavení je povolen TempData zprostředkovatel na základě souboru cookie. Následující `Startup` kód třídy nakonfiguruje TempData zprostředkovatele na bázi relace:

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Následující `Startup` kód třídy nakonfiguruje TempData zprostředkovatele na bázi relace:

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

Pořadí je důležité pro komponenty middlewaru. V předchozím příkladu, k výjimce typu `InvalidOperationException` dojde při `UseSession` je volána poté, co `UseMvcWithDefaultRoute`. V tématu [řazení Middleware](xref:fundamentals/middleware#ordering) další podrobnosti.

> [!IMPORTANT]
> Pokud cílení na rozhraní .NET Framework a pomocí zprostředkovatele na bázi relace, přidejte [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) balíček NuGet do projektu.

## <a name="query-strings"></a>Řetězce dotazů

Můžete předat omezené množství dat z jednoho požadavku na jiný přidáním do novou žádost o řetězce dotazu. To je užitečné pro zaznamenání stavu trvalé způsobem, který umožňuje propojení s embedded stavu sdílení e-mailu nebo sociálních sítí. Ale z tohoto důvodu by nikdy používáte řetězce dotazu pro citlivá data. Kromě se snadno sdílet, včetně dat v řetězcích dotazů můžete vytvořit příležitosti pro [webů požadavku padělání (proti útokům CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) útoků, které můžete obelstít návštěvou škodlivé weby při ověření uživatele. Útočníci mohou pak odcizit data uživatele z vaší aplikace nebo provést škodlivé akce jménem uživatele. Zachovaných stavu application nebo session musí zajistit ochranu proti útokům proti útokům CSRF. Další informace o útoku proti útokům CSRF najdete v tématu [útoky brání webů požadavku padělání (XSRF/proti útokům CSRF) v ASP.NET Core](../security/anti-request-forgery.md).

## <a name="post-data-and-hidden-fields"></a>Následná data a skrytá pole

Data můžete uložit ve skrytého pole a odeslány zpět na další požadavek. To je běžné ve formulářích s více stránkami. Ale vzhledem k tomu, že klient může potenciálně manipulovat s daty, server musí vždy znovu ověřit ho. 

## <a name="cookies"></a>Soubory cookie

Soubory cookie poskytují způsob, jak ukládat data specifická pro uživatele ve webových aplikacích. Soubory cookie jsou odesílány s každou žádost, jejich velikost měli omezit na minimum. V ideálním případě by měly jenom identifikátor uložené v souboru cookie s dat uložených na serveru. Většina prohlížečů soubory cookie omezit na 4096 bajtů. Kromě toho jsou k dispozici pro každou doménu pouze omezený počet souborů cookie.  

Protože soubory cookie se vztahují manipulaci, musí být ověřený na serveru. Odolnost souboru cookie na klientském podléhá zásahu uživatele a vypršení platnosti, jsou obecně nejvíce trvanlivá formu trvalosti dat na straně klienta.

Soubory cookie se často používají pro přizpůsobení, kde je obsah přizpůsobit pro známé uživatele. Protože uživatel je pouze identifikovat a ve většině případů není ověřen, obvykle můžete zabezpečit souboru cookie uložením uživatelské jméno, název účtu nebo jedinečné ID uživatele (například identifikátor GUID) v souboru cookie. Pak můžete soubor cookie pro přístup k infrastruktuře přizpůsobení uživatele lokality.

## <a name="httpcontextitems"></a>HttpContext.Items

`Items` Kolekce je dobré umístění pro uložení dat, která je potřeba při jenom jednoho konkrétního požadavku na zpracování. Obsah kolekce jsou zahozeny po každé žádosti. `Items` Kolekce je nejvhodnější jako způsob, jak součásti nebo middleware pro komunikaci, když budou fungovat v různých okamžicích v době žádost a mají přímý způsob, jak předat parametry. Další informace najdete v tématu [práce s HttpContext.Items](#working-with-httpcontextitems)dál v tomto článku.

## <a name="cache"></a>Mezipaměti

Ukládání do mezipaměti je účinný způsob, jak ukládat a načítat data. Můžete ovládat, doba platnosti položek v mezipaměti na základě času a další důležité informace. Další informace o [ukládání do mezipaměti](../performance/caching/index.md).

<a name="session"></a>
## <a name="working-with-session-state"></a>Práce s stav relace

### <a name="configuring-session"></a>Konfigurace relace

`Microsoft.AspNetCore.Session` Balíček poskytuje middleware pro správu stavu relace. Povolit middleware relace `Startup` musí obsahovat:

- Některé z [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) mezipaměti paměti. `IDistributedCache` Implementace slouží jako úložiště zálohování pro relaci.
- [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) volání, což vyžaduje, aby balíček NuGet "Microsoft.AspNetCore.Session".
- [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) volání.

Následující kód ukazuje, jak nastavit Zprostředkovatel relací v paměti.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

Relace, můžete odkazovat `HttpContext` po je nainstalovaný a nakonfigurovaný.

Pokud se pokusíte přístup `Session` před `UseSession` byla volána, výjimka `InvalidOperationException: Session has not been configured for this application or request` je vyvolána výjimka.

Pokud se pokusíte vytvořit novou `Session` (tedy bez souboru cookie relace byla vytvořena) poté, co jste již byl zahájen zápis do `Response` stream, výjimka `InvalidOperationException: The session cannot be established after the response has started` je vyvolána výjimka. Výjimka naleznete v protokolu webového serveru; se nebude zobrazovat v prohlížeči.

### <a name="loading-session-asynchronously"></a>Asynchronní načítání relace 

Výchozí zprostředkovatel relace v ASP.NET Core načte záznam relace ze základní [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) asynchronně jenom Pokud úložiště [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) metoda je explicitně volána před provedením  `TryGetValue`, `Set`, nebo `Remove` metody. Pokud `LoadAsync` není jako první, základní záznam relace je načtena synchronně, což může potenciálně ovlivnit aplikace škálování.

Pokud chcete, aby aplikace vynutit tento vzor, zabalení [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) a [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementace s verzemi, které způsobí výjimku, pokud `LoadAsync` metoda není volá se před `TryGetValue`, `Set`, nebo `Remove`. Zaregistrujte zabalené verze v kontejneru služby.

### <a name="implementation-details"></a>Podrobnosti implementace

Relace používá ke sledování a identifikaci požadavků z jednoho prohlížeče do souboru cookie. Ve výchozím nastavení, je tento soubor cookie s názvem ". AspNet.Session"a používá cestu"/". Protože výchozí soubor cookie neurčuje doménu, se nebude zpřístupněn pro skript na straně klienta na stránce (protože `CookieHttpOnly` výchozí `true`).

Chcete-li přepsat výchozí hodnoty relace, použijte `SessionOptions`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

Tento server využívá `IdleTimeout` vlastnosti k určení, jak dlouho relace může být nečinnosti, než jsou opuštění její obsah. Tato vlastnost je nezávislá na vypršení platnosti souboru cookie. Každý požadavek, který předává přes middleware relace (číst nebo zapisovat do) obnoví časový limit.

Protože `Session` je *bez uzamčení*, pokud dva požadavky obou pokusí změnit obsah relace, poslední přepíše první. `Session`je implementovaný jako *souvislý relace*, což znamená, že se veškerý obsah ukládat společně. Dva požadavky, které se změnit různé části relace (různé klíče) může stále vzájemně vliv.

### <a name="setting-and-getting-session-values"></a>Nastavení a získání hodnoty relace

Relace je přístupné přes `Session` vlastnost `HttpContext`. Tato vlastnost je [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementace.

Následující příklad ukazuje nastavení a získání int a řetězec:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

Pokud přidáte následující metody rozšíření, můžete nastavit a získat serializovatelné objekty do relace:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

Následující příklad ukazuje, jak nastavit a získat serializovatelný objekt:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>Práce s HttpContext.Items

`HttpContext` Abstrakce zajišťuje podporu pro kolekci slovníku typu `IDictionary<object, object>`, názvem `Items`. Tato kolekce je k dispozici od začátku *požadavku HTTP* a na konci každého požadavku se zahodí. Můžete k němu přístup přiřazením hodnoty pro položku s klíčem nebo tím, že požádá hodnotu pro konkrétní klíč.

V ukázce níže [Middleware](middleware.md) přidá `isVerified` k `Items` kolekce.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Později v kanálu další middleware může k němu přístup:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

Pro middleware, která bude použita pouze jedna aplikace `string` klíče jsou přijatelné. Ale middleware, který bude sdílena mezi aplikací používejte jedinečný objekt předejdete pravděpodobné, že kolize klíče. Pokud vyvíjíte middleware, který musí fungovat na všech několik aplikací, použijte klíč jedinečný objekt definovaný ve třídě middleware, jak je uvedeno níže:

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

Jiný kód, můžete přístup s hodnotou uloženou v `HttpContext.Items` pomocí klíče vystavené middleware třídy:

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

Tento přístup má také výhod odstranění opakování "magic řetězce" na více místech v kódu.

<a name="appstate-errors"></a>

## <a name="application-state-data"></a>Data stavu aplikace

Použití [vkládání závislostí](xref:fundamentals/dependency-injection) jak zpřístupnit data pro všechny uživatele:

1. Definovat službu, která obsahuje data (například třídy s názvem `MyAppData`).

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. Přidání třídy pro službu `ConfigureServices` (například `services.AddSingleton<MyAppData>();`).
3. Třída služby dat v každém řadiči využívat:

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a>Běžné chyby při práci s relací

* "Nelze přeložit služby pro typ 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' při pokusu o aktivaci 'Microsoft.AspNetCore.Session.DistributedSessionStore'."

  To je obvykle způsobeno nedaří nakonfigurovat alespoň jednu `IDistributedCache` implementace. Další informace najdete v tématu [práce s distribuované mezipaměti](xref:performance/caching/distributed) a [v ukládání do mezipaměti](xref:performance/caching/memory).

* V případě, který relace middleware nepodaří zachovat relace (například: Pokud databáze není k dispozici), se protokoluje výjimku a swallows ho. Žádost se pak normálně pokračovat, což vede k velmi nepředvídatelné chování.

Typickým příkladem:

Někdo uloží nákupní košík v relaci. Uživatel přidá položku, ale potvrzení se nezdaří. Aplikace neví o selhání, proto sestavy zpráva "položku přidala", což není pravda.

Je doporučeným způsobem, jak zkontrolovat takové chyby pro volání `await feature.Session.CommitAsync();` z kódu aplikace po dokončení zápisu do relace. Pak můžete udělat, co se vám líbí došlo k chybě. Funguje stejným způsobem jako při volání metody `LoadAsync`.


### <a name="additional-resources"></a>Další prostředky


* [ASP.NET Core 1.x: ukázkový kód v tomto dokumentu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x: ukázkový kód v tomto dokumentu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
