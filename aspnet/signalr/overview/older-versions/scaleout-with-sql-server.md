---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Škálování SignalR s SQL serverem (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7a589f5c6a5196444c3c616b39f501f3a7512471
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26565987"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="e4f94-102">Škálování SignalR s SQL serverem (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="e4f94-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="e4f94-103">podle [Karel Wasson](https://github.com/MikeWasson), [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="e4f94-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="e4f94-104">V tomto kurzu použijete SQL Server k distribuci zpráv mezi aplikací SignalR, která je nasazena v dvě samostatné instance služby IIS.</span><span class="sxs-lookup"><span data-stu-id="e4f94-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="e4f94-105">V tomto kurzu můžete spustit také na jeden testovací počítač, ale pokud chcete získat plný vliv, je potřeba nasadit aplikaci SignalR na dva nebo více serverů.</span><span class="sxs-lookup"><span data-stu-id="e4f94-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="e4f94-106">Musíte také nainstalovat SQL Server na jednom ze serverů nebo na samostatný vyhrazený server.</span><span class="sxs-lookup"><span data-stu-id="e4f94-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="e4f94-107">Další možností je spustit kurz pomocí virtuálních počítačů v Azure.</span><span class="sxs-lookup"><span data-stu-id="e4f94-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="e4f94-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e4f94-108">Prerequisites</span></span>

<span data-ttu-id="e4f94-109">Microsoft SQL Server 2005 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e4f94-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="e4f94-110">Propojovacího rozhraní podporuje stolní počítače a servery edice systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e4f94-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="e4f94-111">Nepodporuje se SQL Server Compact Edition nebo Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="e4f94-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="e4f94-112">(Pokud je vaše aplikace hostována v Azure, zvažte propojovacího rozhraní služby Service Bus místo.)</span><span class="sxs-lookup"><span data-stu-id="e4f94-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="e4f94-113">Přehled</span><span class="sxs-lookup"><span data-stu-id="e4f94-113">Overview</span></span>

<span data-ttu-id="e4f94-114">Předtím, než se nám získat podrobný kurz, zde je rychlý přehled toho, co jste se dělat.</span><span class="sxs-lookup"><span data-stu-id="e4f94-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="e4f94-115">Vytvoření nové prázdné databáze.</span><span class="sxs-lookup"><span data-stu-id="e4f94-115">Create a new empty database.</span></span> <span data-ttu-id="e4f94-116">Propojovacího rozhraní vytvoří nezbytné tabulky v této databázi.</span><span class="sxs-lookup"><span data-stu-id="e4f94-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="e4f94-117">Přidejte tyto balíčky NuGet do vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="e4f94-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="e4f94-118">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="e4f94-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="e4f94-119">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="e4f94-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="e4f94-120">Vytvoření aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f94-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="e4f94-121">Přidejte následující kód do souboru Global.asax konfigurace propojovacího rozhraní:</span><span class="sxs-lookup"><span data-stu-id="e4f94-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="e4f94-122">Konfigurace databáze</span><span class="sxs-lookup"><span data-stu-id="e4f94-122">Configure the Database</span></span>

<span data-ttu-id="e4f94-123">Rozhodněte, jestli aplikaci pomocí ověřování systému Windows nebo ověřování serveru SQL Server pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="e4f94-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="e4f94-124">Bez ohledu na to Ujistěte se, že databáze uživatel má oprávnění k přihlášení, vytvořte schémata a vytvářet tabulky.</span><span class="sxs-lookup"><span data-stu-id="e4f94-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="e4f94-125">Vytvořte novou databázi pro propojovacího rozhraní používat.</span><span class="sxs-lookup"><span data-stu-id="e4f94-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="e4f94-126">Databázi můžete přiřadit libovolný název.</span><span class="sxs-lookup"><span data-stu-id="e4f94-126">You can give the database any name.</span></span> <span data-ttu-id="e4f94-127">Nemusíte vytvářet všechny tabulky v databázi. propojovacího rozhraní vytvoří nezbytné tabulky.</span><span class="sxs-lookup"><span data-stu-id="e4f94-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="e4f94-128">Povolte službu Service Broker</span><span class="sxs-lookup"><span data-stu-id="e4f94-128">Enable Service Broker</span></span>

<span data-ttu-id="e4f94-129">Doporučujeme povolit služby Service Broker pro databázi propojovacího rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e4f94-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="e4f94-130">Service Broker poskytuje nativní podporu pro zasílání zpráv a služby Řízení front v systému SQL Server, který umožňuje získat aktualizace efektivněji propojovacího rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e4f94-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="e4f94-131">(Však propojovacího rozhraní také funguje bez služby Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="e4f94-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="e4f94-132">Chcete-li zkontrolovat, zda je povoleno služby Service Broker, dotaz **je\_zprostředkovatele\_povoleno** sloupec v **zobrazení sys.databases** katalogu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e4f94-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="e4f94-133">Pokud chcete povolit služby Service Broker, pomocí následujícího dotazu SQL:</span><span class="sxs-lookup"><span data-stu-id="e4f94-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="e4f94-134">Pokud tento dotaz zobrazí zablokování, ujistěte se, nejsou žádné aplikace, připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="e4f94-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="e4f94-135">Pokud jste povolili trasování, trasování také zobrazit, zda je povoleno služby Service Broker.</span><span class="sxs-lookup"><span data-stu-id="e4f94-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="e4f94-136">Vytvoření aplikace SignalR</span><span class="sxs-lookup"><span data-stu-id="e4f94-136">Create a SignalR Application</span></span>

<span data-ttu-id="e4f94-137">Vytvoření aplikace SignalR podle buď tyto kurzy:</span><span class="sxs-lookup"><span data-stu-id="e4f94-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="e4f94-138">Začínáme s SignalR</span><span class="sxs-lookup"><span data-stu-id="e4f94-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="e4f94-139">Začínáme s SignalR a MVC 4</span><span class="sxs-lookup"><span data-stu-id="e4f94-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="e4f94-140">V dalším kroku upravíme chatovací aplikace pro podporu škálování s SQL serverem.</span><span class="sxs-lookup"><span data-stu-id="e4f94-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="e4f94-141">Nejprve přidejte balíček SignalR.SqlServer NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="e4f94-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="e4f94-142">V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="e4f94-142">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="e4f94-143">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e4f94-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="e4f94-144">Dále otevřete soubor Global.asax.</span><span class="sxs-lookup"><span data-stu-id="e4f94-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="e4f94-145">Přidejte následující kód, který **aplikace\_spustit** metoda:</span><span class="sxs-lookup"><span data-stu-id="e4f94-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="e4f94-146">Nasazení a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e4f94-146">Deploy and Run the Application</span></span>

<span data-ttu-id="e4f94-147">Připravte vaší instance serveru Windows k nasazení aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f94-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="e4f94-148">Přidejte roli služby IIS.</span><span class="sxs-lookup"><span data-stu-id="e4f94-148">Add the IIS role.</span></span> <span data-ttu-id="e4f94-149">Obsahují funkce "Vývoj aplikací", včetně protokol WebSocket.</span><span class="sxs-lookup"><span data-stu-id="e4f94-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="e4f94-150">Zahrnují taky službu správy (uvedené v části "Nástroje pro správu").</span><span class="sxs-lookup"><span data-stu-id="e4f94-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="e4f94-151">**Instalaci Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="e4f94-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="e4f94-152">Při spuštění Správce služby IIS, vás vyzve k instalaci webové platformy společnosti Microsoft nebo můžete [stáhnout intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="e4f94-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="e4f94-153">V instalačním programu platformy vyhledávání pro nasazení webu a nainstalovat nasazení webu 3.0</span><span class="sxs-lookup"><span data-stu-id="e4f94-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="e4f94-154">Zkontrolujte, zda je spuštěna služba webové správy.</span><span class="sxs-lookup"><span data-stu-id="e4f94-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="e4f94-155">V opačném případě spusťte službu.</span><span class="sxs-lookup"><span data-stu-id="e4f94-155">If not, start the service.</span></span> <span data-ttu-id="e4f94-156">(Pokud nevidíte v seznamu služeb systému Windows služby webové správy, ujistěte se, že jste nainstalovali službu správy při přidání IIS role.)</span><span class="sxs-lookup"><span data-stu-id="e4f94-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="e4f94-157">Nakonec otevřete port 8172 pro protokol TCP.</span><span class="sxs-lookup"><span data-stu-id="e4f94-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="e4f94-158">Toto je port, který používá nástroj pro nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="e4f94-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="e4f94-159">Nyní jste připraveni k nasazení projektu sady Visual Studio v počítači pro vývoj na server.</span><span class="sxs-lookup"><span data-stu-id="e4f94-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="e4f94-160">V Průzkumníku řešení klikněte pravým tlačítkem na řešení a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="e4f94-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="e4f94-161">Podrobnější dokumentaci k nasazení webu, najdete v článku [webové mapa obsahu nasazení pro Visual Studio a ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="e4f94-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="e4f94-162">Pokud nasadíte aplikaci do dvou serverů, můžete otevřít každou instanci v samostatném okně prohlížeče a zjistit, každá z jiných přijímat zprávy SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f94-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="e4f94-163">(Samozřejmě v produkčním prostředí, oba servery by nacházejí za službou Vyrovnávání zatížení.)</span><span class="sxs-lookup"><span data-stu-id="e4f94-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="e4f94-164">Po spuštění aplikace, můžete zjistit, že SignalR vytvořil automaticky tabulky v databázi:</span><span class="sxs-lookup"><span data-stu-id="e4f94-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="e4f94-165">SignalR spravuje tabulky.</span><span class="sxs-lookup"><span data-stu-id="e4f94-165">SignalR manages the tables.</span></span> <span data-ttu-id="e4f94-166">Tak dlouho, dokud vaše aplikace je nasazená, nemáte odstranit řádky, úprava v tabulce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="e4f94-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
