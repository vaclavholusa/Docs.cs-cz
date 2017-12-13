---
uid: signalr/overview/performance/scaleout-with-sql-server
title: "Škálování SignalR s SQL serverem | Microsoft Docs"
author: MikeWasson
description: "Verze softwaru použitým v tomto tématu Visual Studio 2013 .NET 4.5 SignalR verze 2 předchozí verze tohoto tématu informace o předchozích verzí aplikace..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 5bf625a1ef8cc8ceab0014fadfab0c8a23dbc8da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="e4b4d-103">Škálování SignalR s SQL serverem</span><span class="sxs-lookup"><span data-stu-id="e4b4d-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="e4b4d-104">podle [Karel Wasson](https://github.com/MikeWasson), [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="e4b4d-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="e4b4d-105">Verze softwaru použitým v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="e4b4d-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="e4b4d-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e4b4d-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="e4b4d-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="e4b4d-107">.NET 4.5</span></span>
> - <span data-ttu-id="e4b4d-108">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="e4b4d-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="e4b4d-109">Předchozí verze tohoto tématu</span><span class="sxs-lookup"><span data-stu-id="e4b4d-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="e4b4d-110">Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="e4b4d-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="e4b4d-111">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="e4b4d-111">Questions and comments</span></span>
> 
> <span data-ttu-id="e4b4d-112">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="e4b4d-113">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="e4b4d-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="e4b4d-114">V tomto kurzu použijete SQL Server k distribuci zpráv mezi aplikací SignalR, která je nasazena v dvě samostatné instance služby IIS.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="e4b4d-115">V tomto kurzu můžete spustit také na jeden testovací počítač, ale pokud chcete získat plný vliv, je potřeba nasadit aplikaci SignalR na dva nebo více serverů.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="e4b4d-116">Musíte také nainstalovat SQL Server na jednom ze serverů nebo na samostatný vyhrazený server.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="e4b4d-117">Další možností je spustit kurz pomocí virtuálních počítačů v Azure.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="e4b4d-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e4b4d-118">Prerequisites</span></span>

<span data-ttu-id="e4b4d-119">Microsoft SQL Server 2005 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="e4b4d-120">Propojovacího rozhraní podporuje stolní počítače a servery edice systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="e4b4d-121">Nepodporuje se SQL Server Compact Edition nebo Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="e4b4d-122">(Pokud je vaše aplikace hostována v Azure, zvažte propojovacího rozhraní služby Service Bus místo.)</span><span class="sxs-lookup"><span data-stu-id="e4b4d-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="e4b4d-123">Přehled</span><span class="sxs-lookup"><span data-stu-id="e4b4d-123">Overview</span></span>

<span data-ttu-id="e4b4d-124">Předtím, než se nám získat podrobný kurz, zde je rychlý přehled toho, co jste se dělat.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="e4b4d-125">Vytvoření nové prázdné databáze.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-125">Create a new empty database.</span></span> <span data-ttu-id="e4b4d-126">Propojovacího rozhraní vytvoří nezbytné tabulky v této databázi.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="e4b4d-127">Přidejte tyto balíčky NuGet do vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="e4b4d-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="e4b4d-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="e4b4d-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="e4b4d-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="e4b4d-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="e4b4d-130">Vytvoření aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="e4b4d-131">Přidejte následující kód do souboru Startup.cs konfigurace propojovacího rozhraní:</span><span class="sxs-lookup"><span data-stu-id="e4b4d-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

 <span data-ttu-id="e4b4d-132">Tento kód konfiguruje propojovacího rozhraní s výchozími hodnotami u [TableCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) a [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="e4b4d-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="e4b4d-133">Informace o změně těchto hodnot najdete v tématu [výkonu SignalR: metriky škálování](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="e4b4d-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="e4b4d-134">Konfigurace databáze</span><span class="sxs-lookup"><span data-stu-id="e4b4d-134">Configure the Database</span></span>

<span data-ttu-id="e4b4d-135">Rozhodněte, jestli aplikaci pomocí ověřování systému Windows nebo ověřování serveru SQL Server pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="e4b4d-136">Bez ohledu na to Ujistěte se, že databáze uživatel má oprávnění k přihlášení, vytvořte schémata a vytvářet tabulky.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="e4b4d-137">Vytvořte novou databázi pro propojovacího rozhraní používat.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="e4b4d-138">Databázi můžete přiřadit libovolný název.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-138">You can give the database any name.</span></span> <span data-ttu-id="e4b4d-139">Nemusíte vytvářet všechny tabulky v databázi. propojovacího rozhraní vytvoří nezbytné tabulky.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="e4b4d-140">Povolte službu Service Broker</span><span class="sxs-lookup"><span data-stu-id="e4b4d-140">Enable Service Broker</span></span>

<span data-ttu-id="e4b4d-141">Doporučujeme povolit služby Service Broker pro databázi propojovacího rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="e4b4d-142">Service Broker poskytuje nativní podporu pro zasílání zpráv a služby Řízení front v systému SQL Server, který umožňuje získat aktualizace efektivněji propojovacího rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="e4b4d-143">(Však propojovacího rozhraní také funguje bez služby Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="e4b4d-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="e4b4d-144">Chcete-li zkontrolovat, zda je povoleno služby Service Broker, dotaz **je\_zprostředkovatele\_povoleno** sloupec v **zobrazení sys.databases** katalogu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="e4b4d-145">Pokud chcete povolit služby Service Broker, pomocí následujícího dotazu SQL:</span><span class="sxs-lookup"><span data-stu-id="e4b4d-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="e4b4d-146">Pokud tento dotaz zobrazí zablokování, ujistěte se, nejsou žádné aplikace, připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="e4b4d-147">Pokud jste povolili trasování, trasování také zobrazit, zda je povoleno služby Service Broker.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="e4b4d-148">Vytvoření aplikace SignalR</span><span class="sxs-lookup"><span data-stu-id="e4b4d-148">Create a SignalR Application</span></span>

<span data-ttu-id="e4b4d-149">Vytvoření aplikace SignalR podle buď tyto kurzy:</span><span class="sxs-lookup"><span data-stu-id="e4b4d-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="e4b4d-150">Začínáme s SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="e4b4d-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="e4b4d-151">Začínáme s SignalR 2.0 a MVC 5</span><span class="sxs-lookup"><span data-stu-id="e4b4d-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="e4b4d-152">V dalším kroku upravíme chatovací aplikace pro podporu škálování s SQL serverem.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="e4b4d-153">Nejprve přidejte balíček SignalR.SqlServer NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="e4b4d-154">V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="e4b4d-155">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e4b4d-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="e4b4d-156">Dále otevřete soubor Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="e4b4d-157">Přidejte následující kód, který **konfigurace** metoda:</span><span class="sxs-lookup"><span data-stu-id="e4b4d-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="e4b4d-158">Nasazení a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e4b4d-158">Deploy and Run the Application</span></span>

<span data-ttu-id="e4b4d-159">Připravte vaší instance serveru Windows k nasazení aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="e4b4d-160">Přidejte roli služby IIS.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-160">Add the IIS role.</span></span> <span data-ttu-id="e4b4d-161">Obsahují funkce "Vývoj aplikací", včetně protokol WebSocket.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="e4b4d-162">Zahrnují taky službu správy (uvedené v části "Nástroje pro správu").</span><span class="sxs-lookup"><span data-stu-id="e4b4d-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="e4b4d-163">**Instalaci Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="e4b4d-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="e4b4d-164">Při spuštění Správce služby IIS, vás vyzve k instalaci webové platformy společnosti Microsoft nebo můžete [stáhnout intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="e4b4d-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="e4b4d-165">V instalačním programu platformy vyhledávání pro nasazení webu a nainstalovat nasazení webu 3.0</span><span class="sxs-lookup"><span data-stu-id="e4b4d-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="e4b4d-166">Zkontrolujte, zda je spuštěna služba webové správy.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="e4b4d-167">V opačném případě spusťte službu.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-167">If not, start the service.</span></span> <span data-ttu-id="e4b4d-168">(Pokud nevidíte v seznamu služeb systému Windows služby webové správy, ujistěte se, že jste nainstalovali službu správy při přidání IIS role.)</span><span class="sxs-lookup"><span data-stu-id="e4b4d-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="e4b4d-169">Nakonec otevřete port 8172 pro protokol TCP.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="e4b4d-170">Toto je port, který používá nástroj pro nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="e4b4d-171">Nyní jste připraveni k nasazení projektu sady Visual Studio v počítači pro vývoj na server.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="e4b4d-172">V Průzkumníku řešení klikněte pravým tlačítkem na řešení a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="e4b4d-173">Podrobnější dokumentaci k nasazení webu, najdete v článku [webové mapa obsahu nasazení pro Visual Studio a ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="e4b4d-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="e4b4d-174">Pokud nasadíte aplikaci do dvou serverů, můžete otevřít každou instanci v samostatném okně prohlížeče a zjistit, každá z jiných přijímat zprávy SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="e4b4d-175">(Samozřejmě v produkčním prostředí, oba servery by nacházejí za službou Vyrovnávání zatížení.)</span><span class="sxs-lookup"><span data-stu-id="e4b4d-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="e4b4d-176">Po spuštění aplikace, můžete zjistit, že SignalR vytvořil automaticky tabulky v databázi:</span><span class="sxs-lookup"><span data-stu-id="e4b4d-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="e4b4d-177">SignalR spravuje tabulky.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-177">SignalR manages the tables.</span></span> <span data-ttu-id="e4b4d-178">Tak dlouho, dokud vaše aplikace je nasazená, nemáte odstranit řádky, úprava v tabulce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="e4b4d-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
