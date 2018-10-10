---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Použití rozhraní OWIN k samoobslužnému hostování webového rozhraní API 2 ASP.NET | Dokumentace Microsoftu
author: rick-anderson
description: Tento kurz ukazuje, jak hostovat v konzolové aplikaci, používá OWIN k samoobslužnému hostování webového rozhraní API rozhraní ASP.NET Web API. Otevřít Web Interface pro .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 06fd13fe9b12d172d615ae76a71d246a89f5386d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910483"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="c9f0d-104">Použití rozhraní OWIN k samoobslužnému hostování webového rozhraní API 2 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c9f0d-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="c9f0d-105">podle [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="c9f0d-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="c9f0d-106">Tento kurz ukazuje, jak hostovat v konzolové aplikaci, používá OWIN k samoobslužnému hostování webového rozhraní API rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="c9f0d-107">[Otevřete Web Interface pro .NET](http://owin.org) (OWIN) definuje abstrakce mezi .NET webové servery a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="c9f0d-108">OWIN odpojí webové aplikace ze serveru, který je ideální pro webové aplikace ve vašem vlastním procesu mimo službu IIS s vlastním hostováním OWIN.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c9f0d-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="c9f0d-109">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="c9f0d-110">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (funguje taky s Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="c9f0d-110">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="c9f0d-111">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="c9f0d-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="c9f0d-112">Úplný zdrojový kód můžete najít v tomto kurzu na [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="c9f0d-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="c9f0d-113">Vytvoření konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="c9f0d-113">Create a Console Application</span></span>

<span data-ttu-id="c9f0d-114">Na **souboru** nabídky, klikněte na tlačítko **nový**, pak klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="c9f0d-115">Z **nainstalované šablony**, v části Visual C#, klikněte na tlačítko **Windows** a potom klikněte na tlačítko **konzolovou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="c9f0d-116">Pojmenujte projekt "OwinSelfhostSample" a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="c9f0d-117">Přidání webového rozhraní API a balíčky OWIN</span><span class="sxs-lookup"><span data-stu-id="c9f0d-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="c9f0d-118">Z **nástroje** nabídky, klepněte na **Správce balíčků NuGet**, klepněte na **konzoly Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-118">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="c9f0d-119">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c9f0d-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="c9f0d-120">Tím se nainstaluje balíček selfhost WebAPI OWIN a všechny požadované balíčky OWIN.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="c9f0d-121">Konfigurace webového rozhraní API pro hostování na vlastním serveru</span><span class="sxs-lookup"><span data-stu-id="c9f0d-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="c9f0d-122">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **přidat** / **třídy** přidání nové třídy.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="c9f0d-123">Název třídy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="c9f0d-124">Nahraďte všechny často používaný kód v tomto souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c9f0d-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="c9f0d-125">Přidat kontroler rozhraní Web API</span><span class="sxs-lookup"><span data-stu-id="c9f0d-125">Add a Web API Controller</span></span>

<span data-ttu-id="c9f0d-126">Dále přidejte třídu kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="c9f0d-127">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **přidat** / **třídy** přidání nové třídy.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="c9f0d-128">Název třídy `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="c9f0d-129">Nahraďte všechny často používaný kód v tomto souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c9f0d-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="c9f0d-130">Spuštění hostitele OWIN a vytvoříte žádost o pomocí položky HttpClient</span><span class="sxs-lookup"><span data-stu-id="c9f0d-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="c9f0d-131">Nahraďte všechny často používaný kód v souboru Program.cs následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="c9f0d-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="c9f0d-132">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="c9f0d-132">Running the Application</span></span>

<span data-ttu-id="c9f0d-133">Spusťte aplikaci stisknutím klávesy F5 v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c9f0d-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="c9f0d-134">Výstup by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="c9f0d-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="c9f0d-135">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="c9f0d-135">Additional Resources</span></span>

[<span data-ttu-id="c9f0d-136">Přehled projektu Katana</span><span class="sxs-lookup"><span data-stu-id="c9f0d-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="c9f0d-137">Hostování webového rozhraní API ASP.NET v roli pracovního procesu Azure</span><span class="sxs-lookup"><span data-stu-id="c9f0d-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
