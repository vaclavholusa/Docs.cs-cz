---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: '8. část: Nákupní košík s aktualizacemi Ajax | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 8. část se věnuje nákupní košík s aktualizacemi Ajax.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 881d47b5b4df5a4d310a1b3a7eec6ee97b0d42ea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823836"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="b4de9-104">8. část: Nákupní košík s aktualizacemi Ajax</span><span class="sxs-lookup"><span data-stu-id="b4de9-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="b4de9-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="b4de9-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="b4de9-106">MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b4de9-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="b4de9-107">Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="b4de9-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="b4de9-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="b4de9-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="b4de9-109">8. část se věnuje nákupní košík s aktualizacemi Ajax.</span><span class="sxs-lookup"><span data-stu-id="b4de9-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="b4de9-110">Jsme vám umožňují uživatelům umístit alb v jejich košíku i bez registrace, ale budete muset zaregistrovat jako hosty do dokončení registrace.</span><span class="sxs-lookup"><span data-stu-id="b4de9-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="b4de9-111">Proces nákupu a registrace bude možné rozdělit na dva řadiče: ShoppingCart kontroler, který umožňuje anonymně přidávání položek do košíku a řadiči Checkout, která zpracovává proces platby u pokladny.</span><span class="sxs-lookup"><span data-stu-id="b4de9-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="b4de9-112">Jsme budete začínat nákupního košíku v této části a pak vytvořit proces platby u pokladny v následující části.</span><span class="sxs-lookup"><span data-stu-id="b4de9-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="b4de9-113">Přidání třídy modelu košíku, pořadí a OrderDetail</span><span class="sxs-lookup"><span data-stu-id="b4de9-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="b4de9-114">Způsobí, že naše nákupního košíku a Git Checkout procesy pomocí některých nových tříd.</span><span class="sxs-lookup"><span data-stu-id="b4de9-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="b4de9-115">Klikněte pravým tlačítkem na složku modely a přidejte třídu košíku (Cart.cs) následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="b4de9-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="b4de9-116">Tato třída je hodně podobné jiným, které jsme zatím použili s výjimkou atribut [Key] pro vlastnost RecordId.</span><span class="sxs-lookup"><span data-stu-id="b4de9-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="b4de9-117">Naše košíku položky budou mít identifikátor řetězce s názvem CartID umožňující anonymní nákupu, ale v tabulce obsahuje celé číslo primární klíč s názvem RecordId.</span><span class="sxs-lookup"><span data-stu-id="b4de9-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="b4de9-118">Podle konvence Entity Framework Code-First očekává, že primární klíč pro tabulku s názvem košíku bude CartId nebo ID, ale jsme můžete snadno změnit prostřednictvím poznámky nebo kód pokud chceme.</span><span class="sxs-lookup"><span data-stu-id="b4de9-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="b4de9-119">Toto je příklad, jak můžete použít jednoduchý konvence v Entity Framework Code-First při vyhovují nám ale jsme nejsou omezeny je když ne.</span><span class="sxs-lookup"><span data-stu-id="b4de9-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="b4de9-120">V dalším kroku přidejte třídu pořadí (Order.cs) následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="b4de9-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="b4de9-121">Tato třída sleduje informace o souhrnné informace a doručování pro objednávky.</span><span class="sxs-lookup"><span data-stu-id="b4de9-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="b4de9-122">**Nebude zkompilován ještě**, protože má OrderDetails navigační vlastnost, na které závisí na třídě jsme ještě nevytvořili.</span><span class="sxs-lookup"><span data-stu-id="b4de9-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="b4de9-123">Opravte Řekněme, že nyní přidáním třída s názvem OrderDetail.cs, přidáním následujícího kódu.</span><span class="sxs-lookup"><span data-stu-id="b4de9-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="b4de9-124">Teď uděláme jeden poslední aktualizace našich MusicStoreEntities třídy, aby obsahoval DbSets, která zpřístupňují tyto nové třídy modelu, včetně také DbSet&lt;interpreta&gt;.</span><span class="sxs-lookup"><span data-stu-id="b4de9-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="b4de9-125">Aktualizované MusicStoreEntities třídy se zobrazí jako níže.</span><span class="sxs-lookup"><span data-stu-id="b4de9-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="b4de9-126">Správa obchodní logiky nákupního košíku</span><span class="sxs-lookup"><span data-stu-id="b4de9-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="b4de9-127">V dalším kroku vytvoříme třídy ShoppingCart ve složce modely.</span><span class="sxs-lookup"><span data-stu-id="b4de9-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="b4de9-128">ShoppingCart model zajišťuje přístup k datům do košíku tabulky.</span><span class="sxs-lookup"><span data-stu-id="b4de9-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="b4de9-129">Kromě toho zajistí obchodní logiku pro přidávání a odebírání položek z nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="b4de9-130">Protože nechceme budou muset uživatelé zaregistrovat účet pouze pro přidání položky do jejich nákupního košíku, přiřadíme uživatele dočasné jedinečný identifikátor (pomocí identifikátoru GUID, nebo globálně jedinečný identifikátor) když přistupují k nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="b4de9-131">Uložíme toto ID horizontálních oddílů pomocí třídy relace technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b4de9-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="b4de9-132">*Poznámka: Relace technologie ASP.NET je vhodné místo pro ukládání informací specifických pro uživatele, jejíž platnost vyprší po opuštění webu. Zatímco zneužití stav relace může mít vliv na výkon na větších serverech, používáme světla budou fungovat dobře pro demonstrační účely.*</span><span class="sxs-lookup"><span data-stu-id="b4de9-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="b4de9-133">Třída ShoppingCart poskytuje následující metody:</span><span class="sxs-lookup"><span data-stu-id="b4de9-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="b4de9-134">**AddToCart** přebírá jako parametr alba a přidá jej do košíku uživatele.</span><span class="sxs-lookup"><span data-stu-id="b4de9-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="b4de9-135">Protože tabulky košíku sleduje množství pro každý obrázek, obsahuje logiku pouze zvýšit množství, pokud uživatel už má seřazené jednu kopii alba a v případě potřeby vytvořte nový řádek.</span><span class="sxs-lookup"><span data-stu-id="b4de9-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="b4de9-136">**RemoveFromCart** přijímá ID alba a odstraní ji z nákupního košíku uživatele.</span><span class="sxs-lookup"><span data-stu-id="b4de9-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="b4de9-137">Pokud má uživatel, jenom jednu kopii alba v jejich košíku, odeberou se řádek.</span><span class="sxs-lookup"><span data-stu-id="b4de9-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="b4de9-138">**EmptyCart** odebere všechny položky z nákupního košíku uživatele.</span><span class="sxs-lookup"><span data-stu-id="b4de9-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="b4de9-139">**GetCartItems** načte seznam CartItems k zobrazení nebo zpracování.</span><span class="sxs-lookup"><span data-stu-id="b4de9-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="b4de9-140">**GetCount** načte celkový počet alb uživatele má své nákupní košík je prázdný.</span><span class="sxs-lookup"><span data-stu-id="b4de9-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="b4de9-141">**GetTotal** vypočítá celkové náklady na seznam všech položek v košíku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="b4de9-142">**CreateOrder** během fáze checkout převede nákupního košíku objednávku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="b4de9-143">**GetCart** je statická metoda, která umožňuje naše řadiče pro získání objektu košíku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="b4de9-144">Používá **GetCartId** metodu ke zpracování čtení CartId z uživatelské relace.</span><span class="sxs-lookup"><span data-stu-id="b4de9-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="b4de9-145">Metoda GetCartId vyžaduje hodnotu HttpContextBase tak, aby jej číst CartId uživatele z uživatelské relace.</span><span class="sxs-lookup"><span data-stu-id="b4de9-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="b4de9-146">Tady je úplný **ShoppingCart třídy**:</span><span class="sxs-lookup"><span data-stu-id="b4de9-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="b4de9-147">Modely ViewModels</span><span class="sxs-lookup"><span data-stu-id="b4de9-147">ViewModels</span></span>

<span data-ttu-id="b4de9-148">Naše řadič nákupního košíku se potřebují komunikovat některé komplexní informace pro jeho zobrazení, které není přímo namapován na naše objekty modelu.</span><span class="sxs-lookup"><span data-stu-id="b4de9-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="b4de9-149">Nechceme změnit náš modely tak, aby odpovídala naše zobrazení; Model třídy by měly představovat naše domény, nikoli uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b4de9-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="b4de9-150">Jedním řešením může být k předání informací o našich zobrazeními pomocí třídy položek ViewBag, jako jsme to udělali s informacemi o Store správce rozevírací seznam, ale předáním velké množství informací prostřednictvím objekt ViewBag získá náročná na správu.</span><span class="sxs-lookup"><span data-stu-id="b4de9-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="b4de9-151">Toto řešení je použít *ViewModel* vzor.</span><span class="sxs-lookup"><span data-stu-id="b4de9-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="b4de9-152">Při použití tohoto modelu se nám vytvořit třídy silného typu, které jsou optimalizované pro naše konkrétní zobrazení scénáře a které vystavit vlastnosti pro dynamické hodnoty/obsah potřebný pomocí našich šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b4de9-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="b4de9-153">Naše třídy kontroleru můžete naplnit a předat zobrazit šablonu pro použití těchto tříd optimalizovaných pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b4de9-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="b4de9-154">Díky tomu bezpečnost typů, kontrola za kompilace a editor technologie IntelliSense v rámci šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b4de9-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="b4de9-155">Vytvoříme dvě zobrazení modelů pro použití v kontroleru nákupního košíku: ShoppingCartViewModel bude obsahovat obsah nákupního košíku uživatele a ShoppingCartRemoveViewModel se použije k zobrazení informací potvrzení, když uživatel něco z jejich košíku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="b4de9-156">Pojďme vytvořit novou složku modely ViewModels v kořenové složce projektu z důvodu zjednodušení uspořádané.</span><span class="sxs-lookup"><span data-stu-id="b4de9-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="b4de9-157">Klikněte pravým tlačítkem na projekt, vyberte možnost Přidat / novou složku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="b4de9-158">Název složky modely ViewModel.</span><span class="sxs-lookup"><span data-stu-id="b4de9-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="b4de9-159">V dalším kroku přidejte třídu ShoppingCartViewModel ve složce modely ViewModel.</span><span class="sxs-lookup"><span data-stu-id="b4de9-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="b4de9-160">Má dvě vlastnosti: seznam položek v košíku a desítkovou hodnotu pro uložení celkovou cenu pro všechny položky v košíku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="b4de9-161">Nyní přidejte ShoppingCartRemoveViewModel ke složce modely ViewModels s následující čtyři vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b4de9-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="b4de9-162">Řadič nákupního košíku</span><span class="sxs-lookup"><span data-stu-id="b4de9-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="b4de9-163">Řadič nákupního košíku se třem hlavním účelům: Přidání položky do košíku, odebírání položek z košíku a zobrazení položek v košíku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="b4de9-164">Provede pomocí tří tříd jsme právě vytvořili: ShoppingCartViewModel ShoppingCartRemoveViewModel a ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="b4de9-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="b4de9-165">Stejně jako v StoreController a StoreManagerController přidáme pole pro uložení instance MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="b4de9-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="b4de9-166">Nový řadič nákupního košíku přidáte do projektu pomocí šablony prázdný kontroler.</span><span class="sxs-lookup"><span data-stu-id="b4de9-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="b4de9-167">Tady je úplný ShoppingCart Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b4de9-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="b4de9-168">Index a přidat kontroler akce velmi povědomé.</span><span class="sxs-lookup"><span data-stu-id="b4de9-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="b4de9-169">Akce kontroleru odebrat a CartSummary zpracování dva speciální případy, které probereme v následující části.</span><span class="sxs-lookup"><span data-stu-id="b4de9-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="b4de9-170">Aktualizace AJAX pomocí jQuery</span><span class="sxs-lookup"><span data-stu-id="b4de9-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="b4de9-171">Dále vytvoříme stránku nákupní košík Index, který je silně typováno do ShoppingCartViewModel a používá šablonu zobrazení seznamu stejným způsobem jako před.</span><span class="sxs-lookup"><span data-stu-id="b4de9-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="b4de9-172">Ale nemusíte používat Html.ActionLink k odebrání položek z košíku, použijeme jQuery "propojí" událost click pro všechny odkazy v tomto zobrazení, které mají třídy HTML při odebírání odkazu.</span><span class="sxs-lookup"><span data-stu-id="b4de9-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="b4de9-173">Místo odesílání formuláře, tuto obslužnou rutinu události kliknutí stačí provede zpětné volání AJAX naše RemoveFromCart akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b4de9-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="b4de9-174">RemoveFromCart vrátí výsledek serializován do formátu JSON, který pak analyzuje a provádí čtyři rychlé aktualizace stránky pomocí jQuery naše jQuery zpětného volání:</span><span class="sxs-lookup"><span data-stu-id="b4de9-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="b4de9-175">Odebere odstraněné alba ze seznamu</span><span class="sxs-lookup"><span data-stu-id="b4de9-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="b4de9-176">Aktualizuje počet košíku v hlavičce</span><span class="sxs-lookup"><span data-stu-id="b4de9-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="b4de9-177">Zobrazí uživateli zprávu aktualizace</span><span class="sxs-lookup"><span data-stu-id="b4de9-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="b4de9-178">Aktualizuje celková cena košíku</span><span class="sxs-lookup"><span data-stu-id="b4de9-178">Updates the cart total price</span></span>

<span data-ttu-id="b4de9-179">Protože odebrat scénář je zpracovávaných zpětného volání Ajax v rámci zobrazení indexu, nebudeme potřebovat další zobrazení RemoveFromCart akce.</span><span class="sxs-lookup"><span data-stu-id="b4de9-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="b4de9-180">Tady je kompletní kód pro zobrazení /ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="b4de9-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="b4de9-181">Pokud chcete otestovat to zjistit, potřebujeme mít možnost přidávat položky na naše řešení nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="b4de9-182">Aktualizujeme naše **Store podrobnosti** aby zobrazení zahrnovalo na tlačítko "Přidat do košíku".</span><span class="sxs-lookup"><span data-stu-id="b4de9-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="b4de9-183">Když jsme na to, můžeme zahrnout některé z alba Další informace, které jsme přidali protože jsme toto zobrazení poslední aktualizace: Genre, interpreta, ceny a alb.</span><span class="sxs-lookup"><span data-stu-id="b4de9-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="b4de9-184">Aktualizovaný kód Store podrobnosti zobrazení se zobrazí, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="b4de9-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="b4de9-185">Nyní jsme klikněte prostřednictvím obchodu a vyzkoušejte přidávání a odebírání alb do a z naše řešení nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="b4de9-186">Spusťte aplikaci a přejděte do indexu Store.</span><span class="sxs-lookup"><span data-stu-id="b4de9-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="b4de9-187">Pak klikněte na Genre, chcete-li zobrazit seznam alb.</span><span class="sxs-lookup"><span data-stu-id="b4de9-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="b4de9-188">Kliknutí na nadpisech alba teď se teď zobrazují naše aktualizované alba podrobnosti, včetně tlačítko "Přidat do košíku".</span><span class="sxs-lookup"><span data-stu-id="b4de9-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="b4de9-189">Kliknutím na tlačítko "Přidat do košíku" se teď zobrazují naše nákupního košíku Index s souhrnný seznam nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="b4de9-190">Po načtení do nákupního košíku, můžete kliknout na odebrat z nákupního košíku odkazu zobrazíte aktualizace Ajax do nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="b4de9-191">Jsme sestavili funkční nákupní košík, což umožňuje neregistrovaným uživatelů k přidávání položek do jejich košíku.</span><span class="sxs-lookup"><span data-stu-id="b4de9-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="b4de9-192">V následující části jsme vám zajistí, aby registraci a dokončete proces platby u pokladny.</span><span class="sxs-lookup"><span data-stu-id="b4de9-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="b4de9-193">[Předchozí](mvc-music-store-part-7.md)
> [další](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="b4de9-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
