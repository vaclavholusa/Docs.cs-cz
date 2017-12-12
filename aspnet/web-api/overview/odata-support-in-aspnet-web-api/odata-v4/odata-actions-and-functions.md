---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: "Akce a funkce v OData v4, pomocí rozhraní ASP.NET Web API 2.2 | Microsoft Docs"
author: MikeWasson
description: "V prostředí OData akce a funkce jsou způsob, jak přidat serverové chování, které nejsou snadno definován jako operace CRUD na entity. Tento kurz ukazuje, jak..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="3bcf9-104">Akce a funkce v OData v4, pomocí rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="3bcf9-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="3bcf9-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3bcf9-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="3bcf9-106">V prostředí OData akce a funkce jsou způsob, jak přidat serverové chování, které nejsou snadno definován jako operace CRUD na entity.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="3bcf9-107">Tento kurz ukazuje, jak přidat akcí a funkcí do OData v4 koncový bod, pomocí webového rozhraní API 2.2.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="3bcf9-108">Tento kurz je založený na kurz [vytvoření OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="3bcf9-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3bcf9-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="3bcf9-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3bcf9-110">Webové rozhraní API 2.2</span><span class="sxs-lookup"><span data-stu-id="3bcf9-110">Web API 2.2</span></span>
> - <span data-ttu-id="3bcf9-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="3bcf9-111">OData v4</span></span>
> - [<span data-ttu-id="3bcf9-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="3bcf9-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="3bcf9-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3bcf9-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="3bcf9-114">Kurz verze</span><span class="sxs-lookup"><span data-stu-id="3bcf9-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="3bcf9-115">OData verze 3, najdete v části [akcí OData ve webovém rozhraní API 2 ASP.NET](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="3bcf9-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="3bcf9-116">Rozdíl mezi *akce* a *funkce* je, že akce může mít vedlejší účinky, a funkce nepodporují.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="3bcf9-117">Akce a funkce může vrátit data.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-117">Both actions and functions can return data.</span></span> <span data-ttu-id="3bcf9-118">Některé použití akce patří:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-118">Some uses for actions include:</span></span>

- <span data-ttu-id="3bcf9-119">Komplexní transakce.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-119">Complex transactions.</span></span>
- <span data-ttu-id="3bcf9-120">Manipulace se několik entit najednou.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="3bcf9-121">Povolení aktualizací pouze na určité vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="3bcf9-122">Odesílání dat, která není entita.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="3bcf9-123">Funkce jsou užitečné pro vracení informací, které neodpovídá přímo na položku entita nebo kolekce.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="3bcf9-124">Akce (nebo funkce), můžete vybrat jednu entitu nebo kolekce.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="3bcf9-125">V terminologii OData, je to *vazby*.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="3bcf9-126">Můžete taky nechat &quot;nevázaný&quot; akce/funkce, které se nazývají jako statické operace ve službě.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="3bcf9-127">Příklad: Přidání akce</span><span class="sxs-lookup"><span data-stu-id="3bcf9-127">Example: Adding an Action</span></span>

<span data-ttu-id="3bcf9-128">Umožňuje definovat akci, která má hodnocení produktu.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="3bcf9-129">V tomto kurzu vychází kurzu [vytvoření OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="3bcf9-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="3bcf9-130">Nejprve přidejte `ProductRating` modelu, který má představovat hodnocení.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="3bcf9-131">Také přidat **DbSet** k `ProductsContext` třídy, takže EF vytvoří hodnocení tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="3bcf9-132">Přidat akci k modelu EDM</span><span class="sxs-lookup"><span data-stu-id="3bcf9-132">Add the Action to the EDM</span></span>

<span data-ttu-id="3bcf9-133">V WebApiConfig.cs přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="3bcf9-134">**EntityTypeConfiguration.Action** metoda akce přidá do datového modelu entity (EDM).</span><span class="sxs-lookup"><span data-stu-id="3bcf9-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="3bcf9-135">**Parametr** metoda určuje parametr s určeným pro akci.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="3bcf9-136">Tento kód také nastaví obor názvů pro EDM.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="3bcf9-137">Obor názvů záleží, protože identifikátor URI pro akci zahrnuje název plně kvalifikovaný akce:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="3bcf9-138">V typické konfigurace služby IIS tečky v této adresy URL způsobí, že služba IIS vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="3bcf9-139">To lze vyřešit tak, že přidáte do souboru Web.Config v následující části:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="3bcf9-140">Přidání metody Kontroleru pro akci</span><span class="sxs-lookup"><span data-stu-id="3bcf9-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="3bcf9-141">Chcete-li povolit &quot;míra&quot; akce, přidejte následující metodu do `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="3bcf9-142">Všimněte si, že metoda název odpovídá názvu akce.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="3bcf9-143">**[HttpPost]** Určuje atribut metodu je metoda HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="3bcf9-144">Volání akce, klient odešle požadavek HTTP POST takto:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="3bcf9-145">&quot;Míra&quot; akce je svázat s instancemi produktu, tak, aby identifikátor URI pro akci akce plně kvalifikovaný název připojí k identifikátoru URI entity.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="3bcf9-146">(Odvolat, že jsme obor názvů EDM nastavená na &quot;ProductService&quot;, takže je název akce plně kvalifikovaný &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="3bcf9-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="3bcf9-147">Text žádosti obsahuje parametry akcí jako datové části JSON.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="3bcf9-148">Webové rozhraní API automaticky převede datové části JSON do **ODataActionParameters** objekt, který je právě slovník hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="3bcf9-149">Pomocí tohoto slovníku pro přístup k parametrů v metodu řadiče.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="3bcf9-150">Pokud klient odešle parametry akcí v chybného formátování, hodnota **ModelState.IsValid** je false.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="3bcf9-151">Zkontrolujte tento příznak ve své metodě kontroleru a vrátí chybu, pokud **IsValid** je false.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="3bcf9-152">Příklad: Přidání funkce</span><span class="sxs-lookup"><span data-stu-id="3bcf9-152">Example: Adding a Function</span></span>

<span data-ttu-id="3bcf9-153">Nyní Pojďme přidat funkci prostředí OData, vrátí nejnákladnější produktu.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="3bcf9-154">Jako dříve, prvním krokem je přidání funkce do modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="3bcf9-155">V WebApiConfig.cs přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="3bcf9-156">V takovém případě je vázána kolekci produktů, nikoli jednotlivých instancí produktu funkce.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="3bcf9-157">Klienti vyvolání funkce odesláním požadavek GET:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="3bcf9-158">Tady je metoda kontroleru pro tuto funkci:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="3bcf9-159">Všimněte si, že název metody odpovídá názvu funkce.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="3bcf9-160">**[Třídy MetadataExchangeClientMode]** Určuje atribut metodu je metoda HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="3bcf9-161">Zde je odpověď HTTP:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="3bcf9-162">Příklad: Přidání Nesvázanou funkci</span><span class="sxs-lookup"><span data-stu-id="3bcf9-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="3bcf9-163">Předchozí příklad se funkce vazbou na určitou kolekci.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="3bcf9-164">V tomto příkladu další vytvoříme *nevázaný* funkce.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="3bcf9-165">Nevázaný funkce se nazývají jako statické operace ve službě.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="3bcf9-166">Funkce v tomto příkladu vrací DPH pro danou PSČ.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="3bcf9-167">V souboru WebApiConfig přidejte do modelu EDM funkce:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="3bcf9-168">Všimněte si, že jsme volání **funkce** přímo na **Tvůrce ODataModelBuilder**, místo typu entity nebo kolekci.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="3bcf9-169">Tato hodnota informuje Tvůrce modelu funkce nevázaný.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="3bcf9-170">Tady je metoda kontroleru, který implementuje funkce:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="3bcf9-171">Řadič webového rozhraní API, který umístěte tuto metodu v nezáleží.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="3bcf9-172">Může ho dát `ProductsController`, nebo definujte samostatné řadiče.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="3bcf9-173">**[ODataRoute]** atribut definuje šablonu identifikátoru URI pro funkci.</span><span class="sxs-lookup"><span data-stu-id="3bcf9-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="3bcf9-174">Tady je požadavek klienta příklad:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="3bcf9-175">Odpověď HTTP:</span><span class="sxs-lookup"><span data-stu-id="3bcf9-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
