---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Testování jednotek Kontrolerů v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: Toto téma popisuje některé konkrétní postupy pro testování jednotek kontrolerů webového rozhraní API 2. Před čtením tohoto tématu, můžete chtít přečtěte si kurz jednotky...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7b0d5266757219a05b25fc3d1d4cba8514a4dff7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753260"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="1eb83-104">Testování jednotek Kontrolerů v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="1eb83-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="1eb83-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1eb83-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="1eb83-106">Toto téma popisuje některé konkrétní postupy pro testování jednotek kontrolerů webového rozhraní API 2.</span><span class="sxs-lookup"><span data-stu-id="1eb83-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="1eb83-107">Před čtením tohoto tématu, můžete chtít přečtěte si kurz [jednotky testování ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), který ukazuje, jak přidat projekt testování částí do vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="1eb83-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1eb83-108">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="1eb83-108">Software versions used in the tutorial</span></span>
> 
> - [<span data-ttu-id="1eb83-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1eb83-109">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="1eb83-110">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="1eb83-110">Web API 2</span></span>
> - <span data-ttu-id="1eb83-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="1eb83-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="1eb83-112">Můžu použít Moq, ale platí stejné nápad pro libovolné napodobování architektury.</span><span class="sxs-lookup"><span data-stu-id="1eb83-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="1eb83-113">Moq 4.5.30 (a vyšší) podporuje Visual Studio 2017, Roslyn a .NET 4.5 a novější verze.</span><span class="sxs-lookup"><span data-stu-id="1eb83-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="1eb83-114">Běžným vzorem při testech jednotek je &quot;uspořádat, act, vyhodnocení&quot;:</span><span class="sxs-lookup"><span data-stu-id="1eb83-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="1eb83-115">Uspořádat: Nastavte všechny požadované součásti pro test spuštěn.</span><span class="sxs-lookup"><span data-stu-id="1eb83-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="1eb83-116">ACT: Provádění testu.</span><span class="sxs-lookup"><span data-stu-id="1eb83-116">Act: Perform the test.</span></span>
- <span data-ttu-id="1eb83-117">Vyhodnocení: Ověřte, že test bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="1eb83-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="1eb83-118">V kroku uspořádat budou často používat model nebo zástupné objekty.</span><span class="sxs-lookup"><span data-stu-id="1eb83-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="1eb83-119">Které minimalizuje počet závislostí, takže test se zaměřuje na testování jednou z věcí.</span><span class="sxs-lookup"><span data-stu-id="1eb83-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="1eb83-120">Zde jsou některé kroky ve vašich kontrolerech webového rozhraní API by měl test jednotek:</span><span class="sxs-lookup"><span data-stu-id="1eb83-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="1eb83-121">Tato akce vrátí správný typ odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1eb83-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="1eb83-122">Neplatné parametry vrátit odpověď chyby opravte.</span><span class="sxs-lookup"><span data-stu-id="1eb83-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="1eb83-123">Akce volá metodu správné ve vrstvě úložiště nebo službě.</span><span class="sxs-lookup"><span data-stu-id="1eb83-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="1eb83-124">Pokud odpověď obsahuje model domény, zkontrolujte typ modelu.</span><span class="sxs-lookup"><span data-stu-id="1eb83-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="1eb83-125">Toto jsou některé obecné možnosti testování, ale specifika závisí na vaší implementace kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1eb83-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="1eb83-126">Zejména je velký rozdíl, jestli vaše akce kontroleru vrátit **objekt HttpResponseMessage** nebo **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="1eb83-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="1eb83-127">Další informace o těchto typů výsledků najdete v tématu [výsledky akcí ve webovém rozhraní Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="1eb83-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="1eb83-128">Testování akcí, které vracejí objekt HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="1eb83-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="1eb83-129">Tady je příklad řadiče jehož akcí vrácení **objekt HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="1eb83-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="1eb83-130">Všimněte si, že kontroler používá injektáž závislostí vkládat `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="1eb83-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="1eb83-131">Díky tomu se kontroler více možností intenzivního testování, protože můžete vložit mock úložiště.</span><span class="sxs-lookup"><span data-stu-id="1eb83-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="1eb83-132">Následující test jednotky ověřuje, že `Get` metoda zápisy `Product` do těla odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1eb83-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="1eb83-133">Předpokládejme, že `repository` je model `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="1eb83-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="1eb83-134">Je důležité nastavit **žádosti** a **konfigurace** na kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1eb83-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="1eb83-135">V opačném případě test se nezdaří pomocí **ArgumentNullException** nebo **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="1eb83-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="1eb83-136">Testování generování odkazů</span><span class="sxs-lookup"><span data-stu-id="1eb83-136">Testing Link Generation</span></span>

<span data-ttu-id="1eb83-137">`Post` Volání metody **UrlHelper.Link** k vytvoření odkazů v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1eb83-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="1eb83-138">To vyžaduje trochu další nastavení v testu jednotek:</span><span class="sxs-lookup"><span data-stu-id="1eb83-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="1eb83-139">**UrlHelper** třídy musí žádost o adresu URL a data trasy, aby test má k nastavení hodnot pro tyto.</span><span class="sxs-lookup"><span data-stu-id="1eb83-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="1eb83-140">Další možností je model nebo zástupné procedury **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="1eb83-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="1eb83-141">S tímto přístupem nahradíte na výchozí hodnotu [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) s verzí model nebo zástupné procedury, která vrací pevná hodnota.</span><span class="sxs-lookup"><span data-stu-id="1eb83-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="1eb83-142">Umožňuje přepsat pomocí testu [Moq](https://github.com/Moq) rozhraní framework.</span><span class="sxs-lookup"><span data-stu-id="1eb83-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="1eb83-143">Nainstalujte `Moq` NuGet balíčku v testovém projektu.</span><span class="sxs-lookup"><span data-stu-id="1eb83-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="1eb83-144">V této verzi není nutné nastavit všechna data trasy, protože model **UrlHelper** vrátí konstanty typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="1eb83-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="1eb83-145">Testování akcí, které vracejí IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="1eb83-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="1eb83-146">Ve webovém rozhraní API 2, akce kontroleru vrátit **IHttpActionResult**, což je obdobou **ActionResult** v architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1eb83-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="1eb83-147">**IHttpActionResult** rozhraní definuje vzor příkaz pro vytvoření odpovědi protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1eb83-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="1eb83-148">Místo vytváření odpověď přímo, kontroler vrací **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="1eb83-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="1eb83-149">Později, kanál vyvolá **IHttpActionResult** k vytvoření odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1eb83-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="1eb83-150">Tento přístup usnadňuje pro psaní jednotkových testů, protože můžete přeskočit spoustu instalační program, který je nezbytný pro **objekt HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="1eb83-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="1eb83-151">Tady je příklad controller jehož akcí vrácení **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="1eb83-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="1eb83-152">Tento příklad ukazuje některé běžné vzory pomocí **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="1eb83-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="1eb83-153">Podívejme se, jak k jednotce pro jejich testování.</span><span class="sxs-lookup"><span data-stu-id="1eb83-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="1eb83-154">Akce vrátí 200 (OK) v textu odpovědi</span><span class="sxs-lookup"><span data-stu-id="1eb83-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="1eb83-155">`Get` Volání metody `Ok(product)` Pokud je nalezen produkt.</span><span class="sxs-lookup"><span data-stu-id="1eb83-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="1eb83-156">V testu jednotek, ujistěte se, že je návratový typ **OkNegotiatedContentResult** a vrácené produktu má správný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="1eb83-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="1eb83-157">Všimněte si, že Jednotkový test neprovede výsledku akce.</span><span class="sxs-lookup"><span data-stu-id="1eb83-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="1eb83-158">Můžete předpokládat, že výsledek akce správně vytvoří odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="1eb83-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="1eb83-159">(To je důvod, proč rozhraní webového rozhraní API má svůj vlastní testování částí!)</span><span class="sxs-lookup"><span data-stu-id="1eb83-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="1eb83-160">Akce vrátí 404 (Nenalezeno)</span><span class="sxs-lookup"><span data-stu-id="1eb83-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="1eb83-161">`Get` Volání metody `NotFound()` Pokud není nalezen produkt.</span><span class="sxs-lookup"><span data-stu-id="1eb83-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="1eb83-162">Pro tento případ, test jednotky jenom kontroly, pokud je návratový typ **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="1eb83-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="1eb83-163">Akce vrátí 200 (OK) s žádnou odpovědí</span><span class="sxs-lookup"><span data-stu-id="1eb83-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="1eb83-164">`Delete` Volání metody `Ok()` vrátit prázdnou odpověď HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="1eb83-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="1eb83-165">Podobně jako v předchozím příkladu, testování částí kontroluje návratovým typem, v tomto případě **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="1eb83-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="1eb83-166">Akce vrátí 201 (vytvořeno) se hlavička umístění</span><span class="sxs-lookup"><span data-stu-id="1eb83-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="1eb83-167">`Post` Volání metody `CreatedAtRoute` vrátit odpověď HTTP 201 s identifikátorem URI v hlavičce umístění.</span><span class="sxs-lookup"><span data-stu-id="1eb83-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="1eb83-168">V testu jednotek ověřte, že tato akce nastaví správné směrování hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1eb83-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="1eb83-169">Akce vrátí jiný 2xx s tělo odpovědi</span><span class="sxs-lookup"><span data-stu-id="1eb83-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="1eb83-170">`Put` Volání metody `Content` vrátit odpověď HTTP 202 (přijato) s tělo odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1eb83-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="1eb83-171">Tento případ se podobá vrací 200 (OK), ale testování částí byste taky měli zkontrolovat stavový kód.</span><span class="sxs-lookup"><span data-stu-id="1eb83-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="1eb83-172">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="1eb83-172">Additional Resources</span></span>

- [<span data-ttu-id="1eb83-173">Vytvoření modelu Entity Framework při testování rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="1eb83-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="1eb83-174">[Zápis testů pro rozhraní ASP.NET Web API službu](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (příspěvek na blogu od Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="1eb83-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="1eb83-175">Ladění v rozhraní ASP.NET Web API s ladicím programem trasy</span><span class="sxs-lookup"><span data-stu-id="1eb83-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
