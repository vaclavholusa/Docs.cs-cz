---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hostitel rozhraní ASP.NET Web API 2 roli pracovního procesu Azure | Microsoft Docs
author: MikeWasson
description: Tento kurz ukazuje, jak k hostování role pracovního procesu Azure, rozhraní ASP.NET Web API pomocí k hostování na vlastním rozhraní Web API OWIN. Otevřít Web Interface pro .NET (OWIN) de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 7ba1dc850e2f9d9c88e6ddf263a796e1867a98be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="98809-104">Hostitel rozhraní ASP.NET Web API 2 roli pracovního procesu systému Azure</span><span class="sxs-lookup"><span data-stu-id="98809-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="98809-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="98809-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="98809-106">Tento kurz ukazuje, jak k hostování role pracovního procesu Azure, rozhraní ASP.NET Web API pomocí k hostování na vlastním rozhraní Web API OWIN.</span><span class="sxs-lookup"><span data-stu-id="98809-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="98809-107">[Otevřete Web Interface pro .NET](http://owin.org/) (OWIN) definuje abstrakci mezi .NET webové servery a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="98809-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="98809-108">OWIN oddělí webové aplikace ze serveru, takže je ideální pro hostování samoobslužné webové aplikace v vlastní proces mimo službu IIS OWIN – například uvnitř o roli pracovního procesu systému Azure.</span><span class="sxs-lookup"><span data-stu-id="98809-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="98809-109">V tomto kurzu budete používat Microsoft.Owin.Host.HttpListener balíčku, který poskytuje HTTP server, použije k hostování na vlastním aplikací OWIN.</span><span class="sxs-lookup"><span data-stu-id="98809-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="98809-110">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="98809-110">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="98809-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="98809-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="98809-112">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="98809-112">Web API 2</span></span>
> - [<span data-ttu-id="98809-113">Azure SDK pro .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="98809-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="98809-114">Vytvoření projektu Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="98809-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="98809-115">Spuštění sady Visual Studio s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="98809-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="98809-116">K ladění aplikace místně, pomocí emulátor výpočtů v Azure jsou potřeba oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="98809-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="98809-117">Na **soubor** nabídky, klikněte na tlačítko **nový**, pak klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="98809-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="98809-118">Z **nainstalovaných šablonách**, v části Visual C#, klikněte na **cloudu** a pak klikněte na **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="98809-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="98809-119">Název projektu "AzureApp" a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="98809-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="98809-120">V **nová Cloudová služba Windows Azure** dialogové okno, dvakrát klikněte na **Role pracovního procesu**.</span><span class="sxs-lookup"><span data-stu-id="98809-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="98809-121">Ponechte výchozí název ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="98809-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="98809-122">Tento krok přidává roli pracovního procesu k řešení.</span><span class="sxs-lookup"><span data-stu-id="98809-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="98809-123">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="98809-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="98809-124">Řešení nástroje Visual Studio, který se vytvoří obsahuje dva projekty:</span><span class="sxs-lookup"><span data-stu-id="98809-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="98809-125">&quot;AzureApp&quot; definuje role a konfigurace pro aplikaci Azure.</span><span class="sxs-lookup"><span data-stu-id="98809-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="98809-126">&quot;WorkerRole1&quot; obsahuje kód pro roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="98809-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="98809-127">Obecně platí aplikaci Azure může obsahovat víc rolí, i když tento kurz používá jednu roli.</span><span class="sxs-lookup"><span data-stu-id="98809-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="98809-128">Přidání webového rozhraní API a balíčky OWIN</span><span class="sxs-lookup"><span data-stu-id="98809-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="98809-129">Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven**, pak klikněte na tlačítko **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="98809-129">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="98809-130">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="98809-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="98809-131">Přidání koncového bodu protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="98809-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="98809-132">V Průzkumníku řešení rozbalte AzureApp projektu.</span><span class="sxs-lookup"><span data-stu-id="98809-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="98809-133">Rozbalte uzel role, klikněte pravým tlačítkem na WorkerRole1 a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="98809-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="98809-134">Klikněte na tlačítko **koncové body**a potom klikněte na **přidat koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="98809-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="98809-135">V **protokol** rozevíracího seznamu vyberte možnost "http".</span><span class="sxs-lookup"><span data-stu-id="98809-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="98809-136">V **veřejný Port** a **privátní Port**, zadejte 80.</span><span class="sxs-lookup"><span data-stu-id="98809-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="98809-137">Tato čísla portů se může lišit.</span><span class="sxs-lookup"><span data-stu-id="98809-137">These port numbers can be different.</span></span> <span data-ttu-id="98809-138">Veřejný port je co klienti používat při odesílání požadavku do role.</span><span class="sxs-lookup"><span data-stu-id="98809-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="98809-139">Konfigurace webového rozhraní API pro hostování na vlastním serveru</span><span class="sxs-lookup"><span data-stu-id="98809-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="98809-140">V Průzkumníku řešení klikněte pravým tlačítkem na projekt WorkerRole1 a vyberte **přidat** / **třída** pro přidání nové třídy.</span><span class="sxs-lookup"><span data-stu-id="98809-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="98809-141">Název třídy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="98809-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="98809-142">Všechny často používaný kód v tomto souboru nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="98809-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="98809-143">Přidat řadič webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="98809-143">Add a Web API Controller</span></span>

<span data-ttu-id="98809-144">V dalším kroku přidejte třídu kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="98809-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="98809-145">Klikněte pravým tlačítkem na projekt WorkerRole1 a vyberte **přidat** / **třída**.</span><span class="sxs-lookup"><span data-stu-id="98809-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="98809-146">Název třídy TestController.</span><span class="sxs-lookup"><span data-stu-id="98809-146">Name the class TestController.</span></span> <span data-ttu-id="98809-147">Všechny často používaný kód v tomto souboru nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="98809-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="98809-148">Pro jednoduchost definuje tento řadič jen dvě metody GET, které vracejí prostý text.</span><span class="sxs-lookup"><span data-stu-id="98809-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="98809-149">Spuštění hostitele OWIN</span><span class="sxs-lookup"><span data-stu-id="98809-149">Start the OWIN Host</span></span>

<span data-ttu-id="98809-150">Otevřete soubor WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="98809-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="98809-151">Tato třída definuje kód, který je spuštěn při spuštění a zastavení role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="98809-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="98809-152">Přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="98809-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="98809-153">Přidat **IDisposable** člena `WorkerRole` třídy:</span><span class="sxs-lookup"><span data-stu-id="98809-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="98809-154">V `OnStart` metoda, přidejte následující kód pro spuštění hostitele:</span><span class="sxs-lookup"><span data-stu-id="98809-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="98809-155">**WebApp.Start** metoda spustí hostitele OWIN.</span><span class="sxs-lookup"><span data-stu-id="98809-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="98809-156">Název `Startup` třída je typ parametru metody.</span><span class="sxs-lookup"><span data-stu-id="98809-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="98809-157">Podle konvence, bude volat hostitele `Configure` metoda této třídy.</span><span class="sxs-lookup"><span data-stu-id="98809-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="98809-158">Přepsání `OnStop` k odstranění  *\_aplikace* instance:</span><span class="sxs-lookup"><span data-stu-id="98809-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="98809-159">Tady je kompletní kód WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="98809-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="98809-160">Sestavte řešení a stisknutím klávesy F5 místně spustíte aplikaci v emulátoru výpočetní Azure.</span><span class="sxs-lookup"><span data-stu-id="98809-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="98809-161">V závislosti na nastavení brány firewall může musíte povolit emulátoru prostřednictvím brány firewall.</span><span class="sxs-lookup"><span data-stu-id="98809-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="98809-162">Pokud dojde k výjimce takto, najdete v tématu [tomto příspěvku na blogu](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="98809-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="98809-163">"Nelze načíst soubor nebo sestavení ' Microsoft.Owin, verze = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35, nebo jeden z jeho závislých.</span><span class="sxs-lookup"><span data-stu-id="98809-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="98809-164">Definice sestavení manifestu neodpovídá odkaz na sestavení.</span><span class="sxs-lookup"><span data-stu-id="98809-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="98809-165">(Výjimka z HRESULT: 0x80131040) "</span><span class="sxs-lookup"><span data-stu-id="98809-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="98809-166">Emulátoru služby výpočty v přiřadí místní IP adresu ke koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="98809-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="98809-167">Zobrazením uživatelské prostředí emulátoru výpočtů můžete najít IP adresu.</span><span class="sxs-lookup"><span data-stu-id="98809-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="98809-168">Klikněte pravým tlačítkem myši na ikonu emulátoru na hlavním panelu oznamovací oblasti a vyberte **zobrazit uživatelské prostředí emulátoru výpočtů**.</span><span class="sxs-lookup"><span data-stu-id="98809-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="98809-169">Najít IP adresu v rámci nasazení služby, nasazení [id] podrobnosti ze služby.</span><span class="sxs-lookup"><span data-stu-id="98809-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="98809-170">Otevřete webový prohlížeč a přejděte na http://<em>adresu</em>/testovací/1, kde <em>adresu</em> je IP adresa přiřadila emulátoru služby výpočty v; například `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="98809-170">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="98809-171">Měli byste vidět odpovědi z kontroleru webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="98809-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="98809-172">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="98809-172">Deploy to Azure</span></span>

<span data-ttu-id="98809-173">V tomto kroku musí mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="98809-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="98809-174">Pokud jste již nemáte, můžete si během několika minut vytvořit Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="98809-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="98809-175">Podrobnosti najdete v tématu [bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="98809-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="98809-176">V Průzkumníku řešení klikněte pravým tlačítkem na projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="98809-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="98809-177">Vyberte **publikování**.</span><span class="sxs-lookup"><span data-stu-id="98809-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="98809-178">Pokud nejste přihlášení k účtu Azure, klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="98809-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="98809-179">Když jste přihlášeni, vyberte předplatné a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="98809-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="98809-180">Zadejte název pro cloudové služby a vyberte oblast.</span><span class="sxs-lookup"><span data-stu-id="98809-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="98809-181">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="98809-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="98809-182">Klikněte na tlačítko **publikování**.</span><span class="sxs-lookup"><span data-stu-id="98809-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="98809-183">Okno Protokol činnosti Azure zobrazuje průběh nasazení.</span><span class="sxs-lookup"><span data-stu-id="98809-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="98809-184">Při nasazení aplikace, přejděte do http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="98809-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="98809-185">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="98809-185">Additional Resources</span></span>

- [<span data-ttu-id="98809-186">Přehled projektu Katana</span><span class="sxs-lookup"><span data-stu-id="98809-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="98809-187">Katana projektu na Githubu</span><span class="sxs-lookup"><span data-stu-id="98809-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
