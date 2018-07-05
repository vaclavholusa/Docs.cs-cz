---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Směrování ve webovém rozhraní API technologie ASP.NET | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5e3bba70993dafcdd93feed52813ee80697b1038
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374202"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="9e42a-102">Směrování v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="9e42a-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="9e42a-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9e42a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9e42a-104">Tento článek popisuje, jak rozhraní ASP.NET Web API směruje požadavky HTTP na řadiče.</span><span class="sxs-lookup"><span data-stu-id="9e42a-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="9e42a-105">Pokud jste obeznámeni s architekturou ASP.NET MVC, směrování rozhraní Web API je velmi podobný směrování MVC.</span><span class="sxs-lookup"><span data-stu-id="9e42a-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="9e42a-106">Hlavní rozdíl je, že webové rozhraní API používá metoda HTTP není cesta k identifikátoru URI, můžete vybrat akci.</span><span class="sxs-lookup"><span data-stu-id="9e42a-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="9e42a-107">Můžete také použít MVC – vizuální styl směrování v rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="9e42a-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="9e42a-108">Tento článek nepředpokládá žádnou znalost ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9e42a-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="9e42a-109">Směrovací tabulky</span><span class="sxs-lookup"><span data-stu-id="9e42a-109">Routing Tables</span></span>

<span data-ttu-id="9e42a-110">V rozhraní ASP.NET Web API *řadič* je třída, která zpracovává požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="9e42a-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="9e42a-111">Veřejné metody kontroleru se nazývají *metody akce* nebo jednoduše *akce*.</span><span class="sxs-lookup"><span data-stu-id="9e42a-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="9e42a-112">Žádost o přijetí rozhraní Web API přesměruje požadavek na akci.</span><span class="sxs-lookup"><span data-stu-id="9e42a-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="9e42a-113">Chcete-li zjistit, jakou akci, která se má vyvolat, systém použije *směrovací tabulky*.</span><span class="sxs-lookup"><span data-stu-id="9e42a-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="9e42a-114">Šablona projektu sady Visual Studio pro webové rozhraní API vytvoří výchozí trasy:</span><span class="sxs-lookup"><span data-stu-id="9e42a-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="9e42a-115">Tato trasa je definována v souboru WebApiConfig.cs, který je umístěn v aplikaci\_spouštěcí adresář:</span><span class="sxs-lookup"><span data-stu-id="9e42a-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="9e42a-116">Další informace o **WebApiConfig** najdete v tématu [konfigurace ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="9e42a-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="9e42a-117">Pokud samoobslužné hostování webového rozhraní API, je nutné nastavit směrovací tabulky přímo na **HttpSelfHostConfiguration** objektu.</span><span class="sxs-lookup"><span data-stu-id="9e42a-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="9e42a-118">Další informace najdete v tématu [samoobslužné hostování webového rozhraní API](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="9e42a-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="9e42a-119">Obsahuje každou položku v tabulce směrování *šablonu trasy*.</span><span class="sxs-lookup"><span data-stu-id="9e42a-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="9e42a-120">Výchozí šablona trasy pro webové rozhraní API je &quot;rozhraní api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="9e42a-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="9e42a-121">V této šabloně &quot;api&quot; je segment cesty literálu a {controller} a {id} jsou proměnné zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="9e42a-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="9e42a-122">Rozhraní Web API přijme požadavek HTTP, pokusí se vyhledat identifikátor URI pro jeden z šablon trasy do směrovací tabulky.</span><span class="sxs-lookup"><span data-stu-id="9e42a-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="9e42a-123">Pokud neodpovídá žádná trasa klient obdrží chybu 404.</span><span class="sxs-lookup"><span data-stu-id="9e42a-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="9e42a-124">Například následující identifikátory URI odpovídaly výchozí trasy:</span><span class="sxs-lookup"><span data-stu-id="9e42a-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="9e42a-125">/ api/contacts</span><span class="sxs-lookup"><span data-stu-id="9e42a-125">/api/contacts</span></span>
- <span data-ttu-id="9e42a-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="9e42a-126">/api/contacts/1</span></span>
- <span data-ttu-id="9e42a-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="9e42a-127">/api/products/gizmo1</span></span>

<span data-ttu-id="9e42a-128">Ale následující identifikátor URI neodpovídá, protože postrádá &quot;api&quot; segmentu:</span><span class="sxs-lookup"><span data-stu-id="9e42a-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="9e42a-129">/ contacts/1</span><span class="sxs-lookup"><span data-stu-id="9e42a-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="9e42a-130">Pro zabránění kolizím s architekturou ASP.NET MVC směrovací je důvod pomocí "rozhraní api" v této trase.</span><span class="sxs-lookup"><span data-stu-id="9e42a-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="9e42a-131">Tímto způsobem může mít &quot;/kontaktuje&quot; přejít na kontroler MVC, a &quot;/api/contacts&quot; přejít na kontroler Web API.</span><span class="sxs-lookup"><span data-stu-id="9e42a-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="9e42a-132">Samozřejmě pokud se vám tato konvence, můžete změnit výchozí směrovací tabulka.</span><span class="sxs-lookup"><span data-stu-id="9e42a-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="9e42a-133">Po nalezení odpovídající trasy rozhraní Web API vybere kontroleru a akce:</span><span class="sxs-lookup"><span data-stu-id="9e42a-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="9e42a-134">Najít kontroler webového rozhraní API přidá &quot;řadič&quot; na hodnotu *{controller}* proměnné.</span><span class="sxs-lookup"><span data-stu-id="9e42a-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="9e42a-135">Najít akce, webové rozhraní API zjistí metodu HTTP a pak hledá akci, jejichž název začíná s tímto názvem metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="9e42a-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="9e42a-136">Například požadavek GET, webové rozhraní API hledá akci, která začíná &quot;získat... &quot;, jako například &quot;GetContact&quot; nebo &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="9e42a-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="9e42a-137">Tato konvence platí pouze pro GET, POST, PUT a DELETE metody.</span><span class="sxs-lookup"><span data-stu-id="9e42a-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="9e42a-138">Pomocí atributů na vašem řadiči můžete povolit další metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="9e42a-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="9e42a-139">Ukážeme příklad, který později.</span><span class="sxs-lookup"><span data-stu-id="9e42a-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="9e42a-140">Další proměnné zástupný symbol v šabloně trasy, jako například *{id}* jsou mapovány na parametry akce.</span><span class="sxs-lookup"><span data-stu-id="9e42a-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="9e42a-141">Pojďme se podívat na příklad.</span><span class="sxs-lookup"><span data-stu-id="9e42a-141">Let's look at an example.</span></span> <span data-ttu-id="9e42a-142">Předpokládejme, že můžete definovat následující kontroler:</span><span class="sxs-lookup"><span data-stu-id="9e42a-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="9e42a-143">Tady jsou některé možné požadavků protokolu HTTP, spolu s akci, která získá pro každé vyvolání:</span><span class="sxs-lookup"><span data-stu-id="9e42a-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="9e42a-144">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="9e42a-144">HTTP Method</span></span> | <span data-ttu-id="9e42a-145">Cesta identifikátoru URI</span><span class="sxs-lookup"><span data-stu-id="9e42a-145">URI Path</span></span> | <span data-ttu-id="9e42a-146">Akce</span><span class="sxs-lookup"><span data-stu-id="9e42a-146">Action</span></span> | <span data-ttu-id="9e42a-147">Parametr</span><span class="sxs-lookup"><span data-stu-id="9e42a-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9e42a-148">ZÍSKAT</span><span class="sxs-lookup"><span data-stu-id="9e42a-148">GET</span></span> | <span data-ttu-id="9e42a-149">rozhraní API a produktů</span><span class="sxs-lookup"><span data-stu-id="9e42a-149">api/products</span></span> | <span data-ttu-id="9e42a-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="9e42a-150">GetAllProducts</span></span> | <span data-ttu-id="9e42a-151">*(žádné)*</span><span class="sxs-lookup"><span data-stu-id="9e42a-151">*(none)*</span></span> |
| <span data-ttu-id="9e42a-152">ZÍSKAT</span><span class="sxs-lookup"><span data-stu-id="9e42a-152">GET</span></span> | <span data-ttu-id="9e42a-153">rozhraní API, produkty nebo 4</span><span class="sxs-lookup"><span data-stu-id="9e42a-153">api/products/4</span></span> | <span data-ttu-id="9e42a-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="9e42a-154">GetProductById</span></span> | <span data-ttu-id="9e42a-155">4</span><span class="sxs-lookup"><span data-stu-id="9e42a-155">4</span></span> |
| <span data-ttu-id="9e42a-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="9e42a-156">DELETE</span></span> | <span data-ttu-id="9e42a-157">rozhraní API, produkty nebo 4</span><span class="sxs-lookup"><span data-stu-id="9e42a-157">api/products/4</span></span> | <span data-ttu-id="9e42a-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="9e42a-158">DeleteProduct</span></span> | <span data-ttu-id="9e42a-159">4</span><span class="sxs-lookup"><span data-stu-id="9e42a-159">4</span></span> |
| <span data-ttu-id="9e42a-160">PŘÍSPĚVEK</span><span class="sxs-lookup"><span data-stu-id="9e42a-160">POST</span></span> | <span data-ttu-id="9e42a-161">rozhraní API a produktů</span><span class="sxs-lookup"><span data-stu-id="9e42a-161">api/products</span></span> | <span data-ttu-id="9e42a-162">*(žádná shoda)*</span><span class="sxs-lookup"><span data-stu-id="9e42a-162">*(no match)*</span></span> |  |

<span data-ttu-id="9e42a-163">Všimněte si, že *{id}* segment identifikátoru URI, pokud jsou k dispozici, je namapována na *id* parametru akce.</span><span class="sxs-lookup"><span data-stu-id="9e42a-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="9e42a-164">V tomto příkladu kontroleru definuje dvě metody GET, jednu s *id* parametr a jedna bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="9e42a-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="9e42a-165">Všimněte si také, požadavek POST se nezdaří, protože kontroler nedefinuje &quot;příspěvek... &quot; metody.</span><span class="sxs-lookup"><span data-stu-id="9e42a-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="9e42a-166">Směrování odchylky</span><span class="sxs-lookup"><span data-stu-id="9e42a-166">Routing Variations</span></span>

<span data-ttu-id="9e42a-167">Předchozí část popisuje základní mechanismus směrování pro ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="9e42a-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="9e42a-168">Tato část popisuje několik variant.</span><span class="sxs-lookup"><span data-stu-id="9e42a-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="9e42a-169">Metody HTTP</span><span class="sxs-lookup"><span data-stu-id="9e42a-169">HTTP Methods</span></span>

<span data-ttu-id="9e42a-170">Namísto použití zásady vytváření názvů pro metody HTTP, můžete explicitně určit metoda HTTP pro akci pomocí upravení metodu akce pomocí **HttpGet**, **HttpPut**, **HttpPost** , nebo **HttpDelete** atribut.</span><span class="sxs-lookup"><span data-stu-id="9e42a-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="9e42a-171">V následujícím příkladu je metoda FindProduct namapována na požadavky GET:</span><span class="sxs-lookup"><span data-stu-id="9e42a-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="9e42a-172">Povolit více metod HTTP pro akci, nebo povolíte metod HTTP než GET, PUT, POST a DELETE, použijte **AcceptVerbs** atribut, který přebírá seznam metod HTTP.</span><span class="sxs-lookup"><span data-stu-id="9e42a-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="9e42a-173">Směrování tím, že název akce</span><span class="sxs-lookup"><span data-stu-id="9e42a-173">Routing by Action Name</span></span>

<span data-ttu-id="9e42a-174">Výchozí šablonou směrování webového rozhraní API používá metodu HTTP a vyberte akci.</span><span class="sxs-lookup"><span data-stu-id="9e42a-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="9e42a-175">Můžete ale také vytvořit trasu, kde je název akce součástí identifikátoru URI:</span><span class="sxs-lookup"><span data-stu-id="9e42a-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="9e42a-176">V této šabloně trasy *{action}* názvy parametrů metody akce v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="9e42a-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="9e42a-177">S tímto stylem směrování použijte atributy k určení povolené metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="9e42a-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="9e42a-178">Předpokládejme například, že zařízení má následující metodu:</span><span class="sxs-lookup"><span data-stu-id="9e42a-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="9e42a-179">V takovém případě by podrobnosti metody mapování požadavek GET pro "rozhraní api/produkty/podrobnosti/1".</span><span class="sxs-lookup"><span data-stu-id="9e42a-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="9e42a-180">Tento styl směrování se podobá do architektury ASP.NET MVC a může být vhodné pro rozhraní API stylu RPC.</span><span class="sxs-lookup"><span data-stu-id="9e42a-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="9e42a-181">Název akce lze přepsat pomocí **ActionName** atribut.</span><span class="sxs-lookup"><span data-stu-id="9e42a-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="9e42a-182">V následujícím příkladu jsou dvě akce, které mapují na &quot;rozhraní api, produkty/Miniatura/*id*. Jedna podporuje GET a druhý příspěvek:</span><span class="sxs-lookup"><span data-stu-id="9e42a-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="9e42a-183">Bez akce</span><span class="sxs-lookup"><span data-stu-id="9e42a-183">Non-Actions</span></span>

<span data-ttu-id="9e42a-184">Chcete-li metoda zabránit v získání vyvolán jako akci, použijte **NonAction** atribut.</span><span class="sxs-lookup"><span data-stu-id="9e42a-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="9e42a-185">Signál, rozhraní Framework, že metoda není akci, i v případě, že odpovídají jinak pravidel směrování.</span><span class="sxs-lookup"><span data-stu-id="9e42a-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="9e42a-186">Další čtení</span><span class="sxs-lookup"><span data-stu-id="9e42a-186">Further Reading</span></span>

<span data-ttu-id="9e42a-187">Toto téma poskytuje souhrnný přehled směrování.</span><span class="sxs-lookup"><span data-stu-id="9e42a-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="9e42a-188">Další podrobnosti najdete v části [směrování a výběr akce](routing-and-action-selection.md), která popisuje přesně jak rozhraní odpovídá identifikátor URI pro trasu, vybere kontroler a pak vybere akce, která se má vyvolat.</span><span class="sxs-lookup"><span data-stu-id="9e42a-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
