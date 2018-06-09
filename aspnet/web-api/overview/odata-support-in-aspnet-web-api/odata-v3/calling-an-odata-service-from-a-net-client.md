---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Volání služby OData z klienta .NET (C#) | Microsoft Docs
author: MikeWasson
description: Tento kurz ukazuje způsob volání služby OData z klientské aplikace C#. Verze softwaru, které jsou používány kurzu Visual Studio 2013 (funguje s Visual S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "28042391"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="83caf-104">Volání služby OData z klienta .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="83caf-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="83caf-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="83caf-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="83caf-106">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="83caf-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="83caf-107">Tento kurz ukazuje způsob volání služby OData z klientské aplikace C#.</span><span class="sxs-lookup"><span data-stu-id="83caf-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="83caf-108">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="83caf-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="83caf-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funguje s Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="83caf-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="83caf-110">Klientská knihovna pro WCF Data Services</span><span class="sxs-lookup"><span data-stu-id="83caf-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="83caf-111">Webové rozhraní API 2.</span><span class="sxs-lookup"><span data-stu-id="83caf-111">Web API 2.</span></span> <span data-ttu-id="83caf-112">(V příkladu služby OData je sestaven pomocí webového rozhraní API 2, ale klientské aplikace není závislá na webové rozhraní API.)</span><span class="sxs-lookup"><span data-stu-id="83caf-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="83caf-113">V tomto kurzu budete I provede procesem vytvoření klientskou aplikaci, která volá služby OData.</span><span class="sxs-lookup"><span data-stu-id="83caf-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="83caf-114">Služba OData zveřejňuje následující entity:</span><span class="sxs-lookup"><span data-stu-id="83caf-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="83caf-115">Na následující články popisují, jak implementovat službu OData v rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="83caf-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="83caf-116">(Není potřeba je pochopit tohoto kurzu, ale přečíst.)</span><span class="sxs-lookup"><span data-stu-id="83caf-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="83caf-117">Vytváření koncový bod OData v rozhraní Web API 2</span><span class="sxs-lookup"><span data-stu-id="83caf-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="83caf-118">Vztahy entit OData v rozhraní Web API 2</span><span class="sxs-lookup"><span data-stu-id="83caf-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="83caf-119">Akce OData ve webovém rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="83caf-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="83caf-120">Generovat Proxy služby</span><span class="sxs-lookup"><span data-stu-id="83caf-120">Generate the Service Proxy</span></span>

<span data-ttu-id="83caf-121">Prvním krokem je ke generování proxy služby.</span><span class="sxs-lookup"><span data-stu-id="83caf-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="83caf-122">Proxy server služby je třída rozhraní .NET, která definuje metody pro přístup ke službě OData.</span><span class="sxs-lookup"><span data-stu-id="83caf-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="83caf-123">Proxy server přeloží volání metod na požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="83caf-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="83caf-124">Začněte otevřením projektu služby OData v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83caf-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="83caf-125">Stisknutím CTRL + F5 a spusťte službu lokálně v IIS Express.</span><span class="sxs-lookup"><span data-stu-id="83caf-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="83caf-126">Všimněte si místní adresy, včetně číslo portu, který přiřazuje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83caf-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="83caf-127">Tuto adresu budete potřebovat při vytváření proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="83caf-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="83caf-128">Dále otevřete jiná instance sady Visual Studio a vytvořte projekt konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="83caf-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="83caf-129">Konzolové aplikace bude naše OData klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="83caf-129">The console application will be our OData client application.</span></span> <span data-ttu-id="83caf-130">(Můžete také přidat projekt do stejného řešení, jako služba.)</span><span class="sxs-lookup"><span data-stu-id="83caf-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="83caf-131">Příklady zbývajících kroků najdete v konzolovém projektu.</span><span class="sxs-lookup"><span data-stu-id="83caf-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="83caf-132">V Průzkumníku řešení klikněte pravým tlačítkem na **odkazy** a vyberte **přidat odkaz na službu**.</span><span class="sxs-lookup"><span data-stu-id="83caf-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="83caf-133">V **přidat odkaz na službu** dialogové okno, zadejte adresu služby OData:</span><span class="sxs-lookup"><span data-stu-id="83caf-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="83caf-134">kde *portu* je číslo portu.</span><span class="sxs-lookup"><span data-stu-id="83caf-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="83caf-135">Pro **Namespace**, zadejte "ProductService".</span><span class="sxs-lookup"><span data-stu-id="83caf-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="83caf-136">Tato možnost definuje obor názvů, třídy proxy.</span><span class="sxs-lookup"><span data-stu-id="83caf-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="83caf-137">Klikněte na tlačítko **přejděte**.</span><span class="sxs-lookup"><span data-stu-id="83caf-137">Click **Go**.</span></span> <span data-ttu-id="83caf-138">Visual Studio přečte dokument metadat OData ke zjišťování entit ve službě.</span><span class="sxs-lookup"><span data-stu-id="83caf-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="83caf-139">Klikněte na tlačítko **OK** k do projektu přidejte třídu proxy.</span><span class="sxs-lookup"><span data-stu-id="83caf-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="83caf-140">Vytvoření Instance třídy Proxy služby</span><span class="sxs-lookup"><span data-stu-id="83caf-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="83caf-141">Uvnitř vaší `Main` metoda, vytvořte novou instanci třídy proxy, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="83caf-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="83caf-142">Znovu použijte skutečný port číslo se spuštěným služby.</span><span class="sxs-lookup"><span data-stu-id="83caf-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="83caf-143">Při nasazení služby, použijete identifikátor URI služby za provozu.</span><span class="sxs-lookup"><span data-stu-id="83caf-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="83caf-144">Nemusíte aktualizovat proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="83caf-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="83caf-145">Následující kód přidá obslužnou rutinu události, který zobrazí žádost o identifikátory URI v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="83caf-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="83caf-146">Tento krok není povinné, ale je zajímavé najdete v části identifikátory URI pro každý dotaz.</span><span class="sxs-lookup"><span data-stu-id="83caf-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="83caf-147">Zadat dotaz na službu</span><span class="sxs-lookup"><span data-stu-id="83caf-147">Query the Service</span></span>

<span data-ttu-id="83caf-148">Následující kód získá seznam produktů od služby OData.</span><span class="sxs-lookup"><span data-stu-id="83caf-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="83caf-149">Všimněte si, že nemusíte psaní jakéhokoli kódu k odeslání požadavku HTTP nebo analyzovat odpověď.</span><span class="sxs-lookup"><span data-stu-id="83caf-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="83caf-150">Neobsahuje třídu proxy tomto automaticky při vytvoření výčtu `Container.Products` kolekce **foreach** smyčky.</span><span class="sxs-lookup"><span data-stu-id="83caf-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="83caf-151">Při spuštění aplikace výstup by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="83caf-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="83caf-152">K načtení entity podle ID, použijte `where` klauzule.</span><span class="sxs-lookup"><span data-stu-id="83caf-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="83caf-153">Pro zbývající část tohoto tématu, nebude zobrazit celý `Main` fungovat pouze kód potřebných k volání služby.</span><span class="sxs-lookup"><span data-stu-id="83caf-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="83caf-154">Použije možnosti dotazu</span><span class="sxs-lookup"><span data-stu-id="83caf-154">Apply Query Options</span></span>

<span data-ttu-id="83caf-155">Definuje OData [možnosti dotazu](../supporting-odata-query-options.md) který slouží k filtrování, řazení, data stránky a tak dále.</span><span class="sxs-lookup"><span data-stu-id="83caf-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="83caf-156">V proxy služby můžete použít tyto možnosti s využitím různých LINQ – výrazy.</span><span class="sxs-lookup"><span data-stu-id="83caf-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="83caf-157">V této části budete zobrazit stručný příklady.</span><span class="sxs-lookup"><span data-stu-id="83caf-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="83caf-158">Další podrobnosti naleznete v tématu [aspekty LINQ (služby WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="83caf-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="83caf-159">Filtrování ($filter)</span><span class="sxs-lookup"><span data-stu-id="83caf-159">Filtering ($filter)</span></span>

<span data-ttu-id="83caf-160">Chcete-li filtrovat, použijte `where` klauzule.</span><span class="sxs-lookup"><span data-stu-id="83caf-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="83caf-161">Následující příklad filtry podle kategorie produktu.</span><span class="sxs-lookup"><span data-stu-id="83caf-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="83caf-162">Tento kód odpovídá následujícího dotazu OData.</span><span class="sxs-lookup"><span data-stu-id="83caf-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="83caf-163">Všimněte si, že proxy převede `where` klauzule do OData `$filter` výraz.</span><span class="sxs-lookup"><span data-stu-id="83caf-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="83caf-164">Řazení ($orderby)</span><span class="sxs-lookup"><span data-stu-id="83caf-164">Sorting ($orderby)</span></span>

<span data-ttu-id="83caf-165">Chcete-li seřadit, použijte `orderby` klauzule.</span><span class="sxs-lookup"><span data-stu-id="83caf-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="83caf-166">V následujícím příkladu se seřadí podle ceny od nejvyšší po nejnižší.</span><span class="sxs-lookup"><span data-stu-id="83caf-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="83caf-167">Zde je odpovídající žádost OData.</span><span class="sxs-lookup"><span data-stu-id="83caf-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="83caf-168">Klientské stránkování ($skip a $top)</span><span class="sxs-lookup"><span data-stu-id="83caf-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="83caf-169">Pro velké entity sady klient může chtít omezit počet výsledků.</span><span class="sxs-lookup"><span data-stu-id="83caf-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="83caf-170">Klient může například zobrazit 10 položek najednou.</span><span class="sxs-lookup"><span data-stu-id="83caf-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="83caf-171">To se označuje jako *stránkování na straně klienta*.</span><span class="sxs-lookup"><span data-stu-id="83caf-171">This is called *client-side paging*.</span></span> <span data-ttu-id="83caf-172">(K dispozici je také [stránkování na straně serveru](../supporting-odata-query-options.md#server-paging), kde server omezí počet výsledků.) Pokud chcete provést stránkování na straně klienta, použijte LINQ **přeskočit** a **trvat** metody.</span><span class="sxs-lookup"><span data-stu-id="83caf-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="83caf-173">V následujícím příkladu přeskočí první 40 výsledky a provede další 10.</span><span class="sxs-lookup"><span data-stu-id="83caf-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="83caf-174">Tady je odpovídající žádost OData:</span><span class="sxs-lookup"><span data-stu-id="83caf-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="83caf-175">Vyberte ($select) a rozbalte ($expand)</span><span class="sxs-lookup"><span data-stu-id="83caf-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="83caf-176">Chcete-li zahrnout entit v relaci, použijte `DataServiceQuery<t>.Expand` metoda.</span><span class="sxs-lookup"><span data-stu-id="83caf-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="83caf-177">Chcete-li například zahrnout `Supplier` pro každou `Product`:</span><span class="sxs-lookup"><span data-stu-id="83caf-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="83caf-178">Tady je odpovídající žádost OData:</span><span class="sxs-lookup"><span data-stu-id="83caf-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="83caf-179">Obrazec odpovědi, můžete změnit LINQ **vyberte** klauzule.</span><span class="sxs-lookup"><span data-stu-id="83caf-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="83caf-180">Následující příklad získá název jednotlivých produktů, bez jakýchkoli dalších vlastností.</span><span class="sxs-lookup"><span data-stu-id="83caf-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="83caf-181">Tady je odpovídající žádost OData:</span><span class="sxs-lookup"><span data-stu-id="83caf-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="83caf-182">Klauzuli select může zahrnovat entit v relaci.</span><span class="sxs-lookup"><span data-stu-id="83caf-182">A select clause can include related entities.</span></span> <span data-ttu-id="83caf-183">V takovém případě Nevolejte **rozbalte**; proxy serveru v takovém případě automaticky zahrne rozšíření.</span><span class="sxs-lookup"><span data-stu-id="83caf-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="83caf-184">Následující příklad načte název a dodavatele každého produktu.</span><span class="sxs-lookup"><span data-stu-id="83caf-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="83caf-185">Zde je odpovídající žádost OData.</span><span class="sxs-lookup"><span data-stu-id="83caf-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="83caf-186">Všimněte si, že obsahuje **$expand** možnost.</span><span class="sxs-lookup"><span data-stu-id="83caf-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="83caf-187">Další informace o $select a $expand rozbalte, najdete v části [pomocí $select, $, rozbalte položku a $value ve webovém rozhraní API 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="83caf-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="83caf-188">Přidat novou entitu</span><span class="sxs-lookup"><span data-stu-id="83caf-188">Add a New Entity</span></span>

<span data-ttu-id="83caf-189">Chcete-li přidat nové entity na sadu entit, volejte `AddToEntitySet`, kde *EntitySet* je název sady entit.</span><span class="sxs-lookup"><span data-stu-id="83caf-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="83caf-190">Například `AddToProducts` přidá nový `Product` k `Products` sady entit.</span><span class="sxs-lookup"><span data-stu-id="83caf-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="83caf-191">Při generování proxy služby WCF Data Services automaticky vytvoří tyto silného typu **AddTo** metody.</span><span class="sxs-lookup"><span data-stu-id="83caf-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="83caf-192">Chcete-li přidat odkaz mezi dvěma entitami, použijte **AddLink** a **SetLink** metody.</span><span class="sxs-lookup"><span data-stu-id="83caf-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="83caf-193">Následující kód přidá nové dodavatele a nového produktu a poté vytvoří propojení mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="83caf-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="83caf-194">Použití **AddLink** po navigační vlastnost kolekce.</span><span class="sxs-lookup"><span data-stu-id="83caf-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="83caf-195">V tomto příkladu přidáváme produkt, který `Products` kolekce na dodavatele.</span><span class="sxs-lookup"><span data-stu-id="83caf-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="83caf-196">Použití **SetLink** po jedné entity navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="83caf-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="83caf-197">V tomto příkladu jsme nastavení `Supplier` vlastnost na produktu.</span><span class="sxs-lookup"><span data-stu-id="83caf-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="83caf-198">Aktualizovat / oprava</span><span class="sxs-lookup"><span data-stu-id="83caf-198">Update / Patch</span></span>

<span data-ttu-id="83caf-199">Chcete-li aktualizovat entitu, volejte **UpdateObject** metoda.</span><span class="sxs-lookup"><span data-stu-id="83caf-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="83caf-200">Aktualizace se provádí při volání **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="83caf-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="83caf-201">Ve výchozím nastavení WCF odešle požadavek HTTP SLOUČENÍ.</span><span class="sxs-lookup"><span data-stu-id="83caf-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="83caf-202">**PatchOnUpdate** možnost informuje WCF místo toho odeslat HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="83caf-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="83caf-203">Proč PATCH a MERGE?</span><span class="sxs-lookup"><span data-stu-id="83caf-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="83caf-204">Původní specifikace protokolu HTTP 1.1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) nebyly definovány žádné metoda HTTP s sémantiku "částečné aktualizace".</span><span class="sxs-lookup"><span data-stu-id="83caf-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="83caf-205">Specifikace prostředí OData pro podporu částečné aktualizace, definována metodu SLOUČENÍ.</span><span class="sxs-lookup"><span data-stu-id="83caf-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="83caf-206">V 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) definované metodu PATCH pro částečné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="83caf-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="83caf-207">Si můžete přečíst některé z historie v tomto [příspěvku na blogu](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) na blogu WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="83caf-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="83caf-208">OPRAVA je v současné době upřednostňované nad SLOUČENÍ.</span><span class="sxs-lookup"><span data-stu-id="83caf-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="83caf-209">Kontroler OData vytvořený generováním uživatelského rozhraní Web API podporuje obě metody.</span><span class="sxs-lookup"><span data-stu-id="83caf-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="83caf-210">Pokud chcete nahradit celý entity (PUT sémantiku), zadejte **ReplaceOnUpdate** možnost.</span><span class="sxs-lookup"><span data-stu-id="83caf-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="83caf-211">To způsobí, že WCF odeslat požadavek HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="83caf-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="83caf-212">Odstranění Entity</span><span class="sxs-lookup"><span data-stu-id="83caf-212">Delete an Entity</span></span>

<span data-ttu-id="83caf-213">Pokud chcete odstranit entitu, volejte **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="83caf-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="83caf-214">Vyvolání akce OData</span><span class="sxs-lookup"><span data-stu-id="83caf-214">Invoke an OData Action</span></span>

<span data-ttu-id="83caf-215">V prostředí OData [akce](odata-actions.md) představují způsob, jak přidat serverové chování, které nejsou snadno definován jako operace CRUD na entity.</span><span class="sxs-lookup"><span data-stu-id="83caf-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="83caf-216">I když dokument metadat OData popisuje akce, třídu proxy nevytvoří žádné metody silného typu pro ně.</span><span class="sxs-lookup"><span data-stu-id="83caf-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="83caf-217">Přesto můžete vyvolat akci OData pomocí Obecné **Execute** metoda.</span><span class="sxs-lookup"><span data-stu-id="83caf-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="83caf-218">Ale musíte vědět, datové typy parametrů a návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="83caf-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="83caf-219">Například `RateProduct` akce má parametr s názvem "Hodnocení" typu `Int32` a vrátí `double`.</span><span class="sxs-lookup"><span data-stu-id="83caf-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="83caf-220">Následující kód ukazuje, jak má být vyvolán tuto akci.</span><span class="sxs-lookup"><span data-stu-id="83caf-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="83caf-221">Další informace najdete v tématu[volání operací služby a akce](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="83caf-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="83caf-222">Jednou z možností je rozšířit **kontejneru** třída zajistit silného typu metodu, která vyvolá akci:</span><span class="sxs-lookup"><span data-stu-id="83caf-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
