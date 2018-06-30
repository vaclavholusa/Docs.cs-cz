---
title: Jádro ASP.NET SignalR pomocí TypeScript a Webpack
author: ssougnez
description: V tomto kurzu nakonfigurujete Webpack sady a sestavení webové aplikace ASP.NET Core SignalR, jehož klient je napsána v TypeScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: e4b00fa61ea0becba7d678a0b7c94d2d30f06740
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126425"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="d3b50-103">Jádro ASP.NET SignalR pomocí TypeScript a Webpack</span><span class="sxs-lookup"><span data-stu-id="d3b50-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="d3b50-104">Podle [Sébastien Sougnez](https://twitter.com/ssougnez) a [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="d3b50-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="d3b50-105">[Webpack](https://webpack.js.org/) vývojářům umožňuje vytvořit balíček a vytvářet prostředky klientské webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3b50-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="d3b50-106">Tento kurz ukazuje, jak pomocí Webpack ve webové aplikaci ASP.NET Core SignalR, jehož klient je napsána v [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="d3b50-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="d3b50-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="d3b50-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d3b50-108">Vygenerovat úvodní ASP.NET Core SignalR aplikaci</span><span class="sxs-lookup"><span data-stu-id="d3b50-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="d3b50-109">Konfigurace klienta SignalR TypeScript</span><span class="sxs-lookup"><span data-stu-id="d3b50-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="d3b50-110">Konfigurace sestavení kanálu pomocí Webpack</span><span class="sxs-lookup"><span data-stu-id="d3b50-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="d3b50-111">Konfigurace serveru SignalR</span><span class="sxs-lookup"><span data-stu-id="d3b50-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="d3b50-112">Povolit komunikaci mezi klientem a serverem</span><span class="sxs-lookup"><span data-stu-id="d3b50-112">Enable communication between client and server</span></span>

<span data-ttu-id="d3b50-113">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d3b50-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3b50-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d3b50-114">Prerequisites</span></span>

<span data-ttu-id="d3b50-115">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="d3b50-115">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3b50-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3b50-116">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="d3b50-117">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d3b50-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="d3b50-118">[Node.js](https://nodejs.org/) s [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="d3b50-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>
* <span data-ttu-id="d3b50-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.7 nebo novější s **ASP.NET a webové vývoj** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="d3b50-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d3b50-120">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="d3b50-120">.NET Core CLI</span></span>](#tab/netcore-cli)

* [<span data-ttu-id="d3b50-121">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d3b50-121">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="d3b50-122">[Node.js](https://nodejs.org/) s [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="d3b50-122">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="d3b50-123">Vytvoření webové aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3b50-123">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3b50-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3b50-124">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d3b50-125">Konfigurace Visual Studia a hledat npm v *cesta* proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="d3b50-125">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="d3b50-126">Ve výchozím nastavení používá verzi npm nalezené v adresáři jeho instalace sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3b50-126">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="d3b50-127">Postupujte podle těchto pokynů v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d3b50-127">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="d3b50-128">Přejděte na **nástroje** > **možnosti** > **projekty a řešení** > **webové správy balíčků**  >  **Nástroje pro externí Web**.</span><span class="sxs-lookup"><span data-stu-id="d3b50-128">Navigate to **Tools** > **Options** > **Projects and solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="d3b50-129">Vyberte *$(PATH)* položku ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="d3b50-129">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="d3b50-130">Klikněte na šipku nahoru přesunout položku na druhém pozici v seznamu.</span><span class="sxs-lookup"><span data-stu-id="d3b50-130">Click the up arrow to move the entry to the second position in the list.</span></span> <span data-ttu-id="d3b50-131">Jako vyhraďte první položku odkazuje na místní balíčky projektu.</span><span class="sxs-lookup"><span data-stu-id="d3b50-131">As an aside, the first entry refers to the project's local packages.</span></span>

    ![Konfigurace sady Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="d3b50-133">Visual Studio konfigurace je dokončena.</span><span class="sxs-lookup"><span data-stu-id="d3b50-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="d3b50-134">Je čas a vytvořte tak projekt.</span><span class="sxs-lookup"><span data-stu-id="d3b50-134">It's time to create the project.</span></span>

1. <span data-ttu-id="d3b50-135">Použití **soubor** > **nový** > **projektu** nabídky možnost a vyberte **webové aplikace ASP.NET Core** Šablona.</span><span class="sxs-lookup"><span data-stu-id="d3b50-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="d3b50-136">Název projektu *SignalRWebPack*a klikněte na **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3b50-136">Name the project *SignalRWebPack*, and click the **OK** button.</span></span>
1. <span data-ttu-id="d3b50-137">Vyberte *.NET Core* z rozhraní target framework rozevíracího seznamu a vyberte možnost *ASP.NET Core 2.1* z rozevíracího seznamu pro výběr framework.</span><span class="sxs-lookup"><span data-stu-id="d3b50-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.1* from the framework selector drop-down.</span></span> <span data-ttu-id="d3b50-138">Vyberte **prázdný** šablonu a klikněte na **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3b50-138">Select the **Empty** template, and click the **OK** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d3b50-139">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="d3b50-139">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d3b50-140">Spusťte následující příkaz **integrované Terminálové**:</span><span class="sxs-lookup"><span data-stu-id="d3b50-140">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="d3b50-141">Prázdný ASP.NET Core webové aplikace, cílení na .NET Core je vytvořen v *SignalRWebPack* adresáře.</span><span class="sxs-lookup"><span data-stu-id="d3b50-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="d3b50-142">Konfigurace Webpack a TypeScript</span><span class="sxs-lookup"><span data-stu-id="d3b50-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="d3b50-143">Následující kroky konfigurace převodu TypeScript pro jazyk JavaScript a sdružování prostředky na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="d3b50-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="d3b50-144">Spusťte následující příkaz v kořenu projektu pro vytvoření *package.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="d3b50-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="d3b50-145">Přidat zvýrazněná vlastnost, která má *package.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="d3b50-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="d3b50-146">Nastavení `private` vlastnost `true` brání upozornění instalace balíčku v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="d3b50-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="d3b50-147">Nainstalujte npm požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="d3b50-147">Install the required npm packages.</span></span> <span data-ttu-id="d3b50-148">Spusťte následující příkaz z kořenového adresáře projektu:</span><span class="sxs-lookup"><span data-stu-id="d3b50-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    <span data-ttu-id="d3b50-149">Některé podrobnosti příkazu příkazového Všimněte si:</span><span class="sxs-lookup"><span data-stu-id="d3b50-149">Some command details to note:</span></span>

    * <span data-ttu-id="d3b50-150">Číslo verze odpovídá `@` přihlášení pro každý název balíčku.</span><span class="sxs-lookup"><span data-stu-id="d3b50-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="d3b50-151">npm nainstaluje tyto konkrétní balíček verze.</span><span class="sxs-lookup"><span data-stu-id="d3b50-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="d3b50-152">`-E` Možnost zakáže výchozí chování je npm zápis [sémantické verze](https://semver.org/) rozsahu operátorům *package.json*.</span><span class="sxs-lookup"><span data-stu-id="d3b50-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="d3b50-153">Například `"webpack": "4.12.0"` se používá místo `"webpack": "^4.12.0"`.</span><span class="sxs-lookup"><span data-stu-id="d3b50-153">For example, `"webpack": "4.12.0"` is used instead of `"webpack": "^4.12.0"`.</span></span> <span data-ttu-id="d3b50-154">Tato možnost zabraňuje nezamýšleným upgrade na novější verze balíčku.</span><span class="sxs-lookup"><span data-stu-id="d3b50-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="d3b50-155">Najdete v oficiální [npm install](https://docs.npmjs.com/cli/install) dokumentace pro více podrobností.</span><span class="sxs-lookup"><span data-stu-id="d3b50-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="d3b50-156">Nahraďte `scripts` vlastnost *package.json* souboru následujícím fragmentem kódu:</span><span class="sxs-lookup"><span data-stu-id="d3b50-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="d3b50-157">Vysvětlení skripty:</span><span class="sxs-lookup"><span data-stu-id="d3b50-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="d3b50-158">`build`: Obsahuje ureitou prostředkům na straně klienta v režimu pro vývoj a sleduje změny.</span><span class="sxs-lookup"><span data-stu-id="d3b50-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="d3b50-159">Sledovací proces souboru způsobí, že sada znovu vygenerovat pokaždé, když změní soubor projektu.</span><span class="sxs-lookup"><span data-stu-id="d3b50-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="d3b50-160">`mode` Možnost zakáže produkční optimalizace, jako je například že zatřesete stromu a minimalizace.</span><span class="sxs-lookup"><span data-stu-id="d3b50-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="d3b50-161">Používat pouze `build` ve vývoji.</span><span class="sxs-lookup"><span data-stu-id="d3b50-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="d3b50-162">`release`: Obsahuje ureitou prostředkům na straně klienta v produkčním režimu.</span><span class="sxs-lookup"><span data-stu-id="d3b50-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="d3b50-163">`publish`: Běží `release` skript sady prostředky klienta v produkčním režimu.</span><span class="sxs-lookup"><span data-stu-id="d3b50-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="d3b50-164">Zavolá rozhraní .NET Core CLI [publikování](/dotnet/core/tools/dotnet-publish) příkaz pro publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3b50-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="d3b50-165">Vytvořte soubor s názvem *webpack.config.js*, v kořenu projektu s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="d3b50-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="d3b50-166">Soubor předchozí nakonfiguruje Webpack kompilace.</span><span class="sxs-lookup"><span data-stu-id="d3b50-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="d3b50-167">Některé podrobnosti o konfiguraci pro Všimněte si:</span><span class="sxs-lookup"><span data-stu-id="d3b50-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="d3b50-168">`output` Vlastnost má přednost před výchozí hodnotu *dist*.</span><span class="sxs-lookup"><span data-stu-id="d3b50-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="d3b50-169">Sada je místo toho vygenerované v *wwwroot* adresáře.</span><span class="sxs-lookup"><span data-stu-id="d3b50-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="d3b50-170">`resolve.extensions` Zahrnuje pole *.js* import ke klientovi SignalR JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3b50-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="d3b50-171">Vytvořte novou *src* adresář v kořenu projektu.</span><span class="sxs-lookup"><span data-stu-id="d3b50-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="d3b50-172">Jejím účelem je ukládat prostředky klienta projektu.</span><span class="sxs-lookup"><span data-stu-id="d3b50-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="d3b50-173">Vytvoření *src/index.html* s následujícím obsahem.</span><span class="sxs-lookup"><span data-stu-id="d3b50-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="d3b50-174">Předchozí HTML definuje často používaný kód na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="d3b50-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="d3b50-175">Vytvořte novou *src/css* adresáře.</span><span class="sxs-lookup"><span data-stu-id="d3b50-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="d3b50-176">Jejím účelem je k uložení projektu *.css* soubory.</span><span class="sxs-lookup"><span data-stu-id="d3b50-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="d3b50-177">Vytvoření *src/css/main.css* s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="d3b50-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="d3b50-178">Podle předchozích *main.css* souboru styly aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3b50-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="d3b50-179">Vytvoření *src/tsconfig.json* s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="d3b50-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="d3b50-180">Předchozí kód konfiguruje kompilátoru TypeScript vytvoření [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 kompatibilní JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3b50-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="d3b50-181">Vytvoření *src/index.ts* s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="d3b50-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="d3b50-182">Předchozí TypeScript načte odkazy na elementy DOM a připojí dvě obslužné rutiny událostí:</span><span class="sxs-lookup"><span data-stu-id="d3b50-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="d3b50-183">`keyup`: Tato událost aktivuje se v případě, že uživatel zadá něco do textového pole označený `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="d3b50-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="d3b50-184">`send` Funkce je volána, když uživatel stiskne **Enter** klíč.</span><span class="sxs-lookup"><span data-stu-id="d3b50-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="d3b50-185">`click`: Tato událost se aktivuje, když uživatel klikne **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3b50-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="d3b50-186">`send` Funkce je volána.</span><span class="sxs-lookup"><span data-stu-id="d3b50-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="d3b50-187">Konfigurace aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3b50-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="d3b50-188">Kód součástí `Startup.Configure` metoda zobrazí *Hello, World!*.</span><span class="sxs-lookup"><span data-stu-id="d3b50-188">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="d3b50-189">Nahraďte `app.Run` volání metody pomocí volání [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) a [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="d3b50-189">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="d3b50-190">Předchozí kód umožňuje serveru najít a sloužit *index.html* souboru, zda uživatel zadá jeho úplnou adresu URL nebo adresy URL kořenového adresáře webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3b50-190">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="d3b50-191">Volání [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) v `Startup.ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="d3b50-191">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d3b50-192">Přidá do projektu služby SignalR.</span><span class="sxs-lookup"><span data-stu-id="d3b50-192">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="d3b50-193">Mapy */hub* směrovat `ChatHub` rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="d3b50-193">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="d3b50-194">Přidejte následující řádky na konci `Startup.Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="d3b50-194">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="d3b50-195">Vytvořte nový adresář, s názvem *Hubs*, v kořenu projektu.</span><span class="sxs-lookup"><span data-stu-id="d3b50-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="d3b50-196">Jejím účelem je k uložení rozbočovače SignalR, která je vytvořena v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="d3b50-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="d3b50-197">Vytvoření centra *Hubs/ChatHub.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="d3b50-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="d3b50-198">Přidejte následující kód v horní části *Startup.cs* souboru přeložit `ChatHub` odkaz:</span><span class="sxs-lookup"><span data-stu-id="d3b50-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="d3b50-199">Povolení komunikace klienta a serveru</span><span class="sxs-lookup"><span data-stu-id="d3b50-199">Enable client and server communication</span></span>

<span data-ttu-id="d3b50-200">Aplikace zobrazí aktuálně jednoduchý formulář pro odeslání zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3b50-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="d3b50-201">Při pokusu o tomu nedojde k žádné akci.</span><span class="sxs-lookup"><span data-stu-id="d3b50-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="d3b50-202">Server naslouchá na určitém trasu, ale neprovede žádnou akci s odeslaných zpráv.</span><span class="sxs-lookup"><span data-stu-id="d3b50-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="d3b50-203">Spusťte následující příkaz v kořenovém adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="d3b50-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="d3b50-204">Předchozí příkaz nainstaluje [klienta SignalR TypeScript](https://www.npmjs.com/package/@aspnet/signalr), což umožňuje klienta k odesílání zpráv do serveru.</span><span class="sxs-lookup"><span data-stu-id="d3b50-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="d3b50-205">Přidejte zvýrazněný kód, který *src/index.ts* souboru:</span><span class="sxs-lookup"><span data-stu-id="d3b50-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="d3b50-206">Předchozí kód podporuje přijímání zpráv ze serveru.</span><span class="sxs-lookup"><span data-stu-id="d3b50-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="d3b50-207">`HubConnectionBuilder` Třída vytvoří nového tvůrce pro konfiguraci připojení k serveru.</span><span class="sxs-lookup"><span data-stu-id="d3b50-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="d3b50-208">`withUrl` Funkce konfiguruje adresu URL rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="d3b50-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="d3b50-209">SignalR umožňuje výměnu zpráv mezi klientem a serverem.</span><span class="sxs-lookup"><span data-stu-id="d3b50-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="d3b50-210">Každá zpráva má určitý název.</span><span class="sxs-lookup"><span data-stu-id="d3b50-210">Each message has a specific name.</span></span> <span data-ttu-id="d3b50-211">Například můžete mít zprávy s názvem `messageReceived` , provádění logiky, která je odpovědná za zobrazování novou zprávu v zóně zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3b50-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="d3b50-212">Naslouchání na konkrétní zprávou, můžete to udělat pomocí `on` funkce.</span><span class="sxs-lookup"><span data-stu-id="d3b50-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="d3b50-213">Můžete poslouchat libovolný počet názvů zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3b50-213">You can listen to any number of message names.</span></span> <span data-ttu-id="d3b50-214">Je také možné předat parametry zprávy, jako je jméno autora a obsah přijaté zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3b50-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="d3b50-215">Jakmile klient obdrží zprávu, nový `div` element je vytvořen s jméno autora a zpráva obsahu v jeho `innerHTML` atribut.</span><span class="sxs-lookup"><span data-stu-id="d3b50-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="d3b50-216">Přidá do hlavní `div` element zobrazení zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3b50-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="d3b50-217">Teď, když klient může přijímat zprávy, nakonfigurujte ji k odesílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="d3b50-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="d3b50-218">Přidejte zvýrazněný kód, který *src/index.ts* souboru:</span><span class="sxs-lookup"><span data-stu-id="d3b50-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="d3b50-219">Odesílání zprávy prostřednictvím technologie WebSockets připojení vyžaduje volání `send` metoda.</span><span class="sxs-lookup"><span data-stu-id="d3b50-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="d3b50-220">První parametr metody je název zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3b50-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="d3b50-221">Data zpráv se vyskytuje v podoblastech ostatní parametry.</span><span class="sxs-lookup"><span data-stu-id="d3b50-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="d3b50-222">V tomto příkladu zprávu označeno `newMessage` je odeslat na server.</span><span class="sxs-lookup"><span data-stu-id="d3b50-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="d3b50-223">Zpráva se skládá z uživatelské jméno a uživatelský vstup v textovém poli.</span><span class="sxs-lookup"><span data-stu-id="d3b50-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="d3b50-224">Pokud odesílání funguje, není zaškrtnuté políčko textové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d3b50-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="d3b50-225">Zvýrazněná metody pro přidání `ChatHub` třídy:</span><span class="sxs-lookup"><span data-stu-id="d3b50-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="d3b50-226">Předchozí kód všesměrově přijatých zpráv pro všechny připojené uživatele, jakmile server obdrží je.</span><span class="sxs-lookup"><span data-stu-id="d3b50-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="d3b50-227">Není nutné mít obecný `on` metoda přijímat všechny zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3b50-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="d3b50-228">Metoda s názvem po přípony názvu zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3b50-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="d3b50-229">V tomto příkladu TypeScript klient odešle zprávu identifikovat jako `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="d3b50-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="d3b50-230">C# `NewMessage` metoda očekává data odeslaná klientem.</span><span class="sxs-lookup"><span data-stu-id="d3b50-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="d3b50-231">Přišla žádost o k [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metodu [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="d3b50-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="d3b50-232">Přijaté zprávy se odesílají na všechny klienty připojené k rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="d3b50-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="d3b50-233">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="d3b50-233">Test the app</span></span>

<span data-ttu-id="d3b50-234">Potvrďte, že aplikace funguje pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="d3b50-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3b50-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3b50-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="d3b50-236">Spustit Webpack *verze* režimu.</span><span class="sxs-lookup"><span data-stu-id="d3b50-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="d3b50-237">Pomocí **Konzola správce balíčků** okno, spusťte následující příkaz v kořenu projektu:</span><span class="sxs-lookup"><span data-stu-id="d3b50-237">Using the **Package Manager Console** window, execute the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="d3b50-238">Vyberte **ladění** > **spustit bez ladění** spusťte aplikaci v prohlížeči bez připojení ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="d3b50-238">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="d3b50-239">*Wwwroot/index.html* soubor je předán na `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="d3b50-239">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="d3b50-240">Otevřete jiná instance prohlížeče (libovolného prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="d3b50-240">Open another browser instance (any browser).</span></span> <span data-ttu-id="d3b50-241">Vložte adresu URL na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="d3b50-241">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="d3b50-242">Vyberte buď prohlížeče, zadejte v něco **zpráva** textového pole a klikněte na tlačítko **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3b50-242">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="d3b50-243">Jedinečné uživatelské jméno a zpráva se zobrazí na obou stránkách okamžitě.</span><span class="sxs-lookup"><span data-stu-id="d3b50-243">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d3b50-244">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="d3b50-244">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="d3b50-245">Spustit Webpack *verze* režimu spuštěním následujícího příkazu v kořenu projektu:</span><span class="sxs-lookup"><span data-stu-id="d3b50-245">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="d3b50-246">Sestavení a spuštění aplikace tak, že spustíte následující příkaz v kořenu projektu:</span><span class="sxs-lookup"><span data-stu-id="d3b50-246">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="d3b50-247">Webový server spustí aplikaci a zpřístupní ho na localhost.</span><span class="sxs-lookup"><span data-stu-id="d3b50-247">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="d3b50-248">Otevřete prohlížeč na `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="d3b50-248">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="d3b50-249">*Wwwroot/index.html* je soubor zpracován.</span><span class="sxs-lookup"><span data-stu-id="d3b50-249">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="d3b50-250">Zkopírujte adresu URL z panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="d3b50-250">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="d3b50-251">Otevřete jiná instance prohlížeče (libovolného prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="d3b50-251">Open another browser instance (any browser).</span></span> <span data-ttu-id="d3b50-252">Vložte adresu URL na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="d3b50-252">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="d3b50-253">Vyberte buď prohlížeče, zadejte v něco **zpráva** textového pole a klikněte na tlačítko **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3b50-253">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="d3b50-254">Jedinečné uživatelské jméno a zpráva se zobrazí na obou stránkách okamžitě.</span><span class="sxs-lookup"><span data-stu-id="d3b50-254">The unique user name and message are displayed on both pages instantly.</span></span>

---

![zpráva zobrazená v prohlížeči systému windows](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="d3b50-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d3b50-256">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>