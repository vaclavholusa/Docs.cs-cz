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
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="e5d5d-103">Rozdíly mezi SignalR a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e5d5d-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="e5d5d-104">Jádro ASP.NET SignalR není kompatibilní s klientů nebo serverů pro ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="e5d5d-105">Tento článek podrobné informace o funkcích, které byly odebrány nebo změněny v ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="e5d5d-106">Rozdíly ve funkcích</span><span class="sxs-lookup"><span data-stu-id="e5d5d-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="e5d5d-107">Automatické připojování</span><span class="sxs-lookup"><span data-stu-id="e5d5d-107">Automatic reconnects</span></span>

<span data-ttu-id="e5d5d-108">Automatické připojování již nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="e5d5d-109">Dříve SignalR se pokusil připojit k serveru, pokud připojení bylo zrušeno.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="e5d5d-110">Nyní, pokud je klient odpojen uživatel musí explicitně zahájení nového připojení, aby bylo možné znovu připojit.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="e5d5d-111">Podpora protokolu</span><span class="sxs-lookup"><span data-stu-id="e5d5d-111">Protocol support</span></span>

<span data-ttu-id="e5d5d-112">Funkce SignalR technologie ASP.NET Core podporuje JSON, stejně jako nový binární protokol na základě [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="e5d5d-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="e5d5d-113">Kromě toho můžete vytvořit vlastní protokoly.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="e5d5d-114">Rozdíly na serveru</span><span class="sxs-lookup"><span data-stu-id="e5d5d-114">Differences on the server</span></span>

<span data-ttu-id="e5d5d-115">Jsou součástí na straně serveru knihovny SignalR `Microsoft.AspNetCore.App` balíček, který je součástí **webové aplikace ASP.NET Core** šablonu pro projekty MVC i Razor.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="e5d5d-116">SignalR je middlewaru ASP.NET Core, musí být nakonfigurované voláním `AddSignalR` v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="e5d5d-117">Konfigurace směrování, mapy trasy k rozbočovačům uvnitř `UseSignalR` volání metody `Startup.Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="e5d5d-118">Trvalé relace nyní vyžadován</span><span class="sxs-lookup"><span data-stu-id="e5d5d-118">Sticky sessions now required</span></span>

<span data-ttu-id="e5d5d-119">Z důvodu jak šlo Škálováním na více systémů v předchozích verzích nástroje SignalR mohou klienti znovu připojit a odesílání zpráv na libovolný server ve farmě.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="e5d5d-120">Z důvodu změn modelu Škálováním na více systémů, a také nejsou podporovány připojování to už není podporovaná.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="e5d5d-121">Teď Jakmile se klient připojí k serveru je potřeba ho k interakci se stejný server po dobu trvání připojení.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="e5d5d-122">Jednoho rozbočovače za připojení</span><span class="sxs-lookup"><span data-stu-id="e5d5d-122">Single hub per connection</span></span>

<span data-ttu-id="e5d5d-123">V základní funkce SignalR technologie ASP.NET je jednodušší připojení modelu.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="e5d5d-124">Vytváří se připojení přímo do jednoho rozbočovače, nikoli jednoho připojení používá sdílet přístup k více rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="e5d5d-125">Streamování</span><span class="sxs-lookup"><span data-stu-id="e5d5d-125">Streaming</span></span>

<span data-ttu-id="e5d5d-126">SignalR nyní podporuje [streamovaných dat užitečné](xref:signalr/streaming) z rozbočovače klientovi.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="e5d5d-127">Stav</span><span class="sxs-lookup"><span data-stu-id="e5d5d-127">State</span></span>

<span data-ttu-id="e5d5d-128">Umožňuje předat libovolný stavu mezi klienty a rozbočovače (často říká HubState) byl odebrán, a také podporu pro zprávy o průběhu.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="e5d5d-129">V tuto chvíli není žádné protějšku proxy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="e5d5d-130">Rozdíly v klientovi</span><span class="sxs-lookup"><span data-stu-id="e5d5d-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="e5d5d-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="e5d5d-131">TypeScript</span></span>

<span data-ttu-id="e5d5d-132">Verze ASP.NET Core SignalR je napsána v [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="e5d5d-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="e5d5d-133">Můžete napsat v jazyce JavaScript nebo TypeScript při použití [JavaScript klienta](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="e5d5d-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="e5d5d-134">Klient pro JavaScript je hostovaná v [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="e5d5d-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="e5d5d-135">V předchozích verzích se zjišťovala JavaScript klienta prostřednictvím balíčku NuGet v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="e5d5d-136">Pro základní verze [ @aspnet/signalr balíčku npm](https://www.npmjs.com/package/@aspnet/signalr) obsahuje knihoven jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="e5d5d-137">Není součástí tohoto balíčku **webové aplikace ASP.NET Core** šablony.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="e5d5d-138">Pomocí npm získat a nainstalovat `@aspnet/signalr` balíčku npm.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="e5d5d-139">JQuery</span><span class="sxs-lookup"><span data-stu-id="e5d5d-139">jQuery</span></span>

<span data-ttu-id="e5d5d-140">Závislost na jQuery byla odebrána, ale projekty můžete nadále používat jQuery.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="e5d5d-141">Syntaxe jazyka JavaScript klienta – metoda</span><span class="sxs-lookup"><span data-stu-id="e5d5d-141">JavaScript client method syntax</span></span>

<span data-ttu-id="e5d5d-142">Syntaxe jazyka JavaScript došlo ke změně z předchozí verze funkce signalr.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="e5d5d-143">Místo použití `$connection` objektu, vytvořte připojení pomocí `HubConnectionBuilder` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="e5d5d-144">Použití `connection.on` k určení klienta metody, které můžete volat rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="e5d5d-145">Po vytvoření metodu klienta, spusťte připojení rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="e5d5d-146">Řetězec `catch` metodou protokolu nebo zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="e5d5d-147">Proxy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="e5d5d-147">Hub proxies</span></span>

<span data-ttu-id="e5d5d-148">Proxy centra jsou generovány, nebude automaticky.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="e5d5d-149">Místo toho je název metody předána do `invoke` rozhraní API jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="e5d5d-150">Rozhraní .NET a další klienti</span><span class="sxs-lookup"><span data-stu-id="e5d5d-150">.NET and other clients</span></span>

<span data-ttu-id="e5d5d-151">`Microsoft.AspNetCore.SignalR.Client` Balíček NuGet obsahuje klientské knihovny .NET pro ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="e5d5d-152">Použití `HubConnectionBuilder` k vytvoření a vytvoření instance připojení k rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="e5d5d-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="e5d5d-153">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e5d5d-153">Additional resources</span></span>

* [<span data-ttu-id="e5d5d-154">Centra</span><span class="sxs-lookup"><span data-stu-id="e5d5d-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e5d5d-155">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="e5d5d-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="e5d5d-156">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="e5d5d-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e5d5d-157">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="e5d5d-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
