---
title: SignalR HubContext
author: tdykstra
description: Zjistěte, jak použít službu SignalR HubContext technologie ASP.NET Core pro odesílání oznámení klientům mimo rozbočovač.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: 8be888e1f7b16d65ebbaa24b618e84fca029d80b
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207950"
---
# <a name="send-messages-from-outside-a-hub"></a>Odeslání zprávy z mimo rozbočovač

Podle [Mikael Mengistu](https://twitter.com/MikaelM_12)

Rozbočovače SignalR je základní abstrakci pro odesílání zpráv do klientů připojených k serveru funkce SignalR. Je také možné odesílat zprávy z jiného místa portálu vaši aplikaci s použitím `IHubContext` služby. Tento článek vysvětluje, jak získat přístup k knihovnou SignalR `IHubContext` k odesílání oznámení klientům mimo rozbočovač.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(jak stáhnout)](xref:index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Získat instanci IHubContext

V knihovně SignalR technologie ASP.NET Core, můžete přístup k instanci `IHubContext` pomocí vkládání závislostí. Můžete vložit instance `IHubContext` do kontroleru, middleware nebo jiné služby DI. Použijte instanci pro odesílání zpráv do klientů.

> [!NOTE]
> Tím se liší od ASP.NET 4.x SignalR, která používá GlobalHost k poskytnutí přístupu k `IHubContext`. ASP.NET Core má rozhraní injektáž závislostí, které eliminuje potřebu této globální typu singleton.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Vložit instance IHubContext v kontroleru

Můžete vložit instance `IHubContext` do kontroleru tak, že ho přidáte do konstruktoru:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Nyní, s přístupem k instanci `IHubContext`, jako kdyby byly v centru samotné můžete volat metody rozbočovače.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Získat instanci IHubContext v middlewaru

Přístup `IHubContext` v rámci kanálu middleware takto:

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Kdy jsou volány metody rozbočovače z mimo `Hub` třídy, neexistuje žádný volající přidružené k vyvolání. Proto neexistuje žádný přístup k `ConnectionId`, `Caller`, a `Others` vlastnosti.

## <a name="related-resources"></a>Související prostředky

* [Začínáme](xref:tutorials/signalr)
* [Centra](xref:signalr/hubs)
* [Publikování do Azure](xref:signalr/publish-to-azure-web-app)
