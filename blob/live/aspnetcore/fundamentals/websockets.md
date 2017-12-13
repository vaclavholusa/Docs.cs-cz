---
title: Podpora pro objekty WebSockets v ASP.NET Core
author: tdykstra
description: "Zjistěte, jak začít pracovat s objekty WebSockets v ASP.NET Core."
keywords: ASP.NET Core, objekty WebSockets
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.assetid: 0e0fedcd-a7b4-4479-8ae0-36eab0229d7e
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 114d52d831668e5facd1142b5f9e5f68e7456f7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="3c338-104">Úvod do technologie WebSockets v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c338-104">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="3c338-105">Podle [tní Dykstra](https://github.com/tdykstra) a [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="3c338-105">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="3c338-106">Tento článek vysvětluje, jak začít pracovat s objekty WebSockets v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c338-106">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="3c338-107">[Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="3c338-107">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="3c338-108">Používá se pro aplikace, jako je chat, kurzů akcií, hry, vždy, když chcete v reálném čase funkce ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3c338-108">It is used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="3c338-109">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="3c338-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="3c338-110">Najdete v článku [další kroky](#next-steps) části Další informace.</span><span class="sxs-lookup"><span data-stu-id="3c338-110">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="3c338-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3c338-111">Prerequisites</span></span>

* <span data-ttu-id="3c338-112">ASP.NET Core 1.1 (nefunguje na 1.0)</span><span class="sxs-lookup"><span data-stu-id="3c338-112">ASP.NET Core 1.1 (does not run on 1.0)</span></span>
* <span data-ttu-id="3c338-113">Všechny operační systém, který ASP.NET Core běží na:</span><span class="sxs-lookup"><span data-stu-id="3c338-113">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="3c338-114">Windows 7 nebo Windows Server 2008 a novější</span><span class="sxs-lookup"><span data-stu-id="3c338-114">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="3c338-115">Linux</span><span class="sxs-lookup"><span data-stu-id="3c338-115">Linux</span></span>
  * <span data-ttu-id="3c338-116">systému macOS</span><span class="sxs-lookup"><span data-stu-id="3c338-116">macOS</span></span>

* <span data-ttu-id="3c338-117">**Výjimka**: Pokud vaše aplikace běží v systému Windows pomocí služby IIS, nebo s WebListener, je nutné použít:</span><span class="sxs-lookup"><span data-stu-id="3c338-117">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="3c338-118">Windows 8 nebo Windows Server 2012 nebo novější</span><span class="sxs-lookup"><span data-stu-id="3c338-118">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="3c338-119">Služba IIS 8 / Express IIS 8</span><span class="sxs-lookup"><span data-stu-id="3c338-119">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="3c338-120">Protokol WebSocket musí být povolena ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="3c338-120">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="3c338-121">Podporované prohlížeče najdete v části http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="3c338-121">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="3c338-122">Kdy ji použít</span><span class="sxs-lookup"><span data-stu-id="3c338-122">When to use it</span></span>

<span data-ttu-id="3c338-123">Technologie WebSockets použijte, když potřebujete pracovat přímo s připojení soketu.</span><span class="sxs-lookup"><span data-stu-id="3c338-123">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="3c338-124">Můžete například potřebovat, aby nejlepší možný výkon za hru v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="3c338-124">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="3c338-125">[Funkce SignalR technologie ASP.NET](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) poskytuje bohatší model aplikace pro funkce v reálném čase, ale funguje pouze na technologii ASP.NET, není ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c338-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="3c338-126">Základní verzi SignalR je ve vývoji; postupujte podle jeho průběh, najdete v článku [úložiště GitHub pro SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="3c338-126">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="3c338-127">Pokud nechcete čekat SignalR Core, můžete objekty WebSockets přímo teď.</span><span class="sxs-lookup"><span data-stu-id="3c338-127">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="3c338-128">Ale možná budete muset vyvíjet funkcí, které by poskytují SignalR, jako například:</span><span class="sxs-lookup"><span data-stu-id="3c338-128">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="3c338-129">Podpora pro širší škálu verze prohlížeče pomocí automatického přechodu na metody alternativní přenosu.</span><span class="sxs-lookup"><span data-stu-id="3c338-129">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="3c338-130">Automatické obnovení připojení při připojení zahodí.</span><span class="sxs-lookup"><span data-stu-id="3c338-130">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="3c338-131">Podpora pro klienty, volání metod na serveru a naopak.</span><span class="sxs-lookup"><span data-stu-id="3c338-131">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="3c338-132">Podpora pro škálování na více serverů.</span><span class="sxs-lookup"><span data-stu-id="3c338-132">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="3c338-133">Způsobu jeho použití</span><span class="sxs-lookup"><span data-stu-id="3c338-133">How to use it</span></span>

* <span data-ttu-id="3c338-134">Nainstalujte [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="3c338-134">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="3c338-135">Nakonfigurujte middleware.</span><span class="sxs-lookup"><span data-stu-id="3c338-135">Configure the middleware.</span></span>
* <span data-ttu-id="3c338-136">Přijímal požadavky protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3c338-136">Accept WebSocket requests.</span></span>
* <span data-ttu-id="3c338-137">Odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="3c338-137">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="3c338-138">Konfiguraci middlewaru</span><span class="sxs-lookup"><span data-stu-id="3c338-138">Configure the middleware</span></span>

<span data-ttu-id="3c338-139">Přidat objekty WebSockets middleware v `Configure` metodu `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="3c338-139">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="3c338-140">Můžete nakonfigurovat následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="3c338-140">The following settings can be configured:</span></span>

* <span data-ttu-id="3c338-141">`KeepAliveInterval`– Jak často k odesílání rámců "ping" na klientovi, ujistěte se, že proxy nechat otevřené připojení.</span><span class="sxs-lookup"><span data-stu-id="3c338-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="3c338-142">`ReceiveBufferSize`-Velikost vyrovnávací paměti používané přijímat data.</span><span class="sxs-lookup"><span data-stu-id="3c338-142">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="3c338-143">Jenom Pokročilí uživatelé by muset změnit to, pro optimalizaci výkonu na základě velikosti svá data.</span><span class="sxs-lookup"><span data-stu-id="3c338-143">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="3c338-144">Přijímání požadavků protokolu WebSocket</span><span class="sxs-lookup"><span data-stu-id="3c338-144">Accept WebSocket requests</span></span>

<span data-ttu-id="3c338-145">Někde dole v životním cyklu žádosti (dále v `Configure` metoda nebo v MVC akce, např.) zkontrolujte, zda se jedná o žádost protokolu WebSocket a přijměte žádost protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3c338-145">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="3c338-146">V tomto příkladu je z později v `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="3c338-146">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="3c338-147">Žádost protokolu WebSocket může dojít na libovolnou adresu URL, ale tento ukázkový kód přijímá pouze požadavky na `/ws`.</span><span class="sxs-lookup"><span data-stu-id="3c338-147">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="3c338-148">Odesílat a přijímat zprávy</span><span class="sxs-lookup"><span data-stu-id="3c338-148">Send and receive messages</span></span>

<span data-ttu-id="3c338-149">`AcceptWebSocketAsync` Metoda upgradu připojení TCP na připojení protokolu WebSocket a poskytuje [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) objektu.</span><span class="sxs-lookup"><span data-stu-id="3c338-149">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="3c338-150">Pomocí objektu WebSocket odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="3c338-150">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="3c338-151">Předá kód uvedena výše, který přijme žádost protokolu WebSocket `WebSocket` do objektu `Echo` metoda; zde uvádíme `Echo` metoda.</span><span class="sxs-lookup"><span data-stu-id="3c338-151">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="3c338-152">Kód přijme nějakou zprávu a ihned odešle zpět stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="3c338-152">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="3c338-153">Zůstane ve smyčce učinit dokud klient uzavře připojení.</span><span class="sxs-lookup"><span data-stu-id="3c338-153">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="3c338-154">Když přijmete protokol WebSocket před zahájením této smyčky, skončí middlewaru v řadě.</span><span class="sxs-lookup"><span data-stu-id="3c338-154">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="3c338-155">Po zavření soket, unwinds kanálu.</span><span class="sxs-lookup"><span data-stu-id="3c338-155">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="3c338-156">To znamená zastaví požadavek postoupíte v kanálu, když přijímají protokolu WebSocket stejně, jako by při průchodu akce MVC, třeba.</span><span class="sxs-lookup"><span data-stu-id="3c338-156">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="3c338-157">Ale po dokončení této smyčky a zavřete soket, žádost pokračuje zálohování kanálu.</span><span class="sxs-lookup"><span data-stu-id="3c338-157">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c338-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c338-158">Next steps</span></span>

<span data-ttu-id="3c338-159">[Ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) Tento doprovodný článek je jednoduchý odezvu aplikací.</span><span class="sxs-lookup"><span data-stu-id="3c338-159">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="3c338-160">Má na webové stránce, který slouží k propojení protokolu WebSocket a server právě znovu odešle klientovi všechny zprávy, které obdrží.</span><span class="sxs-lookup"><span data-stu-id="3c338-160">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="3c338-161">Spuštění z příkazového řádku (není zřídila ke spuštění ze sady Visual Studio službou IIS Express) a přejděte na http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="3c338-161">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="3c338-162">Webová stránka zobrazuje stav připojení v levém horním:</span><span class="sxs-lookup"><span data-stu-id="3c338-162">The web page shows connection status at the upper left:</span></span>

![Počáteční stav webové stránky](websockets/_static/start.png)

<span data-ttu-id="3c338-164">Vyberte **Connect** na adresu URL v poslat žádost protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3c338-164">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="3c338-165">Zadejte testovací zprávu a vyberte **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="3c338-165">Enter a test message and select **Send**.</span></span> <span data-ttu-id="3c338-166">Až budete hotoví, vyberte **zavřít soketu**.</span><span class="sxs-lookup"><span data-stu-id="3c338-166">When done, select **Close Socket**.</span></span> <span data-ttu-id="3c338-167">**Komunikaci protokolu** části sestavy každou otevřená, odeslání a dochází k němu zavřít akci, jako je.</span><span class="sxs-lookup"><span data-stu-id="3c338-167">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Počáteční stav webové stránky](websockets/_static/end.png)
