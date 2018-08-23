---
title: Iniciování požadavků HTTP
author: stevejgordon
description: Další informace o použití rozhraní IHttpClientFactory ke správě instance logického HttpClient v ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/07/2018
uid: fundamentals/http-requests
ms.openlocfilehash: 2a1bf78edb5068d8b10d66e5ef306b1ad4395da6
ms.sourcegitcommit: 15d7bd0b2c4e6fe9ac335d658bab71a45ca5bc72
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/20/2018
ms.locfileid: "41752523"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="97269-103">Iniciování požadavků HTTP</span><span class="sxs-lookup"><span data-stu-id="97269-103">Initiate HTTP requests</span></span>

<span data-ttu-id="97269-104">Podle [Glenn Condron](https://github.com/glennc), [Ryanem Nowak](https://github.com/rynowak), a [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="97269-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="97269-105">[IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) můžete zaregistrované a slouží ke konfiguraci a vytvořte [HttpClient](/dotnet/api/system.net.http.httpclient) instance v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="97269-105">An [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="97269-106">Nabízí následující výhody:</span><span class="sxs-lookup"><span data-stu-id="97269-106">It offers the following benefits:</span></span>

* <span data-ttu-id="97269-107">Poskytuje centrální umístění pro pojmenovávání a konfiguraci logické `HttpClient` instancí.</span><span class="sxs-lookup"><span data-stu-id="97269-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="97269-108">Například *githubu* klient může zaregistrované a nakonfigurovat tak, aby ke Githubu přistupovat.</span><span class="sxs-lookup"><span data-stu-id="97269-108">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="97269-109">Výchozí klienta lze zaregistrovat k jiným účelům.</span><span class="sxs-lookup"><span data-stu-id="97269-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="97269-110">Kodifikuje koncept odchozí middleware prostřednictvím delegování obslužné rutiny ve `HttpClient` a rozšíření pro middleware založený na Polly výhod, které poskytuje.</span><span class="sxs-lookup"><span data-stu-id="97269-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="97269-111">Spravuje sdružování a dobu života základní `HttpClientMessageHandler` instancí se vyhnout běžným potížím DNS, ke kterým dochází při správě ručně `HttpClient` životnosti.</span><span class="sxs-lookup"><span data-stu-id="97269-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="97269-112">Přidá prostředí konfigurovat protokolování (prostřednictvím `ILogger`) pro všechny požadavky odeslané prostřednictvím klientů, které jsou vytvořeny procesem.</span><span class="sxs-lookup"><span data-stu-id="97269-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="97269-113">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="97269-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97269-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="97269-114">Prerequisites</span></span>

<span data-ttu-id="97269-115">Projekty cílené na rozhraní .NET Framework vyžadují instalaci [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="97269-115">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="97269-116">Projekty, které cílí na .NET Core a odkaz [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) již patří `Microsoft.Extensions.Http` balíčku.</span><span class="sxs-lookup"><span data-stu-id="97269-116">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="97269-117">Vzory využití</span><span class="sxs-lookup"><span data-stu-id="97269-117">Consumption patterns</span></span>

<span data-ttu-id="97269-118">Existuje několik způsobů `IHttpClientFactory` lze použít v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="97269-118">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="97269-119">Základní informace o využití</span><span class="sxs-lookup"><span data-stu-id="97269-119">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="97269-120">Pojmenované klientů</span><span class="sxs-lookup"><span data-stu-id="97269-120">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="97269-121">Typy klientů</span><span class="sxs-lookup"><span data-stu-id="97269-121">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="97269-122">Vygenerovaný klientů</span><span class="sxs-lookup"><span data-stu-id="97269-122">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="97269-123">Žádná z nich není striktně vynikající do jiného.</span><span class="sxs-lookup"><span data-stu-id="97269-123">None of them are strictly superior to another.</span></span> <span data-ttu-id="97269-124">Nejlepší přístup, závisí na omezení aplikace.</span><span class="sxs-lookup"><span data-stu-id="97269-124">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="97269-125">Základní informace o využití</span><span class="sxs-lookup"><span data-stu-id="97269-125">Basic usage</span></span>

<span data-ttu-id="97269-126">`IHttpClientFactory` Lze registrovat pomocí volání `AddHttpClient` rozšiřující metody na `IServiceCollection`uvnitř `Startup.ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="97269-126">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="97269-127">Po registraci může přijmout kódu `IHttpClientFactory` kdekoli služby může být vložený se [injektáž závislostí](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="97269-127">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="97269-128">`IHttpClientFactory` Slouží k vytvoření `HttpClient` instance:</span><span class="sxs-lookup"><span data-stu-id="97269-128">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="97269-129">Pomocí `IHttpClientFactory` tímto způsobem je skvělý způsob, jak Refaktorovat stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="97269-129">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="97269-130">Nemá žádný vliv na způsob, jakým `HttpClient` se používá.</span><span class="sxs-lookup"><span data-stu-id="97269-130">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="97269-131">Na místech, kde `HttpClient` aktuálně se vytvářejí instance, nahraďte volání výskyty [CreateClient](/dotnet/api/system.net.http.ihttpclientfactory.createclient).</span><span class="sxs-lookup"><span data-stu-id="97269-131">In places where `HttpClient` instances are currently created, replace those occurrences with a call to [CreateClient](/dotnet/api/system.net.http.ihttpclientfactory.createclient).</span></span>

### <a name="named-clients"></a><span data-ttu-id="97269-132">Pojmenované klientů</span><span class="sxs-lookup"><span data-stu-id="97269-132">Named clients</span></span>

<span data-ttu-id="97269-133">Pokud aplikace vyžaduje použití mnoha různých `HttpClient`, každý s jinou konfiguraci, je možnost použití **s názvem klienti**.</span><span class="sxs-lookup"><span data-stu-id="97269-133">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="97269-134">Konfigurace pro pojmenovaná `HttpClient` se dá nastavit během registrace v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="97269-134">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="97269-135">V předchozím kódu `AddHttpClient` je volána, že zadáte název *githubu*.</span><span class="sxs-lookup"><span data-stu-id="97269-135">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="97269-136">Některé výchozí konfigurace má tento klient&mdash;totiž základní adrese a dvě záhlaví vyžadována pro práci s rozhraním API pro GitHub.</span><span class="sxs-lookup"><span data-stu-id="97269-136">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="97269-137">Pokaždé, když `CreateClient` nazývá novou instanci třídy `HttpClient` vytvoření a konfigurace akce je volána.</span><span class="sxs-lookup"><span data-stu-id="97269-137">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="97269-138">Využívat pojmenované klienta, může být předán parametr řetězce `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="97269-138">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="97269-139">Zadejte název klienta, který se má vytvořit:</span><span class="sxs-lookup"><span data-stu-id="97269-139">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="97269-140">V předchozím kódu žádost nemusí zadejte název hostitele.</span><span class="sxs-lookup"><span data-stu-id="97269-140">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="97269-141">Ho můžete předat právě cestu, protože se používá základní adresu nakonfigurovanou pro klienta.</span><span class="sxs-lookup"><span data-stu-id="97269-141">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="97269-142">Typy klientů</span><span class="sxs-lookup"><span data-stu-id="97269-142">Typed clients</span></span>

<span data-ttu-id="97269-143">Typy klientů poskytují stejné funkce jako pojmenované klientů bez nutnosti použití řetězců jako klíče.</span><span class="sxs-lookup"><span data-stu-id="97269-143">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="97269-144">Typový klient přístup poskytuje IntelliSense a kompilátoru pomoct při využívání klientů.</span><span class="sxs-lookup"><span data-stu-id="97269-144">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="97269-145">Poskytuje jedno centrální umístění pro konfiguraci a interakci s konkrétní `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="97269-145">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="97269-146">Například konkrétního typu klienta může být použit pro koncový bod jediného back-endu a zapouzdření všech řešení logiku tohoto koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="97269-146">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="97269-147">Další výhodou je, že práce s DI a můžete vloženy vyžadováno ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="97269-147">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="97269-148">Typový klient přijme `HttpClient` parametr v konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="97269-148">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="97269-149">V předchozím kódu konfigurace přesunout do typový klient.</span><span class="sxs-lookup"><span data-stu-id="97269-149">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="97269-150">`HttpClient` Objektu je vystaven jako veřejná vlastnost.</span><span class="sxs-lookup"><span data-stu-id="97269-150">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="97269-151">Je možné definovat metody specifické pro rozhraní API, které vystavují `HttpClient` funkce.</span><span class="sxs-lookup"><span data-stu-id="97269-151">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="97269-152">`GetAspNetDocsIssues` Metoda zapouzdřuje kód potřebný k vyhledání a parsování nejnovější otevřené problémy z úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="97269-152">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="97269-153">K registraci typový klient Obecné `AddHttpClient` metody rozšíření lze použít v `Startup.ConfigureServices`, určení typový klient třídy:</span><span class="sxs-lookup"><span data-stu-id="97269-153">To register a typed client, the generic `AddHttpClient` extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="97269-154">Typový klient je zaregistrovaný jako přechodné s DI.</span><span class="sxs-lookup"><span data-stu-id="97269-154">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="97269-155">Typový klient může vloží a využívat přímo:</span><span class="sxs-lookup"><span data-stu-id="97269-155">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="97269-156">Pokud tomu dávají přednost, konfigurace pro typový klient se dá nastavit během registrace v `Startup.ConfigureServices`, spíše než v konstruktoru typu klienta:</span><span class="sxs-lookup"><span data-stu-id="97269-156">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="97269-157">Je možné zcela zapouzdření `HttpClient` v rámci typu klienta.</span><span class="sxs-lookup"><span data-stu-id="97269-157">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="97269-158">Místo bude vystavená jako vlastnost, je možné veřejné metody poskytnou která volá `HttpClient` instance interně.</span><span class="sxs-lookup"><span data-stu-id="97269-158">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="97269-159">V předchozím kódu `HttpClient` se ukládá jako soukromé pole.</span><span class="sxs-lookup"><span data-stu-id="97269-159">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="97269-160">Veškerý přístup pro volání externích prochází `GetRepos` metody.</span><span class="sxs-lookup"><span data-stu-id="97269-160">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="97269-161">Vygenerovaný klientů</span><span class="sxs-lookup"><span data-stu-id="97269-161">Generated clients</span></span>

<span data-ttu-id="97269-162">`IHttpClientFactory` můžete použít v kombinaci s dalšími knihovnami třetích stran, jako [provedení nového](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="97269-162">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="97269-163">Refit je REST knihovna pro .NET.</span><span class="sxs-lookup"><span data-stu-id="97269-163">Refit is a REST library for .NET.</span></span> <span data-ttu-id="97269-164">Rozhraní REST API se převede na živé interfaces.</span><span class="sxs-lookup"><span data-stu-id="97269-164">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="97269-165">Implementace rozhraní se vygeneruje dynamicky pomocí `RestService`pomocí `HttpClient` aby externí HTTP volání.</span><span class="sxs-lookup"><span data-stu-id="97269-165">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="97269-166">Rozhraní a odpovědi jsou definovány pro reprezentaci externí rozhraní API a odpověď:</span><span class="sxs-lookup"><span data-stu-id="97269-166">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="97269-167">Typový klient můžete přidat, pomocí Refit generovat implementace:</span><span class="sxs-lookup"><span data-stu-id="97269-167">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="97269-168">Definované rozhraní můžete využívat v případě potřeby s implementací poskytované DI a Refit:</span><span class="sxs-lookup"><span data-stu-id="97269-168">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="97269-169">Odchozí žádosti o middlewaru</span><span class="sxs-lookup"><span data-stu-id="97269-169">Outgoing request middleware</span></span>

<span data-ttu-id="97269-170">`HttpClient` již obsahuje koncept delegování obslužné rutiny, které může být propojený pro odchozí požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="97269-170">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="97269-171">`IHttpClientFactory` Usnadňuje k definování obslužných rutin mají použít u každého klienta s názvem.</span><span class="sxs-lookup"><span data-stu-id="97269-171">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="97269-172">Podporuje registraci a zřetězení více obslužných rutin k sestavení kanál middleware odchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="97269-172">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="97269-173">Každá z těchto obslužných rutin je moci provádět úkoly před a za odchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="97269-173">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="97269-174">Tento model je podobný kanál příchozí middlewaru v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="97269-174">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="97269-175">Vzor poskytuje mechanismus ke správě vyskytující aspekty kolem požadavků protokolu HTTP, včetně ukládání do mezipaměti, zpracování chyb, serializaci a protokolování.</span><span class="sxs-lookup"><span data-stu-id="97269-175">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="97269-176">Chcete-li vytvořit obslužnou rutinu, definujte třídu odvozenou z `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="97269-176">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="97269-177">Přepsat `SendAsync` metoda spuštění kódu před předáním požadavku další obslužná rutina kanálu:</span><span class="sxs-lookup"><span data-stu-id="97269-177">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="97269-178">Předcházející kód definuje obslužnou rutinu základní.</span><span class="sxs-lookup"><span data-stu-id="97269-178">The preceding code defines a basic handler.</span></span> <span data-ttu-id="97269-179">Zkontroluje, jestli `X-API-KEY` záhlaví byla zahrnuta v požadavku.</span><span class="sxs-lookup"><span data-stu-id="97269-179">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="97269-180">Pokud chybí záhlaví, můžete vyhnout volání HTTP a vrátí odpověď vhodný.</span><span class="sxs-lookup"><span data-stu-id="97269-180">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="97269-181">Během registrace, lze přidat jeden nebo více obslužných rutin do konfigurace `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="97269-181">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="97269-182">Tato úloha se provádí prostřednictvím metody rozšíření na [IHttpClientBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.ihttpclientbuilder).</span><span class="sxs-lookup"><span data-stu-id="97269-182">This task is accomplished via extension methods on the [IHttpClientBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.ihttpclientbuilder).</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="97269-183">V předchozím kódu `ValidateHeaderHandler` DI zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="97269-183">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="97269-184">Obslužná rutina **musí** zaregistrovat v DI jako přechodné.</span><span class="sxs-lookup"><span data-stu-id="97269-184">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="97269-185">Po registraci [AddHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.addhttpmessagehandler) může být volána, předejte typ pro obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="97269-185">Once registered, [AddHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.addhttpmessagehandler) can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="97269-186">Může být registrováno více obslužných rutin v pořadí, ve kterém by se měl spustit.</span><span class="sxs-lookup"><span data-stu-id="97269-186">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="97269-187">Každý popisovač zabalí další obslužná rutina až do konečné `HttpClientHandler` zpracuje požadavek:</span><span class="sxs-lookup"><span data-stu-id="97269-187">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="97269-188">Použít na základě Polly obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="97269-188">Use Polly-based handlers</span></span>

<span data-ttu-id="97269-189">`IHttpClientFactory` se integruje s oblíbenými knihovnu třetí strany s názvem [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="97269-189">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="97269-190">Polly je komplexní odolnosti a přechodné zpracování chyb library pro .NET.</span><span class="sxs-lookup"><span data-stu-id="97269-190">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="97269-191">Umožňuje vývojářům vyjádřit zásady například opakování, jističe, vypršení časového limitu, přepážka izolace a záložních fluent a bezpečným způsobem.</span><span class="sxs-lookup"><span data-stu-id="97269-191">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="97269-192">Metody rozšíření jsou k dispozici pro povolení použití zásad Polly nakonfigurované `HttpClient` instancí.</span><span class="sxs-lookup"><span data-stu-id="97269-192">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="97269-193">Jsou k dispozici v rozšíření Polly [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="97269-193">The Polly extensions are available in the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="97269-194">Není součástí tohoto balíčku [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="97269-194">This package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="97269-195">Abyste použili rozšíření explicitní `<PackageReference />` by měl být zahrnutý v projektu.</span><span class="sxs-lookup"><span data-stu-id="97269-195">To use the extensions, an explicit `<PackageReference />` should be included in the project.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="97269-196">Po obnovení tohoto balíčku, rozšiřující metody jsou k dispozici pro podporu přidání obslužné rutiny na základě Polly klientům.</span><span class="sxs-lookup"><span data-stu-id="97269-196">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="97269-197">Zpracování přechodných chyb</span><span class="sxs-lookup"><span data-stu-id="97269-197">Handle transient faults</span></span>

<span data-ttu-id="97269-198">Většina běžných chyb nastane, když přechodné externí volání HTTP.</span><span class="sxs-lookup"><span data-stu-id="97269-198">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="97269-199">Pohodlné rozšíření metoda volána `AddTransientHttpErrorPolicy` je zahrnuté umožňuje zásadu, která definované pro zpracování přechodných chyb.</span><span class="sxs-lookup"><span data-stu-id="97269-199">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="97269-200">Zásady konfigurované pomocí tohoto úchytu – metoda rozšíření `HttpRequestException`, odpovědi HTTP 5xx a odpovědi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="97269-200">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="97269-201">`AddTransientHttpErrorPolicy` Rozšíření může být použito v rámci `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="97269-201">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="97269-202">Toto rozšíření poskytuje přístup k `PolicyBuilder` objekt nakonfigurovaný pro zpracování chyb představující možných přechodných chyb:</span><span class="sxs-lookup"><span data-stu-id="97269-202">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="97269-203">V předchozím kódu `WaitAndRetryAsync` definované zásady.</span><span class="sxs-lookup"><span data-stu-id="97269-203">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="97269-204">Neúspěšné požadavky se zopakují až třikrát s zpožděním mezi pokusy o 600 ms.</span><span class="sxs-lookup"><span data-stu-id="97269-204">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="97269-205">Dynamicky vybrat zásady</span><span class="sxs-lookup"><span data-stu-id="97269-205">Dynamically select policies</span></span>

<span data-ttu-id="97269-206">Další rozšiřující metody existují, které lze přidat na základě Polly obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="97269-206">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="97269-207">Je jedno takové rozšíření `AddPolicyHandler`, který má několik přetížení.</span><span class="sxs-lookup"><span data-stu-id="97269-207">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="97269-208">Jedním přetížením mu umožní ho možné zkontrolovat při definování zásad, které chcete použít:</span><span class="sxs-lookup"><span data-stu-id="97269-208">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="97269-209">V předchozím kódu pokud je odchozí požadavek GET, časový limit 10 sekundu se použije.</span><span class="sxs-lookup"><span data-stu-id="97269-209">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="97269-210">Pro jiné metody HTTP se používá s časovým limitem 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="97269-210">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="97269-211">Přidávání více obslužných rutin Polly</span><span class="sxs-lookup"><span data-stu-id="97269-211">Add multiple Polly handlers</span></span>

<span data-ttu-id="97269-212">Je běžné vnořit Polly zásady, které poskytují vylepšené funkce:</span><span class="sxs-lookup"><span data-stu-id="97269-212">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="97269-213">V předchozím příkladu jsou přidány dvě obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="97269-213">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="97269-214">První použití `AddTransientHttpErrorPolicy` rozšíření přidat zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="97269-214">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="97269-215">Neúspěšné požadavky se zopakují až třikrát.</span><span class="sxs-lookup"><span data-stu-id="97269-215">Failed requests are retried up to three times.</span></span> <span data-ttu-id="97269-216">Druhé volání `AddTransientHttpErrorPolicy` přidá zásadu jističe.</span><span class="sxs-lookup"><span data-stu-id="97269-216">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="97269-217">Další externí požadavky jsou blokovány 30 sekund, pokud postupně dojde k 5 neúspěšných pokusů o přihlášení.</span><span class="sxs-lookup"><span data-stu-id="97269-217">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="97269-218">Jistič zásady jsou stavová.</span><span class="sxs-lookup"><span data-stu-id="97269-218">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="97269-219">Všechna volání prostřednictvím tohoto klienta sdílet stejný stav okruhu.</span><span class="sxs-lookup"><span data-stu-id="97269-219">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="97269-220">Přidání zásad z registru Polly</span><span class="sxs-lookup"><span data-stu-id="97269-220">Add policies from the Polly registry</span></span>

<span data-ttu-id="97269-221">Přístup pro správu pravidelně použité zásady, je jejich jednou definovat a registrovat pomocí `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="97269-221">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="97269-222">Metody rozšíření je k dispozici tomu, aby obslužná rutina přidat pomocí zásad z registru:</span><span class="sxs-lookup"><span data-stu-id="97269-222">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="97269-223">V předchozím kódu jsou registrovány dvě zásady, které při `PolicyRegistry` se přidá do `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="97269-223">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="97269-224">Pomocí zásad z registru, `AddPolicyHandlerFromRegistry` metoda se používá, předejte název zásady začaly platit.</span><span class="sxs-lookup"><span data-stu-id="97269-224">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="97269-225">Další informace o `IHttpClientFactory` a integrace Polly můžete najít na [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="97269-225">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="97269-226">HttpClient a životního cyklu správy</span><span class="sxs-lookup"><span data-stu-id="97269-226">HttpClient and lifetime management</span></span>

<span data-ttu-id="97269-227">Nový `HttpClient` pokaždé, když je vrácena instance `CreateClient` je volán na `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="97269-227">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="97269-228">Je [objekt HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler) jednoho klienta s názvem.</span><span class="sxs-lookup"><span data-stu-id="97269-228">There's an [HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler) per named client.</span></span> <span data-ttu-id="97269-229">`IHttpClientFactory` fondy `HttpMessageHandler` instancí, které jsou vytvořeny procesem ke snížení spotřeby prostředků.</span><span class="sxs-lookup"><span data-stu-id="97269-229">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="97269-230">`HttpMessageHandler` Instance může být znovu použít z fondu při vytváření nového `HttpClient` instance Pokud nevypršela platnost svého životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="97269-230">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="97269-231">Sdružování obslužných rutin je žádoucí, protože každá obslužná rutina obvykle spravuje svou vlastní základní připojení protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="97269-231">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="97269-232">Vytváření více obslužných rutin, než je nezbytné, může způsobit zpoždění připojení.</span><span class="sxs-lookup"><span data-stu-id="97269-232">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="97269-233">Některé obslužné rutiny také zachovat připojení otevřené po neomezenou dobu, což může zabránit obslužnou rutinu reakce na změny DNS.</span><span class="sxs-lookup"><span data-stu-id="97269-233">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="97269-234">Výchozí doba života obslužná rutina je dvě minuty.</span><span class="sxs-lookup"><span data-stu-id="97269-234">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="97269-235">Výchozí hodnota se dá přepsat na základě pojmenované klienta.</span><span class="sxs-lookup"><span data-stu-id="97269-235">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="97269-236">Chcete-li přepsat, zavolejte [SetHandlerLifetime](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.sethandlerlifetime) na `IHttpClientBuilder` , který je vrácen při vytváření klienta:</span><span class="sxs-lookup"><span data-stu-id="97269-236">To override it, call [SetHandlerLifetime](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.sethandlerlifetime) on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="97269-237">Vyřazení klienta není povinné.</span><span class="sxs-lookup"><span data-stu-id="97269-237">Disposal of the client isn't required.</span></span> <span data-ttu-id="97269-238">Vyřazení zruší odchozí požadavky a záruky daném `HttpClient` instanci nelze použít po volání [Dispose](/dotnet/api/system.idisposable.dispose#System_IDisposable_Dispose).</span><span class="sxs-lookup"><span data-stu-id="97269-238">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling [Dispose](/dotnet/api/system.idisposable.dispose#System_IDisposable_Dispose).</span></span> <span data-ttu-id="97269-239">`IHttpClientFactory` sleduje a uvolní prostředky využívané třídou `HttpClient` instancí.</span><span class="sxs-lookup"><span data-stu-id="97269-239">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="97269-240">`HttpClient` Instancí lze obecně zacházet jako objekty .NET nevyžaduje vyřazení.</span><span class="sxs-lookup"><span data-stu-id="97269-240">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="97269-241">Zachování jediného `HttpClient` instance zachování připojení pro dlouhé době se běžně používá dříve, než vzniku `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="97269-241">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="97269-242">Tento model se stane zbytečné po migraci na `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="97269-242">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="97269-243">protokolování</span><span class="sxs-lookup"><span data-stu-id="97269-243">Logging</span></span>

<span data-ttu-id="97269-244">Klienty vytvořené prostřednictvím `IHttpClientFactory` záznam protokolu zpráv pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="97269-244">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="97269-245">Povolte úroveň příslušné informace v konfiguraci protokolování tak, aby se zobrazit výchozí zprávy protokolu.</span><span class="sxs-lookup"><span data-stu-id="97269-245">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="97269-246">Dodatečné protokolování, jako je například protokolování hlavičky požadavku je zahrnuta pouze na úroveň trasování.</span><span class="sxs-lookup"><span data-stu-id="97269-246">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="97269-247">Kategorie protokolu používá pro každého klienta obsahuje název klienta.</span><span class="sxs-lookup"><span data-stu-id="97269-247">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="97269-248">Klient s názvem *MyNamedClient*, například zprávy protokolu s kategorii `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="97269-248">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="97269-249">Zprávy doplněny *LogicalHandler* dochází mimo kanál obslužná rutina požadavku.</span><span class="sxs-lookup"><span data-stu-id="97269-249">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="97269-250">V požadavku jsou zprávy zaznamenány předtím, než ji zpracovali ostatních obslužných rutin v kanálu.</span><span class="sxs-lookup"><span data-stu-id="97269-250">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="97269-251">V odpovědi jsou zprávy zaznamenány po všech ostatních obslužných rutin kanálu mají přijetí odpovědi.</span><span class="sxs-lookup"><span data-stu-id="97269-251">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="97269-252">Také dochází k protokolování v kanálu obslužné rutiny žádosti.</span><span class="sxs-lookup"><span data-stu-id="97269-252">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="97269-253">V *MyNamedClient* například tyto zprávy jsou zaznamenané v souvislosti s kategorie protokolu `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="97269-253">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="97269-254">Pro žádost k tomu dochází po spuštění ostatních obslužných rutin a bezprostředně před odesláním žádosti v síti.</span><span class="sxs-lookup"><span data-stu-id="97269-254">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="97269-255">V odpovědi protokolování zahrnuje stav odpovědi před odesláním zpět prostřednictvím kanálu obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="97269-255">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="97269-256">Povolení protokolování mimo a v kanálu umožňuje kontrolu změny provedené ostatních obslužných rutin v kanálu.</span><span class="sxs-lookup"><span data-stu-id="97269-256">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="97269-257">To může zahrnovat změny hlavičky, požadavku, například nebo stavový kód odpovědi.</span><span class="sxs-lookup"><span data-stu-id="97269-257">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="97269-258">Včetně názvu klienta v kategorii protokolů umožňuje filtrování konkrétních klientů s názvem v případě potřeby protokolování.</span><span class="sxs-lookup"><span data-stu-id="97269-258">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="97269-259">Objekt HttpMessageHandler konfigurace</span><span class="sxs-lookup"><span data-stu-id="97269-259">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="97269-260">Může být nutné k řízení konfigurace vnitřního `HttpMessageHandler` používaný klientem.</span><span class="sxs-lookup"><span data-stu-id="97269-260">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="97269-261">`IHttpClientBuilder` Dochází při přidávání s názvem nebo typy klientů.</span><span class="sxs-lookup"><span data-stu-id="97269-261">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="97269-262">[ConfigurePrimaryHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.configureprimaryhttpmessagehandler) metody rozšíření lze použít k definování delegáta.</span><span class="sxs-lookup"><span data-stu-id="97269-262">The [ConfigurePrimaryHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.configureprimaryhttpmessagehandler) extension method can be used to define a delegate.</span></span> <span data-ttu-id="97269-263">Delegát se používá k vytvoření a konfigurace primární `HttpMessageHandler` používané tohoto klienta:</span><span class="sxs-lookup"><span data-stu-id="97269-263">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]
