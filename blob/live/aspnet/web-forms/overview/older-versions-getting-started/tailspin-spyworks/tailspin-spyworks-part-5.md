---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: "Část 5: Obchodní logiky | Microsoft Docs"
author: JoeStagner
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 5 přidá některé obchodní logiku."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e205788e05a2ad94d86d4847c11c40898b1c3113
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-5-business-logic"></a><span data-ttu-id="a0c72-104">Část 5: Obchodní logiky</span><span class="sxs-lookup"><span data-stu-id="a0c72-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="a0c72-105">podle [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="a0c72-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="a0c72-106">Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="a0c72-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="a0c72-107">Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.</span><span class="sxs-lookup"><span data-stu-id="a0c72-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="a0c72-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="a0c72-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="a0c72-109">Část 5 přidá některé obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="a0c72-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a><span data-ttu-id="a0c72-110">Přidání některé obchodní logiky</span><span class="sxs-lookup"><span data-stu-id="a0c72-110">Adding Some Business Logic</span></span>

<span data-ttu-id="a0c72-111">Chceme našich zkušeností nákupní být k dispozici vždy, když někdo navštíví našeho webu.</span><span class="sxs-lookup"><span data-stu-id="a0c72-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="a0c72-112">Návštěvníků bude možné procházet a přidání položek do nákupního košíku, i když nejsou registrované nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="a0c72-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="a0c72-113">Jakmile jsou připravené k rezervaci budou mít možnost ověřit a pokud nejsou ještě členy bude mít k vytvoření účtu.</span><span class="sxs-lookup"><span data-stu-id="a0c72-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="a0c72-114">To znamená, že budeme muset implementovat logiku převést nákupní košík ze anonymní stavu do stavu "Registrovaný uživatel".</span><span class="sxs-lookup"><span data-stu-id="a0c72-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="a0c72-115">Umožňuje vytvořit adresář s názvem "Třídy" pak klikněte pravým tlačítkem na složku a vytvořte nový soubor "Třída" s názvem MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="a0c72-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="a0c72-116">Jak už jsme zmínili jsme rozšíření třídu, která implementuje stránce MyShoppingCart.aspx a Uděláme to pomocí. Vytvořit NET je výkonný "třídu".</span><span class="sxs-lookup"><span data-stu-id="a0c72-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="a0c72-117">Vygenerovaný volání pro naše MyShoppingCart.aspx.cf souborů vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="a0c72-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="a0c72-118">Všimněte si použití klíčového slova "částečné".</span><span class="sxs-lookup"><span data-stu-id="a0c72-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="a0c72-119">Třída soubor, který jsme právě vygeneroval vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="a0c72-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="a0c72-120">Sloučíme bude naše implementace přidáním partial – klíčové slovo také tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="a0c72-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="a0c72-121">Naše nový soubor třídy nyní vypadat třeba takto.</span><span class="sxs-lookup"><span data-stu-id="a0c72-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="a0c72-122">První metodu, která přidáme do našich třídy je metoda "AddItem".</span><span class="sxs-lookup"><span data-stu-id="a0c72-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="a0c72-123">Toto je metoda, která bude volána nakonec i když uživatel klikne na "Přidat do obrázky" odkazy na stránkách seznam produktů a podrobnosti o produktu.</span><span class="sxs-lookup"><span data-stu-id="a0c72-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="a0c72-124">Připojit pomocí následujících příkazů v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="a0c72-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="a0c72-125">A přidejte tuto metodu do třídy MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="a0c72-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="a0c72-126">Pokud položka je již v košíku se používá technologie LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="a0c72-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="a0c72-127">Pokud ano, můžeme aktualizovat pořadí množství položky, jinak jsme vytvořit nový záznam pro vybranou položku.</span><span class="sxs-lookup"><span data-stu-id="a0c72-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="a0c72-128">Aby bylo možné tuto metodu volat bude implementaci stránku AddToCart.aspx, která je jenom třídy tuto metodu, ale pak se zobrazují aktuální nákupního košíku = po přidání položky.</span><span class="sxs-lookup"><span data-stu-id="a0c72-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="a0c72-129">Klikněte pravým tlačítkem na název řešení v Průzkumníku řešení a přidejte a novou stránku s názvem AddToCart.aspx, jak je uvedeno v dříve.</span><span class="sxs-lookup"><span data-stu-id="a0c72-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="a0c72-130">Při dočasné výsledky jako nízkou uložených problémy atd., se na naše implementace jsme může pomocí této stránky, stránky se ve skutečnosti není vykreslení, ale místo volání logice "Přidat" a přesměruje.</span><span class="sxs-lookup"><span data-stu-id="a0c72-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="a0c72-131">K tomu přidáme následující kód na stránku\_zatížení událostí.</span><span class="sxs-lookup"><span data-stu-id="a0c72-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="a0c72-132">Všimněte si, načítají produktu pro přidání do nákupního košíku ze parametr řetězce dotazu a volání metody AddItem naše třídy.</span><span class="sxs-lookup"><span data-stu-id="a0c72-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="a0c72-133">Za předpokladu, že žádné chyby došlo k ovládací prvek předává na stránku SHoppingCart.aspx, který bude plně implementaci Další.</span><span class="sxs-lookup"><span data-stu-id="a0c72-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="a0c72-134">Pokud má být chybu jsme vyvolat výjimku.</span><span class="sxs-lookup"><span data-stu-id="a0c72-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="a0c72-135">Aktuálně jsme nebyly dosud implementován popisovač globální chyb, by se dostala neošetřené v naší aplikaci výjimku, ale jsme se to za chvíli napravit.</span><span class="sxs-lookup"><span data-stu-id="a0c72-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="a0c72-136">Všimněte si také použití příkazu Debug.Fail() (dostupné prostřednictvím`using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="a0c72-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="a0c72-137">Je je aplikace spuštěna v rámci ladicího programu, tato metoda zobrazí podrobné dialogové okno s informacemi o stavu aplikace spolu s chybovou zprávu, která určíme.</span><span class="sxs-lookup"><span data-stu-id="a0c72-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="a0c72-138">Při spuštění v produkčním prostředí Debug.Fail() příkaz je ignorováno.</span><span class="sxs-lookup"><span data-stu-id="a0c72-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="a0c72-139">Všimněte si v kódu výše volání metody v našem nákupní košík – názvy tříd "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="a0c72-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="a0c72-140">Přidáte kód pro implementaci metodu následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="a0c72-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="a0c72-141">Všimněte si, že také přidali jsme aktualizace a najdete v článku věnovaném tlačítka a kde jsme můžete zobrazit košík "celkem" štítek.</span><span class="sxs-lookup"><span data-stu-id="a0c72-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="a0c72-142">Jsme teď můžete přidat položky do naše řešení nákupního košíku ale jsme ještě implementována logiku zobrazíte košíku po přidání produktu.</span><span class="sxs-lookup"><span data-stu-id="a0c72-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="a0c72-143">Ano na stránce MyShoppingCart.aspx přidáme GridVire ovládací prvek EntityDataSource a následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="a0c72-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="a0c72-144">Volání do formuláře v návrháři, aby mohli dvakrát klikněte na tlačítko Aktualizovat košíku a generovat obslužnou rutinu události kliknutí, který je zadaný v deklaraci do kódu.</span><span class="sxs-lookup"><span data-stu-id="a0c72-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="a0c72-145">Podrobnosti budete implementaci později, ale tato funkce bude dejte nám sestavení a spuštění aplikace bez chyb.</span><span class="sxs-lookup"><span data-stu-id="a0c72-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="a0c72-146">Při spuštění aplikace a přidejte položku do nákupního košíku uvidíte to.</span><span class="sxs-lookup"><span data-stu-id="a0c72-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="a0c72-147">Všimněte si, že jsme odchylují od zobrazení mřížky "Výchozí" implementací tři vlastní sloupce.</span><span class="sxs-lookup"><span data-stu-id="a0c72-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="a0c72-148">První je upravit, "Vázaná" pole pro množství:</span><span class="sxs-lookup"><span data-stu-id="a0c72-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="a0c72-149">Další je "počítané" sloupec, který zobrazuje celkový řádku položky (náklady položka časy množství, které má být pořadí):</span><span class="sxs-lookup"><span data-stu-id="a0c72-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="a0c72-150">Nakonec máme vlastní sloupec, který obsahuje ovládací prvek CheckBox, který bude uživatel používat k označení, že položka má být odebrána z nákupní grafu.</span><span class="sxs-lookup"><span data-stu-id="a0c72-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="a0c72-151">Jak vidíte, přidejte pořadí celkový řádek je prázdný, můžeme některé logiku vypočítat celkové pořadí.</span><span class="sxs-lookup"><span data-stu-id="a0c72-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="a0c72-152">Pro naše třída MyShoppingCart jsme nejprve budete implementovat metodu "GetTotal".</span><span class="sxs-lookup"><span data-stu-id="a0c72-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="a0c72-153">V souboru MyShoppingCart.cs přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="a0c72-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="a0c72-154">Pak na stránce\_obslužné rutiny události zatížení může zavoláme vám naše GetTotal metoda.</span><span class="sxs-lookup"><span data-stu-id="a0c72-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="a0c72-155">Ve stejnou dobu přidáme testu chcete zobrazit, pokud je prázdný nákupní košík a odpovídajícím způsobem upravit zobrazení, pokud se jedná.</span><span class="sxs-lookup"><span data-stu-id="a0c72-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="a0c72-156">Teď Pokud prázdný nákupní košík jsme se toto:</span><span class="sxs-lookup"><span data-stu-id="a0c72-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="a0c72-157">A pokud ne, vidíme naše celkem.</span><span class="sxs-lookup"><span data-stu-id="a0c72-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="a0c72-158">Však tato stránka se ještě nedokončilo.</span><span class="sxs-lookup"><span data-stu-id="a0c72-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="a0c72-159">Potřebujeme další logiku přepočítat nákupní košík odstraněním položky označené k odstranění a tak, že určíte nové hodnoty pro množství, protože některé může být změněna v mřížce uživatelem.</span><span class="sxs-lookup"><span data-stu-id="a0c72-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="a0c72-160">Umožňuje přidat metodu "RemoveItem –" naše nákupní košík třídy v MyShoppingCart.cs zpracování případě, pokud uživatel označí položku pro odebrání.</span><span class="sxs-lookup"><span data-stu-id="a0c72-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="a0c72-161">Nyní Pojďme ad metodu ke zpracování za podmínek, když uživatel změní jednoduše kvality chcete objednat v GridView.</span><span class="sxs-lookup"><span data-stu-id="a0c72-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="a0c72-162">Se základními funkcemi odebrat a aktualizace na místě můžeme implementovat logiku, která ve skutečnosti aktualizuje nákupní košík v databázi.</span><span class="sxs-lookup"><span data-stu-id="a0c72-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="a0c72-163">(V MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="a0c72-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="a0c72-164">Budete Všimněte si, že tato metoda očekává dva parametry.</span><span class="sxs-lookup"><span data-stu-id="a0c72-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="a0c72-165">Jedna je nákupní košík Id a druhá je pole objektů uživatelem definovaný typ.</span><span class="sxs-lookup"><span data-stu-id="a0c72-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="a0c72-166">Aby pro minimalizaci závislost na uživatelské rozhraní specifika logiku, jsme jste definovali datová struktura, která jsme slouží k předávání nákupní košík položky na našem kódu bez naše metoda, které vyžadují přímý přístup k ovládacím prvku GridView.</span><span class="sxs-lookup"><span data-stu-id="a0c72-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="a0c72-167">V našem MyShoppingCart.aspx.cs souboru můžete použít tuto strukturu v našich obslužné rutiny události klikněte na tlačítko Aktualizovat následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="a0c72-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="a0c72-168">Všimněte si, že kromě aktualizace košíku budeme aktualizovat také celkové košíku.</span><span class="sxs-lookup"><span data-stu-id="a0c72-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="a0c72-169">Mějte na paměti, zvláštní zájem tento řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="a0c72-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="a0c72-170">GetValues() je speciální pomocné funkce, která bude v MyShoppingCart.aspx.cs implementaci následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="a0c72-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="a0c72-171">To umožňuje vyčištění pro přístup k hodnotám vázaných prvků v ovládacím prvku naše GridView.</span><span class="sxs-lookup"><span data-stu-id="a0c72-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="a0c72-172">Vzhledem k tomu, že naše "Odebrat položka" CheckBox – ovládací prvek není vázán jsme budete k němu přístup prostřednictvím metody FindControl().</span><span class="sxs-lookup"><span data-stu-id="a0c72-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="a0c72-173">V této fázi v projektu na vývoj jsme jsou Příprava implementovat proces checkout.</span><span class="sxs-lookup"><span data-stu-id="a0c72-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="a0c72-174">Než tak umožňuje generovat databázi členství a přidejte uživatele do úložiště členství pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a0c72-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a0c72-175">[Předchozí](tailspin-spyworks-part-4.md)
[další](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="a0c72-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
