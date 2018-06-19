---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Funkce SignalR technologie ASP.NET centra API Průvodce – Server (C#) | Microsoft Docs
author: pfletcher
description: Tento dokument obsahuje úvod do programování na straně serveru rozhraní API ASP.NET SignalR centra pro SignalR verze 2, s ukázky kódu demonstraci...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c2567d4d39a494daf77a23db5dff83c8fae4925d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28039206"
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="b548b-103">Funkce SignalR technologie ASP.NET centra API Průvodce – Server (C#)</span><span class="sxs-lookup"><span data-stu-id="b548b-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>
====================
<span data-ttu-id="b548b-104">podle [Patrik Fletcher](https://github.com/pfletcher), [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b548b-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="b548b-105">Tento dokument obsahuje úvod do programování na straně serveru rozhraní API ASP.NET SignalR centra pro SignalR verze 2, s ukázky kódu demonstraci běžné možnosti.</span><span class="sxs-lookup"><span data-stu-id="b548b-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="b548b-106">Rozhraní API rozbočovače SignalR umožňuje vytvářet vzdálená volání procedur (RPC) ze serveru pro připojené klienty a z klientů k serveru.</span><span class="sxs-lookup"><span data-stu-id="b548b-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="b548b-107">V serverovém kódu můžete definovat metody, které lze volat klienty a volání metody, které běží na klientovi.</span><span class="sxs-lookup"><span data-stu-id="b548b-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="b548b-108">V kódu klienta můžete definovat metody, které lze volat ze serveru a volání metody, které běží na serveru.</span><span class="sxs-lookup"><span data-stu-id="b548b-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="b548b-109">SignalR se stará o všechny záležitosti klient server pro vás.</span><span class="sxs-lookup"><span data-stu-id="b548b-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="b548b-110">Funkce SignalR také nabízí nižší úrovně rozhraní API volat trvalé připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="b548b-111">Úvod do SignalR, rozbočovače a trvalé připojení, najdete v části [Úvod do SignalR 2](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="b548b-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="b548b-112">Verze softwaru použitým v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="b548b-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="b548b-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b548b-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="b548b-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b548b-114">.NET 4.5</span></span>
> - <span data-ttu-id="b548b-115">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="b548b-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="b548b-116">Téma verze</span><span class="sxs-lookup"><span data-stu-id="b548b-116">Topic versions</span></span>
> 
> <span data-ttu-id="b548b-117">Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="b548b-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="b548b-118">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="b548b-118">Questions and comments</span></span>
> 
> <span data-ttu-id="b548b-119">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="b548b-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b548b-120">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b548b-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="b548b-121">Přehled</span><span class="sxs-lookup"><span data-stu-id="b548b-121">Overview</span></span>

<span data-ttu-id="b548b-122">Tento dokument obsahuje následující části:</span><span class="sxs-lookup"><span data-stu-id="b548b-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="b548b-123">Postup registrace middlewaru SignalR.</span><span class="sxs-lookup"><span data-stu-id="b548b-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="b548b-124">Adresa URL /signalr</span><span class="sxs-lookup"><span data-stu-id="b548b-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="b548b-125">Konfigurace možností SignalR</span><span class="sxs-lookup"><span data-stu-id="b548b-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="b548b-126">Postup vytvoření a používání třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="b548b-127">Doba života objektu rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="b548b-128">Formátu camelCase-velká a malá písmena rozbočovače názvů klientů JavaScript</span><span class="sxs-lookup"><span data-stu-id="b548b-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="b548b-129">Více rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="b548b-130">Silného typu rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="b548b-131">Jak definovat metody ve třídě rozbočovače, která můžete volat klientů</span><span class="sxs-lookup"><span data-stu-id="b548b-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="b548b-132">Formátu camelCase-velká a malá písmena metoda názvů klientů JavaScript</span><span class="sxs-lookup"><span data-stu-id="b548b-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="b548b-133">Při spuštění asynchronně</span><span class="sxs-lookup"><span data-stu-id="b548b-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="b548b-134">Definování přetížení</span><span class="sxs-lookup"><span data-stu-id="b548b-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="b548b-135">Vytvoření sestavy průběhu z volání metod rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="b548b-136">Jak volat metody klienta z třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="b548b-137">Výběr klienty, kteří obdrží vzdálené volání Procedur</span><span class="sxs-lookup"><span data-stu-id="b548b-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="b548b-138">Žádné ověření kompilace pro názvy – metoda</span><span class="sxs-lookup"><span data-stu-id="b548b-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="b548b-139">S odpovídajícím názvem velká a malá písmena – metoda</span><span class="sxs-lookup"><span data-stu-id="b548b-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="b548b-140">Asynchronního spuštění</span><span class="sxs-lookup"><span data-stu-id="b548b-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="b548b-141">Správa členství ve skupině z třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="b548b-142">Asynchronní provádění metody přidat a odebrat</span><span class="sxs-lookup"><span data-stu-id="b548b-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="b548b-143">Trvalost členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="b548b-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="b548b-144">Skupiny jednoho uživatele</span><span class="sxs-lookup"><span data-stu-id="b548b-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="b548b-145">Postupy: zpracování události životnost připojení v třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="b548b-146">Kdy jsou volány onconnected rozbočovače, ondisconnected rozbočovače a onreconnected rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="b548b-147">Stav volajícího není naplněno.</span><span class="sxs-lookup"><span data-stu-id="b548b-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="b548b-148">Jak získat informace o klientovi z vlastností kontextu</span><span class="sxs-lookup"><span data-stu-id="b548b-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="b548b-149">Jak předat stavu mezi klienty a třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="b548b-150">Postupy: zpracování chyb v třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="b548b-151">Tom, jak volat metody klienta a Správa skupin z mimo třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="b548b-152">Volání metody klienta</span><span class="sxs-lookup"><span data-stu-id="b548b-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="b548b-153">Správa členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="b548b-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="b548b-154">Postup povolení trasování</span><span class="sxs-lookup"><span data-stu-id="b548b-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="b548b-155">Postup přizpůsobení kanálu rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="b548b-156">Dokumentaci k programu klientů najdete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="b548b-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="b548b-157">Rozhraní API Průvodce pro rozbočovače SignalR – JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="b548b-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="b548b-158">Rozhraní API Průvodce pro rozbočovače SignalR – klient .NET</span><span class="sxs-lookup"><span data-stu-id="b548b-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="b548b-159">Součásti serveru pro funkci SignalR 2 jsou dostupné jenom v rozhraní .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="b548b-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="b548b-160">Servery se systémem .NET 4.0, musíte použít SignalR v1.x.</span><span class="sxs-lookup"><span data-stu-id="b548b-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="b548b-161">Postup registrace middlewaru SignalR.</span><span class="sxs-lookup"><span data-stu-id="b548b-161">How to register SignalR middleware</span></span>

<span data-ttu-id="b548b-162">Chcete-li definovat trasy, která budou klienti používat pro připojení do vašeho centra, volejte `MapSignalR` metoda při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b548b-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="b548b-163">`MapSignalR`je [metoda rozšíření](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) pro `OwinExtensions` třídy.</span><span class="sxs-lookup"><span data-stu-id="b548b-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="b548b-164">Následující příklad ukazuje, jak definovat trasu rozbočovače SignalR pomocí třídy pro spuštění OWIN.</span><span class="sxs-lookup"><span data-stu-id="b548b-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="b548b-165">Pokud přidáváte funkce SignalR pro aplikaci ASP.NET MVC, ujistěte se, zda SignalR trasy, která je přidána před dalším postupům.</span><span class="sxs-lookup"><span data-stu-id="b548b-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="b548b-166">Další informace najdete v tématu [kurz: Začínáme s SignalR 2 a MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="b548b-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="b548b-167">Adresa URL /signalr</span><span class="sxs-lookup"><span data-stu-id="b548b-167">The /signalr URL</span></span>

<span data-ttu-id="b548b-168">Ve výchozím nastavení, je adresa URL trasy, který budou klienti používat pro připojení do vašeho centra "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="b548b-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="b548b-169">(Nepleťte si tuto adresu URL s adresou URL "/ signalr/hubs", což je pro automaticky generované soubor JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b548b-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="b548b-170">Další informace o vygenerovaný proxy server, najdete v části [průvodce rozhraní API rozbočovače SignalR - JavaScript klienta - vygenerovaný proxy server a jakým způsobem vám](hubs-api-guide-javascript-client.md#genproxy).)</span><span class="sxs-lookup"><span data-stu-id="b548b-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="b548b-171">Může být výjimečných případech, které bylo není použít tuto základní adresu URL pro SignalR; například máte složku v projektu s názvem *signalr* a nechcete, aby změna názvu.</span><span class="sxs-lookup"><span data-stu-id="b548b-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="b548b-172">V takovém případě můžete změnit základní adresu URL, jak je znázorněno v následujících příkladech (nahraďte "/ signalr" v ukázkovém kódu s požadovanou adresu URL).</span><span class="sxs-lookup"><span data-stu-id="b548b-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="b548b-173">**Kód serveru, který určuje adresu URL**</span><span class="sxs-lookup"><span data-stu-id="b548b-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="b548b-174">**Kód jazyka JavaScript klienta, který určuje adresu URL (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="b548b-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="b548b-175">**Kód jazyka JavaScript klienta, který určuje adresu URL (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="b548b-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="b548b-176">**Kód klienta rozhraní .NET, který určuje adresu URL**</span><span class="sxs-lookup"><span data-stu-id="b548b-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="b548b-177">Konfigurace možností SignalR</span><span class="sxs-lookup"><span data-stu-id="b548b-177">Configuring SignalR Options</span></span>

<span data-ttu-id="b548b-178">Přetížení `MapSignalR` metoda umožňují zadejte vlastní adresu URL, překladač závislostí vlastní a následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="b548b-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="b548b-179">Povolte volání mezi doménami pomocí CORS nebo JSONP z prohlížečů klientů.</span><span class="sxs-lookup"><span data-stu-id="b548b-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="b548b-180">Obvykle Pokud prohlížeč načte stránky z `http://contoso.com`, připojení SignalR je ve stejné doméně, v `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="b548b-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="b548b-181">Pokud stránku z `http://contoso.com` vytvoří připojení k `http://fabrikam.com/signalr`, který je připojení mezi doménami.</span><span class="sxs-lookup"><span data-stu-id="b548b-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="b548b-182">Z bezpečnostních důvodů jsou ve výchozím nastavení zakázány připojení mezi doménami.</span><span class="sxs-lookup"><span data-stu-id="b548b-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="b548b-183">Další informace najdete v tématu [ASP.NET SignalR centra API Průvodce - JavaScript klienta - postup připojení mezi doménami](hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="b548b-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="b548b-184">Povolte podrobné chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="b548b-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="b548b-185">Pokud dojde k chybám, je výchozí chování SignalR klientům odeslat zprávu oznámení bez podrobnosti o co se stalo.</span><span class="sxs-lookup"><span data-stu-id="b548b-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="b548b-186">Odesílání podrobné informace o chybě klientům se nedoporučuje v produkčním prostředí, protože uživatelé se zlými úmysly pravděpodobně moci použijte informace v útoky na vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b548b-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="b548b-187">Řešení potíží, můžete použít tuto možnost dočasně povolit informativnější zasílání zpráv o chybách.</span><span class="sxs-lookup"><span data-stu-id="b548b-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="b548b-188">Zakažte automaticky generované soubory proxy server JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b548b-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="b548b-189">Ve výchozím nastavení je soubor s proxy JavaScript pro třídy rozbočovače vygenerované v reakci na adresu "/ signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="b548b-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="b548b-190">Pokud nechcete použít třídy proxy JavaScript, nebo pokud chcete generovat ručně tento soubor a odkazovat na fyzický soubor v vašim klientům, můžete tuto možnost zakažte generování proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="b548b-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="b548b-191">Další informace najdete v tématu [průvodce rozhraní API rozbočovače SignalR - JavaScript klienta - vytvoření fyzického souboru pro funkci SignalR generované proxy](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="b548b-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="b548b-192">Následující příklad ukazuje, jak určit adresu URL připojení SignalR a tyto možnosti ve volání `MapSignalR` metoda.</span><span class="sxs-lookup"><span data-stu-id="b548b-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="b548b-193">Chcete-li zadat vlastní adresu URL, nahraďte "/ signalr" v příkladu s adresou URL, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="b548b-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="b548b-194">Postup vytvoření a používání třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-194">How to create and use Hub classes</span></span>

<span data-ttu-id="b548b-195">K vytvoření rozbočovač, vytvořte třídu, která je odvozena z [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="b548b-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="b548b-196">Následující příklad ukazuje jednoduchý třídy rozbočovače pro chatovací aplikace.</span><span class="sxs-lookup"><span data-stu-id="b548b-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="b548b-197">V tomto příkladu můžete volat připojený klient `NewContosoChatMessage` metoda, a pokud ano, je přijatá data vysílali pro všechny připojené klienty.</span><span class="sxs-lookup"><span data-stu-id="b548b-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="b548b-198">Doba života objektu rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-198">Hub object lifetime</span></span>

<span data-ttu-id="b548b-199">Nepřidávejte vytvořit instanci třídy rozbočovače nebo volat jeho metody z vlastního kódu na serveru. všechny možnosti, které je potřeba pro vás kanálu rozbočovače SignalR.</span><span class="sxs-lookup"><span data-stu-id="b548b-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="b548b-200">SignalR vytvoří novou instanci třídy vašeho centra pokaždé, když je potřeba zpracovat operaci rozbočovače, například když je klient připojí, odpojí nebo provede metodu volání na server.</span><span class="sxs-lookup"><span data-stu-id="b548b-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="b548b-201">Protože jsou přechodné instancí třídy rozbočovače, nelze je použít pro uchování stavu z volání jedné metody na další.</span><span class="sxs-lookup"><span data-stu-id="b548b-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="b548b-202">Pokaždé, když server obdrží volání metody z klienta, novou instanci třídy procesů rozbočovače zprávu.</span><span class="sxs-lookup"><span data-stu-id="b548b-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="b548b-203">Pro uchování stavu pomocí více připojení a volání metod, pomocí jiné metody, jako je například databáze, nebo proměnnou statické třídy rozbočovače nebo jinou třídu, která není odvozena od `Hub`.</span><span class="sxs-lookup"><span data-stu-id="b548b-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="b548b-204">Pokud jste uchovávat data v paměti, pomocí metody například statické proměnné na třídy rozbočovače, data se ztratí recyklování domény aplikace.</span><span class="sxs-lookup"><span data-stu-id="b548b-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="b548b-205">Pokud chcete odesílat zprávy pro klienty z vlastní kód, který je spuštěn mimo třídy rozbočovače, nelze provést po vytvoření instance instance třídy rozbočovače, ale můžete provést získáním odkaz na objekt context SignalR pro vlastní třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="b548b-206">Další informace najdete v tématu [jak volat metody klienta a spravovat skupiny z mimo třídy rozbočovače](#callfromoutsidehub) dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="b548b-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="b548b-207">Formátu camelCase-velká a malá písmena rozbočovače názvů klientů JavaScript</span><span class="sxs-lookup"><span data-stu-id="b548b-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="b548b-208">Ve výchozím nastavení najdete klientů JavaScript do centra pomocí názvu třídy verze ve formátu camelCase.</span><span class="sxs-lookup"><span data-stu-id="b548b-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="b548b-209">SignalR tuto změnu automaticky provede tak, aby JavaScript konvence může odpovídat kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b548b-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="b548b-210">V předchozím příkladu by se označuje jako `contosoChatHub` v kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b548b-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="b548b-211">**Server**</span><span class="sxs-lookup"><span data-stu-id="b548b-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="b548b-212">**JavaScript klienta pomocí vygenerovaný proxy server**</span><span class="sxs-lookup"><span data-stu-id="b548b-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="b548b-213">Pokud chcete zadat jiný název pro klienty použít, přidejte `HubName` atribut.</span><span class="sxs-lookup"><span data-stu-id="b548b-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="b548b-214">Při použití `HubName` atributu, se nezměnila název na formát camelCase na klientech JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b548b-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="b548b-215">**Server**</span><span class="sxs-lookup"><span data-stu-id="b548b-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="b548b-216">**JavaScript klienta pomocí vygenerovaný proxy server**</span><span class="sxs-lookup"><span data-stu-id="b548b-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="b548b-217">Více rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-217">Multiple Hubs</span></span>

<span data-ttu-id="b548b-218">V aplikaci můžete definovat více třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="b548b-219">Pokud můžete udělat, připojení se sdílí, ale jsou samostatné skupiny:</span><span class="sxs-lookup"><span data-stu-id="b548b-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="b548b-220">Budou všichni klienti používat stejnou adresu URL k navázání připojení SignalR s služby ("/ signalr" nebo vaše vlastní adresu URL, pokud byl zadán), a zda připojení se používá pro všechny rozbočovače definované službu.</span><span class="sxs-lookup"><span data-stu-id="b548b-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="b548b-221">Neexistuje žádné rozdíly ve výkonnosti pro více rozbočovače ve srovnání s definování všechny funkce rozbočovače do jedné třídy.</span><span class="sxs-lookup"><span data-stu-id="b548b-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="b548b-222">Všechny rozbočovače získat stejné informace o požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="b548b-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="b548b-223">Vzhledem k tomu, že všechny rozbočovače sdílet stejné připojení, je pouze informace žádosti HTTP, který server bude, co se dodává v původní požadavek HTTP, který vytvoří připojení SignalR.</span><span class="sxs-lookup"><span data-stu-id="b548b-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="b548b-224">Pokud používáte k předávání informací z klienta na server tak, že zadáte řetězec dotazu požadavku na připojení, nemůžete zadat odlišné řetězce dotazu do různých rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="b548b-225">Stejné informace se zobrazí všechny rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="b548b-226">Vygenerovaný soubor proxy JavaScript bude obsahovat proxy pro všechny rozbočovače v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="b548b-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="b548b-227">Informace o třídy proxy JavaScript najdete v tématu [průvodce rozhraní API rozbočovače SignalR - JavaScript klienta - vygenerovaný proxy server a jakým způsobem vám](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="b548b-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="b548b-228">Skupiny jsou definovány v rámci centra.</span><span class="sxs-lookup"><span data-stu-id="b548b-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="b548b-229">V systému SignalR, které můžete definovat název skupiny pro všesměrové vysílání pro podmnožiny připojených klientů.</span><span class="sxs-lookup"><span data-stu-id="b548b-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="b548b-230">Skupiny jsou udržovány odděleně pro každý rozbočovač.</span><span class="sxs-lookup"><span data-stu-id="b548b-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="b548b-231">Například skupina s názvem "Administrators" by mělo zahrnovat jednu sadu klientů pro vaše `ContosoChatHub` třídy a stejný název skupiny by se označují jinou sadu klientů pro vaše `StockTickerHub` třídy.</span><span class="sxs-lookup"><span data-stu-id="b548b-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="b548b-232">Silného typu rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="b548b-233">Chcete-li definovat rozhraní pro vaše Centrum metody, které může váš klient odkaz (a povolení technologie Intellisense pro vaše Centrum metody), nemají centra z `Hub<T>` (dostupné ve verzi SignalR 2.1) místo `Hub`:</span><span class="sxs-lookup"><span data-stu-id="b548b-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="b548b-234">Jak definovat metody ve třídě rozbočovače, která můžete volat klientů</span><span class="sxs-lookup"><span data-stu-id="b548b-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="b548b-235">Ke zveřejnění metodu v rozbočovači, kterou chcete být možné volat z klienta, deklarujte veřejnou metodu, jak je znázorněno v následujících příkladech.</span><span class="sxs-lookup"><span data-stu-id="b548b-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="b548b-236">Můžete zadat návratový typ a parametry, včetně komplexní typy a pole, jako byste v libovolné metody C#.</span><span class="sxs-lookup"><span data-stu-id="b548b-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="b548b-237">Žádná data, která přijímat parametry nebo vrací volajícímu informace se předávají mezi klientem a serverem pomocí JSON a SignalR automaticky zpracovává vazba na komplexní objekty a pole objektů.</span><span class="sxs-lookup"><span data-stu-id="b548b-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="b548b-238">Formátu camelCase-velká a malá písmena metoda názvů klientů JavaScript</span><span class="sxs-lookup"><span data-stu-id="b548b-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="b548b-239">Ve výchozím nastavení klienti JavaScript odkazovat na metod rozbočovače pomocí ve formátu camelCase verzi název metody.</span><span class="sxs-lookup"><span data-stu-id="b548b-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="b548b-240">SignalR tuto změnu automaticky provede tak, aby JavaScript konvence může odpovídat kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b548b-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="b548b-241">**Server**</span><span class="sxs-lookup"><span data-stu-id="b548b-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="b548b-242">**JavaScript klienta pomocí vygenerovaný proxy server**</span><span class="sxs-lookup"><span data-stu-id="b548b-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="b548b-243">Pokud chcete zadat jiný název pro klienty použít, přidejte `HubMethodName` atribut.</span><span class="sxs-lookup"><span data-stu-id="b548b-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="b548b-244">**Server**</span><span class="sxs-lookup"><span data-stu-id="b548b-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="b548b-245">**JavaScript klienta pomocí vygenerovaný proxy server**</span><span class="sxs-lookup"><span data-stu-id="b548b-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="b548b-246">Při spuštění asynchronně</span><span class="sxs-lookup"><span data-stu-id="b548b-246">When to execute asynchronously</span></span>

<span data-ttu-id="b548b-247">Pokud metoda bude se dlouho běžící nebo pracovat, by zahrnovat čekání, jako je vyhledávání v databázi nebo volání webové služby, zkontrolujte asynchronní metody rozbočovače vrácením [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (místě `void` vrátit) nebo [ Úloha&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) objektu (místě `T` návratový typ).</span><span class="sxs-lookup"><span data-stu-id="b548b-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="b548b-248">Když se vrátíte `Task` objekt z metody SignalR čeká `Task` k dokončení, a pak pošle rozbalenou výsledek zpět do klienta, takže není žádný rozdíl v tom, jak kód volání metody, které v klientovi.</span><span class="sxs-lookup"><span data-stu-id="b548b-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="b548b-249">Provedení metody rozbočovače asynchronní zabraňuje blokování připojení, když používá přenos protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b548b-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="b548b-250">Když metoda rozbočovače spouští synchronně a přenosu je WebSocket, následných volání metod rozbočovače ze stejného klienta jsou zablokovány, dokud se nedokončí metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="b548b-251">Následující příklad ukazuje stejnou metodu zakódované spouští synchronně nebo asynchronně, za nímž následuje kód JavaScript klienta, který se dá použít pro volání buď verze.</span><span class="sxs-lookup"><span data-stu-id="b548b-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="b548b-252">**Synchronní**</span><span class="sxs-lookup"><span data-stu-id="b548b-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="b548b-253">**Asynchronous**</span><span class="sxs-lookup"><span data-stu-id="b548b-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="b548b-254">**JavaScript klienta pomocí vygenerovaný proxy server**</span><span class="sxs-lookup"><span data-stu-id="b548b-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="b548b-255">Další informace o tom, jak použít asynchronní metody v technologii ASP.NET 4.5 najdete v tématu [použití asynchronních metod v architektuře ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="b548b-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="b548b-256">Definování přetížení</span><span class="sxs-lookup"><span data-stu-id="b548b-256">Defining Overloads</span></span>

<span data-ttu-id="b548b-257">Pokud chcete definovat přetížení pro metodu, musí být jiný počet parametrů v každé přetížení.</span><span class="sxs-lookup"><span data-stu-id="b548b-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="b548b-258">Pokud jste právě zadáním různé typy parametrů rozlišit přetížení, bude kompilovat vaší třídy rozbočovače ale služby SignalR vyvolá výjimku za běhu, když se klienti pokusí volání, jedno z přetížení.</span><span class="sxs-lookup"><span data-stu-id="b548b-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="b548b-259">Vytvoření sestavy průběhu z volání metod rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="b548b-260">SignalR 2.1 přidává podporu pro [průběh reporting vzor](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) byla zavedená v rozhraní .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="b548b-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="b548b-261">K implementaci průběh vytváření sestav, definování `IProgress<T>` parametru pro metodu rozbočovače, kterým může přistupovat vašeho klienta:</span><span class="sxs-lookup"><span data-stu-id="b548b-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="b548b-262">Při zápisu metodu dlouho běžící serveru, je důležité použít asynchronní programování vzor jako asynchronní / Await místo blokování vláken rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="b548b-263">Jak volat metody klienta z třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="b548b-264">Chcete-li volat metody klienta ze serveru, použijte `Clients` vlastnost metodu v třídě rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="b548b-265">Následující příklad ukazuje kód serveru, který volá `addNewMessageToPage` na všechny připojené klienty a klientský kód, který definuje metodu v klientovi JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b548b-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="b548b-266">**Server**</span><span class="sxs-lookup"><span data-stu-id="b548b-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="b548b-267">**JavaScript klienta pomocí vygenerovaný proxy server**</span><span class="sxs-lookup"><span data-stu-id="b548b-267">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="b548b-268">Nelze získat návratovou hodnotu z klienta metody; syntaxe jako `int x = Clients.All.add(1,1)` nefunguje.</span><span class="sxs-lookup"><span data-stu-id="b548b-268">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="b548b-269">Můžete zadat komplexní typy a pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="b548b-269">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="b548b-270">Následující příklad předá do klienta v parametru metody komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="b548b-270">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="b548b-271">**Kód serveru, který volá metodu klienta pomocí komplexní objekt**</span><span class="sxs-lookup"><span data-stu-id="b548b-271">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="b548b-272">**Kód serveru, který definuje komplexní objekt**</span><span class="sxs-lookup"><span data-stu-id="b548b-272">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="b548b-273">**JavaScript klienta pomocí vygenerovaný proxy server**</span><span class="sxs-lookup"><span data-stu-id="b548b-273">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="b548b-274">Výběr klienty, kteří obdrží vzdálené volání Procedur</span><span class="sxs-lookup"><span data-stu-id="b548b-274">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="b548b-275">Vrátí vlastnost klientů [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) objekt, který poskytuje několik možností pro zadání klienty, kteří obdrží vzdálené volání Procedur:</span><span class="sxs-lookup"><span data-stu-id="b548b-275">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="b548b-276">Všichni připojení klienti.</span><span class="sxs-lookup"><span data-stu-id="b548b-276">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="b548b-277">Pouze volajícího klienta.</span><span class="sxs-lookup"><span data-stu-id="b548b-277">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="b548b-278">Všichni klienti s výjimkou volajícího klienta.</span><span class="sxs-lookup"><span data-stu-id="b548b-278">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="b548b-279">Konkrétním klientovi identifikovaný ID připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-279">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="b548b-280">Tento příklad volá `addContosoChatMessageToPage` na volajícího klienta a má stejný účinek jako použití `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="b548b-280">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="b548b-281">Všichni připojení klienti s výjimkou zadaných klientů, identifikovaný ID připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-281">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="b548b-282">Všechny připojené klienty v zadané skupině.</span><span class="sxs-lookup"><span data-stu-id="b548b-282">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="b548b-283">Všechny připojené klienty v zadané skupině s výjimkou zadaných klientů, identifikovaný ID připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-283">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="b548b-284">Všechny připojené klienty v zadané skupině s výjimkou volajícího klienta.</span><span class="sxs-lookup"><span data-stu-id="b548b-284">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="b548b-285">Konkrétní uživatele identifikovaného parametrem ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="b548b-285">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="b548b-286">Ve výchozím nastavení je to `IPrincipal.Identity.Name`, ale to se dá změnit pomocí [registrace implementace IUserIdProvider s globální hostitelem](mapping-users-to-connections.md#IUserIdProvider).</span><span class="sxs-lookup"><span data-stu-id="b548b-286">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="b548b-287">Všichni klienti a skupiny v seznamu ID připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-287">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="b548b-288">Seznam skupin.</span><span class="sxs-lookup"><span data-stu-id="b548b-288">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="b548b-289">Uživatel podle názvu.</span><span class="sxs-lookup"><span data-stu-id="b548b-289">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="b548b-290">Seznam názvů uživatele (dostupné ve verzi SignalR 2.1).</span><span class="sxs-lookup"><span data-stu-id="b548b-290">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="b548b-291">Žádné ověření kompilace pro názvy – metoda</span><span class="sxs-lookup"><span data-stu-id="b548b-291">No compile-time validation for method names</span></span>

<span data-ttu-id="b548b-292">Název metody, který zadáte interpretována jako dynamický objekt, což znamená, že není žádná IntelliSense nebo kompilace přijme se.</span><span class="sxs-lookup"><span data-stu-id="b548b-292">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="b548b-293">Vyhodnotí výraz v době běhu.</span><span class="sxs-lookup"><span data-stu-id="b548b-293">The expression is evaluated at run time.</span></span> <span data-ttu-id="b548b-294">Při volání metody, které spouští, se k němu předávají SignalR odešle klientovi název metody a hodnoty parametru, a pokud má klient metodu, která odpovídá názvu, že metoda je volána a hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="b548b-294">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="b548b-295">Pokud žádná odpovídající metoda nenajde na straně klienta, je vyvolána žádná chyba.</span><span class="sxs-lookup"><span data-stu-id="b548b-295">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="b548b-296">Informace o formátu dat, která SignalR odesílá klientovi na pozadí při volání metody klienta najdete v tématu [Úvod do SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="b548b-296">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="b548b-297">S odpovídajícím názvem velká a malá písmena – metoda</span><span class="sxs-lookup"><span data-stu-id="b548b-297">Case-insensitive method name matching</span></span>

<span data-ttu-id="b548b-298">Metoda shoda názvů nerozlišuje.</span><span class="sxs-lookup"><span data-stu-id="b548b-298">Method name matching is case-insensitive.</span></span> <span data-ttu-id="b548b-299">Například `Clients.All.addContosoChatMessageToPage` na serveru, budou spuštěny `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, nebo `addContosoChatMessageToPage` na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="b548b-299">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="b548b-300">Asynchronního spuštění</span><span class="sxs-lookup"><span data-stu-id="b548b-300">Asynchronous execution</span></span>

<span data-ttu-id="b548b-301">Asynchronně provede metodu, kterou je možné volat.</span><span class="sxs-lookup"><span data-stu-id="b548b-301">The method that you call executes asynchronously.</span></span> <span data-ttu-id="b548b-302">Kód, který se dodává po volání metody pro klienta, budou spuštěny okamžitě bez čekání na SignalR k dokončení přenosu dat do klientů, pokud je určit, že další řádek kódu by měla čekání na dokončení metody.</span><span class="sxs-lookup"><span data-stu-id="b548b-302">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="b548b-303">Následující příklad kódu ukazuje, jak provést dvě metody klienta postupně.</span><span class="sxs-lookup"><span data-stu-id="b548b-303">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="b548b-304">**Pomocí operátoru Await (.NET 4.5)**</span><span class="sxs-lookup"><span data-stu-id="b548b-304">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="b548b-305">Pokud používáte `await` počkat, dokud nebude dokončeno metodu klientské před dalším řádku kódu, který nemusí znamenat, že klienti budou ve skutečnosti zobrazí se zpráva před dalším řádku kódu.</span><span class="sxs-lookup"><span data-stu-id="b548b-305">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="b548b-306">"Dokončení" volání metody klienta pouze znamená, že SignalR provedla všechno potřebné k odeslání zprávy.</span><span class="sxs-lookup"><span data-stu-id="b548b-306">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="b548b-307">Pokud potřebujete, aby klienti zobrazila zpráva ověření, budete muset tento mechanismus programu sami.</span><span class="sxs-lookup"><span data-stu-id="b548b-307">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="b548b-308">Například může kód `MessageReceived` metodu v rozbočovači a v `addContosoChatMessageToPage` metody na straně klienta může volat `MessageReceived` potom činnost, kterou je potřeba udělat na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="b548b-308">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="b548b-309">V `MessageReceived` v centru můžete provést, ať pracovní závisí na příjem skutečné klienta a zpracování původní volání metody.</span><span class="sxs-lookup"><span data-stu-id="b548b-309">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="b548b-310">Postup řetězec proměnnou použít jako název metody</span><span class="sxs-lookup"><span data-stu-id="b548b-310">How to use a string variable as the method name</span></span>

<span data-ttu-id="b548b-311">Pokud chcete vyvolat metodu klienta pomocí proměnné řetězec jako název metody, přetypovat `Clients.All` (nebo `Clients.Others`, `Clients.Caller`atd) k `IClientProxy` a pak zavolají [Invoke (methodName, argumentů...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="b548b-311">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="b548b-312">Správa členství ve skupině z třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-312">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="b548b-313">Skupiny v systému SignalR poskytují metodu pro zprávy všesměrové vysílání pro zadaný podmnožiny připojených klientů.</span><span class="sxs-lookup"><span data-stu-id="b548b-313">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="b548b-314">Skupina může mít libovolný počet klientů a klienta může být členem skupiny libovolný počet skupin.</span><span class="sxs-lookup"><span data-stu-id="b548b-314">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="b548b-315">Chcete-li spravovat členství ve skupinách, použijte [přidat](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) a [odebrat](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metody poskytované `Groups` vlastnost třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-315">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="b548b-316">Následující příklad ukazuje `Groups.Add` a `Groups.Remove` metody používané v metodách rozbočovače, které se nazývají kódem na straně klienta, za nímž následuje kód JavaScript klienta, který volá je.</span><span class="sxs-lookup"><span data-stu-id="b548b-316">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="b548b-317">**Server**</span><span class="sxs-lookup"><span data-stu-id="b548b-317">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="b548b-318">**JavaScript klienta pomocí vygenerovaný proxy server**</span><span class="sxs-lookup"><span data-stu-id="b548b-318">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="b548b-319">Nemáte nemusel vytvářet skupiny.</span><span class="sxs-lookup"><span data-stu-id="b548b-319">You don't have to explicitly create groups.</span></span> <span data-ttu-id="b548b-320">Platí skupina se automaticky vytvoří při prvním zadejte její název ve volání `Groups.Add`, a bude odstraněn, když odeberete poslední připojení z členství v ní.</span><span class="sxs-lookup"><span data-stu-id="b548b-320">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="b548b-321">Neexistuje žádné rozhraní API pro získání seznamu členství ve skupinách nebo seznam skupin.</span><span class="sxs-lookup"><span data-stu-id="b548b-321">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="b548b-322">SignalR odešle zprávy do klientů a skupin na základě [pub nebo sub modelu](http://en.wikipedia.org/wiki/Publish/subscribe), a server neumožňuje spravovat seznam skupin nebo členství ve skupinách.</span><span class="sxs-lookup"><span data-stu-id="b548b-322">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="b548b-323">To pomáhá maximalizovat škálovatelnost, protože při každém přidání uzlu do webové farmy, nějaký stav, který udržuje SignalR musí být rozšířena do nového uzlu.</span><span class="sxs-lookup"><span data-stu-id="b548b-323">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="b548b-324">Asynchronní provádění metody přidat a odebrat</span><span class="sxs-lookup"><span data-stu-id="b548b-324">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="b548b-325">`Groups.Add` a `Groups.Remove` metody provedení asynchronně.</span><span class="sxs-lookup"><span data-stu-id="b548b-325">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="b548b-326">Pokud chcete okamžitě odeslání zprávy do klienta pomocí skupiny a přidání klienta do skupiny, budete muset Ujistěte se, že `Groups.Add` metoda nejprve dokončí.</span><span class="sxs-lookup"><span data-stu-id="b548b-326">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="b548b-327">Následující příklad kódu ukazuje, jak to provést.</span><span class="sxs-lookup"><span data-stu-id="b548b-327">The following code example shows how to do that.</span></span>

<span data-ttu-id="b548b-328">**Přidání klienta do skupiny a pak zasílání zpráv tohoto klienta**</span><span class="sxs-lookup"><span data-stu-id="b548b-328">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="b548b-329">Trvalost členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="b548b-329">Group membership persistence</span></span>

<span data-ttu-id="b548b-330">SignalR sleduje připojení, nikoli uživatelům, takže když chcete, aby uživatel být ve stejné skupině pokaždé, když uživatel naváže připojení, budete muset volat `Groups.Add` pokaždé, když uživatel vytvoří nové připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-330">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="b548b-331">Po k dočasné ztrátě připojení někdy SignalR můžete obnovit připojení automaticky.</span><span class="sxs-lookup"><span data-stu-id="b548b-331">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="b548b-332">V takovém případě SignalR obnovuje stejné připojení, není vytvořeno nové připojení, a proto je členství ve skupině klienta automaticky obnoví.</span><span class="sxs-lookup"><span data-stu-id="b548b-332">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="b548b-333">To lze provést i v případě, že dočasné přerušení je výsledek restartování serveru nebo selhání, protože stav připojení u jednotlivých klientů, včetně členství ve skupinách, je zpětné odbavení klientovi.</span><span class="sxs-lookup"><span data-stu-id="b548b-333">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="b548b-334">Pokud se server přestane fungovat a je nahrazena nový server, než vyprší časový limit připojení, můžete klienta automaticky opětovně připojit k novému serveru a znovu zaregistrovat ve skupinách, které je členem skupiny.</span><span class="sxs-lookup"><span data-stu-id="b548b-334">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="b548b-335">Při připojení nelze obnovit automaticky po ztrátě připojení, nebo pokud vyprší časový limit připojení, nebo pokud se klient neodpojí (například pokud prohlížeč přejde na novou stránku), členství ve skupinách budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="b548b-335">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="b548b-336">Při příštím připojení uživatele bude nové připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-336">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="b548b-337">Chcete-li udržovat členství ve skupinách, pokud stejný uživatel vytvoří nové připojení, aplikace má sledovat přidružení mezi uživatelů a skupin a členství ve skupinách obnovení pokaždé, když uživatel vytvoří nové připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-337">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="b548b-338">Další informace o připojení a opětovná připojení najdete v tématu [zpracování událostí životnost připojení v třídy rozbočovače](#connectionlifetime) dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="b548b-338">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="b548b-339">Skupiny jednoho uživatele</span><span class="sxs-lookup"><span data-stu-id="b548b-339">Single-user groups</span></span>

<span data-ttu-id="b548b-340">Aplikace, které používají SignalR obvykle mají ke sledování asociace mezi uživateli a připojení k vědět, který uživatel odeslal zprávu a které jiní uživatelé měli přijímání zprávy.</span><span class="sxs-lookup"><span data-stu-id="b548b-340">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="b548b-341">Skupiny se používají v jednom ze dvou běžně používané vzorků pro učinit.</span><span class="sxs-lookup"><span data-stu-id="b548b-341">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="b548b-342">Skupiny jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="b548b-342">Single-user groups.</span></span>

    <span data-ttu-id="b548b-343">Můžete zadat uživatelské jméno jako je název skupiny a přidejte aktuální ID připojení ke skupině pokaždé, když se uživatel připojí nebo znovu připojí.</span><span class="sxs-lookup"><span data-stu-id="b548b-343">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="b548b-344">K odeslání zprávy uživateli odeslat do skupiny.</span><span class="sxs-lookup"><span data-stu-id="b548b-344">To send messages to the user you send to the group.</span></span> <span data-ttu-id="b548b-345">Nevýhody této metody je, že skupiny není poskytují způsob, jak zjistit, zda uživatel je online nebo offline.</span><span class="sxs-lookup"><span data-stu-id="b548b-345">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="b548b-346">Sledování přidružení mezi uživatelská jména a ID připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-346">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="b548b-347">Můžete ukládat přidružení mezi každé uživatelské jméno a jeden nebo více připojení ID slovník nebo databáze a aktualizovat uložená data pokaždé, když uživatel připojí nebo odpojí.</span><span class="sxs-lookup"><span data-stu-id="b548b-347">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="b548b-348">Pokud chcete odesílat zprávy uživateli zadáte ID připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-348">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="b548b-349">Nevýhody této metody je, že trvá více paměti.</span><span class="sxs-lookup"><span data-stu-id="b548b-349">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="b548b-350">Postupy: zpracování události životnost připojení v třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-350">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="b548b-351">Typické důvody pro zpracování události životnost připojení jsou ke sledování zda je uživatel připojen nebo Ne a ke sledování přidružení mezi uživatelská jména a ID připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-351">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="b548b-352">Chcete-li spustit vlastní kód, když klienti připojení nebo odpojení, přepište `OnConnected`, `OnDisconnected`, a `OnReconnected` třídy virtuální metody rozbočovače, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="b548b-352">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="b548b-353">Kdy jsou volány onconnected rozbočovače, ondisconnected rozbočovače a onreconnected rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-353">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="b548b-354">Pokaždé, když prohlížeč přejde na novou stránku, nové připojení musí být zavedena, což znamená, že budou spuštěny SignalR `OnDisconnected` metoda následuje `OnConnected` metoda.</span><span class="sxs-lookup"><span data-stu-id="b548b-354">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="b548b-355">SignalR vytvoří nové ID připojení vždy, když je vytvořeno nové připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-355">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="b548b-356">`OnReconnected` Metoda je volána, když bude zjištěna dočasné přerušení připojení SignalR automaticky obnovení v případě, například když kabelu dočasně odpojení a opětovném připojení, než vyprší časový limit připojení. `OnDisconnected` Metoda je volána, když je klient odpojen a SignalR nelze znovu připojit automaticky, například pokud prohlížeč přejde na novou stránku.</span><span class="sxs-lookup"><span data-stu-id="b548b-356">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="b548b-357">Proto je možné pořadí událostí pro daného klienta `OnConnected`, `OnReconnected`, `OnDisconnected`; nebo `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="b548b-357">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="b548b-358">Neuvidíte sekvenci `OnConnected`, `OnDisconnected`, `OnReconnected` pro dané připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-358">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="b548b-359">`OnDisconnected` Metoda nebude zavolána v některých případech, například když server ocitne mimo provoz nebo doména aplikace získá recykluje.</span><span class="sxs-lookup"><span data-stu-id="b548b-359">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="b548b-360">Pokud jiný server obsahuje na řádku nebo doména aplikace dokončení jeho recyklace, může být někteří klienti moci znovu připojit a spusťte `OnReconnected` událostí.</span><span class="sxs-lookup"><span data-stu-id="b548b-360">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="b548b-361">Další informace najdete v tématu [pochopení a zpracování události životnost připojení v systému SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="b548b-361">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="b548b-362">Stav volajícího není naplněno.</span><span class="sxs-lookup"><span data-stu-id="b548b-362">Caller state not populated</span></span>

<span data-ttu-id="b548b-363">Metody obslužné rutiny události životnost připojení se nazývají ze serveru, což znamená, že žádný stav, který jste zadali v `state` objektu na straně klienta se nenaplní v `Caller` vlastnost na serveru.</span><span class="sxs-lookup"><span data-stu-id="b548b-363">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="b548b-364">Informace o `state` objektu a `Caller` vlastnost, najdete v části [jak předat stavu mezi klienty a třídy rozbočovače](#passstate) dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="b548b-364">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="b548b-365">Jak získat informace o klientovi z vlastností kontextu</span><span class="sxs-lookup"><span data-stu-id="b548b-365">How to get information about the client from the Context property</span></span>

<span data-ttu-id="b548b-366">Chcete-li získat informace o klientovi, použijte `Context` vlastnost třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-366">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="b548b-367">`Context` Vlastnost vrátí [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) objekt, který poskytuje přístup k následujícím informacím:</span><span class="sxs-lookup"><span data-stu-id="b548b-367">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="b548b-368">ID připojení volajícího klienta.</span><span class="sxs-lookup"><span data-stu-id="b548b-368">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="b548b-369">ID připojení je identifikátor GUID, který je přiřazen nástrojem SignalR (nelze zadat hodnotu v kódu).</span><span class="sxs-lookup"><span data-stu-id="b548b-369">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="b548b-370">Neexistuje jeden ID připojení pro každé připojení a stejné připojení, které ID je využíváno všechny rozbočovače, pokud máte více Hubs v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b548b-370">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="b548b-371">Data hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b548b-371">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="b548b-372">Můžete také získat hlavičky protokolu HTTP z `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="b548b-372">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="b548b-373">Z důvodu pro několik odkazů na stejnou věc je, že `Context.Headers` byla vytvořena jako první, `Context.Request` vlastnost byla přidána novější, a `Context.Headers` se zachovává kvůli zpětné kompatibilitě.</span><span class="sxs-lookup"><span data-stu-id="b548b-373">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="b548b-374">Data řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="b548b-374">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="b548b-375">Můžete také získat data řetězce dotazu z `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="b548b-375">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="b548b-376">Řetězec dotazu, kterou můžete získat v této vlastnosti je ten, který byl použit s požadavkem HTTP, který vytvořili připojení SignalR.</span><span class="sxs-lookup"><span data-stu-id="b548b-376">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="b548b-377">Konfigurace připojení, což je pohodlný způsob, jak předat data o klientovi z klienta na server, můžete přidat parametrů řetězce dotazu v klientovi.</span><span class="sxs-lookup"><span data-stu-id="b548b-377">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="b548b-378">Následující příklad ukazuje jeden způsob, jak přidat řetězec dotazu v klientovi JavaScript, při použití vygenerovaný proxy server.</span><span class="sxs-lookup"><span data-stu-id="b548b-378">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="b548b-379">Další informace o nastavení parametrů řetězce dotazu, najdete v části příručky rozhraní API pro [JavaScript](hubs-api-guide-javascript-client.md) a [.NET](hubs-api-guide-net-client.md) klientů.</span><span class="sxs-lookup"><span data-stu-id="b548b-379">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="b548b-380">Můžete najít metodu přenosu používá pro připojení v data řetězce dotazu, společně s některé hodnoty, které používá interně SignalR:</span><span class="sxs-lookup"><span data-stu-id="b548b-380">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="b548b-381">Hodnota `transportMethod` bude "Websocket", "serverSentEvents", "foreverFrame" nebo "longPolling".</span><span class="sxs-lookup"><span data-stu-id="b548b-381">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="b548b-382">Všimněte si, že pokud změnami tuto hodnotu `OnConnected` obslužná rutina události, v některých scénářích může nejdřív získat hodnotu přenosu, která není metodu konečné vyjednané přenosu pro připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-382">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="b548b-383">V takovém případě metoda vyvolá výjimku a bude volána později, po vytvoření metodu konečné přenosu.</span><span class="sxs-lookup"><span data-stu-id="b548b-383">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="b548b-384">Soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="b548b-384">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="b548b-385">Můžete získat také soubory cookie z `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="b548b-385">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="b548b-386">Informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="b548b-386">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="b548b-387">Položka HttpContext objektu žádosti:</span><span class="sxs-lookup"><span data-stu-id="b548b-387">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="b548b-388">Tuto metodu použijte místo získávání `HttpContext.Current` získat `HttpContext` objekt pro připojení SignalR.</span><span class="sxs-lookup"><span data-stu-id="b548b-388">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="b548b-389">Jak předat stavu mezi klienty a třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-389">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="b548b-390">Poskytuje proxy serveru klienta `state` objektu, ve kterém můžete ukládat data, která chcete k přenosu na server s každou volání metody.</span><span class="sxs-lookup"><span data-stu-id="b548b-390">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="b548b-391">Na serveru můžete přístup k těmto datům v `Clients.Caller` vlastnost v metodách rozbočovače, které se nazývají klienty.</span><span class="sxs-lookup"><span data-stu-id="b548b-391">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="b548b-392">`Clients.Caller` Vlastnost není vyplněný pro metody obslužné rutiny události životnost připojení `OnConnected`, `OnDisconnected`, a `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="b548b-392">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="b548b-393">Vytváření nebo aktualizaci dat v `state` objektu a `Clients.Caller` vlastnost funguje v obou směrech.</span><span class="sxs-lookup"><span data-stu-id="b548b-393">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="b548b-394">Můžete aktualizovat hodnoty na serveru a je předán zpět do klienta.</span><span class="sxs-lookup"><span data-stu-id="b548b-394">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="b548b-395">Následující příklad ukazuje kód JavaScript klienta, která ukládá stav pro přenos na server s každé volání metody.</span><span class="sxs-lookup"><span data-stu-id="b548b-395">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="b548b-396">Následující příklad ukazuje kód ekvivalent v rozhraní .NET klientovi.</span><span class="sxs-lookup"><span data-stu-id="b548b-396">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="b548b-397">Ve třídě rozbočovače, můžete přístup k těmto datům v `Clients.Caller` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b548b-397">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="b548b-398">Následující příklad ukazuje kód, který načte stav uvedené v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="b548b-398">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="b548b-399">Tento mechanismus pro zachování stavu není určena pro velké objemy dat, protože všechno, co jste zadali v `state` nebo `Clients.Caller` vlastnost je zpětné odbavení s každé volání metody.</span><span class="sxs-lookup"><span data-stu-id="b548b-399">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="b548b-400">Je vhodné pro menší položek, jako jsou uživatelská jména nebo čítače.</span><span class="sxs-lookup"><span data-stu-id="b548b-400">It's useful for smaller items such as user names or counters.</span></span>


<span data-ttu-id="b548b-401">V VB.NET nebo v rozbočovači silného typu, volající objekt stavu nelze přistupovat prostřednictvím `Clients.Caller`; místo toho použijte `Clients.CallerState` (dostupné ve verzi SignalR 2.1):</span><span class="sxs-lookup"><span data-stu-id="b548b-401">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="b548b-402">**Pomocí CallerState v jazyce C#**</span><span class="sxs-lookup"><span data-stu-id="b548b-402">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="b548b-403">**Pomocí CallerState v jazyce Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="b548b-403">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="b548b-404">Postupy: zpracování chyb v třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-404">How to handle errors in the Hub class</span></span>

<span data-ttu-id="b548b-405">Zpracování chyb, které nastat ve vaší metody třídy rozbočovače, použijte jednu nebo více z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="b548b-405">To handle errors that occur in your Hub class methods, use one or more of the following methods:</span></span>

- <span data-ttu-id="b548b-406">Zalomení metoda kódu v try-catch – bloky a protokolu objekt výjimky.</span><span class="sxs-lookup"><span data-stu-id="b548b-406">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="b548b-407">Pro účely ladění můžete odeslat výjimka klienta, ale pro zabezpečení se nedoporučuje důvodů odesílání podrobné informace pro klienty v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b548b-407">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="b548b-408">Vytvoření modulu kanálu rozbočovače, který zpracovává [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="b548b-408">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="b548b-409">Následující příklad ukazuje, kanálů modul, který protokoluje chyby, za nímž následuje kód v souboru Startup.cs, který se vloží do kanálu rozbočovače modul.</span><span class="sxs-lookup"><span data-stu-id="b548b-409">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="b548b-410">Použití `HubException` – třída (počínaje SignalR 2).</span><span class="sxs-lookup"><span data-stu-id="b548b-410">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="b548b-411">Tato chyba může být vyvolána z jakékoli volání rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-411">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="b548b-412">`HubError` Konstruktor přebírá zprávu řetězec a objektu pro ukládání dodatečné údaje o chybě.</span><span class="sxs-lookup"><span data-stu-id="b548b-412">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="b548b-413">SignalR se automaticky serializovat výjimka a odeslat ho do klienta, kde se bude používat odmítnout nebo selže volání metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-413">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="b548b-414">Následující ukázky kódu ukazují, jak má být vyvolána `HubException` během volání rozbočovače a postupy: zpracování výjimky na klienty JavaScript a rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="b548b-414">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="b548b-415">**Ukázka třídy HubException kódu serveru**</span><span class="sxs-lookup"><span data-stu-id="b548b-415">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="b548b-416">**Kód jazyka JavaScript klienta demonstraci odpovědi k vyvolání HubException v rozbočovači**</span><span class="sxs-lookup"><span data-stu-id="b548b-416">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="b548b-417">**Ukázka odpovědi k vyvolání HubException v rozbočovači kód klienta rozhraní .NET**</span><span class="sxs-lookup"><span data-stu-id="b548b-417">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="b548b-418">Další informace o modulech kanálu rozbočovače najdete v tématu [postup přizpůsobení kanálu rozbočovače](#hubpipeline) dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="b548b-418">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="b548b-419">Postup povolení trasování</span><span class="sxs-lookup"><span data-stu-id="b548b-419">How to enable tracing</span></span>

<span data-ttu-id="b548b-420">Povolení trasování na straně serveru, přidejte system.diagnostics element do souboru Web.config, jak je znázorněno v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="b548b-420">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="b548b-421">Když spustíte aplikaci v sadě Visual Studio, můžete zobrazit protokoly v **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="b548b-421">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="b548b-422">Tom, jak volat metody klienta a Správa skupin z mimo třídy rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-422">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="b548b-423">Volání metod klienta z jiné třídy než vaší třídy rozbočovače, získat odkaz na objekt context SignalR pro rozbočovač a použijte ho k volání metody na straně klienta nebo spravovat skupiny.</span><span class="sxs-lookup"><span data-stu-id="b548b-423">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="b548b-424">Následující ukázka `StockTicker` – třída získá objekt kontextu, uloží je v instanci třídy, ukládá instance třídy pomocí statické vlastnosti a používá kontext z instance třídy singleton k volání `updateStockPrice` na klienty, kteří jsou – metoda připojení k rozbočovači s názvem `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="b548b-424">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="b548b-425">Pokud potřebujete použít více časy kontextu v dlohotrvající objektu, získat odkaz na jednou a uložit ji spíš než načítání opakovat.</span><span class="sxs-lookup"><span data-stu-id="b548b-425">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="b548b-426">Získávání kontextu jednou zajistí, že SignalR odesílá zprávy klientům ve stejném pořadí, ve kterém zkontrolujte vaše metody rozbočovače klienta volání metod.</span><span class="sxs-lookup"><span data-stu-id="b548b-426">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="b548b-427">Kurz, který ukazuje způsob použití kontext SignalR pro rozbočovač, najdete v části [Server všesměrového vysílání pomocí funkce SignalR technologie ASP.NET](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="b548b-427">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="b548b-428">Volání metody klienta</span><span class="sxs-lookup"><span data-stu-id="b548b-428">Calling client methods</span></span>

<span data-ttu-id="b548b-429">Můžete určit, kteří klienti dostanou vzdálené volání Procedur, ale máte méně možností než při volání z třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-429">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="b548b-430">Důvodem je, že kontextu není spojen s konkrétní volání od klienta, proto všechny metody, které vyžadují znalosti systému aktuální ID připojení, jako `Clients.Others`, nebo `Clients.Caller`, nebo `Clients.OthersInGroup`, nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b548b-430">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="b548b-431">K dispozici jsou následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="b548b-431">The following options are available:</span></span>

- <span data-ttu-id="b548b-432">Všichni připojení klienti.</span><span class="sxs-lookup"><span data-stu-id="b548b-432">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="b548b-433">Konkrétním klientovi identifikovaný ID připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-433">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="b548b-434">Všichni připojení klienti s výjimkou zadaných klientů, identifikovaný ID připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-434">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="b548b-435">Všechny připojené klienty v zadané skupině.</span><span class="sxs-lookup"><span data-stu-id="b548b-435">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="b548b-436">Všechny připojené klienty v zadané skupině s výjimkou zadaných klientů, identifikovaný ID připojení.</span><span class="sxs-lookup"><span data-stu-id="b548b-436">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="b548b-437">Při volání do vaší třídy jiný Hub z metod ve třídě rozbočovače, můžete předat aktuální ID připojení a použít s `Clients.Client`, `Clients.AllExcept`, nebo `Clients.Group` k simulaci `Clients.Caller`, `Clients.Others`, nebo `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="b548b-437">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="b548b-438">V následujícím příkladu `MoveShapeHub` třída předá ID připojení, které `Broadcaster` – třída tak, aby `Broadcaster` třídy můžete simulovat `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="b548b-438">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="b548b-439">Správa členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="b548b-439">Managing group membership</span></span>

<span data-ttu-id="b548b-440">Pro správu skupin máte stejné možnosti jako v třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b548b-440">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="b548b-441">Přidání klienta do skupiny</span><span class="sxs-lookup"><span data-stu-id="b548b-441">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="b548b-442">Odebrání klienta ze skupiny</span><span class="sxs-lookup"><span data-stu-id="b548b-442">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="b548b-443">Postup přizpůsobení kanálu rozbočovače</span><span class="sxs-lookup"><span data-stu-id="b548b-443">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="b548b-444">SignalR umožňuje vložit do kanálu rozbočovače modul vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="b548b-444">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="b548b-445">Následující příklad ukazuje vlastní modul kanálu rozbočovače, který protokoluje každý příchozí volání metody dostali od klienta a odchozí volání metody vyvolání na straně klienta:</span><span class="sxs-lookup"><span data-stu-id="b548b-445">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="b548b-446">Následující kód na *Startup.cs* souboru zaregistruje modul pro spuštění v kanálu rozbočovače modul:</span><span class="sxs-lookup"><span data-stu-id="b548b-446">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="b548b-447">Existuje mnoho různých způsobů, které můžete přepsat.</span><span class="sxs-lookup"><span data-stu-id="b548b-447">There are many different methods that you can override.</span></span> <span data-ttu-id="b548b-448">Úplný seznam najdete v tématu [HubPipelineModule metody](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="b548b-448">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
