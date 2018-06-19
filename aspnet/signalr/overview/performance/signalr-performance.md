---
uid: signalr/overview/performance/signalr-performance
title: SignalR výkonu | Microsoft Docs
author: pfletcher
description: SignalR výkonu
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 4468ee8031afccca847db67bd4b5b263f0a2c5ac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28041862"
---
<a name="signalr-performance"></a><span data-ttu-id="610c7-103">SignalR výkonu</span><span class="sxs-lookup"><span data-stu-id="610c7-103">SignalR Performance</span></span>
====================
<span data-ttu-id="610c7-104">podle [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="610c7-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="610c7-105">Toto téma popisuje postup návrhu pro měření a zlepšit výkon v aplikaci SignalR.</span><span class="sxs-lookup"><span data-stu-id="610c7-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="610c7-106">Verze softwaru použitým v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="610c7-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="610c7-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="610c7-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="610c7-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="610c7-108">.NET 4.5</span></span>
> - <span data-ttu-id="610c7-109">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="610c7-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="610c7-110">Předchozí verze tohoto tématu</span><span class="sxs-lookup"><span data-stu-id="610c7-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="610c7-111">Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="610c7-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="610c7-112">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="610c7-112">Questions and comments</span></span>
> 
> <span data-ttu-id="610c7-113">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="610c7-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="610c7-114">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="610c7-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="610c7-115">Poslední prezentace na SignalR výkonu a škálování, najdete v části [škálování webu v reálném čase pomocí funkce SignalR technologie ASP.NET](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="610c7-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="610c7-116">Toto téma obsahuje následující oddíly:</span><span class="sxs-lookup"><span data-stu-id="610c7-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="610c7-117">Aspekty návrhu</span><span class="sxs-lookup"><span data-stu-id="610c7-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="610c7-118">Optimalizace výkonu serveru SignalR</span><span class="sxs-lookup"><span data-stu-id="610c7-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="610c7-119">Řešení potíží s výkonem</span><span class="sxs-lookup"><span data-stu-id="610c7-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="610c7-120">Pomocí čítače výkonu SignalR</span><span class="sxs-lookup"><span data-stu-id="610c7-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="610c7-121">Pomocí dalších čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="610c7-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="610c7-122">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="610c7-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="610c7-123">Aspekty návrhu</span><span class="sxs-lookup"><span data-stu-id="610c7-123">Design considerations</span></span>

<span data-ttu-id="610c7-124">Tato část popisuje vzorů, které lze provádět při návrhu aplikace SignalR zajistit, že není právě výkonu omezován generování nepotřebné síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="610c7-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="610c7-125">Frekvence omezení zpráv</span><span class="sxs-lookup"><span data-stu-id="610c7-125">Throttling message frequency</span></span>

<span data-ttu-id="610c7-126">I v aplikaci, která odesílá zprávy s vysoká frekvence (jako například herní aplikací v reálném čase) není nutné odesílat zprávy o několik a sekundu většinu aplikací.</span><span class="sxs-lookup"><span data-stu-id="610c7-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="610c7-127">Snížit objem provozu, který generuje každého klienta, smyčku zpráv se dají implementovat, fronty a odesílá zprávy nejvýše často pevné sazby (tedy až počet zpráv odešle za sekundu, pokud jsou v té době v zprávy terval k odeslání).</span><span class="sxs-lookup"><span data-stu-id="610c7-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="610c7-128">Ukázkové aplikace, která omezí generovaný zprávy do určité míry (z klient i server), najdete v části [vysoká frekvence v reálném čase s SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="610c7-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="610c7-129">Snižuje velikost zprávy</span><span class="sxs-lookup"><span data-stu-id="610c7-129">Reducing message size</span></span>

<span data-ttu-id="610c7-130">Snížením velikosti serializovaných objektů můžete snížit velikost zprávy SignalR.</span><span class="sxs-lookup"><span data-stu-id="610c7-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="610c7-131">V serverovém kódu, pokud odesíláte objekt, který obsahuje vlastnosti, které není nutné přenášet, zabránit tyto vlastnosti serializována pomocí `JsonIgnore` atribut.</span><span class="sxs-lookup"><span data-stu-id="610c7-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="610c7-132">Názvy vlastností jsou také uloženy ve zprávě; názvy vlastností lze zkrátit pomocí `JsonProperty` atribut.</span><span class="sxs-lookup"><span data-stu-id="610c7-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="610c7-133">Následující ukázka kódu ukazuje, jak být vyloučena určitá vlastnost odeslání do klienta a jak tak, aby zkrátil názvy vlastností:</span><span class="sxs-lookup"><span data-stu-id="610c7-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="610c7-134">**Kód serveru .NET, která popisuje atribut JsonIgnore vyloučení dat z odesílány do klienta a atribut JsonProperty snížit velikost zprávy**</span><span class="sxs-lookup"><span data-stu-id="610c7-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="610c7-135">Chcete-li zachovat čitelnost / udržovatelnosti v kódu klienta, názvy zkrácené vlastností, které může být změnu mapování k lidské friendly názvy, po doručení zprávy.</span><span class="sxs-lookup"><span data-stu-id="610c7-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="610c7-136">Následující příklad kódu ukazuje jeden možný způsob přemapování zkrácení názvy chcete delší, tím, že definujete kontrakt zprávy (mapování) a pomocí `reMap` funkce, která se týkají kontrakt třídu optimalizované zpráv:</span><span class="sxs-lookup"><span data-stu-id="610c7-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="610c7-137">**Kód jazyka JavaScript na straně klienta, který změní mapování zkrátit názvy vlastností, abyste čitelná pro člověka názvy**</span><span class="sxs-lookup"><span data-stu-id="610c7-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="610c7-138">Názvy lze zkrátit ve zprávách z klienta na server taky stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="610c7-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="610c7-139">Snižuje nároky na paměť (který je množství paměti pro zprávu) zprávy mohou objekt rovněž zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="610c7-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="610c7-140">Například pokud plný rozsah `int` není potřeba, `short` nebo `byte` můžete místo toho použít.</span><span class="sxs-lookup"><span data-stu-id="610c7-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="610c7-141">Vzhledem k tomu, že zprávy jsou uloženy ve sběrnici zpráv v paměti serveru, zmenšení velikosti této zprávy můžete také řeší problémy paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="610c7-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="610c7-142">Optimalizace výkonu serveru SignalR</span><span class="sxs-lookup"><span data-stu-id="610c7-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="610c7-143">Následující nastavení konfigurace slouží k vyladění vašeho serveru pro lepší výkon v aplikaci SignalR.</span><span class="sxs-lookup"><span data-stu-id="610c7-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="610c7-144">Obecné informace o tom, jak zlepšit výkon v aplikaci ASP.NET najdete v tématu [zlepšení výkonu technologie ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="610c7-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="610c7-145">**Nastavení konfigurace SignalR**</span><span class="sxs-lookup"><span data-stu-id="610c7-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="610c7-146">**DefaultMessageBufferSize**: ve výchozím nastavení, SignalR uchovává zprávy 1000 v paměti za rozbočovač za připojení.</span><span class="sxs-lookup"><span data-stu-id="610c7-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="610c7-147">Pokud se používají velké zprávy, to může způsobit problémy s pamětí, které můžete zmírnit po nastavení této hodnoty.</span><span class="sxs-lookup"><span data-stu-id="610c7-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="610c7-148">Toto nastavení může být nastavena v `Application_Start` obslužné rutiny události v aplikaci ASP.NET nebo v `Configuration` metoda třídy pro spuštění OWIN ve vlastním hostováním aplikaci.</span><span class="sxs-lookup"><span data-stu-id="610c7-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="610c7-149">Následující příklad ukazuje, jak snížit tato hodnota za účelem snížení nároky na paměť pro vaše aplikace za účelem snížení množství paměti serveru:</span><span class="sxs-lookup"><span data-stu-id="610c7-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="610c7-150">**Kód serveru .NET v souboru Startup.cs pro snížení výchozí velikost vyrovnávací paměti zpráv**</span><span class="sxs-lookup"><span data-stu-id="610c7-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="610c7-151">**Nastavení konfigurace služby IIS**</span><span class="sxs-lookup"><span data-stu-id="610c7-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="610c7-152">**Maximální počet souběžných požadavků na aplikaci**: zvýšení počtu souběžných IIS požadavky zvýší prostředky serveru k dispozici pro obsluhovat požadavky.</span><span class="sxs-lookup"><span data-stu-id="610c7-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="610c7-153">Výchozí hodnota je 5 000; Pokud chcete zvýšit toto nastavení, spusťte následující příkazy v příkazovém řádku se zvýšenými oprávněními:</span><span class="sxs-lookup"><span data-stu-id="610c7-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="610c7-154">**ApplicationPool QueueLength**: Toto je maximální počet požadavků, ovladač Http.sys zařadí do fronty pro fond aplikací.</span><span class="sxs-lookup"><span data-stu-id="610c7-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="610c7-155">Při zaplnění fronty, nové požadavky obdrží odpověď 503 "Služba není k dispozici".</span><span class="sxs-lookup"><span data-stu-id="610c7-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="610c7-156">Výchozí hodnota je 1 000.</span><span class="sxs-lookup"><span data-stu-id="610c7-156">The default value is 1000.</span></span>

    <span data-ttu-id="610c7-157">Zkrátit délku fronty pracovního procesu ve fondu aplikací, který je hostitelem vaší aplikace bude šetřit prostředky paměti.</span><span class="sxs-lookup"><span data-stu-id="610c7-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="610c7-158">Další informace najdete v tématu [pro správu, optimalizaci a konfigurace fondů aplikací](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="610c7-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="610c7-159">**Nastavení konfigurace ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="610c7-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="610c7-160">Tato část obsahuje nastavení konfigurace, které lze nastavit v `aspnet.config` souboru.</span><span class="sxs-lookup"><span data-stu-id="610c7-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="610c7-161">Tento soubor se nachází v jednom ze dvou umístěních, v závislosti na platformě:</span><span class="sxs-lookup"><span data-stu-id="610c7-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="610c7-162">Nastavení ASP.NET, které může zvýšit výkon SignalR zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="610c7-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="610c7-163">**Maximální souběžných požadavků na jeden procesor**: zvýšením tohoto nastavení může zmírnit kritické body.</span><span class="sxs-lookup"><span data-stu-id="610c7-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="610c7-164">Chcete-li zvýšit toto nastavení, přidejte následující nastavení konfigurace na `aspnet.config` souboru:</span><span class="sxs-lookup"><span data-stu-id="610c7-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="610c7-165">**Limit fronty požadavků**: Pokud celkový počet připojení překročí `maxConcurrentRequestsPerCPU` nastavení, ASP.NET spustí omezování požadavků pomocí fronty.</span><span class="sxs-lookup"><span data-stu-id="610c7-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="610c7-166">Pokud chcete zvýšit velikost fronty, můžete zvýšit `requestQueueLimit` nastavení.</span><span class="sxs-lookup"><span data-stu-id="610c7-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="610c7-167">K tomu, přidejte následující nastavení konfigurace pro `processModel` uzlu v `config/machine.config` (místo `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="610c7-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="610c7-168">Řešení potíží s výkonem</span><span class="sxs-lookup"><span data-stu-id="610c7-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="610c7-169">Tato část popisuje způsoby jak najít kritické body ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="610c7-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="610c7-170">Ověření, že je použit protokol WebSocket</span><span class="sxs-lookup"><span data-stu-id="610c7-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="610c7-171">Zatímco SignalR používat celou řadu přenosů pro komunikaci mezi klientem a serverem, protokolu WebSocket nabízí využít významně zvýšit výkon a by měl použít, pokud ji podporuje klient a server.</span><span class="sxs-lookup"><span data-stu-id="610c7-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="610c7-172">Pokud chcete zjistit, pokud klient a server splňovat požadavky protokolu WebSocket, najdete v části [přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="610c7-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="610c7-173">Pokud chcete určit, jaký přenos se používá v aplikaci, můžete použít nástroje pro vývojáře prohlížeče a v protokolech zobrazíte, jaký přenos se používá pro připojení.</span><span class="sxs-lookup"><span data-stu-id="610c7-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="610c7-174">Informace o použití nástrojů pro vývoj prohlížeče v Internet Exploreru a Chrome, najdete v části [přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="610c7-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="610c7-175">Pomocí čítače výkonu SignalR</span><span class="sxs-lookup"><span data-stu-id="610c7-175">Using SignalR performance counters</span></span>

<span data-ttu-id="610c7-176">Tato část popisuje, jak povolit a používat čítače výkonu SignalR v nalezen `Microsoft.AspNet.SignalR.Utils` balíčku.</span><span class="sxs-lookup"><span data-stu-id="610c7-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="610c7-177">Instalace signalr.exe</span><span class="sxs-lookup"><span data-stu-id="610c7-177">Installing signalr.exe</span></span>

<span data-ttu-id="610c7-178">Čítače výkonu lze přidat na server názvem SignalR.exe nástroj.</span><span class="sxs-lookup"><span data-stu-id="610c7-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="610c7-179">Chcete-li nainstalovat tento nástroj, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="610c7-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="610c7-180">V aplikaci Visual Studio, vyberte **nástroje**, **Správce balíčků knihoven**, **spravovat balíčky NuGet pro řešení...**</span><span class="sxs-lookup"><span data-stu-id="610c7-180">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="610c7-181">Vyhledejte **signalr.utils**a vyberte možnost instalace.</span><span class="sxs-lookup"><span data-stu-id="610c7-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="610c7-182">Přijměte licenční smlouvu k instalaci balíčku.</span><span class="sxs-lookup"><span data-stu-id="610c7-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="610c7-183">SignalR.exe se nainstaluje pro `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="610c7-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="610c7-184">Instalace s SignalR.exe čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="610c7-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="610c7-185">Pokud chcete nainstalovat čítače výkonu SignalR, spusťte SignalR.exe v příkazovém řádku se zvýšenými s parametrem následující:</span><span class="sxs-lookup"><span data-stu-id="610c7-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="610c7-186">Chcete-li odebrat čítače výkonu SignalR, spusťte SignalR.exe v příkazovém řádku se zvýšenými s následující parametr:</span><span class="sxs-lookup"><span data-stu-id="610c7-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="610c7-187">Čítače výkonu SignalR</span><span class="sxs-lookup"><span data-stu-id="610c7-187">SignalR Performance counters</span></span>

<span data-ttu-id="610c7-188">Balíček nástroje nainstaluje následující čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="610c7-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="610c7-189">"Celkovou" čítače měření počtu událostí od poslední fond aplikací nebo server restartovat.</span><span class="sxs-lookup"><span data-stu-id="610c7-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="610c7-190">**Metriky připojení**</span><span class="sxs-lookup"><span data-stu-id="610c7-190">**Connection metrics**</span></span>

<span data-ttu-id="610c7-191">Následující metriky měření události životnost připojení, ke kterým dochází.</span><span class="sxs-lookup"><span data-stu-id="610c7-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="610c7-192">Další informace najdete v tématu [pochopení a zpracování události životnost připojení](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="610c7-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="610c7-193">**Připojení připojení**</span><span class="sxs-lookup"><span data-stu-id="610c7-193">**Connections Connected**</span></span>
- <span data-ttu-id="610c7-194">**Připojení znovu**</span><span class="sxs-lookup"><span data-stu-id="610c7-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="610c7-195">**Připojení, odpojení**</span><span class="sxs-lookup"><span data-stu-id="610c7-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="610c7-196">**Aktuální připojení**</span><span class="sxs-lookup"><span data-stu-id="610c7-196">**Connections Current**</span></span>

<span data-ttu-id="610c7-197">**Zpráva metriky**</span><span class="sxs-lookup"><span data-stu-id="610c7-197">**Message metrics**</span></span>

<span data-ttu-id="610c7-198">Následující metriky měření provoz zprávy generované SignalR.</span><span class="sxs-lookup"><span data-stu-id="610c7-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="610c7-199">**Celkový počet přijatých zprávy připojení**</span><span class="sxs-lookup"><span data-stu-id="610c7-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="610c7-200">**Celkový počet odesílají zprávy připojení**</span><span class="sxs-lookup"><span data-stu-id="610c7-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="610c7-201">**Přijaté zprávy připojení za sekundu**</span><span class="sxs-lookup"><span data-stu-id="610c7-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="610c7-202">**Připojení odeslaných zpráv za sekundu**</span><span class="sxs-lookup"><span data-stu-id="610c7-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="610c7-203">**Metriky sběrnice zpráv**</span><span class="sxs-lookup"><span data-stu-id="610c7-203">**Message bus metrics**</span></span>

<span data-ttu-id="610c7-204">Následující metriky měření provoz prostřednictvím interní sběrnice zpráv SignalR, fronty, ve kterém jsou umístěny všechny příchozí a odchozí zprávy SignalR.</span><span class="sxs-lookup"><span data-stu-id="610c7-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="610c7-205">Zpráva je **publikováno** při se odesílá nebo všesměrového vysílání.</span><span class="sxs-lookup"><span data-stu-id="610c7-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="610c7-206">A **odběratele** v tomto kontextu je předplatné na sběrnici zpráv; to musí být roven počtu klientů a samotný server.</span><span class="sxs-lookup"><span data-stu-id="610c7-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="610c7-207">**Přidělené pracovní** je komponenta, která odesílá data do active připojení, **zaneprázdněn pracovní** je ten, který je aktivně odesílání zprávy.</span><span class="sxs-lookup"><span data-stu-id="610c7-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="610c7-208">**Celkový počet přijatých zpráv sběrnice zpráv**</span><span class="sxs-lookup"><span data-stu-id="610c7-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="610c7-209">**Sběrnice zpráv zprávy přijaté za sekundu**</span><span class="sxs-lookup"><span data-stu-id="610c7-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="610c7-210">**Celkový počet publikovaných zprávy sběrnice zpráv**</span><span class="sxs-lookup"><span data-stu-id="610c7-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="610c7-211">**Sběrnice zpráv zprávy publikované za sekundu**</span><span class="sxs-lookup"><span data-stu-id="610c7-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="610c7-212">**Aktuální odběratele sběrnice zpráv**</span><span class="sxs-lookup"><span data-stu-id="610c7-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="610c7-213">**Celkový počet odběratele sběrnice zpráv**</span><span class="sxs-lookup"><span data-stu-id="610c7-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="610c7-214">**Odběratelé sběrnice zpráv za sekundu**</span><span class="sxs-lookup"><span data-stu-id="610c7-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="610c7-215">**Sběrnice zpráv přidělené pracovní procesy**</span><span class="sxs-lookup"><span data-stu-id="610c7-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="610c7-216">**Sběrnice zpráv vytížených pracovních procesů**</span><span class="sxs-lookup"><span data-stu-id="610c7-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="610c7-217">**Aktuální témata sběrnice zpráv**</span><span class="sxs-lookup"><span data-stu-id="610c7-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="610c7-218">**Chyba metriky**</span><span class="sxs-lookup"><span data-stu-id="610c7-218">**Error metrics**</span></span>

<span data-ttu-id="610c7-219">Následující metriky měření chybách vygenerovaných provoz zpráv SignalR.</span><span class="sxs-lookup"><span data-stu-id="610c7-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="610c7-220">**Centrum řešení** dojít k chybám, když nelze přeložit rozbočovače nebo metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="610c7-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="610c7-221">**Volání rozbočovače** chyby jsou výjimky vydané při vyvolání metody hub.</span><span class="sxs-lookup"><span data-stu-id="610c7-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="610c7-222">**Přenos** chyby jsou chyby připojení během požadavku nebo odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="610c7-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="610c7-223">**Chyb: Celkový počet všech**</span><span class="sxs-lookup"><span data-stu-id="610c7-223">**Errors: All Total**</span></span>
- <span data-ttu-id="610c7-224">**Chyby: Všechna za sekundu**</span><span class="sxs-lookup"><span data-stu-id="610c7-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="610c7-225">**Chyby: Celkový počet řešení rozbočovače**</span><span class="sxs-lookup"><span data-stu-id="610c7-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="610c7-226">**Chyby: Centrum řešení za sekundu**</span><span class="sxs-lookup"><span data-stu-id="610c7-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="610c7-227">**Chyby: Celkový počet volání rozbočovače**</span><span class="sxs-lookup"><span data-stu-id="610c7-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="610c7-228">**Chyby: Rozbočovače volání za sekundu**</span><span class="sxs-lookup"><span data-stu-id="610c7-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="610c7-229">**Chyb: Celkový počet přenosu**</span><span class="sxs-lookup"><span data-stu-id="610c7-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="610c7-230">**Chyby: Přenos/s**</span><span class="sxs-lookup"><span data-stu-id="610c7-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="610c7-231">**Metriky škálování.**</span><span class="sxs-lookup"><span data-stu-id="610c7-231">**Scaleout metrics**</span></span>

<span data-ttu-id="610c7-232">Následující metriky měření provoz a chyby vygenerované zprostředkovatelem škálování.</span><span class="sxs-lookup"><span data-stu-id="610c7-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="610c7-233">A **datového proudu** v tomto kontextu je jednotka škálování používá zprostředkovatel škálování; jde tabulky, pokud se používá SQL Server, téma, pokud se používá Service Bus a předplatné, pokud se používá Redis.</span><span class="sxs-lookup"><span data-stu-id="610c7-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="610c7-234">Každý datový proud zajišťuje uspořádaný operací čtení a zápisu; jeden datový proud je potenciální problémová místa škálování, takže snížit této kritický bod je možné zvýšit počet datových proudů.</span><span class="sxs-lookup"><span data-stu-id="610c7-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="610c7-235">Pokud používáte víc datových proudů, SignalR automaticky distribuovat zprávy (horizontálních) v těchto datových proudů způsobem, který zajistí, že zpráv odeslaných z daného připojení jsou v pořadí.</span><span class="sxs-lookup"><span data-stu-id="610c7-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="610c7-236">[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) nastavení ovládacích prvků délka fronty pro odesílání dat o škálování udržované SignalR.</span><span class="sxs-lookup"><span data-stu-id="610c7-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="610c7-237">Jeho nastavení na hodnotu větší než 0 umístí všechny zprávy ve frontě odeslání k odeslání po jednom nakonfigurovaných propojovací rozhraní systému zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="610c7-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="610c7-238">Pokud velikost fronty překročí nakonfigurovaná délka, následných výzev k odeslání bude okamžitě nezdaří s [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) dokud počet zpráv ve frontě je menší než nastavení znovu.</span><span class="sxs-lookup"><span data-stu-id="610c7-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="610c7-239">Služby Řízení front je ve výchozím nastavení zakázáno, protože implementované backplanes obvykle mají své vlastní služby Řízení front nebo řízení toku na místě.</span><span class="sxs-lookup"><span data-stu-id="610c7-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="610c7-240">V případě systému SQL Server sdružování připojení efektivně omezí počet zasílá děje v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="610c7-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="610c7-241">Ve výchozím nastavení, je použít pouze jeden datový proud pro SQL Server a Redis, pět datových proudů se používají pro Service Bus a služby Řízení front je zakázaná, ale tato nastavení lze změnit prostřednictvím konfigurace na serveru SQL a sběrnice:</span><span class="sxs-lookup"><span data-stu-id="610c7-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="610c7-242">**Rozhraní .NET serverový kód pro konfiguraci tabulky počet a fronty délku propojovací rozhraní systému SQL Server**</span><span class="sxs-lookup"><span data-stu-id="610c7-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="610c7-243">**Rozhraní .NET serverový kód pro konfiguraci tématu počet a fronty délku propojovací rozhraní systému Service Bus**</span><span class="sxs-lookup"><span data-stu-id="610c7-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="610c7-244">A **ukládání do vyrovnávací paměti** datový proud je ten, který má zadaný chybný stav; když je datový proud v chybovém stavu, všech zpráv odeslaných do propojovacího rozhraní se nezdaří, až do datového proudu už selhal.</span><span class="sxs-lookup"><span data-stu-id="610c7-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="610c7-245">**Délka fronty pro odeslání** je počet zpráv, které byly odeslány, ale ještě nebyla odeslána.</span><span class="sxs-lookup"><span data-stu-id="610c7-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="610c7-246">**Sběrnice zpráv o škálování zprávy přijaté za sekundu**</span><span class="sxs-lookup"><span data-stu-id="610c7-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="610c7-247">**Celkový počet datových proudů škálování.**</span><span class="sxs-lookup"><span data-stu-id="610c7-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="610c7-248">**Škálování datové proudy Open**</span><span class="sxs-lookup"><span data-stu-id="610c7-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="610c7-249">**Datové proudy škálování ukládání do vyrovnávací paměti**</span><span class="sxs-lookup"><span data-stu-id="610c7-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="610c7-250">**Celkový počet chyb škálování.**</span><span class="sxs-lookup"><span data-stu-id="610c7-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="610c7-251">**Škálování chyby/s**</span><span class="sxs-lookup"><span data-stu-id="610c7-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="610c7-252">**Délka fronty pro odesílání škálování.**</span><span class="sxs-lookup"><span data-stu-id="610c7-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="610c7-253">Další informace o co jsou tyto čítače měření najdete v tématu [SignalR škálování s Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="610c7-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="610c7-254">Pomocí dalších čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="610c7-254">Using other performance counters</span></span>

<span data-ttu-id="610c7-255">Následující čítače výkonu může být taky užitečný v monitorování výkonu aplikace.</span><span class="sxs-lookup"><span data-stu-id="610c7-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="610c7-256">**Paměť**</span><span class="sxs-lookup"><span data-stu-id="610c7-256">**Memory**</span></span>

- <span data-ttu-id="610c7-257">Využívání paměti rozhraním .NET CLR\\# bajtů ve všech haldách (pro w3wp)</span><span class="sxs-lookup"><span data-stu-id="610c7-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="610c7-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="610c7-258">**ASP.NET**</span></span>

- <span data-ttu-id="610c7-259">Aktuální ASP.NET\Requests</span><span class="sxs-lookup"><span data-stu-id="610c7-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="610c7-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="610c7-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="610c7-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="610c7-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="610c7-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="610c7-262">**CPU**</span></span>

- <span data-ttu-id="610c7-263">Information\Processor času procesoru</span><span class="sxs-lookup"><span data-stu-id="610c7-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="610c7-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="610c7-264">**TCP/IP**</span></span>

- <span data-ttu-id="610c7-265">TCPv6 nebo připojení</span><span class="sxs-lookup"><span data-stu-id="610c7-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="610c7-266">TCPv4 nebo připojení</span><span class="sxs-lookup"><span data-stu-id="610c7-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="610c7-267">**Web Service**</span><span class="sxs-lookup"><span data-stu-id="610c7-267">**Web Service**</span></span>

- <span data-ttu-id="610c7-268">Webová Service\Current připojení</span><span class="sxs-lookup"><span data-stu-id="610c7-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="610c7-269">Webová Service\Maximum připojení</span><span class="sxs-lookup"><span data-stu-id="610c7-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="610c7-270">**Dělení na vlákna**</span><span class="sxs-lookup"><span data-stu-id="610c7-270">**Threading**</span></span>

- <span data-ttu-id="610c7-271">Rozhraní .NET CLR zamkne a vlákna\\# aktuální logické vláken</span><span class="sxs-lookup"><span data-stu-id="610c7-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="610c7-272">Rozhraní .NET CLR zamkne a vlákna\\# aktuální fyzické vláken</span><span class="sxs-lookup"><span data-stu-id="610c7-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="610c7-273">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="610c7-273">Other Resources</span></span>

<span data-ttu-id="610c7-274">Další informace o výkonu technologie ASP.NET, sledování a ladění najdete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="610c7-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="610c7-275">[Přehled výkonnostní ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="610c7-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="610c7-276">Využití vlákno ASP.NET na IIS 7.5, IIS 7.0 a IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="610c7-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="610c7-277">&lt;applicationPool&gt; – Element (webové nastavení)</span><span class="sxs-lookup"><span data-stu-id="610c7-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
