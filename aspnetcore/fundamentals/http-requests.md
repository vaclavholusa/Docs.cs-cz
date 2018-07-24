---
title: Iniciování požadavků HTTP
author: stevejgordon
description: Další informace o použití rozhraní IHttpClientFactory ke správě instance logického HttpClient v ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: fundamentals/http-requests
ms.openlocfilehash: 0aca2b260e787f9b8aa0846bcccef2b33f372ee6
ms.sourcegitcommit: 6425baa92cec4537368705f8d27f3d0e958e43cd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2018
ms.locfileid: "39220583"
---
# <a name="initiate-http-requests"></a>Iniciování požadavků HTTP

Podle [Glenn Condron](https://github.com/glennc), [Ryanem Nowak](https://github.com/rynowak), a [Steve Gordon](https://github.com/stevejgordon)

[IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) můžete zaregistrované a slouží ke konfiguraci a vytvořte [HttpClient](/dotnet/api/system.net.http.httpclient) instance v aplikaci. Nabízí následující výhody:

* Poskytuje centrální umístění pro pojmenovávání a konfiguraci logické `HttpClient` instancí. Například "githubu" klienta můžete zaregistrované a nakonfigurovat tak, aby ke Githubu přistupovat. Výchozí klienta lze zaregistrovat k jiným účelům.
* Kodifikuje koncept odchozí middleware prostřednictvím delegování obslužné rutiny ve `HttpClient` a rozšíření pro middleware založený na Polly výhod, které poskytuje.
* Spravuje sdružování a dobu života základní `HttpClientMessageHandler` instancí se vyhnout běžným potížím DNS, ke kterým dochází při správě ručně `HttpClient` životnosti.
* Přidá prostředí konfigurovat protokolování (prostřednictvím `ILogger`) pro všechny požadavky odeslané prostřednictvím klientů, které jsou vytvořeny procesem.

## <a name="prerequisites"></a>Požadavky

Projekty cílené na rozhraní .NET Framework vyžadují instalaci [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) balíček NuGet. Projekty, které cílí na .NET Core a odkaz [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) již patří `Microsoft.Extensions.Http` balíčku.

## <a name="consumption-patterns"></a>Vzory využití

Existuje několik způsobů `IHttpClientFactory` lze použít v aplikaci:

* [Základní informace o využití](#basic-usage)
* [Pojmenované klientů](#named-clients)
* [Typy klientů](#typed-clients)
* [Vygenerovaný klientů](#generated-clients)

Žádná z nich není striktně vynikající do jiného. Nejlepší přístup, závisí na omezení aplikace.

### <a name="basic-usage"></a>Základní informace o využití

`IHttpClientFactory` Lze registrovat pomocí volání `AddHttpClient` rozšiřující metody na `IServiceCollection`uvnitř `Startup.ConfigureServices` metoda.

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

Po registraci může přijmout kódu `IHttpClientFactory` kdekoli služby může být vložený se [injektáž závislostí](xref:fundamentals/dependency-injection) (DI). `IHttpClientFactory` Slouží k vytvoření `HttpClient` instance:

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

Pomocí `IHttpClientFactory` tímto způsobem je skvělý způsob, jak Refaktorovat stávající aplikace. Nemá žádný vliv na způsob, jakým `HttpClient` se používá. Na místech, kde `HttpClient` aktuálně se vytvářejí instance, nahraďte volání výskyty `CreateClient`.

### <a name="named-clients"></a>Pojmenované klientů

Pokud aplikace vyžaduje víc různé způsoby použití `HttpClient`, každý s jinou konfiguraci, je možnost použití **s názvem klienti**. Konfigurace pro pojmenovaná `HttpClient` se dá nastavit během registrace v `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

V předchozím kódu `AddHttpClient` je volána, poskytuje název "githubu". Některé výchozí konfigurace má tento klient&mdash;totiž základní adrese a dvě záhlaví vyžadována pro práci s rozhraním API pro GitHub.

Pokaždé, když `CreateClient` nazývá novou instanci třídy `HttpClient` vytvoření a konfigurace akce je volána.

Využívat pojmenované klienta, může být předán parametr řetězce `CreateClient`. Zadejte název klienta, který se má vytvořit:

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

V předchozím kódu žádost nemusí zadejte název hostitele. Ho můžete předat právě cestu, protože se používá základní adresu nakonfigurovanou pro klienta.

### <a name="typed-clients"></a>Typy klientů

Typy klientů poskytují stejné funkce jako pojmenované klientů bez nutnosti použití řetězců jako klíče. Typový klient přístup poskytuje IntelliSense a kompilátoru pomoct při využívání klientů. Poskytuje jedno centrální umístění pro konfiguraci a interakci s konkrétní `HttpClient`. Například konkrétního typu klienta může být použit pro koncový bod jediného back-endu a zapouzdření všech řešení logiku tohoto koncového bodu. Další výhodou je, že práce s DI a můžete vloženy vyžadováno ve vaší aplikaci.

Typový klient přijme `HttpClient` parametr v konstruktoru:

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

V předchozím kódu konfigurace přesunout do typový klient. `HttpClient` Objektu je vystaven jako veřejná vlastnost. Je možné definovat metody specifické pro rozhraní API, které vystavují `HttpClient` funkce. `GetAspNetDocsIssues` Metoda zapouzdřuje kód potřebný k vyhledání a parsování nejnovější otevřené problémy z úložiště GitHub.

K registraci typový klient Obecné `AddHttpClient` metody rozšíření lze použít v `Startup.ConfigureServices`, určení typový klient třídy:

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

Typový klient je zaregistrovaný jako přechodné s DI. Typový klient může vloží a využívat přímo:

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Pokud tomu dávají přednost, konfigurace pro typový klient se dá nastavit během registrace v `Startup.ConfigureServices`, spíše než v konstruktoru typu klienta:

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

Je možné zcela zapouzdření `HttpClient` v rámci typu klienta. Místo bude vystavená jako vlastnost, je možné veřejné metody poskytnou která volá `HttpClient` instance interně.

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

V předchozím kódu `HttpClient` se ukládá jako soukromé pole. Veškerý přístup pro volání externích prochází `GetRepos` metody.

### <a name="generated-clients"></a>Vygenerovaný klientů

`IHttpClientFactory` můžete použít v kombinaci s dalšími knihovnami třetích stran, jako [provedení nového](https://github.com/paulcbetts/refit). Refit je REST knihovna pro .NET. Rozhraní REST API se převede na živé interfaces. Implementace rozhraní se vygeneruje dynamicky pomocí `RestService`pomocí `HttpClient` aby externí HTTP volání.

Rozhraní a odpovědi jsou definovány pro reprezentaci externí rozhraní API a odpověď:

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

Typový klient můžete přidat, pomocí Refit generovat implementace:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

Definované rozhraní můžete využívat v případě potřeby s implementací poskytované DI a Refit:

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a>Odchozí žádosti o middlewaru

`HttpClient` již obsahuje koncept delegování obslužné rutiny, které může být propojený pro odchozí požadavky HTTP. `IHttpClientFactory` Usnadňuje k definování obslužných rutin mají použít u každého klienta s názvem. Podporuje registraci a zřetězení více obslužných rutin k sestavení kanál middleware odchozí požadavek. Každá z těchto obslužných rutin je moci provádět úkoly před a za odchozí požadavek. Tento model je podobný kanál příchozí middlewaru v ASP.NET Core. Vzor poskytuje mechanismus ke správě vyskytující aspekty kolem požadavků protokolu HTTP, včetně ukládání do mezipaměti, zpracování chyb, serializaci a protokolování.

Chcete-li vytvořit obslužnou rutinu, definujte třídu odvozenou z `DelegatingHandler`. Přepsat `SendAsync` metoda spuštění kódu před předáním požadavku další obslužná rutina kanálu:

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Předcházející kód definuje obslužnou rutinu základní. Zkontroluje, pokud u požadavku byla zahrnuta hlavičku X-API-KEY. Pokud chybí záhlaví, můžete vyhnout volání HTTP a vrátí odpověď vhodný.

Během registrace, lze přidat jeden nebo více obslužných rutin do konfigurace `HttpClient`. Tato úloha se provádí prostřednictvím metody rozšíření na `IHttpClientBuilder`.

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

V předchozím kódu `ValidateHeaderHandler` DI zaregistrován. Obslužná rutina **musí** zaregistrovat v DI jako přechodné. Po registraci `AddHttpMessageHandler` může být volána, předejte typ pro obslužné rutiny.

Může být registrováno více obslužných rutin v pořadí, ve kterém by se měl spustit. Každý popisovač zabalí další obslužná rutina až do konečné `HttpClientHandler` zpracuje požadavek:

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a>Použít na základě Polly obslužné rutiny

`IHttpClientFactory` se integruje s oblíbenými knihovnu třetí strany s názvem [Polly](https://github.com/App-vNext/Polly). Polly je komplexní odolnosti a přechodné zpracování chyb library pro .NET. Umožňuje vývojářům vyjádřit zásady například opakování, jističe, vypršení časového limitu, přepážka izolace a záložních fluent a bezpečným způsobem.

Metody rozšíření jsou k dispozici pro povolení použití zásad Polly nakonfigurované `HttpClient` instancí. Jsou k dispozici v rozšíření Polly [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) balíček NuGet. Není součástí tohoto balíčku [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app). Abyste použili rozšíření explicitní `<PackageReference />` by měl být zahrnutý v projektu.

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

Po obnovení tohoto balíčku, rozšiřující metody jsou k dispozici pro podporu přidání obslužné rutiny na základě Polly klientům.

### <a name="handle-transient-faults"></a>Zpracování přechodných chyb

Nejběžnější chybami, na které můžete očekávat při volání HTTP externí bude přechodné. Pohodlné rozšíření metoda volána `AddTransientHttpErrorPolicy` je zahrnuté umožňuje zásadu, která definované pro zpracování přechodných chyb. Zásady konfigurované pomocí tohoto úchytu – metoda rozšíření `HttpRequestException`, odpovědi HTTP 5xx a odpovědi HTTP 408.

`AddTransientHttpErrorPolicy` Rozšíření může být použito v rámci `Startup.ConfigureServices`. Toto rozšíření poskytuje přístup k `PolicyBuilder` objekt nakonfigurovaný pro zpracování chyb představující možných přechodných chyb:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

V předchozím kódu `WaitAndRetryAsync` definované zásady. Neúspěšné požadavky se zopakují až třikrát s zpožděním mezi pokusy o 600 ms.

### <a name="dynamically-select-policies"></a>Dynamicky vybrat zásady

Další rozšiřující metody existují, které lze přidat na základě Polly obslužné rutiny. Je jedno takové rozšíření `AddPolicyHandler`, který má několik přetížení. Jedním přetížením mu umožní ho možné zkontrolovat při definování zásad, které chcete použít:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

V předchozím kódu pokud je odchozí požadavek GET, časový limit 10 sekundu se použije. Pro jiné metody HTTP se používá s časovým limitem 30 sekund.

### <a name="add-multiple-polly-handlers"></a>Přidávání více obslužných rutin Polly

Je běžné vnořit Polly zásady, které poskytují vylepšené funkce:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

V předchozím příkladu jsou přidány dvě obslužné rutiny. První použití `AddTransientHttpErrorPolicy` rozšíření přidat zásady opakování. Neúspěšné požadavky se zopakují až třikrát. Druhé volání `AddTransientHttpErrorPolicy` přidá zásadu jističe. Další externí požadavky jsou blokovány 30 sekund, pokud postupně dojde k 5 neúspěšných pokusů o přihlášení. Jistič zásady jsou stavová. Všechna volání prostřednictvím tohoto klienta sdílet stejný stav okruhu.

### <a name="add-policies-from-the-polly-registry"></a>Přidání zásad z registru Polly

Přístup pro správu pravidelně použité zásady, je jejich jednou definovat a registrovat pomocí `PolicyRegistry`. Metody rozšíření je k dispozici tomu, aby obslužná rutina přidat pomocí zásad z registru:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

V předchozí kód přidá PolicyRegistry se `ServiceCollection` a dvě zásady, které jsou v něm zaregistrovaný. Chcete-li použít zásady z registru, `AddPolicyHandlerFromRegistry` metoda se používá, předejte název zásady začaly platit.

Další informace o `IHttpClientFactory` a integrace Polly můžete najít na [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>HttpClient a životního cyklu správy

Pokaždé, když `CreateClient` je volán na `IHttpClientFactory`, novou instanci třídy `HttpClient` je vrácena. Bude existovat `HttpMessageHandler` jednoho klienta s názvem. `IHttpClientFactory` bude fond `HttpMessageHandler` instancí, které jsou vytvořeny procesem ke snížení spotřeby prostředků. A `HttpMessageHandler` instance může být znovu použít z fondu při vytváření nového `HttpClient` instance Pokud nevypršela platnost svého životního cyklu. 

Sdružování obslužných rutin je žádoucí, protože každá obslužná rutina obvykle spravuje svou vlastní základní připojení protokolu HTTP; vytváření více obslužných rutin, než je nezbytné, může způsobit zpoždění připojení. Některé obslužné rutiny také zachovat připojení otevřené po neomezenou dobu, což může zabránit obslužnou rutinu reakce na změny DNS.

Výchozí doba života obslužná rutina je dvě minuty. Výchozí hodnota se dá přepsat na základě pojmenované klienta. Chcete-li přepsat, zavolejte `SetHandlerLifetime` na `IHttpClientBuilder` , který je vrácen při vytváření klienta:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a>protokolování

Klienty vytvořené prostřednictvím `IHttpClientFactory` záznam protokolu zpráv pro všechny požadavky. Budete muset povolit úroveň příslušné informace v konfiguraci protokolování tak, aby se zobrazit výchozí zprávy protokolu. Dodatečné protokolování, jako je například protokolování hlavičky požadavku je zahrnuta pouze na úroveň trasování.

Kategorie protokolu používá pro každého klienta obsahuje název klienta. Klient s názvem "MyNamedClient", například zprávy protokolu s kategorii `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. Na povrchu kanál obslužná rutina požadavku dojde k zprávy s příponou "LogicalHandler". V požadavku jsou zprávy zaznamenány předtím, než ji zpracovali ostatních obslužných rutin v kanálu. V odpovědi jsou zprávy zaznamenány po všech ostatních obslužných rutin kanálu mají přijetí odpovědi.

Také dochází k protokolování uvnitř kanál obslužná rutina požadavku. V případě příklad "MyNamedClient" tyto zprávy jsou zaznamenané v souvislosti s kategorie protokolu `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. Pro žádost k tomu dochází po spuštění ostatních obslužných rutin a bezprostředně před odesláním žádosti v síti. V odpovědi protokolování zahrnuje stav odpovědi před odesláním zpět prostřednictvím kanálu obslužné rutiny.

Povolení protokolování na povrchu a v rámci kanálu umožňuje kontrolu změny provedené ostatních obslužných rutin v kanálu. To může zahrnovat změny hlavičky, požadavku, například nebo stavový kód odpovědi.

Včetně názvu klienta v kategorii protokolů umožňuje filtrování konkrétních klientů s názvem v případě potřeby protokolování.

## <a name="configure-the-httpmessagehandler"></a>Objekt HttpMessageHandler konfigurace

Může být nutné k řízení konfigurace vnitřního `HttpMessageHandler` používaný klientem.

`IHttpClientBuilder` Dochází při přidávání s názvem nebo typy klientů. `ConfigurePrimaryHttpMessageHandler` Metody rozšíření lze použít k definování delegáta. Delegát se používá k vytvoření a konfigurace primární `HttpMessageHandler` používané tohoto klienta:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
