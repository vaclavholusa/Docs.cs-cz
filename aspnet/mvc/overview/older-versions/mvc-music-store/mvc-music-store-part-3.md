---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: '3. část: Zobrazení a modely ViewModels | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 3. část popisuje zobrazení a modely ViewModel.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 8fd89c2a448877bf13a7828f545ffcd400f63bb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837407"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="2a06d-104">3. část: Zobrazení a modely ViewModels</span><span class="sxs-lookup"><span data-stu-id="2a06d-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="2a06d-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="2a06d-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="2a06d-106">MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2a06d-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="2a06d-107">Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="2a06d-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="2a06d-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="2a06d-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="2a06d-109">3. část popisuje zobrazení a modely ViewModel.</span><span class="sxs-lookup"><span data-stu-id="2a06d-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="2a06d-110">Zatím jsme jste právě byla vracení řetězců z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="2a06d-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="2a06d-111">To je dobrý způsob, jak získat představu o fungování řadiče, ale není jak byste k sestavení aplikace skutečný webu.</span><span class="sxs-lookup"><span data-stu-id="2a06d-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="2a06d-112">Budeme má lepší způsob, jak generují kód HTML zpět do prohlížečů navštěvující náš web – jeden kde soubory šablony můžete použít k více snadno přizpůsobit obsah HTML odeslání zpět.</span><span class="sxs-lookup"><span data-stu-id="2a06d-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="2a06d-113">To je přesně co dělat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2a06d-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="2a06d-114">Přidání zobrazení šablony</span><span class="sxs-lookup"><span data-stu-id="2a06d-114">Adding a View template</span></span>

<span data-ttu-id="2a06d-115">Použít šablonu zobrazení, jsme budete změnit metodu HomeController Index vrátit třídu ActionResult a jej vrátit View(), jako je následující:</span><span class="sxs-lookup"><span data-stu-id="2a06d-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="2a06d-116">Výše uvedené změny označuje, že místo vrátil řetězec, chceme místo toho použít k získání výsledku zpět "Zobrazit".</span><span class="sxs-lookup"><span data-stu-id="2a06d-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="2a06d-117">Teď přidáme vhodnou šablonu zobrazení na našem projektu.</span><span class="sxs-lookup"><span data-stu-id="2a06d-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="2a06d-118">Provedete to tak jsme budete umístit textový kurzor v rámci metody akce indexu, pak klikněte pravým tlačítkem a vyberte "Přidat zobrazení".</span><span class="sxs-lookup"><span data-stu-id="2a06d-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="2a06d-119">Tím se otevře dialogové okno Přidat zobrazení:</span><span class="sxs-lookup"><span data-stu-id="2a06d-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="2a06d-120">![](mvc-music-store-part-3/_static/image1.jpg)![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2a06d-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="2a06d-121">Toto dialogové okno "Přidat zobrazení" umožňuje nám to rychle a snadno generovat soubory šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2a06d-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="2a06d-122">Ve výchozím nastavení "Přidat zobrazení" dialogové okno předem vyplní název zobrazení šablony vytvořit tak, aby odpovídalo metodě akce, která bude používat.</span><span class="sxs-lookup"><span data-stu-id="2a06d-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="2a06d-123">Protože jsme použili v rámci metody akce Index() naše HomeController místní nabídce "Přidat zobrazení", "Přidat zobrazení" dialog výše má "Index" jako název zobrazení předem vyplní ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2a06d-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="2a06d-124">Není třeba změnit možnosti v tomto dialogovém okně, proto klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="2a06d-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="2a06d-125">Když kliknete na tlačítko Přidat, vytvoří Visual Web Developer nové Index.cshtml zobrazit šablonu pro nás v adresáři \Views\Home složku vytvoříte, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2a06d-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="2a06d-126">Název a složku umístění souboru "Index.cshtml" je důležité a dodržuje zásady vytváření názvů výchozí rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2a06d-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="2a06d-127">Název adresáře, \Views\Home, odpovídá kontroler – který se nazývá HomeController.</span><span class="sxs-lookup"><span data-stu-id="2a06d-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="2a06d-128">Název šablony zobrazení, Index, odpovídá metodu akce kontroleru, který se zobrazuje zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2a06d-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="2a06d-129">ASP.NET MVC umožňuje vyhnout se tak nutnosti explicitně zadat název nebo umístění šablony zobrazení, pokud použijeme tyto zásady vytváření názvů pro vrátit zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2a06d-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="2a06d-130">Zobrazí ve výchozím nastavení se pak \Views\Home\Index.cshtml zobrazit šablonu při psaní kódu jako níže v rámci naší HomeController:</span><span class="sxs-lookup"><span data-stu-id="2a06d-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="2a06d-131">Visual Web Developer a otevře šablonu zobrazení "Index.cshtml" poté, co jsme kliknutí na tlačítko "Přidat" v dialogovém okně "Přidat zobrazení".</span><span class="sxs-lookup"><span data-stu-id="2a06d-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="2a06d-132">Obsah Index.cshtml jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="2a06d-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="2a06d-133">Toto zobrazení je pomocí syntaxe Razor, což je stručnější než webových formulářů zobrazovací modul používá v webové formuláře ASP.NET a předchozích verzích rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2a06d-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="2a06d-134">Webové formuláře zobrazovací modul je stále k dispozici v architektuře ASP.NET MVC 3, ale celá řada vývojářů považuje zobrazovací modul Razor velice dobře zapadá vývoj pro ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2a06d-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="2a06d-135">První tři řádky nastavit nadpis stránky pomocí ViewBag.Title.</span><span class="sxs-lookup"><span data-stu-id="2a06d-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="2a06d-136">Vytvoříme podívat na fungování tohoto podrobněji brzy, ale nejprve můžeme aktualizace textu v nadpisu textu a zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="2a06d-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="2a06d-137">Aktualizace &lt;h2&gt; značky říct "Toto je Home Page", jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="2a06d-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="2a06d-138">Spuštění aplikace ukazuje, že náš nový text zobrazený na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="2a06d-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="2a06d-139">Použití rozložení společné prvky webu</span><span class="sxs-lookup"><span data-stu-id="2a06d-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="2a06d-140">Většina webů mají obsah, který je sdílen mezi mnoho stránek: navigace zápatí, loga, odkazy na šablony stylů, atd. Zobrazovací modul Razor umožňuje jednoduše spravovat pomocí stránku s názvem \_Layout.cshtml automaticky vytvořené pro nás ve složce/zobrazení/Shared.</span><span class="sxs-lookup"><span data-stu-id="2a06d-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="2a06d-141">Dvakrát klikněte na tuto složku k zobrazení obsahu, které jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="2a06d-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="2a06d-142">Obsah z našich jednotlivá zobrazení se zobrazí podle @RenderBodypříkazu () a společný obsah, který chceme nacházet mimo, který lze přidat do \_Layout.cshtml značek.</span><span class="sxs-lookup"><span data-stu-id="2a06d-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="2a06d-143">Chceme vám naše MVC Music Store mít společné hlavičky s odkazy na naše oblast domovské stránky a Store na všech stránkách v lokalitě, tak, které přidáme do šablony přímo nad tím @RenderBody– příkaz ().</span><span class="sxs-lookup"><span data-stu-id="2a06d-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="2a06d-144">Aktualizuje se šablona stylů</span><span class="sxs-lookup"><span data-stu-id="2a06d-144">Updating the StyleSheet</span></span>

<span data-ttu-id="2a06d-145">Šablonu prázdného projektu obsahuje velmi zjednodušený soubor šablon stylů CSS, který obsahuje pouze styly slouží k zobrazení ověřovacích zpráv.</span><span class="sxs-lookup"><span data-stu-id="2a06d-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="2a06d-146">Naše návrháře poskytuje některé další šablony stylů CSS a bitové kopie k definování vzhledu a chování našich webu, tak těch v teď přidáme.</span><span class="sxs-lookup"><span data-stu-id="2a06d-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="2a06d-147">Aktualizovaný soubor CSS a image jsou součástí obsahu adresáře MvcMusicStore Assets.zip, která je k dispozici v [MVC. Music Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="2a06d-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="2a06d-148">Budete výběru obou z nich v Průzkumníku Windows a umístit je do složky obsahu naše řešení v aplikaci Visual Web Developer, jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="2a06d-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="2a06d-149">Budete vyzváni k potvrzení, pokud chcete přepsat existující soubor Site.css.</span><span class="sxs-lookup"><span data-stu-id="2a06d-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="2a06d-150">Klikněte na tlačítko Ano.</span><span class="sxs-lookup"><span data-stu-id="2a06d-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="2a06d-151">Obsah složky vaší aplikace se teď budou zobrazovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2a06d-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="2a06d-152">Teď naklonujeme aplikaci spustit a prohlédnout naše změny na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="2a06d-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="2a06d-153">Pojďme se podívat, co se změnilo: metody akce indexu The HomeController najít a zobrazit šablonu \Views\Home\Index.cshtmlView i v případě, že náš kód volat "návratový View()", protože naše zobrazit šablonu a potom standardní zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="2a06d-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="2a06d-154">Na domovské stránce se zobrazuje jednoduché zobrazení uvítací zprávy, která je definována v rámci šablony \Views\Home\Index.cshtml zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2a06d-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="2a06d-155">Domovská stránka používá naše \_Layout.cshtml šablony, a proto se uvítací zpráva je obsažena v rozložení standardní webu ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="2a06d-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="2a06d-156">Použití modelu k předávání informací do našich zobrazení</span><span class="sxs-lookup"><span data-stu-id="2a06d-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="2a06d-157">Zobrazit šablonu, která se zobrazí pouze pevně zakódované HTML není vytvořím velmi zajímavé webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="2a06d-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="2a06d-158">K vytvoření dynamické webové stránky, bude místo toho chcete předávání informací do našich zobrazit šablony z našich akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="2a06d-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="2a06d-159">V modelu Model-View-Controller termín Model odkazuje na objekty, které představují data v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2a06d-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="2a06d-160">Často objekty modelu odpovídají tabulek v databázi, ale nemusí to tak být.</span><span class="sxs-lookup"><span data-stu-id="2a06d-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="2a06d-161">Metody akce kontroleru, které vracejí třídu ActionResult, můžete předat objekt modelu do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2a06d-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="2a06d-162">To umožňuje Kontroleru čistě zabalili všechny informace potřebné k vygenerování odpovědí a potom předal tyto informace zobrazit šablonu pro generování odpovídající odpověď ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="2a06d-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="2a06d-163">Toto je nejjednodušší k seznámení s tím, že zobrazíte v akci, takže můžeme začít.</span><span class="sxs-lookup"><span data-stu-id="2a06d-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="2a06d-164">Nejprve vytvoříme některé třídy modelu k reprezentaci žánry a alb v našem úložišti.</span><span class="sxs-lookup"><span data-stu-id="2a06d-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="2a06d-165">Začněme vytvořením žánr třídy.</span><span class="sxs-lookup"><span data-stu-id="2a06d-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="2a06d-166">Klikněte pravým tlačítkem na složku "Modely" v rámci projektu, zvolte možnost "Přidat třídu" a název souboru "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="2a06d-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="2a06d-167">Pak přidejte řetězec veřejnou vlastnost Name do třídy, který byl vytvořen:</span><span class="sxs-lookup"><span data-stu-id="2a06d-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="2a06d-168">*Poznámka: V případě vás zajímá {get; nastavit;} provádí zápis používání automaticky implementovaných vlastností funkce C#. Tento produkt nám nabízí výhody vlastnost bez nutnosti nám chcete-li deklarovat pole zálohování.*</span><span class="sxs-lookup"><span data-stu-id="2a06d-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="2a06d-169">Potom postupujte podle stejného postupu vytvořte třídu alba (s názvem Album.cs), který má název a vlastnosti žánr:</span><span class="sxs-lookup"><span data-stu-id="2a06d-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="2a06d-170">Teď můžeme upravit StoreController použití zobrazení, které budou zobrazovat dynamické informace z našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="2a06d-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="2a06d-171">Pokud - pro demonstrační účely teď - jsme naše alb podle ID požadavku, zobrazíme tyto informace jako v zobrazení níže.</span><span class="sxs-lookup"><span data-stu-id="2a06d-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="2a06d-172">Začneme tak, že změníte akce Store Details tak, aby zobrazovala informace pro jeden album.</span><span class="sxs-lookup"><span data-stu-id="2a06d-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="2a06d-173">Přidejte "pomocí" příkaz do horní části **StoreControllers** třídy, aby obsahoval obor názvů MvcMusicStore.Models, takže jsme nemusíte zadávat MvcMusicStore.Models.Album pokaždé, když chceme použít třídu alba.</span><span class="sxs-lookup"><span data-stu-id="2a06d-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="2a06d-174">Část "použití" této třídy by teď měl vypadat jako dole.</span><span class="sxs-lookup"><span data-stu-id="2a06d-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="2a06d-175">V dalším kroku aktualizujeme akce kontroleru podrobnosti tak, aby vracel třídu ActionResult, spíše než řetězce, jako jsme to udělali s metodou HomeController Index.</span><span class="sxs-lookup"><span data-stu-id="2a06d-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="2a06d-176">Nyní jsme můžete upravit logiku pro objekt alba vrátit do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2a06d-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="2a06d-177">Později v tomto kurzu jsme se se načítání dat z databáze – ale právě teď budeme používat "fiktivní data" abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="2a06d-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="2a06d-178">*Poznámka: Pokud nejste obeznámeni s jazykem C#, které mohou předpokládat, že pomocí var znamená, že je naše proměnná alba s pozdní vazbou. Který není správný – kompilátor jazyka C# používá odvození typu na základě toho, co jsme se přiřadí proměnné k určení, že je tento alba typu alba a kompilaci alba místní proměnné jako typ alba jsme získali kompilace kontrolu a sady Visual Studio code – editor podpora.*</span><span class="sxs-lookup"><span data-stu-id="2a06d-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="2a06d-179">Pojďme teď vytvořit zobrazit šablonu, která používá naše alba generovat odpověď jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="2a06d-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="2a06d-180">Než to potřebujeme k sestavení projektu tak, aby se dialogové okno Přidat zobrazení ví o náš nově vytvořené třídy alba.</span><span class="sxs-lookup"><span data-stu-id="2a06d-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="2a06d-181">Můžete sestavte projekt výběrem Debug⇨Build MvcMusicStore položky nabídky (pro větší užitečnost můžete klávesovou zkratku Ctrl-Shift-B a sestavte projekt).</span><span class="sxs-lookup"><span data-stu-id="2a06d-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="2a06d-182">Teď, když jsme vytvořili naši podpůrných tříd, jsme připraveni k sestavení upravíme šablonu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2a06d-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="2a06d-183">Klikněte pravým tlačítkem v rámci metody podrobnosti a v místní nabídce vyberte "Přidat zobrazení...".</span><span class="sxs-lookup"><span data-stu-id="2a06d-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="2a06d-184">Budeme vytvořte novou šablonu zobrazení jako jsme to udělali dříve s HomeController.</span><span class="sxs-lookup"><span data-stu-id="2a06d-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="2a06d-185">Protože jsme vytváření z StoreController je ve výchozím nastavení se vygeneruje soubor \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="2a06d-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="2a06d-186">Na rozdíl od dříve, budeme zaškrtněte políčko "Vytvořit silného typu" zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2a06d-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="2a06d-187">Pak budeme vyberte naše "Album" třídu v rámci rozevírací downlist "Data třída zobrazení".</span><span class="sxs-lookup"><span data-stu-id="2a06d-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="2a06d-188">To způsobí, že dialog "Přidat zobrazení" pro vytvoření zobrazit šablonu, která očekává, alba objektu se předají do něj chcete použít.</span><span class="sxs-lookup"><span data-stu-id="2a06d-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="2a06d-189">Když kliknete na tlačítko "Přidat" náš \Views\Store\Details.cshtml zobrazit šablonu se vytvoří, obsahující následující kód.</span><span class="sxs-lookup"><span data-stu-id="2a06d-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="2a06d-190">Všimněte si, že první řádek, který označuje, že toto zobrazení je silně typováno do našich alba třídy.</span><span class="sxs-lookup"><span data-stu-id="2a06d-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="2a06d-191">Zobrazovací modul Razor plně chápe, že ho byl předán objekt alb, abychom mohli snadno přistupovat k vlastnosti modelu a dokonce i využívat technologie IntelliSense v editoru Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="2a06d-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="2a06d-192">Aktualizace &lt;h2&gt; označit tak, aby zobrazila alba název vlastnosti tak, že upravíte tento řádek se zobrazí takto.</span><span class="sxs-lookup"><span data-stu-id="2a06d-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="2a06d-193">Všimněte si, že technologie IntelliSense se aktivuje, když zadáte období po @Model – klíčové slovo ukazující vlastnosti a metody, které podporuje alba třídy.</span><span class="sxs-lookup"><span data-stu-id="2a06d-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="2a06d-194">Pojďme teď znovu spustit našem projektu a přejděte na adresu URL Store/podrobnosti/5.</span><span class="sxs-lookup"><span data-stu-id="2a06d-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="2a06d-195">Uvidíme podrobnosti Album jako níže.</span><span class="sxs-lookup"><span data-stu-id="2a06d-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="2a06d-196">Teď uděláme podobné aktualizace pro Store vyhledejte metodu akce.</span><span class="sxs-lookup"><span data-stu-id="2a06d-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="2a06d-197">Aktualizovat tuto metodu tak, aby vracel třídu ActionResult a upravit logiku metody tak, aby ho vytvoří nový objekt žánr a vrátí ji do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2a06d-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="2a06d-198">Klikněte pravým tlačítkem v metodě Procházet a vyberte v místní nabídce "Přidat zobrazení..." Přidat zobrazení silného typu přidejte do třídy rozšířením podle tematických silného typu.</span><span class="sxs-lookup"><span data-stu-id="2a06d-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="2a06d-199">Aktualizace &lt;h2&gt; element v zobrazení kódu (v /Views/Store/Browse.cshtml) k zobrazení informací žánr.</span><span class="sxs-lookup"><span data-stu-id="2a06d-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="2a06d-200">Nyní Pojďme naši projekt spusťte znovu a přejděte do/Store/Procházet? Rozšířením podle tematických = Disco adresy URL.</span><span class="sxs-lookup"><span data-stu-id="2a06d-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="2a06d-201">Uvidíme procházet stránky zobrazují jako níže.</span><span class="sxs-lookup"><span data-stu-id="2a06d-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="2a06d-202">Nakonec vytvoříme o něco složitější aktualizace **Store Index** metody akce a zobrazení seznamu všech žánry v našem úložišti.</span><span class="sxs-lookup"><span data-stu-id="2a06d-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="2a06d-203">Můžeme to udělat pomocí seznamu žánry jako náš objekt modelu, nikoli jen jeden žánr.</span><span class="sxs-lookup"><span data-stu-id="2a06d-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="2a06d-204">Klikněte pravým tlačítkem na metodu akce Store Index a vyberte Přidat zobrazení, vyberte žánr jako třída modelu a klepněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="2a06d-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="2a06d-205">Nejprve Změníme @model deklarace můžete určit, že zobrazení se očekává několik žánr objekty místo jen jeden.</span><span class="sxs-lookup"><span data-stu-id="2a06d-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="2a06d-206">Změňte první řádek /Store/Index.cshtml takto:</span><span class="sxs-lookup"><span data-stu-id="2a06d-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="2a06d-207">Znamená to, který bude pracovat objekt modelu, který může obsahovat několik objektů žánr zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="2a06d-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="2a06d-208">Používáme použití rozhraní IEnumerable&lt;žánr&gt; namísto seznamu&lt;žánr&gt; protože obecnější, abychom mohli změnit později náš model typ na libovolný typ objektu, který podporuje rozhraní IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="2a06d-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="2a06d-209">Dále jsme vám projít žánr objekty v modelu, jak je znázorněno v následujícím kódu dokončené zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2a06d-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="2a06d-210">Všimněte si, že máme plnou podporu technologie IntelliSense jsme zadávání tento kód tak, že když zadáme "@Model."</span><span class="sxs-lookup"><span data-stu-id="2a06d-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="2a06d-211">vidíme všechny metody a vlastnosti nepodporuje použití typu žánr rozhraní IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="2a06d-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="2a06d-212">V rámci naší smyčky "foreach" Visual Web Developer ví, že každá položka je typu Genre, takže uvidíme IntelliSence pro každý žánr typ.</span><span class="sxs-lookup"><span data-stu-id="2a06d-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="2a06d-213">V dalším kroku funkci generování uživatelského rozhraní prozkoumat žánr objekt a určit, že každý bude mít název vlastnosti, takže prochází a zapíše je. Generuje také upravit, podrobnosti a odstranit odkazy na jednotlivé položky.</span><span class="sxs-lookup"><span data-stu-id="2a06d-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="2a06d-214">Provedeme výhod, které později v našich Správce úložiště, ale prozatím jsme chtěli místo toho máte jednoduchý seznam.</span><span class="sxs-lookup"><span data-stu-id="2a06d-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="2a06d-215">Po spuštění aplikace a přejděte do/Store, vidíme, že se zobrazí počet a seznam žánrů.</span><span class="sxs-lookup"><span data-stu-id="2a06d-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="2a06d-216">Přidání propojení mezi stránkami</span><span class="sxs-lookup"><span data-stu-id="2a06d-216">Adding Links between pages</span></span>

<span data-ttu-id="2a06d-217">Naší adresy URL/Store, který obsahuje seznam aktuálně žánry Vypíše názvy žánr jednoduše jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="2a06d-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="2a06d-218">Umožňuje změnit tak, aby místo prostého textu máme místo toho odkaz názvy žánr na příslušnou adresu URL Store/procházet tak, že kliknete na Hudba žánr jako "ROZ" přejdete na/Store/procházet? žánr = Roz adresy URL.</span><span class="sxs-lookup"><span data-stu-id="2a06d-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="2a06d-219">Může aktualizujeme naše \Views\Store\Index.cshtml zobrazit šablonu pro výstup těchto odkazů pomocí kódu jako níže **(nezadávejte to – teď ještě chvíli Zůstaneme ke zlepšení v něm)**:</span><span class="sxs-lookup"><span data-stu-id="2a06d-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="2a06d-220">To funguje, ale může způsobit potíže při později, protože závisí na pevně zakódované řetězce.</span><span class="sxs-lookup"><span data-stu-id="2a06d-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="2a06d-221">Například jsme chtěli přejmenovat Kontroleru, jsme by nemusíte hledat našeho kódu hledají odkazy, které se musí aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="2a06d-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="2a06d-222">Alternativním přístupem, které používáme můžete je využít metody pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="2a06d-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="2a06d-223">ASP.NET MVC zahrnuje metody pomocné rutiny HTML, které jsou k dispozici z našeho kódu šablony zobrazení provádět řadu běžných úloh stejně jako to.</span><span class="sxs-lookup"><span data-stu-id="2a06d-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="2a06d-224">Pomocná metoda Html.ActionLink() je zvlášť užitečné a usnadňuje sestavení HTML &lt;&gt; odkazy a se postará o obtěžující podrobnosti jako zajistit cesty adresy URL jsou správně kódování URL.</span><span class="sxs-lookup"><span data-stu-id="2a06d-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="2a06d-225">Html.ActionLink() má několik různých přetížení, aby umožňoval zadání co nejvíce informací, jako je třeba pro odkazy.</span><span class="sxs-lookup"><span data-stu-id="2a06d-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="2a06d-226">V nejjednodušším případě budete zadáte text odkazu a metoda akce se má přejít při kliknutí na hypertextový odkaz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="2a06d-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="2a06d-227">Můžete například odkaz na "/ Store /" Index() metodu na stránce s podrobnostmi Store se text odkazu "Přejít na the Store Index" pomocí následujícího volání:</span><span class="sxs-lookup"><span data-stu-id="2a06d-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="2a06d-228">*Poznámka: V tomto případě neměli potřebujeme zadat název kontroleru, protože jsme právě při připojování ke jinou akci v rámci stejné kontroleru, který je vykreslování aktuálního zobrazení.*</span><span class="sxs-lookup"><span data-stu-id="2a06d-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="2a06d-229">Naše odkazy na stránky procházet muset předat parametr, takže použijeme jiné přetížení metody Html.ActionLink, který přijímá tři parametry:</span><span class="sxs-lookup"><span data-stu-id="2a06d-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="2a06d-230">Text odkazu, který se zobrazí název žánru</span><span class="sxs-lookup"><span data-stu-id="2a06d-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="2a06d-231">Název akce kontroleru (Procházet)</span><span class="sxs-lookup"><span data-stu-id="2a06d-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="2a06d-232">Hodnoty parametru trasy, zadáte název (žánr) a hodnota (žánr name)</span><span class="sxs-lookup"><span data-stu-id="2a06d-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="2a06d-233">Uvedení, že všechno dohromady, tady je budeme jak psát tyto odkazy na zobrazení indexu Store:</span><span class="sxs-lookup"><span data-stu-id="2a06d-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="2a06d-234">Nyní když jsme naše projekt spusťte znovu a přístup k adrese URL /Store/ uvidíme seznam žánrů.</span><span class="sxs-lookup"><span data-stu-id="2a06d-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="2a06d-235">Každý žánr je hypertextového odkazu – při klepnutí na potrvá nám na naše/Store/procházet? žánr =*[žánr]* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="2a06d-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="2a06d-236">Kód HTML pro daný seznam žánr vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2a06d-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="2a06d-237">[Předchozí](mvc-music-store-part-2.md)
> [další](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="2a06d-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
