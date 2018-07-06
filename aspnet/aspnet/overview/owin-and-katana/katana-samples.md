---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Použití sady Katana | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: a26cdf144d02f0b429994df9d7863163807ac7b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827269"
---
<a name="katana-samples"></a><span data-ttu-id="9ca67-102">Použití sady Katana</span><span class="sxs-lookup"><span data-stu-id="9ca67-102">Katana Samples</span></span>
====================
<span data-ttu-id="9ca67-103">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9ca67-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="9ca67-104">Použití sady Katana</span><span class="sxs-lookup"><span data-stu-id="9ca67-104">Katana Samples</span></span>

<span data-ttu-id="9ca67-105">**ASP.NET směruje ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="9ca67-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="9ca67-106">V některých aplikacích budete chtít připojení komponenty OWIN v tabulce směrování Asp.Net vedle komponenty bez OWIN.</span><span class="sxs-lookup"><span data-stu-id="9ca67-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="9ca67-107">Tento příklad ukazuje, jak použít rozšiřující metody RouteCollection MapOwinPath a MapOwinRoute poskytované Microsoft.Owin.Host.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="9ca67-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="9ca67-108">**Větvení ukázkové kanály** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="9ca67-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="9ca67-109">Není potřeba mít lineární kanály zpracování žádostí OWIN, je možné větvit ke zpracování požadavků různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="9ca67-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="9ca67-110">Tento příklad ukazuje, jak vytvořit větvení profilace na základě cesty k požadavkům nebo jiné žádosti o data, jako jsou hlavičky.</span><span class="sxs-lookup"><span data-stu-id="9ca67-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="9ca67-111">Tyto součásti jsou k dispozici v balíčku nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="9ca67-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="9ca67-112">**Ukázka vlastních serverových** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="9ca67-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="9ca67-113">Ukazuje, jak použít vlastní server OWIN při vlastním hostování OWIN.</span><span class="sxs-lookup"><span data-stu-id="9ca67-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="9ca67-114">**Vložit ukázková** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="9ca67-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="9ca67-115">Některé servery OWIN lze spustit ve vašem vlastním procesu (&quot;v místním prostředí&quot;).</span><span class="sxs-lookup"><span data-stu-id="9ca67-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="9ca67-116">Tato ukázka předvádí, jak můžete začít pomocí nástroje poskytované subsystémem balíček nuget Microsoft.Owin.Hosting aplikace OWIN.</span><span class="sxs-lookup"><span data-stu-id="9ca67-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="9ca67-117">**Ukázka Hello World** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="9ca67-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="9ca67-118">OWIN je HTTP server abstrakce rozhraní API, která umožňuje přenositelnost aplikací napříč různými servery.</span><span class="sxs-lookup"><span data-stu-id="9ca67-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="9ca67-119">Tato ukázka předvádí, jak psát aplikace Hello World pomocí několika **jednoduché obálky** kolem nezpracovaná abstrakce OWIN a spusťte ho na webovém serveru, jako je ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9ca67-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="9ca67-120">**Ukázka programu Hello World nezpracovaná OWIN** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="9ca67-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="9ca67-121">Tato ukázka předvádí, jak psát aplikace Hello World pomocí **nezpracovaná** abstrakce OWIN a spusťte ho na webovém serveru, jako je Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="9ca67-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="9ca67-122">**Ukázka SignalR** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="9ca67-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="9ca67-123">Ukazuje, jak hostování na vlastním serveru SignalR používá OWIN a Katana.</span><span class="sxs-lookup"><span data-stu-id="9ca67-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="9ca67-124">Další informace o samoobslužné hostování funkci SignalR naleznete v tématu [kurz: SignalR v místním](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="9ca67-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="9ca67-125">**Statické soubory ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="9ca67-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="9ca67-126">Ukazuje, jak pro podporu požadavků HTTP pro statické soubory, které používá OWIN a Katana.</span><span class="sxs-lookup"><span data-stu-id="9ca67-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="9ca67-127">**Webové rozhraní API** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="9ca67-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="9ca67-128">Tento příklad ukazuje, jak k hostování OWIN ve službě IIS a webového rozhraní API přidat do kanálu OWIN.</span><span class="sxs-lookup"><span data-stu-id="9ca67-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="9ca67-129">**Webových soketů ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="9ca67-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="9ca67-130">Ukazuje, jak pomocí podporují webové sokety OWIN [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="9ca67-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
