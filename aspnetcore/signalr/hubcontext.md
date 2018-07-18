---
title: SignalR HubContext
author: tdykstra
description: Zjistěte, jak použít službu SignalR HubContext technologie ASP.NET Core pro odesílání oznámení klientům mimo rozbočovač.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: 6b955c2064d7d6a045594e56326e2f7df282675f
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095304"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="44473-103">Odeslání zprávy z mimo rozbočovač</span><span class="sxs-lookup"><span data-stu-id="44473-103">Send messages from outside a hub</span></span>

<span data-ttu-id="44473-104">Podle [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="44473-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="44473-105">Rozbočovače SignalR je základní abstrakci pro odesílání zpráv do klientů připojených k serveru funkce SignalR.</span><span class="sxs-lookup"><span data-stu-id="44473-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="44473-106">Je také možné odesílat zprávy z jiného místa portálu vaši aplikaci s použitím `IHubContext` služby.</span><span class="sxs-lookup"><span data-stu-id="44473-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="44473-107">Tento článek vysvětluje, jak získat přístup k knihovnou SignalR `IHubContext` k odesílání oznámení klientům mimo rozbočovač.</span><span class="sxs-lookup"><span data-stu-id="44473-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="44473-108">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(jak stáhnout)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="44473-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="44473-109">Získání instance typu `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="44473-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="44473-110">V knihovně SignalR technologie ASP.NET Core, můžete přístup k instanci `IHubContext` pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="44473-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="44473-111">Můžete vložit instance `IHubContext` do kontroleru, middleware nebo jiné služby DI.</span><span class="sxs-lookup"><span data-stu-id="44473-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="44473-112">Použijte instanci pro odesílání zpráv do klientů.</span><span class="sxs-lookup"><span data-stu-id="44473-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="44473-113">Tím se liší od funkce SignalR technologie ASP.NET, který používá GlobalHost pro poskytnutí přístupu k `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="44473-113">This differs from ASP.NET SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="44473-114">ASP.NET Core má rozhraní injektáž závislostí, které eliminuje potřebu této globální typu singleton.</span><span class="sxs-lookup"><span data-stu-id="44473-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="44473-115">Vloží instanci `IHubContext` v kontroleru</span><span class="sxs-lookup"><span data-stu-id="44473-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="44473-116">Můžete vložit instance `IHubContext` do kontroleru tak, že ho přidáte do konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="44473-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="44473-117">Nyní, s přístupem k instanci `IHubContext`, jako kdyby byly v centru samotné můžete volat metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="44473-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="44473-118">Získat instanci `IHubContext` v middlewaru</span><span class="sxs-lookup"><span data-stu-id="44473-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="44473-119">Přístup `IHubContext` v rámci kanálu middleware takto:</span><span class="sxs-lookup"><span data-stu-id="44473-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

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
> <span data-ttu-id="44473-120">Kdy jsou volány metody rozbočovače z mimo `Hub` třídy, neexistuje žádný volající přidružené k vyvolání.</span><span class="sxs-lookup"><span data-stu-id="44473-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="44473-121">Proto neexistuje žádný přístup k `ConnectionId`, `Caller`, a `Others` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="44473-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="44473-122">Související prostředky</span><span class="sxs-lookup"><span data-stu-id="44473-122">Related resources</span></span>

* [<span data-ttu-id="44473-123">Začínáme</span><span class="sxs-lookup"><span data-stu-id="44473-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="44473-124">Centra</span><span class="sxs-lookup"><span data-stu-id="44473-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="44473-125">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="44473-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
