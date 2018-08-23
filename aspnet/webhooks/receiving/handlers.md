---
uid: webhooks/receiving/handlers
title: Obslužné rutiny ASP.NET – Webhooky | Dokumentace Microsoftu
author: rick-anderson
description: Způsob zpracování požadavků v ASP.NET – Webhooky.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754790"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="1d651-103">Obslužné rutiny ASP.NET – Webhooky</span><span class="sxs-lookup"><span data-stu-id="1d651-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="1d651-104">Jakmile Webhooky požadavků byl ověřen voláním Webhooku příjemce, je zpracovat pomocí uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="1d651-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="1d651-105">Tady *obslužné rutiny* se dělí na.</span><span class="sxs-lookup"><span data-stu-id="1d651-105">This is where *handlers* come in.</span></span> <span data-ttu-id="1d651-106">Obslužné rutiny jsou odvozeny z [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) rozhraní, ale obvykle používá [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) třídy namísto přímo z uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1d651-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="1d651-107">Požadavek Webhooku může zpracovat jeden nebo více obslužných rutin.</span><span class="sxs-lookup"><span data-stu-id="1d651-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="1d651-108">Obslužné rutiny jsou volány v pořadí podle jejich příslušných *pořadí* vlastnost, že přejdete od nejnižší a nejvyšší kde pořadí je jednoduchý celé číslo (doporučujeme, abyste být v rozmezí od 1 do 100):</span><span class="sxs-lookup"><span data-stu-id="1d651-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Obslužná rutina pro WebHook pořadí vlastnosti diagramu](_static/Handlers.png)

<span data-ttu-id="1d651-110">Volitelně můžete nastavit obslužnou rutinu *odpovědi* vlastnost WebHookHandlerContext, což povede k zastavení a reakci na odeslána zpět jako odpověď HTTP k Webhooku zpracování.</span><span class="sxs-lookup"><span data-stu-id="1d651-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="1d651-111">V případě výše nebude získat C obslužná rutina volat, protože má vyšší pořadí, než B a B nastaví odpověď.</span><span class="sxs-lookup"><span data-stu-id="1d651-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="1d651-112">Nastavení odpovědi je obvykle pouze relevantní pro Webhooky, kde bude odpověď přenášet informace zpět do původní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1d651-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="1d651-113">Je třeba v případě, kde se pošle odpověď zpět do kanálu Webhooku, odkud Webhooky Slack.</span><span class="sxs-lookup"><span data-stu-id="1d651-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="1d651-114">Obslužné rutiny můžete nastavit vlastnost příjemce, pokud chtějí dostávat Webhooků z tohoto konkrétního příjemce.</span><span class="sxs-lookup"><span data-stu-id="1d651-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="1d651-115">Pokud příjemce, které jsou volány pro všechny z nich není nastavená.</span><span class="sxs-lookup"><span data-stu-id="1d651-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="1d651-116">Jeden další běžné použití odpověď má používat *410 Gone* odpověď Webhooku už je aktivní a by měly být předloženy žádné další požadavky.</span><span class="sxs-lookup"><span data-stu-id="1d651-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="1d651-117">Ve výchozím nastavení bude volána obslužná rutina všechny spuštěné přijímače zpracovaly Webhooku.</span><span class="sxs-lookup"><span data-stu-id="1d651-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="1d651-118">Nicméně pokud *příjemce* je nastavena na název obslužné rutiny, pak tato obslužná rutina bude přijímat pouze požadavky Webhooku z tento příjemce.</span><span class="sxs-lookup"><span data-stu-id="1d651-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="1d651-119">Zpracování Webhooku</span><span class="sxs-lookup"><span data-stu-id="1d651-119">Processing a WebHook</span></span>

<span data-ttu-id="1d651-120">Když je volána obslužnou rutinu, získá [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) obsahující informace o požadavek Webhooku.</span><span class="sxs-lookup"><span data-stu-id="1d651-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="1d651-121">Data, obvykle obsahu žádosti HTTP, je k dispozici *Data* vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1d651-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="1d651-122">Typ dat je obvykle JSON nebo HTML data formuláře, ale je možné přetypovat na konkrétnější typ, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="1d651-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="1d651-123">Například vlastní Webhooky generovaných ASP.NET – Webhooky může být převeden na typ [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1d651-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="1d651-124">Zpracování ve frontě</span><span class="sxs-lookup"><span data-stu-id="1d651-124">Queued Processing</span></span>

<span data-ttu-id="1d651-125">Většina odesílatelů Webhooku se znovu odeslat Webhooku, pokud je odpověď, není vygenerovaný v rámci několika sekund.</span><span class="sxs-lookup"><span data-stu-id="1d651-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="1d651-126">To znamená, že vaše obslužná rutina musí dokončit zpracování za toto časové období v pořadí není pro něj má být volána znovu.</span><span class="sxs-lookup"><span data-stu-id="1d651-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="1d651-127">Pokud zpracování trvá déle, nebo se lépe využívá samostatně pak bude [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) je možné odeslat požadavek Webhooku do fronty, například [fronty Azure Storage](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="1d651-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="1d651-128">Přehled [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementace je k dispozici zde:</span><span class="sxs-lookup"><span data-stu-id="1d651-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
