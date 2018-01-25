---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: "Testování řadiče v rozhraní ASP.NET Web API 2 částí | Microsoft Docs"
author: MikeWasson
description: "Toto téma popisuje některé konkrétní postupy pro testování řadiče ve webovém rozhraní API 2 částí. Než si přečtete Toto téma, můžete chtít přečíst kurz jednotky..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bda5148a4c1553d70f3173de66371fbb8576e83f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="e290d-104">Testování řadiče v rozhraní ASP.NET Web API 2 částí</span><span class="sxs-lookup"><span data-stu-id="e290d-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="e290d-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e290d-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e290d-106">Toto téma popisuje některé konkrétní postupy pro testování řadiče ve webovém rozhraní API 2 částí.</span><span class="sxs-lookup"><span data-stu-id="e290d-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="e290d-107">Než si přečtete Toto téma, můžete chtít přečíst kurz [jednotky testování webové rozhraní API 2 ASP.NET](unit-testing-with-aspnet-web-api.md), který ukazuje, jak přidat projekt testování částí pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="e290d-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e290d-108">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="e290d-108">Software versions used in the tutorial</span></span>
> 
> - [<span data-ttu-id="e290d-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e290d-109">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="e290d-110">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="e290d-110">Web API 2</span></span>
> - <span data-ttu-id="e290d-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="e290d-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="e290d-112">Mohu použít Moq, ale na stejné nápad platí pro všechny mocking framework.</span><span class="sxs-lookup"><span data-stu-id="e290d-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="e290d-113">Moq 4.5.30 (a novější) podporuje Visual Studio 2017, Roslyn a rozhraní .NET 4.5 a novější verze.</span><span class="sxs-lookup"><span data-stu-id="e290d-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="e290d-114">Běžný vzor při testech jednotek je &quot;uspořádat, Application Compatibility Toolkit, assert&quot;:</span><span class="sxs-lookup"><span data-stu-id="e290d-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="e290d-115">Uspořádat: Připravit všechny předpoklady pro test ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="e290d-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="e290d-116">AKT: Proveďte test.</span><span class="sxs-lookup"><span data-stu-id="e290d-116">Act: Perform the test.</span></span>
- <span data-ttu-id="e290d-117">Assert –: Ověřte, zda test proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="e290d-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="e290d-118">V kroku uspořádat budou často používat model nebo objekty zóny se zakázaným inzerováním.</span><span class="sxs-lookup"><span data-stu-id="e290d-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="e290d-119">Test se zaměřuje na testování jednou z věcí, které minimalizuje počet závislostí.</span><span class="sxs-lookup"><span data-stu-id="e290d-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="e290d-120">Zde jsou některé věci, které byste měli testování částí v řadičích webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="e290d-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="e290d-121">Akce vrátí správný typ odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e290d-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="e290d-122">Neplatné parametry vrátí odpověď opravte chyby.</span><span class="sxs-lookup"><span data-stu-id="e290d-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="e290d-123">Akce volá metodu správné ve vrstvě úložiště nebo službě.</span><span class="sxs-lookup"><span data-stu-id="e290d-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="e290d-124">Pokud odpověď obsahuje modelu domény, zkontrolujte typ modelu.</span><span class="sxs-lookup"><span data-stu-id="e290d-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="e290d-125">Toto jsou některé obecné věcí, které chcete otestovat, ale jsou specifikace závisí na implementaci řadiče.</span><span class="sxs-lookup"><span data-stu-id="e290d-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="e290d-126">Zejména, zda vaše akce kontroleru vrátí umožňuje velký rozdíl **objekt HttpResponseMessage** nebo **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="e290d-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="e290d-127">Další informace o těchto typů výsledků najdete v tématu [výsledky akce ve webovém rozhraní Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="e290d-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="e290d-128">Testování akcí, které vracejí objekt HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="e290d-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="e290d-129">Tady je příklad řadiče jehož akce vrátí **objekt HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="e290d-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="e290d-130">Upozornění kontroleru pomocí vkládání závislostí vložit `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="e290d-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="e290d-131">Který zpřístupňuje řadičem více možností intenzivního testování, protože můžete vložit imitované úložiště.</span><span class="sxs-lookup"><span data-stu-id="e290d-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="e290d-132">Následující testování částí ověřuje, že `Get` metoda zápisy `Product` na text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e290d-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="e290d-133">Předpokládáme, že `repository` je model `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="e290d-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="e290d-134">Je důležité nastavit **požadavku** a **konfigurace** na řadiči.</span><span class="sxs-lookup"><span data-stu-id="e290d-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="e290d-135">V opačném případě, test se nezdaří s **ArgumentNullException** nebo **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="e290d-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="e290d-136">Testování generování odkazů</span><span class="sxs-lookup"><span data-stu-id="e290d-136">Testing Link Generation</span></span>

<span data-ttu-id="e290d-137">`Post` Volání metod **UrlHelper.Link** k vytváření propojení v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e290d-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="e290d-138">To vyžaduje trochu další instalační program v testování částí:</span><span class="sxs-lookup"><span data-stu-id="e290d-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="e290d-139">**UrlHelper** třída potřebuje data adresy URL a trasy na žádost, takže testovací má nastavit hodnoty pro tyto.</span><span class="sxs-lookup"><span data-stu-id="e290d-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="e290d-140">Další možností je model nebo se zakázaným inzerováním **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="e290d-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="e290d-141">S tímto přístupem, nahradí výchozí hodnotu [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) s model nebo se zakázaným inzerováním verzí, která vrátí pevnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e290d-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="e290d-142">Pojďme přepisování testu pomocí [Moq](https://github.com/Moq) framework.</span><span class="sxs-lookup"><span data-stu-id="e290d-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="e290d-143">Nainstalujte `Moq` balíček NuGet do projektu test.</span><span class="sxs-lookup"><span data-stu-id="e290d-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="e290d-144">V této verzi, nemusíte nastavit žádná data trasy, protože model **UrlHelper** vrátí konstantní řetězec.</span><span class="sxs-lookup"><span data-stu-id="e290d-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="e290d-145">Testování akcí, které vracejí IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="e290d-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="e290d-146">Ve webovém rozhraní API 2 akce kontroleru vrátit **IHttpActionResult**, což je obdobou **ActionResult** v architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e290d-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="e290d-147">**IHttpActionResult** rozhraní definuje příkaz vzor pro vytváření odpovědí HTTP.</span><span class="sxs-lookup"><span data-stu-id="e290d-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="e290d-148">Místo vytváření odpovědi přímo, kontroleru vrátí **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="e290d-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="e290d-149">Později, vyvolá kanál **IHttpActionResult** k vytvoření odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e290d-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="e290d-150">Tento přístup je snazší zápis testů jednotek, protože můžete přeskočit spoustu instalační program, který je potřebný pro **objekt HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="e290d-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="e290d-151">Tady je příklad controller jehož akce vrátí **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="e290d-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="e290d-152">Tento příklad ukazuje několik běžných vzorů pomocí **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="e290d-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="e290d-153">Podívejme se, jak k jednotce otestovat je.</span><span class="sxs-lookup"><span data-stu-id="e290d-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="e290d-154">Akce vrátí hodnotu 200 (OK) se text odpovědi</span><span class="sxs-lookup"><span data-stu-id="e290d-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="e290d-155">`Get` Volání metod `Ok(product)` Pokud je nalezena produktu.</span><span class="sxs-lookup"><span data-stu-id="e290d-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="e290d-156">Ujistěte se, je návratový typ v testu jednotek **OkNegotiatedContentResult** a vrácené výrobek má správné ID.</span><span class="sxs-lookup"><span data-stu-id="e290d-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="e290d-157">Všimněte si, že testování částí neprovede výsledku akce.</span><span class="sxs-lookup"><span data-stu-id="e290d-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="e290d-158">Můžete předpokládat, že výsledek akce správně vytvoří odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="e290d-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="e290d-159">(To je důvod, proč má svou vlastní testování částí rozhraní Web API!)</span><span class="sxs-lookup"><span data-stu-id="e290d-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="e290d-160">Akce vrátí 404 (Nenalezeno)</span><span class="sxs-lookup"><span data-stu-id="e290d-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="e290d-161">`Get` Volání metod `NotFound()` Pokud není nalezena produktu.</span><span class="sxs-lookup"><span data-stu-id="e290d-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="e290d-162">Jednotka pro tento případ, otestovat jenom kontroly, pokud je návratový typ **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="e290d-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="e290d-163">Akce vrátí hodnotu 200 (OK) se žádný text odpovědi</span><span class="sxs-lookup"><span data-stu-id="e290d-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="e290d-164">`Delete` Volání metod `Ok()` vrátit prázdnou odpověď HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="e290d-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="e290d-165">Podobně jako v předchozím příkladu testování částí kontroluje návratový typ v tomto případě **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="e290d-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="e290d-166">Akce vrátí 201 (vytvořeno) s hlavičku umístění</span><span class="sxs-lookup"><span data-stu-id="e290d-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="e290d-167">`Post` Volání metod `CreatedAtRoute` vrátit odpověď HTTP 201 s identifikátorem URI v hlavičce umístění.</span><span class="sxs-lookup"><span data-stu-id="e290d-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="e290d-168">V testu jednotek ověřte, že akce nastaví správné směrování hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e290d-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="e290d-169">Akce vrátí jiné 2xx s text odpovědi</span><span class="sxs-lookup"><span data-stu-id="e290d-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="e290d-170">`Put` Volání metod `Content` vrátit odpověď HTTP 202 (platných) s text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e290d-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="e290d-171">Tento případ se podobá vrací 200 (OK), ale testování částí byste taky měli zkontrolovat stavový kód.</span><span class="sxs-lookup"><span data-stu-id="e290d-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="e290d-172">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="e290d-172">Additional Resources</span></span>

- [<span data-ttu-id="e290d-173">Rozhraní Entity Framework mocking při rozhraní ASP.NET Web API 2 testování částí</span><span class="sxs-lookup"><span data-stu-id="e290d-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="e290d-174">[Zápis testů pro služby ASP.NET Web API](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (příspěvek na blogu podle Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="e290d-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="e290d-175">Ladění v rozhraní ASP.NET Web API pomocí ladicího programu trasy</span><span class="sxs-lookup"><span data-stu-id="e290d-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
