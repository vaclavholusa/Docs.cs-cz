---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Obslužné rutiny zpráv HttpClient ve webovém rozhraní API technologie ASP.NET | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: db9edcf4fb31e967c3d4e7635f96c68829aec97d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831369"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="9da08-102">Obslužné rutiny zpráv HttpClient ve webovém rozhraní API technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9da08-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="9da08-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9da08-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9da08-104">A *obslužná rutina zprávy* je třída, která přijme požadavek HTTP a vrátí odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="9da08-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="9da08-105">Řada obslužné rutiny zpráv jsou obvykle zřetězen dohromady.</span><span class="sxs-lookup"><span data-stu-id="9da08-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="9da08-106">První obslužná rutina požadavku HTTP, provádí nějaké zpracování a poskytuje k další obslužná rutina požadavku.</span><span class="sxs-lookup"><span data-stu-id="9da08-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="9da08-107">V určitém okamžiku odpovědi se vytvoří a vrátí řetězem.</span><span class="sxs-lookup"><span data-stu-id="9da08-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="9da08-108">Tento model se nazývá *delegování* obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="9da08-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="9da08-109">Na straně klienta **HttpClient** třída používá obslužné rutiny zpráv pro zpracování žádostí.</span><span class="sxs-lookup"><span data-stu-id="9da08-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="9da08-110">Je výchozí obslužnou rutinu **HttpClientHandler**, který odešle požadavek v síti a získá odpověď ze serveru.</span><span class="sxs-lookup"><span data-stu-id="9da08-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="9da08-111">Obslužné rutiny vlastních zpráv můžete vložit do kanálu klienta:</span><span class="sxs-lookup"><span data-stu-id="9da08-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="9da08-112">Rozhraní ASP.NET Web API také používá obslužné rutiny zpráv na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="9da08-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="9da08-113">Další informace najdete v tématu [obslužné rutiny zpráv HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="9da08-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="9da08-114">Obslužné rutiny vlastních zpráv</span><span class="sxs-lookup"><span data-stu-id="9da08-114">Custom Message Handlers</span></span>

<span data-ttu-id="9da08-115">Zápis obslužné rutiny vlastních zpráv, jsou odvozeny z **System.Net.Http.DelegatingHandler** a přepsat **SendAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="9da08-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="9da08-116">Následuje podpis metody:</span><span class="sxs-lookup"><span data-stu-id="9da08-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="9da08-117">Tato metoda přebírá **HttpRequestMessage** jako vstup a asynchronně vrátí **objekt HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="9da08-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="9da08-118">Obvyklá implementace provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="9da08-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="9da08-119">Zpracování zprávy s požadavkem.</span><span class="sxs-lookup"><span data-stu-id="9da08-119">Process the request message.</span></span>
2. <span data-ttu-id="9da08-120">Volání `base.SendAsync` odešlete žádost na vnitřní obslužnou rutinu.</span><span class="sxs-lookup"><span data-stu-id="9da08-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="9da08-121">Vnitřní obslužná rutina vrátí zprávu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9da08-121">The inner handler returns a response message.</span></span> <span data-ttu-id="9da08-122">(Tento krok je asynchronní.)</span><span class="sxs-lookup"><span data-stu-id="9da08-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="9da08-123">Zpracování odpovědi a vrátí řízení volajícímu.</span><span class="sxs-lookup"><span data-stu-id="9da08-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="9da08-124">Následující příklad ukazuje popisovač zpráv, který přidá vlastní hlavičku na odchozí žádost:</span><span class="sxs-lookup"><span data-stu-id="9da08-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="9da08-125">Volání `base.SendAsync` je asynchronní.</span><span class="sxs-lookup"><span data-stu-id="9da08-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="9da08-126">Pokud obslužná rutina nemá žádnou práci po tomto volání, použijte **await** – klíčové slovo pokračování provádění po dokončení metody.</span><span class="sxs-lookup"><span data-stu-id="9da08-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="9da08-127">Následující příklad ukazuje obslužnou rutinu, která protokoluje kódy chyb.</span><span class="sxs-lookup"><span data-stu-id="9da08-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="9da08-128">Protokolování samotný není moc zajímavý, ale tento příklad ukazuje, jak získat odpovědi uvnitř obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="9da08-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="9da08-129">Přidání obslužných rutin zpráv do kanálu klienta</span><span class="sxs-lookup"><span data-stu-id="9da08-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="9da08-130">Chcete-li přidat vlastní obslužné rutiny pro **HttpClient**, použijte **HttpClientFactory.Create** metody:</span><span class="sxs-lookup"><span data-stu-id="9da08-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="9da08-131">Obslužné rutiny zpráv jsou volány v pořadí, ve kterém můžete předat je do **vytvořit** metody.</span><span class="sxs-lookup"><span data-stu-id="9da08-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="9da08-132">Vzhledem k tomu, že jsou vnořené obslužných rutin, v opačném směru přenáší zprávy s odpovědí.</span><span class="sxs-lookup"><span data-stu-id="9da08-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="9da08-133">To znamená že poslední obslužnou rutinou je první získá zprávu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9da08-133">That is, the last handler is the first to get the response message.</span></span>
