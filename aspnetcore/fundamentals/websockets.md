---
title: Podpora pro objekty WebSockets v ASP.NET Core
author: tdykstra
description: "Zjistěte, jak začít pracovat s objekty WebSockets v ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: c6ad2ea249bf837ce86685b67e6460e433692849
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/19/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="d3e2f-103">Úvod do technologie WebSockets v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3e2f-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="d3e2f-104">Podle [tní Dykstra](https://github.com/tdykstra) a [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="d3e2f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="d3e2f-105">Tento článek vysvětluje, jak začít pracovat s objekty WebSockets v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="d3e2f-106">[Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="d3e2f-107">Používá se v aplikacích, které těžit z rychlé, v reálném čase komunikaci, jako je například konverzace a řídicí panel a herní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="d3e2f-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d3e2f-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="d3e2f-109">Najdete v článku [další kroky](#next-steps) části Další informace.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3e2f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d3e2f-110">Prerequisites</span></span>

* <span data-ttu-id="d3e2f-111">ASP.NET Core 1.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d3e2f-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="d3e2f-112">Všechny operační systém, který podporuje ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="d3e2f-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="d3e2f-113">Windows 7 nebo Windows Server 2008 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d3e2f-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="d3e2f-114">Linux</span><span class="sxs-lookup"><span data-stu-id="d3e2f-114">Linux</span></span>
  * <span data-ttu-id="d3e2f-115">macOS</span><span class="sxs-lookup"><span data-stu-id="d3e2f-115">macOS</span></span>
  
* <span data-ttu-id="d3e2f-116">Pokud aplikace běží v systému Windows pomocí služby IIS:</span><span class="sxs-lookup"><span data-stu-id="d3e2f-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="d3e2f-117">Windows 8 nebo Windows Server 2012 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d3e2f-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="d3e2f-118">Služba IIS 8 / Express IIS 8</span><span class="sxs-lookup"><span data-stu-id="d3e2f-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="d3e2f-119">Technologie WebSockets musí být povolena ve službě IIS (najdete v článku [podporu služby IIS/IIS Express](#iisiis-express-support) části.)</span><span class="sxs-lookup"><span data-stu-id="d3e2f-119">WebSockets must be enabled in IIS (See the [IIS/IIS Express support](#iisiis-express-support) section.)</span></span>
  
* <span data-ttu-id="d3e2f-120">Pokud aplikace běží [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="d3e2f-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="d3e2f-121">Windows 8 nebo Windows Server 2012 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d3e2f-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="d3e2f-122">Podporované prohlížeče najdete v části https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="d3e2f-123">Kdy použít objekty WebSockets</span><span class="sxs-lookup"><span data-stu-id="d3e2f-123">When to use WebSockets</span></span>

<span data-ttu-id="d3e2f-124">Pomocí technologie WebSockets pracovat přímo s připojení soketu.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="d3e2f-125">Například použijte Websocket pro nejlepší možný výkon s hry s v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="d3e2f-126">[Funkce SignalR technologie ASP.NET](/aspnet/signalr/overview/getting-started/introduction-to-signalr) poskytuje bohatší model aplikace pro funkce v reálném čase, ale lze spustit pouze na ASP.NET 4.x, ne jádro ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer app model for real-time functionality, but it only runs on ASP.NET 4.x, not ASP.NET Core.</span></span> <span data-ttu-id="d3e2f-127">Ve verzi ASP.NET Core funkce signalr se plánuje vydání s ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-127">An ASP.NET Core version of SignalR is scheduled for release with ASP.NET Core 2.1.</span></span> <span data-ttu-id="d3e2f-128">V tématu [plánování vysoké úrovně ASP.NET Core 2.1](https://github.com/aspnet/Announcements/issues/288).</span><span class="sxs-lookup"><span data-stu-id="d3e2f-128">See [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288).</span></span>

<span data-ttu-id="d3e2f-129">Dokud SignalR Core vydání, můžete použít objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-129">Until SignalR Core is released, WebSockets can be used.</span></span> <span data-ttu-id="d3e2f-130">Funkce, které funkce SignalR poskytuje však musí zadat a vývojář podporována.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-130">However, features that SignalR provides must be provided and supported by the developer.</span></span> <span data-ttu-id="d3e2f-131">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d3e2f-131">For example:</span></span>

* <span data-ttu-id="d3e2f-132">Podpora pro širší škálu verze prohlížeče pomocí automatického přechodu na metody alternativní přenosu.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-132">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="d3e2f-133">Automatické obnovení připojení při připojení zahodí.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-133">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="d3e2f-134">Podpora pro klienty, volání metod na serveru a naopak.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-134">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="d3e2f-135">Podpora pro škálování na více serverů.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-135">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="d3e2f-136">Způsobu jeho použití</span><span class="sxs-lookup"><span data-stu-id="d3e2f-136">How to use it</span></span>

* <span data-ttu-id="d3e2f-137">Nainstalujte [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-137">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="d3e2f-138">Nakonfigurujte middleware.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-138">Configure the middleware.</span></span>
* <span data-ttu-id="d3e2f-139">Přijímal požadavky protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-139">Accept WebSocket requests.</span></span>
* <span data-ttu-id="d3e2f-140">Odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-140">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="d3e2f-141">Konfiguraci middlewaru</span><span class="sxs-lookup"><span data-stu-id="d3e2f-141">Configure the middleware</span></span>

<span data-ttu-id="d3e2f-142">Přidat objekty WebSockets middleware v `Configure` metodu `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="d3e2f-142">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="d3e2f-143">Můžete nakonfigurovat následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="d3e2f-143">The following settings can be configured:</span></span>

* <span data-ttu-id="d3e2f-144">`KeepAliveInterval` – Jak často se bude odesílat rámce "ping" klientovi zajistit proxy nechat otevřené připojení.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-144">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="d3e2f-145">`ReceiveBufferSize` -Velikost vyrovnávací paměti používané přijímat data.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-145">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="d3e2f-146">Pokročilí uživatelé možná muset změnit na optimalizace na základě velikosti dat výkonu.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-146">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="d3e2f-147">Přijímání požadavků protokolu WebSocket</span><span class="sxs-lookup"><span data-stu-id="d3e2f-147">Accept WebSocket requests</span></span>

<span data-ttu-id="d3e2f-148">Někde dole v životním cyklu žádosti (dále v `Configure` metoda nebo v MVC akce, např.) zkontrolujte, zda se jedná o žádost protokolu WebSocket a přijměte žádost protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-148">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="d3e2f-149">V následujícím příkladu je z později v `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="d3e2f-149">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="d3e2f-150">Žádost protokolu WebSocket může dojít na libovolnou adresu URL, ale tento ukázkový kód přijímá pouze požadavky na `/ws`.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-150">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="d3e2f-151">Odesílat a přijímat zprávy</span><span class="sxs-lookup"><span data-stu-id="d3e2f-151">Send and receive messages</span></span>

<span data-ttu-id="d3e2f-152">`AcceptWebSocketAsync` Metoda upgradu připojení TCP na připojení protokolu WebSocket a poskytuje [WebSocket](/dotnet/core/api/system.net.websockets.websocket) objektu.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-152">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="d3e2f-153">Použití `WebSocket` objekt, který chcete odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-153">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="d3e2f-154">Kód zobrazí dříve, který přijme žádost protokolu WebSocket předá `WebSocket` do objektu `Echo` metoda.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-154">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="d3e2f-155">Kód přijme nějakou zprávu a ihned odešle zpět stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-155">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="d3e2f-156">Zpráv odesílaných i přijímaných ve smyčce, dokud klient uzavře připojení:</span><span class="sxs-lookup"><span data-stu-id="d3e2f-156">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="d3e2f-157">Když přijímá připojení protokolu WebSocket před zahájením smyčky, skončí middlewaru v řadě.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-157">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="d3e2f-158">Po zavření soket, unwinds kanálu.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-158">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="d3e2f-159">To znamená požadavek se zastaví postoupíte v kanálu, když je přijímán objektu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-159">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="d3e2f-160">Po dokončení smyčky a zavření soket, žádost pokračuje zálohování kanálu.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-160">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="d3e2f-161">Podpora služby IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="d3e2f-161">IIS/IIS Express support</span></span>

<span data-ttu-id="d3e2f-162">Windows Server 2012 nebo novější a Windows 8 nebo novější s služby IIS/IIS Express 8 nebo novější má podporu protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-162">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

<span data-ttu-id="d3e2f-163">Povolení podpory pro protokol WebSocket v systému Windows Server 2012 nebo novějším:</span><span class="sxs-lookup"><span data-stu-id="d3e2f-163">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

1. <span data-ttu-id="d3e2f-164">Použití **přidat role a funkce** průvodce z **spravovat** nabídky nebo na odkaz v **správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-164">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="d3e2f-165">Vyberte **instalace na základě rolí nebo na základě funkcí**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-165">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="d3e2f-166">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-166">Select **Next**.</span></span>
1. <span data-ttu-id="d3e2f-167">Vyberte příslušný server (ve výchozím nastavení se vybere místní server).</span><span class="sxs-lookup"><span data-stu-id="d3e2f-167">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="d3e2f-168">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-168">Select **Next**.</span></span>
1. <span data-ttu-id="d3e2f-169">Rozbalte položku **webového serveru (IIS)** v **role** stromu, rozbalte položku **Webový Server**a potom rozbalte **vývoj aplikací**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-169">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="d3e2f-170">Vyberte **protokol WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-170">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="d3e2f-171">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-171">Select **Next**.</span></span>
1. <span data-ttu-id="d3e2f-172">Pokud nejsou potřeba další funkce, vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-172">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="d3e2f-173">Vyberte **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-173">Select **Install**.</span></span>
1. <span data-ttu-id="d3e2f-174">Po dokončení instalace, vyberte **Zavřít** ukončíte průvodce.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-174">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="d3e2f-175">Povolení podpory pro protokol WebSocket v systému Windows 8 nebo novější:</span><span class="sxs-lookup"><span data-stu-id="d3e2f-175">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

1. <span data-ttu-id="d3e2f-176">Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky).</span><span class="sxs-lookup"><span data-stu-id="d3e2f-176">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="d3e2f-177">Otevřete následující uzly: **Internetová informační služba** > **webové služby** > **funkce pro vývoj aplikací**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-177">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="d3e2f-178">Vyberte **protokol WebSocket** funkce.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-178">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="d3e2f-179">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-179">Select **OK**.</span></span>

<span data-ttu-id="d3e2f-180">**Zakázat protokol WebSocket, když pomocí socket.io na node.js**</span><span class="sxs-lookup"><span data-stu-id="d3e2f-180">**Disable WebSocket when using socket.io on node.js**</span></span>

<span data-ttu-id="d3e2f-181">Pokud používáte podporu protokolu WebSocket v [socket.io](https://socket.io/) na [Node.js](https://nodejs.org/), zakažte pomocí modulu IIS WebSocket výchozí `webSocket` element v *web.config* nebo *applicationHost.config*. Pokud není tento krok provést, pokusí se modulu protokolu WebSocket IIS zpracovávat komunikaci protokolu WebSocket místo Node.js a aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-181">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="d3e2f-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3e2f-182">Next steps</span></span>

<span data-ttu-id="d3e2f-183">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) Tento doprovodný článek je aplikace odezvu.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-183">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is an echo app.</span></span> <span data-ttu-id="d3e2f-184">Má na webové stránce, který slouží k propojení protokolu WebSocket a server znovu odešle všechny zprávy, které obdrží zpět do klienta.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-184">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="d3e2f-185">Spuštění aplikace z příkazového řádku (není zřídila ke spuštění ze sady Visual Studio službou IIS Express) a přejděte na http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-185">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="d3e2f-186">Webová stránka zobrazuje stav připojení v levé horní části:</span><span class="sxs-lookup"><span data-stu-id="d3e2f-186">The web page shows the connection status in the upper left:</span></span>

![Počáteční stav webové stránky](websockets/_static/start.png)

<span data-ttu-id="d3e2f-188">Vyberte **Connect** na adresu URL v poslat žádost protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-188">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="d3e2f-189">Zadejte testovací zprávu a vyberte **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-189">Enter a test message and select **Send**.</span></span> <span data-ttu-id="d3e2f-190">Až budete hotoví, vyberte **zavřít soketu**.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-190">When done, select **Close Socket**.</span></span> <span data-ttu-id="d3e2f-191">**Komunikaci protokolu** části sestavy každou otevřená, odeslání a dochází k němu zavřít akci, jako je.</span><span class="sxs-lookup"><span data-stu-id="d3e2f-191">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Počáteční stav webové stránky](websockets/_static/end.png)
