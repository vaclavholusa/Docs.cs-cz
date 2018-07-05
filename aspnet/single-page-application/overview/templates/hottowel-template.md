---
uid: single-page-application/overview/templates/hottowel-template
title: Šablona Hot Towel | Dokumentace Microsoftu
author: madskristensen
description: HotTowel šablony
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: de81f12f57d7f2fb7c6478bfa1f3a278ae905a39
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388535"
---
<a name="hot-towel-template"></a><span data-ttu-id="7e740-103">Šablona Hot Towel</span><span class="sxs-lookup"><span data-stu-id="7e740-103">Hot Towel template</span></span>
====================
<span data-ttu-id="7e740-104">podle [Autor: Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="7e740-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="7e740-105">John Papa zapisuje horké šablonu ručníků MVC</span><span class="sxs-lookup"><span data-stu-id="7e740-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="7e740-106">Vyberte, kterou verzi ke stažení:</span><span class="sxs-lookup"><span data-stu-id="7e740-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="7e740-107">Šablona Hot Towel MVC pro sadu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7e740-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="7e740-108">Šablona Hot Towel MVC pro Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7e740-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="7e740-109">Hot Towel: Vzhledem k tomu, že nechcete přejděte do aplikace SPA bez jediného!</span><span class="sxs-lookup"><span data-stu-id="7e740-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="7e740-110">Chcete sestavit SPA, ale nelze rozhodnout, kde začít?</span><span class="sxs-lookup"><span data-stu-id="7e740-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="7e740-111">Použít Hot Towel a během několika sekund budete mít SPA a všechny nástroje, které potřebujete k vytváření na ní!</span><span class="sxs-lookup"><span data-stu-id="7e740-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="7e740-112">Hot Towel vytvoří skvělý výchozí bod pro vytváření jedné stránce aplikace (SPA) pomocí technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7e740-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="7e740-113">Okamžité vám nabízí modulární struktura kódu, navigace zobrazení, vytváření datových vazeb, správu velké množství dat a používání stylů pro jednoduché, ale elegantní.</span><span class="sxs-lookup"><span data-stu-id="7e740-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="7e740-114">Hot Towel nabízí vše, co potřebujete k vytváření jednostránková aplikace, abyste se mohli soustředit na svou aplikaci, nikoli zajistí funkčnost systému.</span><span class="sxs-lookup"><span data-stu-id="7e740-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="7e740-115">Další informace o vytváření SPA z [John Papa videa, kurzy a kurzů Pluralsight](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="7e740-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="7e740-116">Struktury aplikace</span><span class="sxs-lookup"><span data-stu-id="7e740-116">Application Structure</span></span>

<span data-ttu-id="7e740-117">Hot Towel SPA obsahuje složku aplikace, který obsahuje soubory jazyka JavaScript a HTML, které definují aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7e740-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="7e740-118">Ve složce aplikace:</span><span class="sxs-lookup"><span data-stu-id="7e740-118">Inside the App folder:</span></span>

- <span data-ttu-id="7e740-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="7e740-119">durandal</span></span>
- <span data-ttu-id="7e740-120">služby</span><span class="sxs-lookup"><span data-stu-id="7e740-120">services</span></span>
- <span data-ttu-id="7e740-121">modely viewmodels</span><span class="sxs-lookup"><span data-stu-id="7e740-121">viewmodels</span></span>
- <span data-ttu-id="7e740-122">zobrazení</span><span class="sxs-lookup"><span data-stu-id="7e740-122">views</span></span>

<span data-ttu-id="7e740-123">Složky aplikace obsahuje kolekci modulů.</span><span class="sxs-lookup"><span data-stu-id="7e740-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="7e740-124">Tyto moduly zapouzdřují funkce a deklaraci závislosti na dalších modulech.</span><span class="sxs-lookup"><span data-stu-id="7e740-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="7e740-125">Zobrazení složky obsahuje kód HTML pro vaši aplikaci a modely viewmodels složka obsahuje prezentaci logiku pro zobrazení (běžný vzor MVVM).</span><span class="sxs-lookup"><span data-stu-id="7e740-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="7e740-126">Složka služby je ideální pro bydlení žádné společné služby, které vaše aplikace může potřebovat například načítání dat protokolu HTTP nebo místní úložiště interakce.</span><span class="sxs-lookup"><span data-stu-id="7e740-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="7e740-127">Je běžné, že více modely viewmodels opětovné použití kódu z modulů služby.</span><span class="sxs-lookup"><span data-stu-id="7e740-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="7e740-128">Struktury aplikace na straně serveru ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7e740-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="7e740-129">Hot Towel staví na zkušenosti a výkonné strukturu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7e740-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="7e740-130">Aplikace\_Start</span><span class="sxs-lookup"><span data-stu-id="7e740-130">App\_Start</span></span>
- <span data-ttu-id="7e740-131">Obsah</span><span class="sxs-lookup"><span data-stu-id="7e740-131">Content</span></span>
- <span data-ttu-id="7e740-132">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="7e740-132">Controllers</span></span>
- <span data-ttu-id="7e740-133">Modely</span><span class="sxs-lookup"><span data-stu-id="7e740-133">Models</span></span>
- <span data-ttu-id="7e740-134">Skripty</span><span class="sxs-lookup"><span data-stu-id="7e740-134">Scripts</span></span>
- <span data-ttu-id="7e740-135">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="7e740-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="7e740-136">Vybrané knihovny</span><span class="sxs-lookup"><span data-stu-id="7e740-136">Featured Libraries</span></span>

- <span data-ttu-id="7e740-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7e740-137">ASP.NET MVC</span></span>
- <span data-ttu-id="7e740-138">Rozhraní API pro ASP.NET Web</span><span class="sxs-lookup"><span data-stu-id="7e740-138">ASP.NET Web API</span></span>
- <span data-ttu-id="7e740-139">Optimalizace prostředí ASP.NET – sdružování a minifikace</span><span class="sxs-lookup"><span data-stu-id="7e740-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="7e740-140">[Breeze.js](http://Breezejs.com) -velké množství dat správy</span><span class="sxs-lookup"><span data-stu-id="7e740-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="7e740-141">[Durandal.js](http://Durandaljs.com) – procházení a zobrazení sestavení</span><span class="sxs-lookup"><span data-stu-id="7e740-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="7e740-142">[Rozhraní Knockout.js](http://Knockoutjs.com) – datové vazby</span><span class="sxs-lookup"><span data-stu-id="7e740-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="7e740-143">[Require.js](http://requirejs.org) -modularitu AMD a optimalizace</span><span class="sxs-lookup"><span data-stu-id="7e740-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="7e740-144">[Toastr.js](http://jpapa.me/c7toastr) – místní zprávy</span><span class="sxs-lookup"><span data-stu-id="7e740-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="7e740-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) – robustní stylu CSS</span><span class="sxs-lookup"><span data-stu-id="7e740-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="7e740-146">Instalace přes SPA šablona Hot Towel Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7e740-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="7e740-147">Hot Towel je možné nainstalovat jako šablony sady Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="7e740-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="7e740-148">Stačí kliknout na `File`  |  `New Project` a zvolte `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="7e740-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="7e740-149">Vyberte "Hot Towel jednostránkové aplikace" šablony a spusťte!</span><span class="sxs-lookup"><span data-stu-id="7e740-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="7e740-150">Instalace prostřednictvím balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="7e740-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="7e740-151">Hot Towel je také balíček NuGet, která rozšiřuje stávající prázdný projekt ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7e740-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="7e740-152">Stačí nainstalovat pomocí nástroje Nuget a spusťte!</span><span class="sxs-lookup"><span data-stu-id="7e740-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="7e740-153">Jak vytvořit na Hot Towel</span><span class="sxs-lookup"><span data-stu-id="7e740-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="7e740-154">Začněte jednoduše přidávat kód!</span><span class="sxs-lookup"><span data-stu-id="7e740-154">Simply start adding code!</span></span>

1. <span data-ttu-id="7e740-155">Přidejte vlastní kód na straně serveru, pokud možno Entity Framework a WebAPI (což ve skutečnosti lesku pomocí Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="7e740-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="7e740-156">Přidat zobrazení `App/views` složky</span><span class="sxs-lookup"><span data-stu-id="7e740-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="7e740-157">Přidat modely viewmodels k `App/viewmodels` složky</span><span class="sxs-lookup"><span data-stu-id="7e740-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="7e740-158">Přidání kódu HTML a Knockout datové vazby k novým zobrazením</span><span class="sxs-lookup"><span data-stu-id="7e740-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="7e740-159">Aktualizace tras navigace v `shell.js`</span><span class="sxs-lookup"><span data-stu-id="7e740-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="7e740-160">Návod, HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="7e740-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="7e740-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="7e740-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="7e740-162">index.cshtml je výchozí trasy a zobrazení pro aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="7e740-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="7e740-163">Obsahuje všechny značky meta pro standardní, odkazy šablon stylů css a JavaScript odkazy, které očekáváte.</span><span class="sxs-lookup"><span data-stu-id="7e740-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="7e740-164">Text obsahuje jediný `<div>` což je, kde veškerý obsah (zobrazení) budou umístěné při jsou požadovány.</span><span class="sxs-lookup"><span data-stu-id="7e740-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="7e740-165">`@Scripts.Render` Používá ke spuštění úvodní bod pro kód aplikace, která je obsažena v Require.js `main.js` souboru.</span><span class="sxs-lookup"><span data-stu-id="7e740-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="7e740-166">Úvodní obrazovka dostane ukazují, jak vytvořit úvodní obrazovky při načtení aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e740-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="7e740-167">App/main.js</span><span class="sxs-lookup"><span data-stu-id="7e740-167">App/main.js</span></span>

<span data-ttu-id="7e740-168">`main.js` Soubor obsahuje kód, který se spustí poté, co vaše aplikace je načtena.</span><span class="sxs-lookup"><span data-stu-id="7e740-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="7e740-169">Je to, kde chcete definovat navigace trasy, nastavte vaše spuštění zobrazení a provést libovolné instalační/spuštění například Příprava dat vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e740-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="7e740-170">`main.js` Soubor definuje několik modulů durandal ke spuštění aplikace spustit.</span><span class="sxs-lookup"><span data-stu-id="7e740-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="7e740-171">Příkaz definovat pomáhá vyřešit závislosti modulů, aby byly k dispozici pro funkci.</span><span class="sxs-lookup"><span data-stu-id="7e740-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="7e740-172">Nejdříve jsou povoleny zprávy ladění, které odesílat zprávy o událostech, že aplikace provádí v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="7e740-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="7e740-173">Kód app.start říká rozhraní framework durandal ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e740-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="7e740-174">Konvence jsou nastavené tak, aby durandal pozná, všechna zobrazení a modely viewmodels jsou obsaženy ve stejné složce s názvem.</span><span class="sxs-lookup"><span data-stu-id="7e740-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="7e740-175">Nakonec `app.setRoot` Marku zatížení `shell` pomocí předdefinované šablony `entrance` animace.</span><span class="sxs-lookup"><span data-stu-id="7e740-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="7e740-176">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="7e740-176">Views</span></span>

<span data-ttu-id="7e740-177">Zobrazení jsou součástí `App/views` složky.</span><span class="sxs-lookup"><span data-stu-id="7e740-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="7e740-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="7e740-178">shell.html</span></span>

<span data-ttu-id="7e740-179">`shell.html` Obsahuje hlavní rozložení pro kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="7e740-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="7e740-180">Všechny ostatní zobrazení se skládá někde v části vašeho `shell` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7e740-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="7e740-181">Poskytuje Hot Towel `shell` tři tyto oblasti: zápatí, záhlaví a oblast obsahu.</span><span class="sxs-lookup"><span data-stu-id="7e740-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="7e740-182">Každá z těchto oblastí je načtena s tvoří jiných zobrazení při požadavku na obsah.</span><span class="sxs-lookup"><span data-stu-id="7e740-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="7e740-183">`compose` Vazby pro záhlaví a zápatí nich není pevně nastavená v Hot Towel přejděte `nav` a `footer` zobrazení, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="7e740-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="7e740-184">Vytvořit vazbu pro oddíl `#content` je vázán na `router` modulu aktivní položky.</span><span class="sxs-lookup"><span data-stu-id="7e740-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="7e740-185">Jinými slovy po kliknutí na navigační odkaz je odpovídající zobrazení je načten v této oblasti.</span><span class="sxs-lookup"><span data-stu-id="7e740-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="7e740-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="7e740-186">nav.html</span></span>

<span data-ttu-id="7e740-187">`nav.html` Obsahuje odkazy pro tato jednostránková aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e740-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="7e740-188">To je, kde struktura nabídky je možné použít, třeba.</span><span class="sxs-lookup"><span data-stu-id="7e740-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="7e740-189">Často je jím data vázaná (pomocí Knockout) k `router` modul zobrazení navigace, kterou jste definovali v `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="7e740-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="7e740-190">Knockout vyhledá data-bind atributy a aby vazba `shell` viewmodel, aby se zobrazovaly trasy navigace a chcete-li zobrazit ukazatel průběhu (pomocí architekturu Twitter Bootstrap) Pokud `router` modul provádí navigaci z jednoho zobrazení do jiného (viz `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="7e740-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="7e740-191">Home.HTML a details.html</span><span class="sxs-lookup"><span data-stu-id="7e740-191">home.html and details.html</span></span>

<span data-ttu-id="7e740-192">Tato zobrazení obsahují kód jazyka HTML pro vlastní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7e740-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="7e740-193">Když `home` odkaz v `nav` dojde ke kliknutí na nabídky zobrazení, `home` zobrazení se umístí do oblasti obsahu `shell` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7e740-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="7e740-194">Tato zobrazení můžete rozšířit nebo nahradit vlastní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7e740-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="7e740-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="7e740-195">footer.html</span></span>

<span data-ttu-id="7e740-196">`footer.html` Obsahuje kód HTML, který se zobrazí v zápatí, v dolní části `shell` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7e740-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="7e740-197">Modely ViewModels</span><span class="sxs-lookup"><span data-stu-id="7e740-197">ViewModels</span></span>

<span data-ttu-id="7e740-198">Modely ViewModel se nacházejí v `App/viewmodels` složky.</span><span class="sxs-lookup"><span data-stu-id="7e740-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="7e740-199">shell.js</span><span class="sxs-lookup"><span data-stu-id="7e740-199">shell.js</span></span>

<span data-ttu-id="7e740-200">`shell` Viewmodel obsahuje vlastnosti a funkce, které jsou vázány `shell` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7e740-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="7e740-201">Často je jím nalezených vazby navigační nabídce (viz `router.mapNav` logiky).</span><span class="sxs-lookup"><span data-stu-id="7e740-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="7e740-202">Home.js a details.js</span><span class="sxs-lookup"><span data-stu-id="7e740-202">home.js and details.js</span></span>

<span data-ttu-id="7e740-203">Tyto modely viewmodels obsahovat vlastností a funkcí, které jsou vázány `home` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7e740-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="7e740-204">také obsahuje prezentaci logiku pro zobrazení a je skutečným pojidlem mezi daty a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7e740-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="7e740-205">Služby</span><span class="sxs-lookup"><span data-stu-id="7e740-205">Services</span></span>

<span data-ttu-id="7e740-206">Služby se nacházejí ve složce aplikace nebo služeb Team Foundation.</span><span class="sxs-lookup"><span data-stu-id="7e740-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="7e740-207">V ideálním případě můžete umístit vaše budoucí služby jako službě dataservice modul, který je zodpovědný za načtení a publikování vzdálených dat.</span><span class="sxs-lookup"><span data-stu-id="7e740-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="7e740-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="7e740-208">logger.js</span></span>

<span data-ttu-id="7e740-209">Poskytuje Hot Towel `logger` modulu ve složce služby.</span><span class="sxs-lookup"><span data-stu-id="7e740-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="7e740-210">`logger` Modul je ideální pro protokolování zpráv do konzoly a pro uživatele v místní informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="7e740-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
