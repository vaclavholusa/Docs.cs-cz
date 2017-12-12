---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: "Příručka rozhraní API SignalR 1.x centra – klienta JavaScript | Microsoft Docs"
author: pfletcher
description: "Tento dokument obsahuje úvod do používání centra rozhraní API pro SignalR verze 1.1 v klientech JavaScript, například s prohlížeči a Windows Store (WinJS) applic..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 56931827a1a1edf003d2662b2d36964b9b6f3761
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="9fff4-103">Příručka rozhraní API SignalR 1.x centra – JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="9fff4-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="9fff4-104">podle [Patrik Fletcher](https://github.com/pfletcher), [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9fff4-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="9fff4-105">Tento dokument obsahuje úvod do používání centra rozhraní API pro SignalR v klientech JavaScript, například s prohlížeči a aplikace pro Windows Store (WinJS) verze 1.1.</span><span class="sxs-lookup"><span data-stu-id="9fff4-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="9fff4-106">Rozhraní API rozbočovače SignalR umožňuje vytvářet vzdálená volání procedur (RPC) ze serveru pro připojené klienty a z klientů k serveru.</span><span class="sxs-lookup"><span data-stu-id="9fff4-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="9fff4-107">V serverovém kódu můžete definovat metody, které lze volat klienty a volání metody, které běží na klientovi.</span><span class="sxs-lookup"><span data-stu-id="9fff4-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="9fff4-108">V kódu klienta můžete definovat metody, které lze volat ze serveru a volání metody, které běží na serveru.</span><span class="sxs-lookup"><span data-stu-id="9fff4-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="9fff4-109">SignalR se stará o všechny záležitosti klient server pro vás.</span><span class="sxs-lookup"><span data-stu-id="9fff4-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="9fff4-110">Funkce SignalR také nabízí nižší úrovně rozhraní API volat trvalé připojení.</span><span class="sxs-lookup"><span data-stu-id="9fff4-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="9fff4-111">Úvod do SignalR, rozbočovače a trvalé připojení nebo kurz, který ukazuje, jak sestavit kompletní aplikaci SignalR, najdete v části [SignalR – Začínáme](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="9fff4-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="9fff4-112">Přehled</span><span class="sxs-lookup"><span data-stu-id="9fff4-112">Overview</span></span>

<span data-ttu-id="9fff4-113">Tento dokument obsahuje následující části:</span><span class="sxs-lookup"><span data-stu-id="9fff4-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="9fff4-114">Vygenerovaný proxy server a jakým způsobem vám</span><span class="sxs-lookup"><span data-stu-id="9fff4-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="9fff4-115">Kdy použít vygenerovaný proxy server</span><span class="sxs-lookup"><span data-stu-id="9fff4-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="9fff4-116">Instalace klienta</span><span class="sxs-lookup"><span data-stu-id="9fff4-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="9fff4-117">Jak chcete-li dynamicky vygenerovaný proxy server</span><span class="sxs-lookup"><span data-stu-id="9fff4-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="9fff4-118">Postup vytvoření fyzický soubor pro funkci SignalR generované proxy</span><span class="sxs-lookup"><span data-stu-id="9fff4-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="9fff4-119">Postup připojení</span><span class="sxs-lookup"><span data-stu-id="9fff4-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="9fff4-120">$. connection.hub je stejný objekt vytvoří tento $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="9fff4-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="9fff4-121">Asynchronní provádění metody start</span><span class="sxs-lookup"><span data-stu-id="9fff4-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="9fff4-122">Postup připojení mezi doménami</span><span class="sxs-lookup"><span data-stu-id="9fff4-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="9fff4-123">Postup konfigurace připojení</span><span class="sxs-lookup"><span data-stu-id="9fff4-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="9fff4-124">Určení parametrů řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="9fff4-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="9fff4-125">Určení metodu přenosu</span><span class="sxs-lookup"><span data-stu-id="9fff4-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="9fff4-126">Jak získat proxy server pro třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="9fff4-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="9fff4-127">Jak definovat metody na straně klienta, které můžete volat serveru</span><span class="sxs-lookup"><span data-stu-id="9fff4-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="9fff4-128">Jak volat metody serveru z klienta</span><span class="sxs-lookup"><span data-stu-id="9fff4-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="9fff4-129">Postupy: zpracování události životnost připojení</span><span class="sxs-lookup"><span data-stu-id="9fff4-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="9fff4-130">Způsob zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="9fff4-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="9fff4-131">Jak povolit protokolování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="9fff4-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="9fff4-132">Pro dokumentaci o tom, jak program server nebo klientů .NET, naleznete v následujících zdrojích:</span><span class="sxs-lookup"><span data-stu-id="9fff4-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="9fff4-133">Rozhraní API Průvodce pro rozbočovače SignalR – Server</span><span class="sxs-lookup"><span data-stu-id="9fff4-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="9fff4-134">Rozhraní API Průvodce pro rozbočovače SignalR – klient .NET</span><span class="sxs-lookup"><span data-stu-id="9fff4-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="9fff4-135">Odkazy na témata referenční dokumentace rozhraní API jsou na rozhraní .NET 4.5 verzi rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9fff4-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="9fff4-136">Pokud používáte rozhraní .NET 4, přečtěte si téma [verze .NET 4 témat rozhraní API](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="9fff4-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="9fff4-137">Vygenerovaný proxy server a jakým způsobem vám</span><span class="sxs-lookup"><span data-stu-id="9fff4-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="9fff4-138">Můžete naprogramovat JavaScript klienta pro komunikaci se službou SignalR s nebo bez proxy server, který generuje SignalR pro vás.</span><span class="sxs-lookup"><span data-stu-id="9fff4-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="9fff4-139">Proxy vykonává vám je zjednodušení syntaxe kódu můžete použít pro připojení, metody zápisu, které server volá, a volání metody na serveru.</span><span class="sxs-lookup"><span data-stu-id="9fff4-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="9fff4-140">Při psaní kódu pro volání metody server vygenerovaný proxy server umožňuje použijte syntaxi, která vypadá jako kdyby byly provádění místní funkce: můžete napsat `serverMethod(arg1, arg2)` místo `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="9fff4-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="9fff4-141">Syntaxe vygenerovaný proxy server také umožňuje okamžitou a srozumitelné chybě na straně klienta, pokud překlep metoda název serveru.</span><span class="sxs-lookup"><span data-stu-id="9fff4-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="9fff4-142">A pokud ručně vytvoříte soubor, který definuje proxy serverů, můžete také získat podporu technologie IntelliSense pro psaní kódu, který volá metody server.</span><span class="sxs-lookup"><span data-stu-id="9fff4-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="9fff4-143">Předpokládejme například, že máte následující třídy rozbočovače na serveru:</span><span class="sxs-lookup"><span data-stu-id="9fff4-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="9fff4-144">Následující příklady kódu ukazují, co JavaScript kódu vypadá jako pro vyvolání `NewContosoChatMessage` metoda na server a přijímá volání `addContosoChatMessageToPage` metoda ze serveru.</span><span class="sxs-lookup"><span data-stu-id="9fff4-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="9fff4-145">**S vygenerovaný proxy server**</span><span class="sxs-lookup"><span data-stu-id="9fff4-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="9fff4-146">**Bez vygenerovaný proxy server**</span><span class="sxs-lookup"><span data-stu-id="9fff4-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="9fff4-147">Kdy použít vygenerovaný proxy server</span><span class="sxs-lookup"><span data-stu-id="9fff4-147">When to use the generated proxy</span></span>

<span data-ttu-id="9fff4-148">Pokud chcete zaregistrovat více obslužných rutin událostí pro metodu klienta, který volá serveru, nemůžete použít vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="9fff4-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="9fff4-149">Jinak můžete zvolit vygenerovaný proxy server nebo není založen na zúčastníte kódování.</span><span class="sxs-lookup"><span data-stu-id="9fff4-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="9fff4-150">Pokud si zvolíte možnost nepoužít, nemusíte odkazovat na adresu "rozbočovače/signalr" v `script` elementu v kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="9fff4-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="9fff4-151">Instalace klienta</span><span class="sxs-lookup"><span data-stu-id="9fff4-151">Client setup</span></span>

<span data-ttu-id="9fff4-152">Klient JavaScript vyžaduje odkazy na jQuery a soubor JavaScript SignalR core.</span><span class="sxs-lookup"><span data-stu-id="9fff4-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="9fff4-153">JQuery verze musí být 1.6.4 nebo hlavní novější verze, například 1.7.2, 1.8.2 nebo 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="9fff4-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="9fff4-154">Pokud se rozhodnete použít vygenerovaný proxy server, musíte také odkaz k proxy serveru SignalR vygeneruje soubor JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9fff4-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="9fff4-155">Následující příklad ukazuje, jak může odkazy vypadat na stránce HTML, který používá vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="9fff4-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="9fff4-156">Tyto odkazy musí být zahrnuty v tomto pořadí: jQuery první, poslední SignalR core, po který a proxy SignalR.</span><span class="sxs-lookup"><span data-stu-id="9fff4-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="9fff4-157">Jak chcete-li dynamicky vygenerovaný proxy server</span><span class="sxs-lookup"><span data-stu-id="9fff4-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="9fff4-158">V předchozím příkladu je odkaz na proxy SignalR generované dynamicky generovaný kód JavaScript, není pro fyzický soubor.</span><span class="sxs-lookup"><span data-stu-id="9fff4-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="9fff4-159">SignalR vytvoří kód jazyka JavaScript pro proxy server za chodu a slouží ke klientovi v odpovědi na adresu "/ signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="9fff4-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="9fff4-160">Pokud zadáte jiný základní adresu URL pro připojení SignalR na serveru v vaší `MapHubs` metoda, adresa URL souboru dynamicky vygenerovaný proxy server je vaše vlastní adresu URL s "/ hubs" k němu připojen.</span><span class="sxs-lookup"><span data-stu-id="9fff4-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="9fff4-161">U klientů Windows 8 (pro Windows Store) JavaScript použijte místo dynamicky vygenerovaný soubor fyzické proxy.</span><span class="sxs-lookup"><span data-stu-id="9fff4-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="9fff4-162">Další informace najdete v tématu [vytvoření fyzického souboru pro funkci SignalR generované proxy](#manualproxy) dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="9fff4-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="9fff4-163">V zobrazení syntaxe Razor rozhraní ASP.NET MVC 4 použijte k odkazování na kořenový adresář aplikace ve vaší odkaz na soubor proxy tilda:</span><span class="sxs-lookup"><span data-stu-id="9fff4-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="9fff4-164">Další informace o používání SignalR v MVC 4 najdete v tématu [Začínáme s SignalR a MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="9fff4-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="9fff4-165">V zobrazení syntaxe Razor rozhraní ASP.NET MVC 3, použijte `Url.Content` pro vaši informaci, soubor proxy serveru:</span><span class="sxs-lookup"><span data-stu-id="9fff4-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="9fff4-166">V aplikaci webových formulářů ASP.NET, použijte `ResolveClientUrl` pro vaše servery proxy odkaz na soubor nebo registrovat prostřednictvím ScriptManager pomocí relativní cesty aplikace kořenové (začíná tildou):</span><span class="sxs-lookup"><span data-stu-id="9fff4-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="9fff4-167">Obecně platí použijte stejnou metodu pro určení adresy URL "/ signalr/hubs", který používáte pro soubory šablon stylů CSS a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9fff4-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="9fff4-168">Pokud zadáte adresu URL bez použití tildou, v některých scénářích vaše aplikace bude fungovat správně při testování v sadě Visual Studio pomocí služby IIS Express, ale selže s chybou 404 při nasazování do úplnou službu IIS.</span><span class="sxs-lookup"><span data-stu-id="9fff4-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="9fff4-169">Další informace najdete v tématu **řešení odkazy na kořenové úrovni prostředky** v [webové servery v sadě Visual Studio pro webové projekty ASP.NET](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="9fff4-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="9fff4-170">Při spuštění webového projektu v sadě Visual Studio 2012 v režimu ladění, a pokud používáte Internet Explorer jako prohlížeč, zobrazí se soubor proxy serveru v **Průzkumníku řešení** pod **dokumentů skriptu**, jak je znázorněno Následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="9fff4-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Soubor vygenerovaný proxy server JavaScript v Průzkumníku řešení](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="9fff4-172">Chcete-li zobrazit obsah souboru, dvakrát klikněte na **hubs**.</span><span class="sxs-lookup"><span data-stu-id="9fff4-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="9fff4-173">Pokud nepoužíváte sadu Visual Studio 2012 a prohlížeče Internet Explorer, nebo pokud nejsou v režimu ladění, můžete také získat obsah souboru tak, že přejde na adresu "/ signalR/hubs".</span><span class="sxs-lookup"><span data-stu-id="9fff4-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="9fff4-174">Například, pokud vaše lokalita běží v `http://localhost:56699`, přejděte na `http://localhost:56699/SignalR/hubs` v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="9fff4-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="9fff4-175">Postup vytvoření fyzický soubor pro funkci SignalR generované proxy</span><span class="sxs-lookup"><span data-stu-id="9fff4-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="9fff4-176">Jako alternativu k dynamicky vygenerovaný proxy server můžete vytvořit fyzický soubor, který má kód proxy a tento soubor.</span><span class="sxs-lookup"><span data-stu-id="9fff4-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="9fff4-177">Můžete to provést kontrolu nad ukládání do mezipaměti nebo sdružování chování nebo získat IntelliSense při programování volání metody server.</span><span class="sxs-lookup"><span data-stu-id="9fff4-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="9fff4-178">Pokud chcete vytvořit soubor proxy serveru, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9fff4-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="9fff4-179">Nainstalujte [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="9fff4-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="9fff4-180">Otevřete příkazový řádek a přejděte do *nástroje* složku, která obsahuje soubor SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="9fff4-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="9fff4-181">Složky nástroje je v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="9fff4-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="9fff4-182">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9fff4-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="9fff4-183">Cesta k vaší *.dll* je obvykle *bin* ve složce projektu.</span><span class="sxs-lookup"><span data-stu-id="9fff4-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="9fff4-184">Tento příkaz vytvoří soubor s názvem *server.js* ve stejné složce jako *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="9fff4-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="9fff4-185">Umístit *server.js* souborů v příslušné složky ve vašem projektu, přejmenujte jej podle potřeby pro vaši aplikaci a přidejte odkaz na jeho místo odkaz na "rozbočovače/signalr".</span><span class="sxs-lookup"><span data-stu-id="9fff4-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="9fff4-186">Postup připojení</span><span class="sxs-lookup"><span data-stu-id="9fff4-186">How to establish a connection</span></span>

<span data-ttu-id="9fff4-187">Předtím, než může vytvořit připojení, budete muset vytvořit objekt připojení, vytvořit proxy a registraci obslužných rutin událostí pro metody, které lze volat ze serveru.</span><span class="sxs-lookup"><span data-stu-id="9fff4-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="9fff4-188">Pokud jsou nastavení proxy serveru a události obslužných rutin, navázání připojení pomocí volání `start` metoda.</span><span class="sxs-lookup"><span data-stu-id="9fff4-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="9fff4-189">Pokud používáte vygenerovaný proxy server, nemáte vytvořit objekt připojení v kódu, protože kód vygenerovaný proxy server provede za vás.</span><span class="sxs-lookup"><span data-stu-id="9fff4-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="9fff4-190">**Připojení (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="9fff4-191">**Připojení (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="9fff4-192">Ukázkový kód používá výchozí "/ signalr" adresa URL k připojení k službě SignalR.</span><span class="sxs-lookup"><span data-stu-id="9fff4-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="9fff4-193">Informace o tom, jak zadejte jinou adresu URL základní najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – adresa URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="9fff4-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="9fff4-194">Za normálních okolností registraci obslužné rutiny událostí před voláním `start` metodu pro vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="9fff4-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="9fff4-195">Pokud chcete zaregistrovat některé obslužné rutiny událostí po navázání připojení, můžete to udělat, ale je nutné zaregistrovat alespoň jeden z vaší událostí handler(s) před voláním `start` metoda.</span><span class="sxs-lookup"><span data-stu-id="9fff4-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="9fff4-196">Jeden důvodem je to, že může být mnoho Hubs v aplikaci, ale nebude chcete aktivovat `OnConnected` událostí na všechny rozbočovače, chcete-li pouze pomocí jedné z nich.</span><span class="sxs-lookup"><span data-stu-id="9fff4-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="9fff4-197">Při připojení, je přítomnost metodu klienta na proxy server rozbočovače pro co informuje SignalR pro aktivaci `OnConnected` událostí.</span><span class="sxs-lookup"><span data-stu-id="9fff4-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="9fff4-198">Pokud si nezaregistrujete všechny obslužné rutiny před voláním `start` metoda, bude možné volat metody v rozbočovači, ale rozbočovače na `OnConnected` nebude volána metoda a žádné metody klienta, který bude vyvolán ze serveru.</span><span class="sxs-lookup"><span data-stu-id="9fff4-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="9fff4-199">$. connection.hub je stejný objekt vytvoří tento $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="9fff4-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="9fff4-200">Jak je vidět v příkladech, při použití vygenerovaný proxy server, `$.connection.hub` odkazuje na objekt připojení.</span><span class="sxs-lookup"><span data-stu-id="9fff4-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="9fff4-201">Toto je stejný objekt, který získáte při volání `$.hubConnection()` když nepoužíváte vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="9fff4-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="9fff4-202">Kód vygenerovaný proxy server vytvoří připojení můžete spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="9fff4-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Vytvoření připojení v souboru vygenerovaný proxy server](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="9fff4-204">Pokud používáte vygenerovaný proxy server, můžete provést cokoli, co se `$.connection.hub` , které můžete provést s objektem připojení po nepoužíváte vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="9fff4-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="9fff4-205">Asynchronní provádění metody start</span><span class="sxs-lookup"><span data-stu-id="9fff4-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="9fff4-206">`start` Asynchronně provede metodu.</span><span class="sxs-lookup"><span data-stu-id="9fff4-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="9fff4-207">Vrátí [jQuery odložené objekt](http://api.jquery.com/category/deferred-object/), což znamená, že můžete přidat funkce zpětného volání voláním metod `pipe`, `done`, a `fail`.</span><span class="sxs-lookup"><span data-stu-id="9fff4-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="9fff4-208">Pokud máte kód, který chcete spustit po připojení, například volání metody server, vložte tento kód funkce zpětného volání nebo volání z funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="9fff4-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="9fff4-209">`.done` Metoda zpětného volání je proveden po navázání připojení, a po žádný kód, který budete mít ve vaší `OnConnected` metoda obslužné rutiny událostí na serveru dokončí provádění.</span><span class="sxs-lookup"><span data-stu-id="9fff4-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="9fff4-210">Když vložíte příkaz "Teď připojený" z předchozího příkladu jako dalším řádku kódu po `start` volání metody (není v `.done` zpětného volání), `console.log` řádku, budou spuštěny před připojení, jak je znázorněno v následujícím Příklad:</span><span class="sxs-lookup"><span data-stu-id="9fff4-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Nesprávný způsob, jak napsat kód, který spouští po navázání připojení](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="9fff4-212">Postup připojení mezi doménami</span><span class="sxs-lookup"><span data-stu-id="9fff4-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="9fff4-213">Obvykle Pokud prohlížeč načte stránky z `http://contoso.com`, připojení SignalR je ve stejné doméně, v `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="9fff4-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="9fff4-214">Pokud stránku z `http://contoso.com` vytvoří připojení k `http://fabrikam.com/signalr`, který je připojení mezi doménami.</span><span class="sxs-lookup"><span data-stu-id="9fff4-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="9fff4-215">Z bezpečnostních důvodů jsou ve výchozím nastavení zakázány připojení mezi doménami.</span><span class="sxs-lookup"><span data-stu-id="9fff4-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="9fff4-216">Navázat připojení mezi doménami, ujistěte se, že jsou na serveru povoleno připojení mezi doménami a zadejte adresu URL připojení, když vytvoříte objekt připojení.</span><span class="sxs-lookup"><span data-stu-id="9fff4-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="9fff4-217">SignalR bude odpovídající technologii použít pro připojení mezi doménami, například [JSONP](http://en.wikipedia.org/wiki/JSONP) nebo [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="9fff4-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="9fff4-218">Na serveru povolit připojení mezi doménami tak, že vyberete tuto možnost, při volání `MapHubs` metoda.</span><span class="sxs-lookup"><span data-stu-id="9fff4-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="9fff4-219">Na klientovi zadejte adresu URL, když vytvoříte objekt připojení (bez vygenerovaný proxy server), nebo před voláním metody start (s vygenerovaný proxy server).</span><span class="sxs-lookup"><span data-stu-id="9fff4-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="9fff4-220">**Kód klienta, který určuje připojení mezi doménami (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="9fff4-221">**Kód klienta, který určuje připojení mezi doménami (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="9fff4-222">Při použití `$.hubConnection` konstruktoru, nemusíte zahrnovat `signalr` v adrese URL je automaticky přidán (Pokud nezadáte `useDefaultUrl` jako `false`).</span><span class="sxs-lookup"><span data-stu-id="9fff4-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="9fff4-223">Můžete vytvořit více připojení k různým koncovým bodům.</span><span class="sxs-lookup"><span data-stu-id="9fff4-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="9fff4-224">Není nastavený `jQuery.support.cors` na hodnotu true v kódu.</span><span class="sxs-lookup"><span data-stu-id="9fff4-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![JQuery.support.cors není nastavený na hodnotu true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="9fff4-226">SignalR zpracovává používání JSONP nebo CORS.</span><span class="sxs-lookup"><span data-stu-id="9fff4-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="9fff4-227">Nastavení `jQuery.support.cors` na hodnotu true zakáže JSONP, protože způsobuje, že SignalR předpokládat, že je prohlížeč podporuje CORS.</span><span class="sxs-lookup"><span data-stu-id="9fff4-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="9fff4-228">Když se připojujete k adresu URL místního hostitele, Internet Explorer 10 nebude považuje za připojení mezi doménami, takže aplikace se pracovat místně aplikace Internet Explorer 10 i v případě, že jste nepovolili napříč doménami připojení na serveru.</span><span class="sxs-lookup"><span data-stu-id="9fff4-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="9fff4-229">Informace o používání připojení mezi doménami s aplikace Internet Explorer 9 najdete v tématu [tohoto podprocesu StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="9fff4-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="9fff4-230">Informace o používání připojení mezi doménami s Chrome najdete v tématu [tohoto podprocesu StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="9fff4-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="9fff4-231">Ukázkový kód používá výchozí "/ signalr" adresa URL k připojení k službě SignalR.</span><span class="sxs-lookup"><span data-stu-id="9fff4-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="9fff4-232">Informace o tom, jak zadejte jinou adresu URL základní najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – adresa URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="9fff4-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="9fff4-233">Postup konfigurace připojení</span><span class="sxs-lookup"><span data-stu-id="9fff4-233">How to configure the connection</span></span>

<span data-ttu-id="9fff4-234">Před navázáním připojení, můžete určit parametrů řetězce dotazu nebo zadejte metodu přenosu.</span><span class="sxs-lookup"><span data-stu-id="9fff4-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="9fff4-235">Určení parametrů řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="9fff4-235">How to specify query string parameters</span></span>

<span data-ttu-id="9fff4-236">Pokud chcete odesílat data na server, když se klient připojí, můžete přidat parametrů řetězce dotazu pro objekt připojení.</span><span class="sxs-lookup"><span data-stu-id="9fff4-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="9fff4-237">Následující příklady ukazují, jak nastavit parametr řetězce dotazu v kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="9fff4-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="9fff4-238">**Nastavte hodnotu řetězce dotazu před voláním metody start (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="9fff4-239">**Nastavte hodnotu řetězce dotazu před voláním metody start (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="9fff4-240">Následující příklad ukazuje, jak číst parametr řetězce dotazu v serverovém kódu.</span><span class="sxs-lookup"><span data-stu-id="9fff4-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="9fff4-241">Určení metodu přenosu</span><span class="sxs-lookup"><span data-stu-id="9fff4-241">How to specify the transport method</span></span>

<span data-ttu-id="9fff4-242">Jako součást procesu připojení klienta SignalR normálně vyjedná se serverem určit nejlepší přenos, který je podporován server i klienta.</span><span class="sxs-lookup"><span data-stu-id="9fff4-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="9fff4-243">Pokud již víte, které přenosu, kterou chcete použít, můžete tento proces vyjednávání obejít tak, že určíte metodu přenosu při volání `start` metoda.</span><span class="sxs-lookup"><span data-stu-id="9fff4-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="9fff4-244">**Kód klienta, který určuje způsob přepravy (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="9fff4-245">**Kód klienta, který určuje způsob přepravy (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="9fff4-246">Jako alternativu můžete zadat několik metod přenosu v pořadí, ve kterém chcete SignalR pokusit je:</span><span class="sxs-lookup"><span data-stu-id="9fff4-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="9fff4-247">**Kód klienta, který určuje záložní schéma vlastní přenosu (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="9fff4-248">**Kód klienta, který určuje záložní schématu vlastní přenosu (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="9fff4-249">Pro zadání metodu přenosu můžete použít následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="9fff4-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="9fff4-250">"Websocket"</span><span class="sxs-lookup"><span data-stu-id="9fff4-250">"webSockets"</span></span>
- <span data-ttu-id="9fff4-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="9fff4-251">"foreverFrame"</span></span>
- <span data-ttu-id="9fff4-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="9fff4-252">"serverSentEvents"</span></span>
- <span data-ttu-id="9fff4-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="9fff4-253">"longPolling"</span></span>

<span data-ttu-id="9fff4-254">Následující příklady ukazují, jak zjistit, jakou metodu přenosu používá připojení.</span><span class="sxs-lookup"><span data-stu-id="9fff4-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="9fff4-255">**Kód klienta, který zobrazí metodu přenosu používá připojení (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="9fff4-256">**Kód klienta, který zobrazí metodu přenosu používá připojení (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="9fff4-257">Informace o tom, jak zkontrolovat metodu přenosu v serverovém kódu najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – jak získat informace o klientovi z vlastností kontextu](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="9fff4-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="9fff4-258">Další informace o přenosy a případech přejít najdete v tématu [Úvod do SignalR – přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="9fff4-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="9fff4-259">Jak získat proxy server pro třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="9fff4-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="9fff4-260">Každý objekt připojení, který vytvoříte zapouzdří informace o připojení pro službu SignalR, která obsahuje jednu nebo více tříd rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9fff4-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="9fff4-261">Ke komunikaci s třídy rozbočovače, pomocí objektu proxy, která můžete vytvořit sami (Pokud nepoužíváte vygenerovaný proxy server) nebo který je pro vás vygeneroval.</span><span class="sxs-lookup"><span data-stu-id="9fff4-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="9fff4-262">Název proxy serveru na straně klienta je ve formátu camelCase verzi název třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9fff4-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="9fff4-263">SignalR tuto změnu automaticky provede tak, aby JavaScript konvence může odpovídat kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9fff4-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="9fff4-264">**Třídy rozbočovače na serveru**</span><span class="sxs-lookup"><span data-stu-id="9fff4-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="9fff4-265">**Získat odkaz na proxy serveru generovaného klienta pro rozbočovač**</span><span class="sxs-lookup"><span data-stu-id="9fff4-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="9fff4-266">**Vytvořit proxy server klienta pro třídy rozbočovače (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="9fff4-267">Pokud uspořádání vaší třídy rozbočovače s `HubName` atribut, použít přesný název bez Změna velikosti písmen.</span><span class="sxs-lookup"><span data-stu-id="9fff4-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="9fff4-268">**Třídy rozbočovače na serveru s atributem HubName**</span><span class="sxs-lookup"><span data-stu-id="9fff4-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="9fff4-269">**Získat odkaz na proxy serveru generovaného klienta pro rozbočovač**</span><span class="sxs-lookup"><span data-stu-id="9fff4-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="9fff4-270">**Vytvořit proxy server klienta pro třídy rozbočovače (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="9fff4-271">Jak definovat metody na straně klienta, které můžete volat serveru</span><span class="sxs-lookup"><span data-stu-id="9fff4-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="9fff4-272">Zadat metodu, která serveru můžete volat z rozbočovače, přidejte obslužnou rutinu události pro proxy server rozbočovače pomocí `client` vlastnost vygenerovaný proxy server nebo volání `on` metoda, pokud nepoužíváte vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="9fff4-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="9fff4-273">Parametry lze komplexních objektů.</span><span class="sxs-lookup"><span data-stu-id="9fff4-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="9fff4-274">Přidání obslužné rutiny události před voláním `start` metodu pro vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="9fff4-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="9fff4-275">(Pokud chcete přidat obslužné rutiny událostí po volání `start` metodu, najdete v poznámce v [postup připojení](#establishconnection) dříve v tomto dokumentu a použijte syntaxi pro definování metoda bez použití vygenerovaný proxy server.)</span><span class="sxs-lookup"><span data-stu-id="9fff4-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="9fff4-276">Metoda shoda názvů nerozlišuje.</span><span class="sxs-lookup"><span data-stu-id="9fff4-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="9fff4-277">Například `Clients.All.addContosoChatMessageToPage` na serveru, budou spuštěny `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, nebo `addcontosochatmessagetopage` na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="9fff4-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="9fff4-278">**Zadejte metodu na klientovi (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="9fff4-279">**Alternativní způsob, jak definovat metoda na klientovi (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="9fff4-280">**Definovat metoda na klienta (bez vygenerovaný proxy server, nebo při přidávání po volání metody spuštění)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="9fff4-281">**Kód serveru, který volá metodu klienta**</span><span class="sxs-lookup"><span data-stu-id="9fff4-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="9fff4-282">Následující příklady zahrnují komplexní objekt jako parametr metody.</span><span class="sxs-lookup"><span data-stu-id="9fff4-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="9fff4-283">**Zadejte metodu na klienta, který přebírá objekt komplexní (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="9fff4-284">**Zadejte metodu na klienta, který přebírá objekt komplexní (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="9fff4-285">**Kód serveru, který definuje komplexní objekt**</span><span class="sxs-lookup"><span data-stu-id="9fff4-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="9fff4-286">**Kód serveru, který volá metodu klienta pomocí komplexní objekt**</span><span class="sxs-lookup"><span data-stu-id="9fff4-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="9fff4-287">Jak volat metody serveru z klienta</span><span class="sxs-lookup"><span data-stu-id="9fff4-287">How to call server methods from the client</span></span>

<span data-ttu-id="9fff4-288">Chcete-li zavolat metodu serveru z klienta, použijte `server` vlastnost vygenerovaný proxy server nebo `invoke` metodu na proxy server rozbočovače, pokud nepoužíváte vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="9fff4-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="9fff4-289">Návratová hodnota nebo parametry můžou být komplexních objektů.</span><span class="sxs-lookup"><span data-stu-id="9fff4-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="9fff4-290">Předat ve stylu camelCase verzi název metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9fff4-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="9fff4-291">SignalR tuto změnu automaticky provede tak, aby JavaScript konvence může odpovídat kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9fff4-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="9fff4-292">Následující příklady ukazují, jak volat metodu serveru, který nemá návratovou hodnotu a jak volat metodu serveru, která nemá návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9fff4-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="9fff4-293">**Metoda serveru s žádný atribut HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="9fff4-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="9fff4-294">**Kód serveru, který definuje komplexní objekt předaný parametr**</span><span class="sxs-lookup"><span data-stu-id="9fff4-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="9fff4-295">**Kód klienta, která volá metodu serveru (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="9fff4-296">**Kód klienta, která volá metodu serveru (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="9fff4-297">Pokud dekorované metody rozbočovače s `HubMethodName` atribut, použijte tento název bez Změna velikosti písmen.</span><span class="sxs-lookup"><span data-stu-id="9fff4-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="9fff4-298">**Metoda serveru** s atributem HubMethodName</span><span class="sxs-lookup"><span data-stu-id="9fff4-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="9fff4-299">**Kód klienta, která volá metodu serveru (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="9fff4-300">**Kód klienta, která volá metodu serveru (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="9fff4-301">Předchozí příklady ukazují, jak volat metodu serveru, který nemá žádnou návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9fff4-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="9fff4-302">Následující příklady ukazují, jak volat metodu serveru, který má návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9fff4-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="9fff4-303">**Serverový kód pro metodu, která má návratovou hodnotu**</span><span class="sxs-lookup"><span data-stu-id="9fff4-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="9fff4-304">**Použitá pro třída Stock** návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="9fff4-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="9fff4-305">**Kód klienta, která volá metodu serveru (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="9fff4-306">**Kód klienta, která volá metodu serveru (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="9fff4-307">Postupy: zpracování události životnost připojení</span><span class="sxs-lookup"><span data-stu-id="9fff4-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="9fff4-308">Funkce SignalR poskytuje následující připojení životnost události, které může zpracovat:</span><span class="sxs-lookup"><span data-stu-id="9fff4-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="9fff4-309">`starting`: Aktivována před odesláním libovolných dat přes připojení.</span><span class="sxs-lookup"><span data-stu-id="9fff4-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="9fff4-310">`received`: Vyvolá, když je obdržena žádná data k připojení.</span><span class="sxs-lookup"><span data-stu-id="9fff4-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="9fff4-311">Poskytuje přijatá data.</span><span class="sxs-lookup"><span data-stu-id="9fff4-311">Provides the received data.</span></span>
- <span data-ttu-id="9fff4-312">`connectionSlow`: Vyvolá, když klient zjistí pomalé nebo často vyřazení připojení.</span><span class="sxs-lookup"><span data-stu-id="9fff4-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="9fff4-313">`reconnecting`: Vyvolá, když základní přenos začne znovu obnovovat.</span><span class="sxs-lookup"><span data-stu-id="9fff4-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="9fff4-314">`reconnected`: Vyvolá, když opětovně připojil základní přenos.</span><span class="sxs-lookup"><span data-stu-id="9fff4-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="9fff4-315">`stateChanged`: Vyvolána při změně stavu připojení.</span><span class="sxs-lookup"><span data-stu-id="9fff4-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="9fff4-316">Poskytuje stav starý a nový stav (připojení, připojeno, Reconnecting nebo odpojeno).</span><span class="sxs-lookup"><span data-stu-id="9fff4-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="9fff4-317">`disconnected`: Vyvolá, když připojení byl odpojen.</span><span class="sxs-lookup"><span data-stu-id="9fff4-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="9fff4-318">Například, pokud chcete zobrazit upozornění, pokud nejsou potíže s připojením, které by mohly způsobit významnému zpoždění, zpracování `connectionSlow` událostí.</span><span class="sxs-lookup"><span data-stu-id="9fff4-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="9fff4-319">**Zpracovat událost connectionSlow (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="9fff4-320">**Zpracovat událost connectionSlow (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="9fff4-321">Další informace najdete v tématu [pochopení a zpracování události životnost připojení v systému SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="9fff4-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="9fff4-322">Způsob zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="9fff4-322">How to handle errors</span></span>

<span data-ttu-id="9fff4-323">Poskytuje SignalR JavaScript klienta `error` události, které můžete přidat obslužnou rutinu pro.</span><span class="sxs-lookup"><span data-stu-id="9fff4-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="9fff4-324">Můžete také použít metodu služeb při selhání pro přidání obslužné rutiny pro chyby, které jsou výsledkem volání metody serveru.</span><span class="sxs-lookup"><span data-stu-id="9fff4-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="9fff4-325">Pokud nepovolíte explicitně podrobné chybové zprávy na serveru, obsahuje objekt výjimky, která SignalR vrací po chybě minimální informace o této chybě.</span><span class="sxs-lookup"><span data-stu-id="9fff4-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="9fff4-326">Například, pokud volání `newContosoChatMessage` selže, obsahuje chybovou zprávu v objektu chyba "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" podrobné chybové zprávy pro klienty v produkčním prostředí se nedoporučuje z bezpečnostních důvodů, ale pokud chcete povolit podrobné chybové zprávy pro odesílání na serveru pro účely odstraňování potíží, použijte následující kód.</span><span class="sxs-lookup"><span data-stu-id="9fff4-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="9fff4-327">Následující příklad ukazuje, jak přidat obslužnou rutinu události chyby.</span><span class="sxs-lookup"><span data-stu-id="9fff4-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="9fff4-328">**Přidejte obslužnou rutinu chyby (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="9fff4-329">**Přidejte obslužnou rutinu chyby (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="9fff4-330">Následující příklad ukazuje, jak zpracovat chybu z volání metody.</span><span class="sxs-lookup"><span data-stu-id="9fff4-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="9fff4-331">**Zpracování chyby z volání metody (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="9fff4-332">**Zpracování chyby z volání metody (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="9fff4-333">V případě selhání volání metody `error` také událost se vyvolá, takže kódu v `error` metoda obslužné rutiny a v `.fail` by provést metoda zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="9fff4-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="9fff4-334">Jak povolit protokolování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="9fff4-334">How to enable client-side logging</span></span>

<span data-ttu-id="9fff4-335">Chcete-li povolit protokolování na straně klienta na připojení, nastavte `logging` vlastnost u objektu připojení před voláním `start` metodu pro vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="9fff4-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="9fff4-336">**Povolit protokolování (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="9fff4-337">**Povolit protokolování (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="9fff4-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="9fff4-338">Pokud chcete zobrazit protokoly, otevřete v prohlížeči na nástroje pro vývojáře a přejděte na kartu konzoly. Kurz, který zobrazuje podrobné pokyny a obrazovky snímky, které ukazují, jak to udělat, najdete v části [Server všesměrového vysílání pomocí funkce Signalr technologie ASP.NET - povolit protokolování](index.md).</span><span class="sxs-lookup"><span data-stu-id="9fff4-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
