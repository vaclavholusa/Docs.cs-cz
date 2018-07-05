---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: '9. část: Registrace a pokladna | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. Část 9 pokrývá registrace a pokladna.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 935729be0dce790c6fce2e2e982ee063318d64dd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367003"
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="ff0bf-104">9. část: Registrace a pokladna</span><span class="sxs-lookup"><span data-stu-id="ff0bf-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="ff0bf-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="ff0bf-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="ff0bf-106">MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="ff0bf-107">Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="ff0bf-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="ff0bf-109">Část 9 pokrývá registrace a pokladna.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="ff0bf-110">V této části budeme vytvářet CheckoutController, která bude shromažďovat adresu zákazník a platební údaje.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="ff0bf-111">Začneme vyžadovat uživatelům registrovat před rezervace, takže tento řadič bude vyžadovat ověření.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="ff0bf-112">Uživatelé přejdete na proces platby u pokladny z jejich nákupního košíku klepnutím na tlačítko "Rezervace".</span><span class="sxs-lookup"><span data-stu-id="ff0bf-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="ff0bf-113">Pokud uživatel není přihlášen, zobrazí se výzva k.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="ff0bf-114">Po úspěšném přihlášení uživatele se zobrazí zobrazení adres a platba.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="ff0bf-115">Jakmile máte formuláři vyplněný a odesláním objednávky, se bude zobrazovat na potvrzovací obrazovce objednávky.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="ff0bf-116">Při pokusu o zobrazení pořadí neexistující nebo pořadí, které nepatří uživateli se zobrazí zobrazení chyb.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="ff0bf-117">Migrace nákupního košíku</span><span class="sxs-lookup"><span data-stu-id="ff0bf-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="ff0bf-118">Během procesu nákupu je anonymní, když uživatel klikne na tlačítko rezervovat, je třeba k registraci a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="ff0bf-119">Uživatelé se očekávají, že budeme udržovat své nákupní košík informací mezi návštěvy, takže bude potřeba po dokončení registrace nebo přihlášení nákupního košíku informace přidruží k uživateli.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="ff0bf-120">Jde ve skutečnosti velmi jednoduchý postup, jak naše ShoppingCart třída již má metodu, která se přidruží ke všech položek v košíku aktuální uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="ff0bf-121">Právě jsme potřeba volat tuto metodu, když uživatel dokončí registraci nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="ff0bf-122">Otevřít **AccountController** třídu, která jsme přidali jsme byly nastavení členství a ověřování.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="ff0bf-123">Přidat pak pomocí příkazu odkazující na MvcMusicStore.Models, přidejte následující metodu MigrateShoppingCart:</span><span class="sxs-lookup"><span data-stu-id="ff0bf-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="ff0bf-124">Dále upravte akci po přihlášení k volání MigrateShoppingCart po ověření uživatele, jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="ff0bf-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="ff0bf-125">Proveďte stejnou změnu do registru odeslat akci, ihned po úspěšném vytvoření účtu uživatele:</span><span class="sxs-lookup"><span data-stu-id="ff0bf-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="ff0bf-126">Je to – teď je anonymní nákupního košíku se automaticky přesunou do uživatelského účtu po úspěšné registraci nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="ff0bf-127">Vytváří CheckoutController</span><span class="sxs-lookup"><span data-stu-id="ff0bf-127">Creating the CheckoutController</span></span>

<span data-ttu-id="ff0bf-128">Klikněte pravým tlačítkem na složku řadiče a přidejte nový kontroler do projektu s názvem CheckoutController pomocí šablony prázdný kontroler.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="ff0bf-129">Nejprve přidejte atribut Authorize nad deklaraci třídy Kontroleru budou muset uživatelé zaregistrovat rezervaci:</span><span class="sxs-lookup"><span data-stu-id="ff0bf-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="ff0bf-130">*Poznámka: Toto je podobný změnu, kterou jsme dříve provedené StoreManagerController, ale v takovém případě vyžadován atribut ověřit, že se uživatel v roli správce. V Kontroleru Checkout jsme už by být přihlášeni uživatele ale nejsou by se správci.*</span><span class="sxs-lookup"><span data-stu-id="ff0bf-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="ff0bf-131">Z důvodu zjednodušení společnost Microsoft nebude pracující s platební informace v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="ff0bf-132">Místo toho jsme se povolení uživatele, aby pomocí propagační kód.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="ff0bf-133">Jsme tento propagační kód pomocí konstanta s názvem PromoCode uloží.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="ff0bf-134">Stejně jako v StoreController jsme budete deklarovat pole pro uložení instance třídy MusicStoreEntities s názvem storeDB.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="ff0bf-135">Aby bylo možné použít třídy MusicStoreEntities, budeme muset přidat, pomocí příkazu pro obor názvů MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="ff0bf-136">Horní Checkout kontroleru se zobrazí pod.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="ff0bf-137">CheckoutController bude mít následující akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="ff0bf-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="ff0bf-138">**AddressAndPayment (metodu GET)** zobrazí formulář umožňuje uživateli zadat informace o jejich.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="ff0bf-139">**AddressAndPayment (metodu POST)** ověření vstupu, který se zpracování objednávky.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="ff0bf-140">**Kompletní** zobrazí poté, co uživatel úspěšně dokončil proces platby u pokladny.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="ff0bf-141">Toto zobrazení bude obsahovat uživatele pořadové číslo, s potvrzením.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="ff0bf-142">Nejprve do AddressAndPayment přejmenujme akce kontroleru indexu (který se vygeneroval při jsme vytvořili kontroleru).</span><span class="sxs-lookup"><span data-stu-id="ff0bf-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="ff0bf-143">Tato akce kontroleru zobrazí pouze formulář checkout, nevyžaduje žádné informace o modelu.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="ff0bf-144">Naše metodu AddressAndPayment POST bude postupují stejným způsobem jsme použili v StoreManagerController: pokusí přijmout odeslání formuláře a dokončení objednávky a znovu zobrazte formulář, pokud se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="ff0bf-145">Po ověření vstupu formuláře nesplňuje naše požadavky na ověření pro objednávky, jsme se přímo zkontrolujte hodnotu PromoCode formuláře.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="ff0bf-146">Za předpokladu, že je všechno správně, že jsme se uloží aktualizované informace s pořadím, řekněte ShoppingCart objektu k dokončení procesu objednávání a přesměrování na dokončení akce.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="ff0bf-147">Po úspěšném dokončení procesu registrace budete přesměrováni na akce kontroleru kompletní uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="ff0bf-148">Tato akce provede jednoduchou kontrolu pro ověření, že pořadí ve skutečnosti patří do přihlášeného uživatele před zobrazením pořadové číslo jako potvrzení.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="ff0bf-149">*Poznámka: Zobrazení chyb byl automaticky vytvořen pro nás ve složce /Views/Shared když jsme začali s projektu.*</span><span class="sxs-lookup"><span data-stu-id="ff0bf-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="ff0bf-150">Kompletní kód CheckoutController vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="ff0bf-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="ff0bf-151">Přidání zobrazení AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="ff0bf-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="ff0bf-152">Teď vytvoříme AddressAndPayment zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="ff0bf-153">Klikněte pravým tlačítkem na jednu z akcí kontroleru AddressAndPayment a přidat zobrazení s názvem AddressAndPayment, což je silně typováno jako pořadí a používá šablonu upravit, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="ff0bf-154">Toto zobrazení způsobí, že použití dvou technik zvažovali jsme i při vytváření zobrazení StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="ff0bf-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="ff0bf-155">Použijeme Html.EditorForModel() k zobrazení polí formuláře pro model pořadí</span><span class="sxs-lookup"><span data-stu-id="ff0bf-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="ff0bf-156">Společnost Microsoft bude využívat ověřovacích pravidel pomocí atributů ověření třídu pořadí</span><span class="sxs-lookup"><span data-stu-id="ff0bf-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="ff0bf-157">Začneme aktualizací kód formuláře, který použije Html.EditorForModel(), za nímž následuje další textové pole pro propagační kód.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="ff0bf-158">Kompletní kód AddressAndPayment zobrazení je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="ff0bf-159">Definování pravidel ověřování pro pořadí</span><span class="sxs-lookup"><span data-stu-id="ff0bf-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="ff0bf-160">Teď, když náš zobrazení je nastaven, nastavíme ověřovacích pravidel pro náš model pořadí jako jsme to udělali dříve alba modelu.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="ff0bf-161">Klikněte pravým tlačítkem na složku modely a přidejte třídu pojmenovanou pořadí.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="ff0bf-162">Kromě ověřování atributů, které jsme použili dříve alba také použijeme regulární výraz k ověření e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="ff0bf-163">Pokus o odeslání formuláře s chybějící nebo neplatné informace se nyní zobrazí chybovou zprávu pomocí ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="ff0bf-164">Dobře jsme provedli většinu těžkou práci pro proces platby u pokladny; stačilo několik číslům umocněným na druhou a končí na dokončení.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="ff0bf-165">Je potřeba přidat dvě jednoduché zobrazení a musíme starat o předání košíku informace během procesu přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="ff0bf-166">Přidání Checkout úplné zobrazení</span><span class="sxs-lookup"><span data-stu-id="ff0bf-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="ff0bf-167">Rezervace kompletní přehled je docela jednoduché, protože potřebuje pouze k zobrazení ID objednávky.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="ff0bf-168">Klikněte pravým tlačítkem na akce kontroleru kompletní a přidat zobrazení s názvem dokončeno, což je silně typováno jako celé číslo</span><span class="sxs-lookup"><span data-stu-id="ff0bf-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="ff0bf-169">Nyní aktualizujeme zobrazit kód pro zobrazení ID objednávky, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="ff0bf-170">Aktualizace zobrazení The chyb</span><span class="sxs-lookup"><span data-stu-id="ff0bf-170">Updating The Error view</span></span>

<span data-ttu-id="ff0bf-171">Výchozí šablona zahrnuje zobrazení chyb ve sdílené složce zobrazení tak, aby se dá znovu použít jinde v lokalitě.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="ff0bf-172">Toto zobrazení chyba obsahuje chybu velmi jednoduchý a nepoužívá náš web rozložení, abychom vám ho aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="ff0bf-173">Protože toto je stránka obecné chybě, je velmi jednoduchý obsah.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="ff0bf-174">Budete zahrnujeme zprávu a odkaz přejděte na předchozí stránku v historii, pokud chce uživatel znovu opakujte akci.</span><span class="sxs-lookup"><span data-stu-id="ff0bf-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="ff0bf-175">[Předchozí](mvc-music-store-part-8.md)
> [další](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="ff0bf-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
