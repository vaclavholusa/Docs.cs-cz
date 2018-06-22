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
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Použít protokol MessagePack centra v systému SignalR pro ASP.NET Core

Podle [Brennan Conroy](https://github.com/BrennanConroy)

Tento článek předpokládá, že čtečka je seznámit s tématy v [Začínáme](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>Co je MessagePack?

[MessagePack](https://msgpack.org/index.html) je binární serializace formátu, který je rychlý a compact. Je užitečná při výkon a šířku pásma jsou důležité, protože vytvoří menší zprávy ve srovnání s [JSON](https://www.json.org/). Protože je binární formát, zprávy jsou při prohlížení trasování sítě a protokoly, pokud počet bajtů se předaly analyzátor MessagePack nečitelná. SignalR má integrovanou podporu pro formát MessagePack a poskytuje rozhraní API pro klienty a server používat.

## <a name="configure-messagepack-on-the-server"></a>Konfigurace MessagePack na serveru

Chcete-li protokol MessagePack rozbočovače na serveru, nainstalujte `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` balíček ve vaší aplikaci. V souboru Startup.cs přidat `AddMessagePackProtocol` k `AddSignalR` volání povolení podpory MessagePack na serveru.

> [!NOTE]
> JSON je ve výchozím nastavení povolené. Přidání MessagePack umožňuje podporu pro JSON a MessagePack klienty.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Chcete-li přizpůsobit, jak bude MessagePack formátování dat, `AddMessagePackProtocol` trvá delegáta pro možnosti konfigurace. V tento delegát `FormatterResolvers` vlastnost lze použít ke konfiguraci možností MessagePack serializace. Další informace o fungování překladače, navštivte knihovně MessagePack na [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp). Atributy lze použít u objektů, které chcete serializovat definovat, jak má být zpracováno.

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

## <a name="configure-messagepack-on-the-client"></a>Konfigurace MessagePack na straně klienta

### <a name="net-client"></a>Klient .NET

Chcete-li MessagePack v klientovi .NET, nainstalujte `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` balíčku a volání `AddMessagePackProtocol` na `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> To `AddMessagePackProtocol` trvá volání delegáta pro konfiguraci možností stejně jako server.

### <a name="javascript-client"></a>JavaScript klienta

Zajišťuje podporu MessagePack pro Javascript klienta `@aspnet/signalr-protocol-msgpack` balíčku NPM.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Po instalaci balíčku npm, modul použít přímo prostřednictvím načítání modulu JavaScript nebo importovat do prohlížeče odkazem *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  souboru. V prohlížeči `msgpack5` knihovny musí také odkazovat. Použití `<script>` značky k vytvoření odkazu. Knihovny naleznete na adrese *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Při použití `<script>` elementu pořadí je důležité. Pokud *signalr-protocol-msgpack.js* odkazuje před *msgpack5.js*, dojde k chybě při pokusu o připojení s MessagePack. *signalr.js* také je vyžadována před *signalr-protocol-msgpack.js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Přidání `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` k `HubConnectionBuilder` se konfigurace klienta pomocí protokolu MessagePack při připojování k serveru.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> V současné době neexistují žádné možnosti konfigurace pro protokol MessagePack na straně klienta JavaScript.

## <a name="related-resources"></a>Související informační zdroje

* [Začínáme](xref:tutorials/signalr)
* [Klient .NET](xref:signalr/dotnet-client)
* [Klient JavaScriptu](xref:signalr/javascript-client)
