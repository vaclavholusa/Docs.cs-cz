---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Volání služby OData z klienta .NET (C#) | Dokumentace Microsoftu
author: MikeWasson
description: Tento kurz ukazuje postupy při volání služby OData z klientské aplikace C#. Verze softwaru, které jsou používané v kurzu Visual Studio 2013 (funguje s Visual S...
ms.author: aspnetcontent
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 72dca7dc2ec27f15c9f0526621a713267f5835f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819552"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="99383-104">Volání služby OData z klienta .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="99383-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="99383-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="99383-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="99383-106">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="99383-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="99383-107">Tento kurz ukazuje postupy při volání služby OData z klientské aplikace C#.</span><span class="sxs-lookup"><span data-stu-id="99383-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="99383-108">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="99383-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="99383-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funguje v sadě Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="99383-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="99383-110">Klientská knihovna pro WCF Data Services</span><span class="sxs-lookup"><span data-stu-id="99383-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="99383-111">Webové rozhraní API 2.</span><span class="sxs-lookup"><span data-stu-id="99383-111">Web API 2.</span></span> <span data-ttu-id="99383-112">(V příkladu služby OData se vytvořil pomocí webového rozhraní API 2, ale klientské aplikace nezávisí na webového rozhraní API.)</span><span class="sxs-lookup"><span data-stu-id="99383-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="99383-113">V tomto kurzu můžu projdete kroky vytvoření klientské aplikace, která volá ze služby OData.</span><span class="sxs-lookup"><span data-stu-id="99383-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="99383-114">Služba OData zveřejňuje následující entity:</span><span class="sxs-lookup"><span data-stu-id="99383-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="99383-115">Následující články popisují, jak implementovat služby OData v rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="99383-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="99383-116">(Není nutné číst o tomto kurzu se ale.)</span><span class="sxs-lookup"><span data-stu-id="99383-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="99383-117">Vytváří se koncový bod OData ve webovém rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="99383-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="99383-118">Relace entit OData ve webovém rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="99383-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="99383-119">Akce OData ve webovém rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="99383-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="99383-120">Generování Proxy služby</span><span class="sxs-lookup"><span data-stu-id="99383-120">Generate the Service Proxy</span></span>

<span data-ttu-id="99383-121">Prvním krokem je generovat proxy služby.</span><span class="sxs-lookup"><span data-stu-id="99383-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="99383-122">Proxy služby je třída rozhraní .NET, která definuje metody pro přístup ke službě OData.</span><span class="sxs-lookup"><span data-stu-id="99383-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="99383-123">Proxy server překládá volání metod na požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="99383-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="99383-124">Začněte tak, že otevřete projekt služby OData v aplikaci Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="99383-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="99383-125">Stisknutím kláves CTRL + F5 ke spuštění služby místně v rámci služby IIS Express.</span><span class="sxs-lookup"><span data-stu-id="99383-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="99383-126">Poznámka: na místní adrese, včetně číslo portu, který přiřazuje sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="99383-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="99383-127">Tato adresa bude potřebovat při vytváření proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="99383-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="99383-128">Dále otevřete jinou instanci sady Visual Studio a vytvořte projekt konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="99383-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="99383-129">Konzolová aplikace bude naše klientské aplikace OData.</span><span class="sxs-lookup"><span data-stu-id="99383-129">The console application will be our OData client application.</span></span> <span data-ttu-id="99383-130">(Můžete také přidat projekt do stejného řešení jako službu.)</span><span class="sxs-lookup"><span data-stu-id="99383-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="99383-131">Zbývající kroky najdete v projektu konzoly.</span><span class="sxs-lookup"><span data-stu-id="99383-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="99383-132">V Průzkumníku řešení klikněte pravým tlačítkem na **odkazy** a vyberte **přidat odkaz na službu**.</span><span class="sxs-lookup"><span data-stu-id="99383-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="99383-133">V **přidat odkaz na službu** dialogové okno, zadejte adresu služby OData:</span><span class="sxs-lookup"><span data-stu-id="99383-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="99383-134">kde *port* je číslo portu.</span><span class="sxs-lookup"><span data-stu-id="99383-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="99383-135">Pro **Namespace**, zadejte "ProductService".</span><span class="sxs-lookup"><span data-stu-id="99383-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="99383-136">Tato možnost určuje obor názvů, třídy proxy.</span><span class="sxs-lookup"><span data-stu-id="99383-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="99383-137">Klikněte na tlačítko **Přejít**.</span><span class="sxs-lookup"><span data-stu-id="99383-137">Click **Go**.</span></span> <span data-ttu-id="99383-138">Visual Studio načte dokument metadat OData ke zjišťování entit ve službě.</span><span class="sxs-lookup"><span data-stu-id="99383-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="99383-139">Klikněte na tlačítko **OK** do svého projektu přidat třídu proxy.</span><span class="sxs-lookup"><span data-stu-id="99383-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="99383-140">Vytvoření Instance třídy Proxy služby</span><span class="sxs-lookup"><span data-stu-id="99383-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="99383-141">Uvnitř vaší `Main` metodu, vytvořte novou instanci třídy proxy, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="99383-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="99383-142">Znovu použijte číslo skutečný port ve kterém je vaše služba spuštěná.</span><span class="sxs-lookup"><span data-stu-id="99383-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="99383-143">Při nasazování služby budete používat identifikátor URI služby za provozu.</span><span class="sxs-lookup"><span data-stu-id="99383-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="99383-144">Není nutné aktualizovat server proxy.</span><span class="sxs-lookup"><span data-stu-id="99383-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="99383-145">Následující kód přidá obslužnou rutinu události, která zobrazí žádost o identifikátory URI v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="99383-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="99383-146">Tento krok není povinný, ale je zajímavé zobrazíte identifikátory URI pro každý dotaz.</span><span class="sxs-lookup"><span data-stu-id="99383-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="99383-147">Dotazování na službu</span><span class="sxs-lookup"><span data-stu-id="99383-147">Query the Service</span></span>

<span data-ttu-id="99383-148">Následující kód načte seznam produktů ze služby OData.</span><span class="sxs-lookup"><span data-stu-id="99383-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="99383-149">Všimněte si, že nemusíte psát jakýkoli kód k odeslání požadavku HTTP a parsovat odpovědi.</span><span class="sxs-lookup"><span data-stu-id="99383-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="99383-150">Proxy třída nemá tomto automaticky při vytvoření výčtu `Container.Products` kolekce **foreach** smyčky.</span><span class="sxs-lookup"><span data-stu-id="99383-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="99383-151">Když aplikaci spouštíte, výstup by měl vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="99383-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="99383-152">Chcete-li získat entity podle ID, použijte `where` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="99383-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="99383-153">Pro zbývající část tohoto tématu, mohu nezobrazí celý `Main` fungovat, pouze kód, které jsou potřebné k vyvolání služby.</span><span class="sxs-lookup"><span data-stu-id="99383-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="99383-154">Použije možnosti dotazu</span><span class="sxs-lookup"><span data-stu-id="99383-154">Apply Query Options</span></span>

<span data-ttu-id="99383-155">OData definuje [možnosti dotazu](../supporting-odata-query-options.md) , který slouží k filtrování, řazení, data stránky a tak dále.</span><span class="sxs-lookup"><span data-stu-id="99383-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="99383-156">V proxy služby můžete použít tyto možnosti s použitím různých LINQ – výrazy.</span><span class="sxs-lookup"><span data-stu-id="99383-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="99383-157">V této části ukážeme si stručný příklady.</span><span class="sxs-lookup"><span data-stu-id="99383-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="99383-158">Další podrobnosti najdete v tématu [aspekty LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) na webové stránce MSDN.</span><span class="sxs-lookup"><span data-stu-id="99383-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="99383-159">Filtrování ($filter)</span><span class="sxs-lookup"><span data-stu-id="99383-159">Filtering ($filter)</span></span>

<span data-ttu-id="99383-160">Chcete-li filtrovat, použijte `where` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="99383-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="99383-161">Následující příklad filtry podle kategorie produktu.</span><span class="sxs-lookup"><span data-stu-id="99383-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="99383-162">Tento kód odpovídá následující dotaz OData.</span><span class="sxs-lookup"><span data-stu-id="99383-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="99383-163">Všimněte si, že se převede proxy serveru `where` klauzuli do OData `$filter` výrazu.</span><span class="sxs-lookup"><span data-stu-id="99383-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="99383-164">Řazení ($orderby)</span><span class="sxs-lookup"><span data-stu-id="99383-164">Sorting ($orderby)</span></span>

<span data-ttu-id="99383-165">Chcete-li seřadit, použijte `orderby` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="99383-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="99383-166">V následujícím příkladu se seřadí podle cena od nejvyšší po nejnižší.</span><span class="sxs-lookup"><span data-stu-id="99383-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="99383-167">Tady je odpovídající žádost OData.</span><span class="sxs-lookup"><span data-stu-id="99383-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="99383-168">Stránkování na straně klienta ($skip a $top)</span><span class="sxs-lookup"><span data-stu-id="99383-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="99383-169">Pro velká entita sady klient může chtít omezit počet výsledků.</span><span class="sxs-lookup"><span data-stu-id="99383-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="99383-170">Klient může například zobrazit 10 položek najednou.</span><span class="sxs-lookup"><span data-stu-id="99383-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="99383-171">Tento postup se nazývá *stránkování na straně klienta*.</span><span class="sxs-lookup"><span data-stu-id="99383-171">This is called *client-side paging*.</span></span> <span data-ttu-id="99383-172">(K dispozici je také [stránkování na straně serveru](../supporting-odata-query-options.md#server-paging), kde server omezí počet výsledků.) Stránkování na straně klienta, použijte LINQ **přeskočit** a **trvat** metody.</span><span class="sxs-lookup"><span data-stu-id="99383-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="99383-173">V následujícím příkladu Přeskočí prvních 40 výsledky a přijímá další 10.</span><span class="sxs-lookup"><span data-stu-id="99383-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="99383-174">Tady je odpovídající žádost OData:</span><span class="sxs-lookup"><span data-stu-id="99383-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="99383-175">Vyberte ($select) a rozbalení ($expand)</span><span class="sxs-lookup"><span data-stu-id="99383-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="99383-176">Chcete-li zahrnout související entity, použijte `DataServiceQuery<t>.Expand` metody.</span><span class="sxs-lookup"><span data-stu-id="99383-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="99383-177">Například chcete zahrnout `Supplier` pro každou `Product`:</span><span class="sxs-lookup"><span data-stu-id="99383-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="99383-178">Tady je odpovídající žádost OData:</span><span class="sxs-lookup"><span data-stu-id="99383-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="99383-179">Chcete-li změnit tvar dané odpovědi, použití LINQ **vyberte** klauzuli.</span><span class="sxs-lookup"><span data-stu-id="99383-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="99383-180">Následující příklad získá pouze název každého produktu se žádné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="99383-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="99383-181">Tady je odpovídající žádost OData:</span><span class="sxs-lookup"><span data-stu-id="99383-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="99383-182">Klauzule select může obsahovat související entity.</span><span class="sxs-lookup"><span data-stu-id="99383-182">A select clause can include related entities.</span></span> <span data-ttu-id="99383-183">V takovém případě Nevolejte **Rozbalit**; proxy server v tomto případě automaticky zahrnuje rozšíření.</span><span class="sxs-lookup"><span data-stu-id="99383-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="99383-184">Následující příklad získá název a dodavatele výhod každého produktu.</span><span class="sxs-lookup"><span data-stu-id="99383-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="99383-185">Tady je odpovídající žádost OData.</span><span class="sxs-lookup"><span data-stu-id="99383-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="99383-186">Všimněte si, že zahrnuje **$expand** možnost.</span><span class="sxs-lookup"><span data-stu-id="99383-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="99383-187">Další informace o $select a $expand rozbalte naleznete v tématu [pomocí $select $expand a $value ve webovém rozhraní API 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="99383-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="99383-188">Přidat novou entitu</span><span class="sxs-lookup"><span data-stu-id="99383-188">Add a New Entity</span></span>

<span data-ttu-id="99383-189">Chcete-li přidat nové entity na sadu entit, zavolejte `AddToEntitySet`, kde *objektu EntitySet* je název sady entit.</span><span class="sxs-lookup"><span data-stu-id="99383-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="99383-190">Například `AddToProducts` přidá nový `Product` k `Products` sady entit.</span><span class="sxs-lookup"><span data-stu-id="99383-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="99383-191">Při generování proxy WCF Data Services automaticky vytvoří tyto silného typu **AddTo** metody.</span><span class="sxs-lookup"><span data-stu-id="99383-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="99383-192">Chcete-li přidat propojení mezi dvěma entitami, použijte **AddLink** a **SetLink** metody.</span><span class="sxs-lookup"><span data-stu-id="99383-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="99383-193">Následující kód přidá nový dodavatele a nového produktu a vytvoří propojení mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="99383-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="99383-194">Použití **AddLink** po navigační vlastnost kolekce.</span><span class="sxs-lookup"><span data-stu-id="99383-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="99383-195">V tomto příkladu přidáváme produkt, který má `Products` kolekce na dodavatele.</span><span class="sxs-lookup"><span data-stu-id="99383-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="99383-196">Použití **SetLink** po jedné entity navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="99383-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="99383-197">V tomto příkladu jsme nastavujete `Supplier` vlastnost na produktu.</span><span class="sxs-lookup"><span data-stu-id="99383-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="99383-198">Aktualizovat nebo oprava</span><span class="sxs-lookup"><span data-stu-id="99383-198">Update / Patch</span></span>

<span data-ttu-id="99383-199">Chcete-li aktualizovat entitu, zavolejte **UpdateObject** metody.</span><span class="sxs-lookup"><span data-stu-id="99383-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="99383-200">Aktualizace se provádí při volání **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="99383-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="99383-201">Ve výchozím nastavení WCF odešle požadavek HTTP SLOUČENÍ.</span><span class="sxs-lookup"><span data-stu-id="99383-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="99383-202">**PatchOnUpdate** přikazuje WCF místo odeslání HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="99383-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="99383-203">Proč PATCH a MERGE?</span><span class="sxs-lookup"><span data-stu-id="99383-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="99383-204">Původní specifikaci HTTP 1.1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) nedefinuje žádné metoda protokolu HTTP se sémantikou "částečné aktualizace".</span><span class="sxs-lookup"><span data-stu-id="99383-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="99383-205">Specifikace prostředí OData pro podporu částečné aktualizace definované MERGE – metoda.</span><span class="sxs-lookup"><span data-stu-id="99383-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="99383-206">V roce 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) definované metodu PATCH pro částečné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="99383-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="99383-207">Si můžete přečíst některé z historie v tomto [blogový příspěvek](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) na blogu WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="99383-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="99383-208">OPRAVA je v současné době upřednostňované nad SLOUČENÍ.</span><span class="sxs-lookup"><span data-stu-id="99383-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="99383-209">Kontroler OData vytvořený generování uživatelského rozhraní webového rozhraní API podporuje obě metody.</span><span class="sxs-lookup"><span data-stu-id="99383-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="99383-210">Pokud mají být nahrazeny celou entity (PUT sémantiku), zadejte **ReplaceOnUpdate** možnost.</span><span class="sxs-lookup"><span data-stu-id="99383-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="99383-211">To způsobí, že WCF odeslat požadavek HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="99383-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="99383-212">Odstranit entitu</span><span class="sxs-lookup"><span data-stu-id="99383-212">Delete an Entity</span></span>

<span data-ttu-id="99383-213">Chcete-li odstranit entitu, zavolejte **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="99383-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="99383-214">Vyvolat akci OData</span><span class="sxs-lookup"><span data-stu-id="99383-214">Invoke an OData Action</span></span>

<span data-ttu-id="99383-215">V prostředí OData [akce](odata-actions.md) představují způsob, jak přidat chování na straně serveru, které nejsou snadno definované jako operace CRUD u entit.</span><span class="sxs-lookup"><span data-stu-id="99383-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="99383-216">I když dokument metadat OData popisuje akce, třídy proxy nevytvoří žádné metody silného typu pro ně.</span><span class="sxs-lookup"><span data-stu-id="99383-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="99383-217">Stále můžete vyvolat akci OData pomocí obecného **Execute** metody.</span><span class="sxs-lookup"><span data-stu-id="99383-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="99383-218">Je ale potřeba znát datové typy parametrů a návratové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="99383-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="99383-219">Například `RateProduct` akce přijímá parametr s názvem "Hodnocení" typu `Int32` a vrátí `double`.</span><span class="sxs-lookup"><span data-stu-id="99383-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="99383-220">Následující kód ukazuje, jak vyvolat tuto akci.</span><span class="sxs-lookup"><span data-stu-id="99383-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="99383-221">Další informace najdete v tématu[volání operací služby a akce](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="99383-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="99383-222">Jednou z možností je rozšířit **kontejneru** třídy silného typu metodu, která vyvolá akci:</span><span class="sxs-lookup"><span data-stu-id="99383-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
