---
title: Konfigurace jádra SignalR technologie ASP.NET
author: tdykstra
description: Zjistěte, jak nakonfigurovat aplikace SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/configuration
ms.openlocfilehash: 72fc53cad7caf55e85d0668c9dbea1a70fc8674b
ms.sourcegitcommit: c684eb6c0999d11d19e15e65939e5c7f99ba47df
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/19/2018
ms.locfileid: "46292333"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="9567b-103">Konfigurace jádra SignalR technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9567b-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="9567b-104">Serializace JSON/MessagePack možnosti</span><span class="sxs-lookup"><span data-stu-id="9567b-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="9567b-105">Funkce SignalR technologie ASP.NET Core pro kódování zpráv podporuje dva protokoly: [JSON](https://www.json.org/) a [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="9567b-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="9567b-106">Všechny protokoly, které obsahuje možnosti konfigurace serializace.</span><span class="sxs-lookup"><span data-stu-id="9567b-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="9567b-107">Serializace JSON lze nastavit na serveru pomocí [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metody rozšíření, které mohou být přidány po [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) v vaše `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="9567b-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="9567b-108">`AddJsonProtocol` Metoda přijímá delegát, který přijímá `options` objektu.</span><span class="sxs-lookup"><span data-stu-id="9567b-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="9567b-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) vlastnost k tomuto objektu je JSON.NET `JsonSerializerSettings` objekt, který můžete použít ke konfiguraci serializace argumenty a návratové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9567b-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="9567b-110">Najdete v článku [JSON.NET dokumentaci](https://www.newtonsoft.com/json/help/html/Introduction.htm) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9567b-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="9567b-111">Jako příklad konfigurace serializátoru, který chcete použít místo výchozí názvy "camelCase", "PascalCase" názvy vlastností, pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="9567b-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="9567b-112">V klientovi .NET, stejné `AddJsonProtocol` – metoda rozšíření existuje v [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="9567b-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="9567b-113">`Microsoft.Extensions.DependencyInjection` Obor názvů musí být importován do resolve – metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="9567b-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="9567b-114">Není možné konfigurovat pomocí serializace JSON klienta jazyka JavaScript v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="9567b-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="9567b-115">MessagePack možnosti serializace</span><span class="sxs-lookup"><span data-stu-id="9567b-115">MessagePack serialization options</span></span>

<span data-ttu-id="9567b-116">Poskytnutím delegáta, kterého lze nakonfigurovat MessagePack serializace [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) volání.</span><span class="sxs-lookup"><span data-stu-id="9567b-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="9567b-117">Zobrazit [MessagePack v knihovně SignalR](xref:signalr/messagepackhubprotocol) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9567b-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="9567b-118">Není možné konfigurovat MessagePack serializace pomocí jazyka JavaScript klienta v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="9567b-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="9567b-119">Konfigurovat možnosti serveru</span><span class="sxs-lookup"><span data-stu-id="9567b-119">Configure server options</span></span>

<span data-ttu-id="9567b-120">Následující tabulka popisuje možnosti pro konfiguraci rozbočovače SignalR:</span><span class="sxs-lookup"><span data-stu-id="9567b-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="9567b-121">Možnost</span><span class="sxs-lookup"><span data-stu-id="9567b-121">Option</span></span> | <span data-ttu-id="9567b-122">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="9567b-122">Default Value</span></span> | <span data-ttu-id="9567b-123">Popis</span><span class="sxs-lookup"><span data-stu-id="9567b-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="9567b-124">15 sekund</span><span class="sxs-lookup"><span data-stu-id="9567b-124">15 seconds</span></span> | <span data-ttu-id="9567b-125">Pokud klient nebude odeslat zprávu handshake počáteční v tomto časovém intervalu, je připojení ukončeno.</span><span class="sxs-lookup"><span data-stu-id="9567b-125">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="9567b-126">Toto je upřesňující nastavení, by měla být změněna pouze v případě chyby časového limitu handshake dochází z důvodu závažné sítích s latencí.</span><span class="sxs-lookup"><span data-stu-id="9567b-126">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="9567b-127">Další podrobnosti o procesu najdete v článku [specifikace protokolu rozbočovače SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="9567b-127">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="9567b-128">15 sekund</span><span class="sxs-lookup"><span data-stu-id="9567b-128">15 seconds</span></span> | <span data-ttu-id="9567b-129">Pokud server není v rámci tohoto intervalu odeslal zprávu, je automaticky odeslána zpráva příkazu ping na udržení připojení otevřeného.</span><span class="sxs-lookup"><span data-stu-id="9567b-129">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="9567b-130">Při změně `KeepAliveInterval`, změnit `ServerTimeout` / `serverTimeoutInMilliseconds` nastavení na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="9567b-130">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="9567b-131">Doporučené `ServerTimeout` / `serverTimeoutInMilliseconds` hodnotu double `KeepAliveInterval` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9567b-131">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="9567b-132">Všechny nainstalované protokoly</span><span class="sxs-lookup"><span data-stu-id="9567b-132">All installed protocols</span></span> | <span data-ttu-id="9567b-133">Protokolů podporovaných toto centrum.</span><span class="sxs-lookup"><span data-stu-id="9567b-133">Protocols supported by this hub.</span></span> <span data-ttu-id="9567b-134">Ve výchozím nastavení jsou povolené všechny protokoly, které jsou registrované na serveru, ale protokolů lze odebrat z tohoto seznamu zakázat konkrétní protokoly pro jednotlivé rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9567b-134">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="9567b-135">Pokud `true`, podrobné zprávy o výjimkách se vrátí ke klientům, když dojde k výjimce v metodě rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9567b-135">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="9567b-136">Výchozí hodnota je `false`, jak tyto zprávy o výjimkách mohou obsahovat citlivé údaje.</span><span class="sxs-lookup"><span data-stu-id="9567b-136">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="9567b-137">Možnosti je možné nakonfigurovat pro všechna centra poskytnutím delegáta možnosti k `AddSignalR` volání v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9567b-137">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

<span data-ttu-id="9567b-138">Možnosti pro jedno centrum přepsat globální možnosti uvedené v `AddSignalR` a dá se nakonfigurovat pomocí [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="9567b-138">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

<span data-ttu-id="9567b-139">Použití `HttpConnectionDispatcherOptions` konfigurace upřesňujících nastavení související s přenosy a správa vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="9567b-139">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="9567b-140">Tyto možnosti jsou nakonfigurované pomocí předání delegáta, kterého [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="9567b-140">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="9567b-141">Možnost</span><span class="sxs-lookup"><span data-stu-id="9567b-141">Option</span></span> | <span data-ttu-id="9567b-142">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="9567b-142">Default Value</span></span> | <span data-ttu-id="9567b-143">Popis</span><span class="sxs-lookup"><span data-stu-id="9567b-143">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="9567b-144">32 KB.</span><span class="sxs-lookup"><span data-stu-id="9567b-144">32 KB</span></span> | <span data-ttu-id="9567b-145">Maximální počet bajtů přijatých z klienta, který vyrovnávací paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="9567b-145">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="9567b-146">Zvýšení hodnoty tuto umožňuje serveru pro příjem větší zprávy, ale může mít negativní vliv na využití paměti.</span><span class="sxs-lookup"><span data-stu-id="9567b-146">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="9567b-147">Data jsou automaticky shromážděna z `Authorize` atributy použité u třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9567b-147">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="9567b-148">Seznam [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objekty sloužící k určení, pokud je klient autorizaci k připojení k rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="9567b-148">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="9567b-149">32 KB.</span><span class="sxs-lookup"><span data-stu-id="9567b-149">32 KB</span></span> | <span data-ttu-id="9567b-150">Maximální počet bajtů odeslaných aplikace, která vyrovnávací paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="9567b-150">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="9567b-151">Zvýšení hodnoty tuto umožňuje serveru odesílat větší zprávy, ale může mít negativní vliv na využití paměti.</span><span class="sxs-lookup"><span data-stu-id="9567b-151">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="9567b-152">Všechny přenosy jsou povolené.</span><span class="sxs-lookup"><span data-stu-id="9567b-152">All Transports are enabled.</span></span> | <span data-ttu-id="9567b-153">Bitová maska z `HttpTransportType` hodnoty, které můžete omezit přenosy klienta můžete použít pro připojení.</span><span class="sxs-lookup"><span data-stu-id="9567b-153">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="9567b-154">Níže jsou uvedeny.</span><span class="sxs-lookup"><span data-stu-id="9567b-154">See below.</span></span> | <span data-ttu-id="9567b-155">Další možnosti specifické pro dlouhé dotazování přenosu.</span><span class="sxs-lookup"><span data-stu-id="9567b-155">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="9567b-156">Níže jsou uvedeny.</span><span class="sxs-lookup"><span data-stu-id="9567b-156">See below.</span></span> | <span data-ttu-id="9567b-157">Další možnosti specifické pro přenos objekty Websocket.</span><span class="sxs-lookup"><span data-stu-id="9567b-157">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="9567b-158">Dlouhý interval dotazování přenosu má další možnosti, které lze konfigurovat pomocí `LongPolling` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="9567b-158">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="9567b-159">Možnost</span><span class="sxs-lookup"><span data-stu-id="9567b-159">Option</span></span> | <span data-ttu-id="9567b-160">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="9567b-160">Default Value</span></span> | <span data-ttu-id="9567b-161">Popis</span><span class="sxs-lookup"><span data-stu-id="9567b-161">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="9567b-162">90 sekund</span><span class="sxs-lookup"><span data-stu-id="9567b-162">90 seconds</span></span> | <span data-ttu-id="9567b-163">Maximální množství času na server čeká na zprávu k odeslání do klienta před ukončením žádosti o jednotné hlasování.</span><span class="sxs-lookup"><span data-stu-id="9567b-163">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="9567b-164">Snížení hodnoty způsobí, že klient k vydávání nových žádostí o dotazování častěji.</span><span class="sxs-lookup"><span data-stu-id="9567b-164">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="9567b-165">Přenos pomocí protokolu WebSocket má další možnosti, které lze konfigurovat pomocí `WebSockets` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="9567b-165">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="9567b-166">Možnost</span><span class="sxs-lookup"><span data-stu-id="9567b-166">Option</span></span> | <span data-ttu-id="9567b-167">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="9567b-167">Default Value</span></span> | <span data-ttu-id="9567b-168">Popis</span><span class="sxs-lookup"><span data-stu-id="9567b-168">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="9567b-169">5 sekund</span><span class="sxs-lookup"><span data-stu-id="9567b-169">5 seconds</span></span> | <span data-ttu-id="9567b-170">Po zavření serveru, pokud se klientovi nepodaří zavřete v tomto časovém intervalu, připojení se ukončí.</span><span class="sxs-lookup"><span data-stu-id="9567b-170">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="9567b-171">Delegát, který je možné nastavit `Sec-WebSocket-Protocol` vlastní hodnoty záhlaví.</span><span class="sxs-lookup"><span data-stu-id="9567b-171">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="9567b-172">Delegát přijme hodnoty vyžádané klientem jako vstup a měl by vrátit na požadovanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9567b-172">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="9567b-173">Konfigurovat možnosti klienta</span><span class="sxs-lookup"><span data-stu-id="9567b-173">Configure client options</span></span>

<span data-ttu-id="9567b-174">Možnosti klienta lze nakonfigurovat podle `HubConnectionBuilder` typ (k dispozici v rozhraní .NET a JavaScript klientech), stejně jako na `HubConnection` samotný.</span><span class="sxs-lookup"><span data-stu-id="9567b-174">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="9567b-175">Konfigurace protokolování</span><span class="sxs-lookup"><span data-stu-id="9567b-175">Configure logging</span></span>

<span data-ttu-id="9567b-176">Protokolování je nakonfigurován v klientu .NET pomocí `ConfigureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="9567b-176">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="9567b-177">Protokolování poskytovatelů a filtry můžete zaregistrovat stejně, jako jsou na serveru.</span><span class="sxs-lookup"><span data-stu-id="9567b-177">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="9567b-178">Zobrazit [protokolování v ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) Další informace naleznete v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="9567b-178">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="9567b-179">Aby bylo možné zaregistrovat poskytovatele protokolování, je nutné nainstalovat potřebné balíčky.</span><span class="sxs-lookup"><span data-stu-id="9567b-179">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="9567b-180">Najdete v článku [vestavěné protokolování poskytovatelé](xref:fundamentals/logging/index#built-in-logging-providers) část dokumentace pro úplný seznam.</span><span class="sxs-lookup"><span data-stu-id="9567b-180">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="9567b-181">Například pokud chcete povolit protokolování konzoly, nainstalujte `Microsoft.Extensions.Logging.Console` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="9567b-181">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="9567b-182">Volání `AddConsole` – metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="9567b-182">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="9567b-183">V klientovi JavaScript podobný `configureLogging` metody existuje.</span><span class="sxs-lookup"><span data-stu-id="9567b-183">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="9567b-184">Zadejte `LogLevel` hodnotu, která minimální úroveň zprávy protokolu k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="9567b-184">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="9567b-185">Protokoly se zapisují do okna konzoly prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9567b-185">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="9567b-186">Chcete-li zakázat protokolování úplně, zadejte `signalR.LogLevel.None` v `configureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="9567b-186">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="9567b-187">K dispozici ke klientovi JavaScript úrovně protokolu jsou uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="9567b-187">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="9567b-188">Protokolování zpráv při nastavení na úrovni protokolu na jednu z těchto hodnot umožňuje **nebo vyšší** dané úrovni.</span><span class="sxs-lookup"><span data-stu-id="9567b-188">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="9567b-189">úroveň</span><span class="sxs-lookup"><span data-stu-id="9567b-189">Level</span></span> | <span data-ttu-id="9567b-190">Popis</span><span class="sxs-lookup"><span data-stu-id="9567b-190">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="9567b-191">Jsou zaznamenány žádné zprávy.</span><span class="sxs-lookup"><span data-stu-id="9567b-191">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="9567b-192">Zprávy, které indikují chybu celé aplikace.</span><span class="sxs-lookup"><span data-stu-id="9567b-192">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="9567b-193">Zprávy, které indikují chybu aktuální operaci.</span><span class="sxs-lookup"><span data-stu-id="9567b-193">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="9567b-194">Zprávy, které označují méně závažné potíže.</span><span class="sxs-lookup"><span data-stu-id="9567b-194">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="9567b-195">Informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="9567b-195">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="9567b-196">Diagnostické zprávy je užitečné pro ladění.</span><span class="sxs-lookup"><span data-stu-id="9567b-196">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="9567b-197">Velmi podrobné diagnostické zprávy, které jsou navržené pro diagnostiku specifické problémy.</span><span class="sxs-lookup"><span data-stu-id="9567b-197">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="9567b-198">Nakonfigurovat povolené přenosy</span><span class="sxs-lookup"><span data-stu-id="9567b-198">Configure allowed transports</span></span>

<span data-ttu-id="9567b-199">Přenosy systém signalr se dá nakonfigurovat v `WithUrl` volání (`withUrl` v JavaScriptu).</span><span class="sxs-lookup"><span data-stu-id="9567b-199">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="9567b-200">Bitový OR hodnoty `HttpTransportType` slouží k omezení klienta používat pouze zadaný přenosy.</span><span class="sxs-lookup"><span data-stu-id="9567b-200">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="9567b-201">Ve výchozím nastavení jsou povolené všechny přenosy.</span><span class="sxs-lookup"><span data-stu-id="9567b-201">All transports are enabled by default.</span></span>

<span data-ttu-id="9567b-202">Například zakázat přenos Server-Sent události, ale povolte připojení objekty Websocket a dlouhý interval dotazování:</span><span class="sxs-lookup"><span data-stu-id="9567b-202">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="9567b-203">V klientovi JavaScript přenosy jsou nakonfigurovány tak, že nastavíte `transport` na objekt možnosti poskytnuté `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="9567b-203">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="9567b-204">Konfigurace ověřování nosiče</span><span class="sxs-lookup"><span data-stu-id="9567b-204">Configure bearer authentication</span></span>

<span data-ttu-id="9567b-205">Pokud chcete poskytnout ověřovací data spolu s knihovnou SignalR požadavky, použijte `AccessTokenProvider` možnost (`accessTokenFactory` v JavaScriptu) k určení funkce, která vrací požadovaný přístupový token.</span><span class="sxs-lookup"><span data-stu-id="9567b-205">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="9567b-206">V klientovi .NET Tento přístupový token předaný jako HTTP "Ověřování nosiče" token (použití `Authorization` záhlaví s typem `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="9567b-206">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="9567b-207">V klientovi JavaScript přístupový token se používá jako nosný token **s výjimkou** v několika případech, kdy prohlížeč rozhraní API omezit schopnosti uplatňovat hlavičky (konkrétně v Server-Sent události a protokoly Websocket požadavky).</span><span class="sxs-lookup"><span data-stu-id="9567b-207">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="9567b-208">V těchto případech je přístupový token k dispozici jako hodnotu řetězce dotazu `access_token`.</span><span class="sxs-lookup"><span data-stu-id="9567b-208">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="9567b-209">V klientovi .NET `AccessTokenProvider` možnost lze zadat pomocí možnosti delegáta v `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="9567b-209">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="9567b-210">V klientovi JavaScript přístupový token je nakonfigurovaný tak, že nastavíte `accessTokenFactory` na objekt možnosti v `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="9567b-210">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="9567b-211">Konfigurace časového limitu a udržování možnosti</span><span class="sxs-lookup"><span data-stu-id="9567b-211">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="9567b-212">Další možnosti pro konfiguraci časového limitu a zachování chování, které jsou k dispozici na `HubConnection` samotného objektu:</span><span class="sxs-lookup"><span data-stu-id="9567b-212">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="9567b-213">.NET – možnost</span><span class="sxs-lookup"><span data-stu-id="9567b-213">.NET Option</span></span> | <span data-ttu-id="9567b-214">Možnost jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="9567b-214">JavaScript Option</span></span> | <span data-ttu-id="9567b-215">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="9567b-215">Default Value</span></span> | <span data-ttu-id="9567b-216">Popis</span><span class="sxs-lookup"><span data-stu-id="9567b-216">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="9567b-217">30 sekund (30 000 milisekund)</span><span class="sxs-lookup"><span data-stu-id="9567b-217">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="9567b-218">Časový limit pro aktivity serveru.</span><span class="sxs-lookup"><span data-stu-id="9567b-218">Timeout for server activity.</span></span> <span data-ttu-id="9567b-219">Pokud server není v tomto intervalu odeslal zprávu, klient bude považovat za server odpojen a aktivační události `Closed` událostí (`onclose` v JavaScriptu).</span><span class="sxs-lookup"><span data-stu-id="9567b-219">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="9567b-220">Tato hodnota musí být dostatečně velký pro zprávu ping na odeslaných ze serveru **a** klient obdrží během intervalu časového limitu.</span><span class="sxs-lookup"><span data-stu-id="9567b-220">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="9567b-221">Doporučená hodnota je čísla alespoň double server `KeepAliveInterval` hodnotu, abyste měli čas pro příkazy ping pro zjištění dorazí.</span><span class="sxs-lookup"><span data-stu-id="9567b-221">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="9567b-222">Nejde konfigurovat</span><span class="sxs-lookup"><span data-stu-id="9567b-222">Not configurable</span></span> | <span data-ttu-id="9567b-223">15 sekund</span><span class="sxs-lookup"><span data-stu-id="9567b-223">15 seconds</span></span> | <span data-ttu-id="9567b-224">Časový limit pro počáteční server handshake.</span><span class="sxs-lookup"><span data-stu-id="9567b-224">Timeout for initial server handshake.</span></span> <span data-ttu-id="9567b-225">Pokud server není v tomto intervalu odeslání odpovědi handshake, klient zruší handshake a aktivační události `Closed` událostí (`onclose` v JavaScriptu).</span><span class="sxs-lookup"><span data-stu-id="9567b-225">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="9567b-226">Toto je upřesňující nastavení, by měla být změněna pouze v případě chyby časového limitu handshake dochází z důvodu závažné sítích s latencí.</span><span class="sxs-lookup"><span data-stu-id="9567b-226">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="9567b-227">Další podrobnosti o procesu najdete v článku [specifikace protokolu rozbočovače SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="9567b-227">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="9567b-228">V klientovi .NET jsou zadané hodnoty časového limitu jako `TimeSpan` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9567b-228">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="9567b-229">V klientovi JavaScript jsou zadané hodnoty časového limitu jako číslo určující dobu trvání v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="9567b-229">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="9567b-230">Konfigurace dalších možností</span><span class="sxs-lookup"><span data-stu-id="9567b-230">Configure additional options</span></span>

<span data-ttu-id="9567b-231">Další možnosti se dá nakonfigurovat v `WithUrl` (`withUrl` v JavaScriptu) metoda `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9567b-231">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="9567b-232">.NET – možnost</span><span class="sxs-lookup"><span data-stu-id="9567b-232">.NET Option</span></span> | <span data-ttu-id="9567b-233">Možnost jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="9567b-233">JavaScript Option</span></span> | <span data-ttu-id="9567b-234">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="9567b-234">Default Value</span></span> | <span data-ttu-id="9567b-235">Popis</span><span class="sxs-lookup"><span data-stu-id="9567b-235">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="9567b-236">Funkce vrátí řetězec, který je k dispozici jako token nosiče ověřování v požadavcích HTTP.</span><span class="sxs-lookup"><span data-stu-id="9567b-236">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="9567b-237">Nastavte na `true` přeskočit krok vyjednávání.</span><span class="sxs-lookup"><span data-stu-id="9567b-237">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="9567b-238">**Podporuje jenom při přenosu objekty Websocket je jediný povolený přenos**.</span><span class="sxs-lookup"><span data-stu-id="9567b-238">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="9567b-239">Toto nastavení není možné při použití služby Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="9567b-239">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="9567b-240">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="9567b-240">Not configurable \*</span></span> | <span data-ttu-id="9567b-241">prázdný</span><span class="sxs-lookup"><span data-stu-id="9567b-241">Empty</span></span> | <span data-ttu-id="9567b-242">Kolekce certifikáty TLS odeslat k ověření požadavků.</span><span class="sxs-lookup"><span data-stu-id="9567b-242">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="9567b-243">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="9567b-243">Not configurable \*</span></span> | <span data-ttu-id="9567b-244">prázdný</span><span class="sxs-lookup"><span data-stu-id="9567b-244">Empty</span></span> | <span data-ttu-id="9567b-245">Kolekce souborů cookie protokolu HTTP k odeslání při každé žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="9567b-245">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="9567b-246">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="9567b-246">Not configurable \*</span></span> | <span data-ttu-id="9567b-247">prázdný</span><span class="sxs-lookup"><span data-stu-id="9567b-247">Empty</span></span> | <span data-ttu-id="9567b-248">Přihlašovací údaje pro odesílání při každé žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="9567b-248">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="9567b-249">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="9567b-249">Not configurable \*</span></span> | <span data-ttu-id="9567b-250">5 sekund</span><span class="sxs-lookup"><span data-stu-id="9567b-250">5 seconds</span></span> | <span data-ttu-id="9567b-251">Pouze objekty Websocket.</span><span class="sxs-lookup"><span data-stu-id="9567b-251">WebSockets only.</span></span> <span data-ttu-id="9567b-252">Maximální množství času, klient počká po uzavření pro server a potvrďte žádosti o uzavření.</span><span class="sxs-lookup"><span data-stu-id="9567b-252">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="9567b-253">Pokud server není v tuto chvíli Potvrdit uzavření, klient neodpojí.</span><span class="sxs-lookup"><span data-stu-id="9567b-253">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="9567b-254">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="9567b-254">Not configurable \*</span></span> | <span data-ttu-id="9567b-255">prázdný</span><span class="sxs-lookup"><span data-stu-id="9567b-255">Empty</span></span> | <span data-ttu-id="9567b-256">Slovník další hlavičky protokolu HTTP k odeslání při každé žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="9567b-256">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="9567b-257">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="9567b-257">Not configurable \*</span></span> | `null` | <span data-ttu-id="9567b-258">Delegát, který lze použít ke konfiguraci nebo nahradit `HttpMessageHandler` používaný k odesílání požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="9567b-258">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="9567b-259">Není možné použít u připojení pomocí protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9567b-259">Not used for WebSocket connections.</span></span> <span data-ttu-id="9567b-260">Tento delegát musí vrátit nenulovou hodnotu, a přijímá jako parametr výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9567b-260">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="9567b-261">Upravte nastavení podle této výchozí hodnoty a vrácení nebo vrátí novou `HttpMessageHandler` instance.</span><span class="sxs-lookup"><span data-stu-id="9567b-261">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="9567b-262">**Při nahrazování obslužnou rutinu nezapomeňte si zkopírovat na nastavení, která chcete zachovat od zadané obslužné rutiny, v opačném případě nakonfigurovaných možností (například soubory cookie a hlavičky) se nevztahuje na novou obslužnou rutinu.**</span><span class="sxs-lookup"><span data-stu-id="9567b-262">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="9567b-263">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="9567b-263">Not configurable \*</span></span> | `null` | <span data-ttu-id="9567b-264">Proxy server HTTP při odesílání požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="9567b-264">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="9567b-265">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="9567b-265">Not configurable \*</span></span> | `false` | <span data-ttu-id="9567b-266">Nastavte tuto logickou hodnotu k odeslání výchozí přihlašovací údaje pro požadavky HTTP a Websocket.</span><span class="sxs-lookup"><span data-stu-id="9567b-266">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="9567b-267">To umožňuje použití ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="9567b-267">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="9567b-268">Nejde konfigurovat \*</span><span class="sxs-lookup"><span data-stu-id="9567b-268">Not configurable \*</span></span> | `null` | <span data-ttu-id="9567b-269">Delegát, který slouží k nakonfigurování dalších možností protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9567b-269">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="9567b-270">Obdrží instanci [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , který lze použít ke konfiguraci možností.</span><span class="sxs-lookup"><span data-stu-id="9567b-270">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="9567b-271">Možnosti s hvězdičkou (\*) nejsou konfigurovatelné v klientovi JavaScript z důvodu omezení v prohlížeči rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9567b-271">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="9567b-272">V klientovi .NET tyto možnosti je možné upravovat prostřednictvím delegáta možnosti poskytnuté `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="9567b-272">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="9567b-273">V klientovi JavaScript tyto možnosti lze zadat v objektu jazyka JavaScript poskytuje k `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="9567b-273">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="9567b-274">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9567b-274">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
