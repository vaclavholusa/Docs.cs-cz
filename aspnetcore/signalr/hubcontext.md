---
title: SignalR HubContext
author: rachelappel
description: Zjistěte, jak používat službu ASP.NET Core SignalR HubContext pro zasílání oznámení klientům mimo rozbočovače.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: ccfcdc8337275fd26e09c1a43db36cf9ab90cf46
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277758"
---
# <a name="send-messages-from-outside-a-hub"></a>Odeslání zprávy z mimo rozbočovače

Podle [Mikael Mengistu](https://twitter.com/MikaelM_12)

Rozbočovače SignalR je základní abstrakci pro odesílání zpráv do klientů připojených k serveru SignalR. Je také možné k odesílání zpráv z jiného místa portálu vaší aplikace pomocí `IHubContext` služby. Tento článek vysvětluje, jak získat přístup systému SignalR `IHubContext` k odesílání oznámení do klientům mimo rozbočovače.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(jak ke stažení)](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Získat instanci `IHubContext`

V ASP.NET Core SignalR, můžete přístup k instanci `IHubContext` pomocí vkládání závislostí. Můžete vložit instanci `IHubContext` do kontroleru, middleware nebo jiné služby DI. Použijte instanci k odesílání zpráv do klientů.

> [!NOTE]
> Tím se liší od funkce SignalR technologie ASP.NET, který používá GlobalHost k poskytování přístupu k `IHubContext`. ASP.NET Core má rozhraní vkládání závislostí, které nemusejí tuto globální typu singleton.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Vložit instanci `IHubContext` v kontroleru

Můžete vložit instanci `IHubContext` do kontroleru přidáním do konstruktoru:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Nyní, s přístupem k instanci `IHubContext`, jako by byl v centru samotné můžete volat metody rozbočovače.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Získat instanci `IHubContext` v middlewaru.

Přístup `IHubContext` v rámci middlewaru v řadě takto:

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Kdy jsou volány metod rozbočovače z mimo `Hub` třídy, že neexistují žádné volající přidružené k vyvolání. Proto není k dispozici přístup k `ConnectionId`, `Caller`, a `Others` vlastnosti.

## <a name="related-resources"></a>Související informační zdroje

* [Začínáme](xref:tutorials/signalr)
* [Centra](xref:signalr/hubs)
* [Publikování do Azure](xref:signalr/publish-to-azure-web-app)
