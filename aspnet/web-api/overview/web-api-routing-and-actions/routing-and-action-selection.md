---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: "Směrování a výběr akce v rozhraní ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 02c2a01ef8ec2b5a49f2c303ee61f02702a3ba54
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="b27da-102">Směrování a výběr akce v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b27da-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="b27da-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b27da-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b27da-104">Tento článek popisuje, jak rozhraní ASP.NET Web API směruje požadavek HTTP na určité akce v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b27da-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="b27da-105">Souhrnné informace o směrování, najdete v části [směrování v rozhraní ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b27da-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="b27da-106">Tento článek vypadá na podrobnosti o procesu směrování.</span><span class="sxs-lookup"><span data-stu-id="b27da-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="b27da-107">Pokud vytvoříte projekt webového rozhraní API a zjistíte, že některé požadavky Nezískávat směrovány očekávaným způsobem, zpravidla Tento článek vám pomůže.</span><span class="sxs-lookup"><span data-stu-id="b27da-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="b27da-108">Směrování rozdělená do tří hlavních fází:</span><span class="sxs-lookup"><span data-stu-id="b27da-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="b27da-109">Odpovídající identifikátor URI pro šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="b27da-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="b27da-110">Vybere kontroler.</span><span class="sxs-lookup"><span data-stu-id="b27da-110">Selecting a controller.</span></span>
3. <span data-ttu-id="b27da-111">Výběr akce.</span><span class="sxs-lookup"><span data-stu-id="b27da-111">Selecting an action.</span></span>

<span data-ttu-id="b27da-112">Některé části procesu můžete nahradit vlastní vlastní chování.</span><span class="sxs-lookup"><span data-stu-id="b27da-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="b27da-113">V tomto článku I popisují výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="b27da-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="b27da-114">Na konci poznámky I míst, kde lze přizpůsobit chování.</span><span class="sxs-lookup"><span data-stu-id="b27da-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="b27da-115">Šablon trasy</span><span class="sxs-lookup"><span data-stu-id="b27da-115">Route Templates</span></span>

<span data-ttu-id="b27da-116">Šablona trasy bude vypadat podobně jako cesta URI, ale může mít zástupný symbol hodnoty, vyznačené složené závorky:</span><span class="sxs-lookup"><span data-stu-id="b27da-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="b27da-117">Když vytvoříte trasu, je zadat výchozí hodnoty pro některé nebo všechny zástupné symboly:</span><span class="sxs-lookup"><span data-stu-id="b27da-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="b27da-118">Můžete zadat také omezení, které omezují jak segment identifikátoru URI se může shodovat zástupný text:</span><span class="sxs-lookup"><span data-stu-id="b27da-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="b27da-119">Rozhraní se pokusí vyhledat segmentů v cestě URI do šablony.</span><span class="sxs-lookup"><span data-stu-id="b27da-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="b27da-120">Literály v šabloně musí přesně shodovat.</span><span class="sxs-lookup"><span data-stu-id="b27da-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="b27da-121">Zástupný text odpovídá žádnou hodnotu nezadáte omezení.</span><span class="sxs-lookup"><span data-stu-id="b27da-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="b27da-122">Rozhraní framework neodpovídá dalších částí identifikátoru URI, jako je název hostitele nebo parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="b27da-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="b27da-123">Rozhraní framework vybírá první směrování ve směrovací tabulce, která odpovídá identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b27da-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="b27da-124">Existují dva speciální zástupné znaky: "{controller}" a "{action}".</span><span class="sxs-lookup"><span data-stu-id="b27da-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="b27da-125">"{controller}" poskytuje název řadiče.</span><span class="sxs-lookup"><span data-stu-id="b27da-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="b27da-126">"{action}" poskytuje název akce.</span><span class="sxs-lookup"><span data-stu-id="b27da-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="b27da-127">V rozhraní Web API je obvykle konvence vynechejte "{action}".</span><span class="sxs-lookup"><span data-stu-id="b27da-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="b27da-128">Ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="b27da-128">Defaults</span></span>

<span data-ttu-id="b27da-129">Pokud zadáte výchozí hodnoty, trasy, která bude odpovídat identifikátor URI, který chybí tyto segmenty.</span><span class="sxs-lookup"><span data-stu-id="b27da-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="b27da-130">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b27da-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="b27da-131">Identifikátor URI "`http://localhost/api/products`" odpovídá této trase.</span><span class="sxs-lookup"><span data-stu-id="b27da-131">The URI "`http://localhost/api/products`" matches this route.</span></span> <span data-ttu-id="b27da-132">Segment "{kategorie}" je přiřazena "vše" Výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="b27da-132">The "{category}" segment is assigned the default value "all".</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="b27da-133">Slovníku trasy</span><span class="sxs-lookup"><span data-stu-id="b27da-133">Route Dictionary</span></span>

<span data-ttu-id="b27da-134">Pokud rozhraní najde shoda pro identifikátor URI, vytvoří slovník, který obsahuje hodnotu pro každý zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="b27da-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="b27da-135">U klíčů se názvy zástupného symbolu, není včetně složené závorky.</span><span class="sxs-lookup"><span data-stu-id="b27da-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="b27da-136">Hodnoty jsou převzaty z cesta k identifikátoru URI nebo z výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b27da-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="b27da-137">Slovník je uložen v **IHttpRouteData** objektu.</span><span class="sxs-lookup"><span data-stu-id="b27da-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="b27da-138">V průběhu této fáze odpovídající trasy zvláštní "{controller}" a "{action}" zástupné symboly jsou považovány stejně jako ostatní zástupné symboly.</span><span class="sxs-lookup"><span data-stu-id="b27da-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="b27da-139">Jednoduše uložených ve slovníku hodnoty ostatních.</span><span class="sxs-lookup"><span data-stu-id="b27da-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="b27da-140">Výchozí může mít speciální hodnotu **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="b27da-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="b27da-141">Pokud zástupný symbol získá přiřazenou tuto hodnotu, hodnotu nebyla přidána do slovníku trasy.</span><span class="sxs-lookup"><span data-stu-id="b27da-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="b27da-142">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b27da-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="b27da-143">Cesta k identifikátoru URI "rozhraní api nebo produkty" bude obsahovat slovníku trasy:</span><span class="sxs-lookup"><span data-stu-id="b27da-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="b27da-144">Řadič: "produkty"</span><span class="sxs-lookup"><span data-stu-id="b27da-144">controller: "products"</span></span>
- <span data-ttu-id="b27da-145">kategorie: "vše"</span><span class="sxs-lookup"><span data-stu-id="b27da-145">category: "all"</span></span>

<span data-ttu-id="b27da-146">Pro "rozhraní api nebo produkty/toys/123" ale slovníku trasy bude obsahovat:</span><span class="sxs-lookup"><span data-stu-id="b27da-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="b27da-147">Řadič: "produkty"</span><span class="sxs-lookup"><span data-stu-id="b27da-147">controller: "products"</span></span>
- <span data-ttu-id="b27da-148">kategorie: "toys"</span><span class="sxs-lookup"><span data-stu-id="b27da-148">category: "toys"</span></span>
- <span data-ttu-id="b27da-149">ID: "123"</span><span class="sxs-lookup"><span data-stu-id="b27da-149">id: "123"</span></span>

<span data-ttu-id="b27da-150">Výchozí hodnoty použít také hodnotu, která není vyskytovat kdekoli v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="b27da-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="b27da-151">Pokud trasa odpovídá, tato hodnota je uložena ve slovníku.</span><span class="sxs-lookup"><span data-stu-id="b27da-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="b27da-152">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b27da-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="b27da-153">Pokud cesta k identifikátoru URI je "kořenový/api/8", bude obsahovat slovníku dvou hodnot:</span><span class="sxs-lookup"><span data-stu-id="b27da-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="b27da-154">Řadič: "zákazníci"</span><span class="sxs-lookup"><span data-stu-id="b27da-154">controller: "customers"</span></span>
- <span data-ttu-id="b27da-155">ID: "8"</span><span class="sxs-lookup"><span data-stu-id="b27da-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="b27da-156">Výběr Kontroleru</span><span class="sxs-lookup"><span data-stu-id="b27da-156">Selecting a Controller</span></span>

<span data-ttu-id="b27da-157">Výběr řadiče jsou zpracována **IHttpControllerSelector.SelectController** metoda.</span><span class="sxs-lookup"><span data-stu-id="b27da-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="b27da-158">Tato metoda přebírá **HttpRequestMessage** instance a vrátí **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="b27da-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="b27da-159">Poskytuje výchozí implementaci **DefaultHttpControllerSelector** třídy.</span><span class="sxs-lookup"><span data-stu-id="b27da-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="b27da-160">Tato třída využívá přehledné algoritmus:</span><span class="sxs-lookup"><span data-stu-id="b27da-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="b27da-161">Vyhledejte ve slovníku trasy pro klíč "controller".</span><span class="sxs-lookup"><span data-stu-id="b27da-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="b27da-162">Z hodnoty pro tento klíč a připojte řetězec "Controller" k získání názvu typu kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b27da-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="b27da-163">Podívejte se na kontroler Web API s tímto názvem typu.</span><span class="sxs-lookup"><span data-stu-id="b27da-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="b27da-164">Například pokud slovníku trasy obsahuje dvojice klíč hodnota "controller" = "produkty", pak typ kontroleru je "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="b27da-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="b27da-165">Pokud neexistuje žádný odpovídající typ nebo více shod, rozhraní vrátí chybu do klienta.</span><span class="sxs-lookup"><span data-stu-id="b27da-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="b27da-166">Krok 3 **DefaultHttpControllerSelector** používá **IHttpControllerTypeResolver** rozhraní získat seznam typů kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b27da-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="b27da-167">Výchozí implementaci **IHttpControllerTypeResolver** vrátí všechny veřejné třídy, které implementují (a) **IHttpController**, (b) je abstraktní a (c) mít název, který končí na "Controller".</span><span class="sxs-lookup"><span data-stu-id="b27da-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="b27da-168">Výběr akce</span><span class="sxs-lookup"><span data-stu-id="b27da-168">Action Selection</span></span>

<span data-ttu-id="b27da-169">Po výběru kontroleru, rozhraní vybere akci voláním **IHttpActionSelector.SelectAction** metoda.</span><span class="sxs-lookup"><span data-stu-id="b27da-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="b27da-170">Tato metoda přebírá **HttpControllerContext** a vrátí **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="b27da-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="b27da-171">Poskytuje výchozí implementaci **ApiControllerActionSelector** třídy.</span><span class="sxs-lookup"><span data-stu-id="b27da-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="b27da-172">Vyberte akci, vypadá na následující:</span><span class="sxs-lookup"><span data-stu-id="b27da-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="b27da-173">Metoda HTTP požadavku.</span><span class="sxs-lookup"><span data-stu-id="b27da-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="b27da-174">Zástupný symbol "{action}" v šabloně trasy, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b27da-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="b27da-175">Parametry akce v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b27da-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="b27da-176">Před prohlížení algoritmus výběr, je potřeba pochopit některé věci o akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b27da-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="b27da-177">**Které metody na řadiči, jsou považovány za "akce"?**</span><span class="sxs-lookup"><span data-stu-id="b27da-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="b27da-178">Když vyberete akci, rozhraní pouze vypadá na veřejné instance metody na řadiči.</span><span class="sxs-lookup"><span data-stu-id="b27da-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="b27da-179">Navíc vyloučí ["speciální název"](https://msdn.microsoft.com/en-us/library/system.reflection.methodbase.isspecialname) metody (konstruktory, události, přetížení operátoru a tak dále) a metody zděděno z **objektu ApiController** třídy.</span><span class="sxs-lookup"><span data-stu-id="b27da-179">Also, it excludes ["special name"](https://msdn.microsoft.com/en-us/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="b27da-180">**Metody HTTP.**</span><span class="sxs-lookup"><span data-stu-id="b27da-180">**HTTP Methods.**</span></span> <span data-ttu-id="b27da-181">Rozhraní framework vybere pouze akce, které odpovídají metoda HTTP požadavku, stanoven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b27da-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="b27da-182">Zadávat lze metodu HTTP s atributem: **AcceptVerbs**, **HttpDelete**, **třídy MetadataExchangeClientMode**, **HttpHead**,  **Httpoptions měl**, **HttpPatch**, **HttpPost**, nebo **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="b27da-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="b27da-183">Jinak Pokud název metody kontroleru začíná "Get", "Post", "Put", "Odstranit", "Head", "Možnosti" nebo "Patch", pak podle konvence akce podporuje dané metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="b27da-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="b27da-184">Pokud žádná z výše, podporuje metodu POST.</span><span class="sxs-lookup"><span data-stu-id="b27da-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="b27da-185">**Parametr vazby.**</span><span class="sxs-lookup"><span data-stu-id="b27da-185">**Parameter Bindings.**</span></span> <span data-ttu-id="b27da-186">Vazba parametru je, jak webové rozhraní API vytvoří hodnotu pro parametr.</span><span class="sxs-lookup"><span data-stu-id="b27da-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="b27da-187">Tady je výchozí pravidlo pro vazbu parametru:</span><span class="sxs-lookup"><span data-stu-id="b27da-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="b27da-188">Jednoduché typy jsou převzaty z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b27da-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="b27da-189">Komplexní typy jsou převzaty z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="b27da-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="b27da-190">Jednoduché typy zahrnout všechny [primitivní typy rozhraní .NET Framework](https://msdn.microsoft.com/en-us/library/system.type.isprimitive), plus **data a času**, **Decimal**, **Guid**, **řetězec** , a **časový interval**.</span><span class="sxs-lookup"><span data-stu-id="b27da-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/en-us/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="b27da-191">Pro každou akci může číst maximálně jeden parametr textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="b27da-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="b27da-192">Je možné přepsat výchozí pravidla pro vazbu.</span><span class="sxs-lookup"><span data-stu-id="b27da-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="b27da-193">V tématu [vazbu parametru WebAPI pod pokličkou](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="b27da-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="b27da-194">S na pozadí zde je algoritmus pro výběr akce.</span><span class="sxs-lookup"><span data-stu-id="b27da-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="b27da-195">Vytvoří seznam všech akcí na řadiči, který bude odpovídat metoda požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="b27da-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="b27da-196">Pokud slovníku trasy má záznam "action", odeberte akce, jejichž název neodpovídá tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b27da-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="b27da-197">Zkuste tak, aby odpovídaly parametrů akcí na identifikátor URI, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b27da-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="b27da-198">Pro každou akci získáte seznam parametrů, které jsou jednoduchý typ, kde vazby získá parametr z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b27da-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="b27da-199">Vylučte volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="b27da-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="b27da-200">Z tohoto seznamu pokusí se najít shodu pro název každého parametru ve slovníku trasy nebo v řetězci dotazu identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b27da-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="b27da-201">Shoduje se malá a velká písmena a nezávisí na jejich pořadí.</span><span class="sxs-lookup"><span data-stu-id="b27da-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="b27da-202">Vyberte akci, kde každý parametr v seznamu má odpovídající v identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b27da-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="b27da-203">Pokud více této jednu akci splňuje tato kritéria, vyberte jednu s většina odpovídá parametru.</span><span class="sxs-lookup"><span data-stu-id="b27da-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="b27da-204">Ignorovat akce s **[NonAction]** atribut.</span><span class="sxs-lookup"><span data-stu-id="b27da-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="b27da-205">Krok #3 je pravděpodobně matoucí nejvíce.</span><span class="sxs-lookup"><span data-stu-id="b27da-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="b27da-206">Základní cílem je, že parametr můžete získat svou hodnotu z identifikátoru URI, z textu žádosti nebo z vlastní vazby.</span><span class="sxs-lookup"><span data-stu-id="b27da-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="b27da-207">Pro parametry, které pocházejí z identifikátoru URI chceme zkontrolujte, zda identifikátor URI ve skutečnosti obsahuje hodnotu pro tento parametr, v cestě (pomocí slovníku trasy) nebo v řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="b27da-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="b27da-208">Zvažte například následující akce:</span><span class="sxs-lookup"><span data-stu-id="b27da-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="b27da-209">*Id* vazby parametru na identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="b27da-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="b27da-210">Proto může tato akce odpovídá pouze identifikátor URI, který obsahuje hodnotu pro "id", buď ve slovníku trasy, nebo v řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="b27da-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="b27da-211">Volitelné parametry jsou výjimku, protože jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="b27da-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="b27da-212">Volitelný parametr je OK pokud vazby nelze získat hodnotu z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b27da-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="b27da-213">Komplexní typy jsou výjimku různých důvodů.</span><span class="sxs-lookup"><span data-stu-id="b27da-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="b27da-214">Komplexní typ lze vázat pouze k identifikátoru URI prostřednictvím vlastní vazby.</span><span class="sxs-lookup"><span data-stu-id="b27da-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="b27da-215">Ale v takovém případě rozhraní nelze předem vědět, jestli by vazby parametru na konkrétní identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="b27da-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="b27da-216">Pokud chcete zjistit, třeba, aby vyvolání vazby.</span><span class="sxs-lookup"><span data-stu-id="b27da-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="b27da-217">Cílem výběr algoritmů je ze statické popis před vyvoláním žádné vazby. Vyberte akci.</span><span class="sxs-lookup"><span data-stu-id="b27da-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="b27da-218">Komplexní typy jsou tedy vyloučeny z odpovídající algoritmus.</span><span class="sxs-lookup"><span data-stu-id="b27da-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="b27da-219">Po výběru se akce, volá se všechny vazby parametrů.</span><span class="sxs-lookup"><span data-stu-id="b27da-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="b27da-220">Shrnutí:</span><span class="sxs-lookup"><span data-stu-id="b27da-220">Summary:</span></span>

- <span data-ttu-id="b27da-221">Akce musí odpovídat metodu protokolu HTTP žádosti.</span><span class="sxs-lookup"><span data-stu-id="b27da-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="b27da-222">Název akce musí odpovídat "action" položku ve slovníku trasy, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b27da-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="b27da-223">Pro každý parametr akce Pokud parametr jsou převzaty z identifikátoru URI, pak název parametru je nutné nalézt ve slovníku trasy nebo v řetězci dotazu identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b27da-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="b27da-224">(Volitelné parametry a parametry s komplexními typy nevylučují.)</span><span class="sxs-lookup"><span data-stu-id="b27da-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="b27da-225">Pokuste se shodovat s největším počtem parametrů.</span><span class="sxs-lookup"><span data-stu-id="b27da-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="b27da-226">Nejlepší shodu může být metoda bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="b27da-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="b27da-227">Rozšířené příklad</span><span class="sxs-lookup"><span data-stu-id="b27da-227">Extended Example</span></span>

<span data-ttu-id="b27da-228">Postupy:</span><span class="sxs-lookup"><span data-stu-id="b27da-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="b27da-229">Kontroler:</span><span class="sxs-lookup"><span data-stu-id="b27da-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="b27da-230">Požadavek HTTP:</span><span class="sxs-lookup"><span data-stu-id="b27da-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="b27da-231">Směrovat odpovídající</span><span class="sxs-lookup"><span data-stu-id="b27da-231">Route Matching</span></span>

<span data-ttu-id="b27da-232">Identifikátor URI odpovídá trase s názvem "DefaultApi".</span><span class="sxs-lookup"><span data-stu-id="b27da-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="b27da-233">Slovníku trasy obsahuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="b27da-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="b27da-234">Řadič: "produkty"</span><span class="sxs-lookup"><span data-stu-id="b27da-234">controller: "products"</span></span>
- <span data-ttu-id="b27da-235">ID: "1"</span><span class="sxs-lookup"><span data-stu-id="b27da-235">id: "1"</span></span>

<span data-ttu-id="b27da-236">Slovníku trasy neobsahuje parametrů řetězce dotazu, "verze" a "Podrobnosti", ale bude stále charakter během výběr akce.</span><span class="sxs-lookup"><span data-stu-id="b27da-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="b27da-237">Výběr řadiče</span><span class="sxs-lookup"><span data-stu-id="b27da-237">Controller Selection</span></span>

<span data-ttu-id="b27da-238">Ze zadaného "controller" ve slovníku trasy typ kontroleru je `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b27da-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="b27da-239">Výběr akce</span><span class="sxs-lookup"><span data-stu-id="b27da-239">Action Selection</span></span>

<span data-ttu-id="b27da-240">Požadavek HTTP je požadavek GET.</span><span class="sxs-lookup"><span data-stu-id="b27da-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="b27da-241">Akce kontroleru, které podporují GET jsou `GetAll`, `GetById`, a `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="b27da-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="b27da-242">Slovníku trasy neobsahuje položku pro "action", takže jsme nemusíte odpovídal názvu akce.</span><span class="sxs-lookup"><span data-stu-id="b27da-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="b27da-243">V dalším kroku pokusíme se porovnat názvy parametrů pro tyto akce jenom prohlížení akce GET.</span><span class="sxs-lookup"><span data-stu-id="b27da-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="b27da-244">Akce</span><span class="sxs-lookup"><span data-stu-id="b27da-244">Action</span></span> | <span data-ttu-id="b27da-245">Parametry shody</span><span class="sxs-lookup"><span data-stu-id="b27da-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="b27da-246">žádná</span><span class="sxs-lookup"><span data-stu-id="b27da-246">none</span></span> |
| `GetById` | <span data-ttu-id="b27da-247">"id"</span><span class="sxs-lookup"><span data-stu-id="b27da-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="b27da-248">"název"</span><span class="sxs-lookup"><span data-stu-id="b27da-248">"name"</span></span> |

<span data-ttu-id="b27da-249">Všimněte si, že *verze* parametr `GetById` se nepovažuje protože je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="b27da-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="b27da-250">`GetAll` Metoda odpovídá trivially.</span><span class="sxs-lookup"><span data-stu-id="b27da-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="b27da-251">`GetById` Také odpovídající metoda, protože obsahuje "id" slovníku trasy.</span><span class="sxs-lookup"><span data-stu-id="b27da-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="b27da-252">`FindProductsByName` Metoda neodpovídá.</span><span class="sxs-lookup"><span data-stu-id="b27da-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="b27da-253">`GetById` Metoda služby wins, protože odpovídá jeden parametr versus žádné parametry pro `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="b27da-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="b27da-254">Metoda je volána s následující hodnoty parametrů:</span><span class="sxs-lookup"><span data-stu-id="b27da-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="b27da-255">*ID* = 1</span><span class="sxs-lookup"><span data-stu-id="b27da-255">*id* = 1</span></span>
- <span data-ttu-id="b27da-256">*verze* = 1.5</span><span class="sxs-lookup"><span data-stu-id="b27da-256">*version* = 1.5</span></span>

<span data-ttu-id="b27da-257">Všimněte si, že i když *verze* nebyl použit v algoritmu výběr hodnota parametru pochází z řetězce dotazu identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b27da-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="b27da-258">Rozšíření body</span><span class="sxs-lookup"><span data-stu-id="b27da-258">Extension Points</span></span>

<span data-ttu-id="b27da-259">Webové rozhraní API poskytuje rozšíření body pro některé části procesu směrování.</span><span class="sxs-lookup"><span data-stu-id="b27da-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="b27da-260">Rozhraní</span><span class="sxs-lookup"><span data-stu-id="b27da-260">Interface</span></span> | <span data-ttu-id="b27da-261">Popis</span><span class="sxs-lookup"><span data-stu-id="b27da-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b27da-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="b27da-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="b27da-263">Vybere kontroler.</span><span class="sxs-lookup"><span data-stu-id="b27da-263">Selects the controller.</span></span> |
| <span data-ttu-id="b27da-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="b27da-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="b27da-265">Získá seznam typů řadičů.</span><span class="sxs-lookup"><span data-stu-id="b27da-265">Gets the list of controller types.</span></span> <span data-ttu-id="b27da-266">**DefaultHttpControllerSelector** vybere typ kontroleru z tohoto seznamu.</span><span class="sxs-lookup"><span data-stu-id="b27da-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="b27da-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="b27da-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="b27da-268">Získá seznam sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="b27da-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="b27da-269">**IHttpControllerTypeResolver** rozhraní používá tento seznam k vyhledání typů řadičů.</span><span class="sxs-lookup"><span data-stu-id="b27da-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="b27da-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="b27da-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="b27da-271">Vytvoří nové instance kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b27da-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="b27da-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="b27da-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="b27da-273">Vybere akci.</span><span class="sxs-lookup"><span data-stu-id="b27da-273">Selects the action.</span></span> |
| <span data-ttu-id="b27da-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="b27da-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="b27da-275">Vyvolá akci.</span><span class="sxs-lookup"><span data-stu-id="b27da-275">Invokes the action.</span></span> |

<span data-ttu-id="b27da-276">Pokud chcete zadat vlastní implementace pro některý z těchto rozhraní, použijte **služby** kolekce na **HttpConfiguration** objektu:</span><span class="sxs-lookup"><span data-stu-id="b27da-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
