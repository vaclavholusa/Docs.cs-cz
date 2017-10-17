---
title: "Visual Studio Tools pro Docker základní technologie ASP.NET"
description: "Tento článek vás provede pomocí nástrojů Visual Studio 2017 a Docker pro systém Windows, které containerize aplikace ASP.NET Core."
keywords: Kontejner Docker,ASP.NET Core, Visual Studio
author: spboyer
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.prod: asp.net-core
ms.technology: aspnet
ms.assetid: 1f3b9a68-4dea-4b60-8cb3-f46164eedbbf
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: bc436e2c02b05475b84cf9f8bdedf9463a673c4a
ms.sourcegitcommit: b861bab71ea6945f673c62223ae2cba3aa74cb6b
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2017
---
# <a name="visual-studio-tools-for-docker"></a><span data-ttu-id="ee6a7-104">Visual Studio Tools pro Docker</span><span class="sxs-lookup"><span data-stu-id="ee6a7-104">Visual Studio Tools for Docker</span></span>

<span data-ttu-id="ee6a7-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) s [Docker pro systém Windows](https://docs.docker.com/docker-for-windows/install/) podporuje vytváření, ladění a spouštění rozhraní .NET Framework a .NET Core web a konzoly aplikací pomocí kontejnery Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with [Docker for Windows](https://docs.docker.com/docker-for-windows/install/) supports building, debugging, and running .NET Framework and .NET Core web and console applications using Windows and Linux containers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee6a7-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ee6a7-106">Prerequisites</span></span>

- <span data-ttu-id="ee6a7-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) zatížení .NET Core</span><span class="sxs-lookup"><span data-stu-id="ee6a7-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with .NET Core workload</span></span>
- [<span data-ttu-id="ee6a7-108">Docker pro Windows</span><span class="sxs-lookup"><span data-stu-id="ee6a7-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="ee6a7-109">Instalace a nastavení</span><span class="sxs-lookup"><span data-stu-id="ee6a7-109">Installation and setup</span></span>

<span data-ttu-id="ee6a7-110">Nainstalujte [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) se zatížením, .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-110">Install [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) with the .NET Core workload.</span></span>

<span data-ttu-id="ee6a7-111">Pro instalaci Docker, přečtěte si informace v [Docker pro Windows: co potřebujete vědět před instalací](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) a nainstalujte [Docker pro systém Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="ee6a7-111">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="ee6a7-112">Požadovaná konfigurace je na instalaci  **[sdílené disky](https://docs.docker.com/docker-for-windows/#shared-drives)**  v Docker pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-112">A required configuration is to setup **[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows.</span></span> <span data-ttu-id="ee6a7-113">Nastavení je povinné pro svazek mapování a ladění podpory.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-113">The setting is required for the volume mapping and debugging support.</span></span>

<span data-ttu-id="ee6a7-114">Klikněte pravým tlačítkem na ikonu Docker na hlavním panelu systému, klikněte na tlačítko **nastavení**a vyberte **sdílené disky**.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-114">Right-click the Docker icon in the System Tray, click **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="ee6a7-115">Vyberte jednotku, kde bude Docker ukládat soubory a změny.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-115">Select the drive where Docker will store your files and apply changes.</span></span>

![Sdílené disky](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a><span data-ttu-id="ee6a7-117">Vytvoření webové aplikace ASP.NET a přidání podpory Docker</span><span class="sxs-lookup"><span data-stu-id="ee6a7-117">Create an ASP.NET Web Application and add Docker Support</span></span>

<span data-ttu-id="ee6a7-118">Pomocí sady Visual Studio vytvořte novou webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-118">Using Visual Studio, create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="ee6a7-119">Pokud aplikace je načtena, vyberte buď **přidat podporu Docker** z **nabídky projektu** nebo klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **přidat**  >  **Docker podporu**.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-119">When the application is loaded, either select **Add Docker Support** from the **Project Menu** or right-click the project from the Solution Explorer and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="ee6a7-120">*Nabídky projektu*</span><span class="sxs-lookup"><span data-stu-id="ee6a7-120">*Project Menu*</span></span>

![Projekt přidat podporu Docker](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

<span data-ttu-id="ee6a7-122">*Místní nabídky projektu*</span><span class="sxs-lookup"><span data-stu-id="ee6a7-122">*Project Context Menu*</span></span>

![Pravým tlačítkem klikněte na Přidat podporu Docker](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

<span data-ttu-id="ee6a7-124">Když přidáte podporu Docker do projektu, můžete Windows nebo Linux kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-124">When you add Docker support to your project, you can choose either Windows or Linux containers.</span></span> <span data-ttu-id="ee6a7-125">(Hostitelů Docker musí používat stejný typ kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-125">(The Docker host must be running the same container type.</span></span> <span data-ttu-id="ee6a7-126">Pokud je třeba změnit typ kontejneru v běžící instance Docker, klikněte pravým tlačítkem **Docker** ikonu na hlavním panelu a vyberte **přepnout do kontejnerů Windows** nebo **přepnout do systému Linux kontejnery**.)</span><span class="sxs-lookup"><span data-stu-id="ee6a7-126">If you need change the container type in the running Docker instance, right-click the **Docker** icon in the System Tray, and choose **Switch to Windows containers** or **Switch to Linux containers**.)</span></span> 

<span data-ttu-id="ee6a7-127">Následující soubory budou přidány do projektu:</span><span class="sxs-lookup"><span data-stu-id="ee6a7-127">The following files are added to the project:</span></span>

- <span data-ttu-id="ee6a7-128">**Soubor Docker**: soubor Docker pro aplikace ASP.NET Core vychází [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-128">**Dockerfile**: the Docker file for ASP.NET Core applications is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="ee6a7-129">Tento image obsahuje balíčků ASP.NET Core NuGet, které byly před jitted vylepšení výkonu při spuštění.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-129">This image includes the ASP.NET Core NuGet packages, which have been pre-jitted improving startup performance.</span></span> <span data-ttu-id="ee6a7-130">Při vytváření aplikace konzoly .NET Core, soubor Docker ze bude odkazovat poslední [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-130">When building .NET Core Console Applications, the Dockerfile FROM will reference the most recent [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) image.</span></span>   
- <span data-ttu-id="ee6a7-131">**docker-compose.yml**: základního souboru Docker Compose používá k definování kolekce obrázků být vytvořené a spustit s docker-vytvořit sestavení a spuštění.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-131">**docker-compose.yml**: base Docker Compose file used to define the collection of images to be built and run with docker-compose build/run.</span></span>   
- <span data-ttu-id="ee6a7-132">**docker compose.dev.debug.yml**: Další docker compose soubor s iterativní změny konfiguraci nastavena na ladění.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-132">**docker-compose.dev.debug.yml**: additional docker-compose file with for iterative changes when your configuration is set to debug.</span></span> <span data-ttu-id="ee6a7-133">Visual Studio bude volat -f docker-compose.yml - f docker-compose.dev.debug.yml na sloučení těchto společně.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-133">Visual Studio will call -f docker-compose.yml -f docker-compose.dev.debug.yml to merge these together.</span></span> <span data-ttu-id="ee6a7-134">Tento soubor compose je používán vývojářské nástroje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-134">This compose file is used by Visual Studio development tools.</span></span>   
- <span data-ttu-id="ee6a7-135">**docker compose.dev.release.yml**: další soubor Docker Compose k ladění vaší verze definice.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-135">**docker-compose.dev.release.yml**: additional Docker Compose file to debug your release definition.</span></span> <span data-ttu-id="ee6a7-136">Zruší připojení svazku ladicího programu, se nezmění obsah produkční bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-136">It will volume mount the debugger so it doesn't change the contents of the production image.</span></span>  

<span data-ttu-id="ee6a7-137">*Docker-compose.yml* soubor obsahuje název obrázku, který se vytvoří při spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-137">The *docker-compose.yml* file contains the name of the image that is created when project is run.</span></span> 

```
version '2'

services:
  hellodockertools:
    image:  user/hellodockertools${TAG}
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80"
``` 

<span data-ttu-id="ee6a7-138">V tomto příkladu `image: user/hellodockertools${TAG}` generuje bitovou kopii `user/hellodockertools:dev` při spuštění aplikace **ladění** režimu a `user/hellodockertools:latest` v **verze** režimu v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-138">In this example, `image: user/hellodockertools${TAG}` generates the image `user/hellodockertools:dev` when the application is run in **Debug** mode and `user/hellodockertools:latest` in **Release** mode respectively.</span></span> 

<span data-ttu-id="ee6a7-139">Můžete změnit `user` k vaší [úložiště Docker Hub](https://hub.docker.com/) uživatelské jméno, pokud máte v úmyslu push bitovou kopii do registru.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-139">You will want to change the `user` to your [Docker Hub](https://hub.docker.com/) username if you plan to push the image to the registry.</span></span> <span data-ttu-id="ee6a7-140">Například `spboyer/hellodockertools`, nebo změňte adresu URL vašeho privátní registru `privateregistry.domain.com/` v závislosti na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-140">For example, `spboyer/hellodockertools`, or change to your private registry URL `privateregistry.domain.com/` depending on your configuration.</span></span>

### <a name="debugging"></a><span data-ttu-id="ee6a7-141">Ladění</span><span class="sxs-lookup"><span data-stu-id="ee6a7-141">Debugging</span></span>

<span data-ttu-id="ee6a7-142">Vyberte **Docker** z rozevíracího seznamu v panelu nástrojů a použití F5 spusťte ladění aplikace ladění.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-142">Select **Docker** from the debug drop-down in the toolbar and use F5 to start debugging the application.</span></span> 

- <span data-ttu-id="ee6a7-143">*Microsoft/aspnetcore* bitové kopie je získali (pokud ještě není v mezipaměti)</span><span class="sxs-lookup"><span data-stu-id="ee6a7-143">The *microsoft/aspnetcore* image is acquired (if not already in your cache)</span></span>
- <span data-ttu-id="ee6a7-144">*ASPNETCORE_ENVIRONMENT* se nastaví na vývoji v kontejneru</span><span class="sxs-lookup"><span data-stu-id="ee6a7-144">*ASPNETCORE_ENVIRONMENT* is set to Development within the container</span></span>
- <span data-ttu-id="ee6a7-145">PORT 80 je VYSTAVEN a mapovat na dynamicky přiřazené port pro místního hostitele.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-145">PORT 80 is EXPOSED and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="ee6a7-146">Port je dáno hostitelů docker a může být dotazován s docker ps.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-146">The port is determined by the docker host and can be queried with docker ps.</span></span> 
- <span data-ttu-id="ee6a7-147">Aplikace se zkopíruje do kontejneru</span><span class="sxs-lookup"><span data-stu-id="ee6a7-147">Your application is copied to the container</span></span>
- <span data-ttu-id="ee6a7-148">Spuštění výchozího prohlížeče s ladicím programem připojené ke kontejneru, pomocí dynamicky přiřazeného portu.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-148">Default browser is launched with the debugger attached to the container, using the dynamically assigned port.</span></span> 

<span data-ttu-id="ee6a7-149">Je výsledný obraz Docker postavené *dev* bitové kopie aplikace s *microsoft/aspnetcore* bitové kopie jako základní bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-149">The resulting Docker image built is the *dev* image of your application with the *microsoft/aspnetcore* images as the base image.</span></span>

<span data-ttu-id="ee6a7-150">**Poznámka:** bitovou kopii vývojářů je prázdný obsah vaší aplikace, jako ladění konfigurace používají připojení svazku a poskytuje iterativní prostředí.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-150">**Note:** The dev image is empty of your app contents as Debug configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="ee6a7-151">Chcete-li push bitovou kopii, použijte konfigurace verze.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-151">To push an image, use the Release configuration.</span></span>

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

<span data-ttu-id="ee6a7-152">Je aplikace spuštěna pomocí kontejneru, který se zobrazí spuštěním `docker ps` příkaz.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-152">The application is running using the container, which you can see by running the `docker ps` command.</span></span>

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a><span data-ttu-id="ee6a7-153">Upravit a pokračovat</span><span class="sxs-lookup"><span data-stu-id="ee6a7-153">Edit and Continue</span></span>

<span data-ttu-id="ee6a7-154">Změní na statické soubory nebo soubory šablon razor (*.cshtml*) se automaticky aktualizují, aniž by bylo potřeba kompilační krok.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-154">Changes to static files and/or razor template files (*.cshtml*) are automatically updated without the need of a compilation step.</span></span> <span data-ttu-id="ee6a7-155">Proveďte požadovanou změnu, uložit a klepněte na aktualizace v prohlížeči zobrazit aktualizace.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-155">Make the change, save, and tap refresh in the browser to view the update.</span></span>  

<span data-ttu-id="ee6a7-156">Úpravy, aby se soubory kódu vyžadují kompilování a restartování Kestrel v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-156">Modifications to code files require compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="ee6a7-157">Po provedení změny, pomocí kombinace kláves CTRL + F5 provést proces a spusťte aplikaci v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-157">After making the change, use CTRL + F5 to perform the process and start the application within the container.</span></span> <span data-ttu-id="ee6a7-158">Kontejner Docker není znovu sestavit nebo zastavená; pomocí `docker ps` v příkazovém řádku, uvidíte, že původní kontejner je stále spuštěná od 10 minutami.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-158">The Docker container is not rebuilt or stopped; using `docker ps` in the command line, you can see that the original container is still running as of 10 minutes ago.</span></span> 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a><span data-ttu-id="ee6a7-159">Publikování imagí Dockeru</span><span class="sxs-lookup"><span data-stu-id="ee6a7-159">Publishing Docker images</span></span>

<span data-ttu-id="ee6a7-160">Po dokončení cyklu vývoj a ladění aplikace sady Visual Studio Tools for Docker vám pomůže vytvořit bitovou kopii produkčního vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-160">Once you have completed the develop and debug cycle of your application, the Visual Studio Tools for Docker will help you create the production image of your application.</span></span> <span data-ttu-id="ee6a7-161">Změnit rozevírací nabídce ladění na **verze** a sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-161">Change the debug dropdown to **Release** and build the application.</span></span> <span data-ttu-id="ee6a7-162">Nástroji vytvoří bitovou kopii s `:latest` značky, které můžete posouvat privátní registru nebo úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-162">The tooling will produce the image with the `:latest` tag which you can push to your private registry or Docker Hub.</span></span> 

<span data-ttu-id="ee6a7-163">Pomocí `docker images` příkaz, zobrazí se v seznamu bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-163">Using the `docker images` command, you can see the list of images.</span></span>

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

<span data-ttu-id="ee6a7-164">Může být očekávání pro produkční nebo verzi obrázek, který má být menší velikost oproti **dev** obraz; však prostřednictvím mapování svazku, aplikace a ladicí program byly ve skutečnosti spuštěná z vaší místní počítače a ne do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-164">There may be an expectation for the production or release image to be smaller in size by comparison to the **dev** image; however, through the use of the volume mapping, the debugger and application were actually being run from your local machine and not within the container.</span></span> <span data-ttu-id="ee6a7-165">**Nejnovější** image má zabalí kód celá aplikace potřebné ke spuštění aplikace na hostitelském počítači, proto rozdíl je velikost kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="ee6a7-165">The **latest** image has packaged the entire application code needed to run the application on a host machine, therefore the delta is the size of your application code.</span></span>
