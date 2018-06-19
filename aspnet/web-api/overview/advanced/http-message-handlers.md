---
uid: web-api/overview/advanced/http-message-handlers
title: Obslužné rutiny zpráv HTTP v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566389"
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="b015b-102">Obslužné rutiny zpráv HTTP v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b015b-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="b015b-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b015b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b015b-104">A *obslužné rutiny zpráv* je třída, která přijímá požadavek HTTP a vrátí odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="b015b-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="b015b-105">Obslužné rutiny zpráv jsou odvozeny od abstraktní **HttpMessageHandler** třídy.</span><span class="sxs-lookup"><span data-stu-id="b015b-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="b015b-106">Řadu obslužné rutiny zpráv jsou obvykle zřetězen dohromady.</span><span class="sxs-lookup"><span data-stu-id="b015b-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="b015b-107">První obslužná rutina obdrží požadavek HTTP, nemá nějaké zpracování a poskytuje požadavku na další obslužnou rutinu.</span><span class="sxs-lookup"><span data-stu-id="b015b-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="b015b-108">Odpověď v určitém okamžiku se vytvoří a přejde zpět do řetězu.</span><span class="sxs-lookup"><span data-stu-id="b015b-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="b015b-109">Tento vzor nazývá *delegování* obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="b015b-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="b015b-110">Obslužné rutiny zpráv na straně serveru</span><span class="sxs-lookup"><span data-stu-id="b015b-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="b015b-111">Na straně serveru používá kanál rozhraní Web API obslužné rutiny některé integrované zpráv:</span><span class="sxs-lookup"><span data-stu-id="b015b-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="b015b-112">**HttpServer** získá požadavek z hostitele.</span><span class="sxs-lookup"><span data-stu-id="b015b-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="b015b-113">**HttpRoutingDispatcher** odešle zprávu požadavku na základě trasy.</span><span class="sxs-lookup"><span data-stu-id="b015b-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="b015b-114">**HttpControllerDispatcher** odešle žádost do kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b015b-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="b015b-115">Můžete přidat vlastní obslužné rutiny do kanálu.</span><span class="sxs-lookup"><span data-stu-id="b015b-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="b015b-116">Obslužné rutiny zpráv jsou vhodné pro vyjímání mezi aspekty, které fungují na úrovni HTTP zprávy (nikoli akce kontroleru).</span><span class="sxs-lookup"><span data-stu-id="b015b-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="b015b-117">Například může obslužné rutiny zpráv:</span><span class="sxs-lookup"><span data-stu-id="b015b-117">For example, a message handler might:</span></span>

- <span data-ttu-id="b015b-118">Čtení nebo úpravy hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="b015b-118">Read or modify request headers.</span></span>
- <span data-ttu-id="b015b-119">Přidání hlavičky odpovědi do odpovědi.</span><span class="sxs-lookup"><span data-stu-id="b015b-119">Add a response header to responses.</span></span>
- <span data-ttu-id="b015b-120">Ověření požadavků před nedostanou kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b015b-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="b015b-121">Tento diagram zobrazuje dvě vlastní obslužné rutiny, které jsou vloženy do kanálu:</span><span class="sxs-lookup"><span data-stu-id="b015b-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="b015b-122">Na straně klienta používá HttpClient také obslužné rutiny zpráv.</span><span class="sxs-lookup"><span data-stu-id="b015b-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="b015b-123">Další informace najdete v tématu [obslužné rutiny zpráv HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="b015b-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="b015b-124">Obslužné rutiny vlastní zpráv</span><span class="sxs-lookup"><span data-stu-id="b015b-124">Custom Message Handlers</span></span>

<span data-ttu-id="b015b-125">Zápis obslužné rutiny zpráv vlastní, jsou odvozeny od **System.Net.Http.DelegatingHandler** a přepsat **SendAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="b015b-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="b015b-126">Tato metoda má následující podpis:</span><span class="sxs-lookup"><span data-stu-id="b015b-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="b015b-127">Tato metoda přebírá **HttpRequestMessage** jako vstup a asynchronně vrátí **objekt HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="b015b-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="b015b-128">Typická implementace provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="b015b-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="b015b-129">Zpracovat zprávu požadavku.</span><span class="sxs-lookup"><span data-stu-id="b015b-129">Process the request message.</span></span>
2. <span data-ttu-id="b015b-130">Volání `base.SendAsync` odeslat požadavek na vnitřní obslužnou rutinu.</span><span class="sxs-lookup"><span data-stu-id="b015b-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="b015b-131">Vnitřní obslužná rutina vrátí zprávu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="b015b-131">The inner handler returns a response message.</span></span> <span data-ttu-id="b015b-132">(Tento krok je asynchronní.)</span><span class="sxs-lookup"><span data-stu-id="b015b-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="b015b-133">Zpracování odpovědi a vrátí volajícímu.</span><span class="sxs-lookup"><span data-stu-id="b015b-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="b015b-134">Zde je jednoduchý příklad:</span><span class="sxs-lookup"><span data-stu-id="b015b-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="b015b-135">Volání `base.SendAsync` je asynchronní.</span><span class="sxs-lookup"><span data-stu-id="b015b-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="b015b-136">Pokud obslužná rutina nemá žádné pracovní po toto volání, použijte **await** – klíčové slovo, jak je vidět.</span><span class="sxs-lookup"><span data-stu-id="b015b-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="b015b-137">Delegování obslužnou rutinu můžete také přeskočit vnitřní obslužnou rutinu a vytvořit odpověď přímo:</span><span class="sxs-lookup"><span data-stu-id="b015b-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="b015b-138">Pokud delegování obslužná rutina vytvoří odpověď bez volání `base.SendAsync`, žádost přeskočí zbytek kanálu.</span><span class="sxs-lookup"><span data-stu-id="b015b-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="b015b-139">To může být užitečné pro obslužnou rutinu, která ověří žádost (vytvoření chybnou odpověď).</span><span class="sxs-lookup"><span data-stu-id="b015b-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="b015b-140">Přidání obslužné rutiny do kanálu</span><span class="sxs-lookup"><span data-stu-id="b015b-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="b015b-141">Přidání obslužné rutiny zpráv na straně serveru, přidejte obslužnou rutinu do **HttpConfiguration.MessageHandlers** kolekce.</span><span class="sxs-lookup"><span data-stu-id="b015b-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="b015b-142">Pokud jste použili šablony "ASP.NET MVC 4 webové aplikace" a vytvořte tak projekt, můžete to udělat tento uvnitř **WebApiConfig** třídy:</span><span class="sxs-lookup"><span data-stu-id="b015b-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="b015b-143">Obslužné rutiny zpráv se nazývají ve stejném pořadí, ve kterém se zobrazují v **MessageHandlers** kolekce.</span><span class="sxs-lookup"><span data-stu-id="b015b-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="b015b-144">Protože se nejedná o vnořené, zprávu odpovědi přenáší opačným směrem.</span><span class="sxs-lookup"><span data-stu-id="b015b-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="b015b-145">To znamená že poslední obslužná rutina je první, zobrazí se zpráva odpovědi.</span><span class="sxs-lookup"><span data-stu-id="b015b-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="b015b-146">Všimněte si, že nemusíte nastavit vnitřní obslužné rutiny; rozhraní Web API se automaticky připojí obslužné rutiny zpráv.</span><span class="sxs-lookup"><span data-stu-id="b015b-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="b015b-147">Pokud jste [vlastní hostování](../older-versions/self-host-a-web-api.md), vytvořte instanci **HttpSelfHostConfiguration** třídu a přidejte obslužné rutiny na **MessageHandlers** kolekce.</span><span class="sxs-lookup"><span data-stu-id="b015b-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="b015b-148">Nyní Podíváme se na některé příklady obslužné rutiny zpráv vlastní.</span><span class="sxs-lookup"><span data-stu-id="b015b-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="b015b-149">Příklad: X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="b015b-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="b015b-150">X-HTTP-Method-Override je nestandardní hlavičku HTTP.</span><span class="sxs-lookup"><span data-stu-id="b015b-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="b015b-151">Je určený pro klienty, kteří nelze odeslat určité typy požadavků HTTP, například PUT nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="b015b-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="b015b-152">Místo toho klient odešle požadavek POST a nastaví hlavičku X-HTTP-Method-Override na požadovanou metodu.</span><span class="sxs-lookup"><span data-stu-id="b015b-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="b015b-153">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b015b-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="b015b-154">Tady je popisovač zpráv, který přidává podporu pro X-HTTP-Method-Override:</span><span class="sxs-lookup"><span data-stu-id="b015b-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="b015b-155">V **SendAsync** metody obslužná rutina kontroluje, zda zpráva požadavku je požadavek POST a zda obsahuje hlavičku X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="b015b-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="b015b-156">Pokud ano, ověří hodnotu hlavičky a pak upravením metodu požadavku.</span><span class="sxs-lookup"><span data-stu-id="b015b-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="b015b-157">Nakonec volá obslužnou rutinu `base.SendAsync` předat zprávu další obslužné rutině.</span><span class="sxs-lookup"><span data-stu-id="b015b-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="b015b-158">Když požadavek dosáhne **HttpControllerDispatcher** třídy, **HttpControllerDispatcher** bude směrovat žádosti založené na metodě aktualizované požadavku.</span><span class="sxs-lookup"><span data-stu-id="b015b-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="b015b-159">Příklad: Přidání vlastní hlavičky odpovědi</span><span class="sxs-lookup"><span data-stu-id="b015b-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="b015b-160">Tady je popisovač zpráv, který přidá vlastní hlavičku každou zprávu s odpovědí:</span><span class="sxs-lookup"><span data-stu-id="b015b-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="b015b-161">Nejprve obslužná rutina volá `base.SendAsync` k předání požadavku do vnitřní popisovač zprávy.</span><span class="sxs-lookup"><span data-stu-id="b015b-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="b015b-162">Vnitřní obslužná rutina vrátí zprávu odpovědi, ale nepracuje tak asynchronně pomocí **úloh&lt;T&gt;**  objektu.</span><span class="sxs-lookup"><span data-stu-id="b015b-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="b015b-163">Zpráva odpovědi není dostupná, dokud nebude `base.SendAsync` dokončení asynchronně.</span><span class="sxs-lookup"><span data-stu-id="b015b-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="b015b-164">Tento příklad používá **await** – klíčové slovo pro práci asynchronně po `SendAsync` dokončení.</span><span class="sxs-lookup"><span data-stu-id="b015b-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="b015b-165">Pokud cílíte na rozhraní .NET Framework 4.0, použijte **úloh**&lt;T&gt;**. ContinueWith** metoda:</span><span class="sxs-lookup"><span data-stu-id="b015b-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="b015b-166">Příklad: Kontrola klíč rozhraní API</span><span class="sxs-lookup"><span data-stu-id="b015b-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="b015b-167">Některé webové služby, aby klienti obsahovat klíč rozhraní API v jejich požadavku.</span><span class="sxs-lookup"><span data-stu-id="b015b-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="b015b-168">Následující příklad ukazuje, jak obslužné rutiny zpráv můžete zkontrolovat požadavky na platný klíč rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="b015b-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="b015b-169">Tato rutina vyhledá klíč rozhraní API v řetězci dotazu identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b015b-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="b015b-170">(V tomto příkladu předpokládáme, že klíč je statický řetězec.</span><span class="sxs-lookup"><span data-stu-id="b015b-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="b015b-171">Skutečné implementace by pravděpodobně používají složitější ověřování.) Pokud řetězec dotazu obsahuje klíč, obslužná rutina předá požadavek na vnitřní obslužnou rutinu.</span><span class="sxs-lookup"><span data-stu-id="b015b-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="b015b-172">Pokud požadavek nemá platný klíč, obslužná rutina vytvoří zprávu odpovědi, která se stavem 403, zakázáno.</span><span class="sxs-lookup"><span data-stu-id="b015b-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="b015b-173">V takovém případě obslužná rutina nevyvolá `base.SendAsync`, takže vnitřní obslužnou rutinu nikdy obdrží požadavek ani neprovádí kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b015b-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="b015b-174">Proto řadičem můžete předpokládá, že všechny příchozí požadavky platný klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b015b-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="b015b-175">Pokud klíč rozhraní API se vztahuje pouze na určité akce kontroleru, zvažte použití filtru akce místo obslužné rutiny zpráv.</span><span class="sxs-lookup"><span data-stu-id="b015b-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="b015b-176">Filtry akcí spustit po URI směrování se provádí.</span><span class="sxs-lookup"><span data-stu-id="b015b-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="b015b-177">Obslužné rutiny zpráv za trasy</span><span class="sxs-lookup"><span data-stu-id="b015b-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="b015b-178">Obslužné rutiny v **HttpConfiguration.MessageHandlers** kolekce platí globálně.</span><span class="sxs-lookup"><span data-stu-id="b015b-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="b015b-179">Alternativně můžete přidat obslužné rutiny zpráv na určený postup při definování trasy:</span><span class="sxs-lookup"><span data-stu-id="b015b-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="b015b-180">V tomto příkladu, pokud odpovídá identifikátoru URI požadavku "Route2", žádost je odeslaných `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="b015b-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="b015b-181">Následující diagram znázorňuje kanálu pro tyto dvě cesty:</span><span class="sxs-lookup"><span data-stu-id="b015b-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="b015b-182">Všimněte si, že `MessageHandler2` nahradí výchozí **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="b015b-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="b015b-183">V tomto příkladu `MessageHandler2` vytvoří odpovědi a požadavky, které odpovídají "Route2" nikdy přejděte do řadiče.</span><span class="sxs-lookup"><span data-stu-id="b015b-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="b015b-184">Díky tomu můžete nahradit vlastní koncový bod celého mechanismus kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b015b-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="b015b-185">Alternativně můžete delegovat obslužné rutiny zpráv za trasy na **HttpControllerDispatcher**, který pak odešle zprávu do řadiče.</span><span class="sxs-lookup"><span data-stu-id="b015b-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="b015b-186">Následující kód ukazuje, jak nakonfigurovat tento postup:</span><span class="sxs-lookup"><span data-stu-id="b015b-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
