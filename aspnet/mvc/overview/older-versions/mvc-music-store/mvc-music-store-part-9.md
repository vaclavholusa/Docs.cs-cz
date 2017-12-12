---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: "Část 9: Registrace a ověření | Microsoft Docs"
author: jongalloway
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 9 popisuje registraci a ověření."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 799985f889b1063c53df7bce365ae3989809ba79
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="52ed5-104">Část 9: Registrace a ověření</span><span class="sxs-lookup"><span data-stu-id="52ed5-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="52ed5-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="52ed5-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="52ed5-106">Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.</span><span class="sxs-lookup"><span data-stu-id="52ed5-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="52ed5-107">Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="52ed5-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="52ed5-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="52ed5-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="52ed5-109">Část 9 popisuje registraci a ověření.</span><span class="sxs-lookup"><span data-stu-id="52ed5-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="52ed5-110">V této části se budeme vytvářet CheckoutController, která bude shromažďovat kupujícímu adresy a platebních údajů.</span><span class="sxs-lookup"><span data-stu-id="52ed5-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="52ed5-111">Jsme bude od uživatelů zaregistrujte se na náš web před odhlašuje, aby tento řadič bude vyžadovat autorizace vyžadovat.</span><span class="sxs-lookup"><span data-stu-id="52ed5-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="52ed5-112">Uživatelé budou přejděte do procesu najdete v článku věnovaném z jejich nákupní košík kliknutím na tlačítko "Ověření".</span><span class="sxs-lookup"><span data-stu-id="52ed5-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="52ed5-113">Pokud uživatel není přihlášen, se zobrazí výzva k.</span><span class="sxs-lookup"><span data-stu-id="52ed5-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="52ed5-114">Po úspěšném přihlášení uživatele jsou pak uvedeny adresy a platebních zobrazení.</span><span class="sxs-lookup"><span data-stu-id="52ed5-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="52ed5-115">Jakmile budou mít vyplněno formuláře a odeslána pořadí, budou se zobrazovat na potvrzovací obrazovce a pořadí.</span><span class="sxs-lookup"><span data-stu-id="52ed5-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="52ed5-116">Při pokusu o zobrazení pořadí neexistující nebo pořadí, které nepatří do vám zobrazí zobrazení chyb.</span><span class="sxs-lookup"><span data-stu-id="52ed5-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="52ed5-117">Migrace nákupní košík</span><span class="sxs-lookup"><span data-stu-id="52ed5-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="52ed5-118">Nákupní proces je anonymní, když uživatel klikne na tlačítko Checkout, bude nutné registrace a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="52ed5-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="52ed5-119">Uživatelé budou předpokládají, že uchováváme jejich nákupní košík informací mezi návštěvy, takže je potřeba přidružit nákupní košík informace uživatele, když po dokončení registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="52ed5-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="52ed5-120">Jedná se ve skutečnosti velmi jednoduché k účelu, protože naše třída ShoppingCart již má metodu, která budou všechny položky v košíku aktuální přidružit k uživatelskému jménu.</span><span class="sxs-lookup"><span data-stu-id="52ed5-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="52ed5-121">Právě jsme potřeba volat tuto metodu, když uživatel dokončení registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="52ed5-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="52ed5-122">Otevřete **AccountController** třídu, která jsme přidali, když jsme se nastavení členství a autorizace.</span><span class="sxs-lookup"><span data-stu-id="52ed5-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="52ed5-123">Přidat pak pomocí příkazu odkazující na MvcMusicStore.Models, přidejte následující metodu MigrateShoppingCart:</span><span class="sxs-lookup"><span data-stu-id="52ed5-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="52ed5-124">Potom upravte akce post přihlášení k volání MigrateShoppingCart po ověření uživatele, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="52ed5-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="52ed5-125">Ujistěte se, stejné změnu registru post akce ihned po úspěšném vytvoření účtu uživatele:</span><span class="sxs-lookup"><span data-stu-id="52ed5-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="52ed5-126">Je to – nyní se anonymní nákupní košík automaticky převede na uživatelský účet po úspěšné registraci nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="52ed5-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="52ed5-127">Vytváření CheckoutController</span><span class="sxs-lookup"><span data-stu-id="52ed5-127">Creating the CheckoutController</span></span>

<span data-ttu-id="52ed5-128">Klikněte pravým tlačítkem na složku řadiče a přidejte nový řadič do projektu s názvem CheckoutController pomocí šablony prázdný kontroler.</span><span class="sxs-lookup"><span data-stu-id="52ed5-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="52ed5-129">Nejprve přidejte atribut autorizovat nad deklaraci třídy Controller budou muset uživatelé zaregistrovat, teprve pak najdete v článku věnovaném:</span><span class="sxs-lookup"><span data-stu-id="52ed5-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="52ed5-130">*Poznámka: Toto je podobná změny, které jsme dříve provedené StoreManagerController, ale v takovém případě atribut autorizovat vyžaduje, aby se uživatel v roli správce. V Kontroleru najdete v článku věnovaném jsme se nutnosti být uživatel přihlášen, ale nejsou vyžadující, aby být správci.*</span><span class="sxs-lookup"><span data-stu-id="52ed5-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="52ed5-131">Z důvodu zjednodušení jsme nebude zabývajících se informace o platebních v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="52ed5-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="52ed5-132">Místo toho jsme se umožnit uživatelům rezervovat pomocí propagační kód.</span><span class="sxs-lookup"><span data-stu-id="52ed5-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="52ed5-133">Tento propagační kód použití konstant s názvem PromoCode jsme se uloží.</span><span class="sxs-lookup"><span data-stu-id="52ed5-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="52ed5-134">Jako StoreController jsme budete pole pro uložení instance třídy MusicStoreEntities s názvem storeDB deklarovat.</span><span class="sxs-lookup"><span data-stu-id="52ed5-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="52ed5-135">Aby bylo možné použít třídy MusicStoreEntities, budeme muset přidat, pomocí příkazu pro obor názvů MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="52ed5-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="52ed5-136">Horní naše najdete v článku věnovaném řadiče se zobrazí níže.</span><span class="sxs-lookup"><span data-stu-id="52ed5-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="52ed5-137">CheckoutController bude mít řadič takto:</span><span class="sxs-lookup"><span data-stu-id="52ed5-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="52ed5-138">**AddressAndPayment (metoda GET)** zobrazí formulář umožňuje, aby uživatel zadal své informace.</span><span class="sxs-lookup"><span data-stu-id="52ed5-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="52ed5-139">**AddressAndPayment (metodu POST)** se ověření vstupu a pořadí zpracování.</span><span class="sxs-lookup"><span data-stu-id="52ed5-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="52ed5-140">**Dokončení** se zobrazí po uživatel úspěšně dokončil proces checkout.</span><span class="sxs-lookup"><span data-stu-id="52ed5-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="52ed5-141">Toto zobrazení bude obsahovat uživatele pořadové číslo, jako potvrzení.</span><span class="sxs-lookup"><span data-stu-id="52ed5-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="52ed5-142">Nejprve umožňuje AddressAndPayment přejmenujte akce kontroleru indexu (který byl vygenerován při jsme vytvořili řadičem).</span><span class="sxs-lookup"><span data-stu-id="52ed5-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="52ed5-143">Tato akce kontroleru právě zobrazí formulář najdete v článku věnovaném, není třeba žádné informace o modelu.</span><span class="sxs-lookup"><span data-stu-id="52ed5-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="52ed5-144">Naše metodu AddressAndPayment POST bude postupovat podle stejného vzoru jsme použili v StoreManagerController: pokusí přijměte odeslání formuláře a dokončení objednávky a znovu zobrazit formulář, pokud se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="52ed5-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="52ed5-145">Po ověření vstupu z formuláře splňuje požadavky naše ověření pořadí, jsme hodnotu formuláře PromoCode zkontrolujte přímo.</span><span class="sxs-lookup"><span data-stu-id="52ed5-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="52ed5-146">Za předpokladu, že je vše v pořádku, že jsme se s pořadím uložit aktualizované informace, řekněte ShoppingCart objekt k dokončení procesu pořadí a přesměrování na dokončení akce.</span><span class="sxs-lookup"><span data-stu-id="52ed5-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="52ed5-147">Po úspěšném dokončení procesu najdete v článku věnovaném uživatelé přesměrováni na akce dokončení kontroleru.</span><span class="sxs-lookup"><span data-stu-id="52ed5-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="52ed5-148">Tato akce provede kontrolu jednoduché k ověření, že pořadí skutečně patří do přihlášeného uživatele před zobrazením pořadové číslo jako potvrzení.</span><span class="sxs-lookup"><span data-stu-id="52ed5-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="52ed5-149">*Poznámka: Zobrazení chyb byl automaticky vytvořen pro nás ve složce /Views/Shared když jsme začali projektu.*</span><span class="sxs-lookup"><span data-stu-id="52ed5-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="52ed5-150">Kód dokončení CheckoutController vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="52ed5-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="52ed5-151">Přidání zobrazení AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="52ed5-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="52ed5-152">Nyní vytvoříme AddressAndPayment zobrazení.</span><span class="sxs-lookup"><span data-stu-id="52ed5-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="52ed5-153">Klikněte pravým tlačítkem na jednu z akce kontroleru AddressAndPayment a přidat zobrazení s názvem AddressAndPayment, který je silného typu jako pořadí a používá šablony upravit, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="52ed5-153">Right-click on one of the the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="52ed5-154">Toto zobrazení se budou používat dva postupy jsme se podívali na při vytváření zobrazení StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="52ed5-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="52ed5-155">Chcete-li zobrazit pole formuláře pro model pořadí použijeme Html.EditorForModel()</span><span class="sxs-lookup"><span data-stu-id="52ed5-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="52ed5-156">Jsme využije ověřovacích pravidel pomocí atributů ověření třídu pořadí</span><span class="sxs-lookup"><span data-stu-id="52ed5-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="52ed5-157">Začneme aktualizací formuláře kód, který použije Html.EditorForModel(), následuje další textové pole pro tento propagační kód.</span><span class="sxs-lookup"><span data-stu-id="52ed5-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="52ed5-158">Kód dokončení pro zobrazení AddressAndPayment jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="52ed5-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="52ed5-159">Definice ověřovacích pravidel pro pořadí</span><span class="sxs-lookup"><span data-stu-id="52ed5-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="52ed5-160">Teď, když je naše zobrazení nastavili, jsme nastaví ověřovacích pravidel pro náš model pořadí jako jsme to udělali dřív Album modelu.</span><span class="sxs-lookup"><span data-stu-id="52ed5-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="52ed5-161">Klikněte pravým tlačítkem na složku modely a přidejte třídu s názvem pořadí.</span><span class="sxs-lookup"><span data-stu-id="52ed5-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="52ed5-162">Kromě ověření atributy, které jsme použili dříve alba také použijeme regulární výraz k ověření e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="52ed5-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's e-mail address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="52ed5-163">Probíhá pokus o odeslání formuláře s chybějící nebo neplatné informace se nyní zobrazí chybovou zprávu pomocí ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="52ed5-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="52ed5-164">V pořádku máme Hotovo většinu práce pevný pro proces najdete v článku věnovaném; právě máme několik pravděpodobnost a končí na dokončení.</span><span class="sxs-lookup"><span data-stu-id="52ed5-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="52ed5-165">Je potřeba přidat dvě jednoduché zobrazení a musíme postará o předání informací košíku během procesu přihlášení.</span><span class="sxs-lookup"><span data-stu-id="52ed5-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="52ed5-166">Přidání zobrazení najdete v článku věnovaném dokončení</span><span class="sxs-lookup"><span data-stu-id="52ed5-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="52ed5-167">Dokončení najdete v článku věnovaném zobrazení je poměrně jednoduché jako potřebuje pouze pro zobrazení ID pořadí.</span><span class="sxs-lookup"><span data-stu-id="52ed5-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="52ed5-168">Klikněte pravým tlačítkem na akce dokončení kontroleru a přidat zobrazení s názvem dokončeno, což je silného typu jako typ int.</span><span class="sxs-lookup"><span data-stu-id="52ed5-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="52ed5-169">Nyní budeme aktualizovat zobrazení kód, který zobrazí ID pořadí, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="52ed5-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="52ed5-170">Aktualizace zobrazení chyb</span><span class="sxs-lookup"><span data-stu-id="52ed5-170">Updating The Error view</span></span>

<span data-ttu-id="52ed5-171">Výchozí šablony zahrnuje zobrazení chyb ve sdílené složce, zobrazení, aby bylo možné ho znovu použít jinde v lokalitě.</span><span class="sxs-lookup"><span data-stu-id="52ed5-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="52ed5-172">Toto zobrazení chyba obsahuje chybu velmi jednoduchý a nepoužívá náš web rozložení, takže jsme ji budete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="52ed5-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="52ed5-173">Vzhledem k tomu, že toto je obecná chyba stránky, je velmi jednoduchý obsah.</span><span class="sxs-lookup"><span data-stu-id="52ed5-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="52ed5-174">Budete zahrnuta zprávu a odkaz přejít na předchozí stránku v historii, pokud chce uživatel zkuste akci znovu.</span><span class="sxs-lookup"><span data-stu-id="52ed5-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
<span data-ttu-id="52ed5-175">[Předchozí](mvc-music-store-part-8.md)
[další](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="52ed5-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
