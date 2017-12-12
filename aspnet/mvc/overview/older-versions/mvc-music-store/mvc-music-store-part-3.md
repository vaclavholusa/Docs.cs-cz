---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: "Část 3: Zobrazení a ViewModels | Microsoft Docs"
author: jongalloway
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 3 popisuje zobrazení a ViewModels."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 5b38f88283c5d2d93f0bab283dbd9451855d95e3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="dc973-104">Část 3: Zobrazení a ViewModels</span><span class="sxs-lookup"><span data-stu-id="dc973-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="dc973-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="dc973-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="dc973-106">Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.</span><span class="sxs-lookup"><span data-stu-id="dc973-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="dc973-107">Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="dc973-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="dc973-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="dc973-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="dc973-109">Část 3 popisuje zobrazení a ViewModels.</span><span class="sxs-lookup"><span data-stu-id="dc973-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="dc973-110">Pokud jsme jste právě byla vrácení řetězce z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="dc973-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="dc973-111">Který je dobrý způsob, jak získat představu o fungování řadiče, ale není by způsob vytvoření skutečné webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="dc973-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="dc973-112">Přidáme má lepší způsob, jak generují kód HTML do prohlížečů navštěvující náš web – jeden kde šablony soubory můžete použít k více snadno přizpůsobit obsah HTML, který zaslán zpět.</span><span class="sxs-lookup"><span data-stu-id="dc973-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="dc973-113">Je přesně co dělat, zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc973-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="dc973-114">Přidání zobrazení šablony</span><span class="sxs-lookup"><span data-stu-id="dc973-114">Adding a View template</span></span>

<span data-ttu-id="dc973-115">Použít šablonu zobrazení, jsme budete změnit metodu HomeController Index vrátit třídu ActionResult a s její pomocí vrátit View(), jako je nižší než:</span><span class="sxs-lookup"><span data-stu-id="dc973-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="dc973-116">Výše uvedené změnu označuje, že místo vrátí řetězec, chceme místo toho použijte "Náhled" k získání výsledku zpět.</span><span class="sxs-lookup"><span data-stu-id="dc973-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="dc973-117">Teď přidáme odpovídající zobrazit šablonu do našich projektu.</span><span class="sxs-lookup"><span data-stu-id="dc973-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="dc973-118">Uděláte to tak jsme budete umístěte kurzor text v rámci metody akce indexu, pak klikněte pravým tlačítkem a vyberte možnost "Přidat zobrazení".</span><span class="sxs-lookup"><span data-stu-id="dc973-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="dc973-119">Tím se otevře dialogové okno Přidat zobrazení:</span><span class="sxs-lookup"><span data-stu-id="dc973-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="dc973-120">![](mvc-music-store-part-3/_static/image1.jpg)![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dc973-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="dc973-121">Dialogové okno "Přidat zobrazení" umožňuje snadno a rychle generovat soubory šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc973-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="dc973-122">Ve výchozím nastavení "Přidat zobrazení" dialogové okno předem vyplní název zobrazení šablony vytvořit tak, aby odpovídala metody akce, která bude používat.</span><span class="sxs-lookup"><span data-stu-id="dc973-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="dc973-123">Protože jsme použili v místní nabídce "Přidat zobrazení" v metodě akce Index() naše HomeController, dialogové okno "Přidat zobrazení" výše má "Index" jako název zobrazení předem vyplní ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="dc973-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="dc973-124">Není třeba změnit nastavení v tomto dialogovém okně, proto klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="dc973-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="dc973-125">Když kliknete na tlačítko Přidat, Visual Web Developer vytvoří novou Index.cshtml zobrazit šablonu pro nám v adresáři \Views\Home složku vytvoříte, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="dc973-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="dc973-126">Název a umístění složky souboru "Index.cshtml" je důležité a dodržuje konvence pojmenování výchozí rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="dc973-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="dc973-127">Název adresáře, \Views\Home, odpovídá řadičem – který se nazývá HomeController.</span><span class="sxs-lookup"><span data-stu-id="dc973-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="dc973-128">Název šablony zobrazení, Index, odpovídá metoda akce kontroleru, který bude zobrazit zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc973-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="dc973-129">ASP.NET MVC umožňuje vyhnout se nutnosti explicitně zadat název nebo umístění zobrazení šablony při používáme tyto zásady vytváření názvů lze vrátit zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc973-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="dc973-130">Se budou ve výchozím nastavení vykreslovat zobrazit šablonu \Views\Home\Index.cshtml při psaní kódu jako níže v rámci naší HomeController:</span><span class="sxs-lookup"><span data-stu-id="dc973-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="dc973-131">Visual Web Developer vytvořil a spustil zobrazit šablonu "Index.cshtml" po jsme kliknutí na tlačítko "Přidat" v dialogovém okně "Přidat zobrazení".</span><span class="sxs-lookup"><span data-stu-id="dc973-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="dc973-132">Níže jsou uvedeny obsah Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="dc973-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="dc973-133">Toto zobrazení je pomocí syntaxe Razor, který je přesnější, než bude zobrazovací modul webových formulářů používají ve webových formulářů ASP.NET a předchozích verzích rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="dc973-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="dc973-134">Modul zobrazení webových formulářů je stále k dispozici v architektuře ASP.NET MVC 3, ale celá řada vývojářů najít zobrazovací modul Razor skutečně dobře vyhovuje vývoj ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="dc973-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="dc973-135">První tři řádky nastavit název stránky pomocí ViewBag.Title.</span><span class="sxs-lookup"><span data-stu-id="dc973-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="dc973-136">Jsme budete podívejte se jak to funguje podrobněji brzy, ale nejdřív umožňuje aktualizace textu v textu nadpisu a zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="dc973-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="dc973-137">Aktualizace &lt;h2&gt; značky k vyslovení "Toto je na domovskou stránku", jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="dc973-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="dc973-138">Spuštění aplikace zobrazí, že naše nového textu je zobrazen na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="dc973-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="dc973-139">Společné prvky lokality pomocí rozložení</span><span class="sxs-lookup"><span data-stu-id="dc973-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="dc973-140">Většina webů mají obsah, který je sdílen mezi počet stránek: navigace, zápatí stránky, loga, odkazy na šablony stylů, atd. Zobrazovací modul Razor umožňuje snadno spravovat pomocí stránku s názvem \_Layout.cshtml, která automaticky byla vytvořena pro nás do sdílené složky nebo zobrazení /.</span><span class="sxs-lookup"><span data-stu-id="dc973-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="dc973-141">Dvakrát klikněte na tuto složku pro zobrazení obsahu, který je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="dc973-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="dc973-142">Zobrazí se obsah z našich jednotlivých zobrazení ve @RenderBodypříkazu () a všechny společného obsahu, který má být nacházet mimo, lze přidat do \_Layout.cshtml značek.</span><span class="sxs-lookup"><span data-stu-id="dc973-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="dc973-143">Chceme budete naše úložiště Hudba MVC mít běžné hlavička s odkazy na našich oblast domovské stránky a úložiště na všechny stránky v lokalitě, která přidáme do šablony přímo vyšší @RenderBody– příkaz ().</span><span class="sxs-lookup"><span data-stu-id="dc973-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="dc973-144">Aktualizace šablony stylů</span><span class="sxs-lookup"><span data-stu-id="dc973-144">Updating the StyleSheet</span></span>

<span data-ttu-id="dc973-145">Šablona prázdný projekt obsahuje soubor velmi efektivní šablon stylů CSS, který obsahuje pouze styly slouží k zobrazení zpráv ověření.</span><span class="sxs-lookup"><span data-stu-id="dc973-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="dc973-146">Naše Návrhář poskytl některé další šablon stylů CSS a bitové kopie k definování vzhledu a chování pro náš web, tak v nyní přidáme.</span><span class="sxs-lookup"><span data-stu-id="dc973-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="dc973-147">Aktualizovaný soubor CSS a image jsou součástí obsahu adresáře MvcMusicStore Assets.zip, která je k dispozici v [MVC. Hudba úložiště](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="dc973-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="dc973-148">Jsme budete oba dva vyberte v Průzkumníku Windows a jejich umístění do složky obsahu naše řešení v aplikaci Visual Web Developer, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="dc973-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="dc973-149">Budete vyzváni k potvrzení, pokud ji chcete přepsat existující soubor Site.css.</span><span class="sxs-lookup"><span data-stu-id="dc973-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="dc973-150">Klikněte na tlačítko Ano.</span><span class="sxs-lookup"><span data-stu-id="dc973-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="dc973-151">Složku obsahu aplikace bude nyní vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="dc973-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="dc973-152">Teď umožňuje spuštění aplikace a zjistit, jak naše změny zobrazovat na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="dc973-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="dc973-153">Pojďme si shrnout, co se změnilo: metody akce HomeController Index najít a zobrazí šabloně \Views\Home\Index.cshtmlView, i když naše kód volat "návratový View()", protože naše zobrazit šablonu a potom standardní zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="dc973-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="dc973-154">Domovská stránka zobrazuje jednoduché uvítací zprávy, která je definována v rámci šablony \Views\Home\Index.cshtml zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc973-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="dc973-155">Domovská stránka používá naše \_Layout.cshtml šablony, a proto zobrazení uvítací zprávy je obsažena v rozložení standardní webu HTML.</span><span class="sxs-lookup"><span data-stu-id="dc973-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="dc973-156">Použití modelu k předávání informací do našich zobrazení</span><span class="sxs-lookup"><span data-stu-id="dc973-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="dc973-157">Zobrazit šablonu, která se zobrazí pouze pevně zakódované HTML nepůjde zajistit velmi zajímavé webu.</span><span class="sxs-lookup"><span data-stu-id="dc973-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="dc973-158">K vytvoření dynamického webového serveru, budete místo chceme předávání informací z našich akce kontroleru našich šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc973-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="dc973-159">Ve vzoru Model-View-Controller pojem modelu odkazuje na objekty, které představují data v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dc973-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="dc973-160">Často objekty modelu odpovídají tabulek v databázi, ale není nutné.</span><span class="sxs-lookup"><span data-stu-id="dc973-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="dc973-161">Metody akce kontroleru, které vrací třídu ActionResult můžete předat objekt modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc973-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="dc973-162">To umožňuje Kontroleru do této aplikace balíčku se všechny informace potřebné k generovat odpověď a pak předejte tyto informace do zobrazit šablonu použít pro vygenerování odpovídající odpověď protokolu HTML.</span><span class="sxs-lookup"><span data-stu-id="dc973-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="dc973-163">Toto je nejjednodušší pochopit zobrazuje v akci, takže můžeme začít.</span><span class="sxs-lookup"><span data-stu-id="dc973-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="dc973-164">Nejdříve vytvoříme některé třídy modelu k reprezentaci žánry a alb v rámci naší úložiště.</span><span class="sxs-lookup"><span data-stu-id="dc973-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="dc973-165">Začněme vytvořením třídy Genre.</span><span class="sxs-lookup"><span data-stu-id="dc973-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="dc973-166">Klikněte pravým tlačítkem na složku "Modely" v rámci projektu, zvolte možnost "Přidat třídu" a "Genre.cs" název souboru.</span><span class="sxs-lookup"><span data-stu-id="dc973-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="dc973-167">Pak přidáte řetězec veřejný název vlastnosti pro třídu, která byla vytvořena:</span><span class="sxs-lookup"><span data-stu-id="dc973-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="dc973-168">*Poznámka: V případě přemýšlíte {získat; nastavit;} zápis je zajistit použití funkce jazyka C# je automaticky implementované vlastnosti. To nám poskytuje výhody vlastnost bez nutnosti nám základní pole deklarovat.*</span><span class="sxs-lookup"><span data-stu-id="dc973-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="dc973-169">Potom postupujte podle stejných kroků pro vytvoření alb třídy (s názvem Album.cs), který má název a Genre vlastnost:</span><span class="sxs-lookup"><span data-stu-id="dc973-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="dc973-170">Nyní jsme upravit StoreController použít zobrazení, které budou zobrazovat dynamické informace z našich modelu.</span><span class="sxs-lookup"><span data-stu-id="dc973-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="dc973-171">Pokud - pro demonstrační účely teď - jsme pojmenovali naše alb podle ID požadavku, zobrazujeme tyto informace jako zobrazení níže.</span><span class="sxs-lookup"><span data-stu-id="dc973-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="dc973-172">Začneme změnou akce podrobnosti úložiště tak, že se zobrazí informace o jednom alb.</span><span class="sxs-lookup"><span data-stu-id="dc973-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="dc973-173">Přidejte příkaz, "pomocí" do horní části **StoreControllers** třída zahrnout MvcMusicStore.Models oboru názvů, takže jsme není nutné zadávat MvcMusicStore.Models.Album pokaždé, když chcete použít třídu alb.</span><span class="sxs-lookup"><span data-stu-id="dc973-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="dc973-174">V části "direktiv Using" této třídy by se měla zobrazit jako níže.</span><span class="sxs-lookup"><span data-stu-id="dc973-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="dc973-175">Dále budete aktualizujeme akce kontroleru podrobnosti tak, aby vracel třídu ActionResult, nikoli řetězec, jako jsme to udělali s metodou HomeController Index.</span><span class="sxs-lookup"><span data-stu-id="dc973-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="dc973-176">Nyní jsme upravit logika pro vrátit objekt alb do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc973-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="dc973-177">Později v tomto kurzu jsme bude možné načítání dat z databáze – ale pro nyní budeme používat "fiktivní data" začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="dc973-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="dc973-178">*Poznámka: Pokud jste obeznámeni s C#, jste mohou předpokládat, že pomocí var znamená, že naše album proměnné se pozdní vazbou. Není správný – kompilátor jazyka C# používá odvození typu na základě co jsme se přiřazení k proměnné můžete určit, že alb je typu alba a kompilování proměnnou místní album jako typ alb, proto jsme kontrolu kompilaci a Visual Studio – editor kódu podpora.*</span><span class="sxs-lookup"><span data-stu-id="dc973-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="dc973-179">Nyní vytvoříme zobrazení šablony, která používá naše Album ke generování odpovědi HTML.</span><span class="sxs-lookup"><span data-stu-id="dc973-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="dc973-180">Než to potřebujeme sestavte projekt tak, aby se dialogové okno Přidat zobrazení, že zná naše nově vytvořené třídy alb.</span><span class="sxs-lookup"><span data-stu-id="dc973-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="dc973-181">Můžete vytvořit projekt tak, že vyberete Debug⇨Build MvcMusicStore položku nabídky (pokud to pro další platební můžete použít zástupce Ctrl-Shift-B a tím projekt sestavit).</span><span class="sxs-lookup"><span data-stu-id="dc973-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="dc973-182">Nastavili jsme naše podpůrných tříd, jsme připraveni k sestavení naše zobrazit šablonu.</span><span class="sxs-lookup"><span data-stu-id="dc973-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="dc973-183">Klikněte pravým tlačítkem do metody podrobnosti a vyberte možnost "Přidat zobrazení..."</span><span class="sxs-lookup"><span data-stu-id="dc973-183">Right-click within the Details method and select "Add View…"</span></span> <span data-ttu-id="dc973-184">v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="dc973-184">from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="dc973-185">Přidáme vytvořte novou šablonu zobrazení jako jsme to udělali před s HomeController.</span><span class="sxs-lookup"><span data-stu-id="dc973-185">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="dc973-186">Protože jsme vytváříte z StoreController je ve výchozím nastavení vygeneruje v souboru \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="dc973-186">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="dc973-187">Na rozdíl od před, přidáme zaškrtnutím políčka "Vytvořit silného typu" zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc973-187">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="dc973-188">Potom přidáme vyberte třídu naše "Album" v rámci rozevírací downlist "Zobrazení třída dat".</span><span class="sxs-lookup"><span data-stu-id="dc973-188">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="dc973-189">To způsobí, že "Přidat zobrazení" dialogové okno vytvořit šablony zobrazení, která očekává, alba objektu se předá ho na použití.</span><span class="sxs-lookup"><span data-stu-id="dc973-189">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="dc973-190">Když kliknete na tlačítko "Přidat" naše \Views\Store\Details.cshtml zobrazení bude vytvořit šablonu, obsahující následující kód.</span><span class="sxs-lookup"><span data-stu-id="dc973-190">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="dc973-191">Všimněte si první řádek, který označuje, že je toto zobrazení silného typu naše Album třídy.</span><span class="sxs-lookup"><span data-stu-id="dc973-191">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="dc973-192">Zobrazovací modul Razor plně chápe, že ho byl předán objekt alb, proto jsme snadný přístup k vlastnosti modelu a i využívat výhod IntelliSense v editoru Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="dc973-192">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="dc973-193">Aktualizace &lt;h2&gt; značky proto zobrazí název vlastnosti alba změnou daného řádku a zobrazí následující.</span><span class="sxs-lookup"><span data-stu-id="dc973-193">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="dc973-194">Všimněte si, že IntelliSense se aktivuje, když zadáte dobu, po @Model – klíčové slovo, zobrazuje vlastnosti a metody, které třída Album podporuje.</span><span class="sxs-lookup"><span data-stu-id="dc973-194">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="dc973-195">Pojďme nyní znovu spustit naše projektu a navštívíte adresu úložiště/podrobnosti/5.</span><span class="sxs-lookup"><span data-stu-id="dc973-195">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="dc973-196">Jsme zobrazí podrobnosti o Album jako níže.</span><span class="sxs-lookup"><span data-stu-id="dc973-196">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="dc973-197">Nyní vytočit podobné aktualizace pro metodu akce procházet úložiště.</span><span class="sxs-lookup"><span data-stu-id="dc973-197">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="dc973-198">Aktualizujte metodu tak, aby vracel třídu ActionResult a změnit metoda logiku, takže se vytvoří nový objekt Genre a vrátí ji do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc973-198">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="dc973-199">Klikněte pravým tlačítkem v metodě Procházet a vyberte možnost "Přidat zobrazení..."</span><span class="sxs-lookup"><span data-stu-id="dc973-199">Right-click in the Browse method and select "Add View…"</span></span> <span data-ttu-id="dc973-200">v místní nabídce pak přidat zobrazení, které je silně typované třídy Genre přidat silného typu.</span><span class="sxs-lookup"><span data-stu-id="dc973-200">from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="dc973-201">Aktualizace &lt;h2&gt; element v zobrazení Chcete-li zobrazit informace o Genre kód (v /Views/Store/Browse.cshtml).</span><span class="sxs-lookup"><span data-stu-id="dc973-201">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="dc973-202">Nyní Pojďme znovu spustit naše projektu a vyhledejte/úložiště/Procházet? Genre = Disco adresy URL.</span><span class="sxs-lookup"><span data-stu-id="dc973-202">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="dc973-203">Uvidíme procházet stránka zobrazená jako níže.</span><span class="sxs-lookup"><span data-stu-id="dc973-203">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="dc973-204">Nakonec vytvoříme trochu složitější aktualizace **ukládání Index** metody akce a zobrazení seznamu všech žánry v našem úložišti.</span><span class="sxs-lookup"><span data-stu-id="dc973-204">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="dc973-205">Jsme to udělat pomocí seznamu žánry jako naše objekt modelu, nikoli jen jeden Genre.</span><span class="sxs-lookup"><span data-stu-id="dc973-205">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="dc973-206">Klikněte pravým tlačítkem na metodu akce Index úložiště a vyberte možnost Přidat zobrazení jako dříve, vyberte Genre jako třídu modelu a klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="dc973-206">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="dc973-207">Nejprve Změníme @model deklarace k označení, že zobrazení se očekává několik Genre objekty místo pouze jeden.</span><span class="sxs-lookup"><span data-stu-id="dc973-207">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="dc973-208">Změna první řádek /Store/Index.cshtml tímto:</span><span class="sxs-lookup"><span data-stu-id="dc973-208">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="dc973-209">Tato hodnota informuje zobrazovací modul Razor, který bude práce objekt modelu, který může obsahovat několik Genre objekty.</span><span class="sxs-lookup"><span data-stu-id="dc973-209">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="dc973-210">Použití rozhraní IEnumerable používáme&lt;Genre&gt; místo seznam&lt;Genre&gt; vzhledem k tomu, že je více obecné, abychom mohli změnit později naše typu modelu k libovolnému typu objektu, který podporuje rozhraní IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="dc973-210">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="dc973-211">V dalším kroku jsme vám projít Genre objekty v modelu, jak je znázorněno v následujícím kódu dokončené zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc973-211">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="dc973-212">Všimněte si, že jsme mají úplnou podporu technologie IntelliSense jako jsme tento kód zadejte, že když jsme zadejte "@Model."</span><span class="sxs-lookup"><span data-stu-id="dc973-212">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="dc973-213">vidíte všechny metody a vlastnosti podporuje použití rozhraní IEnumerable typu Genre.</span><span class="sxs-lookup"><span data-stu-id="dc973-213">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="dc973-214">V našem smyčka "typu foreach" Visual Web Developer ví, že jednotlivé položky je typu Genre, tak vidíme IntelliSence pro každý typ Genre.</span><span class="sxs-lookup"><span data-stu-id="dc973-214">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="dc973-215">Funkce generování uživatelského rozhraní v dalším kroku zkontrolován objekt Genre a zjistit, název vlastnosti, každý bude mít, aby prochází a zapíše. Generuje také upravit, podrobnosti a odstranit odkazy na jednotlivé položky.</span><span class="sxs-lookup"><span data-stu-id="dc973-215">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="dc973-216">Provedeme výhod, dále v našem Správce úložiště, ale prozatím jsme chtěli místo toho máte jednoduchý seznam.</span><span class="sxs-lookup"><span data-stu-id="dc973-216">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="dc973-217">Když jsme aplikaci spustit a vyhledejte/Store, vidíme, že se zobrazí počet a seznam žánry.</span><span class="sxs-lookup"><span data-stu-id="dc973-217">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="dc973-218">Přidávání odkazů mezi stránkami</span><span class="sxs-lookup"><span data-stu-id="dc973-218">Adding Links between pages</span></span>

<span data-ttu-id="dc973-219">Naše/Store URL, která uvádí žánry aktuálně seznam názvů Genre jednoduše jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="dc973-219">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="dc973-220">Umožňuje změnit tak, aby místo prostého textu máme místo odkaz Genre názvy na příslušnou adresu URL úložiště nebo procházení, tak, že kliknete na genre Hudba, jako jsou "Disco" bude přejděte/úložiště / procházet? genre = Disco adresy URL.</span><span class="sxs-lookup"><span data-stu-id="dc973-220">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="dc973-221">Aktualizujeme může naše \Views\Store\Index.cshtml zobrazit šablonu výstupu tyto odkazy pomocí kódu jako níže **(to nezadávejte – vytvoříme ke zlepšení na něm)**:</span><span class="sxs-lookup"><span data-stu-id="dc973-221">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="dc973-222">To funguje, ale by mohlo vést k řešení problémů později vzhledem k tomu, že přitom spoléhá na pevně kódovaný řetězec.</span><span class="sxs-lookup"><span data-stu-id="dc973-222">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="dc973-223">Například pokud jsme chtěli přejmenovat Kontroleru, by potřebujeme k hledání v našem kódu, hledá odkazy, které je nutné aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="dc973-223">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="dc973-224">Alternativní způsob, které můžete používáme je využít metody pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="dc973-224">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="dc973-225">ASP.NET MVC zahrnuje metody pomocné rutiny HTML, které jsou k dispozici z našich kód šablony zobrazení k provádění různých běžných úloh stejně jako tento.</span><span class="sxs-lookup"><span data-stu-id="dc973-225">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="dc973-226">Pomocná metoda Html.ActionLink() je užitečné a usnadňuje sestavení HTML &lt;&gt; odkazy a má na starosti nepříjemných podrobnosti, třeba zajistit cest URL mohly správně kódovaná adresou URL.</span><span class="sxs-lookup"><span data-stu-id="dc973-226">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="dc973-227">Html.ActionLink() má několik různých přetížení umožňující zadání tolik informací, jako je třeba pro vaše odkazy.</span><span class="sxs-lookup"><span data-stu-id="dc973-227">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="dc973-228">V nejjednodušším případě zadejte text odkazu a metoda akce se má přejít při kliknutí na hypertextový odkaz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="dc973-228">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="dc973-229">Například můžeme můžete propojit "/ úložiště /" metoda Index() na stránce Podrobnosti úložiště s text odkazu "Přejděte na v úložišti Index" pomocí následující volání:</span><span class="sxs-lookup"><span data-stu-id="dc973-229">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="dc973-230">*Poznámka: V tomto případě nebylo musíme zadat jeho název, protože jsme se právě propojení k jiné akce v rámci stejné kontroleru, který je vykreslování aktuální zobrazení.*</span><span class="sxs-lookup"><span data-stu-id="dc973-230">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="dc973-231">Naše odkazy na stránce procházení se bude muset předání parametru, ale proto použijeme jiné přetížení metody Html.ActionLink, která přijímá tři parametry:</span><span class="sxs-lookup"><span data-stu-id="dc973-231">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="dc973-232">Text odkazu, který se zobrazí název Genre</span><span class="sxs-lookup"><span data-stu-id="dc973-232">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="dc973-233">Název akce kontroleru (Procházet)</span><span class="sxs-lookup"><span data-stu-id="dc973-233">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="dc973-234">Hodnoty parametru trasy, zadání názvu (Genre) a hodnota (Genre název)</span><span class="sxs-lookup"><span data-stu-id="dc973-234">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="dc973-235">Uvedení, že všechny společně, zde je jak tyto odkazy budete psát do zobrazení Index úložiště:</span><span class="sxs-lookup"><span data-stu-id="dc973-235">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="dc973-236">Nyní když jsme naše projekt znovu spustit a přístup k adrese URL /Store/ jsme se zobrazí seznam žánry.</span><span class="sxs-lookup"><span data-stu-id="dc973-236">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="dc973-237">Každý genre je při kliknutí na hypertextový odkaz – bude trvat nám naše/úložiště/procházet? genre =*[genre]* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="dc973-237">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="dc973-238">Ve zdroji HTML seznamu genre vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="dc973-238">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


>[!div class="step-by-step"]
<span data-ttu-id="dc973-239">[Předchozí](mvc-music-store-part-2.md)
[další](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="dc973-239">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
