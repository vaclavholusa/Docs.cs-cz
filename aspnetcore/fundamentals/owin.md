---
title: Spustit nástroj webové rozhraní pro platformu .NET (OWIN) s ASP.NET Core
author: ardalis
description: Zjistit, jak ASP.NET Core podporuje Open Web Interface pro .NET (OWIN), což umožňuje webových aplikací pro být odděleno od webové servery.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
uid: fundamentals/owin
ms.openlocfilehash: 864580edd62032ad1409c1d3263cb5d464fa59fe
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273621"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a>Spustit nástroj webové rozhraní pro platformu .NET (OWIN) s ASP.NET Core

Podle [Steve Smith](https://ardalis.com/) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Jádro ASP.NET podporuje Open Web Interface pro .NET (OWIN). OWIN umožňuje webových aplikací pro být odděleno od webové servery. Definuje standardní způsob pro middleware, který se má použít v kanálu zpracování požadavků a související odpovědi. Aplikace ASP.NET Core a middleware dokáže spolupracovat s aplikací OWIN, servery a middleware.

OWIN poskytuje oddělovací vrstva, která umožňuje dvě rozhraní s různorodých objektové modely pro společné použití. `Microsoft.AspNetCore.Owin` Balíček poskytuje dva adaptér implementace:
- Jádro ASP.NET, které OWIN 
- OWIN na jádro ASP.NET

To umožňuje ASP.NET Core pro hostování nad OWIN kompatibilní server nebo počítač, nebo pro jiné komponenty OWIN kompatibilní běžela nad ASP.NET Core.

Poznámka: Použití těchto adaptérů dodává s nákladů na výkon. Aplikace, které používají pouze komponenty ASP.NET Core neměli používat balíček Owin nebo adaptéry.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>Spuštění OWIN middleware v kanálu ASP.NET

Podpora OWIN ASP.NET Core je nasazen jako součást `Microsoft.AspNetCore.Owin` balíčku. Podpora OWIN můžete importovat do projektu instalaci tohoto balíčku.

Middlewaru OWIN, který odpovídá [specifikace OWIN](http://owin.org/spec/spec/owin-1.0.0.html), což vyžaduje, aby `Func<IDictionary<string, object>, Task>` nastavit rozhraní a konkrétní klíče (například `owin.ResponseBody`). Následující jednoduché middlewaru OWIN, který se zobrazí "Hello World":

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

Vrátí podpis ukázka `Task` a přijímá `IDictionary<string, object>` podle požadavku OWIN.

Následující kód ukazuje, jak přidat `OwinHello` middleware (viz výše) do kanálu ASP.NET s `UseOwin` metoda rozšíření.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

Můžete nakonfigurovat další akce provést v rámci kanálu OWIN.

> [!NOTE]
> Hlavičky odpovědi by měl být upraven pouze před prvním zápisu do datového proudu odpovědi.

> [!NOTE]
> Více volá, aby se `UseOwin` z důvodů výkonu se nedoporučuje. Komponenty OWIN bude nejlépe fungovat, pokud seskupeny dohromady.

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

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>Pomocí hostování prostředí ASP.NET na serveru na základě OWIN

Na základě OWIN servery můžou hostovat aplikace ASP.NET. Je jeden takový server [Nowin](https://github.com/Bobris/Nowin), webový server .NET OWIN. V ukázce pro v tomto článku jste projekt, který odkazuje na Nowin a používá k vytvoření připojená `IServer` může být hostitelem samoobslužné ASP.NET Core.

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer` je rozhraní, které vyžaduje `Features` vlastnost a `Start` metoda.

`Start` je zodpovědná za konfiguraci a spuštění na server, který v tomto případě se provádí pomocí řady fluent volání rozhraní API, které nastavit adresy analyzovat z IServerAddressesFeature. Všimněte si, že fluent konfiguraci `_builder` proměnná Určuje, že bude zpracovávat požadavky `appFunc` definované dříve v metodě. To `Func` se volá na každý požadavek zpracovat příchozí požadavky.

Také přidáme `IWebHostBuilder` rozšíření usnadňují přidejte a nakonfigurujte Nowin server.

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

S tímto zavedené vyvolat rozšíření v *Program.cs* spouštět aplikace ASP.NET Core pomocí tento vlastní server:

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

Další informace o ASP.NET [servery](servers/index.md).

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>Na serveru na základě OWIN spustit ASP.NET Core a použít jeho podporu technologie WebSockets

Můžete využít další příklad, jak na základě OWIN servery funkcí ASP.NET Core je přístup k funkcím jako objekty WebSockets. Webový server OWIN rozhraní .NET, který se používá v předchozím příkladu obsahuje podporu pro webové sokety součástí, které můžete využít aplikaci ASP.NET Core. Následující příklad ukazuje jednoduché webové aplikace, která podporuje objekty Websocket a vrátí zpět všechno odeslat na server prostřednictvím objekty WebSockets.

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

To [ukázka](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) je konfigurován pomocí stejné `NowinServer` jako předchozí - jediný rozdíl spočívá v tom, jak je aplikace nakonfigurovaná v jeho `Configure` metoda. Test pomocí [klienta jednoduché websocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) ukazuje aplikace:

![Webový soket testovacího klienta](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>Prostředí OWIN

Můžete vytvořit prostředí OWIN pomocí `HttpContext`.

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>Klíče OWIN

OWIN závisí na `IDictionary<string,object>` objekt ke sdělování informací v celé výměně požadavků a odpovědí HTTP. ASP.NET Core implementuje klíče uvedené níže. Najdete v článku [primární specifikace, rozšíření](http://owin.org/#spec), a [OWIN klíč pokyny a společné klíče](http://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>Data požadavku (OWIN v1.0.0)

| Key               | Hodnota (typ) | Popis |
| ----------------- | ------------ | ----------- |
| owin.RequestScheme | `String` |  |
| owin. RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin.RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owin.RequestProtocol  | `String` | |    
| owin. RequestHeaders | `IDictionary<string,string[]>`  | |
| owin. RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>Data požadavku (OWIN v1.1.0)

| Key               | Hodnota (typ) | Popis |
| ----------------- | ------------ | ----------- |
| owin. ID žádosti | `String` | Nepovinné |

### <a name="response-data-owin-v100"></a>Data odpovědi (OWIN v1.0.0)

| Key               | Hodnota (typ) | Popis |
| ----------------- | ------------ | ----------- |
| owin. ResponseStatusCode | `int` | Nepovinné |
| owin. ResponseReasonPhrase | `String` | Nepovinné |
| owin. ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin. ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>Další data (OWIN v1.0.0)

| Key               | Hodnota (typ) | Popis |
| ----------------- | ------------ | ----------- |
| owin. CallCancelled | `CancellationToken` |  |
| owin. Verze  | `String` | |   


### <a name="common-keys"></a>Běžné klíče

| Key               | Hodnota (typ) | Popis |
| ----------------- | ------------ | ----------- |
| protokol SSL. ClientCertificate | `X509Certificate` |  |
| ssl.LoadClientCertAsync  | `Func<Task>` | |    
| Server. Vzdálená_adresa_ip  | `String` | |    
| Server. Vzdálený port | `String` | |     
| server.LocalIpAddress  | `String` | |    
| server.LocalPort  | `String` | |    
| server.IsLocal  | `bool` | |    
| server.OnSendingHeaders  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| Key               | Hodnota (typ) | Popis |
| ----------------- | ------------ | ----------- |
| sendfile.SendAsync | V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | Každý požadavek |


### <a name="opaque-v030"></a>Neprůhledné v0.3.0

| Key               | Hodnota (typ) | Popis |
| ----------------- | ------------ | ----------- |
| opaque.Version | `String` |  |
| neprůhledné. Upgrade | `OpaqueUpgrade` | V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| opaque.Stream | `Stream` |  |
| neprůhledné. CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>V0.3.0 protokolu WebSocket

| Key               | Hodnota (typ) | Popis |
| ----------------- | ------------ | ----------- |
| protokol websocket. Verze | `String` |  |
| protokol websocket. Přijmout | `WebSocketAccept` | V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| protokol websocket. AcceptAlt |  | Bez specifikace |
| protokol websocket. SubProtocol | `String` | V tématu [RFC6455 části 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) krok 5,5 |
| protokol websocket. SendAsync | `WebSocketSendAsync` | V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| protokol websocket. CloseAsync | `WebSocketCloseAsync` | V tématu [podpisu delegáta](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| protokol websocket. CallCancelled | `CancellationToken` |  |
| protokol websocket. ClientCloseStatus | `int` | Nepovinné |
| protokol websocket. ClientCloseDescription | `String` | Nepovinné |

## <a name="additional-resources"></a>Další zdroje

* [Middleware](xref:fundamentals/middleware/index)
* [Servery](xref:fundamentals/servers/index)
