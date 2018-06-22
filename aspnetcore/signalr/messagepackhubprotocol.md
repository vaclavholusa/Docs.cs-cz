---
title: Použít protokol MessagePack centra v systému SignalR pro ASP.NET Core
author: rachelappel
description: Přidáte protokol MessagePack rozbočovače SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 702c77502868d6666cb2634b6959f029e036d14e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274986"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="6e0a2-103">Použít protokol MessagePack centra v systému SignalR pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e0a2-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="6e0a2-104">Podle [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="6e0a2-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="6e0a2-105">Tento článek předpokládá, že čtečka je seznámit s tématy v [Začínáme](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="6e0a2-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="6e0a2-106">Co je MessagePack?</span><span class="sxs-lookup"><span data-stu-id="6e0a2-106">What is MessagePack?</span></span>

<span data-ttu-id="6e0a2-107">[MessagePack](https://msgpack.org/index.html) je binární serializace formátu, který je rychlý a compact.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="6e0a2-108">Je užitečná při výkon a šířku pásma jsou důležité, protože vytvoří menší zprávy ve srovnání s [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="6e0a2-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="6e0a2-109">Protože je binární formát, zprávy jsou při prohlížení trasování sítě a protokoly, pokud počet bajtů se předaly analyzátor MessagePack nečitelná.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="6e0a2-110">SignalR má integrovanou podporu pro formát MessagePack a poskytuje rozhraní API pro klienty a server používat.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="6e0a2-111">Konfigurace MessagePack na serveru</span><span class="sxs-lookup"><span data-stu-id="6e0a2-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="6e0a2-112">Chcete-li protokol MessagePack rozbočovače na serveru, nainstalujte `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` balíček ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="6e0a2-113">V souboru Startup.cs přidat `AddMessagePackProtocol` k `AddSignalR` volání povolení podpory MessagePack na serveru.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="6e0a2-114">JSON je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-114">JSON is enabled by default.</span></span> <span data-ttu-id="6e0a2-115">Přidání MessagePack umožňuje podporu pro JSON a MessagePack klienty.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="6e0a2-116">Chcete-li přizpůsobit, jak bude MessagePack formátování dat, `AddMessagePackProtocol` trvá delegáta pro možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="6e0a2-117">V tento delegát `FormatterResolvers` vlastnost lze použít ke konfiguraci možností MessagePack serializace.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="6e0a2-118">Další informace o fungování překladače, navštivte knihovně MessagePack na [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="6e0a2-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="6e0a2-119">Atributy lze použít u objektů, které chcete serializovat definovat, jak má být zpracováno.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="6e0a2-120">Konfigurace MessagePack na straně klienta</span><span class="sxs-lookup"><span data-stu-id="6e0a2-120">Configure MessagePack on the client</span></span>

### <a name="net-client"></a><span data-ttu-id="6e0a2-121">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="6e0a2-121">.NET client</span></span>

<span data-ttu-id="6e0a2-122">Chcete-li MessagePack v klientovi .NET, nainstalujte `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` balíčku a volání `AddMessagePackProtocol` na `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-122">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="6e0a2-123">To `AddMessagePackProtocol` trvá volání delegáta pro konfiguraci možností stejně jako server.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-123">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="6e0a2-124">JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="6e0a2-124">JavaScript client</span></span>

<span data-ttu-id="6e0a2-125">Zajišťuje podporu MessagePack pro Javascript klienta `@aspnet/signalr-protocol-msgpack` balíčku NPM.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-125">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="6e0a2-126">Po instalaci balíčku npm, modul použít přímo prostřednictvím načítání modulu JavaScript nebo importovat do prohlížeče odkazem *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  souboru.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-126">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="6e0a2-127">V prohlížeči `msgpack5` knihovny musí také odkazovat.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-127">In a browser the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="6e0a2-128">Použití `<script>` značky k vytvoření odkazu.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-128">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="6e0a2-129">Knihovny naleznete na adrese *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-129">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="6e0a2-130">Při použití `<script>` elementu pořadí je důležité.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-130">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="6e0a2-131">Pokud *signalr-protocol-msgpack.js* odkazuje před *msgpack5.js*, dojde k chybě při pokusu o připojení s MessagePack.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-131">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="6e0a2-132">*signalr.js* také je vyžadována před *signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-132">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="6e0a2-133">Přidání `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` k `HubConnectionBuilder` se konfigurace klienta pomocí protokolu MessagePack při připojování k serveru.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-133">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="6e0a2-134">V současné době neexistují žádné možnosti konfigurace pro protokol MessagePack na straně klienta JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6e0a2-134">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="6e0a2-135">Související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="6e0a2-135">Related resources</span></span>

* [<span data-ttu-id="6e0a2-136">Začínáme</span><span class="sxs-lookup"><span data-stu-id="6e0a2-136">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="6e0a2-137">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="6e0a2-137">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="6e0a2-138">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="6e0a2-138">JavaScript client</span></span>](xref:signalr/javascript-client)
