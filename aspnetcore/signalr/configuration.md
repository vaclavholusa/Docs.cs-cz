---
title: Konfigurace ASP.NET Core SignalR
author: rachelappel
description: Informace o konfiguraci aplikace ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36962452"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="e2677-103">Konfigurace ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e2677-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="e2677-104">Serializace JSON nebo MessagePack možnosti</span><span class="sxs-lookup"><span data-stu-id="e2677-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="e2677-105">Funkce SignalR technologie ASP.NET Core podporuje dva protokoly pro kódování zpráv: [JSON](https://www.json.org/) a [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="e2677-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="e2677-106">Každý protokol obsahuje možnosti konfigurace serializace.</span><span class="sxs-lookup"><span data-stu-id="e2677-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="e2677-107">Serializace JSON se dá konfigurovat na serveru pomocí [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metoda rozšíření, které lze přidat po [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) v vaší `Startup.ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="e2677-107">JSON serialization can be configured on the server using the [`AddJsonProtocol`](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="e2677-108">`AddJsonProtocol` Metoda přebírá delegáta, který obdrží `options` objektu.</span><span class="sxs-lookup"><span data-stu-id="e2677-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="e2677-109">[ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) Vlastnost na tento objekt je JSON.NET `JsonSerializerSettings` objekt, který můžete použít ke konfiguraci serializace argumentů a návratové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e2677-109">The [`PayloadSerializerSettings`](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="e2677-110">Najdete v článku [JSON.NET dokumentace](https://www.newtonsoft.com/json/help/html/Introduction.htm) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e2677-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="e2677-111">Jako příklad ke konfiguraci serializátoru, který je pro použití názvy vlastností "PascalCase" místo "camelCase" výchozí názvy, použijte následující kód:</span><span class="sxs-lookup"><span data-stu-id="e2677-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="e2677-112">V klientovi .NET stejné `AddJsonHubProtocol` metoda rozšíření existuje v [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="e2677-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="e2677-113">`Microsoft.Extensions.DependencyInjection` Oboru názvů musí být importovány na resolve – metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="e2677-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="e2677-114">Není možné nakonfigurovat serializace JSON v JavaScript klienta v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="e2677-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="e2677-115">Možnosti serializace MessagePack</span><span class="sxs-lookup"><span data-stu-id="e2677-115">MessagePack serialization options</span></span>

<span data-ttu-id="e2677-116">Tím, že poskytuje delegáta, kterého lze nakonfigurovat MessagePack serializace [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) volání.</span><span class="sxs-lookup"><span data-stu-id="e2677-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="e2677-117">V tématu [MessagePack v systému SignalR](xref:signalr/messagepackhubprotocol) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e2677-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="e2677-118">Není možné nakonfigurovat MessagePack serializace v JavaScript klienta v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="e2677-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="e2677-119">Konfigurovat možnosti serveru</span><span class="sxs-lookup"><span data-stu-id="e2677-119">Configure server options</span></span>

<span data-ttu-id="e2677-120">Následující tabulka popisuje možnosti pro konfiguraci rozbočovače SignalR:</span><span class="sxs-lookup"><span data-stu-id="e2677-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="e2677-121">Možnost</span><span class="sxs-lookup"><span data-stu-id="e2677-121">Option</span></span> | <span data-ttu-id="e2677-122">Popis</span><span class="sxs-lookup"><span data-stu-id="e2677-122">Description</span></span> |
| ------ | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="e2677-123">Pokud klient není v rámci tohoto intervalu odešle zprávu počáteční handshake, je připojení ukončeno.</span><span class="sxs-lookup"><span data-stu-id="e2677-123">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="e2677-124">Pokud server neodeslal zprávu v rámci tento interval, je automaticky odeslána zpráva ping nechat otevřené připojení.</span><span class="sxs-lookup"><span data-stu-id="e2677-124">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="e2677-125">Protokolů podporovaných toto centrum.</span><span class="sxs-lookup"><span data-stu-id="e2677-125">Protocols supported by this hub.</span></span> <span data-ttu-id="e2677-126">Ve výchozím nastavení jsou povolené všechny protokoly, které jsou registrované na serveru, ale protokoly můžete odebrat z tohoto seznamu zakázat konkrétními protokoly pro jednotlivé rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="e2677-126">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | <span data-ttu-id="e2677-127">Pokud `true`, podrobné zprávy o výjimkách se vrátíte na klienty, pokud je vyvolána výjimka v metodě rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="e2677-127">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="e2677-128">Výchozí hodnota je `false`, jak tyto zprávy o výjimkách mohou obsahovat citlivé údaje.</span><span class="sxs-lookup"><span data-stu-id="e2677-128">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="e2677-129">Možnosti lze konfigurovat pro všechny rozbočovače tím, že poskytuje delegáta možnosti na `AddSignalR` volání v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e2677-129">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="e2677-130">Možnosti pro jednoho rozbočovače přepsat globální možností v `AddSignalR` a dá se nakonfigurovat pomocí [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="e2677-130">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [`AddHubOptions<T>`](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="e2677-131">Použití `HttpConnectionDispatcherOptions` nakonfigurovat upřesňující nastavení související s přenosy a správa vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="e2677-131">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="e2677-132">Tyto možnosti jsou nakonfigurované pomocí předání delegáta, kterého [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="e2677-132">These options are configured by passing a delegate to [`MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="e2677-133">Možnost</span><span class="sxs-lookup"><span data-stu-id="e2677-133">Option</span></span> | <span data-ttu-id="e2677-134">Popis</span><span class="sxs-lookup"><span data-stu-id="e2677-134">Description</span></span> |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="e2677-135">Maximální počet bajtů přijatých z klienta, serveru vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="e2677-135">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="e2677-136">Zvýšení hodnoty tuto umožňuje serveru přijímat větší zprávy, ale může mít negativní vliv na využití paměti.</span><span class="sxs-lookup"><span data-stu-id="e2677-136">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="e2677-137">Výchozí hodnota je 32KB.</span><span class="sxs-lookup"><span data-stu-id="e2677-137">The default value is 32KB.</span></span> |
| `AuthorizationData` | <span data-ttu-id="e2677-138">Seznam [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objekty používané k určení, pokud je klient autorizaci k připojení k rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="e2677-138">A list of [`IAuthorizeData`](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> <span data-ttu-id="e2677-139">Ve výchozím nastavení, to je naplněnou hodnotami `Authorize` atributy použité u třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="e2677-139">By default, this is populated with values from the `Authorize` attributes applied to the Hub class.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="e2677-140">Maximální počet bajtů odeslaných aplikací, vyrovnávací paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="e2677-140">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="e2677-141">Zvýšení hodnoty tuto umožňuje serveru odesílat větší zprávy, ale může mít negativní vliv na využití paměti.</span><span class="sxs-lookup"><span data-stu-id="e2677-141">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="e2677-142">Výchozí hodnota je 32KB.</span><span class="sxs-lookup"><span data-stu-id="e2677-142">The default value is 32KB.</span></span> |
| `Transports` | <span data-ttu-id="e2677-143">Bitová maska s `HttpTransportType` hodnoty, které můžete omezit přenosy klienta můžete použít pro připojení.</span><span class="sxs-lookup"><span data-stu-id="e2677-143">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> <span data-ttu-id="e2677-144">Ve výchozím nastavení jsou povolené všechny přenosy.</span><span class="sxs-lookup"><span data-stu-id="e2677-144">All transports are enabled by default.</span></span> |
| `LongPolling` | <span data-ttu-id="e2677-145">Další možnosti specifické pro dlouhé dotazování přenosu.</span><span class="sxs-lookup"><span data-stu-id="e2677-145">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="e2677-146">Další možnosti specifické pro objekty WebSockets přenosu.</span><span class="sxs-lookup"><span data-stu-id="e2677-146">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="e2677-147">Dlouhé dotazování přenosu má další možnosti, které lze nakonfigurovat pomocí `LongPolling` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="e2677-147">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="e2677-148">Možnost</span><span class="sxs-lookup"><span data-stu-id="e2677-148">Option</span></span> | <span data-ttu-id="e2677-149">Popis</span><span class="sxs-lookup"><span data-stu-id="e2677-149">Description</span></span> |
| ------ | ----------- |
| `PollTimeout` | <span data-ttu-id="e2677-150">Maximální množství času server čeká na zprávu k odeslání do klienta před ukončením žádost o jeden dotazování.</span><span class="sxs-lookup"><span data-stu-id="e2677-150">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="e2677-151">Snížení hodnoty způsobí, že klient k vydání nové žádosti o dotazování častěji.</span><span class="sxs-lookup"><span data-stu-id="e2677-151">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> <span data-ttu-id="e2677-152">Výchozí hodnota je 90 sekund.</span><span class="sxs-lookup"><span data-stu-id="e2677-152">The default value is 90 seconds.</span></span> |

<span data-ttu-id="e2677-153">Přenos protokolu WebSocket obsahuje další možnosti, které lze nakonfigurovat pomocí `WebSockets` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="e2677-153">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="e2677-154">Možnost</span><span class="sxs-lookup"><span data-stu-id="e2677-154">Option</span></span> | <span data-ttu-id="e2677-155">Popis</span><span class="sxs-lookup"><span data-stu-id="e2677-155">Description</span></span> |
| ------ | ----------- |
| `CloseTimeout` | <span data-ttu-id="e2677-156">Po zavření serveru, pokud se klientovi nepodaří zavřete v rámci tohoto intervalu, připojení bylo ukončeno.</span><span class="sxs-lookup"><span data-stu-id="e2677-156">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | <span data-ttu-id="e2677-157">Delegát, který můžete použít k nastavení `Sec-WebSocket-Protocol` na hodnotu vlastní hlavičky.</span><span class="sxs-lookup"><span data-stu-id="e2677-157">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="e2677-158">Delegát přijímá hodnoty požadavku klienta jako vstup a zpět požadovanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e2677-158">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="e2677-159">Konfigurace možností klienta</span><span class="sxs-lookup"><span data-stu-id="e2677-159">Configure client options</span></span>

<span data-ttu-id="e2677-160">Možnosti klienta lze nakonfigurovat podle `HubConnectionBuilder` typu (k dispozici v rozhraní .NET a JavaScript klientů), stejně jako na `HubConnection` sám sebe.</span><span class="sxs-lookup"><span data-stu-id="e2677-160">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="e2677-161">Konfigurace protokolování</span><span class="sxs-lookup"><span data-stu-id="e2677-161">Configure logging</span></span>

<span data-ttu-id="e2677-162">Protokolování je nastaveno v rozhraní .NET klienta pomocí `ConfigureLogging` metoda.</span><span class="sxs-lookup"><span data-stu-id="e2677-162">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="e2677-163">Protokolování zprostředkovatelé a filtry lze registrovat stejným způsobem, jako jsou na serveru.</span><span class="sxs-lookup"><span data-stu-id="e2677-163">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="e2677-164">Najdete v článku [protokolování v ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) Další informace naleznete v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="e2677-164">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="e2677-165">Chcete-li zaregistrovat zprostředkovatelé protokolování, je nutné nainstalovat nezbytných balíčků.</span><span class="sxs-lookup"><span data-stu-id="e2677-165">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="e2677-166">Najdete v článku [integrované protokolování zprostředkovatelé](xref:fundamentals/logging/index#built-in-logging-providers) části dokumentace pro úplný seznam.</span><span class="sxs-lookup"><span data-stu-id="e2677-166">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="e2677-167">Například konzole protokolování povolit, nainstalujte `Microsoft.Extensions.Logging.Console` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="e2677-167">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="e2677-168">Volání `AddConsole` metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="e2677-168">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="e2677-169">V klientovi JavaScript podobné `configureLogging` metoda existuje.</span><span class="sxs-lookup"><span data-stu-id="e2677-169">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="e2677-170">Zadejte `LogLevel` hodnotu, která určuje minimální úroveň protokolu zprávy a pokuste se vytvořit.</span><span class="sxs-lookup"><span data-stu-id="e2677-170">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="e2677-171">Protokoly se zapisují do okna konzoly prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e2677-171">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="e2677-172">Pokud chcete zakázat protokolování zcela, zadejte `signalR.LogLevel.None` v `configureLogging` metoda.</span><span class="sxs-lookup"><span data-stu-id="e2677-172">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="e2677-173">Úrovně protokolu dostupných pro klienta JavaScript jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="e2677-173">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="e2677-174">Nastavení úrovně protokolu na jednu z těchto hodnot umožňuje protokolování zpráv v **nebo vyšší** této úrovni.</span><span class="sxs-lookup"><span data-stu-id="e2677-174">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="e2677-175">Úroveň</span><span class="sxs-lookup"><span data-stu-id="e2677-175">Level</span></span> | <span data-ttu-id="e2677-176">Popis</span><span class="sxs-lookup"><span data-stu-id="e2677-176">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="e2677-177">Jsou protokolovány žádné zprávy.</span><span class="sxs-lookup"><span data-stu-id="e2677-177">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="e2677-178">Zprávy, které indikují chybu v celé aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e2677-178">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="e2677-179">Zprávy, které indikují chybu v aktuální operace.</span><span class="sxs-lookup"><span data-stu-id="e2677-179">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="e2677-180">Zprávy, které indikují nezávažné problém.</span><span class="sxs-lookup"><span data-stu-id="e2677-180">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="e2677-181">Informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="e2677-181">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="e2677-182">Diagnostické zprávy pro ladění.</span><span class="sxs-lookup"><span data-stu-id="e2677-182">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="e2677-183">Velmi podrobné diagnostické zprávy určené pro diagnostikování konkrétní problémy.</span><span class="sxs-lookup"><span data-stu-id="e2677-183">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="e2677-184">Konfigurace povolené přenosy</span><span class="sxs-lookup"><span data-stu-id="e2677-184">Configure allowed transports</span></span>

<span data-ttu-id="e2677-185">Přenosy systém signalr se dá nakonfigurovat v `WithUrl` volání (`withUrl` v jazyce JavaScript).</span><span class="sxs-lookup"><span data-stu-id="e2677-185">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="e2677-186">Bitové operace OR hodnoty `HttpTransportType` slouží k omezení klientovi použít pouze zadané přenosy.</span><span class="sxs-lookup"><span data-stu-id="e2677-186">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="e2677-187">Ve výchozím nastavení jsou povolené všechny přenosy.</span><span class="sxs-lookup"><span data-stu-id="e2677-187">All transports are enabled by default.</span></span>

<span data-ttu-id="e2677-188">Například zakázat přenos Server-Sent události, ale povolte objekty WebSockets a dlouhé dotazování připojení:</span><span class="sxs-lookup"><span data-stu-id="e2677-188">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="e2677-189">V klientovi JavaScript, jsou přenosy nakonfigurované nastavením `transport` na zadaný pro objekt možnosti `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="e2677-189">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="e2677-190">Konfigurace ověřování nosiče</span><span class="sxs-lookup"><span data-stu-id="e2677-190">Configure bearer authentication</span></span>

<span data-ttu-id="e2677-191">Pokud chcete zadat data ověřování společně s požadavky SignalR, použijte `AccessTokenProvider` možnost (`accessTokenFactory` v jazyce JavaScript) k určení funkci, která vrátí požadované přístupový token.</span><span class="sxs-lookup"><span data-stu-id="e2677-191">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="e2677-192">V rozhraní .NET klienta, je předaná tento token přístupu jako HTTP "Ověřování nosiče" tokenu (pomocí `Authorization` záhlaví s typem `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="e2677-192">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="e2677-193">V klientovi JavaScript přístupový token slouží jako token nosiče **s výjimkou** v určitých případech, kdy prohlížeč rozhraní API omezit schopnosti uplatňovat hlavičky (konkrétně v Server-Sent události a WebSockets žádostí).</span><span class="sxs-lookup"><span data-stu-id="e2677-193">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="e2677-194">V těchto případech je přístupový token zadaný jako hodnota řetězce dotazu `access_token`.</span><span class="sxs-lookup"><span data-stu-id="e2677-194">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="e2677-195">V klientovi .NET `AccessTokenProvider` možnost lze zadat pomocí možnosti delegáta v `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="e2677-195">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="e2677-196">V klientovi JavaScript přístupový token nakonfiguroval nastavení `accessTokenFactory` pole možnosti objektu ve `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="e2677-196">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="e2677-197">Nakonfigurujte časový limit a udržování možnosti</span><span class="sxs-lookup"><span data-stu-id="e2677-197">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="e2677-198">Další možnosti pro konfiguraci časového limitu a udržování chování jsou k dispozici na `HubConnection` samotného objektu:</span><span class="sxs-lookup"><span data-stu-id="e2677-198">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="e2677-199">Možnost rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="e2677-199">.NET Option</span></span> | <span data-ttu-id="e2677-200">JavaScript – možnost</span><span class="sxs-lookup"><span data-stu-id="e2677-200">JavaScript Option</span></span> | <span data-ttu-id="e2677-201">Popis</span><span class="sxs-lookup"><span data-stu-id="e2677-201">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="e2677-202">Časový limit aktivity serveru.</span><span class="sxs-lookup"><span data-stu-id="e2677-202">Timeout for server activity.</span></span> <span data-ttu-id="e2677-203">Pokud server není v tomto intervalu odeslal jakékoli zprávy, klient považuje server odpojí a aktivační události `Closed` událostí (`onclose` v jazyce JavaScript).</span><span class="sxs-lookup"><span data-stu-id="e2677-203">If the server hasn't sent any message in this interval, the client considers the server disconnected and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="e2677-204">Nejde konfigurovat</span><span class="sxs-lookup"><span data-stu-id="e2677-204">Not configurable</span></span> | <span data-ttu-id="e2677-205">Časový limit pro počáteční server handshake.</span><span class="sxs-lookup"><span data-stu-id="e2677-205">Timeout for initial server handshake.</span></span> <span data-ttu-id="e2677-206">Pokud server není v tomto intervalu odeslání odpovědi handshake, klient zruší metody handshake a aktivační události `Closed` událostí (`onclose` v jazyce JavaScript).</span><span class="sxs-lookup"><span data-stu-id="e2677-206">If the server doesn't send a handshake response in this interval, the client cancels the handshake and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |

<span data-ttu-id="e2677-207">V klientovi .NET, časový limit hodnoty jsou specifikované jako `TimeSpan` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e2677-207">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="e2677-208">V klientovi JavaScript jsou hodnoty časového limitu zadané jako čísla.</span><span class="sxs-lookup"><span data-stu-id="e2677-208">In the JavaScript client, timeout values are specified as numbers.</span></span> <span data-ttu-id="e2677-209">Čísla, která představují hodnoty času v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="e2677-209">The numbers represent time values in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="e2677-210">Konfigurace dalších možností</span><span class="sxs-lookup"><span data-stu-id="e2677-210">Configure additional options</span></span>

<span data-ttu-id="e2677-211">Další možnosti se dá nakonfigurovat v `WithUrl` (`withUrl` v jazyce JavaScript) metodu `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e2677-211">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="e2677-212">Možnost rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="e2677-212">.NET Option</span></span> | <span data-ttu-id="e2677-213">JavaScript – možnost</span><span class="sxs-lookup"><span data-stu-id="e2677-213">JavaScript Option</span></span> | <span data-ttu-id="e2677-214">Popis</span><span class="sxs-lookup"><span data-stu-id="e2677-214">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | <span data-ttu-id="e2677-215">Funkce vrátí řetězec, který je k dispozici jako token nosiče ověřování v požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2677-215">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | <span data-ttu-id="e2677-216">Tuto možnost nastavíte na `true` k vyjednávání krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="e2677-216">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="e2677-217">**Podporována pouze v případě technologie WebSockets přenosu je pouze povolené přenos**.</span><span class="sxs-lookup"><span data-stu-id="e2677-217">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="e2677-218">Toto nastavení nelze povolit, pokud používáte službu Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="e2677-218">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="e2677-219">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="e2677-219">Not configurable \*</span></span> | <span data-ttu-id="e2677-220">Kolekce odeslat k ověřování žádostí o certifikáty protokolu TLS.</span><span class="sxs-lookup"><span data-stu-id="e2677-220">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="e2677-221">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="e2677-221">Not configurable \*</span></span> | <span data-ttu-id="e2677-222">Kolekce souborů cookie HTTP k odeslání s každou žádostí HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2677-222">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="e2677-223">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="e2677-223">Not configurable \*</span></span> | <span data-ttu-id="e2677-224">Přihlašovací údaje odeslat spolu s každou žádost HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2677-224">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="e2677-225">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="e2677-225">Not configurable \*</span></span> | <span data-ttu-id="e2677-226">Pouze objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="e2677-226">WebSockets only.</span></span> <span data-ttu-id="e2677-227">Maximální množství času klient počká po zavření pro server, aby vzali na vědomí žádosti o uzavření.</span><span class="sxs-lookup"><span data-stu-id="e2677-227">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="e2677-228">Pokud server není v tuto chvíli vědomí ukončení, se klient neodpojí.</span><span class="sxs-lookup"><span data-stu-id="e2677-228">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="e2677-229">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="e2677-229">Not configurable \*</span></span> | <span data-ttu-id="e2677-230">Slovník další hlavičky protokolu HTTP k odeslání s každou žádostí HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2677-230">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="e2677-231">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="e2677-231">Not configurable \*</span></span> | <span data-ttu-id="e2677-232">Delegát, který lze nakonfigurovat, nebo nahrazení `HttpMessageHandler` používá k odesílání požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2677-232">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="e2677-233">Nepoužívá se pro připojení protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="e2677-233">Not used for WebSocket connections.</span></span> <span data-ttu-id="e2677-234">Tento delegát musí vracet hodnotu než null a obdrží výchozí hodnota jako parametr.</span><span class="sxs-lookup"><span data-stu-id="e2677-234">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="e2677-235">Buď změňte nastavení na této výchozí hodnotu a vrátit nebo vrátit úplně nové `HttpMessageHandler` instance.</span><span class="sxs-lookup"><span data-stu-id="e2677-235">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="e2677-236">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="e2677-236">Not configurable \*</span></span> | <span data-ttu-id="e2677-237">Proxy server HTTP pro použití při odesílání požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2677-237">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="e2677-238">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="e2677-238">Not configurable \*</span></span> | <span data-ttu-id="e2677-239">Nastavte tuto logickou hodnotu k odeslání výchozí pověření pro požadavky HTTP a objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="e2677-239">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="e2677-240">To umožňuje použití ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e2677-240">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="e2677-241">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="e2677-241">Not configurable \*</span></span> | <span data-ttu-id="e2677-242">Delegát, který můžete použít ke konfiguraci dalších možností protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="e2677-242">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="e2677-243">Obdrží instanci [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , můžete použít ke konfiguraci možností.</span><span class="sxs-lookup"><span data-stu-id="e2677-243">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="e2677-244">Možnosti, které jsou označené hvězdičkou (\*) nejsou konfigurovatelné v klientovi JavaScript z důvodu omezení v prohlížeči rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e2677-244">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="e2677-245">V rozhraní .NET klientovi tyto možnosti mohou být upravena delegáta možnosti poskytované `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="e2677-245">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="e2677-246">V klientovi JavaScript, tyto možnosti lze zadat do objekt jazyka JavaScript poskytované `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="e2677-246">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="e2677-247">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e2677-247">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
