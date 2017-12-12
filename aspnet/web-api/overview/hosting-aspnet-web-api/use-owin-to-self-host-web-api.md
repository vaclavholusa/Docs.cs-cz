---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: "Hostování na vlastním rozhraní ASP.NET Web API 2 pomocí OWIN | Microsoft Docs"
author: rick-anderson
description: "Tento kurz ukazuje, jak k hostování webové rozhraní API ASP.NET v konzolové aplikaci, pomocí k hostování na vlastním rozhraní Web API OWIN. Spustit nástroj webové rozhraní pro rozhraní .NET (OWIN) d..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="ea81f-104">Hostování na vlastním rozhraní ASP.NET Web API 2 pomocí OWIN</span><span class="sxs-lookup"><span data-stu-id="ea81f-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="ea81f-105">podle [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="ea81f-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="ea81f-106">Tento kurz ukazuje, jak k hostování webové rozhraní API ASP.NET v konzolové aplikaci, pomocí k hostování na vlastním rozhraní Web API OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea81f-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="ea81f-107">[Otevřete Web Interface pro .NET](http://owin.org) (OWIN) definuje abstrakci mezi .NET webové servery a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ea81f-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="ea81f-108">OWIN oddělí webové aplikace ze serveru, takže je ideální pro samoobslužné webové aplikace v vlastní proces mimo službu IIS hostování OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea81f-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ea81f-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="ea81f-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ea81f-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funguje taky s Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="ea81f-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="ea81f-111">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="ea81f-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="ea81f-112">Úplný zdrojový kód najdete v tomto kurzu v [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="ea81f-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="ea81f-113">Vytvoření konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="ea81f-113">Create a Console Application</span></span>

<span data-ttu-id="ea81f-114">Na **soubor** nabídky, klikněte na tlačítko **nový**, pak klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="ea81f-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="ea81f-115">Z **nainstalovaných šablonách**, v části Visual C#, klikněte na **Windows** a pak klikněte na **konzolové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ea81f-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="ea81f-116">Název projektu "OwinSelfhostSample" a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea81f-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="ea81f-117">Přidání webového rozhraní API a balíčky OWIN</span><span class="sxs-lookup"><span data-stu-id="ea81f-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="ea81f-118">Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven**, pak klikněte na tlačítko **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="ea81f-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="ea81f-119">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ea81f-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="ea81f-120">Nainstaluje balíček selfhost WebAPI OWIN a všechny požadované balíčky OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea81f-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="ea81f-121">Konfigurace webového rozhraní API pro hostování na vlastním serveru</span><span class="sxs-lookup"><span data-stu-id="ea81f-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="ea81f-122">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **přidat** / **třída** pro přidání nové třídy.</span><span class="sxs-lookup"><span data-stu-id="ea81f-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="ea81f-123">Název třídy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ea81f-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="ea81f-124">Všechny často používaný kód v tomto souboru nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="ea81f-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="ea81f-125">Přidat řadič webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ea81f-125">Add a Web API Controller</span></span>

<span data-ttu-id="ea81f-126">V dalším kroku přidejte třídu kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ea81f-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="ea81f-127">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **přidat** / **třída** pro přidání nové třídy.</span><span class="sxs-lookup"><span data-stu-id="ea81f-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="ea81f-128">Název třídy `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="ea81f-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="ea81f-129">Všechny často používaný kód v tomto souboru nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="ea81f-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="ea81f-130">Spuštění hostitele OWIN a podání žádosti o pomocí položky HttpClient</span><span class="sxs-lookup"><span data-stu-id="ea81f-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="ea81f-131">Nahraďte všechny často používaný kód v souboru Program.cs následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="ea81f-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="ea81f-132">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ea81f-132">Running the Application</span></span>

<span data-ttu-id="ea81f-133">Ke spuštění aplikace, stisknutím klávesy F5 v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea81f-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="ea81f-134">Výstup by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="ea81f-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="ea81f-135">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="ea81f-135">Additional Resources</span></span>

[<span data-ttu-id="ea81f-136">Přehled Katana projektu</span><span class="sxs-lookup"><span data-stu-id="ea81f-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="ea81f-137">Hostitel rozhraní ASP.NET Web API roli pracovního procesu systému Azure</span><span class="sxs-lookup"><span data-stu-id="ea81f-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
