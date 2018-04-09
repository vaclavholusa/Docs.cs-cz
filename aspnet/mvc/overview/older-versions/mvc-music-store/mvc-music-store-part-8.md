---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Část 8: Nákupní košík s aktualizace Ajax | Microsoft Docs'
author: jongalloway
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 8 obsahuje nákupní košík s aktualizace Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="280fd-104">Část 8: Nákupní košík s aktualizace Ajax</span><span class="sxs-lookup"><span data-stu-id="280fd-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="280fd-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="280fd-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="280fd-106">Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.</span><span class="sxs-lookup"><span data-stu-id="280fd-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="280fd-107">Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="280fd-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="280fd-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="280fd-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="280fd-109">Část 8 obsahuje nákupní košík s aktualizace Ajax.</span><span class="sxs-lookup"><span data-stu-id="280fd-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="280fd-110">Jsme vám umožňují uživatelům umístit alb jejich košíku bez registrace, ale budete muset zaregistrovat jako hosty do dokončení checkout.</span><span class="sxs-lookup"><span data-stu-id="280fd-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="280fd-111">Proces nákupního a najdete v článku věnovaném rozdělena na dva řadiče: ShoppingCart řadiči, který umožňuje anonymně přidávání položek do košíku a řadič Checkout, která zpracovává proces checkout.</span><span class="sxs-lookup"><span data-stu-id="280fd-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="280fd-112">Jsme budete začínat nákupní košík v této části a pak vytvořit proces najdete v článku věnovaném v následující části.</span><span class="sxs-lookup"><span data-stu-id="280fd-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="280fd-113">Přidání třídy modelu košíku, pořadím a OrderDetail</span><span class="sxs-lookup"><span data-stu-id="280fd-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="280fd-114">Naše nákupní košík a najdete v článku věnovaném procesy budou používat některých nových tříd.</span><span class="sxs-lookup"><span data-stu-id="280fd-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="280fd-115">Klikněte pravým tlačítkem na složku modely a přidejte třídu košíku (Cart.cs) s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="280fd-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="280fd-116">Tato třída je poměrně podobně jako ostatní, které jste použili jsme dosavadní práce, s výjimkou [klíč] atributu pro vlastnost RecordId.</span><span class="sxs-lookup"><span data-stu-id="280fd-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="280fd-117">Naše položky v košíku budou mít identifikátor řetězec s názvem CartID umožnit anonymní nákupy, ale v tabulce jsou zahrnuty celé číslo primární klíč s názvem RecordId.</span><span class="sxs-lookup"><span data-stu-id="280fd-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="280fd-118">Podle konvence Entity Framework Code-první očekává primární klíč pro tabulku s názvem košíku budou CartId nebo ID, že jsme můžete snadno přepsat, prostřednictvím poznámky nebo kód pokud chceme.</span><span class="sxs-lookup"><span data-stu-id="280fd-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="280fd-119">Toto je příklad, jak můžete použít jednoduchý konvence v Entity Framework Code-první při jejich nám vyhovovat, ale nemůžeme nejsou omezeny je při ne.</span><span class="sxs-lookup"><span data-stu-id="280fd-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="280fd-120">Dál přidejte třídu pořadí (Order.cs) s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="280fd-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="280fd-121">Tato třída sleduje informace o pořadí souhrn a doručení.</span><span class="sxs-lookup"><span data-stu-id="280fd-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="280fd-122">**Nebude zkompilován ještě**, protože obsahuje rozpis objednávek navigační vlastnost, na které závisí na třídě jsme ještě nevytvořili.</span><span class="sxs-lookup"><span data-stu-id="280fd-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="280fd-123">Umožňuje opravit, že nyní přidáním třídu s názvem OrderDetail.cs, přidání následující kód.</span><span class="sxs-lookup"><span data-stu-id="280fd-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="280fd-124">Vytočit jednu poslední aktualizace našich MusicStoreEntities třídy zahrnout DbSets, který vystavit tyto nové třídy modelu, včetně také DbSet&lt;umělcem&gt;.</span><span class="sxs-lookup"><span data-stu-id="280fd-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="280fd-125">Aktualizované třídy MusicStoreEntities se zobrazí jako níže.</span><span class="sxs-lookup"><span data-stu-id="280fd-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="280fd-126">Správa obchodní logiky nákupní košík</span><span class="sxs-lookup"><span data-stu-id="280fd-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="280fd-127">V dalším kroku vytvoříme třídu ShoppingCart ve složce modelů.</span><span class="sxs-lookup"><span data-stu-id="280fd-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="280fd-128">ShoppingCart model zpracovává data přístup k tabulce košíku.</span><span class="sxs-lookup"><span data-stu-id="280fd-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="280fd-129">Kromě toho zpracuje obchodní logiku pro přidávání a odebírání položek z nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="280fd-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="280fd-130">Vzhledem k tomu, že jsme nechcete, aby tak, aby vyžadovala uživatelům zaregistrovat pro účet, stačí k přidávání položek do jejich nákupní košík, jsme přiřadí uživatelům dočasné jedinečný identifikátor (pomocí identifikátoru GUID nebo globálně jedinečný identifikátor) při přístupu k nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="280fd-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="280fd-131">Uložíme toto ID pomocí třídy relace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="280fd-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="280fd-132">*Poznámka: Relace ASP.NET je vhodné místo k uložení informace specifické pro uživatele, který vyprší po jejich opustit web. Při zneužití stav relace může mít vliv na výkon na větší lokalit, naše světla používání bude fungovat i pro demonstrační účely.*</span><span class="sxs-lookup"><span data-stu-id="280fd-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="280fd-133">Třída ShoppingCart poskytuje následující metody:</span><span class="sxs-lookup"><span data-stu-id="280fd-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="280fd-134">**AddToCart** trvá Album jako parametr a přidá ji do košíku uživatele.</span><span class="sxs-lookup"><span data-stu-id="280fd-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="280fd-135">Vzhledem k tomu, že v tabulce košíku sleduje množství pro každý alb, obsahuje logiku a v případě potřeby vytvořte nový řádek nebo právě zvýšit množství, pokud uživatel již má seřazené jedna kopie alba.</span><span class="sxs-lookup"><span data-stu-id="280fd-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="280fd-136">**RemoveFromCart** trvá ID alba a odstraní ji z košíku uživatele.</span><span class="sxs-lookup"><span data-stu-id="280fd-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="280fd-137">Pokud má uživatel, jenom jedna kopie alba v jejich košíku, odeberou se řádek.</span><span class="sxs-lookup"><span data-stu-id="280fd-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="280fd-138">**EmptyCart** odebere všechny položky z nákupní košík uživatele.</span><span class="sxs-lookup"><span data-stu-id="280fd-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="280fd-139">**GetCartItems** načte seznam CartItems pro zobrazení nebo zpracování.</span><span class="sxs-lookup"><span data-stu-id="280fd-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="280fd-140">**GetCount –** načte celkový počet alb má uživatel v jejich nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="280fd-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="280fd-141">**GetTotal** vypočítá celkové náklady na všechny položky v košíku.</span><span class="sxs-lookup"><span data-stu-id="280fd-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="280fd-142">**CreateOrder** převede nákupní košík pořadí během fáze checkout.</span><span class="sxs-lookup"><span data-stu-id="280fd-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="280fd-143">**GetCart** je statickou metodu, která umožňuje naše řadiče pro získání objektu košíku.</span><span class="sxs-lookup"><span data-stu-id="280fd-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="280fd-144">Použije **GetCartId** metodu ke zpracování čtení CartId z relace uživatele.</span><span class="sxs-lookup"><span data-stu-id="280fd-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="280fd-145">Metoda GetCartId vyžaduje hodnotu HttpContextBase tak, aby ho může číst CartId uživatele z relace uživatele.</span><span class="sxs-lookup"><span data-stu-id="280fd-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="280fd-146">Tady je kompletní **ShoppingCart třída**:</span><span class="sxs-lookup"><span data-stu-id="280fd-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="280fd-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="280fd-147">ViewModels</span></span>

<span data-ttu-id="280fd-148">Naše nákupního košíku řadiče muset komunikovat některé komplexní informace k jeho zobrazení, který není řádně namapován na našem objekty modelu.</span><span class="sxs-lookup"><span data-stu-id="280fd-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="280fd-149">Neradi upravit naše modely tak, aby vyhovovala naše zobrazení; Třídy modelu by měl představovat naše domény, ne uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="280fd-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="280fd-150">Jedno řešení by k předání informací o našem zobrazeními pomocí třídy ViewBag, jako jsme to udělali s informace rozevírací správce obchodu, avšak předávání velké množství informací prostřednictvím ViewBag získá obtížné spravovat.</span><span class="sxs-lookup"><span data-stu-id="280fd-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="280fd-151">Toto řešení je použití *ViewModel* vzor.</span><span class="sxs-lookup"><span data-stu-id="280fd-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="280fd-152">Při použití tohoto vzoru vytvoříme silně typované třídy, který jsou optimalizované pro naše zobrazení konkrétní scénáře, a který vystavit vlastnosti pro dynamické hodnoty nebo obsah vyžaduje našich šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="280fd-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="280fd-153">Naše třídy kontroleru můžete naplnit a předat naše zobrazit šablonu použít tyto třídy optimalizované zobrazení.</span><span class="sxs-lookup"><span data-stu-id="280fd-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="280fd-154">To umožňuje bezpečnost typů, který kontroluje kompilaci a editor IntelliSense v rámci šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="280fd-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="280fd-155">Vytvoříme dva modely zobrazení pro použití v kontroleru nákupní košík: ShoppingCartViewModel bude obsahovat obsah nákupní košík uživatele a ShoppingCartRemoveViewModel se použije k zobrazení informací potvrzení, když uživatel něco vyjme z jejich košíku.</span><span class="sxs-lookup"><span data-stu-id="280fd-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="280fd-156">Umožňuje vytvořit novou složku ViewModels v kořenovém naše projektu k uspořádání informací.</span><span class="sxs-lookup"><span data-stu-id="280fd-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="280fd-157">Klikněte pravým tlačítkem na projekt, vyberte možnost Přidat nebo novou složku.</span><span class="sxs-lookup"><span data-stu-id="280fd-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="280fd-158">Název složky ViewModels.</span><span class="sxs-lookup"><span data-stu-id="280fd-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="280fd-159">V dalším kroku přidejte třídu ShoppingCartViewModel ve složce ViewModels.</span><span class="sxs-lookup"><span data-stu-id="280fd-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="280fd-160">Má dvě vlastnosti: seznam položek košíku a desetinnou hodnotu pro uložení celková cena pro všechny položky v košíku.</span><span class="sxs-lookup"><span data-stu-id="280fd-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="280fd-161">Teď můžete přidáte ShoppingCartRemoveViewModel ke složce ViewModels s následujícími vlastnostmi čtyři.</span><span class="sxs-lookup"><span data-stu-id="280fd-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="280fd-162">Řadičem nákupní košík</span><span class="sxs-lookup"><span data-stu-id="280fd-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="280fd-163">Řadičem nákupní košík má tři hlavní funkce: přidávání položek do košíku, odebrání položky z košíku a zobrazení položky v košíku.</span><span class="sxs-lookup"><span data-stu-id="280fd-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="280fd-164">Bude nutné používat tří tříd jsme právě vytvořili: ShoppingCartViewModel, ShoppingCartRemoveViewModel a ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="280fd-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="280fd-165">Jako StoreController a StoreManagerController přidáme pole pro instanci MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="280fd-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="280fd-166">Přidejte nový řadič nákupního košíku do projektu pomocí šablony prázdný kontroler.</span><span class="sxs-lookup"><span data-stu-id="280fd-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="280fd-167">Zde je kompletní ShoppingCart řadiče.</span><span class="sxs-lookup"><span data-stu-id="280fd-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="280fd-168">Index a přidat kontroler akce velmi povědomé.</span><span class="sxs-lookup"><span data-stu-id="280fd-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="280fd-169">Akce kontroleru odebrat a CartSummary zpracovávat dva speciální případů, které probereme v následující části.</span><span class="sxs-lookup"><span data-stu-id="280fd-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="280fd-170">Aktualizace AJAX s jQuery</span><span class="sxs-lookup"><span data-stu-id="280fd-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="280fd-171">Dále vytvoříme nákupního košíku Index stránky, který je pro ShoppingCartViewModel silného typu a, použije se stejným způsobem jako před šablona zobrazení seznamu.</span><span class="sxs-lookup"><span data-stu-id="280fd-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="280fd-172">Však místo použití Html.ActionLink odebrat položky z košíku, použijeme jQuery k "připojit až" klikněte na událost pro všechna propojení v tomto zobrazení, které mají třídu HTML při odebírání odkazu.</span><span class="sxs-lookup"><span data-stu-id="280fd-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="280fd-173">Místo publikování formuláře, této obslužné rutiny události klikněte na právě budou zpětného volání AJAX do našich RemoveFromCart akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="280fd-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="280fd-174">RemoveFromCart vrací výsledek serializován do formátu JSON, který pak analyzuje a provede čtyři rychlé aktualizace na stránku pomocí jQuery naše jQuery zpětného volání:</span><span class="sxs-lookup"><span data-stu-id="280fd-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="280fd-175">Odebere odstraněné album ze seznamu</span><span class="sxs-lookup"><span data-stu-id="280fd-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="280fd-176">Aktualizace počet košíku v hlavičce</span><span class="sxs-lookup"><span data-stu-id="280fd-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="280fd-177">Zobrazí aktualizace zprávu pro uživatele</span><span class="sxs-lookup"><span data-stu-id="280fd-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="280fd-178">Celková cena košíku aktualizací</span><span class="sxs-lookup"><span data-stu-id="280fd-178">Updates the cart total price</span></span>

<span data-ttu-id="280fd-179">Vzhledem k tomu, že zpětného volání Ajax v rámci zobrazení indexu je zpracovanou scénář odebrat, není potřebujeme další zobrazení RemoveFromCart akce.</span><span class="sxs-lookup"><span data-stu-id="280fd-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="280fd-180">Tady je kompletní kód /ShoppingCart/Index zobrazení:</span><span class="sxs-lookup"><span data-stu-id="280fd-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="280fd-181">Chcete-li otestovat to zjistit, potřebujeme mít k přidávání položek do naše řešení nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="280fd-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="280fd-182">Budete aktualizujeme naše **podrobnosti úložiště** aby zobrazení zahrnovalo tlačítko "Přidat do košíku".</span><span class="sxs-lookup"><span data-stu-id="280fd-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="280fd-183">Když jsme se na to, jsme jsou některé z alba Další informace, které jsme přidali vzhledem k tomu, že jsme v tomto zobrazení poslední aktualizace: Genre, umělcem, ceny a alba.</span><span class="sxs-lookup"><span data-stu-id="280fd-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="280fd-184">Kód zobrazení aktualizované podrobnosti úložiště se zobrazí, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="280fd-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="280fd-185">Nyní jsme klikněte na tlačítko prostřednictvím úložiště a otestovat přidávání a odebírání alb do a z naše řešení nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="280fd-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="280fd-186">Spusťte aplikaci a přejděte do Index úložiště.</span><span class="sxs-lookup"><span data-stu-id="280fd-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="280fd-187">Klikněte na tlačítko na Genre, chcete-li zobrazit seznam alb.</span><span class="sxs-lookup"><span data-stu-id="280fd-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="280fd-188">Kliknutím na název alba nyní zobrazuje naše aktualizované zobrazení podrobností alb, včetně tlačítko "Přidat do košíku".</span><span class="sxs-lookup"><span data-stu-id="280fd-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="280fd-189">Kliknutím na tlačítko "Přidat do košíku" ukazuje naše nákupní košík Index zobrazení seznamu souhrn nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="280fd-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="280fd-190">Po načtení do nákupního košíku, můžete kliknutím na odebrat z košíku odkaz zobrazíte aktualizace Ajax do nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="280fd-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="280fd-191">Sestavili jsme si funkční nákupní košík, což umožňuje uživatelům zrušit registraci k přidávání položek do jejich košíku.</span><span class="sxs-lookup"><span data-stu-id="280fd-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="280fd-192">V následující části jsme budete mohly k registraci a dokončete proces checkout.</span><span class="sxs-lookup"><span data-stu-id="280fd-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="280fd-193">[Předchozí](mvc-music-store-part-7.md)
> [další](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="280fd-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
