---
title: Vytvořit Krásný a pohotově reagujících weby s Bootstrap a ASP.NET Core
author: ardalis
description: Další informace o použití Bootstrap pro vývoj přizpůsobivý webové aplikace s ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bootstrap
ms.openlocfilehash: a11ed13c709830795ebfd0e658d3f2fd2fd5a458
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483708"
---
# <a name="build-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a><span data-ttu-id="e5aed-103">Vytvořit Krásný a pohotově reagujících weby s Bootstrap a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e5aed-103">Build beautiful, responsive sites with Bootstrap and ASP.NET Core</span></span>

<a name="bootstrap-index"></a>

<span data-ttu-id="e5aed-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e5aed-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e5aed-105">Bootstrap je aktuálně nejoblíbenější webové rozhraní pro vývoj přizpůsobivý webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="e5aed-105">Bootstrap is currently the most popular web framework for developing responsive web applications.</span></span> <span data-ttu-id="e5aed-106">Nabízí několik funkce a výhody, které může zvýšit vaši uživatelé zkušenosti s webu, ať už jste začínající na front-endu návrh a vývoj nebo odborníka.</span><span class="sxs-lookup"><span data-stu-id="e5aed-106">It offers a number of features and benefits that can improve your users' experience with your web site, whether you're a novice at front-end design and development or an expert.</span></span> <span data-ttu-id="e5aed-107">Bootstrap je nasazený jako sada souborů CSS a JavaScript a slouží k efektivní Nápověda škálování váš web nebo aplikaci z telefonů tablety pro stolní počítače.</span><span class="sxs-lookup"><span data-stu-id="e5aed-107">Bootstrap is deployed as a set of CSS and JavaScript files, and is designed to help your website or application scale efficiently from phones to tablets to desktops.</span></span>

## <a name="get-started"></a><span data-ttu-id="e5aed-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="e5aed-108">Get started</span></span>

<span data-ttu-id="e5aed-109">Existuje několik způsobů, jak začít s Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="e5aed-109">There are several ways to get started with Bootstrap.</span></span> <span data-ttu-id="e5aed-110">Pokud začínáte novou webovou aplikaci v sadě Visual Studio, můžete výchozí šablona starter pro ASP.NET Core, ve kterém budou pocházet případu Bootstrap předem nainstalovaná:</span><span class="sxs-lookup"><span data-stu-id="e5aed-110">If you're starting a new web application in Visual Studio, you can choose the default starter template for ASP.NET Core, in which case Bootstrap will come pre-installed:</span></span>

![V zobrazení řešení šablona starter Bootstrap](bootstrap/_static/bootstrap-in-starter-template.png)

<span data-ttu-id="e5aed-112">Přidání Bootstrap ASP.NET Core projektu se pouze záležitost jejím přidáním do *bower.json* jako závislost:</span><span class="sxs-lookup"><span data-stu-id="e5aed-112">Adding Bootstrap to an ASP.NET Core project is simply a matter of adding it to *bower.json* as a dependency:</span></span>

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

<span data-ttu-id="e5aed-113">Toto je doporučeným způsobem, jak přidat Bootstrap do projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e5aed-113">This is the recommended way to add Bootstrap to an ASP.NET Core project.</span></span>

<span data-ttu-id="e5aed-114">Můžete také nainstalovat bootstrap pomocí jedné z několika správce balíčku, například Bower, npm nebo NuGet.</span><span class="sxs-lookup"><span data-stu-id="e5aed-114">You can also install bootstrap using one of several package managers, such as Bower, npm, or NuGet.</span></span> <span data-ttu-id="e5aed-115">V každém případě proces je v podstatě stejný:</span><span class="sxs-lookup"><span data-stu-id="e5aed-115">In each case, the process is essentially the same:</span></span>

### <a name="bower"></a><span data-ttu-id="e5aed-116">Bower</span><span class="sxs-lookup"><span data-stu-id="e5aed-116">Bower</span></span>

```console
bower install bootstrap
```

### <a name="npm"></a><span data-ttu-id="e5aed-117">npm</span><span class="sxs-lookup"><span data-stu-id="e5aed-117">npm</span></span>

```console
npm install bootstrap
```

### <a name="nuget"></a><span data-ttu-id="e5aed-118">NuGet</span><span class="sxs-lookup"><span data-stu-id="e5aed-118">NuGet</span></span>

```console
Install-Package bootstrap
```

> [!NOTE]
> <span data-ttu-id="e5aed-119">Doporučený způsob, jak nainstalovat klienta závislosti, jako je Bootstrap v ASP.NET Core prostřednictvím Bower (pomocí *bower.json*, jak je uvedeno výše).</span><span class="sxs-lookup"><span data-stu-id="e5aed-119">The recommended way to install client-side dependencies like Bootstrap in ASP.NET Core is via Bower (using *bower.json*, as shown above).</span></span> <span data-ttu-id="e5aed-120">Použití npm/NuGet se zobrazují ukazují, jak snadno lze přidat Bootstrap na jiné typy webových aplikací, včetně starší verze technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e5aed-120">The use of npm/NuGet are shown to demonstrate how easily Bootstrap can be added to other kinds of web applications, including earlier versions of ASP.NET.</span></span>

<span data-ttu-id="e5aed-121">Pokud jste odkazující na vlastní místní verze Bootstrap, budete muset na ně odkazovat ve všech stránek, které budou používat.</span><span class="sxs-lookup"><span data-stu-id="e5aed-121">If you're referencing your own local versions of Bootstrap, you'll need to reference them in any pages that will use it.</span></span> <span data-ttu-id="e5aed-122">V produkčním prostředí by měl odkazovat bootstrap pomocí název CDN.</span><span class="sxs-lookup"><span data-stu-id="e5aed-122">In production you should reference bootstrap using a CDN.</span></span> <span data-ttu-id="e5aed-123">Ve výchozí šablona webu ASP.NET *_Layout.cshtml* souboru tak, jako tento:</span><span class="sxs-lookup"><span data-stu-id="e5aed-123">In the default ASP.NET site template, the *_Layout.cshtml* file does so like this:</span></span>

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> <span data-ttu-id="e5aed-124">Pokud se chystáte používat žádné z modulů plug-in jQuery Bootstrap společnosti, musíte se také tak, aby odkazovaly jQuery.</span><span class="sxs-lookup"><span data-stu-id="e5aed-124">If you're going to be using any of Bootstrap's jQuery plugins, you will also need to reference jQuery.</span></span>

## <a name="basic-templates-and-features"></a><span data-ttu-id="e5aed-125">Základní šablony a funkce</span><span class="sxs-lookup"><span data-stu-id="e5aed-125">Basic templates and features</span></span>

<span data-ttu-id="e5aed-126">Nejzákladnější Bootstrap šablony vypadá hodně podobá *_Layout.cshtml* souboru ukazuje vyšší a jednoduše obsahuje základní nabídky pro navigaci a místo pro vykreslení zbývající části stránky.</span><span class="sxs-lookup"><span data-stu-id="e5aed-126">The most basic Bootstrap template looks very much like the *_Layout.cshtml* file shown above, and simply includes a basic menu for navigation and a place to render the rest of the page.</span></span>

### <a name="basic-navigation"></a><span data-ttu-id="e5aed-127">Základní navigaci</span><span class="sxs-lookup"><span data-stu-id="e5aed-127">Basic navigation</span></span>

<span data-ttu-id="e5aed-128">Výchozí šablona používá sadu `<div>` elementy pro vykreslení horní navigační panel a hlavní část stránky.</span><span class="sxs-lookup"><span data-stu-id="e5aed-128">The default template uses a set of `<div>` elements to render a top navbar and the main body of the page.</span></span> <span data-ttu-id="e5aed-129">Pokud používáte HTML5, můžete nahradit první `<div>` značku s `<nav>` značky získat stejného efektu, ale s přesnější sémantiku.</span><span class="sxs-lookup"><span data-stu-id="e5aed-129">If you're using HTML5, you can replace the first `<div>` tag with a `<nav>` tag to get the same effect, but with more precise semantics.</span></span> <span data-ttu-id="e5aed-130">V rámci této první `<div>` se zobrazí několik i další.</span><span class="sxs-lookup"><span data-stu-id="e5aed-130">Within this first `<div>` you can see there are several others.</span></span> <span data-ttu-id="e5aed-131">První, `<div>` s třídou "kontejneru" a pak v rámci této, dva další `<div>` elementy: "navbar záhlaví" a "navbar sbalit".</span><span class="sxs-lookup"><span data-stu-id="e5aed-131">First, a `<div>` with a class of "container", and then within that, two more `<div>` elements: "navbar-header" and "navbar-collapse".</span></span> <span data-ttu-id="e5aed-132">Navigační panel záhlaví div obsahuje tlačítko, které se zobrazí, pokud je na obrazovce níže určité minimální šířky, zobrazující 3 vodorovné čáry (takzvané "hamburger ikonu").</span><span class="sxs-lookup"><span data-stu-id="e5aed-132">The navbar-header div includes a button that will appear when the screen is below a certain minimum width, showing 3 horizontal lines (a so-called "hamburger icon").</span></span> <span data-ttu-id="e5aed-133">Ikona je vykreslen pomocí čistý HTML a CSS; je vyžadován žádný obrázek.</span><span class="sxs-lookup"><span data-stu-id="e5aed-133">The icon is rendered using pure HTML and CSS; no image is required.</span></span> <span data-ttu-id="e5aed-134">Toto je kód, který se zobrazí ikona s jednotlivými <span> značky vykreslování mezi bílé řádky:</span><span class="sxs-lookup"><span data-stu-id="e5aed-134">This is the code that displays the icon, with each of the <span> tags rendering one of the white bars:</span></span>

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

<span data-ttu-id="e5aed-135">Zahrnuje také název aplikace, která se zobrazí v levé horní části.</span><span class="sxs-lookup"><span data-stu-id="e5aed-135">It also includes the application name, which appears in the top left.</span></span> <span data-ttu-id="e5aed-136">Hlavní navigační nabídce je vykreslen metodou `<ul>` v rámci druhého div a obsahuje odkazy na Domů, o a obraťte se na.</span><span class="sxs-lookup"><span data-stu-id="e5aed-136">The main navigation menu is rendered by the `<ul>` element within the second div, and includes links to Home, About, and Contact.</span></span> <span data-ttu-id="e5aed-137">Níže navigaci, hlavní části každé stránce vykreslením v jiném `<div>`označené jako s třídami "kontejner" a "obsah".</span><span class="sxs-lookup"><span data-stu-id="e5aed-137">Below the navigation, the main body of each page is rendered in another `<div>`, marked with the "container" and "body-content" classes.</span></span> <span data-ttu-id="e5aed-138">V souboru _Layout jednoduché výchozí vidět tady jsou obsahu stránce vykreslen zobrazením konkrétní přidružené stránce a pak jednoduchou `<footer>` se přidá na konec `<div>` elementu.</span><span class="sxs-lookup"><span data-stu-id="e5aed-138">In the simple default _Layout file shown here, the contents of the page are rendered by the specific View associated with the page, and then a simple `<footer>` is added to the end of the `<div>` element.</span></span> <span data-ttu-id="e5aed-139">Uvidíte, jak zobrazuje integrované o stránku pomocí této šablony:</span><span class="sxs-lookup"><span data-stu-id="e5aed-139">You can see how the built-in About page appears using this template:</span></span>

![O stránku](bootstrap/_static/about-page-wide.png)

<span data-ttu-id="e5aed-141">Sbalené navigační panel s tlačítkem "hamburger" v horním pravém, se zobrazí, když okno klesne pod určitou:</span><span class="sxs-lookup"><span data-stu-id="e5aed-141">The collapsed navbar, with "hamburger" button in the top right, appears when the window drops below a certain width:</span></span>

![o stránce s hamburger nabídky](bootstrap/_static/about-page-hamburger.png)

<span data-ttu-id="e5aed-143">Kliknutím na ikonu zjistí položky nabídky svislé zásuvce, který snímky dolů z horní části stránky:</span><span class="sxs-lookup"><span data-stu-id="e5aed-143">Clicking the icon reveals the menu items in a vertical drawer that slides down from the top of the page:</span></span>

![o stránce s hamburger nabídky rozšířit](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a><span data-ttu-id="e5aed-145">Typografie a odkazy</span><span class="sxs-lookup"><span data-stu-id="e5aed-145">Typography and links</span></span>

<span data-ttu-id="e5aed-146">Bootstrap nastaví základní typografii v lokalitě, barvy a odkaz formátování ve svém souboru šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="e5aed-146">Bootstrap sets up the site's basic typography, colors, and link formatting in its CSS file.</span></span> <span data-ttu-id="e5aed-147">Tento soubor CSS zahrnuje výchozí styly pro tabulky, tlačítka, elementy form, obrázky a další ([Další](http://getbootstrap.com/css/)).</span><span class="sxs-lookup"><span data-stu-id="e5aed-147">This CSS file includes default styles for tables, buttons, form elements, images, and more ([learn more](http://getbootstrap.com/css/)).</span></span> <span data-ttu-id="e5aed-148">Jeden užitečné funkce je systému rozložení mřížky, popsané dále.</span><span class="sxs-lookup"><span data-stu-id="e5aed-148">One particularly useful feature is the grid layout system, covered next.</span></span>

### <a name="grids"></a><span data-ttu-id="e5aed-149">Mřížky</span><span class="sxs-lookup"><span data-stu-id="e5aed-149">Grids</span></span>

<span data-ttu-id="e5aed-150">Jednou z nejčastěji používané funkce Bootstrap je jeho systém rozložení mřížky.</span><span class="sxs-lookup"><span data-stu-id="e5aed-150">One of the most popular features of Bootstrap is its grid layout system.</span></span> <span data-ttu-id="e5aed-151">Moderních webových aplikací byste neměli používat `<table>` značky pro rozložení, místo toho omezení použití tohoto elementu k skutečné tabulková data.</span><span class="sxs-lookup"><span data-stu-id="e5aed-151">Modern web applications should avoid using the `<table>` tag for layout, instead restricting the use of this element to actual tabular data.</span></span> <span data-ttu-id="e5aed-152">Místo toho sloupců a řádků můžete rozloženy pomocí řady `<div>` elementy a příslušné třídy CSS.</span><span class="sxs-lookup"><span data-stu-id="e5aed-152">Instead, columns and rows can be laid out using a series of `<div>` elements and the appropriate CSS classes.</span></span> <span data-ttu-id="e5aed-153">Existuje několik výhod tohoto přístupu, včetně možnosti nastavit rozložení mřížky zobrazíte svisle na úzkých obrazovkách, například na telefonech.</span><span class="sxs-lookup"><span data-stu-id="e5aed-153">There are several advantages to this approach, including the ability to adjust the layout of grids to display vertically on narrow screens, such as on phones.</span></span>

<span data-ttu-id="e5aed-154">[Systém rozložení mřížky na Bootstrap](http://getbootstrap.com/css/#grid) je založena na dvanáct sloupce.</span><span class="sxs-lookup"><span data-stu-id="e5aed-154">[Bootstrap's grid layout system](http://getbootstrap.com/css/#grid) is based on twelve columns.</span></span> <span data-ttu-id="e5aed-155">Toto číslo jste vybrali, protože je možné rozdělit rovnoměrně do 1, 2, 3 nebo 4 sloupce a šířku sloupců můžete k pohybovat v rozmezí 1/12 svislé šířky na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="e5aed-155">This number was chosen because it can be divided evenly into 1, 2, 3, or 4 columns, and column widths can vary to within 1/12th of the vertical width of the screen.</span></span> <span data-ttu-id="e5aed-156">Pokud chcete začít používat systém rozložení mřížky, měli byste začít s kontejner `<div>` a poté přidejte řádek `<div>`, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="e5aed-156">To start using the grid layout system, you should begin with a container `<div>` and then add a row `<div>`, as shown here:</span></span>

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

<span data-ttu-id="e5aed-157">V dalším kroku přidejte další `<div>` prvky pro každý sloupec a zadat počet sloupců, která `<div>` by měla zabírat (mimo 12) jako součást třídy CSS počínaje "sloupec - md –".</span><span class="sxs-lookup"><span data-stu-id="e5aed-157">Next, add additional `<div>` elements for each column, and specify the number of columns that `<div>` should occupy (out of 12) as part of a CSS class starting with "col-md-".</span></span> <span data-ttu-id="e5aed-158">Například pokud chcete jednoduše mít dva sloupce stejné velikosti, by použití třídy "sloupec md – 6" pro každé z nich.</span><span class="sxs-lookup"><span data-stu-id="e5aed-158">For instance, if you want to simply have two columns of equal size, you would use a class of "col-md-6" for each one.</span></span> <span data-ttu-id="e5aed-159">V tomto případě "md" je zkratka pro "střední" a odkazuje na standardní velikosti stolní počítač velikosti zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e5aed-159">In this case "md" is short for "medium" and refers to standard-sized desktop computer display sizes.</span></span> <span data-ttu-id="e5aed-160">Existují čtyři různé možnosti, které můžete vybrat z, a každý se použije pro vyšší šířky přepsána (takže pokud chcete rozložení opravit bez ohledu na šířku obrazovky, můžete zadat jenom xs třídy).</span><span class="sxs-lookup"><span data-stu-id="e5aed-160">There are four different options you can choose from, and each will be used for higher widths unless overridden (so if you want the layout to be fixed regardless of screen width, you can just specify xs classes).</span></span>

<span data-ttu-id="e5aed-161">Předpona šablon stylů CSS – třída</span><span class="sxs-lookup"><span data-stu-id="e5aed-161">CSS Class Prefix</span></span> | <span data-ttu-id="e5aed-162">Vrstva zařízení</span><span class="sxs-lookup"><span data-stu-id="e5aed-162">Device Tier</span></span> | <span data-ttu-id="e5aed-163">Šířka</span><span class="sxs-lookup"><span data-stu-id="e5aed-163">Width</span></span>
:---: | :---: | :---:
<span data-ttu-id="e5aed-164">col-xs-</span><span class="sxs-lookup"><span data-stu-id="e5aed-164">col-xs-</span></span> | <span data-ttu-id="e5aed-165">Telefony</span><span class="sxs-lookup"><span data-stu-id="e5aed-165">Phones</span></span> | <span data-ttu-id="e5aed-166">< 768px</span><span class="sxs-lookup"><span data-stu-id="e5aed-166">< 768px</span></span>
<span data-ttu-id="e5aed-167">sloupec-sm -</span><span class="sxs-lookup"><span data-stu-id="e5aed-167">col-sm-</span></span> | <span data-ttu-id="e5aed-168">Tablety</span><span class="sxs-lookup"><span data-stu-id="e5aed-168">Tablets</span></span> | <span data-ttu-id="e5aed-169">>= 768px</span><span class="sxs-lookup"><span data-stu-id="e5aed-169">>= 768px</span></span>
<span data-ttu-id="e5aed-170">sloupec-md –</span><span class="sxs-lookup"><span data-stu-id="e5aed-170">col-md-</span></span> | <span data-ttu-id="e5aed-171">Stolní počítače</span><span class="sxs-lookup"><span data-stu-id="e5aed-171">Desktops</span></span> | <span data-ttu-id="e5aed-172">>= 992px</span><span class="sxs-lookup"><span data-stu-id="e5aed-172">>= 992px</span></span>
<span data-ttu-id="e5aed-173">sloupec-kontaktní skupina -</span><span class="sxs-lookup"><span data-stu-id="e5aed-173">col-lg-</span></span> | <span data-ttu-id="e5aed-174">Zobrazí větší plochy</span><span class="sxs-lookup"><span data-stu-id="e5aed-174">Larger Desktop Displays</span></span> | <span data-ttu-id="e5aed-175">>= 1200px</span><span class="sxs-lookup"><span data-stu-id="e5aed-175">>= 1200px</span></span>

<span data-ttu-id="e5aed-176">Při zadávání dva sloupce obou se "sloupec md – 6" výsledné rozložení bude dva sloupce plochy rozlišení, ale tyto dva sloupce se svisle zásobníku při vykreslení v menší zařízení (nebo užší okno prohlížeče na ploše), což umožňuje uživatelům snadno zobrazit obsah, aniž by bylo nutné vodorovný posun.</span><span class="sxs-lookup"><span data-stu-id="e5aed-176">When specifying two columns both with "col-md-6" the resulting layout will be two columns at desktop resolutions, but these two columns will stack vertically when rendered on smaller devices (or a narrower browser window on a desktop), allowing users to easily view content without the need to scroll horizontally.</span></span>

<span data-ttu-id="e5aed-177">Bootstrap bude vždy výchozí rozložení jednoho sloupce, takže je třeba zadat sloupce, pokud chcete více než jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="e5aed-177">Bootstrap will always default to a single-column layout, so you only need to specify columns when you want more than one column.</span></span> <span data-ttu-id="e5aed-178">Jenom jednou, můžete chtít explicitně určit, že `<div>` trvat až 12 všechny sloupce by k potlačení chování větší vrstvy zařízení.</span><span class="sxs-lookup"><span data-stu-id="e5aed-178">The only time you would want to explicitly specify that a `<div>` take up all 12 columns would be to override the behavior of a larger device tier.</span></span> <span data-ttu-id="e5aed-179">Při zadávání více tříd zařízení vrstvy, můžete resetovat vykreslování sloupec v určitých bodech.</span><span class="sxs-lookup"><span data-stu-id="e5aed-179">When specifying multiple device tier classes, you may need to reset the column rendering at certain points.</span></span> <span data-ttu-id="e5aed-180">Přidání div clearfix, který se zobrazí v rámci určitých zobrazení může dosáhnout, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="e5aed-180">Adding a clearfix div that's only visible within a certain viewport can achieve this, as shown here:</span></span>

![omezený a široké zobrazení mřížky](bootstrap/_static/narrow-and-wide-viewport-grid.png)

<span data-ttu-id="e5aed-182">V předchozím příkladu jeden a dva sdílet řádek v rozložení "md", zatímco dva a tři sdílet v řádku v rozložení "xs".</span><span class="sxs-lookup"><span data-stu-id="e5aed-182">In the above example, One and Two share a row in the "md" layout, while Two and Three share a row in the "xs" layout.</span></span> <span data-ttu-id="e5aed-183">Bez clearfix `<div>`, druhý a třetí nezobrazují správně v zobrazení "xs" (Všimněte si, že se zobrazují pouze jeden, čtyři a pět):</span><span class="sxs-lookup"><span data-stu-id="e5aed-183">Without the clearfix `<div>`, Two and Three are not shown correctly in the "xs" view (note that only One, Four, and Five are shown):</span></span>

![mřížky bez použití clearfix](bootstrap/_static/grid-without-clearfix.png)

<span data-ttu-id="e5aed-185">V tomto příkladu pouze jeden řádek `<div>` byl použit, a zavedení stále většinou nebyla správné věci s ohledem na rozložení a překrývání sloupců.</span><span class="sxs-lookup"><span data-stu-id="e5aed-185">In this example, only a single row `<div>` was used, and Bootstrap still mostly did the right thing with regard to the layout and stacking of the columns.</span></span> <span data-ttu-id="e5aed-186">Obvykle byste měli zadat řádek `<div>` pro každý řádek vodorovné vyžaduje vaše rozložení a samozřejmě můžete vnořovat Bootstrap mřížky v rámci jednu na druhou.</span><span class="sxs-lookup"><span data-stu-id="e5aed-186">Typically, you should specify a row `<div>` for each horizontal row your layout requires, and of course you can nest Bootstrap grids within one another.</span></span> <span data-ttu-id="e5aed-187">Když to uděláte, bude každé vnořené mřížky zabírat 100 % šířky elementu, ve kterém je umístěn, který lze potom rozdělit pomocí třídy sloupce.</span><span class="sxs-lookup"><span data-stu-id="e5aed-187">When you do, each nested grid will occupy 100% of the width of the element in which it's placed, which can then be subdivided using column classes.</span></span>

### <a name="jumbotron"></a><span data-ttu-id="e5aed-188">Jumbotron</span><span class="sxs-lookup"><span data-stu-id="e5aed-188">Jumbotron</span></span>

<span data-ttu-id="e5aed-189">Pokud jste použili výchozí šablony ASP.NET MVC v sadě Visual Studio 2012 nebo 2013, pravděpodobně vidíte Jumbotron v akci.</span><span class="sxs-lookup"><span data-stu-id="e5aed-189">If you've used the default ASP.NET MVC templates in Visual Studio 2012 or 2013, you've probably seen the Jumbotron in action.</span></span> <span data-ttu-id="e5aed-190">Odkazuje na velká část stránky, který umožňuje zobrazit obrázek na pozadí velké volání akce, rotator nebo podobným elementům plnou šířkou.</span><span class="sxs-lookup"><span data-stu-id="e5aed-190">It refers to a large full-width section of a page that can be used to display a large background image, a call to action, a rotator, or similar elements.</span></span> <span data-ttu-id="e5aed-191">Přidat jumbotron na stránku, jednoduše přidat `<div>` a pojmenujte ho třídu "jumbotron", pak umístit kontejner `<div>` uvnitř a přidejte svůj obsah.</span><span class="sxs-lookup"><span data-stu-id="e5aed-191">To add a jumbotron to a page, simply add a `<div>` and give it a class of "jumbotron", then place a container `<div>` inside and add your content.</span></span> <span data-ttu-id="e5aed-192">Jsme můžete snadno upravit standardní o stránce sloužící jumbotron pro hlavní názvy sloupců, které se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="e5aed-192">We can easily adjust the standard About page to use a jumbotron for the main headings it displays:</span></span>

![Příklad jumbotron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a><span data-ttu-id="e5aed-194">Tlačítka</span><span class="sxs-lookup"><span data-stu-id="e5aed-194">Buttons</span></span>

<span data-ttu-id="e5aed-195">Výchozí tlačítko třídy a jejich barvy jsou uvedené v následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="e5aed-195">The default button classes and their colors are shown in the figure below.</span></span>

![motivu tlačítka](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a><span data-ttu-id="e5aed-197">Odznaky</span><span class="sxs-lookup"><span data-stu-id="e5aed-197">Badges</span></span>

<span data-ttu-id="e5aed-198">Odznaky naleznete malé, obvykle číselné popisky vedle položky navigace.</span><span class="sxs-lookup"><span data-stu-id="e5aed-198">Badges refer to small, usually numeric callouts next to a navigation item.</span></span> <span data-ttu-id="e5aed-199">Můžete indikuje počet zpráv nebo oznámení čekání nebo přítomnosti aktualizací.</span><span class="sxs-lookup"><span data-stu-id="e5aed-199">They can indicate a number of messages or notifications waiting, or the presence of updates.</span></span> <span data-ttu-id="e5aed-200">Určení takové odznaky je jednoduché, přidávání `<span>` obsahující text, s třídou "BADGE":</span><span class="sxs-lookup"><span data-stu-id="e5aed-200">Specifying such badges is as simple as adding a `<span>` containing the text, with a class of "badge":</span></span>

![odznaky s motivu](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a><span data-ttu-id="e5aed-202">Upozornění</span><span class="sxs-lookup"><span data-stu-id="e5aed-202">Alerts</span></span>

<span data-ttu-id="e5aed-203">Potřebujete zobrazí nějaké oznámení, upozornění nebo chybovou zprávu uživatelům vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e5aed-203">You may need to display some kind of notification, alert, or error message to your application's users.</span></span> <span data-ttu-id="e5aed-204">To je, kde jsou užitečné standardní třídy výstrahy.</span><span class="sxs-lookup"><span data-stu-id="e5aed-204">That's where the standard alert classes are useful.</span></span> <span data-ttu-id="e5aed-205">Existují čtyři různé úrovně závažnosti s přidružené barevná schémata:</span><span class="sxs-lookup"><span data-stu-id="e5aed-205">There are four different severity levels with associated color schemes:</span></span>

![motivu výstrahy](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a><span data-ttu-id="e5aed-207">Navbars a nabídek</span><span class="sxs-lookup"><span data-stu-id="e5aed-207">Navbars and menus</span></span>

<span data-ttu-id="e5aed-208">Naše rozložení už obsahuje standardní navigační panel, ale motivu spuštění, který podporuje možnosti dalšími styly.</span><span class="sxs-lookup"><span data-stu-id="e5aed-208">Our layout already includes a standard navbar, but the Bootstrap theme supports additional styling options.</span></span> <span data-ttu-id="e5aed-209">Nemůžeme se můžete také snadno rozhodnout pro zobrazení navigační panel svisle místo vodorovně Pokud, která obsahuje upřednostňované, také přidání dílčí navigační se položky v nabídkách plovoucím panelem.</span><span class="sxs-lookup"><span data-stu-id="e5aed-209">We can also easily opt to display the navbar vertically rather than horizontally if that's preferred, as well as adding sub-navigation items in flyout menus.</span></span> <span data-ttu-id="e5aed-210">Jednoduché navigační nabídky, jako je karta proužky jsou postavený na `<ul>` elementy.</span><span class="sxs-lookup"><span data-stu-id="e5aed-210">Simple navigation menus, like tab strips, are built on top of `<ul>` elements.</span></span> <span data-ttu-id="e5aed-211">Ty lze vytvořit velmi jednoduše tak, že právě poskytovat jim třídy CSS "nav" a "nav karty":</span><span class="sxs-lookup"><span data-stu-id="e5aed-211">These can be created very simply by just providing them with the CSS classes "nav" and "nav-tabs":</span></span>

![motivu tabstrips](bootstrap/_static/theme-tabstrips.png)

<span data-ttu-id="e5aed-213">Navbars jsou vytvořeny podobně, ale trochu složitější.</span><span class="sxs-lookup"><span data-stu-id="e5aed-213">Navbars are built similarly, but are a bit more complex.</span></span> <span data-ttu-id="e5aed-214">Jsou při spuštění systému `<nav>` nebo `<div>` s třídou "navbar", ve kterém div kontejner obsahuje zbytek elementy.</span><span class="sxs-lookup"><span data-stu-id="e5aed-214">They start with a `<nav>` or `<div>` with a class of "navbar", within which a container div holds the rest of the elements.</span></span> <span data-ttu-id="e5aed-215">Naše stránka obsahuje navigační panel v hlavičce již – následující jednoduše rozbalí na to, přidání podpory pro rozevírací nabídce:</span><span class="sxs-lookup"><span data-stu-id="e5aed-215">Our page includes a navbar in its header already – the one shown below simply expands on this, adding support for a dropdown menu:</span></span>

![motivu navbars](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a><span data-ttu-id="e5aed-217">Další prvky</span><span class="sxs-lookup"><span data-stu-id="e5aed-217">Additional elements</span></span>

<span data-ttu-id="e5aed-218">Výchozí motiv lze také k dispozici ve stylu vhodně formátovaná, včetně podpory pro prokládané zobrazení tabulek HTML.</span><span class="sxs-lookup"><span data-stu-id="e5aed-218">The default theme can also be used to present HTML tables in a nicely formatted style, including support for striped views.</span></span> <span data-ttu-id="e5aed-219">Existují popisky se styly, které jsou podobné jako u tlačítka.</span><span class="sxs-lookup"><span data-stu-id="e5aed-219">There are labels with styles that are similar to those of the buttons.</span></span> <span data-ttu-id="e5aed-220">Můžete vytvořit vlastní rozevírací nabídky, které podporují dalšími styly možnosti nad rámec standardních HTML `<select>` element společně s Navbars stejný, jako je naše výchozí úvodní lokality již používá.</span><span class="sxs-lookup"><span data-stu-id="e5aed-220">You can create custom Dropdown menus that support additional styling options beyond the standard HTML `<select>` element, along with Navbars like the one our default starter site is already using.</span></span> <span data-ttu-id="e5aed-221">Pokud potřebujete indikátor průběhu, existuje několik styly můžete vybírat, a také seznam skupin a panely, které obsahují název a obsahu.</span><span class="sxs-lookup"><span data-stu-id="e5aed-221">If you need a progress bar, there are several styles to choose from, as well as List Groups and panels that include a title and content.</span></span> <span data-ttu-id="e5aed-222">Prozkoumejte další možnosti v standardní Bootstrap motivu, který zde:</span><span class="sxs-lookup"><span data-stu-id="e5aed-222">Explore additional options within the standard Bootstrap Theme here:</span></span>

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a><span data-ttu-id="e5aed-223">Další motivy</span><span class="sxs-lookup"><span data-stu-id="e5aed-223">More themes</span></span>

<span data-ttu-id="e5aed-224">Standardní motivu spuštění, který můžete rozšířit přepsáním některé nebo všechny jeho CSS, jak přizpůsobit barvy a styly, aby odpovídaly potřebám vaší vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e5aed-224">You can extend the standard Bootstrap theme by overriding some or all of its CSS, adjusting the colors and styles to suit your own application's needs.</span></span> <span data-ttu-id="e5aed-225">Pokud chcete spustit z připravených motiv, existuje několik motiv galerie dostupných online, specializují na Bootstrap motivy, jako je například WrapBootstrap.com (což je mnoho motivů komerční) a Bootswatch.com (který nabízí bezplatné motivy).</span><span class="sxs-lookup"><span data-stu-id="e5aed-225">If you'd like to start from a ready-made theme, there are several theme galleries available online that specialize in Bootstrap themes, such as WrapBootstrap.com (which has a variety of commercial themes) and Bootswatch.com (which offers free themes).</span></span> <span data-ttu-id="e5aed-226">Některé placené šablony dostupné poskytují značnou část funkce nad rámec základní motivu spuštění, který, jako je například bohatou podporu pro správu nabídek a řídicích panelů s bohatou grafů a ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="e5aed-226">Some of the paid templates available provide a great deal of functionality on top of the basic Bootstrap theme, such as rich support for administrative menus, and dashboards with rich charts and gauges.</span></span> <span data-ttu-id="e5aed-227">Příklad oblíbených placené šablony Inspinia, je aktuálně pro prodej pro $18, který obsahuje šablonu ASP.NET MVC5 kromě AngularJS a statické verze HTML.</span><span class="sxs-lookup"><span data-stu-id="e5aed-227">An example of a popular paid template is Inspinia, currently for sale for $18, which includes an ASP.NET MVC5 template in addition to AngularJS and static HTML versions.</span></span> <span data-ttu-id="e5aed-228">Snímek obrazovky ukázkové jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="e5aed-228">A sample screenshot is shown below.</span></span>

![Příklad motiv inspinia](bootstrap/_static/theme-inspinia.png)

<span data-ttu-id="e5aed-230">Pokud chcete Změna motivu spuštění, umístí *bootstrap.css* soubor pro motiv, který chcete v **wwwroot/css** složky a změňte odkazy v *_Layout.cshtml* a nasměrovat ho.</span><span class="sxs-lookup"><span data-stu-id="e5aed-230">If you want to change your Bootstrap theme, put the *bootstrap.css* file for the theme you want in the **wwwroot/css** folder and change the references in *_Layout.cshtml* to point it.</span></span> <span data-ttu-id="e5aed-231">Změna odkazy pro všechna prostředí:</span><span class="sxs-lookup"><span data-stu-id="e5aed-231">Change the links for all environments:</span></span>

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

<span data-ttu-id="e5aed-232">Pokud chcete sestavit vlastní řídicí panel, můžete spustit z volného příkladu, které jsou k dispozici zde: [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/).</span><span class="sxs-lookup"><span data-stu-id="e5aed-232">If you want to build your own dashboard, you can start from the free example available here: [http://getbootstrap.com/examples/dashboard/](http://getbootstrap.com/examples/dashboard/).</span></span>

## <a name="components"></a><span data-ttu-id="e5aed-233">Součásti</span><span class="sxs-lookup"><span data-stu-id="e5aed-233">Components</span></span>

<span data-ttu-id="e5aed-234">Kromě těchto elementů již popsané, Bootstrap zahrnuje podporu pro celou řadu [integrované komponenty uživatelského rozhraní](http://getbootstrap.com/components/).</span><span class="sxs-lookup"><span data-stu-id="e5aed-234">In addition to those elements already discussed, Bootstrap includes support for a variety of [built-in UI components](http://getbootstrap.com/components/).</span></span>

### <a name="glyphicons"></a><span data-ttu-id="e5aed-235">Glyphicons</span><span class="sxs-lookup"><span data-stu-id="e5aed-235">Glyphicons</span></span>

<span data-ttu-id="e5aed-236">Bootstrap obsahuje ikonu sady z Glyphicons ([http://glyphicons.com](http://glyphicons.com)), s více než 200 ikonami volně dostupných pro použití v rámci Bootstrap povolené webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e5aed-236">Bootstrap includes icon sets from Glyphicons ([http://glyphicons.com](http://glyphicons.com)), with over 200 icons freely available for use within your Bootstrap-enabled web application.</span></span> <span data-ttu-id="e5aed-237">Tady je jenom malé ukázkové:</span><span class="sxs-lookup"><span data-stu-id="e5aed-237">Here's just a small sample:</span></span>

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a><span data-ttu-id="e5aed-239">vstupní skupiny</span><span class="sxs-lookup"><span data-stu-id="e5aed-239">Input groups</span></span>

<span data-ttu-id="e5aed-240">Vstupní skupin povolit sdružování další text nebo tlačítka s input element tak uživatelům více intuitivní prostředí:</span><span class="sxs-lookup"><span data-stu-id="e5aed-240">Input groups allow bundling of additional text or buttons with an input element, providing the user with a more intuitive experience:</span></span>

![vstupní skupiny](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a><span data-ttu-id="e5aed-242">Popis cesty</span><span class="sxs-lookup"><span data-stu-id="e5aed-242">Breadcrumbs</span></span>

<span data-ttu-id="e5aed-243">Cesta ke stránce jsou běžné součást uživatelského rozhraní používá k zobrazení uživatele jejich nedávné historii nebo hloubka v rámci hierarchie navigace webu.</span><span class="sxs-lookup"><span data-stu-id="e5aed-243">Breadcrumbs are a common UI component used to show a user their recent history or depth within a site's navigation hierarchy.</span></span> <span data-ttu-id="e5aed-244">Snadno přidat použitím třídy "s popisem cesty" na všechny `<ol>` element seznamu.</span><span class="sxs-lookup"><span data-stu-id="e5aed-244">Add them easily by applying the "breadcrumb" class to any `<ol>` list element.</span></span> <span data-ttu-id="e5aed-245">Zahrnout integrovanou podporu pro stránkování pomocí třídy "stránkování" na `<ul>` v rámci `<nav>`.</span><span class="sxs-lookup"><span data-stu-id="e5aed-245">Include built-in support for pagination by using the "pagination" class on a `<ul>` element within a `<nav>`.</span></span> <span data-ttu-id="e5aed-246">Přidání přizpůsobivý embedded prezentace a video s použitím `<iframe>`, `<embed>`, `<video>`, nebo `<object>` prvky, které budou automaticky styl Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="e5aed-246">Add responsive embedded slideshows and video by using `<iframe>`, `<embed>`, `<video>`, or `<object>` elements, which Bootstrap will style automatically.</span></span> <span data-ttu-id="e5aed-247">Zadejte konkrétní poměr stran pomocí určité třídy, jako je "Vložit přizpůsobivý 16by9".</span><span class="sxs-lookup"><span data-stu-id="e5aed-247">Specify a particular aspect ratio by using specific classes like "embed-responsive-16by9".</span></span>

## <a name="javascript-support"></a><span data-ttu-id="e5aed-248">Podpora jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="e5aed-248">JavaScript support</span></span>

<span data-ttu-id="e5aed-249">Knihovna JavaScript Bootstrap na zahrnuje podporu rozhraní API pro zahrnuté součásti, vám umožní ovládat jejich chování prostřednictvím kódu programu v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e5aed-249">Bootstrap's JavaScript library includes API support for the included components, allowing you to control their behavior programmatically within your application.</span></span> <span data-ttu-id="e5aed-250">Kromě toho *bootstrap.js* obsahuje více než tucet detekce (aktualizace styly, podle které má uživatel přešli v dokumentu), posuňte se vlastní jQuery modulů plug-in, že poskytuje další funkce, například přechody, modálních dialogových oken sbalte chování, karusely a vyznačení nabídek do okna, takže nemáte přejděte z obrazovky.</span><span class="sxs-lookup"><span data-stu-id="e5aed-250">In addition, *bootstrap.js* includes over a dozen custom jQuery plugins, providing additional features like transitions, modal dialogs, scroll detection (updating styles based on where the user has scrolled in the document), collapse behavior, carousels, and affixing menus to the window so they don't scroll off the screen.</span></span> <span data-ttu-id="e5aed-251">Není dostatek místa na všechny doplňky JavaScript součástí Bootstrap – Další informace naleznete [ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/).</span><span class="sxs-lookup"><span data-stu-id="e5aed-251">There's not sufficient room to cover all of the JavaScript add-ons built into Bootstrap – to learn more please visit [http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/).</span></span>

## <a name="summary"></a><span data-ttu-id="e5aed-252">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e5aed-252">Summary</span></span>

<span data-ttu-id="e5aed-253">Bootstrap poskytuje webové rozhraní, které lze použít k rychlé a efektivní rozvržení a stylu širokou škálu webům a aplikacím.</span><span class="sxs-lookup"><span data-stu-id="e5aed-253">Bootstrap provides a web framework that can be used to quickly and productively lay out and style a wide variety of websites and applications.</span></span> <span data-ttu-id="e5aed-254">Její základní typografii a styly poskytují příjemný vzhled a chování, která lze snadno manipulovat prostřednictvím podpory vlastní motiv, který můžete ručně vytvořené nebo komerčně zakoupili.</span><span class="sxs-lookup"><span data-stu-id="e5aed-254">Its basic typography and styles provide a pleasant look and feel that can easily be manipulated through custom theme support, which can be hand-crafted or purchased commercially.</span></span> <span data-ttu-id="e5aed-255">Podporuje hostitel webové komponenty, které v minulosti by jste požadovali nákladné ovládací prvky třetích stran k dosažení, spolu s podporou moderní a otevřete webové standardy.</span><span class="sxs-lookup"><span data-stu-id="e5aed-255">It supports a host of web components that in the past would've required expensive third-party controls to accomplish, while supporting modern and open web standards.</span></span>
