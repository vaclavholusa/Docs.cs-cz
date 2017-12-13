---
uid: single-page-application/overview/templates/hottowel-template
title: "Aktivní ručníků šablony | Microsoft Docs"
author: madskristensen
description: "HotTowel šablony"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: bfc6e2c884c422f44e8be5f4f29554ae86f7ecb6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="hot-towel-template"></a><span data-ttu-id="317b9-103">Aktivní ručníků šablony</span><span class="sxs-lookup"><span data-stu-id="317b9-103">Hot Towel template</span></span>
====================
<span data-ttu-id="317b9-104">podle [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="317b9-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="317b9-105">Aktivní šablony MVC ručníků jsou zapsána Papa Jan</span><span class="sxs-lookup"><span data-stu-id="317b9-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="317b9-106">Vyberte, kterou verzi ke stažení:</span><span class="sxs-lookup"><span data-stu-id="317b9-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="317b9-107">Šablony MVC aktivní ručníků pro sadu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="317b9-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="317b9-108">Šablony MVC aktivní ručníků pro Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="317b9-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)


> <span data-ttu-id="317b9-109">Aktivní ručníků: Vzhledem k tomu, že nechcete, aby přejít na SPA bez jeden!</span><span class="sxs-lookup"><span data-stu-id="317b9-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="317b9-110">Chcete vytvářet SPA, ale nemůže rozhodnout, kde začít?</span><span class="sxs-lookup"><span data-stu-id="317b9-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="317b9-111">Použít aktivní ručníků a v sekundách budete mít SPA a všechny nástroje, které potřebujete k vytváření na něm!</span><span class="sxs-lookup"><span data-stu-id="317b9-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="317b9-112">Aktivní ručníků vytvoří skvělý výchozí bod pro sestavení jedné stránce aplikace (SPA) s technologií ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="317b9-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="317b9-113">Ihned můžete poskytuje modulární struktura pro kód, zobrazení navigační, vazby dat, Správa bohaté dat a jednoduchý, ale elegantní stylů.</span><span class="sxs-lookup"><span data-stu-id="317b9-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="317b9-114">Aktivní ručníků poskytuje vše, co potřebujete k vytvoření SPA, abyste se mohli zaměřit na aplikace, není vložení.</span><span class="sxs-lookup"><span data-stu-id="317b9-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="317b9-115">Další informace o vytváření SPA z [Jan Papa videa, kurzy a kurzů Pluralsight](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="317b9-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="317b9-116">Struktura aplikace</span><span class="sxs-lookup"><span data-stu-id="317b9-116">Application Structure</span></span>

<span data-ttu-id="317b9-117">Aktivní ručníků SPA poskytuje aplikaci složku, která obsahuje soubory JavaScript a HTML, které definují aplikaci.</span><span class="sxs-lookup"><span data-stu-id="317b9-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="317b9-118">Ve složce aplikace:</span><span class="sxs-lookup"><span data-stu-id="317b9-118">Inside the App folder:</span></span>

- <span data-ttu-id="317b9-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="317b9-119">durandal</span></span>
- <span data-ttu-id="317b9-120">služby</span><span class="sxs-lookup"><span data-stu-id="317b9-120">services</span></span>
- <span data-ttu-id="317b9-121">viewmodels</span><span class="sxs-lookup"><span data-stu-id="317b9-121">viewmodels</span></span>
- <span data-ttu-id="317b9-122">zobrazení</span><span class="sxs-lookup"><span data-stu-id="317b9-122">views</span></span>

<span data-ttu-id="317b9-123">Složky aplikace obsahuje kolekci modulů.</span><span class="sxs-lookup"><span data-stu-id="317b9-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="317b9-124">Tyto moduly zapouzdření funkce a na dalších modulů deklarovat závislosti.</span><span class="sxs-lookup"><span data-stu-id="317b9-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="317b9-125">Složka zobrazení HTML pro vaši aplikaci a složce viewmodels obsahuje prezentační logiku pro zobrazení (běžný vzor modelem MVVM).</span><span class="sxs-lookup"><span data-stu-id="317b9-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="317b9-126">Složka služby je ideální pro nachází běžné služby, které vaše aplikace může být nutné například načítání dat protokolu HTTP nebo interakci s místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="317b9-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="317b9-127">Chcete-li znovu použít kód z modulů služby je běžné pro více viewmodels.</span><span class="sxs-lookup"><span data-stu-id="317b9-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="317b9-128">Struktury aplikace straně serveru ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="317b9-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="317b9-129">Aktivní ručníků staví na strukturu známé a výkonné rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="317b9-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="317b9-130">Aplikace\_Start</span><span class="sxs-lookup"><span data-stu-id="317b9-130">App\_Start</span></span>
- <span data-ttu-id="317b9-131">Obsah</span><span class="sxs-lookup"><span data-stu-id="317b9-131">Content</span></span>
- <span data-ttu-id="317b9-132">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="317b9-132">Controllers</span></span>
- <span data-ttu-id="317b9-133">Modely</span><span class="sxs-lookup"><span data-stu-id="317b9-133">Models</span></span>
- <span data-ttu-id="317b9-134">Skripty</span><span class="sxs-lookup"><span data-stu-id="317b9-134">Scripts</span></span>
- <span data-ttu-id="317b9-135">zobrazení</span><span class="sxs-lookup"><span data-stu-id="317b9-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="317b9-136">Vybrané knihovny</span><span class="sxs-lookup"><span data-stu-id="317b9-136">Featured Libraries</span></span>

- <span data-ttu-id="317b9-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="317b9-137">ASP.NET MVC</span></span>
- <span data-ttu-id="317b9-138">Rozhraní API pro ASP.NET Web</span><span class="sxs-lookup"><span data-stu-id="317b9-138">ASP.NET Web API</span></span>
- <span data-ttu-id="317b9-139">ASP.NET – webové optimalizace - sdružování a minimalizace</span><span class="sxs-lookup"><span data-stu-id="317b9-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="317b9-140">[Breeze.js](http://Breezejs.com) -Správa bohaté dat</span><span class="sxs-lookup"><span data-stu-id="317b9-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="317b9-141">[Durandal.js](http://Durandaljs.com) -navigaci a zobrazení složení</span><span class="sxs-lookup"><span data-stu-id="317b9-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="317b9-142">[Knockout.js](http://Knockoutjs.com) -datové vazby</span><span class="sxs-lookup"><span data-stu-id="317b9-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="317b9-143">[Require.js](http://requirejs.org) -modularitu s AMD a optimalizace</span><span class="sxs-lookup"><span data-stu-id="317b9-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="317b9-144">[Toastr.js](http://jpapa.me/c7toastr) -automaticky otevírané okno zprávy</span><span class="sxs-lookup"><span data-stu-id="317b9-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="317b9-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) – robustní stylu CSS</span><span class="sxs-lookup"><span data-stu-id="317b9-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="317b9-146">Instalace pomocí šablony sady Visual Studio 2012 aktivní ručníků SPA</span><span class="sxs-lookup"><span data-stu-id="317b9-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="317b9-147">Aktivní ručníků je možné nainstalovat jako šablony sady Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="317b9-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="317b9-148">Stačí kliknout na `File`  |  `New Project` a zvolte `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="317b9-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="317b9-149">Pak vyberte ' aktivní ručníků jedné stránky aplikace "šablony a spusťte!</span><span class="sxs-lookup"><span data-stu-id="317b9-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="317b9-150">Instalace prostřednictvím balíčku NuGet</span><span class="sxs-lookup"><span data-stu-id="317b9-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="317b9-151">Aktivní ručníků je také balíčku NuGet, který rozšiřuje existujícího projektu ASP.NET MVC prázdný.</span><span class="sxs-lookup"><span data-stu-id="317b9-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="317b9-152">Stačí nainstalovat pomocí nástroje Nuget a spusťte!</span><span class="sxs-lookup"><span data-stu-id="317b9-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="317b9-153">Jak vytvářet na aktivní ručníků?</span><span class="sxs-lookup"><span data-stu-id="317b9-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="317b9-154">Jednoduše spustíte, přidání kódu!</span><span class="sxs-lookup"><span data-stu-id="317b9-154">Simply start adding code!</span></span>

1. <span data-ttu-id="317b9-155">Přidejte vlastní serverový kód, pokud možno Entity Framework a WebAPI (které skutečně Vylepšete s Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="317b9-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="317b9-156">Přidat zobrazení, která `App/views` složky</span><span class="sxs-lookup"><span data-stu-id="317b9-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="317b9-157">Přidat viewmodels k `App/viewmodels` složky</span><span class="sxs-lookup"><span data-stu-id="317b9-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="317b9-158">Přidat kód HTML a Knockout vazby dat k novým zobrazením</span><span class="sxs-lookup"><span data-stu-id="317b9-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="317b9-159">Aktualizace tras navigace v`shell.js`</span><span class="sxs-lookup"><span data-stu-id="317b9-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="317b9-160">Návod, HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="317b9-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="317b9-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="317b9-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="317b9-162">index.cshtml je výchozí trasy a zobrazení pro aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="317b9-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="317b9-163">Obsahuje všechny značky meta pro standardní, odkazy šablon stylů css a JavaScript odkazy, které očekáváte.</span><span class="sxs-lookup"><span data-stu-id="317b9-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="317b9-164">Text obsahuje jediný `<div>` tedy, kde veškerý obsah (zobrazení) budou umístěné okamžiku vyžádání.</span><span class="sxs-lookup"><span data-stu-id="317b9-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="317b9-165">`@Scripts.Render` Require.js používá ke spuštění vstupu bod pro kód aplikace, který je součástí `main.js` souboru.</span><span class="sxs-lookup"><span data-stu-id="317b9-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="317b9-166">Úvodní obrazovka zajišťuje ukazují, jak vytvářet úvodní obrazovky při aplikace načte.</span><span class="sxs-lookup"><span data-stu-id="317b9-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="317b9-167">App/Main.js</span><span class="sxs-lookup"><span data-stu-id="317b9-167">App/main.js</span></span>

<span data-ttu-id="317b9-168">`main.js` Soubor obsahuje kód, který se spustí hned, jak vaše aplikace je načtena.</span><span class="sxs-lookup"><span data-stu-id="317b9-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="317b9-169">Toto je, kde chcete definovat navigační trasy, nastavte vaše po spuštění zobrazení a provést všechny instalační program nebo zavádění například priming data aplikace.</span><span class="sxs-lookup"><span data-stu-id="317b9-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="317b9-170">`main.js` Soubor definuje několik durandal na moduly, které pomůžou akci aplikace spustit.</span><span class="sxs-lookup"><span data-stu-id="317b9-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="317b9-171">Příkaz definovat pomůže vyřešit závislosti moduly, takže jsou k dispozici pro tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="317b9-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="317b9-172">Nejprve jsou povolené zprávy ladění, který odesílat zprávy o událostech, které aplikace provádí v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="317b9-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="317b9-173">Kód app.start informuje durandal framework a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="317b9-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="317b9-174">Se názvů jsou nastavené tak, aby durandal zná všechna zobrazení a viewmodels jsou součástí stejné s názvem složky, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="317b9-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="317b9-175">Nakonec `app.setRoot` zatížení se spustí `shell` pomocí předdefinované šablony `entrance` animace.</span><span class="sxs-lookup"><span data-stu-id="317b9-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="317b9-176">zobrazení</span><span class="sxs-lookup"><span data-stu-id="317b9-176">Views</span></span>

<span data-ttu-id="317b9-177">Zobrazení jsou součástí `App/views` složky.</span><span class="sxs-lookup"><span data-stu-id="317b9-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="317b9-178">Shell.HTML</span><span class="sxs-lookup"><span data-stu-id="317b9-178">shell.html</span></span>

<span data-ttu-id="317b9-179">`shell.html` Obsahuje hlavní rozložení pro kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="317b9-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="317b9-180">Všechna vaše zobrazení se skládá někde v straně vaší `shell` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="317b9-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="317b9-181">Poskytuje aktivní ručníků `shell` s tři tyto oblasti: zápatí, záhlaví a oblast obsahu.</span><span class="sxs-lookup"><span data-stu-id="317b9-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="317b9-182">Každý z těchto oblastí je načtena s dalšími zobrazeními vyžádání tvoří obsah.</span><span class="sxs-lookup"><span data-stu-id="317b9-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="317b9-183">`compose` Vazby pro záhlaví a zápatí se obtížně programového v aktivní ručníků tak, aby odkazoval `nav` a `footer` zobrazení, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="317b9-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="317b9-184">Vytvářené vazby pro oddíl `#content` je vázána `router` modulu active položky.</span><span class="sxs-lookup"><span data-stu-id="317b9-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="317b9-185">Jinými slovy po kliknutí na tlačítko je načtena navigační odkaz je odpovídající zobrazení v této oblasti.</span><span class="sxs-lookup"><span data-stu-id="317b9-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="317b9-186">NAV.HTML</span><span class="sxs-lookup"><span data-stu-id="317b9-186">nav.html</span></span>

<span data-ttu-id="317b9-187">`nav.html` Obsahuje navigační odkazy zabezpečené ověřování HESLA.</span><span class="sxs-lookup"><span data-stu-id="317b9-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="317b9-188">Toto je strukturu nabídky můžete umístění, například.</span><span class="sxs-lookup"><span data-stu-id="317b9-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="317b9-189">Často je jím data vázaná (pomocí Knockout) k `router` modulu zobrazení navigace definované v `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="317b9-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="317b9-190">Knockout vypadá pro atributy data-bind a aby se váže `shell` viewmodel k zobrazení navigační trasy a zobrazit komponenta progressbar (pomocí služby Twitter Bootstrap) Pokud `router` modulu zrovna navigaci z jednoho zobrazení na jiné (viz `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="317b9-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="317b9-191">Home.HTML a details.html</span><span class="sxs-lookup"><span data-stu-id="317b9-191">home.html and details.html</span></span>

<span data-ttu-id="317b9-192">Tato zobrazení obsahovat HTML pro vlastní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="317b9-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="317b9-193">Když `home` na odkaz v `nav` zobrazení nabídky po kliknutí na, `home` zobrazení bude uložena v oblasti obsahu `shell` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="317b9-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="317b9-194">Tato zobrazení můžete rozšířit nebo nahradit vlastní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="317b9-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="317b9-195">Footer.HTML</span><span class="sxs-lookup"><span data-stu-id="317b9-195">footer.html</span></span>

<span data-ttu-id="317b9-196">`footer.html` Obsahuje HTML, který se zobrazí v zápatí, v dolní části `shell` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="317b9-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="317b9-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="317b9-197">ViewModels</span></span>

<span data-ttu-id="317b9-198">ViewModels se nacházejí v `App/viewmodels` složky.</span><span class="sxs-lookup"><span data-stu-id="317b9-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="317b9-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="317b9-199">shell.js</span></span>

<span data-ttu-id="317b9-200">`shell` Viewmodel obsahuje vlastnosti a funkce, které jsou vázány `shell` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="317b9-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="317b9-201">Často je jím kde se nacházejí vazby navigační nabídky (viz `router.mapNav` logiku).</span><span class="sxs-lookup"><span data-stu-id="317b9-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="317b9-202">Home.js a details.js</span><span class="sxs-lookup"><span data-stu-id="317b9-202">home.js and details.js</span></span>

<span data-ttu-id="317b9-203">Tyto viewmodels obsahovat vlastnosti a funkce, které jsou vázány `home` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="317b9-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="317b9-204">také obsahuje prezentační logiku pro zobrazení a je pojidlem mezi daty a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="317b9-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="317b9-205">Služby</span><span class="sxs-lookup"><span data-stu-id="317b9-205">Services</span></span>

<span data-ttu-id="317b9-206">Služby se nacházejí ve složce aplikace nebo služby.</span><span class="sxs-lookup"><span data-stu-id="317b9-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="317b9-207">V ideálním případě vaše budoucí služby, jako je například dataservice modul, který zodpovídá za načítání a publikování vzdálených dat, může být umístěny.</span><span class="sxs-lookup"><span data-stu-id="317b9-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="317b9-208">Logger.js</span><span class="sxs-lookup"><span data-stu-id="317b9-208">logger.js</span></span>

<span data-ttu-id="317b9-209">Poskytuje aktivní ručníků `logger` modulu ve složce služby.</span><span class="sxs-lookup"><span data-stu-id="317b9-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="317b9-210">`logger` Modul je ideální pro protokolování zpráv do konzoly a uživateli v překryvu informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="317b9-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
