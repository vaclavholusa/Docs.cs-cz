---
title: Inicializace požadavků HTTP
author: stevejgordon
description: Další informace o použití rozhraní IHttpClientFactory ke správě logické HttpClient instance v ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/http-requests
ms.openlocfilehash: 1f2c7522a10220cd9520d78846d2e897115447c2
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
ms.locfileid: "33838758"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="eae7a-103">Inicializace požadavků HTTP</span><span class="sxs-lookup"><span data-stu-id="eae7a-103">Initiate HTTP requests</span></span>

<span data-ttu-id="eae7a-104">Podle [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), a [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="eae7a-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="eae7a-105">`IHttpClientFactory` Můžete zaregistrovat a použít ke konfiguraci a vytvořit [HttpClient](/dotnet/api/system.net.http.httpclient) instancí v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eae7a-105">An `IHttpClientFactory` can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="eae7a-106">Nabízí následující výhody:</span><span class="sxs-lookup"><span data-stu-id="eae7a-106">It offers the following benefits:</span></span>

* <span data-ttu-id="eae7a-107">Poskytuje centrální umístění pro pojmenovávání a konfiguraci logické `HttpClient` instance.</span><span class="sxs-lookup"><span data-stu-id="eae7a-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="eae7a-108">Například "githubu" klienta můžete zaregistrovat a nakonfigurovaná pro přístup k webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="eae7a-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="eae7a-109">Výchozí klienta lze zaregistrovat pro jiné účely.</span><span class="sxs-lookup"><span data-stu-id="eae7a-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="eae7a-110">Codifies koncept odchozí middleware prostřednictvím delegování obslužné rutiny v `HttpClient` a poskytuje rozšíření pro middleware na základě Polly využít této.</span><span class="sxs-lookup"><span data-stu-id="eae7a-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="eae7a-111">Spravuje sdružování a dobu života základní `HttpClientMessageHandler` instancí, aby se zabránilo běžné problémy DNS, ke kterým dochází při správě ručně `HttpClient` životnosti.</span><span class="sxs-lookup"><span data-stu-id="eae7a-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="eae7a-112">Přidá prostředí konfigurovat protokolování (prostřednictvím `ILogger`) pro všechny požadavky, které se budou odesílat prostřednictvím klienti vytvořený objekt pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="eae7a-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="eae7a-113">Vzory spotřeba</span><span class="sxs-lookup"><span data-stu-id="eae7a-113">Consumption patterns</span></span>

<span data-ttu-id="eae7a-114">Existuje několik způsobů `IHttpClientFactory` slouží v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="eae7a-114">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="eae7a-115">Základní informace o využití</span><span class="sxs-lookup"><span data-stu-id="eae7a-115">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="eae7a-116">Klienti s názvem</span><span class="sxs-lookup"><span data-stu-id="eae7a-116">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="eae7a-117">Typové klientů</span><span class="sxs-lookup"><span data-stu-id="eae7a-117">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="eae7a-118">Vygenerovaný klientů</span><span class="sxs-lookup"><span data-stu-id="eae7a-118">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="eae7a-119">Žádný z nich jsou výhradně hodnotu větší než jiné.</span><span class="sxs-lookup"><span data-stu-id="eae7a-119">None of them are strictly superior to another.</span></span> <span data-ttu-id="eae7a-120">Nejlepší metodou závisí na omezení aplikace.</span><span class="sxs-lookup"><span data-stu-id="eae7a-120">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="eae7a-121">Základní informace o využití</span><span class="sxs-lookup"><span data-stu-id="eae7a-121">Basic usage</span></span>

<span data-ttu-id="eae7a-122">`IHttpClientFactory` Lze registrovat pomocí volání `AddHttpClient` rozšiřující metody na `IServiceCollection`uvnitř `ConfigureServices` metoda v souboru Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="eae7a-122">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `ConfigureServices` method in Startup.cs.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="eae7a-123">Po registraci můžete přijmout kódu `IHttpClientFactory` kdekoli může vložit služby s [vkládání závislostí](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="eae7a-123">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="eae7a-124">`IHttpClientFactory` Slouží k vytvoření `HttpClient` instance:</span><span class="sxs-lookup"><span data-stu-id="eae7a-124">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="eae7a-125">Pomocí `IHttpClientFactory` tímto způsobem je skvělým způsobem, jak Refaktorovat stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="eae7a-125">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="eae7a-126">Nemá žádný vliv na způsob `HttpClient` se používá.</span><span class="sxs-lookup"><span data-stu-id="eae7a-126">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="eae7a-127">V místech, kde `HttpClient` aktuálně vytváření instancí, nahraďte výskyty volání `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="eae7a-127">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="eae7a-128">Klienti s názvem</span><span class="sxs-lookup"><span data-stu-id="eae7a-128">Named clients</span></span>

<span data-ttu-id="eae7a-129">Pokud aplikace vyžaduje použití více jedinečných `HttpClient`, každý s jinou konfiguraci, je možnost použít **s názvem klienti**.</span><span class="sxs-lookup"><span data-stu-id="eae7a-129">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="eae7a-130">Konfigurace pro pojmenovaná `HttpClient` lze zadat během registrace v `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="eae7a-130">Configuration for a named `HttpClient` can be specified during registration in `ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="eae7a-131">V předchozí kód `AddHttpClient` je volána, poskytuje název "githubu".</span><span class="sxs-lookup"><span data-stu-id="eae7a-131">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="eae7a-132">Tento klient má některé výchozí konfigurace použít&mdash;konkrétně základní adresu a dvě hlavičky, které jsou potřebné pro práci s rozhraním API pro GitHub.</span><span class="sxs-lookup"><span data-stu-id="eae7a-132">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="eae7a-133">Pokaždé, když `CreateClient` nazývá novou instanci třídy `HttpClient` vytvoření a konfigurace akce je volána.</span><span class="sxs-lookup"><span data-stu-id="eae7a-133">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="eae7a-134">Používat s názvem klient, může být předán parametr řetězce `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="eae7a-134">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="eae7a-135">Zadejte název klienta, který se má vytvořit:</span><span class="sxs-lookup"><span data-stu-id="eae7a-135">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="eae7a-136">V předchozím kódu nemusí požadavek zadejte název hostitele.</span><span class="sxs-lookup"><span data-stu-id="eae7a-136">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="eae7a-137">Vzhledem k tomu, že se používá základní adresu konfigurovanou pro klienta, pak může předat jenom cesty.</span><span class="sxs-lookup"><span data-stu-id="eae7a-137">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="eae7a-138">Typové klientů</span><span class="sxs-lookup"><span data-stu-id="eae7a-138">Typed clients</span></span>

<span data-ttu-id="eae7a-139">Typové klienty poskytují stejné schopnosti jako s názvem klienty, aniž by bylo nutné použít řetězce jako klíče.</span><span class="sxs-lookup"><span data-stu-id="eae7a-139">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="eae7a-140">Typový klient přístup poskytuje funkce IntelliSense a kompilátoru pomoci při využívání klientů.</span><span class="sxs-lookup"><span data-stu-id="eae7a-140">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="eae7a-141">Poskytuje jedno centrální umístění pro konfiguraci a interakci s konkrétní `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="eae7a-141">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="eae7a-142">Například konkrétního typu klienta může být použit pro koncový bod jeden back-end a zapouzdřují všechny logiku práci s tohoto koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="eae7a-142">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="eae7a-143">Další výhodou je, že pracovat s DI a může vložit, pokud to vyžaduje ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eae7a-143">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="eae7a-144">Typový klient přijme `HttpClient` parametr v jeho konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="eae7a-144">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="eae7a-145">V předchozí kód konfigurace přesunutí typový klient.</span><span class="sxs-lookup"><span data-stu-id="eae7a-145">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="eae7a-146">`HttpClient` Objektu je zpřístupněná jako veřejné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="eae7a-146">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="eae7a-147">Je možné definovat metody specifické pro rozhraní API, které zveřejňují `HttpClient` funkce.</span><span class="sxs-lookup"><span data-stu-id="eae7a-147">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="eae7a-148">`GetAspNetDocsIssues` Metoda zapouzdřuje kód potřebný k dotazu a analyzovat na nejnovější otevřené problémy z úložiště Githubu.</span><span class="sxs-lookup"><span data-stu-id="eae7a-148">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="eae7a-149">K registraci typový klient, Obecné `AddHttpClient` rozšíření metodu je možné použít v rámci `ConfigureServices`, specifikace typový klient třídy:</span><span class="sxs-lookup"><span data-stu-id="eae7a-149">To register a typed client, the generic `AddHttpClient` extension method can be used within `ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="eae7a-150">Typový klient je zaregistrován jako přechodný s DI.</span><span class="sxs-lookup"><span data-stu-id="eae7a-150">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="eae7a-151">Typový klient můžete vložit a využívat přímo:</span><span class="sxs-lookup"><span data-stu-id="eae7a-151">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="eae7a-152">Pokud dáváte přednost, konfigurace pro typový klient lze zadat během registrace v `ConfigureServices`, a nikoli v konstruktoru typový klient:</span><span class="sxs-lookup"><span data-stu-id="eae7a-152">If preferred, the configuration for a typed client can be specified during registration in `ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="eae7a-153">Je možné zcela zapouzdření `HttpClient` v rámci typový klient.</span><span class="sxs-lookup"><span data-stu-id="eae7a-153">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="eae7a-154">Místo vystavení ji jako vlastnost, lze veřejné metody zadat které volání `HttpClient` instance interně.</span><span class="sxs-lookup"><span data-stu-id="eae7a-154">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="eae7a-155">V předchozí kód `HttpClient` se ukládají jako soukromé pole.</span><span class="sxs-lookup"><span data-stu-id="eae7a-155">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="eae7a-156">Veškerý přístup k externí volání prochází `GetRepos` metoda.</span><span class="sxs-lookup"><span data-stu-id="eae7a-156">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="eae7a-157">Vygenerovaný klientů</span><span class="sxs-lookup"><span data-stu-id="eae7a-157">Generated clients</span></span>

<span data-ttu-id="eae7a-158">`IHttpClientFactory` lze použít v kombinaci s další knihovny třetích stran, jako [provedení nového](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="eae7a-158">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="eae7a-159">Refit je REST knihovna pro .NET.</span><span class="sxs-lookup"><span data-stu-id="eae7a-159">Refit is a REST library for .NET.</span></span> <span data-ttu-id="eae7a-160">Rozhraní REST API převede do provozu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="eae7a-160">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="eae7a-161">Implementace rozhraní je generována dynamicky pomocí `RestService`pomocí `HttpClient` aby externí HTTP volání.</span><span class="sxs-lookup"><span data-stu-id="eae7a-161">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="eae7a-162">K reprezentaci externí rozhraní API a odpovědi jsou definovány rozhraní a odpověď:</span><span class="sxs-lookup"><span data-stu-id="eae7a-162">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="eae7a-163">Typový klient lze přidat, pomocí Refit ke generování implementace:</span><span class="sxs-lookup"><span data-stu-id="eae7a-163">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="eae7a-164">Rozhraní definované mohou být využívány v případě potřeby s implementací poskytované DI a Refit:</span><span class="sxs-lookup"><span data-stu-id="eae7a-164">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="eae7a-165">Odchozí middleware požadavku</span><span class="sxs-lookup"><span data-stu-id="eae7a-165">Outgoing request middleware</span></span>

<span data-ttu-id="eae7a-166">`HttpClient` již obsahuje koncept delegování obslužných rutin, které může být propojený pro odchozí požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="eae7a-166">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="eae7a-167">`IHttpClientFactory` Usnadňuje definovat obslužné rutiny pro použití u jednotlivých klientů s názvem.</span><span class="sxs-lookup"><span data-stu-id="eae7a-167">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="eae7a-168">Podporuje registraci a řetězení více obslužných rutin k sestavení odchozí požadavku kanálu middleware.</span><span class="sxs-lookup"><span data-stu-id="eae7a-168">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="eae7a-169">Každá z těchto obslužné rutiny je moct práci před a po dokončení žádosti o odchozí.</span><span class="sxs-lookup"><span data-stu-id="eae7a-169">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="eae7a-170">Tento vzor je podobná příchozí middlewaru v řadě v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eae7a-170">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="eae7a-171">Vzor poskytuje mechanismus ke správě mezi vyjímání otázky ohledně požadavků HTTP, včetně ukládání do mezipaměti, chyba zpracování serializace a protokolování.</span><span class="sxs-lookup"><span data-stu-id="eae7a-171">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="eae7a-172">Chcete-li vytvořit obslužnou rutinu, definovat třídu odvozování z `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="eae7a-172">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="eae7a-173">Přepsání `SendAsync` metoda ke spouštění kódu před předáním požadavku na další obslužnou rutinu v kanálu:</span><span class="sxs-lookup"><span data-stu-id="eae7a-173">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="eae7a-174">Předchozí kód definuje základní obslužná rutina.</span><span class="sxs-lookup"><span data-stu-id="eae7a-174">The preceding code defines a basic handler.</span></span> <span data-ttu-id="eae7a-175">Zkontroluje, pokud byl zahrnut hlavičku X-API-KEY v žádosti.</span><span class="sxs-lookup"><span data-stu-id="eae7a-175">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="eae7a-176">Pokud chybí záhlaví, se vyhněte se volání protokolu HTTP a vrátit vhodný odpověď.</span><span class="sxs-lookup"><span data-stu-id="eae7a-176">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="eae7a-177">Během registrace, lze přidat jeden nebo více obslužné rutiny do konfigurace `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="eae7a-177">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="eae7a-178">Tato úloha se provádí prostřednictvím metody rozšíření na `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="eae7a-178">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="eae7a-179">V předchozí kód `ValidateHeaderHandler` není zaregistrována DI.</span><span class="sxs-lookup"><span data-stu-id="eae7a-179">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="eae7a-180">Obslužná rutina **musí** zaregistrovat v DI jako přechodný.</span><span class="sxs-lookup"><span data-stu-id="eae7a-180">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="eae7a-181">Po registraci `AddHttpMessageHandler` lze volat, probíhá předání v typu obslužná rutina.</span><span class="sxs-lookup"><span data-stu-id="eae7a-181">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="eae7a-182">Může být registrováno více obslužných rutin v pořadí, ve kterém se má spustit.</span><span class="sxs-lookup"><span data-stu-id="eae7a-182">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="eae7a-183">Každý obslužné rutiny obslužná rutina další zabalí až do konečné `HttpClientHandler` zpracuje požadavek:</span><span class="sxs-lookup"><span data-stu-id="eae7a-183">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="eae7a-184">Použít na základě Polly obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="eae7a-184">Use Polly-based handlers</span></span>

<span data-ttu-id="eae7a-185">`IHttpClientFactory` integruje pomocí Oblíbené knihovny třetích stran názvem [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="eae7a-185">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="eae7a-186">Polly je komplexní odolnost a přechodné chyby zpracování knihovna pro .NET.</span><span class="sxs-lookup"><span data-stu-id="eae7a-186">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="eae7a-187">To umožňuje vývojářům express zásady, třeba opakování, jistič, vypršení časového limitu, přepážkovou izolace a záložního fluent a bezpečným způsobem.</span><span class="sxs-lookup"><span data-stu-id="eae7a-187">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="eae7a-188">Metody rozšíření poskytované umožnit použití zásad Polly s nakonfigurované `HttpClient` instance.</span><span class="sxs-lookup"><span data-stu-id="eae7a-188">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="eae7a-189">Rozšíření Polly jsou k dispozici v balíček NuGet s názvem 'Microsoft.Extensions.Http.Polly'.</span><span class="sxs-lookup"><span data-stu-id="eae7a-189">The Polly extensions are available in a NuGet package called 'Microsoft.Extensions.Http.Polly'.</span></span> <span data-ttu-id="eae7a-190">Tento balíček není zahrnut ve výchozím nastavení podle metapackage 'Microsoft.AspNetCore.App'.</span><span class="sxs-lookup"><span data-stu-id="eae7a-190">This package is not included by default by the 'Microsoft.AspNetCore.App' metapackage.</span></span> <span data-ttu-id="eae7a-191">Pokud chcete používat rozšíření, PackageReference by měl být explicitně součástí projektu.</span><span class="sxs-lookup"><span data-stu-id="eae7a-191">To use the extensions, a PackageReference should be explicitly included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="eae7a-192">Po obnovení tohoto balíčku, jsou k dispozici pro podporu přidání obslužné rutiny na základě Polly klientům rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="eae7a-192">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="eae7a-193">Zpracování přechodných</span><span class="sxs-lookup"><span data-stu-id="eae7a-193">Handle transient faults</span></span>

<span data-ttu-id="eae7a-194">Většina běžných chyb, které může nastat, když externí volání protokolu HTTP bude přechodný.</span><span class="sxs-lookup"><span data-stu-id="eae7a-194">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="eae7a-195">Volána metoda vhodné rozšíření `AddTransientHttpErrorPolicy` je zahrnuta, což umožňuje definovat za účelem zpracování přechodné chyby zásad.</span><span class="sxs-lookup"><span data-stu-id="eae7a-195">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="eae7a-196">Zásady nakonfigurované s tímto popisovačem – metoda rozšíření `HttpRequestException`, odpovědi HTTP 5xx a odpovědi protokolu HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="eae7a-196">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="eae7a-197">`AddTransientHttpErrorPolicy` Rozšíření lze použít v rámci `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="eae7a-197">The `AddTransientHttpErrorPolicy` extension can be used within `ConfigureServices`.</span></span> <span data-ttu-id="eae7a-198">Poskytuje přístup k rozšíření `PolicyBuilder` objekt pro zpracování chyby představující možné přechodná chyba nakonfigurována:</span><span class="sxs-lookup"><span data-stu-id="eae7a-198">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="eae7a-199">V předchozí kód `WaitAndRetryAsync` definované zásady.</span><span class="sxs-lookup"><span data-stu-id="eae7a-199">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="eae7a-200">Neúspěšné požadavky jsou opakována až třikrát s zpožděním mezi pokusy o 600 ms.</span><span class="sxs-lookup"><span data-stu-id="eae7a-200">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="eae7a-201">Dynamicky vyberte zásady</span><span class="sxs-lookup"><span data-stu-id="eae7a-201">Dynamically select policies</span></span>

<span data-ttu-id="eae7a-202">Další rozšíření metody existují, které můžete použít k přidání obslužné rutiny na základě Polly.</span><span class="sxs-lookup"><span data-stu-id="eae7a-202">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="eae7a-203">Jedno takové rozšíření je `AddPolicyHandler`, který má několik přetížení.</span><span class="sxs-lookup"><span data-stu-id="eae7a-203">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="eae7a-204">Jedním přetížením umožňuje žádost o být prověřovány při definování kterou zásadu použít:</span><span class="sxs-lookup"><span data-stu-id="eae7a-204">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="eae7a-205">Předchozí kód Pokud je odchozí požadavek GET, vypršení časového limitu 10 sekund, se použije.</span><span class="sxs-lookup"><span data-stu-id="eae7a-205">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="eae7a-206">Ostatní metody protokolu HTTP se používá vypršení časového limitu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="eae7a-206">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="eae7a-207">Přidávání více obslužných rutin Polly</span><span class="sxs-lookup"><span data-stu-id="eae7a-207">Add multiple Polly handlers</span></span>

<span data-ttu-id="eae7a-208">Je běžné vnořit Polly zásady zajistit rozšířené funkce:</span><span class="sxs-lookup"><span data-stu-id="eae7a-208">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="eae7a-209">V předchozím příkladu se přidají dvě obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="eae7a-209">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="eae7a-210">První používá `AddTransientHttpErrorPolicy` rozšíření, které chcete přidat zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="eae7a-210">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="eae7a-211">Neúspěšné požadavky jsou opakovat maximálně třikrát.</span><span class="sxs-lookup"><span data-stu-id="eae7a-211">Failed requests are retried up to three times.</span></span> <span data-ttu-id="eae7a-212">Druhé volání `AddTransientHttpErrorPolicy` přidá jistič zásadu.</span><span class="sxs-lookup"><span data-stu-id="eae7a-212">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="eae7a-213">Další externí požadavky jsou blokovány po dobu 30 sekund, pokud pět neúspěšných pokusů o přihlášení, dojde k postupně.</span><span class="sxs-lookup"><span data-stu-id="eae7a-213">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="eae7a-214">Zásady jistič jsou stateful.</span><span class="sxs-lookup"><span data-stu-id="eae7a-214">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="eae7a-215">Všechna volání prostřednictvím tohoto klienta sdílet stejnou stav okruhu.</span><span class="sxs-lookup"><span data-stu-id="eae7a-215">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="eae7a-216">Přidání zásad z registru Polly</span><span class="sxs-lookup"><span data-stu-id="eae7a-216">Add policies from the Polly registry</span></span>

<span data-ttu-id="eae7a-217">Přístup pro správu pravidelně použité zásady je třeba definovat jednou a registrovat s `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="eae7a-217">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="eae7a-218">Metody rozšíření je poskytována, což umožňuje, aby obslužná rutina přidat pomocí zásad z registru:</span><span class="sxs-lookup"><span data-stu-id="eae7a-218">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="eae7a-219">V předchozí kód PolicyRegistry přidán `ServiceCollection` a dvě zásady, které jsou registrovány s ním.</span><span class="sxs-lookup"><span data-stu-id="eae7a-219">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="eae7a-220">Chcete-li použít zásady z registru, `AddPolicyHandlerFromRegistry` metoda se používá, předávání název zásady použít.</span><span class="sxs-lookup"><span data-stu-id="eae7a-220">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="eae7a-221">Další informace o `IHttpClientFactory` a integrace v rámci Polly naleznete na [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="eae7a-221">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="eae7a-222">Správa HttpClient a doba platnosti</span><span class="sxs-lookup"><span data-stu-id="eae7a-222">HttpClient and lifetime management</span></span>

<span data-ttu-id="eae7a-223">Pokaždé, když `CreateClient` se volá na `IHttpClientFactory`, novou instanci třídy `HttpClient` je vrácen.</span><span class="sxs-lookup"><span data-stu-id="eae7a-223">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="eae7a-224">Budou existovat `HttpMessageHandler` jednotlivé klienty, s názvem.</span><span class="sxs-lookup"><span data-stu-id="eae7a-224">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="eae7a-225">`IHttpClientFactory` bude fond `HttpMessageHandler` instance vytvořený objekt factory ke snížení spotřeby prostředků.</span><span class="sxs-lookup"><span data-stu-id="eae7a-225">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="eae7a-226">A `HttpMessageHandler` může při vytvoření nové instance z fondu znovu použít `HttpClient` instance Pokud nevypršela platnost celé jeho životnosti.</span><span class="sxs-lookup"><span data-stu-id="eae7a-226">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="eae7a-227">Sdružování obslužných rutin je žádoucí, jak každou obslužnou rutinu obvykle spravuje vlastní základní připojení protokolu HTTP; Vytvoření více obslužných rutin, než je nezbytné, může mít za následek zpoždění připojení.</span><span class="sxs-lookup"><span data-stu-id="eae7a-227">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="eae7a-228">Některé obslužné rutiny také nechat připojení otevřené bez omezení, která zabránit, aby obslužná rutina reaguje na změny DNS.</span><span class="sxs-lookup"><span data-stu-id="eae7a-228">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="eae7a-229">Výchozí doba života obslužná rutina je dvě minuty.</span><span class="sxs-lookup"><span data-stu-id="eae7a-229">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="eae7a-230">Výchozí hodnota je možné přepsat na za pojmenované klienty.</span><span class="sxs-lookup"><span data-stu-id="eae7a-230">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="eae7a-231">Chcete-li přepsat, volejte `SetHandlerLifetime` na `IHttpClientBuilder` při vytváření klienta, je vrácena:</span><span class="sxs-lookup"><span data-stu-id="eae7a-231">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="eae7a-232">protokolování</span><span class="sxs-lookup"><span data-stu-id="eae7a-232">Logging</span></span>

<span data-ttu-id="eae7a-233">Klienti vytvořené prostřednictvím `IHttpClientFactory` záznam protokolu zpráv pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="eae7a-233">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="eae7a-234">Budete muset povolit na úrovni příslušné informace o konfiguraci protokolování zobrazíte výchozí zprávy protokolu.</span><span class="sxs-lookup"><span data-stu-id="eae7a-234">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="eae7a-235">Další protokolování, například protokolování hlaviček požadavků, je uvedený jenom na úroveň trasování.</span><span class="sxs-lookup"><span data-stu-id="eae7a-235">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="eae7a-236">Kategorii protokolu se používá pro každého klienta obsahuje název klienta.</span><span class="sxs-lookup"><span data-stu-id="eae7a-236">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="eae7a-237">Klient s názvem "MyNamedClient", například protokoly zprávy s kategorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="eae7a-237">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="eae7a-238">Zprávy s příponou "LogicalHandler", ke kterým došlo u mimo kanálu obslužná rutina požadavku.</span><span class="sxs-lookup"><span data-stu-id="eae7a-238">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="eae7a-239">V žádosti zprávy jsou zaznamenány předtím, než ho mít zpracován ostatních obslužných rutin v kanálu.</span><span class="sxs-lookup"><span data-stu-id="eae7a-239">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="eae7a-240">V odpovědi zprávy jsou zaznamenány po všech ostatních obslužných rutin kanálu obdrželi odpovědi.</span><span class="sxs-lookup"><span data-stu-id="eae7a-240">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="eae7a-241">Také dochází k protokolování uvnitř kanál obslužná rutina požadavku.</span><span class="sxs-lookup"><span data-stu-id="eae7a-241">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="eae7a-242">V případě příkladu "MyNamedClient" tyto zprávy se zaznamenávají proti protokolu kategorie `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="eae7a-242">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="eae7a-243">Pro daný požadavek k tomu dochází po spuštění ostatních obslužných rutin a bezprostředně před odesláním požadavku v síti.</span><span class="sxs-lookup"><span data-stu-id="eae7a-243">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="eae7a-244">V odpovědi protokolování zahrnuje stav odpovědi předtím, než se předává zpátky pomocí kanálu obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="eae7a-244">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="eae7a-245">Povolení protokolování na vnější a v rámci kanálu umožní kontrolu změny provedené ostatních obslužných rutin v kanálu.</span><span class="sxs-lookup"><span data-stu-id="eae7a-245">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="eae7a-246">To může zahrnovat změny k žádosti hlavičky, například nebo stavový kód odpovědi.</span><span class="sxs-lookup"><span data-stu-id="eae7a-246">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="eae7a-247">Umožňuje filtrování pro konkrétní pojmenované klienty potřeby protokolu, včetně názvu klienta v kategorii protokolu.</span><span class="sxs-lookup"><span data-stu-id="eae7a-247">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="eae7a-248">Objekt HttpMessageHandler konfigurace</span><span class="sxs-lookup"><span data-stu-id="eae7a-248">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="eae7a-249">Může být nezbytné k řízení konfigurace vnitřního `HttpMessageHandler` používaný klientem.</span><span class="sxs-lookup"><span data-stu-id="eae7a-249">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="eae7a-250">`IHttpClientBuilder` Se vrátí při přidávání s názvem nebo zadali klientů.</span><span class="sxs-lookup"><span data-stu-id="eae7a-250">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="eae7a-251">`ConfigurePrimaryHttpMessageHandler` Metoda rozšíření lze použít k definování delegáta.</span><span class="sxs-lookup"><span data-stu-id="eae7a-251">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="eae7a-252">Delegát slouží k vytvoření a konfigurace primární `HttpMessageHandler` používané tohoto klienta:</span><span class="sxs-lookup"><span data-stu-id="eae7a-252">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
