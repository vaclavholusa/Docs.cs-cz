---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Směrování v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="8e0d3-102">Směrování v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="8e0d3-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="8e0d3-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8e0d3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8e0d3-104">Tento článek popisuje, jak rozhraní ASP.NET Web API směruje požadavky HTTP na řadiče.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="8e0d3-105">Pokud jste obeznámeni s architekturou ASP.NET MVC, směrování rozhraní Web API je velmi podobný směrování MVC.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="8e0d3-106">Hlavní rozdíl je, že webového rozhraní API používá metoda HTTP není cesta k identifikátoru URI, můžete vybrat akci.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="8e0d3-107">Můžete také použít MVC stylu směrování v rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="8e0d3-108">Tento článek nepřebírá žádnou znalost rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="8e0d3-109">Směrovacích tabulek</span><span class="sxs-lookup"><span data-stu-id="8e0d3-109">Routing Tables</span></span>

<span data-ttu-id="8e0d3-110">V rozhraní ASP.NET Web API *řadič* je třída, která zpracovává požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="8e0d3-111">Veřejné metody řadiče se nazývají *metody akce* nebo jednoduše *akce*.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="8e0d3-112">Když rozhraní Web API přijme požadavek, přesměruje požadavek na akci.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="8e0d3-113">Určit akci, která má být vyvolán, používá rozhraní *směrovací tabulky*.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="8e0d3-114">Šablona projektu sady Visual Studio pro webové rozhraní API vytvoří výchozí trasa:</span><span class="sxs-lookup"><span data-stu-id="8e0d3-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="8e0d3-115">Tato trasa je definována v souboru WebApiConfig.cs, který je umístěn v aplikaci\_spouštěcí adresář:</span><span class="sxs-lookup"><span data-stu-id="8e0d3-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="8e0d3-116">Další informace o **WebApiConfig** třídy najdete v tématu [konfigurace rozhraní ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="8e0d3-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="8e0d3-117">Pokud hostujete samoobslužné webové rozhraní API, musíte nastavit do směrovací tabulky přímo na **HttpSelfHostConfiguration** objektu.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="8e0d3-118">Další informace najdete v tématu [hostování na vlastním serveru webového rozhraní API](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="8e0d3-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="8e0d3-119">Obsahuje každou položku v tabulce směrování *šablonu trasy*.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="8e0d3-120">Výchozí šablona trasy pro webového rozhraní API je &quot;rozhraní api nebo {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="8e0d3-121">V této šabloně &quot;rozhraní api&quot; je segment literálu cesty a {controller} a {id} jsou proměnné zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="8e0d3-122">Když rozhraní Web API obdrží požadavek HTTP, pokouší se identifikátor URI pro jeden z šablon trasy do směrovací tabulky.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="8e0d3-123">Pokud žádná trasa odpovídá, klient obdrží chybu 404.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="8e0d3-124">Například následující identifikátory URI odpovídají výchozí trasu:</span><span class="sxs-lookup"><span data-stu-id="8e0d3-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="8e0d3-125">/ api/contacts</span><span class="sxs-lookup"><span data-stu-id="8e0d3-125">/api/contacts</span></span>
- <span data-ttu-id="8e0d3-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="8e0d3-126">/api/contacts/1</span></span>
- <span data-ttu-id="8e0d3-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="8e0d3-127">/api/products/gizmo1</span></span>

<span data-ttu-id="8e0d3-128">Ale následující identifikátor URI neodpovídá, protože jí chybí &quot;rozhraní api&quot; segmentu:</span><span class="sxs-lookup"><span data-stu-id="8e0d3-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="8e0d3-129">/ contacts/1</span><span class="sxs-lookup"><span data-stu-id="8e0d3-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="8e0d3-130">Důvod pomocí "rozhraní api" v postupu je zabrání se tím kolizím s architekturou ASP.NET MVC směrování.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="8e0d3-131">Tímto způsobem může mít &quot;/kontaktuje&quot; přejděte na řadič MVC a &quot;/api/contacts&quot; přejděte do kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="8e0d3-132">Samozřejmě pokud vám nevyhovuje touto konvencí, můžete změnit výchozí směrovací tabulka.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="8e0d3-133">Jakmile je nalezen odpovídající trasy, vybere webového rozhraní API kontroleru a akce:</span><span class="sxs-lookup"><span data-stu-id="8e0d3-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="8e0d3-134">Najít kontroler, přidá webového rozhraní API &quot;řadič&quot; na hodnotu *{controller}* proměnné.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="8e0d3-135">Akce najdete webového rozhraní API porovná metodu protokolu HTTP a pak hledá akce jejichž název začíná tímto názvem metoda HTTP.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="8e0d3-136">Například s požadavek GET webového rozhraní API vypadá pro akci, která začíná &quot;získat... &quot;, jako například &quot;GetContact&quot; nebo &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="8e0d3-137">Touto konvencí platí pouze pro GET, POST, PUT a DELETE metody.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="8e0d3-138">Můžete povolit další metody HTTP pomocí atributů na vašem řadiči.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="8e0d3-139">Příklad této uvidíme později.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="8e0d3-140">Ostatní proměnné zástupný symbol v šabloně trasy, jako *{id}* jsou namapované na parametrů akcí.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="8e0d3-141">Podívejme se na příklad.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-141">Let's look at an example.</span></span> <span data-ttu-id="8e0d3-142">Předpokládejme, že definovat následující řadiče:</span><span class="sxs-lookup"><span data-stu-id="8e0d3-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="8e0d3-143">Zde jsou některé možné požadavků protokolu HTTP, společně s akci, která volán pro každou:</span><span class="sxs-lookup"><span data-stu-id="8e0d3-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="8e0d3-144">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="8e0d3-144">HTTP Method</span></span> | <span data-ttu-id="8e0d3-145">Cesta URI</span><span class="sxs-lookup"><span data-stu-id="8e0d3-145">URI Path</span></span> | <span data-ttu-id="8e0d3-146">Akce</span><span class="sxs-lookup"><span data-stu-id="8e0d3-146">Action</span></span> | <span data-ttu-id="8e0d3-147">Parametr</span><span class="sxs-lookup"><span data-stu-id="8e0d3-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8e0d3-148">GET</span><span class="sxs-lookup"><span data-stu-id="8e0d3-148">GET</span></span> | <span data-ttu-id="8e0d3-149">rozhraní API nebo produkty</span><span class="sxs-lookup"><span data-stu-id="8e0d3-149">api/products</span></span> | <span data-ttu-id="8e0d3-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="8e0d3-150">GetAllProducts</span></span> | <span data-ttu-id="8e0d3-151">*(žádný)*</span><span class="sxs-lookup"><span data-stu-id="8e0d3-151">*(none)*</span></span> |
| <span data-ttu-id="8e0d3-152">GET</span><span class="sxs-lookup"><span data-stu-id="8e0d3-152">GET</span></span> | <span data-ttu-id="8e0d3-153">rozhraní API, produkty nebo 4</span><span class="sxs-lookup"><span data-stu-id="8e0d3-153">api/products/4</span></span> | <span data-ttu-id="8e0d3-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="8e0d3-154">GetProductById</span></span> | <span data-ttu-id="8e0d3-155">4</span><span class="sxs-lookup"><span data-stu-id="8e0d3-155">4</span></span> |
| <span data-ttu-id="8e0d3-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="8e0d3-156">DELETE</span></span> | <span data-ttu-id="8e0d3-157">rozhraní API, produkty nebo 4</span><span class="sxs-lookup"><span data-stu-id="8e0d3-157">api/products/4</span></span> | <span data-ttu-id="8e0d3-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="8e0d3-158">DeleteProduct</span></span> | <span data-ttu-id="8e0d3-159">4</span><span class="sxs-lookup"><span data-stu-id="8e0d3-159">4</span></span> |
| <span data-ttu-id="8e0d3-160">POST</span><span class="sxs-lookup"><span data-stu-id="8e0d3-160">POST</span></span> | <span data-ttu-id="8e0d3-161">rozhraní API nebo produkty</span><span class="sxs-lookup"><span data-stu-id="8e0d3-161">api/products</span></span> | <span data-ttu-id="8e0d3-162">*(žádná shoda)*</span><span class="sxs-lookup"><span data-stu-id="8e0d3-162">*(no match)*</span></span> |  |

<span data-ttu-id="8e0d3-163">Všimněte si, že *{id}* segment identifikátoru URI, pokud existuje, je namapovaný na *id* parametr akce.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="8e0d3-164">V tomto příkladu řadičem definuje dvě metody GET, jeden s *id* parametr a jeden bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="8e0d3-165">Také Upozorňujeme, že požadavek POST se nezdaří, protože nedefinuje kontroleru &quot;Post... &quot; metoda.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="8e0d3-166">Směrování variant</span><span class="sxs-lookup"><span data-stu-id="8e0d3-166">Routing Variations</span></span>

<span data-ttu-id="8e0d3-167">Popsané v předchozí části základní mechanismus směrování pro ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="8e0d3-168">Tato část popisuje některé rozdíly.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="8e0d3-169">Metody HTTP</span><span class="sxs-lookup"><span data-stu-id="8e0d3-169">HTTP Methods</span></span>

<span data-ttu-id="8e0d3-170">Místo použití zásady vytváření názvů pro metody HTTP, můžete explicitně zadat metoda HTTP pro akce architekturu metoda akce s **třídy MetadataExchangeClientMode**, **HttpPut**, **HttpPost** , nebo **HttpDelete** atribut.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="8e0d3-171">V následujícím příkladu je metoda FindProduct namapovaný na požadavky GET:</span><span class="sxs-lookup"><span data-stu-id="8e0d3-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="8e0d3-172">Povolit více metod HTTP pro akci, nebo povolíte metod HTTP než GET, PUT, POST a odstranění, použijte **AcceptVerbs** atribut, který přebírá seznam metod HTTP.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="8e0d3-173">Směrování podle názvu akce</span><span class="sxs-lookup"><span data-stu-id="8e0d3-173">Routing by Action Name</span></span>

<span data-ttu-id="8e0d3-174">Výchozí směrování šablonu webového rozhraní API používá metodu protokolu HTTP a vyberte akci.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="8e0d3-175">Můžete však také vytvořit trasu, kde je název akce zahrnutá v identifikátoru URI:</span><span class="sxs-lookup"><span data-stu-id="8e0d3-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="8e0d3-176">V této šabloně trasy *{action}* názvů parametrů metody akce v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="8e0d3-177">Pomocí této styl směrování slouží k zadání povolených metod HTTP atributy.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="8e0d3-178">Předpokládejme například, že herní zařízení má následující metodu:</span><span class="sxs-lookup"><span data-stu-id="8e0d3-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="8e0d3-179">V takovém případě by metodu podrobnosti mapování požadavek GET pro "api/produkty/podrobnosti/1".</span><span class="sxs-lookup"><span data-stu-id="8e0d3-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="8e0d3-180">Tento styl směrování je podobná ASP.NET MVC a může být vhodné pro rozhraní API stylu RPC.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="8e0d3-181">Název akce můžete přepsat pomocí **název akce** atribut.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="8e0d3-182">V následujícím příkladu, existují dvě akce, které jsou mapovány na &quot;rozhraní api, produkty nebo miniaturu/*id*. Jedna podporuje GET a dalších POST:</span><span class="sxs-lookup"><span data-stu-id="8e0d3-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="8e0d3-183">Jiné akce</span><span class="sxs-lookup"><span data-stu-id="8e0d3-183">Non-Actions</span></span>

<span data-ttu-id="8e0d3-184">Abyste zabránili získávání vyvolána jako akce metodu, pomocí **NonAction** atribut.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="8e0d3-185">To signalizuje do rozhraní metoda není akce, i v případě, že by se jinak neshodovaly pravidla směrování.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="8e0d3-186">Další čtení</span><span class="sxs-lookup"><span data-stu-id="8e0d3-186">Further Reading</span></span>

<span data-ttu-id="8e0d3-187">Toto téma poskytuje podrobný pohled směrování.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="8e0d3-188">Další podrobnosti naleznete v [směrování a výběr akce](routing-and-action-selection.md), který popisuje, přesně jak rozhraní odpovídá identifikátor URI pro trasu, vybere řadič a potom vybere akci k vyvolání.</span><span class="sxs-lookup"><span data-stu-id="8e0d3-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
