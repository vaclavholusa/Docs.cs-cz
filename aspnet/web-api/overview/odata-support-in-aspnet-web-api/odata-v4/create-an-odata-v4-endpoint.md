---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Vytvoření koncového bodu OData v4 pomocí rozhraní ASP.NET Web API 2.2 | Dokumentace Microsoftu
author: MikeWasson
description: Open Data Protocol (OData) je protokol data access pro web. OData nabízí jednotným způsobem pro dotazování a manipulaci s datovými sadami prostřednictvím operace CRUD...
ms.author: riande
ms.date: 06/24/2014
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 48c1a78c96cb0ebfa0b053dfef84e76433112650
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795415"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="95570-104">Vytvoření koncového bodu OData v4 pomocí rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="95570-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="95570-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="95570-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="95570-106">Open Data Protocol (OData) je protokol data access pro web.</span><span class="sxs-lookup"><span data-stu-id="95570-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="95570-107">OData nabízí jednotným způsobem pro dotazování a manipulaci s datovými sadami prostřednictvím operace CRUD (vytváření, čtení, aktualizace a odstranění).</span><span class="sxs-lookup"><span data-stu-id="95570-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
>
> <span data-ttu-id="95570-108">Rozhraní ASP.NET Web API podporuje v3 a v4 protokolu.</span><span class="sxs-lookup"><span data-stu-id="95570-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="95570-109">Může probíhat dokonce na koncový bod v4, na kterém běží vedle sebe s koncovým bodem v3.</span><span class="sxs-lookup"><span data-stu-id="95570-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
>
> <span data-ttu-id="95570-110">Tento kurz ukazuje, jak vytvořit koncový bod OData v4, který podporuje operace CRUD.</span><span class="sxs-lookup"><span data-stu-id="95570-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="95570-111">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="95570-111">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="95570-112">Webové rozhraní API 2.2</span><span class="sxs-lookup"><span data-stu-id="95570-112">Web API 2.2</span></span>
> - <span data-ttu-id="95570-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="95570-113">OData v4</span></span>
> - <span data-ttu-id="95570-114">Visual Studio 2013 (stáhněte si Visual Studio 2017 [tady](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="95570-114">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="95570-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="95570-115">Entity Framework 6</span></span>
> - <span data-ttu-id="95570-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="95570-116">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="95570-117">Kurz verze</span><span class="sxs-lookup"><span data-stu-id="95570-117">Tutorial versions</span></span>
>
> <span data-ttu-id="95570-118">OData verze 3, naleznete v tématu [vytváření koncového bodu OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="95570-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="95570-119">Vytvoření projektu sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95570-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="95570-120">V sadě Visual Studio z **souboru** nabídce vyberte možnost **nový** &gt; **projektu**.</span><span class="sxs-lookup"><span data-stu-id="95570-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="95570-121">Rozbalte **nainstalováno** &gt; **šablony** &gt; **Visual C#** &gt; **webové**a vyberte  **Webová aplikace ASP.NET** šablony.</span><span class="sxs-lookup"><span data-stu-id="95570-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="95570-122">Pojmenujte projekt &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="95570-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="95570-123">V **nový projekt** dialogového okna, vyberte **prázdný** šablony.</span><span class="sxs-lookup"><span data-stu-id="95570-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="95570-124">V části &quot;přidat složky a základní odkazy... &quot;, klikněte na tlačítko **webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="95570-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="95570-125">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="95570-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="95570-126">Instalace balíčků OData</span><span class="sxs-lookup"><span data-stu-id="95570-126">Install the OData Packages</span></span>

<span data-ttu-id="95570-127">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** &gt; **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="95570-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="95570-128">V okně konzoly Správce balíčků zadejte příkaz:</span><span class="sxs-lookup"><span data-stu-id="95570-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="95570-129">Tento příkaz nainstaluje nejnovější balíčky OData NuGet.</span><span class="sxs-lookup"><span data-stu-id="95570-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="95570-130">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="95570-130">Add a Model Class</span></span>

<span data-ttu-id="95570-131">A *modelu* je objekt, který představuje entitu dat ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="95570-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="95570-132">V Průzkumníku řešení klikněte pravým tlačítkem na složku modely.</span><span class="sxs-lookup"><span data-stu-id="95570-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="95570-133">V místní nabídce vyberte **přidat** &gt; **třídy**.</span><span class="sxs-lookup"><span data-stu-id="95570-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="95570-134">Podle konvence tříd modelu jsou umístěny ve složce modely, ale není nutné postupovat podle tohoto vytváření názvů pro svoje vlastní projekty.</span><span class="sxs-lookup"><span data-stu-id="95570-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="95570-135">Název třídy `Product`.</span><span class="sxs-lookup"><span data-stu-id="95570-135">Name the class `Product`.</span></span> <span data-ttu-id="95570-136">V souboru Product.cs nahraďte často používaný kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="95570-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="95570-137">`Id` Vlastnost je klíč entity.</span><span class="sxs-lookup"><span data-stu-id="95570-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="95570-138">Klienti mohou odesílat dotazy entit podle klíče.</span><span class="sxs-lookup"><span data-stu-id="95570-138">Clients can query entities by key.</span></span> <span data-ttu-id="95570-139">Například pokud chcete získat produkt s ID 5, identifikátor URI je `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="95570-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="95570-140">`Id` Vlastnosti také bude primární klíč v back-end databáze.</span><span class="sxs-lookup"><span data-stu-id="95570-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="95570-141">Povolení rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="95570-141">Enable Entity Framework</span></span>

<span data-ttu-id="95570-142">V tomto kurzu použijeme Entity Framework (EF) Code First k vytvoření back-end databáze.</span><span class="sxs-lookup"><span data-stu-id="95570-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="95570-143">Web API OData EF nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="95570-143">Web API OData does not require EF.</span></span> <span data-ttu-id="95570-144">Použijte všechny vrstvy přístup k datům, která jsou dobře převeditelné entity databáze do modelů.</span><span class="sxs-lookup"><span data-stu-id="95570-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="95570-145">Nejdřív nainstalujte balíček NuGet pro EF.</span><span class="sxs-lookup"><span data-stu-id="95570-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="95570-146">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** &gt; **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="95570-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="95570-147">V okně konzoly Správce balíčků zadejte příkaz:</span><span class="sxs-lookup"><span data-stu-id="95570-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="95570-148">Otevřete soubor Web.config a přidat následující části uvnitř **konfigurace** element, za **configSections** elementu.</span><span class="sxs-lookup"><span data-stu-id="95570-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="95570-149">Toto nastavení přidá připojovacího řetězce pro databázi LocalDB.</span><span class="sxs-lookup"><span data-stu-id="95570-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="95570-150">Tato databáze se použije při spuštění aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="95570-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="95570-151">V dalším kroku přidejte třídu pojmenovanou `ProductsContext` ke složce modely:</span><span class="sxs-lookup"><span data-stu-id="95570-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="95570-152">V konstruktoru `"name=ProductsContext"` poskytuje název připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="95570-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="95570-153">Konfigurace koncového bodu OData</span><span class="sxs-lookup"><span data-stu-id="95570-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="95570-154">Otevřete soubor aplikace\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="95570-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="95570-155">Přidejte následující **pomocí** příkazy:</span><span class="sxs-lookup"><span data-stu-id="95570-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="95570-156">Pak přidejte následující kód, který **zaregistrovat** metody:</span><span class="sxs-lookup"><span data-stu-id="95570-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="95570-157">Tento kód provede dvě věci:</span><span class="sxs-lookup"><span data-stu-id="95570-157">This code does two things:</span></span>

- <span data-ttu-id="95570-158">Vytvoří Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="95570-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="95570-159">Přidá trasu.</span><span class="sxs-lookup"><span data-stu-id="95570-159">Adds a route.</span></span>

<span data-ttu-id="95570-160">EDM je abstraktní modelem data.</span><span class="sxs-lookup"><span data-stu-id="95570-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="95570-161">EDM slouží k vytvoření dokumentu metadat služby.</span><span class="sxs-lookup"><span data-stu-id="95570-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="95570-162">**ODataConventionModelBuilder** třídy pomocí výchozí konvence pojmenování vytvoří modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="95570-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="95570-163">Tento přístup vyžaduje nejméně kódu.</span><span class="sxs-lookup"><span data-stu-id="95570-163">This approach requires the least code.</span></span> <span data-ttu-id="95570-164">Pokud chcete mít větší kontrolu nad EDM, můžete použít **Tvůrce ODataModelBuilder** třídy za účelem vytvoření tak, že explicitně přidáte vlastností, klíčů a navigačních vlastností EDM.</span><span class="sxs-lookup"><span data-stu-id="95570-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="95570-165">A *trasy* informuje webového rozhraní API, jak směrovat požadavky HTTP na koncový bod.</span><span class="sxs-lookup"><span data-stu-id="95570-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="95570-166">Chcete-li vytvořit trasy OData v4, zavolejte **MapODataServiceRoute** – metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="95570-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="95570-167">Pokud aplikace obsahuje více koncových bodů protokolu OData, vytvořte pro každou samostatnou trasu.</span><span class="sxs-lookup"><span data-stu-id="95570-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="95570-168">Zadejte všechny trasy, trasy jedinečný název a předpony.</span><span class="sxs-lookup"><span data-stu-id="95570-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="95570-169">Přidat kontroler OData</span><span class="sxs-lookup"><span data-stu-id="95570-169">Add the OData Controller</span></span>

<span data-ttu-id="95570-170">A *řadič* je třída, která zpracovává požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="95570-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="95570-171">Můžete vytvořit samostatné kontrolerem pro každou sadu entit v služby OData.</span><span class="sxs-lookup"><span data-stu-id="95570-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="95570-172">V tomto kurzu vytvoříte jeden řadič pro `Product` entity.</span><span class="sxs-lookup"><span data-stu-id="95570-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="95570-173">V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče a vyberte **přidat** &gt; **třídy**.</span><span class="sxs-lookup"><span data-stu-id="95570-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="95570-174">Název třídy `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="95570-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="95570-175">Verzi tohoto kurzu pro prostředí OData v3 používá **přidat kontroler** generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="95570-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="95570-176">V současné době neexistuje žádný generování uživatelského rozhraní pro OData v4.</span><span class="sxs-lookup"><span data-stu-id="95570-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="95570-177">Často používaný kód v ProductsController.cs nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="95570-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="95570-178">Využívá kontroler `ProductsContext` pro přístup k databázi pomocí EF.</span><span class="sxs-lookup"><span data-stu-id="95570-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="95570-179">Všimněte si, že přepíše kontroleru **Dispose** metoda a neprovede **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="95570-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="95570-180">Toto je výchozí bod pro kontroler.</span><span class="sxs-lookup"><span data-stu-id="95570-180">This is the starting point for the controller.</span></span> <span data-ttu-id="95570-181">V dalším kroku přidáme metody pro všechny operace CRUD.</span><span class="sxs-lookup"><span data-stu-id="95570-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="95570-182">Dotazování na sadu entit</span><span class="sxs-lookup"><span data-stu-id="95570-182">Querying the Entity Set</span></span>

<span data-ttu-id="95570-183">Přidejte následující metody, které `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="95570-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="95570-184">Konstruktor bez parametrů verzi `Get` metoda vrátí celou kolekci produktů.</span><span class="sxs-lookup"><span data-stu-id="95570-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="95570-185">`Get` Metody *klíč* parametr vyhledá produktu podle jeho klíče (v tomto případě `Id` vlastnost).</span><span class="sxs-lookup"><span data-stu-id="95570-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="95570-186">**[EnableQuery]** atribut umožňuje upravit dotaz, pomocí možnosti dotazu, jako je například $filter, $sort a $page klientům.</span><span class="sxs-lookup"><span data-stu-id="95570-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="95570-187">Další informace najdete v tématu [podporuje možnosti dotazu OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="95570-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="95570-188">Přidání Entity do sady entit</span><span class="sxs-lookup"><span data-stu-id="95570-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="95570-189">Pokud chcete povolit klientům přidání nového produktu do databáze, přidejte následující metodu do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="95570-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="95570-190">Aktualizují se Entity</span><span class="sxs-lookup"><span data-stu-id="95570-190">Updating an Entity</span></span>

<span data-ttu-id="95570-191">OData podporuje dvě různé sémantiky pro aktualizaci entity, opravy a PUT.</span><span class="sxs-lookup"><span data-stu-id="95570-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="95570-192">PATCH provede částečnou aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="95570-192">PATCH performs a partial update.</span></span> <span data-ttu-id="95570-193">Klient určuje vlastnosti k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="95570-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="95570-194">PUT nahradí celý entity.</span><span class="sxs-lookup"><span data-stu-id="95570-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="95570-195">Nevýhodou PUT je, že klient musí odesílat hodnoty pro všechny vlastnosti v entitě, včetně hodnoty, které se mění.</span><span class="sxs-lookup"><span data-stu-id="95570-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="95570-196">[Specifikace prostředí OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) uvádí, že je upřednostňovaný opravy.</span><span class="sxs-lookup"><span data-stu-id="95570-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="95570-197">V každém případě zde je kód pro opravu a PUT metody:</span><span class="sxs-lookup"><span data-stu-id="95570-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="95570-198">V případě opravy, využívá kontroler **Delta&lt;T&gt;**  typ sledovat změny.</span><span class="sxs-lookup"><span data-stu-id="95570-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="95570-199">Odstraňuje se entita</span><span class="sxs-lookup"><span data-stu-id="95570-199">Deleting an Entity</span></span>

<span data-ttu-id="95570-200">Pokud chcete povolit klientům z databáze odstranit produkt, přidejte následující metodu do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="95570-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
