---
title: Konfigurace jádra SignalR technologie ASP.NET
author: tdykstra
description: Zjistěte, jak nakonfigurovat aplikace SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: fac0226c939f4cf446c876b1c0b359d6c5b9dfd3
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095399"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="ee83c-103">Konfigurace jádra SignalR technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ee83c-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="ee83c-104">Serializace JSON/MessagePack možnosti</span><span class="sxs-lookup"><span data-stu-id="ee83c-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="ee83c-105">Funkce SignalR technologie ASP.NET Core pro kódování zpráv podporuje dva protokoly: [JSON](https://www.json.org/) a [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="ee83c-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="ee83c-106">Všechny protokoly, které obsahuje možnosti konfigurace serializace.</span><span class="sxs-lookup"><span data-stu-id="ee83c-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="ee83c-107">Serializace JSON lze nastavit na serveru pomocí [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metody rozšíření, které mohou být přidány po [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) v vaše `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="ee83c-107">JSON serialization can be configured on the server using the [`AddJsonProtocol`](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ee83c-108">`AddJsonProtocol` Metoda přijímá delegát, který přijímá `options` objektu.</span><span class="sxs-lookup"><span data-stu-id="ee83c-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ee83c-109">[ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) Vlastnost k tomuto objektu je JSON.NET `JsonSerializerSettings` objekt, který můžete použít ke konfiguraci serializace argumenty a návratové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ee83c-109">The [`PayloadSerializerSettings`](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ee83c-110">Najdete v článku [JSON.NET dokumentaci](https://www.newtonsoft.com/json/help/html/Introduction.htm) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ee83c-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="ee83c-111">Jako příklad konfigurace serializátoru, který chcete použít místo výchozí názvy "camelCase", "PascalCase" názvy vlastností, pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="ee83c-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="ee83c-112">V klientovi .NET, stejné `AddJsonHubProtocol` – metoda rozšíření existuje v [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="ee83c-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ee83c-113">`Microsoft.Extensions.DependencyInjection` Obor názvů musí být importován do resolve – metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="ee83c-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="ee83c-114">Není možné konfigurovat pomocí serializace JSON klienta jazyka JavaScript v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="ee83c-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="ee83c-115">MessagePack možnosti serializace</span><span class="sxs-lookup"><span data-stu-id="ee83c-115">MessagePack serialization options</span></span>

<span data-ttu-id="ee83c-116">Poskytnutím delegáta, kterého lze nakonfigurovat MessagePack serializace [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) volání.</span><span class="sxs-lookup"><span data-stu-id="ee83c-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="ee83c-117">Zobrazit [MessagePack v knihovně SignalR](xref:signalr/messagepackhubprotocol) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ee83c-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="ee83c-118">Není možné konfigurovat MessagePack serializace pomocí jazyka JavaScript klienta v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="ee83c-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="ee83c-119">Konfigurovat možnosti serveru</span><span class="sxs-lookup"><span data-stu-id="ee83c-119">Configure server options</span></span>

<span data-ttu-id="ee83c-120">Následující tabulka popisuje možnosti pro konfiguraci rozbočovače SignalR:</span><span class="sxs-lookup"><span data-stu-id="ee83c-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="ee83c-121">Možnost</span><span class="sxs-lookup"><span data-stu-id="ee83c-121">Option</span></span> | <span data-ttu-id="ee83c-122">Popis</span><span class="sxs-lookup"><span data-stu-id="ee83c-122">Description</span></span> |
| ------ | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="ee83c-123">Pokud klient nebude odeslat zprávu handshake počáteční v tomto časovém intervalu, je připojení ukončeno.</span><span class="sxs-lookup"><span data-stu-id="ee83c-123">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ee83c-124">Pokud server není v rámci tohoto intervalu odeslal zprávu, je automaticky odeslána zpráva příkazu ping na udržení připojení otevřeného.</span><span class="sxs-lookup"><span data-stu-id="ee83c-124">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="ee83c-125">Protokolů podporovaných toto centrum.</span><span class="sxs-lookup"><span data-stu-id="ee83c-125">Protocols supported by this hub.</span></span> <span data-ttu-id="ee83c-126">Ve výchozím nastavení jsou povolené všechny protokoly, které jsou registrované na serveru, ale protokolů lze odebrat z tohoto seznamu zakázat konkrétní protokoly pro jednotlivé rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="ee83c-126">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | <span data-ttu-id="ee83c-127">Pokud `true`, podrobné zprávy o výjimkách se vrátí ke klientům, když dojde k výjimce v metodě rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="ee83c-127">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ee83c-128">Výchozí hodnota je `false`, jak tyto zprávy o výjimkách mohou obsahovat citlivé údaje.</span><span class="sxs-lookup"><span data-stu-id="ee83c-128">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="ee83c-129">Možnosti je možné nakonfigurovat pro všechna centra poskytnutím delegáta možnosti k `AddSignalR` volání v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ee83c-129">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="ee83c-130">Možnosti pro jedno centrum přepsat globální možnosti uvedené v `AddSignalR` a dá se nakonfigurovat pomocí [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="ee83c-130">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [`AddHubOptions<T>`](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="ee83c-131">Použití `HttpConnectionDispatcherOptions` konfigurace upřesňujících nastavení související s přenosy a správa vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="ee83c-131">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ee83c-132">Tyto možnosti jsou nakonfigurované pomocí předání delegáta, kterého [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="ee83c-132">These options are configured by passing a delegate to [`MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="ee83c-133">Možnost</span><span class="sxs-lookup"><span data-stu-id="ee83c-133">Option</span></span> | <span data-ttu-id="ee83c-134">Popis</span><span class="sxs-lookup"><span data-stu-id="ee83c-134">Description</span></span> |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ee83c-135">Maximální počet bajtů přijatých z klienta, který vyrovnávací paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="ee83c-135">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="ee83c-136">Zvýšení hodnoty tuto umožňuje serveru pro příjem větší zprávy, ale může mít negativní vliv na využití paměti.</span><span class="sxs-lookup"><span data-stu-id="ee83c-136">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="ee83c-137">Výchozí hodnota je 32KB.</span><span class="sxs-lookup"><span data-stu-id="ee83c-137">The default value is 32KB.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ee83c-138">Seznam [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objekty sloužící k určení, pokud je klient autorizaci k připojení k rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="ee83c-138">A list of [`IAuthorizeData`](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> <span data-ttu-id="ee83c-139">Ve výchozím nastavení, to je naplněnou hodnotami `Authorize` atributy použité u třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="ee83c-139">By default, this is populated with values from the `Authorize` attributes applied to the Hub class.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ee83c-140">Maximální počet bajtů odeslaných aplikace, která vyrovnávací paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="ee83c-140">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="ee83c-141">Zvýšení hodnoty tuto umožňuje serveru odesílat větší zprávy, ale může mít negativní vliv na využití paměti.</span><span class="sxs-lookup"><span data-stu-id="ee83c-141">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="ee83c-142">Výchozí hodnota je 32KB.</span><span class="sxs-lookup"><span data-stu-id="ee83c-142">The default value is 32KB.</span></span> |
| `Transports` | <span data-ttu-id="ee83c-143">Bitová maska z `HttpTransportType` hodnoty, které můžete omezit přenosy klienta můžete použít pro připojení.</span><span class="sxs-lookup"><span data-stu-id="ee83c-143">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> <span data-ttu-id="ee83c-144">Ve výchozím nastavení jsou povolené všechny přenosy.</span><span class="sxs-lookup"><span data-stu-id="ee83c-144">All transports are enabled by default.</span></span> |
| `LongPolling` | <span data-ttu-id="ee83c-145">Další možnosti specifické pro dlouhé dotazování přenosu.</span><span class="sxs-lookup"><span data-stu-id="ee83c-145">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ee83c-146">Další možnosti specifické pro přenos objekty Websocket.</span><span class="sxs-lookup"><span data-stu-id="ee83c-146">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="ee83c-147">Dlouhý interval dotazování přenosu má další možnosti, které lze konfigurovat pomocí `LongPolling` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="ee83c-147">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="ee83c-148">Možnost</span><span class="sxs-lookup"><span data-stu-id="ee83c-148">Option</span></span> | <span data-ttu-id="ee83c-149">Popis</span><span class="sxs-lookup"><span data-stu-id="ee83c-149">Description</span></span> |
| ------ | ----------- |
| `PollTimeout` | <span data-ttu-id="ee83c-150">Maximální množství času na server čeká na zprávu k odeslání do klienta před ukončením žádosti o jednotné hlasování.</span><span class="sxs-lookup"><span data-stu-id="ee83c-150">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="ee83c-151">Snížení hodnoty způsobí, že klient k vydávání nových žádostí o dotazování častěji.</span><span class="sxs-lookup"><span data-stu-id="ee83c-151">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> <span data-ttu-id="ee83c-152">Výchozí hodnota je 90 sekund.</span><span class="sxs-lookup"><span data-stu-id="ee83c-152">The default value is 90 seconds.</span></span> |

<span data-ttu-id="ee83c-153">Přenos pomocí protokolu WebSocket má další možnosti, které lze konfigurovat pomocí `WebSockets` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="ee83c-153">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="ee83c-154">Možnost</span><span class="sxs-lookup"><span data-stu-id="ee83c-154">Option</span></span> | <span data-ttu-id="ee83c-155">Popis</span><span class="sxs-lookup"><span data-stu-id="ee83c-155">Description</span></span> |
| ------ | ----------- |
| `CloseTimeout` | <span data-ttu-id="ee83c-156">Po zavření serveru, pokud se klientovi nepodaří zavřete v tomto časovém intervalu, připojení se ukončí.</span><span class="sxs-lookup"><span data-stu-id="ee83c-156">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | <span data-ttu-id="ee83c-157">Delegát, který je možné nastavit `Sec-WebSocket-Protocol` vlastní hodnoty záhlaví.</span><span class="sxs-lookup"><span data-stu-id="ee83c-157">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="ee83c-158">Delegát přijme hodnoty vyžádané klientem jako vstup a měl by vrátit na požadovanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ee83c-158">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="ee83c-159">Konfigurovat možnosti klienta</span><span class="sxs-lookup"><span data-stu-id="ee83c-159">Configure client options</span></span>

<span data-ttu-id="ee83c-160">Možnosti klienta lze nakonfigurovat podle `HubConnectionBuilder` typ (k dispozici v rozhraní .NET a JavaScript klientech), stejně jako na `HubConnection` samotný.</span><span class="sxs-lookup"><span data-stu-id="ee83c-160">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="ee83c-161">Konfigurace protokolování</span><span class="sxs-lookup"><span data-stu-id="ee83c-161">Configure logging</span></span>

<span data-ttu-id="ee83c-162">Protokolování je nakonfigurován v klientu .NET pomocí `ConfigureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="ee83c-162">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="ee83c-163">Protokolování poskytovatelů a filtry můžete zaregistrovat stejně, jako jsou na serveru.</span><span class="sxs-lookup"><span data-stu-id="ee83c-163">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="ee83c-164">Zobrazit [protokolování v ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) Další informace naleznete v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="ee83c-164">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="ee83c-165">Aby bylo možné zaregistrovat poskytovatele protokolování, je nutné nainstalovat potřebné balíčky.</span><span class="sxs-lookup"><span data-stu-id="ee83c-165">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="ee83c-166">Najdete v článku [vestavěné protokolování poskytovatelé](xref:fundamentals/logging/index#built-in-logging-providers) část dokumentace pro úplný seznam.</span><span class="sxs-lookup"><span data-stu-id="ee83c-166">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="ee83c-167">Například pokud chcete povolit protokolování konzoly, nainstalujte `Microsoft.Extensions.Logging.Console` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="ee83c-167">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="ee83c-168">Volání `AddConsole` – metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="ee83c-168">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="ee83c-169">V klientovi JavaScript podobný `configureLogging` metody existuje.</span><span class="sxs-lookup"><span data-stu-id="ee83c-169">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="ee83c-170">Zadejte `LogLevel` hodnotu, která minimální úroveň zprávy protokolu k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="ee83c-170">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="ee83c-171">Protokoly se zapisují do okna konzoly prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ee83c-171">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="ee83c-172">Chcete-li zakázat protokolování úplně, zadejte `signalR.LogLevel.None` v `configureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="ee83c-172">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="ee83c-173">K dispozici ke klientovi JavaScript úrovně protokolu jsou uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="ee83c-173">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="ee83c-174">Protokolování zpráv při nastavení na úrovni protokolu na jednu z těchto hodnot umožňuje **nebo vyšší** dané úrovni.</span><span class="sxs-lookup"><span data-stu-id="ee83c-174">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="ee83c-175">úroveň</span><span class="sxs-lookup"><span data-stu-id="ee83c-175">Level</span></span> | <span data-ttu-id="ee83c-176">Popis</span><span class="sxs-lookup"><span data-stu-id="ee83c-176">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="ee83c-177">Jsou zaznamenány žádné zprávy.</span><span class="sxs-lookup"><span data-stu-id="ee83c-177">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="ee83c-178">Zprávy, které indikují chybu celé aplikace.</span><span class="sxs-lookup"><span data-stu-id="ee83c-178">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="ee83c-179">Zprávy, které indikují chybu aktuální operaci.</span><span class="sxs-lookup"><span data-stu-id="ee83c-179">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="ee83c-180">Zprávy, které označují méně závažné potíže.</span><span class="sxs-lookup"><span data-stu-id="ee83c-180">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="ee83c-181">Informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="ee83c-181">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="ee83c-182">Diagnostické zprávy je užitečné pro ladění.</span><span class="sxs-lookup"><span data-stu-id="ee83c-182">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="ee83c-183">Velmi podrobné diagnostické zprávy, které jsou navržené pro diagnostiku specifické problémy.</span><span class="sxs-lookup"><span data-stu-id="ee83c-183">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="ee83c-184">Nakonfigurovat povolené přenosy</span><span class="sxs-lookup"><span data-stu-id="ee83c-184">Configure allowed transports</span></span>

<span data-ttu-id="ee83c-185">Přenosy systém signalr se dá nakonfigurovat v `WithUrl` volání (`withUrl` v JavaScriptu).</span><span class="sxs-lookup"><span data-stu-id="ee83c-185">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="ee83c-186">Bitový OR hodnoty `HttpTransportType` slouží k omezení klienta používat pouze zadaný přenosy.</span><span class="sxs-lookup"><span data-stu-id="ee83c-186">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="ee83c-187">Ve výchozím nastavení jsou povolené všechny přenosy.</span><span class="sxs-lookup"><span data-stu-id="ee83c-187">All transports are enabled by default.</span></span>

<span data-ttu-id="ee83c-188">Například zakázat přenos Server-Sent události, ale povolte připojení objekty Websocket a dlouhý interval dotazování:</span><span class="sxs-lookup"><span data-stu-id="ee83c-188">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="ee83c-189">V klientovi JavaScript přenosy jsou nakonfigurovány tak, že nastavíte `transport` na objekt možnosti poskytnuté `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="ee83c-189">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="ee83c-190">Konfigurace ověřování nosiče</span><span class="sxs-lookup"><span data-stu-id="ee83c-190">Configure bearer authentication</span></span>

<span data-ttu-id="ee83c-191">Pokud chcete poskytnout ověřovací data spolu s knihovnou SignalR požadavky, použijte `AccessTokenProvider` možnost (`accessTokenFactory` v JavaScriptu) k určení funkce, která vrací požadovaný přístupový token.</span><span class="sxs-lookup"><span data-stu-id="ee83c-191">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="ee83c-192">V klientovi .NET Tento přístupový token předaný jako HTTP "Ověřování nosiče" token (použití `Authorization` záhlaví s typem `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="ee83c-192">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="ee83c-193">V klientovi JavaScript přístupový token se používá jako nosný token **s výjimkou** v několika případech, kdy prohlížeč rozhraní API omezit schopnosti uplatňovat hlavičky (konkrétně v Server-Sent události a protokoly Websocket požadavky).</span><span class="sxs-lookup"><span data-stu-id="ee83c-193">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="ee83c-194">V těchto případech je přístupový token k dispozici jako hodnotu řetězce dotazu `access_token`.</span><span class="sxs-lookup"><span data-stu-id="ee83c-194">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="ee83c-195">V klientovi .NET `AccessTokenProvider` možnost lze zadat pomocí možnosti delegáta v `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="ee83c-195">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="ee83c-196">V klientovi JavaScript přístupový token je nakonfigurovaný tak, že nastavíte `accessTokenFactory` na objekt možnosti v `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="ee83c-196">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="ee83c-197">Konfigurace časového limitu a udržování možnosti</span><span class="sxs-lookup"><span data-stu-id="ee83c-197">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="ee83c-198">Další možnosti pro konfiguraci časového limitu a zachování chování, které jsou k dispozici na `HubConnection` samotného objektu:</span><span class="sxs-lookup"><span data-stu-id="ee83c-198">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="ee83c-199">.NET – možnost</span><span class="sxs-lookup"><span data-stu-id="ee83c-199">.NET Option</span></span> | <span data-ttu-id="ee83c-200">Možnost jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="ee83c-200">JavaScript Option</span></span> | <span data-ttu-id="ee83c-201">Popis</span><span class="sxs-lookup"><span data-stu-id="ee83c-201">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="ee83c-202">Časový limit pro aktivity serveru.</span><span class="sxs-lookup"><span data-stu-id="ee83c-202">Timeout for server activity.</span></span> <span data-ttu-id="ee83c-203">Pokud nějaká zpráva nebyla v tomto intervalu odeslané serverem, klient bude považovat za server odpojen a aktivační události `Closed` událostí (`onclose` v JavaScriptu).</span><span class="sxs-lookup"><span data-stu-id="ee83c-203">If the server hasn't sent any message in this interval, the client considers the server disconnected and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ee83c-204">Nejde konfigurovat</span><span class="sxs-lookup"><span data-stu-id="ee83c-204">Not configurable</span></span> | <span data-ttu-id="ee83c-205">Časový limit pro počáteční server handshake.</span><span class="sxs-lookup"><span data-stu-id="ee83c-205">Timeout for initial server handshake.</span></span> <span data-ttu-id="ee83c-206">Pokud server není v tomto intervalu odeslání odpovědi handshake, klient zruší handshake a aktivační události `Closed` událostí (`onclose` v JavaScriptu).</span><span class="sxs-lookup"><span data-stu-id="ee83c-206">If the server doesn't send a handshake response in this interval, the client cancels the handshake and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |

<span data-ttu-id="ee83c-207">V klientovi .NET jsou zadané hodnoty časového limitu jako `TimeSpan` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ee83c-207">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="ee83c-208">V klientovi JavaScript jsou hodnoty časového limitu zadané jako čísla.</span><span class="sxs-lookup"><span data-stu-id="ee83c-208">In the JavaScript client, timeout values are specified as numbers.</span></span> <span data-ttu-id="ee83c-209">Čísla udávají hodnoty času v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="ee83c-209">The numbers represent time values in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="ee83c-210">Konfigurace dalších možností</span><span class="sxs-lookup"><span data-stu-id="ee83c-210">Configure additional options</span></span>

<span data-ttu-id="ee83c-211">Další možnosti se dá nakonfigurovat v `WithUrl` (`withUrl` v JavaScriptu) metoda `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="ee83c-211">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="ee83c-212">.NET – možnost</span><span class="sxs-lookup"><span data-stu-id="ee83c-212">.NET Option</span></span> | <span data-ttu-id="ee83c-213">Možnost jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="ee83c-213">JavaScript Option</span></span> | <span data-ttu-id="ee83c-214">Popis</span><span class="sxs-lookup"><span data-stu-id="ee83c-214">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | <span data-ttu-id="ee83c-215">Funkce vrátí řetězec, který je k dispozici jako token nosiče ověřování v požadavcích HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee83c-215">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | <span data-ttu-id="ee83c-216">Nastavte na `true` přeskočit krok vyjednávání.</span><span class="sxs-lookup"><span data-stu-id="ee83c-216">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ee83c-217">**Podporuje jenom při přenosu objekty Websocket je jediný povolený přenos**.</span><span class="sxs-lookup"><span data-stu-id="ee83c-217">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ee83c-218">Toto nastavení není možné při použití služby Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="ee83c-218">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="ee83c-219">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="ee83c-219">Not configurable \*</span></span> | <span data-ttu-id="ee83c-220">Kolekce certifikáty TLS odeslat k ověření požadavků.</span><span class="sxs-lookup"><span data-stu-id="ee83c-220">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="ee83c-221">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="ee83c-221">Not configurable \*</span></span> | <span data-ttu-id="ee83c-222">Kolekce souborů cookie protokolu HTTP k odeslání při každé žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee83c-222">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="ee83c-223">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="ee83c-223">Not configurable \*</span></span> | <span data-ttu-id="ee83c-224">Přihlašovací údaje pro odesílání při každé žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee83c-224">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="ee83c-225">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="ee83c-225">Not configurable \*</span></span> | <span data-ttu-id="ee83c-226">Pouze objekty Websocket.</span><span class="sxs-lookup"><span data-stu-id="ee83c-226">WebSockets only.</span></span> <span data-ttu-id="ee83c-227">Maximální množství času, klient počká po uzavření pro server a potvrďte žádosti o uzavření.</span><span class="sxs-lookup"><span data-stu-id="ee83c-227">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="ee83c-228">Pokud server není v tuto chvíli Potvrdit uzavření, klient neodpojí.</span><span class="sxs-lookup"><span data-stu-id="ee83c-228">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="ee83c-229">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="ee83c-229">Not configurable \*</span></span> | <span data-ttu-id="ee83c-230">Slovník další hlavičky protokolu HTTP k odeslání při každé žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee83c-230">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="ee83c-231">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="ee83c-231">Not configurable \*</span></span> | <span data-ttu-id="ee83c-232">Delegát, který lze použít ke konfiguraci nebo nahradit `HttpMessageHandler` používaný k odesílání požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee83c-232">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="ee83c-233">Není možné použít u připojení pomocí protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ee83c-233">Not used for WebSocket connections.</span></span> <span data-ttu-id="ee83c-234">Tento delegát musí vrátit nenulovou hodnotu, a přijímá jako parametr výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ee83c-234">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="ee83c-235">Upravte nastavení podle této výchozí hodnoty a vrácení nebo vrátit úplně nové `HttpMessageHandler` instance.</span><span class="sxs-lookup"><span data-stu-id="ee83c-235">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="ee83c-236">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="ee83c-236">Not configurable \*</span></span> | <span data-ttu-id="ee83c-237">Proxy server HTTP při odesílání požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee83c-237">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="ee83c-238">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="ee83c-238">Not configurable \*</span></span> | <span data-ttu-id="ee83c-239">Nastavte tuto logickou hodnotu k odeslání výchozí přihlašovací údaje pro požadavky HTTP a Websocket.</span><span class="sxs-lookup"><span data-stu-id="ee83c-239">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="ee83c-240">To umožňuje použití ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="ee83c-240">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="ee83c-241">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="ee83c-241">Not configurable \*</span></span> | <span data-ttu-id="ee83c-242">Delegát, který slouží k nakonfigurování dalších možností protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ee83c-242">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="ee83c-243">Obdrží instanci [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , který lze použít ke konfiguraci možností.</span><span class="sxs-lookup"><span data-stu-id="ee83c-243">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="ee83c-244">Možnosti s hvězdičkou (\*) nejsou konfigurovatelné v klientovi JavaScript z důvodu omezení v prohlížeči rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ee83c-244">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="ee83c-245">V klientovi .NET tyto možnosti je možné upravovat prostřednictvím delegáta možnosti poskytnuté `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="ee83c-245">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="ee83c-246">V klientovi JavaScript tyto možnosti lze zadat v objektu jazyka JavaScript poskytuje k `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="ee83c-246">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="ee83c-247">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ee83c-247">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
