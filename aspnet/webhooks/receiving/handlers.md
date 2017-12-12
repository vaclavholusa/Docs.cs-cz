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
# <a name="aspnet-webhooks-handlers"></a>Obslužné rutiny ASP.NET Webhooky

Jakmile Webhooky žádosti byl ověřen voláním Webhooku příjemce, je připraven ke zpracování pomocí uživatelského kódu. To je, kdy *obslužné rutiny* mají. Obslužné rutiny jsou odvozeny od [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) rozhraní ale většinou se používá [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) třída místo odvozování přímo z uživatelského rozhraní.

Žádost o Webhooku může zpracovat jeden nebo více obslužné rutiny. V pořadí podle jejich odpovídajících jsou volány obslužné rutiny *pořadí* vlastnost přecházející z nejnižší a nejvyšší kde pořadí je jednoduchý celé číslo (navrhované být od 1 do 100):

![Obslužná rutina Webhooku pořadí vlastnost Diagram](_static/Handlers.png)

Volitelně můžete nastavit obslužnou rutinu *odpovědi* vlastnost WebHookHandlerContext, což povede k zastavení a odpověď na odeslána zpět jako odpověď HTTP na Webhooku zpracování. V případě výše nebude získat C obslužná rutina volat, protože má vyšší pořadí než B a B a nastaví odpověď.

Nastavení odpovědi je obvykle pouze relevantní pro Webhooků, kde bude odpověď přenášet informace zpět do původní rozhraní API. Je třeba v případě, kde odpověď je odeslána zpět do kanálu, odkud pochází Webhooku Webhooky Slack. Obslužné rutiny můžete nastavit vlastnost příjemce, pokud chtějí dostávat Webhooků, že konkrétní příjemce. Pokud není nastavená příjemce jsou volány pro všechny z nich.

Jeden další běžné použití odpovědi je používat *410 Gone* odpověď na znamenat, že Webhooku už je aktivní a měly by být předloženy žádné další požadavky.

Ve výchozím nastavení bude volána obslužná rutina všechny Webhooku příjemci. Ale pokud *příjemce* je nastavena na název obslužná rutina pak této obslužné rutiny bude přijímat pouze požadavky Webhooku z této příjemce.

## <a name="processing-a-webhook"></a>Zpracování WebHook, jehož

Když je volána obslužná rutina, získá [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) obsahující informace o požadavku Webhooku. Data, obvykle obsahu žádosti HTTP, je k dispozici z *Data* vlastnost.

Typ dat je obvykle dat formuláře HTML nebo JSON, ale je možné převést na více konkrétního typu, v případě potřeby. Například může být vlastní Webhooků, které jsou generované ASP.NET Webhooky přetypovat na typ [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) následujícím způsobem:

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

  ## <a name="queued-processing"></a>Zpracování ve frontě

Většina odesílatelé Webhooku se znovu odeslat WebHook, jehož, pokud odpověď nevygenerovala během pár sekund. To znamená, že vaší obslužné rutiny musí dokončit zpracování za toto časové období v pořadí není pro něj má být volána znovu.

Pokud zpracování trvá déle, nebo je vhodnější provádět samostatně potom [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) lze použít k odeslání požadavku Webhooku do fronty, například [fronty Azure Storage](https://msdn.microsoft.com/en-us/library/azure/dd179353.aspx).

Přehled [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementace je k dispozici zde:

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
