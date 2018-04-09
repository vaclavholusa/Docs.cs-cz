---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Část 2: Řadiče | Microsoft Docs'
author: jongalloway
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 2 popisuje řadiče.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 680cdea388d9b01961bd626643c0fd91c9205ed7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-2-controllers"></a><span data-ttu-id="ce321-104">Část 2: řadiče</span><span class="sxs-lookup"><span data-stu-id="ce321-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="ce321-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="ce321-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="ce321-106">Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.</span><span class="sxs-lookup"><span data-stu-id="ce321-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="ce321-107">Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="ce321-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="ce321-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="ce321-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="ce321-109">Část 2 popisuje řadiče.</span><span class="sxs-lookup"><span data-stu-id="ce321-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="ce321-110">Tradiční webové rozhraní jsou příchozí adresy URL obvykle mapovány na soubory na disku.</span><span class="sxs-lookup"><span data-stu-id="ce321-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="ce321-111">Příklad: žádost o adresu URL, například "/ Products.aspx" nebo "/ Products.php" může zpracovat soubor "Products.aspx" nebo "Products.php".</span><span class="sxs-lookup"><span data-stu-id="ce321-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="ce321-112">Webové rozhraní MVC mapování adres URL na serverový kód mírně jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="ce321-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="ce321-113">Místo mapování příchozí adresy URL k souborům, mapují místo adresy URL pro metody na třídách.</span><span class="sxs-lookup"><span data-stu-id="ce321-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="ce321-114">Tyto třídy, se nazývají "Řadiče" a jsou zodpovědná za zpracování příchozí požadavky protokolu HTTP a zpracování uživatelského vstupu, načítání a ukládání dat a určení odpověď k odeslání zpět do klienta (zobrazení HTML, stažení souboru, přesměrování na jiný Adresa URL atd.).</span><span class="sxs-lookup"><span data-stu-id="ce321-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="ce321-115">Přidání HomeController</span><span class="sxs-lookup"><span data-stu-id="ce321-115">Adding a HomeController</span></span>

<span data-ttu-id="ce321-116">Začneme budete naše aplikace MVC Hudba úložiště přidáním řadiče třídu, která bude zpracovávat adresy URL na domovskou stránku tohoto webu.</span><span class="sxs-lookup"><span data-stu-id="ce321-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="ce321-117">Jsme budete postupovat podle výchozí zásady vytváření názvů rozhraní ASP.NET MVC a volání HomeController.</span><span class="sxs-lookup"><span data-stu-id="ce321-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="ce321-118">"Řadiče" složky v Průzkumníku řešení klikněte pravým tlačítkem a vyberte "Přidat" a "Řadiče..."</span><span class="sxs-lookup"><span data-stu-id="ce321-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…"</span></span> <span data-ttu-id="ce321-119">příkaz:</span><span class="sxs-lookup"><span data-stu-id="ce321-119">command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="ce321-120">Otevře se dialog "Přidat kontroler".</span><span class="sxs-lookup"><span data-stu-id="ce321-120">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="ce321-121">Název kontroleru "HomeController" a klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="ce321-121">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="ce321-122">Tím se vytvoří nový soubor, HomeController.cs, s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ce321-122">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="ce321-123">Pokud chcete spustit co nejsnáze, můžeme nahraďte metodu Index jednoduché metodu, která právě vrátí řetězec.</span><span class="sxs-lookup"><span data-stu-id="ce321-123">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="ce321-124">Vytočit dvě změny:</span><span class="sxs-lookup"><span data-stu-id="ce321-124">We'll make two changes:</span></span>

- <span data-ttu-id="ce321-125">Změnit metodu vrátit řetězec místo třídu ActionResult</span><span class="sxs-lookup"><span data-stu-id="ce321-125">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="ce321-126">Změnit návratový vrátit "Hello z Domů"</span><span class="sxs-lookup"><span data-stu-id="ce321-126">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="ce321-127">Metoda by měl nyní vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="ce321-127">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="ce321-128">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ce321-128">Running the Application</span></span>

<span data-ttu-id="ce321-129">Teď umožňuje spuštění tohoto webu.</span><span class="sxs-lookup"><span data-stu-id="ce321-129">Now let's run the site.</span></span> <span data-ttu-id="ce321-130">Můžeme spustí naše webový server a vyzkoušejte webu pomocí kteréhokoli z následujících::</span><span class="sxs-lookup"><span data-stu-id="ce321-130">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="ce321-131">Zvolte ladění ⇨ spustit ladění položku nabídky</span><span class="sxs-lookup"><span data-stu-id="ce321-131">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="ce321-132">Klikněte na tlačítko zelenou šipku na panelu nástrojů ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="ce321-132">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="ce321-133">Použijte klávesovou zkratku, F5.</span><span class="sxs-lookup"><span data-stu-id="ce321-133">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="ce321-134">Pomocí kterékoli z výše uvedené kroky budou kompilaci naše projektu a potom způsobit, že je ASP.NET Development Server, který je integrovaný do aplikace Visual Web Developer spustit.</span><span class="sxs-lookup"><span data-stu-id="ce321-134">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="ce321-135">Oznámení se zobrazí v dolním rohu obrazovky indikující, že je ASP.NET Development Server byla spuštěna a zobrazí číslo portu, zda je spuštěna pod.</span><span class="sxs-lookup"><span data-stu-id="ce321-135">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="ce321-136">Visual Web Developer se pak automaticky otevře okno prohlížeče, jejichž adresa URL odkazuje na našem webu.</span><span class="sxs-lookup"><span data-stu-id="ce321-136">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="ce321-137">To vám umožní nám pro rychlé vyzkoušení naše webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="ce321-137">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="ce321-138">V pořádku, který byl poměrně rychle – jsme vytvořili nový web, přidat tři vložených funkcí a My jsme text v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ce321-138">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="ce321-139">Není Atomový vědecké účely, ale je spuštění.</span><span class="sxs-lookup"><span data-stu-id="ce321-139">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="ce321-140">*Poznámka: Visual Web Developer obsahuje vývojový Server ASP.NET, který se spustí vašeho webu na číslo volné náhodných "portu". Na snímku obrazovky výše, lokality se spouští ve `http://localhost:26641/`, takže používá port 26641. Vaše číslo portu se liší. Když v souvislosti se například /Store/Browse adresy URL v tomto kurzu, který přejde po číslo portu. Za předpokladu, že číslo portu 26641, procházení/úložiště / procházení bude znamenat, že procházení k `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="ce321-140">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="ce321-141">Přidání StoreController</span><span class="sxs-lookup"><span data-stu-id="ce321-141">Adding a StoreController</span></span>

<span data-ttu-id="ce321-142">Jsme přidali jednoduché HomeController, který implementuje domovské stránce tohoto webu.</span><span class="sxs-lookup"><span data-stu-id="ce321-142">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="ce321-143">Nyní Pojďme přidat jiného řadiče, který použijeme k implementaci funkci procházení naše Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="ce321-143">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="ce321-144">Naše řadič úložiště bude podporovat tři scénáře:</span><span class="sxs-lookup"><span data-stu-id="ce321-144">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="ce321-145">Stránka výpis žánry Hudba v našem úložišti Hudba</span><span class="sxs-lookup"><span data-stu-id="ce321-145">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="ce321-146">Procházet stránky, který se zobrazí seznam všech hudebních alb v konkrétní genre</span><span class="sxs-lookup"><span data-stu-id="ce321-146">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="ce321-147">Stránka Podrobnosti, která se zobrazují informace o konkrétní Hudba album</span><span class="sxs-lookup"><span data-stu-id="ce321-147">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="ce321-148">Začneme přidáním nové třídy StoreController...</span><span class="sxs-lookup"><span data-stu-id="ce321-148">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="ce321-149">Pokud jste to ještě neudělali, zastaví aplikace buď zavření prohlížeče nebo výběrem položky nabídky Zastavte ladění ladění ⇨.</span><span class="sxs-lookup"><span data-stu-id="ce321-149">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="ce321-150">Nyní přidejte nové StoreController.</span><span class="sxs-lookup"><span data-stu-id="ce321-150">Now add a new StoreController.</span></span> <span data-ttu-id="ce321-151">Stejně jako jsme to udělali s HomeController, provedeme to pravým tlačítkem na složku "Řadiče" v Průzkumníku řešení a zvolením Add -&gt;řadiče položky nabídky</span><span class="sxs-lookup"><span data-stu-id="ce321-151">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="ce321-152">Naše nové StoreController již metodu "Index".</span><span class="sxs-lookup"><span data-stu-id="ce321-152">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="ce321-153">Tato metoda "Index" použijeme k implementaci na naší stránce výpis, který obsahuje seznam všech žánry v našem úložišti Hudba.</span><span class="sxs-lookup"><span data-stu-id="ce321-153">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="ce321-154">Přidáme také dva další metody k implementaci dva scénáře chceme, aby naše StoreController pro zpracování: procházení a podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ce321-154">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="ce321-155">Tyto metody (Index, procházet a podrobnosti) v rámci Kontroleru se používá označení "Akce Kontroleru" a jak jste už viděli s metodou akce HomeController.Index (), jejich úlohy je a reagovat na požadavky na adresu URL (obecně) určit, který obsah by měly být odeslány zpět do prohlížeče nebo uživatele, který volá adresu URL.</span><span class="sxs-lookup"><span data-stu-id="ce321-155">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="ce321-156">Začneme naše implementace StoreController změnou theIndex() metoda vrátí řetězec "Hello z Store.Index()" a přidáme podobné metody pro Browse() a Details():</span><span class="sxs-lookup"><span data-stu-id="ce321-156">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="ce321-157">Spusťte projekt znovu a přejděte na následující adresy URL:</span><span class="sxs-lookup"><span data-stu-id="ce321-157">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="ce321-158">/ Úložiště</span><span class="sxs-lookup"><span data-stu-id="ce321-158">/Store</span></span>
- <span data-ttu-id="ce321-159">/ Úložiště nebo procházení</span><span class="sxs-lookup"><span data-stu-id="ce321-159">/Store/Browse</span></span>
- <span data-ttu-id="ce321-160">/ / Podrobnosti úložiště</span><span class="sxs-lookup"><span data-stu-id="ce321-160">/Store/Details</span></span>

<span data-ttu-id="ce321-161">Přístup k tyto adresy URL vyvolání metod akce v rámci Kontroleru a vrátí řetězec odpovědí:</span><span class="sxs-lookup"><span data-stu-id="ce321-161">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="ce321-162">To je výborné, ale jedná se o řetězce pouze konstantní.</span><span class="sxs-lookup"><span data-stu-id="ce321-162">That's great, but these are just constant strings.</span></span> <span data-ttu-id="ce321-163">Umožňuje převést na dynamický, tak, aby se trvat informace z adresy URL a zobrazit ji ve výstupu stránky.</span><span class="sxs-lookup"><span data-stu-id="ce321-163">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="ce321-164">Nejprve Změníme metody akce procházet načíst hodnotu řetězce dotazu z adresy URL.</span><span class="sxs-lookup"><span data-stu-id="ce321-164">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="ce321-165">Jsme to můžete provést tak, že přidáte parametr "genre" na našem metodu akce.</span><span class="sxs-lookup"><span data-stu-id="ce321-165">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="ce321-166">Když jsme to ASP.NET MVC automaticky předat žádné parametry post řetězci dotazu nebo formuláře při vyvolání s názvem "genre" na našem metodu akce.</span><span class="sxs-lookup"><span data-stu-id="ce321-166">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="ce321-167">*Poznámka: Používáme metodu HttpUtility.HtmlEncode nástroj pro úpravu vstup uživatele. To zabrání uživatelům vložení Javascript do našich zobrazení s odkazem jako /Store/Browse? Genre =&lt;skriptu&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="ce321-167">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="ce321-168">Teď umožňuje procházet/úložiště / Procházet? Genre = Disco</span><span class="sxs-lookup"><span data-stu-id="ce321-168">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="ce321-169">Změňte další akce ke čtení a zobrazení vstupní parametr s názvem ID.</span><span class="sxs-lookup"><span data-stu-id="ce321-169">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="ce321-170">Na rozdíl od našich předchozí metoda jsme nebude vložení hodnota ID jako parametr řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="ce321-170">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="ce321-171">Místo toho jsme budete vložte přímo v rámci adresy URL samotné.</span><span class="sxs-lookup"><span data-stu-id="ce321-171">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="ce321-172">Příklad: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="ce321-172">For example: /Store/Details/5.</span></span>

<span data-ttu-id="ce321-173">ASP.NET MVC umožňuje nám snadno to provést bez nutnosti měnit žádnou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ce321-173">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="ce321-174">ASP.NET MVC výchozí konvenci směrování se zachází segment adresy URL jako parametr s názvem "ID" po název metody akce.</span><span class="sxs-lookup"><span data-stu-id="ce321-174">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="ce321-175">Pokud má parametr s názvem ID své metodě akce, poté ASP.NET MVC automaticky předá segment adresy URL je jako parametr.</span><span class="sxs-lookup"><span data-stu-id="ce321-175">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="ce321-176">Spusťte aplikaci a přejděte do /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="ce321-176">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="ce321-177">Pojďme recap, co máme jste Hotovo, pokud:</span><span class="sxs-lookup"><span data-stu-id="ce321-177">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="ce321-178">Vytvořili jsme nový projekt ASP.NET MVC v aplikaci Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="ce321-178">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="ce321-179">Jsme probrali struktura složek základní architektury ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ce321-179">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="ce321-180">Jsme jste se naučili spuštění naše webu pomocí vývojový Server ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ce321-180">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="ce321-181">Vytvořili jsme dvě třídy Controller: HomeController a StoreController</span><span class="sxs-lookup"><span data-stu-id="ce321-181">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="ce321-182">Přidali jsme metody akce na našem řadiče, které reagují na požadavky na adresu URL a vrací text do prohlížeče</span><span class="sxs-lookup"><span data-stu-id="ce321-182">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="ce321-183">[Předchozí](mvc-music-store-part-1.md)
> [další](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="ce321-183">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
