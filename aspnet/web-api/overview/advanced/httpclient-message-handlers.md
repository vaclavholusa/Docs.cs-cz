---
uid: web-api/overview/advanced/httpclient-message-handlers
title: "Obslužné rutiny zpráv HttpClient v rozhraní ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="ef0f3-102">Obslužné rutiny zpráv HttpClient v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="ef0f3-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="ef0f3-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ef0f3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ef0f3-104">A *obslužné rutiny zpráv* je třída, která přijímá požadavek HTTP a vrátí odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="ef0f3-105">Řadu obslužné rutiny zpráv jsou obvykle zřetězen dohromady.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="ef0f3-106">První obslužná rutina obdrží požadavek HTTP, nemá nějaké zpracování a poskytuje požadavku na další obslužnou rutinu.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="ef0f3-107">Odpověď v určitém okamžiku se vytvoří a přejde zpět do řetězu.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="ef0f3-108">Tento vzor nazývá *delegování* obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="ef0f3-109">Na straně klienta **HttpClient** třída používá obslužné rutiny zpráv pro zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="ef0f3-110">Výchozí obslužnou rutinu je **HttpClientHandler**, který odešle žádost přes síť a získá odpověď ze serveru.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="ef0f3-111">Obslužné rutiny zpráv vlastní můžete vložit do kanálu klienta:</span><span class="sxs-lookup"><span data-stu-id="ef0f3-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="ef0f3-112">Rozhraní ASP.NET Web API používá také obslužné rutiny zpráv na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="ef0f3-113">Další informace najdete v tématu [obslužné rutiny zpráv HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="ef0f3-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="ef0f3-114">Obslužné rutiny vlastní zpráv</span><span class="sxs-lookup"><span data-stu-id="ef0f3-114">Custom Message Handlers</span></span>

<span data-ttu-id="ef0f3-115">Zápis obslužné rutiny zpráv vlastní, jsou odvozeny od **System.Net.Http.DelegatingHandler** a přepsat **SendAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="ef0f3-116">Tady je podpis metody:</span><span class="sxs-lookup"><span data-stu-id="ef0f3-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="ef0f3-117">Tato metoda přebírá **HttpRequestMessage** jako vstup a asynchronně vrátí **objekt HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="ef0f3-118">Typická implementace provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="ef0f3-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="ef0f3-119">Zpracovat zprávu požadavku.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-119">Process the request message.</span></span>
2. <span data-ttu-id="ef0f3-120">Volání `base.SendAsync` odeslat požadavek na vnitřní obslužnou rutinu.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="ef0f3-121">Vnitřní obslužná rutina vrátí zprávu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-121">The inner handler returns a response message.</span></span> <span data-ttu-id="ef0f3-122">(Tento krok je asynchronní.)</span><span class="sxs-lookup"><span data-stu-id="ef0f3-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="ef0f3-123">Zpracování odpovědi a vrátí volajícímu.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="ef0f3-124">Následující příklad ukazuje popisovač zpráv, který přidá vlastní hlavičku odchozí žádost:</span><span class="sxs-lookup"><span data-stu-id="ef0f3-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="ef0f3-125">Volání `base.SendAsync` je asynchronní.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="ef0f3-126">Pokud obslužná rutina nemá žádné pracovní po toto volání, použijte **await** – klíčové slovo Chcete-li pokračovat v provádění po dokončení metody.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="ef0f3-127">Následující příklad ukazuje obslužnou rutinu, která protokoly kódy chyb.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="ef0f3-128">Protokolování samotné není velmi zajímavé, ale tento příklad ukazuje, jak získat v odpovědi uvnitř obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="ef0f3-129">Přidání obslužných rutin zpráv do kanálu klienta</span><span class="sxs-lookup"><span data-stu-id="ef0f3-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="ef0f3-130">Chcete-li přidat vlastní obslužné rutiny na **HttpClient**, použijte **HttpClientFactory.Create** metoda:</span><span class="sxs-lookup"><span data-stu-id="ef0f3-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="ef0f3-131">Obslužné rutiny zpráv se nazývají v pořadí, ve kterém můžete předat je do **vytvořit** metoda.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="ef0f3-132">Protože jsou vnořené obslužné rutiny, zprávu odpovědi přenáší opačným směrem.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="ef0f3-133">To znamená že poslední obslužná rutina je první, zobrazí se zpráva odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ef0f3-133">That is, the last handler is the first to get the response message.</span></span>
