---
uid: signalr/overview/older-versions/scaleout-with-redis
title: "Škálování SignalR s Redisem (SignalR 1.x) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: c0d6fd421dad02298326d1975ae68d1e7cc78d8c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="709c6-102">Škálování SignalR s Redisem (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="709c6-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>
====================
<span data-ttu-id="709c6-103">podle [Karel Wasson](https://github.com/MikeWasson), [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="709c6-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="709c6-104">V tomto kurzu budete používat [Redis](http://redis.io/) distribuci zprávy přes SignalR aplikace, který je nasazen na dvě samostatné instance služby IIS.</span><span class="sxs-lookup"><span data-stu-id="709c6-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="709c6-105">Redis je úložišti klíč hodnota v paměti.</span><span class="sxs-lookup"><span data-stu-id="709c6-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="709c6-106">Mimoto podporuje i systém zasílání zpráv s modelem publikování a přihlášení k odběru.</span><span class="sxs-lookup"><span data-stu-id="709c6-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="709c6-107">SignalR Redis propojovacího rozhraní používá protokol pub nebo sub funkci k předávání zpráv na jiné servery.</span><span class="sxs-lookup"><span data-stu-id="709c6-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="709c6-108">V tomto kurzu budete používat tři servery:</span><span class="sxs-lookup"><span data-stu-id="709c6-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="709c6-109">Dva servery se systémem Windows, který použijete k nasazení aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="709c6-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="709c6-110">Jeden server se systémem Linux, který použijete ke spuštění Redis.</span><span class="sxs-lookup"><span data-stu-id="709c6-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="709c6-111">Snímky obrazovky v tomto kurzu byl použit Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="709c6-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="709c6-112">Pokud nemáte tří fyzických serverů používat, můžete vytvořit virtuální počítače v Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="709c6-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="709c6-113">Další možností je vytvořit virtuální počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="709c6-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="709c6-114">I když tento kurz používá oficiální implementace Redis, k dispozici je také [Windows port Redis](https://github.com/MSOpenTech/redis) z MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="709c6-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="709c6-115">Instalace a konfigurace se liší, ale jinak kroky jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="709c6-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="709c6-116">Škálování SignalR s Redis nepodporuje clustery Redis.</span><span class="sxs-lookup"><span data-stu-id="709c6-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="709c6-117">Přehled</span><span class="sxs-lookup"><span data-stu-id="709c6-117">Overview</span></span>

<span data-ttu-id="709c6-118">Předtím, než se nám získat podrobný kurz, zde je rychlý přehled toho, co jste se dělat.</span><span class="sxs-lookup"><span data-stu-id="709c6-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="709c6-119">Instalace Redis a spuštění serveru Redis.</span><span class="sxs-lookup"><span data-stu-id="709c6-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="709c6-120">Přidejte tyto balíčky NuGet do vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="709c6-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="709c6-121">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="709c6-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="709c6-122">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="709c6-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="709c6-123">Vytvoření aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="709c6-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="709c6-124">Přidejte následující kód do souboru Global.asax konfigurace propojovacího rozhraní:</span><span class="sxs-lookup"><span data-stu-id="709c6-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="709c6-125">Ubuntu na technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="709c6-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="709c6-126">Pomocí Windows Hyper-V, můžete snadno vytvořit virtuálního počítače s Ubuntu v systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="709c6-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="709c6-127">Stáhnout soubor ISO Ubuntu od [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="709c6-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="709c6-128">Technologie Hyper-V přidejte nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="709c6-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="709c6-129">V **připojit virtuální pevný Disk** krok, vyberte **vytvořit virtuální pevný disk**.</span><span class="sxs-lookup"><span data-stu-id="709c6-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="709c6-130">V **možnosti instalace** krok, vyberte **soubor bitové kopie (.iso)**, klikněte na tlačítko **Procházet**a přejděte do instalace Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="709c6-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="709c6-131">Nainstalujte Redis</span><span class="sxs-lookup"><span data-stu-id="709c6-131">Install Redis</span></span>

<span data-ttu-id="709c6-132">Postupujte podle kroků v [http://redis.io/download](http://redis.io/download) ke stažení a sestavení Redis.</span><span class="sxs-lookup"><span data-stu-id="709c6-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="709c6-133">Toto sestavení binárních souborů Redis `src` adresáře.</span><span class="sxs-lookup"><span data-stu-id="709c6-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="709c6-134">Ve výchozím nastavení Redis nevyžaduje heslo.</span><span class="sxs-lookup"><span data-stu-id="709c6-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="709c6-135">Chcete-li nastavit heslo, upravte `redis.conf` souboru, který se nachází v kořenovém adresáři zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="709c6-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="709c6-136">(Před zahájením úprav, vytvořili záložní kopii souboru!) Přidejte následující direktivu pro `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="709c6-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="709c6-137">Nyní spusťte serveru Redis:</span><span class="sxs-lookup"><span data-stu-id="709c6-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="709c6-138">Otevřete port 6379, což je výchozí port, který Redis naslouchá.</span><span class="sxs-lookup"><span data-stu-id="709c6-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="709c6-139">(Můžete změnit číslo portu v konfiguračním souboru.)</span><span class="sxs-lookup"><span data-stu-id="709c6-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="709c6-140">Vytvoření aplikace nástroje SignalR</span><span class="sxs-lookup"><span data-stu-id="709c6-140">Create the SignalR Application</span></span>

<span data-ttu-id="709c6-141">Vytvoření aplikace SignalR podle buď tyto kurzy:</span><span class="sxs-lookup"><span data-stu-id="709c6-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="709c6-142">Začínáme s SignalR</span><span class="sxs-lookup"><span data-stu-id="709c6-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="709c6-143">Začínáme s SignalR a MVC 4</span><span class="sxs-lookup"><span data-stu-id="709c6-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="709c6-144">V dalším kroku upravíme chatovací aplikace pro podporu škálování s Redis.</span><span class="sxs-lookup"><span data-stu-id="709c6-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="709c6-145">Nejprve přidejte balíček SignalR.Redis NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="709c6-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="709c6-146">V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="709c6-146">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="709c6-147">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="709c6-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="709c6-148">Dále otevřete soubor Global.asax.</span><span class="sxs-lookup"><span data-stu-id="709c6-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="709c6-149">Přidejte následující kód, který **aplikace\_spustit** metoda:</span><span class="sxs-lookup"><span data-stu-id="709c6-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="709c6-150">"server" je název serveru, který běží Redis.</span><span class="sxs-lookup"><span data-stu-id="709c6-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="709c6-151">*port* je číslo portu</span><span class="sxs-lookup"><span data-stu-id="709c6-151">*port* is the port number</span></span>
- <span data-ttu-id="709c6-152">"password" je heslo, které jste zadali v souboru redis.conf.</span><span class="sxs-lookup"><span data-stu-id="709c6-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="709c6-153">"AppName" je libovolný řetězec.</span><span class="sxs-lookup"><span data-stu-id="709c6-153">"AppName" is any string.</span></span> <span data-ttu-id="709c6-154">SignalR vytvoří kanál pub nebo sub Redis s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="709c6-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="709c6-155">Příklad:</span><span class="sxs-lookup"><span data-stu-id="709c6-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="709c6-156">Nasazení a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="709c6-156">Deploy and Run the Application</span></span>

<span data-ttu-id="709c6-157">Připravte vaší instance serveru Windows k nasazení aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="709c6-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="709c6-158">Přidejte roli služby IIS.</span><span class="sxs-lookup"><span data-stu-id="709c6-158">Add the IIS role.</span></span> <span data-ttu-id="709c6-159">Obsahují funkce "Vývoj aplikací", včetně protokol WebSocket.</span><span class="sxs-lookup"><span data-stu-id="709c6-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="709c6-160">Zahrnují taky službu správy (uvedené v části "Nástroje pro správu").</span><span class="sxs-lookup"><span data-stu-id="709c6-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="709c6-161">**Instalaci Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="709c6-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="709c6-162">Při spuštění Správce služby IIS, vás vyzve k instalaci webové platformy společnosti Microsoft nebo můžete [stáhnout intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="709c6-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="709c6-163">V instalačním programu platformy vyhledávání pro nasazení webu a nainstalovat nasazení webu 3.0</span><span class="sxs-lookup"><span data-stu-id="709c6-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="709c6-164">Zkontrolujte, zda je spuštěna služba webové správy.</span><span class="sxs-lookup"><span data-stu-id="709c6-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="709c6-165">V opačném případě spusťte službu.</span><span class="sxs-lookup"><span data-stu-id="709c6-165">If not, start the service.</span></span> <span data-ttu-id="709c6-166">(Pokud nevidíte v seznamu služeb systému Windows služby webové správy, ujistěte se, že jste nainstalovali službu správy při přidání IIS role.)</span><span class="sxs-lookup"><span data-stu-id="709c6-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="709c6-167">Ve výchozím nastavení služba webové správy naslouchá na portu TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="709c6-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="709c6-168">V bráně Windows Firewall vytvořte nové příchozí pravidlo umožňující přenos TCP na portu 8172.</span><span class="sxs-lookup"><span data-stu-id="709c6-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="709c6-169">Další informace najdete v tématu [konfigurace pravidel brány Firewall](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="709c6-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="709c6-170">(Pokud jsou hostiteli virtuálních počítačů v Azure, musíte přímo na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="709c6-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="709c6-171">V tématu [jak nastavit koncové body k virtuálnímu počítači](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="709c6-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="709c6-172">Nyní jste připraveni k nasazení projektu sady Visual Studio v počítači pro vývoj na server.</span><span class="sxs-lookup"><span data-stu-id="709c6-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="709c6-173">V Průzkumníku řešení klikněte pravým tlačítkem na řešení a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="709c6-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="709c6-174">Podrobnější dokumentaci k nasazení webu, najdete v článku [webové mapa obsahu nasazení pro Visual Studio a ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="709c6-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="709c6-175">Pokud nasadíte aplikaci do dvou serverů, můžete otevřít každou instanci v samostatném okně prohlížeče a zjistit, každá z jiných přijímat zprávy SignalR.</span><span class="sxs-lookup"><span data-stu-id="709c6-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="709c6-176">(Samozřejmě v produkčním prostředí, oba servery by nacházejí za službou Vyrovnávání zatížení.)</span><span class="sxs-lookup"><span data-stu-id="709c6-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="709c6-177">Pokud jste zvědaví zobrazíte zprávy, které se odesílají do Redis, můžete použít **rozhraní příkazového řádku redis** klienta, který se instaluje s Redis.</span><span class="sxs-lookup"><span data-stu-id="709c6-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
