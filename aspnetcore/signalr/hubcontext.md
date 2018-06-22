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
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="73430-103">Odeslání zprávy z mimo rozbočovače</span><span class="sxs-lookup"><span data-stu-id="73430-103">Send messages from outside a hub</span></span>

<span data-ttu-id="73430-104">Podle [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="73430-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="73430-105">Rozbočovače SignalR je základní abstrakci pro odesílání zpráv do klientů připojených k serveru SignalR.</span><span class="sxs-lookup"><span data-stu-id="73430-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="73430-106">Je také možné k odesílání zpráv z jiného místa portálu vaší aplikace pomocí `IHubContext` služby.</span><span class="sxs-lookup"><span data-stu-id="73430-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="73430-107">Tento článek vysvětluje, jak získat přístup systému SignalR `IHubContext` k odesílání oznámení do klientům mimo rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="73430-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="73430-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(jak ke stažení)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="73430-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="73430-109">Získat instanci `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="73430-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="73430-110">V ASP.NET Core SignalR, můžete přístup k instanci `IHubContext` pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="73430-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="73430-111">Můžete vložit instanci `IHubContext` do kontroleru, middleware nebo jiné služby DI.</span><span class="sxs-lookup"><span data-stu-id="73430-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="73430-112">Použijte instanci k odesílání zpráv do klientů.</span><span class="sxs-lookup"><span data-stu-id="73430-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="73430-113">Tím se liší od funkce SignalR technologie ASP.NET, který používá GlobalHost k poskytování přístupu k `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="73430-113">This differs from ASP.NET SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="73430-114">ASP.NET Core má rozhraní vkládání závislostí, které nemusejí tuto globální typu singleton.</span><span class="sxs-lookup"><span data-stu-id="73430-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="73430-115">Vložit instanci `IHubContext` v kontroleru</span><span class="sxs-lookup"><span data-stu-id="73430-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="73430-116">Můžete vložit instanci `IHubContext` do kontroleru přidáním do konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="73430-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="73430-117">Nyní, s přístupem k instanci `IHubContext`, jako by byl v centru samotné můžete volat metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="73430-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="73430-118">Získat instanci `IHubContext` v middlewaru.</span><span class="sxs-lookup"><span data-stu-id="73430-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="73430-119">Přístup `IHubContext` v rámci middlewaru v řadě takto:</span><span class="sxs-lookup"><span data-stu-id="73430-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

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
> <span data-ttu-id="73430-120">Kdy jsou volány metod rozbočovače z mimo `Hub` třídy, že neexistují žádné volající přidružené k vyvolání.</span><span class="sxs-lookup"><span data-stu-id="73430-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="73430-121">Proto není k dispozici přístup k `ConnectionId`, `Caller`, a `Others` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="73430-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="73430-122">Související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="73430-122">Related resources</span></span>

* [<span data-ttu-id="73430-123">Začínáme</span><span class="sxs-lookup"><span data-stu-id="73430-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="73430-124">Centra</span><span class="sxs-lookup"><span data-stu-id="73430-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="73430-125">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="73430-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
