---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: "Akce výsledků v rozhraní Web API 2 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: d0db5c6d45020861d7295ab1db989caee525fff9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="action-results-in-web-api-2"></a><span data-ttu-id="2e5ce-102">Výsledky akce v rozhraní Web API 2</span><span class="sxs-lookup"><span data-stu-id="2e5ce-102">Action Results in Web API 2</span></span>
====================
<span data-ttu-id="2e5ce-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2e5ce-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="2e5ce-104">Toto téma popisuje, jak rozhraní ASP.NET Web API převádí návratovou hodnotu z akce kontroleru do zprávy odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="2e5ce-105">Akce kontroleru webového rozhraní API se můžete vrátit některé z následujících:</span><span class="sxs-lookup"><span data-stu-id="2e5ce-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="2e5ce-106">void</span><span class="sxs-lookup"><span data-stu-id="2e5ce-106">void</span></span>
2. <span data-ttu-id="2e5ce-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="2e5ce-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="2e5ce-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="2e5ce-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="2e5ce-109">Jiný typ</span><span class="sxs-lookup"><span data-stu-id="2e5ce-109">Some other type</span></span>

<span data-ttu-id="2e5ce-110">V závislosti na tom, které z nich se vrátí, webového rozhraní API používá jiný mechanismus k vytvoření odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="2e5ce-111">Návratový typ</span><span class="sxs-lookup"><span data-stu-id="2e5ce-111">Return type</span></span> | <span data-ttu-id="2e5ce-112">Jak webového rozhraní API vytvoří odpovědi</span><span class="sxs-lookup"><span data-stu-id="2e5ce-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="2e5ce-113">void</span><span class="sxs-lookup"><span data-stu-id="2e5ce-113">void</span></span> | <span data-ttu-id="2e5ce-114">Vrátí prázdný 204 (ne obsahu)</span><span class="sxs-lookup"><span data-stu-id="2e5ce-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="2e5ce-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="2e5ce-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="2e5ce-116">Převeďte přímo na zprávu odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="2e5ce-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="2e5ce-117">**IHttpActionResult**</span></span> | <span data-ttu-id="2e5ce-118">Volání **ExecuteAsync** vytvořit **objekt HttpResponseMessage**, pak převést na zprávu odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="2e5ce-119">Jiný typ</span><span class="sxs-lookup"><span data-stu-id="2e5ce-119">Other type</span></span> | <span data-ttu-id="2e5ce-120">Zápis serializovaných návratovou hodnotu do odpovědi; Vrátí 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="2e5ce-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="2e5ce-121">Zbývající část tohoto tématu popisuje jednotlivé možnosti podrobněji.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="2e5ce-122">void</span><span class="sxs-lookup"><span data-stu-id="2e5ce-122">void</span></span>

<span data-ttu-id="2e5ce-123">Pokud je návratový typ `void`, webového rozhraní API jednoduše vrátí prázdnou odpověď HTTP se stavovým kódem 204 (ne obsahu).</span><span class="sxs-lookup"><span data-stu-id="2e5ce-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="2e5ce-124">Příklad řadič:</span><span class="sxs-lookup"><span data-stu-id="2e5ce-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="2e5ce-125">Odpověď HTTP:</span><span class="sxs-lookup"><span data-stu-id="2e5ce-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="2e5ce-126">Objekt HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="2e5ce-126">HttpResponseMessage</span></span>

<span data-ttu-id="2e5ce-127">Akce vrátí-li [objekt HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), webového rozhraní API převede návratovou hodnotu přímo do zprávu odpovědi HTTP pomocí vlastnosti **objekt HttpResponseMessage** objektu k naplnění odpověď.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="2e5ce-128">Tato možnost vám přináší značnou kontroly nad zprávu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="2e5ce-129">Například následující akce kontroleru nastaví hlavičku Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="2e5ce-130">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="2e5ce-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="2e5ce-131">Pokud předáte modelu domény k **CreateResponse** metoda, používá webového rozhraní API [formátovací modul média](../formats-and-model-binding/media-formatters.md) k zápisu serializovaného modelu do text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="2e5ce-132">Webové rozhraní API pomocí hlavička Accept v požadavku formátování.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="2e5ce-133">Další informace najdete v tématu [vyjednávání obsahu](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="2e5ce-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="2e5ce-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="2e5ce-134">IHttpActionResult</span></span>

<span data-ttu-id="2e5ce-135">**IHttpActionResult** rozhraní byla zavedena ve webovém rozhraní API 2.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="2e5ce-136">V podstatě definuje **objekt HttpResponseMessage** objekt pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="2e5ce-137">Tady jsou některé výhody používání **IHttpActionResult** rozhraní:</span><span class="sxs-lookup"><span data-stu-id="2e5ce-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="2e5ce-138">Zjednodušuje [testování částí](../testing-and-debugging/unit-testing-controllers-in-web-api.md) řadičů.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="2e5ce-139">Přesune běžné logiku pro vytváření odpovědí HTTP do samostatné třídy.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="2e5ce-140">Díky záměr jasnější, akce kontroleru skrytím nízké úrovně podrobnosti o vytváření odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="2e5ce-141">**IHttpActionResult** obsahuje jedinou metodu **ExecuteAsync**, který asynchronně vytvoří **objekt HttpResponseMessage** instance.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="2e5ce-142">Akce kontroleru vrátí-li **IHttpActionResult**, volání webového rozhraní API **ExecuteAsync** metodu pro vytvoření **objekt HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="2e5ce-143">Potom převede ji **objekt HttpResponseMessage** do zprávy odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="2e5ce-144">Zde je jednoduchý implementaton z **IHttpActionResult** vytvářející Prostý text odpovědi:</span><span class="sxs-lookup"><span data-stu-id="2e5ce-144">Here is a simple implementaton of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="2e5ce-145">Akce kontroleru příklad:</span><span class="sxs-lookup"><span data-stu-id="2e5ce-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="2e5ce-146">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="2e5ce-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="2e5ce-147">Více často, bude používat **IHttpActionResult** implementace, které jsou definované v  **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)**  oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-147">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="2e5ce-148">**Objektu ApiController** třída definuje pomocné metody, které vracejí výsledky těchto vestavěná akce.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="2e5ce-149">V následujícím příkladu, pokud požadavek neodpovídá stávající ID produktu, zavolá řadičem [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) k vytvoření odpovědi 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="2e5ce-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="2e5ce-150">Jinak hodnota řadičem volá [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), která vytvoří odpovědi 200 (OK), obsahuje produktu.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="2e5ce-151">Ostatní návratové typy</span><span class="sxs-lookup"><span data-stu-id="2e5ce-151">Other Return Types</span></span>

<span data-ttu-id="2e5ce-152">Pro všechny ostatní návratové typy webového rozhraní API používá [formátovací modul média](../formats-and-model-binding/media-formatters.md) k serializaci návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="2e5ce-153">Webové rozhraní API zapíše serializovaná hodnota do text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="2e5ce-154">Stavový kód odpovědi je 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="2e5ce-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="2e5ce-155">Nevýhodou tohoto přístupu je, že nelze vrátit přímo kód chyby, jako je například 404.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="2e5ce-156">Ale můžete vyvolat **HttpResponseException** pro kódy chyb.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="2e5ce-157">Další informace najdete v tématu [zpracování výjimek v rozhraní ASP.NET Web API](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="2e5ce-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="2e5ce-158">Webové rozhraní API pomocí hlavička Accept v požadavku formátování.</span><span class="sxs-lookup"><span data-stu-id="2e5ce-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="2e5ce-159">Další informace najdete v tématu [vyjednávání obsahu](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="2e5ce-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="2e5ce-160">Příklad požadavku</span><span class="sxs-lookup"><span data-stu-id="2e5ce-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="2e5ce-161">Příklad odpovědi:</span><span class="sxs-lookup"><span data-stu-id="2e5ce-161">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
