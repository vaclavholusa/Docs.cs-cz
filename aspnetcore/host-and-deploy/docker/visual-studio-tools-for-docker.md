---
title: Visual Studio Tools for Docker s ASP.NET Core
author: spboyer
description: Zjistěte, jak kontejnerizovat aplikace ASP.NET Core pomocí nástroje Visual Studio 2017 a Docker pro Windows.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/18/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: afa7b05820ba021c50d9c23804095f7edd8b71f1
ms.sourcegitcommit: ee2b26c7d08b38c908c668522554b52ab8efa221
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146881"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="7664c-103">Visual Studio Tools for Docker s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7664c-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="7664c-104">[Visual Studio 2017](https://www.visualstudio.com/) podporuje vytváření, ladění a spouštění kontejnerizovaných ASP.NET Core aplikace cílí na .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7664c-104">[Visual Studio 2017](https://www.visualstudio.com/) supports building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="7664c-105">Jsou podporovány kontejnery Windows i Linuxu.</span><span class="sxs-lookup"><span data-stu-id="7664c-105">Both Windows and Linux containers are supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7664c-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7664c-106">Prerequisites</span></span>

* <span data-ttu-id="7664c-107">[Visual Studio 2017](https://www.visualstudio.com/) s **vývoj pro různé platformy .NET Core** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="7664c-107">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>
* [<span data-ttu-id="7664c-108">Docker pro Windows</span><span class="sxs-lookup"><span data-stu-id="7664c-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="7664c-109">Instalace a nastavení</span><span class="sxs-lookup"><span data-stu-id="7664c-109">Installation and setup</span></span>

<span data-ttu-id="7664c-110">Instalace Dockeru, přečtěte si informace na [Docker pro Windows: co potřebujete vědět před instalací](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) a nainstalujte [Docker pro Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="7664c-110">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="7664c-111">**[Sdílené jednotky](https://docs.docker.com/docker-for-windows/#shared-drives)**  v Docker pro Windows musí být nakonfigurované pro podporu mapování svazku a ladění.</span><span class="sxs-lookup"><span data-stu-id="7664c-111">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="7664c-112">Klikněte pravým tlačítkem na ikonu Dockeru na hlavním panelu, vyberte **nastavení...** a vyberte **sdílené jednotky**.</span><span class="sxs-lookup"><span data-stu-id="7664c-112">Right-click the System Tray's Docker icon, select **Settings...**, and select **Shared Drives**.</span></span> <span data-ttu-id="7664c-113">Vyberte jednotku, kde Docker ukládá soubory.</span><span class="sxs-lookup"><span data-stu-id="7664c-113">Select the drive where Docker stores files.</span></span> <span data-ttu-id="7664c-114">Vyberte **použít**.</span><span class="sxs-lookup"><span data-stu-id="7664c-114">Select **Apply**.</span></span>

![Sdílené jednotky](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="7664c-116">Visual Studio 2017 verze 15.6 a vyšší výzvu, při **sdílené jednotky** nejsou nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="7664c-116">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-docker-support-to-an-app"></a><span data-ttu-id="7664c-117">Do aplikace přidat podporu Dockeru</span><span class="sxs-lookup"><span data-stu-id="7664c-117">Add Docker support to an app</span></span>

<span data-ttu-id="7664c-118">Do projektu aplikace ASP.NET Core přidat podporu Dockeru, musí projekt cílit na .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7664c-118">To add Docker support to an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="7664c-119">Jsou podporovány kontejnery Linux i Windows.</span><span class="sxs-lookup"><span data-stu-id="7664c-119">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="7664c-120">Při přidávání podpory Docker do projektu, zvolte Windows nebo Linuxem kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7664c-120">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="7664c-121">Hostitele Docker musí běžet stejný typ kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7664c-121">The Docker host must be running the same container type.</span></span> <span data-ttu-id="7664c-122">Chcete-li změnit typ kontejneru v běžící instanci Dockeru, klikněte pravým tlačítkem na ikonu Dockeru na hlavním panelu a vyberte **přepnout na kontejnery Windows...**  nebo **přepnout na kontejnery Linuxu...** .</span><span class="sxs-lookup"><span data-stu-id="7664c-122">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="7664c-123">Nová aplikace</span><span class="sxs-lookup"><span data-stu-id="7664c-123">New app</span></span>

<span data-ttu-id="7664c-124">Při vytváření nové aplikace s **webové aplikace ASP.NET Core** šablony projektu, vyberte **povolit podporu Dockeru** zaškrtávací políčko:</span><span class="sxs-lookup"><span data-stu-id="7664c-124">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** check box:</span></span>

![Povolit podporu Dockeru zaškrtávací políčko](visual-studio-tools-for-docker/_static/enable-docker-support-check box.png)

<span data-ttu-id="7664c-126">Pokud je verze cílového rozhraní .NET Core **OS** umožňuje rozevírací seznam pro výběr typu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7664c-126">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="7664c-127">Existující aplikace</span><span class="sxs-lookup"><span data-stu-id="7664c-127">Existing app</span></span>

<span data-ttu-id="7664c-128">Visual Studio Tools for Docker nepodporuje přidávání Dockeru do existujícího projektu ASP.NET Core cílí na rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7664c-128">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span> <span data-ttu-id="7664c-129">Pro projekty ASP.NET Core, jejichž cílem je .NET Core existují dvě možnosti pro přidání podpory Dockeru pomocí nástrojů.</span><span class="sxs-lookup"><span data-stu-id="7664c-129">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="7664c-130">Otevřete projekt v sadě Visual Studio a zvolte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="7664c-130">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="7664c-131">Vyberte **podporu Dockeru** z **projektu** nabídky.</span><span class="sxs-lookup"><span data-stu-id="7664c-131">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="7664c-132">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **přidat** > **podporu Dockeru**.</span><span class="sxs-lookup"><span data-stu-id="7664c-132">Right-click the project in Solution Explorer and select **Add** > **Docker Support**.</span></span>

## <a name="docker-assets-overview"></a><span data-ttu-id="7664c-133">Přehled prostředků dockeru</span><span class="sxs-lookup"><span data-stu-id="7664c-133">Docker assets overview</span></span>

<span data-ttu-id="7664c-134">Přidejte Visual Studio Tools for Docker *docker-compose* projektu do řešení, který obsahuje následující soubory:</span><span class="sxs-lookup"><span data-stu-id="7664c-134">The Visual Studio Tools for Docker add a *docker-compose* project to the solution containing the following files:</span></span>

* <span data-ttu-id="7664c-135">*.dockerignore*: obsahuje seznam vzorů souborů a adresářů pro vyloučení při generování kontext sestavení.</span><span class="sxs-lookup"><span data-stu-id="7664c-135">*.dockerignore*: Contains a list of file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="7664c-136">*docker-compose.yml*: základní [Docker Compose](https://docs.docker.com/compose/overview/) soubor se používá k definování kolekce imagí k sestavení a spuštění pomocí `docker-compose build` a `docker-compose run`v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="7664c-136">*docker-compose.yml*: The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images to be built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="7664c-137">*docker-compose.override.yml*: volitelný soubor číst pomocí Docker Compose, obsahující konfiguraci přepsání pro služby.</span><span class="sxs-lookup"><span data-stu-id="7664c-137">*docker-compose.override.yml*: An optional file, read by Docker Compose, containing configuration overrides for services.</span></span> <span data-ttu-id="7664c-138">Visual Studio provede `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` sloučit tyto soubory.</span><span class="sxs-lookup"><span data-stu-id="7664c-138">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="7664c-139">A *soubor Dockerfile*, předpisu pro vytvoření finální image Dockeru, je přidán do kořenového adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="7664c-139">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="7664c-140">Odkazovat na [odkaz na soubor Dockerfile](https://docs.docker.com/engine/reference/builder/) pro pochopení příkazy v rámci něj.</span><span class="sxs-lookup"><span data-stu-id="7664c-140">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="7664c-141">Tento konkrétní *soubor Dockerfile* používá [vícefázových sestavení](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) obsahující čtyři různé, s názvem fáze sestavení umísťují:</span><span class="sxs-lookup"><span data-stu-id="7664c-141">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) containing four distinct, named build stages:</span></span>

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

<span data-ttu-id="7664c-142">*Soubor Dockerfile* vychází [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="7664c-142">The *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="7664c-143">Tento základní image obsahuje balíčky ASP.NET Core NuGet, které byly zpracované kompilátorem JIT předem zlepšit výkon při spouštění.</span><span class="sxs-lookup"><span data-stu-id="7664c-143">This base image includes the ASP.NET Core NuGet packages, which have been pre-jitted to improve startup performance.</span></span>

<span data-ttu-id="7664c-144">*Docker-compose.yml* soubor obsahuje název obrázku, který je vytvořen při spuštění projektu:</span><span class="sxs-lookup"><span data-stu-id="7664c-144">The *docker-compose.yml* file contains the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

<span data-ttu-id="7664c-145">V předchozím příkladu `image: hellodockertools` generuje obrázek `hellodockertools:dev` při spuštění aplikace **ladění** režimu.</span><span class="sxs-lookup"><span data-stu-id="7664c-145">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="7664c-146">`hellodockertools:latest` Image se vygeneruje, když aplikace běží v **vydání** režimu.</span><span class="sxs-lookup"><span data-stu-id="7664c-146">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="7664c-147">Před názvem obrázku [Docker Hubu](https://hub.docker.com/) uživatelské jméno (třeba `dockerhubusername/hellodockertools`) Pokud bitovou kopii se vloží do registru.</span><span class="sxs-lookup"><span data-stu-id="7664c-147">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image will be pushed to the registry.</span></span> <span data-ttu-id="7664c-148">Můžete také změnit název image přidat adresu URL privátního registru (například `privateregistry.domain.com/hellodockertools`) v závislosti na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="7664c-148">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

## <a name="debug"></a><span data-ttu-id="7664c-149">Ladit</span><span class="sxs-lookup"><span data-stu-id="7664c-149">Debug</span></span>

<span data-ttu-id="7664c-150">Vyberte **Docker** z ladění rozevírací seznam v panelu nástrojů a spusťte ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="7664c-150">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="7664c-151">**Docker** zobrazení **výstup** okno zobrazuje probíhají následující akce:</span><span class="sxs-lookup"><span data-stu-id="7664c-151">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

* <span data-ttu-id="7664c-152">*Microsoft/aspnetcore* bitové kopie modulu runtime je získali (pokud ještě není v mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="7664c-152">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="7664c-153">*Microsoft/aspnetcore-build* kompilace/publikování image je získali (pokud ještě není v mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="7664c-153">The *microsoft/aspnetcore-build* compile/publish image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="7664c-154">*ASPNETCORE_ENVIRONMENT* proměnná prostředí je nastavená na `Development` v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7664c-154">The *ASPNETCORE_ENVIRONMENT* environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="7664c-155">Port 80 je vystavena a mapují na dynamicky přidělovanou port pro místního hostitele.</span><span class="sxs-lookup"><span data-stu-id="7664c-155">Port 80 is exposed and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="7664c-156">Port, který je určený hostitelem Dockeru a může být dotázán pomocí `docker ps` příkazu.</span><span class="sxs-lookup"><span data-stu-id="7664c-156">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="7664c-157">Aplikace se zkopíruje do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7664c-157">The app is copied to the container.</span></span>
* <span data-ttu-id="7664c-158">Spustí výchozí prohlížeč se ladicí program připojený ke kontejneru pomocí dynamicky přiřazeného portu.</span><span class="sxs-lookup"><span data-stu-id="7664c-158">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="7664c-159">Je výsledný image Dockeru *dev* image aplikace s využitím *microsoft/aspnetcore* bitové kopie jako základní image.</span><span class="sxs-lookup"><span data-stu-id="7664c-159">The resulting Docker image is the *dev* image of the app with the *microsoft/aspnetcore* images as the base image.</span></span> <span data-ttu-id="7664c-160">Spustit `docker images` v příkaz **Konzola správce balíčků** okno (PMC).</span><span class="sxs-lookup"><span data-stu-id="7664c-160">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="7664c-161">Image na počítači se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="7664c-161">The images on the machine are displayed:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="7664c-162">Obrázek dev chybí obsah aplikace, jako **ladění** konfigurace používají připojení svazku pro zajištění iterativní prostředí.</span><span class="sxs-lookup"><span data-stu-id="7664c-162">The dev image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="7664c-163">Chcete-li odeslat image, použijte **vydání** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7664c-163">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="7664c-164">Spustit `docker ps` příkazu v konzole PMC.</span><span class="sxs-lookup"><span data-stu-id="7664c-164">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="7664c-165">Všimněte si, že je aplikace spuštěna pomocí kontejneru:</span><span class="sxs-lookup"><span data-stu-id="7664c-165">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="7664c-166">Upravit a pokračovat</span><span class="sxs-lookup"><span data-stu-id="7664c-166">Edit and continue</span></span>

<span data-ttu-id="7664c-167">Změny statických souborů a zobrazeními Razor se automaticky aktualizují bez nutnosti kompilační krok.</span><span class="sxs-lookup"><span data-stu-id="7664c-167">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="7664c-168">Proveďte požadovanou změnu, uložte a aktualizujte prohlížeč, chcete-li zobrazit aktualizace.</span><span class="sxs-lookup"><span data-stu-id="7664c-168">Make the change, save, and refresh the browser to view the update.</span></span>

<span data-ttu-id="7664c-169">Úpravy souborů kódu vyžadují kompilace a restartování Kestrel v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7664c-169">Code file modifications require compilation and a restart of Kestrel within the container.</span></span> <span data-ttu-id="7664c-170">Po provedení změn, použijte `CTRL+F5` provést proces a spusťte aplikaci v rámci kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7664c-170">After making the change, use `CTRL+F5` to perform the process and start the app within the container.</span></span> <span data-ttu-id="7664c-171">Kontejner Dockeru se znovu sestavit nebo zastavená.</span><span class="sxs-lookup"><span data-stu-id="7664c-171">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="7664c-172">Spustit `docker ps` příkazu v konzole PMC.</span><span class="sxs-lookup"><span data-stu-id="7664c-172">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="7664c-173">Všimněte si, že původní kontejneru je stále spuštěna od 10 minut před:</span><span class="sxs-lookup"><span data-stu-id="7664c-173">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="7664c-174">Publikování Image Dockeru</span><span class="sxs-lookup"><span data-stu-id="7664c-174">Publish Docker images</span></span>

<span data-ttu-id="7664c-175">Po dokončení cyklu vývoje a ladění aplikace Visual Studio Tools for Docker pomáhají při vytváření bitové kopie produkční aplikace.</span><span class="sxs-lookup"><span data-stu-id="7664c-175">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="7664c-176">Změna konfigurace rozevíracího seznamu **vydání** a sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="7664c-176">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="7664c-177">Bitová kopie se vytváří nástroje *nejnovější* značky, který může doručit bez vyžádání do privátního registru nebo centra Dockeru.</span><span class="sxs-lookup"><span data-stu-id="7664c-177">The tooling produces the image with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span>

<span data-ttu-id="7664c-178">Spustit `docker images` příkazu v konzole PMC zobrazíte seznam imagí:</span><span class="sxs-lookup"><span data-stu-id="7664c-178">Run the `docker images` command in PMC to see the list of images:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="7664c-179">`docker images` Příkaz vrátí zprostředkující obrázků s názvy úložišť a značky identifikována jako  *\<žádný >* (které nejsou uvedené výše).</span><span class="sxs-lookup"><span data-stu-id="7664c-179">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="7664c-180">Tyto nepojmenované bitové kopie jsou vytvářeny [vícefázových sestavení](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *soubor Dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="7664c-180">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="7664c-181">Pomáhají zvýšit efektivitu vytváření konečném obrazu&mdash;při změnách se znovu sestavit pouze nezbytné vrstvy.</span><span class="sxs-lookup"><span data-stu-id="7664c-181">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="7664c-182">Pokud zprostředkující bitové kopie jsou už je nepotřebujete, odstraňte je pomocí [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) příkazu.</span><span class="sxs-lookup"><span data-stu-id="7664c-182">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="7664c-183">Může být očekávání pro produkci nebo verze image, která se má menší velikosti oproti tomu na *dev* bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="7664c-183">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="7664c-184">Z důvodu mapování svazku byly spuštěné ladicího programu a aplikace z místního počítače a není v rámci kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7664c-184">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="7664c-185">*Nejnovější* image se zabalit kód potřebné aplikace a spusťte tak aplikaci na hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="7664c-185">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="7664c-186">Delta proto je velikost kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="7664c-186">Therefore, the delta is the size of the app code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7664c-187">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7664c-187">Additional resources</span></span>

* [<span data-ttu-id="7664c-188">Řešení potíží s Visual Studio 2017 vývoj aplikací pomocí Dockeru</span><span class="sxs-lookup"><span data-stu-id="7664c-188">Troubleshoot Visual Studio 2017 development with Docker</span></span>](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [<span data-ttu-id="7664c-189">Visual Studio Tools pro úložiště GitHub Dockeru</span><span class="sxs-lookup"><span data-stu-id="7664c-189">Visual Studio Tools for Docker GitHub repository</span></span>](https://github.com/Microsoft/DockerTools)
