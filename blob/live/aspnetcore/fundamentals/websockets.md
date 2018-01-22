---
title: Podpora pro objekty WebSockets v ASP.NET Core
author: tdykstra
description: "Zjistěte, jak začít pracovat s objekty WebSockets v ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 46c1f42b925a43df470d7491a1e259ab51ea5f50
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="9260c-103">Úvod do technologie WebSockets v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9260c-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="9260c-104">Podle [tní Dykstra](https://github.com/tdykstra) a [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="9260c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="9260c-105">Tento článek vysvětluje, jak začít pracovat s objekty WebSockets v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9260c-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="9260c-106">[Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="9260c-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="9260c-107">Používá se pro aplikace, jako je chat, kurzů akcií, hry, vždy, když chcete v reálném čase funkce ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9260c-107">It is used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="9260c-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="9260c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="9260c-109">Najdete v článku [další kroky](#next-steps) části Další informace.</span><span class="sxs-lookup"><span data-stu-id="9260c-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="9260c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9260c-110">Prerequisites</span></span>

* <span data-ttu-id="9260c-111">ASP.NET Core 1.1 (nefunguje na 1.0)</span><span class="sxs-lookup"><span data-stu-id="9260c-111">ASP.NET Core 1.1 (does not run on 1.0)</span></span>
* <span data-ttu-id="9260c-112">Všechny operační systém, který ASP.NET Core běží na:</span><span class="sxs-lookup"><span data-stu-id="9260c-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="9260c-113">Windows 7 nebo Windows Server 2008 a novější</span><span class="sxs-lookup"><span data-stu-id="9260c-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="9260c-114">Linux</span><span class="sxs-lookup"><span data-stu-id="9260c-114">Linux</span></span>
  * <span data-ttu-id="9260c-115">macOS</span><span class="sxs-lookup"><span data-stu-id="9260c-115">macOS</span></span>

* <span data-ttu-id="9260c-116">**Výjimka**: Pokud vaše aplikace běží v systému Windows pomocí služby IIS, nebo s WebListener, je nutné použít:</span><span class="sxs-lookup"><span data-stu-id="9260c-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="9260c-117">Windows 8 nebo Windows Server 2012 nebo novější</span><span class="sxs-lookup"><span data-stu-id="9260c-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="9260c-118">Služba IIS 8 / Express IIS 8</span><span class="sxs-lookup"><span data-stu-id="9260c-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="9260c-119">Protokol WebSocket musí být povolena ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="9260c-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="9260c-120">Podporované prohlížeče najdete v části http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="9260c-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="9260c-121">Kdy ji použít</span><span class="sxs-lookup"><span data-stu-id="9260c-121">When to use it</span></span>

<span data-ttu-id="9260c-122">Technologie WebSockets použijte, když potřebujete pracovat přímo s připojení soketu.</span><span class="sxs-lookup"><span data-stu-id="9260c-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="9260c-123">Můžete například potřebovat, aby nejlepší možný výkon za hru v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="9260c-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="9260c-124">[Funkce SignalR technologie ASP.NET](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) poskytuje bohatší model aplikace pro funkce v reálném čase, ale funguje pouze na technologii ASP.NET, není ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9260c-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="9260c-125">Základní verzi SignalR je ve vývoji; postupujte podle jeho průběh, najdete v článku [úložiště GitHub pro SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="9260c-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="9260c-126">Pokud nechcete čekat SignalR Core, můžete objekty WebSockets přímo teď.</span><span class="sxs-lookup"><span data-stu-id="9260c-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="9260c-127">Ale možná budete muset vyvíjet funkcí, které by poskytují SignalR, jako například:</span><span class="sxs-lookup"><span data-stu-id="9260c-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="9260c-128">Podpora pro širší škálu verze prohlížeče pomocí automatického přechodu na metody alternativní přenosu.</span><span class="sxs-lookup"><span data-stu-id="9260c-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="9260c-129">Automatické obnovení připojení při připojení zahodí.</span><span class="sxs-lookup"><span data-stu-id="9260c-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="9260c-130">Podpora pro klienty, volání metod na serveru a naopak.</span><span class="sxs-lookup"><span data-stu-id="9260c-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="9260c-131">Podpora pro škálování na více serverů.</span><span class="sxs-lookup"><span data-stu-id="9260c-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="9260c-132">Způsobu jeho použití</span><span class="sxs-lookup"><span data-stu-id="9260c-132">How to use it</span></span>

* <span data-ttu-id="9260c-133">Nainstalujte [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="9260c-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="9260c-134">Nakonfigurujte middleware.</span><span class="sxs-lookup"><span data-stu-id="9260c-134">Configure the middleware.</span></span>
* <span data-ttu-id="9260c-135">Přijímal požadavky protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9260c-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="9260c-136">Odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="9260c-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="9260c-137">Konfiguraci middlewaru</span><span class="sxs-lookup"><span data-stu-id="9260c-137">Configure the middleware</span></span>

<span data-ttu-id="9260c-138">Přidat objekty WebSockets middleware v `Configure` metodu `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="9260c-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="9260c-139">Můžete nakonfigurovat následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="9260c-139">The following settings can be configured:</span></span>

* <span data-ttu-id="9260c-140">`KeepAliveInterval`– Jak často k odesílání rámců "ping" na klientovi, ujistěte se, že proxy nechat otevřené připojení.</span><span class="sxs-lookup"><span data-stu-id="9260c-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="9260c-141">`ReceiveBufferSize`-Velikost vyrovnávací paměti používané přijímat data.</span><span class="sxs-lookup"><span data-stu-id="9260c-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="9260c-142">Jenom Pokročilí uživatelé by muset změnit to, pro optimalizaci výkonu na základě velikosti svá data.</span><span class="sxs-lookup"><span data-stu-id="9260c-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="9260c-143">Přijímání požadavků protokolu WebSocket</span><span class="sxs-lookup"><span data-stu-id="9260c-143">Accept WebSocket requests</span></span>

<span data-ttu-id="9260c-144">Někde dole v životním cyklu žádosti (dále v `Configure` metoda nebo v MVC akce, např.) zkontrolujte, zda se jedná o žádost protokolu WebSocket a přijměte žádost protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9260c-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="9260c-145">V tomto příkladu je z později v `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="9260c-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="9260c-146">Žádost protokolu WebSocket může dojít na libovolnou adresu URL, ale tento ukázkový kód přijímá pouze požadavky na `/ws`.</span><span class="sxs-lookup"><span data-stu-id="9260c-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="9260c-147">Odesílat a přijímat zprávy</span><span class="sxs-lookup"><span data-stu-id="9260c-147">Send and receive messages</span></span>

<span data-ttu-id="9260c-148">`AcceptWebSocketAsync` Metoda upgradu připojení TCP na připojení protokolu WebSocket a poskytuje [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) objektu.</span><span class="sxs-lookup"><span data-stu-id="9260c-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="9260c-149">Pomocí objektu WebSocket odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="9260c-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="9260c-150">Předá kód uvedena výše, který přijme žádost protokolu WebSocket `WebSocket` do objektu `Echo` metoda; zde uvádíme `Echo` metoda.</span><span class="sxs-lookup"><span data-stu-id="9260c-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="9260c-151">Kód přijme nějakou zprávu a ihned odešle zpět stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="9260c-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="9260c-152">Zůstane ve smyčce učinit dokud klient uzavře připojení.</span><span class="sxs-lookup"><span data-stu-id="9260c-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="9260c-153">Když přijmete protokol WebSocket před zahájením této smyčky, skončí middlewaru v řadě.</span><span class="sxs-lookup"><span data-stu-id="9260c-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="9260c-154">Po zavření soket, unwinds kanálu.</span><span class="sxs-lookup"><span data-stu-id="9260c-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="9260c-155">To znamená zastaví požadavek postoupíte v kanálu, když přijímají protokolu WebSocket stejně, jako by při průchodu akce MVC, třeba.</span><span class="sxs-lookup"><span data-stu-id="9260c-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="9260c-156">Ale po dokončení této smyčky a zavřete soket, žádost pokračuje zálohování kanálu.</span><span class="sxs-lookup"><span data-stu-id="9260c-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9260c-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9260c-157">Next steps</span></span>

<span data-ttu-id="9260c-158">[Ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) Tento doprovodný článek je jednoduchý odezvu aplikací.</span><span class="sxs-lookup"><span data-stu-id="9260c-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="9260c-159">Má na webové stránce, který slouží k propojení protokolu WebSocket a server právě znovu odešle klientovi všechny zprávy, které obdrží.</span><span class="sxs-lookup"><span data-stu-id="9260c-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="9260c-160">Spuštění z příkazového řádku (není zřídila ke spuštění ze sady Visual Studio službou IIS Express) a přejděte na http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="9260c-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="9260c-161">Webová stránka zobrazuje stav připojení v levém horním:</span><span class="sxs-lookup"><span data-stu-id="9260c-161">The web page shows connection status at the upper left:</span></span>

![Počáteční stav webové stránky](websockets/_static/start.png)

<span data-ttu-id="9260c-163">Vyberte **Connect** na adresu URL v poslat žádost protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9260c-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="9260c-164">Zadejte testovací zprávu a vyberte **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="9260c-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="9260c-165">Až budete hotoví, vyberte **zavřít soketu**.</span><span class="sxs-lookup"><span data-stu-id="9260c-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="9260c-166">**Komunikaci protokolu** části sestavy každou otevřená, odeslání a dochází k němu zavřít akci, jako je.</span><span class="sxs-lookup"><span data-stu-id="9260c-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Počáteční stav webové stránky](websockets/_static/end.png)
