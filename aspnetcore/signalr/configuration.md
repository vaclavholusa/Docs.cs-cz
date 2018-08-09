---
title: Konfigurace jádra SignalR technologie ASP.NET
author: tdykstra
description: Zjistěte, jak nakonfigurovat aplikace SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/31/2018
uid: signalr/configuration
ms.openlocfilehash: eac1202828edbcd295d7e52aa424cd625ee70e34
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722461"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="db677-103">Konfigurace jádra SignalR technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="db677-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="db677-104">Serializace JSON/MessagePack možnosti</span><span class="sxs-lookup"><span data-stu-id="db677-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="db677-105">Funkce SignalR technologie ASP.NET Core pro kódování zpráv podporuje dva protokoly: [JSON](https://www.json.org/) a [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="db677-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="db677-106">Všechny protokoly, které obsahuje možnosti konfigurace serializace.</span><span class="sxs-lookup"><span data-stu-id="db677-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="db677-107">Serializace JSON lze nastavit na serveru pomocí [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metody rozšíření, které mohou být přidány po [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) v vaše `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="db677-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="db677-108">`AddJsonProtocol` Metoda přijímá delegát, který přijímá `options` objektu.</span><span class="sxs-lookup"><span data-stu-id="db677-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="db677-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) vlastnost k tomuto objektu je JSON.NET `JsonSerializerSettings` objekt, který můžete použít ke konfiguraci serializace argumenty a návratové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="db677-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="db677-110">Najdete v článku [JSON.NET dokumentaci](https://www.newtonsoft.com/json/help/html/Introduction.htm) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="db677-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="db677-111">Jako příklad konfigurace serializátoru, který chcete použít místo výchozí názvy "camelCase", "PascalCase" názvy vlastností, pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="db677-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="db677-112">V klientovi .NET, stejné `AddJsonHubProtocol` – metoda rozšíření existuje v [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="db677-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="db677-113">`Microsoft.Extensions.DependencyInjection` Obor názvů musí být importován do resolve – metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="db677-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> <span data-ttu-id="db677-114">Není možné konfigurovat pomocí serializace JSON klienta jazyka JavaScript v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="db677-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="db677-115">MessagePack možnosti serializace</span><span class="sxs-lookup"><span data-stu-id="db677-115">MessagePack serialization options</span></span>

<span data-ttu-id="db677-116">Poskytnutím delegáta, kterého lze nakonfigurovat MessagePack serializace [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) volání.</span><span class="sxs-lookup"><span data-stu-id="db677-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="db677-117">Zobrazit [MessagePack v knihovně SignalR](xref:signalr/messagepackhubprotocol) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="db677-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="db677-118">Není možné konfigurovat MessagePack serializace pomocí jazyka JavaScript klienta v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="db677-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="db677-119">Konfigurovat možnosti serveru</span><span class="sxs-lookup"><span data-stu-id="db677-119">Configure server options</span></span>

<span data-ttu-id="db677-120">Následující tabulka popisuje možnosti pro konfiguraci rozbočovače SignalR:</span><span class="sxs-lookup"><span data-stu-id="db677-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="db677-121">Možnost</span><span class="sxs-lookup"><span data-stu-id="db677-121">Option</span></span> | <span data-ttu-id="db677-122">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="db677-122">Default Value</span></span> | <span data-ttu-id="db677-123">Popis</span><span class="sxs-lookup"><span data-stu-id="db677-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="db677-124">15 sekund</span><span class="sxs-lookup"><span data-stu-id="db677-124">15 seconds</span></span> | <span data-ttu-id="db677-125">Pokud klient nebude odeslat zprávu handshake počáteční v tomto časovém intervalu, je připojení ukončeno.</span><span class="sxs-lookup"><span data-stu-id="db677-125">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="db677-126">Toto je upřesňující nastavení, by měla být změněna pouze v případě chyby časového limitu handshake dochází z důvodu závažné sítích s latencí.</span><span class="sxs-lookup"><span data-stu-id="db677-126">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="db677-127">Další podrobnosti o procesu najdete v článku [specifikace protokolu rozbočovače SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="db677-127">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="db677-128">15 sekund</span><span class="sxs-lookup"><span data-stu-id="db677-128">15 seconds</span></span> | <span data-ttu-id="db677-129">Pokud server není v rámci tohoto intervalu odeslal zprávu, je automaticky odeslána zpráva příkazu ping na udržení připojení otevřeného.</span><span class="sxs-lookup"><span data-stu-id="db677-129">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="db677-130">Všechny nainstalované protokoly</span><span class="sxs-lookup"><span data-stu-id="db677-130">All installed protocols</span></span> | <span data-ttu-id="db677-131">Protokolů podporovaných toto centrum.</span><span class="sxs-lookup"><span data-stu-id="db677-131">Protocols supported by this hub.</span></span> <span data-ttu-id="db677-132">Ve výchozím nastavení jsou povolené všechny protokoly, které jsou registrované na serveru, ale protokolů lze odebrat z tohoto seznamu zakázat konkrétní protokoly pro jednotlivé rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="db677-132">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="db677-133">Pokud `true`, podrobné zprávy o výjimkách se vrátí ke klientům, když dojde k výjimce v metodě rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="db677-133">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="db677-134">Výchozí hodnota je `false`, jak tyto zprávy o výjimkách mohou obsahovat citlivé údaje.</span><span class="sxs-lookup"><span data-stu-id="db677-134">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="db677-135">Možnosti je možné nakonfigurovat pro všechna centra poskytnutím delegáta možnosti k `AddSignalR` volání v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="db677-135">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

<span data-ttu-id="db677-136">Možnosti pro jedno centrum přepsat globální možnosti uvedené v `AddSignalR` a dá se nakonfigurovat pomocí [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="db677-136">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="db677-137">Použití `HttpConnectionDispatcherOptions` konfigurace upřesňujících nastavení související s přenosy a správa vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="db677-137">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="db677-138">Tyto možnosti jsou nakonfigurované pomocí předání delegáta, kterého [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="db677-138">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="db677-139">Možnost</span><span class="sxs-lookup"><span data-stu-id="db677-139">Option</span></span> | <span data-ttu-id="db677-140">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="db677-140">Default Value</span></span> | <span data-ttu-id="db677-141">Popis</span><span class="sxs-lookup"><span data-stu-id="db677-141">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="db677-142">32 KB.</span><span class="sxs-lookup"><span data-stu-id="db677-142">32 KB</span></span> | <span data-ttu-id="db677-143">Maximální počet bajtů přijatých z klienta, který vyrovnávací paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="db677-143">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="db677-144">Zvýšení hodnoty tuto umožňuje serveru pro příjem větší zprávy, ale může mít negativní vliv na využití paměti.</span><span class="sxs-lookup"><span data-stu-id="db677-144">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="db677-145">Data jsou automaticky shromážděna z `Authorize` atributy použité u třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="db677-145">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="db677-146">Seznam [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objekty sloužící k určení, pokud je klient autorizaci k připojení k rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="db677-146">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="db677-147">32 KB.</span><span class="sxs-lookup"><span data-stu-id="db677-147">32 KB</span></span> | <span data-ttu-id="db677-148">Maximální počet bajtů odeslaných aplikace, která vyrovnávací paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="db677-148">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="db677-149">Zvýšení hodnoty tuto umožňuje serveru odesílat větší zprávy, ale může mít negativní vliv na využití paměti.</span><span class="sxs-lookup"><span data-stu-id="db677-149">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="db677-150">Všechny přenosy jsou povolené.</span><span class="sxs-lookup"><span data-stu-id="db677-150">All Transports are enabled.</span></span> | <span data-ttu-id="db677-151">Bitová maska z `HttpTransportType` hodnoty, které můžete omezit přenosy klienta můžete použít pro připojení.</span><span class="sxs-lookup"><span data-stu-id="db677-151">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="db677-152">Níže jsou uvedeny.</span><span class="sxs-lookup"><span data-stu-id="db677-152">See below.</span></span> | <span data-ttu-id="db677-153">Další možnosti specifické pro dlouhé dotazování přenosu.</span><span class="sxs-lookup"><span data-stu-id="db677-153">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="db677-154">Níže jsou uvedeny.</span><span class="sxs-lookup"><span data-stu-id="db677-154">See below.</span></span> | <span data-ttu-id="db677-155">Další možnosti specifické pro přenos objekty Websocket.</span><span class="sxs-lookup"><span data-stu-id="db677-155">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="db677-156">Dlouhý interval dotazování přenosu má další možnosti, které lze konfigurovat pomocí `LongPolling` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="db677-156">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="db677-157">Možnost</span><span class="sxs-lookup"><span data-stu-id="db677-157">Option</span></span> | <span data-ttu-id="db677-158">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="db677-158">Default Value</span></span> | <span data-ttu-id="db677-159">Popis</span><span class="sxs-lookup"><span data-stu-id="db677-159">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="db677-160">90 sekund</span><span class="sxs-lookup"><span data-stu-id="db677-160">90 seconds</span></span> | <span data-ttu-id="db677-161">Maximální množství času na server čeká na zprávu k odeslání do klienta před ukončením žádosti o jednotné hlasování.</span><span class="sxs-lookup"><span data-stu-id="db677-161">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="db677-162">Snížení hodnoty způsobí, že klient k vydávání nových žádostí o dotazování častěji.</span><span class="sxs-lookup"><span data-stu-id="db677-162">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="db677-163">Přenos pomocí protokolu WebSocket má další možnosti, které lze konfigurovat pomocí `WebSockets` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="db677-163">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="db677-164">Možnost</span><span class="sxs-lookup"><span data-stu-id="db677-164">Option</span></span> | <span data-ttu-id="db677-165">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="db677-165">Default Value</span></span> | <span data-ttu-id="db677-166">Popis</span><span class="sxs-lookup"><span data-stu-id="db677-166">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="db677-167">5 sekund</span><span class="sxs-lookup"><span data-stu-id="db677-167">5 seconds</span></span> | <span data-ttu-id="db677-168">Po zavření serveru, pokud se klientovi nepodaří zavřete v tomto časovém intervalu, připojení se ukončí.</span><span class="sxs-lookup"><span data-stu-id="db677-168">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="db677-169">Delegát, který je možné nastavit `Sec-WebSocket-Protocol` vlastní hodnoty záhlaví.</span><span class="sxs-lookup"><span data-stu-id="db677-169">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="db677-170">Delegát přijme hodnoty vyžádané klientem jako vstup a měl by vrátit na požadovanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="db677-170">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="db677-171">Konfigurovat možnosti klienta</span><span class="sxs-lookup"><span data-stu-id="db677-171">Configure client options</span></span>

<span data-ttu-id="db677-172">Možnosti klienta lze nakonfigurovat podle `HubConnectionBuilder` typ (k dispozici v rozhraní .NET a JavaScript klientech), stejně jako na `HubConnection` samotný.</span><span class="sxs-lookup"><span data-stu-id="db677-172">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="db677-173">Konfigurace protokolování</span><span class="sxs-lookup"><span data-stu-id="db677-173">Configure logging</span></span>

<span data-ttu-id="db677-174">Protokolování je nakonfigurován v klientu .NET pomocí `ConfigureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="db677-174">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="db677-175">Protokolování poskytovatelů a filtry můžete zaregistrovat stejně, jako jsou na serveru.</span><span class="sxs-lookup"><span data-stu-id="db677-175">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="db677-176">Zobrazit [protokolování v ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) Další informace naleznete v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="db677-176">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="db677-177">Aby bylo možné zaregistrovat poskytovatele protokolování, je nutné nainstalovat potřebné balíčky.</span><span class="sxs-lookup"><span data-stu-id="db677-177">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="db677-178">Najdete v článku [vestavěné protokolování poskytovatelé](xref:fundamentals/logging/index#built-in-logging-providers) část dokumentace pro úplný seznam.</span><span class="sxs-lookup"><span data-stu-id="db677-178">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="db677-179">Například pokud chcete povolit protokolování konzoly, nainstalujte `Microsoft.Extensions.Logging.Console` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="db677-179">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="db677-180">Volání `AddConsole` – metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="db677-180">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="db677-181">V klientovi JavaScript podobný `configureLogging` metody existuje.</span><span class="sxs-lookup"><span data-stu-id="db677-181">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="db677-182">Zadejte `LogLevel` hodnotu, která minimální úroveň zprávy protokolu k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="db677-182">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="db677-183">Protokoly se zapisují do okna konzoly prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="db677-183">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="db677-184">Chcete-li zakázat protokolování úplně, zadejte `signalR.LogLevel.None` v `configureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="db677-184">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="db677-185">K dispozici ke klientovi JavaScript úrovně protokolu jsou uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="db677-185">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="db677-186">Protokolování zpráv při nastavení na úrovni protokolu na jednu z těchto hodnot umožňuje **nebo vyšší** dané úrovni.</span><span class="sxs-lookup"><span data-stu-id="db677-186">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="db677-187">úroveň</span><span class="sxs-lookup"><span data-stu-id="db677-187">Level</span></span> | <span data-ttu-id="db677-188">Popis</span><span class="sxs-lookup"><span data-stu-id="db677-188">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="db677-189">Jsou zaznamenány žádné zprávy.</span><span class="sxs-lookup"><span data-stu-id="db677-189">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="db677-190">Zprávy, které indikují chybu celé aplikace.</span><span class="sxs-lookup"><span data-stu-id="db677-190">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="db677-191">Zprávy, které indikují chybu aktuální operaci.</span><span class="sxs-lookup"><span data-stu-id="db677-191">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="db677-192">Zprávy, které označují méně závažné potíže.</span><span class="sxs-lookup"><span data-stu-id="db677-192">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="db677-193">Informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="db677-193">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="db677-194">Diagnostické zprávy je užitečné pro ladění.</span><span class="sxs-lookup"><span data-stu-id="db677-194">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="db677-195">Velmi podrobné diagnostické zprávy, které jsou navržené pro diagnostiku specifické problémy.</span><span class="sxs-lookup"><span data-stu-id="db677-195">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="db677-196">Nakonfigurovat povolené přenosy</span><span class="sxs-lookup"><span data-stu-id="db677-196">Configure allowed transports</span></span>

<span data-ttu-id="db677-197">Přenosy systém signalr se dá nakonfigurovat v `WithUrl` volání (`withUrl` v JavaScriptu).</span><span class="sxs-lookup"><span data-stu-id="db677-197">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="db677-198">Bitový OR hodnoty `HttpTransportType` slouží k omezení klienta používat pouze zadaný přenosy.</span><span class="sxs-lookup"><span data-stu-id="db677-198">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="db677-199">Ve výchozím nastavení jsou povolené všechny přenosy.</span><span class="sxs-lookup"><span data-stu-id="db677-199">All transports are enabled by default.</span></span>

<span data-ttu-id="db677-200">Například zakázat přenos Server-Sent události, ale povolte připojení objekty Websocket a dlouhý interval dotazování:</span><span class="sxs-lookup"><span data-stu-id="db677-200">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="db677-201">V klientovi JavaScript přenosy jsou nakonfigurovány tak, že nastavíte `transport` na objekt možnosti poskytnuté `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="db677-201">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="db677-202">Konfigurace ověřování nosiče</span><span class="sxs-lookup"><span data-stu-id="db677-202">Configure bearer authentication</span></span>

<span data-ttu-id="db677-203">Pokud chcete poskytnout ověřovací data spolu s knihovnou SignalR požadavky, použijte `AccessTokenProvider` možnost (`accessTokenFactory` v JavaScriptu) k určení funkce, která vrací požadovaný přístupový token.</span><span class="sxs-lookup"><span data-stu-id="db677-203">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="db677-204">V klientovi .NET Tento přístupový token předaný jako HTTP "Ověřování nosiče" token (použití `Authorization` záhlaví s typem `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="db677-204">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="db677-205">V klientovi JavaScript přístupový token se používá jako nosný token **s výjimkou** v několika případech, kdy prohlížeč rozhraní API omezit schopnosti uplatňovat hlavičky (konkrétně v Server-Sent události a protokoly Websocket požadavky).</span><span class="sxs-lookup"><span data-stu-id="db677-205">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="db677-206">V těchto případech je přístupový token k dispozici jako hodnotu řetězce dotazu `access_token`.</span><span class="sxs-lookup"><span data-stu-id="db677-206">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="db677-207">V klientovi .NET `AccessTokenProvider` možnost lze zadat pomocí možnosti delegáta v `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="db677-207">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="db677-208">V klientovi JavaScript přístupový token je nakonfigurovaný tak, že nastavíte `accessTokenFactory` na objekt možnosti v `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="db677-208">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="db677-209">Konfigurace časového limitu a udržování možnosti</span><span class="sxs-lookup"><span data-stu-id="db677-209">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="db677-210">Další možnosti pro konfiguraci časového limitu a zachování chování, které jsou k dispozici na `HubConnection` samotného objektu:</span><span class="sxs-lookup"><span data-stu-id="db677-210">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="db677-211">.NET – možnost</span><span class="sxs-lookup"><span data-stu-id="db677-211">.NET Option</span></span> | <span data-ttu-id="db677-212">Možnost jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="db677-212">JavaScript Option</span></span> | <span data-ttu-id="db677-213">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="db677-213">Default Value</span></span> | <span data-ttu-id="db677-214">Popis</span><span class="sxs-lookup"><span data-stu-id="db677-214">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="db677-215">30 sekund (30 000 milisekund)</span><span class="sxs-lookup"><span data-stu-id="db677-215">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="db677-216">Časový limit pro aktivity serveru.</span><span class="sxs-lookup"><span data-stu-id="db677-216">Timeout for server activity.</span></span> <span data-ttu-id="db677-217">Pokud server není v tomto intervalu odeslal zprávu, klient bude považovat za server odpojen a aktivační události `Closed` událostí (`onclose` v JavaScriptu).</span><span class="sxs-lookup"><span data-stu-id="db677-217">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="db677-218">Nejde konfigurovat</span><span class="sxs-lookup"><span data-stu-id="db677-218">Not configurable</span></span> | <span data-ttu-id="db677-219">15 sekund</span><span class="sxs-lookup"><span data-stu-id="db677-219">15 seconds</span></span> | <span data-ttu-id="db677-220">Časový limit pro počáteční server handshake.</span><span class="sxs-lookup"><span data-stu-id="db677-220">Timeout for initial server handshake.</span></span> <span data-ttu-id="db677-221">Pokud server není v tomto intervalu odeslání odpovědi handshake, klient zruší handshake a aktivační události `Closed` událostí (`onclose` v JavaScriptu).</span><span class="sxs-lookup"><span data-stu-id="db677-221">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="db677-222">Toto je upřesňující nastavení, by měla být změněna pouze v případě chyby časového limitu handshake dochází z důvodu závažné sítích s latencí.</span><span class="sxs-lookup"><span data-stu-id="db677-222">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="db677-223">Další podrobnosti o procesu najdete v článku [specifikace protokolu rozbočovače SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="db677-223">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="db677-224">V klientovi .NET jsou zadané hodnoty časového limitu jako `TimeSpan` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="db677-224">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="db677-225">V klientovi JavaScript jsou zadané hodnoty časového limitu jako číslo určující dobu trvání v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="db677-225">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="db677-226">Konfigurace dalších možností</span><span class="sxs-lookup"><span data-stu-id="db677-226">Configure additional options</span></span>

<span data-ttu-id="db677-227">Další možnosti se dá nakonfigurovat v `WithUrl` (`withUrl` v JavaScriptu) metoda `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="db677-227">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="db677-228">.NET – možnost</span><span class="sxs-lookup"><span data-stu-id="db677-228">.NET Option</span></span> | <span data-ttu-id="db677-229">Možnost jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="db677-229">JavaScript Option</span></span> | <span data-ttu-id="db677-230">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="db677-230">Default Value</span></span> | <span data-ttu-id="db677-231">Popis</span><span class="sxs-lookup"><span data-stu-id="db677-231">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="db677-232">Funkce vrátí řetězec, který je k dispozici jako token nosiče ověřování v požadavcích HTTP.</span><span class="sxs-lookup"><span data-stu-id="db677-232">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="db677-233">Nastavte na `true` přeskočit krok vyjednávání.</span><span class="sxs-lookup"><span data-stu-id="db677-233">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="db677-234">**Podporuje jenom při přenosu objekty Websocket je jediný povolený přenos**.</span><span class="sxs-lookup"><span data-stu-id="db677-234">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="db677-235">Toto nastavení není možné při použití služby Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="db677-235">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="db677-236">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="db677-236">Not configurable \*</span></span> | <span data-ttu-id="db677-237">prázdný</span><span class="sxs-lookup"><span data-stu-id="db677-237">Empty</span></span> | <span data-ttu-id="db677-238">Kolekce certifikáty TLS odeslat k ověření požadavků.</span><span class="sxs-lookup"><span data-stu-id="db677-238">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="db677-239">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="db677-239">Not configurable \*</span></span> | <span data-ttu-id="db677-240">prázdný</span><span class="sxs-lookup"><span data-stu-id="db677-240">Empty</span></span> | <span data-ttu-id="db677-241">Kolekce souborů cookie protokolu HTTP k odeslání při každé žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="db677-241">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="db677-242">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="db677-242">Not configurable \*</span></span> | <span data-ttu-id="db677-243">prázdný</span><span class="sxs-lookup"><span data-stu-id="db677-243">Empty</span></span> | <span data-ttu-id="db677-244">Přihlašovací údaje pro odesílání při každé žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="db677-244">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="db677-245">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="db677-245">Not configurable \*</span></span> | <span data-ttu-id="db677-246">5 sekund</span><span class="sxs-lookup"><span data-stu-id="db677-246">5 seconds</span></span> | <span data-ttu-id="db677-247">Pouze objekty Websocket.</span><span class="sxs-lookup"><span data-stu-id="db677-247">WebSockets only.</span></span> <span data-ttu-id="db677-248">Maximální množství času, klient počká po uzavření pro server a potvrďte žádosti o uzavření.</span><span class="sxs-lookup"><span data-stu-id="db677-248">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="db677-249">Pokud server není v tuto chvíli Potvrdit uzavření, klient neodpojí.</span><span class="sxs-lookup"><span data-stu-id="db677-249">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="db677-250">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="db677-250">Not configurable \*</span></span> | <span data-ttu-id="db677-251">prázdný</span><span class="sxs-lookup"><span data-stu-id="db677-251">Empty</span></span> | <span data-ttu-id="db677-252">Slovník další hlavičky protokolu HTTP k odeslání při každé žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="db677-252">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="db677-253">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="db677-253">Not configurable \*</span></span> | `null` | <span data-ttu-id="db677-254">Delegát, který lze použít ke konfiguraci nebo nahradit `HttpMessageHandler` používaný k odesílání požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="db677-254">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="db677-255">Není možné použít u připojení pomocí protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="db677-255">Not used for WebSocket connections.</span></span> <span data-ttu-id="db677-256">Tento delegát musí vrátit nenulovou hodnotu, a přijímá jako parametr výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="db677-256">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="db677-257">Upravte nastavení podle této výchozí hodnoty a vrácení nebo vrátí novou `HttpMessageHandler` instance.</span><span class="sxs-lookup"><span data-stu-id="db677-257">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="db677-258">**Při nahrazování obslužnou rutinu nezapomeňte si zkopírovat na nastavení, která chcete zachovat od zadané obslužné rutiny, v opačném případě nakonfigurovaných možností (například soubory cookie a hlavičky) se nevztahuje na novou obslužnou rutinu.**</span><span class="sxs-lookup"><span data-stu-id="db677-258">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="db677-259">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="db677-259">Not configurable \*</span></span> | `null` | <span data-ttu-id="db677-260">Proxy server HTTP při odesílání požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="db677-260">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="db677-261">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="db677-261">Not configurable \*</span></span> | `false` | <span data-ttu-id="db677-262">Nastavte tuto logickou hodnotu k odeslání výchozí přihlašovací údaje pro požadavky HTTP a Websocket.</span><span class="sxs-lookup"><span data-stu-id="db677-262">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="db677-263">To umožňuje použití ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="db677-263">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="db677-264">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="db677-264">Not configurable \*</span></span> | `null` | <span data-ttu-id="db677-265">Delegát, který slouží k nakonfigurování dalších možností protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="db677-265">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="db677-266">Obdrží instanci [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , který lze použít ke konfiguraci možností.</span><span class="sxs-lookup"><span data-stu-id="db677-266">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="db677-267">Možnosti s hvězdičkou (\*) nejsou konfigurovatelné v klientovi JavaScript z důvodu omezení v prohlížeči rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="db677-267">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="db677-268">V klientovi .NET tyto možnosti je možné upravovat prostřednictvím delegáta možnosti poskytnuté `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="db677-268">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="db677-269">V klientovi JavaScript tyto možnosti lze zadat v objektu jazyka JavaScript poskytuje k `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="db677-269">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="db677-270">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="db677-270">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
