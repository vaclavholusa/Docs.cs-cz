---
title: Rozdíly mezi SignalR a funkce SignalR technologie ASP.NET Core
author: tdykstra
description: Rozdíly mezi SignalR a funkce SignalR technologie ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: 6ed7e2e1ecadef08d71c4d7a7c3469738d07bcda
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095005"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Rozdíly mezi SignalR a funkce SignalR technologie ASP.NET Core

Funkce SignalR technologie ASP.NET Core není kompatibilní s klientů nebo serverů pro funkci SignalR technologie ASP.NET. Tento článek podrobně popisuje funkce, které byly odstraněny nebo změněny v knihovně SignalR technologie ASP.NET Core.

## <a name="feature-differences"></a>Rozdíly ve funkcích

### <a name="automatic-reconnects"></a>Automatické připojování

Automatické připojování již nejsou podporovány. Dříve SignalR pokusil připojit k serveru, pokud připojení bylo zrušeno. Když teď aplikaci filled klient je odpojen, uživatel musí explicitně spustit nové připojení, pokud se chcete znovu připojit.

### <a name="protocol-support"></a>Podpora protokolu

Funkce SignalR technologie ASP.NET Core podporuje JSON, jakož i nové binární protokol založený na [MessagePack](xref:signalr/messagepackhubprotocol). Kromě toho je možné vytvářet vlastní protokoly.

## <a name="differences-on-the-server"></a>Rozdíly na serveru

Jsou součástí serverové knihovny SignalR `Microsoft.AspNetCore.App` balíček, který je součástí **webové aplikace ASP.NET Core** šablony pro projekty Razor a MVC.

SignalR je middleware ASP.NET Core, musí se nakonfigurovat voláním `AddSignalR` v `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Konfigurace směrování, mapování tras k rozbočovačům uvnitř `UseSignalR` volání metody `Startup.Configure` metoda.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Nyní vyžaduje rychlé relace

Z důvodu jak horizontální navýšení kapacity již dříve pracovali v předchozích verzích nástroje SignalR klienti znovu připojit a odesílání zpráv na libovolný server ve farmě. Z důvodu změny na model horizontální navýšení kapacity, jakož i nenabízí připojování to se už nepodporuje. Teď Jakmile se klient připojí k serveru je potřeba pracovat stejném serveru po dobu trvání připojení.

### <a name="single-hub-per-connection"></a>Jedno Centrum za připojení

Funkce SignalR technologie ASP.NET Core je zjednodušený model připojení. Připojení jsou provedeny přímo do jednoho rozbočovače, nikoli jednoho připojení používá sdílet přístup k více rozbočovače.

### <a name="streaming"></a>Streamování

Teď podporuje funkci SignalR [streamovaná data](xref:signalr/streaming) z rozbočovače klientovi.

### <a name="state"></a>Stav

Umožňuje předat libovolný stav mezi klienty a centrum (často označované jako HubState) byla odebrána a také podporu pro zprávy o průběhu. V tuto chvíli není nevyskytují proxy rozbočovače.

## <a name="differences-on-the-client"></a>Rozdíly v klientovi

### <a name="typescript"></a>TypeScript

ASP.NET Core verzi knihovny SignalR je napsána v [TypeScript](https://www.typescriptlang.org/). Můžete psát v jazyce JavaScript nebo TypeScript při použití [javascriptový klient](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>JavaScript klienta je hostovaná na [npm](https://www.npmjs.com/)

V předchozích verzích klienta JavaScript byl získán prostřednictvím balíčku NuGet v sadě Visual Studio. Verze jádra [ @aspnet/signalr balíčku npm](https://www.npmjs.com/package/@aspnet/signalr) obsahuje knihovny jazyka JavaScript. Není součástí tohoto balíčku **webové aplikace ASP.NET Core** šablony. Získejte a nainstalujte pomocí npm `@aspnet/signalr` balíčku npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

Závislost na jQuery byla odebrána, ale projekty můžete dál používat jQuery.

### <a name="javascript-client-method-syntax"></a>Syntaxe využívající metody JavaScript klienta

Syntaxe jazyka JavaScript byl změněn z předchozí verze funkce SignalR. Místo použití `$connection` objektu, vytvořte pomocí připojení `HubConnectionBuilder` rozhraní API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Použití `connection.on` k určení klienta metody, které můžete volat rozbočovače.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Po vytvoření metody klienta, spusťte připojení rozbočovače. Řetězce `catch` metodou protokolu nebo zpracování chyb.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Proxy servery hub

Proxy servery hub už nebude automaticky generovány. Místo toho název metody je předán `invoke` rozhraní API jako řetězec.

### <a name="net-and-other-clients"></a>.NET a další klienti

`Microsoft.AspNetCore.SignalR.Client` Balíček NuGet obsahuje klientské knihovny .NET pro funkci SignalR technologie ASP.NET Core.

Použití `HubConnectionBuilder` vytvářet a sestavovat instanci, připojení k rozbočovači.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a>Další zdroje

* [Centra](xref:signalr/hubs)
* [Klient JavaScriptu](xref:signalr/javascript-client)
* [Klient .NET](xref:signalr/dotnet-client)
* [Podporované platformy](xref:signalr/supported-platforms)
