---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Atribut směrování v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: e15f89ba98acef68279e51b278e3c7045569607a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373227"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="96d4f-102">Směrování atributů v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="96d4f-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="96d4f-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="96d4f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="96d4f-104">*Směrování* je, jak webové rozhraní API odpovídá identifikátor URI pro akci.</span><span class="sxs-lookup"><span data-stu-id="96d4f-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="96d4f-105">Webové rozhraní API 2 podporuje nový typ směrování, nazývá *směrováním atributů*.</span><span class="sxs-lookup"><span data-stu-id="96d4f-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="96d4f-106">Jak již název napovídá, směrování atributů používá atributy pro definování tras.</span><span class="sxs-lookup"><span data-stu-id="96d4f-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="96d4f-107">Směrování atributů vám dává větší kontrolu nad identifikátory URI webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="96d4f-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="96d4f-108">Například můžete snadno vytvořit identifikátory URI, které popisují hierarchie prostředků.</span><span class="sxs-lookup"><span data-stu-id="96d4f-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="96d4f-109">Starší styl směrování, volá se, založené na konvenci směrování, je stále plně podporovány.</span><span class="sxs-lookup"><span data-stu-id="96d4f-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="96d4f-110">Ve skutečnosti můžete kombinovat obě tyto metody ve stejném projektu.</span><span class="sxs-lookup"><span data-stu-id="96d4f-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="96d4f-111">Toto téma ukazuje, jak povolit směrování atributů a popisuje různé možnosti pro směrování atributů.</span><span class="sxs-lookup"><span data-stu-id="96d4f-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="96d4f-112">Kurz začátku do konce, který používá atribut směrování, najdete v tématu [vytvořit rozhraní REST API se směrováním atributů ve webovém rozhraní API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="96d4f-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="96d4f-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="96d4f-113">Prerequisites</span></span>

<span data-ttu-id="96d4f-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional nebo Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="96d4f-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="96d4f-115">Alternativně můžete použijte Správce balíčků NuGet nainstalujte potřebné balíčky.</span><span class="sxs-lookup"><span data-stu-id="96d4f-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="96d4f-116">Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**a pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="96d4f-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="96d4f-117">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="96d4f-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="96d4f-118">Proč atribut směrování?</span><span class="sxs-lookup"><span data-stu-id="96d4f-118">Why Attribute Routing?</span></span>

<span data-ttu-id="96d4f-119">První verze webového rozhraní API používá *podle úmluvy* směrování.</span><span class="sxs-lookup"><span data-stu-id="96d4f-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="96d4f-120">V tomto typu směrování můžete definovat jeden nebo více šablon trasy, které jsou v podstatě s parametry řetězce.</span><span class="sxs-lookup"><span data-stu-id="96d4f-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="96d4f-121">Žádost o přijetí rozhraní odpovídá identifikátoru URI pro šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="96d4f-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="96d4f-122">(Další informace o směrování založené na konvenci, naleznete v tématu [směrování v rozhraní ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="96d4f-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="96d4f-123">Jednou z výhod založené na konvenci směrování je, že šablony jsou definovány na jednom místě a pravidla směrování konzistentní napříč všemi řadiči.</span><span class="sxs-lookup"><span data-stu-id="96d4f-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="96d4f-124">Bohužel založené na konvenci směrování je obtížné podporovat některé URI vzorce, které jsou obvyklé v rozhraní RESTful API.</span><span class="sxs-lookup"><span data-stu-id="96d4f-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="96d4f-125">Například prostředky často obsahují podřízené prostředky: Zákazníci mají objednávky, actors mají filmy, knihy jste autoři a tak dále.</span><span class="sxs-lookup"><span data-stu-id="96d4f-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="96d4f-126">Je přirozeně k vytvoření identifikátorů URI, které zahrnují tyto vztahy:</span><span class="sxs-lookup"><span data-stu-id="96d4f-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="96d4f-127">Tento typ identifikátoru URI je těžké vytvořit pomocí směrování na základě konvence.</span><span class="sxs-lookup"><span data-stu-id="96d4f-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="96d4f-128">I když to můžete udělat, výsledky neškálují, pokud máte mnoho řadičů nebo typy prostředků.</span><span class="sxs-lookup"><span data-stu-id="96d4f-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="96d4f-129">Se směrováním atributů, že je jednoduché definovat trasu pro tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="96d4f-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="96d4f-130">Jednoduše přidejte atribut do akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="96d4f-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="96d4f-131">Tady jsou některé vzory, které atribut směrování umožňuje snadné.</span><span class="sxs-lookup"><span data-stu-id="96d4f-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="96d4f-132">**Správa verzí rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="96d4f-132">**API versioning**</span></span>

<span data-ttu-id="96d4f-133">V tomto příkladu "/ api/v1/produktů" bude směrovat do jiného řadiče než "/ api/v2/produktů".</span><span class="sxs-lookup"><span data-stu-id="96d4f-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="96d4f-134">**Přetížení identifikátoru URI segmenty**</span><span class="sxs-lookup"><span data-stu-id="96d4f-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="96d4f-135">V tomto příkladu "1" je číslo objednávky, ale "čekající na vyřízení" mapuje na kolekci.</span><span class="sxs-lookup"><span data-stu-id="96d4f-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="96d4f-136">**Více typy parametrů**</span><span class="sxs-lookup"><span data-stu-id="96d4f-136">**Multiple parameter types**</span></span>

<span data-ttu-id="96d4f-137">V tomto příkladu "1" je číslo objednávky, ale "06/2013/16" Určuje datum.</span><span class="sxs-lookup"><span data-stu-id="96d4f-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="96d4f-138">Povolení směrování atributů</span><span class="sxs-lookup"><span data-stu-id="96d4f-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="96d4f-139">Chcete-li povolit směrování atributů, zavolejte **MapHttpAttributeRoutes** během konfigurace.</span><span class="sxs-lookup"><span data-stu-id="96d4f-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="96d4f-140">Tato metoda rozšíření je definována v **System.Web.Http.HttpConfigurationExtensions** třídy.</span><span class="sxs-lookup"><span data-stu-id="96d4f-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="96d4f-141">Směrování atributů je možné kombinovat s [podle úmluvy](routing-in-aspnet-web-api.md) směrování.</span><span class="sxs-lookup"><span data-stu-id="96d4f-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="96d4f-142">Chcete-li definovat založené na konvenci směrování, zavolejte **MapHttpRoute** metody.</span><span class="sxs-lookup"><span data-stu-id="96d4f-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="96d4f-143">Další informace o konfiguraci webového rozhraní API najdete v tématu [konfigurace ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="96d4f-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="96d4f-144">Poznámka: Migrace z webového rozhraní API 1</span><span class="sxs-lookup"><span data-stu-id="96d4f-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="96d4f-145">Před webovým rozhraním API 2 šablony projektu webového rozhraní API generovaný kód následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="96d4f-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="96d4f-146">Pokud je povoleno směrování atributů, tento kód vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="96d4f-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="96d4f-147">Pokud provádíte upgrade existujícího projektu webového rozhraní API používat směrování atributů, nezapomeňte aktualizovat tento kód konfigurace takto:</span><span class="sxs-lookup"><span data-stu-id="96d4f-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="96d4f-148">Další informace najdete v tématu [konfigurace webového rozhraní API s hostování v technologii ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="96d4f-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="96d4f-149">Přidávání atributů trasy</span><span class="sxs-lookup"><span data-stu-id="96d4f-149">Adding Route Attributes</span></span>

<span data-ttu-id="96d4f-150">Tady je příklad postupu definovány pomocí atributu:</span><span class="sxs-lookup"><span data-stu-id="96d4f-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="96d4f-151">Řetězec &quot;zákazníci / {Idzakaznika} / orders&quot; je šablona identifikátoru URI pro trasu.</span><span class="sxs-lookup"><span data-stu-id="96d4f-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="96d4f-152">Webové rozhraní API se pokusí shodovat se identifikátoru URI požadavku do šablony.</span><span class="sxs-lookup"><span data-stu-id="96d4f-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="96d4f-153">V tomto příkladu "zákazníci" a "orders" je literál segmentů a "{Idzakaznika}" je parametrů proměnných.</span><span class="sxs-lookup"><span data-stu-id="96d4f-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="96d4f-154">Následující identifikátory URI by tato šablona porovnat:</span><span class="sxs-lookup"><span data-stu-id="96d4f-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="96d4f-155">Můžete omezit porovnávání pomocí [omezení](#constraints), které jsou popsány dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="96d4f-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="96d4f-156">Všimněte si, že &quot;{Idzakaznika}&quot; parametrů v šabloně postupu odpovídá názvu *customerId* parametr metody.</span><span class="sxs-lookup"><span data-stu-id="96d4f-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="96d4f-157">Webové rozhraní API vyvolá akce kontroleru, pokusí se vytvořit vazbu parametry trasy.</span><span class="sxs-lookup"><span data-stu-id="96d4f-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="96d4f-158">Například, pokud je identifikátor URI `http://example.com/customers/1/orders`, webové rozhraní API se pokusí vytvořit vazbu hodnotu "1" *customerId* parametr v akci.</span><span class="sxs-lookup"><span data-stu-id="96d4f-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="96d4f-159">Šablona identifikátoru URI může mít několik parametrů:</span><span class="sxs-lookup"><span data-stu-id="96d4f-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="96d4f-160">Všechny metody kontroleru, které nemají atribut trasy pomocí směrování na základě konvence.</span><span class="sxs-lookup"><span data-stu-id="96d4f-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="96d4f-161">Díky tomu můžete kombinovat oba typy směrování ve stejném projektu.</span><span class="sxs-lookup"><span data-stu-id="96d4f-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="96d4f-162">Metody HTTP</span><span class="sxs-lookup"><span data-stu-id="96d4f-162">HTTP Methods</span></span>

<span data-ttu-id="96d4f-163">Webové rozhraní API také vybere akce podle metody HTTP požadavku (GET, POST atd.).</span><span class="sxs-lookup"><span data-stu-id="96d4f-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="96d4f-164">Ve výchozím nastavení hledá webového rozhraní API nerozlišují se začátkem metoda názvu kontroleru.</span><span class="sxs-lookup"><span data-stu-id="96d4f-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="96d4f-165">Například kontroler metodu s názvem `PutCustomers` odpovídá požadavek HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="96d4f-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="96d4f-166">Tato konvence můžete přepsat pomocí upravení metodu s jakoukoli následující atributy:</span><span class="sxs-lookup"><span data-stu-id="96d4f-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="96d4f-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="96d4f-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="96d4f-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="96d4f-168">**[HttpGet]**</span></span>
- <span data-ttu-id="96d4f-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="96d4f-169">**[HttpHead]**</span></span>
- <span data-ttu-id="96d4f-170">**[Httpoptions měl]**</span><span class="sxs-lookup"><span data-stu-id="96d4f-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="96d4f-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="96d4f-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="96d4f-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="96d4f-172">**[HttpPost]**</span></span>
- <span data-ttu-id="96d4f-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="96d4f-173">**[HttpPut]**</span></span>

<span data-ttu-id="96d4f-174">V následujícím příkladu metoda CreateBook mapuje na požadavky HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="96d4f-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="96d4f-175">Pro všechny ostatní metody HTTP, včetně nestandardní metody, použijte **AcceptVerbs** atribut, který přebírá seznam metod HTTP.</span><span class="sxs-lookup"><span data-stu-id="96d4f-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="96d4f-176">Předpony trasy</span><span class="sxs-lookup"><span data-stu-id="96d4f-176">Route Prefixes</span></span>

<span data-ttu-id="96d4f-177">Trasy se často v kontroleru všechny začínají stejnou předponou.</span><span class="sxs-lookup"><span data-stu-id="96d4f-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="96d4f-178">Příklad:</span><span class="sxs-lookup"><span data-stu-id="96d4f-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="96d4f-179">Běžnou předponu můžete nastavit pro celý kontroler pomocí **[parametr RoutePrefix]** atribut:</span><span class="sxs-lookup"><span data-stu-id="96d4f-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="96d4f-180">Použijte tilda (~) na atribut method přepsání předpony trasy:</span><span class="sxs-lookup"><span data-stu-id="96d4f-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="96d4f-181">Předpona trasy může obsahovat parametry:</span><span class="sxs-lookup"><span data-stu-id="96d4f-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="96d4f-182">Omezení trasy</span><span class="sxs-lookup"><span data-stu-id="96d4f-182">Route Constraints</span></span>

<span data-ttu-id="96d4f-183">Omezení trasy umožňují omezit, jak jsou porovnány parametry v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="96d4f-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="96d4f-184">Je Obecná syntaxe &quot;{parametr: omezení}&quot;.</span><span class="sxs-lookup"><span data-stu-id="96d4f-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="96d4f-185">Příklad:</span><span class="sxs-lookup"><span data-stu-id="96d4f-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="96d4f-186">Tady, první trasa se vybrat pouze pokud &quot;id&quot; segment identifikátoru URI je celé číslo.</span><span class="sxs-lookup"><span data-stu-id="96d4f-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="96d4f-187">V opačném případě bude vybrán Druhá trasa.</span><span class="sxs-lookup"><span data-stu-id="96d4f-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="96d4f-188">Následující tabulka uvádí omezení, které jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="96d4f-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="96d4f-189">Omezení</span><span class="sxs-lookup"><span data-stu-id="96d4f-189">Constraint</span></span> | <span data-ttu-id="96d4f-190">Popis</span><span class="sxs-lookup"><span data-stu-id="96d4f-190">Description</span></span> | <span data-ttu-id="96d4f-191">Příklad</span><span class="sxs-lookup"><span data-stu-id="96d4f-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="96d4f-192">systém Alpha</span><span class="sxs-lookup"><span data-stu-id="96d4f-192">alpha</span></span> | <span data-ttu-id="96d4f-193">Odpovídá velká nebo malá latinská abeceda znaky (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="96d4f-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="96d4f-194">{x: alfa}</span><span class="sxs-lookup"><span data-stu-id="96d4f-194">{x:alpha}</span></span> |
| <span data-ttu-id="96d4f-195">bool</span><span class="sxs-lookup"><span data-stu-id="96d4f-195">bool</span></span> | <span data-ttu-id="96d4f-196">Odpovídá hodnotu typu Boolean.</span><span class="sxs-lookup"><span data-stu-id="96d4f-196">Matches a Boolean value.</span></span> | <span data-ttu-id="96d4f-197">{x: bool}</span><span class="sxs-lookup"><span data-stu-id="96d4f-197">{x:bool}</span></span> |
| <span data-ttu-id="96d4f-198">Datum a čas</span><span class="sxs-lookup"><span data-stu-id="96d4f-198">datetime</span></span> | <span data-ttu-id="96d4f-199">Shody **data a času** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="96d4f-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="96d4f-200">{x: datetime}</span><span class="sxs-lookup"><span data-stu-id="96d4f-200">{x:datetime}</span></span> |
| <span data-ttu-id="96d4f-201">decimal</span><span class="sxs-lookup"><span data-stu-id="96d4f-201">decimal</span></span> | <span data-ttu-id="96d4f-202">Odpovídá desítkovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="96d4f-202">Matches a decimal value.</span></span> | <span data-ttu-id="96d4f-203">{x: decimal}</span><span class="sxs-lookup"><span data-stu-id="96d4f-203">{x:decimal}</span></span> |
| <span data-ttu-id="96d4f-204">double</span><span class="sxs-lookup"><span data-stu-id="96d4f-204">double</span></span> | <span data-ttu-id="96d4f-205">Odpovídá 64 bitů hodnotu s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="96d4f-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="96d4f-206">{x: double}</span><span class="sxs-lookup"><span data-stu-id="96d4f-206">{x:double}</span></span> |
| <span data-ttu-id="96d4f-207">float</span><span class="sxs-lookup"><span data-stu-id="96d4f-207">float</span></span> | <span data-ttu-id="96d4f-208">Odpovídá 32bitová hodnota s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="96d4f-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="96d4f-209">{x: float}</span><span class="sxs-lookup"><span data-stu-id="96d4f-209">{x:float}</span></span> |
| <span data-ttu-id="96d4f-210">identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="96d4f-210">guid</span></span> | <span data-ttu-id="96d4f-211">Odpovídá hodnotě identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="96d4f-211">Matches a GUID value.</span></span> | <span data-ttu-id="96d4f-212">{x: guid}</span><span class="sxs-lookup"><span data-stu-id="96d4f-212">{x:guid}</span></span> |
| <span data-ttu-id="96d4f-213">int</span><span class="sxs-lookup"><span data-stu-id="96d4f-213">int</span></span> | <span data-ttu-id="96d4f-214">Odpovídá hodnotě 32bitové celé číslo.</span><span class="sxs-lookup"><span data-stu-id="96d4f-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="96d4f-215">{x: int}</span><span class="sxs-lookup"><span data-stu-id="96d4f-215">{x:int}</span></span> |
| <span data-ttu-id="96d4f-216">length</span><span class="sxs-lookup"><span data-stu-id="96d4f-216">length</span></span> | <span data-ttu-id="96d4f-217">Odpovídá řetězci se zadanou délkou nebo do zadaného rozsahu délek.</span><span class="sxs-lookup"><span data-stu-id="96d4f-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="96d4f-218">{x: length(6)} {x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="96d4f-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="96d4f-219">long</span><span class="sxs-lookup"><span data-stu-id="96d4f-219">long</span></span> | <span data-ttu-id="96d4f-220">Odpovídá hodnotě 64bitové celé číslo.</span><span class="sxs-lookup"><span data-stu-id="96d4f-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="96d4f-221">{x: long}</span><span class="sxs-lookup"><span data-stu-id="96d4f-221">{x:long}</span></span> |
| <span data-ttu-id="96d4f-222">max</span><span class="sxs-lookup"><span data-stu-id="96d4f-222">max</span></span> | <span data-ttu-id="96d4f-223">Odpovídá celé číslo s maximální hodnotou.</span><span class="sxs-lookup"><span data-stu-id="96d4f-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="96d4f-224">{x: max(10)}</span><span class="sxs-lookup"><span data-stu-id="96d4f-224">{x:max(10)}</span></span> |
| <span data-ttu-id="96d4f-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="96d4f-225">maxlength</span></span> | <span data-ttu-id="96d4f-226">Odpovídá řetězci s maximální délkou.</span><span class="sxs-lookup"><span data-stu-id="96d4f-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="96d4f-227">{x: maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="96d4f-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="96d4f-228">min</span><span class="sxs-lookup"><span data-stu-id="96d4f-228">min</span></span> | <span data-ttu-id="96d4f-229">Odpovídá celé číslo s minimální hodnotou.</span><span class="sxs-lookup"><span data-stu-id="96d4f-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="96d4f-230">{x: min(10)}</span><span class="sxs-lookup"><span data-stu-id="96d4f-230">{x:min(10)}</span></span> |
| <span data-ttu-id="96d4f-231">MinLength</span><span class="sxs-lookup"><span data-stu-id="96d4f-231">minlength</span></span> | <span data-ttu-id="96d4f-232">Odpovídá řetězci s minimální délkou.</span><span class="sxs-lookup"><span data-stu-id="96d4f-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="96d4f-233">{x: minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="96d4f-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="96d4f-234">range</span><span class="sxs-lookup"><span data-stu-id="96d4f-234">range</span></span> | <span data-ttu-id="96d4f-235">Odpovídá celé číslo v rozsahu hodnot.</span><span class="sxs-lookup"><span data-stu-id="96d4f-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="96d4f-236">{x: range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="96d4f-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="96d4f-237">regulární výraz</span><span class="sxs-lookup"><span data-stu-id="96d4f-237">regex</span></span> | <span data-ttu-id="96d4f-238">Odpovídá regulárnímu výrazu.</span><span class="sxs-lookup"><span data-stu-id="96d4f-238">Matches a regular expression.</span></span> | <span data-ttu-id="96d4f-239">{x: regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="96d4f-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="96d4f-240">Všimněte si, že některá omezení, jako například &quot;min&quot;, nepřebírají argumenty v závorkách.</span><span class="sxs-lookup"><span data-stu-id="96d4f-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="96d4f-241">Více omezení můžete použít parametr, oddělených středníkem.</span><span class="sxs-lookup"><span data-stu-id="96d4f-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="96d4f-242">Omezení vlastní trasy</span><span class="sxs-lookup"><span data-stu-id="96d4f-242">Custom Route Constraints</span></span>

<span data-ttu-id="96d4f-243">Můžete vytvořit vlastní trasy omezení implementací **IHttpRouteConstraint** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="96d4f-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="96d4f-244">Například následující omezení omezuje parametr na nenulovou celočíselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="96d4f-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="96d4f-245">Následující kód ukazuje, jak zaregistrovat omezení:</span><span class="sxs-lookup"><span data-stu-id="96d4f-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="96d4f-246">Teď můžete použít omezení v trasy:</span><span class="sxs-lookup"><span data-stu-id="96d4f-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="96d4f-247">Můžete také nahradit celý **DefaultInlineConstraintResolver** třídy implementací **IInlineConstraintResolver** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="96d4f-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="96d4f-248">Tím dojde k nahrazení všech předdefinovaných omezujících podmínek, není-li vaše implementace **IInlineConstraintResolver** konkrétně se přidají.</span><span class="sxs-lookup"><span data-stu-id="96d4f-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="96d4f-249">Parametry identifikátoru URI nepovinný a výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="96d4f-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="96d4f-250">Volitelný parametr URI můžete provést přidáním otazník do parametru trasy.</span><span class="sxs-lookup"><span data-stu-id="96d4f-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="96d4f-251">Pokud je určitý parametr trasy nepovinný, je nutné definovat výchozí hodnotu pro parametr metody.</span><span class="sxs-lookup"><span data-stu-id="96d4f-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="96d4f-252">V tomto příkladu `/api/books/locale/1033` a `/api/books/locale` vrátit stejný prostředek.</span><span class="sxs-lookup"><span data-stu-id="96d4f-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="96d4f-253">Alternativně můžete zadat výchozí hodnotu v šabloně trasy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="96d4f-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="96d4f-254">Toto je téměř stejný jako předchozí příklad, ale neexistuje malého rozdílu chování při použití výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="96d4f-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="96d4f-255">V prvním příkladu ("{lcid?}") je přiřazen výchozí hodnotu 1033 přímo do parametru metody tak, že parametr bude mít tento přesná hodnota.</span><span class="sxs-lookup"><span data-stu-id="96d4f-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="96d4f-256">V druhém příkladu ("{lcid = 1033}"), výchozí hodnotu "1033" projde procesem vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="96d4f-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="96d4f-257">Vazač modelu výchozí se převede na číselnou hodnotu 1033 "1033".</span><span class="sxs-lookup"><span data-stu-id="96d4f-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="96d4f-258">Může však zapojte vlastní vazač modelu, který může dělat něco jiného.</span><span class="sxs-lookup"><span data-stu-id="96d4f-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="96d4f-259">(Ve většině případů, pokud nemáte vlastního modelu pořadače ve vašem kanálu dvě různými formami bude ekvivalentní.)</span><span class="sxs-lookup"><span data-stu-id="96d4f-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="96d4f-260">Názvy tras</span><span class="sxs-lookup"><span data-stu-id="96d4f-260">Route Names</span></span>

<span data-ttu-id="96d4f-261">Všechny trasy v rozhraní Web API, má název.</span><span class="sxs-lookup"><span data-stu-id="96d4f-261">In Web API, every route has a name.</span></span> <span data-ttu-id="96d4f-262">Názvy tras jsou užitečné pro generování odkazů, takže můžete uvést odkaz v odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="96d4f-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="96d4f-263">Chcete-li zadat název trasy, nastavte **název** vlastnost pro atribut.</span><span class="sxs-lookup"><span data-stu-id="96d4f-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="96d4f-264">Následující příklad ukazuje, jak nastavit název trasy a také použití název trasy, která při generování odkazu.</span><span class="sxs-lookup"><span data-stu-id="96d4f-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="96d4f-265">Pořadí trasy</span><span class="sxs-lookup"><span data-stu-id="96d4f-265">Route Order</span></span>

<span data-ttu-id="96d4f-266">Rozhraní se pokusí tak, aby odpovídaly identifikátor URI s trasy, je vyhodnocen jako trasy v určitém pořadí.</span><span class="sxs-lookup"><span data-stu-id="96d4f-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="96d4f-267">Chcete-li určit pořadí, nastavte **RouteOrder** vlastnost pro atribut trasy.</span><span class="sxs-lookup"><span data-stu-id="96d4f-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="96d4f-268">Nižší hodnoty jsou vyhodnocen jako první.</span><span class="sxs-lookup"><span data-stu-id="96d4f-268">Lower values are evaluated first.</span></span> <span data-ttu-id="96d4f-269">Výchozí hodnota pořadí je nula.</span><span class="sxs-lookup"><span data-stu-id="96d4f-269">The default order value is zero.</span></span>

<span data-ttu-id="96d4f-270">Zde je, jak se určuje celkový počet pořadí:</span><span class="sxs-lookup"><span data-stu-id="96d4f-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="96d4f-271">Porovnání **RouteOrder** vlastnost atribut trasy.</span><span class="sxs-lookup"><span data-stu-id="96d4f-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="96d4f-272">Podívejte se na každý segment identifikátoru URI v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="96d4f-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="96d4f-273">Pro každý segment pořadí následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="96d4f-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="96d4f-274">Literál segmenty.</span><span class="sxs-lookup"><span data-stu-id="96d4f-274">Literal segments.</span></span>
    2. <span data-ttu-id="96d4f-275">Parametry trasy s omezeními.</span><span class="sxs-lookup"><span data-stu-id="96d4f-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="96d4f-276">Parametry trasy bez omezení.</span><span class="sxs-lookup"><span data-stu-id="96d4f-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="96d4f-277">Zástupný parametr segmenty s omezeními.</span><span class="sxs-lookup"><span data-stu-id="96d4f-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="96d4f-278">Parametr segmenty zástupný znak bez omezení.</span><span class="sxs-lookup"><span data-stu-id="96d4f-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="96d4f-279">V případě rovnosti, trasy jsou řazeny podle porovnání nerozlišuje velikost písmen řetězců podle pořadového čísla ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) šablony trasy.</span><span class="sxs-lookup"><span data-stu-id="96d4f-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="96d4f-280">Zde je příklad.</span><span class="sxs-lookup"><span data-stu-id="96d4f-280">Here is an example.</span></span> <span data-ttu-id="96d4f-281">Předpokládejme, že můžete definovat následující kontroler:</span><span class="sxs-lookup"><span data-stu-id="96d4f-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="96d4f-282">Tyto postupy jsou uspořádaná následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="96d4f-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="96d4f-283">Příkazy a podrobnosti</span><span class="sxs-lookup"><span data-stu-id="96d4f-283">orders/details</span></span>
2. <span data-ttu-id="96d4f-284">objednávky / {id}</span><span class="sxs-lookup"><span data-stu-id="96d4f-284">orders/{id}</span></span>
3. <span data-ttu-id="96d4f-285">objednávky / {customerName}</span><span class="sxs-lookup"><span data-stu-id="96d4f-285">orders/{customerName}</span></span>
4. <span data-ttu-id="96d4f-286">objednávky / {\*datum}</span><span class="sxs-lookup"><span data-stu-id="96d4f-286">orders/{\*date}</span></span>
5. <span data-ttu-id="96d4f-287">objednávky / čekající na vyřízení</span><span class="sxs-lookup"><span data-stu-id="96d4f-287">orders/pending</span></span>

<span data-ttu-id="96d4f-288">Všimněte si, že "details" je segmentů literálů a před {id}"se zobrazí, ale"až"se zobrazí poslední vzhledem k tomu, **RouteOrder** vlastnost je 1.</span><span class="sxs-lookup"><span data-stu-id="96d4f-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="96d4f-289">(Tento příklad předpokládá existuje se žádní zákazníci s názvem "details" nebo "čeká na vyřízení".</span><span class="sxs-lookup"><span data-stu-id="96d4f-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="96d4f-290">Obecně platí snažte se vyhnout nejednoznačný trasy.</span><span class="sxs-lookup"><span data-stu-id="96d4f-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="96d4f-291">V tomto příkladu lepší šablona trasy pro `GetByCustomer` je "zákazníkům / {customerName}")</span><span class="sxs-lookup"><span data-stu-id="96d4f-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
