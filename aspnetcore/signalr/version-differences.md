---
title: Rozdíly mezi SignalR a ASP.NET Core SignalR
author: rachelappel
description: Rozdíly mezi SignalR a ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090176"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Rozdíly mezi SignalR a ASP.NET Core SignalR

Jádro ASP.NET SignalR není kompatibilní s klientů nebo serverů pro ASP.NET SignalR. Tento článek podrobné informace o funkcích, které byly odebrány nebo změněny v ASP.NET Core SignalR.

## <a name="feature-differences"></a>Rozdíly ve funkcích

### <a name="automatic-reconnects"></a>Automatické připojování

Automatické připojování již nejsou podporovány. Dříve SignalR se pokusil připojit k serveru, pokud připojení bylo zrušeno. Nyní, pokud je klient odpojen uživatel musí explicitně zahájení nového připojení, aby bylo možné znovu připojit.

### <a name="protocol-support"></a>Podpora protokolu

Funkce SignalR technologie ASP.NET Core podporuje JSON, stejně jako nový binární protokol na základě [MessagePack](xref:signalr/messagepackhubprotocol). Kromě toho můžete vytvořit vlastní protokoly.

## <a name="differences-on-the-server"></a>Rozdíly na serveru

Jsou součástí na straně serveru knihovny SignalR `Microsoft.AspNetCore.App` balíček, který je součástí **webové aplikace ASP.NET Core** šablonu pro projekty MVC i Razor.

SignalR je middlewaru ASP.NET Core, musí být nakonfigurované voláním `AddSignalR` v `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Konfigurace směrování, mapy trasy k rozbočovačům uvnitř `UseSignalR` volání metody `Startup.Configure` metoda.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Trvalé relace nyní vyžadován

Z důvodu jak šlo Škálováním na více systémů v předchozích verzích nástroje SignalR mohou klienti znovu připojit a odesílání zpráv na libovolný server ve farmě. Z důvodu změn modelu Škálováním na více systémů, a také nejsou podporovány připojování to už není podporovaná. Teď Jakmile se klient připojí k serveru je potřeba ho k interakci se stejný server po dobu trvání připojení.

### <a name="single-hub-per-connection"></a>Jednoho rozbočovače za připojení

V základní funkce SignalR technologie ASP.NET je jednodušší připojení modelu. Vytváří se připojení přímo do jednoho rozbočovače, nikoli jednoho připojení používá sdílet přístup k více rozbočovače.

### <a name="streaming"></a>Streamování

SignalR nyní podporuje [streamovaných dat užitečné](xref:signalr/streaming) z rozbočovače klientovi.

### <a name="state"></a>Stav

Umožňuje předat libovolný stavu mezi klienty a rozbočovače (často říká HubState) byl odebrán, a také podporu pro zprávy o průběhu. V tuto chvíli není žádné protějšku proxy rozbočovače.

## <a name="differences-on-the-client"></a>Rozdíly v klientovi

### <a name="typescript"></a>TypeScript

Verze ASP.NET Core SignalR je napsána v [TypeScript](https://www.typescriptlang.org/). Můžete napsat v jazyce JavaScript nebo TypeScript při použití [JavaScript klienta](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Klient pro JavaScript je hostovaná v [npm](https://www.npmjs.com/)

V předchozích verzích se zjišťovala JavaScript klienta prostřednictvím balíčku NuGet v sadě Visual Studio. Pro základní verze [ @aspnet/signalr balíčku npm](https://www.npmjs.com/package/@aspnet/signalr) obsahuje knihoven jazyka JavaScript. Není součástí tohoto balíčku **webové aplikace ASP.NET Core** šablony. Pomocí npm získat a nainstalovat `@aspnet/signalr` balíčku npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>JQuery

Závislost na jQuery byla odebrána, ale projekty můžete nadále používat jQuery.

### <a name="javascript-client-method-syntax"></a>Syntaxe jazyka JavaScript klienta – metoda

Syntaxe jazyka JavaScript došlo ke změně z předchozí verze funkce signalr. Místo použití `$connection` objektu, vytvořte připojení pomocí `HubConnectionBuilder` rozhraní API.

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

Po vytvoření metodu klienta, spusťte připojení rozbočovače. Řetězec `catch` metodou protokolu nebo zpracování chyb.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Proxy rozbočovače

Proxy centra jsou generovány, nebude automaticky. Místo toho je název metody předána do `invoke` rozhraní API jako řetězec.

### <a name="net-and-other-clients"></a>Rozhraní .NET a další klienti

`Microsoft.AspNetCore.SignalR.Client` Balíček NuGet obsahuje klientské knihovny .NET pro ASP.NET Core SignalR.

Použití `HubConnectionBuilder` k vytvoření a vytvoření instance připojení k rozbočovači.

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
