---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Použití sady Katana | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: b8ce2b40a19e0429f1ccedb03b8f829582652d24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752454"
---
<a name="katana-samples"></a><span data-ttu-id="c0456-102">Použití sady Katana</span><span class="sxs-lookup"><span data-stu-id="c0456-102">Katana Samples</span></span>
====================
<span data-ttu-id="c0456-103">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c0456-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="c0456-104">Použití sady Katana</span><span class="sxs-lookup"><span data-stu-id="c0456-104">Katana Samples</span></span>

<span data-ttu-id="c0456-105">**ASP.NET směruje ukázka** | [zdrojový kód](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span><span class="sxs-lookup"><span data-stu-id="c0456-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="c0456-106">V některých aplikacích budete chtít připojení komponenty OWIN v tabulce směrování Asp.Net vedle komponenty bez OWIN.</span><span class="sxs-lookup"><span data-stu-id="c0456-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="c0456-107">Tento příklad ukazuje, jak použít rozšiřující metody RouteCollection MapOwinPath a MapOwinRoute poskytované Microsoft.Owin.Host.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="c0456-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="c0456-108">**Větvení ukázkové kanály** | [zdrojový kód](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span><span class="sxs-lookup"><span data-stu-id="c0456-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="c0456-109">Není potřeba mít lineární kanály zpracování žádostí OWIN, je možné větvit ke zpracování požadavků různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="c0456-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="c0456-110">Tento příklad ukazuje, jak vytvořit větvení profilace na základě cesty k požadavkům nebo jiné žádosti o data, jako jsou hlavičky.</span><span class="sxs-lookup"><span data-stu-id="c0456-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="c0456-111">Tyto součásti jsou k dispozici v balíčku nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="c0456-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="c0456-112">**Ukázka vlastních serverových** | [zdrojový kód](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span><span class="sxs-lookup"><span data-stu-id="c0456-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="c0456-113">Ukazuje, jak použít vlastní server OWIN při vlastním hostování OWIN.</span><span class="sxs-lookup"><span data-stu-id="c0456-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="c0456-114">**Vložit ukázková** | [zdrojový kód](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span><span class="sxs-lookup"><span data-stu-id="c0456-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="c0456-115">Některé servery OWIN lze spustit ve vašem vlastním procesu (&quot;v místním prostředí&quot;).</span><span class="sxs-lookup"><span data-stu-id="c0456-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="c0456-116">Tato ukázka předvádí, jak můžete začít pomocí nástroje poskytované subsystémem balíček nuget Microsoft.Owin.Hosting aplikace OWIN.</span><span class="sxs-lookup"><span data-stu-id="c0456-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="c0456-117">**Ukázka Hello World** | [zdrojový kód](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span><span class="sxs-lookup"><span data-stu-id="c0456-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="c0456-118">OWIN je HTTP server abstrakce rozhraní API, která umožňuje přenositelnost aplikací napříč různými servery.</span><span class="sxs-lookup"><span data-stu-id="c0456-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="c0456-119">Tato ukázka předvádí, jak psát aplikace Hello World pomocí několika **jednoduché obálky** kolem nezpracovaná abstrakce OWIN a spusťte ho na webovém serveru, jako je ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c0456-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="c0456-120">**Ukázka programu Hello World nezpracovaná OWIN** | [zdrojový kód](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span><span class="sxs-lookup"><span data-stu-id="c0456-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="c0456-121">Tato ukázka předvádí, jak psát aplikace Hello World pomocí **nezpracovaná** abstrakce OWIN a spusťte ho na webovém serveru, jako je Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="c0456-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="c0456-122">**Ukázka SignalR** | [zdrojový kód](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span><span class="sxs-lookup"><span data-stu-id="c0456-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="c0456-123">Ukazuje, jak hostování na vlastním serveru SignalR používá OWIN a Katana.</span><span class="sxs-lookup"><span data-stu-id="c0456-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="c0456-124">Další informace o samoobslužné hostování funkci SignalR naleznete v tématu [kurz: SignalR v místním](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="c0456-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="c0456-125">**Statické soubory ukázka** | [zdrojový kód](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span><span class="sxs-lookup"><span data-stu-id="c0456-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="c0456-126">Ukazuje, jak pro podporu požadavků HTTP pro statické soubory, které používá OWIN a Katana.</span><span class="sxs-lookup"><span data-stu-id="c0456-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="c0456-127">**Webové rozhraní API** | [zdrojový kód](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span><span class="sxs-lookup"><span data-stu-id="c0456-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="c0456-128">Tento příklad ukazuje, jak k hostování OWIN ve službě IIS a webového rozhraní API přidat do kanálu OWIN.</span><span class="sxs-lookup"><span data-stu-id="c0456-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="c0456-129">**Webových soketů ukázka** | [zdrojový kód](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span><span class="sxs-lookup"><span data-stu-id="c0456-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="c0456-130">Ukazuje, jak pomocí podporují webové sokety OWIN [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="c0456-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
