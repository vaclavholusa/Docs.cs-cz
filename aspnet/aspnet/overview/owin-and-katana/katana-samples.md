---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Ukázky Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034487"
---
<a name="katana-samples"></a><span data-ttu-id="4fe14-102">Ukázky Katana</span><span class="sxs-lookup"><span data-stu-id="4fe14-102">Katana Samples</span></span>
====================
<span data-ttu-id="4fe14-103">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4fe14-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="4fe14-104">Ukázky Katana</span><span class="sxs-lookup"><span data-stu-id="4fe14-104">Katana Samples</span></span>

<span data-ttu-id="4fe14-105">**ASP.NET směruje ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="4fe14-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="4fe14-106">V některých aplikacích můžete spojit komponenty OWIN v tabulce směrování Asp.Net node souběžně s komponenty jiných OWIN.</span><span class="sxs-lookup"><span data-stu-id="4fe14-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="4fe14-107">Tento příklad ukazuje způsob použití metod rozšíření RouteCollection MapOwinPath a MapOwinRoute poskytované Microsoft.Owin.Host.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="4fe14-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="4fe14-108">**Vytvoření větve kanálů ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="4fe14-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="4fe14-109">Zpracování žádosti OWIN kanály nemusí být lineární, že můžete vytvořit větve zpracovávat požadavky různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="4fe14-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="4fe14-110">Tento příklad ukazuje, jak vytvořit větvení kanál na základě cesty k požadavku nebo jiná data požadavku třeba hlavičky.</span><span class="sxs-lookup"><span data-stu-id="4fe14-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="4fe14-111">Tyto součásti jsou k dispozici v Microsoft.Owin.Mapping balíček nuget.</span><span class="sxs-lookup"><span data-stu-id="4fe14-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="4fe14-112">**Ukázka vlastního serveru** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="4fe14-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="4fe14-113">Ukazuje, jak použít vlastní server OWIN při vlastním hostování OWIN.</span><span class="sxs-lookup"><span data-stu-id="4fe14-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="4fe14-114">**Ukázka vložených** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="4fe14-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="4fe14-115">Některé servery OWIN lze spustit v rámci vlastní proces (&quot;hostovanou na vlastním&quot;).</span><span class="sxs-lookup"><span data-stu-id="4fe14-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="4fe14-116">Tento příklad ukazuje postup spuštění OWIN aplikace pomocí nástroje poskytované subsystémem pro balíček Microsoft.Owin.Hosting nuget.</span><span class="sxs-lookup"><span data-stu-id="4fe14-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="4fe14-117">**Ukázka HelloWorld** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="4fe14-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="4fe14-118">OWIN je server HTTP abstrakce rozhraní API, která umožňuje přenosnost aplikací na různých serverech.</span><span class="sxs-lookup"><span data-stu-id="4fe14-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="4fe14-119">Tento příklad ukazuje, jak psát aplikace Hello World pomocí některé **jednoduché obálky** kolem nezpracovaná abstrakce OWIN a spusťte ho na webovém serveru jako ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4fe14-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="4fe14-120">**Ukázka programu Hello World nezpracovaná OWIN** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="4fe14-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="4fe14-121">Tento příklad znázorňuje, jak psát aplikace Hello, World pomocí **nezpracovaná** abstrakce OWIN a potom ho spusťte na webovém serveru, jako je technologie Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="4fe14-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="4fe14-122">**Ukázka SignalR** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="4fe14-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="4fe14-123">Ukazuje, jak hostování na vlastním SignalR pomocí OWIN nebo Katana.</span><span class="sxs-lookup"><span data-stu-id="4fe14-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="4fe14-124">Další informace o samoobslužné hostování SignalR najdete v tématu [kurz: SignalR hostování na vlastním](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="4fe14-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="4fe14-125">**Statické soubory ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="4fe14-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="4fe14-126">Ukazuje, jak pro podporu požadavků HTTP pro statické soubory pomocí OWIN nebo Katana.</span><span class="sxs-lookup"><span data-stu-id="4fe14-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="4fe14-127">**Webové rozhraní API** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="4fe14-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="4fe14-128">Tento příklad ukazuje, jak pro hostování OWIN ve službě IIS a přidání webového rozhraní API do kanálu OWIN.</span><span class="sxs-lookup"><span data-stu-id="4fe14-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="4fe14-129">**Webový soket ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="4fe14-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="4fe14-130">Ukazuje, jak pro podporu webové sokety v OWIN pomocí [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="4fe14-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
