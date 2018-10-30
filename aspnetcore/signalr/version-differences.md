---
title: Rozdíly mezi SignalR a funkce SignalR technologie ASP.NET Core
author: tdykstra
description: Rozdíly mezi SignalR a funkce SignalR technologie ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 3cec37719b743b3c805ada77249f526278e44599
ms.sourcegitcommit: 2ef32676c16f76282f7c23154d13affce8c8bf35
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234602"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="07825-103">Rozdíly mezi funkce SignalR technologie ASP.NET a technologie SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07825-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="07825-104">Funkce SignalR technologie ASP.NET Core není kompatibilní s klientů nebo serverů pro funkci SignalR technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="07825-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="07825-105">Tento článek podrobně popisuje funkce, které byly odstraněny nebo změněny v knihovně SignalR technologie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07825-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="07825-106">Jak určit verzi SignalR</span><span class="sxs-lookup"><span data-stu-id="07825-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="07825-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="07825-107">ASP.NET SignalR</span></span> | <span data-ttu-id="07825-108">Funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07825-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="07825-109">Balíček NuGet server</span><span class="sxs-lookup"><span data-stu-id="07825-109">Server NuGet Package</span></span> | [<span data-ttu-id="07825-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="07825-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="07825-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="07825-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="07825-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="07825-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="07825-113">Balíčky NuGet pro klienta</span><span class="sxs-lookup"><span data-stu-id="07825-113">Client NuGet Packages</span></span> | [<span data-ttu-id="07825-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="07825-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="07825-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="07825-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="07825-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="07825-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="07825-117">Npm Package klienta</span><span class="sxs-lookup"><span data-stu-id="07825-117">Client npm Package</span></span> | [<span data-ttu-id="07825-118">Funkce signalr</span><span class="sxs-lookup"><span data-stu-id="07825-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="07825-119">Typ aplikace serveru</span><span class="sxs-lookup"><span data-stu-id="07825-119">Server App Type</span></span> | <span data-ttu-id="07825-120">Technologie ASP.NET (System.Web) nebo samoobslužné hostování OWIN</span><span class="sxs-lookup"><span data-stu-id="07825-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="07825-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07825-121">ASP.NET Core</span></span> |
| <span data-ttu-id="07825-122">Podporované serverové platformy</span><span class="sxs-lookup"><span data-stu-id="07825-122">Supported Server Platforms</span></span> | <span data-ttu-id="07825-123">Rozhraní .NET framework 4.5 nebo novější</span><span class="sxs-lookup"><span data-stu-id="07825-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="07825-124">Rozhraní .NET framework 4.6.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="07825-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="07825-125">.NET core 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="07825-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="07825-126">Rozdíly ve funkcích</span><span class="sxs-lookup"><span data-stu-id="07825-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="07825-127">Automatické připojování</span><span class="sxs-lookup"><span data-stu-id="07825-127">Automatic reconnects</span></span>

<span data-ttu-id="07825-128">Automatické připojování nepodporují funkce SignalR technologie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07825-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="07825-129">Pokud klient je odpojen, uživatel musí explicitně spustit nové připojení, aby připojení bylo možné.</span><span class="sxs-lookup"><span data-stu-id="07825-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="07825-130">V funkce SignalR technologie ASP.NET SignalR pokusí znovu připojit k serveru, pokud připojení se ukončí.</span><span class="sxs-lookup"><span data-stu-id="07825-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="07825-131">Podpora protokolu</span><span class="sxs-lookup"><span data-stu-id="07825-131">Protocol support</span></span>

<span data-ttu-id="07825-132">Funkce SignalR technologie ASP.NET Core podporuje JSON, jakož i nové binární protokol založený na [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="07825-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="07825-133">Kromě toho je možné vytvářet vlastní protokoly.</span><span class="sxs-lookup"><span data-stu-id="07825-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="07825-134">Rozdíly na serveru</span><span class="sxs-lookup"><span data-stu-id="07825-134">Differences on the server</span></span>

<span data-ttu-id="07825-135">Jsou součástí knihoven na straně serveru funkce SignalR technologie ASP.NET Core [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) balíček, který je součástí **webové aplikace ASP.NET Core** šablony pro syntaxi Razor a MVC projekty.</span><span class="sxs-lookup"><span data-stu-id="07825-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="07825-136">Funkce SignalR technologie ASP.NET Core je middleware ASP.NET Core, takže musí být nakonfigurovaný pomocí volání [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="07825-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="07825-137">Konfigurace směrování, mapování tras k rozbočovačům uvnitř [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) volání metody `Startup.Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="07825-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a><span data-ttu-id="07825-138">Rychlé relace</span><span class="sxs-lookup"><span data-stu-id="07825-138">Sticky sessions</span></span>

<span data-ttu-id="07825-139">Model horizontálním navýšením kapacity pro funkci SignalR technologie ASP.NET umožňuje klientům znovu připojit a odesílání zpráv na libovolný server ve farmě.</span><span class="sxs-lookup"><span data-stu-id="07825-139">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="07825-140">V knihovně SignalR technologie ASP.NET Core musí klient pracovat na stejném serveru po dobu trvání připojení.</span><span class="sxs-lookup"><span data-stu-id="07825-140">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="07825-141">Pro škálování, použití Redis, to znamená, že se vyžaduje rychlé relace.</span><span class="sxs-lookup"><span data-stu-id="07825-141">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="07825-142">Pro práci s horizontálním navýšením kapacity [služby Azure SignalR](/azure/azure-signalr/), rychlé relace nejsou vyžadovány, protože služba zpracovává připojení ke klientům.</span><span class="sxs-lookup"><span data-stu-id="07825-142">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span> 

### <a name="single-hub-per-connection"></a><span data-ttu-id="07825-143">Jedno Centrum za připojení</span><span class="sxs-lookup"><span data-stu-id="07825-143">Single hub per connection</span></span>

<span data-ttu-id="07825-144">Funkce SignalR technologie ASP.NET Core je zjednodušený model připojení.</span><span class="sxs-lookup"><span data-stu-id="07825-144">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="07825-145">Připojení jsou provedeny přímo do jednoho rozbočovače, nikoli jednoho připojení používá sdílet přístup k více rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="07825-145">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="07825-146">Streamování</span><span class="sxs-lookup"><span data-stu-id="07825-146">Streaming</span></span>

<span data-ttu-id="07825-147">ASP.NET Core SignalR teď podporuje [streamovaná data](xref:signalr/streaming) z rozbočovače klientovi.</span><span class="sxs-lookup"><span data-stu-id="07825-147">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="07825-148">Stav</span><span class="sxs-lookup"><span data-stu-id="07825-148">State</span></span>

<span data-ttu-id="07825-149">Umožňuje předat libovolný stav mezi klienty a centrum (často označované jako HubState) byla odebrána a také podporu pro zprávy o průběhu.</span><span class="sxs-lookup"><span data-stu-id="07825-149">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="07825-150">V tuto chvíli není nevyskytují proxy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="07825-150">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="07825-151">Rozdíly v klientovi</span><span class="sxs-lookup"><span data-stu-id="07825-151">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="07825-152">TypeScript</span><span class="sxs-lookup"><span data-stu-id="07825-152">TypeScript</span></span>

<span data-ttu-id="07825-153">Klient funkce SignalR technologie ASP.NET Core je napsána v [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="07825-153">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="07825-154">Můžete psát v jazyce JavaScript nebo TypeScript při použití [javascriptový klient](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="07825-154">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="07825-155">JavaScript klienta je hostovaná na [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="07825-155">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="07825-156">V předchozích verzích klienta JavaScript byl získán prostřednictvím balíčku NuGet v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="07825-156">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="07825-157">Verze jádra [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) balíčku npm obsahuje knihovny jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="07825-157">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="07825-158">Není součástí tohoto balíčku **webové aplikace ASP.NET Core** šablony.</span><span class="sxs-lookup"><span data-stu-id="07825-158">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="07825-159">Získejte a nainstalujte pomocí npm `@aspnet/signalr` balíčku npm.</span><span class="sxs-lookup"><span data-stu-id="07825-159">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="07825-160">jQuery</span><span class="sxs-lookup"><span data-stu-id="07825-160">jQuery</span></span>

<span data-ttu-id="07825-161">Závislost na jQuery byla odebrána, ale projekty můžete dál používat jQuery.</span><span class="sxs-lookup"><span data-stu-id="07825-161">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="07825-162">Syntaxe využívající metody JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="07825-162">JavaScript client method syntax</span></span>

<span data-ttu-id="07825-163">Syntaxe jazyka JavaScript byl změněn z předchozí verze funkce SignalR.</span><span class="sxs-lookup"><span data-stu-id="07825-163">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="07825-164">Místo použití `$connection` objektu, vytvořte pomocí připojení [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="07825-164">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="07825-165">Použití [na](/javascript/api/@aspnet/signalr/HubConnection#on) metoda určují metody klienta můžete volání rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="07825-165">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="07825-166">Po vytvoření metody klienta, spusťte připojení rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="07825-166">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="07825-167">Řetězce [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodou protokolu nebo zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="07825-167">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="07825-168">Proxy servery hub</span><span class="sxs-lookup"><span data-stu-id="07825-168">Hub proxies</span></span>

<span data-ttu-id="07825-169">Proxy servery hub už nebude automaticky generovány.</span><span class="sxs-lookup"><span data-stu-id="07825-169">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="07825-170">Místo toho název metody je předán [vyvolat](/javascript/api/%40aspnet/signalr/hubconnection#invoke) rozhraní API jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="07825-170">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="07825-171">.NET a další klienti</span><span class="sxs-lookup"><span data-stu-id="07825-171">.NET and other clients</span></span>

<span data-ttu-id="07825-172">`Microsoft.AspNetCore.SignalR.Client` Balíček NuGet obsahuje klientské knihovny .NET pro funkci SignalR technologie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07825-172">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="07825-173">Použití [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) vytvářet a sestavovat instanci, připojení k rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="07825-173">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="07825-174">Rozdíly horizontálním navýšením kapacity</span><span class="sxs-lookup"><span data-stu-id="07825-174">Scaleout differences</span></span>

<span data-ttu-id="07825-175">Funkce SignalR technologie ASP.NET podporuje SQL Server a Redis.</span><span class="sxs-lookup"><span data-stu-id="07825-175">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="07825-176">Funkce SignalR technologie ASP.NET Core podporuje služby Azure SignalR a Redis.</span><span class="sxs-lookup"><span data-stu-id="07825-176">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="07825-177">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="07825-177">ASP.NET</span></span>

* [<span data-ttu-id="07825-178">Škálování aplikace SignalR službou Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="07825-178">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="07825-179">Škálování aplikace SignalR službou Redis</span><span class="sxs-lookup"><span data-stu-id="07825-179">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="07825-180">Škálování aplikace SignalR službou SQL Server</span><span class="sxs-lookup"><span data-stu-id="07825-180">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="07825-181">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07825-181">ASP.NET Core</span></span>

* [<span data-ttu-id="07825-182">Službě Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="07825-182">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="07825-183">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="07825-183">Additional resources</span></span>

* [<span data-ttu-id="07825-184">Centra</span><span class="sxs-lookup"><span data-stu-id="07825-184">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="07825-185">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="07825-185">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="07825-186">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="07825-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="07825-187">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="07825-187">Supported platforms</span></span>](xref:signalr/supported-platforms)
