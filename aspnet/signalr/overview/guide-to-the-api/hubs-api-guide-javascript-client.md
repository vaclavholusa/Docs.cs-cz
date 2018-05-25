---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Funkce SignalR technologie ASP.NET centra API Průvodce – klienta JavaScript | Microsoft Docs
author: pfletcher
description: Tento dokument obsahuje úvod do používání centra rozhraní API pro SignalR verze 2 v klientech JavaScript, například s prohlížeči a applicat Windows Store (WinJS)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 794ab576d3c6600911f331bab7c335476e45a0c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="6dafd-103">Funkce SignalR technologie ASP.NET centra API Průvodce – JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="6dafd-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="6dafd-104">podle [Patrik Fletcher](https://github.com/pfletcher), [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6dafd-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="6dafd-105">Tento dokument obsahuje úvod do používání centra rozhraní API pro SignalR verze 2 v klientech JavaScript, například s prohlížeči a aplikace pro Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="6dafd-105">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="6dafd-106">Rozhraní API rozbočovače SignalR umožňuje vytvářet vzdálená volání procedur (RPC) ze serveru pro připojené klienty a z klientů k serveru.</span><span class="sxs-lookup"><span data-stu-id="6dafd-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="6dafd-107">V serverovém kódu můžete definovat metody, které lze volat klienty a volání metody, které běží na klientovi.</span><span class="sxs-lookup"><span data-stu-id="6dafd-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="6dafd-108">V kódu klienta můžete definovat metody, které lze volat ze serveru a volání metody, které běží na serveru.</span><span class="sxs-lookup"><span data-stu-id="6dafd-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="6dafd-109">SignalR se stará o všechny záležitosti klient server pro vás.</span><span class="sxs-lookup"><span data-stu-id="6dafd-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="6dafd-110">Funkce SignalR také nabízí nižší úrovně rozhraní API volat trvalé připojení.</span><span class="sxs-lookup"><span data-stu-id="6dafd-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="6dafd-111">Úvod do SignalR, rozbočovače a trvalé připojení, najdete v části [Úvod do SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="6dafd-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="6dafd-112">Verze softwaru použitým v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="6dafd-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="6dafd-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6dafd-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="6dafd-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6dafd-114">.NET 4.5</span></span>
> - <span data-ttu-id="6dafd-115">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="6dafd-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="6dafd-116">Předchozí verze tohoto tématu</span><span class="sxs-lookup"><span data-stu-id="6dafd-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="6dafd-117">Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="6dafd-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="6dafd-118">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="6dafd-118">Questions and comments</span></span>
> 
> <span data-ttu-id="6dafd-119">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="6dafd-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="6dafd-120">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="6dafd-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="6dafd-121">Přehled</span><span class="sxs-lookup"><span data-stu-id="6dafd-121">Overview</span></span>

<span data-ttu-id="6dafd-122">Tento dokument obsahuje následující části:</span><span class="sxs-lookup"><span data-stu-id="6dafd-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="6dafd-123">Vygenerovaný proxy server a jakým způsobem vám</span><span class="sxs-lookup"><span data-stu-id="6dafd-123">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="6dafd-124">Kdy použít vygenerovaný proxy server</span><span class="sxs-lookup"><span data-stu-id="6dafd-124">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="6dafd-125">Instalace klienta</span><span class="sxs-lookup"><span data-stu-id="6dafd-125">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="6dafd-126">Jak chcete-li dynamicky vygenerovaný proxy server</span><span class="sxs-lookup"><span data-stu-id="6dafd-126">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="6dafd-127">Postup vytvoření fyzický soubor pro funkci SignalR generované proxy</span><span class="sxs-lookup"><span data-stu-id="6dafd-127">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="6dafd-128">Postup připojení</span><span class="sxs-lookup"><span data-stu-id="6dafd-128">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="6dafd-129">$. connection.hub je stejný objekt vytvoří tento $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="6dafd-129">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="6dafd-130">Asynchronní provádění metody start</span><span class="sxs-lookup"><span data-stu-id="6dafd-130">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="6dafd-131">Postup připojení mezi doménami</span><span class="sxs-lookup"><span data-stu-id="6dafd-131">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="6dafd-132">Postup konfigurace připojení</span><span class="sxs-lookup"><span data-stu-id="6dafd-132">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="6dafd-133">Určení parametrů řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="6dafd-133">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="6dafd-134">Určení metodu přenosu</span><span class="sxs-lookup"><span data-stu-id="6dafd-134">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="6dafd-135">Jak získat proxy server pro třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="6dafd-135">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="6dafd-136">Jak definovat metody na straně klienta, které můžete volat serveru</span><span class="sxs-lookup"><span data-stu-id="6dafd-136">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="6dafd-137">Jak volat metody serveru z klienta</span><span class="sxs-lookup"><span data-stu-id="6dafd-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="6dafd-138">Postupy: zpracování události životnost připojení</span><span class="sxs-lookup"><span data-stu-id="6dafd-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="6dafd-139">Způsob zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="6dafd-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="6dafd-140">Jak povolit protokolování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="6dafd-140">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="6dafd-141">Pro dokumentaci o tom, jak program server nebo klientů .NET, naleznete v následujících zdrojích:</span><span class="sxs-lookup"><span data-stu-id="6dafd-141">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="6dafd-142">Rozhraní API Průvodce pro rozbočovače SignalR – Server</span><span class="sxs-lookup"><span data-stu-id="6dafd-142">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="6dafd-143">Rozhraní API Průvodce pro rozbočovače SignalR – klient .NET</span><span class="sxs-lookup"><span data-stu-id="6dafd-143">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="6dafd-144">Součást serveru SignalR 2 je dostupná pouze na rozhraní .NET 4.5 (i když je klient .NET pro SignalR 2 na rozhraní .NET 4.0).</span><span class="sxs-lookup"><span data-stu-id="6dafd-144">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="6dafd-145">Vygenerovaný proxy server a jakým způsobem vám</span><span class="sxs-lookup"><span data-stu-id="6dafd-145">The generated proxy and what it does for you</span></span>

<span data-ttu-id="6dafd-146">Můžete naprogramovat JavaScript klienta pro komunikaci se službou SignalR s nebo bez proxy server, který generuje SignalR pro vás.</span><span class="sxs-lookup"><span data-stu-id="6dafd-146">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="6dafd-147">Proxy vykonává vám je zjednodušení syntaxe kódu můžete použít pro připojení, metody zápisu, které server volá, a volání metody na serveru.</span><span class="sxs-lookup"><span data-stu-id="6dafd-147">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="6dafd-148">Při psaní kódu pro volání metody server vygenerovaný proxy server umožňuje použijte syntaxi, která vypadá jako kdyby byly provádění místní funkce: můžete napsat `serverMethod(arg1, arg2)` místo `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="6dafd-148">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="6dafd-149">Syntaxe vygenerovaný proxy server také umožňuje okamžitou a srozumitelné chybě na straně klienta, pokud překlep metoda název serveru.</span><span class="sxs-lookup"><span data-stu-id="6dafd-149">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="6dafd-150">A pokud ručně vytvoříte soubor, který definuje proxy serverů, můžete také získat podporu technologie IntelliSense pro psaní kódu, který volá metody server.</span><span class="sxs-lookup"><span data-stu-id="6dafd-150">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="6dafd-151">Předpokládejme například, že máte následující třídy rozbočovače na serveru:</span><span class="sxs-lookup"><span data-stu-id="6dafd-151">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="6dafd-152">Následující příklady kódu ukazují, co JavaScript kódu vypadá jako pro vyvolání `NewContosoChatMessage` metoda na server a přijímá volání `addContosoChatMessageToPage` metoda ze serveru.</span><span class="sxs-lookup"><span data-stu-id="6dafd-152">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="6dafd-153">**S vygenerovaný proxy server**</span><span class="sxs-lookup"><span data-stu-id="6dafd-153">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="6dafd-154">**Bez vygenerovaný proxy server**</span><span class="sxs-lookup"><span data-stu-id="6dafd-154">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="6dafd-155">Kdy použít vygenerovaný proxy server</span><span class="sxs-lookup"><span data-stu-id="6dafd-155">When to use the generated proxy</span></span>

<span data-ttu-id="6dafd-156">Pokud chcete zaregistrovat více obslužných rutin událostí pro metodu klienta, který volá serveru, nemůžete použít vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="6dafd-156">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="6dafd-157">Jinak můžete zvolit vygenerovaný proxy server nebo není založen na zúčastníte kódování.</span><span class="sxs-lookup"><span data-stu-id="6dafd-157">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="6dafd-158">Pokud si zvolíte možnost nepoužít, nemusíte odkazovat na adresu "rozbočovače/signalr" v `script` elementu v kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="6dafd-158">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="6dafd-159">Instalace klienta</span><span class="sxs-lookup"><span data-stu-id="6dafd-159">Client setup</span></span>

<span data-ttu-id="6dafd-160">Klient JavaScript vyžaduje odkazy na jQuery a soubor JavaScript SignalR core.</span><span class="sxs-lookup"><span data-stu-id="6dafd-160">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="6dafd-161">JQuery verze musí být 1.6.4 nebo hlavní novější verze, například 1.7.2, 1.8.2 nebo 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="6dafd-161">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="6dafd-162">Pokud se rozhodnete použít vygenerovaný proxy server, musíte také odkaz k proxy serveru SignalR vygeneruje soubor JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6dafd-162">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="6dafd-163">Následující příklad ukazuje, jak může odkazy vypadat na stránce HTML, který používá vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="6dafd-163">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="6dafd-164">Tyto odkazy musí být zahrnuty v tomto pořadí: jQuery první, poslední SignalR core, po který a proxy SignalR.</span><span class="sxs-lookup"><span data-stu-id="6dafd-164">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="6dafd-165">Jak chcete-li dynamicky vygenerovaný proxy server</span><span class="sxs-lookup"><span data-stu-id="6dafd-165">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="6dafd-166">V předchozím příkladu je odkaz na proxy SignalR generované dynamicky generovaný kód JavaScript, není pro fyzický soubor.</span><span class="sxs-lookup"><span data-stu-id="6dafd-166">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="6dafd-167">SignalR vytvoří kód jazyka JavaScript pro proxy server za chodu a slouží ke klientovi v odpovědi na adresu "/ signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="6dafd-167">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="6dafd-168">Pokud zadáte jiný základní adresu URL pro připojení SignalR na serveru v vaší `MapSignalR` metoda, adresa URL souboru dynamicky vygenerovaný proxy server je vaše vlastní adresu URL s "/ hubs" k němu připojen.</span><span class="sxs-lookup"><span data-stu-id="6dafd-168">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="6dafd-169">U klientů Windows 8 (pro Windows Store) JavaScript použijte místo dynamicky vygenerovaný soubor fyzické proxy.</span><span class="sxs-lookup"><span data-stu-id="6dafd-169">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="6dafd-170">Další informace najdete v tématu [vytvoření fyzického souboru pro funkci SignalR generované proxy](#manualproxy) dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="6dafd-170">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="6dafd-171">V ASP.NET MVC 4 nebo 5 zobrazení syntaxe Razor použijte k odkazování na kořenový adresář aplikace ve vaší odkaz na soubor proxy tilda:</span><span class="sxs-lookup"><span data-stu-id="6dafd-171">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="6dafd-172">Další informace o používání SignalR v MVC 5 v tématu [Začínáme s SignalR a MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="6dafd-172">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="6dafd-173">V zobrazení syntaxe Razor rozhraní ASP.NET MVC 3, použijte `Url.Content` pro vaši informaci, soubor proxy serveru:</span><span class="sxs-lookup"><span data-stu-id="6dafd-173">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="6dafd-174">V aplikaci webových formulářů ASP.NET, použijte `ResolveClientUrl` pro vaše servery proxy odkaz na soubor nebo registrovat prostřednictvím ScriptManager pomocí relativní cesty aplikace kořenové (začíná tildou):</span><span class="sxs-lookup"><span data-stu-id="6dafd-174">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="6dafd-175">Obecně platí použijte stejnou metodu pro určení adresy URL "/ signalr/hubs", který používáte pro soubory šablon stylů CSS a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6dafd-175">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="6dafd-176">Pokud zadáte adresu URL bez použití tildou, v některých scénářích vaše aplikace bude fungovat správně při testování v sadě Visual Studio pomocí služby IIS Express, ale selže s chybou 404 při nasazování do úplnou službu IIS.</span><span class="sxs-lookup"><span data-stu-id="6dafd-176">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="6dafd-177">Další informace najdete v tématu **řešení odkazy na kořenové úrovni prostředky** v [webové servery v sadě Visual Studio pro webové projekty ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="6dafd-177">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="6dafd-178">Při spuštění webového projektu v sadě Visual Studio 2013 v režimu ladění, a pokud používáte Internet Explorer jako prohlížeč, zobrazí se soubor proxy serveru v **Průzkumníku řešení** pod **dokumentů skriptu**, jak je znázorněno Následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="6dafd-178">When you run a web project in Visual Studio 2013 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Soubor vygenerovaný proxy server JavaScript v Průzkumníku řešení](hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="6dafd-180">Chcete-li zobrazit obsah souboru, dvakrát klikněte na **hubs**.</span><span class="sxs-lookup"><span data-stu-id="6dafd-180">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="6dafd-181">Pokud nepoužíváte Visual Studio 2012 nebo 2013 a prohlížeče Internet Explorer, nebo pokud nejsou v režimu ladění, můžete také získat obsah souboru tak, že přejde na adresu "/ signalR/hubs".</span><span class="sxs-lookup"><span data-stu-id="6dafd-181">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="6dafd-182">Například, pokud vaše lokalita běží v `http://localhost:56699`, přejděte na `http://localhost:56699/SignalR/hubs` v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6dafd-182">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="6dafd-183">Postup vytvoření fyzický soubor pro funkci SignalR generované proxy</span><span class="sxs-lookup"><span data-stu-id="6dafd-183">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="6dafd-184">Jako alternativu k dynamicky vygenerovaný proxy server můžete vytvořit fyzický soubor, který má kód proxy a tento soubor.</span><span class="sxs-lookup"><span data-stu-id="6dafd-184">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="6dafd-185">Můžete to provést kontrolu nad ukládání do mezipaměti nebo sdružování chování nebo získat IntelliSense při programování volání metody server.</span><span class="sxs-lookup"><span data-stu-id="6dafd-185">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="6dafd-186">Pokud chcete vytvořit soubor proxy serveru, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6dafd-186">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="6dafd-187">Nainstalujte [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="6dafd-187">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="6dafd-188">Otevřete příkazový řádek a přejděte do *nástroje* složku, která obsahuje soubor SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="6dafd-188">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="6dafd-189">Složky nástroje je v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="6dafd-189">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="6dafd-190">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6dafd-190">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="6dafd-191">Cesta k vaší *.dll* je obvykle *bin* ve složce projektu.</span><span class="sxs-lookup"><span data-stu-id="6dafd-191">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="6dafd-192">Tento příkaz vytvoří soubor s názvem *server.js* ve stejné složce jako *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="6dafd-192">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="6dafd-193">Umístit *server.js* souborů v příslušné složky ve vašem projektu, přejmenujte jej podle potřeby pro vaši aplikaci a přidejte odkaz na jeho místo odkaz na "rozbočovače/signalr".</span><span class="sxs-lookup"><span data-stu-id="6dafd-193">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="6dafd-194">Postup připojení</span><span class="sxs-lookup"><span data-stu-id="6dafd-194">How to establish a connection</span></span>

<span data-ttu-id="6dafd-195">Předtím, než může vytvořit připojení, budete muset vytvořit objekt připojení, vytvořit proxy a registraci obslužných rutin událostí pro metody, které lze volat ze serveru.</span><span class="sxs-lookup"><span data-stu-id="6dafd-195">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="6dafd-196">Pokud jsou nastavení proxy serveru a události obslužných rutin, navázání připojení pomocí volání `start` metoda.</span><span class="sxs-lookup"><span data-stu-id="6dafd-196">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="6dafd-197">Pokud používáte vygenerovaný proxy server, nemáte vytvořit objekt připojení v kódu, protože kód vygenerovaný proxy server provede za vás.</span><span class="sxs-lookup"><span data-stu-id="6dafd-197">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="6dafd-198">**Připojení (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-198">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="6dafd-199">**Připojení (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-199">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="6dafd-200">Ukázkový kód používá výchozí "/ signalr" adresa URL k připojení k službě SignalR.</span><span class="sxs-lookup"><span data-stu-id="6dafd-200">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="6dafd-201">Informace o tom, jak zadejte jinou adresu URL základní najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – adresa URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="6dafd-201">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="6dafd-202">Ve výchozím umístění rozbočovače, je aktuální server; Pokud se připojujete k jinému serveru, zadejte adresu URL před voláním `start` metoda, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="6dafd-202">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="6dafd-203">Za normálních okolností registraci obslužné rutiny událostí před voláním `start` metodu pro vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="6dafd-203">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="6dafd-204">Pokud chcete zaregistrovat některé obslužné rutiny událostí po navázání připojení, můžete to udělat, ale je nutné zaregistrovat alespoň jeden z vaší událostí handler(s) před voláním `start` metoda.</span><span class="sxs-lookup"><span data-stu-id="6dafd-204">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="6dafd-205">Jeden důvodem je to, že může být mnoho Hubs v aplikaci, ale nebude chcete aktivovat `OnConnected` událostí na všechny rozbočovače, chcete-li pouze pomocí jedné z nich.</span><span class="sxs-lookup"><span data-stu-id="6dafd-205">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="6dafd-206">Při připojení, je přítomnost metodu klienta na proxy server rozbočovače pro co informuje SignalR pro aktivaci `OnConnected` událostí.</span><span class="sxs-lookup"><span data-stu-id="6dafd-206">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="6dafd-207">Pokud si nezaregistrujete všechny obslužné rutiny před voláním `start` metoda, bude možné volat metody v rozbočovači, ale rozbočovače na `OnConnected` nebude volána metoda a žádné metody klienta, který bude vyvolán ze serveru.</span><span class="sxs-lookup"><span data-stu-id="6dafd-207">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="6dafd-208">$. connection.hub je stejný objekt vytvoří tento $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="6dafd-208">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="6dafd-209">Jak je vidět v příkladech, při použití vygenerovaný proxy server, `$.connection.hub` odkazuje na objekt připojení.</span><span class="sxs-lookup"><span data-stu-id="6dafd-209">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="6dafd-210">Toto je stejný objekt, který získáte při volání `$.hubConnection()` když nepoužíváte vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="6dafd-210">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="6dafd-211">Kód vygenerovaný proxy server vytvoří připojení můžete spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="6dafd-211">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Vytvoření připojení v souboru vygenerovaný proxy server](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="6dafd-213">Pokud používáte vygenerovaný proxy server, můžete provést cokoli, co se `$.connection.hub` , které můžete provést s objektem připojení po nepoužíváte vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="6dafd-213">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="6dafd-214">Asynchronní provádění metody start</span><span class="sxs-lookup"><span data-stu-id="6dafd-214">Asynchronous execution of the start method</span></span>

<span data-ttu-id="6dafd-215">`start` Asynchronně provede metodu.</span><span class="sxs-lookup"><span data-stu-id="6dafd-215">The `start` method executes asynchronously.</span></span> <span data-ttu-id="6dafd-216">Vrátí [jQuery odložené objekt](http://api.jquery.com/category/deferred-object/), což znamená, že můžete přidat funkce zpětného volání voláním metod `pipe`, `done`, a `fail`.</span><span class="sxs-lookup"><span data-stu-id="6dafd-216">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="6dafd-217">Pokud máte kód, který chcete spustit po připojení, například volání metody server, vložte tento kód funkce zpětného volání nebo volání z funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="6dafd-217">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="6dafd-218">`.done` Metoda zpětného volání je proveden po navázání připojení, a po žádný kód, který budete mít ve vaší `OnConnected` metoda obslužné rutiny událostí na serveru dokončí provádění.</span><span class="sxs-lookup"><span data-stu-id="6dafd-218">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="6dafd-219">Když vložíte příkaz "Teď připojený" z předchozího příkladu jako dalším řádku kódu po `start` volání metody (není v `.done` zpětného volání), `console.log` řádku, budou spuštěny před připojení, jak je znázorněno v následujícím Příklad:</span><span class="sxs-lookup"><span data-stu-id="6dafd-219">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Nesprávný způsob, jak napsat kód, který spouští po navázání připojení](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="6dafd-221">Postup připojení mezi doménami</span><span class="sxs-lookup"><span data-stu-id="6dafd-221">How to establish a cross-domain connection</span></span>

<span data-ttu-id="6dafd-222">Obvykle Pokud prohlížeč načte stránky z `http://contoso.com`, připojení SignalR je ve stejné doméně, v `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="6dafd-222">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="6dafd-223">Pokud stránku z `http://contoso.com` vytvoří připojení k `http://fabrikam.com/signalr`, který je připojení mezi doménami.</span><span class="sxs-lookup"><span data-stu-id="6dafd-223">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="6dafd-224">Z bezpečnostních důvodů jsou ve výchozím nastavení zakázány připojení mezi doménami.</span><span class="sxs-lookup"><span data-stu-id="6dafd-224">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="6dafd-225">V systému SignalR 1.x napříč požadavky domény kontrolovala jeden příznak EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="6dafd-225">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="6dafd-226">Tento příznak řídí požadavky JSONP a CORS.</span><span class="sxs-lookup"><span data-stu-id="6dafd-226">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="6dafd-227">Pro větší flexibilitu, podpora všechny CORS byla odebrána z komponenty serveru SignalR (klientů JavaScript i nadále používat CORS normálně když se zjistí, zda prohlížeč podporuje), a nové middlewaru OWIN, který má je k dispozici pro tyto scénáře podporovat.</span><span class="sxs-lookup"><span data-stu-id="6dafd-227">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="6dafd-228">Pokud JSONP je vyžadována na straně klienta (pro podporu požadavků napříč doménami v prohlížečích starší), ji budou muset být explicitně povoleno nastavením `EnableJSONP` na `HubConfiguration` do objektu `true`, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="6dafd-228">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="6dafd-229">JSONP je standardně zakázána, protože je to méně bezpečné než CORS.</span><span class="sxs-lookup"><span data-stu-id="6dafd-229">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="6dafd-230">**Přidání Microsoft.Owin.Cors do projektu:** k instalaci této knihovny, spusťte následující příkaz v konzole Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="6dafd-230">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="6dafd-231">Tento příkaz přidá 2.1.0 verze balíčku do projektu.</span><span class="sxs-lookup"><span data-stu-id="6dafd-231">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="6dafd-232">Volání metody UseCors</span><span class="sxs-lookup"><span data-stu-id="6dafd-232">Calling UseCors</span></span>

 <span data-ttu-id="6dafd-233">Následující fragment kódu ukazuje, jak implementovat připojení mezi doménami v SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="6dafd-233">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span> 

<span data-ttu-id="6dafd-234">**Implementace žádosti napříč doménami v SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="6dafd-234">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="6dafd-235">Následující kód ukazuje, jak povolit CORS a JSONP v projektu SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="6dafd-235">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="6dafd-236">Tento příklad používá `Map` a `RunSignalR` místo `MapSignalR`, aby se spouštěla CORS middleware jenom pro SignalR požadavky, které vyžadují podporu CORS (nikoli pro všechny přenosy v zadané cestě v `MapSignalR`.) Mapa mohou sloužit také pro veškerý middleware, který je potřeba spustit pro konkrétní předponu adresy URL, nikoli pro celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6dafd-236">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - <span data-ttu-id="6dafd-237">Není nastavený `jQuery.support.cors` na hodnotu true v kódu.</span><span class="sxs-lookup"><span data-stu-id="6dafd-237">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![JQuery.support.cors není nastavený na hodnotu true](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="6dafd-239">SignalR zpracovává použití CORS.</span><span class="sxs-lookup"><span data-stu-id="6dafd-239">SignalR handles the use of CORS.</span></span> <span data-ttu-id="6dafd-240">Nastavení `jQuery.support.cors` na hodnotu true zakáže JSONP, protože způsobuje, že SignalR předpokládat, že je prohlížeč podporuje CORS.</span><span class="sxs-lookup"><span data-stu-id="6dafd-240">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="6dafd-241">Když se připojujete k adresu URL místního hostitele, Internet Explorer 10 nebude považuje za připojení mezi doménami, takže aplikace se pracovat místně aplikace Internet Explorer 10 i v případě, že jste nepovolili napříč doménami připojení na serveru.</span><span class="sxs-lookup"><span data-stu-id="6dafd-241">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="6dafd-242">Informace o používání připojení mezi doménami s aplikace Internet Explorer 9 najdete v tématu [tohoto podprocesu StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="6dafd-242">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="6dafd-243">Informace o používání připojení mezi doménami s Chrome najdete v tématu [tohoto podprocesu StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="6dafd-243">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="6dafd-244">Ukázkový kód používá výchozí "/ signalr" adresa URL k připojení k službě SignalR.</span><span class="sxs-lookup"><span data-stu-id="6dafd-244">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="6dafd-245">Informace o tom, jak zadejte jinou adresu URL základní najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – adresa URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="6dafd-245">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="6dafd-246">Postup konfigurace připojení</span><span class="sxs-lookup"><span data-stu-id="6dafd-246">How to configure the connection</span></span>

<span data-ttu-id="6dafd-247">Před navázáním připojení, můžete určit parametrů řetězce dotazu nebo zadejte metodu přenosu.</span><span class="sxs-lookup"><span data-stu-id="6dafd-247">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="6dafd-248">Určení parametrů řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="6dafd-248">How to specify query string parameters</span></span>

<span data-ttu-id="6dafd-249">Pokud chcete odesílat data na server, když se klient připojí, můžete přidat parametrů řetězce dotazu pro objekt připojení.</span><span class="sxs-lookup"><span data-stu-id="6dafd-249">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="6dafd-250">Následující příklady ukazují, jak nastavit parametr řetězce dotazu v kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="6dafd-250">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="6dafd-251">**Nastavte hodnotu řetězce dotazu před voláním metody start (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-251">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="6dafd-252">**Nastavte hodnotu řetězce dotazu před voláním metody start (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-252">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="6dafd-253">Následující příklad ukazuje, jak číst parametr řetězce dotazu v serverovém kódu.</span><span class="sxs-lookup"><span data-stu-id="6dafd-253">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="6dafd-254">Určení metodu přenosu</span><span class="sxs-lookup"><span data-stu-id="6dafd-254">How to specify the transport method</span></span>

<span data-ttu-id="6dafd-255">Jako součást procesu připojení klienta SignalR normálně vyjedná se serverem určit nejlepší přenos, který je podporován server i klienta.</span><span class="sxs-lookup"><span data-stu-id="6dafd-255">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="6dafd-256">Pokud již víte, které přenosu, kterou chcete použít, můžete tento proces vyjednávání obejít tak, že určíte metodu přenosu při volání `start` metoda.</span><span class="sxs-lookup"><span data-stu-id="6dafd-256">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="6dafd-257">**Kód klienta, který určuje způsob přepravy (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-257">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="6dafd-258">**Kód klienta, který určuje způsob přepravy (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-258">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="6dafd-259">Jako alternativu můžete zadat několik metod přenosu v pořadí, ve kterém chcete SignalR pokusit je:</span><span class="sxs-lookup"><span data-stu-id="6dafd-259">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="6dafd-260">**Kód klienta, který určuje záložní schéma vlastní přenosu (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-260">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="6dafd-261">**Kód klienta, který určuje záložní schématu vlastní přenosu (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-261">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="6dafd-262">Pro zadání metodu přenosu můžete použít následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="6dafd-262">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="6dafd-263">"Websocket"</span><span class="sxs-lookup"><span data-stu-id="6dafd-263">"webSockets"</span></span>
- <span data-ttu-id="6dafd-264">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="6dafd-264">"foreverFrame"</span></span>
- <span data-ttu-id="6dafd-265">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="6dafd-265">"serverSentEvents"</span></span>
- <span data-ttu-id="6dafd-266">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="6dafd-266">"longPolling"</span></span>

<span data-ttu-id="6dafd-267">Následující příklady ukazují, jak zjistit, jakou metodu přenosu používá připojení.</span><span class="sxs-lookup"><span data-stu-id="6dafd-267">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="6dafd-268">**Kód klienta, který zobrazí metodu přenosu používá připojení (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-268">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="6dafd-269">**Kód klienta, který zobrazí metodu přenosu používá připojení (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-269">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="6dafd-270">Informace o tom, jak zkontrolovat metodu přenosu v serverovém kódu najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – jak získat informace o klientovi z vlastností kontextu](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="6dafd-270">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="6dafd-271">Další informace o přenosy a případech přejít najdete v tématu [Úvod do SignalR – přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="6dafd-271">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="6dafd-272">Jak získat proxy server pro třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="6dafd-272">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="6dafd-273">Každý objekt připojení, který vytvoříte zapouzdří informace o připojení pro službu SignalR, která obsahuje jednu nebo více tříd rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="6dafd-273">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="6dafd-274">Ke komunikaci s třídy rozbočovače, pomocí objektu proxy, která můžete vytvořit sami (Pokud nepoužíváte vygenerovaný proxy server) nebo který je pro vás vygeneroval.</span><span class="sxs-lookup"><span data-stu-id="6dafd-274">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="6dafd-275">Název proxy serveru na straně klienta je ve formátu camelCase verzi název třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="6dafd-275">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="6dafd-276">SignalR tuto změnu automaticky provede tak, aby JavaScript konvence může odpovídat kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6dafd-276">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="6dafd-277">**Třídy rozbočovače na serveru**</span><span class="sxs-lookup"><span data-stu-id="6dafd-277">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="6dafd-278">**Získat odkaz na proxy serveru generovaného klienta pro rozbočovač**</span><span class="sxs-lookup"><span data-stu-id="6dafd-278">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="6dafd-279">**Vytvořit proxy server klienta pro třídy rozbočovače (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-279">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="6dafd-280">Pokud uspořádání vaší třídy rozbočovače s `HubName` atribut, použít přesný název bez Změna velikosti písmen.</span><span class="sxs-lookup"><span data-stu-id="6dafd-280">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="6dafd-281">**Třídy rozbočovače na serveru s atributem HubName**</span><span class="sxs-lookup"><span data-stu-id="6dafd-281">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="6dafd-282">**Získat odkaz na proxy serveru generovaného klienta pro rozbočovač**</span><span class="sxs-lookup"><span data-stu-id="6dafd-282">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="6dafd-283">**Vytvořit proxy server klienta pro třídy rozbočovače (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-283">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="6dafd-284">Jak definovat metody na straně klienta, které můžete volat serveru</span><span class="sxs-lookup"><span data-stu-id="6dafd-284">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="6dafd-285">Zadat metodu, která serveru můžete volat z rozbočovače, přidejte obslužnou rutinu události pro proxy server rozbočovače pomocí `client` vlastnost vygenerovaný proxy server nebo volání `on` metoda, pokud nepoužíváte vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="6dafd-285">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="6dafd-286">Parametry lze komplexních objektů.</span><span class="sxs-lookup"><span data-stu-id="6dafd-286">The parameters can be complex objects.</span></span>

<span data-ttu-id="6dafd-287">Přidání obslužné rutiny události před voláním `start` metodu pro vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="6dafd-287">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="6dafd-288">(Pokud chcete přidat obslužné rutiny událostí po volání `start` metodu, najdete v poznámce v [postup připojení](#establishconnection) dříve v tomto dokumentu a použijte syntaxi pro definování metoda bez použití vygenerovaný proxy server.)</span><span class="sxs-lookup"><span data-stu-id="6dafd-288">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="6dafd-289">Metoda shoda názvů nerozlišuje.</span><span class="sxs-lookup"><span data-stu-id="6dafd-289">Method name matching is case-insensitive.</span></span> <span data-ttu-id="6dafd-290">Například `Clients.All.addContosoChatMessageToPage` na serveru, budou spuštěny `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, nebo `addcontosochatmessagetopage` na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="6dafd-290">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="6dafd-291">**Zadejte metodu na klientovi (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-291">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="6dafd-292">**Alternativní způsob, jak definovat metoda na klientovi (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-292">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="6dafd-293">**Definovat metoda na klienta (bez vygenerovaný proxy server, nebo při přidávání po volání metody spuštění)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-293">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="6dafd-294">**Kód serveru, který volá metodu klienta**</span><span class="sxs-lookup"><span data-stu-id="6dafd-294">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="6dafd-295">Následující příklady zahrnují komplexní objekt jako parametr metody.</span><span class="sxs-lookup"><span data-stu-id="6dafd-295">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="6dafd-296">**Zadejte metodu na klienta, který přebírá objekt komplexní (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-296">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="6dafd-297">**Zadejte metodu na klienta, který přebírá objekt komplexní (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-297">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="6dafd-298">**Kód serveru, který definuje komplexní objekt**</span><span class="sxs-lookup"><span data-stu-id="6dafd-298">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="6dafd-299">**Kód serveru, který volá metodu klienta pomocí komplexní objekt**</span><span class="sxs-lookup"><span data-stu-id="6dafd-299">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="6dafd-300">Jak volat metody serveru z klienta</span><span class="sxs-lookup"><span data-stu-id="6dafd-300">How to call server methods from the client</span></span>

<span data-ttu-id="6dafd-301">Chcete-li zavolat metodu serveru z klienta, použijte `server` vlastnost vygenerovaný proxy server nebo `invoke` metodu na proxy server rozbočovače, pokud nepoužíváte vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="6dafd-301">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="6dafd-302">Návratová hodnota nebo parametry můžou být komplexních objektů.</span><span class="sxs-lookup"><span data-stu-id="6dafd-302">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="6dafd-303">Předat ve stylu camelCase verzi název metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="6dafd-303">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="6dafd-304">SignalR tuto změnu automaticky provede tak, aby JavaScript konvence může odpovídat kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6dafd-304">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="6dafd-305">Následující příklady ukazují, jak volat metodu serveru, který nemá návratovou hodnotu a jak volat metodu serveru, která nemá návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6dafd-305">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="6dafd-306">**Metoda serveru s žádný atribut HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="6dafd-306">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="6dafd-307">**Kód serveru, který definuje komplexní objekt předaný parametr**</span><span class="sxs-lookup"><span data-stu-id="6dafd-307">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="6dafd-308">**Kód klienta, která volá metodu serveru (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-308">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="6dafd-309">**Kód klienta, která volá metodu serveru (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-309">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="6dafd-310">Pokud dekorované metody rozbočovače s `HubMethodName` atribut, použijte tento název bez Změna velikosti písmen.</span><span class="sxs-lookup"><span data-stu-id="6dafd-310">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="6dafd-311">**Metoda serveru** s atributem HubMethodName</span><span class="sxs-lookup"><span data-stu-id="6dafd-311">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="6dafd-312">**Kód klienta, která volá metodu serveru (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-312">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="6dafd-313">**Kód klienta, která volá metodu serveru (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-313">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="6dafd-314">Předchozí příklady ukazují, jak volat metodu serveru, který nemá žádnou návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6dafd-314">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="6dafd-315">Následující příklady ukazují, jak volat metodu serveru, který má návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6dafd-315">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="6dafd-316">**Serverový kód pro metodu, která má návratovou hodnotu**</span><span class="sxs-lookup"><span data-stu-id="6dafd-316">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="6dafd-317">**Použitá pro třída Stock** návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="6dafd-317">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="6dafd-318">**Kód klienta, která volá metodu serveru (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-318">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="6dafd-319">**Kód klienta, která volá metodu serveru (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-319">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="6dafd-320">Postupy: zpracování události životnost připojení</span><span class="sxs-lookup"><span data-stu-id="6dafd-320">How to handle connection lifetime events</span></span>

<span data-ttu-id="6dafd-321">Funkce SignalR poskytuje následující připojení životnost události, které může zpracovat:</span><span class="sxs-lookup"><span data-stu-id="6dafd-321">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="6dafd-322">`starting`: Aktivována před odesláním libovolných dat přes připojení.</span><span class="sxs-lookup"><span data-stu-id="6dafd-322">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="6dafd-323">`received`: Vyvolá, když je obdržena žádná data k připojení.</span><span class="sxs-lookup"><span data-stu-id="6dafd-323">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="6dafd-324">Poskytuje přijatá data.</span><span class="sxs-lookup"><span data-stu-id="6dafd-324">Provides the received data.</span></span>
- <span data-ttu-id="6dafd-325">`connectionSlow`: Vyvolá, když klient zjistí pomalé nebo často vyřazení připojení.</span><span class="sxs-lookup"><span data-stu-id="6dafd-325">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="6dafd-326">`reconnecting`: Vyvolá, když základní přenos začne znovu obnovovat.</span><span class="sxs-lookup"><span data-stu-id="6dafd-326">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="6dafd-327">`reconnected`: Vyvolá, když opětovně připojil základní přenos.</span><span class="sxs-lookup"><span data-stu-id="6dafd-327">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="6dafd-328">`stateChanged`: Vyvolána při změně stavu připojení.</span><span class="sxs-lookup"><span data-stu-id="6dafd-328">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="6dafd-329">Poskytuje stav starý a nový stav (připojení, připojeno, Reconnecting nebo odpojeno).</span><span class="sxs-lookup"><span data-stu-id="6dafd-329">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="6dafd-330">`disconnected`: Vyvolá, když připojení byl odpojen.</span><span class="sxs-lookup"><span data-stu-id="6dafd-330">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="6dafd-331">Například, pokud chcete zobrazit upozornění, pokud nejsou potíže s připojením, které by mohly způsobit významnému zpoždění, zpracování `connectionSlow` událostí.</span><span class="sxs-lookup"><span data-stu-id="6dafd-331">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="6dafd-332">**Zpracovat událost connectionSlow (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-332">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="6dafd-333">**Zpracovat událost connectionSlow (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-333">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="6dafd-334">Další informace najdete v tématu [pochopení a zpracování události životnost připojení v systému SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="6dafd-334">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="6dafd-335">Způsob zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="6dafd-335">How to handle errors</span></span>

<span data-ttu-id="6dafd-336">Poskytuje SignalR JavaScript klienta `error` události, které můžete přidat obslužnou rutinu pro.</span><span class="sxs-lookup"><span data-stu-id="6dafd-336">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="6dafd-337">Můžete také použít metodu služeb při selhání pro přidání obslužné rutiny pro chyby, které jsou výsledkem volání metody serveru.</span><span class="sxs-lookup"><span data-stu-id="6dafd-337">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="6dafd-338">Pokud nepovolíte explicitně podrobné chybové zprávy na serveru, obsahuje objekt výjimky, která SignalR vrací po chybě minimální informace o této chybě.</span><span class="sxs-lookup"><span data-stu-id="6dafd-338">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="6dafd-339">Například, pokud volání `newContosoChatMessage` selže, obsahuje chybovou zprávu v objektu chyba "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" podrobné chybové zprávy pro klienty v produkčním prostředí se nedoporučuje z bezpečnostních důvodů, ale pokud chcete povolit podrobné chybové zprávy pro odesílání na serveru pro účely odstraňování potíží, použijte následující kód.</span><span class="sxs-lookup"><span data-stu-id="6dafd-339">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="6dafd-340">Následující příklad ukazuje, jak přidat obslužnou rutinu události chyby.</span><span class="sxs-lookup"><span data-stu-id="6dafd-340">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="6dafd-341">**Přidejte obslužnou rutinu chyby (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-341">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="6dafd-342">**Přidejte obslužnou rutinu chyby (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-342">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="6dafd-343">Následující příklad ukazuje, jak zpracovat chybu z volání metody.</span><span class="sxs-lookup"><span data-stu-id="6dafd-343">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="6dafd-344">**Zpracování chyby z volání metody (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-344">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="6dafd-345">**Zpracování chyby z volání metody (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-345">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="6dafd-346">V případě selhání volání metody `error` také událost se vyvolá, takže kódu v `error` metoda obslužné rutiny a v `.fail` by provést metoda zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="6dafd-346">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="6dafd-347">Jak povolit protokolování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="6dafd-347">How to enable client-side logging</span></span>

<span data-ttu-id="6dafd-348">Chcete-li povolit protokolování na straně klienta na připojení, nastavte `logging` vlastnost u objektu připojení před voláním `start` metodu pro vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="6dafd-348">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="6dafd-349">**Povolit protokolování (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-349">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="6dafd-350">**Povolit protokolování (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="6dafd-350">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="6dafd-351">Pokud chcete zobrazit protokoly, otevřete v prohlížeči na nástroje pro vývojáře a přejděte na kartu konzoly. Kurz, který zobrazuje podrobné pokyny a obrazovky snímky, které ukazují, jak to udělat, najdete v části [Server všesměrového vysílání pomocí funkce Signalr technologie ASP.NET - povolit protokolování](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span><span class="sxs-lookup"><span data-stu-id="6dafd-351">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span></span>
