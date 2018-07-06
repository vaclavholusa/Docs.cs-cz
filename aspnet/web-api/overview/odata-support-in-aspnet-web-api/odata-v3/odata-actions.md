---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Podpora akce OData v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: 'V prostředí OData akce jsou způsob, jak přidat chování na straně serveru, které nejsou snadno definované jako operace CRUD u entit. Mezi použití akce patří: implementace...'
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: e02ab21b864e328fe6892a00e5d5aca3f88eb9a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837769"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="d54f9-104">Podpora akce OData v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="d54f9-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="d54f9-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d54f9-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d54f9-106">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="d54f9-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="d54f9-107">V prostředí OData *akce* představují způsob, jak přidat chování na straně serveru, které nejsou snadno definované jako operace CRUD u entit.</span><span class="sxs-lookup"><span data-stu-id="d54f9-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="d54f9-108">Mezi použití akce patří:</span><span class="sxs-lookup"><span data-stu-id="d54f9-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="d54f9-109">Implementace složité transakce.</span><span class="sxs-lookup"><span data-stu-id="d54f9-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="d54f9-110">Manipulace s několika entit najednou.</span><span class="sxs-lookup"><span data-stu-id="d54f9-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="d54f9-111">Povolení aktualizací pouze k určité vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="d54f9-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="d54f9-112">Odeslání informací na server, který není definovaná v entitě.</span><span class="sxs-lookup"><span data-stu-id="d54f9-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d54f9-113">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="d54f9-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d54f9-114">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="d54f9-114">Web API 2</span></span>
> - <span data-ttu-id="d54f9-115">OData verze 3</span><span class="sxs-lookup"><span data-stu-id="d54f9-115">OData Version 3</span></span>
> - <span data-ttu-id="d54f9-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="d54f9-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="d54f9-117">Příklad: Hodnocení produktu</span><span class="sxs-lookup"><span data-stu-id="d54f9-117">Example: Rating a Product</span></span>

<span data-ttu-id="d54f9-118">V tomto příkladu chceme umožnit uživatelům hodnocení produktů a potom vystavit průměrné hodnocení pro jednotlivé produkty.</span><span class="sxs-lookup"><span data-stu-id="d54f9-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="d54f9-119">V databázi uložíme seznam hodnocení, s klíči produktů.</span><span class="sxs-lookup"><span data-stu-id="d54f9-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="d54f9-120">Tady je model, který bychom může použít k reprezentaci hodnocení v Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="d54f9-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="d54f9-121">Nechceme klientům příspěvek, ale `ProductRating` objektu do kolekce "Hodnocení".</span><span class="sxs-lookup"><span data-stu-id="d54f9-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="d54f9-122">Intuitivně hodnocení souvisí s produkty kolekce a klienta by mělo být pouze potřeba odeslání hodnocení.</span><span class="sxs-lookup"><span data-stu-id="d54f9-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="d54f9-123">Proto nemusíte používat běžné operace CRUD, definujeme, který může klient vyvolat akci na produktu.</span><span class="sxs-lookup"><span data-stu-id="d54f9-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="d54f9-124">V terminologii OData "action" je *vázán* k entitám produktu.</span><span class="sxs-lookup"><span data-stu-id="d54f9-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="d54f9-125">Akce mají vedlejší účinky na serveru.</span><span class="sxs-lookup"><span data-stu-id="d54f9-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="d54f9-126">Z tohoto důvodu jsou vyvolány pomocí požadavků HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="d54f9-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="d54f9-127">Akce může mít parametry a návratové typy, které jsou popsány v metadatech služby.</span><span class="sxs-lookup"><span data-stu-id="d54f9-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="d54f9-128">Klient odešle parametry v textu požadavku a server odešle vrácená hodnota v textu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d54f9-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="d54f9-129">K vyvolání akce "Frekvence produkt", klient odešle příspěvek na identifikátor URI vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="d54f9-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="d54f9-130">Data v požadavku POST je jednoduše hodnocení produktu:</span><span class="sxs-lookup"><span data-stu-id="d54f9-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="d54f9-131">Deklarovat akce v modelu Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="d54f9-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="d54f9-132">V konfiguraci webového rozhraní API přidáte akci k modelu entity data model (EDM):</span><span class="sxs-lookup"><span data-stu-id="d54f9-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="d54f9-133">Tento kód definuje "RateProduct" jako akci, která lze provést u entity produktu.</span><span class="sxs-lookup"><span data-stu-id="d54f9-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="d54f9-134">Také deklaruje, že tato akce trvá **int** parametr s názvem "Hodnocení" a vrátí **int** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d54f9-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="d54f9-135">Přidání akce Kontroleru</span><span class="sxs-lookup"><span data-stu-id="d54f9-135">Add the Action to the Controller</span></span>

<span data-ttu-id="d54f9-136">Akce "RateProduct" je vázán na entity produktu.</span><span class="sxs-lookup"><span data-stu-id="d54f9-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="d54f9-137">K provedení akce, přidejte metodu s názvem `RateProduct` ke kontroleru produkty:</span><span class="sxs-lookup"><span data-stu-id="d54f9-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="d54f9-138">Všimněte si, že název metody odpovídá názvu akce v modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="d54f9-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="d54f9-139">Metoda má dva parametry:</span><span class="sxs-lookup"><span data-stu-id="d54f9-139">The method has two parameters:</span></span>

- <span data-ttu-id="d54f9-140">*klíč*: klíč produktu pro míru.</span><span class="sxs-lookup"><span data-stu-id="d54f9-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="d54f9-141">*Parametry*: slovník hodnot parametrů akce.</span><span class="sxs-lookup"><span data-stu-id="d54f9-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="d54f9-142">Pokud používáte výchozích konvencí směrování, parametr klíče musí mít název "klíče".</span><span class="sxs-lookup"><span data-stu-id="d54f9-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="d54f9-143">Je také důležité zahrnout **[FromOdataUri]** atributu, jak je znázorněno.</span><span class="sxs-lookup"><span data-stu-id="d54f9-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="d54f9-144">Tento atribut oznamuje webového rozhraní API pro použití pravidel syntaxe OData při analýze klíč z identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="d54f9-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="d54f9-145">Použití *parametry* slovníku zobrazíte parametry akce:</span><span class="sxs-lookup"><span data-stu-id="d54f9-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="d54f9-146">Pokud klient odešle parametry akce ve správné formátování, hodnota **ModelState.IsValid** má hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="d54f9-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="d54f9-147">V takovém případě můžete použít **ODataActionParameters** slovníku k získání hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="d54f9-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="d54f9-148">V tomto příkladu `RateProduct` akce přijímá jeden parametr s názvem "Hodnocení".</span><span class="sxs-lookup"><span data-stu-id="d54f9-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="d54f9-149">Akce metadat</span><span class="sxs-lookup"><span data-stu-id="d54f9-149">Action Metadata</span></span>

<span data-ttu-id="d54f9-150">Chcete-li zobrazit metadata služby, odeslat požadavek GET na /odata/$ metadat.</span><span class="sxs-lookup"><span data-stu-id="d54f9-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="d54f9-151">Tady je část metadata, která deklaruje `RateProduct` akce:</span><span class="sxs-lookup"><span data-stu-id="d54f9-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="d54f9-152">**Element FunctionImport** element deklaruje akce.</span><span class="sxs-lookup"><span data-stu-id="d54f9-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="d54f9-153">Většina polí není potřeba vysvětlovat, ale dvě stojí za zmínku:</span><span class="sxs-lookup"><span data-stu-id="d54f9-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="d54f9-154">**Vlastnost IsBindable** znamená, že tato akce může být vyvolána na cílovou entitu, alespoň některé času.</span><span class="sxs-lookup"><span data-stu-id="d54f9-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="d54f9-155">**Isalwaysbindable pro položku** znamená, že tato akce může být vyvolána vždy na cílovou entitu.</span><span class="sxs-lookup"><span data-stu-id="d54f9-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="d54f9-156">Rozdíl je, že některé akce jsou vždy k dispozici pro klienty, ale jiné akce může záviset na stav entity.</span><span class="sxs-lookup"><span data-stu-id="d54f9-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="d54f9-157">Předpokládejme například, že definujete akce "Koupit".</span><span class="sxs-lookup"><span data-stu-id="d54f9-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="d54f9-158">Můžete zakoupit pouze položky, která je na skladě.</span><span class="sxs-lookup"><span data-stu-id="d54f9-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="d54f9-159">Pokud je položka vyčerpány zásoby, klient nejde vyvolat tuto akci.</span><span class="sxs-lookup"><span data-stu-id="d54f9-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="d54f9-160">Při definování EDM, **akce** metoda vytvoří vždy vazbu akce:</span><span class="sxs-lookup"><span data-stu-id="d54f9-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="d54f9-161">Můžu budeme mluvit o not vždy – s možností vazby akce (také nazývané *přechodné* akce) dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="d54f9-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="d54f9-162">Vyvolání akce</span><span class="sxs-lookup"><span data-stu-id="d54f9-162">Invoking the Action</span></span>

<span data-ttu-id="d54f9-163">Nyní Podíváme se, jak by měl klient vyvolat tuto akci.</span><span class="sxs-lookup"><span data-stu-id="d54f9-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="d54f9-164">Předpokládejme, že klient chce poskytnout hodnocení 2 pro produkt s ID = 4.</span><span class="sxs-lookup"><span data-stu-id="d54f9-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="d54f9-165">Tady je ukázková zpráva požadavku, ve formátu JSON pro datovou část požadavku:</span><span class="sxs-lookup"><span data-stu-id="d54f9-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="d54f9-166">Tady je zpráva odpovědi:</span><span class="sxs-lookup"><span data-stu-id="d54f9-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="d54f9-167">Vytvoření vazby akce na sadu entit</span><span class="sxs-lookup"><span data-stu-id="d54f9-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="d54f9-168">V předchozím příkladu, akce je vázán na jednu entitu: klient hodnotí jednoho produktu.</span><span class="sxs-lookup"><span data-stu-id="d54f9-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="d54f9-169">Akci můžete také navázat na kolekci entit.</span><span class="sxs-lookup"><span data-stu-id="d54f9-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="d54f9-170">Stačí proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="d54f9-170">Just make the following changes:</span></span>

<span data-ttu-id="d54f9-171">V modelu EDM, přidání akce do entity **kolekce** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d54f9-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="d54f9-172">V metodě kontroleru vynechat, nechte *klíč* parametru.</span><span class="sxs-lookup"><span data-stu-id="d54f9-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="d54f9-173">Nyní klient vyvolá akci na sadu entit produkty:</span><span class="sxs-lookup"><span data-stu-id="d54f9-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="d54f9-174">Akce s parametry kolekce</span><span class="sxs-lookup"><span data-stu-id="d54f9-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="d54f9-175">Akce může mít parametry, které berou kolekci hodnot.</span><span class="sxs-lookup"><span data-stu-id="d54f9-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="d54f9-176">V modelu EDM, použijte **CollectionParameter&lt;T&gt;**  k deklaraci parametru.</span><span class="sxs-lookup"><span data-stu-id="d54f9-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="d54f9-177">To deklaruje parametr s názvem "Hodnocení", která vezme kolekci **int** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d54f9-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="d54f9-178">V metodě kontroler stále získána hodnota parametru **ODataActionParameters** objektu, ale teď hodnota je **rozhraní ICollection&lt;int&gt;**  hodnotu:</span><span class="sxs-lookup"><span data-stu-id="d54f9-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="d54f9-179">Přechodné akce</span><span class="sxs-lookup"><span data-stu-id="d54f9-179">Transient Actions</span></span>

<span data-ttu-id="d54f9-180">V příkladu "RateProduct" uživatelé vždy ohodnotit produktu, akce je vždycky k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d54f9-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="d54f9-181">Ale některá akce závisí na stavu entity.</span><span class="sxs-lookup"><span data-stu-id="d54f9-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="d54f9-182">Například ve službě video pronájmu "Rezervace" akce není vždy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d54f9-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="d54f9-183">(Záleží, zda je k dispozici kopii tohoto videa.) Tento typ akce je volána *přechodné* akce.</span><span class="sxs-lookup"><span data-stu-id="d54f9-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="d54f9-184">V metadatech služby přechodné akce má **isalwaysbindable pro položku** rovná na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="d54f9-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="d54f9-185">Který je ve skutečnosti výchozí hodnotu, takže metadata bude vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="d54f9-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="d54f9-186">Tady je Proč je to důležité: Pokud akce je přechodná, server musí dali pokyn klientovi, pokud je daná akce dostupná.</span><span class="sxs-lookup"><span data-stu-id="d54f9-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="d54f9-187">Dělá to tak, že včetně odkazu na akci v dané entitě.</span><span class="sxs-lookup"><span data-stu-id="d54f9-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="d54f9-188">Tady je příklad entity filmu:</span><span class="sxs-lookup"><span data-stu-id="d54f9-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="d54f9-189">Vlastnost "#CheckOut" obsahuje odkaz na akce CheckOut.</span><span class="sxs-lookup"><span data-stu-id="d54f9-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="d54f9-190">Pokud akce není k dispozici, server vynechá odkazu.</span><span class="sxs-lookup"><span data-stu-id="d54f9-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="d54f9-191">Chcete-li deklarovat přechodné akce v modelu EDM, zavolejte **TransientAction** metody:</span><span class="sxs-lookup"><span data-stu-id="d54f9-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="d54f9-192">Kromě toho je nutné zadat funkce vracející odkaz akce pro danou entitu.</span><span class="sxs-lookup"><span data-stu-id="d54f9-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="d54f9-193">Nastavit tuto funkci voláním **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="d54f9-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="d54f9-194">Jako výraz lambda lze napsat funkci:</span><span class="sxs-lookup"><span data-stu-id="d54f9-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="d54f9-195">Pokud je daná akce dostupná, výraz lambda vrátí odkaz na akci.</span><span class="sxs-lookup"><span data-stu-id="d54f9-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="d54f9-196">Serializátor prostředí OData zahrnuje tento odkaz, když dojde k serializaci entit.</span><span class="sxs-lookup"><span data-stu-id="d54f9-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="d54f9-197">Pokud akce není k dispozici, funkce vrátí `null`.</span><span class="sxs-lookup"><span data-stu-id="d54f9-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d54f9-198">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="d54f9-198">Additional Resources</span></span>

[<span data-ttu-id="d54f9-199">Ukázka akce OData</span><span class="sxs-lookup"><span data-stu-id="d54f9-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
