---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: Vytváření testů jednotek pro aplikace ASP.NET MVC (C#) | Microsoft Docs
author: StephenWalther
description: Naučte se vytvářet testy částí pro akce kontroleru. V tomto kurzu Stephen Walther ukazuje, jak otestovat, zda akce kontroleru vrátí sloupce části...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: ccd9a1b3aee8379c23c01c5eb7f756a786f6359d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869709"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a><span data-ttu-id="15f72-104">Vytváření testů jednotek pro aplikace ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="15f72-104">Creating Unit Tests for ASP.NET MVC Applications (C#)</span></span>
====================
<span data-ttu-id="15f72-105">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="15f72-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="15f72-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="15f72-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> <span data-ttu-id="15f72-107">Naučte se vytvářet testy částí pro akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="15f72-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="15f72-108">V tomto kurzu Stephen Walther ukazuje, jak otestovat, zda akce kontroleru vrátí konkrétní zobrazení, vrátí sadu dat nebo vrátí jiný typ výsledku akce.</span><span class="sxs-lookup"><span data-stu-id="15f72-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="15f72-109">Cílem tohoto kurzu je ukázka, jak můžete psát testování částí pro řadiče v aplikaci ASP.NET MVC aplikace.</span><span class="sxs-lookup"><span data-stu-id="15f72-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="15f72-110">Probereme, jak vytvořit tři různé typy testů jednotek.</span><span class="sxs-lookup"><span data-stu-id="15f72-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="15f72-111">Zjistíte, jak testovat zobrazení vrácené akce kontroleru, postup testování zobrazení dat vrácených akce kontroleru a jak otestovat, zda jeden akce kontroleru vás přesměruje na druhou akci kontroleru.</span><span class="sxs-lookup"><span data-stu-id="15f72-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="15f72-112">Vytvoření řadiče testovaného</span><span class="sxs-lookup"><span data-stu-id="15f72-112">Creating the Controller under Test</span></span>

<span data-ttu-id="15f72-113">Začněme vytvořením kontroler, který jsme chcete otestovat.</span><span class="sxs-lookup"><span data-stu-id="15f72-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="15f72-114">Řadič, s názvem `ProductController`, je součástí výpis 1.</span><span class="sxs-lookup"><span data-stu-id="15f72-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="15f72-115">**Výpis 1 – `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="15f72-115">**Listing 1 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

<span data-ttu-id="15f72-116">`ProductController` Obsahuje dvě metody akce s názvem `Index()` a `Details()`.</span><span class="sxs-lookup"><span data-stu-id="15f72-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="15f72-117">Obě metody akce vracejí zobrazení.</span><span class="sxs-lookup"><span data-stu-id="15f72-117">Both action methods return a view.</span></span> <span data-ttu-id="15f72-118">Všimněte si, že `Details()` akce přijme parametr s názvem ID.</span><span class="sxs-lookup"><span data-stu-id="15f72-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="15f72-119">Testování zobrazení vrácený řadič</span><span class="sxs-lookup"><span data-stu-id="15f72-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="15f72-120">Představte si, že nám chcete otestovat, jestli `ProductController` vrátí pravého zobrazení.</span><span class="sxs-lookup"><span data-stu-id="15f72-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="15f72-121">Chceme, abyste měli jistotu, že pokud `ProductController.Details()` akce je volána, je vrácen zobrazení podrobností.</span><span class="sxs-lookup"><span data-stu-id="15f72-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="15f72-122">Třída testu v výpis 2 obsahuje testů jednotek pro testování zobrazení vrácené `ProductController.Details()` akce.</span><span class="sxs-lookup"><span data-stu-id="15f72-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="15f72-123">**Výpis 2 – `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="15f72-123">**Listing 2 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

<span data-ttu-id="15f72-124">Třída v výpis 2 zahrnuje testovací metodu s názvem `TestDetailsView()`.</span><span class="sxs-lookup"><span data-stu-id="15f72-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="15f72-125">Tato metoda obsahuje tři řádky kódu.</span><span class="sxs-lookup"><span data-stu-id="15f72-125">This method contains three lines of code.</span></span> <span data-ttu-id="15f72-126">První řádek kódu vytvoří novou instanci třídy `ProductController` třídy.</span><span class="sxs-lookup"><span data-stu-id="15f72-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="15f72-127">Druhý řádek kódu vyvolá kontroleru `Details()` metody akce.</span><span class="sxs-lookup"><span data-stu-id="15f72-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="15f72-128">Nakonec poslední řádek kódu kontroly, jestli zobrazení vrácené `Details()` zobrazení podrobností není akce.</span><span class="sxs-lookup"><span data-stu-id="15f72-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="15f72-129">`ViewResult.ViewName` Vlastnost představuje název zobrazení, které vrácený řadič.</span><span class="sxs-lookup"><span data-stu-id="15f72-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="15f72-130">Jeden velký upozornění o testování tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="15f72-130">One big warning about testing this property.</span></span> <span data-ttu-id="15f72-131">Existují dva způsoby, že řadič můžete vrátit zobrazení.</span><span class="sxs-lookup"><span data-stu-id="15f72-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="15f72-132">Řadič explicitně vrátit zobrazení takto:</span><span class="sxs-lookup"><span data-stu-id="15f72-132">A controller can explicitly return a view like this:</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

<span data-ttu-id="15f72-133">Název zobrazení Alternativně lze odvodit z názvu akce kontroleru takto:</span><span class="sxs-lookup"><span data-stu-id="15f72-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

<span data-ttu-id="15f72-134">Tato akce kontroleru vrátí také zobrazení s názvem `Details`.</span><span class="sxs-lookup"><span data-stu-id="15f72-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="15f72-135">Název zobrazení, je však odvodit z název akce.</span><span class="sxs-lookup"><span data-stu-id="15f72-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="15f72-136">Pokud chcete testovat název zobrazení, pak musí explicitně vrátíte název zobrazení z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="15f72-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="15f72-137">Testování částí v výpis 2 můžete spustit tak, že zadáte kombinace kláves **Ctrl-R, A** nebo kliknutím **spustit všechny testy v řešení** tlačítko (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="15f72-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="15f72-138">Pokud testovací úspěšně projde, se zobrazí okno výsledky testu na obrázku 2.</span><span class="sxs-lookup"><span data-stu-id="15f72-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="15f72-139">[![Spustit všechny testy v řešení](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="15f72-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)</span></span>

<span data-ttu-id="15f72-140">**Obrázek 01**: spustit všechny testy v řešení ([Kliknutím zobrazit obrázek v plné velikosti](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="15f72-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))</span></span>


<span data-ttu-id="15f72-141">[![Úspěšné!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="15f72-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)</span></span>

<span data-ttu-id="15f72-142">**Obrázek 02**: Úspěch!</span><span class="sxs-lookup"><span data-stu-id="15f72-142">**Figure 02**: Success!</span></span> <span data-ttu-id="15f72-143">([Kliknutím zobrazit obrázek v plné velikosti](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="15f72-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="15f72-144">Testování zobrazení dat vrácených řadič</span><span class="sxs-lookup"><span data-stu-id="15f72-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="15f72-145">Kontroler MVC předá dat zobrazení pomocí takzvaný *`View Data`*.</span><span class="sxs-lookup"><span data-stu-id="15f72-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="15f72-146">Představte si například, že chcete zobrazit podrobnosti pro konkrétní produkt při vyvolání `ProductController Details()` akce.</span><span class="sxs-lookup"><span data-stu-id="15f72-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="15f72-147">V takovém případě můžete vytvořit instanci `Product` – třída (definovanou v modelu) a předejte instanci systému na `Details` zobrazení využitím `View Data`.</span><span class="sxs-lookup"><span data-stu-id="15f72-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="15f72-148">Upravenou `ProductController` ve výpisu 3 zahrnuje aktualizované `Details()` akce, která vrací produktu.</span><span class="sxs-lookup"><span data-stu-id="15f72-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="15f72-149">**Výpis 3 – `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="15f72-149">**Listing 3 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

<span data-ttu-id="15f72-150">Nejdřív `Details()` akce vytvoří novou instanci třídy `Product` třídu, která představuje přenosný počítač.</span><span class="sxs-lookup"><span data-stu-id="15f72-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="15f72-151">Další, instanci systému `Product` třída je předán jako druhý parametr `View()` metoda.</span><span class="sxs-lookup"><span data-stu-id="15f72-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="15f72-152">Můžete napsat testy jednotek a otestovat, jestli je očekávaná data obsažená v zobrazení data.</span><span class="sxs-lookup"><span data-stu-id="15f72-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="15f72-153">Jednotkové testování v testech výpis 4 zda produkt představující přenosný počítač je vrácena v případě, že zavoláte `ProductController Details()` metody akce.</span><span class="sxs-lookup"><span data-stu-id="15f72-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="15f72-154">**Výpis 4 – `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="15f72-154">**Listing 4 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

<span data-ttu-id="15f72-155">Výpis 4 `TestDetailsView()` metoda otestováním zobrazení dat vrácených vyvoláním `Details()` metoda.</span><span class="sxs-lookup"><span data-stu-id="15f72-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="15f72-156">`ViewData` Je k dispozici jako vlastnost na `ViewResult` vrácený vyvolání `Details()` metoda.</span><span class="sxs-lookup"><span data-stu-id="15f72-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="15f72-157">`ViewData.Model` Vlastnost obsahuje produktu předaná do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="15f72-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="15f72-158">Test jednoduše ověřuje, že produkt obsažené v zobrazení dat má název přenosný počítač.</span><span class="sxs-lookup"><span data-stu-id="15f72-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="15f72-159">Testování výsledek akce vrácené řadič</span><span class="sxs-lookup"><span data-stu-id="15f72-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="15f72-160">Složitější akce kontroleru může vracet různé typy výsledků akcí v závislosti na hodnoty parametrů předaných pracovnímu akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="15f72-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="15f72-161">Akce kontroleru vrátit celou řadu typů výsledky akce včetně `ViewResult`, `RedirectToRouteResult`, nebo `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="15f72-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="15f72-162">Například upravené `Details()` vrátí akce v výpis 5 `Details` zobrazit při předání platný produkt Id akci.</span><span class="sxs-lookup"><span data-stu-id="15f72-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="15f72-163">Pokud předáte o neplatný produkt Id – Id s hodnotou, menší než 1 – pak budete přesměrováni `Index()` akce.</span><span class="sxs-lookup"><span data-stu-id="15f72-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="15f72-164">**Výpis 5 – `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="15f72-164">**Listing 5 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

<span data-ttu-id="15f72-165">Můžete otestovat chování `Details()` akce s testování částí v výpis 6.</span><span class="sxs-lookup"><span data-stu-id="15f72-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="15f72-166">Testování částí v výpis 6 ověřuje, že budete přesměrováni na `Index` zobrazit, když je předána Id s hodnotou -1 `Details()` metoda.</span><span class="sxs-lookup"><span data-stu-id="15f72-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="15f72-167">**Výpis 6 – `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="15f72-167">**Listing 6 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

<span data-ttu-id="15f72-168">Při volání `RedirectToAction()` metoda akce kontroleru akce kontroleru vrátí `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="15f72-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="15f72-169">Test kontroluje jestli `RedirectToRouteResult` přesměruje uživatele na akce kontroleru s názvem `Index`.</span><span class="sxs-lookup"><span data-stu-id="15f72-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="15f72-170">Souhrn</span><span class="sxs-lookup"><span data-stu-id="15f72-170">Summary</span></span>

<span data-ttu-id="15f72-171">V tomto kurzu jste se naučili vytvářet testy částí pro akce kontroleru MVC.</span><span class="sxs-lookup"><span data-stu-id="15f72-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="15f72-172">Nejdříve jste zjistili, jak ověřit, zda akce kontroleru vrátí pravého zobrazení.</span><span class="sxs-lookup"><span data-stu-id="15f72-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="15f72-173">Jste se dozvěděli, jak používat `ViewResult.ViewName` vlastnost ověřte název zobrazení.</span><span class="sxs-lookup"><span data-stu-id="15f72-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="15f72-174">V dalším kroku jsme se zaměřili na tom, jak můžete otestovat obsah `View Data`.</span><span class="sxs-lookup"><span data-stu-id="15f72-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="15f72-175">Jste se dozvěděli, jak zkontrolovat, zda je správný produkt vrátil v `View Data` po volání metody akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="15f72-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="15f72-176">Nakonec jsme probrali, jak můžete zkontrolovat, zda jsou různé typy výsledků akce vrácené z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="15f72-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="15f72-177">Jste zjistili, jak k ověření, zda řadič vrátí `ViewResult` nebo `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="15f72-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="15f72-178">Next</span><span class="sxs-lookup"><span data-stu-id="15f72-178">Next</span></span>](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
