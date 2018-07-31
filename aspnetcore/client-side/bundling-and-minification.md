---
title: Vytvoření balíčku a minifikace statické prostředky v ASP.NET Core
author: scottaddie
description: Zjistěte, jak optimalizovat statické prostředky ve webové aplikaci ASP.NET Core s použitím technik sdružování a minifikace.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
uid: client-side/bundling-and-minification
ms.openlocfilehash: bab2f288f3c6956e44ff929bfd2e257301a5806a
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356698"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="6e6d5-103">Vytvoření balíčku a minifikace statické prostředky v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e6d5-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="6e6d5-104">Podle [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="6e6d5-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="6e6d5-105">Tento článek vysvětluje výhody použití sdružování a minifikace, včetně použití těchto funkcí s webovými aplikacemi ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="6e6d5-106">Co je sdružování a minifikace</span><span class="sxs-lookup"><span data-stu-id="6e6d5-106">What is bundling and minification</span></span>

<span data-ttu-id="6e6d5-107">Sdružování a minifikace jsou dvě optimalizace odlišné výkonu, které můžete použít ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="6e6d5-108">Použijí společně, sdružování a minifikace zvýšit výkon snížením počtu požadavků serveru a snížením velikosti požadovaných prostředků statické.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="6e6d5-109">Sdružování a minifikace především zvýšení první čas načítání stránky požadavku.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="6e6d5-110">Po požádal webovou stránku v prohlížeči ukládá do mezipaměti statické prostředky (jazyk JavaScript, CSS a imagí).</span><span class="sxs-lookup"><span data-stu-id="6e6d5-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="6e6d5-111">V důsledku toho sdružování a minifikace není výkon při vyžadování stejnou stránku nebo stránky, na stejném místě požaduje stejné prostředky.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="6e6d5-112">Pokud vypršení platnosti záhlaví není správně nastaven na prostředky a pokud se nepoužívá sdružování a minifikace, prohlížeče aktuálnosti heuristiky označit prostředky zastaralé po několika dnech.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="6e6d5-113">Kromě toho v prohlížeči vyžaduje žádost o ověření pro každý prostředek.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="6e6d5-114">V takovém případě sdružování a minifikace poskytují vylepšení výkonu i po prvním požadavku na stránku.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="6e6d5-115">Sdružování</span><span class="sxs-lookup"><span data-stu-id="6e6d5-115">Bundling</span></span>

<span data-ttu-id="6e6d5-116">Sdružování kombinuje několik souborů do jediného souboru.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="6e6d5-117">Sdružování snižuje počet požadavků serveru, které jsou potřebné k vykreslení webové aktivem, například na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="6e6d5-118">Můžete vytvořit libovolný počet jednotlivých sad speciálně pro šablony stylů CSS, JavaScript, atd. Menší počet souborů znamená méně požadavky HTTP z prohlížeče na server nebo službu poskytující vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="6e6d5-119">Výsledkem Vylepšený výkon načítání první stránky.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="6e6d5-120">Připravenost k minifikaci</span><span class="sxs-lookup"><span data-stu-id="6e6d5-120">Minification</span></span>

<span data-ttu-id="6e6d5-121">Minifikace odebírá nepotřebné znaky z kódu bez změnila jejich funkce.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="6e6d5-122">Výsledkem je snížení významnou velikostí požadovaných prostředků (jako jsou šablony stylů CSS, obrázky a Javascriptové soubory).</span><span class="sxs-lookup"><span data-stu-id="6e6d5-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="6e6d5-123">Běžné vedlejším účinkům připravenost k minifikaci zahrnují zkrácení názvy proměnných jeden znak a odebírání komentářů a zbytečné mezery.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="6e6d5-124">Vezměte v úvahu následující funkce jazyka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="6e6d5-125">Připravenost k minifikaci snižuje funkce takto:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="6e6d5-126">Kromě odebrání komentáře a zbytečné mezery, byly přejmenovány následující názvy parametrů a proměnné následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="6e6d5-127">Původní</span><span class="sxs-lookup"><span data-stu-id="6e6d5-127">Original</span></span> | <span data-ttu-id="6e6d5-128">Přejmenovat</span><span class="sxs-lookup"><span data-stu-id="6e6d5-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="6e6d5-129">Dopad sdružování a minifikace</span><span class="sxs-lookup"><span data-stu-id="6e6d5-129">Impact of bundling and minification</span></span>

<span data-ttu-id="6e6d5-130">Následující tabulka popisuje rozdíly mezi jednotlivě načte prostředky a pomocí sdružování a minifikace:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="6e6d5-131">Akce</span><span class="sxs-lookup"><span data-stu-id="6e6d5-131">Action</span></span> | <span data-ttu-id="6e6d5-132">S B/M</span><span class="sxs-lookup"><span data-stu-id="6e6d5-132">With B/M</span></span> | <span data-ttu-id="6e6d5-133">Bez B/M</span><span class="sxs-lookup"><span data-stu-id="6e6d5-133">Without B/M</span></span> | <span data-ttu-id="6e6d5-134">Změna</span><span class="sxs-lookup"><span data-stu-id="6e6d5-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="6e6d5-135">Požadavky na soubory</span><span class="sxs-lookup"><span data-stu-id="6e6d5-135">File Requests</span></span>  | <span data-ttu-id="6e6d5-136">7</span><span class="sxs-lookup"><span data-stu-id="6e6d5-136">7</span></span>   | <span data-ttu-id="6e6d5-137">18</span><span class="sxs-lookup"><span data-stu-id="6e6d5-137">18</span></span>     | <span data-ttu-id="6e6d5-138">157%</span><span class="sxs-lookup"><span data-stu-id="6e6d5-138">157%</span></span>
<span data-ttu-id="6e6d5-139">KB přenosu</span><span class="sxs-lookup"><span data-stu-id="6e6d5-139">KB Transferred</span></span> | <span data-ttu-id="6e6d5-140">156</span><span class="sxs-lookup"><span data-stu-id="6e6d5-140">156</span></span> | <span data-ttu-id="6e6d5-141">264.68</span><span class="sxs-lookup"><span data-stu-id="6e6d5-141">264.68</span></span> | <span data-ttu-id="6e6d5-142">70%</span><span class="sxs-lookup"><span data-stu-id="6e6d5-142">70%</span></span>
<span data-ttu-id="6e6d5-143">Čas načítání (ms)</span><span class="sxs-lookup"><span data-stu-id="6e6d5-143">Load Time (ms)</span></span> | <span data-ttu-id="6e6d5-144">885</span><span class="sxs-lookup"><span data-stu-id="6e6d5-144">885</span></span> | <span data-ttu-id="6e6d5-145">2360</span><span class="sxs-lookup"><span data-stu-id="6e6d5-145">2360</span></span>   | <span data-ttu-id="6e6d5-146">167%</span><span class="sxs-lookup"><span data-stu-id="6e6d5-146">167%</span></span>

<span data-ttu-id="6e6d5-147">Prohlížeče jsou poměrně podrobné s ohledem na hlavičky požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="6e6d5-148">Celkový počet bajtů odeslané metrika viděli k výraznému snížení při vytváření sady.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="6e6d5-149">Doba načtení ukazuje výrazným vylepšením, ale v tomto příkladu byl spuštěn místně.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="6e6d5-150">Větší zvýšení výkonu jsou realizované při použití sdružování a minifikace s prostředky přenést přes síť.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="6e6d5-151">Volba strategie sdružování a minifikace</span><span class="sxs-lookup"><span data-stu-id="6e6d5-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="6e6d5-152">Šablony projektů MVC a stránky Razor poskytují out-of-the-box řešení pro sdružování a minifikace skládající se z konfiguračního souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="6e6d5-153">Nástroje třetích stran, jako [Gulp](xref:client-side/using-gulp) a [Grunt](xref:client-side/using-grunt) spouštěčů úloh, provádět stejné úlohy s o něco složitější.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="6e6d5-154">Nástroj třetí strany je skvěle hodí, když pracovního postupu vývoje vyžaduje zpracování nad rámec sdružování a minifikace&mdash;jako je optimalizace linting a image.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="6e6d5-155">Pomocí sdružování a minifikace návrhu minifikovaný soubory vytvořené před nasazením aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="6e6d5-156">Sdružování a minifikace před nasazením poskytuje výhodu v podobě zatížení serveru.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="6e6d5-157">Ale je důležité uvědomit si tohoto návrhu sdružování a minifikace zvyšuje složitost sestavení a funguje jenom se statickými soubory.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="6e6d5-158">Konfigurace sdružování a minifikace</span><span class="sxs-lookup"><span data-stu-id="6e6d5-158">Configure bundling and minification</span></span>

<span data-ttu-id="6e6d5-159">Zadejte šablon projektu MVC a Razor Pages *bundleconfig.json* konfigurační soubor, který definuje možnosti pro každou sadu.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="6e6d5-160">Ve výchozím nastavení, konfigurace jedné sadě je definována pro vlastní jazyk JavaScript (*wwwroot/js/site.js*) a šablony stylů (*wwwroot/css/site.css*) soubory:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="6e6d5-161">Možnosti konfigurace patří:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-161">Configuration options include:</span></span>

* <span data-ttu-id="6e6d5-162">`outputFileName`: Název souboru svazku do výstupu.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="6e6d5-163">Může obsahovat relativní cestu z *bundleconfig.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="6e6d5-164">**Vyžaduje**</span><span class="sxs-lookup"><span data-stu-id="6e6d5-164">**required**</span></span>
* <span data-ttu-id="6e6d5-165">`inputFiles`: Pole souborů, které mají spojit dohromady.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="6e6d5-166">Jedná se o relativní cesty ke konfiguračnímu souboru.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="6e6d5-167">**volitelné**, \* prázdnou hodnotu výsledkem prázdná výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-167">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="6e6d5-168">[podpory zástupných znaků](http://www.tldp.org/LDP/abs/html/globbingref.html) vzory jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="6e6d5-169">`minify`Možnosti výstupního typu: připravenost k minifikaci.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="6e6d5-170">**volitelné**, *výchozí – `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="6e6d5-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="6e6d5-171">Možnosti konfigurace jsou k dispozici na typ výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="6e6d5-172">Minifier šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="6e6d5-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="6e6d5-173">Minifier jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="6e6d5-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="6e6d5-174">HTML Minifier</span><span class="sxs-lookup"><span data-stu-id="6e6d5-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="6e6d5-175">`includeInProject`: Příznak označující, jestli se má přidat generované soubory do souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="6e6d5-176">**volitelné**, *výchozí – false*</span><span class="sxs-lookup"><span data-stu-id="6e6d5-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="6e6d5-177">`sourceMap`: Příznak označující, jestli se má generovat mapy zdrojového souboru jako součást balíčku.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="6e6d5-178">**volitelné**, *výchozí – false*</span><span class="sxs-lookup"><span data-stu-id="6e6d5-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="6e6d5-179">`sourceMapRootPath`: Kořenová cesta pro ukládání vytvořeném zdrojovém souboru mapy.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="6e6d5-180">Čas sestavení provádění sdružování a minifikace</span><span class="sxs-lookup"><span data-stu-id="6e6d5-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="6e6d5-181">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) balíček NuGet umožňuje provádění sdružování a minifikace v okamžiku sestavení.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="6e6d5-182">Balíček vkládá [cíle nástroje MSBuild](/visualstudio/msbuild/msbuild-targets) kterého se spouští v sestavení a vyčištění čas.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="6e6d5-183">*Bundleconfig.json* soubor analyzován pomocí procesu sestavení pro vytvoření výstupních souborů na základě definované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="6e6d5-184">BuildBundlerMinifier patří do komunitní projekt na Githubu, pro které společnost Microsoft poskytuje bez podpory.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-184">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="6e6d5-185">Problémy měli archivované [tady](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="6e6d5-185">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e6d5-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e6d5-186">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6e6d5-187">Přidat *BuildBundlerMinifier* do svého projektu balíček.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-187">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="6e6d5-188">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-188">Build the project.</span></span> <span data-ttu-id="6e6d5-189">Tímto se zobrazí v okně výstupu:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-189">The following appears in the Output window:</span></span>

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

<span data-ttu-id="6e6d5-190">Vyčistěte projekt.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-190">Clean the project.</span></span> <span data-ttu-id="6e6d5-191">Tímto se zobrazí v okně výstupu:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-191">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6e6d5-192">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="6e6d5-192">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6e6d5-193">Přidat *BuildBundlerMinifier* do svého projektu balíček:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-193">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="6e6d5-194">Pokud používáte ASP.NET Core 1.x, obnovit nově přidané balíček:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-194">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="6e6d5-195">Sestavte projekt:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-195">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="6e6d5-196">Zobrazí se následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-196">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="6e6d5-197">Vyčistěte projekt:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-197">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="6e6d5-198">Zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-198">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="6e6d5-199">Ad hoc provádění sdružování a minifikace</span><span class="sxs-lookup"><span data-stu-id="6e6d5-199">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="6e6d5-200">Je možné ke spouštění úloh sdružování a minifikace na základě ad hoc bez sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-200">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="6e6d5-201">Přidat [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) do svého projektu balíček NuGet:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-201">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="6e6d5-202">BundlerMinifier.Core patří do komunitní projekt na Githubu, pro které společnost Microsoft poskytuje bez podpory.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-202">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="6e6d5-203">Problémy měli archivované [tady](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="6e6d5-203">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="6e6d5-204">Tento balíček rozšiřuje rozhraní příkazového řádku .NET Core mají zahrnout *dotnet sady* nástroj.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-204">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="6e6d5-205">V okně konzoly Správce balíčků (PMC) nebo v příkazovém řádku, mohou být provedeny následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-205">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="6e6d5-206">Správce balíčků NuGet přidá závislosti jako soubory \*.csproj `<PackageReference />` uzly.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-206">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="6e6d5-207">`dotnet bundle` Příkaz je registrovaný pomocí rozhraní příkazového řádku .NET Core pouze tehdy, když `<DotNetCliToolReference />` uzel se používá.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-207">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="6e6d5-208">Upravte soubor \*.csproj odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-208">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="6e6d5-209">Přidání souborů do pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="6e6d5-209">Add files to workflow</span></span>

<span data-ttu-id="6e6d5-210">Vezměte v úvahu příklad, ve kterém Další *custom.css* přidá soubor podobné následující:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-210">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="6e6d5-211">K minifikaci *custom.css* a zabalí jej *site.css* do *site.min.css* přidejte relativní cesta k *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-211">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="6e6d5-212">Případně může použít následující vzor podpory zástupných znaků:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-212">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="6e6d5-213">Tento model podpory zástupných znaků vyhledá všechny soubory šablon stylů CSS a vyloučí vzor minifikovaný souboru.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-213">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="6e6d5-214">Sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-214">Build the application.</span></span> <span data-ttu-id="6e6d5-215">Otevřít *site.min.css* a Všimněte si, že obsah *custom.css* je připojen na konec souboru.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-215">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="6e6d5-216">Prostředí založené na sdružování a minifikace</span><span class="sxs-lookup"><span data-stu-id="6e6d5-216">Environment-based bundling and minification</span></span>

<span data-ttu-id="6e6d5-217">Jako osvědčený postup by měla sloužit jako součást balíčku a minifikovaný soubory vaší aplikace v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-217">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="6e6d5-218">Během vývoje zkontrolujte původní soubory pro snazší ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-218">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="6e6d5-219">Zadejte soubory, které chcete zahrnout do stránky s použitím [pomocná rutina značky prostředí](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-219">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="6e6d5-220">Pomocná rutina značky prostředí pouze vykreslí obsah při spuštění v konkrétní [prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="6e6d5-220">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="6e6d5-221">Následující `environment` značky vykreslí nezpracovaných souborů CSS, při spuštění v `Development` prostředí:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-221">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6e6d5-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6e6d5-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6e6d5-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6e6d5-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

<span data-ttu-id="6e6d5-224">Následující `environment` značky vykreslí soubory šablon stylů CSS jako součást balíčku a minifikovaný při spuštění v prostředí než `Development`.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-224">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="6e6d5-225">Například používané `Production` nebo `Staging` aktivuje vykreslování tyto šablony stylů:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-225">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6e6d5-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6e6d5-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6e6d5-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6e6d5-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="6e6d5-228">Využívání bundleconfig.json z Gulp</span><span class="sxs-lookup"><span data-stu-id="6e6d5-228">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="6e6d5-229">Existují případy, ve kterých aplikace sdružování a minifikace pracovní postup vyžaduje další zpracování.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-229">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="6e6d5-230">Mezi příklady patří optimalizovat image, nejnovějších mezipaměti a CDN asset zpracování.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-230">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="6e6d5-231">Chcete-li tyto požadavky splňují, můžete převést sdružování a minifikace pracovní postup použití nástroje Gulp.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-231">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="6e6d5-232">Použití rozšíření například položky Bundler & Minifier</span><span class="sxs-lookup"><span data-stu-id="6e6d5-232">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="6e6d5-233">Visual Studio [například položky Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) rozšíření zpracovává server převod na Gulp.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-233">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="6e6d5-234">Rozšíření například položky Bundler & Minifier patří do komunitní projekt na Githubu, pro které společnost Microsoft poskytuje bez podpory.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-234">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="6e6d5-235">Problémy měli archivované [tady](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="6e6d5-235">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="6e6d5-236">Klikněte pravým tlačítkem myši *bundleconfig.json* v Průzkumníku řešení a vyberte možnost **například položky Bundler & Minifier** > **převést na Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="6e6d5-236">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Převést na Gulp položka kontextové nabídky](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="6e6d5-238">*Gulpfile.js* a *package.json* soubory budou přidány do projektu.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-238">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="6e6d5-239">Podpůrné [npm](https://www.npmjs.com/) balíčky uvedené v *package.json* souboru `devDependencies` bodu jsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-239">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="6e6d5-240">Spusťte následující příkaz v okně konzole PMC k instalaci rozhraní příkazového řádku Gulp jako globální závislost:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-240">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="6e6d5-241">*Gulpfile.js* čtení souborů *bundleconfig.json* soubor pro vstupy, výstupy a nastavení.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-241">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="6e6d5-242">Převést ručně</span><span class="sxs-lookup"><span data-stu-id="6e6d5-242">Convert manually</span></span>

<span data-ttu-id="6e6d5-243">Pokud nejsou k dispozici sady Visual Studio nebo rozšíření například položky Bundler & Minifier, převeďte ručně.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-243">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="6e6d5-244">Přidat *package.json* souboru následujícím kódem `devDependencies`, do kořenového adresáře projektu:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-244">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="6e6d5-245">Nainstalovat závislosti spuštěním následujícího příkazu na stejné úrovni jako *package.json*:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-245">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="6e6d5-246">Instalace rozhraní příkazového řádku Gulp jako globální závislost:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-246">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="6e6d5-247">Kopírovat *gulpfile.js* souboru pod do kořenového adresáře projektu:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-247">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="6e6d5-248">Spuštění úlohy Gulp</span><span class="sxs-lookup"><span data-stu-id="6e6d5-248">Run Gulp tasks</span></span>

<span data-ttu-id="6e6d5-249">K aktivaci úlohy Gulp připravenost k minifikaci, než sestavení projektu v sadě Visual Studio, přidejte následující [cíl nástroje MSBuild](/visualstudio/msbuild/msbuild-targets) \*.csproj souboru:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-249">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="6e6d5-250">V tomto příkladu všechny úkoly definované v rámci `MyPreCompileTarget` cíl spuštění před předdefinovaného `Build` cíl.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-250">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="6e6d5-251">V okně výstupu sady Visual Studio se zobrazí výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="6e6d5-251">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

<span data-ttu-id="6e6d5-252">Visual Studio Task Runner Explorer lze také vázat úlohy Gulp k určité události aplikace Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-252">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="6e6d5-253">Zobrazit [spuštění úlohy výchozí](xref:client-side/using-gulp#running-default-tasks) pokyny týkající se toho.</span><span class="sxs-lookup"><span data-stu-id="6e6d5-253">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6e6d5-254">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6e6d5-254">Additional resources</span></span>

* [<span data-ttu-id="6e6d5-255">Použití nástroje Gulp</span><span class="sxs-lookup"><span data-stu-id="6e6d5-255">Use Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="6e6d5-256">Použití nástroje Grunt</span><span class="sxs-lookup"><span data-stu-id="6e6d5-256">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="6e6d5-257">Používání více prostředí</span><span class="sxs-lookup"><span data-stu-id="6e6d5-257">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="6e6d5-258">Pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="6e6d5-258">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
