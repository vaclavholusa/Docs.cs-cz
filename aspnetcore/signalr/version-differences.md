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
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="73d10-103">Rozdíly mezi SignalR a funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73d10-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="73d10-104">Funkce SignalR technologie ASP.NET Core není kompatibilní s klientů nebo serverů pro funkci SignalR technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="73d10-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="73d10-105">Tento článek podrobně popisuje funkce, které byly odstraněny nebo změněny v knihovně SignalR technologie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73d10-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="73d10-106">Rozdíly ve funkcích</span><span class="sxs-lookup"><span data-stu-id="73d10-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="73d10-107">Automatické připojování</span><span class="sxs-lookup"><span data-stu-id="73d10-107">Automatic reconnects</span></span>

<span data-ttu-id="73d10-108">Automatické připojování již nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="73d10-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="73d10-109">Dříve SignalR pokusil připojit k serveru, pokud připojení bylo zrušeno.</span><span class="sxs-lookup"><span data-stu-id="73d10-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="73d10-110">Když teď aplikaci filled klient je odpojen, uživatel musí explicitně spustit nové připojení, pokud se chcete znovu připojit.</span><span class="sxs-lookup"><span data-stu-id="73d10-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="73d10-111">Podpora protokolu</span><span class="sxs-lookup"><span data-stu-id="73d10-111">Protocol support</span></span>

<span data-ttu-id="73d10-112">Funkce SignalR technologie ASP.NET Core podporuje JSON, jakož i nové binární protokol založený na [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="73d10-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="73d10-113">Kromě toho je možné vytvářet vlastní protokoly.</span><span class="sxs-lookup"><span data-stu-id="73d10-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="73d10-114">Rozdíly na serveru</span><span class="sxs-lookup"><span data-stu-id="73d10-114">Differences on the server</span></span>

<span data-ttu-id="73d10-115">Jsou součástí serverové knihovny SignalR `Microsoft.AspNetCore.App` balíček, který je součástí **webové aplikace ASP.NET Core** šablony pro projekty Razor a MVC.</span><span class="sxs-lookup"><span data-stu-id="73d10-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="73d10-116">SignalR je middleware ASP.NET Core, musí se nakonfigurovat voláním `AddSignalR` v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="73d10-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="73d10-117">Konfigurace směrování, mapování tras k rozbočovačům uvnitř `UseSignalR` volání metody `Startup.Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="73d10-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="73d10-118">Nyní vyžaduje rychlé relace</span><span class="sxs-lookup"><span data-stu-id="73d10-118">Sticky sessions now required</span></span>

<span data-ttu-id="73d10-119">Z důvodu jak horizontální navýšení kapacity již dříve pracovali v předchozích verzích nástroje SignalR klienti znovu připojit a odesílání zpráv na libovolný server ve farmě.</span><span class="sxs-lookup"><span data-stu-id="73d10-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="73d10-120">Z důvodu změny na model horizontální navýšení kapacity, jakož i nenabízí připojování to se už nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="73d10-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="73d10-121">Teď Jakmile se klient připojí k serveru je potřeba pracovat stejném serveru po dobu trvání připojení.</span><span class="sxs-lookup"><span data-stu-id="73d10-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="73d10-122">Jedno Centrum za připojení</span><span class="sxs-lookup"><span data-stu-id="73d10-122">Single hub per connection</span></span>

<span data-ttu-id="73d10-123">Funkce SignalR technologie ASP.NET Core je zjednodušený model připojení.</span><span class="sxs-lookup"><span data-stu-id="73d10-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="73d10-124">Připojení jsou provedeny přímo do jednoho rozbočovače, nikoli jednoho připojení používá sdílet přístup k více rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="73d10-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="73d10-125">Streamování</span><span class="sxs-lookup"><span data-stu-id="73d10-125">Streaming</span></span>

<span data-ttu-id="73d10-126">Teď podporuje funkci SignalR [streamovaná data](xref:signalr/streaming) z rozbočovače klientovi.</span><span class="sxs-lookup"><span data-stu-id="73d10-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="73d10-127">Stav</span><span class="sxs-lookup"><span data-stu-id="73d10-127">State</span></span>

<span data-ttu-id="73d10-128">Umožňuje předat libovolný stav mezi klienty a centrum (často označované jako HubState) byla odebrána a také podporu pro zprávy o průběhu.</span><span class="sxs-lookup"><span data-stu-id="73d10-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="73d10-129">V tuto chvíli není nevyskytují proxy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="73d10-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="73d10-130">Rozdíly v klientovi</span><span class="sxs-lookup"><span data-stu-id="73d10-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="73d10-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="73d10-131">TypeScript</span></span>

<span data-ttu-id="73d10-132">ASP.NET Core verzi knihovny SignalR je napsána v [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="73d10-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="73d10-133">Můžete psát v jazyce JavaScript nebo TypeScript při použití [javascriptový klient](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="73d10-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="73d10-134">JavaScript klienta je hostovaná na [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="73d10-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="73d10-135">V předchozích verzích klienta JavaScript byl získán prostřednictvím balíčku NuGet v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73d10-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="73d10-136">Verze jádra [ @aspnet/signalr balíčku npm](https://www.npmjs.com/package/@aspnet/signalr) obsahuje knihovny jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73d10-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="73d10-137">Není součástí tohoto balíčku **webové aplikace ASP.NET Core** šablony.</span><span class="sxs-lookup"><span data-stu-id="73d10-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="73d10-138">Získejte a nainstalujte pomocí npm `@aspnet/signalr` balíčku npm.</span><span class="sxs-lookup"><span data-stu-id="73d10-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="73d10-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="73d10-139">jQuery</span></span>

<span data-ttu-id="73d10-140">Závislost na jQuery byla odebrána, ale projekty můžete dál používat jQuery.</span><span class="sxs-lookup"><span data-stu-id="73d10-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="73d10-141">Syntaxe využívající metody JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="73d10-141">JavaScript client method syntax</span></span>

<span data-ttu-id="73d10-142">Syntaxe jazyka JavaScript byl změněn z předchozí verze funkce SignalR.</span><span class="sxs-lookup"><span data-stu-id="73d10-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="73d10-143">Místo použití `$connection` objektu, vytvořte pomocí připojení `HubConnectionBuilder` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="73d10-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="73d10-144">Použití `connection.on` k určení klienta metody, které můžete volat rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="73d10-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="73d10-145">Po vytvoření metody klienta, spusťte připojení rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="73d10-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="73d10-146">Řetězce `catch` metodou protokolu nebo zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="73d10-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="73d10-147">Proxy servery hub</span><span class="sxs-lookup"><span data-stu-id="73d10-147">Hub proxies</span></span>

<span data-ttu-id="73d10-148">Proxy servery hub už nebude automaticky generovány.</span><span class="sxs-lookup"><span data-stu-id="73d10-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="73d10-149">Místo toho název metody je předán `invoke` rozhraní API jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="73d10-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="73d10-150">.NET a další klienti</span><span class="sxs-lookup"><span data-stu-id="73d10-150">.NET and other clients</span></span>

<span data-ttu-id="73d10-151">`Microsoft.AspNetCore.SignalR.Client` Balíček NuGet obsahuje klientské knihovny .NET pro funkci SignalR technologie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73d10-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="73d10-152">Použití `HubConnectionBuilder` vytvářet a sestavovat instanci, připojení k rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="73d10-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="73d10-153">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="73d10-153">Additional resources</span></span>

* [<span data-ttu-id="73d10-154">Centra</span><span class="sxs-lookup"><span data-stu-id="73d10-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="73d10-155">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="73d10-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="73d10-156">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="73d10-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="73d10-157">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="73d10-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
