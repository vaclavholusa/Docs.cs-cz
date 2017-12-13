---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: "Podpora v rozhraní ASP.NET Web API 2 OData akce | Microsoft Docs"
author: MikeWasson
description: "V prostředí OData akce jsou způsob, jak přidat serverové chování, které nejsou snadno definován jako operace CRUD na entity. Některé použití akce patří: implementace..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="49881-104">Podpora v rozhraní ASP.NET Web API 2 OData akce</span><span class="sxs-lookup"><span data-stu-id="49881-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="49881-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="49881-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="49881-106">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="49881-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="49881-107">V prostředí OData *akce* představují způsob, jak přidat serverové chování, které nejsou snadno definován jako operace CRUD na entity.</span><span class="sxs-lookup"><span data-stu-id="49881-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="49881-108">Některé použití akce patří:</span><span class="sxs-lookup"><span data-stu-id="49881-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="49881-109">Implementace složité transakce.</span><span class="sxs-lookup"><span data-stu-id="49881-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="49881-110">Manipulace se několik entit najednou.</span><span class="sxs-lookup"><span data-stu-id="49881-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="49881-111">Povolení aktualizací pouze na určité vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="49881-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="49881-112">Odesílání informací na server, který není definován v entitě.</span><span class="sxs-lookup"><span data-stu-id="49881-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="49881-113">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="49881-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="49881-114">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="49881-114">Web API 2</span></span>
> - <span data-ttu-id="49881-115">OData verze 3</span><span class="sxs-lookup"><span data-stu-id="49881-115">OData Version 3</span></span>
> - <span data-ttu-id="49881-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="49881-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="49881-117">Příklad: Hodnocení produktu</span><span class="sxs-lookup"><span data-stu-id="49881-117">Example: Rating a Product</span></span>

<span data-ttu-id="49881-118">V tomto příkladu chceme, aby mohli uživatelé míra produkty a následně je zpřístupněte průměrné hodnocení pro jednotlivé produkty.</span><span class="sxs-lookup"><span data-stu-id="49881-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="49881-119">V databázi ukládáme seznam hodnocení, s klíči pro produkty.</span><span class="sxs-lookup"><span data-stu-id="49881-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="49881-120">Tady je model, který bychom může použít k reprezentování hodnocení v Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="49881-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="49881-121">Ale Neradi bychom klientům POST `ProductRating` objektu do kolekce "Hodnocení".</span><span class="sxs-lookup"><span data-stu-id="49881-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="49881-122">Intuitivně hodnocení souvisí s produkty kolekce a klienta pouhým post hodnoty hodnocení.</span><span class="sxs-lookup"><span data-stu-id="49881-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="49881-123">Proto místo použití normální operace CRUD, jsme definovali akci, která můžete vyvolat klienta na produktu.</span><span class="sxs-lookup"><span data-stu-id="49881-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="49881-124">V terminologii OData je akce *vázaný* na entity produktu.</span><span class="sxs-lookup"><span data-stu-id="49881-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="49881-125">Akce mají vedlejší účinky na serveru.</span><span class="sxs-lookup"><span data-stu-id="49881-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="49881-126">Z toho důvodu jsou vyvolány pomocí požadavky HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="49881-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="49881-127">Akce může mít parametry a návratové typy, které jsou popsány v metadata služby.</span><span class="sxs-lookup"><span data-stu-id="49881-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="49881-128">Klient odešle parametry v textu požadavku a server odešle návratovou hodnotu v textu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="49881-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="49881-129">Volání akce "Míra produkt", klient odešle metodu POST SMĚŘUJÍCÍ do identifikátoru URI takto:</span><span class="sxs-lookup"><span data-stu-id="49881-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="49881-130">Data v požadavku POST je jednoduše hodnocení produktu:</span><span class="sxs-lookup"><span data-stu-id="49881-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="49881-131">Deklarovat akce v datovém modelu Entity</span><span class="sxs-lookup"><span data-stu-id="49881-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="49881-132">V konfiguraci vašeho webového rozhraní API přidejte do datového modelu entity (EDM) akce:</span><span class="sxs-lookup"><span data-stu-id="49881-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="49881-133">Tento kód definuje "RateProduct" jako akci, která lze provést u entity produktu.</span><span class="sxs-lookup"><span data-stu-id="49881-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="49881-134">Také deklaruje, že provede akci **int** parametr s názvem "Hodnocení" a vrátí **int** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="49881-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="49881-135">Přidejte k řadiči akce</span><span class="sxs-lookup"><span data-stu-id="49881-135">Add the Action to the Controller</span></span>

<span data-ttu-id="49881-136">Akce "RateProduct" se váže k entitám produktu.</span><span class="sxs-lookup"><span data-stu-id="49881-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="49881-137">K provedení akce, přidejte metodu s názvem `RateProduct` řadiče produkty:</span><span class="sxs-lookup"><span data-stu-id="49881-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="49881-138">Všimněte si, že metoda název odpovídá názvu akce v modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="49881-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="49881-139">Metoda má dva parametry:</span><span class="sxs-lookup"><span data-stu-id="49881-139">The method has two parameters:</span></span>

- <span data-ttu-id="49881-140">*klíč*: klíč produktu míru.</span><span class="sxs-lookup"><span data-stu-id="49881-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="49881-141">*Parametry*: slovník hodnot parametrů akce.</span><span class="sxs-lookup"><span data-stu-id="49881-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="49881-142">Pokud používáte výchozích konvencí směrování, parametr klíče musí mít název "klíče".</span><span class="sxs-lookup"><span data-stu-id="49881-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="49881-143">Je také důležité zahrnout **[FromOdataUri]** atributu, jak je vidět.</span><span class="sxs-lookup"><span data-stu-id="49881-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="49881-144">Tento atribut určuje webového rozhraní API používat OData syntaxe pravidla v případě, že analyzuje klíč z identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="49881-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="49881-145">Použití *parametry* adresář k získání parametrů akcí:</span><span class="sxs-lookup"><span data-stu-id="49881-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="49881-146">Pokud klient odešle parametry akcí ve správné formátování, hodnota **ModelState.IsValid** hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="49881-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="49881-147">V takovém případě můžete použít **ODataActionParameters** slovník k získání hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="49881-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="49881-148">V tomto příkladu `RateProduct` akce přijímá jeden parametr s názvem "Hodnocení".</span><span class="sxs-lookup"><span data-stu-id="49881-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="49881-149">Akce metadat</span><span class="sxs-lookup"><span data-stu-id="49881-149">Action Metadata</span></span>

<span data-ttu-id="49881-150">Chcete-li zobrazit metadata služby, odeslat požadavek GET na /odata/$ metadat.</span><span class="sxs-lookup"><span data-stu-id="49881-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="49881-151">Zde je část metadat, který deklaruje `RateProduct` akce:</span><span class="sxs-lookup"><span data-stu-id="49881-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="49881-152">**FunctionImport** element deklaruje akce.</span><span class="sxs-lookup"><span data-stu-id="49881-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="49881-153">Většina polí jsou zřejmé, ale dva jsou vhodné poznamenat:</span><span class="sxs-lookup"><span data-stu-id="49881-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="49881-154">**Vlastnost IsBindable** znamená akce mohou být vyvolány na cílovou entitu alespoň určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="49881-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="49881-155">**IsAlwaysBindable** znamená akce může být vyvolána vždy na cílovou entitu.</span><span class="sxs-lookup"><span data-stu-id="49881-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="49881-156">Rozdílem je, že některé akce jsou vždy k dispozici pro klienty, ale může další akce závisí na stavu entity.</span><span class="sxs-lookup"><span data-stu-id="49881-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="49881-157">Předpokládejme například, že můžete definovat akce "Nákupu".</span><span class="sxs-lookup"><span data-stu-id="49881-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="49881-158">Pouze si můžete zakoupit položku, která je ve skladu.</span><span class="sxs-lookup"><span data-stu-id="49881-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="49881-159">Pokud položka není skladě, nemůže klient vyvolat této akce.</span><span class="sxs-lookup"><span data-stu-id="49881-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="49881-160">Když definujete EDM, **akce** metoda vytvoří vždy vázat akce:</span><span class="sxs-lookup"><span data-stu-id="49881-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="49881-161">I mluvit o akcích, není vždy vazbu (také nazývané *přechodný* akce) dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="49881-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="49881-162">Vyvolání akce</span><span class="sxs-lookup"><span data-stu-id="49881-162">Invoking the Action</span></span>

<span data-ttu-id="49881-163">Nyní si ukážeme, jak by měl klient vyvolat tuto akci.</span><span class="sxs-lookup"><span data-stu-id="49881-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="49881-164">Předpokládejme, že klient chce umožnit hodnocení 2 pro produkt s ID = 4.</span><span class="sxs-lookup"><span data-stu-id="49881-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="49881-165">Tady je příklad zprávu s žádostí o, formátu JSON pro textu žádosti:</span><span class="sxs-lookup"><span data-stu-id="49881-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="49881-166">Zde je zpráva odpovědi:</span><span class="sxs-lookup"><span data-stu-id="49881-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="49881-167">Vytvoření vazby akce na sadu entit</span><span class="sxs-lookup"><span data-stu-id="49881-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="49881-168">V předchozím příkladu akce vázán na jedné entity: klient hodnotí jednoho produktu.</span><span class="sxs-lookup"><span data-stu-id="49881-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="49881-169">Akce můžete vázat také na kolekci entit.</span><span class="sxs-lookup"><span data-stu-id="49881-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="49881-170">Stačí proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="49881-170">Just make the following changes:</span></span>

<span data-ttu-id="49881-171">V modelu EDM, přidat akci v entitě **kolekce** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="49881-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="49881-172">V metodě řadiče, vynechejte *klíč* parametr.</span><span class="sxs-lookup"><span data-stu-id="49881-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="49881-173">Teď klienta vyvolá akci na sadu entit produkty:</span><span class="sxs-lookup"><span data-stu-id="49881-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="49881-174">Akce s parametry kolekce</span><span class="sxs-lookup"><span data-stu-id="49881-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="49881-175">Akce může mít parametry, které berou kolekci hodnot.</span><span class="sxs-lookup"><span data-stu-id="49881-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="49881-176">V modelu EDM, použijte **CollectionParameter&lt;T&gt;**  deklarovat parametr.</span><span class="sxs-lookup"><span data-stu-id="49881-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="49881-177">Tento parametr s názvem "Hodnocení", která vezme kolekci deklaruje **int** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="49881-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="49881-178">V metodě kontroleru, stále získána hodnota parametru **ODataActionParameters** objektu, ale teď hodnota je **ICollection&lt;int&gt;**  hodnotu:</span><span class="sxs-lookup"><span data-stu-id="49881-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="49881-179">Přechodné akce</span><span class="sxs-lookup"><span data-stu-id="49881-179">Transient Actions</span></span>

<span data-ttu-id="49881-180">V příkladu "RateProduct" uživatelé vždy ohodnotit produktu, takže akce je vždy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="49881-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="49881-181">Ale některé akce závisí na stavu entity.</span><span class="sxs-lookup"><span data-stu-id="49881-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="49881-182">Například ve službě video pronájem akce "Najdete v článku věnovaném" není vždy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="49881-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="49881-183">(To závisí, zda je k dispozici kopii tohoto videa.) Tento typ akce se označuje *přechodný* akce.</span><span class="sxs-lookup"><span data-stu-id="49881-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="49881-184">V metadatech služby přechodné akce má **IsAlwaysBindable** rovnou na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="49881-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="49881-185">Který je ve skutečnosti výchozí hodnota, takže metadata bude vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="49881-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="49881-186">Zde uvádíme Proč to záleží: Pokud je přechodný, server musí dali pokyn klientovi, pokud je daná akce dostupná.</span><span class="sxs-lookup"><span data-stu-id="49881-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="49881-187">Dělá to pomocí zahrnutím odkazu na akce v entitě.</span><span class="sxs-lookup"><span data-stu-id="49881-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="49881-188">Tady je příklad pro entitu film:</span><span class="sxs-lookup"><span data-stu-id="49881-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="49881-189">Vlastnost "#CheckOut" obsahuje odkaz na akce CheckOut.</span><span class="sxs-lookup"><span data-stu-id="49881-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="49881-190">Pokud akce není dostupná, server vynechá odkaz.</span><span class="sxs-lookup"><span data-stu-id="49881-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="49881-191">Pokud chcete deklarovat přechodné akce v modelu EDM, zavolejte **TransientAction** metoda:</span><span class="sxs-lookup"><span data-stu-id="49881-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="49881-192">Navíc je nutné zadat funkci, která vrátí pro danou entitu odkaz akce.</span><span class="sxs-lookup"><span data-stu-id="49881-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="49881-193">Nastavení této funkce voláním **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="49881-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="49881-194">Jako výrazu lambda může zapisovat funkce:</span><span class="sxs-lookup"><span data-stu-id="49881-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="49881-195">Pokud je daná akce dostupná, výrazu lambda vrátí odkaz na akci.</span><span class="sxs-lookup"><span data-stu-id="49881-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="49881-196">Serializátor prostředí OData zahrnuje tento odkaz se serializuje entity.</span><span class="sxs-lookup"><span data-stu-id="49881-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="49881-197">Když akce není k dispozici, funkce vrátí hodnotu `null`.</span><span class="sxs-lookup"><span data-stu-id="49881-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49881-198">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="49881-198">Additional Resources</span></span>

[<span data-ttu-id="49881-199">Ukázka akce OData</span><span class="sxs-lookup"><span data-stu-id="49881-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
