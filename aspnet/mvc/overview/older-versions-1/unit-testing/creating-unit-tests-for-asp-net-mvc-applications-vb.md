---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: "Vytváření testů jednotek pro aplikace ASP.NET MVC (VB) | Microsoft Docs"
author: StephenWalther
description: "Naučte se vytvářet testy částí pro akce kontroleru. V tomto kurzu Stephen Walther ukazuje, jak otestovat, zda akce kontroleru vrátí sloupce části..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: d92ee0c26787e5c482e8695001d8809d3ee9ee30
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a><span data-ttu-id="6e473-104">Vytváření testů jednotek pro aplikace ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="6e473-104">Creating Unit Tests for ASP.NET MVC Applications (VB)</span></span>
====================
<span data-ttu-id="6e473-105">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="6e473-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="6e473-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="6e473-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> <span data-ttu-id="6e473-107">Naučte se vytvářet testy částí pro akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6e473-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="6e473-108">V tomto kurzu Stephen Walther ukazuje, jak otestovat, zda akce kontroleru vrátí konkrétní zobrazení, vrátí sadu dat nebo vrátí jiný typ výsledku akce.</span><span class="sxs-lookup"><span data-stu-id="6e473-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="6e473-109">Cílem tohoto kurzu je ukázka, jak můžete psát testování částí pro řadiče v aplikaci ASP.NET MVC aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e473-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="6e473-110">Probereme, jak vytvořit tři různé typy testů jednotek.</span><span class="sxs-lookup"><span data-stu-id="6e473-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="6e473-111">Zjistíte, jak testovat zobrazení vrácené akce kontroleru, postup testování zobrazení dat vrácených akce kontroleru a jak otestovat, zda jeden akce kontroleru vás přesměruje na druhou akci kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6e473-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="6e473-112">Vytvoření řadiče testovaného</span><span class="sxs-lookup"><span data-stu-id="6e473-112">Creating the Controller under Test</span></span>

<span data-ttu-id="6e473-113">Začněme vytvořením kontroler, který jsme chcete otestovat.</span><span class="sxs-lookup"><span data-stu-id="6e473-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="6e473-114">Řadič, s názvem `ProductController`, je součástí výpis 1.</span><span class="sxs-lookup"><span data-stu-id="6e473-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="6e473-115">**Výpis 1 –`ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="6e473-115">**Listing 1 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

<span data-ttu-id="6e473-116">`ProductController` Obsahuje dvě metody akce s názvem `Index()` a `Details()`.</span><span class="sxs-lookup"><span data-stu-id="6e473-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="6e473-117">Obě metody akce vracejí zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6e473-117">Both action methods return a view.</span></span> <span data-ttu-id="6e473-118">Všimněte si, že `Details()` akce přijme parametr s názvem ID.</span><span class="sxs-lookup"><span data-stu-id="6e473-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="6e473-119">Testování zobrazení vrácený řadič</span><span class="sxs-lookup"><span data-stu-id="6e473-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="6e473-120">Představte si, že nám chcete otestovat, jestli `ProductController` vrátí pravého zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6e473-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="6e473-121">Chceme, abyste měli jistotu, že pokud `ProductController.Details()` akce je volána, je vrácen zobrazení podrobností.</span><span class="sxs-lookup"><span data-stu-id="6e473-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="6e473-122">Třída testu v výpis 2 obsahuje testů jednotek pro testování zobrazení vrácené `ProductController.Details()` akce.</span><span class="sxs-lookup"><span data-stu-id="6e473-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="6e473-123">**Výpis 2 –`ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="6e473-123">**Listing 2 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

<span data-ttu-id="6e473-124">Třída v výpis 2 zahrnuje testovací metodu s názvem `TestDetailsView()`.</span><span class="sxs-lookup"><span data-stu-id="6e473-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="6e473-125">Tato metoda obsahuje tři řádky kódu.</span><span class="sxs-lookup"><span data-stu-id="6e473-125">This method contains three lines of code.</span></span> <span data-ttu-id="6e473-126">První řádek kódu vytvoří novou instanci třídy `ProductController` třídy.</span><span class="sxs-lookup"><span data-stu-id="6e473-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="6e473-127">Druhý řádek kódu vyvolá kontroleru `Details()` metody akce.</span><span class="sxs-lookup"><span data-stu-id="6e473-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="6e473-128">Nakonec poslední řádek kódu kontroly, jestli zobrazení vrácené `Details()` zobrazení podrobností není akce.</span><span class="sxs-lookup"><span data-stu-id="6e473-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="6e473-129">`ViewResult.ViewName` Vlastnost představuje název zobrazení, které vrácený řadič.</span><span class="sxs-lookup"><span data-stu-id="6e473-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="6e473-130">Jeden velký upozornění o testování tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6e473-130">One big warning about testing this property.</span></span> <span data-ttu-id="6e473-131">Existují dva způsoby, že řadič můžete vrátit zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6e473-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="6e473-132">Řadič explicitně vrátit zobrazení takto:</span><span class="sxs-lookup"><span data-stu-id="6e473-132">A controller can explicitly return a view like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

<span data-ttu-id="6e473-133">Název zobrazení Alternativně lze odvodit z názvu akce kontroleru takto:</span><span class="sxs-lookup"><span data-stu-id="6e473-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

<span data-ttu-id="6e473-134">Tato akce kontroleru vrátí také zobrazení s názvem `Details`.</span><span class="sxs-lookup"><span data-stu-id="6e473-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="6e473-135">Název zobrazení, je však odvodit z název akce.</span><span class="sxs-lookup"><span data-stu-id="6e473-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="6e473-136">Pokud chcete testovat název zobrazení, pak musí explicitně vrátíte název zobrazení z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6e473-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="6e473-137">Testování částí v výpis 2 můžete spustit tak, že zadáte kombinace kláves **Ctrl-R, A** nebo kliknutím **spustit všechny testy v řešení** tlačítko (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="6e473-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="6e473-138">Pokud testovací úspěšně projde, se zobrazí okno výsledky testu na obrázku 2.</span><span class="sxs-lookup"><span data-stu-id="6e473-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="6e473-139">[![Spustit všechny testy v řešení](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6e473-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span></span>

<span data-ttu-id="6e473-140">**Obrázek 01**: spustit všechny testy v řešení ([Kliknutím zobrazit obrázek v plné velikosti](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6e473-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span></span>


<span data-ttu-id="6e473-141">[![Úspěšné!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="6e473-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span></span>

<span data-ttu-id="6e473-142">**Obrázek 02**: Úspěch!</span><span class="sxs-lookup"><span data-stu-id="6e473-142">**Figure 02**: Success!</span></span> <span data-ttu-id="6e473-143">([Kliknutím zobrazit obrázek v plné velikosti](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6e473-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="6e473-144">Testování zobrazení dat vrácených řadič</span><span class="sxs-lookup"><span data-stu-id="6e473-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="6e473-145">Kontroler MVC předá dat zobrazení pomocí takzvaný  *`View Data`* .</span><span class="sxs-lookup"><span data-stu-id="6e473-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="6e473-146">Představte si například, že chcete zobrazit podrobnosti pro konkrétní produkt při vyvolání `ProductController Details()` akce.</span><span class="sxs-lookup"><span data-stu-id="6e473-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="6e473-147">V takovém případě můžete vytvořit instanci `Product` – třída (definovanou v modelu) a předejte instanci systému na `Details` zobrazení využitím `View Data`.</span><span class="sxs-lookup"><span data-stu-id="6e473-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="6e473-148">Upravenou `ProductController` ve výpisu 3 zahrnuje aktualizované `Details()` akce, která vrací produktu.</span><span class="sxs-lookup"><span data-stu-id="6e473-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="6e473-149">**Výpis 3 –`ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="6e473-149">**Listing 3 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

<span data-ttu-id="6e473-150">Nejdřív `Details()` akce vytvoří novou instanci třídy `Product` třídu, která představuje přenosný počítač.</span><span class="sxs-lookup"><span data-stu-id="6e473-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="6e473-151">Další, instanci systému `Product` třída je předán jako druhý parametr `View()` metoda.</span><span class="sxs-lookup"><span data-stu-id="6e473-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="6e473-152">Můžete napsat testy jednotek a otestovat, jestli je očekávaná data obsažená v zobrazení data.</span><span class="sxs-lookup"><span data-stu-id="6e473-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="6e473-153">Jednotkové testování v testech výpis 4 zda produkt představující přenosný počítač je vrácena v případě, že zavoláte `ProductController Details()` metody akce.</span><span class="sxs-lookup"><span data-stu-id="6e473-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="6e473-154">**Výpis 4 –`ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="6e473-154">**Listing 4 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

<span data-ttu-id="6e473-155">Výpis 4 `TestDetailsView()` metoda otestováním zobrazení dat vrácených vyvoláním `Details()` metoda.</span><span class="sxs-lookup"><span data-stu-id="6e473-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="6e473-156">`ViewData` Je k dispozici jako vlastnost na `ViewResult` vrácený vyvolání `Details()` metoda.</span><span class="sxs-lookup"><span data-stu-id="6e473-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="6e473-157">`ViewData.Model` Vlastnost obsahuje produktu předaná do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6e473-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="6e473-158">Test jednoduše ověřuje, že produkt obsažené v zobrazení dat má název přenosný počítač.</span><span class="sxs-lookup"><span data-stu-id="6e473-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="6e473-159">Testování výsledek akce vrácené řadič</span><span class="sxs-lookup"><span data-stu-id="6e473-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="6e473-160">Složitější akce kontroleru může vracet různé typy výsledků akcí v závislosti na hodnoty parametrů předaných pracovnímu akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6e473-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="6e473-161">Akce kontroleru vrátit celou řadu typů výsledky akce včetně `ViewResult`, `RedirectToRouteResult`, nebo `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="6e473-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="6e473-162">Například upravené `Details()` vrátí akce v výpis 5 `Details` zobrazit při předání platný produkt Id akci.</span><span class="sxs-lookup"><span data-stu-id="6e473-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="6e473-163">Pokud předáte o neplatný produkt Id – Id s hodnotou, menší než 1 – pak budete přesměrováni `Index()` akce.</span><span class="sxs-lookup"><span data-stu-id="6e473-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="6e473-164">**Výpis 5 –`ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="6e473-164">**Listing 5 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

<span data-ttu-id="6e473-165">Můžete otestovat chování `Details()` akce s testování částí v výpis 6.</span><span class="sxs-lookup"><span data-stu-id="6e473-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="6e473-166">Testování částí v výpis 6 ověřuje, že budete přesměrováni na `Index` zobrazit, když je předána Id s hodnotou -1 `Details()` metoda.</span><span class="sxs-lookup"><span data-stu-id="6e473-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="6e473-167">**Výpis 6 –`ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="6e473-167">**Listing 6 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

<span data-ttu-id="6e473-168">Při volání `RedirectToAction()` metoda akce kontroleru akce kontroleru vrátí `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="6e473-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="6e473-169">Test kontroluje jestli `RedirectToRouteResult` přesměruje uživatele na akce kontroleru s názvem `Index`.</span><span class="sxs-lookup"><span data-stu-id="6e473-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="6e473-170">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6e473-170">Summary</span></span>

<span data-ttu-id="6e473-171">V tomto kurzu jste se naučili vytvářet testy částí pro akce kontroleru MVC.</span><span class="sxs-lookup"><span data-stu-id="6e473-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="6e473-172">Nejdříve jste zjistili, jak ověřit, zda akce kontroleru vrátí pravého zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6e473-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="6e473-173">Jste se dozvěděli, jak používat `ViewResult.ViewName` vlastnost ověřte název zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6e473-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="6e473-174">V dalším kroku jsme se zaměřili na tom, jak můžete otestovat obsah `View Data`.</span><span class="sxs-lookup"><span data-stu-id="6e473-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="6e473-175">Jste se dozvěděli, jak zkontrolovat, zda je správný produkt vrátil v `View Data` po volání metody akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6e473-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="6e473-176">Nakonec jsme probrali, jak můžete zkontrolovat, zda jsou různé typy výsledků akce vrácené z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6e473-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="6e473-177">Jste zjistili, jak k ověření, zda řadič vrátí `ViewResult` nebo `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="6e473-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="6e473-178">Předchozí</span><span class="sxs-lookup"><span data-stu-id="6e473-178">Previous</span></span>](creating-unit-tests-for-asp-net-mvc-applications-cs.md)