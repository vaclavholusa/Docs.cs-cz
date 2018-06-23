---
title: Inicializace požadavků HTTP
author: stevejgordon
description: Další informace o použití rozhraní IHttpClientFactory ke správě logické HttpClient instance v ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/22/2018
uid: fundamentals/http-requests
ms.openlocfilehash: e56c7a3ed80cc08103f6178859a1a99f1a5ec068
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327519"
---
# <a name="initiate-http-requests"></a>Inicializace požadavků HTTP

Podle [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), a [Steve Gordon](https://github.com/stevejgordon)

[IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) můžete zaregistrovat a použít ke konfiguraci a vytvořit [HttpClient](/dotnet/api/system.net.http.httpclient) instancí v aplikaci. Nabízí následující výhody:

* Poskytuje centrální umístění pro pojmenovávání a konfiguraci logické `HttpClient` instance. Například "githubu" klienta můžete zaregistrovat a nakonfigurovaná pro přístup k webu GitHub. Výchozí klienta lze zaregistrovat pro jiné účely.
* Codifies koncept odchozí middleware prostřednictvím delegování obslužné rutiny v `HttpClient` a poskytuje rozšíření pro middleware na základě Polly využít této.
* Spravuje sdružování a dobu života základní `HttpClientMessageHandler` instancí, aby se zabránilo běžné problémy DNS, ke kterým dochází při správě ručně `HttpClient` životnosti.
* Přidá prostředí konfigurovat protokolování (prostřednictvím `ILogger`) pro všechny požadavky, které se budou odesílat prostřednictvím klienti vytvořený objekt pro vytváření.

## <a name="consumption-patterns"></a>Vzory spotřeba

Existuje několik způsobů `IHttpClientFactory` slouží v aplikaci:

* [Základní informace o využití](#basic-usage)
* [Klienti s názvem](#named-clients)
* [Typové klientů](#typed-clients)
* [Vygenerovaný klientů](#generated-clients)

Žádný z nich jsou výhradně hodnotu větší než jiné. Nejlepší metodou závisí na omezení aplikace.

### <a name="basic-usage"></a>Základní informace o využití

`IHttpClientFactory` Lze registrovat pomocí volání `AddHttpClient` rozšiřující metody na `IServiceCollection`uvnitř `Startup.ConfigureServices` metoda.

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

Po registraci můžete přijmout kódu `IHttpClientFactory` kdekoli může vložit služby s [vkládání závislostí](xref:fundamentals/dependency-injection) (DI). `IHttpClientFactory` Slouží k vytvoření `HttpClient` instance:

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

Pomocí `IHttpClientFactory` tímto způsobem je skvělým způsobem, jak Refaktorovat stávající aplikace. Nemá žádný vliv na způsob `HttpClient` se používá. V místech, kde `HttpClient` aktuálně vytváření instancí, nahraďte výskyty volání `CreateClient`.

### <a name="named-clients"></a>Klienti s názvem

Pokud aplikace vyžaduje použití více jedinečných `HttpClient`, každý s jinou konfiguraci, je možnost použít **s názvem klienti**. Konfigurace pro pojmenovaná `HttpClient` lze zadat během registrace v `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

V předchozí kód `AddHttpClient` je volána, poskytuje název "githubu". Tento klient má některé výchozí konfigurace použít&mdash;konkrétně základní adresu a dvě hlavičky, které jsou potřebné pro práci s rozhraním API pro GitHub.

Pokaždé, když `CreateClient` nazývá novou instanci třídy `HttpClient` vytvoření a konfigurace akce je volána.

Používat s názvem klient, může být předán parametr řetězce `CreateClient`. Zadejte název klienta, který se má vytvořit:

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

V předchozím kódu nemusí požadavek zadejte název hostitele. Vzhledem k tomu, že se používá základní adresu konfigurovanou pro klienta, pak může předat jenom cesty.

### <a name="typed-clients"></a>Typové klientů

Typové klienty poskytují stejné schopnosti jako s názvem klienty, aniž by bylo nutné použít řetězce jako klíče. Typový klient přístup poskytuje funkce IntelliSense a kompilátoru pomoci při využívání klientů. Poskytuje jedno centrální umístění pro konfiguraci a interakci s konkrétní `HttpClient`. Například konkrétního typu klienta může být použit pro koncový bod jeden back-end a zapouzdřují všechny logiku práci s tohoto koncového bodu. Další výhodou je, že pracovat s DI a může vložit, pokud to vyžaduje ve vaší aplikaci.

Typový klient přijme `HttpClient` parametr v jeho konstruktoru:

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

V předchozí kód konfigurace přesunutí typový klient. `HttpClient` Objektu je zpřístupněná jako veřejné vlastnosti. Je možné definovat metody specifické pro rozhraní API, které zveřejňují `HttpClient` funkce. `GetAspNetDocsIssues` Metoda zapouzdřuje kód potřebný k dotazu a analyzovat na nejnovější otevřené problémy z úložiště Githubu.

K registraci typový klient, Obecné `AddHttpClient` rozšíření metodu je možné použít v rámci `Startup.ConfigureServices`, specifikace typový klient třídy:

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

Typový klient je zaregistrován jako přechodný s DI. Typový klient můžete vložit a využívat přímo:

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Pokud dáváte přednost, konfigurace pro typový klient lze zadat během registrace v `Startup.ConfigureServices`, a nikoli v konstruktoru typový klient:

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

Je možné zcela zapouzdření `HttpClient` v rámci typový klient. Místo vystavení ji jako vlastnost, lze veřejné metody zadat které volání `HttpClient` instance interně.

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

V předchozí kód `HttpClient` se ukládají jako soukromé pole. Veškerý přístup k externí volání prochází `GetRepos` metoda.

### <a name="generated-clients"></a>Vygenerovaný klientů

`IHttpClientFactory` lze použít v kombinaci s další knihovny třetích stran, jako [provedení nového](https://github.com/paulcbetts/refit). Refit je REST knihovna pro .NET. Rozhraní REST API převede do provozu rozhraní. Implementace rozhraní je generována dynamicky pomocí `RestService`pomocí `HttpClient` aby externí HTTP volání.

K reprezentaci externí rozhraní API a odpovědi jsou definovány rozhraní a odpověď:

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

Typový klient lze přidat, pomocí Refit ke generování implementace:

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

Rozhraní definované mohou být využívány v případě potřeby s implementací poskytované DI a Refit:

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

## <a name="outgoing-request-middleware"></a>Odchozí middleware požadavku

`HttpClient` již obsahuje koncept delegování obslužných rutin, které může být propojený pro odchozí požadavky HTTP. `IHttpClientFactory` Usnadňuje definovat obslužné rutiny pro použití u jednotlivých klientů s názvem. Podporuje registraci a řetězení více obslužných rutin k sestavení odchozí požadavku kanálu middleware. Každá z těchto obslužné rutiny je moct práci před a po dokončení žádosti o odchozí. Tento vzor je podobná příchozí middlewaru v řadě v ASP.NET Core. Vzor poskytuje mechanismus ke správě mezi vyjímání otázky ohledně požadavků HTTP, včetně ukládání do mezipaměti, chyba zpracování serializace a protokolování.

Chcete-li vytvořit obslužnou rutinu, definovat třídu odvozování z `DelegatingHandler`. Přepsání `SendAsync` metoda ke spouštění kódu před předáním požadavku na další obslužnou rutinu v kanálu:

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Předchozí kód definuje základní obslužná rutina. Zkontroluje, pokud byl zahrnut hlavičku X-API-KEY v žádosti. Pokud chybí záhlaví, se vyhněte se volání protokolu HTTP a vrátit vhodný odpověď.

Během registrace, lze přidat jeden nebo více obslužné rutiny do konfigurace `HttpClient`. Tato úloha se provádí prostřednictvím metody rozšíření na `IHttpClientBuilder`.

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

V předchozí kód `ValidateHeaderHandler` není zaregistrována DI. Obslužná rutina **musí** zaregistrovat v DI jako přechodný. Po registraci `AddHttpMessageHandler` lze volat, probíhá předání v typu obslužná rutina.

Může být registrováno více obslužných rutin v pořadí, ve kterém se má spustit. Každý obslužné rutiny obslužná rutina další zabalí až do konečné `HttpClientHandler` zpracuje požadavek:

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a>Použít na základě Polly obslužné rutiny

`IHttpClientFactory` integruje pomocí Oblíbené knihovny třetích stran názvem [Polly](https://github.com/App-vNext/Polly). Polly je komplexní odolnost a přechodné chyby zpracování knihovna pro .NET. To umožňuje vývojářům express zásady, třeba opakování, jistič, vypršení časového limitu, přepážkovou izolace a záložního fluent a bezpečným způsobem.

Metody rozšíření poskytované umožnit použití zásad Polly s nakonfigurované `HttpClient` instance. Jsou k dispozici v rozšíření Polly [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) balíček NuGet. Není součástí tohoto balíčku [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app). Chcete-li používat rozšíření, explicitního `<PackageReference />` by měl být zahrnutý v projektu.

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

Po obnovení tohoto balíčku, jsou k dispozici pro podporu přidání obslužné rutiny na základě Polly klientům rozšiřující metody.

### <a name="handle-transient-faults"></a>Zpracování přechodných

Většina běžných chyb, které může nastat, když externí volání protokolu HTTP bude přechodný. Volána metoda vhodné rozšíření `AddTransientHttpErrorPolicy` je zahrnuta, což umožňuje definovat za účelem zpracování přechodné chyby zásad. Zásady nakonfigurované s tímto popisovačem – metoda rozšíření `HttpRequestException`, odpovědi HTTP 5xx a odpovědi protokolu HTTP 408.

`AddTransientHttpErrorPolicy` Rozšíření lze použít v rámci `Startup.ConfigureServices`. Poskytuje přístup k rozšíření `PolicyBuilder` objekt pro zpracování chyby představující možné přechodná chyba nakonfigurována:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

V předchozí kód `WaitAndRetryAsync` definované zásady. Neúspěšné požadavky jsou opakována až třikrát s zpožděním mezi pokusy o 600 ms.

### <a name="dynamically-select-policies"></a>Dynamicky vyberte zásady

Další rozšíření metody existují, které můžete použít k přidání obslužné rutiny na základě Polly. Jedno takové rozšíření je `AddPolicyHandler`, který má několik přetížení. Jedním přetížením umožňuje žádost o být prověřovány při definování kterou zásadu použít:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

Předchozí kód Pokud je odchozí požadavek GET, vypršení časového limitu 10 sekund, se použije. Ostatní metody protokolu HTTP se používá vypršení časového limitu 30 sekund.

### <a name="add-multiple-polly-handlers"></a>Přidávání více obslužných rutin Polly

Je běžné vnořit Polly zásady zajistit rozšířené funkce:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

V předchozím příkladu se přidají dvě obslužné rutiny. První používá `AddTransientHttpErrorPolicy` rozšíření, které chcete přidat zásady opakování. Neúspěšné požadavky jsou opakovat maximálně třikrát. Druhé volání `AddTransientHttpErrorPolicy` přidá jistič zásadu. Další externí požadavky jsou blokovány po dobu 30 sekund, pokud pět neúspěšných pokusů o přihlášení, dojde k postupně. Zásady jistič jsou stateful. Všechna volání prostřednictvím tohoto klienta sdílet stejnou stav okruhu.

### <a name="add-policies-from-the-polly-registry"></a>Přidání zásad z registru Polly

Přístup pro správu pravidelně použité zásady je třeba definovat jednou a registrovat s `PolicyRegistry`. Metody rozšíření je poskytována, což umožňuje, aby obslužná rutina přidat pomocí zásad z registru:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

V předchozí kód PolicyRegistry přidán `ServiceCollection` a dvě zásady, které jsou registrovány s ním. Chcete-li použít zásady z registru, `AddPolicyHandlerFromRegistry` metoda se používá, předávání název zásady použít.

Další informace o `IHttpClientFactory` a integrace v rámci Polly naleznete na [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>Správa HttpClient a doba platnosti

Pokaždé, když `CreateClient` se volá na `IHttpClientFactory`, novou instanci třídy `HttpClient` je vrácen. Budou existovat `HttpMessageHandler` jednotlivé klienty, s názvem. `IHttpClientFactory` bude fond `HttpMessageHandler` instance vytvořený objekt factory ke snížení spotřeby prostředků. A `HttpMessageHandler` může při vytvoření nové instance z fondu znovu použít `HttpClient` instance Pokud nevypršela platnost celé jeho životnosti. 

Sdružování obslužných rutin je žádoucí, jak každou obslužnou rutinu obvykle spravuje vlastní základní připojení protokolu HTTP; Vytvoření více obslužných rutin, než je nezbytné, může mít za následek zpoždění připojení. Některé obslužné rutiny také nechat připojení otevřené bez omezení, která zabránit, aby obslužná rutina reaguje na změny DNS.

Výchozí doba života obslužná rutina je dvě minuty. Výchozí hodnota je možné přepsat na za pojmenované klienty. Chcete-li přepsat, volejte `SetHandlerLifetime` na `IHttpClientBuilder` při vytváření klienta, je vrácena:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a>protokolování

Klienti vytvořené prostřednictvím `IHttpClientFactory` záznam protokolu zpráv pro všechny požadavky. Budete muset povolit na úrovni příslušné informace o konfiguraci protokolování zobrazíte výchozí zprávy protokolu. Další protokolování, například protokolování hlaviček požadavků, je uvedený jenom na úroveň trasování.

Kategorii protokolu se používá pro každého klienta obsahuje název klienta. Klient s názvem "MyNamedClient", například protokoly zprávy s kategorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. Zprávy s příponou "LogicalHandler", ke kterým došlo u mimo kanálu obslužná rutina požadavku. V žádosti zprávy jsou zaznamenány předtím, než ho mít zpracován ostatních obslužných rutin v kanálu. V odpovědi zprávy jsou zaznamenány po všech ostatních obslužných rutin kanálu obdrželi odpovědi.

Také dochází k protokolování uvnitř kanál obslužná rutina požadavku. V případě příkladu "MyNamedClient" tyto zprávy se zaznamenávají proti protokolu kategorie `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. Pro daný požadavek k tomu dochází po spuštění ostatních obslužných rutin a bezprostředně před odesláním požadavku v síti. V odpovědi protokolování zahrnuje stav odpovědi předtím, než se předává zpátky pomocí kanálu obslužné rutiny.

Povolení protokolování na vnější a v rámci kanálu umožní kontrolu změny provedené ostatních obslužných rutin v kanálu. To může zahrnovat změny k žádosti hlavičky, například nebo stavový kód odpovědi.

Umožňuje filtrování pro konkrétní pojmenované klienty potřeby protokolu, včetně názvu klienta v kategorii protokolu.

## <a name="configure-the-httpmessagehandler"></a>Objekt HttpMessageHandler konfigurace

Může být nezbytné k řízení konfigurace vnitřního `HttpMessageHandler` používaný klientem.

`IHttpClientBuilder` Se vrátí při přidávání s názvem nebo zadali klientů. `ConfigurePrimaryHttpMessageHandler` Metoda rozšíření lze použít k definování delegáta. Delegát slouží k vytvoření a konfigurace primární `HttpMessageHandler` používané tohoto klienta:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
