---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Funkce SignalR technologie ASP.NET pokyny k rozhraní API Center – klient .NET (C#) | Dokumentace Microsoftu
author: pfletcher
description: Tento dokument obsahuje úvod k používání rozhraní API rozbočovače pro funkci SignalR verze 2 v rozhraní .NET klientů, jako jsou Windows Store (WinRT), WPF, Silverlight a nevýhody...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: ce952e26c35b85582f53aa3708943848ec4998ec
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755735"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="a44b0-103">Funkce SignalR technologie ASP.NET pokyny k rozhraní API Center – klient .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="a44b0-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>
====================
<span data-ttu-id="a44b0-104">podle [Patrick Fletcher](https://github.com/pfletcher), [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a44b0-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="a44b0-105">Tento dokument obsahuje úvod k používání rozhraní API rozbočovače pro funkci SignalR verze 2 v rozhraní .NET klientů, jako jsou Windows Store (WinRT), WPF, Silverlight a konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a44b0-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="a44b0-106">Rozhraní API pro rozbočovače SignalR umožňuje vytvářet vzdálených volání procedur (RPC) ze serveru pro připojené klienty a z klientů k serveru.</span><span class="sxs-lookup"><span data-stu-id="a44b0-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="a44b0-107">V serverovém kódu můžete definovat metody, které mohou být volány klientů a volat metody, které běží na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="a44b0-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="a44b0-108">V klientském kódu můžete definovat metody, které lze volat ze serveru a volání metody, které běží na serveru.</span><span class="sxs-lookup"><span data-stu-id="a44b0-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="a44b0-109">Funkce SignalR postará za vás zajistí funkčnost systému klient server.</span><span class="sxs-lookup"><span data-stu-id="a44b0-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="a44b0-110">Funkce SignalR také nabízí nižší úrovně rozhraní API volá trvalé připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="a44b0-111">Úvod do SignalR, rozbočovačů a trvalá připojení, nebo kurz, který ukazuje, jak sestavit kompletní aplikace SignalR, přečtěte si téma [SignalR – Začínáme](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="a44b0-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="a44b0-112">Verze softwaru použitým v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="a44b0-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="a44b0-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a44b0-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="a44b0-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a44b0-114">.NET 4.5</span></span>
> - <span data-ttu-id="a44b0-115">Funkce SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="a44b0-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="a44b0-116">Předchozích verzích tohoto tématu</span><span class="sxs-lookup"><span data-stu-id="a44b0-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="a44b0-117">Informace o předchozích verzích systému SignalR naleznete v tématu [starší verze funkce SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="a44b0-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="a44b0-118">Otázky a komentáře</span><span class="sxs-lookup"><span data-stu-id="a44b0-118">Questions and comments</span></span>
> 
> <span data-ttu-id="a44b0-119">Napište prosím zpětnou vazbu o tom, jak vám líbilo v tomto kurzu a co můžeme zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="a44b0-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a44b0-120">Pokud máte nějaké otázky, které přímo nesouvisejí, najdete v tomto kurzu, můžete je publikovat [fórum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="a44b0-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="a44b0-121">Přehled</span><span class="sxs-lookup"><span data-stu-id="a44b0-121">Overview</span></span>

<span data-ttu-id="a44b0-122">Tento dokument obsahuje následující části:</span><span class="sxs-lookup"><span data-stu-id="a44b0-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="a44b0-123">Instalace klienta</span><span class="sxs-lookup"><span data-stu-id="a44b0-123">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="a44b0-124">Postup vytvoření připojení</span><span class="sxs-lookup"><span data-stu-id="a44b0-124">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="a44b0-125">Připojení mezi doménami z klienty prostředí Silverlight</span><span class="sxs-lookup"><span data-stu-id="a44b0-125">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="a44b0-126">Postup konfigurace připojení</span><span class="sxs-lookup"><span data-stu-id="a44b0-126">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="a44b0-127">Jak nastavit maximální počet souběžných připojení v klientech WPF</span><span class="sxs-lookup"><span data-stu-id="a44b0-127">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="a44b0-128">Určení parametrů řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="a44b0-128">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="a44b0-129">Jak určit metodu přenosu</span><span class="sxs-lookup"><span data-stu-id="a44b0-129">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="a44b0-130">Jak zadat hlavičky protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="a44b0-130">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="a44b0-131">Určení klientských certifikátů</span><span class="sxs-lookup"><span data-stu-id="a44b0-131">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="a44b0-132">Jak vytvořit proxy server rozbočovače</span><span class="sxs-lookup"><span data-stu-id="a44b0-132">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="a44b0-133">Definování metody na straně klienta, která může volat na serveru</span><span class="sxs-lookup"><span data-stu-id="a44b0-133">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="a44b0-134">Metody bez parametrů</span><span class="sxs-lookup"><span data-stu-id="a44b0-134">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="a44b0-135">Metody s parametry, určující typy parametrů</span><span class="sxs-lookup"><span data-stu-id="a44b0-135">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="a44b0-136">Metody s parametry, dynamických objektů pro parametry</span><span class="sxs-lookup"><span data-stu-id="a44b0-136">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="a44b0-137">Jak odebrat obslužnou rutinu</span><span class="sxs-lookup"><span data-stu-id="a44b0-137">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="a44b0-138">Volání metody serveru z klienta</span><span class="sxs-lookup"><span data-stu-id="a44b0-138">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="a44b0-139">Zpracování událostí doby platnosti</span><span class="sxs-lookup"><span data-stu-id="a44b0-139">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="a44b0-140">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="a44b0-140">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="a44b0-141">Jak povolit protokolování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="a44b0-141">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="a44b0-142">WPF, Silverlight a Konzolová aplikace ukázky kódu pro metody klienta, které můžete volat na serveru</span><span class="sxs-lookup"><span data-stu-id="a44b0-142">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="a44b0-143">Ukázka .NET klientské projekty naleznete v následujících zdrojích:</span><span class="sxs-lookup"><span data-stu-id="a44b0-143">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="a44b0-144">[gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) na webu GitHub.com (příklady WinRT, Silverlight, konzoly aplikace).</span><span class="sxs-lookup"><span data-stu-id="a44b0-144">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="a44b0-145">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) na webu GitHub.com (např. WPF).</span><span class="sxs-lookup"><span data-stu-id="a44b0-145">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="a44b0-146">[Funkce SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) na webu GitHub.com (např. aplikace konzoly).</span><span class="sxs-lookup"><span data-stu-id="a44b0-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="a44b0-147">Dokumentace o tom, jak program na serveru nebo klientů JavaScript naleznete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="a44b0-147">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="a44b0-148">Pokyny k rozhraní API Center SignalR – Server</span><span class="sxs-lookup"><span data-stu-id="a44b0-148">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="a44b0-149">Pokyny k rozhraní API Center SignalR – javascriptový klient</span><span class="sxs-lookup"><span data-stu-id="a44b0-149">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="a44b0-150">Odkazy na témata, Reference k rozhraní API se API verze rozhraní .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="a44b0-150">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="a44b0-151">Pokud používáte .NET 4, přečtěte si téma [verze .NET 4 témat API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="a44b0-151">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="a44b0-152">Instalace klienta</span><span class="sxs-lookup"><span data-stu-id="a44b0-152">Client setup</span></span>

<span data-ttu-id="a44b0-153">Nainstalujte [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) balíčku NuGet (ne [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) balíček).</span><span class="sxs-lookup"><span data-stu-id="a44b0-153">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="a44b0-154">Tento balíček podporuje WinRT, Silverlight, WPF, konzolovou aplikaci a klienti Windows Phone, .NET 4 a .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="a44b0-154">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="a44b0-155">Pokud verze funkce signalr, která máte na straně klienta se liší od verze, které máte na serveru, SignalR často je možné přizpůsobit pro rozdíl.</span><span class="sxs-lookup"><span data-stu-id="a44b0-155">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="a44b0-156">Například server se systémem SignalR verze 2 bude podporovat klienty, kteří mají nainstalované 1.1.x, stejně jako klienti, kteří mají verzi 2 nainstalované.</span><span class="sxs-lookup"><span data-stu-id="a44b0-156">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="a44b0-157">Pokud je příliš velký rozdíl mezi verzí na serveru a verze na klientovi, nebo pokud klient je novější než server, vyvolá funkce SignalR `InvalidOperationException` výjimky, když se klient pokusí navázat připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-157">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="a44b0-158">Chybová zpráva "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="a44b0-158">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="a44b0-159">Postup vytvoření připojení</span><span class="sxs-lookup"><span data-stu-id="a44b0-159">How to establish a connection</span></span>

<span data-ttu-id="a44b0-160">Předtím, než můžete navázat spojení, je nutné vytvořit `HubConnection` objektů a vytvořit proxy.</span><span class="sxs-lookup"><span data-stu-id="a44b0-160">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="a44b0-161">K navázání připojení, zavolejte `Start` metodu `HubConnection` objektu.</span><span class="sxs-lookup"><span data-stu-id="a44b0-161">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="a44b0-162">Pro klienty jazyka JavaScript, je nutné provést registraci aspoň jednu obslužnou rutinu události před voláním `Start` metoda k navázání připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-162">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="a44b0-163">To není nutné pro klienty .NET.</span><span class="sxs-lookup"><span data-stu-id="a44b0-163">This is not necessary for .NET clients.</span></span> <span data-ttu-id="a44b0-164">Pro klienty jazyka JavaScript, vygenerovaném kódu proxy automaticky vytvoří proxy pro všechna centra, které existují na serveru a registraci obslužné rutiny je způsob, jak naznačit které rozbočovače klienta chce využít.</span><span class="sxs-lookup"><span data-stu-id="a44b0-164">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="a44b0-165">Ale pro klienta .NET vytvořit proxy servery Hub ručně, tak SignalR předpokládá, že budete používat libovolné centrum, kterou vytvoříte pro proxy server.</span><span class="sxs-lookup"><span data-stu-id="a44b0-165">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="a44b0-166">Vzorový kód používá výchozí "/ signalr" adresa URL k připojení do služby SignalR.</span><span class="sxs-lookup"><span data-stu-id="a44b0-166">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="a44b0-167">Informace o tom, jak určit různé základní adresu URL najdete v tématu [ASP.NET pokyny k rozhraní API Center SignalR - Server - /signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="a44b0-167">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="a44b0-168">`Start` Metoda provedena asynchronně.</span><span class="sxs-lookup"><span data-stu-id="a44b0-168">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="a44b0-169">Ujistěte se, že následující řádky kódu není můžete spustit až po vytvoření připojení, použijte `await` v technologii ASP.NET 4.5 asynchronní metodu nebo `.Wait()` v synchronní metodě.</span><span class="sxs-lookup"><span data-stu-id="a44b0-169">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="a44b0-170">Nepoužívejte `.Wait()` v klientovi WinRT.</span><span class="sxs-lookup"><span data-stu-id="a44b0-170">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="a44b0-171">Připojení mezi doménami z klienty prostředí Silverlight</span><span class="sxs-lookup"><span data-stu-id="a44b0-171">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="a44b0-172">Informace o tom, jak povolit připojení mezi doménami z klienty prostředí Silverlight naleznete v tématu [zpřístupnění služby k dispozici napříč hranicemi domén](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="a44b0-172">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="a44b0-173">Postup konfigurace připojení</span><span class="sxs-lookup"><span data-stu-id="a44b0-173">How to configure the connection</span></span>

<span data-ttu-id="a44b0-174">Než vytvoříte připojení, můžete určit kterékoli z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="a44b0-174">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="a44b0-175">Limit souběžných připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-175">Concurrent connections limit.</span></span>
- <span data-ttu-id="a44b0-176">Parametry řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="a44b0-176">Query string parameters.</span></span>
- <span data-ttu-id="a44b0-177">Metoda přenosu.</span><span class="sxs-lookup"><span data-stu-id="a44b0-177">The transport method.</span></span>
- <span data-ttu-id="a44b0-178">Hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a44b0-178">HTTP headers.</span></span>
- <span data-ttu-id="a44b0-179">Klientské certifikáty.</span><span class="sxs-lookup"><span data-stu-id="a44b0-179">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="a44b0-180">Jak nastavit maximální počet souběžných připojení v klientech WPF</span><span class="sxs-lookup"><span data-stu-id="a44b0-180">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="a44b0-181">V WPF klienty bude pravděpodobně zvýšit maximální počet souběžných připojení z jeho výchozí hodnotu 2.</span><span class="sxs-lookup"><span data-stu-id="a44b0-181">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="a44b0-182">Doporučená hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="a44b0-182">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="a44b0-183">Další informace najdete v tématu [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="a44b0-183">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="a44b0-184">Určení parametrů řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="a44b0-184">How to specify query string parameters</span></span>

<span data-ttu-id="a44b0-185">Pokud chcete odesílat data do serveru, když se klient připojí, můžete přidat parametry řetězce dotazu do objekt připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-185">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="a44b0-186">Následující příklad ukazuje, jak nastavit parametr řetězce dotazu v klientském kódu.</span><span class="sxs-lookup"><span data-stu-id="a44b0-186">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="a44b0-187">Následující příklad znázorňuje způsob čtení parametru řetězce dotazu v serverovém kódu.</span><span class="sxs-lookup"><span data-stu-id="a44b0-187">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="a44b0-188">Jak určit metodu přenosu</span><span class="sxs-lookup"><span data-stu-id="a44b0-188">How to specify the transport method</span></span>

<span data-ttu-id="a44b0-189">Jako součást procesu připojování klienta SignalR obvykle vyjedná se serverem a určit nejlepší přenos, který je podporovaný server i klient.</span><span class="sxs-lookup"><span data-stu-id="a44b0-189">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="a44b0-190">Pokud již víte, jaké přenosu, kterou chcete použít, můžete obejít tento proces vyjednávání.</span><span class="sxs-lookup"><span data-stu-id="a44b0-190">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="a44b0-191">K určení metodu přenosu, předejte objekt transport metodě Start.</span><span class="sxs-lookup"><span data-stu-id="a44b0-191">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="a44b0-192">Následující příklad ukazuje, jak zadat metodu přenosu v klientském kódu.</span><span class="sxs-lookup"><span data-stu-id="a44b0-192">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="a44b0-193">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) obor názvů obsahuje následující třídy, které můžete použít k určení přenos.</span><span class="sxs-lookup"><span data-stu-id="a44b0-193">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="a44b0-194">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="a44b0-194">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="a44b0-195">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="a44b0-195">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="a44b0-196">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (dostupné pouze v případě serverů i klientů pomocí rozhraní .NET 4.5.)</span><span class="sxs-lookup"><span data-stu-id="a44b0-196">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="a44b0-197">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (automaticky vybere nejlepší přenos, který podporuje klient i server.</span><span class="sxs-lookup"><span data-stu-id="a44b0-197">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="a44b0-198">Toto je výchozí přenos.</span><span class="sxs-lookup"><span data-stu-id="a44b0-198">This is the default transport.</span></span> <span data-ttu-id="a44b0-199">Tuto hodnotu na předání `Start` metoda má stejný účinek jako ne vyhovující co.)</span><span class="sxs-lookup"><span data-stu-id="a44b0-199">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="a44b0-200">Přenos ForeverFrame není zahrnuta v tomto seznamu, protože se používá pouze pomocí prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="a44b0-200">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="a44b0-201">Informace o tom, jak zkontrolovat metodu přenosu v serverovém kódu najdete v tématu [ASP.NET pokyny k rozhraní API Center SignalR - Server - jak získat informace o klientovi z kontextové vlastnosti](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="a44b0-201">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="a44b0-202">Další informace o přenosy a náhrad najdete v tématu [Úvod ke knihovně SignalR – přenosy a náhrad](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="a44b0-202">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="a44b0-203">Jak zadat hlavičky protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="a44b0-203">How to specify HTTP headers</span></span>

<span data-ttu-id="a44b0-204">K nastavení hlavičky protokolu HTTP, použijte `Headers` vlastnost na objekt připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-204">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="a44b0-205">Následující příklad ukazuje, jak přidat hlavičku protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a44b0-205">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="a44b0-206">Určení klientských certifikátů</span><span class="sxs-lookup"><span data-stu-id="a44b0-206">How to specify client certificates</span></span>

<span data-ttu-id="a44b0-207">Chcete-li přidat klientské certifikáty, použijte `AddClientCertificate` metodu na objekt připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-207">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="a44b0-208">Jak vytvořit proxy server rozbočovače</span><span class="sxs-lookup"><span data-stu-id="a44b0-208">How to create the Hub proxy</span></span>

<span data-ttu-id="a44b0-209">Aby bylo možné definovat metody na straně klienta, která centrum může volat ze serveru a volání metod rozbočovače na serveru, vytvořit proxy server rozbočovače voláním `CreateHubProxy` na objekt připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-209">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="a44b0-210">Řetězec předáním `CreateHubProxy` je název třídy rozbočovače nebo název určený `HubName` atribut, pokud bylo použito na serveru.</span><span class="sxs-lookup"><span data-stu-id="a44b0-210">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="a44b0-211">Shoda názvů nerozlišuje.</span><span class="sxs-lookup"><span data-stu-id="a44b0-211">Name matching is case-insensitive.</span></span>

<span data-ttu-id="a44b0-212">**Třída rozbočovače na serveru**</span><span class="sxs-lookup"><span data-stu-id="a44b0-212">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="a44b0-213">**Vytvořit proxy klienta pro rozbočovač třídu**</span><span class="sxs-lookup"><span data-stu-id="a44b0-213">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="a44b0-214">Pokud uspořádání vaší třídy centra s `HubName` atribut, použijte tento název.</span><span class="sxs-lookup"><span data-stu-id="a44b0-214">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="a44b0-215">**Třída rozbočovače na serveru**</span><span class="sxs-lookup"><span data-stu-id="a44b0-215">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="a44b0-216">**Vytvořit proxy klienta pro rozbočovač třídu**</span><span class="sxs-lookup"><span data-stu-id="a44b0-216">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="a44b0-217">Při volání `HubConnection.CreateHubProxy` víckrát se stejným `hubName`, získáte stejné mezipaměti `IHubProxy` objektu.</span><span class="sxs-lookup"><span data-stu-id="a44b0-217">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="a44b0-218">Definování metody na straně klienta, která může volat na serveru</span><span class="sxs-lookup"><span data-stu-id="a44b0-218">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="a44b0-219">Chcete-li definovat metodu, která může volat na serveru, používat proxy server, na `On` metody pro registraci obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="a44b0-219">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="a44b0-220">Shoda názvu metody je velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="a44b0-220">Method name matching is case-insensitive.</span></span> <span data-ttu-id="a44b0-221">Například `Clients.All.UpdateStockPrice` na serveru spustí `updateStockPrice`, `updatestockprice`, nebo `UpdateStockPrice` na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="a44b0-221">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="a44b0-222">Různé klientské platformy mají různé požadavky na jak psát kód, metoda pro aktualizaci uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a44b0-222">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="a44b0-223">Příklady uvedené jsou pro klienty WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="a44b0-223">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="a44b0-224">WPF, Silverlight a příklady aplikací konzoly jsou k dispozici v [samostatné části dále v tomto tématu](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="a44b0-224">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="a44b0-225">Metody bez parametrů</span><span class="sxs-lookup"><span data-stu-id="a44b0-225">Methods without parameters</span></span>

<span data-ttu-id="a44b0-226">Pokud metoda, že zpracování nemá žádné parametry, použijte přetížení obecné `On` metody:</span><span class="sxs-lookup"><span data-stu-id="a44b0-226">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="a44b0-227">**Volání metody bez parametrů kódu serveru**</span><span class="sxs-lookup"><span data-stu-id="a44b0-227">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="a44b0-228">**WinRT klientský kód pro metodu volat ze serveru bez parametrů ([WPF a Silverlight příklady dále v tomto tématu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="a44b0-228">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="a44b0-229">Metody s parametry, určující typy parametrů</span><span class="sxs-lookup"><span data-stu-id="a44b0-229">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="a44b0-230">Pokud jste zpracování metody obsahuje parametry, zadejte typy parametrů jako obecné typy `On` metody.</span><span class="sxs-lookup"><span data-stu-id="a44b0-230">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="a44b0-231">Existují přetížení obecné `On` metoda vám umožní určit až na 8 parametry (4 ve Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="a44b0-231">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="a44b0-232">V následujícím příkladu je jeden parametr odeslán `UpdateStockPrice` metody.</span><span class="sxs-lookup"><span data-stu-id="a44b0-232">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="a44b0-233">**Volání metody s parametrem kódu serveru**</span><span class="sxs-lookup"><span data-stu-id="a44b0-233">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="a44b0-234">**Třída akcie použitý pro parametr**</span><span class="sxs-lookup"><span data-stu-id="a44b0-234">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="a44b0-235">**WinRT klientský kód pro metodu volat ze serveru s parametrem ([WPF a Silverlight příklady dále v tomto tématu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="a44b0-235">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="a44b0-236">Metody s parametry, dynamických objektů pro parametry</span><span class="sxs-lookup"><span data-stu-id="a44b0-236">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="a44b0-237">Jako alternativu k zadání parametrů jako obecné typy `On` metodu, můžete zadat parametry jako dynamické objekty:</span><span class="sxs-lookup"><span data-stu-id="a44b0-237">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="a44b0-238">**Volání metody s parametrem kódu serveru**</span><span class="sxs-lookup"><span data-stu-id="a44b0-238">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="a44b0-239">**Třída akcie použitý pro parametr**</span><span class="sxs-lookup"><span data-stu-id="a44b0-239">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="a44b0-240">**WinRT klientský kód pro metodu volat ze serveru s parametrem, pomocí dynamického objektu pro parametr ([WPF a Silverlight příklady dále v tomto tématu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="a44b0-240">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="a44b0-241">Jak odebrat obslužnou rutinu</span><span class="sxs-lookup"><span data-stu-id="a44b0-241">How to remove a handler</span></span>

<span data-ttu-id="a44b0-242">Chcete-li odebrat obslužnou rutinu, zavolejte jeho `Dispose` metoda.</span><span class="sxs-lookup"><span data-stu-id="a44b0-242">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="a44b0-243">**Klientský kód pro metodu volat ze serveru**</span><span class="sxs-lookup"><span data-stu-id="a44b0-243">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="a44b0-244">**Klientský kód odebrat obslužnou rutinu**</span><span class="sxs-lookup"><span data-stu-id="a44b0-244">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="a44b0-245">Volání metody serveru z klienta</span><span class="sxs-lookup"><span data-stu-id="a44b0-245">How to call server methods from the client</span></span>

<span data-ttu-id="a44b0-246">Chcete-li volat metodu na serveru, použijte `Invoke` metodu na proxy server rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="a44b0-246">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="a44b0-247">Pokud metoda server nemá žádnou návratovou hodnotu, použijte přetížení obecné `Invoke` metody.</span><span class="sxs-lookup"><span data-stu-id="a44b0-247">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="a44b0-248">**Serverový kód pro metodu, která nemá žádnou návratovou hodnotu**</span><span class="sxs-lookup"><span data-stu-id="a44b0-248">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="a44b0-249">**Klientský kód volá metodu, která nemá žádnou návratovou hodnotu**</span><span class="sxs-lookup"><span data-stu-id="a44b0-249">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="a44b0-250">Pokud metoda serveru má návratovou hodnotu, určit návratový typ jako generický typ `Invoke` metody.</span><span class="sxs-lookup"><span data-stu-id="a44b0-250">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="a44b0-251">**Serverový kód pro metodu, nemá návratovou hodnotu, která přebírá parametr komplexního typu**</span><span class="sxs-lookup"><span data-stu-id="a44b0-251">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="a44b0-252">**Třída akcie použitý pro parametr a vrátí hodnotu**</span><span class="sxs-lookup"><span data-stu-id="a44b0-252">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="a44b0-253">**Klientský kód volá metodu, nemá návratovou hodnotu, která přebírá parametr komplexního typu, v asynchronní metodě technologie ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="a44b0-253">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="a44b0-254">**Klientský kód volá metodu, nemá návratovou hodnotu, která přebírá parametr komplexního typu, v synchronní metodě**</span><span class="sxs-lookup"><span data-stu-id="a44b0-254">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="a44b0-255">`Invoke` Metoda asynchronně provede a vrátí `Task` objektu.</span><span class="sxs-lookup"><span data-stu-id="a44b0-255">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="a44b0-256">Pokud nezadáte `await` nebo `.Wait()`, dalším řádku kódu se provedou předtím, než metody, která je zapotřebí vyvolat neskončí.</span><span class="sxs-lookup"><span data-stu-id="a44b0-256">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="a44b0-257">Zpracování událostí doby platnosti</span><span class="sxs-lookup"><span data-stu-id="a44b0-257">How to handle connection lifetime events</span></span>

<span data-ttu-id="a44b0-258">Funkce SignalR poskytuje následující připojení události doby života, které dokáže zpracovat:</span><span class="sxs-lookup"><span data-stu-id="a44b0-258">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="a44b0-259">`Received`: Vyvolá se při přijetí žádná data připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-259">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="a44b0-260">Poskytuje přijatá data.</span><span class="sxs-lookup"><span data-stu-id="a44b0-260">Provides the received data.</span></span>
- <span data-ttu-id="a44b0-261">`ConnectionSlow`: Vyvolá, když klient zjistí pomalý nebo často vyřazení připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-261">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="a44b0-262">`Reconnecting`: Vyvolá se při přenosu začne znovu obnovovat.</span><span class="sxs-lookup"><span data-stu-id="a44b0-262">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="a44b0-263">`Reconnected`: Vyvolá se při přenosu má připojen.</span><span class="sxs-lookup"><span data-stu-id="a44b0-263">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="a44b0-264">`StateChanged`: Vyvolá se při změně stavu připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-264">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="a44b0-265">Poskytuje stav starý a nový stav.</span><span class="sxs-lookup"><span data-stu-id="a44b0-265">Provides the old state and the new state.</span></span> <span data-ttu-id="a44b0-266">Informace o připojení najdete v části hodnoty stavu [ConnectionState výčet](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="a44b0-266">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="a44b0-267">`Closed`: Vyvolá se při připojení se odpojil.</span><span class="sxs-lookup"><span data-stu-id="a44b0-267">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="a44b0-268">Například pokud chcete zobrazit varovné zprávy o chybách, které nejsou závažné, ale způsobit problémy s přerušovaným připojením, například jako pomalost nebo častému odstranění připojení, zpracování `ConnectionSlow` událostí.</span><span class="sxs-lookup"><span data-stu-id="a44b0-268">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="a44b0-269">Další informace najdete v tématu [principy a zpracování událostí doby platnosti v knihovně SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="a44b0-269">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="a44b0-270">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="a44b0-270">How to handle errors</span></span>

<span data-ttu-id="a44b0-271">Pokud není explicitně povolit podrobné chybové zprávy na serveru, obsahuje objekt výjimky, která vrací SignalR po chybě minimální informace o této chybě.</span><span class="sxs-lookup"><span data-stu-id="a44b0-271">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="a44b0-272">Například, pokud je volání `newContosoChatMessage` selže, chybová zpráva v objektu chyba obsahuje "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" podrobné chybové zprávy pro klienty v produkčním prostředí se nedoporučuje z bezpečnostních důvodů, ale pokud chcete povolit podrobné chybové zprávy pro odesílání na serveru pro účely odstraňování potíží, použijte následující kód.</span><span class="sxs-lookup"><span data-stu-id="a44b0-272">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="a44b0-273">Zpracování chyb, které vyvolává SignalR, můžete přidat obslužnou rutinu pro `Error` události u objektu připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-273">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="a44b0-274">Zpracování chyb z volání metod, zabalte kód v bloku try-catch.</span><span class="sxs-lookup"><span data-stu-id="a44b0-274">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="a44b0-275">Jak povolit protokolování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="a44b0-275">How to enable client-side logging</span></span>

<span data-ttu-id="a44b0-276">Chcete-li povolit protokolování na straně klienta, nastavte `TraceLevel` a `TraceWriter` vlastnosti objektu připojení.</span><span class="sxs-lookup"><span data-stu-id="a44b0-276">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="a44b0-277">WPF, Silverlight a Konzolová aplikace ukázky kódu pro metody klienta, které můžete volat na serveru</span><span class="sxs-lookup"><span data-stu-id="a44b0-277">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="a44b0-278">Ukázky kódu je uvedeno výše pro definování metody klienta, které můžete volat serveru platí pro klienty WinRT.</span><span class="sxs-lookup"><span data-stu-id="a44b0-278">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="a44b0-279">Následující ukázky ukazují ekvivalentní kód pro WPF, Silverlight a klienty aplikace konzoly.</span><span class="sxs-lookup"><span data-stu-id="a44b0-279">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="a44b0-280">Metody bez parametrů</span><span class="sxs-lookup"><span data-stu-id="a44b0-280">Methods without parameters</span></span>

<span data-ttu-id="a44b0-281">**WPF klientský kód pro metodu s názvem ze serveru bez parametrů**</span><span class="sxs-lookup"><span data-stu-id="a44b0-281">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="a44b0-282">**Kód klienta programu Silverlight pro metodu s názvem ze serveru bez parametrů**</span><span class="sxs-lookup"><span data-stu-id="a44b0-282">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="a44b0-283">**Konzole application klientský kód pro metodu s názvem ze serveru bez parametrů**</span><span class="sxs-lookup"><span data-stu-id="a44b0-283">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="a44b0-284">Metody s parametry, určující typy parametrů</span><span class="sxs-lookup"><span data-stu-id="a44b0-284">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="a44b0-285">**WPF klientský kód pro metodu volat ze serveru s parametrem**</span><span class="sxs-lookup"><span data-stu-id="a44b0-285">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="a44b0-286">**Kód klienta programu Silverlight pro metodu volat ze serveru s parametrem**</span><span class="sxs-lookup"><span data-stu-id="a44b0-286">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="a44b0-287">**Kód klienta konzolové aplikace pro metodu volat ze serveru s parametrem**</span><span class="sxs-lookup"><span data-stu-id="a44b0-287">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="a44b0-288">Metody s parametry, dynamických objektů pro parametry</span><span class="sxs-lookup"><span data-stu-id="a44b0-288">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="a44b0-289">**WPF klientský kód pro metodu volat ze serveru s parametrem, pomocí dynamického objektu pro parametr**</span><span class="sxs-lookup"><span data-stu-id="a44b0-289">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="a44b0-290">**Kód klienta programu Silverlight pro metodu volat ze serveru s parametrem, pomocí dynamického objektu pro parametr**</span><span class="sxs-lookup"><span data-stu-id="a44b0-290">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="a44b0-291">**Kód klienta konzolové aplikace pro metodu volat ze serveru s parametrem, pomocí dynamického objektu pro parametr**</span><span class="sxs-lookup"><span data-stu-id="a44b0-291">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
