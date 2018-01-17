---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: "Atribut směrování v rozhraní ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7c563f566b8456b63ffe0a3c4876432c60a19e89
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="fbdc8-102">Atribut směrování v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="fbdc8-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="fbdc8-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fbdc8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fbdc8-104">*Směrování* je, jak webové rozhraní API odpovídá identifikátoru URI k akci.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="fbdc8-105">Webové rozhraní API 2 podporuje nový typ směrování, nazývá *směrováním atributů*.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="fbdc8-106">Jak již název napovídá, směrováním atributů používá atributy pro definování tras.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="fbdc8-107">Atribut směrování vám dává větší kontrolu nad identifikátory URI ve vašem webovém rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="fbdc8-108">Například můžete snadno vytvořit identifikátory URI, které popisují hierarchie prostředků.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="fbdc8-109">Starší styl směrování, názvem založené na konvenci směrování, je stále plně podporována.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="fbdc8-110">Ve skutečnosti můžete kombinovat obě tyto metody ve stejném projektu.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="fbdc8-111">Toto téma ukazuje, jak povolit směrováním atributů a popisuje různé možnosti pro atribut směrování.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="fbdc8-112">Kurz začátku do konce, který používá atribut směrování, najdete v části [vytvořit rozhraní REST API s atribut směrování ve webovém rozhraní API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="fbdc8-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="fbdc8-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fbdc8-113">Prerequisites</span></span>

<span data-ttu-id="fbdc8-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional nebo Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="fbdc8-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="fbdc8-115">Případně můžete použijte Správce balíčků NuGet k instalaci nezbytných balíčků.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="fbdc8-116">Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="fbdc8-117">Zadejte následující příkaz v okně konzoly Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="fbdc8-118">Proč atribut směrování?</span><span class="sxs-lookup"><span data-stu-id="fbdc8-118">Why Attribute Routing?</span></span>

<span data-ttu-id="fbdc8-119">První verze součásti webového rozhraní API používá *založené na konvenci* směrování.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="fbdc8-120">V tomto typu směrování definujete jeden nebo více šablon trasy, které jsou v podstatě parametry řetězce.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="fbdc8-121">Žádost o přijetí rozhraní odpovídá identifikátoru URI pro šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="fbdc8-122">(Další informace o směrování založené na konvenci, najdete v části [směrování v rozhraní ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="fbdc8-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="fbdc8-123">Jednou z výhod založené na konvenci směrování je, že šablony jsou definovány na jednom místě a pravidla směrování se použít konzistentně napříč všechny řadiče.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="fbdc8-124">Bohužel založené na konvenci směrování důvodu je těžké podporují určité vzorce identifikátor URI, které jsou společné rozhraní RESTful API.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="fbdc8-125">Například prostředků často obsahují podřízené prostředky: Zákazníci mají objednávky, filmy mít aktéři, knihy mít autoři a tak dále.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="fbdc8-126">Jedná se o přirozené vytvořit identifikátory URI, která tyto vztahy v úvahu:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="fbdc8-127">Tento typ identifikátoru URI je těžké vytvořit pomocí založené na konvenci směrování.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="fbdc8-128">I když je možné ji provést, není výsledky škálovat dobře, pokud máte mnoho řadiče nebo typy prostředků.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="fbdc8-129">Se směrováním atributů je jednoduchá k definování trasu pro tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="fbdc8-130">Jednoduše přidání atributu do akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="fbdc8-131">Tady jsou některé vzory, tento atribut směrování díky snadné.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="fbdc8-132">**Správa verzí rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="fbdc8-132">**API versioning**</span></span>

<span data-ttu-id="fbdc8-133">V tomto příkladu "/ api/v1/products" by směrované na řadič jiný než "/ api/v2 nebo produktů".</span><span class="sxs-lookup"><span data-stu-id="fbdc8-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="fbdc8-134">**Přetížené segmenty identifikátor URI**</span><span class="sxs-lookup"><span data-stu-id="fbdc8-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="fbdc8-135">V tomto příkladu "1" je číslo objednávky, ale "čekající" mapy do kolekce.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="fbdc8-136">**Více typy parametrů**</span><span class="sxs-lookup"><span data-stu-id="fbdc8-136">**Mulitple parameter types**</span></span>

<span data-ttu-id="fbdc8-137">V tomto příkladu "1" je číslo objednávky, ale "2013/06/16" Určuje datum.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="fbdc8-138">Povolení směrováním atributů</span><span class="sxs-lookup"><span data-stu-id="fbdc8-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="fbdc8-139">Chcete-li povolit směrováním atributů, volejte **MapHttpAttributeRoutes** během konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="fbdc8-140">Tato metoda rozšíření je definována v **System.Web.Http.HttpConfigurationExtensions** třídy.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="fbdc8-141">Atribut směrování je možné kombinovat s [založené na konvenci](routing-in-aspnet-web-api.md) směrování.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="fbdc8-142">Chcete-li definovat založené na konvenci směrování, volejte **MapHttpRoute** metoda.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="fbdc8-143">Další informace o konfiguraci webového rozhraní API najdete v tématu [konfigurace ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="fbdc8-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="fbdc8-144">Poznámka: Migrace z webového rozhraní API 1</span><span class="sxs-lookup"><span data-stu-id="fbdc8-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="fbdc8-145">Před webovém rozhraní API 2 šablony projektu webového rozhraní API generovaného kódu takto:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="fbdc8-146">Pokud je povoleno atribut směrování, tento kód vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="fbdc8-147">Pokud upgradujete existující projekt webového rozhraní API používat směrováním atributů, nezapomeňte aktualizovat tento kód konfigurace takto:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="fbdc8-148">Další informace najdete v tématu [konfiguraci webového rozhraní API s hostování prostředí ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="fbdc8-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="fbdc8-149">Přidání atributů tras</span><span class="sxs-lookup"><span data-stu-id="fbdc8-149">Adding Route Attributes</span></span>

<span data-ttu-id="fbdc8-150">Tady je příklad trasy definované pomocí atribut:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="fbdc8-151">Řetězec &quot;zákazníkům / {customerId} / řadí&quot; je šablona identifikátor URI pro trasu.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="fbdc8-152">Webové rozhraní API se pokusí vyhledat identifikátoru URI požadavku do šablony.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="fbdc8-153">V tomto příkladu jsou literálu segmenty "zákazníci" a "objednávky" a "{customerId}" je parametr proměnné.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="fbdc8-154">Následující identifikátory URI odpovídá této šablony:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="fbdc8-155">Můžete omezit odpovídající pomocí [omezení](#constraints), které jsou popsány dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="fbdc8-156">Všimněte si, že &quot;{customerId}&quot; odpovídá názvu parametru v šabloně trasy *customerId* parametr v metodě.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="fbdc8-157">Když webového rozhraní API vyvolá akce kontroleru, pokusí se vytvořit vazbu parametry trasy.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="fbdc8-158">Například pokud je identifikátor URI `http://example.com/customers/1/orders`, webového rozhraní API se pokusí vytvořit vazbu hodnotu "1" *customerId* parametr v akci.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="fbdc8-159">Identifikátor URI šablona může mít několik parametrů:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="fbdc8-160">Všechny metody kontroleru, které nemají atribut trasy používat založené na konvenci směrování.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="fbdc8-161">Tímto způsobem můžete kombinovat oba typy směrování v rámci jednoho projektu.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="fbdc8-162">Metody HTTP</span><span class="sxs-lookup"><span data-stu-id="fbdc8-162">HTTP Methods</span></span>

<span data-ttu-id="fbdc8-163">Webové rozhraní API také vybere akce založené na metodě HTTP požadavku (GET, POST atd.).</span><span class="sxs-lookup"><span data-stu-id="fbdc8-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="fbdc8-164">Ve výchozím nastavení hledá webového rozhraní API velká a malá písmena shody s začátek metoda názvu kontroleru.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="fbdc8-165">Například metoda kontroleru s názvem `PutCustomers` odpovídá požadavek HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="fbdc8-166">Touto konvencí můžete přepsat pomocí architekturu metodu s jakoukoli následující atributy:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="fbdc8-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="fbdc8-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="fbdc8-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="fbdc8-168">**[HttpGet]**</span></span>
- <span data-ttu-id="fbdc8-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="fbdc8-169">**[HttpHead]**</span></span>
- <span data-ttu-id="fbdc8-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="fbdc8-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="fbdc8-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="fbdc8-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="fbdc8-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="fbdc8-172">**[HttpPost]**</span></span>
- <span data-ttu-id="fbdc8-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="fbdc8-173">**[HttpPut]**</span></span>

<span data-ttu-id="fbdc8-174">Následující příklad mapuje metodu CreateBook na požadavky HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="fbdc8-175">Pro všechny ostatní metody HTTP, včetně nestandardní metod, pomocí **AcceptVerbs** atribut, který přebírá seznam metod HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="fbdc8-176">Předpony trasy</span><span class="sxs-lookup"><span data-stu-id="fbdc8-176">Route Prefixes</span></span>

<span data-ttu-id="fbdc8-177">Trasy se často v kontroleru všechny začínají stejnou předponou.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="fbdc8-178">Příklad:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="fbdc8-179">Běžné předpony pro celý kontroler můžete nastavit pomocí **[RoutePrefix]** atribut:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="fbdc8-180">Použijte tildou (~) v atributu metoda přepsat předponu trasy:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="fbdc8-181">Předpona trasy může obsahovat parametry:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="fbdc8-182">Omezení trasy</span><span class="sxs-lookup"><span data-stu-id="fbdc8-182">Route Constraints</span></span>

<span data-ttu-id="fbdc8-183">Omezení trasy vám umožní omezit, jak se splní parametrů v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="fbdc8-184">Je Obecná syntaxe &quot;{parametr: omezení}&quot;.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="fbdc8-185">Příklad:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="fbdc8-186">Zde první trasy se vybrat pouze pokud &quot;id&quot; segment identifikátoru URI je celé číslo.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="fbdc8-187">Druhý trasy, jinak bude vybrána.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="fbdc8-188">Následující tabulka uvádí omezení, které jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="fbdc8-189">Omezení</span><span class="sxs-lookup"><span data-stu-id="fbdc8-189">Constraint</span></span> | <span data-ttu-id="fbdc8-190">Popis</span><span class="sxs-lookup"><span data-stu-id="fbdc8-190">Description</span></span> | <span data-ttu-id="fbdc8-191">Příklad</span><span class="sxs-lookup"><span data-stu-id="fbdc8-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fbdc8-192">Alpha</span><span class="sxs-lookup"><span data-stu-id="fbdc8-192">alpha</span></span> | <span data-ttu-id="fbdc8-193">Odpovídá velká nebo malá písmena latinky znaky (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="fbdc8-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="fbdc8-194">{x: alpha}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-194">{x:alpha}</span></span> |
| <span data-ttu-id="fbdc8-195">bool</span><span class="sxs-lookup"><span data-stu-id="fbdc8-195">bool</span></span> | <span data-ttu-id="fbdc8-196">Odpovídá logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-196">Matches a Boolean value.</span></span> | <span data-ttu-id="fbdc8-197">{x: bool}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-197">{x:bool}</span></span> |
| <span data-ttu-id="fbdc8-198">Data a času</span><span class="sxs-lookup"><span data-stu-id="fbdc8-198">datetime</span></span> | <span data-ttu-id="fbdc8-199">Odpovídá **data a času** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="fbdc8-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-200">{x:datetime}</span></span> |
| <span data-ttu-id="fbdc8-201">decimal</span><span class="sxs-lookup"><span data-stu-id="fbdc8-201">decimal</span></span> | <span data-ttu-id="fbdc8-202">Odpovídá desítkovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-202">Matches a decimal value.</span></span> | <span data-ttu-id="fbdc8-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-203">{x:decimal}</span></span> |
| <span data-ttu-id="fbdc8-204">double</span><span class="sxs-lookup"><span data-stu-id="fbdc8-204">double</span></span> | <span data-ttu-id="fbdc8-205">Odpovídá 64bitová hodnota s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="fbdc8-206">{x: double}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-206">{x:double}</span></span> |
| <span data-ttu-id="fbdc8-207">float</span><span class="sxs-lookup"><span data-stu-id="fbdc8-207">float</span></span> | <span data-ttu-id="fbdc8-208">Odpovídá 32bitovou hodnotu s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="fbdc8-209">{x: float}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-209">{x:float}</span></span> |
| <span data-ttu-id="fbdc8-210">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="fbdc8-210">guid</span></span> | <span data-ttu-id="fbdc8-211">Odpovídá hodnota identifikátoru GUID.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-211">Matches a GUID value.</span></span> | <span data-ttu-id="fbdc8-212">{x: guid}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-212">{x:guid}</span></span> |
| <span data-ttu-id="fbdc8-213">int</span><span class="sxs-lookup"><span data-stu-id="fbdc8-213">int</span></span> | <span data-ttu-id="fbdc8-214">Odpovídá hodnotě 32bitové celé číslo.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="fbdc8-215">{x: int}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-215">{x:int}</span></span> |
| <span data-ttu-id="fbdc8-216">length</span><span class="sxs-lookup"><span data-stu-id="fbdc8-216">length</span></span> | <span data-ttu-id="fbdc8-217">Odpovídá řetězci se zadanou délkou nebo v rámci zadaného rozsahu délek.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="fbdc8-218">{x: length(6)} {x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="fbdc8-219">long</span><span class="sxs-lookup"><span data-stu-id="fbdc8-219">long</span></span> | <span data-ttu-id="fbdc8-220">Odpovídá hodnotě 64bitové celé číslo.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="fbdc8-221">{x: dlouho}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-221">{x:long}</span></span> |
| <span data-ttu-id="fbdc8-222">max</span><span class="sxs-lookup"><span data-stu-id="fbdc8-222">max</span></span> | <span data-ttu-id="fbdc8-223">Odpovídá celé číslo s maximální hodnotou.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="fbdc8-224">{x: max(10)}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-224">{x:max(10)}</span></span> |
| <span data-ttu-id="fbdc8-225">hodnota MaxLength</span><span class="sxs-lookup"><span data-stu-id="fbdc8-225">maxlength</span></span> | <span data-ttu-id="fbdc8-226">Odpovídá řetězec s maximální délkou.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="fbdc8-227">{x: maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="fbdc8-228">min</span><span class="sxs-lookup"><span data-stu-id="fbdc8-228">min</span></span> | <span data-ttu-id="fbdc8-229">Odpovídá celé číslo s minimální hodnotou.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="fbdc8-230">{x:min(10)}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-230">{x:min(10)}</span></span> |
| <span data-ttu-id="fbdc8-231">MinLength</span><span class="sxs-lookup"><span data-stu-id="fbdc8-231">minlength</span></span> | <span data-ttu-id="fbdc8-232">Odpovídá řetězci s minimální délkou.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="fbdc8-233">{x: minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="fbdc8-234">range</span><span class="sxs-lookup"><span data-stu-id="fbdc8-234">range</span></span> | <span data-ttu-id="fbdc8-235">Odpovídá celé číslo v rozsahu hodnot.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="fbdc8-236">{x: range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="fbdc8-237">regex</span><span class="sxs-lookup"><span data-stu-id="fbdc8-237">regex</span></span> | <span data-ttu-id="fbdc8-238">Odpovídá regulárnímu výrazu.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-238">Matches a regular expression.</span></span> | <span data-ttu-id="fbdc8-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="fbdc8-240">Všimněte si některá omezení, jako například &quot;min&quot;, trvat argumenty v závorkách.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="fbdc8-241">Můžete použít více omezení pro parametr oddělené dvojtečkou.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="fbdc8-242">Omezení vlastní trasy</span><span class="sxs-lookup"><span data-stu-id="fbdc8-242">Custom Route Constraints</span></span>

<span data-ttu-id="fbdc8-243">Můžete vytvořit vlastní trasy omezení implementací **IHttpRouteConstraint** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="fbdc8-244">Například následující omezení omezuje parametr nenulovou celočíselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="fbdc8-245">Následující kód ukazuje, jak zaregistrovat omezení:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="fbdc8-246">Teď můžete použít omezení v trasy:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="fbdc8-247">Můžete také nahradit celý **DefaultInlineConstraintResolver** třída implementací **IInlineConstraintResolver** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="fbdc8-248">Díky tomu dojde k nahrazení všech předdefinovaných omezení, pokud vaši implementaci **IInlineConstraintResolver** konkrétně přidává je.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="fbdc8-249">Identifikátor URI volitelné parametry a výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="fbdc8-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="fbdc8-250">Volitelný parametr URI můžete provést přidáním otazník parametru trasy.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="fbdc8-251">Pokud je volitelný parametr trasy, je nutné zadat výchozí hodnotu pro parametr metody.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="fbdc8-252">V tomto příkladu `/api/books/locale/1033` a `/api/books/locale` vrátit stejného zdroje.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="fbdc8-253">Alternativně můžete zadat výchozí hodnotu v šabloně trasy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="fbdc8-254">Toto je téměř stejný jako předchozí příklad, ale pokud je použita výchozí hodnota je malého rozdílu chování.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="fbdc8-255">V prvním příkladu ("{lcid?}") výchozí hodnotu 1033 je přiřazen přímo parametru metody, tak parametr bude mít tento přesná hodnota.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="fbdc8-256">V druhém příkladu ("{lcid = 1033}"), výchozí hodnota "1033" projde procesem vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="fbdc8-257">Vazač modelu výchozí převede "1033" číselnou hodnotu 1033.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="fbdc8-258">Můžete však zařaďte vlastní vazač modelu, který může dělat něco jiného.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="fbdc8-259">(Ve většině případů, pokud máte v kanálu, vazače modelů vlastní dvě formy bude ekvivalentní.)</span><span class="sxs-lookup"><span data-stu-id="fbdc8-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="fbdc8-260">Názvy tras</span><span class="sxs-lookup"><span data-stu-id="fbdc8-260">Route Names</span></span>

<span data-ttu-id="fbdc8-261">Všechny trasy v rozhraní Web API, má název.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-261">In Web API, every route has a name.</span></span> <span data-ttu-id="fbdc8-262">Názvy tras jsou užitečné pro generování odkazů, tak, aby odkaz můžete zahrnout do odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="fbdc8-263">Chcete-li zadat název trasy, nastavte **název** vlastnost v atributu.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="fbdc8-264">Následující příklad ukazuje, jak nastavit název trasy a také použití název trasy, která při generování odkazu.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="fbdc8-265">Pořadí trasy</span><span class="sxs-lookup"><span data-stu-id="fbdc8-265">Route Order</span></span>

<span data-ttu-id="fbdc8-266">Když se pokusí rozhraní tak, aby odpovídaly identifikátor URI s trasu, vyhodnotí trasy v určitém pořadí.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="fbdc8-267">Chcete-li určit pořadí, nastavte **RouteOrder** vlastnost na atribut trasy.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="fbdc8-268">Nižší hodnoty se vyhodnocují jako první.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-268">Lower values are evaluated first.</span></span> <span data-ttu-id="fbdc8-269">Výchozí hodnota pořadí je nulová.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-269">The default order value is zero.</span></span>

<span data-ttu-id="fbdc8-270">Zde je, jakým způsobem je určován celkový řazení:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="fbdc8-271">Porovnání **RouteOrder** vlastnost atribut trasy.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="fbdc8-272">Podívejte se na každý segment identifikátoru URI v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="fbdc8-273">Pro každý segment pořadí následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="fbdc8-274">Literál segmenty.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-274">Literal segments.</span></span>
    2. <span data-ttu-id="fbdc8-275">Parametry trasy s omezeními.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="fbdc8-276">Parametry trasy bez omezení.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="fbdc8-277">Zástupný parametr segmenty s omezeními.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="fbdc8-278">Zástupný parametr segmenty bez omezení.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="fbdc8-279">V případě vazbě, trasy jsou seřazené podle porovnání velká a malá písmena pořadí řetězců ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="fbdc8-280">Zde je příklad.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-280">Here is an example.</span></span> <span data-ttu-id="fbdc8-281">Předpokládejme, že definujete řadičem následující:</span><span class="sxs-lookup"><span data-stu-id="fbdc8-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="fbdc8-282">Tyto trasy seřazeni následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="fbdc8-283">objednávky nebo podrobnosti</span><span class="sxs-lookup"><span data-stu-id="fbdc8-283">orders/details</span></span>
2. <span data-ttu-id="fbdc8-284">objednávky nebo {id}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-284">orders/{id}</span></span>
3. <span data-ttu-id="fbdc8-285">objednávky nebo {JménoZákazníka}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-285">orders/{customerName}</span></span>
4. <span data-ttu-id="fbdc8-286">orders/{\\*date}</span><span class="sxs-lookup"><span data-stu-id="fbdc8-286">orders/{\\*date}</span></span>
5. <span data-ttu-id="fbdc8-287">objednávky / čekající na vyřízení</span><span class="sxs-lookup"><span data-stu-id="fbdc8-287">orders/pending</span></span>

<span data-ttu-id="fbdc8-288">Všimněte si, že "Podrobnosti" je literál segment a se zobrazuje před "{id}, ale"čekající na vyřízení"zobrazí poslední, protože **RouteOrder** vlastnost je 1.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="fbdc8-289">(Tento příklad předpokládá existuje jsou odběratelé s názvem "Podrobnosti" nebo "čeká na vyřízení".</span><span class="sxs-lookup"><span data-stu-id="fbdc8-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="fbdc8-290">Obecně platí pokuste se vyhnout nejednoznačný trasy.</span><span class="sxs-lookup"><span data-stu-id="fbdc8-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="fbdc8-291">V tomto příkladu lepší šablona trasy pro `GetByCustomer` je "zákazníkům / {JménoZákazníka}")</span><span class="sxs-lookup"><span data-stu-id="fbdc8-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
