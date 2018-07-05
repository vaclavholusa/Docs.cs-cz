---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Směrování a výběr akce v rozhraní ASP.NET Web API | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: dc0edbdf62ceab1baf441301b47c0293de9a5c5d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388611"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="39558-102">Směrování a výběr akce v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="39558-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="39558-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="39558-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="39558-104">Tento článek popisuje, jak rozhraní ASP.NET Web API směruje požadavek HTTP na provedení určité akce v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="39558-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="39558-105">Přehled směrování, naleznete v tématu [směrování v rozhraní ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="39558-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="39558-106">Tento článek ukazuje podrobnosti o procesu směrování.</span><span class="sxs-lookup"><span data-stu-id="39558-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="39558-107">Pokud vytváříte projekt webového rozhraní API a zjistíte, že některé požadavky Nezískávat směrovány očekávaným způsobem, snad Tento článek vám pomůže.</span><span class="sxs-lookup"><span data-stu-id="39558-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="39558-108">Směrování má tři hlavní fáze:</span><span class="sxs-lookup"><span data-stu-id="39558-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="39558-109">Shodujícím se identifikátoru URI pro šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="39558-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="39558-110">Výběr kontroleru.</span><span class="sxs-lookup"><span data-stu-id="39558-110">Selecting a controller.</span></span>
3. <span data-ttu-id="39558-111">Výběr akce.</span><span class="sxs-lookup"><span data-stu-id="39558-111">Selecting an action.</span></span>

<span data-ttu-id="39558-112">Některé části procesu můžete nahradit vlastní vlastní chování.</span><span class="sxs-lookup"><span data-stu-id="39558-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="39558-113">V tomto článku popisují I výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="39558-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="39558-114">Na konci můžu mějte na paměti na místech, kde můžete přizpůsobit chování.</span><span class="sxs-lookup"><span data-stu-id="39558-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="39558-115">Šablony trasy</span><span class="sxs-lookup"><span data-stu-id="39558-115">Route Templates</span></span>

<span data-ttu-id="39558-116">Šablona trasy vypadá podobně jako na cestu URI, ale může mít hodnoty zástupných symbolů se složených závorek:</span><span class="sxs-lookup"><span data-stu-id="39558-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="39558-117">Při vytváření trasy můžete zadat výchozí hodnoty pro některé nebo všechny zástupné symboly:</span><span class="sxs-lookup"><span data-stu-id="39558-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="39558-118">Můžete také zadat omezení, které omezují jak segment identifikátoru URI může odpovídat zástupný symbol:</span><span class="sxs-lookup"><span data-stu-id="39558-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="39558-119">Rozhraní se pokusí vyhledat segmentů v cestě identifikátor URI v šabloně.</span><span class="sxs-lookup"><span data-stu-id="39558-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="39558-120">Literály v šabloně musí přesně odpovídat.</span><span class="sxs-lookup"><span data-stu-id="39558-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="39558-121">Zástupný text odpovídá libovolné hodnotě, není-li zadat omezení.</span><span class="sxs-lookup"><span data-stu-id="39558-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="39558-122">Rozhraní se neshoduje s jiné části identifikátoru URI, jako je název hostitele nebo parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="39558-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="39558-123">Rozhraní framework vybere první trasy ve směrovací tabulce, která odpovídá identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="39558-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="39558-124">Existují dva speciální zástupné znaky: "{controller}" a "{action}".</span><span class="sxs-lookup"><span data-stu-id="39558-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="39558-125">"{controller}" obsahuje název kontroleru.</span><span class="sxs-lookup"><span data-stu-id="39558-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="39558-126">"{action} obsahuje název akce.</span><span class="sxs-lookup"><span data-stu-id="39558-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="39558-127">V rozhraní Web API je "{action} vynechejte běžné konvence.</span><span class="sxs-lookup"><span data-stu-id="39558-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="39558-128">Ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="39558-128">Defaults</span></span>

<span data-ttu-id="39558-129">Pokud zadáte výchozí hodnoty, bude odpovídat trasy identifikátor URI, který neobsahuje tyto segmenty.</span><span class="sxs-lookup"><span data-stu-id="39558-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="39558-130">Příklad:</span><span class="sxs-lookup"><span data-stu-id="39558-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="39558-131">Identifikátor URI "`http://localhost/api/products`" odpovídá této trase.</span><span class="sxs-lookup"><span data-stu-id="39558-131">The URI "`http://localhost/api/products`" matches this route.</span></span> <span data-ttu-id="39558-132">Segment "{category}" je přiřazena "vše" Výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="39558-132">The "{category}" segment is assigned the default value "all".</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="39558-133">Slovníku trasy</span><span class="sxs-lookup"><span data-stu-id="39558-133">Route Dictionary</span></span>

<span data-ttu-id="39558-134">Pokud rozhraní najde shodu pro identifikátor URI, vytvoří slovník, který obsahuje hodnotu pro každý zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="39558-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="39558-135">Klíče jsou zástupné názvy, bez složených závorek.</span><span class="sxs-lookup"><span data-stu-id="39558-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="39558-136">Hodnoty pocházejí z cesty identifikátoru URI nebo z výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="39558-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="39558-137">Slovník je uložen v **IHttpRouteData** objektu.</span><span class="sxs-lookup"><span data-stu-id="39558-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="39558-138">V této fázi odpovídající trasy speciální "{controller}" a "{action} zástupné symboly zachází stejně jako jiné zástupné symboly.</span><span class="sxs-lookup"><span data-stu-id="39558-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="39558-139">Jednoduše se ukládají ve slovníku s jiné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="39558-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="39558-140">Výchozí může mít zvláštní hodnota **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="39558-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="39558-141">Pokud zástupný symbol získá přiřadí tuto hodnotu, hodnota není přidán do slovníku trasy.</span><span class="sxs-lookup"><span data-stu-id="39558-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="39558-142">Příklad:</span><span class="sxs-lookup"><span data-stu-id="39558-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="39558-143">Pro identifikátor URI cesty "/ produktů s rozhraním api" bude obsahovat slovníku trasy:</span><span class="sxs-lookup"><span data-stu-id="39558-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="39558-144">kontroler: "produktů"</span><span class="sxs-lookup"><span data-stu-id="39558-144">controller: "products"</span></span>
- <span data-ttu-id="39558-145">kategorie: "vše"</span><span class="sxs-lookup"><span data-stu-id="39558-145">category: "all"</span></span>

<span data-ttu-id="39558-146">"Api/produkty/toys/123" ale slovníku trasy obsahovat:</span><span class="sxs-lookup"><span data-stu-id="39558-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="39558-147">kontroler: "produktů"</span><span class="sxs-lookup"><span data-stu-id="39558-147">controller: "products"</span></span>
- <span data-ttu-id="39558-148">kategorie: "toys"</span><span class="sxs-lookup"><span data-stu-id="39558-148">category: "toys"</span></span>
- <span data-ttu-id="39558-149">ID: "123"</span><span class="sxs-lookup"><span data-stu-id="39558-149">id: "123"</span></span>

<span data-ttu-id="39558-150">Výchozí hodnoty může také obsahovat hodnotu, která se nezobrazí kdekoli v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="39558-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="39558-151">Pokud trasa odpovídá, tato hodnota je uložena ve slovníku.</span><span class="sxs-lookup"><span data-stu-id="39558-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="39558-152">Příklad:</span><span class="sxs-lookup"><span data-stu-id="39558-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="39558-153">Pokud cesta k identifikátoru URI je "rozhraní api/root/8", slovníku obsahovat dvě hodnoty:</span><span class="sxs-lookup"><span data-stu-id="39558-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="39558-154">kontroler: "zákazníci"</span><span class="sxs-lookup"><span data-stu-id="39558-154">controller: "customers"</span></span>
- <span data-ttu-id="39558-155">ID: "8"</span><span class="sxs-lookup"><span data-stu-id="39558-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="39558-156">Vyberte kontroler</span><span class="sxs-lookup"><span data-stu-id="39558-156">Selecting a Controller</span></span>

<span data-ttu-id="39558-157">Výběr řadiče se zpracovává souborem **IHttpControllerSelector.SelectController** metody.</span><span class="sxs-lookup"><span data-stu-id="39558-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="39558-158">Tato metoda přebírá **HttpRequestMessage** instance a vrátí **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="39558-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="39558-159">Poskytuje výchozí implementaci **DefaultHttpControllerSelector** třídy.</span><span class="sxs-lookup"><span data-stu-id="39558-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="39558-160">Tato třída používá jednoduché algoritmus:</span><span class="sxs-lookup"><span data-stu-id="39558-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="39558-161">Podívejte se ve slovníku trasy pro klíč "controller".</span><span class="sxs-lookup"><span data-stu-id="39558-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="39558-162">Přijmout hodnotu pro tento klíč a připojte řetězec "Controller" získat tak název typu kontroleru.</span><span class="sxs-lookup"><span data-stu-id="39558-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="39558-163">Vyhledejte kontroler Web API s tímto názvem typu.</span><span class="sxs-lookup"><span data-stu-id="39558-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="39558-164">Například pokud slovníku trasy obsahuje pár klíč hodnota "controller" = "produktů", "ProductsController" je typ kontroleru.</span><span class="sxs-lookup"><span data-stu-id="39558-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="39558-165">Pokud neexistuje žádný odpovídající typ nebo více shod, rozhraní chybovou zprávu do klienta.</span><span class="sxs-lookup"><span data-stu-id="39558-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="39558-166">Krok 3 **DefaultHttpControllerSelector** používá **IHttpControllerTypeResolver** rozhraní se získat seznam typy kontrolerů webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="39558-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="39558-167">Výchozí implementace **IHttpControllerTypeResolver** vrátí všechny veřejné třídy, které implementují (a) **IHttpController**, (b) není abstraktní a (c) mít název, který končí na "Controller".</span><span class="sxs-lookup"><span data-stu-id="39558-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="39558-168">Výběr akce</span><span class="sxs-lookup"><span data-stu-id="39558-168">Action Selection</span></span>

<span data-ttu-id="39558-169">Po výběru kontroler rozhraní vybere akci voláním **IHttpActionSelector.SelectAction** metody.</span><span class="sxs-lookup"><span data-stu-id="39558-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="39558-170">Tato metoda přebírá **HttpControllerContext** a vrátí **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="39558-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="39558-171">Poskytuje výchozí implementaci **ApiControllerActionSelector** třídy.</span><span class="sxs-lookup"><span data-stu-id="39558-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="39558-172">Pokud chcete vybrat akci, dohlíží na následující:</span><span class="sxs-lookup"><span data-stu-id="39558-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="39558-173">Metoda HTTP požadavku.</span><span class="sxs-lookup"><span data-stu-id="39558-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="39558-174">Zástupný text "{action}" v šabloně trasy, pokud jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="39558-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="39558-175">Parametry akce v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="39558-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="39558-176">Před zobrazením algoritmus výběru, Nejdřív musíte seznámit pár věcí o akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="39558-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="39558-177">**Které metody na řadiči, jsou považovány za "akce"?**</span><span class="sxs-lookup"><span data-stu-id="39558-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="39558-178">Když vyberete akci, rozhraní pouze prohledá veřejné instanci metody ve kontroleru.</span><span class="sxs-lookup"><span data-stu-id="39558-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="39558-179">Také, vyloučí ["speciální název"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) metod (konstruktorů, události, přetížení operátoru a tak dále) a metod zděděných z **objektu ApiController** třídy.</span><span class="sxs-lookup"><span data-stu-id="39558-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="39558-180">**Metody HTTP.**</span><span class="sxs-lookup"><span data-stu-id="39558-180">**HTTP Methods.**</span></span> <span data-ttu-id="39558-181">Rozhraní framework vybírá pouze akce, které odpovídají metoda HTTP požadavku stanoven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="39558-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="39558-182">Můžete určit metodu HTTP s atributem: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **Httpoptions měl**, **HttpPatch**, **HttpPost**, nebo **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="39558-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="39558-183">Jinak Pokud název metody kontroleru začíná řetězcem "Get", "Post", "Vložit", "Odstranit", "Head", "Options" nebo "Opravnou", pak podle konvence akci podporuje metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="39558-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="39558-184">Pokud žádná z výše uvedených podporuje metodu POST.</span><span class="sxs-lookup"><span data-stu-id="39558-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="39558-185">**Vazby parametru.**</span><span class="sxs-lookup"><span data-stu-id="39558-185">**Parameter Bindings.**</span></span> <span data-ttu-id="39558-186">Vazby parametru je způsob, jak vytvořit hodnotu pro parametr webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="39558-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="39558-187">Tady je výchozí pravidlo pro vazbu parametru:</span><span class="sxs-lookup"><span data-stu-id="39558-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="39558-188">Jednoduché typy pocházejí z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="39558-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="39558-189">Komplexní typy pocházejí z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="39558-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="39558-190">Jednoduché typy zahrnují všechny [primitivní typy rozhraní .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), plus **data a času**, **desítkové**, **Guid**, **řetězec** , a **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="39558-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="39558-191">Pro každou akci můžete přečíst maximálně jeden parametr těla požadavku.</span><span class="sxs-lookup"><span data-stu-id="39558-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="39558-192">Je možné přepsat výchozí pravidla pro vazbu.</span><span class="sxs-lookup"><span data-stu-id="39558-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="39558-193">Zobrazit [vazbu parametru WebAPI pod pokličkou](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="39558-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="39558-194">Tedy tady je výběr algoritmu akce.</span><span class="sxs-lookup"><span data-stu-id="39558-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="39558-195">Vytvoří seznam všech akcí na řadiči, které odpovídají metoda požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="39558-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="39558-196">Pokud slovníku trasy má záznam "action", odeberte akce, jejíž název se neshoduje s Tato hodnota.</span><span class="sxs-lookup"><span data-stu-id="39558-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="39558-197">Vyzkoušejte tak, aby odpovídaly parametry akce na identifikátor URI, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="39558-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="39558-198">Pro každou akci Získejte seznam parametrů, které jsou jednoduchý typ, pokud vazba získá parametr z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="39558-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="39558-199">Vylučte volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="39558-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="39558-200">Z tohoto seznamu pokuste se najít shodu pro každý název parametru ve slovníku trasy nebo v řetězci dotazu identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="39558-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="39558-201">Shody jsou malá a velká písmena a nezávisí na pořadí parametrů.</span><span class="sxs-lookup"><span data-stu-id="39558-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="39558-202">Vyberte akci, kde každý parametr v seznamu existuje shoda v identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="39558-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="39558-203">Pokud více tuto jednu akci se bude splňovat tyto podmínky, vyberte si jednu s nejvíce odpovídá parametru.</span><span class="sxs-lookup"><span data-stu-id="39558-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="39558-204">Ignorovat akce s **[NonAction]** atribut.</span><span class="sxs-lookup"><span data-stu-id="39558-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="39558-205">Krok #3 je pravděpodobně matoucí nejvíce.</span><span class="sxs-lookup"><span data-stu-id="39558-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="39558-206">Základní myšlenka je, že parametr dostat jeho hodnotu z identifikátoru URI, z textu požadavku nebo z vlastní vazby.</span><span class="sxs-lookup"><span data-stu-id="39558-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="39558-207">Pro parametry, které pochází z identifikátoru URI chceme se ujistit, že identifikátor URI ve skutečnosti obsahuje hodnotu pro tento parametr, a to buď v cestě (pomocí slovníku trasy), nebo v řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="39558-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="39558-208">Představte si třeba následující akce:</span><span class="sxs-lookup"><span data-stu-id="39558-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="39558-209">*Id* vazby parametru na identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="39558-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="39558-210">Proto tato akce může odpovídat pouze identifikátor URI, který obsahuje hodnotu pro "id" ve slovníku trasy nebo v řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="39558-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="39558-211">Volitelné parametry jsou výjimku, protože to jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="39558-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="39558-212">Pro volitelný parametr je OK, pokud vazby nelze získat hodnotu z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="39558-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="39558-213">Komplexní typy jsou výjimky pro jiný důvod.</span><span class="sxs-lookup"><span data-stu-id="39558-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="39558-214">Komplexní typ může být svázán pouze k identifikátoru URI prostřednictvím vlastní vazby.</span><span class="sxs-lookup"><span data-stu-id="39558-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="39558-215">Ale v takovém případě rozhraní nemůže předem vědět, jestli parametr by svázat s konkrétní identifikátorem URI.</span><span class="sxs-lookup"><span data-stu-id="39558-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="39558-216">Pokud chcete zjistit, je nutné vyvolat vazby.</span><span class="sxs-lookup"><span data-stu-id="39558-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="39558-217">Cílem algoritmus výběru je ze statické popis před vyvoláním žádné vazby. Vyberte akci.</span><span class="sxs-lookup"><span data-stu-id="39558-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="39558-218">Proto komplexní typy jsou vyloučeny z porovnávací algoritmus.</span><span class="sxs-lookup"><span data-stu-id="39558-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="39558-219">Po výběru akce jsou vyvolány všechny vazby parametru.</span><span class="sxs-lookup"><span data-stu-id="39558-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="39558-220">Shrnutí:</span><span class="sxs-lookup"><span data-stu-id="39558-220">Summary:</span></span>

- <span data-ttu-id="39558-221">Metoda HTTP požadavku musí odpovídat akce.</span><span class="sxs-lookup"><span data-stu-id="39558-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="39558-222">Název akce musí odpovídat "action" položky ve slovníku trasy, pokud jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="39558-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="39558-223">Pro každý parametr akce Pokud tento parametr je převzata z identifikátoru URI, pak název parametru musí najít ve slovníku trasy nebo v řetězci dotazu identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="39558-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="39558-224">(Volitelné parametry a parametry s komplexní typy jsou vyloučeny.)</span><span class="sxs-lookup"><span data-stu-id="39558-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="39558-225">Pokuste se shodovat s největším počtem parametrů.</span><span class="sxs-lookup"><span data-stu-id="39558-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="39558-226">Nejlepší shody může být metoda bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="39558-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="39558-227">Rozšířený příklad</span><span class="sxs-lookup"><span data-stu-id="39558-227">Extended Example</span></span>

<span data-ttu-id="39558-228">Trasy:</span><span class="sxs-lookup"><span data-stu-id="39558-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="39558-229">Kontroler:</span><span class="sxs-lookup"><span data-stu-id="39558-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="39558-230">Požadavek protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="39558-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="39558-231">Odpovídající trasy</span><span class="sxs-lookup"><span data-stu-id="39558-231">Route Matching</span></span>

<span data-ttu-id="39558-232">Identifikátor URI odpovídá trasa s názvem "DefaultApi".</span><span class="sxs-lookup"><span data-stu-id="39558-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="39558-233">Slovníku trasy obsahuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="39558-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="39558-234">kontroler: "produktů"</span><span class="sxs-lookup"><span data-stu-id="39558-234">controller: "products"</span></span>
- <span data-ttu-id="39558-235">ID: "1"</span><span class="sxs-lookup"><span data-stu-id="39558-235">id: "1"</span></span>

<span data-ttu-id="39558-236">Slovníku trasy neobsahuje parametry řetězce dotazu, "verze" a "details", ale ty budou mít nadále za během výběr akce.</span><span class="sxs-lookup"><span data-stu-id="39558-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="39558-237">Výběr řadiče</span><span class="sxs-lookup"><span data-stu-id="39558-237">Controller Selection</span></span>

<span data-ttu-id="39558-238">Z položky "controller" ve slovníku trasy, je typ kontroleru `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="39558-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="39558-239">Výběr akce</span><span class="sxs-lookup"><span data-stu-id="39558-239">Action Selection</span></span>

<span data-ttu-id="39558-240">Požadavek HTTP je požadavek GET.</span><span class="sxs-lookup"><span data-stu-id="39558-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="39558-241">Jsou akce kontroleru, které podporují GET `GetAll`, `GetById`, a `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="39558-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="39558-242">Slovníku trasy neobsahuje položku pro "action", proto nepotřebujeme tak, aby odpovídaly názvu akce.</span><span class="sxs-lookup"><span data-stu-id="39558-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="39558-243">Dále pokusíme se porovnat názvy parametrů pro akce, pouze na akce GET.</span><span class="sxs-lookup"><span data-stu-id="39558-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="39558-244">Akce</span><span class="sxs-lookup"><span data-stu-id="39558-244">Action</span></span> | <span data-ttu-id="39558-245">Parametry shody</span><span class="sxs-lookup"><span data-stu-id="39558-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="39558-246">žádná</span><span class="sxs-lookup"><span data-stu-id="39558-246">none</span></span> |
| `GetById` | <span data-ttu-id="39558-247">"id"</span><span class="sxs-lookup"><span data-stu-id="39558-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="39558-248">"název"</span><span class="sxs-lookup"><span data-stu-id="39558-248">"name"</span></span> |

<span data-ttu-id="39558-249">Všimněte si, *verze* parametr `GetById` nepovažuje, protože se jedná volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="39558-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="39558-250">`GetAll` Metoda odpovídá triviálně.</span><span class="sxs-lookup"><span data-stu-id="39558-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="39558-251">`GetById` Metoda také odpovídající, protože obsahuje "id" slovníku trasy.</span><span class="sxs-lookup"><span data-stu-id="39558-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="39558-252">`FindProductsByName` Metoda se neshoduje.</span><span class="sxs-lookup"><span data-stu-id="39558-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="39558-253">`GetById` Metoda služby wins, protože odpovídá jeden parametr, a bez parametrů pro `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="39558-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="39558-254">Metoda je volána s následující hodnoty parametrů:</span><span class="sxs-lookup"><span data-stu-id="39558-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="39558-255">*ID* = 1</span><span class="sxs-lookup"><span data-stu-id="39558-255">*id* = 1</span></span>
- <span data-ttu-id="39558-256">*verze* = 1.5</span><span class="sxs-lookup"><span data-stu-id="39558-256">*version* = 1.5</span></span>

<span data-ttu-id="39558-257">Všimněte si, že i když *verze* nebyl použit algoritmus výběru pochází hodnotu parametru řetězce dotazu URI.</span><span class="sxs-lookup"><span data-stu-id="39558-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="39558-258">Rozšiřovací body</span><span class="sxs-lookup"><span data-stu-id="39558-258">Extension Points</span></span>

<span data-ttu-id="39558-259">Webové rozhraní API poskytuje Rozšiřovací body pro některé části procesu směrování.</span><span class="sxs-lookup"><span data-stu-id="39558-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="39558-260">Rozhraní</span><span class="sxs-lookup"><span data-stu-id="39558-260">Interface</span></span> | <span data-ttu-id="39558-261">Popis</span><span class="sxs-lookup"><span data-stu-id="39558-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="39558-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="39558-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="39558-263">Vybere kontroler.</span><span class="sxs-lookup"><span data-stu-id="39558-263">Selects the controller.</span></span> |
| <span data-ttu-id="39558-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="39558-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="39558-265">Získá seznam typů řadičů.</span><span class="sxs-lookup"><span data-stu-id="39558-265">Gets the list of controller types.</span></span> <span data-ttu-id="39558-266">**DefaultHttpControllerSelector** z tohoto seznamu zvolí typ kontroleru.</span><span class="sxs-lookup"><span data-stu-id="39558-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="39558-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="39558-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="39558-268">Získá seznam sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="39558-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="39558-269">**IHttpControllerTypeResolver** rozhraní používá tento seznam k nalezení typů řadičů.</span><span class="sxs-lookup"><span data-stu-id="39558-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="39558-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="39558-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="39558-271">Vytvoří nové instance kontroleru.</span><span class="sxs-lookup"><span data-stu-id="39558-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="39558-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="39558-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="39558-273">Vybere akci.</span><span class="sxs-lookup"><span data-stu-id="39558-273">Selects the action.</span></span> |
| <span data-ttu-id="39558-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="39558-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="39558-275">Vyvolá akci.</span><span class="sxs-lookup"><span data-stu-id="39558-275">Invokes the action.</span></span> |

<span data-ttu-id="39558-276">Pokud chcete poskytnout vlastní implementaci pro kterékoli z těchto rozhraní, použijte **služby** kolekce na **HttpConfiguration** objektu:</span><span class="sxs-lookup"><span data-stu-id="39558-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
