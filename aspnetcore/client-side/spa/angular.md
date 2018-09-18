---
title: Šablona projektu Angular s ASP.NET Core
author: SteveSandersonMS
description: Zjistěte, jak začít pracovat se šablonou projektu ASP.NET Core jedné stránky aplikace (SPA) zaujetí pro Angular a Angular CLI.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/angular
ms.openlocfilehash: 763b4fff7c64432328af660c66e6ff3f8f697f71
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011351"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a><span data-ttu-id="61150-103">Šablona projektu Angular s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61150-103">Use the Angular project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="61150-104">Tato dokumentace se o šablona projektu Angular součástí ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="61150-104">This documentation isn't about the Angular project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="61150-105">Jde o novější šablony Angular, do kterého můžete ručně aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="61150-105">It's about the newer Angular template to which you can update manually.</span></span> <span data-ttu-id="61150-106">Šablona je zahrnuta v ASP.NET Core 2.1 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="61150-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="61150-107">Šablona projektu aktualizované Angular poskytuje příhodný výchozí bod pro ASP.NET Core pomocí Angular a Angular CLI k implementaci bohatě vybaveným a na straně klienta uživatelské rozhraní (UI) aplikace.</span><span class="sxs-lookup"><span data-stu-id="61150-107">The updated Angular project template provides a convenient starting point for ASP.NET Core apps using Angular and the Angular CLI to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="61150-108">Šablona je ekvivalentní k vytváření projektu aplikace ASP.NET Core tak, aby fungoval jako back-endu rozhraní API a projekt Angular CLI tak, aby fungoval jako uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="61150-108">The template is equivalent to creating an ASP.NET Core project to act as an API backend and an Angular CLI project to act as a UI.</span></span> <span data-ttu-id="61150-109">Šablona nabízí praktické hostování oba typy projektů v projektu aplikace s jedním.</span><span class="sxs-lookup"><span data-stu-id="61150-109">The template offers the convenience of hosting both project types in a single app project.</span></span> <span data-ttu-id="61150-110">V důsledku toho projekt aplikace dají vytvořit a publikovat jako jeden celek.</span><span class="sxs-lookup"><span data-stu-id="61150-110">Consequently, the app project can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="61150-111">Vytvoření nové aplikace</span><span class="sxs-lookup"><span data-stu-id="61150-111">Create a new app</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="61150-112">Pokud používáte ASP.NET Core 2.0, zkontrolujte, že máte [nainstalované aktualizovaná šablona projektu React](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="61150-112">If using ASP.NET Core 2.0, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="61150-113">Pokud máte ASP.NET Core 2.1 nainstalovaný, není nutné k instalaci šablona projektu Angular.</span><span class="sxs-lookup"><span data-stu-id="61150-113">If you have ASP.NET Core 2.1 installed, there's no need to install the Angular project template.</span></span>

::: moniker-end

<span data-ttu-id="61150-114">Vytvoření nového projektu z příkazového řádku pomocí příkazu `dotnet new angular` v prázdném adresáři.</span><span class="sxs-lookup"><span data-stu-id="61150-114">Create a new project from a command prompt using the command `dotnet new angular` in an empty directory.</span></span> <span data-ttu-id="61150-115">Například následující příkazy vytvoří aplikaci *my nové app* adresáře a přepnete se do tohoto adresáře:</span><span class="sxs-lookup"><span data-stu-id="61150-115">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new angular -o my-new-app
cd my-new-app
```

<span data-ttu-id="61150-116">Spuštění aplikace Visual Studio nebo .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="61150-116">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61150-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61150-117">Visual Studio</span></span>](#tab/visual-studio/)

<span data-ttu-id="61150-118">Otevřete vygenerovaný *.csproj* souboru a spuštění aplikace jako za normálních okolností z něj.</span><span class="sxs-lookup"><span data-stu-id="61150-118">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="61150-119">Proces sestavení obnoví závislosti npm při prvním spuštění, což může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="61150-119">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="61150-120">Následující sestavení jsou mnohem rychlejší.</span><span class="sxs-lookup"><span data-stu-id="61150-120">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="61150-121">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="61150-121">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="61150-122">Ujistěte se, máte proměnnou prostředí volá `ASPNETCORE_Environment` s hodnotou `Development`.</span><span class="sxs-lookup"><span data-stu-id="61150-122">Ensure you have an environment variable called `ASPNETCORE_Environment` with a value of `Development`.</span></span> <span data-ttu-id="61150-123">Na Windows (v výzev – prostředí PowerShell), spusťte `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="61150-123">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="61150-124">V systému macOS nebo Linux spusťte `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="61150-124">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="61150-125">Spustit [dotnet sestavení](/dotnet/core/tools/dotnet-build) správně k ověření aplikace sestavena.</span><span class="sxs-lookup"><span data-stu-id="61150-125">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify the app builds correctly.</span></span> <span data-ttu-id="61150-126">Při prvním spuštění procesu sestavení obnoví závislosti na npm, což může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="61150-126">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="61150-127">Následující sestavení jsou mnohem rychlejší.</span><span class="sxs-lookup"><span data-stu-id="61150-127">Subsequent builds are much faster.</span></span>

<span data-ttu-id="61150-128">Spustit [dotnet spustit](/dotnet/core/tools/dotnet-run) a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="61150-128">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span> <span data-ttu-id="61150-129">Bude zaznamenána zpráva podobná této:</span><span class="sxs-lookup"><span data-stu-id="61150-129">A message similar to the following is logged:</span></span>

```console
Now listening on: http://localhost:<port>
```

<span data-ttu-id="61150-130">Přejděte na tuto adresu URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="61150-130">Navigate to this URL in a browser.</span></span>

<span data-ttu-id="61150-131">Spuštění aplikace instance serveru Angular CLI na pozadí.</span><span class="sxs-lookup"><span data-stu-id="61150-131">The app starts up an instance of the Angular CLI server in the background.</span></span> <span data-ttu-id="61150-132">Bude zaznamenána zpráva podobná následující: *NG Live vývojový Server naslouchá na localhost:&lt;otherport&gt;, otevřete prohlížeč na http://localhost:&lt; otherport&gt; /*  .</span><span class="sxs-lookup"><span data-stu-id="61150-132">A message similar to the following is logged: *NG Live Development Server is listening on localhost:&lt;otherport&gt;, open your browser on http://localhost:&lt;otherport&gt;/*.</span></span> <span data-ttu-id="61150-133">Tuto zprávu ignorovat&mdash;má **není** adresu URL pro kombinované aplikace ASP.NET Core a Angular CLI.</span><span class="sxs-lookup"><span data-stu-id="61150-133">Ignore this message&mdash;it's **not** the URL for the combined ASP.NET Core and Angular CLI app.</span></span>

---

<span data-ttu-id="61150-134">Šablona projektu vytvoří aplikace ASP.NET Core a aplikaci Angular.</span><span class="sxs-lookup"><span data-stu-id="61150-134">The project template creates an ASP.NET Core app and an Angular app.</span></span> <span data-ttu-id="61150-135">Aplikace ASP.NET Core je určena pro použití pro přístup k datům, autorizaci a další aspekty na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="61150-135">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="61150-136">Aplikaci Angular, které se nacházejí v *ClientApp* podadresář, je určena pro použití pro všechny aspekty uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="61150-136">The Angular app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="61150-137">Přidání stránek, obrázků, styly, moduly, atd.</span><span class="sxs-lookup"><span data-stu-id="61150-137">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="61150-138">*ClientApp* adresář obsahuje standardní aplikaci Angular CLI.</span><span class="sxs-lookup"><span data-stu-id="61150-138">The *ClientApp* directory contains a standard Angular CLI app.</span></span> <span data-ttu-id="61150-139">Najdete v oficiální [Angular dokumentaci](https://github.com/angular/angular-cli/wiki) Další informace.</span><span class="sxs-lookup"><span data-stu-id="61150-139">See the official [Angular documentation](https://github.com/angular/angular-cli/wiki) for more information.</span></span>

<span data-ttu-id="61150-140">Existují mírné rozdíly mezi aplikaci Angular, které jsou vytvořené pomocí této šablony a vytvořeny pomocí Angular CLI (prostřednictvím `ng new`); nicméně jsou funkcí aplikace beze změny.</span><span class="sxs-lookup"><span data-stu-id="61150-140">There are slight differences between the Angular app created by this template and the one created by Angular CLI itself (via `ng new`); however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="61150-141">Obsahuje aplikaci vytvořenou pomocí šablony [Bootstrap](https://getbootstrap.com/)– na základě rozložení a základní příklad směrování.</span><span class="sxs-lookup"><span data-stu-id="61150-141">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="run-ng-commands"></a><span data-ttu-id="61150-142">Spusťte příkazy ng</span><span class="sxs-lookup"><span data-stu-id="61150-142">Run ng commands</span></span>

<span data-ttu-id="61150-143">V příkazovém řádku přejděte *ClientApp* podadresáři:</span><span class="sxs-lookup"><span data-stu-id="61150-143">In a command prompt, switch to the *ClientApp* subdirectory:</span></span>

```console
cd ClientApp
```

<span data-ttu-id="61150-144">Pokud máte `ng` globálně nainstalovaný nástroj, můžete spustit některý z jeho příkazů.</span><span class="sxs-lookup"><span data-stu-id="61150-144">If you have the `ng` tool installed globally, you can run any of its commands.</span></span> <span data-ttu-id="61150-145">Například můžete spustit `ng lint`, `ng test`, ani žádný z nich [Angular CLI příkazy](https://github.com/angular/angular-cli/wiki#additional-commands).</span><span class="sxs-lookup"><span data-stu-id="61150-145">For example, you can run `ng lint`, `ng test`, or any of the other [Angular CLI commands](https://github.com/angular/angular-cli/wiki#additional-commands).</span></span> <span data-ttu-id="61150-146">Není nutné ke spuštění `ng serve` , protože vaše aplikace ASP.NET Core se zabývá obsluhující serverové a klientské součásti vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="61150-146">There's no need to run `ng serve` though, because your ASP.NET Core app deals with serving both server-side and client-side parts of your app.</span></span> <span data-ttu-id="61150-147">Interně používá `ng serve` ve vývoji.</span><span class="sxs-lookup"><span data-stu-id="61150-147">Internally, it uses `ng serve` in development.</span></span>

<span data-ttu-id="61150-148">Pokud nemáte k dispozici `ng` nástroj nainstalovali, spusťte `npm run ng` místo.</span><span class="sxs-lookup"><span data-stu-id="61150-148">If you don't have the `ng` tool installed, run `npm run ng` instead.</span></span> <span data-ttu-id="61150-149">Například můžete spustit `npm run ng lint` nebo `npm run ng test`.</span><span class="sxs-lookup"><span data-stu-id="61150-149">For example, you can run `npm run ng lint` or `npm run ng test`.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="61150-150">Instalace balíčků npm</span><span class="sxs-lookup"><span data-stu-id="61150-150">Install npm packages</span></span>

<span data-ttu-id="61150-151">Pokud chcete nainstalovat balíčky npm třetích stran, použijte příkazový řádek v *ClientApp* podadresáře.</span><span class="sxs-lookup"><span data-stu-id="61150-151">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="61150-152">Příklad:</span><span class="sxs-lookup"><span data-stu-id="61150-152">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="61150-153">Publikování a nasazení</span><span class="sxs-lookup"><span data-stu-id="61150-153">Publish and deploy</span></span>

<span data-ttu-id="61150-154">Při vývoji se aplikace běží v režimu optimalizované pro usnadnění práce vývojářů.</span><span class="sxs-lookup"><span data-stu-id="61150-154">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="61150-155">Například jazyka JavaScript sady obsahují zdrojových mapování (tak, aby při ladění, zobrazí se váš původním kód TypeScript).</span><span class="sxs-lookup"><span data-stu-id="61150-155">For example, JavaScript bundles include source maps (so that when debugging, you can see your original TypeScript code).</span></span> <span data-ttu-id="61150-156">Aplikace sleduje TypeScript, HTML a CSS změny souborů na disku a automaticky se znovu zkompiluje a znovu načte, když vidí tyto soubory změnit.</span><span class="sxs-lookup"><span data-stu-id="61150-156">The app watches for TypeScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="61150-157">V produkčním prostředí sloužit verzi vaší aplikace, které je optimalizované pro výkon.</span><span class="sxs-lookup"><span data-stu-id="61150-157">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="61150-158">Ta se nakonfiguruje, která se provede automaticky.</span><span class="sxs-lookup"><span data-stu-id="61150-158">This is configured to happen automatically.</span></span> <span data-ttu-id="61150-159">Když publikujete, konfigurace sestavení generuje minifikovaný, ahead-of-time (AoT) zkompilován sestavení kódu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="61150-159">When you publish, the build configuration emits a minified, ahead-of-time (AoT) compiled build of your client-side code.</span></span> <span data-ttu-id="61150-160">Na rozdíl od sestavení vývoj nevyžaduje produkční build Node.js nainstalovaný na serveru (Pokud jste povolili [dokončení fáze před vykreslením na straně serveru](#server-side-rendering)).</span><span class="sxs-lookup"><span data-stu-id="61150-160">Unlike the development build, the production build doesn't require Node.js to be installed on the server (unless you have enabled [server-side prerendering](#server-side-rendering)).</span></span>

<span data-ttu-id="61150-161">Můžete použít standardní [metody hostování a nasazení ASP.NET Core](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="61150-161">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-ng-serve-independently"></a><span data-ttu-id="61150-162">Spuštění "ng slouží" nezávisle na sobě</span><span class="sxs-lookup"><span data-stu-id="61150-162">Run "ng serve" independently</span></span>

<span data-ttu-id="61150-163">Projekt je nakonfigurován ke spuštění svoji vlastní instanci serveru Angular CLI na pozadí při spuštění v režimu pro vývoj aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61150-163">The project is configured to start its own instance of the Angular CLI server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="61150-164">To je vhodné, protože není nutné ručně spustit na samostatný server.</span><span class="sxs-lookup"><span data-stu-id="61150-164">This is convenient because you don't have to run a separate server manually.</span></span>

<span data-ttu-id="61150-165">Nevýhodou této výchozí nastavení není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="61150-165">There's a drawback to this default setup.</span></span> <span data-ttu-id="61150-166">Pokaždé, když změníte kód jazyka C# a vaše ASP.NET Core, které aplikace potřebuje k restartování, Angular CLI server se restartuje.</span><span class="sxs-lookup"><span data-stu-id="61150-166">Each time you modify your C# code and your ASP.NET Core app needs to restart, the Angular CLI server restarts.</span></span> <span data-ttu-id="61150-167">Přibližně 10 sekund je potřebná ke spuštění zálohování.</span><span class="sxs-lookup"><span data-stu-id="61150-167">Around 10 seconds is required to start back up.</span></span> <span data-ttu-id="61150-168">Pokud vytváříte časté úpravy kódu jazyka C# a nechcete čekat na restartování, Angular CLI spustit server Angular CLI externě, nezávisle na procesu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61150-168">If you're making frequent C# code edits and don't want to wait for Angular CLI to restart, run the Angular CLI server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="61150-169">Postup:</span><span class="sxs-lookup"><span data-stu-id="61150-169">To do so:</span></span>

1. <span data-ttu-id="61150-170">V příkazovém řádku přejděte *ClientApp* podadresáře a spusťte vývojový server sady Angular CLI:</span><span class="sxs-lookup"><span data-stu-id="61150-170">In a command prompt, switch to the *ClientApp* subdirectory, and launch the Angular CLI development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="61150-171">Použití `npm start` nelze spustit vývojový server Angular CLI, `ng serve`tak, aby konfigurace v *package.json* je dodržena.</span><span class="sxs-lookup"><span data-stu-id="61150-171">Use `npm start` to launch the Angular CLI development server, not `ng serve`, so that the configuration in *package.json* is respected.</span></span> <span data-ttu-id="61150-172">Chcete-li předat další parametry serveru Angular CLI, přidejte ho do příslušné `scripts` řádku v vaše *package.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="61150-172">To pass additional parameters to the Angular CLI server, add them to the relevant `scripts` line in your *package.json* file.</span></span>

2. <span data-ttu-id="61150-173">Upravte aplikace ASP.NET Core pro použití namísto spuštění jednu vlastní instance externího Angular CLI.</span><span class="sxs-lookup"><span data-stu-id="61150-173">Modify your ASP.NET Core app to use the external Angular CLI instance instead of launching one of its own.</span></span> <span data-ttu-id="61150-174">Ve vaší *spuštění* třídy, nahraďte `spa.UseAngularCliServer` vyvolání následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="61150-174">In your *Startup* class, replace the `spa.UseAngularCliServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

<span data-ttu-id="61150-175">Při spuštění aplikace ASP.NET Core se nespustí serveru Angular CLI.</span><span class="sxs-lookup"><span data-stu-id="61150-175">When you start your ASP.NET Core app, it won't launch an Angular CLI server.</span></span> <span data-ttu-id="61150-176">Místo toho se používá instanci, kterou jste spustili ručně.</span><span class="sxs-lookup"><span data-stu-id="61150-176">The instance you started manually is used instead.</span></span> <span data-ttu-id="61150-177">To umožňuje spuštění a restartování rychleji.</span><span class="sxs-lookup"><span data-stu-id="61150-177">This enables it to start and restart faster.</span></span> <span data-ttu-id="61150-178">Se už čeká Angular CLI sestavení pokaždé, když klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="61150-178">It's no longer waiting for Angular CLI to rebuild your client app each time.</span></span>

## <a name="server-side-rendering"></a><span data-ttu-id="61150-179">Vykreslování na straně serveru</span><span class="sxs-lookup"><span data-stu-id="61150-179">Server-side rendering</span></span>

<span data-ttu-id="61150-180">Jako funkce výkonu můžete předem vygenerované aplikaci Angular na serveru, jakož i v klientovi spuštěna.</span><span class="sxs-lookup"><span data-stu-id="61150-180">As a performance feature, you can choose to pre-render your Angular app on the server as well as running it on the client.</span></span> <span data-ttu-id="61150-181">To znamená, že prohlížeč obdrží značka jazyka HTML představující počáteční Uživatelském rozhraní aplikace, takže se zobrazí i před stažením a spouští vaše sady JavaScript.</span><span class="sxs-lookup"><span data-stu-id="61150-181">This means that browsers receive HTML markup representing your app's initial UI, so they display it even before downloading and executing your JavaScript bundles.</span></span> <span data-ttu-id="61150-182">Implementaci této pochází z Angular funkci s názvem [Angular univerzální](https://universal.angular.io/).</span><span class="sxs-lookup"><span data-stu-id="61150-182">Most of the implementation of this comes from an Angular feature called [Angular Universal](https://universal.angular.io/).</span></span>

> [!TIP]
> <span data-ttu-id="61150-183">Povoluje vykreslování na straně serveru (SSR) představuje celou řadou dalších komplikace i při vývoji a nasazení.</span><span class="sxs-lookup"><span data-stu-id="61150-183">Enabling server-side rendering (SSR) introduces a number of extra complications both during development and deployment.</span></span> <span data-ttu-id="61150-184">Čtení [nevýhody SSR](#drawbacks-of-ssr) k určení, zda SSR je vhodné pro vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="61150-184">Read [drawbacks of SSR](#drawbacks-of-ssr) to determine if SSR is a good fit for your requirements.</span></span>

<span data-ttu-id="61150-185">Povolit SSR, budete muset vytvořit počet přidání do projektu.</span><span class="sxs-lookup"><span data-stu-id="61150-185">To enable SSR, you need to make a number of additions to your project.</span></span>

<span data-ttu-id="61150-186">V *spuštění* třídy *po* řádku, který konfiguruje `spa.Options.SourcePath`, a *před* volání `UseAngularCliServer` nebo `UseProxyToSpaDevelopmentServer`, přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="61150-186">In the *Startup* class, *after* the line that configures `spa.Options.SourcePath`, and *before* the call to `UseAngularCliServer` or `UseProxyToSpaDevelopmentServer`, add the following:</span></span>

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

<span data-ttu-id="61150-187">V režimu pro vývoj, tento kód se pokouší sestavení sady SSR spuštěním skriptu `build:ssr`, který je definován v *ClientApp\package.json*.</span><span class="sxs-lookup"><span data-stu-id="61150-187">In development mode, this code attempts to build the SSR bundle by running the script `build:ssr`, which is defined in *ClientApp\package.json*.</span></span> <span data-ttu-id="61150-188">Tento postup sestaví aplikaci Angular s názvem `ssr`, která ještě není definovaná.</span><span class="sxs-lookup"><span data-stu-id="61150-188">This builds an Angular app named `ssr`, which isn't yet defined.</span></span>

<span data-ttu-id="61150-189">Na konci `apps` obsahuje pole *ClientApp/.angular-cli.json*, definovat další aplikace s názvem `ssr`.</span><span class="sxs-lookup"><span data-stu-id="61150-189">At the end of the `apps` array in *ClientApp/.angular-cli.json*, define an extra app with name `ssr`.</span></span> <span data-ttu-id="61150-190">Pomocí následujících možností:</span><span class="sxs-lookup"><span data-stu-id="61150-190">Use the following options:</span></span>

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

<span data-ttu-id="61150-191">Tato nová konfigurace aplikace s podporou SSR vyžaduje dva další soubory: *tsconfig.server.json* a *main.server.ts*.</span><span class="sxs-lookup"><span data-stu-id="61150-191">This new SSR-enabled app configuration requires two further files: *tsconfig.server.json* and *main.server.ts*.</span></span> <span data-ttu-id="61150-192">*Tsconfig.server.json* soubor Určuje možnosti kompilace TypeScript.</span><span class="sxs-lookup"><span data-stu-id="61150-192">The *tsconfig.server.json* file specifies TypeScript compilation options.</span></span> <span data-ttu-id="61150-193">*Main.server.ts* souboru slouží jako vstupní bod kódu během SSR.</span><span class="sxs-lookup"><span data-stu-id="61150-193">The *main.server.ts* file serves as the code entry point during SSR.</span></span>

<span data-ttu-id="61150-194">Přidat nový soubor s názvem *tsconfig.server.json* uvnitř *ClientApp/src* (spolu s existující *tsconfig.app.json*), která obsahuje následující:</span><span class="sxs-lookup"><span data-stu-id="61150-194">Add a new file called *tsconfig.server.json* inside *ClientApp/src* (alongside the existing *tsconfig.app.json*), containing the following:</span></span>

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

<span data-ttu-id="61150-195">Tento soubor nastaví pro Angular kompilátor AoT vyhledejte modul s názvem `app.server.module`.</span><span class="sxs-lookup"><span data-stu-id="61150-195">This file configures Angular's AoT compiler to look for a module called `app.server.module`.</span></span> <span data-ttu-id="61150-196">Přidat tak, že vytvoříte nový soubor na *ClientApp/src/app/app.server.module.ts* (spolu s existující *app.module.ts*) obsahující následující:</span><span class="sxs-lookup"><span data-stu-id="61150-196">Add this by creating a new file at *ClientApp/src/app/app.server.module.ts* (alongside the existing *app.module.ts*) containing the following:</span></span>

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

<span data-ttu-id="61150-197">Tento modul se dědí z vašeho klienta `app.module` a definuje, které velmi Angular moduly jsou k dispozici během SSR.</span><span class="sxs-lookup"><span data-stu-id="61150-197">This module inherits from your client-side `app.module` and defines which extra Angular modules are available during SSR.</span></span>

<span data-ttu-id="61150-198">Vzpomeňte si, že nový `ssr` záznam v *.angular cli.json* odkazovaný soubor vstupního bodu s názvem *main.server.ts*.</span><span class="sxs-lookup"><span data-stu-id="61150-198">Recall that the new `ssr` entry in *.angular-cli.json* referenced an entry point file called *main.server.ts*.</span></span> <span data-ttu-id="61150-199">Ještě nepřidali tento soubor a nyní je čas Uděláte to tak.</span><span class="sxs-lookup"><span data-stu-id="61150-199">You haven't yet added that file, and now is time to do so.</span></span> <span data-ttu-id="61150-200">Vytvořte nový soubor na *ClientApp/src/main.server.ts* (spolu s existující *main.ts*), která obsahuje následující:</span><span class="sxs-lookup"><span data-stu-id="61150-200">Create a new file at *ClientApp/src/main.server.ts* (alongside the existing *main.ts*), containing the following:</span></span>

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

<span data-ttu-id="61150-201">Tento soubor kódu je co ASP.NET Core provádí pro každý požadavek při spuštění `UseSpaPrerendering` middleware, který jste přidali do *spuštění* třídy.</span><span class="sxs-lookup"><span data-stu-id="61150-201">This file's code is what ASP.NET Core executes for each request when it runs the `UseSpaPrerendering` middleware that you added to the *Startup* class.</span></span> <span data-ttu-id="61150-202">Se zabývá přijímají `params` z kódu .NET (jako je například adresa URL žádá) a volání rozhraní API pro Angular SSR zobrazíte výsledného souboru HTML.</span><span class="sxs-lookup"><span data-stu-id="61150-202">It deals with receiving `params` from the .NET code (such as the URL being requested), and making calls to Angular SSR APIs to get the resulting HTML.</span></span>

<span data-ttu-id="61150-203">Přísně anglicky mluvícího, to stačí pro povolení SSR ve vývojovém režimu.</span><span class="sxs-lookup"><span data-stu-id="61150-203">Strictly-speaking, this is sufficient to enable SSR in development mode.</span></span> <span data-ttu-id="61150-204">Je nezbytné, aby jeden poslední změny tak, aby vaše aplikace funguje správně při publikování.</span><span class="sxs-lookup"><span data-stu-id="61150-204">It's essential to make one final change so that your app works correctly when published.</span></span> <span data-ttu-id="61150-205">V hlavní aplikaci prvku *.csproj* souboru `BuildServerSideRenderer` hodnoty vlastnosti `true`:</span><span class="sxs-lookup"><span data-stu-id="61150-205">In your app's main *.csproj* file, set the `BuildServerSideRenderer` property value to `true`:</span></span>

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

<span data-ttu-id="61150-206">Tím se nakonfiguruje proces sestavení prováděn `build:ssr` během publikování a nasazení souborů SSR k serveru.</span><span class="sxs-lookup"><span data-stu-id="61150-206">This configures the build process to run `build:ssr` during publishing and deploy the SSR files to the server.</span></span> <span data-ttu-id="61150-207">Pokud nechcete povolit, SSR selže v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="61150-207">If you don't enable this, SSR fails in production.</span></span>

<span data-ttu-id="61150-208">Pokud vaše aplikace běží v režimu vývojové nebo produkční prostředí, kód Angular předem vykreslí jako kódu HTML na serveru.</span><span class="sxs-lookup"><span data-stu-id="61150-208">When your app runs in either development or production mode, the Angular code pre-renders as HTML on the server.</span></span> <span data-ttu-id="61150-209">Spustí kód na straně klienta jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="61150-209">The client-side code executes as normal.</span></span>

### <a name="pass-data-from-net-code-into-typescript-code"></a><span data-ttu-id="61150-210">Předání dat z kódu .NET do kódu TypeScript</span><span class="sxs-lookup"><span data-stu-id="61150-210">Pass data from .NET code into TypeScript code</span></span>

<span data-ttu-id="61150-211">Během SSR může být vhodné k předávání dat na žádost z vaší aplikace ASP.NET Core do aplikace pro Angular.</span><span class="sxs-lookup"><span data-stu-id="61150-211">During SSR, you might want to pass per-request data from your ASP.NET Core app into your Angular app.</span></span> <span data-ttu-id="61150-212">Například může předat informace o souboru cookie nebo něco čtení z databáze.</span><span class="sxs-lookup"><span data-stu-id="61150-212">For example, you could pass cookie information or something read from a database.</span></span> <span data-ttu-id="61150-213">Chcete-li to provést, upravte vaše *spuštění* třídy.</span><span class="sxs-lookup"><span data-stu-id="61150-213">To do this, edit your *Startup* class.</span></span> <span data-ttu-id="61150-214">Při zpětném volání pro `UseSpaPrerendering`, nastavte hodnotu pro `options.SupplyData` jako je následující:</span><span class="sxs-lookup"><span data-stu-id="61150-214">In the callback for `UseSpaPrerendering`, set a value for `options.SupplyData` such as the following:</span></span>

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

<span data-ttu-id="61150-215">`SupplyData` Zpětného volání vám umožní předat libovolný jednotlivých žádostí, serializovat JSON data (například řetězce, logické hodnoty nebo číslice).</span><span class="sxs-lookup"><span data-stu-id="61150-215">The `SupplyData` callback lets you pass arbitrary, per-request, JSON-serializable data (for example, strings, booleans, or numbers).</span></span> <span data-ttu-id="61150-216">Vaše *main.server.ts* kód přijímá jako `params.data`.</span><span class="sxs-lookup"><span data-stu-id="61150-216">Your *main.server.ts* code receives this as `params.data`.</span></span> <span data-ttu-id="61150-217">Například předchozí příklad kódu předá hodnotu typu boolean jako `params.data.isHttpsRequest` do `createServerRenderer` zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="61150-217">For example, the preceding code sample passes a boolean value as `params.data.isHttpsRequest` into the `createServerRenderer` callback.</span></span> <span data-ttu-id="61150-218">To můžete předat do jiné části aplikace žádným způsobem podporuje Angular.</span><span class="sxs-lookup"><span data-stu-id="61150-218">You can pass this to other parts of your app in any way supported by Angular.</span></span> <span data-ttu-id="61150-219">Třeba zjistit, jak *main.server.ts* předává `BASE_URL` hodnotu pro všechny součásti, jejíž konstruktor je deklarované ho přijímat pomocí.</span><span class="sxs-lookup"><span data-stu-id="61150-219">For example, see how *main.server.ts* passes the `BASE_URL` value to any component whose constructor is declared to receive it.</span></span>

### <a name="drawbacks-of-ssr"></a><span data-ttu-id="61150-220">Nevýhody SSR</span><span class="sxs-lookup"><span data-stu-id="61150-220">Drawbacks of SSR</span></span>

<span data-ttu-id="61150-221">Ne všechny aplikace s výhodou SSR.</span><span class="sxs-lookup"><span data-stu-id="61150-221">Not all apps benefit from SSR.</span></span> <span data-ttu-id="61150-222">Primární výhoda je vnímaný výkon.</span><span class="sxs-lookup"><span data-stu-id="61150-222">The primary benefit is perceived performance.</span></span> <span data-ttu-id="61150-223">Návštěvníci dosáhnout aplikace přes pomalé připojení k síti nebo na mobilních zařízeních pomalé počáteční uživatelského rozhraní zobrazovat rychle, i v případě, že trvá při načtení nebo parsovat sady JavaScript.</span><span class="sxs-lookup"><span data-stu-id="61150-223">Visitors reaching your app over a slow network connection or on slow mobile devices see the initial UI quickly, even if it takes a while to fetch or parse the JavaScript bundles.</span></span> <span data-ttu-id="61150-224">Ale mnoho SPA slouží především přes rychlé, interní firemní sítě na počítačích rychlé kde se zobrazí aplikace, téměř okamžitě.</span><span class="sxs-lookup"><span data-stu-id="61150-224">However, many SPAs are mainly used over fast, internal company networks on fast computers where the app appears almost instantly.</span></span>

<span data-ttu-id="61150-225">Ve stejnou dobu jsou významné nevýhod povolení SSR.</span><span class="sxs-lookup"><span data-stu-id="61150-225">At the same time, there are significant drawbacks to enabling SSR.</span></span> <span data-ttu-id="61150-226">To zvyšuje složitost vašeho vývojového procesu.</span><span class="sxs-lookup"><span data-stu-id="61150-226">It adds complexity to your development process.</span></span> <span data-ttu-id="61150-227">Váš kód musí běžet ve dvou různých prostředích: na straně klienta i stranu serveru (v prostředí Node.js vyvolat pomocí ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="61150-227">Your code must run in two different environments: client-side and server-side (in a Node.js environment invoked from ASP.NET Core).</span></span> <span data-ttu-id="61150-228">Tady jsou některé věci k berte v úvahu:</span><span class="sxs-lookup"><span data-stu-id="61150-228">Here are some things to bear in mind:</span></span>

* <span data-ttu-id="61150-229">SSR vyžaduje instalaci Node.js na provozních serverech.</span><span class="sxs-lookup"><span data-stu-id="61150-229">SSR requires a Node.js installation on your production servers.</span></span> <span data-ttu-id="61150-230">Toto je automaticky případ pro některé scénáře nasazení, jako je například Azure App Services, ale ne pro jiné, jako je Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="61150-230">This is automatically the case for some deployment scenarios, such as Azure App Services, but not for others, such as Azure Service Fabric.</span></span>
* <span data-ttu-id="61150-231">Povolení `BuildServerSideRenderer` sestavení příznak způsobí, že vaše *node_modules* directory k publikování.</span><span class="sxs-lookup"><span data-stu-id="61150-231">Enabling the `BuildServerSideRenderer` build flag causes your *node_modules* directory to publish.</span></span> <span data-ttu-id="61150-232">Tato složka obsahuje 20 000 + soubory, které zvýší dobu nasazení.</span><span class="sxs-lookup"><span data-stu-id="61150-232">This folder contains 20,000+ files, which increases deployment time.</span></span>
* <span data-ttu-id="61150-233">Ke spuštění kódu v prostředí Node.js, se nelze spoléhat na existenci specifické pro prohlížeč rozhraní API jazyka JavaScript, jako `window` nebo `localStorage`.</span><span class="sxs-lookup"><span data-stu-id="61150-233">To run your code in a Node.js environment, it can't rely on the existence of browser-specific JavaScript APIs such as `window` or `localStorage`.</span></span> <span data-ttu-id="61150-234">Pokud váš kód (nebo některé knihovny třetí strany, který odkazujete) pokusí o pomocí těchto rozhraní API, budete při SSR dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="61150-234">If your code (or some third-party library you reference) tries to use these APIs, you'll get an error during SSR.</span></span> <span data-ttu-id="61150-235">Například nepoužívejte jQuery protože odkazuje na rozhraní API pro konkrétní prohlížeče na mnoha místech.</span><span class="sxs-lookup"><span data-stu-id="61150-235">For example, don't use jQuery because it references browser-specific APIs in many places.</span></span> <span data-ttu-id="61150-236">Chcete-li zabránit chybám, musíte vyhnout SSR nebo Vyhněte se rozhraní API nebo knihovny specifické pro prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="61150-236">To prevent errors, you must either avoid SSR or avoid browser-specific APIs or libraries.</span></span> <span data-ttu-id="61150-237">Všechna volání do těchto rozhraní API můžete zabalit do kontroly pro zajištění, že nejsou vyvolány během SSR.</span><span class="sxs-lookup"><span data-stu-id="61150-237">You can wrap any calls to such APIs in checks to ensure they aren't invoked during SSR.</span></span> <span data-ttu-id="61150-238">Například v kódu jazyka JavaScript nebo TypeScript použijte kontrolu, jako je následující:</span><span class="sxs-lookup"><span data-stu-id="61150-238">For example, use a check such as the following in JavaScript or TypeScript code:</span></span>

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
