---
title: "Sestavení projektů s Yeoman v ASP.NET Core"
author: spboyer
description: "Tento článek vás provede vytváření webové aplikace pomocí Yeoman ASP.NET Core generátor v systému macOS."
keywords: "ASP.NET Core, Yeoman, křížové platformy yo aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a><span data-ttu-id="e0e05-104">Úvod do vytváření projektů s Yeoman v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0e05-104">Introduction to building projects with Yeoman in ASP.NET Core</span></span>

<span data-ttu-id="e0e05-105">[Yeoman](http://yeoman.io/) je systém generování uživatelského rozhraní projektu pro vytváření mnoho typů aplikací.</span><span class="sxs-lookup"><span data-stu-id="e0e05-105">[Yeoman](http://yeoman.io/) is a project scaffolding system for creating many kinds of applications.</span></span> <span data-ttu-id="e0e05-106">Yeoman generátor pro ASP.NET Core obsahuje různé šablony projektů pro spuštění nové webové, MVC nebo konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e0e05-106">The Yeoman generator for ASP.NET Core contains a variety of project templates for starting a new web, MVC, or console application.</span></span>

## <a name="install-nodejs-npm-and-yeoman"></a><span data-ttu-id="e0e05-107">Nainstalovat Node.js, npm a Yeoman</span><span class="sxs-lookup"><span data-stu-id="e0e05-107">Install Node.js, npm, and Yeoman</span></span>

### <a name="prerequisites"></a><span data-ttu-id="e0e05-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e0e05-108">Prerequisites</span></span>

<span data-ttu-id="e0e05-109">Jsou vyžadovány pro Yeoman Node.js a npm.</span><span class="sxs-lookup"><span data-stu-id="e0e05-109">Node.js and npm are required for Yeoman.</span></span> <span data-ttu-id="e0e05-110">Stáhnout z [Node.js](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="e0e05-110">Download from [Node.js](https://nodejs.org/).</span></span> <span data-ttu-id="e0e05-111">Instalační program obsahuje [Node.js](https://nodejs.org/) a [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="e0e05-111">The installer includes [Node.js](https://nodejs.org/) and [npm](https://www.npmjs.com/).</span></span> <span data-ttu-id="e0e05-112">Také je vyžadována pro instalaci architektury uživatelského rozhraní, jako je Bootstrap bower.</span><span class="sxs-lookup"><span data-stu-id="e0e05-112">Bower is also required for installing UI frameworks like Bootstrap.</span></span>

<span data-ttu-id="e0e05-113">Chcete-li nainstalovat Yeoman a Bower, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e0e05-113">To install Yeoman and Bower, run the following command:</span></span>

```console
npm install -g yo bower
```

>[!Note]
><span data-ttu-id="e0e05-114">Pokud dojde k chybě `npm ERR! Please try running this command again as root/Administrator.` v systému macOS, spusťte následující příkaz pomocí [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`</span><span class="sxs-lookup"><span data-stu-id="e0e05-114">If you get the error `npm ERR! Please try running this command again as root/Administrator.` on macOS, run the following command using [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html): `sudo npm install -g yo bower`</span></span>

<span data-ttu-id="e0e05-115">Z příkazového řádku nainstalujte generátor ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="e0e05-115">From a command prompt, install the ASP.NET generator:</span></span>

```console
npm install -g generator-aspnet
```

> [!NOTE]
> <span data-ttu-id="e0e05-116">Pokud dojde k chybě oprávnění, spusťte příkaz pod `sudo` jak bylo popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="e0e05-116">If you get a permission error, run the command under `sudo` as described above.</span></span>

<span data-ttu-id="e0e05-117">`–g` Příznak nainstaluje generátor globálně, takže lze z cesty.</span><span class="sxs-lookup"><span data-stu-id="e0e05-117">The `–g` flag installs the generator globally, so that it can be used from any path.</span></span>

## <a name="create-an-aspnet-app"></a><span data-ttu-id="e0e05-118">Vytvoření aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e0e05-118">Create an ASP.NET app</span></span>

<span data-ttu-id="e0e05-119">Spusťte generátor na základě Yeoman ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="e0e05-119">Run the Yeoman-based ASP.NET generator:</span></span>

```console
yo aspnet
```

<span data-ttu-id="e0e05-120">Generátor zobrazí nabídku.</span><span class="sxs-lookup"><span data-stu-id="e0e05-120">The generator displays a menu.</span></span> <span data-ttu-id="e0e05-121">Šipka dolů **webové aplikace základní [bez členství a autorizace]** projektu a klepněte na **Enter**:</span><span class="sxs-lookup"><span data-stu-id="e0e05-121">Arrow down to the **Web Application Basic [without Membership and Authorization]** project and tap **Enter**:</span></span>

![Příkazové okno: Jaký typ aplikace chcete vytvořit?](yeoman/_static/yeoman-yo-aspnet.png)

<span data-ttu-id="e0e05-124">Vyberte Bootstrap jako rozhraní uživatelského rozhraní a klepněte na **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e0e05-124">Select Bootstrap as the UI Framework and tap **Enter**.</span></span>

<span data-ttu-id="e0e05-125">Použití "**MyWebApp**" pro název aplikace a potom klepněte na **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e0e05-125">Use "**MyWebApp**" for the app name and then tap **Enter**.</span></span>

<span data-ttu-id="e0e05-126">Yeoman bude vygenerovat projektu a jeho podpůrné soubory.</span><span class="sxs-lookup"><span data-stu-id="e0e05-126">Yeoman will scaffold the project and its supporting files.</span></span> <span data-ttu-id="e0e05-127">Navrhované další kroky jsou také uvedeny ve formě příkazy.</span><span class="sxs-lookup"><span data-stu-id="e0e05-127">Suggested next steps are also provided in the form of commands.</span></span>

![Příkazové okno: co je název vaší aplikace ASP.NET?](yeoman/_static/yeoman-yo-aspnet-created.png)

<span data-ttu-id="e0e05-130">[ASP.NET generátor](https://www.npmjs.com/package/generator-aspnet) vytvoří ASP.NET Core projekty, které mohou být načtena do Visual Studio Code, Visual Studio, nebo spusťte z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e0e05-130">The [ASP.NET generator](https://www.npmjs.com/package/generator-aspnet) creates ASP.NET Core projects that can be loaded into Visual Studio Code, Visual Studio, or run from the command line.</span></span>

## <a name="restore-build-and-run"></a><span data-ttu-id="e0e05-131">Obnovení, vytvoření a spuštění</span><span class="sxs-lookup"><span data-stu-id="e0e05-131">Restore, build, and run</span></span>

<span data-ttu-id="e0e05-132">Postupujte podle navrhované příkazy změnou do adresáře `MyWebApp` adresáře.</span><span class="sxs-lookup"><span data-stu-id="e0e05-132">Follow the suggested commands by changing directories to the `MyWebApp` directory.</span></span> <span data-ttu-id="e0e05-133">Spusťte `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="e0e05-133">Then run `dotnet restore`.</span></span>

![Příkazové okno](yeoman/_static/dotnet-restore.png)

<span data-ttu-id="e0e05-135">Sestavení a spuštění aplikace pomocí `dotnet build` a `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="e0e05-135">Build and run the app using `dotnet build` and `dotnet run`:</span></span>

![Příkazové okno](yeoman/_static/dotnet-build-run.png)

<span data-ttu-id="e0e05-137">V tomto okamžiku můžete přejít na adresu URL v k testování aplikace ASP.NET Core nově vytvořený.</span><span class="sxs-lookup"><span data-stu-id="e0e05-137">At this point, you can navigate to the URL shown to test the newly-created ASP.NET Core app.</span></span>

## <a name="client-side-packages"></a><span data-ttu-id="e0e05-138">Balíčky na straně klienta</span><span class="sxs-lookup"><span data-stu-id="e0e05-138">Client-side packages</span></span>

<span data-ttu-id="e0e05-139">Front-endové prostředky jsou poskytovány šablony z Yeoman generátor pomocí [Bower](xref:client-side/bower) Správce balíčků na straně klienta, přidání *bower.json* a *.bowerrc* soubory obnovení klientské balíčky pomocí Bower.</span><span class="sxs-lookup"><span data-stu-id="e0e05-139">The front-end resources are provided by the templates from the Yeoman generator using the [Bower](xref:client-side/bower) client-side package manager, adding *bower.json* and *.bowerrc* files to restore client-side packages using Bower.</span></span>

<span data-ttu-id="e0e05-140">[BundlerMinifier](xref:client-side/bundling-and-minification) součásti jsou také obsaženy ve výchozím nastavení pro snadné zřetězení (sdružování) a minimalizace šablon stylů CSS, JavaScript a HTML.</span><span class="sxs-lookup"><span data-stu-id="e0e05-140">The [BundlerMinifier](xref:client-side/bundling-and-minification) component is also included by default for ease of concatenation (bundling) and minification of CSS, JavaScript, and HTML.</span></span>

## <a name="building-and-running-from-visual-studio"></a><span data-ttu-id="e0e05-141">Vytváření a spouštění ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e0e05-141">Building and running from Visual Studio</span></span>

<span data-ttu-id="e0e05-142">Můžete načíst generovaného webový projekt ASP.NET Core přímo do sady Visual Studio, pak sestavit a spustit projekt z ní.</span><span class="sxs-lookup"><span data-stu-id="e0e05-142">You can load your generated ASP.NET Core web project directly into Visual Studio, then build and run your project from there.</span></span> <span data-ttu-id="e0e05-143">Postupujte podle pokynů výše a vygenerovat novou aplikaci ASP.NET Core pomocí Yeoman.</span><span class="sxs-lookup"><span data-stu-id="e0e05-143">Follow the instructions above to scaffold a new ASP.NET Core app using Yeoman.</span></span> <span data-ttu-id="e0e05-144">Tentokrát vyberte **webové aplikace** z nabídky a název aplikace `MyWebApp`.</span><span class="sxs-lookup"><span data-stu-id="e0e05-144">This time, choose **Web Application** from the menu and name the app `MyWebApp`.</span></span>

<span data-ttu-id="e0e05-145">Otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0e05-145">Open Visual Studio.</span></span> <span data-ttu-id="e0e05-146">V nabídce Soubor vyberte otevřete ‣ projekt nebo řešení.</span><span class="sxs-lookup"><span data-stu-id="e0e05-146">From the File menu, select Open ‣ Project/Solution.</span></span>

<span data-ttu-id="e0e05-147">V dialogovém okně Otevřít projekt, přejděte na *.csproj* souboru, vyberte ho a klikněte **otevřete** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e0e05-147">In the Open Project dialog, navigate to the *.csproj* file, select it, and click the **Open** button.</span></span> <span data-ttu-id="e0e05-148">V Průzkumníku řešení projekt by měl vypadat podobně jako na následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="e0e05-148">In the Solution Explorer, the project should look something like the screenshot below.</span></span>

![Soubory a složky na nový projekt v Průzkumníku řešení](yeoman/_static/yeoman-solution.png)

<span data-ttu-id="e0e05-150">Yeoman scaffolds webové aplikace MVC, dokončení s oběma straně serveru a klienta sestavení podpory.</span><span class="sxs-lookup"><span data-stu-id="e0e05-150">Yeoman scaffolds a MVC web application, complete with both server- and client-side build support.</span></span> <span data-ttu-id="e0e05-151">Serverové závislosti jsou uvedeny v seznamu **závislosti nebo NuGet** uzel a závislostí na straně klienta v **závislosti/Bower** uzlu v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="e0e05-151">Server-side dependencies are listed under the **Dependencies/NuGet** node, and client-side dependencies in the **Dependencies/Bower** node of Solution Explorer.</span></span> <span data-ttu-id="e0e05-152">Závislosti jsou obnoveny automaticky při načtení projektu.</span><span class="sxs-lookup"><span data-stu-id="e0e05-152">Dependencies are restored automatically when the project is loaded.</span></span>

![V uzlu závislosti ve stromovém zobrazení Průzkumník řešení složce Bower je otevřený, výpis jeho závislé součásti.](yeoman/_static/yeoman-loading-dependencies.png)

<span data-ttu-id="e0e05-154">Když se obnoví všechny závislosti, stiskněte klávesu **F5** spustit projekt.</span><span class="sxs-lookup"><span data-stu-id="e0e05-154">When all the dependencies are restored, press **F5** to run the project.</span></span> <span data-ttu-id="e0e05-155">Zobrazí výchozí domovskou stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="e0e05-155">The default home page displays in the browser.</span></span>

![Webové aplikace, otevřete v Microsoft Edge](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a><span data-ttu-id="e0e05-157">Obnovení, vytvoření a hostování z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e0e05-157">Restoring, building, and hosting from a command line</span></span>

<span data-ttu-id="e0e05-158">Můžete připravit a hostitele webové aplikace pomocí rozhraní příkazového řádku .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e0e05-158">You can prepare and host your web application using the .NET Core CLI.</span></span>

<span data-ttu-id="e0e05-159">Na příkazovém řádku změňte aktuální adresář na složku obsahující projekt (to znamená, složku, která obsahuje *.csproj* souborů):</span><span class="sxs-lookup"><span data-stu-id="e0e05-159">At a command prompt, change the current directory to the folder containing the project (that is, the folder containing the *.csproj* file):</span></span>

```console
cd src\MyWebApp
```

<span data-ttu-id="e0e05-160">Obnovte závislosti balíčků NuGet projektu:</span><span class="sxs-lookup"><span data-stu-id="e0e05-160">Restore the project's NuGet package dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="e0e05-161">Spusťte aplikaci:</span><span class="sxs-lookup"><span data-stu-id="e0e05-161">Run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="e0e05-162">Napříč platformami [Kestrel](xref:fundamentals/servers/kestrel) webový server bude zahájeno naslouchání na portu 5000.</span><span class="sxs-lookup"><span data-stu-id="e0e05-162">The cross-platform [Kestrel](xref:fundamentals/servers/kestrel) web server will begin listening on port 5000.</span></span>

<span data-ttu-id="e0e05-163">Otevřete webový prohlížeč a přejděte na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e0e05-163">Open a web browser, and navigate to `http://localhost:5000`.</span></span>

![Webové aplikace, otevřete v Microsoft Edge](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a><span data-ttu-id="e0e05-165">Přidání do projektu s dílčí generátory</span><span class="sxs-lookup"><span data-stu-id="e0e05-165">Adding to your project with sub generators</span></span>

<span data-ttu-id="e0e05-166">Pomocí Yeoman [sub generátory](https://github.com/omnisharp/generator-aspnet), můžete přidat buď `nuget.config` nebo `web.config` po vytvoření projektu.</span><span class="sxs-lookup"><span data-stu-id="e0e05-166">Using Yeoman [sub generators](https://github.com/omnisharp/generator-aspnet), you can add either a `nuget.config` or a `web.config` after the project is created.</span></span> <span data-ttu-id="e0e05-167">Například spusťte následující příkaz z adresáře, ve kterém by vytvořit soubor:</span><span class="sxs-lookup"><span data-stu-id="e0e05-167">For example, execute the following command from the directory in which the file should be created:</span></span>

```console
yo aspnet:nugetconfig
```

<span data-ttu-id="e0e05-168">Výsledkem je NuGet konfigurační soubor s názvem `nuget.config` s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="e0e05-168">The result is a NuGet configuration file named `nuget.config` with the following content:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a><span data-ttu-id="e0e05-169">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e0e05-169">Additional resources</span></span>

* [<span data-ttu-id="e0e05-170">Servery (Kestrel a WebListener)</span><span class="sxs-lookup"><span data-stu-id="e0e05-170">Servers (Kestrel and WebListener)</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="e0e05-171">Základy</span><span class="sxs-lookup"><span data-stu-id="e0e05-171">Fundamentals</span></span>](xref:fundamentals/index)
