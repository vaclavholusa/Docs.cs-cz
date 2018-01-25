---
uid: signalr/overview/performance/scaleout-with-redis
title: "Škálování SignalR s Redisem | Microsoft Docs"
author: MikeWasson
description: "Verze softwaru použitým v tomto tématu Visual Studio 2013 .NET 4.5 SignalR verze 2 předchozí verze tohoto tématu informace o předchozích verzí aplikace..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 2ef161f35e69ef4a754d2740199166ee48c3fbab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-redis"></a><span data-ttu-id="ab781-103">Škálování SignalR s Redisem</span><span class="sxs-lookup"><span data-stu-id="ab781-103">SignalR Scaleout with Redis</span></span>
====================
<span data-ttu-id="ab781-104">podle [Karel Wasson](https://github.com/MikeWasson), [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="ab781-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="ab781-105">Verze softwaru použitým v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="ab781-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="ab781-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="ab781-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="ab781-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="ab781-107">.NET 4.5</span></span>
> - <span data-ttu-id="ab781-108">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="ab781-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="ab781-109">Předchozí verze tohoto tématu</span><span class="sxs-lookup"><span data-stu-id="ab781-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="ab781-110">Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="ab781-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="ab781-111">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="ab781-111">Questions and comments</span></span>
> 
> <span data-ttu-id="ab781-112">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="ab781-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="ab781-113">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="ab781-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="ab781-114">V tomto kurzu budete používat [Redis](http://redis.io/) distribuci zprávy přes SignalR aplikace, který je nasazen na dvě samostatné instance služby IIS.</span><span class="sxs-lookup"><span data-stu-id="ab781-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="ab781-115">Redis je úložišti klíč hodnota v paměti.</span><span class="sxs-lookup"><span data-stu-id="ab781-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="ab781-116">Mimoto podporuje i systém zasílání zpráv s modelem publikování a přihlášení k odběru.</span><span class="sxs-lookup"><span data-stu-id="ab781-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="ab781-117">SignalR Redis propojovacího rozhraní používá protokol pub nebo sub funkci k předávání zpráv na jiné servery.</span><span class="sxs-lookup"><span data-stu-id="ab781-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="ab781-118">V tomto kurzu budete používat tři servery:</span><span class="sxs-lookup"><span data-stu-id="ab781-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="ab781-119">Dva servery se systémem Windows, který použijete k nasazení aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab781-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="ab781-120">Jeden server se systémem Linux, který použijete ke spuštění Redis.</span><span class="sxs-lookup"><span data-stu-id="ab781-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="ab781-121">Snímky obrazovky v tomto kurzu byl použit Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="ab781-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="ab781-122">Pokud nemáte tří fyzických serverů používat, můžete vytvořit virtuální počítače v Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="ab781-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="ab781-123">Další možností je vytvořit virtuální počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="ab781-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="ab781-124">I když tento kurz používá oficiální implementace Redis, k dispozici je také [Windows port Redis](https://github.com/MSOpenTech/redis) z MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="ab781-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="ab781-125">Instalace a konfigurace se liší, ale jinak kroky jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="ab781-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ab781-126">Škálování SignalR s Redis nepodporuje clustery Redis.</span><span class="sxs-lookup"><span data-stu-id="ab781-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="ab781-127">Přehled</span><span class="sxs-lookup"><span data-stu-id="ab781-127">Overview</span></span>

<span data-ttu-id="ab781-128">Předtím, než se nám získat podrobný kurz, zde je rychlý přehled toho, co jste se dělat.</span><span class="sxs-lookup"><span data-stu-id="ab781-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="ab781-129">Instalace Redis a spuštění serveru Redis.</span><span class="sxs-lookup"><span data-stu-id="ab781-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="ab781-130">Přidejte tyto balíčky NuGet do vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="ab781-130">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="ab781-131">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="ab781-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="ab781-132">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="ab781-132">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="ab781-133">Vytvoření aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab781-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="ab781-134">Přidejte následující kód do souboru Startup.cs konfigurace propojovacího rozhraní:</span><span class="sxs-lookup"><span data-stu-id="ab781-134">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="ab781-135">Ubuntu na technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="ab781-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="ab781-136">Pomocí Windows Hyper-V, můžete snadno vytvořit virtuálního počítače s Ubuntu v systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="ab781-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="ab781-137">Stáhnout soubor ISO Ubuntu od [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="ab781-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="ab781-138">Technologie Hyper-V přidejte nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ab781-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="ab781-139">V **připojit virtuální pevný Disk** krok, vyberte **vytvořit virtuální pevný disk**.</span><span class="sxs-lookup"><span data-stu-id="ab781-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="ab781-140">V **možnosti instalace** krok, vyberte **soubor bitové kopie (.iso)**, klikněte na tlačítko **Procházet**a přejděte do instalace Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="ab781-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="ab781-141">Nainstalujte Redis</span><span class="sxs-lookup"><span data-stu-id="ab781-141">Install Redis</span></span>

<span data-ttu-id="ab781-142">Postupujte podle kroků v [http://redis.io/download](http://redis.io/download) ke stažení a sestavení Redis.</span><span class="sxs-lookup"><span data-stu-id="ab781-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="ab781-143">Toto sestavení binárních souborů Redis `src` adresáře.</span><span class="sxs-lookup"><span data-stu-id="ab781-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="ab781-144">Ve výchozím nastavení Redis nevyžaduje heslo.</span><span class="sxs-lookup"><span data-stu-id="ab781-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="ab781-145">Chcete-li nastavit heslo, upravte `redis.conf` souboru, který se nachází v kořenovém adresáři zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="ab781-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="ab781-146">(Před zahájením úprav, vytvořili záložní kopii souboru!) Přidejte následující direktivu pro `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="ab781-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="ab781-147">Nyní spusťte serveru Redis:</span><span class="sxs-lookup"><span data-stu-id="ab781-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="ab781-148">Otevřete port 6379, což je výchozí port, který Redis naslouchá.</span><span class="sxs-lookup"><span data-stu-id="ab781-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="ab781-149">(Můžete změnit číslo portu v konfiguračním souboru.)</span><span class="sxs-lookup"><span data-stu-id="ab781-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="ab781-150">Vytvoření aplikace nástroje SignalR</span><span class="sxs-lookup"><span data-stu-id="ab781-150">Create the SignalR Application</span></span>

<span data-ttu-id="ab781-151">Vytvoření aplikace SignalR podle buď tyto kurzy:</span><span class="sxs-lookup"><span data-stu-id="ab781-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="ab781-152">Začínáme s SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="ab781-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="ab781-153">Začínáme s SignalR 2.0 a MVC 5</span><span class="sxs-lookup"><span data-stu-id="ab781-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="ab781-154">V dalším kroku upravíme chatovací aplikace pro podporu škálování s Redis.</span><span class="sxs-lookup"><span data-stu-id="ab781-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="ab781-155">Nejprve přidejte balíček SignalR.Redis NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="ab781-155">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="ab781-156">V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="ab781-156">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="ab781-157">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ab781-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="ab781-158">Dále otevřete soubor Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="ab781-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="ab781-159">Přidejte následující kód, který **konfigurace** metoda:</span><span class="sxs-lookup"><span data-stu-id="ab781-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="ab781-160">"server" je název serveru, který běží Redis.</span><span class="sxs-lookup"><span data-stu-id="ab781-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="ab781-161">*port* je číslo portu</span><span class="sxs-lookup"><span data-stu-id="ab781-161">*port* is the port number</span></span>
- <span data-ttu-id="ab781-162">"password" je heslo, které jste zadali v souboru redis.conf.</span><span class="sxs-lookup"><span data-stu-id="ab781-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="ab781-163">"AppName" je libovolný řetězec.</span><span class="sxs-lookup"><span data-stu-id="ab781-163">"AppName" is any string.</span></span> <span data-ttu-id="ab781-164">SignalR vytvoří kanál pub nebo sub Redis s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="ab781-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="ab781-165">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ab781-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="ab781-166">Nasazení a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ab781-166">Deploy and Run the Application</span></span>

<span data-ttu-id="ab781-167">Připravte vaší instance serveru Windows k nasazení aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab781-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="ab781-168">Přidejte roli služby IIS.</span><span class="sxs-lookup"><span data-stu-id="ab781-168">Add the IIS role.</span></span> <span data-ttu-id="ab781-169">Obsahují funkce "Vývoj aplikací", včetně protokol WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ab781-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="ab781-170">Zahrnují taky službu správy (uvedené v části "Nástroje pro správu").</span><span class="sxs-lookup"><span data-stu-id="ab781-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="ab781-171">**Instalaci Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="ab781-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="ab781-172">Při spuštění Správce služby IIS, vás vyzve k instalaci webové platformy společnosti Microsoft nebo můžete [stáhnout intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="ab781-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="ab781-173">V instalačním programu platformy vyhledávání pro nasazení webu a nainstalovat nasazení webu 3.0</span><span class="sxs-lookup"><span data-stu-id="ab781-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="ab781-174">Zkontrolujte, zda je spuštěna služba webové správy.</span><span class="sxs-lookup"><span data-stu-id="ab781-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="ab781-175">V opačném případě spusťte službu.</span><span class="sxs-lookup"><span data-stu-id="ab781-175">If not, start the service.</span></span> <span data-ttu-id="ab781-176">(Pokud nevidíte v seznamu služeb systému Windows služby webové správy, ujistěte se, že jste nainstalovali službu správy při přidání IIS role.)</span><span class="sxs-lookup"><span data-stu-id="ab781-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="ab781-177">Ve výchozím nastavení služba webové správy naslouchá na portu TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="ab781-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="ab781-178">V bráně Windows Firewall vytvořte nové příchozí pravidlo umožňující přenos TCP na portu 8172.</span><span class="sxs-lookup"><span data-stu-id="ab781-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="ab781-179">Další informace najdete v tématu [konfigurace pravidel brány Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="ab781-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="ab781-180">(Pokud jsou hostiteli virtuálních počítačů v Azure, musíte přímo na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ab781-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="ab781-181">V tématu [jak nastavit koncové body k virtuálnímu počítači](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="ab781-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="ab781-182">Nyní jste připraveni k nasazení projektu sady Visual Studio v počítači pro vývoj na server.</span><span class="sxs-lookup"><span data-stu-id="ab781-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="ab781-183">V Průzkumníku řešení klikněte pravým tlačítkem na řešení a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ab781-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="ab781-184">Podrobnější dokumentaci k nasazení webu, najdete v článku [webové mapa obsahu nasazení pro Visual Studio a ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="ab781-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="ab781-185">Pokud nasadíte aplikaci do dvou serverů, můžete otevřít každou instanci v samostatném okně prohlížeče a zjistit, každá z jiných přijímat zprávy SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab781-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="ab781-186">(Samozřejmě v produkčním prostředí, oba servery by nacházejí za službou Vyrovnávání zatížení.)</span><span class="sxs-lookup"><span data-stu-id="ab781-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="ab781-187">Pokud jste zvědaví zobrazíte zprávy, které se odesílají do Redis, můžete použít **rozhraní příkazového řádku redis** klienta, který se instaluje s Redis.</span><span class="sxs-lookup"><span data-stu-id="ab781-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
