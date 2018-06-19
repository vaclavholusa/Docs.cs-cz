---
uid: signalr/overview/older-versions/signalr-performance
title: SignalR výkonu (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: SignalR výkonu
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bad742af28d6c36bb1b66207c2ba09d140332449
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28037149"
---
<a name="signalr-performance-signalr-1x"></a><span data-ttu-id="f8be4-103">SignalR výkonu (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="f8be4-103">SignalR Performance (SignalR 1.x)</span></span>
====================
<span data-ttu-id="f8be4-104">podle [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f8be4-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="f8be4-105">Toto téma popisuje postup návrhu pro měření a zlepšit výkon v aplikaci SignalR.</span><span class="sxs-lookup"><span data-stu-id="f8be4-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>


<span data-ttu-id="f8be4-106">Poslední prezentace na SignalR výkonu a škálování, najdete v části [škálování webu v reálném čase pomocí funkce SignalR technologie ASP.NET](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="f8be4-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="f8be4-107">Toto téma obsahuje následující oddíly:</span><span class="sxs-lookup"><span data-stu-id="f8be4-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f8be4-108">Aspekty návrhu</span><span class="sxs-lookup"><span data-stu-id="f8be4-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="f8be4-109">Optimalizace výkonu serveru SignalR</span><span class="sxs-lookup"><span data-stu-id="f8be4-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="f8be4-110">Řešení potíží s výkonem</span><span class="sxs-lookup"><span data-stu-id="f8be4-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="f8be4-111">Pomocí čítače výkonu SignalR</span><span class="sxs-lookup"><span data-stu-id="f8be4-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="f8be4-112">Pomocí dalších čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="f8be4-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="f8be4-113">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="f8be4-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="f8be4-114">Aspekty návrhu</span><span class="sxs-lookup"><span data-stu-id="f8be4-114">Design considerations</span></span>

<span data-ttu-id="f8be4-115">Tato část popisuje vzorů, které lze provádět při návrhu aplikace SignalR zajistit, že není právě výkonu omezován generování nepotřebné síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="f8be4-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="f8be4-116">Frekvence omezení zpráv</span><span class="sxs-lookup"><span data-stu-id="f8be4-116">Throttling message frequency</span></span>

<span data-ttu-id="f8be4-117">I v aplikaci, která odesílá zprávy s vysoká frekvence (jako například herní aplikací v reálném čase) není nutné odesílat zprávy o několik a sekundu většinu aplikací.</span><span class="sxs-lookup"><span data-stu-id="f8be4-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="f8be4-118">Snížit objem provozu, který generuje každého klienta, smyčku zpráv se dají implementovat, fronty a odesílá zprávy nejvýše často pevné sazby (tedy až počet zpráv odešle za sekundu, pokud jsou v té době v zprávy terval k odeslání).</span><span class="sxs-lookup"><span data-stu-id="f8be4-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="f8be4-119">Ukázkové aplikace, která omezí generovaný zprávy do určité míry (z klient i server), najdete v části [vysoká frekvence v reálném čase s SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="f8be4-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="f8be4-120">Snižuje velikost zprávy</span><span class="sxs-lookup"><span data-stu-id="f8be4-120">Reducing message size</span></span>

<span data-ttu-id="f8be4-121">Snížením velikosti serializovaných objektů můžete snížit velikost zprávy SignalR.</span><span class="sxs-lookup"><span data-stu-id="f8be4-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="f8be4-122">V serverovém kódu, pokud odesíláte objekt, který obsahuje vlastnosti, které není nutné přenášet, zabránit tyto vlastnosti serializována pomocí `JsonIgnore` atribut.</span><span class="sxs-lookup"><span data-stu-id="f8be4-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="f8be4-123">Názvy vlastností jsou také uloženy ve zprávě; názvy vlastností lze zkrátit pomocí `JsonProperty` atribut.</span><span class="sxs-lookup"><span data-stu-id="f8be4-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="f8be4-124">Následující ukázka kódu ukazuje, jak být vyloučena určitá vlastnost odeslání do klienta a jak tak, aby zkrátil názvy vlastností:</span><span class="sxs-lookup"><span data-stu-id="f8be4-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="f8be4-125">**Kód serveru .NET, která popisuje atribut JsonIgnore vyloučení dat z odesílány do klienta a atribut JsonProperty snížit velikost zprávy**</span><span class="sxs-lookup"><span data-stu-id="f8be4-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="f8be4-126">Chcete-li zachovat čitelnost / udržovatelnosti v kódu klienta, názvy zkrácené vlastností, které může být změnu mapování k lidské friendly názvy, po doručení zprávy.</span><span class="sxs-lookup"><span data-stu-id="f8be4-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="f8be4-127">Následující příklad kódu ukazuje jeden možný způsob přemapování zkrácení názvy chcete delší, tím, že definujete kontrakt zprávy (mapování) a pomocí `reMap` funkce, která se týkají kontrakt třídu optimalizované zpráv:</span><span class="sxs-lookup"><span data-stu-id="f8be4-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="f8be4-128">**Kód jazyka JavaScript na straně klienta, který změní mapování zkrátit názvy vlastností, abyste čitelná pro člověka názvy**</span><span class="sxs-lookup"><span data-stu-id="f8be4-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="f8be4-129">Názvy lze zkrátit ve zprávách z klienta na server taky stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="f8be4-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="f8be4-130">Snižuje nároky na paměť (který je množství paměti pro zprávu) zprávy mohou objekt rovněž zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="f8be4-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="f8be4-131">Například pokud plný rozsah `int` není potřeba, `short` nebo `byte` můžete místo toho použít.</span><span class="sxs-lookup"><span data-stu-id="f8be4-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="f8be4-132">Vzhledem k tomu, že zprávy jsou uloženy ve sběrnici zpráv v paměti serveru, zmenšení velikosti této zprávy můžete také řeší problémy paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="f8be4-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="f8be4-133">Optimalizace výkonu serveru SignalR</span><span class="sxs-lookup"><span data-stu-id="f8be4-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="f8be4-134">Následující nastavení konfigurace slouží k vyladění vašeho serveru pro lepší výkon v aplikaci SignalR.</span><span class="sxs-lookup"><span data-stu-id="f8be4-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="f8be4-135">Obecné informace o tom, jak zlepšit výkon v aplikaci ASP.NET najdete v tématu [zlepšení výkonu technologie ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="f8be4-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="f8be4-136">**Nastavení konfigurace SignalR**</span><span class="sxs-lookup"><span data-stu-id="f8be4-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="f8be4-137">**DefaultMessageBufferSize**: ve výchozím nastavení, SignalR uchovává zprávy 1000 v paměti za rozbočovač za připojení.</span><span class="sxs-lookup"><span data-stu-id="f8be4-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="f8be4-138">Pokud se používají velké zprávy, to může způsobit problémy s pamětí, které můžete zmírnit po nastavení této hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f8be4-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="f8be4-139">Toto nastavení může být nastavena v `Application_Start` obslužné rutiny události v aplikaci ASP.NET nebo v `Configuration` metoda třídy pro spuštění OWIN ve vlastním hostováním aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f8be4-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="f8be4-140">Následující příklad ukazuje, jak snížit tato hodnota za účelem snížení nároky na paměť pro vaše aplikace za účelem snížení množství paměti serveru:</span><span class="sxs-lookup"><span data-stu-id="f8be4-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="f8be4-141">**Kód serveru .NET v souboru Global.asax pro snížení výchozí velikost vyrovnávací paměti zpráv**</span><span class="sxs-lookup"><span data-stu-id="f8be4-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="f8be4-142">**Nastavení konfigurace služby IIS**</span><span class="sxs-lookup"><span data-stu-id="f8be4-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="f8be4-143">**Maximální počet souběžných požadavků na aplikaci**: zvýšení počtu souběžných IIS požadavky zvýší prostředky serveru k dispozici pro obsluhovat požadavky.</span><span class="sxs-lookup"><span data-stu-id="f8be4-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="f8be4-144">Výchozí hodnota je 5 000; Pokud chcete zvýšit toto nastavení, spusťte následující příkazy v příkazovém řádku se zvýšenými oprávněními:</span><span class="sxs-lookup"><span data-stu-id="f8be4-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="f8be4-145">**Nastavení konfigurace ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="f8be4-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="f8be4-146">Tato část obsahuje nastavení konfigurace, které lze nastavit v `aspnet.config` souboru.</span><span class="sxs-lookup"><span data-stu-id="f8be4-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="f8be4-147">Tento soubor se nachází v jednom ze dvou umístěních, v závislosti na platformě:</span><span class="sxs-lookup"><span data-stu-id="f8be4-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="f8be4-148">Nastavení ASP.NET, které může zvýšit výkon SignalR zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="f8be4-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="f8be4-149">**Maximální souběžných požadavků na jeden procesor**: zvýšením tohoto nastavení může zmírnit kritické body.</span><span class="sxs-lookup"><span data-stu-id="f8be4-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="f8be4-150">Chcete-li zvýšit toto nastavení, přidejte následující nastavení konfigurace na `aspnet.config` souboru:</span><span class="sxs-lookup"><span data-stu-id="f8be4-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="f8be4-151">**Limit fronty požadavků**: Pokud celkový počet připojení překročí `maxConcurrentRequestsPerCPU` nastavení, ASP.NET spustí omezování požadavků pomocí fronty.</span><span class="sxs-lookup"><span data-stu-id="f8be4-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="f8be4-152">Pokud chcete zvýšit velikost fronty, můžete zvýšit `requestQueueLimit` nastavení.</span><span class="sxs-lookup"><span data-stu-id="f8be4-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="f8be4-153">K tomu, přidejte následující nastavení konfigurace pro `processModel` uzlu v `config/machine.config` (místo `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="f8be4-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="f8be4-154">Řešení potíží s výkonem</span><span class="sxs-lookup"><span data-stu-id="f8be4-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="f8be4-155">Tato část popisuje způsoby jak najít kritické body ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f8be4-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="f8be4-156">Ověření, že je použit protokol WebSocket</span><span class="sxs-lookup"><span data-stu-id="f8be4-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="f8be4-157">Zatímco SignalR používat celou řadu přenosů pro komunikaci mezi klientem a serverem, protokolu WebSocket nabízí využít významně zvýšit výkon a by měl použít, pokud ji podporuje klient a server.</span><span class="sxs-lookup"><span data-stu-id="f8be4-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="f8be4-158">Pokud chcete zjistit, pokud klient a server splňovat požadavky protokolu WebSocket, najdete v části [přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="f8be4-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="f8be4-159">Pokud chcete určit, jaký přenos se používá v aplikaci, můžete použít nástroje pro vývojáře prohlížeče a v protokolech zobrazíte, jaký přenos se používá pro připojení.</span><span class="sxs-lookup"><span data-stu-id="f8be4-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="f8be4-160">Informace o použití nástrojů pro vývoj prohlížeče v Internet Exploreru a Chrome, najdete v části [přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="f8be4-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="f8be4-161">Pomocí čítače výkonu SignalR</span><span class="sxs-lookup"><span data-stu-id="f8be4-161">Using SignalR performance counters</span></span>

<span data-ttu-id="f8be4-162">Tato část popisuje, jak povolit a používat čítače výkonu SignalR v nalezen `Microsoft.AspNet.SignalR.Utils` balíčku.</span><span class="sxs-lookup"><span data-stu-id="f8be4-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="f8be4-163">Instalace signalr.exe</span><span class="sxs-lookup"><span data-stu-id="f8be4-163">Installing signalr.exe</span></span>

<span data-ttu-id="f8be4-164">Čítače problémových mohou být přidány k serveru pomocí nástroj SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="f8be4-164">Peformance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="f8be4-165">Chcete-li nainstalovat tento nástroj, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="f8be4-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="f8be4-166">V aplikaci Visual Studio, vyberte **nástroje**, **Správce balíčků knihoven**, **spravovat balíčky NuGet pro řešení...**</span><span class="sxs-lookup"><span data-stu-id="f8be4-166">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="f8be4-167">Vyhledejte **signalr.utils**a vyberte možnost instalace.</span><span class="sxs-lookup"><span data-stu-id="f8be4-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="f8be4-168">Přijměte licenční smlouvu k instalaci balíčku.</span><span class="sxs-lookup"><span data-stu-id="f8be4-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="f8be4-169">SignalR.exe se nainstaluje pro `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="f8be4-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="f8be4-170">Instalace s SignalR.exe čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="f8be4-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="f8be4-171">Pokud chcete nainstalovat čítače výkonu SignalR, spusťte SignalR.exe v příkazovém řádku se zvýšenými s parametrem následující:</span><span class="sxs-lookup"><span data-stu-id="f8be4-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="f8be4-172">Chcete-li odebrat čítače výkonu SignalR, spusťte SignalR.exe v příkazovém řádku se zvýšenými s následující parametr:</span><span class="sxs-lookup"><span data-stu-id="f8be4-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="f8be4-173">Čítače výkonu SignalR</span><span class="sxs-lookup"><span data-stu-id="f8be4-173">SignalR Performance counters</span></span>

<span data-ttu-id="f8be4-174">Balíček nástroje nainstaluje následující čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="f8be4-174">The utilites package installs the following performance counters.</span></span> <span data-ttu-id="f8be4-175">"Celkovou" čítače měření počtu událostí od poslední fond aplikací nebo server restartovat.</span><span class="sxs-lookup"><span data-stu-id="f8be4-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="f8be4-176">**Metriky připojení**</span><span class="sxs-lookup"><span data-stu-id="f8be4-176">**Connection metrics**</span></span>

<span data-ttu-id="f8be4-177">Následující metriky měření události životnost připojení, ke kterým dochází.</span><span class="sxs-lookup"><span data-stu-id="f8be4-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="f8be4-178">Další informace najdete v tématu [pochopení a zpracování události životnost připojení](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="f8be4-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="f8be4-179">**Připojení připojení**</span><span class="sxs-lookup"><span data-stu-id="f8be4-179">**Connections Connected**</span></span>
- <span data-ttu-id="f8be4-180">**Připojení znovu**</span><span class="sxs-lookup"><span data-stu-id="f8be4-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="f8be4-181">**Disonnected připojení**</span><span class="sxs-lookup"><span data-stu-id="f8be4-181">**Connections Disonnected**</span></span>
- <span data-ttu-id="f8be4-182">**Aktuální připojení**</span><span class="sxs-lookup"><span data-stu-id="f8be4-182">**Connections Current**</span></span>

<span data-ttu-id="f8be4-183">**Zpráva metriky**</span><span class="sxs-lookup"><span data-stu-id="f8be4-183">**Message metrics**</span></span>

<span data-ttu-id="f8be4-184">Následující metriky měření provoz zprávy generované SignalR.</span><span class="sxs-lookup"><span data-stu-id="f8be4-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="f8be4-185">**Celkový počet přijatých zprávy připojení**</span><span class="sxs-lookup"><span data-stu-id="f8be4-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="f8be4-186">**Celkový počet odesílají zprávy připojení**</span><span class="sxs-lookup"><span data-stu-id="f8be4-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="f8be4-187">**Přijaté zprávy připojení za sekundu**</span><span class="sxs-lookup"><span data-stu-id="f8be4-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="f8be4-188">**Připojení odeslaných zpráv za sekundu**</span><span class="sxs-lookup"><span data-stu-id="f8be4-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="f8be4-189">**Metriky sběrnice zpráv**</span><span class="sxs-lookup"><span data-stu-id="f8be4-189">**Message bus metrics**</span></span>

<span data-ttu-id="f8be4-190">Následující metriky měření provoz prostřednictvím interní sběrnice zpráv SignalR, fronty, ve kterém jsou umístěny všechny příchozí a odchozí zprávy SignalR.</span><span class="sxs-lookup"><span data-stu-id="f8be4-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="f8be4-191">Zpráva je **publikováno** při se odesílá nebo všesměrového vysílání.</span><span class="sxs-lookup"><span data-stu-id="f8be4-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="f8be4-192">A **odběratele** v tomto kontextu je předplatné na sběrnici zpráv; to musí být roven počtu klientů a samotný server.</span><span class="sxs-lookup"><span data-stu-id="f8be4-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="f8be4-193">**Přidělené pracovní** je komponenta, která odesílá data do active připojení, **zaneprázdněn pracovní** je ten, který je aktivně odesílání zprávy.</span><span class="sxs-lookup"><span data-stu-id="f8be4-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="f8be4-194">**Celkový počet přijatých zpráv sběrnice zpráv**</span><span class="sxs-lookup"><span data-stu-id="f8be4-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="f8be4-195">**Sběrnice zpráv zprávy přijaté za sekundu**</span><span class="sxs-lookup"><span data-stu-id="f8be4-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="f8be4-196">**Celkový počet publikovaných zprávy sběrnice zpráv**</span><span class="sxs-lookup"><span data-stu-id="f8be4-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="f8be4-197">**Sběrnice zpráv zprávy publikované za sekundu**</span><span class="sxs-lookup"><span data-stu-id="f8be4-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="f8be4-198">**Aktuální odběratele sběrnice zpráv**</span><span class="sxs-lookup"><span data-stu-id="f8be4-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="f8be4-199">**Celkový počet odběratele sběrnice zpráv**</span><span class="sxs-lookup"><span data-stu-id="f8be4-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="f8be4-200">**Odběratelé sběrnice zpráv za sekundu**</span><span class="sxs-lookup"><span data-stu-id="f8be4-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="f8be4-201">**Sběrnice zpráv přidělené pracovní procesy**</span><span class="sxs-lookup"><span data-stu-id="f8be4-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="f8be4-202">**Sběrnice zpráv vytížených pracovních procesů**</span><span class="sxs-lookup"><span data-stu-id="f8be4-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="f8be4-203">**Aktuální témata sběrnice zpráv**</span><span class="sxs-lookup"><span data-stu-id="f8be4-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="f8be4-204">**Chyba metriky**</span><span class="sxs-lookup"><span data-stu-id="f8be4-204">**Error metrics**</span></span>

<span data-ttu-id="f8be4-205">Následující metriky měření chybách vygenerovaných provoz zpráv SignalR.</span><span class="sxs-lookup"><span data-stu-id="f8be4-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="f8be4-206">**Centrum řešení** dojít k chybám, když nelze přeložit rozbočovače nebo metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="f8be4-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="f8be4-207">**Volání rozbočovače** chyby jsou výjimky vydané při vyvolání metody hub.</span><span class="sxs-lookup"><span data-stu-id="f8be4-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="f8be4-208">**Přenos** chyby jsou chyby připojení během požadavku nebo odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8be4-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="f8be4-209">**Chyb: Celkový počet všech**</span><span class="sxs-lookup"><span data-stu-id="f8be4-209">**Errors: All Total**</span></span>
- <span data-ttu-id="f8be4-210">**Chyby: Všechna za sekundu**</span><span class="sxs-lookup"><span data-stu-id="f8be4-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="f8be4-211">**Chyby: Celkový počet řešení rozbočovače**</span><span class="sxs-lookup"><span data-stu-id="f8be4-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="f8be4-212">**Chyby: Centrum řešení za sekundu**</span><span class="sxs-lookup"><span data-stu-id="f8be4-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="f8be4-213">**Chyby: Celkový počet volání rozbočovače**</span><span class="sxs-lookup"><span data-stu-id="f8be4-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="f8be4-214">**Chyby: Rozbočovače volání za sekundu**</span><span class="sxs-lookup"><span data-stu-id="f8be4-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="f8be4-215">**Chyb: Celkový počet přenosu**</span><span class="sxs-lookup"><span data-stu-id="f8be4-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="f8be4-216">**Chyby: Přenos/s**</span><span class="sxs-lookup"><span data-stu-id="f8be4-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="f8be4-217">**Metriky škálování.**</span><span class="sxs-lookup"><span data-stu-id="f8be4-217">**Scaleout metrics**</span></span>

<span data-ttu-id="f8be4-218">Následující metriky měření provoz a chyby vygenerované zprostředkovatelem škálování.</span><span class="sxs-lookup"><span data-stu-id="f8be4-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="f8be4-219">A **datového proudu** v tomto kontextu je jednotka škálování používá zprostředkovatel škálování; jde tabulky, pokud se používá SQL Server, téma, pokud se používá Service Bus a předplatné, pokud se používá Redis.</span><span class="sxs-lookup"><span data-stu-id="f8be4-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="f8be4-220">Ve výchozím nastavení se používá pouze jeden datový proud, ale to může být zvýšena pomocí konfigurace na serveru SQL a sběrnice.</span><span class="sxs-lookup"><span data-stu-id="f8be4-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="f8be4-221">A **ukládání do vyrovnávací paměti** datový proud je ten, který má zadaný chybný stav; když je datový proud v chybovém stavu, všech zpráv odeslaných do propojovacího rozhraní se nezdaří, až do datového proudu už selhal.</span><span class="sxs-lookup"><span data-stu-id="f8be4-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="f8be4-222">**Délka fronty pro odeslání** je počet zpráv, které byly odeslány, ale ještě nebyla odeslána.</span><span class="sxs-lookup"><span data-stu-id="f8be4-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="f8be4-223">**Sběrnice zpráv o škálování zprávy přijaté za sekundu**</span><span class="sxs-lookup"><span data-stu-id="f8be4-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="f8be4-224">**Celkový počet datových proudů škálování.**</span><span class="sxs-lookup"><span data-stu-id="f8be4-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="f8be4-225">**Škálování datové proudy Open**</span><span class="sxs-lookup"><span data-stu-id="f8be4-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="f8be4-226">**Datové proudy škálování ukládání do vyrovnávací paměti**</span><span class="sxs-lookup"><span data-stu-id="f8be4-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="f8be4-227">**Celkový počet chyb škálování.**</span><span class="sxs-lookup"><span data-stu-id="f8be4-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="f8be4-228">**Škálování chyby/s**</span><span class="sxs-lookup"><span data-stu-id="f8be4-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="f8be4-229">**Délka fronty pro odesílání škálování.**</span><span class="sxs-lookup"><span data-stu-id="f8be4-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="f8be4-230">Další informace o co jsou tyto čítače měření najdete v tématu [SignalR škálování s Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="f8be4-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="f8be4-231">Pomocí dalších čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="f8be4-231">Using other performance counters</span></span>

<span data-ttu-id="f8be4-232">Následující čítače výkonu může být taky užitečný v monitorování výkonu aplikace.</span><span class="sxs-lookup"><span data-stu-id="f8be4-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="f8be4-233">**Paměť**</span><span class="sxs-lookup"><span data-stu-id="f8be4-233">**Memory**</span></span>

- <span data-ttu-id="f8be4-234">Rozhraní .NET CLR paměti počet bajtů ve všech haldách (pro w3wp)</span><span class="sxs-lookup"><span data-stu-id="f8be4-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="f8be4-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="f8be4-235">**ASP.NET**</span></span>

- <span data-ttu-id="f8be4-236">Aktuální ASP.NET\Requests</span><span class="sxs-lookup"><span data-stu-id="f8be4-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="f8be4-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="f8be4-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="f8be4-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="f8be4-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="f8be4-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="f8be4-239">**CPU**</span></span>

- <span data-ttu-id="f8be4-240">Information\Processor času procesoru</span><span class="sxs-lookup"><span data-stu-id="f8be4-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="f8be4-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="f8be4-241">**TCP/IP**</span></span>

- <span data-ttu-id="f8be4-242">TCPv6 nebo připojení</span><span class="sxs-lookup"><span data-stu-id="f8be4-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="f8be4-243">TCPv4 nebo připojení</span><span class="sxs-lookup"><span data-stu-id="f8be4-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="f8be4-244">**Web Service**</span><span class="sxs-lookup"><span data-stu-id="f8be4-244">**Web Service**</span></span>

- <span data-ttu-id="f8be4-245">Webová Service\Current připojení</span><span class="sxs-lookup"><span data-stu-id="f8be4-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="f8be4-246">Webová Service\Maximum připojení</span><span class="sxs-lookup"><span data-stu-id="f8be4-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="f8be4-247">**Dělení na vlákna**</span><span class="sxs-lookup"><span data-stu-id="f8be4-247">**Threading**</span></span>

- <span data-ttu-id="f8be4-248">Rozhraní .NET CLR LocksAndThreads\# aktuální logické vláken</span><span class="sxs-lookup"><span data-stu-id="f8be4-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="f8be4-249">Rozhraní .NET CLR LocksAnd vláken\# aktuální fyzické vláken</span><span class="sxs-lookup"><span data-stu-id="f8be4-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="f8be4-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f8be4-250">Other Resources</span></span>

<span data-ttu-id="f8be4-251">Další informace o výkonu technologie ASP.NET, sledování a ladění najdete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="f8be4-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="f8be4-252">[Přehled výkonnostní ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="f8be4-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="f8be4-253">Využití vlákno ASP.NET na IIS 7.5, IIS 7.0 a IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="f8be4-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="f8be4-254">&lt;applicationPool&gt; – Element (webové nastavení)</span><span class="sxs-lookup"><span data-stu-id="f8be4-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
