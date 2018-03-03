---
title: "Spustit nástroj webové rozhraní pro platformu .NET (OWIN)"
author: ardalis
description: "Zjistit, jak ASP.NET Core podporuje Open Web Interface pro .NET (OWIN), což umožňuje webových aplikací pro být odděleno od webové servery."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/owin
ms.openlocfilehash: 5cb5236335e71a02452788ced4e49de36092088c
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="d4953-103">Úvod do spustit nástroj webové rozhraní pro platformu .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="d4953-103">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="d4953-104">Podle [Steve Smith](https://ardalis.com/) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d4953-104">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d4953-105">Jádro ASP.NET podporuje Open Web Interface pro .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="d4953-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="d4953-106">OWIN umožňuje webových aplikací pro být odděleno od webové servery.</span><span class="sxs-lookup"><span data-stu-id="d4953-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="d4953-107">Definuje standardní způsob pro middleware, který se má použít v kanálu zpracování požadavků a související odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d4953-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="d4953-108">Aplikace ASP.NET Core a middleware dokáže spolupracovat s aplikací OWIN, servery a middleware.</span><span class="sxs-lookup"><span data-stu-id="d4953-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="d4953-109">OWIN poskytuje oddělovací vrstva, která umožňuje dvě rozhraní s různorodých objektové modely pro společné použití.</span><span class="sxs-lookup"><span data-stu-id="d4953-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="d4953-110">`Microsoft.AspNetCore.Owin` Balíček poskytuje dva adaptér implementace:</span><span class="sxs-lookup"><span data-stu-id="d4953-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="d4953-111">Jádro ASP.NET, které OWIN</span><span class="sxs-lookup"><span data-stu-id="d4953-111">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="d4953-112">OWIN na jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d4953-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="d4953-113">To umožňuje ASP.NET Core pro hostování nad OWIN kompatibilní server nebo počítač, nebo pro jiné komponenty OWIN kompatibilní běžela nad ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4953-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="d4953-114">Poznámka: Použití těchto adaptérů dodává s nákladů na výkon.</span><span class="sxs-lookup"><span data-stu-id="d4953-114">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="d4953-115">Aplikace, které používají pouze komponenty ASP.NET Core neměli používat balíček Owin nebo adaptéry.</span><span class="sxs-lookup"><span data-stu-id="d4953-115">Applications using only ASP.NET Core components shouldn't use the Owin package or adapters.</span></span>

<span data-ttu-id="d4953-116">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d4953-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="d4953-117">Spuštění OWIN middleware v kanálu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d4953-117">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="d4953-118">Podpora OWIN ASP.NET Core je nasazen jako součást `Microsoft.AspNetCore.Owin` balíčku.</span><span class="sxs-lookup"><span data-stu-id="d4953-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="d4953-119">Podpora OWIN můžete importovat do projektu instalaci tohoto balíčku.</span><span class="sxs-lookup"><span data-stu-id="d4953-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="d4953-120">Middlewaru OWIN, který odpovídá [specifikace OWIN](http://owin.org/spec/spec/owin-1.0.0.html), což vyžaduje, aby `Func<IDictionary<string, object>, Task>` nastavit rozhraní a konkrétní klíče (například `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="d4953-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="d4953-121">Následující jednoduché middlewaru OWIN, který se zobrazí "Hello World":</span><span class="sxs-lookup"><span data-stu-id="d4953-121">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="d4953-122">Vrátí podpis ukázka `Task` a přijímá `IDictionary<string, object>` podle požadavku OWIN.</span><span class="sxs-lookup"><span data-stu-id="d4953-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="d4953-123">Následující kód ukazuje, jak přidat `OwinHello` middleware (viz výše) do kanálu ASP.NET s `UseOwin` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d4953-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="d4953-124">Můžete nakonfigurovat další akce provést v rámci kanálu OWIN.</span><span class="sxs-lookup"><span data-stu-id="d4953-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="d4953-125">Hlavičky odpovědi by měl být upraven pouze před prvním zápisu do datového proudu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d4953-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="d4953-126">Více volá, aby se `UseOwin` z důvodů výkonu se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="d4953-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="d4953-127">Komponenty OWIN bude nejlépe fungovat, pokud seskupeny dohromady.</span><span class="sxs-lookup"><span data-stu-id="d4953-127">OWIN components will operate best if grouped together.</span></span>

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

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="d4953-128">Pomocí hostování prostředí ASP.NET na serveru na základě OWIN</span><span class="sxs-lookup"><span data-stu-id="d4953-128">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="d4953-129">Na základě OWIN servery můžou hostovat aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d4953-129">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="d4953-130">Je jeden takový server [Nowin](https://github.com/Bobris/Nowin), webový server .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="d4953-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="d4953-131">V ukázce pro v tomto článku jste projekt, který odkazuje na Nowin a používá k vytvoření připojená `IServer` může být hostitelem samoobslužné ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4953-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="d4953-132">`IServer` je rozhraní, které vyžaduje `Features` vlastnost a `Start` metoda.</span><span class="sxs-lookup"><span data-stu-id="d4953-132">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="d4953-133">`Start` je zodpovědná za konfiguraci a spuštění na server, který v tomto případě se provádí pomocí řady fluent volání rozhraní API, které nastavit adresy analyzovat z IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="d4953-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="d4953-134">Všimněte si, že fluent konfiguraci `_builder` proměnná Určuje, že bude zpracovávat požadavky `appFunc` definované dříve v metodě.</span><span class="sxs-lookup"><span data-stu-id="d4953-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="d4953-135">To `Func` se volá na každý požadavek zpracovat příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="d4953-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="d4953-136">Také přidáme `IWebHostBuilder` rozšíření usnadňují přidejte a nakonfigurujte Nowin server.</span><span class="sxs-lookup"><span data-stu-id="d4953-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

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

<span data-ttu-id="d4953-137">Pomocí této na místě, všechny možnosti, které je potřeba spustit aplikaci ASP.NET pomocí tento vlastní server volat rozšíření v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d4953-137">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

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

<span data-ttu-id="d4953-138">Další informace o ASP.NET [servery](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="d4953-138">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="d4953-139">Na serveru na základě OWIN spustit ASP.NET Core a použít jeho podporu technologie WebSockets</span><span class="sxs-lookup"><span data-stu-id="d4953-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="d4953-140">Můžete využít další příklad, jak na základě OWIN servery funkcí ASP.NET Core je přístup k funkcím jako objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="d4953-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="d4953-141">Webový server OWIN rozhraní .NET, který se používá v předchozím příkladu obsahuje podporu pro webové sokety součástí, které můžete využít aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4953-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="d4953-142">Následující příklad ukazuje jednoduché webové aplikace, která podporuje objekty Websocket a vrátí zpět všechno odeslat na server prostřednictvím objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="d4953-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

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

<span data-ttu-id="d4953-143">To [ukázka](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) je konfigurován pomocí stejné `NowinServer` jako předchozí - jediný rozdíl spočívá v tom, jak je aplikace nakonfigurovaná v jeho `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="d4953-143">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="d4953-144">Test pomocí [klienta jednoduché websocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) ukazuje aplikace:</span><span class="sxs-lookup"><span data-stu-id="d4953-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Webový soket testovacího klienta](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="d4953-146">Prostředí OWIN</span><span class="sxs-lookup"><span data-stu-id="d4953-146">OWIN environment</span></span>

<span data-ttu-id="d4953-147">Můžete vytvořit pomocí prostředí OWIN `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="d4953-147">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="d4953-148">Klíče OWIN</span><span class="sxs-lookup"><span data-stu-id="d4953-148">OWIN keys</span></span>

<span data-ttu-id="d4953-149">OWIN závisí na `IDictionary<string,object>` objekt ke sdělování informací v celé výměně požadavků a odpovědí HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4953-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="d4953-150">ASP.NET Core implementuje klíče uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="d4953-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="d4953-151">Najdete v článku [primární specifikace, rozšíření](http://owin.org/#spec), a [OWIN klíč pokyny a společné klíče](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="d4953-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="d4953-152">Data požadavku (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="d4953-152">Request data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="d4953-153">Key</span><span class="sxs-lookup"><span data-stu-id="d4953-153">Key</span></span>               | <span data-ttu-id="d4953-154">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="d4953-154">Value (type)</span></span> | <span data-ttu-id="d4953-155">Popis</span><span class="sxs-lookup"><span data-stu-id="d4953-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d4953-156">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="d4953-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="d4953-157">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="d4953-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="d4953-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="d4953-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="d4953-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="d4953-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="d4953-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="d4953-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="d4953-161">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="d4953-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="d4953-162">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="d4953-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="d4953-163">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="d4953-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="d4953-164">Data požadavku (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="d4953-164">Request data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="d4953-165">Key</span><span class="sxs-lookup"><span data-stu-id="d4953-165">Key</span></span>               | <span data-ttu-id="d4953-166">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="d4953-166">Value (type)</span></span> | <span data-ttu-id="d4953-167">Popis</span><span class="sxs-lookup"><span data-stu-id="d4953-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d4953-168">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="d4953-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="d4953-169">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="d4953-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="d4953-170">Data odpovědi (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="d4953-170">Response data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="d4953-171">Key</span><span class="sxs-lookup"><span data-stu-id="d4953-171">Key</span></span>               | <span data-ttu-id="d4953-172">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="d4953-172">Value (type)</span></span> | <span data-ttu-id="d4953-173">Popis</span><span class="sxs-lookup"><span data-stu-id="d4953-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d4953-174">owin. ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="d4953-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="d4953-175">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="d4953-175">Optional</span></span> |
| <span data-ttu-id="d4953-176">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="d4953-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="d4953-177">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="d4953-177">Optional</span></span> |
| <span data-ttu-id="d4953-178">owin. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="d4953-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="d4953-179">owin. ResponseBody</span><span class="sxs-lookup"><span data-stu-id="d4953-179">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="d4953-180">Další data (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="d4953-180">Other data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="d4953-181">Key</span><span class="sxs-lookup"><span data-stu-id="d4953-181">Key</span></span>               | <span data-ttu-id="d4953-182">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="d4953-182">Value (type)</span></span> | <span data-ttu-id="d4953-183">Popis</span><span class="sxs-lookup"><span data-stu-id="d4953-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d4953-184">owin.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="d4953-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="d4953-185">owin.Version</span><span class="sxs-lookup"><span data-stu-id="d4953-185">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="d4953-186">Běžné klíče</span><span class="sxs-lookup"><span data-stu-id="d4953-186">Common keys</span></span>

| <span data-ttu-id="d4953-187">Key</span><span class="sxs-lookup"><span data-stu-id="d4953-187">Key</span></span>               | <span data-ttu-id="d4953-188">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="d4953-188">Value (type)</span></span> | <span data-ttu-id="d4953-189">Popis</span><span class="sxs-lookup"><span data-stu-id="d4953-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d4953-190">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="d4953-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="d4953-191">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="d4953-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="d4953-192">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="d4953-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="d4953-193">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="d4953-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="d4953-194">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="d4953-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="d4953-195">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="d4953-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="d4953-196">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="d4953-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="d4953-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="d4953-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="d4953-198">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="d4953-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="d4953-199">Key</span><span class="sxs-lookup"><span data-stu-id="d4953-199">Key</span></span>               | <span data-ttu-id="d4953-200">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="d4953-200">Value (type)</span></span> | <span data-ttu-id="d4953-201">Popis</span><span class="sxs-lookup"><span data-stu-id="d4953-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d4953-202">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="d4953-202">sendfile.SendAsync</span></span> | <span data-ttu-id="d4953-203">V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="d4953-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="d4953-204">Každý požadavek</span><span class="sxs-lookup"><span data-stu-id="d4953-204">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="d4953-205">Neprůhledné v0.3.0</span><span class="sxs-lookup"><span data-stu-id="d4953-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="d4953-206">Key</span><span class="sxs-lookup"><span data-stu-id="d4953-206">Key</span></span>               | <span data-ttu-id="d4953-207">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="d4953-207">Value (type)</span></span> | <span data-ttu-id="d4953-208">Popis</span><span class="sxs-lookup"><span data-stu-id="d4953-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d4953-209">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="d4953-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="d4953-210">opaque.Upgrade</span><span class="sxs-lookup"><span data-stu-id="d4953-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="d4953-211">V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="d4953-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="d4953-212">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="d4953-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="d4953-213">neprůhledné. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="d4953-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="d4953-214">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="d4953-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="d4953-215">Key</span><span class="sxs-lookup"><span data-stu-id="d4953-215">Key</span></span>               | <span data-ttu-id="d4953-216">Hodnota (typ)</span><span class="sxs-lookup"><span data-stu-id="d4953-216">Value (type)</span></span> | <span data-ttu-id="d4953-217">Popis</span><span class="sxs-lookup"><span data-stu-id="d4953-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d4953-218">websocket.Version</span><span class="sxs-lookup"><span data-stu-id="d4953-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="d4953-219">protokol websocket. Přijmout</span><span class="sxs-lookup"><span data-stu-id="d4953-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="d4953-220">V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="d4953-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="d4953-221">protokol websocket. AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="d4953-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="d4953-222">Bez specifikace</span><span class="sxs-lookup"><span data-stu-id="d4953-222">Non-spec</span></span> |
| <span data-ttu-id="d4953-223">protokol websocket. SubProtocol</span><span class="sxs-lookup"><span data-stu-id="d4953-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="d4953-224">V tématu [RFC6455 části 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) krok 5,5</span><span class="sxs-lookup"><span data-stu-id="d4953-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="d4953-225">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="d4953-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="d4953-226">V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="d4953-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="d4953-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="d4953-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="d4953-228">V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="d4953-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="d4953-229">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="d4953-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="d4953-230">V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="d4953-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="d4953-231">protokol websocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="d4953-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="d4953-232">protokol websocket. ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="d4953-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="d4953-233">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="d4953-233">Optional</span></span> |
| <span data-ttu-id="d4953-234">protokol websocket. ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="d4953-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="d4953-235">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="d4953-235">Optional</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="d4953-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d4953-236">Additional resources</span></span>

* [<span data-ttu-id="d4953-237">Middleware</span><span class="sxs-lookup"><span data-stu-id="d4953-237">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="d4953-238">Servery</span><span class="sxs-lookup"><span data-stu-id="d4953-238">Servers</span></span>](xref:fundamentals/servers/index)
