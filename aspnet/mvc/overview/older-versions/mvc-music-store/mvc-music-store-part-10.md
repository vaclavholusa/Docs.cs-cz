---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Část 10: Poslední aktualizace navigace a návrhu webu, uzavření | Microsoft Docs'
author: jongalloway
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 10 obsahuje poslední aktualizace navigaci a S....
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: b40d194c4d08f3564da59bacde4b5d3d7663373a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="f8f7c-104">Část 10: Poslední aktualizace navigace a návrhu webu, uzavření</span><span class="sxs-lookup"><span data-stu-id="f8f7c-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="f8f7c-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="f8f7c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="f8f7c-106">Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="f8f7c-107">Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="f8f7c-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="f8f7c-109">Část 10 obsahuje poslední aktualizace navigační a návrhu webu, uzavření.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="f8f7c-110">Dokončení všechny hlavní funkce pro náš web, ale stále k dispozici některé funkce pro přidání do webové navigace, domovské stránky a stránky procházet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="f8f7c-111">Vytváření nákupní košík souhrnné částečné zobrazení</span><span class="sxs-lookup"><span data-stu-id="f8f7c-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="f8f7c-112">Chceme zpřístupnit počet položek v nákupní košík uživatele celého webu.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="f8f7c-113">Jsme to snadno implementovat vytvořením částečné zobrazení, která se přidá do našich Site.master.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="f8f7c-114">Jak je uvedeno dříve, zahrnuje řadičem ShoppingCart CartSummary metoda akce, která vrátí hodnotu částečné zobrazení:</span><span class="sxs-lookup"><span data-stu-id="f8f7c-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="f8f7c-115">Chcete-li vytvořit CartSummary částečné zobrazení, klikněte pravým tlačítkem na složku, zobrazení nebo ShoppingCart a vyberte Přidat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="f8f7c-116">Název zobrazení CartSummary a zaškrtnutím políčka "Vytvořit částečné zobrazení", jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="f8f7c-117">Částečné zobrazení CartSummary skutečně jednoduché – je právě odkaz na zobrazení ShoppingCart Index, který zobrazuje počet položek v košíku.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="f8f7c-118">Kompletní kód CartSummary.cshtml vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="f8f7c-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="f8f7c-119">Do libovolné stránce webu, včetně hlavním lokality pomocí metody Html.RenderAction jsme můžete zahrnout částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="f8f7c-120">RenderAction vyžaduje nám zadejte název akce ("CartSummary") a názvu Kontroleru ("ShoppingCart") jako níže.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="f8f7c-121">Před přidáním to k webu rozložení, vytvoříme i v nabídce Genre, abychom mohli provádět všechny aktualizace našich Site.master najednou.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="f8f7c-122">Vytváření Genre částečné zobrazení nabídky</span><span class="sxs-lookup"><span data-stu-id="f8f7c-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="f8f7c-123">Jsme můžete bylo mnohem snazší pro naši uživatelé procházet úložišti přidáním Genre nabídky, které jsou uvedeny všechny žánry k dispozici v našem úložišti.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="f8f7c-124">Jsme bude postupovat podle stejné kroky také vytvořit částečné zobrazení GenreMenu a pak můžete přidáme oba k hlavnímu serveru lokality.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="f8f7c-125">Nejprve přidejte následující akce kontroleru GenreMenu StoreController:</span><span class="sxs-lookup"><span data-stu-id="f8f7c-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="f8f7c-126">Tato akce vrátí seznam hodnot žánry, které se zobrazí v částečné zobrazení, které vytvoříme Další.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="f8f7c-127">*Poznámka: Jsme přidali atribut [ChildActionOnly] s touto akcí kontroleru, který označuje, že chceme jenom této akci je možné použít částečné zobrazení. Tento atribut bude brání provedení akce kontroleru vykonáván procházením /Store/GenreMenu. Není to povinné pro částečné zobrazení, ale je dobrým zvykem, vzhledem k tomu, že chceme, abyste měli jistotu, že naše akce kontroleru se používají jako plánujeme. Také jsme se vrací PartialView spíše než zobrazení, která umožní zobrazovací modul vědět, že nepoužívejte rozložení pro toto zobrazení, jako je nebudou zahrnuty v ostatních zobrazeních.*</span><span class="sxs-lookup"><span data-stu-id="f8f7c-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="f8f7c-128">Klikněte pravým tlačítkem na akce kontroleru GenreMenu a vytvořit částečné zobrazení s názvem GenreMenu, což je silného typu pomocí třídy Genre zobrazení dat, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="f8f7c-129">Aktualizujte kód zobrazení pro částečné zobrazení GenreMenu a zobrazit seznam položek pomocí neuspořádaný seznam následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="f8f7c-130">Aktualizace lokality rozložení-li zobrazit naše částečné zobrazení</span><span class="sxs-lookup"><span data-stu-id="f8f7c-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="f8f7c-131">Přidáme naše částečné zobrazení k rozložení lokality (nebozobrazení/sdílené/\_Layout.cshtml) voláním Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="f8f7c-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="f8f7c-132">Přidáme je obě, jakož i některé další značek k zobrazení, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="f8f7c-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="f8f7c-133">Nyní když jsme aplikaci spustit, jsme se zobrazí souhrn košíku v horní části a Genre v levé navigační oblasti.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="f8f7c-134">Aktualizovat na stránce úložiště Procházet</span><span class="sxs-lookup"><span data-stu-id="f8f7c-134">Update to the Store Browse page</span></span>

<span data-ttu-id="f8f7c-135">Stránka procházet úložiště je funkční, ale nevypadá velmi vhodné.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="f8f7c-136">Aktualizujeme stránku můžete zobrazit alb v lepší rozložení aktualizací zobrazení kód (nalezené v /Views/Store/Browse.cshtml) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f8f7c-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="f8f7c-137">Zde vydáváme používat Url.Action než Html.ActionLink tak, aby jsme můžete použít zvláštní formátování odkaz zahrnout kresby alb.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="f8f7c-138">*Poznámka: Jsme se zobrazení Obecné alb pro tyto alb. Tyto informace jsou uloženy v databázi a upravovat přes Správce úložiště. Jste Vítejte přidat vlastní kresby.*</span><span class="sxs-lookup"><span data-stu-id="f8f7c-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="f8f7c-139">Nyní když jsme vyhledejte Genre, jsme se zobrazí alb v mřížce s kresby alb.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="f8f7c-140">Aktualizace na domovskou stránku a zobrazí prodejní alb horní</span><span class="sxs-lookup"><span data-stu-id="f8f7c-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="f8f7c-141">Chceme funkci Naše nejlépe prodávané alb na domovské stránce zvýšit prodej.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="f8f7c-142">Naše HomeController ke zpracování a přidat některé další grafického jsme budete provádět některé aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="f8f7c-143">Nejprve přidáme navigační vlastnosti do našich Album třídy tak, aby EntityFramework ví, že jsou přidružené.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="f8f7c-144">Posledních několika řádků naše **Album** třída by teď měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="f8f7c-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="f8f7c-145">*Poznámka: To bude vyžadovat přidání pomocí příkazu mají být předány System.Collections.Generic – obor názvů.*</span><span class="sxs-lookup"><span data-stu-id="f8f7c-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="f8f7c-146">Nejprve přidáme pole storeDB a MvcMusicStore.Models příkazy, jako v našich ostatních řadičů.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="f8f7c-147">Potom přidáme metodu HomeController, který se dotazuje databáze najít nejvyšší prodejní alb podle Rozpis objednávek.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="f8f7c-148">To je soukromá metoda, protože jsme nechcete zpřístupní ji jako akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="f8f7c-149">Jsme včetně v HomeController pro jednoduchost, ale doporučujeme přesunout obchodní logiku do třídy samostatné služby podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="f8f7c-150">S třídou na místě budeme aktualizovat Index akce kontroleru pro dotazování top 5 prodávané alb a vrátí je do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="f8f7c-151">Kód dokončení pro aktualizovaný HomeController je, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="f8f7c-152">Nakonec jsme budete muset aktualizovat naše Domů Index zobrazení tak, aby ho můžete zobrazit seznam alb aktualizace typu modelu a přidáním seznamu alb do dolní části.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="f8f7c-153">Tato možnost také přidat nadpis a povýšení část na stránku jsme bude trvat.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="f8f7c-154">Nyní když jsme aplikaci spustit, uvidíme aktualizované domovské stránky s nejvyšší prodejní alb a naše propagační zprávou.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="f8f7c-155">Závěr</span><span class="sxs-lookup"><span data-stu-id="f8f7c-155">Conclusion</span></span>

<span data-ttu-id="f8f7c-156">Jste viděli, že rozhraní ASP.NET MVC lze snadno vytvořit sofistikované web s přístupem k databázi, členství, AJAX, atd.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="f8f7c-157">poměrně rychle.</span><span class="sxs-lookup"><span data-stu-id="f8f7c-157">pretty quickly.</span></span> <span data-ttu-id="f8f7c-158">Zpravidla v tomto kurzu jste obdrželi nástroje, které potřebujete, abyste mohli začít vytváření vlastní rozhraní ASP.NET MVC aplikací!</span><span class="sxs-lookup"><span data-stu-id="f8f7c-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="f8f7c-159">Předchozí</span><span class="sxs-lookup"><span data-stu-id="f8f7c-159">Previous</span></span>](mvc-music-store-part-9.md)
