---
title: Rozdíly mezi SignalR a funkce SignalR technologie ASP.NET Core
author: tdykstra
description: Rozdíly mezi SignalR a funkce SignalR technologie ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 2f3458f27fd7f22339751e0734dd8c5da709a3c0
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340118"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>Rozdíly mezi funkce SignalR technologie ASP.NET a technologie SignalR technologie ASP.NET Core

Funkce SignalR technologie ASP.NET Core není kompatibilní s klientů nebo serverů pro funkci SignalR technologie ASP.NET. Tento článek podrobně popisuje funkce, které byly odstraněny nebo změněny v knihovně SignalR technologie ASP.NET Core.

## <a name="how-to-identify-the-signalr-version"></a>Jak určit verzi SignalR

|                      | ASP.NET SignalR | Funkce SignalR technologie ASP.NET Core |
| -------------------- | --------------- | -------------------- |
| Balíček NuGet server | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Balíčky NuGet pro klienta | [Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Npm Package klienta | [Funkce signalr](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Typ aplikace serveru | Technologie ASP.NET (System.Web) nebo samoobslužné hostování OWIN | ASP.NET Core |
| Podporované serverové platformy | Rozhraní .NET framework 4.5 nebo novější | Rozhraní .NET framework 4.6.1 nebo novější<br>.NET core 2.1 nebo novější |

## <a name="feature-differences"></a>Rozdíly ve funkcích

### <a name="automatic-reconnects"></a>Automatické připojování

Automatické připojování již nejsou podporovány. Dříve SignalR pokusil připojit k serveru, pokud připojení bylo zrušeno. Když teď aplikaci filled klient je odpojen, uživatel musí explicitně spustit nové připojení, pokud se chcete znovu připojit.

### <a name="protocol-support"></a>Podpora protokolu

Funkce SignalR technologie ASP.NET Core podporuje JSON, jakož i nové binární protokol založený na [MessagePack](xref:signalr/messagepackhubprotocol). Kromě toho je možné vytvářet vlastní protokoly.

## <a name="differences-on-the-server"></a>Rozdíly na serveru

Jsou součástí knihoven na straně serveru funkce SignalR technologie ASP.NET Core [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) balíček, který je součástí **webové aplikace ASP.NET Core** šablony pro syntaxi Razor a MVC projekty.

Funkce SignalR technologie ASP.NET Core je middleware ASP.NET Core, takže musí být nakonfigurovaný pomocí volání [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) v `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

Konfigurace směrování, mapování tras k rozbočovačům uvnitř [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) volání metody `Startup.Configure` metoda.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Nyní vyžaduje rychlé relace

Z důvodu jak horizontální navýšení kapacity pracoval jsem s se funkce SignalR technologie ASP.NET klienti znovu připojit a odesílání zpráv na libovolný server ve farmě. Z důvodu změny na model horizontální navýšení kapacity, jakož i nenabízí připojování to se už nepodporuje. Jakmile se klient připojí k serveru, musí pracovat na stejném serveru po dobu trvání připojení.

### <a name="single-hub-per-connection"></a>Jedno Centrum za připojení

Funkce SignalR technologie ASP.NET Core je zjednodušený model připojení. Připojení jsou provedeny přímo do jednoho rozbočovače, nikoli jednoho připojení používá sdílet přístup k více rozbočovače.

### <a name="streaming"></a>Streamování

ASP.NET Core SignalR teď podporuje [streamovaná data](xref:signalr/streaming) z rozbočovače klientovi.

### <a name="state"></a>Stav

Umožňuje předat libovolný stav mezi klienty a centrum (často označované jako HubState) byla odebrána a také podporu pro zprávy o průběhu. V tuto chvíli není nevyskytují proxy rozbočovače.

## <a name="differences-on-the-client"></a>Rozdíly v klientovi

### <a name="typescript"></a>TypeScript

Klient funkce SignalR technologie ASP.NET Core je napsána v [TypeScript](https://www.typescriptlang.org/). Můžete psát v jazyce JavaScript nebo TypeScript při použití [javascriptový klient](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>JavaScript klienta je hostovaná na [npm](https://www.npmjs.com/)

V předchozích verzích klienta JavaScript byl získán prostřednictvím balíčku NuGet v sadě Visual Studio. Verze jádra [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) balíčku npm obsahuje knihovny jazyka JavaScript. Není součástí tohoto balíčku **webové aplikace ASP.NET Core** šablony. Získejte a nainstalujte pomocí npm `@aspnet/signalr` balíčku npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

Závislost na jQuery byla odebrána, ale projekty můžete dál používat jQuery.

### <a name="javascript-client-method-syntax"></a>Syntaxe využívající metody JavaScript klienta

Syntaxe jazyka JavaScript byl změněn z předchozí verze funkce SignalR. Místo použití `$connection` objektu, vytvořte pomocí připojení [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) rozhraní API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Použití [na](/javascript/api/@aspnet/signalr/HubConnection#on) metoda určují metody klienta můžete volání rozbočovače.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Po vytvoření metody klienta, spusťte připojení rozbočovače. Řetězce [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodou protokolu nebo zpracování chyb.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Proxy servery hub

Proxy servery hub už nebude automaticky generovány. Místo toho název metody je předán [vyvolat](/javascript/api/%40aspnet/signalr/hubconnection#invoke) rozhraní API jako řetězec.

### <a name="net-and-other-clients"></a>.NET a další klienti

`Microsoft.AspNetCore.SignalR.Client` Balíček NuGet obsahuje klientské knihovny .NET pro funkci SignalR technologie ASP.NET Core.

Použití [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) vytvářet a sestavovat instanci, připojení k rozbočovači.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Rozdíly horizontálním navýšením kapacity

Funkce SignalR technologie ASP.NET podporuje SQL Server a Redis. Funkce SignalR technologie ASP.NET Core podporuje služby Azure SignalR a Redis.

### <a name="aspnet"></a>ASP.NET

* [Škálování aplikace SignalR službou Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [Škálování aplikace SignalR službou Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [Škálování aplikace SignalR službou SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Službě Azure SignalR](/azure/azure-signalr/)

## <a name="additional-resources"></a>Další zdroje

* [Centra](xref:signalr/hubs)
* [Klient JavaScriptu](xref:signalr/javascript-client)
* [Klient .NET](xref:signalr/dotnet-client)
* [Podporované platformy](xref:signalr/supported-platforms)
