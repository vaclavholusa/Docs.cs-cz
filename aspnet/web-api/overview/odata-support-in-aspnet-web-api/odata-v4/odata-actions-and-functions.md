---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Akce a funkce v OData v4 pomocí rozhraní ASP.NET Web API 2.2 | Dokumentace Microsoftu
author: MikeWasson
description: V prostředí OData akce a funkce jsou způsob, jak přidat chování na straně serveru, které nejsou snadno definované jako operace CRUD u entit. Tento kurz ukazuje, jak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: c78a9acabcb346b33a4fe53aa0d18ef67ddcaf33
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371960"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="e8782-104">Akce a funkce v OData v4 pomocí rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="e8782-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="e8782-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e8782-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e8782-106">V prostředí OData akce a funkce jsou způsob, jak přidat chování na straně serveru, které nejsou snadno definované jako operace CRUD u entit.</span><span class="sxs-lookup"><span data-stu-id="e8782-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="e8782-107">Tento kurz ukazuje postupy při přidání akcí a funkcí do koncového bodu OData v4, pomocí Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="e8782-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="e8782-108">Tento kurz vychází z kurzu [vytvořit OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="e8782-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e8782-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="e8782-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e8782-110">Webové rozhraní API 2.2</span><span class="sxs-lookup"><span data-stu-id="e8782-110">Web API 2.2</span></span>
> - <span data-ttu-id="e8782-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="e8782-111">OData v4</span></span>
> - [<span data-ttu-id="e8782-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="e8782-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="e8782-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="e8782-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="e8782-114">Kurz verze</span><span class="sxs-lookup"><span data-stu-id="e8782-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="e8782-115">OData verze 3, naleznete v tématu [akcí OData, které v prostředí ASP.NET Web API 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="e8782-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="e8782-116">Rozdíl mezi *akce* a *funkce* je, že akce může mít vedlejší účinky a funkce nepodporují.</span><span class="sxs-lookup"><span data-stu-id="e8782-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="e8782-117">Akce a funkce může vrátit data.</span><span class="sxs-lookup"><span data-stu-id="e8782-117">Both actions and functions can return data.</span></span> <span data-ttu-id="e8782-118">Mezi použití akce patří:</span><span class="sxs-lookup"><span data-stu-id="e8782-118">Some uses for actions include:</span></span>

- <span data-ttu-id="e8782-119">Složité transakce.</span><span class="sxs-lookup"><span data-stu-id="e8782-119">Complex transactions.</span></span>
- <span data-ttu-id="e8782-120">Manipulace s několika entit najednou.</span><span class="sxs-lookup"><span data-stu-id="e8782-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="e8782-121">Povolení aktualizací pouze k určité vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="e8782-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="e8782-122">Odesílání dat, která není entita.</span><span class="sxs-lookup"><span data-stu-id="e8782-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="e8782-123">Funkce jsou užitečné pro vracení informací, které neodpovídá přímo na entitu nebo kolekci.</span><span class="sxs-lookup"><span data-stu-id="e8782-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="e8782-124">Akce (nebo funkce) můžete cílit na jednu entitu nebo kolekci.</span><span class="sxs-lookup"><span data-stu-id="e8782-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="e8782-125">V terminologii OData, je to *vazby*.</span><span class="sxs-lookup"><span data-stu-id="e8782-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="e8782-126">Můžete taky nechat &quot;nevázaného&quot; akce/funkce, které se nazývají jako statické operace ve službě.</span><span class="sxs-lookup"><span data-stu-id="e8782-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="e8782-127">Příklad: Přidání akce</span><span class="sxs-lookup"><span data-stu-id="e8782-127">Example: Adding an Action</span></span>

<span data-ttu-id="e8782-128">Umožňuje definovat akci, která hodnocení produktu.</span><span class="sxs-lookup"><span data-stu-id="e8782-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="e8782-129">V tomto kurzu vychází z kurzu [vytvořit OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="e8782-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="e8782-130">Nejprve přidejte `ProductRating` modelu k reprezentaci hodnocení.</span><span class="sxs-lookup"><span data-stu-id="e8782-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="e8782-131">Také přidat **DbSet** k `ProductsContext` třídy, aby EF vytvoří hodnocení tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="e8782-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="e8782-132">Přidání akce do modelu EDM</span><span class="sxs-lookup"><span data-stu-id="e8782-132">Add the Action to the EDM</span></span>

<span data-ttu-id="e8782-133">V WebApiConfig.cs přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="e8782-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="e8782-134">**EntityTypeConfiguration.Action** metoda přidá akci do modelu entity data model (EDM).</span><span class="sxs-lookup"><span data-stu-id="e8782-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="e8782-135">**Parametr** metody určuje parametr typu pro akci.</span><span class="sxs-lookup"><span data-stu-id="e8782-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="e8782-136">Tento kód také nastaví obor názvů pro modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="e8782-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="e8782-137">Obor názvů je důležité, protože identifikátor URI pro akce obsahuje akce plně kvalifikovaný název:</span><span class="sxs-lookup"><span data-stu-id="e8782-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="e8782-138">V typické konfiguraci služby IIS způsobí tečky v této adresy URL IIS vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="e8782-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="e8782-139">Můžete to vyřešit přidáním následujícího oddílu do souboru Web.Config:</span><span class="sxs-lookup"><span data-stu-id="e8782-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="e8782-140">Přidání Kontroleru metody akce</span><span class="sxs-lookup"><span data-stu-id="e8782-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="e8782-141">Povolit &quot;míra&quot; akce, přidejte následující metodu do `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="e8782-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="e8782-142">Všimněte si, že název metody odpovídá názvu akce.</span><span class="sxs-lookup"><span data-stu-id="e8782-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="e8782-143">**[HttpPost]** atribut určuje, že metoda je metodou HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="e8782-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="e8782-144">Abyste mohli vyvolat akce, klient odešle požadavek HTTP POST podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="e8782-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="e8782-145">&quot;Míra&quot; akce je svázat s instancemi produktu, takže akce plně kvalifikovaný název připojí k identifikátoru URI entity je identifikátoru URI pro akci.</span><span class="sxs-lookup"><span data-stu-id="e8782-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="e8782-146">(Připomínáme, že nastavíme na obor názvů EDM &quot;ProductService&quot;, takže je název akce plně kvalifikovaný &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="e8782-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="e8782-147">Text žádosti obsahuje parametry akce jako datovou část JSON.</span><span class="sxs-lookup"><span data-stu-id="e8782-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="e8782-148">Webové rozhraní API automaticky převede datové části JSON do **ODataActionParameters** objekt, který je právě slovník hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="e8782-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="e8782-149">Pomocí tohoto slovníku pro přístup k parametrům ve své metodě kontroleru.</span><span class="sxs-lookup"><span data-stu-id="e8782-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="e8782-150">Pokud klient odešle parametry akce v chybného formátování, hodnota **ModelState.IsValid** má hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="e8782-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="e8782-151">Zkontrolujte tento příznak ve své metodě kontroleru a vrátí chybu, pokud **IsValid** má hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="e8782-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="e8782-152">Příklad: Přidání funkce</span><span class="sxs-lookup"><span data-stu-id="e8782-152">Example: Adding a Function</span></span>

<span data-ttu-id="e8782-153">Nyní Pojďme přidat funkci prostředí OData, která vrací nejdražší produktu.</span><span class="sxs-lookup"><span data-stu-id="e8782-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="e8782-154">Jako dříve, prvním krokem je přidání funkce do modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="e8782-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="e8782-155">V WebApiConfig.cs přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="e8782-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="e8782-156">V takovém případě je funkce vázaná kolekci produktů, spíše než jednotlivé instance produktu.</span><span class="sxs-lookup"><span data-stu-id="e8782-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="e8782-157">Klienti vyvolat funkci odesláním požadavku GET:</span><span class="sxs-lookup"><span data-stu-id="e8782-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="e8782-158">Tady je metoda kontroleru pro tuto funkci:</span><span class="sxs-lookup"><span data-stu-id="e8782-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="e8782-159">Všimněte si, že název metody odpovídá názvu funkce.</span><span class="sxs-lookup"><span data-stu-id="e8782-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="e8782-160">**[HttpGet]** atribut určuje, že metoda je metoda HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="e8782-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="e8782-161">Tady je odpovědi HTTP:</span><span class="sxs-lookup"><span data-stu-id="e8782-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="e8782-162">Příklad: Přidání nepřipojeného – funkce</span><span class="sxs-lookup"><span data-stu-id="e8782-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="e8782-163">V předchozím příkladu se funkce vazbou na určitou kolekci.</span><span class="sxs-lookup"><span data-stu-id="e8782-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="e8782-164">V tomto příkladu další vytvoříme *nevázaného* funkce.</span><span class="sxs-lookup"><span data-stu-id="e8782-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="e8782-165">Nevázaný funkce jsou volány jako statické operace ve službě.</span><span class="sxs-lookup"><span data-stu-id="e8782-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="e8782-166">Funkce v tomto příkladu vrátí daň z prodeje pro danou poštovní směrovací číslo.</span><span class="sxs-lookup"><span data-stu-id="e8782-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="e8782-167">V souboru WebApiConfig přidáte funkce do modelu EDM:</span><span class="sxs-lookup"><span data-stu-id="e8782-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="e8782-168">Všimněte si, že jsme volání **funkce** přímo na **Tvůrce ODataModelBuilder**, místo typu entity nebo kolekci.</span><span class="sxs-lookup"><span data-stu-id="e8782-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="e8782-169">Říká Tvůrce modelu, že funkce není vázaný.</span><span class="sxs-lookup"><span data-stu-id="e8782-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="e8782-170">Tady je metoda kontroleru, který implementuje funkce:</span><span class="sxs-lookup"><span data-stu-id="e8782-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="e8782-171">Které kontroler Web API je umístit tuto metodu v nezáleží.</span><span class="sxs-lookup"><span data-stu-id="e8782-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="e8782-172">Může ho dát `ProductsController`, nebo definujte samostatný kontroleru.</span><span class="sxs-lookup"><span data-stu-id="e8782-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="e8782-173">**[ODataRoute]** atribut definuje šablonu identifikátoru URI pro funkci.</span><span class="sxs-lookup"><span data-stu-id="e8782-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="e8782-174">Tady je příklad žádosti klienta:</span><span class="sxs-lookup"><span data-stu-id="e8782-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="e8782-175">Odpověď HTTP:</span><span class="sxs-lookup"><span data-stu-id="e8782-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
