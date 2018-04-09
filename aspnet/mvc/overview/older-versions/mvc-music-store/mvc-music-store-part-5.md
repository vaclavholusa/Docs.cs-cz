---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Část 5: Úpravy formulářů a ukázka | Microsoft Docs'
author: jongalloway
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 5 popisuje úpravy formulářů a ukázka.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: d584e614b5a4124044cd9decd2272192ca164643
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="dea22-104">Část 5: Úpravy formulářů a ukázka</span><span class="sxs-lookup"><span data-stu-id="dea22-104">Part 5: Edit Forms and Templating</span></span>
====================
<span data-ttu-id="dea22-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="dea22-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="dea22-106">Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.</span><span class="sxs-lookup"><span data-stu-id="dea22-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="dea22-107">Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="dea22-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="dea22-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="dea22-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="dea22-109">Část 5 popisuje úpravy formulářů a ukázka.</span><span class="sxs-lookup"><span data-stu-id="dea22-109">Part 5 covers Edit Forms and Templating.</span></span>


<span data-ttu-id="dea22-110">V posledních kapitoly jsme byly načítání dat z databáze a jeho zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dea22-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="dea22-111">V této kapitole jsme budete také povolit úpravy data.</span><span class="sxs-lookup"><span data-stu-id="dea22-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="dea22-112">Vytváření StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="dea22-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="dea22-113">Budete začneme vytvořením nového řadiče názvem **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="dea22-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="dea22-114">Pro tento kontroler můžeme využít výhod funkce generování uživatelského rozhraní dostupné v ASP.NET MVC 3 nástroje pro aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="dea22-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="dea22-115">Nastavení možností pro dialogové okno Přidat kontroler, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="dea22-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="dea22-116">Když kliknete na tlačítko Přidat, uvidíte, že mechanismus generování uživatelského rozhraní ASP.NET MVC 3 nemá dobré množství práce, můžete:</span><span class="sxs-lookup"><span data-stu-id="dea22-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="dea22-117">Vytvoří nový StoreManagerController s místní proměnné Entity Framework</span><span class="sxs-lookup"><span data-stu-id="dea22-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="dea22-118">Přidá do projektu zobrazení složky StoreManager složku</span><span class="sxs-lookup"><span data-stu-id="dea22-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="dea22-119">Přidá Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml a Index.cshtml zobrazení silného typu k třídě Album</span><span class="sxs-lookup"><span data-stu-id="dea22-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="dea22-120">Nové třídy kontroleru StoreManager zahrnuje CRUD (vytvořit, číst, aktualizovat, odstraňovat) akce kontroleru, které vědět, jak pracovat s alba třída modelu a použije naše kontext Entity Framework pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="dea22-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="dea22-121">Úprava vygenerované zobrazení</span><span class="sxs-lookup"><span data-stu-id="dea22-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="dea22-122">Je důležité si pamatovat, že když tento kód se vygeneroval pro nás, je standardní kódu ASP.NET MVC, stejně jako jste jsme byla zápis v rámci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="dea22-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="dea22-123">Je určena k ušetříte čas, který by potřebují na psaní kódu kontroleru standardní a ruční vytvoření zobrazení se silnými typy, ale nejedná se o typ generovaného kódu může viděli jste, kterými pří upozornění v komentářích o tom, jak nesmí změnit kód.</span><span class="sxs-lookup"><span data-stu-id="dea22-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="dea22-124">Toto je váš kód a jste chtěli ho změnit.</span><span class="sxs-lookup"><span data-stu-id="dea22-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="dea22-125">Ano začneme rychlé upravit pro zobrazení indexu StoreManager (nebo Views/StoreManager/Index.cshtml).</span><span class="sxs-lookup"><span data-stu-id="dea22-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="dea22-126">Toto zobrazení zobrazí tabulky, který uvádí alb v našem úložišti pomocí operace upravit / podrobnosti / odstranit odkazy a zahrnuje alba veřejné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dea22-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="dea22-127">Odstraníme AlbumArtUrl pole, protože se nejedná o velmi užitečná v tomto zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dea22-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="dea22-128">V &lt;tabulky&gt; část zobrazení kódu, odeberte &lt;tý&gt; a &lt;td&gt; elementy, které obaluje AlbumArtUrl odkazy, jak znázorňuje následující zvýrazněné řádky:</span><span class="sxs-lookup"><span data-stu-id="dea22-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="dea22-129">Kód upravené zobrazení bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="dea22-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="dea22-130">První podívejte se na správce úložiště</span><span class="sxs-lookup"><span data-stu-id="dea22-130">A first look at the Store Manager</span></span>

<span data-ttu-id="dea22-131">Nyní spusťte aplikaci a vyhledejte/StoreManager /.</span><span class="sxs-lookup"><span data-stu-id="dea22-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="dea22-132">Zobrazí se jsme právě změněna, zobrazující seznam alba v úložišti s odkazy na Upravit podrobnosti a odstraňte Index Správce úložiště.</span><span class="sxs-lookup"><span data-stu-id="dea22-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="dea22-133">Kliknutím na odkaz pro úpravy zobrazí formulář upravit s poli alba, včetně rozevírací seznamy pro Genre a umělcem.</span><span class="sxs-lookup"><span data-stu-id="dea22-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="dea22-134">Klikněte na odkaz "Zpět na seznam" v dolní části, a potom klikněte na odkaz na podrobnosti pro Album.</span><span class="sxs-lookup"><span data-stu-id="dea22-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="dea22-135">Zobrazí podrobné informace o jednotlivých alb.</span><span class="sxs-lookup"><span data-stu-id="dea22-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="dea22-136">Znovu klikněte na tlačítko zpět do seznamu odkazů a potom klikněte na odkaz pro odstranění.</span><span class="sxs-lookup"><span data-stu-id="dea22-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="dea22-137">Zobrazí se dialogové okno s potvrzením, zobrazení podrobností alba a s dotazem, pokud jsme si jisti, že chceme odstranit.</span><span class="sxs-lookup"><span data-stu-id="dea22-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="dea22-138">Kliknutím na tlačítko Odstranit v dolní části odstraňte alba a vrátíte se na indexovou stránku, který ukazuje album odstranit.</span><span class="sxs-lookup"><span data-stu-id="dea22-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="dea22-139">Jsme neprovádí se správcem úložiště, ale máme řadiče a zobrazení kódu pro operace CRUD spuštění z.</span><span class="sxs-lookup"><span data-stu-id="dea22-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="dea22-140">Prohlížení kódu Kontroleru Správce úložiště</span><span class="sxs-lookup"><span data-stu-id="dea22-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="dea22-141">Řadič Správce úložiště obsahuje dobrý množství kódu.</span><span class="sxs-lookup"><span data-stu-id="dea22-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="dea22-142">Přejděte přes tento shora dolů.</span><span class="sxs-lookup"><span data-stu-id="dea22-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="dea22-143">Řadičem zahrnuje některé standardní obory názvů pro kontroler MVC, stejně jako odkaz na obor názvů naše modely.</span><span class="sxs-lookup"><span data-stu-id="dea22-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="dea22-144">Řadičem má privátní instanci MusicStoreEntities, každý z akce kontroleru používané pro přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="dea22-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="dea22-145">Uložení akce správce Index a podrobnosti</span><span class="sxs-lookup"><span data-stu-id="dea22-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="dea22-146">Zobrazení indexu načte seznam alb, včetně každé album odkazované Genre a umělcem informace, jak jsme viděli dříve při práci na metodě procházet úložiště.</span><span class="sxs-lookup"><span data-stu-id="dea22-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="dea22-147">Zobrazení indexu je následující odkazy na propojené objekty tak, že ho můžete zobrazení každé album Genre a umělcem název, kontroler, která má být efektivní a dotazování pro tyto informace v původní žádosti.</span><span class="sxs-lookup"><span data-stu-id="dea22-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="dea22-148">Akce kontroleru podrobnosti řadičem StoreManager funguje stejně jako akce podrobnosti řadič úložiště jsme napsali dříve – vyžádá alba podle ID pomocí metody Find(), vrátí ji do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dea22-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="dea22-149">Vytvořit metody akce</span><span class="sxs-lookup"><span data-stu-id="dea22-149">The Create Action Methods</span></span>

<span data-ttu-id="dea22-150">Vytvořit metody akce jsou jen málo liší od těch, které jsou, které jste viděli dosavadní práce, protože vstup formuláře, které zpracovávají.</span><span class="sxs-lookup"><span data-stu-id="dea22-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="dea22-151">Pokud uživatel nejprve navštíví /StoreManager nebo vytvořit nebo se zobrazí prázdné formuláře.</span><span class="sxs-lookup"><span data-stu-id="dea22-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="dea22-152">Tato stránka HTML bude obsahovat &lt;formuláře&gt; elementy, kde můžete zadat podrobnosti alba vstupní element, který obsahuje rozevírací seznam a textové pole.</span><span class="sxs-lookup"><span data-stu-id="dea22-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="dea22-153">Poté, co uživatel vyplní hodnot formuláře alb, jejich stiskněte tlačítko "Uložit" k odeslání, že tyto změny zpět do aplikace pro uložení v databázi.</span><span class="sxs-lookup"><span data-stu-id="dea22-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="dea22-154">Uživatel stiskne tlačítko "uložit" &lt;formuláře&gt; provede POST protokolu HTTP zpět na /StoreManager nebo vytvořit nebo adresa URL a odeslat &lt;formuláře&gt; hodnoty v rámci protokolu HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="dea22-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="dea22-155">ASP.NET MVC umožňuje snadno rozložit logiku z těchto dvou scénářů volání adresy URL povolením nám implementovat dvě samostatné metody akce "Vytvořit", v rámci třídy Naše StoreManagerController – jednu pro zpracování počáteční procházet HTTP GET a /StoreManager/Create Nebo adresa URL a dalších pro zpracování požadavku HTTP POST odeslané změny.</span><span class="sxs-lookup"><span data-stu-id="dea22-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="dea22-156">Předání informací zobrazení použití položek ViewBag</span><span class="sxs-lookup"><span data-stu-id="dea22-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="dea22-157">Jsme dříve v tomto kurzu jste použili objekt ViewBag, ale nebyly mluvili mnohem o něm.</span><span class="sxs-lookup"><span data-stu-id="dea22-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="dea22-158">Objekt ViewBag umožňuje nám předávat informace k zobrazení bez použití objekt silného typu modelu.</span><span class="sxs-lookup"><span data-stu-id="dea22-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="dea22-159">V takovém případě naší akce kontroleru HTTP GET upravit musí předat i seznam žánry a umělci do formuláře k naplnění rozevíracích seznamů a nejjednodušší způsob, jak to udělat je mohli je vrátit jako položek ViewBag.</span><span class="sxs-lookup"><span data-stu-id="dea22-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="dea22-160">Objekt ViewBag je dynamický objekt, což znamená, že můžete zadat ViewBag.Foo nebo ViewBag.YourNameHere, aniž byste museli psát kód, který definuje tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dea22-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="dea22-161">Kód řadiče používá v tomto případě ViewBag.GenreId a ViewBag.ArtistId tak, aby hodnoty rozevírací odeslána s formuláře budou GenreId a ArtistId, které jsou vlastnostmi alb, které bude nastavení.</span><span class="sxs-lookup"><span data-stu-id="dea22-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="dea22-162">Tyto hodnoty rozevírací seznam se vrátíte na formuláři pomocí SelectList objekt, který je vytvořen pouze k tomuto účelu.</span><span class="sxs-lookup"><span data-stu-id="dea22-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="dea22-163">Děje se tak pomocí kódu takto:</span><span class="sxs-lookup"><span data-stu-id="dea22-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="dea22-164">Jak je vidět z kódu metoda akce tři parametry jsou používány k vytvoření tohoto objektu:</span><span class="sxs-lookup"><span data-stu-id="dea22-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="dea22-165">Seznam položek, které bude možné zobrazení rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="dea22-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="dea22-166">Všimněte si, že tato akce není právě řetězec – seznam žánry jsme se předávání.</span><span class="sxs-lookup"><span data-stu-id="dea22-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="dea22-167">Další parametr předána SelectList je vybraná hodnota.</span><span class="sxs-lookup"><span data-stu-id="dea22-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="dea22-168">Tento způsob SelectList umí předem vyberte položku v seznamu.</span><span class="sxs-lookup"><span data-stu-id="dea22-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="dea22-169">To bude jednodušší zjistit, když se podíváme na úpravy formuláře, který je poměrně podobné.</span><span class="sxs-lookup"><span data-stu-id="dea22-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="dea22-170">Konečný parametr je vlastnost, která má být zobrazen.</span><span class="sxs-lookup"><span data-stu-id="dea22-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="dea22-171">V tomto případě to je označující, zda je vlastnost Genre.Name, které se zobrazí uživateli.</span><span class="sxs-lookup"><span data-stu-id="dea22-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="dea22-172">Si uvědomit pak je poměrně jednoduché akce HTTP GET vytvořit – dvě SelectLists jsou přidány do objekt ViewBag a předán žádný objekt modelu formuláře (protože nebyl ještě vytvořen).</span><span class="sxs-lookup"><span data-stu-id="dea22-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="dea22-173">Pomocné rutiny HTML zobrazení seznamy vyřadit v zobrazení vytvořit</span><span class="sxs-lookup"><span data-stu-id="dea22-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="dea22-174">Vzhledem k tomu, že jste už jsme mluvili o tom, jak se předávají rozevíracího seznamu hodnot do zobrazení, podívejme rychlý přehled najdete v zobrazení tyto hodnoty zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dea22-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="dea22-175">V zobrazení kódu (nebo Views/StoreManager/Create.cshtml), zobrazí se následující volání přišla zobrazíte rozevírací Genre dolů.</span><span class="sxs-lookup"><span data-stu-id="dea22-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="dea22-176">To se označuje jako HTML Helper - nástroj metodu, která provádí úlohu běžné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dea22-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="dea22-177">Pomocné rutiny HTML jsou velmi užitečné při ochraně našich kód zobrazení stručná a čitelná.</span><span class="sxs-lookup"><span data-stu-id="dea22-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="dea22-178">Poskytuje pomocné rutiny Html.DropDownList ASP.NET MVC, ale jako ukážeme později je možné vytvořit vlastní pomocné rutiny pro zobrazení kód, který jsme budete znovu použít v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dea22-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="dea22-179">Volání Html.DropDownList právě musí být sdělili dvě věci –, kde k získání seznamu zobrazíte a jaké hodnota (pokud existuje) by měla být předem vybrané.</span><span class="sxs-lookup"><span data-stu-id="dea22-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="dea22-180">Určuje první parametr, GenreId, rozevírací seznam a hledat hodnotu s názvem GenreId ve model nebo ViewBag.</span><span class="sxs-lookup"><span data-stu-id="dea22-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="dea22-181">Druhý parametr je slouží k určení hodnota, která má zobrazit jako zpočátku vybrali v rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="dea22-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="dea22-182">Vzhledem k tomu, že tento formulář je formulář pro vytvoření, neexistuje žádná hodnota být Zkontrolujte předem vybrané a je předán String.Empty.</span><span class="sxs-lookup"><span data-stu-id="dea22-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="dea22-183">Zpracování odeslání formuláře hodnoty</span><span class="sxs-lookup"><span data-stu-id="dea22-183">Handling the Posted Form values</span></span>

<span data-ttu-id="dea22-184">Jak již bylo zmíněno před, existují dvě metody akce, které jsou spojené s každou formuláře.</span><span class="sxs-lookup"><span data-stu-id="dea22-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="dea22-185">První zpracuje požadavek HTTP GET a zobrazí formulář.</span><span class="sxs-lookup"><span data-stu-id="dea22-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="dea22-186">Druhý zpracuje požadavek HTTP POST, který obsahuje hodnoty odeslaného formuláře.</span><span class="sxs-lookup"><span data-stu-id="dea22-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="dea22-187">Všimněte si, že má akce kontroleru [HttpPost] atributu, který aplikaci ASP.NET MVC oznámeno, že má pouze odpovědět na požadavky HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="dea22-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="dea22-188">Tato akce má čtyři zodpovědnosti:</span><span class="sxs-lookup"><span data-stu-id="dea22-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="dea22-189">Čtení hodnot formuláře</span><span class="sxs-lookup"><span data-stu-id="dea22-189">Read the form values</span></span>
- 2. <span data-ttu-id="dea22-190">Zkontrolujte, pokud formuláře hodnoty předejte všechna pravidla ověřování</span><span class="sxs-lookup"><span data-stu-id="dea22-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="dea22-191">Pokud je odeslání formuláře je platný, uložte data a zobrazit aktualizovaný seznam</span><span class="sxs-lookup"><span data-stu-id="dea22-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="dea22-192">Pokud odeslání formuláře není platný, je třeba znovu zobrazte formulář s chyby ověření</span><span class="sxs-lookup"><span data-stu-id="dea22-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="dea22-193">Čtení hodnot formuláře s vazby modelu</span><span class="sxs-lookup"><span data-stu-id="dea22-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="dea22-194">Odeslání formuláře, který obsahuje hodnoty pro GenreId a ArtistId (z rozevíracího seznamu) a hodnoty textového pole pro název, ceny a AlbumArtUrl zpracovává akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="dea22-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="dea22-195">I když je možné přímý přístup k hodnotám formuláře, je vhodnější je použití možností vytvoření vazby modelu integrovaných v architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="dea22-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="dea22-196">Když akce kontroleru přebírá jako parametr typu modelu, ASP.NET MVC se pokusí naplnit objekt daného typu pomocí formuláře vstupy (stejně jako hodnoty trasy a řetězce dotazu).</span><span class="sxs-lookup"><span data-stu-id="dea22-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="dea22-197">Dělá to tak, že vyhledá hodnoty, jejichž názvy odpovídají vlastností objektu modelu, například při nastavení hodnoty GenreId nový objekt alb, hledá vstup s názvem GenreId.</span><span class="sxs-lookup"><span data-stu-id="dea22-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="dea22-198">Při vytváření zobrazení pomocí standardních metod v architektuře ASP.NET MVC formuláře budou vždy být vykreslen pomocí názvů vlastností jako vstupní pole názvy, tak to názvy polí se právě odpovídat.</span><span class="sxs-lookup"><span data-stu-id="dea22-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="dea22-199">Ověření modelu</span><span class="sxs-lookup"><span data-stu-id="dea22-199">Validating the Model</span></span>

<span data-ttu-id="dea22-200">Model byl ověřen pomocí jednoduché volání ModelState.IsValid.</span><span class="sxs-lookup"><span data-stu-id="dea22-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="dea22-201">Jsme nepřidali žádné ověřovací pravidla Naše třída Album ještě - provedeme, které za chvilku – proto nyní tato kontrola nemá mnohem udělat.</span><span class="sxs-lookup"><span data-stu-id="dea22-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="dea22-202">Co je důležité je, že tato kontrola ModelStat.IsValid se přizpůsobí se jí ověřovacích pravidel, které jsme umístí našeho modelu, takže budoucí změny pravidel ověřování nevyžadují žádné aktualizace kódu akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="dea22-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="dea22-203">Ukládání odeslaná hodnoty</span><span class="sxs-lookup"><span data-stu-id="dea22-203">Saving the submitted values</span></span>

<span data-ttu-id="dea22-204">Pokud odeslání formuláře ověřením úspěšně projde, je čas k uložení hodnoty do databáze.</span><span class="sxs-lookup"><span data-stu-id="dea22-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="dea22-205">S platformou Entity Framework, který právě vyžaduje přidávání modelu do kolekce alb a volání SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="dea22-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="dea22-206">Rozhraní Entity Framework generuje příslušnými příkazy SQL k zachování této hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dea22-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="dea22-207">Po uložení dat, můžeme přesměrovat zpět do seznamu alb, uvidíme náš aktualizace.</span><span class="sxs-lookup"><span data-stu-id="dea22-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="dea22-208">K tomu je potřeba vrácení RedirectToAction s názvem akce kontroleru, který jsme má zobrazit.</span><span class="sxs-lookup"><span data-stu-id="dea22-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="dea22-209">V takovém případě, je metoda Index.</span><span class="sxs-lookup"><span data-stu-id="dea22-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="dea22-210">Zobrazení odesílání neplatný formuláře s chybami ověření</span><span class="sxs-lookup"><span data-stu-id="dea22-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="dea22-211">V případě vstup neplatný formuláře hodnoty rozevírací jsou přidány do objekt ViewBag (jako v případě HTTP GET) a model vazby hodnoty jsou předán zpět do zobrazení pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dea22-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="dea22-212">Chyby ověření se automaticky zobrazí pomocí @Html.ValidationMessageFor pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="dea22-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="dea22-213">Testování vytvořit formulář</span><span class="sxs-lookup"><span data-stu-id="dea22-213">Testing the Create Form</span></span>

<span data-ttu-id="dea22-214">K otestování tohoto limitu, spusťte aplikaci a přejděte k /StoreManager/Create / - zobrazí se prázdný formulář, který byl vrácen metodou HTTP pro vytvoření StoreController-GET.</span><span class="sxs-lookup"><span data-stu-id="dea22-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="dea22-215">Vyplňte hodnoty a klikněte na tlačítko Vytvořit k odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="dea22-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="dea22-216">Zpracování úprav</span><span class="sxs-lookup"><span data-stu-id="dea22-216">Handling Edits</span></span>

<span data-ttu-id="dea22-217">Upravit dvojici akce (HTTP GET a HTTP POST) jsou velmi podobné vytvořit metody akce, kterou jsme právě prohlédli.</span><span class="sxs-lookup"><span data-stu-id="dea22-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="dea22-218">Vzhledem k tomu, že Upravit scénář zahrnuje práci s existujícího alba, upravit HTTP-GET metoda načte založené na parametr "id" Album předána prostřednictvím trasy.</span><span class="sxs-lookup"><span data-stu-id="dea22-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="dea22-219">Tento kód pro načtení album od AlbumId je stejný, jako jsme jste dříve prohlédl v podrobnosti akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="dea22-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="dea22-220">Stejně jako u vytvořením / metoda HTTP GET rozevíracího seznamu hodnoty jsou vráceny prostřednictvím objekt ViewBag.</span><span class="sxs-lookup"><span data-stu-id="dea22-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="dea22-221">To umožňuje nám vrátit Album jako naše model objektu do zobrazení (což je pro třídu Album silného typu) při předání další data (např. seznam žánry) prostřednictvím objekt ViewBag.</span><span class="sxs-lookup"><span data-stu-id="dea22-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="dea22-222">Akce HTTP POST upravit je velmi podobné akce HTTP POST vytvořit.</span><span class="sxs-lookup"><span data-stu-id="dea22-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="dea22-223">Jediným rozdílem je, že místo přidávání nové album k databázi. Kolekce alb, jsme se hledání aktuální instance alba pomocí db. Entry(album) a nastavení jeho stavu na změněné.</span><span class="sxs-lookup"><span data-stu-id="dea22-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="dea22-224">Tato hodnota informuje Entity Framework upravovat existujícího alba a vytvoření nového.</span><span class="sxs-lookup"><span data-stu-id="dea22-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="dea22-225">Abychom mohli otestovat to zjistit tak, že spuštění aplikace a procházení/StoreManger / pak kliknutím na odkaz pro úpravy pro album.</span><span class="sxs-lookup"><span data-stu-id="dea22-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="dea22-226">Zobrazí se upravit formu jako je uvedené metodou HTTP GET upravit.</span><span class="sxs-lookup"><span data-stu-id="dea22-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="dea22-227">Vyplňte hodnoty a klikněte na tlačítko Uložit.</span><span class="sxs-lookup"><span data-stu-id="dea22-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="dea22-228">To formulář odešle, uloží hodnoty a vrátí nám do seznamu alb, zobrazující, že hodnoty byly aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="dea22-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="dea22-229">Zpracování odstranění</span><span class="sxs-lookup"><span data-stu-id="dea22-229">Handling Deletion</span></span>

<span data-ttu-id="dea22-230">Odstranění následující stejné jako upravit a vytvořit, pomocí akce jeden řadič k zobrazení formuláře potvrzení a jiné akce kontroleru pro zpracování odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="dea22-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="dea22-231">Akce kontroleru HTTP GET odstranit je přesně stejný jako naše předchozí akce kontroleru podrobnosti Správce úložiště.</span><span class="sxs-lookup"><span data-stu-id="dea22-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="dea22-232">Zobrazuje se formulář, který je silného typu typ alb, pomocí šablony odstranit zobrazení obsahu.</span><span class="sxs-lookup"><span data-stu-id="dea22-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="dea22-233">Odstranit šablonu zobrazí všechna pole pro model, ale nemůžeme odlišují zjednodušení této nižší.</span><span class="sxs-lookup"><span data-stu-id="dea22-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="dea22-234">Zobrazení kódu v /Views/StoreManager/Delete.cshtml změňte na následující.</span><span class="sxs-lookup"><span data-stu-id="dea22-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="dea22-235">Zobrazí zjednodušená potvrzení odstranění.</span><span class="sxs-lookup"><span data-stu-id="dea22-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="dea22-236">Kliknutím na tlačítko Odstranit způsobí, že formulář poslat zpět na server, který provede akci DeleteConfirmed.</span><span class="sxs-lookup"><span data-stu-id="dea22-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="dea22-237">Naše HTTP POST odstranit řadiče akce provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="dea22-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="dea22-238">Načte Album podle ID</span><span class="sxs-lookup"><span data-stu-id="dea22-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="dea22-239">Neodstraní alba a uložit změny</span><span class="sxs-lookup"><span data-stu-id="dea22-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="dea22-240">Provede přesměrování Index, zobrazující, že alba byla odebrána ze seznamu</span><span class="sxs-lookup"><span data-stu-id="dea22-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="dea22-241">Abyste to mohli otestovat, spusťte aplikaci a přejděte do /StoreManager.</span><span class="sxs-lookup"><span data-stu-id="dea22-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="dea22-242">Vyberte album ze seznamu a klikněte na odkaz odstranit.</span><span class="sxs-lookup"><span data-stu-id="dea22-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="dea22-243">Zobrazí se naše potvrzovací obrazovce a odstranění.</span><span class="sxs-lookup"><span data-stu-id="dea22-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="dea22-244">Kliknutím na tlačítko Odstranit odebere alba a vrátí nám na stránku Index Správce úložiště, který ukazuje, že alba byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="dea22-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="dea22-245">Použití vlastní pomocné rutiny HTML zkrátit text</span><span class="sxs-lookup"><span data-stu-id="dea22-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="dea22-246">My jsme jedním z možných problémů s naší stránce s Index Správce úložiště.</span><span class="sxs-lookup"><span data-stu-id="dea22-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="dea22-247">Naše název alba a umělcem název vlastnosti můžete obě být dostatečně dlouhé, může vyvolat vypnout naše formátování tabulky.</span><span class="sxs-lookup"><span data-stu-id="dea22-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="dea22-248">Vytvoříme vlastní pomocné rutiny HTML umožněte nám snadno zkrátit těchto a dalších vlastností v našem zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dea22-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="dea22-249">Syntaxe Razor pro @helper syntaxe má provedené velmi snadné vytvoření vlastní pomocné funkce pro použití v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dea22-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="dea22-250">Otevření zobrazení /Views/StoreManager/Index.cshtml a následující kód přidejte přímo po @model řádku.</span><span class="sxs-lookup"><span data-stu-id="dea22-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="dea22-251">Tato pomocná metoda přebírá řetězec a umožňuje maximální délku.</span><span class="sxs-lookup"><span data-stu-id="dea22-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="dea22-252">Pokud zadaný text je kratší než zadaná délka, pomocné rutiny výstupy jej jako-je.</span><span class="sxs-lookup"><span data-stu-id="dea22-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="dea22-253">Pokud je delší, zkrátí text a vykreslí "..." pro zbytek.</span><span class="sxs-lookup"><span data-stu-id="dea22-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="dea22-254">Nyní jsme vám pomůže naše Truncate pomocníka název alba i umělcem název vlastnosti musí být kratší než 25 znaků.</span><span class="sxs-lookup"><span data-stu-id="dea22-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="dea22-255">Kód dokončení zobrazení pomocí našeho nového pomocníka Truncate se zobrazí níže.</span><span class="sxs-lookup"><span data-stu-id="dea22-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="dea22-256">Nyní když jsme procházejí /StoreManager/ adresu URL, alba a názvy jsou zachovány níže naše maximální délky.</span><span class="sxs-lookup"><span data-stu-id="dea22-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="dea22-257">Poznámka: Zobrazí v případě jednoduchého vytváření a používání pomocné rutiny v jednom zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dea22-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="dea22-258">Další informace o vytváření Pomocníci, které můžete použít v celé vaší lokality, najdete v části Moje blogu: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="dea22-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="dea22-259">[Předchozí](mvc-music-store-part-4.md)
> [další](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="dea22-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>
