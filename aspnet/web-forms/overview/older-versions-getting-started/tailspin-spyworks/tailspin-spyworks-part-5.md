---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: '5. část: Obchodní logiku | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 5. dílu přidá spustí nějakou obchodní logiku.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: fdb73c01d7b6076bc292c640376aa35cc5f52ea6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827045"
---
<a name="part-5-business-logic"></a><span data-ttu-id="e30cf-104">5. část: Obchodní logiky</span><span class="sxs-lookup"><span data-stu-id="e30cf-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="e30cf-105">podle [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e30cf-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e30cf-106">Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="e30cf-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e30cf-107">Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.</span><span class="sxs-lookup"><span data-stu-id="e30cf-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e30cf-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="e30cf-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e30cf-109">5. dílu přidá spustí nějakou obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="e30cf-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="e30cf-110">Přidání spustí nějakou obchodní logiku</span><span class="sxs-lookup"><span data-stu-id="e30cf-110">Adding Some Business Logic</span></span>

<span data-ttu-id="e30cf-111">Chceme, aby naše prostředí pro nákup bude k dispozici pokaždé, když uživatel navštíví naše webové stránky.</span><span class="sxs-lookup"><span data-stu-id="e30cf-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="e30cf-112">Uživatelé budou moci procházet a přidávat položky do nákupního košíku, i v případě, že nejsou zaregistrované nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="e30cf-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="e30cf-113">Jakmile jsou připravené k rezervaci budou mít možnost ověřovat a pokud nejsou ještě členové budou moct vytvořit účet.</span><span class="sxs-lookup"><span data-stu-id="e30cf-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="e30cf-114">To znamená, že budeme muset implementovat logiku do nákupního košíku převést anonymní stavu do stavu "Registrovaný uživatel".</span><span class="sxs-lookup"><span data-stu-id="e30cf-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="e30cf-115">Pojďme vytvořit adresář s názvem "Třídy" a klikněte pravým tlačítkem na složku a vytvořte nový soubor "Třída" MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="e30cf-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="e30cf-116">Jak už jsme zmínili jsme rozšíření třídy, která implementuje stránce MyShoppingCart.aspx a Uděláme to pomocí. Konstrukce výkonné "částečné třídy" vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="e30cf-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="e30cf-117">Vygenerovaný volání pro naše soubor MyShoppingCart.aspx.cf vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="e30cf-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="e30cf-118">Všimněte si použití klíčového slova "částečné".</span><span class="sxs-lookup"><span data-stu-id="e30cf-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="e30cf-119">Soubor třídy, který jsme právě vygenerovali vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="e30cf-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="e30cf-120">Můžeme tak, že přidáte partial – klíčové slovo k tomuto souboru a sloučí naše implementace.</span><span class="sxs-lookup"><span data-stu-id="e30cf-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="e30cf-121">Náš nový soubor třídy nyní vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="e30cf-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="e30cf-122">První metoda, která přidáme k naší třídy je metoda "AddItem".</span><span class="sxs-lookup"><span data-stu-id="e30cf-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="e30cf-123">Toto je metoda, která bude volána nakonec po kliknutí na "Přidat do obrázky" odkazy na stránkách seznam produktů a podrobnosti o produktu.</span><span class="sxs-lookup"><span data-stu-id="e30cf-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="e30cf-124">Připojte pomocí příkazů v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="e30cf-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="e30cf-125">A přidejte tuto metodu do třídy MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="e30cf-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="e30cf-126">Chcete-li zobrazit, pokud položka se již v košíku se používá technologii LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="e30cf-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="e30cf-127">Pokud ano, aktualizujeme množství objednávky položku, jinak se nám vytvořit nový záznam pro vybranou položku</span><span class="sxs-lookup"><span data-stu-id="e30cf-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="e30cf-128">Aby bylo možné tuto metodu volat Implementujeme AddToCart.aspx stránky, který nejen třídy tuto metodu, ale zobrazí aktuální nákupního košíku = po přidání položky.</span><span class="sxs-lookup"><span data-stu-id="e30cf-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="e30cf-129">Klikněte pravým tlačítkem na název řešení v Průzkumníku řešení a přidejte a novou stránku s názvem AddToCart.aspx, protože jsme udělali dříve.</span><span class="sxs-lookup"><span data-stu-id="e30cf-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="e30cf-130">Když jsme může pomocí této stránky lze zobrazit dočasné výsledky, jako jsou problémy s nízkou uložených atd., v naší implementaci, na stránce nejsou ve skutečnosti vykreslit, ale spíše zavolá logiky "Add" a přesměrování.</span><span class="sxs-lookup"><span data-stu-id="e30cf-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="e30cf-131">K tomu přidáme následující kód na stránku\_událost Load.</span><span class="sxs-lookup"><span data-stu-id="e30cf-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="e30cf-132">Mějte na paměti, že jsme načítají produktu pro přidání do nákupního košíku z parametru řetězce dotazu a volání metody AddItem naší třídy.</span><span class="sxs-lookup"><span data-stu-id="e30cf-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="e30cf-133">Pokud nedojde k chybě nedojde k řízení je předáno SHoppingCart.aspx stránky, které jsme plně implementuje další.</span><span class="sxs-lookup"><span data-stu-id="e30cf-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="e30cf-134">Pokud má být chybu jsme vyvolat výjimku.</span><span class="sxs-lookup"><span data-stu-id="e30cf-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="e30cf-135">Právě jsme dosud nebyla implementována globální obslužná rutina chyb, tato výjimka přejde neošetřené v naší aplikaci, ale jsme se to za chvíli napravit.</span><span class="sxs-lookup"><span data-stu-id="e30cf-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="e30cf-136">Všimněte si také použití příkazu Debug.Fail() (k dispozici prostřednictvím `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="e30cf-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="e30cf-137">Je aplikace běží v ladicím programu, tato metoda zobrazí podrobné dialogové okno s informacemi o stavu aplikace spolu s chybovou zprávu, které určíme.</span><span class="sxs-lookup"><span data-stu-id="e30cf-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="e30cf-138">Při spouštění v produkčním prostředí Debug.Fail() příkaz je ignorován.</span><span class="sxs-lookup"><span data-stu-id="e30cf-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="e30cf-139">Všimněte si v kódu nad volání metody v našich nákupního košíku – názvy tříd "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="e30cf-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="e30cf-140">Přidejte kód pro implementaci metody následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e30cf-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="e30cf-141">Všimněte si, že jsme taky přidali aktualizace a ověření tlačítka a popisek, kde můžeme Zobrazit košík "celkem".</span><span class="sxs-lookup"><span data-stu-id="e30cf-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="e30cf-142">Jsme teď můžete přidat položky na naše řešení nákupního košíku, ale jsme neimplementovali logiku pro zobrazení košíku po produktu se přidala.</span><span class="sxs-lookup"><span data-stu-id="e30cf-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="e30cf-143">Tak na stránce MyShoppingCart.aspx přidáme ovládací prvek EntityDataSource a ovládací prvek GridVire následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e30cf-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="e30cf-144">Volání nahoru na formulář v návrháři, mohli dvakrát klikněte na tlačítko Aktualizovat košík a generování obslužné rutiny události kliknutí, který je zadaný v deklaraci v kódu.</span><span class="sxs-lookup"><span data-stu-id="e30cf-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="e30cf-145">Podrobnosti nám budete implementovat později, ale to se nám sestavte a spusťte naši aplikaci bez chyb.</span><span class="sxs-lookup"><span data-stu-id="e30cf-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="e30cf-146">Při spuštění aplikace a přidání položky do nákupního košíku se zobrazí toto.</span><span class="sxs-lookup"><span data-stu-id="e30cf-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="e30cf-147">Všimněte si, že jsme odchylují od zobrazení mřížky "Výchozí" implementací tři vlastních sloupců.</span><span class="sxs-lookup"><span data-stu-id="e30cf-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="e30cf-148">První je upravit "Vázané" pole pro množství:</span><span class="sxs-lookup"><span data-stu-id="e30cf-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="e30cf-149">Další je "počítané" sloupec, který zobrazuje celkový počet řádků položky (náklady položky krát množství povolujeme):</span><span class="sxs-lookup"><span data-stu-id="e30cf-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="e30cf-150">A konečně budeme mít vlastní sloupec, který obsahuje ovládací prvek CheckBox, který bude uživatel používat k označení, že by položka odebrána z nákupního grafu.</span><span class="sxs-lookup"><span data-stu-id="e30cf-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="e30cf-151">Jak je vidět, pořadí celkový řádek je prázdný Pojďme přidat nějaké logiky k výpočtu celkového pořadí.</span><span class="sxs-lookup"><span data-stu-id="e30cf-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="e30cf-152">Do naší třídy MyShoppingCart jsme nejprve budete implementovat metodu "GetTotal".</span><span class="sxs-lookup"><span data-stu-id="e30cf-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="e30cf-153">V souboru MyShoppingCart.cs přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="e30cf-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="e30cf-154">Pak na stránce\_můžete zavoláme vám metodě GetTotal zatížení obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="e30cf-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="e30cf-155">Ve stejnou dobu přidáme test jestli nákupní košík je prázdný a pokud je odpovídajícím způsobem upravit zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e30cf-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="e30cf-156">Teď pokud nákupní košík je prázdný získáme toto:</span><span class="sxs-lookup"><span data-stu-id="e30cf-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="e30cf-157">A pokud ne, uvidíme náš celkem.</span><span class="sxs-lookup"><span data-stu-id="e30cf-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="e30cf-158">Nicméně tato stránka ještě nebyla dokončena.</span><span class="sxs-lookup"><span data-stu-id="e30cf-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="e30cf-159">Budeme potřebovat další logiku k přepočítání nákupního košíku, tak, že odeberete položky označené k odstranění a tak, že určíte nové hodnoty pro množství, protože některé mohou se změnily v mřížce uživatelem.</span><span class="sxs-lookup"><span data-stu-id="e30cf-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="e30cf-160">Umožňuje přidat metodu "RemoveItem" do našich nákupního košíku třídy v MyShoppingCart.cs pro zpracování případu, když uživatel označí položku pro odebrání.</span><span class="sxs-lookup"><span data-stu-id="e30cf-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="e30cf-161">Nyní Řekněme ad metodu ke zpracování situaci, když uživatel změní jednoduše kvality povolujeme v prvku GridView.</span><span class="sxs-lookup"><span data-stu-id="e30cf-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="e30cf-162">Pomocí základních funkcí odebrat a aktualizace na místě můžeme implementovat do logiky, která ve skutečnosti aktualizace nákupního košíku v databázi.</span><span class="sxs-lookup"><span data-stu-id="e30cf-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="e30cf-163">(V MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="e30cf-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="e30cf-164">Všimněte si, že tato metoda očekává dva parametry.</span><span class="sxs-lookup"><span data-stu-id="e30cf-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="e30cf-165">Jeden je nákupní košík Id a druhý je pole objektů typu definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="e30cf-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="e30cf-166">Aby se minimalizovalo závislost naše logiku specifika uživatelské rozhraní, jsme definovali strukturu dat, která můžeme použít k předání nákupního košíku položky do našeho kódu bez metodě vyžadující přímý přístup k ovládacím prvku GridView.</span><span class="sxs-lookup"><span data-stu-id="e30cf-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="e30cf-167">V našem MyShoppingCart.aspx.cs souboru můžete použít tuto strukturu v našich obslužná rutina události klikněte na tlačítko aktualizace následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e30cf-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="e30cf-168">Všimněte si, že kromě aktualizací košíku aktualizujeme celý košíku.</span><span class="sxs-lookup"><span data-stu-id="e30cf-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="e30cf-169">Mějte na paměti se zajímají hlavně o tento řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="e30cf-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="e30cf-170">GetValues() je speciální pomocná funkce, které jsme se implementovat v MyShoppingCart.aspx.cs následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e30cf-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="e30cf-171">To poskytuje čistý způsob, jak přistupovat k hodnotám vázaných prvků v našich prvku GridView.</span><span class="sxs-lookup"><span data-stu-id="e30cf-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="e30cf-172">Vzhledem k tomu, že naše ovládací prvek zaškrtávací políčko "Odebrat položku" není vázán jsme budete k němu přístup prostřednictvím metody FindControl().</span><span class="sxs-lookup"><span data-stu-id="e30cf-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="e30cf-173">V této fázi ve vývoji projektu již brzy implementovat proces platby u pokladny.</span><span class="sxs-lookup"><span data-stu-id="e30cf-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="e30cf-174">Před tím můžeme pomocí sady Visual Studio vygenerovala databáze členství a přidejte uživatele do úložiště členství.</span><span class="sxs-lookup"><span data-stu-id="e30cf-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e30cf-175">[Předchozí](tailspin-spyworks-part-4.md)
> [další](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="e30cf-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
