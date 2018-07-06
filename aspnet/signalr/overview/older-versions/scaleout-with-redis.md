---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Škálování aplikace SignalR službou Redis (SignalR 1.x) | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 1aa391aadfada85f90fe37a70a983b2efa3fbb62
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833109"
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="c8c0c-102">Škálování aplikace SignalR službou Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="c8c0c-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>
====================
<span data-ttu-id="c8c0c-103">podle [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c8c0c-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="c8c0c-104">V tomto kurzu budete používat [Redis](http://redis.io/) k distribuci zpráv do aplikace SignalR, který je nasazen na dvou samostatných instancí služby IIS.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="c8c0c-105">Redis je úložiště klíč / hodnota v paměti.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="c8c0c-106">Podporuje také systému zasílání zpráv s modelem publikování a přihlášení k odběru.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="c8c0c-107">Propojovací rozhraní systému SignalR Redis používá funkci pub/sub pro předávání zpráv na jiné servery.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="c8c0c-108">Pro účely tohoto kurzu budete používat tři servery:</span><span class="sxs-lookup"><span data-stu-id="c8c0c-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="c8c0c-109">Dva servery se systémem Windows, který použijete k nasazení aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="c8c0c-110">Jeden server se systémem Linux, který použijete ke spuštění Redis.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="c8c0c-111">Snímky obrazovky v tomto kurzu můžu použít Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="c8c0c-112">Pokud nemáte tří fyzických serverů pro použití, můžete vytvořit virtuální počítače v Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="c8c0c-113">Další možností je vytvořit virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="c8c0c-114">I když v tomto kurzu používá oficiální implementace Redis, k dispozici je také [Windows port Redis](https://github.com/MSOpenTech/redis) z MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="c8c0c-115">Instalace a konfigurace se liší, ale jinak kroky jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c8c0c-116">Škálování aplikace SignalR službou Redis nepodporuje clustery Redis.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="c8c0c-117">Přehled</span><span class="sxs-lookup"><span data-stu-id="c8c0c-117">Overview</span></span>

<span data-ttu-id="c8c0c-118">Předtím, než získáme podrobný kurz, zde je rychlý přehled toho, co budete dělat.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="c8c0c-119">Nainstalujte Redis a spusťte Redis server.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="c8c0c-120">Přidejte tyto balíčky NuGet pro vaši aplikaci:</span><span class="sxs-lookup"><span data-stu-id="c8c0c-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="c8c0c-121">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="c8c0c-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="c8c0c-122">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="c8c0c-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="c8c0c-123">Vytvoření aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="c8c0c-124">Přidejte následující kód do souboru Global.asax konfigurace propojovacího rozhraní:</span><span class="sxs-lookup"><span data-stu-id="c8c0c-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="c8c0c-125">Ubuntu na Hyper-V</span><span class="sxs-lookup"><span data-stu-id="c8c0c-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="c8c0c-126">Pomocí Windows Hyper-V, můžete snadno vytvořit virtuální počítač s Ubuntu ve Windows serveru.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="c8c0c-127">Stáhněte si bitovou kopii ISO se systémem Ubuntu z [ http://www.ubuntu.com ](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="c8c0c-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="c8c0c-128">Technologie Hyper-V přidejte nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="c8c0c-129">V **připojit virtuální pevný Disk** kroku, vyberte **virtuální pevný disk vytvoříte**.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="c8c0c-130">V **možnosti instalace** kroku, vyberte **soubor bitové kopie (.iso)**, klikněte na tlačítko **Procházet**a přejděte do instalační Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="c8c0c-131">Nainstalujte Redis</span><span class="sxs-lookup"><span data-stu-id="c8c0c-131">Install Redis</span></span>

<span data-ttu-id="c8c0c-132">Postupujte podle kroků uvedených v [ http://redis.io/download ](http://redis.io/download) ke stažení a sestavení Redis.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="c8c0c-133">Tento postup sestaví binární soubory Redis `src` adresáře.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="c8c0c-134">Ve výchozím nastavení Redis, aby heslo.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="c8c0c-135">Chcete-li nastavit heslo, upravte `redis.conf` soubor, který je umístěn v kořenovém adresáři zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="c8c0c-136">(Před zahájením úprav, vytvořili záložní kopii souboru!) Přidejte následující direktivy k `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="c8c0c-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="c8c0c-137">Nyní spusťte Redis server:</span><span class="sxs-lookup"><span data-stu-id="c8c0c-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="c8c0c-138">Otevřený port 6379, což je výchozí port, který Redis naslouchá.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="c8c0c-139">(Můžete změnit číslo portu v konfiguračním souboru.)</span><span class="sxs-lookup"><span data-stu-id="c8c0c-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="c8c0c-140">Vytvoření aplikace SignalR</span><span class="sxs-lookup"><span data-stu-id="c8c0c-140">Create the SignalR Application</span></span>

<span data-ttu-id="c8c0c-141">Vytvoření aplikace SignalR pomocí některé z těchto kurzů:</span><span class="sxs-lookup"><span data-stu-id="c8c0c-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="c8c0c-142">Začínáme s knihovnou SignalR</span><span class="sxs-lookup"><span data-stu-id="c8c0c-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="c8c0c-143">Začínáme s knihovnou SignalR a MVC 4</span><span class="sxs-lookup"><span data-stu-id="c8c0c-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="c8c0c-144">V dalším kroku upravíme, aby byl chatovací aplikaci, aby podporovala škálování s využitím Redis.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="c8c0c-145">Nejprve přidejte balíček SignalR.Redis NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="c8c0c-146">V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-146">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c8c0c-147">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c8c0c-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="c8c0c-148">Dále otevřete soubor Global.asax.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="c8c0c-149">Přidejte následující kód, který **aplikace\_Start** metody:</span><span class="sxs-lookup"><span data-stu-id="c8c0c-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="c8c0c-150">"server" je název serveru, na kterém běží Redis.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="c8c0c-151">*port* je číslo portu</span><span class="sxs-lookup"><span data-stu-id="c8c0c-151">*port* is the port number</span></span>
- <span data-ttu-id="c8c0c-152">"password" je heslo, které jste definovali v souboru redis.conf.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="c8c0c-153">"AppName" je jakýkoli řetězec.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-153">"AppName" is any string.</span></span> <span data-ttu-id="c8c0c-154">Funkce SignalR vytvoří kanál pub/sub Redis s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="c8c0c-155">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c8c0c-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="c8c0c-156">Nasazení a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="c8c0c-156">Deploy and Run the Application</span></span>

<span data-ttu-id="c8c0c-157">Příprava vašich instancí Windows serveru k nasazení aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="c8c0c-158">Přidejte roli služby IIS.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-158">Add the IIS role.</span></span> <span data-ttu-id="c8c0c-159">Zahrnují funkce "Vývoj aplikací", včetně protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="c8c0c-160">Zahrnují taky službu správy (uvedené v části "Nástroje pro správu").</span><span class="sxs-lookup"><span data-stu-id="c8c0c-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="c8c0c-161">**Nainstalovat Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="c8c0c-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="c8c0c-162">Když spustíte Správce služby IIS, vás vyzve k instalaci webové platformy společnosti Microsoft, nebo se dají [stáhnout intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="c8c0c-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="c8c0c-163">Do platformy hledání pro nasazení webu a nainstalovat nasazení webu 3.0</span><span class="sxs-lookup"><span data-stu-id="c8c0c-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="c8c0c-164">Zkontrolujte, že je spuštěná služby webové správy.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="c8c0c-165">Pokud ne, spusťte službu.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-165">If not, start the service.</span></span> <span data-ttu-id="c8c0c-166">(Pokud nevidíte v seznamu služeb Windows služby webové správy, ujistěte se, že jste nainstalovali službu pro správu při přidání IIS role.)</span><span class="sxs-lookup"><span data-stu-id="c8c0c-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="c8c0c-167">Ve výchozím nastavení služby webové správy naslouchá na TCP port 8172.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="c8c0c-168">V bráně Windows Firewall vytvořte nové pravidlo, které povoluje provoz TCP na port 8172.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="c8c0c-169">Další informace najdete v tématu [konfigurace pravidel brány Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="c8c0c-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="c8c0c-170">(Pokud jsou hostiteli virtuálních počítačů v Azure, můžete provést přímo na webu Azure portal.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="c8c0c-171">Zobrazit [jak nastavit koncové body k virtuálnímu počítači](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="c8c0c-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="c8c0c-172">Nyní jste připraveni k nasazení projektu sady Visual Studio z vývojového počítače k serveru.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="c8c0c-173">V Průzkumníku řešení klikněte pravým tlačítkem myši na řešení a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="c8c0c-174">Podrobnější dokumentaci k nasazení webu, najdete v článku [mapa obsahu webové nasazení pro Visual Studio a ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="c8c0c-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="c8c0c-175">Pokud nasadíte aplikaci do dvou serverů, můžete otevřít každou instanci v samostatném okně prohlížeče a zobrazit, že každá z jiných příjem zpráv SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="c8c0c-176">(Samozřejmě v produkčním prostředí, dva servery strávil za nástrojem pro vyrovnávání zatížení.)</span><span class="sxs-lookup"><span data-stu-id="c8c0c-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="c8c0c-177">Pokud vás to zajímá, zobrazíte zprávy, které se odesílají do Redis, můžete použít **rozhraní příkazového řádku redis** klienta, který se nainstaluje s využitím Redis.</span><span class="sxs-lookup"><span data-stu-id="c8c0c-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
