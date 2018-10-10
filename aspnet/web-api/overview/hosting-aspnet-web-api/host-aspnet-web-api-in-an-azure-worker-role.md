---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hostovat rozhraní ASP.NET Web API 2 v roli pracovního procesu Azure | Dokumentace Microsoftu
author: MikeWasson
description: Tento kurz ukazuje, jak hostovat rozhraní ASP.NET Web API v roli pracovního procesu Azure používá OWIN k samoobslužnému hostování webového rozhraní API rozhraní framework. Otevřít Web Interface pro .NET (OWIN) de...
ms.author: riande
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 40cb1a4514beaf81e7ed75bbd3e478f2ba146fe5
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910743"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="2d800-104">Hostovat rozhraní ASP.NET Web API 2 v roli pracovního procesu Azure</span><span class="sxs-lookup"><span data-stu-id="2d800-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="2d800-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2d800-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="2d800-106">Tento kurz ukazuje, jak hostovat rozhraní ASP.NET Web API v roli pracovního procesu Azure používá OWIN k samoobslužnému hostování webového rozhraní API rozhraní framework.</span><span class="sxs-lookup"><span data-stu-id="2d800-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="2d800-107">[Otevřete Web Interface pro .NET](http://owin.org/) (OWIN) definuje abstrakce mezi .NET webové servery a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2d800-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="2d800-108">Webové aplikace ze serveru, který je ideální pro webové aplikace ve vašem vlastním procesu mimo službu IIS s vlastním hostováním OWIN OWIN odděluje obě části – například v roli pracovního procesu systému Azure.</span><span class="sxs-lookup"><span data-stu-id="2d800-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="2d800-109">V tomto kurzu budete používat Microsoft.Owin.Host.HttpListener balíček, který poskytuje HTTP server, který použije k samoobslužnému hostování aplikací OWIN.</span><span class="sxs-lookup"><span data-stu-id="2d800-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2d800-110">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="2d800-110">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="2d800-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2d800-111">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="2d800-112">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="2d800-112">Web API 2</span></span>
> - [<span data-ttu-id="2d800-113">Sada Azure SDK for .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="2d800-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="2d800-114">Vytvořte projekt Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="2d800-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="2d800-115">Spusťte Visual Studio s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="2d800-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="2d800-116">Oprávnění správce jsou potřebné k ladění aplikace v místním prostředí, pomocí emulátoru služby výpočty v Azure.</span><span class="sxs-lookup"><span data-stu-id="2d800-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="2d800-117">Na **souboru** nabídky, klikněte na tlačítko **nový**, pak klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="2d800-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="2d800-118">Z **nainstalované šablony**, v části Visual C#, klikněte na tlačítko **cloudu** a potom klikněte na tlačítko **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="2d800-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="2d800-119">Pojmenujte projekt "AzureApp" a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d800-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="2d800-120">V **nová Cloudová služba Windows Azure** dialogového okna, dvakrát klikněte na panel **Role pracovního procesu**.</span><span class="sxs-lookup"><span data-stu-id="2d800-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="2d800-121">Ponechte výchozí název ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="2d800-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="2d800-122">Tento krok přidává role pracovního procesu k řešení.</span><span class="sxs-lookup"><span data-stu-id="2d800-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="2d800-123">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d800-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="2d800-124">Řešení sady Visual Studio, který je vytvořen obsahuje dva projekty:</span><span class="sxs-lookup"><span data-stu-id="2d800-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="2d800-125">&quot;AzureApp&quot; definuje role a konfigurace pro aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="2d800-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="2d800-126">&quot;WorkerRole1&quot; obsahuje kód pro roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="2d800-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="2d800-127">Obecně platí aplikace Azure může obsahovat více rolí, i když tento kurz používá jednu roli.</span><span class="sxs-lookup"><span data-stu-id="2d800-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="2d800-128">Přidání webového rozhraní API a balíčky OWIN</span><span class="sxs-lookup"><span data-stu-id="2d800-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="2d800-129">Z **nástroje** nabídky, klepněte na **Správce balíčků NuGet**, klepněte na **konzoly Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="2d800-129">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="2d800-130">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2d800-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="2d800-131">Přidat koncový bod HTTP</span><span class="sxs-lookup"><span data-stu-id="2d800-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="2d800-132">V Průzkumníku řešení rozbalte projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="2d800-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="2d800-133">Rozbalte uzel role, klikněte pravým tlačítkem na WorkerRole1 a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="2d800-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="2d800-134">Klikněte na tlačítko **koncové body**a potom klikněte na tlačítko **přidat koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="2d800-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="2d800-135">V **protokol** rozevíracího seznamu vyberte možnost "http".</span><span class="sxs-lookup"><span data-stu-id="2d800-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="2d800-136">V **veřejný Port** a **privátní Port**, zadejte 80.</span><span class="sxs-lookup"><span data-stu-id="2d800-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="2d800-137">Tato čísla portů se mohou lišit.</span><span class="sxs-lookup"><span data-stu-id="2d800-137">These port numbers can be different.</span></span> <span data-ttu-id="2d800-138">Veřejný port je, co klienti používat při odesílání požadavku do role.</span><span class="sxs-lookup"><span data-stu-id="2d800-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="2d800-139">Konfigurace webového rozhraní API pro hostování na vlastním serveru</span><span class="sxs-lookup"><span data-stu-id="2d800-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="2d800-140">V Průzkumníku řešení klikněte pravým tlačítkem myši klikněte na WorkerRole1 projekt a vyberte **přidat** / **třídy** přidání nové třídy.</span><span class="sxs-lookup"><span data-stu-id="2d800-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="2d800-141">Název třídy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="2d800-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="2d800-142">Nahraďte všechny často používaný kód v tomto souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2d800-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="2d800-143">Přidat kontroler rozhraní Web API</span><span class="sxs-lookup"><span data-stu-id="2d800-143">Add a Web API Controller</span></span>

<span data-ttu-id="2d800-144">Dále přidejte třídu kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2d800-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="2d800-145">Klikněte pravým tlačítkem na projekt WorkerRole1 a vyberte **přidat** / **třídy**.</span><span class="sxs-lookup"><span data-stu-id="2d800-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="2d800-146">Název třídy TestController.</span><span class="sxs-lookup"><span data-stu-id="2d800-146">Name the class TestController.</span></span> <span data-ttu-id="2d800-147">Nahraďte všechny často používaný kód v tomto souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2d800-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="2d800-148">Pro zjednodušení tohoto kontroleru pouze definuje dvě metody GET, které vracejí prostý text.</span><span class="sxs-lookup"><span data-stu-id="2d800-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="2d800-149">Spuštění hostitele OWIN</span><span class="sxs-lookup"><span data-stu-id="2d800-149">Start the OWIN Host</span></span>

<span data-ttu-id="2d800-150">Otevřete soubor WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="2d800-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="2d800-151">Tato třída definuje kód, který se spustí při spuštění a zastavení role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="2d800-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="2d800-152">Přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="2d800-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="2d800-153">Přidat **IDisposable** člena `WorkerRole` třídy:</span><span class="sxs-lookup"><span data-stu-id="2d800-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="2d800-154">V `OnStart` metodu, přidejte následující kód pro spuštění hostitele:</span><span class="sxs-lookup"><span data-stu-id="2d800-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="2d800-155">**WebApp.Start** metoda spustí hostitele OWIN.</span><span class="sxs-lookup"><span data-stu-id="2d800-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="2d800-156">Název `Startup` třídy je parametr typu metody.</span><span class="sxs-lookup"><span data-stu-id="2d800-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="2d800-157">Podle konvence, bude volat hostitele `Configure` metody této třídy.</span><span class="sxs-lookup"><span data-stu-id="2d800-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="2d800-158">Přepsat `OnStop` a neprovede  *\_aplikace* instance:</span><span class="sxs-lookup"><span data-stu-id="2d800-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="2d800-159">Tady je kompletní kód WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="2d800-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="2d800-160">Sestavte řešení a stisknutím klávesy F5 spusťte aplikaci místně v emulátoru Azure Compute.</span><span class="sxs-lookup"><span data-stu-id="2d800-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="2d800-161">V závislosti na nastavení brány firewall potřebujete povolit emulátor přes bránu firewall.</span><span class="sxs-lookup"><span data-stu-id="2d800-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="2d800-162">Pokud dojde k výjimce následujícím postupem, najdete [tento příspěvek na blogu](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="2d800-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="2d800-163">"Nelze načíst soubor nebo sestavení" Microsoft.Owin, verze = 2.0.2.0, jazyková verze = neutral, PublicKeyToken = 31bf3856ad364e35' nebo některou z jeho závislostí.</span><span class="sxs-lookup"><span data-stu-id="2d800-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="2d800-164">Definice manifestu sestavení neodpovídá odkazu na sestavení.</span><span class="sxs-lookup"><span data-stu-id="2d800-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="2d800-165">(Výjimka z HRESULT: 0x80131040) "</span><span class="sxs-lookup"><span data-stu-id="2d800-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="2d800-166">Emulátor služby výpočty místní IP adresa přiřadí ke koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="2d800-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="2d800-167">IP adresu můžete najít zobrazením Uživatelském prostředí emulátoru výpočtů.</span><span class="sxs-lookup"><span data-stu-id="2d800-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="2d800-168">Klikněte pravým tlačítkem na hlavním panelu oznamovací oblasti ikonu emulátoru a vyberte **zobrazit Uživatelském prostředí emulátoru výpočtů**.</span><span class="sxs-lookup"><span data-stu-id="2d800-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="2d800-169">Nalezení IP adresy v rámci služby nasazení, nasazení [id], podrobnosti o službě.</span><span class="sxs-lookup"><span data-stu-id="2d800-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="2d800-170">Otevřete webový prohlížeč a přejdete na http://<em>adresu</em>/testovací/1, kde <em>adresu</em> je IP adresa přidělí emulátor služby výpočty; například `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="2d800-170">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="2d800-171">Měli byste vidět odpovědi z kontroleru webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="2d800-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="2d800-172">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="2d800-172">Deploy to Azure</span></span>

<span data-ttu-id="2d800-173">Pro tento krok musí mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="2d800-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="2d800-174">Pokud již nemáte, můžete během několika minut vytvořit Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="2d800-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="2d800-175">Podrobnosti najdete v tématu [bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="2d800-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="2d800-176">V Průzkumníku řešení klikněte pravým tlačítkem na projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="2d800-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="2d800-177">Vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="2d800-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="2d800-178">Pokud nejste přihlášení k účtu Azure, klikněte na tlačítko **Sign In**.</span><span class="sxs-lookup"><span data-stu-id="2d800-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="2d800-179">Když jste přihlášeni, zvolte předplatné a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2d800-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="2d800-180">Zadejte název pro cloudovou službu a zvolte oblast.</span><span class="sxs-lookup"><span data-stu-id="2d800-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="2d800-181">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2d800-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="2d800-182">Klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="2d800-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="2d800-183">Okno Protokol aktivit Azure zobrazuje průběh nasazení.</span><span class="sxs-lookup"><span data-stu-id="2d800-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="2d800-184">Když je aplikace nasazena, přejdete k http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="2d800-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="2d800-185">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="2d800-185">Additional Resources</span></span>

- [<span data-ttu-id="2d800-186">Přehled projektu Katana</span><span class="sxs-lookup"><span data-stu-id="2d800-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="2d800-187">Katana projektu na Githubu</span><span class="sxs-lookup"><span data-stu-id="2d800-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
