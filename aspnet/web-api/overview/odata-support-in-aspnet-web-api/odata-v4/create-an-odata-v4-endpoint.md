---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Vytvořit koncový bod OData v4 pomocí rozhraní ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) je protokol přístupu dat pro web. OData poskytuje jednotným způsobem pro dotazování a zpracování datových sad prostřednictvím operace CRUD...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566719"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="f10b5-104">Vytvořit koncový bod OData v4 pomocí rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="f10b5-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="f10b5-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f10b5-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="f10b5-106">Open Data Protocol (OData) je protokol přístupu dat pro web.</span><span class="sxs-lookup"><span data-stu-id="f10b5-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="f10b5-107">Poskytuje jednotným způsobem pro dotazování a zpracování datových sad prostřednictvím operace CRUD OData (vytvářet, číst, aktualizovat a odstranit).</span><span class="sxs-lookup"><span data-stu-id="f10b5-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
> 
> <span data-ttu-id="f10b5-108">Rozhraní ASP.NET Web API podporuje v3 a v4 protokolu.</span><span class="sxs-lookup"><span data-stu-id="f10b5-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="f10b5-109">Můžete mít i v4 koncový bod, který běží souběžně sdílená s koncovým bodem v3.</span><span class="sxs-lookup"><span data-stu-id="f10b5-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
> 
> <span data-ttu-id="f10b5-110">Tento kurz ukazuje, jak vytvořit koncový bod OData v4, který podporuje operace CRUD.</span><span class="sxs-lookup"><span data-stu-id="f10b5-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f10b5-111">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="f10b5-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f10b5-112">Webové rozhraní API 2.2</span><span class="sxs-lookup"><span data-stu-id="f10b5-112">Web API 2.2</span></span>
> - <span data-ttu-id="f10b5-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="f10b5-113">OData v4</span></span>
> - [<span data-ttu-id="f10b5-114">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="f10b5-114">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="f10b5-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="f10b5-115">Entity Framework 6</span></span>
> - <span data-ttu-id="f10b5-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f10b5-116">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="f10b5-117">Kurz verze</span><span class="sxs-lookup"><span data-stu-id="f10b5-117">Tutorial versions</span></span>
> 
> <span data-ttu-id="f10b5-118">OData verze 3, najdete v části [vytváření koncový bod OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="f10b5-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="f10b5-119">Vytvoření projektu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f10b5-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="f10b5-120">V sadě Visual Studio z **soubor** nabídce vyberte možnost **nový** &gt; **projektu**.</span><span class="sxs-lookup"><span data-stu-id="f10b5-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="f10b5-121">Rozbalte položku **nainstalovaná** &gt; **šablony** &gt; **Visual C#** &gt; **webové**a vyberte  **Webové aplikace ASP.NET** šablony.</span><span class="sxs-lookup"><span data-stu-id="f10b5-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="f10b5-122">Název projektu &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="f10b5-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="f10b5-123">V **nový projekt** dialogovém okně, vyberte **prázdný** šablony.</span><span class="sxs-lookup"><span data-stu-id="f10b5-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="f10b5-124">V části &quot;přidat složky a odkazy na základní... &quot;, klikněte na tlačítko **webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="f10b5-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="f10b5-125">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="f10b5-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="f10b5-126">Instalovat balíčky OData</span><span class="sxs-lookup"><span data-stu-id="f10b5-126">Install the OData Packages</span></span>

<span data-ttu-id="f10b5-127">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** &gt; **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="f10b5-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="f10b5-128">V okně konzoly Správce balíčků zadejte:</span><span class="sxs-lookup"><span data-stu-id="f10b5-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="f10b5-129">Tento příkaz nainstaluje nejnovější balíčky OData NuGet.</span><span class="sxs-lookup"><span data-stu-id="f10b5-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="f10b5-130">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="f10b5-130">Add a Model Class</span></span>

<span data-ttu-id="f10b5-131">A *modelu* je objekt, který představuje data entity ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f10b5-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="f10b5-132">V Průzkumníku řešení klikněte pravým tlačítkem na složku modely.</span><span class="sxs-lookup"><span data-stu-id="f10b5-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="f10b5-133">V místní nabídce vyberte **přidat** &gt; **třída**.</span><span class="sxs-lookup"><span data-stu-id="f10b5-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="f10b5-134">Podle konvence třídy modelu jsou umístěny ve složce modely, ale nemáte postupujte podle touto konvencí ve vašich vlastních projektů.</span><span class="sxs-lookup"><span data-stu-id="f10b5-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="f10b5-135">Název třídy `Product`.</span><span class="sxs-lookup"><span data-stu-id="f10b5-135">Name the class `Product`.</span></span> <span data-ttu-id="f10b5-136">V souboru Product.cs nahraďte často používaný kód následující:</span><span class="sxs-lookup"><span data-stu-id="f10b5-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="f10b5-137">`Id` Je vlastnost klíčem entity.</span><span class="sxs-lookup"><span data-stu-id="f10b5-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="f10b5-138">Klienti mohou odesílat dotazy entit podle klíče.</span><span class="sxs-lookup"><span data-stu-id="f10b5-138">Clients can query entities by key.</span></span> <span data-ttu-id="f10b5-139">Například pokud chcete získat produktu s ID 5, je identifikátor URI `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="f10b5-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="f10b5-140">`Id` Vlastnost bude také primární klíč v databázi back-end.</span><span class="sxs-lookup"><span data-stu-id="f10b5-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="f10b5-141">Povolit rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f10b5-141">Enable Entity Framework</span></span>

<span data-ttu-id="f10b5-142">V tomto kurzu použijeme Entity Framework (EF) Code First k vytvoření databáze back-end.</span><span class="sxs-lookup"><span data-stu-id="f10b5-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="f10b5-143">Web API OData nevyžaduje EF.</span><span class="sxs-lookup"><span data-stu-id="f10b5-143">Web API OData does not require EF.</span></span> <span data-ttu-id="f10b5-144">Použijte všechny vrstva přístupu k datům, která může překládat entity databáze do modely.</span><span class="sxs-lookup"><span data-stu-id="f10b5-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="f10b5-145">Nejdřív nainstalujte balíček NuGet pro EF.</span><span class="sxs-lookup"><span data-stu-id="f10b5-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="f10b5-146">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** &gt; **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="f10b5-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="f10b5-147">V okně konzoly Správce balíčků zadejte:</span><span class="sxs-lookup"><span data-stu-id="f10b5-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="f10b5-148">Otevřete soubor Web.config a přidejte následující části uvnitř **konfigurace** element, po **configSections** elementu.</span><span class="sxs-lookup"><span data-stu-id="f10b5-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="f10b5-149">Toto nastavení přidá připojovací řetězec databáze LocalDB.</span><span class="sxs-lookup"><span data-stu-id="f10b5-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="f10b5-150">Tato databáze se použije při spuštění aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="f10b5-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="f10b5-151">V dalším kroku přidejte třídu s názvem `ProductsContext` ke složce modely:</span><span class="sxs-lookup"><span data-stu-id="f10b5-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="f10b5-152">V konstruktoru `"name=ProductsContext"` poskytuje název připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="f10b5-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="f10b5-153">Konfigurace koncového bodu OData</span><span class="sxs-lookup"><span data-stu-id="f10b5-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="f10b5-154">Otevřete soubor aplikace\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="f10b5-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="f10b5-155">Přidejte následující **pomocí** příkazy:</span><span class="sxs-lookup"><span data-stu-id="f10b5-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="f10b5-156">Pak přidejte následující kód, který **zaregistrovat** metoda:</span><span class="sxs-lookup"><span data-stu-id="f10b5-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="f10b5-157">Tento kód provede dvě věci:</span><span class="sxs-lookup"><span data-stu-id="f10b5-157">This code does two things:</span></span>

- <span data-ttu-id="f10b5-158">Vytvoří datový Model Entity (EDM).</span><span class="sxs-lookup"><span data-stu-id="f10b5-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="f10b5-159">Přidá trasy.</span><span class="sxs-lookup"><span data-stu-id="f10b5-159">Adds a route.</span></span>

<span data-ttu-id="f10b5-160">EDM je abstraktní model dat.</span><span class="sxs-lookup"><span data-stu-id="f10b5-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="f10b5-161">EDM se používá k vytvoření dokumentu metadat služby.</span><span class="sxs-lookup"><span data-stu-id="f10b5-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="f10b5-162">**ODataConventionModelBuilder** třída vytvoří EDM pomocí výchozí zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="f10b5-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="f10b5-163">Tento postup vyžaduje minimálně kódu.</span><span class="sxs-lookup"><span data-stu-id="f10b5-163">This approach requires the least code.</span></span> <span data-ttu-id="f10b5-164">Pokud chcete mít větší kontrolu nad EDM, můžete použít **Tvůrce ODataModelBuilder** třídy za účelem vytvoření modelu EDM přidáním vlastností, klíčů a navigačních vlastností explicitně.</span><span class="sxs-lookup"><span data-stu-id="f10b5-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="f10b5-165">A *trasy* informuje webového rozhraní API, jak pro směrování požadavků HTTP na koncový bod.</span><span class="sxs-lookup"><span data-stu-id="f10b5-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="f10b5-166">Chcete-li vytvořit trasu OData v4, zavolejte **MapODataServiceRoute** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f10b5-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="f10b5-167">Pokud aplikace obsahuje více koncových bodů protokolu OData, vytvořte pro každý samostatný trasy.</span><span class="sxs-lookup"><span data-stu-id="f10b5-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="f10b5-168">Každý postup poskytněte název jedinečný trasy a předponu.</span><span class="sxs-lookup"><span data-stu-id="f10b5-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="f10b5-169">Přidat kontroler OData</span><span class="sxs-lookup"><span data-stu-id="f10b5-169">Add the OData Controller</span></span>

<span data-ttu-id="f10b5-170">A *řadič* je třída, která zpracovává požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="f10b5-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="f10b5-171">Můžete vytvořit samostatný řadič pro každou sadu entit v služby OData.</span><span class="sxs-lookup"><span data-stu-id="f10b5-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="f10b5-172">V tomto kurzu vytvoříte jeden řadič pro `Product` entity.</span><span class="sxs-lookup"><span data-stu-id="f10b5-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="f10b5-173">V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče a vyberte **přidat** &gt; **třída**.</span><span class="sxs-lookup"><span data-stu-id="f10b5-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="f10b5-174">Název třídy `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="f10b5-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="f10b5-175">Verze tohoto kurzu pro OData v3 používá **přidat kontroler** generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f10b5-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="f10b5-176">V současné době není k dispozici žádné generování uživatelského rozhraní pro OData v4.</span><span class="sxs-lookup"><span data-stu-id="f10b5-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="f10b5-177">Nahraďte kód často používaný v ProductsController.cs následující.</span><span class="sxs-lookup"><span data-stu-id="f10b5-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="f10b5-178">Používá řadič `ProductsContext` třídy pro přístup k databázi pomocí EF.</span><span class="sxs-lookup"><span data-stu-id="f10b5-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="f10b5-179">Všimněte si, že přepíše řadičem **Dispose** metodu pro odstranění **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="f10b5-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="f10b5-180">Toto je výchozí bod pro kontroler.</span><span class="sxs-lookup"><span data-stu-id="f10b5-180">This is the starting point for the controller.</span></span> <span data-ttu-id="f10b5-181">Potom přidáme metody pro všechny operace CRUD.</span><span class="sxs-lookup"><span data-stu-id="f10b5-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="f10b5-182">Dotaz na sadu entit</span><span class="sxs-lookup"><span data-stu-id="f10b5-182">Querying the Entity Set</span></span>

<span data-ttu-id="f10b5-183">Přidejte následující metody, které `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="f10b5-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="f10b5-184">Bez parametrů verzi `Get` metoda vrátí celou kolekci produkty.</span><span class="sxs-lookup"><span data-stu-id="f10b5-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="f10b5-185">`Get` Metoda s *klíč* parametr vyhledá produkt na základě klíče (v takovém případě `Id` vlastnost).</span><span class="sxs-lookup"><span data-stu-id="f10b5-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="f10b5-186">**[EnableQuery]** atribut umožňuje klientům upravit dotaz, pomocí možnosti dotazu, jako je například $filter, $sort a $page.</span><span class="sxs-lookup"><span data-stu-id="f10b5-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="f10b5-187">Další informace najdete v tématu [podporu možností dotazu OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="f10b5-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="f10b5-188">Přidání Entity do sady entit</span><span class="sxs-lookup"><span data-stu-id="f10b5-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="f10b5-189">Povolit klientům přidání nového produktu do databáze, přidejte následující metodu do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="f10b5-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="f10b5-190">Aktualizace Entity</span><span class="sxs-lookup"><span data-stu-id="f10b5-190">Updating an Entity</span></span>

<span data-ttu-id="f10b5-191">OData podporuje dva různé sémantiku pro aktualizaci entity, opravy a PUT.</span><span class="sxs-lookup"><span data-stu-id="f10b5-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="f10b5-192">OPRAVA provede částečné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f10b5-192">PATCH performs a partial update.</span></span> <span data-ttu-id="f10b5-193">Klient určuje vlastnosti aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f10b5-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="f10b5-194">PUT nahradí celý entity.</span><span class="sxs-lookup"><span data-stu-id="f10b5-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="f10b5-195">Nevýhodou PUT je, že klient musí odeslat hodnoty pro všechny vlastnosti entity, včetně hodnot, které nejsou změna.</span><span class="sxs-lookup"><span data-stu-id="f10b5-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="f10b5-196">[OData specifikace](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) uvádí, že je upřednostňovaný opravy.</span><span class="sxs-lookup"><span data-stu-id="f10b5-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="f10b5-197">V každém případě tady je kód pro opravy a PUT metody:</span><span class="sxs-lookup"><span data-stu-id="f10b5-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="f10b5-198">V případě opravy, používá řadičem **rozdílů&lt;T&gt;**  typ sledovat změny.</span><span class="sxs-lookup"><span data-stu-id="f10b5-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="f10b5-199">Odstranění Entity</span><span class="sxs-lookup"><span data-stu-id="f10b5-199">Deleting an Entity</span></span>

<span data-ttu-id="f10b5-200">Pokud chcete povolit klientům z databáze odstranit produktu, přidejte následující metodu do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="f10b5-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
