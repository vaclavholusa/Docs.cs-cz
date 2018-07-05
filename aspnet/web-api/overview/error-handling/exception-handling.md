---
uid: web-api/overview/error-handling/exception-handling
title: Zpracování výjimek v rozhraní ASP.NET Web API | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: ac01a4f35cde99a1f8ec699e6d31bf597f1d334e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400817"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="12102-102">Zpracování výjimek v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="12102-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="12102-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="12102-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="12102-104">Tento článek popisuje chyb a zpracování výjimek v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="12102-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="12102-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="12102-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="12102-106">Filtry výjimek</span><span class="sxs-lookup"><span data-stu-id="12102-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="12102-107">Registrace filtry výjimek</span><span class="sxs-lookup"><span data-stu-id="12102-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="12102-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="12102-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="12102-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="12102-109">HttpResponseException</span></span>

<span data-ttu-id="12102-110">Co se stane, když kontroler webového rozhraní API vrátí nezachycenou výjimku?</span><span class="sxs-lookup"><span data-stu-id="12102-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="12102-111">Ve výchozím nastavení většina výjimek jsou přeloženy do odpovědi HTTP se stavovým kódem 500, vnitřní chyba serveru.</span><span class="sxs-lookup"><span data-stu-id="12102-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="12102-112">**HttpResponseException** typ je zvláštní případ.</span><span class="sxs-lookup"><span data-stu-id="12102-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="12102-113">Tato výjimka vrátí všechny stavový kód HTTP, který zadáte v konstruktoru výjimky.</span><span class="sxs-lookup"><span data-stu-id="12102-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="12102-114">Například následující metoda vrátí 404, nebyl nalezen, pokud *id* parametr není platný.</span><span class="sxs-lookup"><span data-stu-id="12102-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="12102-115">Pro větší kontrolu nad odpověď a můžete také vytvořit celé zprávy odpovědi zahrnout s **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="12102-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="12102-116">Filtry výjimek</span><span class="sxs-lookup"><span data-stu-id="12102-116">Exception Filters</span></span>

<span data-ttu-id="12102-117">Můžete přizpůsobit, jak webové rozhraní API zpracovává výjimky napsáním *filtr výjimek*.</span><span class="sxs-lookup"><span data-stu-id="12102-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="12102-118">Filtr výjimek se spustí v případě řadiče metoda vyvolá neošetřená výjimka, která je *není* **HttpResponseException** výjimky.</span><span class="sxs-lookup"><span data-stu-id="12102-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="12102-119">**HttpResponseException** typ je zvláštní případ, protože je navržená speciálně pro vrácení odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="12102-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="12102-120">Filtry výjimek implementovat **System.Web.Http.Filters.IExceptionFilter** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="12102-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="12102-121">Nejjednodušší způsob, jak zápis filtru výjimek je odvozena z **System.Web.Http.Filters.ExceptionFilterAttribute** třídy a přepsat **OnException** metody.</span><span class="sxs-lookup"><span data-stu-id="12102-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="12102-122">Filtry výjimek v rozhraní ASP.NET Web API jsou podobné těm v architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="12102-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="12102-123">Nicméně jsou deklarovány v samostatném oboru názvů a funkce samostatně.</span><span class="sxs-lookup"><span data-stu-id="12102-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="12102-124">Zejména v případě **HandleErrorAttribute** třída používaná v MVC nezpracovává výjimky vyvolané kontrolerů rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="12102-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="12102-125">Tady je filtr, který převede **NotImplementedException** 501 Neimplementováno kód výjimky na stav protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="12102-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="12102-126">**Odpovědi** vlastnost **HttpActionExecutedContext** objekt obsahuje zprávy s odpovědí HTTP, které se pošlou do klienta.</span><span class="sxs-lookup"><span data-stu-id="12102-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="12102-127">Registrace filtry výjimek</span><span class="sxs-lookup"><span data-stu-id="12102-127">Registering Exception Filters</span></span>

<span data-ttu-id="12102-128">Existuje několik způsobů, jak zaregistrovat filtr výjimek webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="12102-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="12102-129">Akce</span><span class="sxs-lookup"><span data-stu-id="12102-129">By action</span></span>
- <span data-ttu-id="12102-130">Adaptér</span><span class="sxs-lookup"><span data-stu-id="12102-130">By controller</span></span>
- <span data-ttu-id="12102-131">Globálně</span><span class="sxs-lookup"><span data-stu-id="12102-131">Globally</span></span>

<span data-ttu-id="12102-132">Použít filtr pro určité akce, přidejte filtr jako atribut na akci:</span><span class="sxs-lookup"><span data-stu-id="12102-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="12102-133">Použít filtr pro všechny akce v kontroleru, přidejte filtr jako atribut třídy kontroleru:</span><span class="sxs-lookup"><span data-stu-id="12102-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="12102-134">Chcete-li použít filtr globálně na všechny řadiče webové rozhraní API, přidejte instance filtru, který **GlobalConfiguration.Configuration.Filters** kolekce.</span><span class="sxs-lookup"><span data-stu-id="12102-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="12102-135">Filtry Geosyncservice v této kolekci se použijí na každou akci kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="12102-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="12102-136">Pokud používáte šablonu projektu "Aplikace pro Web ASP.NET MVC 4" k vytvoření projektu, ukládejte kód webového rozhraní API konfigurace uvnitř `WebApiConfig` třídu, která se nachází v aplikaci\_spouštěcí složka:</span><span class="sxs-lookup"><span data-stu-id="12102-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="12102-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="12102-137">HttpError</span></span>

<span data-ttu-id="12102-138">**HttpError** objekt poskytuje konzistentní způsob, jak vrátit informace o chybě v textu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="12102-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="12102-139">Následující příklad ukazuje, jak vrátit stavový kód HTTP 404 (Nenalezeno) pomocí **HttpError** v textu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="12102-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="12102-140">**CreateErrorResponse** je rozšiřující metody definované v **System.Net.Http.HttpRequestMessageExtensions** třídy.</span><span class="sxs-lookup"><span data-stu-id="12102-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="12102-141">Interně **CreateErrorResponse** vytvoří **HttpError** instance a potom vytvoří **objekt HttpResponseMessage** obsahující **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="12102-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="12102-142">V tomto příkladu Pokud je metoda úspěšná, vrátí produktu v odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="12102-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="12102-143">Pokud není nalezen požadovaný produkt, obsahuje odpověď HTTP, ale **HttpError** v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="12102-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="12102-144">Odpověď může vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="12102-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="12102-145">Všimněte si, **HttpError** byl serializován do formátu JSON v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="12102-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="12102-146">Jednou z výhod použití **HttpError** je, že se prochází stejné [vyjednávání obsahu](../formats-and-model-binding/content-negotiation.md) a serializace zpracovat jako žádný jiný model silného typu.</span><span class="sxs-lookup"><span data-stu-id="12102-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="12102-147">HttpError a ověření modelu</span><span class="sxs-lookup"><span data-stu-id="12102-147">HttpError and Model Validation</span></span>

<span data-ttu-id="12102-148">Pro ověření modelu, můžete předat stav modelu, který má **CreateErrorResponse**, aby zahrnoval chyby ověření v odpovědi:</span><span class="sxs-lookup"><span data-stu-id="12102-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="12102-149">V tomto příkladu může vrátit následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="12102-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="12102-150">Další informace o ověření modelu naleznete v tématu [ověření modelu v rozhraní ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="12102-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="12102-151">Pomocí HttpResponseException HttpError</span><span class="sxs-lookup"><span data-stu-id="12102-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="12102-152">V předchozích příkladech vrátit **objekt HttpResponseMessage** zprávy z akce kontroleru, ale můžete také použít **HttpResponseException** se vraťte **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="12102-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="12102-153">To umožňuje vracet modelem silného typu v případě úspěchu normální při vrácení stále **HttpError** Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="12102-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
