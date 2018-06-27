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
# <a name="aspnet-core-signalr-configuration"></a>Konfigurace ASP.NET Core SignalR

## <a name="jsonmessagepack-serialization-options"></a>Serializace JSON nebo MessagePack možnosti

Funkce SignalR technologie ASP.NET Core podporuje dva protokoly pro kódování zpráv: [JSON](https://www.json.org/) a [MessagePack](https://msgpack.org/index.html). Každý protokol obsahuje možnosti konfigurace serializace.

Serializace JSON se dá konfigurovat na serveru pomocí [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metoda rozšíření, které lze přidat po [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) v vaší `Startup.ConfigureServices` metoda. `AddJsonProtocol` Metoda přebírá delegáta, který obdrží `options` objektu. [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) Vlastnost na tento objekt je JSON.NET `JsonSerializerSettings` objekt, který můžete použít ke konfiguraci serializace argumentů a návratové hodnoty. Najdete v článku [JSON.NET dokumentace](https://www.newtonsoft.com/json/help/html/Introduction.htm) další podrobnosti.

Jako příklad ke konfiguraci serializátoru, který je pro použití názvy vlastností "PascalCase" místo "camelCase" výchozí názvy, použijte následující kód:

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

V klientovi .NET stejné `AddJsonHubProtocol` metoda rozšíření existuje v [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). `Microsoft.Extensions.DependencyInjection` Oboru názvů musí být importovány na resolve – metoda rozšíření:

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
> Není možné nakonfigurovat serializace JSON v JavaScript klienta v tuto chvíli.

### <a name="messagepack-serialization-options"></a>Možnosti serializace MessagePack

Tím, že poskytuje delegáta, kterého lze nakonfigurovat MessagePack serializace [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) volání. V tématu [MessagePack v systému SignalR](xref:signalr/messagepackhubprotocol) další podrobnosti.

> [!NOTE]
> Není možné nakonfigurovat MessagePack serializace v JavaScript klienta v tuto chvíli.

## <a name="configure-server-options"></a>Konfigurovat možnosti serveru

Následující tabulka popisuje možnosti pro konfiguraci rozbočovače SignalR:

| Možnost | Popis |
| ------ | ----------- |
| `HandshakeTimeout` | Pokud klient není v rámci tohoto intervalu odešle zprávu počáteční handshake, je připojení ukončeno. |
| `KeepAliveInterval` | Pokud server neodeslal zprávu v rámci tento interval, je automaticky odeslána zpráva ping nechat otevřené připojení. |
| `SupportedProtocols` | Protokolů podporovaných toto centrum. Ve výchozím nastavení jsou povolené všechny protokoly, které jsou registrované na serveru, ale protokoly můžete odebrat z tohoto seznamu zakázat konkrétními protokoly pro jednotlivé rozbočovače. |
| `EnableDetailedErrors` | Pokud `true`, podrobné zprávy o výjimkách se vrátíte na klienty, pokud je vyvolána výjimka v metodě rozbočovače. Výchozí hodnota je `false`, jak tyto zprávy o výjimkách mohou obsahovat citlivé údaje. |

Možnosti lze konfigurovat pro všechny rozbočovače tím, že poskytuje delegáta možnosti na `AddSignalR` volání v `Startup.ConfigureServices`.

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

Možnosti pro jednoho rozbočovače přepsat globální možností v `AddSignalR` a dá se nakonfigurovat pomocí [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

Použití `HttpConnectionDispatcherOptions` nakonfigurovat upřesňující nastavení související s přenosy a správa vyrovnávací paměti. Tyto možnosti jsou nakonfigurované pomocí předání delegáta, kterého [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Možnost | Popis |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | Maximální počet bajtů přijatých z klienta, serveru vyrovnávací paměti. Zvýšení hodnoty tuto umožňuje serveru přijímat větší zprávy, ale může mít negativní vliv na využití paměti. Výchozí hodnota je 32KB. |
| `AuthorizationData` | Seznam [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objekty používané k určení, pokud je klient autorizaci k připojení k rozbočovači. Ve výchozím nastavení, to je naplněnou hodnotami `Authorize` atributy použité u třídy rozbočovače. |
| `TransportMaxBufferSize` | Maximální počet bajtů odeslaných aplikací, vyrovnávací paměti serveru. Zvýšení hodnoty tuto umožňuje serveru odesílat větší zprávy, ale může mít negativní vliv na využití paměti. Výchozí hodnota je 32KB. |
| `Transports` | Bitová maska s `HttpTransportType` hodnoty, které můžete omezit přenosy klienta můžete použít pro připojení. Ve výchozím nastavení jsou povolené všechny přenosy. |
| `LongPolling` | Další možnosti specifické pro dlouhé dotazování přenosu. |
| `WebSockets` | Další možnosti specifické pro objekty WebSockets přenosu. |

Dlouhé dotazování přenosu má další možnosti, které lze nakonfigurovat pomocí `LongPolling` vlastnost:

| Možnost | Popis |
| ------ | ----------- |
| `PollTimeout` | Maximální množství času server čeká na zprávu k odeslání do klienta před ukončením žádost o jeden dotazování. Snížení hodnoty způsobí, že klient k vydání nové žádosti o dotazování častěji. Výchozí hodnota je 90 sekund. |

Přenos protokolu WebSocket obsahuje další možnosti, které lze nakonfigurovat pomocí `WebSockets` vlastnost:

| Možnost | Popis |
| ------ | ----------- |
| `CloseTimeout` | Po zavření serveru, pokud se klientovi nepodaří zavřete v rámci tohoto intervalu, připojení bylo ukončeno. |
| `SubProtocolSelector` | Delegát, který můžete použít k nastavení `Sec-WebSocket-Protocol` na hodnotu vlastní hlavičky. Delegát přijímá hodnoty požadavku klienta jako vstup a zpět požadovanou hodnotu. |

## <a name="configure-client-options"></a>Konfigurace možností klienta

Možnosti klienta lze nakonfigurovat podle `HubConnectionBuilder` typu (k dispozici v rozhraní .NET a JavaScript klientů), stejně jako na `HubConnection` sám sebe.

### <a name="configure-logging"></a>Konfigurace protokolování

Protokolování je nastaveno v rozhraní .NET klienta pomocí `ConfigureLogging` metoda. Protokolování zprostředkovatelé a filtry lze registrovat stejným způsobem, jako jsou na serveru. Najdete v článku [protokolování v ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) Další informace naleznete v dokumentaci.

> [!NOTE]
> Chcete-li zaregistrovat zprostředkovatelé protokolování, je nutné nainstalovat nezbytných balíčků. Najdete v článku [integrované protokolování zprostředkovatelé](xref:fundamentals/logging/index#built-in-logging-providers) části dokumentace pro úplný seznam.

Například konzole protokolování povolit, nainstalujte `Microsoft.Extensions.Logging.Console` balíček NuGet. Volání `AddConsole` metoda rozšíření:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

V klientovi JavaScript podobné `configureLogging` metoda existuje. Zadejte `LogLevel` hodnotu, která určuje minimální úroveň protokolu zprávy a pokuste se vytvořit. Protokoly se zapisují do okna konzoly prohlížeče.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Pokud chcete zakázat protokolování zcela, zadejte `signalR.LogLevel.None` v `configureLogging` metoda.

Úrovně protokolu dostupných pro klienta JavaScript jsou uvedeny níže. Nastavení úrovně protokolu na jednu z těchto hodnot umožňuje protokolování zpráv v **nebo vyšší** této úrovni.

| Úroveň | Popis |
| ----- | ----------- |
| `None` | Jsou protokolovány žádné zprávy. |
| `Critical` | Zprávy, které indikují chybu v celé aplikaci. |
| `Error` | Zprávy, které indikují chybu v aktuální operace. |
| `Warning` | Zprávy, které indikují nezávažné problém. |
| `Information` | Informační zprávy. |
| `Debug` | Diagnostické zprávy pro ladění. |
| `Trace` | Velmi podrobné diagnostické zprávy určené pro diagnostikování konkrétní problémy. |

### <a name="configure-allowed-transports"></a>Konfigurace povolené přenosy

Přenosy systém signalr se dá nakonfigurovat v `WithUrl` volání (`withUrl` v jazyce JavaScript). Bitové operace OR hodnoty `HttpTransportType` slouží k omezení klientovi použít pouze zadané přenosy. Ve výchozím nastavení jsou povolené všechny přenosy.

Například zakázat přenos Server-Sent události, ale povolte objekty WebSockets a dlouhé dotazování připojení:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

V klientovi JavaScript, jsou přenosy nakonfigurované nastavením `transport` na zadaný pro objekt možnosti `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>Konfigurace ověřování nosiče

Pokud chcete zadat data ověřování společně s požadavky SignalR, použijte `AccessTokenProvider` možnost (`accessTokenFactory` v jazyce JavaScript) k určení funkci, která vrátí požadované přístupový token. V rozhraní .NET klienta, je předaná tento token přístupu jako HTTP "Ověřování nosiče" tokenu (pomocí `Authorization` záhlaví s typem `Bearer`). V klientovi JavaScript přístupový token slouží jako token nosiče **s výjimkou** v určitých případech, kdy prohlížeč rozhraní API omezit schopnosti uplatňovat hlavičky (konkrétně v Server-Sent události a WebSockets žádostí). V těchto případech je přístupový token zadaný jako hodnota řetězce dotazu `access_token`.

V klientovi .NET `AccessTokenProvider` možnost lze zadat pomocí možnosti delegáta v `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

V klientovi JavaScript přístupový token nakonfiguroval nastavení `accessTokenFactory` pole možnosti objektu ve `withUrl`:

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

### <a name="configure-timeout-and-keep-alive-options"></a>Nakonfigurujte časový limit a udržování možnosti

Další možnosti pro konfiguraci časového limitu a udržování chování jsou k dispozici na `HubConnection` samotného objektu:

| Možnost rozhraní .NET | JavaScript – možnost | Popis |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | Časový limit aktivity serveru. Pokud server není v tomto intervalu odeslal jakékoli zprávy, klient považuje server odpojí a aktivační události `Closed` událostí (`onclose` v jazyce JavaScript). |
| `HandshakeTimeout` | Nejde konfigurovat | Časový limit pro počáteční server handshake. Pokud server není v tomto intervalu odeslání odpovědi handshake, klient zruší metody handshake a aktivační události `Closed` událostí (`onclose` v jazyce JavaScript). |

V klientovi .NET, časový limit hodnoty jsou specifikované jako `TimeSpan` hodnoty. V klientovi JavaScript jsou hodnoty časového limitu zadané jako čísla. Čísla, která představují hodnoty času v milisekundách.

### <a name="configure-additional-options"></a>Konfigurace dalších možností

Další možnosti se dá nakonfigurovat v `WithUrl` (`withUrl` v jazyce JavaScript) metodu `HubConnectionBuilder`:

| Možnost rozhraní .NET | JavaScript – možnost | Popis |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | Funkce vrátí řetězec, který je k dispozici jako token nosiče ověřování v požadavky HTTP. |
| `SkipNegotiation` | `skipNegotiation` | Tuto možnost nastavíte na `true` k vyjednávání krok přeskočit. **Podporována pouze v případě technologie WebSockets přenosu je pouze povolené přenos**. Toto nastavení nelze povolit, pokud používáte službu Azure SignalR. |
| `ClientCertificates` | Nejde konfigurovat * | Kolekce odeslat k ověřování žádostí o certifikáty protokolu TLS. |
| `Cookies` | Nejde konfigurovat * | Kolekce souborů cookie HTTP k odeslání s každou žádostí HTTP. |
| `Credentials` | Nejde konfigurovat * | Přihlašovací údaje odeslat spolu s každou žádost HTTP. |
| `CloseTimeout` | Nejde konfigurovat * | Pouze objekty WebSockets. Maximální množství času klient počká po zavření pro server, aby vzali na vědomí žádosti o uzavření. Pokud server není v tuto chvíli vědomí ukončení, se klient neodpojí. |
| `Headers` | Nejde konfigurovat * | Slovník další hlavičky protokolu HTTP k odeslání s každou žádostí HTTP. |
| `HttpMessageHandlerFactory` | Nejde konfigurovat * | Delegát, který lze nakonfigurovat, nebo nahrazení `HttpMessageHandler` používá k odesílání požadavků HTTP. Nepoužívá se pro připojení protokolu WebSocket. Tento delegát musí vracet hodnotu než null a obdrží výchozí hodnota jako parametr. Buď změňte nastavení na této výchozí hodnotu a vrátit nebo vrátit úplně nové `HttpMessageHandler` instance. |
| `Proxy` | Nejde konfigurovat * | Proxy server HTTP pro použití při odesílání požadavků HTTP. |
| `UseDefaultCredentials` | Nejde konfigurovat * | Nastavte tuto logickou hodnotu k odeslání výchozí pověření pro požadavky HTTP a objekty WebSockets. To umožňuje použití ověřování systému Windows. |
| `WebSocketConfiguration` | Nejde konfigurovat * | Delegát, který můžete použít ke konfiguraci dalších možností protokolu WebSocket. Obdrží instanci [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , můžete použít ke konfiguraci možností. |

Možnosti, které jsou označené hvězdičkou (*) nejsou konfigurovatelné v klientovi JavaScript z důvodu omezení v prohlížeči rozhraní API.

V rozhraní .NET klientovi tyto možnosti mohou být upravena delegáta možnosti poskytované `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

V klientovi JavaScript, tyto možnosti lze zadat do objekt jazyka JavaScript poskytované `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a>Další zdroje

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
