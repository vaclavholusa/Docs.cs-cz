---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Přehled řadiče ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: V tomto kurzu Stephen Walther vás seznámí s ASP.NET MVC řadiče. Zjistíte, jak vytvořit nové řadiče a vrátíte se různé typy res akce...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0a45630e8f2d5ae0548bb6b8496df08ca5877a40
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="d6657-104">Přehled řadiče ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="d6657-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="d6657-105">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="d6657-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="d6657-106">V tomto kurzu Stephen Walther vás seznámí s ASP.NET MVC řadiče.</span><span class="sxs-lookup"><span data-stu-id="d6657-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="d6657-107">Zjistíte, jak vytvořit nové řadiče a vrátíte se různé typy výsledky akce.</span><span class="sxs-lookup"><span data-stu-id="d6657-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="d6657-108">V tomto kurzu prozkoumá téma řadiče ASP.NET MVC, akce kontroleru a výsledky akce.</span><span class="sxs-lookup"><span data-stu-id="d6657-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="d6657-109">Po dokončení tohoto kurzu bude pochopit, jak se řadiče používají k řízení způsobu, jakým návštěvníka komunikuje s webem ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d6657-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="d6657-110">Vysvětlení Kontrolerů</span><span class="sxs-lookup"><span data-stu-id="d6657-110">Understanding Controllers</span></span>

<span data-ttu-id="d6657-111">Řadiče MVC je zodpovědná za neodpovídá na požadavky vytvořené pro stránku ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d6657-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="d6657-112">Každý požadavek prohlížeče je namapována na určitý kontroler.</span><span class="sxs-lookup"><span data-stu-id="d6657-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="d6657-113">Představte si například, zadejte následující adresu URL do adresního řádku prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="d6657-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="d6657-114">V takovém případě je volána řadič ProductController.</span><span class="sxs-lookup"><span data-stu-id="d6657-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="d6657-115">ProductController je zodpovědný za generování odpovědi na požadavek prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d6657-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="d6657-116">Například kontroler může vrátit konkrétní zobrazení zpět do prohlížeče nebo řadičem může přesměruje uživatele na jiný řadič.</span><span class="sxs-lookup"><span data-stu-id="d6657-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="d6657-117">Výpis 1 obsahuje řadič jednoduché s názvem ProductController.</span><span class="sxs-lookup"><span data-stu-id="d6657-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="d6657-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="d6657-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="d6657-119">Jak je vidět na výpisu 1, řadič je právě třídy (třídy jazyka Visual Basic .NET nebo C#).</span><span class="sxs-lookup"><span data-stu-id="d6657-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="d6657-120">Řadič je třída, která je odvozena ze základní třídy System.Web.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="d6657-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="d6657-121">Protože řadič dědí z této základní třídy, řadič dědí několik užitečných metod zdarma (probereme tyto metody za chvíli).</span><span class="sxs-lookup"><span data-stu-id="d6657-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="d6657-122">Pochopení akce Kontroleru</span><span class="sxs-lookup"><span data-stu-id="d6657-122">Understanding Controller Actions</span></span>

<span data-ttu-id="d6657-123">Řadič zpřístupní akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d6657-123">A controller exposes controller actions.</span></span> <span data-ttu-id="d6657-124">Akce je metoda na řadič, která je volána, když zadáte konkrétní adresu URL do adresního řádku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d6657-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="d6657-125">Představte si například, že zadáte požadavek na následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="d6657-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="d6657-126">V takovém případě je na třídě ProductController volána metoda Index().</span><span class="sxs-lookup"><span data-stu-id="d6657-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="d6657-127">Metoda Index() je příklad akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d6657-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="d6657-128">Akce kontroleru musí být veřejné metody třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d6657-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="d6657-129">Visual Basic.NET metody, ve výchozím nastavení, jsou veřejné metody.</span><span class="sxs-lookup"><span data-stu-id="d6657-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="d6657-130">Uvědomte si, že všechny veřejná metoda, která přidáte do třídy kontroleru je k dispozici jako akce kontroleru automaticky (je třeba dbát o to vzhledem k tomu, že akce kontroleru můžete vyvolat každý, kdo universe jednoduše tak, že adresa URL pravého psaní do adresního řádku prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="d6657-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="d6657-131">Existují některé další požadavky, které musí splnit akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d6657-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="d6657-132">Nemohou být přetíženy metodu použít jako akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d6657-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="d6657-133">Kromě toho akce kontroleru nemůže být statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="d6657-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="d6657-134">Než tu, která můžete použít jenom o libovolnou metodu jako akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d6657-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="d6657-135">Seznámení s výsledky akce</span><span class="sxs-lookup"><span data-stu-id="d6657-135">Understanding Action Results</span></span>

<span data-ttu-id="d6657-136">Akce kontroleru vrátí takzvaný *výsledek akce*.</span><span class="sxs-lookup"><span data-stu-id="d6657-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="d6657-137">Výsledek akce je akce kontroleru vrátí v reakci na žádost prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d6657-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="d6657-138">Rozhraní ASP.NET MVC podporuje několik typů výsledky akcí, včetně:</span><span class="sxs-lookup"><span data-stu-id="d6657-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="d6657-139">ViewResult - představuje HTML a značek.</span><span class="sxs-lookup"><span data-stu-id="d6657-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="d6657-140">EmptyResult - představuje žádný výsledek.</span><span class="sxs-lookup"><span data-stu-id="d6657-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="d6657-141">RedirectResult - představuje požadavek na přesměrování na novou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d6657-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="d6657-142">JsonResult - představuje JavaScript Object Notation výsledek, který lze použít v aplikaci AJAX.</span><span class="sxs-lookup"><span data-stu-id="d6657-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="d6657-143">JavaScriptResult - představuje skript jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d6657-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="d6657-144">ContentResult - představuje výsledek text.</span><span class="sxs-lookup"><span data-stu-id="d6657-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="d6657-145">FileContentResult - představuje soubor ke stažení (s binární obsah).</span><span class="sxs-lookup"><span data-stu-id="d6657-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="d6657-146">FilePathResult - představuje soubor ke stažení (s cestou).</span><span class="sxs-lookup"><span data-stu-id="d6657-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="d6657-147">FileStreamResult - představuje soubor ke stažení (s datový proud souboru).</span><span class="sxs-lookup"><span data-stu-id="d6657-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="d6657-148">Všechny tyto výsledky akce dědí ze základní třídy ActionResult.</span><span class="sxs-lookup"><span data-stu-id="d6657-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="d6657-149">Ve většině případů se akce kontroleru vrátí ViewResult.</span><span class="sxs-lookup"><span data-stu-id="d6657-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="d6657-150">Například akce kontroleru indexu v výpis 2 vrátí ViewResult.</span><span class="sxs-lookup"><span data-stu-id="d6657-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="d6657-151">**Výpis 2 - Controllers\BookController.vb**</span><span class="sxs-lookup"><span data-stu-id="d6657-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="d6657-152">Když akce vrátí ViewResult, HTML se vrátí do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d6657-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="d6657-153">Vrátí metoda Index() v výpis 2 zobrazení s názvem Index do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d6657-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="d6657-154">Všimněte si, že akce Index() ve výpisu 2 nevrací ViewResult().</span><span class="sxs-lookup"><span data-stu-id="d6657-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="d6657-155">Místo toho je volána metoda View() základní třídy Controller.</span><span class="sxs-lookup"><span data-stu-id="d6657-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="d6657-156">Za normálních okolností je nevrátí výsledek akce přímo.</span><span class="sxs-lookup"><span data-stu-id="d6657-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="d6657-157">Místo toho volání jednoho z následujících metod základní třídy Controller:</span><span class="sxs-lookup"><span data-stu-id="d6657-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="d6657-158">Zobrazení – vrátí výsledek akce ViewResult.</span><span class="sxs-lookup"><span data-stu-id="d6657-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="d6657-159">Přesměrování – vrátí výsledek akce RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="d6657-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="d6657-160">RedirectToAction - vrací výsledek akce RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="d6657-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="d6657-161">RedirectToRoute - vrací výsledek akce RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="d6657-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="d6657-162">JSON – vrátí výsledek akce JsonResult.</span><span class="sxs-lookup"><span data-stu-id="d6657-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="d6657-163">JavaScriptResult – vrátí JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="d6657-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="d6657-164">Obsah – vrátí výsledek akce ContentResult.</span><span class="sxs-lookup"><span data-stu-id="d6657-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="d6657-165">Soubor – vrátí FileContentResult, FilePathResult nebo FileStreamResult v závislosti na parametrech předaný metodě.</span><span class="sxs-lookup"><span data-stu-id="d6657-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="d6657-166">Ano Pokud chcete vrátit zobrazení do prohlížeče, volejte metodu View().</span><span class="sxs-lookup"><span data-stu-id="d6657-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="d6657-167">Pokud chcete přesměrovat uživatele z jednoho řadiče akce do druhého, lze volat metodu RedirectToAction().</span><span class="sxs-lookup"><span data-stu-id="d6657-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="d6657-168">Například akce Details() ve výpisu 3 buď zobrazení nebo přesměruje uživatele na Index() akci v závislosti na tom, jestli parametr Id má hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d6657-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="d6657-169">**Výpis 3 - CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="d6657-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="d6657-170">Výsledek akce ContentResult je speciální.</span><span class="sxs-lookup"><span data-stu-id="d6657-170">The ContentResult action result is special.</span></span> <span data-ttu-id="d6657-171">Výsledek akce ContentResult můžete použít k návratu výsledku akce jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="d6657-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="d6657-172">Například metoda Index() výpis 4 vrátí zprávu jako prostý text a ne jako HTML.</span><span class="sxs-lookup"><span data-stu-id="d6657-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="d6657-173">**Výpis 4 - Controllers\StatusController.vb**</span><span class="sxs-lookup"><span data-stu-id="d6657-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="d6657-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="d6657-174">StatusController</span></span>
> 
> 
> <span data-ttu-id="d6657-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="d6657-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="d6657-176">Po vyvolání akce StatusController.Index() zobrazení nevrátí.</span><span class="sxs-lookup"><span data-stu-id="d6657-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="d6657-177">Místo toho nezpracovaný text "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="d6657-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="d6657-178">se vrátí do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d6657-178">is returned to the browser.</span></span>

<span data-ttu-id="d6657-179">Pokud akce kontroleru vrátí výsledek, který není výsledek akce - je například datum nebo celé číslo – pak výsledek je uzavřen do ContentResult automaticky.</span><span class="sxs-lookup"><span data-stu-id="d6657-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="d6657-180">Například po vyvolání akce Index() WorkController v výpis 5 datum se vrátí jako ContentResult automaticky.</span><span class="sxs-lookup"><span data-stu-id="d6657-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="d6657-181">**Výpis 5 - WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="d6657-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="d6657-182">Akce Index() ve výpisu 5 vrátí objekt data a času.</span><span class="sxs-lookup"><span data-stu-id="d6657-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="d6657-183">Rozhraní ASP.NET MVC převádí data a času objekt na řetězec a zabalí hodnoty DateTime ve ContentResult automaticky.</span><span class="sxs-lookup"><span data-stu-id="d6657-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="d6657-184">Prohlížeč obdrží datum a čas jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="d6657-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="d6657-185">Souhrn</span><span class="sxs-lookup"><span data-stu-id="d6657-185">Summary</span></span>

<span data-ttu-id="d6657-186">Účelem tohoto kurzu bylo seznámí s koncepty řadičů ASP.NET MVC, akce kontroleru a výsledky akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d6657-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="d6657-187">V první části jste zjistili, jak k přidávání nových řadičů do projektu aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d6657-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="d6657-188">V dalším kroku jste se dozvěděli, jak veřejné metody řadiče jsou zveřejněné na základní soubor jako akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d6657-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="d6657-189">Nakonec jsme probrali různé typy výsledků akcí, které mohou být vráceny z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d6657-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="d6657-190">Konkrétně jsme se bavili jak vracet ViewResult, RedirectToActionResult a ContentResult z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d6657-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d6657-191">[Předchozí](creating-a-custom-route-constraint-cs.md)
> [další](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d6657-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
