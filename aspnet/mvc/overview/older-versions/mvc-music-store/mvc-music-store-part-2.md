---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: '2. část: Kontrolery | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 2. část se věnuje řadiče.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8824d5d2f5670aee2df6dc6e74767e4a851dd4ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841092"
---
<a name="part-2-controllers"></a><span data-ttu-id="5f00c-104">2. část: Kontrolery</span><span class="sxs-lookup"><span data-stu-id="5f00c-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="5f00c-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="5f00c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="5f00c-106">MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5f00c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="5f00c-107">Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="5f00c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="5f00c-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="5f00c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="5f00c-109">2. část se věnuje řadiče.</span><span class="sxs-lookup"><span data-stu-id="5f00c-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="5f00c-110">Příchozí adresy URL se s tradičními webovými rozhraními obvykle mapují na soubory na disku.</span><span class="sxs-lookup"><span data-stu-id="5f00c-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="5f00c-111">Příklad: požadavek na adresu URL, jako je "/ Products.aspx" nebo "/ Products.php" může být zpracována "Products.aspx" nebo "Products.php" soubor.</span><span class="sxs-lookup"><span data-stu-id="5f00c-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="5f00c-112">Webové rozhraní MVC mapování adres URL do kódu serveru mírně odlišným způsobem.</span><span class="sxs-lookup"><span data-stu-id="5f00c-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="5f00c-113">Místo souborů mapování příchozích adres URL, že místo toho mapování adres URL na metody třídy.</span><span class="sxs-lookup"><span data-stu-id="5f00c-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="5f00c-114">Tyto třídy se nazývají "Řadiče" a zodpovídají za zpracování příchozích požadavků HTTP, zpracování uživatelského vstupu, načítání a ukládání dat a určení odpověď k odeslání zpátky do klienta (zobrazení HTML, stáhněte si soubor, přesměrovat na jinou Adresa URL atd.).</span><span class="sxs-lookup"><span data-stu-id="5f00c-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="5f00c-115">Přidání HomeController</span><span class="sxs-lookup"><span data-stu-id="5f00c-115">Adding a HomeController</span></span>

<span data-ttu-id="5f00c-116">Brzy jsme naše aplikace MVC Music Store tak, že přidáte třídu Kontroleru, který bude zpracovávat adresy URL na domovskou stránku našeho webu.</span><span class="sxs-lookup"><span data-stu-id="5f00c-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="5f00c-117">Vytvoříme dodržují konvence pojmenování výchozí rozhraní ASP.NET MVC a jeho volání HomeController.</span><span class="sxs-lookup"><span data-stu-id="5f00c-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="5f00c-118">Klikněte pravým tlačítkem na složku "Řadiče" v Průzkumníkovi řešení a vyberte "Add" a "Kontroleru..." příkaz:</span><span class="sxs-lookup"><span data-stu-id="5f00c-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="5f00c-119">Tím se otevře dialogové okno "Přidat kontroler".</span><span class="sxs-lookup"><span data-stu-id="5f00c-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="5f00c-120">Název kontroleru "HomeController" a stiskněte tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="5f00c-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="5f00c-121">Tím se vytvoří nový soubor, HomeController.cs, následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="5f00c-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="5f00c-122">Spustit co nejsnáze, podíváme nahradit Index – metoda s jednoduchý způsob, který právě vrátí hodnotu typu string.</span><span class="sxs-lookup"><span data-stu-id="5f00c-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="5f00c-123">Teď uděláme dvě změny:</span><span class="sxs-lookup"><span data-stu-id="5f00c-123">We'll make two changes:</span></span>

- <span data-ttu-id="5f00c-124">Změnit metodu k vrácení řetězce, místo třídu ActionResult</span><span class="sxs-lookup"><span data-stu-id="5f00c-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="5f00c-125">Změnit návratový příkaz návrat "Hello z"</span><span class="sxs-lookup"><span data-stu-id="5f00c-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="5f00c-126">Metoda by teď měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="5f00c-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="5f00c-127">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="5f00c-127">Running the Application</span></span>

<span data-ttu-id="5f00c-128">Teď spustíme webu.</span><span class="sxs-lookup"><span data-stu-id="5f00c-128">Now let's run the site.</span></span> <span data-ttu-id="5f00c-129">Jsme start náš webový server a vyzkoušejte si web pomocí kteréhokoli z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="5f00c-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="5f00c-130">Vyberte položku nabídky spustit ladění ladění ⇨</span><span class="sxs-lookup"><span data-stu-id="5f00c-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="5f00c-131">Klikněte na zelenou šipku tlačítko na panelu nástrojů ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="5f00c-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="5f00c-132">Použijte klávesovou zkratku, F5.</span><span class="sxs-lookup"><span data-stu-id="5f00c-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="5f00c-133">Některou z výše uvedených kroků se kompilace projektu a poté způsobí serveru ASP.NET Development Server, která je integrovaná do aplikace Visual Web Developer spustit.</span><span class="sxs-lookup"><span data-stu-id="5f00c-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="5f00c-134">Oznámení se zobrazí v dolním rohu obrazovky, chcete-li určit, že má spuštění serveru ASP.NET Development Server a zobrazí číslo portu, že je spuštěný pod.</span><span class="sxs-lookup"><span data-stu-id="5f00c-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="5f00c-135">Visual Web Developer se pak automaticky otevře okno prohlížeče, jehož adresa URL odkazuje na našem webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="5f00c-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="5f00c-136">Umožní vám to rychle vyzkoušet naši webovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="5f00c-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="5f00c-137">Dobře, to bylo poměrně rychlé – jsme vytvořili nový web, přidány tři vložených funkcí a máme text v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="5f00c-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="5f00c-138">Není Atomový vědy, ale je spuštění.</span><span class="sxs-lookup"><span data-stu-id="5f00c-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="5f00c-139">*Poznámka: Visual Web Developer zahrnuje serveru ASP.NET Development Server, který se spustí váš web na číslo náhodný volný "portu". Na snímku obrazovky výše je web spuštěný v `http://localhost:26641/`, takže se používá port 26641. Vaše číslo portu se liší. Když mluvíme o like /Store/Browse adresy URL v tomto kurzu, který přejde po číslo portu. Za předpokladu, že číslo portu 26641, přejdete do/Store/procházet bude znamenat, že přejdete na adresu `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="5f00c-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="5f00c-140">Přidání StoreController</span><span class="sxs-lookup"><span data-stu-id="5f00c-140">Adding a StoreController</span></span>

<span data-ttu-id="5f00c-141">Přidali jsme jednoduché HomeController, který implementuje domovské stránce našeho webu.</span><span class="sxs-lookup"><span data-stu-id="5f00c-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="5f00c-142">Teď přidáme jiný kontroler, který použijeme k implementaci funkcionality procházení našeho music úložiště.</span><span class="sxs-lookup"><span data-stu-id="5f00c-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="5f00c-143">Kontroleru úložiště bude podporovat tři scénáře:</span><span class="sxs-lookup"><span data-stu-id="5f00c-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="5f00c-144">Stránce žánrů Hudba v našich music store</span><span class="sxs-lookup"><span data-stu-id="5f00c-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="5f00c-145">Procházet stránku, která obsahuje seznam všech hudebních alb v konkrétní žánr</span><span class="sxs-lookup"><span data-stu-id="5f00c-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="5f00c-146">Stránka s podrobnostmi s informacemi o alba konkrétní Hudba</span><span class="sxs-lookup"><span data-stu-id="5f00c-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="5f00c-147">Začneme tak, že přidáte novou třídu StoreController...</span><span class="sxs-lookup"><span data-stu-id="5f00c-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="5f00c-148">Pokud jste tak dosud neučinili, zastaví aplikace tak, že zavření prohlížeče nebo jeho výběru položky nabídky ladění Zastavit ladění ⇨.</span><span class="sxs-lookup"><span data-stu-id="5f00c-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="5f00c-149">Nyní přidejte nové StoreController.</span><span class="sxs-lookup"><span data-stu-id="5f00c-149">Now add a new StoreController.</span></span> <span data-ttu-id="5f00c-150">Stejným způsobem, jako jsme to udělali s HomeController, Uděláme to tak, že pravým tlačítkem na složku "Řadiče" v Průzkumníkovi řešení a zvolíte Add -&gt;řadič položky nabídky</span><span class="sxs-lookup"><span data-stu-id="5f00c-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="5f00c-151">Naše nové StoreController již má metodu "Index".</span><span class="sxs-lookup"><span data-stu-id="5f00c-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="5f00c-152">Tato metoda "Index" použijeme k implementaci naší stránce, která obsahuje seznam všech žánry v našich music store.</span><span class="sxs-lookup"><span data-stu-id="5f00c-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="5f00c-153">Přidáme také další dvě metody k implementaci těchto dvou dalších scénářích chceme, aby naše StoreController ke zpracování: procházení a podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="5f00c-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="5f00c-154">Tyto metody (Index, procházet a podrobnosti) v rámci Kontroleru se používá označení "Akce Kontroleru" a jak pomocí metody akce () HomeController.Index již víte, své práce je a reagovat na žádosti o adresy URL (obecně) určit, jaký obsah by měly být odeslány zpět do prohlížeče nebo uživatel, který vyvolal adresu URL.</span><span class="sxs-lookup"><span data-stu-id="5f00c-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="5f00c-155">Začneme tak, že změníte theIndex() metoda vrátí řetězec "Hello z Store.Index()" naše implementace StoreController a přidáme Browse() a Details() podobné metody:</span><span class="sxs-lookup"><span data-stu-id="5f00c-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="5f00c-156">Spusťte projekt znovu a přejděte na následující adresy URL:</span><span class="sxs-lookup"><span data-stu-id="5f00c-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="5f00c-157">/ Store</span><span class="sxs-lookup"><span data-stu-id="5f00c-157">/Store</span></span>
- <span data-ttu-id="5f00c-158">/ Store/procházení</span><span class="sxs-lookup"><span data-stu-id="5f00c-158">/Store/Browse</span></span>
- <span data-ttu-id="5f00c-159">/ Store/podrobností</span><span class="sxs-lookup"><span data-stu-id="5f00c-159">/Store/Details</span></span>

<span data-ttu-id="5f00c-160">Přístup k těmto adresám URL vyvolání metod akce v Kontroleru a vrátí řetězec odpovědi:</span><span class="sxs-lookup"><span data-stu-id="5f00c-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="5f00c-161">To je skvělé, ale jedná se pouze konstantní řetězce.</span><span class="sxs-lookup"><span data-stu-id="5f00c-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="5f00c-162">Vytvoříme je dynamická, takže využít informace z adresy URL a zobrazit je výstup stránky.</span><span class="sxs-lookup"><span data-stu-id="5f00c-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="5f00c-163">Nejprve Změníme metody akce procházení k načtení hodnoty řetězce dotazu z adresy URL.</span><span class="sxs-lookup"><span data-stu-id="5f00c-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="5f00c-164">Můžeme to udělat tak, že přidáte "žánr" parametru k metodě akce.</span><span class="sxs-lookup"><span data-stu-id="5f00c-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="5f00c-165">Když to uděláme ASP.NET MVC předá automaticky všechny řetězce dotazu nebo parametry formuláře post s názvem "žánr" k metodě akce, při jeho vyvolání.</span><span class="sxs-lookup"><span data-stu-id="5f00c-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="5f00c-166">*Poznámka: Pomocí metody nástrojů HttpUtility.HtmlEncode kterého neupravují uživatelský vstup. To zabrání uživatelům v vkládání jazyka Javascript do našich zobrazení s odkazem jako /Store/Browse? Rozšířením podle tematických =&lt;skript&gt;window.location= "http://hackersite.com"&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="5f00c-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="5f00c-167">Nyní Pojďme procházet/Store/Procházet? Rozšířením podle tematických = Roz</span><span class="sxs-lookup"><span data-stu-id="5f00c-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="5f00c-168">Dále Změníme podrobnosti akce ke čtení a zobrazení vstupní parametr s názvem ID.</span><span class="sxs-lookup"><span data-stu-id="5f00c-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="5f00c-169">Na rozdíl od našich předchozí metoda jsme nebude vkládání hodnotu ID jako parametr řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="5f00c-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="5f00c-170">Místo toho vložíme ho přímo v rámci URL samotného.</span><span class="sxs-lookup"><span data-stu-id="5f00c-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="5f00c-171">Příklad: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="5f00c-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="5f00c-172">ASP.NET MVC umožňuje nám to snadno provést nemusíte nic konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="5f00c-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="5f00c-173">ASP.NET MVC výchozí konvenci směrování se zachází segment adresy URL po názvu metody akce, jako parametr s názvem "ID".</span><span class="sxs-lookup"><span data-stu-id="5f00c-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="5f00c-174">Pokud vaše metoda akce má parametr ID ASP.NET MVC se automaticky předat segment adresy URL pro vás jako parametr.</span><span class="sxs-lookup"><span data-stu-id="5f00c-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="5f00c-175">Spusťte aplikaci a přejděte do /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="5f00c-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="5f00c-176">Pojďme rekapitulace, co jsme dosud provedli:</span><span class="sxs-lookup"><span data-stu-id="5f00c-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="5f00c-177">Jsme vytvořili nový projekt ASP.NET MVC v aplikaci Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="5f00c-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="5f00c-178">Výše strukturu složek základní aplikaci ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5f00c-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="5f00c-179">Jsme jste zjistili, jak spustit našeho webu pomocí serveru ASP.NET Development Server</span><span class="sxs-lookup"><span data-stu-id="5f00c-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="5f00c-180">Vytvořili jsme dvě třídy Kontroleru: HomeController a StoreController</span><span class="sxs-lookup"><span data-stu-id="5f00c-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="5f00c-181">Přidali jsme metody akce k naší řadiče, které reagují na požadavky na adresu URL a vrací text do prohlížeče</span><span class="sxs-lookup"><span data-stu-id="5f00c-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="5f00c-182">[Předchozí](mvc-music-store-part-1.md)
> [další](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="5f00c-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
