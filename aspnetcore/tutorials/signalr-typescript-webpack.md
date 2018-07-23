---
title: Použití funkce SignalR technologie ASP.NET Core s TypeScript a Webpacku
author: ssougnez
description: V tomto kurzu nakonfigurujete Webpacku k vytvoření balíčku a sestavit webovou aplikaci funkce SignalR technologie ASP.NET Core, jejichž klienta je napsána v TypeScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 7f47192dbf39432334f6d7ae4829a45860dafcc5
ms.sourcegitcommit: ee2b26c7d08b38c908c668522554b52ab8efa221
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146842"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="8a0c7-103">Použití funkce SignalR technologie ASP.NET Core s TypeScript a Webpacku</span><span class="sxs-lookup"><span data-stu-id="8a0c7-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="8a0c7-104">Podle [Sébastien Sougnez](https://twitter.com/ssougnez) a [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="8a0c7-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="8a0c7-105">[Webpacku](https://webpack.js.org/) vývojářům umožňuje vytvořit balíček a vytvářet prostředky webové aplikace na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="8a0c7-106">Tento kurz ukazuje použití Webpacku ve webové aplikaci funkce SignalR technologie ASP.NET Core, jejichž klienta je napsána v [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="8a0c7-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="8a0c7-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8a0c7-108">Generování uživatelského rozhraní si první aplikaci funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a0c7-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="8a0c7-109">Konfigurace klienta SignalR TypeScript</span><span class="sxs-lookup"><span data-stu-id="8a0c7-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="8a0c7-110">Nakonfigurujte kanál sestavení pomocí Webpacku</span><span class="sxs-lookup"><span data-stu-id="8a0c7-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="8a0c7-111">Konfigurace serveru SignalR</span><span class="sxs-lookup"><span data-stu-id="8a0c7-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="8a0c7-112">Povolení komunikace mezi klientem a serverem</span><span class="sxs-lookup"><span data-stu-id="8a0c7-112">Enable communication between client and server</span></span>

<span data-ttu-id="8a0c7-113">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8a0c7-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a0c7-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8a0c7-114">Prerequisites</span></span>

<span data-ttu-id="8a0c7-115">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-115">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8a0c7-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8a0c7-116">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="8a0c7-117">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="8a0c7-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="8a0c7-118">[Node.js](https://nodejs.org/) s [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="8a0c7-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>
* <span data-ttu-id="8a0c7-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.7.3 nebo novější s **vývoj pro ASP.NET a web** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="8a0c7-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8a0c7-120">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="8a0c7-120">.NET Core CLI</span></span>](#tab/netcore-cli)

* [<span data-ttu-id="8a0c7-121">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="8a0c7-121">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="8a0c7-122">[Node.js](https://nodejs.org/) s [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="8a0c7-122">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="8a0c7-123">Vytvoření webové aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a0c7-123">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8a0c7-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8a0c7-124">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8a0c7-125">Konfigurace sady Visual Studio k vyhledání npm v *cesta* proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-125">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="8a0c7-126">Ve výchozím nastavení používá verzi npm nalezen v adresáři instalace sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-126">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="8a0c7-127">Postupujte podle těchto pokynů v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-127">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="8a0c7-128">Přejděte do **nástroje** > **možnosti** > **projekty a řešení** > **správy webových balíčků**  >  **Externí webové nástroje**.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-128">Navigate to **Tools** > **Options** > **Projects and solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="8a0c7-129">Vyberte *$(PATH)* položku seznamu.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-129">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="8a0c7-130">Klikněte na šipku nahoru přesunout položku na druhém pozici v seznamu.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-130">Click the up arrow to move the entry to the second position in the list.</span></span> <span data-ttu-id="8a0c7-131">Jako aside první položka odkazuje na místní balíčky v projektu.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-131">As an aside, the first entry refers to the project's local packages.</span></span>

    ![Konfigurace sady Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="8a0c7-133">Konfigurace Visual Studio je dokončen.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="8a0c7-134">Je čas vytvoření projektu.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-134">It's time to create the project.</span></span>

1. <span data-ttu-id="8a0c7-135">Použití **souboru** > **nový** > **projektu** nabídku a vyberte **webové aplikace ASP.NET Core** Šablona.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="8a0c7-136">Pojmenujte projekt *SignalRWebPack*a klikněte na tlačítko **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-136">Name the project *SignalRWebPack*, and click the **OK** button.</span></span>
1. <span data-ttu-id="8a0c7-137">Vyberte *.NET Core* cílovém rozhraní rozevíracího seznamu a vyberte *ASP.NET Core 2.1* z rozevírací seznam pro výběr framework.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.1* from the framework selector drop-down.</span></span> <span data-ttu-id="8a0c7-138">Vyberte **prázdný** šablony a kliknutím **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-138">Select the **Empty** template, and click the **OK** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8a0c7-139">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="8a0c7-139">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8a0c7-140">Spuštěním následujícího příkazu **integrovaný terminál**:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-140">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="8a0c7-141">Prázdné webové aplikace ASP.NET Core, jejichž cílem je .NET Core, je vytvořen v *SignalRWebPack* adresáře.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="8a0c7-142">Konfigurace Webpacku a TypeScript</span><span class="sxs-lookup"><span data-stu-id="8a0c7-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="8a0c7-143">Následující kroky konfigurace převod TypeScript pro JavaScript a vytváření prostředků na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="8a0c7-144">Spuštěním následujícího příkazu v kořenovém adresáři projektu vytvořte *package.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="8a0c7-145">Přidat vlastnost zvýrazněný na *package.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="8a0c7-146">Nastavení `private` vlastnost `true` brání upozornění instalace balíčku v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="8a0c7-147">Instalace balíčků npm vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-147">Install the required npm packages.</span></span> <span data-ttu-id="8a0c7-148">Spusťte následující příkaz z kořenového adresáře projektu:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    <span data-ttu-id="8a0c7-149">Některé podrobnosti příkazu mějte na paměti:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-149">Some command details to note:</span></span>

    * <span data-ttu-id="8a0c7-150">Následuje číslo verze `@` přihlášení pro každý název balíčku.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="8a0c7-151">npm instalaci těchto verzí určitého balíčku.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="8a0c7-152">`-E` Možnost zakáže npm, a výchozí chování psaní [sémantické správy verzí](https://semver.org/) rozsahu operátorům *package.json*.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="8a0c7-153">Například `"webpack": "4.12.0"` se použije namísto `"webpack": "^4.12.0"`.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-153">For example, `"webpack": "4.12.0"` is used instead of `"webpack": "^4.12.0"`.</span></span> <span data-ttu-id="8a0c7-154">Tato možnost zabraňuje neúmyslnému upgrade na novější verze balíčku.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="8a0c7-155">Najdete v oficiální [npm install](https://docs.npmjs.com/cli/install) dokumentace pro více podrobností.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="8a0c7-156">Nahradit `scripts` vlastnost *package.json* souboru následujícím fragmentem kódu:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="8a0c7-157">Vysvětlení skriptů:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="8a0c7-158">`build`: Obsahuje ureitou vašich prostředků na straně klienta v režimu pro vývoj a sleduje změny souborů.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="8a0c7-159">Sledovací proces souborů způsobí, že sada, která má znovu pokaždé, když změny souborů projektu.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="8a0c7-160">`mode` Možnost zakáže optimalizace produkčního prostředí, jako je například strom, přičemž a připravenost k minifikaci.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="8a0c7-161">Používejte pouze `build` ve vývoji.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="8a0c7-162">`release`: Obsahuje ureitou vašich prostředků na straně klienta v provozním režimu.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="8a0c7-163">`publish`: Spustí `release` skript k vytvoření balíčku sady prostředků na straně klienta v provozním režimu.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="8a0c7-164">Volá v .NET Core CLI [publikovat](/dotnet/core/tools/dotnet-publish) příkaz pro publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="8a0c7-165">Vytvořte soubor s názvem *webpack.config.js*, v kořenové složce projektu s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="8a0c7-166">Předchozí soubor nastaví Webpacku kompilace.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="8a0c7-167">Některé podrobnosti konfigurace mějte na paměti:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="8a0c7-168">`output` Vlastnost přepíše výchozí hodnoty *dist*.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="8a0c7-169">Do sady prostředků místo toho je vygenerován v *wwwroot* adresáře.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="8a0c7-170">`resolve.extensions` Pole zahrnuje *js* k importu jazyka JavaScript klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="8a0c7-171">Vytvořte nový *src* adresáře v kořenové složce projektu.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="8a0c7-172">Jeho účelem je ukládat prostředky projektu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="8a0c7-173">Vytvoření *src/index.html* s následujícím obsahem.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="8a0c7-174">Předchozí kód HTML definuje často používaný kód na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="8a0c7-175">Vytvořte nový *src/css* adresáře.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="8a0c7-176">Jeho účelem je pro uložení projektu *.css* soubory.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="8a0c7-177">Vytvoření *src/css/main.css* s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="8a0c7-178">Předchozí *main.css* styly souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="8a0c7-179">Vytvoření *src/tsconfig.json* s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="8a0c7-180">Předchozí kód konfiguruje kompilátor TypeScript vytvoří [ECMAScript](https://wikipedia.org/wiki/ECMAScript) JavaScript 5 kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="8a0c7-181">Vytvoření *src/index.ts* s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="8a0c7-182">Předchozí TypeScript načte odkazy na elementy modelu DOM a připojí dvě obslužné rutiny události:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="8a0c7-183">`keyup`: Tato událost je vyvoláno, když uživatel zadá něco do textového pole označeno `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="8a0c7-184">`send` Funkce se volá, když uživatel stiskne klávesu **Enter** klíč.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="8a0c7-185">`click`: Tato událost se aktivuje, když uživatel klikne **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="8a0c7-186">`send` Funkce je volána.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="8a0c7-187">Konfigurace aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a0c7-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="8a0c7-188">V kódu `Startup.Configure` metoda zobrazí *Hello World!*.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-188">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="8a0c7-189">Nahradit `app.Run` volání metody s voláními [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) a [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="8a0c7-189">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="8a0c7-190">Předchozí kód umožňuje serveru k vyhledání a poskytovat *index.html* souborů, zda uživatel zadá jeho úplnou adresu URL nebo kořenovou adresu URL webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-190">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="8a0c7-191">Volání [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) v `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-191">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="8a0c7-192">Přidá do projektu služby SignalR.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-192">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="8a0c7-193">Mapování */hub* směrovat `ChatHub` rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-193">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="8a0c7-194">Přidejte následující řádky na konci `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-194">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="8a0c7-195">Vytvořte nový adresář, s názvem *rozbočovače*, v kořenové složce projektu.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="8a0c7-196">Jeho účelem je ukládat rozbočovače SignalR, která je vytvořena v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="8a0c7-197">Vytvoření centra *Hubs/ChatHub.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="8a0c7-198">Přidejte následující kód v horní části *Startup.cs* soubor řešení `ChatHub` odkaz:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="8a0c7-199">Povolení komunikace klienta a serveru</span><span class="sxs-lookup"><span data-stu-id="8a0c7-199">Enable client and server communication</span></span>

<span data-ttu-id="8a0c7-200">Aplikace nyní zobrazí jednoduchý formulář pro odesílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="8a0c7-201">Nic se nestane při pokusu provést.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="8a0c7-202">Server naslouchá na konkrétní postup, ale neprovede žádnou akci s odeslané zprávy.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="8a0c7-203">Spuštěním následujícího příkazu v kořenovém adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="8a0c7-204">Předchozí příkaz nainstaluje [klienta SignalR TypeScript](https://www.npmjs.com/package/@aspnet/signalr), což umožňuje klientovi umožní odeslat zprávy na server.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="8a0c7-205">Přidejte zvýrazněný kód, který *src/index.ts* souboru:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="8a0c7-206">Předchozí kód podporuje příjem zpráv ze serveru.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="8a0c7-207">`HubConnectionBuilder` Třída vytvoří nového tvůrce pro konfiguraci připojení k serveru.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="8a0c7-208">`withUrl` Funkce nakonfiguruje adresu URL centra.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="8a0c7-209">Funkce SignalR umožňuje výměnu zpráv mezi klientem a serverem.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="8a0c7-210">Každá zpráva má specifický název.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-210">Each message has a specific name.</span></span> <span data-ttu-id="8a0c7-211">Například můžete mít zprávy s názvem `messageReceived` spouštěných logiku za zobrazení novou zprávu v zóně zprávy.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="8a0c7-212">Naslouchání na konkrétní zprávou, můžete to udělat pomocí `on` funkce.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="8a0c7-213">Může naslouchat na libovolný počet názvů zpráv.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-213">You can listen to any number of message names.</span></span> <span data-ttu-id="8a0c7-214">Je také možné předat parametry zpráv, jako je například jméno autora nebo obsah ve zprávě přijaté.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="8a0c7-215">Jakmile klient obdrží zprávu, nový `div` element je vytvořena s jméno autora a zpráva obsahu v jeho `innerHTML` atribut.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="8a0c7-216">Přidá se do hlavní `div` element zobrazení zprávy.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="8a0c7-217">Teď, když klient může obdržet zprávu, nakonfigurujte ji k odesílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="8a0c7-218">Přidejte zvýrazněný kód, který *src/index.ts* souboru:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="8a0c7-219">Odeslání zprávy přes WebSockets připojení vyžaduje volání `send` metody.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="8a0c7-220">První parametr metody je název zprávy.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="8a0c7-221">Data zprávy se vyskytuje v podoblastech ostatní parametry.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="8a0c7-222">V tomto příkladu zprávu identifikována jako `newMessage` je odeslána na server.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="8a0c7-223">Zpráva se skládá z uživatelského jména a uživatelský vstup z textového pole.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="8a0c7-224">Pokud odesílání funguje, se vymaže hodnotu textového pole.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="8a0c7-225">Přidat metodu zvýrazněný `ChatHub` třídy:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="8a0c7-226">Předchozí kód vysílá přijaté zprávy na všechny připojené uživatele po server je obdrží.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="8a0c7-227">Už není potřeba mít obecný `on` metoda přijímat všechny zprávy.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="8a0c7-228">Metodu s názvem po přípony název zprávy.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="8a0c7-229">V tomto příkladu TypeScript klient odešle zprávu identifikována jako `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="8a0c7-230">C# `NewMessage` metoda očekává, že data odeslaná klientem.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="8a0c7-231">Je provedeno volání [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metoda [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="8a0c7-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="8a0c7-232">Přijaté zprávy jsou odeslány na všechny klienty připojené k rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="8a0c7-233">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="8a0c7-233">Test the app</span></span>

<span data-ttu-id="8a0c7-234">Potvrďte, že aplikace funguje pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8a0c7-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8a0c7-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="8a0c7-236">Spustit Webpacku *release* režimu.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="8a0c7-237">Použití **Konzola správce balíčků** okna, spusťte následující příkaz v kořenové složce projektu:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-237">Using the **Package Manager Console** window, execute the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="8a0c7-238">Vyberte **ladění** > **spustit bez ladění** pro spuštění aplikace v prohlížeči bez připojení ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-238">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="8a0c7-239">*Wwwroot/index.html* se použijí pro soubor `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-239">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="8a0c7-240">Otevřete jiná instance prohlížeče (libovolného prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="8a0c7-240">Open another browser instance (any browser).</span></span> <span data-ttu-id="8a0c7-241">Vložte adresu URL do adresního řádku.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-241">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="8a0c7-242">Zvolte buď prohlížeče, zadejte v něco **zpráva** textového pole a klikněte na tlačítko **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-242">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="8a0c7-243">Jedinečné uživatelské jméno a zpráva se zobrazí na obě stránky okamžitě.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-243">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8a0c7-244">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="8a0c7-244">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="8a0c7-245">Spustit Webpacku *release* režimu spuštěním následujícího příkazu v kořenovém adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-245">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="8a0c7-246">Sestavte a spusťte aplikaci spuštěním následujícího příkazu v kořenovém adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="8a0c7-246">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="8a0c7-247">Webový server spustí aplikaci a zpřístupňuje je v místním hostiteli.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-247">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="8a0c7-248">Otevřete prohlížeč na `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-248">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="8a0c7-249">*Wwwroot/index.html* soubor je předán.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-249">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="8a0c7-250">Zkopírujte adresu URL z panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-250">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="8a0c7-251">Otevřete jiná instance prohlížeče (libovolného prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="8a0c7-251">Open another browser instance (any browser).</span></span> <span data-ttu-id="8a0c7-252">Vložte adresu URL do adresního řádku.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-252">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="8a0c7-253">Zvolte buď prohlížeče, zadejte v něco **zpráva** textového pole a klikněte na tlačítko **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-253">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="8a0c7-254">Jedinečné uživatelské jméno a zpráva se zobrazí na obě stránky okamžitě.</span><span class="sxs-lookup"><span data-stu-id="8a0c7-254">The unique user name and message are displayed on both pages instantly.</span></span>

---

![zpráva zobrazená v prohlížeči windows](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="8a0c7-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8a0c7-256">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
