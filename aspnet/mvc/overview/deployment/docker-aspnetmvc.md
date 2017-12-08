---
uid: mvc/overview/deployment/docker
title: "Migrace aplikací ASP.NET MVC do kontejneru systému Windows"
description: "Zjistěte, jak využít existující aplikaci ASP.NET MVC a spustíte ho v systému Windows kontejner Docker"
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.topic: article
ms.prod: .net-framework
ms.technology: dotnet-mvc
ms.devlang: dotnet
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 336d0e9d2247dd2d63c779fd446f9a50be6dbc50
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="a8f4a-104">Migrace aplikací ASP.NET MVC do kontejneru systému Windows</span><span class="sxs-lookup"><span data-stu-id="a8f4a-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="a8f4a-105">Spuštění existující aplikace založené na rozhraní .NET Framework v kontejneru Windows navíc nevyžaduje žádné změny do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="a8f4a-106">Ke spouštění vaší aplikace v kontejneru systému Windows vytvořit bitovou kopii Docker obsahující vaší aplikace a spusťte kontejner.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="a8f4a-107">Toto téma vysvětluje, jak využít existující [aplikace ASP.NET MVC](http://www.asp.net/mvc) a nasadit ji v kontejneru systému Windows.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="a8f4a-108">Začněte s existující aplikaci ASP.NET MVC a potom vytvořit publikované prostředky pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="a8f4a-109">Docker použijete k vytvoření bitové kopie, který obsahuje a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="a8f4a-110">Budete přejděte na web spuštěn v systému Windows kontejneru a ověřte, zda že aplikace pracuje.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="a8f4a-111">Tento článek předpokládá základní znalosti o Docker.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="a8f4a-112">Můžete další informace o Docker načtením [Docker přehled](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="a8f4a-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="a8f4a-113">Aplikace, které spustíte v kontejneru je jednoduchý web, který obsahuje odpovědi na dotazy náhodně.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="a8f4a-114">Tato aplikace je základní aplikace MVC bez ověřování a úložiště databáze; umožňuje zaměřit se na přesun se webová úroveň do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="a8f4a-115">Budoucí témata ukazují, jak přesunout a spravovat trvalé úložiště v kontejnerizované aplikacích.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="a8f4a-116">Přesunutí aplikace zahrnuje tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="a8f4a-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="a8f4a-117">Vytvoření úlohy Publikovat k vytváření prostředků pro bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="a8f4a-118">Vytváření obrazem Docker, které se spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="a8f4a-119">Spouštění kontejner Docker, který spouští bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="a8f4a-120">Ověření aplikace pomocí prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="a8f4a-121">[Dokončení aplikace](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) je na Githubu.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-121">The [finished application](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8f4a-122">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a8f4a-122">Prerequisites</span></span>

<span data-ttu-id="a8f4a-123">Vývojovém počítači musí být spuštěna.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-123">The development machine must be running</span></span>

- <span data-ttu-id="a8f4a-124">[Windows 10 Anniversary Update](https://www.microsoft.com/en-us/software-download/windows10/) (nebo vyšší) nebo [systému Windows Server 2016](https://www.microsoft.com/en-us/cloud-platform/windows-server) (nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="a8f4a-124">[Windows 10 Anniversary Update](https://www.microsoft.com/en-us/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/en-us/cloud-platform/windows-server) (or higher).</span></span>
- <span data-ttu-id="a8f4a-125">[Docker pro systém Windows](https://docs.docker.com/docker-for-windows/) -Beta 26 verze stabilní 1.13.0 nebo 1.12 (nebo novější verze)</span><span class="sxs-lookup"><span data-stu-id="a8f4a-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- <span data-ttu-id="a8f4a-126">[Visual Studio 2017](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8f4a-126">[Visual Studio 2017](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8f4a-127">Pokud používáte systém Windows Server 2016, postupujte podle pokynů pro [nasazení hostitele kontejneru - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span><span class="sxs-lookup"><span data-stu-id="a8f4a-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="a8f4a-128">Po instalaci a spuštění Docker, klikněte pravým tlačítkem na ikonu na hlavním panelu a vyberte **přepnout do kontejnerů Windows**.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-128">After installing and starting Docker,  right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="a8f4a-129">To se vyžaduje ke spuštění Docker Image založené na systému Windows.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="a8f4a-130">Tento příkaz má několik sekund provést:</span><span class="sxs-lookup"><span data-stu-id="a8f4a-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="a8f4a-131">![Kontejneru systému Windows][windows-container]</span><span class="sxs-lookup"><span data-stu-id="a8f4a-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="a8f4a-132">Publikování skriptu</span><span class="sxs-lookup"><span data-stu-id="a8f4a-132">Publish script</span></span>

<span data-ttu-id="a8f4a-133">Shromážděte všechny prostředky, které je potřeba načíst do bitové kopie Docker na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="a8f4a-134">Visual Studio můžete použít **publikovat** příkazu vytvoříte profil publikování pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="a8f4a-135">Tento profil se uveďte všechny prostředky v jedné stromu adresářů, který je zkopírovat do bitové kopie cíl později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="a8f4a-136">**Postup publikování**</span><span class="sxs-lookup"><span data-stu-id="a8f4a-136">**Publish Steps**</span></span>

1. <span data-ttu-id="a8f4a-137">Klikněte pravým tlačítkem na webového projektu v sadě Visual Studio a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="a8f4a-138">Klikněte **tlačítko vlastní profil**a potom vyberte **systém souborů** jako metodu.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="a8f4a-139">Vyberte adresář.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-139">Choose the directory.</span></span> <span data-ttu-id="a8f4a-140">Podle konvence stažené Ukázka používá `bin\Release\PublishOutput`.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="a8f4a-141">![Publikování připojení][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="a8f4a-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="a8f4a-142">Otevřete **možností publikování souboru** části **nastavení** kartě. Vyberte **Precompile během publikování**.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="a8f4a-143">Tato optimalizace znamená, že budete kompilování zobrazení v kontejner Docker, kopírujete předkompilovaných zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="a8f4a-144">![Nastavení publikování][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="a8f4a-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="a8f4a-145">Klikněte na tlačítko **publikovat**, a Visual Studio zkopíruje všechny potřebné prostředky do cílové složky.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="a8f4a-146">Vytvořit bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="a8f4a-146">Build the image</span></span>

<span data-ttu-id="a8f4a-147">Definujte Docker image v soubor Docker.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-147">Define your Docker image in a Dockerfile.</span></span> <span data-ttu-id="a8f4a-148">Soubor Docker obsahuje pokyny pro základní bitovou kopii, další součásti, aplikace, kterou chcete spustit a ostatní konfigurace Image.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-148">The Dockerfile contains instructions for the base image, additional components, the app you want to run, and other configuration images.</span></span>  <span data-ttu-id="a8f4a-149">Soubor Docker je vstup `docker build` příkaz, který vytvoří bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-149">The Dockerfile is the input to the `docker build` command, which creates the image.</span></span>

<span data-ttu-id="a8f4a-150">Vytvoříte bitovou kopii na základě `microsft/aspnet` image uložená na [úložiště Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span><span class="sxs-lookup"><span data-stu-id="a8f4a-150">You will build an image based on the `microsft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="a8f4a-151">Základní image `microsoft/aspnet`, je bitová kopie systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="a8f4a-152">Obsahuje jádro systému Windows Server, IIS a ASP.NET 4.6.2.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-152">It contains Windows Server Core, IIS and ASP.NET 4.6.2.</span></span> <span data-ttu-id="a8f4a-153">Když spustíte tuto bitovou kopii do vašeho kontejneru, automaticky spustí službu IIS a nainstalované weby.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="a8f4a-154">Soubor Docker, která vytvoří image vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a8f4a-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="a8f4a-155">Neexistuje žádné `ENTRYPOINT` v tento soubor Docker.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="a8f4a-156">Nepotřebujete jeden.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-156">You don't need one.</span></span> <span data-ttu-id="a8f4a-157">Při spuštění systému Windows Server se službou IIS, proces služby IIS je vstupní bod, který je nakonfigurován na spuštění v základní image aspnet.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="a8f4a-158">Spusťte příkaz sestavení Docker vytvoření bitové kopie, který spouští aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="a8f4a-159">Chcete-li to provést, v adresáři projektu otevřete okno prostředí PowerShell a zadejte následující příkaz v adresáři řešení:</span><span class="sxs-lookup"><span data-stu-id="a8f4a-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="a8f4a-160">Tento příkaz vytvoří novou bitovou kopii pomocí pokynů v váš soubor Docker pojmenování (-t označování) bitové kopie jako mvcrandomanswers.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="a8f4a-161">To může zahrnovat stahování základní bitovou kopii z [úložiště Docker Hub](http://hub.docker.com)a pak přidání aplikace do této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="a8f4a-162">Po dokončení tohoto příkazu můžete spustit `docker images` příkazu zobrazte informace na novou bitovou kopii:</span><span class="sxs-lookup"><span data-stu-id="a8f4a-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="a8f4a-163">ID OBRÁZKU se liší v počítači.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="a8f4a-164">Teď umožňuje spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="a8f4a-165">Spusťte kontejner</span><span class="sxs-lookup"><span data-stu-id="a8f4a-165">Start a container</span></span>

<span data-ttu-id="a8f4a-166">Spusťte kontejner spuštěním následujícího `docker run` příkaz:</span><span class="sxs-lookup"><span data-stu-id="a8f4a-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="a8f4a-167">`-d` Argument určuje Docker zahájíte bitovou kopii v odpojeném režimu.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="a8f4a-168">To znamená, že bitovou kopii Docker spouští odpojené z aktuální prostředí.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="a8f4a-169">V mnoha docker příklady může se zobrazit -p mapování portů kontejneru a hostitele.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="a8f4a-170">Výchozí image aspnet již nakonfigurována kontejneru zveřejnění a naslouchat na portu 80.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span> 

<span data-ttu-id="a8f4a-171">`--name randomanswers` Poskytuje název ke kontejneru spuštěné.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="a8f4a-172">Tento název může používat místo ID kontejneru v většina příkazů.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="a8f4a-173">`mvcrandomanswers` Je název bitovou kopii k spouštění.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="a8f4a-174">Ověřte v prohlížeči</span><span class="sxs-lookup"><span data-stu-id="a8f4a-174">Verify in the browser</span></span>

> [!NOTE]
> <span data-ttu-id="a8f4a-175">V aktuální verzi Windows kontejner nelze vyhledat `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-175">With the current Windows Container release, you can't browse to `http://localhost`.</span></span>
> <span data-ttu-id="a8f4a-176">Toto je známé chování v WinNAT a bude v budoucnu řešit.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-176">This is a known behavior in WinNAT, and it will be resolved in the future.</span></span> <span data-ttu-id="a8f4a-177">Dokud, která je určena, budete muset použít IP adresu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-177">Until that is addressed, you need to use the IP address of the container.</span></span>

<span data-ttu-id="a8f4a-178">Jakmile se spustí kontejneru, najděte jeho IP adresu, můžete připojit k vaší spuštěné kontejneru z prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="a8f4a-178">Once the container starts, find its IP address so that you can connect to your running container from a browser:</span></span>

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

<span data-ttu-id="a8f4a-179">Připojení ke kontejneru spuštěná pomocí adresu IPv4 `http://172.31.194.61` ukazuje příklad.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-179">Connect to the running container using the IPv4 address, `http://172.31.194.61` in the example shown.</span></span> <span data-ttu-id="a8f4a-180">Zadejte tuto adresu URL do prohlížeče a měli byste vidět web spuštěný.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-180">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="a8f4a-181">Navigace do vaší lokality mohou zabránit určitý software VPN nebo proxy server.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-181">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="a8f4a-182">Můžete ji a ujistěte se, že vaše kontejneru funguje dočasně zakázat.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-182">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="a8f4a-183">Obsahuje ukázkové adresáře na Githubu [skript prostředí PowerShell](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) který pro vás provede tyto příkazy.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-183">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="a8f4a-184">Otevřete okno prostředí PowerShell, změňte adresář na adresář řešení a typ:</span><span class="sxs-lookup"><span data-stu-id="a8f4a-184">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="a8f4a-185">Výše uvedeného příkazu vytvoří bitovou kopii, zobrazí se seznam obrázků na váš počítač, spustí kontejner a zobrazí IP adresu pro tento kontejner.</span><span class="sxs-lookup"><span data-stu-id="a8f4a-185">The command above builds the image, displays the list of images on your machine, starts a container, and displays the IP address for that container.</span></span>

<span data-ttu-id="a8f4a-186">Pokud chcete zastavit vašeho kontejneru, vydávání `docker
stop` příkaz:</span><span class="sxs-lookup"><span data-stu-id="a8f4a-186">To stop your container, issue a `docker
stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="a8f4a-187">Pokud chcete odebrat kontejner, vydávání `docker rm` příkaz:</span><span class="sxs-lookup"><span data-stu-id="a8f4a-187">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Přepněte do kontejneru systému Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Publikovat do systému souborů"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Nastavení publikování"
