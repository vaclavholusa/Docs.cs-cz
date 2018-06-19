---
title: Sady a minifiy statické prostředky v ASP.NET Core
author: scottaddie
description: Informace o optimalizaci statické prostředky ve webové aplikaci ASP.NET Core použitím sdružování a minimalizace techniky.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: a3d49315fbb62eb1a42eb1b30885dc19a81c0a91
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094642"
---
# <a name="bundle-and-minifiy-static-assets-in-aspnet-core"></a><span data-ttu-id="e6215-103">Sady a minifiy statické prostředky v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6215-103">Bundle and minifiy static assets in ASP.NET Core</span></span>

<span data-ttu-id="e6215-104">Podle [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="e6215-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="e6215-105">Tento článek vysvětluje výhody použití sdružování a minimalizace, včetně použití těchto funkcí s webovými aplikacemi ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6215-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="e6215-106">Co je sdružování a minimalizace?</span><span class="sxs-lookup"><span data-stu-id="e6215-106">What is bundling and minification?</span></span>

<span data-ttu-id="e6215-107">Sdružování a minimalizace jsou dvě odlišné výkonu optimalizace, které můžete použít ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e6215-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="e6215-108">Použít společně, sdružování a minimalizace vylepšit výkon, snižuje počet požadavků serveru a zmenšení velikosti požadovaných prostředků statické.</span><span class="sxs-lookup"><span data-stu-id="e6215-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="e6215-109">Sdružování a minimalizace především zlepšit první čas načítání stránky požadavku.</span><span class="sxs-lookup"><span data-stu-id="e6215-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="e6215-110">Jakmile požádal na webové stránce v prohlížeči ukládá do mezipaměti statické prostředky (JavaScript, CSS a bitové kopie).</span><span class="sxs-lookup"><span data-stu-id="e6215-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="e6215-111">V důsledku toho sdružování a minimalizace nemáte výkon při požadavku stejné stránce nebo stránky, na stejném webu požaduje stejné prostředky.</span><span class="sxs-lookup"><span data-stu-id="e6215-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="e6215-112">Pokud vyprší platnost hlavičky není správně nastaven na prostředky a pokud se nepoužívá sdružování a minimalizace, heuristiky aktuálnosti prohlížeče označit prostředky zastaralé po několik dní.</span><span class="sxs-lookup"><span data-stu-id="e6215-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="e6215-113">Kromě toho v prohlížeči vyžaduje žádost o ověření pro každý prostředek.</span><span class="sxs-lookup"><span data-stu-id="e6215-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="e6215-114">V takovém případě sdružování a minimalizace poskytnout zlepšování výkonu i po první požadavek na stránku.</span><span class="sxs-lookup"><span data-stu-id="e6215-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="e6215-115">Sdružování</span><span class="sxs-lookup"><span data-stu-id="e6215-115">Bundling</span></span>

<span data-ttu-id="e6215-116">Sdružování kombinuje několik souborů do jediného souboru.</span><span class="sxs-lookup"><span data-stu-id="e6215-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="e6215-117">Sdružování snižuje počet požadavků serveru, které jsou nezbytné k vykreslení webové prostředku, například na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="e6215-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="e6215-118">Můžete vytvořit libovolný počet jednotlivých sad speciálně pro šablon stylů CSS, JavaScript, atd. Méně souborů znamená méně požadavky HTTP z prohlížeče na server nebo služba poskytující vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6215-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="e6215-119">Výsledkem zvýšení výkonu zatížení první stránky.</span><span class="sxs-lookup"><span data-stu-id="e6215-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="e6215-120">Minimalizace</span><span class="sxs-lookup"><span data-stu-id="e6215-120">Minification</span></span>

<span data-ttu-id="e6215-121">Minimalizace odstraní nepotřebné znaky z kódu beze změny funkce.</span><span class="sxs-lookup"><span data-stu-id="e6215-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="e6215-122">Výsledkem je snížení významné velikosti požadovaných prostředků (například šablon stylů CSS, Image a soubory JavaScript).</span><span class="sxs-lookup"><span data-stu-id="e6215-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="e6215-123">Běžné důsledky minimalizace zahrnují zkrátit názvy proměnných jeden znak a odebírání komentářů a nepotřebné prázdné znaky.</span><span class="sxs-lookup"><span data-stu-id="e6215-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="e6215-124">Vezměte v úvahu následující funkce JavaScript:</span><span class="sxs-lookup"><span data-stu-id="e6215-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="e6215-125">Minimalizace snižuje funkce takto:</span><span class="sxs-lookup"><span data-stu-id="e6215-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="e6215-126">Kromě odebírání komentářů a nepotřebné prázdné znaky, byly následující názvy parametrů a proměnnou přejmenovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e6215-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="e6215-127">Původní</span><span class="sxs-lookup"><span data-stu-id="e6215-127">Original</span></span> | <span data-ttu-id="e6215-128">Přejmenovat</span><span class="sxs-lookup"><span data-stu-id="e6215-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="e6215-129">Dopad sdružování a minimalizace</span><span class="sxs-lookup"><span data-stu-id="e6215-129">Impact of bundling and minification</span></span>

<span data-ttu-id="e6215-130">Následující tabulka popisuje rozdíly mezi jednotlivě načítání prostředků a pomocí sdružování a minimalizace:</span><span class="sxs-lookup"><span data-stu-id="e6215-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="e6215-131">Akce</span><span class="sxs-lookup"><span data-stu-id="e6215-131">Action</span></span> | <span data-ttu-id="e6215-132">S B/M</span><span class="sxs-lookup"><span data-stu-id="e6215-132">With B/M</span></span> | <span data-ttu-id="e6215-133">Bez B/M</span><span class="sxs-lookup"><span data-stu-id="e6215-133">Without B/M</span></span> | <span data-ttu-id="e6215-134">Změna</span><span class="sxs-lookup"><span data-stu-id="e6215-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="e6215-135">Požadavky na soubory</span><span class="sxs-lookup"><span data-stu-id="e6215-135">File Requests</span></span>  | <span data-ttu-id="e6215-136">7</span><span class="sxs-lookup"><span data-stu-id="e6215-136">7</span></span>   | <span data-ttu-id="e6215-137">18</span><span class="sxs-lookup"><span data-stu-id="e6215-137">18</span></span>     | <span data-ttu-id="e6215-138">157%</span><span class="sxs-lookup"><span data-stu-id="e6215-138">157%</span></span>
<span data-ttu-id="e6215-139">Přenést KB</span><span class="sxs-lookup"><span data-stu-id="e6215-139">KB Transferred</span></span> | <span data-ttu-id="e6215-140">156</span><span class="sxs-lookup"><span data-stu-id="e6215-140">156</span></span> | <span data-ttu-id="e6215-141">264.68</span><span class="sxs-lookup"><span data-stu-id="e6215-141">264.68</span></span> | <span data-ttu-id="e6215-142">70%</span><span class="sxs-lookup"><span data-stu-id="e6215-142">70%</span></span>
<span data-ttu-id="e6215-143">Čas načítání (ms)</span><span class="sxs-lookup"><span data-stu-id="e6215-143">Load Time (ms)</span></span> | <span data-ttu-id="e6215-144">885</span><span class="sxs-lookup"><span data-stu-id="e6215-144">885</span></span> | <span data-ttu-id="e6215-145">2360</span><span class="sxs-lookup"><span data-stu-id="e6215-145">2360</span></span>   | <span data-ttu-id="e6215-146">167%</span><span class="sxs-lookup"><span data-stu-id="e6215-146">167%</span></span>

<span data-ttu-id="e6215-147">Prohlížeče jsou poměrně podrobné s ohledem na hlavičkách žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e6215-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="e6215-148">Celkový počet bajtů odeslaných metrika viděli k výraznému snížení při sdružování.</span><span class="sxs-lookup"><span data-stu-id="e6215-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="e6215-149">Čas načítání ukazuje výrazným vylepšením, ale v tomto příkladu byla spuštěna místně.</span><span class="sxs-lookup"><span data-stu-id="e6215-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="e6215-150">Větší zvýšení výkonu jsou realizovány při použití sdružování a minimalizace s prostředky přenést přes síť.</span><span class="sxs-lookup"><span data-stu-id="e6215-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="e6215-151">Volba strategie sdružování a minimalizace</span><span class="sxs-lookup"><span data-stu-id="e6215-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="e6215-152">Šablony projektů MVC a stránky Razor poskytují out-of-the-box řešení pro sdružování a minimalizace skládající se z konfiguračního souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="e6215-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="e6215-153">Nástroje třetích stran, jako [Gulp](xref:client-side/using-gulp) a [Grunt](xref:client-side/using-grunt) úkolů závodníky, provádění stejných úloh s trochu složitější.</span><span class="sxs-lookup"><span data-stu-id="e6215-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="e6215-154">Nástroj třetí strany je skvělým přizpůsobení, pokud vaše pracovní postup vývoje vyžaduje zpracování nad rámec sdružování a minimalizace&mdash;například optimalizace linting a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="e6215-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="e6215-155">Pomocí sdružování a minimalizace návrhu se vytváří minifikovaný soubory před nasazením aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6215-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="e6215-156">Sdružování a minifikace před nasazením poskytuje výhodu v podobě zatížení serveru.</span><span class="sxs-lookup"><span data-stu-id="e6215-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="e6215-157">Ale je důležité vědět, že návrhu sdružování a minimalizace zvyšuje složitost sestavení a funguje jenom se statické soubory.</span><span class="sxs-lookup"><span data-stu-id="e6215-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="e6215-158">Konfigurace sdružování a minimalizace</span><span class="sxs-lookup"><span data-stu-id="e6215-158">Configure bundling and minification</span></span>

<span data-ttu-id="e6215-159">Projektu šablony MVC a stránky syntaxe Razor poskytují *bundleconfig.json* konfigurační soubor, který definuje možnosti pro každou sadu.</span><span class="sxs-lookup"><span data-stu-id="e6215-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="e6215-160">Ve výchozím nastavení, je konfigurace jedné sadě definován pro vlastní jazyk JavaScript (*wwwroot/js/site.js*) a šablony stylů (*wwwroot/css/site.css*) souborů:</span><span class="sxs-lookup"><span data-stu-id="e6215-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="e6215-161">Možnosti konfigurace patří:</span><span class="sxs-lookup"><span data-stu-id="e6215-161">Configuration options include:</span></span>

* <span data-ttu-id="e6215-162">`outputFileName`: Název souboru kompletu na výstup.</span><span class="sxs-lookup"><span data-stu-id="e6215-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="e6215-163">Může obsahovat relativní cestu z *bundleconfig.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="e6215-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="e6215-164">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="e6215-164">**required**</span></span>
* <span data-ttu-id="e6215-165">`inputFiles`: Pole soubory sady společně.</span><span class="sxs-lookup"><span data-stu-id="e6215-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="e6215-166">Jedná se o relativní cesty k souboru konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e6215-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="e6215-167">**volitelné**, \* prázdnou hodnotu výsledkem prázdná výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="e6215-167">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="e6215-168">[režim expanze](http://www.tldp.org/LDP/abs/html/globbingref.html) vzory jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="e6215-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="e6215-169">`minify`: Možnosti minimalizace typ výstupu.</span><span class="sxs-lookup"><span data-stu-id="e6215-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="e6215-170">**volitelné**, *výchozí – `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="e6215-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="e6215-171">Možnosti konfigurace jsou k dispozici na typ výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="e6215-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="e6215-172">Minifikátor šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="e6215-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="e6215-173">Minifikátor JavaScript</span><span class="sxs-lookup"><span data-stu-id="e6215-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="e6215-174">Minifikátor HTML</span><span class="sxs-lookup"><span data-stu-id="e6215-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="e6215-175">`includeInProject`: Příznak určující, zda má-li přidat vygenerované soubory do souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="e6215-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="e6215-176">**volitelné**, *výchozí - false.*</span><span class="sxs-lookup"><span data-stu-id="e6215-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="e6215-177">`sourceMap`: Příznak, který udává, jestli se má Generovat mapu zdroj pro soubor připojené.</span><span class="sxs-lookup"><span data-stu-id="e6215-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="e6215-178">**volitelné**, *výchozí - false.*</span><span class="sxs-lookup"><span data-stu-id="e6215-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="e6215-179">`sourceMapRootPath`: Kořenovou cestu pro ukládání vytvořeném zdrojovém souboru mapy.</span><span class="sxs-lookup"><span data-stu-id="e6215-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="e6215-180">Sestavení – doba provádění sdružování a minimalizace</span><span class="sxs-lookup"><span data-stu-id="e6215-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="e6215-181">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) balíček NuGet umožňuje provádění sdružování a minimalizace v čase vytvoření buildu.</span><span class="sxs-lookup"><span data-stu-id="e6215-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="e6215-182">Vloží balíček [cíle MSBuild](/visualstudio/msbuild/msbuild-targets) kterého spouští v sestavení a vyčištění čas.</span><span class="sxs-lookup"><span data-stu-id="e6215-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="e6215-183">*Bundleconfig.json* analýzy souboru procesem sestavení nevytvořila výstupní soubory na základě definovaných konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e6215-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="e6215-184">BuildBundlerMinifier patří do komunitou vytvářený projektu na Githubu, pro které společnost Microsoft poskytuje bez podpory.</span><span class="sxs-lookup"><span data-stu-id="e6215-184">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="e6215-185">Podává problémy by měl být [zde](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="e6215-185">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e6215-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6215-186">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="e6215-187">Přidat *BuildBundlerMinifier* balíčku do projektu.</span><span class="sxs-lookup"><span data-stu-id="e6215-187">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="e6215-188">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="e6215-188">Build the project.</span></span> <span data-ttu-id="e6215-189">Tímto se zobrazí v okně výstupu:</span><span class="sxs-lookup"><span data-stu-id="e6215-189">The following appears in the Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="e6215-190">Vyčistěte projekt.</span><span class="sxs-lookup"><span data-stu-id="e6215-190">Clean the project.</span></span> <span data-ttu-id="e6215-191">Tímto se zobrazí v okně výstupu:</span><span class="sxs-lookup"><span data-stu-id="e6215-191">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e6215-192">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e6215-192">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="e6215-193">Přidat *BuildBundlerMinifier* balíčku do projektu:</span><span class="sxs-lookup"><span data-stu-id="e6215-193">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="e6215-194">Pokud používáte ASP.NET základní 1.x, obnovení nově přidaného balíčku:</span><span class="sxs-lookup"><span data-stu-id="e6215-194">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="e6215-195">Sestavení projektu:</span><span class="sxs-lookup"><span data-stu-id="e6215-195">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="e6215-196">Zobrazí se následující:</span><span class="sxs-lookup"><span data-stu-id="e6215-196">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="e6215-197">Vyčistěte projekt:</span><span class="sxs-lookup"><span data-stu-id="e6215-197">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="e6215-198">Zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="e6215-198">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="e6215-199">Ad hoc provádění sdružování a minimalizace</span><span class="sxs-lookup"><span data-stu-id="e6215-199">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="e6215-200">Je možné spustit úlohy sdružování a minimalizace na základě ad hoc bez vytváření projektu.</span><span class="sxs-lookup"><span data-stu-id="e6215-200">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="e6215-201">Přidat [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) balíček NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="e6215-201">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="e6215-202">BundlerMinifier.Core patří do komunitou vytvářený projektu na Githubu, pro které společnost Microsoft poskytuje bez podpory.</span><span class="sxs-lookup"><span data-stu-id="e6215-202">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="e6215-203">Podává problémy by měl být [zde](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="e6215-203">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="e6215-204">Tento balíček rozšiřuje rozhraní příkazového řádku .NET Core zahrnout *dotnet sady* nástroj.</span><span class="sxs-lookup"><span data-stu-id="e6215-204">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="e6215-205">V okně konzoly Správce balíčků (PMC) nebo v příkazovém řádku, mohou být provedeny následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6215-205">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="e6215-206">Správce balíčků NuGet přidá do souboru \*.csproj jako závislosti `<PackageReference />` uzlů.</span><span class="sxs-lookup"><span data-stu-id="e6215-206">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="e6215-207">`dotnet bundle` Příkaz je zaregistrovaná pomocí rozhraní příkazového řádku .NET Core pouze tehdy, když `<DotNetCliToolReference />` uzel se používá.</span><span class="sxs-lookup"><span data-stu-id="e6215-207">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="e6215-208">Upravte soubor \*.csproj odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e6215-208">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="e6215-209">Přidání souborů do pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="e6215-209">Add files to workflow</span></span>

<span data-ttu-id="e6215-210">Zvažte příklad, ve kterém Další *custom.css* se přidá soubor připomínající následující:</span><span class="sxs-lookup"><span data-stu-id="e6215-210">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="e6215-211">K minifikaci *custom.css* a její sady *site.css* do *site.min.css* soubor, přidejte relativní cestu k *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="e6215-211">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="e6215-212">Alternativně může následující vzor expanze názvů:</span><span class="sxs-lookup"><span data-stu-id="e6215-212">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="e6215-213">Tento vzor expanze názvů odpovídá všechny soubory šablon stylů CSS a vyloučí vzor minifikovaný souborů.</span><span class="sxs-lookup"><span data-stu-id="e6215-213">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="e6215-214">Sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6215-214">Build the application.</span></span> <span data-ttu-id="e6215-215">Otevřete *site.min.css* a Všimněte si obsah *custom.css* je připojen na konec souboru.</span><span class="sxs-lookup"><span data-stu-id="e6215-215">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="e6215-216">Na základě prostředí sdružování a minimalizace</span><span class="sxs-lookup"><span data-stu-id="e6215-216">Environment-based bundling and minification</span></span>

<span data-ttu-id="e6215-217">Jako osvědčený postup připojené a minifikovaný soubory aplikace by měla lze použít v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e6215-217">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="e6215-218">Během vývoje zkontrolujte původní soubory pro snazší ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6215-218">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="e6215-219">Zadejte soubory, které chcete zahrnout do vaší stránky pomocí [pomocná značku prostředí](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e6215-219">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="e6215-220">Pomocník značku prostředí pouze vykreslí obsah při spuštění v konkrétní [prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="e6215-220">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="e6215-221">Následující `environment` značky vykreslí nezpracované soubory šablon stylů CSS při spuštění v `Development` prostředí:</span><span class="sxs-lookup"><span data-stu-id="e6215-221">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e6215-222">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="e6215-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e6215-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e6215-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

<span data-ttu-id="e6215-224">Následující `environment` značky vykreslí souborů CSS připojené a minifikovaný při spuštění v prostředí s jiným než `Development`.</span><span class="sxs-lookup"><span data-stu-id="e6215-224">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="e6215-225">Například běžet `Production` nebo `Staging` aktivuje vykreslování těchto šablon:</span><span class="sxs-lookup"><span data-stu-id="e6215-225">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e6215-226">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="e6215-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e6215-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e6215-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="e6215-228">Využívat bundleconfig.json z Gulp</span><span class="sxs-lookup"><span data-stu-id="e6215-228">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="e6215-229">Existují případy, ve kterých aplikace sdružování a minimalizace pracovní postup vyžaduje další zpracování.</span><span class="sxs-lookup"><span data-stu-id="e6215-229">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="e6215-230">Mezi příklady patří Optimalizace bitové kopie, nejnovějších mezipaměti a zpracování asset CDN.</span><span class="sxs-lookup"><span data-stu-id="e6215-230">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="e6215-231">Splňovat tyto požadavky, můžete převést sdružování a minimalizace pracovní postup použít Gulp.</span><span class="sxs-lookup"><span data-stu-id="e6215-231">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="e6215-232">Použití rozšíření instalující další produkty & Minifikátor</span><span class="sxs-lookup"><span data-stu-id="e6215-232">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="e6215-233">Visual Studio [instalující další produkty & Minifikátor](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) rozšíření zpracovává převod Gulp.</span><span class="sxs-lookup"><span data-stu-id="e6215-233">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="e6215-234">Rozšíření instalující další produkty & Minifikátor patří do komunitou vytvářený projektu na Githubu, pro které společnost Microsoft poskytuje bez podpory.</span><span class="sxs-lookup"><span data-stu-id="e6215-234">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="e6215-235">Podává problémy by měl být [zde](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="e6215-235">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="e6215-236">Klikněte pravým tlačítkem myši *bundleconfig.json* soubor v Průzkumníku řešení a vyberte **instalující další produkty & Minifikátor** > **převést na Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="e6215-236">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Převést na Gulp položky kontextové nabídky](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="e6215-238">*Gulpfile.js* a *package.json* soubory budou přidány do projektu.</span><span class="sxs-lookup"><span data-stu-id="e6215-238">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="e6215-239">V podporu [npm](https://www.npmjs.com/) balíčky uvedené v *package.json* souboru `devDependencies` části jsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="e6215-239">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="e6215-240">Spusťte následující příkaz v okně pomocí PMC nainstalovat rozhraní příkazového řádku Gulp jako globální závislost:</span><span class="sxs-lookup"><span data-stu-id="e6215-240">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="e6215-241">*Gulpfile.js* souboru čtení *bundleconfig.json* soubor pro vstupy, výstupy a nastavení.</span><span class="sxs-lookup"><span data-stu-id="e6215-241">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="e6215-242">Převést ručně</span><span class="sxs-lookup"><span data-stu-id="e6215-242">Convert manually</span></span>

<span data-ttu-id="e6215-243">Pokud Visual Studio a rozšíření instalující další produkty & Minifikátor nejsou k dispozici, převeďte ručně.</span><span class="sxs-lookup"><span data-stu-id="e6215-243">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="e6215-244">Přidat *package.json* soubor s následující `devDependencies`, do kořenového adresáře projektu:</span><span class="sxs-lookup"><span data-stu-id="e6215-244">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="e6215-245">Nainstalujte závislosti spuštěním následujícího příkazu na stejné úrovni jako *package.json*:</span><span class="sxs-lookup"><span data-stu-id="e6215-245">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="e6215-246">Nainstalujte rozhraní příkazového řádku Gulp jako globální závislost:</span><span class="sxs-lookup"><span data-stu-id="e6215-246">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="e6215-247">Kopírování *gulpfile.js* souboru do kořenového adresáře projektu níže:</span><span class="sxs-lookup"><span data-stu-id="e6215-247">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="e6215-248">Spuštění úloh Gulp</span><span class="sxs-lookup"><span data-stu-id="e6215-248">Run Gulp tasks</span></span>

<span data-ttu-id="e6215-249">Spustit úlohu minimalizace Gulp před sestavení projektu v sadě Visual Studio, přidejte následující [MSBuild cíl](/visualstudio/msbuild/msbuild-targets) \*.csproj souboru:</span><span class="sxs-lookup"><span data-stu-id="e6215-249">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="e6215-250">V tomto příkladu jsou všechny úlohy definované v rámci `MyPreCompileTarget` cílových spouštění před předdefinovanou `Build` cíl.</span><span class="sxs-lookup"><span data-stu-id="e6215-250">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="e6215-251">V okně výstupu sady Visual Studio zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="e6215-251">Output similar to the following appears in Visual Studio's Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="e6215-252">Průzkumník Spouštěče úloh sady Visual Studio lze případně vázat Gulp úlohy k událostem konkrétní sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e6215-252">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="e6215-253">V tématu [spuštěné úkoly výchozí](xref:client-side/using-gulp#running-default-tasks) pokyny učinit.</span><span class="sxs-lookup"><span data-stu-id="e6215-253">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e6215-254">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e6215-254">Additional resources</span></span>

* [<span data-ttu-id="e6215-255">Použití nástroje Gulp</span><span class="sxs-lookup"><span data-stu-id="e6215-255">Use Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="e6215-256">Použití nástroje Grunt</span><span class="sxs-lookup"><span data-stu-id="e6215-256">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="e6215-257">Používání více prostředí</span><span class="sxs-lookup"><span data-stu-id="e6215-257">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="e6215-258">Pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="e6215-258">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
