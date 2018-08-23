---
title: Otevřete Web Interface pro .NET (OWIN) s ASP.NET Core
author: ardalis
description: Zjistěte, jak ASP.NET Core podporuje Open Web Interface pro .NET (OWIN), což umožní webové aplikace k oddělení od webových serverů.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
uid: fundamentals/owin
ms.openlocfilehash: db28eeff88a13dc95c469f3b7c0746c807da830f
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752521"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a><span data-ttu-id="8dc5f-103">Otevřete Web Interface pro .NET (OWIN) s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8dc5f-103">Open Web Interface for .NET (OWIN) with ASP.NET Core</span></span>

<span data-ttu-id="8dc5f-104">Podle [Steve Smith](https://ardalis.com/) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-104">By [Steve Smith](https://ardalis.com/) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8dc5f-105">ASP.NET Core podporuje Open Web Interface pro .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="8dc5f-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="8dc5f-106">OWIN umožňuje webové aplikace k oddělení od webových serverů.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="8dc5f-107">Definuje standardní způsob pro middleware v kanálu použít ke zpracování požadavků a související odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="8dc5f-108">Aplikace ASP.NET Core a middleware dokáže spolupracovat s aplikací OWIN, servery a middlewaru.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="8dc5f-109">OWIN poskytuje oddělovací vrstvu, která umožňuje dvě architektury s různorodých objektové modely, který se má použít společně.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="8dc5f-110">`Microsoft.AspNetCore.Owin` Balíček poskytuje dvě implementace adaptér:</span><span class="sxs-lookup"><span data-stu-id="8dc5f-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>

* <span data-ttu-id="8dc5f-111">ASP.NET Core do OWIN</span><span class="sxs-lookup"><span data-stu-id="8dc5f-111">ASP.NET Core to OWIN</span></span> 
* <span data-ttu-id="8dc5f-112">OWIN k ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8dc5f-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="8dc5f-113">To umožňuje zajistit také jejich hostování nad OWIN kompatibilní serveru/hostitele služby nebo pro jiné komponenty kompatibilní OWIN pro spuštění nad ASP.NET Core ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="8dc5f-114">Pomocí těchto adaptérů součástí nákladů na výkon.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-114">Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="8dc5f-115">Neměli byste používat aplikace s využitím pouze součásti ASP.NET Core `Microsoft.AspNetCore.Owin` balíčku nebo adaptéry.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-115">Apps using only ASP.NET Core components shouldn't use the `Microsoft.AspNetCore.Owin` package or adapters.</span></span>

<span data-ttu-id="8dc5f-116">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8dc5f-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-core-pipeline"></a><span data-ttu-id="8dc5f-117">Spuštění OWIN middleware v kanálu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8dc5f-117">Running OWIN middleware in the ASP.NET Core pipeline</span></span>

<span data-ttu-id="8dc5f-118">Podpora OWIN ASP.NET Core je nasazen jako součást `Microsoft.AspNetCore.Owin` balíčku.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="8dc5f-119">Podpora OWIN můžete importovat do projektu po instalaci tohoto balíčku.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="8dc5f-120">Middlewaru OWIN, který odpovídá [specifikace OWIN](http://owin.org/spec/spec/owin-1.0.0.html), což vyžaduje `Func<IDictionary<string, object>, Task>` nastaví rozhraní a konkrétní klíče (například `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="8dc5f-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="8dc5f-121">Následující jednoduchý middlewaru OWIN, který se zobrazí "Hello World":</span><span class="sxs-lookup"><span data-stu-id="8dc5f-121">The following simple OWIN middleware displays "Hello World":</span></span>

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

<span data-ttu-id="8dc5f-122">Vrátí vzorek podpis `Task` a přijímá `IDictionary<string, object>` podle požadavku OWIN.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="8dc5f-123">Následující kód ukazuje, jak přidat `OwinHello` middleware (popsaný výš) do kanálu ASP.NET Core s `UseOwin` – metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET Core pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="8dc5f-124">Můžete nakonfigurovat další akce, aby proběhla v rámci kanálu OWIN.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="8dc5f-125">Hlavičky odpovědí by měl být upraven pouze před prvním zápisu do datového proudu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="8dc5f-126">Více volání `UseOwin` se nedoporučuje kvůli výkonu.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="8dc5f-127">Komponenty OWIN bude fungovat nejlíp Pokud seskupené dohromady.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-127">OWIN components will operate best if grouped together.</span></span>

```csharp
app.UseOwin(pipeline =>
{
    pipeline(async (next) =>
    {
        // do something before
        await OwinHello(new OwinEnvironment(HttpContext));
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-core-hosting-on-an-owin-based-server"></a><span data-ttu-id="8dc5f-128">Použití hostování v technologii ASP.NET Core na server s procesorem OWIN</span><span class="sxs-lookup"><span data-stu-id="8dc5f-128">Using ASP.NET Core Hosting on an OWIN-based server</span></span>

<span data-ttu-id="8dc5f-129">Na základě OWIN servery můžou hostovat aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-129">OWIN-based servers can host ASP.NET Core apps.</span></span> <span data-ttu-id="8dc5f-130">Jeden takový server je [Nowin](https://github.com/Bobris/Nowin), webový server .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="8dc5f-131">V ukázce pro účely tohoto článku, můžu zahrnuli projekt, který odkazuje na Nowin a použije ho k vytvoření `IServer` dokáže ASP.NET Core s vlastním hostováním.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="8dc5f-132">`IServer` je rozhraní, která vyžaduje `Features` vlastnost a `Start` metoda.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-132">`IServer` is an interface that requires a `Features` property and a `Start` method.</span></span>

<span data-ttu-id="8dc5f-133">`Start` je zodpovědná za konfiguraci a spuštění serveru, který v tomto případě se provádí prostřednictvím řady fluent volání rozhraní API, které nastavit adresy analyzovat z IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="8dc5f-134">Všimněte si, že fluent konfiguraci `_builder` proměnná Určuje, že se zpracovat požadavky `appFunc` definovaný dříve v metodě.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="8dc5f-135">To `Func` je volána pro každý požadavek na zpracování příchozích požadavků.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="8dc5f-136">Přidáme také `IWebHostBuilder` rozšíření, aby byly snadno přidávat a konfigurovat Nowin server.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

<span data-ttu-id="8dc5f-137">Díky tomu na místě vyvolat rozšíření v *Program.cs* ke spuštění aplikace ASP.NET Core pomocí tohoto vlastního serveru:</span><span class="sxs-lookup"><span data-stu-id="8dc5f-137">With this in place, invoke the extension in *Program.cs* to run an ASP.NET Core app using this custom server:</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}
```

<span data-ttu-id="8dc5f-138">Další informace o ASP.NET [servery](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="8dc5f-138">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="8dc5f-139">Spustit ASP.NET Core na server s procesorem OWIN a použít jeho podporu Websocket</span><span class="sxs-lookup"><span data-stu-id="8dc5f-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="8dc5f-140">Další příklad, jak na základě OWIN servery funkcí mohou využívat technologie ASP.NET Core je přístup k funkcím, jako jsou objekty Websocket.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="8dc5f-141">Webový server .NET OWIN použitých v předchozím příkladu obsahuje podporu pro webové sokety součástí, které mohou využívat aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="8dc5f-142">Následující příklad ukazuje jednoduché webové aplikace, která podporuje objekty Websocket a vrátí zpět vše, co odeslána pro server Websocket.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

<span data-ttu-id="8dc5f-143">To [ukázka](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) je nakonfigurovaný pomocí stejných `NowinServer` jako předchozí - jediný rozdíl je v konfiguraci aplikace v jeho `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-143">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="8dc5f-144">Test pomocí [jednoduchého objektu websocket na straně klienta](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) ukazuje aplikace:</span><span class="sxs-lookup"><span data-stu-id="8dc5f-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Testovací klient webových soketů](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="8dc5f-146">Prostředí OWIN</span><span class="sxs-lookup"><span data-stu-id="8dc5f-146">OWIN environment</span></span>

<span data-ttu-id="8dc5f-147">Prostředí OWIN pomocí můžete sestavit `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-147">You can construct an OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="8dc5f-148">Klíče OWIN</span><span class="sxs-lookup"><span data-stu-id="8dc5f-148">OWIN keys</span></span>

<span data-ttu-id="8dc5f-149">OWIN závisí `IDictionary<string,object>` objekt ke sdělování informací v celé výměně požadavků/odpovědí HTTP.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="8dc5f-150">ASP.NET Core implementuje klíče uvedených níže.</span><span class="sxs-lookup"><span data-stu-id="8dc5f-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="8dc5f-151">Zobrazit [primární specifikace, rozšíření](http://owin.org/#spec), a [pokyny klíč OWIN a společné klíče](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="8dc5f-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="8dc5f-152">Data žádosti (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-152">Request data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="8dc5f-153">Key</span><span class="sxs-lookup"><span data-stu-id="8dc5f-153">Key</span></span>               | <span data-ttu-id="8dc5f-154">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-154">Value (type)</span></span> | <span data-ttu-id="8dc5f-155">Popis</span><span class="sxs-lookup"><span data-stu-id="8dc5f-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8dc5f-156">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="8dc5f-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="8dc5f-157">owin. RequestMethod</span><span class="sxs-lookup"><span data-stu-id="8dc5f-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="8dc5f-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="8dc5f-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="8dc5f-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="8dc5f-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="8dc5f-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="8dc5f-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="8dc5f-161">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="8dc5f-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="8dc5f-162">owin. RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="8dc5f-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="8dc5f-163">owin. Includesearchresults: true</span><span class="sxs-lookup"><span data-stu-id="8dc5f-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="8dc5f-164">Data žádosti (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-164">Request data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="8dc5f-165">Key</span><span class="sxs-lookup"><span data-stu-id="8dc5f-165">Key</span></span>               | <span data-ttu-id="8dc5f-166">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-166">Value (type)</span></span> | <span data-ttu-id="8dc5f-167">Popis</span><span class="sxs-lookup"><span data-stu-id="8dc5f-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8dc5f-168">owin. ID žádosti</span><span class="sxs-lookup"><span data-stu-id="8dc5f-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="8dc5f-169">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="8dc5f-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="8dc5f-170">Data odpovědi (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-170">Response data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="8dc5f-171">Key</span><span class="sxs-lookup"><span data-stu-id="8dc5f-171">Key</span></span>               | <span data-ttu-id="8dc5f-172">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-172">Value (type)</span></span> | <span data-ttu-id="8dc5f-173">Popis</span><span class="sxs-lookup"><span data-stu-id="8dc5f-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8dc5f-174">owin. ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="8dc5f-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="8dc5f-175">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="8dc5f-175">Optional</span></span> |
| <span data-ttu-id="8dc5f-176">owin. ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="8dc5f-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="8dc5f-177">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="8dc5f-177">Optional</span></span> |
| <span data-ttu-id="8dc5f-178">owin. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="8dc5f-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="8dc5f-179">owin. ResponseBody</span><span class="sxs-lookup"><span data-stu-id="8dc5f-179">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="8dc5f-180">Další data (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-180">Other data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="8dc5f-181">Key</span><span class="sxs-lookup"><span data-stu-id="8dc5f-181">Key</span></span>               | <span data-ttu-id="8dc5f-182">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-182">Value (type)</span></span> | <span data-ttu-id="8dc5f-183">Popis</span><span class="sxs-lookup"><span data-stu-id="8dc5f-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8dc5f-184">owin. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="8dc5f-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="8dc5f-185">owin. Verze</span><span class="sxs-lookup"><span data-stu-id="8dc5f-185">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="8dc5f-186">Společné klíče</span><span class="sxs-lookup"><span data-stu-id="8dc5f-186">Common keys</span></span>

| <span data-ttu-id="8dc5f-187">Key</span><span class="sxs-lookup"><span data-stu-id="8dc5f-187">Key</span></span>               | <span data-ttu-id="8dc5f-188">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-188">Value (type)</span></span> | <span data-ttu-id="8dc5f-189">Popis</span><span class="sxs-lookup"><span data-stu-id="8dc5f-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8dc5f-190">protokol SSL. ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="8dc5f-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="8dc5f-191">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="8dc5f-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="8dc5f-192">Server. Vzdálená_adresa_ip</span><span class="sxs-lookup"><span data-stu-id="8dc5f-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="8dc5f-193">Server. Vzdálený port</span><span class="sxs-lookup"><span data-stu-id="8dc5f-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="8dc5f-194">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="8dc5f-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="8dc5f-195">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="8dc5f-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="8dc5f-196">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="8dc5f-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="8dc5f-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="8dc5f-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="8dc5f-198">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="8dc5f-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="8dc5f-199">Key</span><span class="sxs-lookup"><span data-stu-id="8dc5f-199">Key</span></span>               | <span data-ttu-id="8dc5f-200">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-200">Value (type)</span></span> | <span data-ttu-id="8dc5f-201">Popis</span><span class="sxs-lookup"><span data-stu-id="8dc5f-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8dc5f-202">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="8dc5f-202">sendfile.SendAsync</span></span> | <span data-ttu-id="8dc5f-203">Zobrazit [signatura delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="8dc5f-204">Každý požadavek</span><span class="sxs-lookup"><span data-stu-id="8dc5f-204">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="8dc5f-205">Neprůhledný v0.3.0</span><span class="sxs-lookup"><span data-stu-id="8dc5f-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="8dc5f-206">Key</span><span class="sxs-lookup"><span data-stu-id="8dc5f-206">Key</span></span>               | <span data-ttu-id="8dc5f-207">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-207">Value (type)</span></span> | <span data-ttu-id="8dc5f-208">Popis</span><span class="sxs-lookup"><span data-stu-id="8dc5f-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8dc5f-209">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="8dc5f-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="8dc5f-210">neprůhledný. Upgrade</span><span class="sxs-lookup"><span data-stu-id="8dc5f-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="8dc5f-211">Zobrazit [signatura delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="8dc5f-212">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="8dc5f-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="8dc5f-213">neprůhledný. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="8dc5f-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="8dc5f-214">Protokol WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="8dc5f-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="8dc5f-215">Key</span><span class="sxs-lookup"><span data-stu-id="8dc5f-215">Key</span></span>               | <span data-ttu-id="8dc5f-216">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-216">Value (type)</span></span> | <span data-ttu-id="8dc5f-217">Popis</span><span class="sxs-lookup"><span data-stu-id="8dc5f-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8dc5f-218">objekt websocket. Verze</span><span class="sxs-lookup"><span data-stu-id="8dc5f-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="8dc5f-219">objekt websocket. Přijmout</span><span class="sxs-lookup"><span data-stu-id="8dc5f-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="8dc5f-220">Zobrazit [signatura delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="8dc5f-221">objekt websocket. AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="8dc5f-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="8dc5f-222">Bez specifikace</span><span class="sxs-lookup"><span data-stu-id="8dc5f-222">Non-spec</span></span> |
| <span data-ttu-id="8dc5f-223">objekt websocket. Dílčí protokol</span><span class="sxs-lookup"><span data-stu-id="8dc5f-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="8dc5f-224">Zobrazit [RFC6455 části 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) krok 5.5</span><span class="sxs-lookup"><span data-stu-id="8dc5f-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="8dc5f-225">objekt websocket. SendAsync</span><span class="sxs-lookup"><span data-stu-id="8dc5f-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="8dc5f-226">Zobrazit [signatura delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="8dc5f-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="8dc5f-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="8dc5f-228">Zobrazit [signatura delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="8dc5f-229">objekt websocket. CloseAsync</span><span class="sxs-lookup"><span data-stu-id="8dc5f-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="8dc5f-230">Zobrazit [signatura delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="8dc5f-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="8dc5f-231">objekt websocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="8dc5f-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="8dc5f-232">objekt websocket. ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="8dc5f-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="8dc5f-233">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="8dc5f-233">Optional</span></span> |
| <span data-ttu-id="8dc5f-234">objekt websocket. ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="8dc5f-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="8dc5f-235">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="8dc5f-235">Optional</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="8dc5f-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8dc5f-236">Additional resources</span></span>

* [<span data-ttu-id="8dc5f-237">Middleware</span><span class="sxs-lookup"><span data-stu-id="8dc5f-237">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="8dc5f-238">Servery</span><span class="sxs-lookup"><span data-stu-id="8dc5f-238">Servers</span></span>](xref:fundamentals/servers/index)
