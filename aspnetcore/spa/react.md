---
title: Šablona projektu reagují pomocí ASP.NET Core
author: SteveSandersonMS
description: Zjistěte, jak začít pracovat se šablonou projektu ASP.NET Core jedné stránky aplikace (SPA) pro reagují a vytvořit aplikaci reagují.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: c88320c628f219652a2cb63a16b494661c481ffb
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/27/2018
---
# <a name="use-the-react-project-template-with-aspnet-core"></a><span data-ttu-id="4d9cf-103">Šablona projektu reagují pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d9cf-103">Use the React project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="4d9cf-104">Tato dokumentace není o šablona projektu reagují součástí technologie ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-104">This documentation isn't about the React project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="4d9cf-105">Jde o šabloně novější reagují, do kterého můžete ručně aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-105">It's about the newer React template to which you can update manually.</span></span> <span data-ttu-id="4d9cf-106">Šablona je součástí 2.1 jádro ASP.NET ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="4d9cf-107">Aktualizovanou šablonu projektu reagují poskytuje příhodný výchozí bod pro ASP.NET Core aplikací pomocí reagují a [vytvořit aplikaci reagují](https://github.com/facebookincubator/create-react-app) konvence (CRA) k implementaci bohatou a klientské uživatelské rozhraní (UI).</span><span class="sxs-lookup"><span data-stu-id="4d9cf-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="4d9cf-108">Šablona je ekvivalentní k vytvoření projektu ASP.NET Core tak, aby fungoval jako back-end rozhraní API i standardní CRA reagovat projekt tak, aby fungoval jako uživatelského rozhraní, ale s výhodou hostování oba seznamy v jedné aplikaci projekt, který můžete vytvořená a publikovaná jako na jednu jednotku.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="4d9cf-109">Vytvoření nové aplikace</span><span class="sxs-lookup"><span data-stu-id="4d9cf-109">Create a new app</span></span>

<span data-ttu-id="4d9cf-110">Pokud používáte technologii ASP.NET 2.0 jádra, ujistěte se, když jste [nainstalovat aktualizovanou šablonu projektu reagují](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="4d9cf-110">If using ASP.NET Core 2.0, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span> <span data-ttu-id="4d9cf-111">Pokud máte 2.1 jádro ASP.NET, není nutné ji nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-111">If you have ASP.NET Core 2.1, there's no need to install it.</span></span>

<span data-ttu-id="4d9cf-112">Vytvoření nového projektu z příkazového řádku pomocí příkazu `dotnet new react` v prázdného adresáře.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="4d9cf-113">Můžete například vytvořit následující příkazy v aplikaci *-nové aplikace my* adresáře a přepnete se do této složky:</span><span class="sxs-lookup"><span data-stu-id="4d9cf-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="4d9cf-114">Spusťte aplikaci v sadě Visual Studio nebo .NET Core rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="4d9cf-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4d9cf-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4d9cf-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4d9cf-116">Otevřete vygenerovaného *.csproj* souboru a spuštění aplikace jako normální odtud.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="4d9cf-117">Proces sestavení obnoví npm závislosti během prvního spuštění, což může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="4d9cf-118">Následující sestavení je mnohem rychlejší.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4d9cf-119">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="4d9cf-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4d9cf-120">Zajistěte, abyste měli proměnné prostředí s názvem `ASPNETCORE_Environment` s hodnotou `Development`.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="4d9cf-121">V systému Windows (v výzvy – prostředí PowerShell), spusťte `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="4d9cf-122">V systému macOS nebo Linux, spusťte `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="4d9cf-123">Spustit [dotnet sestavení](/dotnet/core/tools/dotnet-build) k ověření aplikace sestavení správně.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-123">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify your app builds correctly.</span></span> <span data-ttu-id="4d9cf-124">Při prvním spuštění procesu sestavení obnoví npm závislosti, které může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="4d9cf-125">Následující sestavení je mnohem rychlejší.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="4d9cf-126">Spustit [dotnet spustit](/dotnet/core/tools/dotnet-run) a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-126">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span>

---

<span data-ttu-id="4d9cf-127">Šablona projektu vytvoří aplikace ASP.NET Core a reagují aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="4d9cf-128">Aplikace ASP.NET Core se má použít pro přístup k datům, autorizaci a další otázky straně serveru.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="4d9cf-129">Reagují aplikace, které se nacházejí v *ClientApp* podadresáři, je určena k použití pro všechny otázky uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="4d9cf-130">Přidání stránky, obrázky, styly, moduly, atd.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="4d9cf-131">*ClientApp* adresář je standardní CRA reagovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="4d9cf-132">Najdete v oficiální [CRA dokumentaci](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="4d9cf-133">Jsou drobné rozdíly mezi reagují aplikace vytvořené pomocí této šablony a jeden vytvořené CRA sama o sobě; schopnosti aplikace jsou však beze změny.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="4d9cf-134">Obsahuje aplikace vytvořené pomocí šablony [Bootstrap](https://getbootstrap.com/)– na základě rozložení a základní příklad směrování.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="4d9cf-135">Instalovat balíčky npm</span><span class="sxs-lookup"><span data-stu-id="4d9cf-135">Install npm packages</span></span>

<span data-ttu-id="4d9cf-136">Chcete-li instalovat balíčky jiných výrobců npm, použijte na příkazovém řádku ve *ClientApp* podadresáři.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="4d9cf-137">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4d9cf-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="4d9cf-138">Publikování a nasazení</span><span class="sxs-lookup"><span data-stu-id="4d9cf-138">Publish and deploy</span></span>

<span data-ttu-id="4d9cf-139">Ve vývoji aplikace běží v režimu optimalizovaná pro vývojáře pohodlí.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="4d9cf-140">Například JavaScript sady zahrnují zdroj mapy (tak, aby při ladění, zobrazí se původní zdrojový kód).</span><span class="sxs-lookup"><span data-stu-id="4d9cf-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="4d9cf-141">Aplikace sleduje JavaScript, HTML a CSS změny souborů na disku a automaticky znovu zkompiluje a znovu načte, jakmile je detekována tyto soubory změnit.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="4d9cf-142">V produkčním prostředí sloužit verzi vaší aplikace, která je optimalizovaná pro výkon.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-142">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="4d9cf-143">Ta se nakonfiguruje automaticky provést.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-143">This is configured to happen automatically.</span></span> <span data-ttu-id="4d9cf-144">Když publikujete, je konfigurace sestavení vysílá minifikovaný, sestavení transpiled kódu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="4d9cf-145">Na rozdíl od sestavení vývoj nevyžaduje produkční build Node.js k instalaci na serveru.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="4d9cf-146">Můžete použít standardní [metody nasazení a hostování ASP.NET Core](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="4d9cf-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="4d9cf-147">Spusťte CRA server nezávisle</span><span class="sxs-lookup"><span data-stu-id="4d9cf-147">Run the CRA server independently</span></span>

<span data-ttu-id="4d9cf-148">Projekt je nakonfigurován na spuštění svou vlastní instanci vývojový server CRA na pozadí při spuštění v režimu pro vývoj aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="4d9cf-149">To je výhodné, protože znamená to, že nemusíte ručně spustit samostatný server.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="4d9cf-150">Není nevýhodou toto výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-150">There's a drawback to this default setup.</span></span> <span data-ttu-id="4d9cf-151">Pokaždé, když upravíte kódu C# a vaše ASP.NET Core aplikace potřebuje k restartování, CRA server se restartuje.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="4d9cf-152">Několik sekund jsou nutné ke spuštění zálohování.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="4d9cf-153">Pokud provádíme za časté úpravy kódu jazyka C# a nechcete čekat CRA server restartovat, spusťte server CRA externě, nezávisle na procesu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="4d9cf-154">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="4d9cf-154">To do so:</span></span>

1. <span data-ttu-id="4d9cf-155">V příkazovém řádku přepnout *ClientApp* podadresáři a spouštět CRA vývojový server:</span><span class="sxs-lookup"><span data-stu-id="4d9cf-155">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

2. <span data-ttu-id="4d9cf-156">Upravte v aplikaci ASP.NET Core použít externí instance serveru CRA místo spuštění jeho vlastní.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-156">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="4d9cf-157">Ve vaší *spuštění* třídy, nahraďte `spa.UseReactDevelopmentServer` volání následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="4d9cf-157">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="4d9cf-158">Při spuštění aplikace ASP.NET Core nespustí CRA serveru.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-158">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="4d9cf-159">Instance, kterou jste spustili ručně bude místo něj použita.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-159">The instance you started manually is used instead.</span></span> <span data-ttu-id="4d9cf-160">To umožňuje spustit a restartovat rychlejší.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-160">This enables it to start and restart faster.</span></span> <span data-ttu-id="4d9cf-161">Se už čeká aplikace reagují pokaždé, když znovu sestavit.</span><span class="sxs-lookup"><span data-stu-id="4d9cf-161">It's no longer waiting for your React app to rebuild each time.</span></span>
