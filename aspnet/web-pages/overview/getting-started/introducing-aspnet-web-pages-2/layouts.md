---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Představení technologie ASP.NET Web Pages – vytváření konzistentního rozložení | Microsoft Docs
author: tfitzmac
description: V tomto kurzu se dozvíte, jak používat rozložení k vytvoření konzistentního vzhledu stránky na webu, který používá rozhraní ASP.NET Web Pages. Předpokládá, že jste dokončili...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: c2d5c4d8ed8a71979c16d484ab90d283a45de537
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="dec4c-104">Představení technologie ASP.NET Web Pages – vytváření konzistentního rozložení</span><span class="sxs-lookup"><span data-stu-id="dec4c-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="dec4c-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dec4c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="dec4c-106">V tomto kurzu se dozvíte, jak používat *rozložení* k vytvoření konzistentního vzhledu stránky na webu, který používá rozhraní ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="dec4c-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="dec4c-107">Předpokládá, že jste dokončili řady prostřednictvím [odstranění dat z databáze na webových stránkách ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="dec4c-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="dec4c-108">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="dec4c-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="dec4c-109">Rozložení stránky je.</span><span class="sxs-lookup"><span data-stu-id="dec4c-109">What a layout page is.</span></span>
> - <span data-ttu-id="dec4c-110">Jak kombinovat rozložení stránek s dynamickým obsahem.</span><span class="sxs-lookup"><span data-stu-id="dec4c-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="dec4c-111">Jak předat hodnoty ke stránce rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="dec4c-112">O rozložení</span><span class="sxs-lookup"><span data-stu-id="dec4c-112">About Layouts</span></span>

<span data-ttu-id="dec4c-113">Stránky, které jste vytvořili, pokud mají byla dokončena, samostatné stránky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="dec4c-114">Všechny patří do stejné lokality, ale nemají žádné společné prvky nebo standardní vzhled.</span><span class="sxs-lookup"><span data-stu-id="dec4c-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="dec4c-115">Většina serverů mají konzistentní vzhled a rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="dec4c-116">Například můžete přejít [http://www.microsoft.com/web/gallery/install.aspx?appid=IISExpress](https://www.microsoft.com/web/) lokality a vyhledejte, uvidíte, že všechny stránky splňovat celkové rozložení a vizuální motiv:</span><span class="sxs-lookup"><span data-stu-id="dec4c-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Http://www.microsoft.com/web/gallery/install.aspx?appid=IISExpress stránka zobrazující rozložení záhlaví, navigační oblast, oblast obsahu a zápatí](layouts/_static/image1.png)

<span data-ttu-id="dec4c-118">*Neefektivní* způsob, jak vytvořit toto rozložení by k definování záhlaví, navigačním panelu a zápatí samostatně na jednotlivých stránkách.</span><span class="sxs-lookup"><span data-stu-id="dec4c-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="dec4c-119">Můžete by být duplikování stejný kód pokaždé, když.</span><span class="sxs-lookup"><span data-stu-id="dec4c-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="dec4c-120">Pokud chcete něco změnit (například aktualizace zápatí), budete muset změnit každou stránku samostatně.</span><span class="sxs-lookup"><span data-stu-id="dec4c-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="dec4c-121">Kde je *rozložení stránky* mají.</span><span class="sxs-lookup"><span data-stu-id="dec4c-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="dec4c-122">Na webových stránkách ASP.NET, můžete definovat rozložení stránky, který poskytuje celkový kontejner pro stránky na váš web.</span><span class="sxs-lookup"><span data-stu-id="dec4c-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="dec4c-123">Ke stránce rozložení může například obsahovat záhlaví, navigační oblasti a zápatí stránky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="dec4c-124">Ke stránce rozložení obsahuje zástupný symbol kde hlavní obsah stránky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="dec4c-125">Potom můžete definovat jednotlivé stránky obsahu, které obsahují kód a kód pouze pro danou stránku.</span><span class="sxs-lookup"><span data-stu-id="dec4c-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="dec4c-126">Stránky obsahu nemusí být kompletní stránky HTML; Nemáte i tak, aby měl `<body>` elementu.</span><span class="sxs-lookup"><span data-stu-id="dec4c-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="dec4c-127">Mají také řádek kódu, který určuje ASP.NET, jaké rozložení stránky, které chcete zobrazit obsah.</span><span class="sxs-lookup"><span data-stu-id="dec4c-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="dec4c-128">Tady je obrázek, který zhruba ukazuje, jak funguje tuto relaci:</span><span class="sxs-lookup"><span data-stu-id="dec4c-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Koncepční diagram, který popisuje dvě stránky obsahu a ke stránce rozložení, do kterého se vešly](layouts/_static/image2.png)

<span data-ttu-id="dec4c-130">Tato interakce je snadno zjistit, když se zobrazí v akci.</span><span class="sxs-lookup"><span data-stu-id="dec4c-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="dec4c-131">V tomto kurzu změníte svoje filmy stránky použít rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="dec4c-132">Přidání stránky rozložení</span><span class="sxs-lookup"><span data-stu-id="dec4c-132">Adding a Layout Page</span></span>

<span data-ttu-id="dec4c-133">Budete začněte vytvořením ke stránce rozložení, který definuje rozložení typické stránky v záhlaví, zápatí a oblast pro hlavní obsah.</span><span class="sxs-lookup"><span data-stu-id="dec4c-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="dec4c-134">V lokalitě WebPagesMovies přidat CSHTML stránku s názvem  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dec4c-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="dec4c-135">Vedoucí znak podtržítko ( `_` ) znak je důležité.</span><span class="sxs-lookup"><span data-stu-id="dec4c-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="dec4c-136">Pokud na stránce název začíná podtržítkem, nebudou ASP.NET přímo odesílat této stránce v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="dec4c-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="dec4c-137">Touto konvencí umožňuje definovat stránky, které jsou požadovány pro svůj web, ale, že uživatelé neměli mít možnost požádat přímo.</span><span class="sxs-lookup"><span data-stu-id="dec4c-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="dec4c-138">Nahraďte obsah na stránce s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="dec4c-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="dec4c-139">Jak je vidět, tento kód je právě HTML, který používá `<div>` elementy k definování třech částech v stránce plus jedna více `<div>` element pro uložení tři oddíly.</span><span class="sxs-lookup"><span data-stu-id="dec4c-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="dec4c-140">Zápatí obsahuje bit kódu Razor: `@DateTime.Now.Year`, který vykreslí do aktuálního roku v tomto umístění na stránce.</span><span class="sxs-lookup"><span data-stu-id="dec4c-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="dec4c-141">Všimněte si, že je odkaz na šablonu stylů s názvem *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="dec4c-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="dec4c-142">Šablony stylů je, kde budou určené podrobnosti o fyzické rozložení elementů.</span><span class="sxs-lookup"><span data-stu-id="dec4c-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="dec4c-143">Vytvoříte, za chvíli.</span><span class="sxs-lookup"><span data-stu-id="dec4c-143">You'll create that in a moment.</span></span>

<span data-ttu-id="dec4c-144">Pouze neobvyklou funkce v tomto  *\_Layout.cshtml* stránka je `@Render.Body()` řádku.</span><span class="sxs-lookup"><span data-stu-id="dec4c-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="dec4c-145">To je zástupný symbol, kde bude přejděte obsah, když toto rozložení je sloučen s jinou stránku.</span><span class="sxs-lookup"><span data-stu-id="dec4c-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="dec4c-146">Přidání souboru .css</span><span class="sxs-lookup"><span data-stu-id="dec4c-146">Adding a .css File</span></span>

<span data-ttu-id="dec4c-147">Upřednostňovaný způsob, jak definovat skutečné uspořádání (vzhled) elementy na stránce je použití pravidla kaskádových stylů (CSS).</span><span class="sxs-lookup"><span data-stu-id="dec4c-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="dec4c-148">Takže vytvoříte *.css* soubor, který má pravidla pro nové rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="dec4c-149">Ve službě WebMatrix vyberte kořenový vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="dec4c-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="dec4c-150">Potom v **soubory** karty na pásu karet klikněte na šipku v části **nový** tlačítko a pak klikněte na **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="dec4c-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![Možnost "novou složku, ve sloupci Nová na pásu karet.](layouts/_static/image3.png)

<span data-ttu-id="dec4c-152">Název nové složky *styly*.</span><span class="sxs-lookup"><span data-stu-id="dec4c-152">Name the new folder *Styles*.</span></span>

![Pojmenování nové složky, styly.](layouts/_static/image4.png)

<span data-ttu-id="dec4c-154">Uvnitř nové *styly* složky, vytvořte soubor s názvem *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="dec4c-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Vytvoření nového souboru Movies.css](layouts/_static/image5.png)

<span data-ttu-id="dec4c-156">Nahraďte obsah nové *.css* soubor s následující:</span><span class="sxs-lookup"><span data-stu-id="dec4c-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="dec4c-157">Říkáme nebude mnohem o tato pravidla šablon stylů CSS, s výjimkou a Všimněte si dvě věci.</span><span class="sxs-lookup"><span data-stu-id="dec4c-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="dec4c-158">Jeden je, že kromě nastavení písma a velikosti, pravidla používat absolutní umístění pro vytvoření umístění záhlaví, zápatí a oblast hlavního obsahu.</span><span class="sxs-lookup"><span data-stu-id="dec4c-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="dec4c-159">Pokud jste nové umístění v jazyce CSS, si můžete přečíst [umístění šablon stylů CSS](http://www.w3schools.com/css/css_positioning.asp) kurz na webu W3Schools.</span><span class="sxs-lookup"><span data-stu-id="dec4c-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="dec4c-160">Dalším krokem, si je, že v dolní části, jsme jste zkopírovali pravidel stylu, které byly původně jednotlivě v definované *Movies.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="dec4c-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="dec4c-161">Tato pravidla, které byly používány v [Úvod do zobrazení dat ve pomocí rozhraní ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) kurzu, aby `WebGrid` pomocné rutiny vykreslení kódu, který rozděluje do tabulky přidat.</span><span class="sxs-lookup"><span data-stu-id="dec4c-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="dec4c-162">(Pokud se chystáte použít *.css* soubor definice styl, můžete také ukládat pravidla stylu pro celý web v ní.)</span><span class="sxs-lookup"><span data-stu-id="dec4c-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="dec4c-163">Aktualizuje se tento soubor filmy rozložení</span><span class="sxs-lookup"><span data-stu-id="dec4c-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="dec4c-164">Nyní můžete aktualizovat existující soubory ve vaší lokalitě použít nové rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="dec4c-165">Otevřete *Movies.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="dec4c-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="dec4c-166">V horní části přidejte jako první řádek kódu, následující:</span><span class="sxs-lookup"><span data-stu-id="dec4c-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="dec4c-167">Stránky teď začíná takto:</span><span class="sxs-lookup"><span data-stu-id="dec4c-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="dec4c-168">Tento jeden řádek kódu instruuje ASP.NET který po *filmy* spuštění stránky, by měly být sloučeny s  *\_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="dec4c-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="dec4c-169">Vzhledem k tomu, *Movies.cshtml* souboru teď používá ke stránce rozložení, můžete odebrat kód z *Movies.cshtml* stránky, která má postaráno podle  *\_Layout.cshtml*souboru.</span><span class="sxs-lookup"><span data-stu-id="dec4c-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="dec4c-170">Vyjměte `<!DOCTYPE>`, `<html>`, a `<body>` otvírání a zavírání značky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="dec4c-171">Vyjměte celý `<head>` elementu a jeho obsah, který obsahuje pravidla stylu mřížky, protože nyní vám tato pravidla *.css* souboru.</span><span class="sxs-lookup"><span data-stu-id="dec4c-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="dec4c-172">Když budete na to, změňte existující `<h1>` elementu, který chcete `<h2>` element; budete mít `<h1>` element na stránce rozložení již.</span><span class="sxs-lookup"><span data-stu-id="dec4c-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="dec4c-173">Změna `<h2>` text na "Seznamu filmy".</span><span class="sxs-lookup"><span data-stu-id="dec4c-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="dec4c-174">Za normálních okolností by je nutné provést těchto nastavení neovlivní obsahu stránce.</span><span class="sxs-lookup"><span data-stu-id="dec4c-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="dec4c-175">Při spuštění webu s ke stránce rozložení, vytváření obsahu stránky bez těchto prvků na začátku.</span><span class="sxs-lookup"><span data-stu-id="dec4c-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="dec4c-176">V takovém případě ale převádíte na samostatné stránce na takový, který používá rozložení, takže je bit čištění.</span><span class="sxs-lookup"><span data-stu-id="dec4c-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="dec4c-177">Jakmile budete hotovi, *Movies.cshtml* stránka bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="dec4c-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="dec4c-178">Testování rozložení</span><span class="sxs-lookup"><span data-stu-id="dec4c-178">Testing the Layout</span></span>

<span data-ttu-id="dec4c-179">Nyní uvidíte, jak vypadá rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="dec4c-180">Ve službě WebMatrix, klikněte pravým tlačítkem myši *Movies.cshtml* a vyberte **spustit v prohlížeči**.</span><span class="sxs-lookup"><span data-stu-id="dec4c-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="dec4c-181">Pokud prohlížeč zobrazí stránku, vypadá na této stránce:</span><span class="sxs-lookup"><span data-stu-id="dec4c-181">When the browser displays the page, it looks like this page:</span></span>

![Stránka filmy vykreslen pomocí rozložení](layouts/_static/image6.png)

<span data-ttu-id="dec4c-183">ASP.NET se sloučil obsahu stránce Movies.cshtml do  *\_Layout.cshtml* stránky správné where `RenderBody` je metoda.</span><span class="sxs-lookup"><span data-stu-id="dec4c-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="dec4c-184">A samozřejmě  *\_Layout.cshtml* stránka odkazy *.css* souboru, která definuje vzhled stránky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="dec4c-185">Aktualizace stránky AddMovie používat rozložení</span><span class="sxs-lookup"><span data-stu-id="dec4c-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="dec4c-186">Skutečné výhodou rozložení je, že je můžete použít pro všechny stránky ve vaší lokalitě.</span><span class="sxs-lookup"><span data-stu-id="dec4c-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="dec4c-187">Otevřete *AddMovie.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="dec4c-188">Může nezapomeňte, že *AddMovie.cshtml* stránky původně měli některá pravidla šablon stylů CSS v něm k definování vzhledu chybových zpráv ověření.</span><span class="sxs-lookup"><span data-stu-id="dec4c-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="dec4c-189">Vzhledem k tomu, že máte *.css* souboru pro váš webový server, můžete přesunout tyto pravidla, která *.css* souboru.</span><span class="sxs-lookup"><span data-stu-id="dec4c-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="dec4c-190">Odeberte je ze *AddMovie.cshtml* souboru a přidat je do dolní části *Movies.css* souboru.</span><span class="sxs-lookup"><span data-stu-id="dec4c-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="dec4c-191">Přesouváte následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="dec4c-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="dec4c-192">Nyní provést stejné druhům změny v *AddMovie.cshtml* nástroji *Movies.cshtml* – přidat `Layout="~/_Layout.cshtml;` a odebrání jazyka HTML, který je nyní nadbytečné.</span><span class="sxs-lookup"><span data-stu-id="dec4c-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="dec4c-193">Změna `<h1>` element `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="dec4c-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="dec4c-194">Když jste hotovi, stránka bude vypadat v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="dec4c-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="dec4c-195">Spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-195">Run the page.</span></span> <span data-ttu-id="dec4c-196">Teď to vypadá tomto obrázku:</span><span class="sxs-lookup"><span data-stu-id="dec4c-196">Now it looks like this illustration:</span></span>

!["Přidat filmy, stránku vykreslen pomocí rozložení](layouts/_static/image7.png)

<span data-ttu-id="dec4c-198">Chcete provést podobné změny stránek v lokalitě – *EditMovie.cshtml* a *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dec4c-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="dec4c-199">Ale předtím, než to uděláte, můžete nastavit další změnou rozložení, která usnadňuje trochu flexibilnější.</span><span class="sxs-lookup"><span data-stu-id="dec4c-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="dec4c-200">Informace o předávání titulu na stránku rozložení</span><span class="sxs-lookup"><span data-stu-id="dec4c-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="dec4c-201"> *\_Layout.cshtml* stránce, kterou jste vytvořili `<title>` element, který je nastaven na "Tento server film".</span><span class="sxs-lookup"><span data-stu-id="dec4c-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="dec4c-202">Většina prohlížečů zobrazí jako text na kartě obsah tento element:</span><span class="sxs-lookup"><span data-stu-id="dec4c-202">Most browsers display the content of this element as the text on a tab:</span></span>

![Na stránce &lt;název&gt; element zobrazí na záložce prohlížeče](layouts/_static/image8.png)

<span data-ttu-id="dec4c-204">Tyto informace název je obecná.</span><span class="sxs-lookup"><span data-stu-id="dec4c-204">This title information is generic.</span></span> <span data-ttu-id="dec4c-205">Předpokládejme, že chcete názvu text, který má být konkrétnější na aktuální stránce.</span><span class="sxs-lookup"><span data-stu-id="dec4c-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="dec4c-206">(Text názvu se používá také vyhledávači určit, co je stránka o.) Můžete předat informace ze stránky obsahu jako *Movies.cshtml* nebo *AddMovie.cshtml* na stránku rozložení a pak použijte tyto informace k přizpůsobení stránky rozložení vykreslí.</span><span class="sxs-lookup"><span data-stu-id="dec4c-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="dec4c-207">Otevřete *Movies.cshtml* stránku znovu.</span><span class="sxs-lookup"><span data-stu-id="dec4c-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="dec4c-208">V kódu v horní části přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="dec4c-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="dec4c-209">`Page` Objekt je k dispozici na všech *.cshtml* stránky a je pro tento účel, a to sdílení informací mezi stránky a jeho rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="dec4c-210">Otevřete<em>\_Layout.cshtml</em> stránky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="dec4c-211">Změna `<title>` element tak, aby vypadá jako tento kód:</span><span class="sxs-lookup"><span data-stu-id="dec4c-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="dec4c-212">Tento kód vykreslí, bez ohledu `Page.Title` vlastnost přímo v tomto umístění na stránce.</span><span class="sxs-lookup"><span data-stu-id="dec4c-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="dec4c-213">Spustit *Movies.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="dec4c-214">Tentokrát záložce prohlížeče ukazuje předán jako hodnotu `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="dec4c-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Na záložce prohlížeče zobrazující název vytváří dynamicky](layouts/_static/image9.png)

<span data-ttu-id="dec4c-216">Pokud chcete, zobrazte zdroj stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="dec4c-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="dec4c-217">Uvidíte, že `<title>` prvek je vykreslen jako `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="dec4c-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="dec4c-218">**Objekt stránky**</span><span class="sxs-lookup"><span data-stu-id="dec4c-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="dec4c-219">Užitečné funkce `Page` je, že se jedná o dynamický objekt – `Title` vlastnost není pevnou velikostí, nebo vyhrazený název.</span><span class="sxs-lookup"><span data-stu-id="dec4c-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="dec4c-220">Můžete použít *žádné* název hodnotu `Page` objektu.</span><span class="sxs-lookup"><span data-stu-id="dec4c-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="dec4c-221">Například může snadno předané nadpis pomocí vlastnost s názvem `Page.CurrentName` nebo `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="dec4c-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="dec4c-222">Pouze omezení je, že název má podle běžných pravidel pro jaké vlastnosti může mít název.</span><span class="sxs-lookup"><span data-stu-id="dec4c-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="dec4c-223">(Například název nesmí obsahovat mezery.)</span><span class="sxs-lookup"><span data-stu-id="dec4c-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="dec4c-224">Můžete předat libovolný počet hodnot pomocí `Page` objektu.</span><span class="sxs-lookup"><span data-stu-id="dec4c-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="dec4c-225">Pokud jste chtěli předat film informace ke stránce rozložení, můžete předat hodnoty pomocí něčeho jako jazyk `Page.MovieTitle` a `Page.Genre` a `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="dec4c-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="dec4c-226">(Nebo jiné názvy, které jste již dříve vynalezli uložit informace o.) Jediným požadavkem, což je pravděpodobně zřejmé – je, že budete muset použít stejné názvy v stránky obsahu a ke stránce rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="dec4c-227">Informace o předávání pomocí `Page` objekt není omezen na právě text, který se zobrazí na stránce rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="dec4c-228">Můžete předat hodnotu na stránku rozložení a poté kódu na stránce rozložení použít hodnotu rozhodnout, jestli se má zobrazit oddíl stránky, co *.css* souboru použít a tak dále.</span><span class="sxs-lookup"><span data-stu-id="dec4c-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="dec4c-229">Hodnoty, kterou předáte `Page` objekt jsou jako žádné jiné hodnoty, které použijete v kódu.</span><span class="sxs-lookup"><span data-stu-id="dec4c-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="dec4c-230">Je právě zda hodnoty pocházejí ze stránky obsahu a jsou předávány ke stránce rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="dec4c-231">Otevřete *AddMovie.cshtml* stránky a přidá řádek do horní části kódu, který poskytuje název *AddMovie.cshtml* stránky:</span><span class="sxs-lookup"><span data-stu-id="dec4c-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="dec4c-232">Spustit *AddMovie.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="dec4c-233">Zobrazí nový název:</span><span class="sxs-lookup"><span data-stu-id="dec4c-233">You see the new title there:</span></span>

![Na záložce prohlížeče zobrazující název "přidat filmy vytváří dynamicky](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="dec4c-235">Aktualizace zbývající stránky, které chcete použít pro rozložení</span><span class="sxs-lookup"><span data-stu-id="dec4c-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="dec4c-236">Nyní můžete dokončit zbývající stránky ve vaší lokalitě, aby používají nové rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="dec4c-237">Otevřete *EditMovie.cshtml* a *DeleteMovie.cshtml* v zapnout a provést stejné změny v každé.</span><span class="sxs-lookup"><span data-stu-id="dec4c-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="dec4c-238">Přidejte řádek kódu, který odkazuje na stránku rozložení:</span><span class="sxs-lookup"><span data-stu-id="dec4c-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="dec4c-239">Přidejte řádek nastavit název stránky:</span><span class="sxs-lookup"><span data-stu-id="dec4c-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="dec4c-240">nebo:</span><span class="sxs-lookup"><span data-stu-id="dec4c-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="dec4c-241">Odeberte všechny nadbytečné značka jazyka HTML – v zásadě platí, nechte jenom službu bits, které jsou uvnitř `<body>` – element (plus blok kódu v horní části).</span><span class="sxs-lookup"><span data-stu-id="dec4c-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="dec4c-242">Změna `<h1>` element `<h2>` elementu.</span><span class="sxs-lookup"><span data-stu-id="dec4c-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="dec4c-243">Po provedení těchto změn, otestujte a ujistěte se, že je zobrazení správně a zda je správný název.</span><span class="sxs-lookup"><span data-stu-id="dec4c-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="dec4c-244">Závěrečný myšlenky o rozložení stránky</span><span class="sxs-lookup"><span data-stu-id="dec4c-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="dec4c-245">V tomto kurzu jste vytvořili  *\_Layout.cshtml* stránky a použít `RenderBody` metoda sloučit obsahu z jiné stránky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="dec4c-246">To je základní vzor pro používání rozložení ve webových stránkách.</span><span class="sxs-lookup"><span data-stu-id="dec4c-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="dec4c-247">Rozložení stránky mají další funkce, které nebylo nabídneme sem.</span><span class="sxs-lookup"><span data-stu-id="dec4c-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="dec4c-248">Můžete například vnořit rozložení stránky – jednu stránku rozložení můžete zase odkazují jiné.</span><span class="sxs-lookup"><span data-stu-id="dec4c-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="dec4c-249">Vnořené rozložení může být užitečné, pokud pracujete s témata lokality, které vyžadují různé rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="dec4c-250">Můžete také použít další metody (například `RenderSection`) k nastavení s názvem části na stránce rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="dec4c-251">Kombinace stránkami rozložení a *.css* je výkonný soubory.</span><span class="sxs-lookup"><span data-stu-id="dec4c-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="dec4c-252">Jak uvidíte v dalším kurzu řad, ve službě WebMatrix můžete vytvořit na základě lokality *šablony*, což dává web, který obsahuje předkompilované funkce v ní.</span><span class="sxs-lookup"><span data-stu-id="dec4c-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="dec4c-253">Šablony využít rozložení stránky a šablon stylů CSS chcete vytvářet weby, které vypadají skvěle a které mají funkce, například nabídky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="dec4c-254">Zde je snímek domovské stránky z lokality na základě šablony, zobrazuje funkce, které používají rozložení stránky a šablon stylů CSS:</span><span class="sxs-lookup"><span data-stu-id="dec4c-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Rozložení vytvořené šablony webu služby WebMatrix zobrazující záhlaví, navigační oblast, oblasti obsahu, nepovinný oddíl a přihlášení odkazy](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="dec4c-256">Úplný seznam pro stránku Movie (aktualizovat, a použít ke stránce rozložení)</span><span class="sxs-lookup"><span data-stu-id="dec4c-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="dec4c-257">Stránka dokončení výpis pro přidání stránky Movie (aktualizovat rozložení)</span><span class="sxs-lookup"><span data-stu-id="dec4c-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="dec4c-258">Stránka dokončení seznamu pro odstranění film stránku (aktualizovat rozložení)</span><span class="sxs-lookup"><span data-stu-id="dec4c-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="dec4c-259">Stránka dokončení výpis pro stránku film upravit (aktualizovat rozložení)</span><span class="sxs-lookup"><span data-stu-id="dec4c-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="dec4c-260">Objevuje další</span><span class="sxs-lookup"><span data-stu-id="dec4c-260">Coming Up Next</span></span>

<span data-ttu-id="dec4c-261">V dalším kurzu dozvíte, jak publikovat vaší lokality k Internetu, takže ji vidí všichni.</span><span class="sxs-lookup"><span data-stu-id="dec4c-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dec4c-262">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="dec4c-262">Additional Resources</span></span>

- <span data-ttu-id="dec4c-263">[Vytváření konzistentní vypadat](https://go.microsoft.com/fwlink/?LinkID=202891) – článek, který obsahuje některé další podrobnosti o práci s rozložení.</span><span class="sxs-lookup"><span data-stu-id="dec4c-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="dec4c-264">Také popisuje, jak předat hodnotu ke stránce rozložení, který zobrazí nebo skryje část obsahu.</span><span class="sxs-lookup"><span data-stu-id="dec4c-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="dec4c-265">[Vnořené rozložení stránky s jádrem Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) – Karel Brind blogy příklad toho, jak lze vnořit rozložení stránky.</span><span class="sxs-lookup"><span data-stu-id="dec4c-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="dec4c-266">(Včetně stažení stránek.)</span><span class="sxs-lookup"><span data-stu-id="dec4c-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dec4c-267">[Předchozí](deleting-data.md)
> [další](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="dec4c-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
