---
uid: webhooks/receiving/handlers
title: "Obslužné rutiny ASP.NET Webhooky | Microsoft Docs"
author: rick-anderson
description: "Postupy: zpracování požadavků ASP.NET Webhooky."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 3aaef756ee00d7e44aa757062e1ef297312ecf22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="ad523-103">Obslužné rutiny ASP.NET Webhooky</span><span class="sxs-lookup"><span data-stu-id="ad523-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="ad523-104">Jakmile Webhooky žádosti byl ověřen voláním Webhooku příjemce, je připraven ke zpracování pomocí uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="ad523-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="ad523-105">To je, kdy *obslužné rutiny* mají.</span><span class="sxs-lookup"><span data-stu-id="ad523-105">This is where *handlers* come in.</span></span> <span data-ttu-id="ad523-106">Obslužné rutiny jsou odvozeny od [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) rozhraní ale většinou se používá [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) třída místo odvozování přímo z uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ad523-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="ad523-107">Žádost o Webhooku může zpracovat jeden nebo více obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="ad523-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="ad523-108">V pořadí podle jejich odpovídajících jsou volány obslužné rutiny *pořadí* vlastnost přecházející z nejnižší a nejvyšší kde pořadí je jednoduchý celé číslo (navrhované být od 1 do 100):</span><span class="sxs-lookup"><span data-stu-id="ad523-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Obslužná rutina Webhooku pořadí vlastnost Diagram](_static/Handlers.png)

<span data-ttu-id="ad523-110">Volitelně můžete nastavit obslužnou rutinu *odpovědi* vlastnost WebHookHandlerContext, což povede k zastavení a odpověď na odeslána zpět jako odpověď HTTP na Webhooku zpracování.</span><span class="sxs-lookup"><span data-stu-id="ad523-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="ad523-111">V případě výše nebude získat C obslužná rutina volat, protože má vyšší pořadí než B a B a nastaví odpověď.</span><span class="sxs-lookup"><span data-stu-id="ad523-111">In the case above, Handler C won’t get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="ad523-112">Nastavení odpovědi je obvykle pouze relevantní pro Webhooků, kde bude odpověď přenášet informace zpět do původní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ad523-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="ad523-113">Je třeba v případě, kde odpověď je odeslána zpět do kanálu, odkud pochází Webhooku Webhooky Slack.</span><span class="sxs-lookup"><span data-stu-id="ad523-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="ad523-114">Obslužné rutiny můžete nastavit vlastnost příjemce, pokud chtějí dostávat Webhooků, že konkrétní příjemce.</span><span class="sxs-lookup"><span data-stu-id="ad523-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="ad523-115">Pokud není nastavená příjemce jsou volány pro všechny z nich.</span><span class="sxs-lookup"><span data-stu-id="ad523-115">If they don’t set the receiver they are called for all of them.</span></span>

<span data-ttu-id="ad523-116">Jeden další běžné použití odpovědi je používat *410 Gone* odpověď na znamenat, že Webhooku už je aktivní a měly by být předloženy žádné další požadavky.</span><span class="sxs-lookup"><span data-stu-id="ad523-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="ad523-117">Ve výchozím nastavení bude volána obslužná rutina všechny Webhooku příjemci.</span><span class="sxs-lookup"><span data-stu-id="ad523-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="ad523-118">Ale pokud *příjemce* je nastavena na název obslužná rutina pak této obslužné rutiny bude přijímat pouze požadavky Webhooku z této příjemce.</span><span class="sxs-lookup"><span data-stu-id="ad523-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="ad523-119">Zpracování WebHook, jehož</span><span class="sxs-lookup"><span data-stu-id="ad523-119">Processing a WebHook</span></span>

<span data-ttu-id="ad523-120">Když je volána obslužná rutina, získá [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) obsahující informace o požadavku Webhooku.</span><span class="sxs-lookup"><span data-stu-id="ad523-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="ad523-121">Data, obvykle obsahu žádosti HTTP, je k dispozici z *Data* vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ad523-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="ad523-122">Typ dat je obvykle dat formuláře HTML nebo JSON, ale je možné převést na více konkrétního typu, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="ad523-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="ad523-123">Například může být vlastní Webhooků, které jsou generované ASP.NET Webhooky přetypovat na typ [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ad523-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="ad523-124">Zpracování ve frontě</span><span class="sxs-lookup"><span data-stu-id="ad523-124">Queued Processing</span></span>

<span data-ttu-id="ad523-125">Většina odesílatelé Webhooku se znovu odeslat WebHook, jehož, pokud odpověď nevygenerovala během pár sekund.</span><span class="sxs-lookup"><span data-stu-id="ad523-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="ad523-126">To znamená, že vaší obslužné rutiny musí dokončit zpracování za toto časové období v pořadí není pro něj má být volána znovu.</span><span class="sxs-lookup"><span data-stu-id="ad523-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="ad523-127">Pokud zpracování trvá déle, nebo je vhodnější provádět samostatně potom [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) lze použít k odeslání požadavku Webhooku do fronty, například [fronty Azure Storage](https://msdn.microsoft.com/en-us/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad523-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/en-us/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="ad523-128">Přehled [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementace je k dispozici zde:</span><span class="sxs-lookup"><span data-stu-id="ad523-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
