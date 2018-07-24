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
# <a name="initiate-http-requests"></a><span data-ttu-id="01fc0-103">Iniciování požadavků HTTP</span><span class="sxs-lookup"><span data-stu-id="01fc0-103">Initiate HTTP requests</span></span>

<span data-ttu-id="01fc0-104">Podle [Glenn Condron](https://github.com/glennc), [Ryanem Nowak](https://github.com/rynowak), a [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="01fc0-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="01fc0-105">[IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) můžete zaregistrované a slouží ke konfiguraci a vytvořte [HttpClient](/dotnet/api/system.net.http.httpclient) instance v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01fc0-105">An [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="01fc0-106">Nabízí následující výhody:</span><span class="sxs-lookup"><span data-stu-id="01fc0-106">It offers the following benefits:</span></span>

* <span data-ttu-id="01fc0-107">Poskytuje centrální umístění pro pojmenovávání a konfiguraci logické `HttpClient` instancí.</span><span class="sxs-lookup"><span data-stu-id="01fc0-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="01fc0-108">Například "githubu" klienta můžete zaregistrované a nakonfigurovat tak, aby ke Githubu přistupovat.</span><span class="sxs-lookup"><span data-stu-id="01fc0-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="01fc0-109">Výchozí klienta lze zaregistrovat k jiným účelům.</span><span class="sxs-lookup"><span data-stu-id="01fc0-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="01fc0-110">Kodifikuje koncept odchozí middleware prostřednictvím delegování obslužné rutiny ve `HttpClient` a rozšíření pro middleware založený na Polly výhod, které poskytuje.</span><span class="sxs-lookup"><span data-stu-id="01fc0-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="01fc0-111">Spravuje sdružování a dobu života základní `HttpClientMessageHandler` instancí se vyhnout běžným potížím DNS, ke kterým dochází při správě ručně `HttpClient` životnosti.</span><span class="sxs-lookup"><span data-stu-id="01fc0-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="01fc0-112">Přidá prostředí konfigurovat protokolování (prostřednictvím `ILogger`) pro všechny požadavky odeslané prostřednictvím klientů, které jsou vytvořeny procesem.</span><span class="sxs-lookup"><span data-stu-id="01fc0-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01fc0-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01fc0-113">Prerequisites</span></span>

<span data-ttu-id="01fc0-114">Projekty cílené na rozhraní .NET Framework vyžadují instalaci [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="01fc0-114">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="01fc0-115">Projekty, které cílí na .NET Core a odkaz [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) již patří `Microsoft.Extensions.Http` balíčku.</span><span class="sxs-lookup"><span data-stu-id="01fc0-115">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="01fc0-116">Vzory využití</span><span class="sxs-lookup"><span data-stu-id="01fc0-116">Consumption patterns</span></span>

<span data-ttu-id="01fc0-117">Existuje několik způsobů `IHttpClientFactory` lze použít v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="01fc0-117">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="01fc0-118">Základní informace o využití</span><span class="sxs-lookup"><span data-stu-id="01fc0-118">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="01fc0-119">Pojmenované klientů</span><span class="sxs-lookup"><span data-stu-id="01fc0-119">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="01fc0-120">Typy klientů</span><span class="sxs-lookup"><span data-stu-id="01fc0-120">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="01fc0-121">Vygenerovaný klientů</span><span class="sxs-lookup"><span data-stu-id="01fc0-121">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="01fc0-122">Žádná z nich není striktně vynikající do jiného.</span><span class="sxs-lookup"><span data-stu-id="01fc0-122">None of them are strictly superior to another.</span></span> <span data-ttu-id="01fc0-123">Nejlepší přístup, závisí na omezení aplikace.</span><span class="sxs-lookup"><span data-stu-id="01fc0-123">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="01fc0-124">Základní informace o využití</span><span class="sxs-lookup"><span data-stu-id="01fc0-124">Basic usage</span></span>

<span data-ttu-id="01fc0-125">`IHttpClientFactory` Lze registrovat pomocí volání `AddHttpClient` rozšiřující metody na `IServiceCollection`uvnitř `Startup.ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="01fc0-125">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="01fc0-126">Po registraci může přijmout kódu `IHttpClientFactory` kdekoli služby může být vložený se [injektáž závislostí](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="01fc0-126">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="01fc0-127">`IHttpClientFactory` Slouží k vytvoření `HttpClient` instance:</span><span class="sxs-lookup"><span data-stu-id="01fc0-127">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="01fc0-128">Pomocí `IHttpClientFactory` tímto způsobem je skvělý způsob, jak Refaktorovat stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="01fc0-128">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="01fc0-129">Nemá žádný vliv na způsob, jakým `HttpClient` se používá.</span><span class="sxs-lookup"><span data-stu-id="01fc0-129">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="01fc0-130">Na místech, kde `HttpClient` aktuálně se vytvářejí instance, nahraďte volání výskyty `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="01fc0-130">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="01fc0-131">Pojmenované klientů</span><span class="sxs-lookup"><span data-stu-id="01fc0-131">Named clients</span></span>

<span data-ttu-id="01fc0-132">Pokud aplikace vyžaduje víc různé způsoby použití `HttpClient`, každý s jinou konfiguraci, je možnost použití **s názvem klienti**.</span><span class="sxs-lookup"><span data-stu-id="01fc0-132">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="01fc0-133">Konfigurace pro pojmenovaná `HttpClient` se dá nastavit během registrace v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="01fc0-133">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="01fc0-134">V předchozím kódu `AddHttpClient` je volána, poskytuje název "githubu".</span><span class="sxs-lookup"><span data-stu-id="01fc0-134">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="01fc0-135">Některé výchozí konfigurace má tento klient&mdash;totiž základní adrese a dvě záhlaví vyžadována pro práci s rozhraním API pro GitHub.</span><span class="sxs-lookup"><span data-stu-id="01fc0-135">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="01fc0-136">Pokaždé, když `CreateClient` nazývá novou instanci třídy `HttpClient` vytvoření a konfigurace akce je volána.</span><span class="sxs-lookup"><span data-stu-id="01fc0-136">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="01fc0-137">Využívat pojmenované klienta, může být předán parametr řetězce `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="01fc0-137">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="01fc0-138">Zadejte název klienta, který se má vytvořit:</span><span class="sxs-lookup"><span data-stu-id="01fc0-138">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="01fc0-139">V předchozím kódu žádost nemusí zadejte název hostitele.</span><span class="sxs-lookup"><span data-stu-id="01fc0-139">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="01fc0-140">Ho můžete předat právě cestu, protože se používá základní adresu nakonfigurovanou pro klienta.</span><span class="sxs-lookup"><span data-stu-id="01fc0-140">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="01fc0-141">Typy klientů</span><span class="sxs-lookup"><span data-stu-id="01fc0-141">Typed clients</span></span>

<span data-ttu-id="01fc0-142">Typy klientů poskytují stejné funkce jako pojmenované klientů bez nutnosti použití řetězců jako klíče.</span><span class="sxs-lookup"><span data-stu-id="01fc0-142">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="01fc0-143">Typový klient přístup poskytuje IntelliSense a kompilátoru pomoct při využívání klientů.</span><span class="sxs-lookup"><span data-stu-id="01fc0-143">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="01fc0-144">Poskytuje jedno centrální umístění pro konfiguraci a interakci s konkrétní `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="01fc0-144">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="01fc0-145">Například konkrétního typu klienta může být použit pro koncový bod jediného back-endu a zapouzdření všech řešení logiku tohoto koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="01fc0-145">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="01fc0-146">Další výhodou je, že práce s DI a můžete vloženy vyžadováno ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01fc0-146">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="01fc0-147">Typový klient přijme `HttpClient` parametr v konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="01fc0-147">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="01fc0-148">V předchozím kódu konfigurace přesunout do typový klient.</span><span class="sxs-lookup"><span data-stu-id="01fc0-148">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="01fc0-149">`HttpClient` Objektu je vystaven jako veřejná vlastnost.</span><span class="sxs-lookup"><span data-stu-id="01fc0-149">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="01fc0-150">Je možné definovat metody specifické pro rozhraní API, které vystavují `HttpClient` funkce.</span><span class="sxs-lookup"><span data-stu-id="01fc0-150">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="01fc0-151">`GetAspNetDocsIssues` Metoda zapouzdřuje kód potřebný k vyhledání a parsování nejnovější otevřené problémy z úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="01fc0-151">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="01fc0-152">K registraci typový klient Obecné `AddHttpClient` metody rozšíření lze použít v `Startup.ConfigureServices`, určení typový klient třídy:</span><span class="sxs-lookup"><span data-stu-id="01fc0-152">To register a typed client, the generic `AddHttpClient` extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="01fc0-153">Typový klient je zaregistrovaný jako přechodné s DI.</span><span class="sxs-lookup"><span data-stu-id="01fc0-153">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="01fc0-154">Typový klient může vloží a využívat přímo:</span><span class="sxs-lookup"><span data-stu-id="01fc0-154">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="01fc0-155">Pokud tomu dávají přednost, konfigurace pro typový klient se dá nastavit během registrace v `Startup.ConfigureServices`, spíše než v konstruktoru typu klienta:</span><span class="sxs-lookup"><span data-stu-id="01fc0-155">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="01fc0-156">Je možné zcela zapouzdření `HttpClient` v rámci typu klienta.</span><span class="sxs-lookup"><span data-stu-id="01fc0-156">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="01fc0-157">Místo bude vystavená jako vlastnost, je možné veřejné metody poskytnou která volá `HttpClient` instance interně.</span><span class="sxs-lookup"><span data-stu-id="01fc0-157">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="01fc0-158">V předchozím kódu `HttpClient` se ukládá jako soukromé pole.</span><span class="sxs-lookup"><span data-stu-id="01fc0-158">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="01fc0-159">Veškerý přístup pro volání externích prochází `GetRepos` metody.</span><span class="sxs-lookup"><span data-stu-id="01fc0-159">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="01fc0-160">Vygenerovaný klientů</span><span class="sxs-lookup"><span data-stu-id="01fc0-160">Generated clients</span></span>

<span data-ttu-id="01fc0-161">`IHttpClientFactory` můžete použít v kombinaci s dalšími knihovnami třetích stran, jako [provedení nového](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="01fc0-161">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="01fc0-162">Refit je REST knihovna pro .NET.</span><span class="sxs-lookup"><span data-stu-id="01fc0-162">Refit is a REST library for .NET.</span></span> <span data-ttu-id="01fc0-163">Rozhraní REST API se převede na živé interfaces.</span><span class="sxs-lookup"><span data-stu-id="01fc0-163">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="01fc0-164">Implementace rozhraní se vygeneruje dynamicky pomocí `RestService`pomocí `HttpClient` aby externí HTTP volání.</span><span class="sxs-lookup"><span data-stu-id="01fc0-164">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="01fc0-165">Rozhraní a odpovědi jsou definovány pro reprezentaci externí rozhraní API a odpověď:</span><span class="sxs-lookup"><span data-stu-id="01fc0-165">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="01fc0-166">Typový klient můžete přidat, pomocí Refit generovat implementace:</span><span class="sxs-lookup"><span data-stu-id="01fc0-166">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="01fc0-167">Definované rozhraní můžete využívat v případě potřeby s implementací poskytované DI a Refit:</span><span class="sxs-lookup"><span data-stu-id="01fc0-167">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="01fc0-168">Odchozí žádosti o middlewaru</span><span class="sxs-lookup"><span data-stu-id="01fc0-168">Outgoing request middleware</span></span>

<span data-ttu-id="01fc0-169">`HttpClient` již obsahuje koncept delegování obslužné rutiny, které může být propojený pro odchozí požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="01fc0-169">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="01fc0-170">`IHttpClientFactory` Usnadňuje k definování obslužných rutin mají použít u každého klienta s názvem.</span><span class="sxs-lookup"><span data-stu-id="01fc0-170">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="01fc0-171">Podporuje registraci a zřetězení více obslužných rutin k sestavení kanál middleware odchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="01fc0-171">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="01fc0-172">Každá z těchto obslužných rutin je moci provádět úkoly před a za odchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="01fc0-172">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="01fc0-173">Tento model je podobný kanál příchozí middlewaru v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01fc0-173">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="01fc0-174">Vzor poskytuje mechanismus ke správě vyskytující aspekty kolem požadavků protokolu HTTP, včetně ukládání do mezipaměti, zpracování chyb, serializaci a protokolování.</span><span class="sxs-lookup"><span data-stu-id="01fc0-174">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="01fc0-175">Chcete-li vytvořit obslužnou rutinu, definujte třídu odvozenou z `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="01fc0-175">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="01fc0-176">Přepsat `SendAsync` metoda spuštění kódu před předáním požadavku další obslužná rutina kanálu:</span><span class="sxs-lookup"><span data-stu-id="01fc0-176">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="01fc0-177">Předcházející kód definuje obslužnou rutinu základní.</span><span class="sxs-lookup"><span data-stu-id="01fc0-177">The preceding code defines a basic handler.</span></span> <span data-ttu-id="01fc0-178">Zkontroluje, pokud u požadavku byla zahrnuta hlavičku X-API-KEY.</span><span class="sxs-lookup"><span data-stu-id="01fc0-178">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="01fc0-179">Pokud chybí záhlaví, můžete vyhnout volání HTTP a vrátí odpověď vhodný.</span><span class="sxs-lookup"><span data-stu-id="01fc0-179">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="01fc0-180">Během registrace, lze přidat jeden nebo více obslužných rutin do konfigurace `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="01fc0-180">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="01fc0-181">Tato úloha se provádí prostřednictvím metody rozšíření na `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="01fc0-181">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="01fc0-182">V předchozím kódu `ValidateHeaderHandler` DI zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="01fc0-182">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="01fc0-183">Obslužná rutina **musí** zaregistrovat v DI jako přechodné.</span><span class="sxs-lookup"><span data-stu-id="01fc0-183">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="01fc0-184">Po registraci `AddHttpMessageHandler` může být volána, předejte typ pro obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="01fc0-184">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="01fc0-185">Může být registrováno více obslužných rutin v pořadí, ve kterém by se měl spustit.</span><span class="sxs-lookup"><span data-stu-id="01fc0-185">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="01fc0-186">Každý popisovač zabalí další obslužná rutina až do konečné `HttpClientHandler` zpracuje požadavek:</span><span class="sxs-lookup"><span data-stu-id="01fc0-186">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="01fc0-187">Použít na základě Polly obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="01fc0-187">Use Polly-based handlers</span></span>

<span data-ttu-id="01fc0-188">`IHttpClientFactory` se integruje s oblíbenými knihovnu třetí strany s názvem [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="01fc0-188">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="01fc0-189">Polly je komplexní odolnosti a přechodné zpracování chyb library pro .NET.</span><span class="sxs-lookup"><span data-stu-id="01fc0-189">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="01fc0-190">Umožňuje vývojářům vyjádřit zásady například opakování, jističe, vypršení časového limitu, přepážka izolace a záložních fluent a bezpečným způsobem.</span><span class="sxs-lookup"><span data-stu-id="01fc0-190">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="01fc0-191">Metody rozšíření jsou k dispozici pro povolení použití zásad Polly nakonfigurované `HttpClient` instancí.</span><span class="sxs-lookup"><span data-stu-id="01fc0-191">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="01fc0-192">Jsou k dispozici v rozšíření Polly [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="01fc0-192">The Polly extensions are available in the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="01fc0-193">Není součástí tohoto balíčku [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="01fc0-193">This package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="01fc0-194">Abyste použili rozšíření explicitní `<PackageReference />` by měl být zahrnutý v projektu.</span><span class="sxs-lookup"><span data-stu-id="01fc0-194">To use the extensions, an explicit `<PackageReference />` should be included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="01fc0-195">Po obnovení tohoto balíčku, rozšiřující metody jsou k dispozici pro podporu přidání obslužné rutiny na základě Polly klientům.</span><span class="sxs-lookup"><span data-stu-id="01fc0-195">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="01fc0-196">Zpracování přechodných chyb</span><span class="sxs-lookup"><span data-stu-id="01fc0-196">Handle transient faults</span></span>

<span data-ttu-id="01fc0-197">Nejběžnější chybami, na které můžete očekávat při volání HTTP externí bude přechodné.</span><span class="sxs-lookup"><span data-stu-id="01fc0-197">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="01fc0-198">Pohodlné rozšíření metoda volána `AddTransientHttpErrorPolicy` je zahrnuté umožňuje zásadu, která definované pro zpracování přechodných chyb.</span><span class="sxs-lookup"><span data-stu-id="01fc0-198">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="01fc0-199">Zásady konfigurované pomocí tohoto úchytu – metoda rozšíření `HttpRequestException`, odpovědi HTTP 5xx a odpovědi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="01fc0-199">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="01fc0-200">`AddTransientHttpErrorPolicy` Rozšíření může být použito v rámci `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="01fc0-200">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="01fc0-201">Toto rozšíření poskytuje přístup k `PolicyBuilder` objekt nakonfigurovaný pro zpracování chyb představující možných přechodných chyb:</span><span class="sxs-lookup"><span data-stu-id="01fc0-201">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="01fc0-202">V předchozím kódu `WaitAndRetryAsync` definované zásady.</span><span class="sxs-lookup"><span data-stu-id="01fc0-202">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="01fc0-203">Neúspěšné požadavky se zopakují až třikrát s zpožděním mezi pokusy o 600 ms.</span><span class="sxs-lookup"><span data-stu-id="01fc0-203">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="01fc0-204">Dynamicky vybrat zásady</span><span class="sxs-lookup"><span data-stu-id="01fc0-204">Dynamically select policies</span></span>

<span data-ttu-id="01fc0-205">Další rozšiřující metody existují, které lze přidat na základě Polly obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="01fc0-205">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="01fc0-206">Je jedno takové rozšíření `AddPolicyHandler`, který má několik přetížení.</span><span class="sxs-lookup"><span data-stu-id="01fc0-206">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="01fc0-207">Jedním přetížením mu umožní ho možné zkontrolovat při definování zásad, které chcete použít:</span><span class="sxs-lookup"><span data-stu-id="01fc0-207">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="01fc0-208">V předchozím kódu pokud je odchozí požadavek GET, časový limit 10 sekundu se použije.</span><span class="sxs-lookup"><span data-stu-id="01fc0-208">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="01fc0-209">Pro jiné metody HTTP se používá s časovým limitem 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="01fc0-209">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="01fc0-210">Přidávání více obslužných rutin Polly</span><span class="sxs-lookup"><span data-stu-id="01fc0-210">Add multiple Polly handlers</span></span>

<span data-ttu-id="01fc0-211">Je běžné vnořit Polly zásady, které poskytují vylepšené funkce:</span><span class="sxs-lookup"><span data-stu-id="01fc0-211">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="01fc0-212">V předchozím příkladu jsou přidány dvě obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="01fc0-212">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="01fc0-213">První použití `AddTransientHttpErrorPolicy` rozšíření přidat zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="01fc0-213">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="01fc0-214">Neúspěšné požadavky se zopakují až třikrát.</span><span class="sxs-lookup"><span data-stu-id="01fc0-214">Failed requests are retried up to three times.</span></span> <span data-ttu-id="01fc0-215">Druhé volání `AddTransientHttpErrorPolicy` přidá zásadu jističe.</span><span class="sxs-lookup"><span data-stu-id="01fc0-215">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="01fc0-216">Další externí požadavky jsou blokovány 30 sekund, pokud postupně dojde k 5 neúspěšných pokusů o přihlášení.</span><span class="sxs-lookup"><span data-stu-id="01fc0-216">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="01fc0-217">Jistič zásady jsou stavová.</span><span class="sxs-lookup"><span data-stu-id="01fc0-217">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="01fc0-218">Všechna volání prostřednictvím tohoto klienta sdílet stejný stav okruhu.</span><span class="sxs-lookup"><span data-stu-id="01fc0-218">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="01fc0-219">Přidání zásad z registru Polly</span><span class="sxs-lookup"><span data-stu-id="01fc0-219">Add policies from the Polly registry</span></span>

<span data-ttu-id="01fc0-220">Přístup pro správu pravidelně použité zásady, je jejich jednou definovat a registrovat pomocí `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="01fc0-220">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="01fc0-221">Metody rozšíření je k dispozici tomu, aby obslužná rutina přidat pomocí zásad z registru:</span><span class="sxs-lookup"><span data-stu-id="01fc0-221">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="01fc0-222">V předchozí kód přidá PolicyRegistry se `ServiceCollection` a dvě zásady, které jsou v něm zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="01fc0-222">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="01fc0-223">Chcete-li použít zásady z registru, `AddPolicyHandlerFromRegistry` metoda se používá, předejte název zásady začaly platit.</span><span class="sxs-lookup"><span data-stu-id="01fc0-223">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="01fc0-224">Další informace o `IHttpClientFactory` a integrace Polly můžete najít na [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="01fc0-224">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="01fc0-225">HttpClient a životního cyklu správy</span><span class="sxs-lookup"><span data-stu-id="01fc0-225">HttpClient and lifetime management</span></span>

<span data-ttu-id="01fc0-226">Pokaždé, když `CreateClient` je volán na `IHttpClientFactory`, novou instanci třídy `HttpClient` je vrácena.</span><span class="sxs-lookup"><span data-stu-id="01fc0-226">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="01fc0-227">Bude existovat `HttpMessageHandler` jednoho klienta s názvem.</span><span class="sxs-lookup"><span data-stu-id="01fc0-227">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="01fc0-228">`IHttpClientFactory` bude fond `HttpMessageHandler` instancí, které jsou vytvořeny procesem ke snížení spotřeby prostředků.</span><span class="sxs-lookup"><span data-stu-id="01fc0-228">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="01fc0-229">A `HttpMessageHandler` instance může být znovu použít z fondu při vytváření nového `HttpClient` instance Pokud nevypršela platnost svého životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="01fc0-229">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="01fc0-230">Sdružování obslužných rutin je žádoucí, protože každá obslužná rutina obvykle spravuje svou vlastní základní připojení protokolu HTTP; vytváření více obslužných rutin, než je nezbytné, může způsobit zpoždění připojení.</span><span class="sxs-lookup"><span data-stu-id="01fc0-230">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="01fc0-231">Některé obslužné rutiny také zachovat připojení otevřené po neomezenou dobu, což může zabránit obslužnou rutinu reakce na změny DNS.</span><span class="sxs-lookup"><span data-stu-id="01fc0-231">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="01fc0-232">Výchozí doba života obslužná rutina je dvě minuty.</span><span class="sxs-lookup"><span data-stu-id="01fc0-232">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="01fc0-233">Výchozí hodnota se dá přepsat na základě pojmenované klienta.</span><span class="sxs-lookup"><span data-stu-id="01fc0-233">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="01fc0-234">Chcete-li přepsat, zavolejte `SetHandlerLifetime` na `IHttpClientBuilder` , který je vrácen při vytváření klienta:</span><span class="sxs-lookup"><span data-stu-id="01fc0-234">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="01fc0-235">protokolování</span><span class="sxs-lookup"><span data-stu-id="01fc0-235">Logging</span></span>

<span data-ttu-id="01fc0-236">Klienty vytvořené prostřednictvím `IHttpClientFactory` záznam protokolu zpráv pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="01fc0-236">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="01fc0-237">Budete muset povolit úroveň příslušné informace v konfiguraci protokolování tak, aby se zobrazit výchozí zprávy protokolu.</span><span class="sxs-lookup"><span data-stu-id="01fc0-237">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="01fc0-238">Dodatečné protokolování, jako je například protokolování hlavičky požadavku je zahrnuta pouze na úroveň trasování.</span><span class="sxs-lookup"><span data-stu-id="01fc0-238">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="01fc0-239">Kategorie protokolu používá pro každého klienta obsahuje název klienta.</span><span class="sxs-lookup"><span data-stu-id="01fc0-239">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="01fc0-240">Klient s názvem "MyNamedClient", například zprávy protokolu s kategorii `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="01fc0-240">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="01fc0-241">Na povrchu kanál obslužná rutina požadavku dojde k zprávy s příponou "LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="01fc0-241">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="01fc0-242">V požadavku jsou zprávy zaznamenány předtím, než ji zpracovali ostatních obslužných rutin v kanálu.</span><span class="sxs-lookup"><span data-stu-id="01fc0-242">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="01fc0-243">V odpovědi jsou zprávy zaznamenány po všech ostatních obslužných rutin kanálu mají přijetí odpovědi.</span><span class="sxs-lookup"><span data-stu-id="01fc0-243">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="01fc0-244">Také dochází k protokolování uvnitř kanál obslužná rutina požadavku.</span><span class="sxs-lookup"><span data-stu-id="01fc0-244">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="01fc0-245">V případě příklad "MyNamedClient" tyto zprávy jsou zaznamenané v souvislosti s kategorie protokolu `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="01fc0-245">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="01fc0-246">Pro žádost k tomu dochází po spuštění ostatních obslužných rutin a bezprostředně před odesláním žádosti v síti.</span><span class="sxs-lookup"><span data-stu-id="01fc0-246">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="01fc0-247">V odpovědi protokolování zahrnuje stav odpovědi před odesláním zpět prostřednictvím kanálu obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="01fc0-247">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="01fc0-248">Povolení protokolování na povrchu a v rámci kanálu umožňuje kontrolu změny provedené ostatních obslužných rutin v kanálu.</span><span class="sxs-lookup"><span data-stu-id="01fc0-248">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="01fc0-249">To může zahrnovat změny hlavičky, požadavku, například nebo stavový kód odpovědi.</span><span class="sxs-lookup"><span data-stu-id="01fc0-249">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="01fc0-250">Včetně názvu klienta v kategorii protokolů umožňuje filtrování konkrétních klientů s názvem v případě potřeby protokolování.</span><span class="sxs-lookup"><span data-stu-id="01fc0-250">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="01fc0-251">Objekt HttpMessageHandler konfigurace</span><span class="sxs-lookup"><span data-stu-id="01fc0-251">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="01fc0-252">Může být nutné k řízení konfigurace vnitřního `HttpMessageHandler` používaný klientem.</span><span class="sxs-lookup"><span data-stu-id="01fc0-252">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="01fc0-253">`IHttpClientBuilder` Dochází při přidávání s názvem nebo typy klientů.</span><span class="sxs-lookup"><span data-stu-id="01fc0-253">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="01fc0-254">`ConfigurePrimaryHttpMessageHandler` Metody rozšíření lze použít k definování delegáta.</span><span class="sxs-lookup"><span data-stu-id="01fc0-254">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="01fc0-255">Delegát se používá k vytvoření a konfigurace primární `HttpMessageHandler` používané tohoto klienta:</span><span class="sxs-lookup"><span data-stu-id="01fc0-255">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
