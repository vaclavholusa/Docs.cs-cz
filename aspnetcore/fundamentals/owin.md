---
title: "Spustit nástroj webové rozhraní pro platformu .NET (OWIN)"
author: ardalis
description: "Zjistit, jak ASP.NET Core podporuje Open Web Interface pro .NET (OWIN), což umožňuje webových aplikací pro být odděleno od webové servery."
keywords: "Jádro ASP.NET, otevřete Web Interface pro .NET, OWIN"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 70c4e6bc-a773-4039-96ec-6fe557c9369d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2ee970a1c9cd05ebee76b92c3e2c7c6c6cc6ef8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="7a803-104">Úvod do spustit nástroj webové rozhraní pro platformu .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="7a803-104">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="7a803-105">Podle [Steve Smith](https://ardalis.com/) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7a803-105">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7a803-106">Jádro ASP.NET podporuje Open Web Interface pro .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="7a803-106">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="7a803-107">OWIN umožňuje webových aplikací pro být odděleno od webové servery.</span><span class="sxs-lookup"><span data-stu-id="7a803-107">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="7a803-108">Definuje standardní způsob pro middleware, který se má použít v kanálu zpracování požadavků a související odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7a803-108">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="7a803-109">Aplikace ASP.NET Core a middleware dokáže spolupracovat s aplikací OWIN, servery a middleware.</span><span class="sxs-lookup"><span data-stu-id="7a803-109">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="7a803-110">OWIN poskytuje oddělovací vrstva, která umožňuje dvě rozhraní s různorodých objektové modely pro společné použití.</span><span class="sxs-lookup"><span data-stu-id="7a803-110">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="7a803-111">`Microsoft.AspNetCore.Owin` Balíček poskytuje dva adaptér implementace:</span><span class="sxs-lookup"><span data-stu-id="7a803-111">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="7a803-112">Jádro ASP.NET, které OWIN</span><span class="sxs-lookup"><span data-stu-id="7a803-112">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="7a803-113">OWIN na jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7a803-113">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="7a803-114">To umožňuje ASP.NET Core pro hostování nad OWIN kompatibilní server nebo počítač, nebo pro jiné komponenty OWIN kompatibilní běžela nad ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a803-114">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="7a803-115">Poznámka: Použití těchto adaptérů dodává s nákladů na výkon.</span><span class="sxs-lookup"><span data-stu-id="7a803-115">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="7a803-116">Aplikace, které používají pouze komponenty ASP.NET Core neměli používat balíček Owin nebo adaptéry.</span><span class="sxs-lookup"><span data-stu-id="7a803-116">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

<span data-ttu-id="7a803-117">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7a803-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="7a803-118">Spuštění OWIN middleware v kanálu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7a803-118">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="7a803-119">Podpora OWIN ASP.NET Core je nasazen jako součást `Microsoft.AspNetCore.Owin` balíčku.</span><span class="sxs-lookup"><span data-stu-id="7a803-119">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="7a803-120">Podpora OWIN můžete importovat do projektu instalaci tohoto balíčku.</span><span class="sxs-lookup"><span data-stu-id="7a803-120">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="7a803-121">Middlewaru OWIN, který odpovídá [specifikace OWIN](http://owin.org/spec/spec/owin-1.0.0.html), což vyžaduje, aby `Func<IDictionary<string, object>, Task>` nastavit rozhraní a konkrétní klíče (například `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="7a803-121">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="7a803-122">Následující jednoduché middlewaru OWIN, který se zobrazí "Hello World":</span><span class="sxs-lookup"><span data-stu-id="7a803-122">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="7a803-123">Vrátí podpis ukázka `Task` a přijímá `IDictionary<string, object>` podle požadavku OWIN.</span><span class="sxs-lookup"><span data-stu-id="7a803-123">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="7a803-124">Následující kód ukazuje, jak přidat `OwinHello` middleware (viz výše) do kanálu ASP.NET s `UseOwin` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7a803-124">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="7a803-125">Můžete nakonfigurovat další akce provést v rámci kanálu OWIN.</span><span class="sxs-lookup"><span data-stu-id="7a803-125">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="7a803-126">Hlavičky odpovědi by měl být upraven pouze před prvním zápisu do datového proudu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7a803-126">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="7a803-127">Více volá, aby se `UseOwin` z důvodů výkonu se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="7a803-127">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="7a803-128">Komponenty OWIN bude nejlépe fungovat, pokud seskupeny dohromady.</span><span class="sxs-lookup"><span data-stu-id="7a803-128">OWIN components will operate best if grouped together.</span></span>

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="7a803-129">Pomocí hostování prostředí ASP.NET na serveru na základě OWIN</span><span class="sxs-lookup"><span data-stu-id="7a803-129">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="7a803-130">Na základě OWIN servery můžou hostovat aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7a803-130">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="7a803-131">Je jeden takový server [Nowin](https://github.com/Bobris/Nowin), webový server .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="7a803-131">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="7a803-132">V ukázce pro v tomto článku jste projekt, který odkazuje na Nowin a používá k vytvoření připojená `IServer` může být hostitelem samoobslužné ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a803-132">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="7a803-133">`IServer`je rozhraní, které vyžaduje `Features` vlastnost a `Start` metoda.</span><span class="sxs-lookup"><span data-stu-id="7a803-133">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="7a803-134">`Start`je zodpovědná za konfiguraci a spuštění na server, který v tomto případě se provádí pomocí řady fluent volání rozhraní API, které nastavit adresy analyzovat z IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="7a803-134">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="7a803-135">Všimněte si, že fluent konfiguraci `_builder` proměnná Určuje, že bude zpracovávat požadavky `appFunc` definované dříve v metodě.</span><span class="sxs-lookup"><span data-stu-id="7a803-135">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="7a803-136">To `Func` se volá na každý požadavek zpracovat příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="7a803-136">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="7a803-137">Také přidáme `IWebHostBuilder` rozšíření usnadňují přidejte a nakonfigurujte Nowin server.</span><span class="sxs-lookup"><span data-stu-id="7a803-137">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

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

<span data-ttu-id="7a803-138">Pomocí této na místě, všechny možnosti, které je potřeba spustit aplikaci ASP.NET pomocí tento vlastní server volat rozšíření v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7a803-138">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

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

<span data-ttu-id="7a803-139">Další informace o ASP.NET [servery](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="7a803-139">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="7a803-140">Na serveru na základě OWIN spustit ASP.NET Core a použít jeho podporu technologie WebSockets</span><span class="sxs-lookup"><span data-stu-id="7a803-140">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="7a803-141">Můžete využít další příklad, jak na základě OWIN servery funkcí ASP.NET Core je přístup k funkcím jako objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="7a803-141">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="7a803-142">Webový server OWIN rozhraní .NET, který se používá v předchozím příkladu obsahuje podporu pro webové sokety součástí, které můžete využít aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a803-142">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="7a803-143">Následující příklad ukazuje jednoduché webové aplikace, která podporuje objekty Websocket a vrátí zpět všechno odeslat na server prostřednictvím objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="7a803-143">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

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

<span data-ttu-id="7a803-144">To [ukázka](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) je konfigurován pomocí stejné `NowinServer` jako předchozí - jediný rozdíl spočívá v tom, jak je aplikace nakonfigurovaná v jeho `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="7a803-144">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="7a803-145">Test pomocí [klienta jednoduché websocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) ukazuje aplikace:</span><span class="sxs-lookup"><span data-stu-id="7a803-145">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Webový soket testovacího klienta](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="7a803-147">Prostředí OWIN</span><span class="sxs-lookup"><span data-stu-id="7a803-147">OWIN environment</span></span>

<span data-ttu-id="7a803-148">Můžete vytvořit pomocí prostředí OWIN `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="7a803-148">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="7a803-149">Klíče OWIN</span><span class="sxs-lookup"><span data-stu-id="7a803-149">OWIN keys</span></span>

<span data-ttu-id="7a803-150">OWIN závisí na `IDictionary<string,object>` objekt ke sdělování informací v celé výměně požadavků a odpovědí HTTP.</span><span class="sxs-lookup"><span data-stu-id="7a803-150">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="7a803-151">ASP.NET Core implementuje klíče uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="7a803-151">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="7a803-152">Najdete v článku [primární specifikace, rozšíření](http://owin.org/#spec), a [OWIN klíč pokyny a společné klíče](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="7a803-152">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="7a803-153">Data požadavku (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="7a803-153">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="7a803-154">Key</span><span class="sxs-lookup"><span data-stu-id="7a803-154">Key</span></span>               | <span data-ttu-id="7a803-155">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="7a803-155">Value (type)</span></span> | <span data-ttu-id="7a803-156">Popis</span><span class="sxs-lookup"><span data-stu-id="7a803-156">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7a803-157">owin. RequestScheme</span><span class="sxs-lookup"><span data-stu-id="7a803-157">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="7a803-158">owin. RequestMethod</span><span class="sxs-lookup"><span data-stu-id="7a803-158">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="7a803-159">owin. RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="7a803-159">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="7a803-160">owin. RequestPath</span><span class="sxs-lookup"><span data-stu-id="7a803-160">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="7a803-161">owin. RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="7a803-161">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="7a803-162">owin. RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="7a803-162">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="7a803-163">owin. RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="7a803-163">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="7a803-164">owin. RequestBody</span><span class="sxs-lookup"><span data-stu-id="7a803-164">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="7a803-165">Data požadavku (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="7a803-165">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="7a803-166">Key</span><span class="sxs-lookup"><span data-stu-id="7a803-166">Key</span></span>               | <span data-ttu-id="7a803-167">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="7a803-167">Value (type)</span></span> | <span data-ttu-id="7a803-168">Popis</span><span class="sxs-lookup"><span data-stu-id="7a803-168">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7a803-169">owin. ID žádosti</span><span class="sxs-lookup"><span data-stu-id="7a803-169">owin.RequestId</span></span> | `String` | <span data-ttu-id="7a803-170">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="7a803-170">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="7a803-171">Data odpovědi (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="7a803-171">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="7a803-172">Key</span><span class="sxs-lookup"><span data-stu-id="7a803-172">Key</span></span>               | <span data-ttu-id="7a803-173">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="7a803-173">Value (type)</span></span> | <span data-ttu-id="7a803-174">Popis</span><span class="sxs-lookup"><span data-stu-id="7a803-174">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7a803-175">owin. ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="7a803-175">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="7a803-176">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="7a803-176">Optional</span></span> |
| <span data-ttu-id="7a803-177">owin. ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="7a803-177">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="7a803-178">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="7a803-178">Optional</span></span> |
| <span data-ttu-id="7a803-179">owin. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="7a803-179">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="7a803-180">owin. ResponseBody</span><span class="sxs-lookup"><span data-stu-id="7a803-180">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="7a803-181">Další Data (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="7a803-181">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="7a803-182">Key</span><span class="sxs-lookup"><span data-stu-id="7a803-182">Key</span></span>               | <span data-ttu-id="7a803-183">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="7a803-183">Value (type)</span></span> | <span data-ttu-id="7a803-184">Popis</span><span class="sxs-lookup"><span data-stu-id="7a803-184">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7a803-185">owin. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="7a803-185">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="7a803-186">owin. Verze</span><span class="sxs-lookup"><span data-stu-id="7a803-186">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="7a803-187">Běžné klíče</span><span class="sxs-lookup"><span data-stu-id="7a803-187">Common Keys</span></span>

| <span data-ttu-id="7a803-188">Key</span><span class="sxs-lookup"><span data-stu-id="7a803-188">Key</span></span>               | <span data-ttu-id="7a803-189">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="7a803-189">Value (type)</span></span> | <span data-ttu-id="7a803-190">Popis</span><span class="sxs-lookup"><span data-stu-id="7a803-190">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7a803-191">protokol SSL. ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="7a803-191">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="7a803-192">protokol SSL. LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="7a803-192">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="7a803-193">Server. Vzdálená_adresa_ip</span><span class="sxs-lookup"><span data-stu-id="7a803-193">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="7a803-194">Server. Vzdálený port</span><span class="sxs-lookup"><span data-stu-id="7a803-194">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="7a803-195">Server. Místní_adresa_ip</span><span class="sxs-lookup"><span data-stu-id="7a803-195">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="7a803-196">Server. Místní_port</span><span class="sxs-lookup"><span data-stu-id="7a803-196">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="7a803-197">Server. IsLocal</span><span class="sxs-lookup"><span data-stu-id="7a803-197">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="7a803-198">Server. OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="7a803-198">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="7a803-199">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="7a803-199">SendFiles v0.3.0</span></span>

| <span data-ttu-id="7a803-200">Key</span><span class="sxs-lookup"><span data-stu-id="7a803-200">Key</span></span>               | <span data-ttu-id="7a803-201">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="7a803-201">Value (type)</span></span> | <span data-ttu-id="7a803-202">Popis</span><span class="sxs-lookup"><span data-stu-id="7a803-202">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7a803-203">sendfile. SendAsync</span><span class="sxs-lookup"><span data-stu-id="7a803-203">sendfile.SendAsync</span></span> | <span data-ttu-id="7a803-204">V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="7a803-204">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="7a803-205">Každý požadavek</span><span class="sxs-lookup"><span data-stu-id="7a803-205">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="7a803-206">Neprůhledné v0.3.0</span><span class="sxs-lookup"><span data-stu-id="7a803-206">Opaque v0.3.0</span></span>

| <span data-ttu-id="7a803-207">Key</span><span class="sxs-lookup"><span data-stu-id="7a803-207">Key</span></span>               | <span data-ttu-id="7a803-208">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="7a803-208">Value (type)</span></span> | <span data-ttu-id="7a803-209">Popis</span><span class="sxs-lookup"><span data-stu-id="7a803-209">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7a803-210">neprůhledné. Verze</span><span class="sxs-lookup"><span data-stu-id="7a803-210">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="7a803-211">neprůhledné. Upgrade</span><span class="sxs-lookup"><span data-stu-id="7a803-211">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="7a803-212">V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="7a803-212">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="7a803-213">neprůhledné. Datový proud</span><span class="sxs-lookup"><span data-stu-id="7a803-213">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="7a803-214">neprůhledné. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="7a803-214">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="7a803-215">V0.3.0 protokolu WebSocket</span><span class="sxs-lookup"><span data-stu-id="7a803-215">WebSocket v0.3.0</span></span>

| <span data-ttu-id="7a803-216">Key</span><span class="sxs-lookup"><span data-stu-id="7a803-216">Key</span></span>               | <span data-ttu-id="7a803-217">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="7a803-217">Value (type)</span></span> | <span data-ttu-id="7a803-218">Popis</span><span class="sxs-lookup"><span data-stu-id="7a803-218">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7a803-219">protokol websocket. Verze</span><span class="sxs-lookup"><span data-stu-id="7a803-219">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="7a803-220">protokol websocket. Přijmout</span><span class="sxs-lookup"><span data-stu-id="7a803-220">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="7a803-221">V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="7a803-221">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="7a803-222">protokol websocket. AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="7a803-222">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="7a803-223">Bez specifikace</span><span class="sxs-lookup"><span data-stu-id="7a803-223">Non-spec</span></span> |
| <span data-ttu-id="7a803-224">protokol websocket. SubProtocol</span><span class="sxs-lookup"><span data-stu-id="7a803-224">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="7a803-225">V tématu [RFC6455 části 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) krok 5,5</span><span class="sxs-lookup"><span data-stu-id="7a803-225">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="7a803-226">protokol websocket. SendAsync</span><span class="sxs-lookup"><span data-stu-id="7a803-226">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="7a803-227">V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="7a803-227">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="7a803-228">protokol websocket. ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="7a803-228">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="7a803-229">V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="7a803-229">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="7a803-230">protokol websocket. CloseAsync</span><span class="sxs-lookup"><span data-stu-id="7a803-230">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="7a803-231">V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="7a803-231">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="7a803-232">protokol websocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="7a803-232">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="7a803-233">protokol websocket. ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="7a803-233">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="7a803-234">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="7a803-234">Optional</span></span> |
| <span data-ttu-id="7a803-235">protokol websocket. ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="7a803-235">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="7a803-236">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="7a803-236">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="7a803-237">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="7a803-237">Additional Resources</span></span>

* [<span data-ttu-id="7a803-238">Middleware</span><span class="sxs-lookup"><span data-stu-id="7a803-238">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="7a803-239">Servery</span><span class="sxs-lookup"><span data-stu-id="7a803-239">Servers</span></span>](servers/index.md)
