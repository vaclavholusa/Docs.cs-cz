---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: "Povolení trasování SignalR | Microsoft Docs"
author: tfitzmac
description: "Tento dokument popisuje, jak povolit a konfigurovat trasování pro SignalR servery a klienty. Trasování umožňuje zobrazit diagnostické informace o události..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: ac979acf162084a195bb769f842e77ad2498c7f3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="2a799-104">Povolení trasování SignalR</span><span class="sxs-lookup"><span data-stu-id="2a799-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="2a799-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2a799-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2a799-106">Tento dokument popisuje, jak povolit a konfigurovat trasování pro SignalR servery a klienty.</span><span class="sxs-lookup"><span data-stu-id="2a799-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="2a799-107">Trasování umožňuje zobrazit diagnostické informace o událostech v aplikaci SignalR.</span><span class="sxs-lookup"><span data-stu-id="2a799-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
> 
> <span data-ttu-id="2a799-108">Toto téma byl původně zapsán pomocí Patrik Fletcher.</span><span class="sxs-lookup"><span data-stu-id="2a799-108">This topic was originally written by Patrick Fletcher.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2a799-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="2a799-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="2a799-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2a799-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="2a799-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="2a799-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="2a799-112">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="2a799-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="2a799-113">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="2a799-113">Questions and comments</span></span>
> 
> <span data-ttu-id="2a799-114">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="2a799-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="2a799-115">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="2a799-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="2a799-116">Pokud je povoleno sledování, vytvoří aplikace SignalR položky protokolu událostí.</span><span class="sxs-lookup"><span data-stu-id="2a799-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="2a799-117">Z klienta a server může protokolovat události.</span><span class="sxs-lookup"><span data-stu-id="2a799-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="2a799-118">Trasování na připojení k serveru protokoly, zprostředkovatele o škálování a události sběrnice zpráv.</span><span class="sxs-lookup"><span data-stu-id="2a799-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="2a799-119">Trasování událostí připojení protokolů klienta.</span><span class="sxs-lookup"><span data-stu-id="2a799-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="2a799-120">V systému SignalR 2.1 nebo novější protokolů trasování v klientovi celý obsah zprávy volání rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="2a799-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="2a799-121">Obsah</span><span class="sxs-lookup"><span data-stu-id="2a799-121">Contents</span></span>

- [<span data-ttu-id="2a799-122">Povolení trasování na serveru</span><span class="sxs-lookup"><span data-stu-id="2a799-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="2a799-123">Protokolování událostí serveru do textových souborů</span><span class="sxs-lookup"><span data-stu-id="2a799-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="2a799-124">Protokolování serveru události do protokolu událostí</span><span class="sxs-lookup"><span data-stu-id="2a799-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="2a799-125">Povolení trasování v rozhraní .NET klienta (aplikace Windows Desktop)</span><span class="sxs-lookup"><span data-stu-id="2a799-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="2a799-126">Protokolování událostí klienta pro stolní počítače do konzoly nástroje</span><span class="sxs-lookup"><span data-stu-id="2a799-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="2a799-127">Protokolování událostí plochy klienta do textového souboru</span><span class="sxs-lookup"><span data-stu-id="2a799-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="2a799-128">Povolení trasování v klientech Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="2a799-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="2a799-129">Protokolování událostí pro Windows Phone klienta do uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="2a799-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="2a799-130">Protokolování událostí pro Windows Phone klienta ke konzole ladění</span><span class="sxs-lookup"><span data-stu-id="2a799-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="2a799-131">Povolení trasování v klientovi JavaScript</span><span class="sxs-lookup"><span data-stu-id="2a799-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="2a799-132">Povolení trasování na serveru</span><span class="sxs-lookup"><span data-stu-id="2a799-132">Enabling tracing on the server</span></span>

<span data-ttu-id="2a799-133">Povolení trasování na serveru v rámci konfiguračního souboru aplikace (App.config nebo Web.config v závislosti na typu projektu.) Zadáte, které kategorie události do protokolu.</span><span class="sxs-lookup"><span data-stu-id="2a799-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="2a799-134">V konfiguračním souboru, můžete také určit, zda chcete protokolovat události do textového souboru, protokol událostí systému Windows nebo vlastního protokolu pomocí implementace [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="2a799-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="2a799-135">Kategorie události serveru zahrnují následující řazení zpráv:</span><span class="sxs-lookup"><span data-stu-id="2a799-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="2a799-136">Zdroj</span><span class="sxs-lookup"><span data-stu-id="2a799-136">Source</span></span> | <span data-ttu-id="2a799-137">Zprávy</span><span class="sxs-lookup"><span data-stu-id="2a799-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="2a799-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="2a799-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="2a799-139">Instalace zprostředkovatele služby SQL sběrnice zpráv o škálování, operace databáze, chyby a události vypršení časového limitu</span><span class="sxs-lookup"><span data-stu-id="2a799-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="2a799-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="2a799-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="2a799-141">Vytváření tématu zprostředkovatele škálování sběrnice služby a předplatného, chyb a zasílání zpráv události</span><span class="sxs-lookup"><span data-stu-id="2a799-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="2a799-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="2a799-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="2a799-143">Události zprostředkovatele připojení, odpojení a chybové škálování redis</span><span class="sxs-lookup"><span data-stu-id="2a799-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="2a799-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="2a799-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="2a799-145">Události zasílání zpráv o škálování</span><span class="sxs-lookup"><span data-stu-id="2a799-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="2a799-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="2a799-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="2a799-147">Přenos protokolu WebSocket připojení, odpojení, zasílání zpráv a chybové události</span><span class="sxs-lookup"><span data-stu-id="2a799-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="2a799-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="2a799-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="2a799-149">ServerSentEvents přenosu připojení, odpojení, zasílání zpráv a chybové události</span><span class="sxs-lookup"><span data-stu-id="2a799-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="2a799-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="2a799-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="2a799-151">ForeverFrame přenos připojení, odpojení, zasílání zpráv a chybové události</span><span class="sxs-lookup"><span data-stu-id="2a799-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="2a799-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="2a799-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="2a799-153">LongPolling přenos připojení, odpojení, zasílání zpráv a chybové události</span><span class="sxs-lookup"><span data-stu-id="2a799-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="2a799-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="2a799-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="2a799-155">Události připojení, odpojení a keepalive přenosu</span><span class="sxs-lookup"><span data-stu-id="2a799-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="2a799-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="2a799-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="2a799-157">Události zjišťování rozbočovače</span><span class="sxs-lookup"><span data-stu-id="2a799-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="2a799-158">Protokolování událostí serveru do textových souborů</span><span class="sxs-lookup"><span data-stu-id="2a799-158">Logging server events to text files</span></span>

<span data-ttu-id="2a799-159">Následující kód ukazuje, jak povolit trasování pro každou kategorii událostí.</span><span class="sxs-lookup"><span data-stu-id="2a799-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="2a799-160">Tato ukázka konfiguruje aplikaci, do protokolu událostí textových souborů.</span><span class="sxs-lookup"><span data-stu-id="2a799-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="2a799-161">**Kód XML serveru pro povolení trasování**</span><span class="sxs-lookup"><span data-stu-id="2a799-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="2a799-162">Ve výše, kódu `SignalRSwitch` položka určuje [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) používá pro událostí odeslaných do zadaného protokolu.</span><span class="sxs-lookup"><span data-stu-id="2a799-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="2a799-163">V takovém případě je nastavený na `Verbose` to znamená, všechny ladění a trasování zprávy se zaznamenávají.</span><span class="sxs-lookup"><span data-stu-id="2a799-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="2a799-164">Následující výstup zobrazuje záznamy z `transports.log.txt` soubor pro aplikaci pomocí konfiguračního souboru výše.</span><span class="sxs-lookup"><span data-stu-id="2a799-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="2a799-165">Zobrazuje nové připojení, odebrané připojení a přenosu prezenční signál události.</span><span class="sxs-lookup"><span data-stu-id="2a799-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="2a799-166">Protokolování serveru události do protokolu událostí</span><span class="sxs-lookup"><span data-stu-id="2a799-166">Logging server events to the event log</span></span>

<span data-ttu-id="2a799-167">Chcete-li protokolovat události do protokolu událostí, nikoli textového souboru, změňte hodnoty pro položky v `sharedListeners` uzlu.</span><span class="sxs-lookup"><span data-stu-id="2a799-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="2a799-168">Následující kód ukazuje, jak se přihlásit do protokolu událostí události serveru:</span><span class="sxs-lookup"><span data-stu-id="2a799-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="2a799-169">**Serverový kód XML pro protokolování události do protokolu událostí**</span><span class="sxs-lookup"><span data-stu-id="2a799-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="2a799-170">Události se zaznamenávají v protokolu aplikace a jsou k dispozici prostřednictvím v prohlížeči událostí, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="2a799-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Prohlížeč událostí zobrazuje protokoly SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="2a799-172">Při použití protokolu událostí, **TraceLevel** k **chyba** zachovat spravovat počet zpráv.</span><span class="sxs-lookup"><span data-stu-id="2a799-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="2a799-173">Povolení trasování v rozhraní .NET klienta (aplikace Windows Desktop)</span><span class="sxs-lookup"><span data-stu-id="2a799-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="2a799-174">Klient .NET může protokolovat události konzole, s textovým souborem, nebo vlastní protokol pomocí implementace [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="2a799-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="2a799-175">Povolení protokolování v rozhraní .NET klienta, nastavte jiné připojení `TraceLevel` vlastnosti [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) hodnotu a `TraceWriter` vlastnost, která má platnou [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span><span class="sxs-lookup"><span data-stu-id="2a799-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="2a799-176">Protokolování událostí klienta pro stolní počítače do konzoly nástroje</span><span class="sxs-lookup"><span data-stu-id="2a799-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="2a799-177">Následující kód C# ukazuje, jak do protokolu událostí v rozhraní .NET klienta do konzoly:</span><span class="sxs-lookup"><span data-stu-id="2a799-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="2a799-178">Protokolování událostí plochy klienta do textového souboru</span><span class="sxs-lookup"><span data-stu-id="2a799-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="2a799-179">Následující kód C# ukazuje, jak do protokolu událostí v rozhraní .NET klienta do textového souboru:</span><span class="sxs-lookup"><span data-stu-id="2a799-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="2a799-180">Následující výstup zobrazuje záznamy z `ClientLog.txt` soubor pro aplikaci pomocí konfiguračního souboru výše.</span><span class="sxs-lookup"><span data-stu-id="2a799-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="2a799-181">Zobrazuje klient pro připojování k serveru a rozbočovače volání metody klienta s názvem `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="2a799-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="2a799-182">Povolení trasování v klientech Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="2a799-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="2a799-183">SignalR aplikací pro aplikace Windows Phone pomocí stejného klienta rozhraní .NET jako desktopové aplikace, ale [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) a zápis do souboru s [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2a799-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="2a799-184">Místo toho je potřeba vytvořit vlastní provádění [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) pro trasování.</span><span class="sxs-lookup"><span data-stu-id="2a799-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span> 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="2a799-185">Protokolování událostí pro Windows Phone klienta do uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="2a799-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="2a799-186">[Základu kódu SignalR](https://github.com/SignalR/SignalR/archive/master.zip) zahrnuje Windows Phone vzorku, který zapíše výstup trasování do [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) pomocí vlastní [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementace volá `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="2a799-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="2a799-187">Tato třída lze nalézt v **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** projektu.</span><span class="sxs-lookup"><span data-stu-id="2a799-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="2a799-188">Při vytváření instance `TextBlockWriter`, předejte aktuální [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) kde se vytvoří [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) pro trasování výstup:</span><span class="sxs-lookup"><span data-stu-id="2a799-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="2a799-189">Výstup trasování budou zapsány do nového [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) vytvořené v [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) předán v:</span><span class="sxs-lookup"><span data-stu-id="2a799-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="2a799-190">Protokolování událostí pro Windows Phone klienta ke konzole ladění</span><span class="sxs-lookup"><span data-stu-id="2a799-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="2a799-191">K odeslání výstupu konzoly ladění, místo uživatelského rozhraní, vytvořte implementaci [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) , zapíše se do okna ladění a přiřaďte ji k připojení k [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) vlastnost:</span><span class="sxs-lookup"><span data-stu-id="2a799-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="2a799-192">Informace o trasování budou zapsány do okna ladění v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="2a799-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="2a799-193">Povolení trasování v klientovi JavaScript</span><span class="sxs-lookup"><span data-stu-id="2a799-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="2a799-194">Chcete-li povolit protokolování na straně klienta na připojení, nastavte `logging` vlastnost u objektu připojení před voláním `start` metodu pro vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="2a799-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="2a799-195">**Kód JavaScript klienta pro povolení trasování do konzoly prohlížeče (s vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="2a799-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="2a799-196">**Kód JavaScript klienta pro povolení trasování do konzoly prohlížeče (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="2a799-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="2a799-197">Pokud je povoleno sledování, JavaScript klienta protokoluje události do konzoly prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2a799-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="2a799-198">Pro přístup do konzoly prohlížeče, najdete v části [monitorování přenosy](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="2a799-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="2a799-199">Následující snímek obrazovky ukazuje SignalR JavaScript klienta s povolené trasování.</span><span class="sxs-lookup"><span data-stu-id="2a799-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="2a799-200">V konzole prohlížeče zobrazuje připojení a rozbočovače vyvolání události:</span><span class="sxs-lookup"><span data-stu-id="2a799-200">It shows connection and hub invocation events in the browser console:</span></span>

![Události trasování SignalR v konzole prohlížeče](enabling-signalr-tracing/_static/image4.png)
