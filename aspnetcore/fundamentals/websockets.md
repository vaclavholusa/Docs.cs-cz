---
title: Webové sockety v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak začít pracovat s objekty Websocket v ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: a9fe13ef7895ea3ab43257dbbaf4521f883c0804
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433984"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="becb8-103">Webové sockety v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="becb8-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="becb8-104">Podle [Petr Dykstra](https://github.com/tdykstra) a [Andrew Stanton sestry](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="becb8-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="becb8-105">Tento článek vysvětluje, jak začít pracovat s objekty Websocket v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="becb8-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="becb8-106">[Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="becb8-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="becb8-107">Používá se v aplikacích, které využívají samosprávné komunikace rychlé, v reálném čase, jako jsou konverzace, řídicí panel a herních aplikací.</span><span class="sxs-lookup"><span data-stu-id="becb8-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="becb8-108">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="becb8-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="becb8-109">Zobrazit [další kroky](#next-steps) části Další informace.</span><span class="sxs-lookup"><span data-stu-id="becb8-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="becb8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="becb8-110">Prerequisites</span></span>

* <span data-ttu-id="becb8-111">ASP.NET Core 1.1 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="becb8-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="becb8-112">Libovolný operační systém, který podporuje ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="becb8-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="becb8-113">Windows 7 / Windows Server 2008 nebo novější</span><span class="sxs-lookup"><span data-stu-id="becb8-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="becb8-114">Linux</span><span class="sxs-lookup"><span data-stu-id="becb8-114">Linux</span></span>
  * <span data-ttu-id="becb8-115">macOS</span><span class="sxs-lookup"><span data-stu-id="becb8-115">macOS</span></span>
  
* <span data-ttu-id="becb8-116">Pokud aplikace běží na Windows se službou IIS:</span><span class="sxs-lookup"><span data-stu-id="becb8-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="becb8-117">Windows 8 nebo Windows Server 2012 nebo novější</span><span class="sxs-lookup"><span data-stu-id="becb8-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="becb8-118">Služba IIS 8 / 8 služby IIS Express</span><span class="sxs-lookup"><span data-stu-id="becb8-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="becb8-119">Protokoly Websocket, musí být povolené ve službě IIS (najdete v článku [podpora služby IIS/IIS Express](#iisiis-express-support) části.)</span><span class="sxs-lookup"><span data-stu-id="becb8-119">WebSockets must be enabled in IIS (See the [IIS/IIS Express support](#iisiis-express-support) section.)</span></span>
  
* <span data-ttu-id="becb8-120">Pokud aplikace běží na [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="becb8-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="becb8-121">Windows 8 nebo Windows Server 2012 nebo novější</span><span class="sxs-lookup"><span data-stu-id="becb8-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="becb8-122">Podporované prohlížeče, naleznete v tématu https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="becb8-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="becb8-123">Kdy použít objekty Websocket</span><span class="sxs-lookup"><span data-stu-id="becb8-123">When to use WebSockets</span></span>

<span data-ttu-id="becb8-124">Použijte pracovat přímo s připojení soketu Websocket.</span><span class="sxs-lookup"><span data-stu-id="becb8-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="becb8-125">Například můžete použijte objekty Websocket pro zajištění nejlepšího možného výkonu s hry v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="becb8-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="becb8-126">[Funkce SignalR technologie ASP.NET Core](xref:signalr/introduction) je knihovna, která zjednodušuje přidávání funkce webu v reálném čase do aplikací.</span><span class="sxs-lookup"><span data-stu-id="becb8-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="becb8-127">Používá objekty Websocket, kdykoli je to možné.</span><span class="sxs-lookup"><span data-stu-id="becb8-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="becb8-128">Jak používat objekty Websocket</span><span class="sxs-lookup"><span data-stu-id="becb8-128">How to use WebSockets</span></span>

* <span data-ttu-id="becb8-129">Nainstalujte [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="becb8-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="becb8-130">Nakonfigurujte middleware.</span><span class="sxs-lookup"><span data-stu-id="becb8-130">Configure the middleware.</span></span>
* <span data-ttu-id="becb8-131">Přijímal požadavky protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="becb8-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="becb8-132">Odesílání a příjem zpráv.</span><span class="sxs-lookup"><span data-stu-id="becb8-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="becb8-133">Nakonfigurujte middleware</span><span class="sxs-lookup"><span data-stu-id="becb8-133">Configure the middleware</span></span>

<span data-ttu-id="becb8-134">Přidat objekty Websocket middlewaru v `Configure` metodu `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="becb8-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="becb8-135">Můžete nakonfigurovat následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="becb8-135">The following settings can be configured:</span></span>

* <span data-ttu-id="becb8-136">`KeepAliveInterval` – Jak často k odesílání rámců "příkaz ping" ke klientovi, aby proxy servery udržení připojení otevřeného.</span><span class="sxs-lookup"><span data-stu-id="becb8-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="becb8-137">`ReceiveBufferSize` – Velikost vyrovnávací paměti používané přijímat data.</span><span class="sxs-lookup"><span data-stu-id="becb8-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="becb8-138">Pokročilí uživatelé může být nutné změnit to pro optimalizace na základě velikosti dat výkonu.</span><span class="sxs-lookup"><span data-stu-id="becb8-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="becb8-139">Přijímal požadavky protokolu WebSocket</span><span class="sxs-lookup"><span data-stu-id="becb8-139">Accept WebSocket requests</span></span>

<span data-ttu-id="becb8-140">Někde dále v cyklu požadavku (dále v `Configure` metoda nebo akce MVC, například) zkontrolujte, jestli je požadavek protokolu WebSocket a přijmout požadavek protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="becb8-140">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="becb8-141">Následující příklad je z později v `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="becb8-141">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="becb8-142">Požadavek protokolu WebSocket může dojít na libovolnou adresu URL, ale tento vzorový kód přijímá jenom žádosti o `/ws`.</span><span class="sxs-lookup"><span data-stu-id="becb8-142">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="becb8-143">Odesílání a příjem zpráv</span><span class="sxs-lookup"><span data-stu-id="becb8-143">Send and receive messages</span></span>

<span data-ttu-id="becb8-144">`AcceptWebSocketAsync` Metoda upgraduje na připojení soketu websocket bylo připojení TCP a poskytuje [protokolu WebSocket](/dotnet/core/api/system.net.websockets.websocket) objektu.</span><span class="sxs-lookup"><span data-stu-id="becb8-144">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="becb8-145">Použití `WebSocket` objektu pro odesílání a příjem zpráv.</span><span class="sxs-lookup"><span data-stu-id="becb8-145">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="becb8-146">Předá kódu uvedeného výše, který přijme žádost protokolu WebSocket `WebSocket` objektu `Echo` metody.</span><span class="sxs-lookup"><span data-stu-id="becb8-146">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="becb8-147">Kód přijme zprávu a okamžitě odešle zpět stejné zprávy.</span><span class="sxs-lookup"><span data-stu-id="becb8-147">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="becb8-148">Zprávy jsou odeslané a přijaté ve smyčce, dokud klient ukončí připojení:</span><span class="sxs-lookup"><span data-stu-id="becb8-148">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="becb8-149">Při přijímání připojení soketu websocket bylo před zahájením smyčky, middleware kanálu se ukončí.</span><span class="sxs-lookup"><span data-stu-id="becb8-149">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="becb8-150">Po zavření soketu, unwinds kanálu.</span><span class="sxs-lookup"><span data-stu-id="becb8-150">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="becb8-151">To znamená požadavek se zastaví v budoucnu v kanálu při přijetí objekt WebSocket.</span><span class="sxs-lookup"><span data-stu-id="becb8-151">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="becb8-152">Když je smyčka dokončena, je uzavřený soket žádost pokračuje zálohování kanálu.</span><span class="sxs-lookup"><span data-stu-id="becb8-152">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="becb8-153">Podpora služby IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="becb8-153">IIS/IIS Express support</span></span>

<span data-ttu-id="becb8-154">Windows Server 2012 nebo novější a Windows 8 nebo novější s služby IIS/IIS Express 8 nebo novější obsahuje podporu protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="becb8-154">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

<span data-ttu-id="becb8-155">Pokud chcete povolit podporu protokolu WebSocket ve Windows serveru 2012 nebo novější:</span><span class="sxs-lookup"><span data-stu-id="becb8-155">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

1. <span data-ttu-id="becb8-156">Použití **přidat role a funkce** z průvodce **spravovat** nabídky nebo na odkaz v **správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="becb8-156">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="becb8-157">Vyberte **instalace na základě rolí nebo na základě funkcí**.</span><span class="sxs-lookup"><span data-stu-id="becb8-157">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="becb8-158">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="becb8-158">Select **Next**.</span></span>
1. <span data-ttu-id="becb8-159">Vyberte příslušný server (ve výchozím nastavení je vybraný místní server).</span><span class="sxs-lookup"><span data-stu-id="becb8-159">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="becb8-160">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="becb8-160">Select **Next**.</span></span>
1. <span data-ttu-id="becb8-161">Rozbalte **webového serveru (IIS)** v **role** stromu, rozbalte **Webový Server**a potom rozbalte **vývoj aplikací**.</span><span class="sxs-lookup"><span data-stu-id="becb8-161">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="becb8-162">Vyberte **protokol WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="becb8-162">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="becb8-163">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="becb8-163">Select **Next**.</span></span>
1. <span data-ttu-id="becb8-164">Pokud nejsou potřebné další funkce, vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="becb8-164">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="becb8-165">Vyberte **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="becb8-165">Select **Install**.</span></span>
1. <span data-ttu-id="becb8-166">Po dokončení instalace, vybrat **Zavřít** ukončíte průvodce.</span><span class="sxs-lookup"><span data-stu-id="becb8-166">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="becb8-167">Pokud chcete povolit podporu protokolu WebSocket v systému Windows 8 nebo novější:</span><span class="sxs-lookup"><span data-stu-id="becb8-167">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

1. <span data-ttu-id="becb8-168">Přejděte do **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky).</span><span class="sxs-lookup"><span data-stu-id="becb8-168">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="becb8-169">Otevřete následující uzly: **Internetová informační služba** > **webové služby** > **funkce pro vývoj aplikací**.</span><span class="sxs-lookup"><span data-stu-id="becb8-169">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="becb8-170">Vyberte **protokol WebSocket** funkce.</span><span class="sxs-lookup"><span data-stu-id="becb8-170">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="becb8-171">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="becb8-171">Select **OK**.</span></span>

<span data-ttu-id="becb8-172">**Zakázat protokol WebSocket, když v node.js pomocí socket.io**</span><span class="sxs-lookup"><span data-stu-id="becb8-172">**Disable WebSocket when using socket.io on node.js**</span></span>

<span data-ttu-id="becb8-173">Pokud používáte podpora protokolu WebSocket v [socket.io](https://socket.io/) na [Node.js](https://nodejs.org/), zakázat pomocí modul WebSocket služby IIS výchozí `webSocket` element v *web.config* nebo *applicationHost.config*. Pokud není tento krok provést, modul WebSocket služby IIS se pokusí se zpracovat komunikaci pomocí protokolu WebSocket spíše než Node.js a aplikace.</span><span class="sxs-lookup"><span data-stu-id="becb8-173">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="becb8-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="becb8-174">Next steps</span></span>

<span data-ttu-id="becb8-175">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) , který doprovází tento článek je odezvu aplikace.</span><span class="sxs-lookup"><span data-stu-id="becb8-175">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is an echo app.</span></span> <span data-ttu-id="becb8-176">Obsahuje webovou stránku, která umožňuje připojení pomocí protokolu WebSocket, a server odešle všechny zprávy, které obdrží zpět do klienta.</span><span class="sxs-lookup"><span data-stu-id="becb8-176">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="becb8-177">Spustit z příkazového řádku (ne zřídila pro spuštění ze sady Visual Studio službou IIS Express) a přejděte do http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="becb8-177">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="becb8-178">Webové stránky zobrazí v levém horním rohu stav připojení:</span><span class="sxs-lookup"><span data-stu-id="becb8-178">The web page shows the connection status in the upper left:</span></span>

![Počáteční stav webové stránky](websockets/_static/start.png)

<span data-ttu-id="becb8-180">Vyberte **připojit** na adresu URL odeslat požadavek protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="becb8-180">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="becb8-181">Zadejte testovací zprávu a vybrat **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="becb8-181">Enter a test message and select **Send**.</span></span> <span data-ttu-id="becb8-182">Až budete hotovi, vyberte **zavřít soketu**.</span><span class="sxs-lookup"><span data-stu-id="becb8-182">When done, select **Close Socket**.</span></span> <span data-ttu-id="becb8-183">**Komunikace protokolu** části sestavy každý otevřený, odesílání a akce při zavření jako to se stane.</span><span class="sxs-lookup"><span data-stu-id="becb8-183">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Počáteční stav webové stránky](websockets/_static/end.png)
