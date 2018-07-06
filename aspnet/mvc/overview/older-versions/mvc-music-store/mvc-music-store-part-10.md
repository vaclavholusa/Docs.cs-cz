---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: '10. část: Závěrečné aktualizace návrhu navigace a webu, závěr | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. Část 10 popisuje závěrečné aktualizace navigace a S...
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 40ed7c337e097675199ab66229095bd3315d8c8d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836803"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="4a104-104">10. část: Závěrečné aktualizace návrhu navigace a webu, závěr</span><span class="sxs-lookup"><span data-stu-id="4a104-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="4a104-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="4a104-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="4a104-106">MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4a104-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="4a104-107">Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="4a104-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="4a104-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="4a104-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="4a104-109">10. část se věnuje závěrečné aktualizace návrhu navigace a webu, závěr.</span><span class="sxs-lookup"><span data-stu-id="4a104-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="4a104-110">Jsme pro náš web dokončili všechny hlavní funkce, ale máme některé funkce, které chcete přidat k navigaci na webu, na domovské stránce a stránce procházet Store.</span><span class="sxs-lookup"><span data-stu-id="4a104-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="4a104-111">Vytvoření nákupního košíku částečné zobrazení se souhrnnými</span><span class="sxs-lookup"><span data-stu-id="4a104-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="4a104-112">Chcete vystavit počet položek v nákupním košíku uživatele napříč celou lokalitu.</span><span class="sxs-lookup"><span data-stu-id="4a104-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="4a104-113">Můžeme to snadno implementovat tak, že vytvoříte částečné zobrazení, která se přidá do našich Site.master.</span><span class="sxs-lookup"><span data-stu-id="4a104-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="4a104-114">Jak bylo uvedeno výše, zahrnuje kontroleru ShoppingCart CartSummary metody akce, které vrací částečné zobrazení:</span><span class="sxs-lookup"><span data-stu-id="4a104-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="4a104-115">Chcete-li vytvořit CartSummary částečné zobrazení, klikněte pravým tlačítkem na složku zobrazení/ShoppingCart a vyberte Přidat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4a104-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="4a104-116">Název zobrazení CartSummary a zaškrtněte políčko "Vytvořit částečné zobrazení", jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="4a104-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="4a104-117">Částečné zobrazení CartSummary je opravdu jednoduché – je pouze odkaz na zobrazení ShoppingCart Index, který zobrazuje počet položek v košíku.</span><span class="sxs-lookup"><span data-stu-id="4a104-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="4a104-118">Kompletní kód CartSummary.cshtml vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4a104-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="4a104-119">Na libovolné stránce na webu, včetně hlavnímu serveru pomocí metody Html.RenderAction jsme může zahrnovat částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4a104-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="4a104-120">RenderAction vyžaduje, abychom mohli zadat název akce ("CartSummary") a názvu Kontroleru ("ShoppingCart") jako níže.</span><span class="sxs-lookup"><span data-stu-id="4a104-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="4a104-121">Před přidáním to k webu rozložení, vytvoříme i nabídce žánr proto všechny naše Site.master aktualizace najednou.</span><span class="sxs-lookup"><span data-stu-id="4a104-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="4a104-122">Vytváření žánr částečného zobrazení nabídky</span><span class="sxs-lookup"><span data-stu-id="4a104-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="4a104-123">Můžeme provádět je mnohem snazší pro naši uživatelé procházet úložiště tak, že přidáte žánr nabídky, který vypíše všechny žánry k dispozici v našem úložišti.</span><span class="sxs-lookup"><span data-stu-id="4a104-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="4a104-124">Budeme postupovat stejný postup také vytvořit GenreMenu částečné zobrazení, a potom můžeme přidat je do hlavní větve serveru.</span><span class="sxs-lookup"><span data-stu-id="4a104-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="4a104-125">Nejprve přidejte následující akce kontroleru GenreMenu k StoreController:</span><span class="sxs-lookup"><span data-stu-id="4a104-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="4a104-126">Tato akce vrátí seznam hodnot žánry, které se zobrazí podle částečné zobrazení, které budou dalších krocích vytvoříme.</span><span class="sxs-lookup"><span data-stu-id="4a104-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="4a104-127">*Poznámka: Jsme přidali atribut [ChildActionOnly] do této akce kontroleru, který označuje, že chceme jenom tato akce pro použití v částečné zobrazení. Tento atribut nebudou moct být prováděn tak, že přejdete do /Store/GenreMenu akce kontroleru. Není to povinné pro částečné zobrazení, ale je dobrým zvykem, protože chceme mít jistotu, že naše akce kontroleru se používají jako plánujeme. Můžeme také vrací PartialView spíše než zobrazení, které vám umožní zobrazovací modul ví, že neměli používat rozložení pro toto zobrazení, jak je zahrnuto v ostatních zobrazeních.*</span><span class="sxs-lookup"><span data-stu-id="4a104-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="4a104-128">Klikněte pravým tlačítkem na akce kontroleru GenreMenu a vytvořit částečné zobrazení s názvem GenreMenu, což je silně typováno pomocí třídy rozšířením podle tematických zobrazení dat, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="4a104-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="4a104-129">Aktualizace kódu zobrazení pro částečné zobrazení GenreMenu pro zobrazení položek neuspořádaný seznam následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="4a104-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="4a104-130">Aktualizace rozložení webu do našich částečné zobrazení</span><span class="sxs-lookup"><span data-stu-id="4a104-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="4a104-131">Přidáme náš částečné zobrazení k rozložení webu (/Zobrazit/Shared/\_Layout.cshtml) voláním Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="4a104-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="4a104-132">Přidáme je do, jakož i některé další značky se zobrazí, jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="4a104-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="4a104-133">Nyní když jsme aplikaci spustit, uvidíme žánr v levé navigační oblasti a košíku v horní části.</span><span class="sxs-lookup"><span data-stu-id="4a104-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="4a104-134">Aktualizovat stránku procházet Store</span><span class="sxs-lookup"><span data-stu-id="4a104-134">Update to the Store Browse page</span></span>

<span data-ttu-id="4a104-135">Stránka procházet Store je funkční, ale nevypadá velmi dobré.</span><span class="sxs-lookup"><span data-stu-id="4a104-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="4a104-136">Abychom mohli aktualizovat stránku zobrazíte alba lepšího rozložení aktualizováním zobrazení kódu (nachází se /Views/Store/Browse.cshtml) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4a104-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="4a104-137">Tady jsou nyní používají Url.Action spíše než Html.ActionLink, můžete použít speciální formátování na propojení zahrnout alba obrázky.</span><span class="sxs-lookup"><span data-stu-id="4a104-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="4a104-138">*Poznámka: Jsme zobrazujete obecný alb tyto alb. Tyto informace jsou uloženy v databázi a je možné upravovat prostřednictvím Správce Store. Určitě chcete-li přidat vlastní obrázky.*</span><span class="sxs-lookup"><span data-stu-id="4a104-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="4a104-139">Nyní když jsme procházet rozšířením podle tematických, uvidíme alb je znázorněno v mřížka s obrázky alba.</span><span class="sxs-lookup"><span data-stu-id="4a104-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="4a104-140">Aktualizuje se na domovskou stránku a zobrazit prodej alb nahoru</span><span class="sxs-lookup"><span data-stu-id="4a104-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="4a104-141">Chceme, aby funkce naší nejvyšší prodeje alb na domovskou stránku a zvýšit prodej.</span><span class="sxs-lookup"><span data-stu-id="4a104-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="4a104-142">Provedeme některé aktualizace s naší HomeController ke zpracování a přidat některé další grafiky.</span><span class="sxs-lookup"><span data-stu-id="4a104-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="4a104-143">Nejprve přidáme navigační vlastnost pro naše třída alba tak, aby EntityFramework ví, že jsou přidružené.</span><span class="sxs-lookup"><span data-stu-id="4a104-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="4a104-144">Několik posledních řádků z našich **alba** třídy by teď měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="4a104-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="4a104-145">*Poznámka: To vyžaduje přidání pomocí příkazu pro System.Collections.Generic – obor názvů.*</span><span class="sxs-lookup"><span data-stu-id="4a104-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="4a104-146">Nejprve přidáme storeDB pole a MvcMusicStore.Models pomocí příkazů, stejně jako v našich řadiče.</span><span class="sxs-lookup"><span data-stu-id="4a104-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="4a104-147">V dalším kroku přidáme následující metodu HomeController, který se dotazuje databázi najít hlavní alb prodej podle OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="4a104-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="4a104-148">Toto je privátní metodu, protože nechceme, aby byl k dispozici jako akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="4a104-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="4a104-149">Jsme včetně v HomeController pro zjednodušení, ale doporučujeme přesunout vaši obchodní logiku do třídy samostatné služby podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4a104-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="4a104-150">To na místě abychom mohli aktualizovat Index akce kontroleru pro dotazování hlavních 5 prodej alba a vrátit do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4a104-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="4a104-151">Kompletní kód pro aktualizovaný HomeController je uvedeno dále.</span><span class="sxs-lookup"><span data-stu-id="4a104-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="4a104-152">Nakonec potřebujeme aktualizace našich zobrazení Home Index tak, aby ho můžete zobrazit seznam alb aktualizace typu modelu a přidáním do dolní části seznamu alb.</span><span class="sxs-lookup"><span data-stu-id="4a104-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="4a104-153">My podnikneme této příležitosti a také přidat nadpis a povýšení části na stránku.</span><span class="sxs-lookup"><span data-stu-id="4a104-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="4a104-154">Nyní když jsme aplikaci spustit, uvidíme aktualizované domovské stránky s nejvyšší prodejní alba a naše propagační zprávy.</span><span class="sxs-lookup"><span data-stu-id="4a104-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="4a104-155">Závěr</span><span class="sxs-lookup"><span data-stu-id="4a104-155">Conclusion</span></span>

<span data-ttu-id="4a104-156">Viděli jsme, že technologie ASP.NET MVC lze snadno vytvořit sofistikované web se přístup k databázi, členství, AJAX, atd.</span><span class="sxs-lookup"><span data-stu-id="4a104-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="4a104-157">poměrně rychle.</span><span class="sxs-lookup"><span data-stu-id="4a104-157">pretty quickly.</span></span> <span data-ttu-id="4a104-158">Snad v tomto kurzu, má udělené vám nástroje, které potřebujete, abyste mohli začít vytvářet své vlastní technologie ASP.NET MVC aplikace!</span><span class="sxs-lookup"><span data-stu-id="4a104-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="4a104-159">Předchozí</span><span class="sxs-lookup"><span data-stu-id="4a104-159">Previous</span></span>](mvc-music-store-part-9.md)
