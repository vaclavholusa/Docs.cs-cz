---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: ASP.NET MVC – přehled Kontrolerů (C#) | Dokumentace Microsoftu
author: StephenWalther
description: V tomto kurzu Stephen Walther vás seznámí s kontrolery ASP.NET MVC. Zjistíte, jak vytvořit nové řadiče a vracet různé druhy res akce...
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: f8ff1ed19e6357a9a4e98f755a677d05ae5c2ec4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821719"
---
<a name="aspnet-mvc-controller-overview-c"></a><span data-ttu-id="8616c-104">ASP.NET MVC – přehled Kontrolerů (C#)</span><span class="sxs-lookup"><span data-stu-id="8616c-104">ASP.NET MVC Controller Overview (C#)</span></span>
====================
<span data-ttu-id="8616c-105">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="8616c-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="8616c-106">V tomto kurzu Stephen Walther vás seznámí s kontrolery ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8616c-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="8616c-107">Zjistíte, jak vytvořit nové řadiče a vracet různé typy výsledků akcí.</span><span class="sxs-lookup"><span data-stu-id="8616c-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="8616c-108">Tento kurz se věnuje téma kontrolery ASP.NET MVC, akce kontroleru a výsledky akce.</span><span class="sxs-lookup"><span data-stu-id="8616c-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="8616c-109">Po dokončení tohoto kurzu budete rozumět, jak se řadiče používají k řízení způsobu, jakým návštěvník komunikuje se službou Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8616c-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="8616c-110">Vysvětlení Kontrolerů</span><span class="sxs-lookup"><span data-stu-id="8616c-110">Understanding Controllers</span></span>

<span data-ttu-id="8616c-111">Kontrolery MVC je zodpovědná za odpověď na požadavky na web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8616c-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="8616c-112">Každý požadavek prohlížeče je namapována na určitý kontroler.</span><span class="sxs-lookup"><span data-stu-id="8616c-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="8616c-113">Představte si například, zadejte následující adresu URL do adresního řádku svého prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="8616c-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="8616c-114">V takovém případě je vyvolána s názvem ProductController kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8616c-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="8616c-115">Je zodpovědná za generování odpověď na žádost prohlížeče ProductController.</span><span class="sxs-lookup"><span data-stu-id="8616c-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="8616c-116">Například kontroler může vrátit konkrétní zobrazení zpět do prohlížeče nebo kontroler může přesměruje uživatele na jiný kontroler.</span><span class="sxs-lookup"><span data-stu-id="8616c-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="8616c-117">Výpis 1 obsahuje jednoduché ProductController řadič.</span><span class="sxs-lookup"><span data-stu-id="8616c-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="8616c-118">**Listing1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="8616c-118">**Listing1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

<span data-ttu-id="8616c-119">Jak je vidět z výpisu 1 kontroleru je právě třídy (třídy jazyka Visual Basic .NET nebo C#).</span><span class="sxs-lookup"><span data-stu-id="8616c-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="8616c-120">Kontroler je třída, která je odvozena ze základní třídy System.Web.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="8616c-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="8616c-121">Protože kontroler dědí z této základní třídy, řadič zdarma zdědí několik užitečných metod (můžeme probírat tyto metody za chvíli).</span><span class="sxs-lookup"><span data-stu-id="8616c-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="8616c-122">Principy akce Kontroleru</span><span class="sxs-lookup"><span data-stu-id="8616c-122">Understanding Controller Actions</span></span>

<span data-ttu-id="8616c-123">Kontroler zpřístupní akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8616c-123">A controller exposes controller actions.</span></span> <span data-ttu-id="8616c-124">Akce je metoda na řadiči, která je volána při zadání určité adresy URL v adresním řádku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="8616c-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="8616c-125">Představte si například, že zadáte požadavek na následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="8616c-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="8616c-126">V takovém případě je volána metoda Index() ProductController třídy.</span><span class="sxs-lookup"><span data-stu-id="8616c-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="8616c-127">Metoda Index() je příklad akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8616c-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="8616c-128">Akce kontroleru musí být veřejné metody třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8616c-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="8616c-129">C# metody, ve výchozím nastavení, jsou privátní metody.</span><span class="sxs-lookup"><span data-stu-id="8616c-129">C# methods, by default, are private methods.</span></span> <span data-ttu-id="8616c-130">Uvědomte si, že všechny veřejné metody, který přidáte do třídu kontroleru je automaticky vystavena jako akce kontroleru (musíte být opatrní při to od akce kontroleru můžete vyvolat kdokoli v universe jednoduše tak, že zadáte správné adresy URL do adresního řádku prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="8616c-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="8616c-131">Existují některé další požadavky, které musí být splněno akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8616c-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="8616c-132">Metoda používá jako akce kontroleru nemohou být přetíženy.</span><span class="sxs-lookup"><span data-stu-id="8616c-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="8616c-133">Kromě toho akce kontroleru nemůže být statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="8616c-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="8616c-134">Kromě toho můžete použít prakticky jakoukoli metodu jako akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8616c-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="8616c-135">Principy výsledky akcí</span><span class="sxs-lookup"><span data-stu-id="8616c-135">Understanding Action Results</span></span>

<span data-ttu-id="8616c-136">Akce kontroleru vrátí hodnotu s názvem *výsledek akce*.</span><span class="sxs-lookup"><span data-stu-id="8616c-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="8616c-137">Výsledek akce se akce kontroleru vrátí v reakci na žádost prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="8616c-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="8616c-138">Architektura ASP.NET MVC podporuje několik typů výsledky akcí, včetně:</span><span class="sxs-lookup"><span data-stu-id="8616c-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="8616c-139">ViewResult – představuje HTML a kódu.</span><span class="sxs-lookup"><span data-stu-id="8616c-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="8616c-140">EmptyResult – představuje žádný výsledek.</span><span class="sxs-lookup"><span data-stu-id="8616c-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="8616c-141">RedirectResult – představuje přesměrování na novou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8616c-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="8616c-142">JsonResult – představuje JavaScript Object Notation výsledek, který lze použít v aplikaci AJAX.</span><span class="sxs-lookup"><span data-stu-id="8616c-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="8616c-143">JavaScriptResult – představuje skriptu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8616c-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="8616c-144">ContentResult – představuje výsledek text.</span><span class="sxs-lookup"><span data-stu-id="8616c-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="8616c-145">FileContentResult – představuje soubor ke stažení (s binární obsah).</span><span class="sxs-lookup"><span data-stu-id="8616c-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="8616c-146">FilePathResult – představuje soubor ke stažení (s cestou).</span><span class="sxs-lookup"><span data-stu-id="8616c-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="8616c-147">FileStreamResult – představuje soubor ke stažení (s datový proud souboru).</span><span class="sxs-lookup"><span data-stu-id="8616c-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="8616c-148">Všechny tyto akce výsledky dědí ze základní třídy ActionResult.</span><span class="sxs-lookup"><span data-stu-id="8616c-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="8616c-149">Ve většině případů se akce kontroleru vrátí ViewResult.</span><span class="sxs-lookup"><span data-stu-id="8616c-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="8616c-150">Například vrátí Index akce kontroleru v zobrazení 2 ViewResult.</span><span class="sxs-lookup"><span data-stu-id="8616c-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="8616c-151">**Výpis 2 - Controllers\BookController.cs**</span><span class="sxs-lookup"><span data-stu-id="8616c-151">**Listing 2 - Controllers\BookController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

<span data-ttu-id="8616c-152">Po návratu akce ViewResult, HTML se vrátí do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="8616c-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="8616c-153">Metoda Index() ve výpisu 2 vrátí zobrazení s názvem Index do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="8616c-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="8616c-154">Všimněte si, že akce Index() ve výpisu 2 nevrací ViewResult().</span><span class="sxs-lookup"><span data-stu-id="8616c-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="8616c-155">Místo toho je volána metoda View() základní třídy Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8616c-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="8616c-156">Za normálních okolností je nevrátí výsledek akce přímo.</span><span class="sxs-lookup"><span data-stu-id="8616c-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="8616c-157">Místo toho můžete volat jednu z následujících metod základní třídy Kontroleru:</span><span class="sxs-lookup"><span data-stu-id="8616c-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="8616c-158">Zobrazit – vrátí výsledek akce ViewResult.</span><span class="sxs-lookup"><span data-stu-id="8616c-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="8616c-159">Přesměrování - vrací výsledek akce RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="8616c-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="8616c-160">RedirectToAction – vrátí výsledek akce RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="8616c-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="8616c-161">Metodu RedirectToRoute – vrátí výsledek akce RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="8616c-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="8616c-162">JSON – vrátí výsledek akce JsonResult.</span><span class="sxs-lookup"><span data-stu-id="8616c-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="8616c-163">JavaScriptResult – vrátí JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="8616c-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="8616c-164">Obsah – vrátí výsledek akce ContentResult.</span><span class="sxs-lookup"><span data-stu-id="8616c-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="8616c-165">Soubor – vrátí FileContentResult, FilePathResult nebo FileStreamResult v závislosti na parametrech předaný metodě.</span><span class="sxs-lookup"><span data-stu-id="8616c-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="8616c-166">Ano Pokud chcete vrátit zobrazení v prohlížeči, zavolejte metodu View().</span><span class="sxs-lookup"><span data-stu-id="8616c-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="8616c-167">Pokud chcete přesměrovat uživatele z jednoho řadiče akce do druhého, zavolejte metodu RedirectToAction().</span><span class="sxs-lookup"><span data-stu-id="8616c-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="8616c-168">Například akce Details() ve výpisu 3 zobrazuje zobrazení nebo přesměruje uživatele na Index() akci v závislosti na tom, zda má parametr Id hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8616c-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="8616c-169">**Výpis 3 - CustomerController.cs**</span><span class="sxs-lookup"><span data-stu-id="8616c-169">**Listing 3 - CustomerController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

<span data-ttu-id="8616c-170">Výsledek akce ContentResult je speciální.</span><span class="sxs-lookup"><span data-stu-id="8616c-170">The ContentResult action result is special.</span></span> <span data-ttu-id="8616c-171">Výsledek akce ContentResult můžete použít k vrácení výsledku akce jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="8616c-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="8616c-172">Metoda Index() ve výpisu 4 například vrátí zprávu jako prostý text a ne jako HTML.</span><span class="sxs-lookup"><span data-stu-id="8616c-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="8616c-173">**Část 4 – Controllers\StatusController.cs**</span><span class="sxs-lookup"><span data-stu-id="8616c-173">**Listing 4 - Controllers\StatusController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

<span data-ttu-id="8616c-174">Při vyvolání akce StatusController.Index() zobrazení nevrátí.</span><span class="sxs-lookup"><span data-stu-id="8616c-174">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="8616c-175">Místo toho nezpracovaný text "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="8616c-175">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="8616c-176">se vrátí do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="8616c-176">is returned to the browser.</span></span>

<span data-ttu-id="8616c-177">Pokud akce kontroleru vrátí výsledek, který není výsledek akce – například datum nebo celé číslo – potom výsledek je zabalen ContentResult automaticky.</span><span class="sxs-lookup"><span data-stu-id="8616c-177">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="8616c-178">Například při vyvolání akce Index() WorkController výpis 5 datum se vrátí jako ContentResult automaticky.</span><span class="sxs-lookup"><span data-stu-id="8616c-178">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="8616c-179">**Výpis 5 - WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="8616c-179">**Listing 5 - WorkController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

<span data-ttu-id="8616c-180">Akce Index() ve výpisu 5 vrátí objekt DateTime.</span><span class="sxs-lookup"><span data-stu-id="8616c-180">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="8616c-181">Architektura ASP.NET MVC převede objekt DateTime na řetězec a automaticky zabalí ContentResult hodnotu data a času.</span><span class="sxs-lookup"><span data-stu-id="8616c-181">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="8616c-182">Prohlížeč obdrží data a času jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="8616c-182">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="8616c-183">Souhrn</span><span class="sxs-lookup"><span data-stu-id="8616c-183">Summary</span></span>

<span data-ttu-id="8616c-184">Účelem tohoto kurzu bylo seznámí s koncepty kontrolery ASP.NET MVC, akce kontroleru a výsledky akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8616c-184">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="8616c-185">V prvním oddílu jste zjistili, jak přidat nové řadiče do projektu aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8616c-185">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="8616c-186">Dále jste zjistili, jak veřejné metody řadiče jsou vystaveny rozhraní universe jako akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8616c-186">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="8616c-187">Nakonec jsme probírali různé druhy výsledky akcí, které mohou být vráceny z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8616c-187">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="8616c-188">Konkrétně jsme probírali jak vrátit ViewResult, RedirectToActionResult a ContentResult z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8616c-188">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8616c-189">[Předchozí](creating-an-action-vb.md)
> [další](creating-custom-routes-cs.md)</span><span class="sxs-lookup"><span data-stu-id="8616c-189">[Previous](creating-an-action-vb.md)
[Next](creating-custom-routes-cs.md)</span></span>
