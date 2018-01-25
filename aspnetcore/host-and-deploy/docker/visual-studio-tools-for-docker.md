---
title: "Visual Studio Tools pro Docker základní technologie ASP.NET"
author: spboyer
description: "Zjistěte, jak pomocí nástrojů Visual Studio 2017 a Docker pro systém Windows containerize aplikace ASP.NET Core."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: caf0e423d8e6f61fd2470d1f4ea2dd93909c3696
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="55d56-103">Visual Studio Tools pro Docker základní technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="55d56-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="55d56-104">[Visual Studio 2017](https://www.visualstudio.com/) podporuje vytváření, ladění a spouštění ASP.NET Core aplikace cílený na rozhraní .NET Framework nebo .NET Core.</span><span class="sxs-lookup"><span data-stu-id="55d56-104">[Visual Studio 2017](https://www.visualstudio.com/) supports building, debugging, and running ASP.NET Core apps targeting either .NET Framework or .NET Core.</span></span> <span data-ttu-id="55d56-105">Kontejnery Windows a Linux jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="55d56-105">Both Windows and Linux containers are supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55d56-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="55d56-106">Prerequisites</span></span>

* <span data-ttu-id="55d56-107">[Visual Studio 2017](https://www.visualstudio.com/) s **vývoj pro různé platformy .NET Core** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="55d56-107">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>
* [<span data-ttu-id="55d56-108">Docker pro Windows</span><span class="sxs-lookup"><span data-stu-id="55d56-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="55d56-109">Instalace a nastavení</span><span class="sxs-lookup"><span data-stu-id="55d56-109">Installation and setup</span></span>

<span data-ttu-id="55d56-110">Pro instalaci Docker, přečtěte si informace v [Docker pro Windows: co potřebujete vědět před instalací](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) a nainstalujte [Docker pro systém Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="55d56-110">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="55d56-111">**[Sdílené disky](https://docs.docker.com/docker-for-windows/#shared-drives)**  v Docker pro systém Windows musí být nakonfigurované pro podporu mapování svazku a ladění.</span><span class="sxs-lookup"><span data-stu-id="55d56-111">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="55d56-112">Klikněte pravým tlačítkem na ikonu na hlavním panelu Docker, vyberte **nastavení...** a vyberte **sdílené disky**.</span><span class="sxs-lookup"><span data-stu-id="55d56-112">Right-click the System Tray's Docker icon, select **Settings...**, and select **Shared Drives**.</span></span> <span data-ttu-id="55d56-113">Vyberte jednotku, kde Docker ukládá soubory.</span><span class="sxs-lookup"><span data-stu-id="55d56-113">Select the drive where Docker stores files.</span></span> <span data-ttu-id="55d56-114">Vyberte **použít**.</span><span class="sxs-lookup"><span data-stu-id="55d56-114">Select **Apply**.</span></span>

![Sdílené disky](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="55d56-116">Visual Studio 2017 verze 15.6 a novější výzvu, při **sdílené disky** nejsou nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="55d56-116">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-docker-support-to-an-app"></a><span data-ttu-id="55d56-117">Přidání podpory Docker do aplikace</span><span class="sxs-lookup"><span data-stu-id="55d56-117">Add Docker support to an app</span></span>

<span data-ttu-id="55d56-118">Cílový framework projektu ASP.NET Core Určuje typy podporované kontejneru.</span><span class="sxs-lookup"><span data-stu-id="55d56-118">The ASP.NET Core project's target framework determines the supported container types.</span></span> <span data-ttu-id="55d56-119">Projektech zacílených na .NET Core podporovat kontejnery Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="55d56-119">Projects targeting .NET Core support both Linux and Windows containers.</span></span> <span data-ttu-id="55d56-120">Projekty pouze cílení na rozhraní .NET Framework podporují Windows kontejnery.</span><span class="sxs-lookup"><span data-stu-id="55d56-120">Projects targeting .NET Framework only support Windows containers.</span></span>

<span data-ttu-id="55d56-121">Když přidáváte podporu Docker do projektu, zvolte Windows nebo Linux kontejneru.</span><span class="sxs-lookup"><span data-stu-id="55d56-121">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="55d56-122">Hostitelů Docker musí používat stejný typ kontejneru.</span><span class="sxs-lookup"><span data-stu-id="55d56-122">The Docker host must be running the same container type.</span></span> <span data-ttu-id="55d56-123">Chcete-li změnit typ kontejneru v běžící instance Docker, klikněte pravým tlačítkem na ikonu Docker na hlavním panelu a vyberte **přepnout do kontejnerů Windows...**  nebo **přepnout do kontejnerů Linux...** .</span><span class="sxs-lookup"><span data-stu-id="55d56-123">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="55d56-124">Nové aplikace</span><span class="sxs-lookup"><span data-stu-id="55d56-124">New app</span></span>

<span data-ttu-id="55d56-125">Při vytváření nové aplikace s **webové aplikace ASP.NET Core** šablony projektů, vyberte **povolení podpory Docker** políčko:</span><span class="sxs-lookup"><span data-stu-id="55d56-125">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** checkbox:</span></span>

![Povolit podporu Docker zaškrtávací políčko](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

<span data-ttu-id="55d56-127">Pokud je cílový framework .NET Core **OS** umožňuje rozevíracího seznamu pro výběr typu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="55d56-127">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="55d56-128">Stávající aplikace</span><span class="sxs-lookup"><span data-stu-id="55d56-128">Existing app</span></span>

<span data-ttu-id="55d56-129">Visual Studio Tools for Docker nepodporují, přidání Docker do existujícího projektu ASP.NET Core cílení na rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="55d56-129">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span> <span data-ttu-id="55d56-130">Pro projekty ASP.NET Core cílení na .NET Core existují dvě možnosti pro přidání podpory Docker prostřednictvím nástrojů.</span><span class="sxs-lookup"><span data-stu-id="55d56-130">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="55d56-131">Otevřete projekt v sadě Visual Studio a zvolte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="55d56-131">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="55d56-132">Vyberte **Docker podporu** z **projektu** nabídky.</span><span class="sxs-lookup"><span data-stu-id="55d56-132">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="55d56-133">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **přidat** > **Docker podporu**.</span><span class="sxs-lookup"><span data-stu-id="55d56-133">Right-click the project in Solution Explorer and select **Add** > **Docker Support**.</span></span>

## <a name="docker-assets-overview"></a><span data-ttu-id="55d56-134">Přehled prostředků docker</span><span class="sxs-lookup"><span data-stu-id="55d56-134">Docker assets overview</span></span>

<span data-ttu-id="55d56-135">Přidat sady Visual Studio Tools for Docker *docker-tvoří* projektu a řešení, která obsahuje následující:</span><span class="sxs-lookup"><span data-stu-id="55d56-135">The Visual Studio Tools for Docker add a *docker-compose* project to the solution, containing the following:</span></span>

* <span data-ttu-id="55d56-136">*.dockerignore*: obsahuje seznam vzorů souborů a adresářů pro vyloučení při generování sestavení kontextu.</span><span class="sxs-lookup"><span data-stu-id="55d56-136">*.dockerignore*: Contains a list of file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="55d56-137">*docker-compose.yml*: základní [Docker Compose](https://docs.docker.com/compose/overview/) soubor používá k definování kolekce bitové kopie vytvořené a spusťte s `docker-compose build` a `docker-compose run`, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="55d56-137">*docker-compose.yml*: The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images to be built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="55d56-138">*docker compose.override.yml*: volitelný soubor, přečtěte si pomocí Docker Compose, obsahující konfiguraci přepsání pro služby.</span><span class="sxs-lookup"><span data-stu-id="55d56-138">*docker-compose.override.yml*: An optional file, read by Docker Compose, containing configuration overrides for services.</span></span> <span data-ttu-id="55d56-139">Visual Studio provede `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` na sloučení těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="55d56-139">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="55d56-140">A *soubor Docker*, recepturách pro vytvoření finální image Docker, se přidá do kořenového adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="55d56-140">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="55d56-141">Odkazovat na [odkaz na soubor Docker](https://docs.docker.com/engine/reference/builder/) pro pochopení příkazy v něm.</span><span class="sxs-lookup"><span data-stu-id="55d56-141">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="55d56-142">Tato konkrétní *soubor Docker* používá [více fáze sestavení](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) obsahující jedinečné, čtyři fáze sestavení s názvem:</span><span class="sxs-lookup"><span data-stu-id="55d56-142">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) containing four distinct, named build stages:</span></span>

[!code-text[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

<span data-ttu-id="55d56-143">*Soubor Docker* je založena na [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="55d56-143">The *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="55d56-144">Tato základní image obsahuje balíčků ASP.NET Core NuGet, které byly před jitted ke zlepšení výkonu při spuštění.</span><span class="sxs-lookup"><span data-stu-id="55d56-144">This base image includes the ASP.NET Core NuGet packages, which have been pre-jitted to improve startup performance.</span></span>

<span data-ttu-id="55d56-145">*Docker-compose.yml* soubor obsahuje název obrázku, který se vytvoří při spuštění projektu:</span><span class="sxs-lookup"><span data-stu-id="55d56-145">The *docker-compose.yml* file contains the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

<span data-ttu-id="55d56-146">V předchozím příkladu `image: hellodockertools` generuje bitovou kopii `hellodockertools:dev` při spuštění aplikace **ladění** režimu.</span><span class="sxs-lookup"><span data-stu-id="55d56-146">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="55d56-147">`hellodockertools:latest` Bitové kopie se vygeneruje, když aplikace běží v **verze** režimu.</span><span class="sxs-lookup"><span data-stu-id="55d56-147">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="55d56-148">Zadejte před název bitové kopie [úložiště Docker Hub](https://hub.docker.com/) uživatelské jméno (například `dockerhubusername/hellodockertools`) Pokud bitovou kopii se vloží do registru.</span><span class="sxs-lookup"><span data-stu-id="55d56-148">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image will be pushed to the registry.</span></span> <span data-ttu-id="55d56-149">Můžete taky změnit název bitové kopie přidat URL privátní registru (například `privateregistry.domain.com/hellodockertools`) v závislosti na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="55d56-149">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

## <a name="debug"></a><span data-ttu-id="55d56-150">Ladit</span><span class="sxs-lookup"><span data-stu-id="55d56-150">Debug</span></span>

<span data-ttu-id="55d56-151">Vyberte **Docker** z rozevíracího seznamu v panelu nástrojů a spuštění ladění aplikace ladění.</span><span class="sxs-lookup"><span data-stu-id="55d56-151">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="55d56-152">**Docker** zobrazení **výstup** v okně se zobrazí probíhají následující akce:</span><span class="sxs-lookup"><span data-stu-id="55d56-152">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

* <span data-ttu-id="55d56-153">*Microsoft/aspnetcore* runtime image je získali (pokud ještě není v mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="55d56-153">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="55d56-154">*Microsoft/aspnetcore sestavení* kompilace nebo publikování bitové kopie je získali (pokud ještě není v mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="55d56-154">The *microsoft/aspnetcore-build* compile/publish image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="55d56-155">*ASPNETCORE_ENVIRONMENT* proměnná prostředí je nastavená na `Development` v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="55d56-155">The *ASPNETCORE_ENVIRONMENT* environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="55d56-156">Port 80 je vystavený a mapovat na port dynamicky přiřadit pro místního hostitele.</span><span class="sxs-lookup"><span data-stu-id="55d56-156">Port 80 is exposed and mapped to a dynamically-assigned port for localhost.</span></span> <span data-ttu-id="55d56-157">Port je dáno hostitelů Docker a lze dotazovat se `docker ps` příkaz.</span><span class="sxs-lookup"><span data-stu-id="55d56-157">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="55d56-158">Aplikace se zkopíruje do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="55d56-158">The app is copied to the container.</span></span>
* <span data-ttu-id="55d56-159">Spuštění výchozího prohlížeče s ladicím programem připojené ke kontejneru dynamicky přiřadit port je používán.</span><span class="sxs-lookup"><span data-stu-id="55d56-159">The default browser is launched with the debugger attached to the container using the dynamically-assigned port.</span></span> 

<span data-ttu-id="55d56-160">Je výsledný obraz Docker *dev* bitové kopie aplikace pomocí *microsoft/aspnetcore* bitové kopie jako základní bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="55d56-160">The resulting Docker image is the *dev* image of the app with the *microsoft/aspnetcore* images as the base image.</span></span> <span data-ttu-id="55d56-161">Spustit `docker images` v příkazu **Konzola správce balíčků** okno (pomocí PMC).</span><span class="sxs-lookup"><span data-stu-id="55d56-161">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="55d56-162">Image na počítači se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="55d56-162">The images on the machine are displayed:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="55d56-163">Bitovou kopii dev chybí obsah aplikace, jako **ladění** konfigurace používají připojení svazku a poskytuje iterativní prostředí.</span><span class="sxs-lookup"><span data-stu-id="55d56-163">The dev image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="55d56-164">Chcete-li push bitovou kopii, použijte **verze** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="55d56-164">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="55d56-165">Spustit `docker ps` v pomocí PMC.</span><span class="sxs-lookup"><span data-stu-id="55d56-165">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="55d56-166">Všimněte si, že je že aplikace spuštěná pomocí kontejneru:</span><span class="sxs-lookup"><span data-stu-id="55d56-166">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="55d56-167">Upravit a pokračovat</span><span class="sxs-lookup"><span data-stu-id="55d56-167">Edit and continue</span></span>

<span data-ttu-id="55d56-168">Změny statické soubory a zobrazení syntaxe Razor se automaticky aktualizují bez nutnosti kompilační krok.</span><span class="sxs-lookup"><span data-stu-id="55d56-168">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="55d56-169">Proveďte požadovanou změnu, uložte a aktualizujte stránku prohlížeče zobrazíte aktualizace.</span><span class="sxs-lookup"><span data-stu-id="55d56-169">Make the change, save, and refresh the browser to view the update.</span></span>  

<span data-ttu-id="55d56-170">Úpravy, aby se soubory kódu vyžaduje kompilování a restartování Kestrel v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="55d56-170">Modifications to code files requires compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="55d56-171">Po provedení změny, použijte kombinaci kláves CTRL + F5 k provedení proces a spusťte aplikaci v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="55d56-171">After making the change, use CTRL + F5 to perform the process and start the app within the container.</span></span> <span data-ttu-id="55d56-172">Kontejner Docker není znovu sestavit nebo zastavená.</span><span class="sxs-lookup"><span data-stu-id="55d56-172">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="55d56-173">Spustit `docker ps` v pomocí PMC.</span><span class="sxs-lookup"><span data-stu-id="55d56-173">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="55d56-174">Všimněte si původní kontejner je stále spuštěná od 10 minut před:</span><span class="sxs-lookup"><span data-stu-id="55d56-174">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="55d56-175">Publikování imagí Dockeru</span><span class="sxs-lookup"><span data-stu-id="55d56-175">Publish Docker images</span></span>

<span data-ttu-id="55d56-176">Po dokončení cyklu vývoj a ladění aplikace sady Visual Studio Tools for Docker pomůžou při vytváření bitové kopie produkční aplikace.</span><span class="sxs-lookup"><span data-stu-id="55d56-176">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="55d56-177">Změnit konfiguraci rozevírací seznam pro **verze** a sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="55d56-177">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="55d56-178">Nástroji vytvoří bitovou kopii s *nejnovější* značku, která se můžou poslat privátní registru nebo úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="55d56-178">The tooling produces the image with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span> 

<span data-ttu-id="55d56-179">Spustit `docker images` v pomocí PMC zobrazíte seznam bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="55d56-179">Run the `docker images` command in PMC to see the list of images:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="55d56-180">`docker images` Příkaz vrátí zprostředkující bitové kopie s názvy úložiště a tagy identifikovaného jako  *\<žádné >* (které nejsou uvedené výše).</span><span class="sxs-lookup"><span data-stu-id="55d56-180">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="55d56-181">Tyto bitové kopie nepojmenované vznikají pomocí funkcí [více fáze sestavení](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *soubor Docker*.</span><span class="sxs-lookup"><span data-stu-id="55d56-181">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="55d56-182">Jejich zlepšit efektivitu vytváření finální image&mdash;pouze nezbytné vrstvy se znovu sestavit při změnách.</span><span class="sxs-lookup"><span data-stu-id="55d56-182">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="55d56-183">Když zprostředkující bitové kopie již nejsou potřebné, je odstranit pomocí [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) příkaz.</span><span class="sxs-lookup"><span data-stu-id="55d56-183">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="55d56-184">Může být očekávání pro produkční nebo verzi obrázek, který má být menší velikost oproti *dev* bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="55d56-184">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="55d56-185">Z důvodu mapování svazku, aplikace a ladicí program běžely z místního počítače a ne do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="55d56-185">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="55d56-186">*Nejnovější* image má zabalí kód potřebné aplikace a spusťte aplikaci na hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="55d56-186">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="55d56-187">Rozdíl je proto velikost kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="55d56-187">Therefore, the delta is the size of the app code.</span></span>
