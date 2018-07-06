---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hostování OWIN v roli pracovního procesu Azure | Dokumentace Microsoftu
author: MikeWasson
description: Tento kurz ukazuje, jak k samoobslužnému hostování OWIN v roli pracovního procesu Microsoft Azure. Open Web Interface pro .NET (OWIN) definuje abstrakce mezi .NET webový server...
ms.author: aspnetcontent
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: f62b9299a4e369ae3a938c85e60dd6a79108548d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826478"
---
<a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="ac3ad-104">Hostování specifikace OWIN v roli pracovního procesu Azure</span><span class="sxs-lookup"><span data-stu-id="ac3ad-104">Host OWIN in an Azure Worker Role</span></span>
====================
<span data-ttu-id="ac3ad-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ac3ad-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="ac3ad-106">Tento kurz ukazuje, jak k samoobslužnému hostování OWIN v roli pracovního procesu Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
> 
> <span data-ttu-id="ac3ad-107">[Otevřete Web Interface pro .NET](http://owin.org/) (OWIN) definuje abstrakce mezi .NET webové servery a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="ac3ad-108">Webové aplikace ze serveru, který je ideální pro webové aplikace ve vašem vlastním procesu mimo službu IIS s vlastním hostováním OWIN OWIN odděluje obě části – například v roli pracovního procesu systému Azure.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="ac3ad-109">V tomto kurzu se dozvíte, jak hostování na vlastním serveru aplikací OWIN v roli pracovního procesu Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="ac3ad-110">Další informace o rolích pracovního procesu najdete v tématu [spouštění modelů Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="ac3ad-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ac3ad-111">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="ac3ad-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="ac3ad-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="ac3ad-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="ac3ad-113">Sada Azure SDK for .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="ac3ad-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="ac3ad-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="ac3ad-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="ac3ad-115">Vytvořte projekt Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="ac3ad-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="ac3ad-116">Spusťte Visual Studio s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="ac3ad-117">Oprávnění správce jsou potřebné k ladění aplikace v místním prostředí, pomocí emulátoru služby výpočty v Azure.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="ac3ad-118">Na **souboru** nabídky, klikněte na tlačítko **nový**, pak klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="ac3ad-119">Z **nainstalované šablony**, v části Visual C#, klikněte na tlačítko **cloudu** a potom klikněte na tlačítko **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="ac3ad-120">Pojmenujte projekt "AzureApp" a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="ac3ad-121">V **nová Cloudová služba Windows Azure** dialogového okna, dvakrát klikněte na panel **Role pracovního procesu**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="ac3ad-122">Ponechte výchozí název ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="ac3ad-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="ac3ad-123">Tento krok přidává role pracovního procesu k řešení.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="ac3ad-124">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="ac3ad-125">Řešení sady Visual Studio, který je vytvořen obsahuje dva projekty:</span><span class="sxs-lookup"><span data-stu-id="ac3ad-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="ac3ad-126">&quot;AzureApp&quot; definuje role a konfigurace pro aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="ac3ad-127">&quot;WorkerRole1&quot; obsahuje kód pro roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="ac3ad-128">Obecně platí aplikace Azure může obsahovat více rolí, i když tento kurz používá jednu roli.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="ac3ad-129">Přidání balíčků samoobslužné hostování OWIN</span><span class="sxs-lookup"><span data-stu-id="ac3ad-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="ac3ad-130">Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven**, pak klikněte na tlačítko **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-130">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="ac3ad-131">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ac3ad-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="ac3ad-132">Přidat koncový bod HTTP</span><span class="sxs-lookup"><span data-stu-id="ac3ad-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="ac3ad-133">V Průzkumníku řešení rozbalte projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="ac3ad-134">Rozbalte uzel role, klikněte pravým tlačítkem na WorkerRole1 a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="ac3ad-135">Klikněte na tlačítko **koncové body**a potom klikněte na tlačítko **přidat koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="ac3ad-136">V **protokol** rozevíracího seznamu vyberte možnost "http".</span><span class="sxs-lookup"><span data-stu-id="ac3ad-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="ac3ad-137">V **veřejný Port** a **privátní Port**, zadejte 80.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="ac3ad-138">Tato čísla portů se mohou lišit.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-138">These port numbers can be different.</span></span> <span data-ttu-id="ac3ad-139">Veřejný port je, co klienti používat při odesílání požadavku do role.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="ac3ad-140">Vytvoření třídy pro spuštění OWIN</span><span class="sxs-lookup"><span data-stu-id="ac3ad-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="ac3ad-141">V Průzkumníku řešení klikněte pravým tlačítkem myši klikněte na WorkerRole1 projekt a vyberte **přidat** / **třídy** přidání nové třídy.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="ac3ad-142">Název třídy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-142">Name the class `Startup`.</span></span>

<span data-ttu-id="ac3ad-143">Nahraďte všechny často používaný kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ac3ad-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="ac3ad-144">`UseWelcomePage` – Metoda rozšíření přidá jednoduché stránky HTML pro vaši aplikaci, ověřte funkčnost lokality.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="ac3ad-145">Spuštění hostitele OWIN</span><span class="sxs-lookup"><span data-stu-id="ac3ad-145">Start the OWIN Host</span></span>

<span data-ttu-id="ac3ad-146">Otevřete soubor WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="ac3ad-147">Tato třída definuje kód, který se spustí při spuštění a zastavení role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="ac3ad-148">Přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="ac3ad-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="ac3ad-149">Přidat **IDisposable** člena `WorkerRole` třídy:</span><span class="sxs-lookup"><span data-stu-id="ac3ad-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="ac3ad-150">V `OnStart` metodu, přidejte následující kód pro spuštění hostitele:</span><span class="sxs-lookup"><span data-stu-id="ac3ad-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="ac3ad-151">**WebApp.Start** metoda spustí hostitele OWIN.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="ac3ad-152">Název `Startup` třídy je parametr typu metody.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="ac3ad-153">Podle konvence, bude volat hostitele `Configure` metody této třídy.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="ac3ad-154">Přepsat `OnStop` a neprovede  *\_aplikace* instance:</span><span class="sxs-lookup"><span data-stu-id="ac3ad-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="ac3ad-155">Tady je kompletní kód WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="ac3ad-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="ac3ad-156">Sestavte řešení a stisknutím klávesy F5 spusťte aplikaci místně v emulátoru Azure Compute.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="ac3ad-157">V závislosti na nastavení brány firewall potřebujete povolit emulátor přes bránu firewall.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="ac3ad-158">Emulátor služby výpočty místní IP adresa přiřadí ke koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="ac3ad-159">IP adresu můžete najít zobrazením Uživatelském prostředí emulátoru výpočtů.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="ac3ad-160">Klikněte pravým tlačítkem na hlavním panelu oznamovací oblasti ikonu emulátoru a vyberte **zobrazit Uživatelském prostředí emulátoru výpočtů**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="ac3ad-161">Zjistit IP adresu v rámci nasazení služeb, nasazení [id], podrobnosti o službě.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="ac3ad-162">Otevřete webový prohlížeč a přejdete na http://<em>adresu</em>, kde <em>adresu</em> je IP adresa přidělí emulátor služby výpočty; například `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-162">Open a web browser and navigate to http://<em>address</em>, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="ac3ad-163">Zobrazí se úvodní stránka OWIN:</span><span class="sxs-lookup"><span data-stu-id="ac3ad-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="ac3ad-164">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="ac3ad-164">Deploy to Azure</span></span>

<span data-ttu-id="ac3ad-165">Pro tento krok musí mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="ac3ad-166">Pokud již nemáte, můžete během několika minut vytvořit Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ac3ad-167">Podrobnosti najdete v tématu [bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="ac3ad-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="ac3ad-168">V Průzkumníku řešení klikněte pravým tlačítkem na projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="ac3ad-169">Vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="ac3ad-170">Pokud nejste přihlášení k účtu Azure, klikněte na tlačítko **Sign In**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="ac3ad-171">Když jste přihlášeni, zvolte předplatné a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="ac3ad-172">Zadejte název pro cloudovou službu a zvolte oblast.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="ac3ad-173">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="ac3ad-174">Klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="ac3ad-175">Okno Protokol aktivit Azure zobrazuje průběh nasazení.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="ac3ad-176">Když je aplikace nasazena, přejdete k `http://appname.cloudapp.net/`, kde *appname* je název vaší cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="ac3ad-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ac3ad-177">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="ac3ad-177">Additional Resources</span></span>

- [<span data-ttu-id="ac3ad-178">Přehled projektu Katana</span><span class="sxs-lookup"><span data-stu-id="ac3ad-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="ac3ad-179">Katana projektu na Githubu</span><span class="sxs-lookup"><span data-stu-id="ac3ad-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
