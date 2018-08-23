---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Vytvoření koncového bodu OData v3 s webovým rozhraním API 2 | Dokumentace Microsoftu
author: MikeWasson
description: Open Data Protocol (OData) je protokol data access pro web. OData nabízí jednotné způsob, jak strukturovat data, zadávat dotazy na data a manipulaci s daty...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 654f697c8d095d45ba31e2808c52f9ad24b606c8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756671"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="91bbd-104">Vytvoření koncového bodu OData v3 s webovým rozhraním API 2</span><span class="sxs-lookup"><span data-stu-id="91bbd-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="91bbd-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="91bbd-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="91bbd-106">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="91bbd-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="91bbd-107">[Open Data Protocol](http://www.odata.org/) (OData) je protokol data access pro web.</span><span class="sxs-lookup"><span data-stu-id="91bbd-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="91bbd-108">OData nabízí jednotné způsob, jak strukturovat data, zadávat dotazy na data a manipulovat s sady dat prostřednictvím operace CRUD (vytváření, čtení, aktualizace a odstranění).</span><span class="sxs-lookup"><span data-stu-id="91bbd-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="91bbd-109">OData podporuje formáty JSON a AtomPub (XML).</span><span class="sxs-lookup"><span data-stu-id="91bbd-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="91bbd-110">OData definuje také způsob, jak vystavit metadata o datech.</span><span class="sxs-lookup"><span data-stu-id="91bbd-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="91bbd-111">Klienti mohou používat metadata a zjistit informace o typu a vztahy pro datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
> 
> <span data-ttu-id="91bbd-112">Rozhraní ASP.NET Web API usnadňuje vytvoření koncového bodu OData pro datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="91bbd-113">Můžete řídit přesně OData operace, které podporuje koncový bod.</span><span class="sxs-lookup"><span data-stu-id="91bbd-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="91bbd-114">Můžete hostovat víc koncových bodů protokolu OData, společně s koncovými body mimo prostředí OData.</span><span class="sxs-lookup"><span data-stu-id="91bbd-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="91bbd-115">Máte plnou kontrolu nad datový model, back-end obchodní logiku a data vrstev.</span><span class="sxs-lookup"><span data-stu-id="91bbd-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="91bbd-116">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="91bbd-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="91bbd-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="91bbd-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="91bbd-118">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="91bbd-118">Web API 2</span></span>
> - <span data-ttu-id="91bbd-119">OData verze 3</span><span class="sxs-lookup"><span data-stu-id="91bbd-119">OData Version 3</span></span>
> - <span data-ttu-id="91bbd-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="91bbd-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="91bbd-121">Fiddler webový ladicí proxy server (volitelné)</span><span class="sxs-lookup"><span data-stu-id="91bbd-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
> 
> <span data-ttu-id="91bbd-122">Přidala se podpora web API OData v [technologie ASP.NET a Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="91bbd-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="91bbd-123">Tento kurz používá však generování uživatelského rozhraní, která byla přidána do sady Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="91bbd-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="91bbd-124">V tomto kurzu vytvoříte jednoduchou koncový bod OData, který můžou klienti dotazovat.</span><span class="sxs-lookup"><span data-stu-id="91bbd-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="91bbd-125">Pokud vytvoříte klienta jazyka C# pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="91bbd-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="91bbd-126">Po dokončení tohoto kurzu, další sadu kurzy ukazují, jak přidat další funkce, včetně vztahů entit, akce, a vyberte rozbalte $/ $.</span><span class="sxs-lookup"><span data-stu-id="91bbd-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="91bbd-127">Vytvoření projektu sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91bbd-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="91bbd-128">Přidání modelu Entity</span><span class="sxs-lookup"><span data-stu-id="91bbd-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="91bbd-129">Přidat kontroler OData</span><span class="sxs-lookup"><span data-stu-id="91bbd-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="91bbd-130">Přidat EDM a trasy</span><span class="sxs-lookup"><span data-stu-id="91bbd-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="91bbd-131">Přidání dat do databáze (volitelné)</span><span class="sxs-lookup"><span data-stu-id="91bbd-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="91bbd-132">Zkoumání koncového bodu OData</span><span class="sxs-lookup"><span data-stu-id="91bbd-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="91bbd-133">Formáty serializace OData</span><span class="sxs-lookup"><span data-stu-id="91bbd-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="91bbd-134">Vytvoření projektu sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91bbd-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="91bbd-135">V tomto kurzu vytvoříte koncový bod OData, který podporuje základní operace CRUD.</span><span class="sxs-lookup"><span data-stu-id="91bbd-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="91bbd-136">Koncový bod bude vystavovat jeden prostředek, seznam produktů.</span><span class="sxs-lookup"><span data-stu-id="91bbd-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="91bbd-137">Dalších kurzech přidáte další funkce.</span><span class="sxs-lookup"><span data-stu-id="91bbd-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="91bbd-138">Spusťte sadu Visual Studio a vyberte **nový projekt** z úvodní stránky.</span><span class="sxs-lookup"><span data-stu-id="91bbd-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="91bbd-139">Nebo z **souboru** nabídce vyberte možnost **nový** a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="91bbd-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="91bbd-140">V **šablony** vyberte **nainstalované šablony** a rozbalte uzel Visual C#.</span><span class="sxs-lookup"><span data-stu-id="91bbd-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="91bbd-141">V části **Visual C#** vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="91bbd-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="91bbd-142">Vyberte **webová aplikace ASP.NET** šablony.</span><span class="sxs-lookup"><span data-stu-id="91bbd-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="91bbd-143">V **nový projekt ASP.NET** dialogového okna, vyberte **prázdný** šablony.</span><span class="sxs-lookup"><span data-stu-id="91bbd-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="91bbd-144">V části &quot;přidat složky a základní odkazy pro... &quot;, zkontrolujte **webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="91bbd-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="91bbd-145">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="91bbd-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="91bbd-146">Přidání modelu Entity</span><span class="sxs-lookup"><span data-stu-id="91bbd-146">Add an Entity Model</span></span>

<span data-ttu-id="91bbd-147">A *modelu* je objekt, který představuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="91bbd-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="91bbd-148">Pro účely tohoto kurzu potřebujeme model, který představuje produkt.</span><span class="sxs-lookup"><span data-stu-id="91bbd-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="91bbd-149">Model odpovídá naše prostředí OData typu entity.</span><span class="sxs-lookup"><span data-stu-id="91bbd-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="91bbd-150">V Průzkumníku řešení klikněte pravým tlačítkem na složku modely.</span><span class="sxs-lookup"><span data-stu-id="91bbd-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="91bbd-151">V místní nabídce vyberte **přidat** vyberte **třídy**.</span><span class="sxs-lookup"><span data-stu-id="91bbd-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="91bbd-152">V **přidat nový** položky dialogového okna, název třídy &quot;produktu&quot;.</span><span class="sxs-lookup"><span data-stu-id="91bbd-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="91bbd-153">Podle konvence tříd modelu jsou umístěny ve složce modely.</span><span class="sxs-lookup"><span data-stu-id="91bbd-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="91bbd-154">Nemusíte si odpovídají této konvenci ve vašich vlastních projektů, ale použijeme ho pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="91bbd-155">V souboru Product.cs přidejte následující definici třídy:</span><span class="sxs-lookup"><span data-stu-id="91bbd-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="91bbd-156">Vlastnost ID bude klíč entity.</span><span class="sxs-lookup"><span data-stu-id="91bbd-156">The ID property will be the entity key.</span></span> <span data-ttu-id="91bbd-157">Klienti mohou odesílat dotazy produkty podle ID.</span><span class="sxs-lookup"><span data-stu-id="91bbd-157">Clients can query products by ID.</span></span> <span data-ttu-id="91bbd-158">Toto pole by také být primární klíč v back-end databáze.</span><span class="sxs-lookup"><span data-stu-id="91bbd-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="91bbd-159">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="91bbd-159">Build the project now.</span></span> <span data-ttu-id="91bbd-160">V dalším kroku použijeme některé sady Visual Studio generování uživatelského rozhraní, který používá reflexi najít typ produktu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="91bbd-161">Přidat kontroler OData</span><span class="sxs-lookup"><span data-stu-id="91bbd-161">Add an OData Controller</span></span>

<span data-ttu-id="91bbd-162">A *řadič* je třída, která zpracovává požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="91bbd-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="91bbd-163">Definujte samostatný kontrolerem pro každou sadu entit v můžete službu OData.</span><span class="sxs-lookup"><span data-stu-id="91bbd-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="91bbd-164">V tomto kurzu vytvoříme jediného kontroleru.</span><span class="sxs-lookup"><span data-stu-id="91bbd-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="91bbd-165">V Průzkumníku řešení klikněte pravým tlačítkem myši na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="91bbd-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="91bbd-166">Vyberte **přidat** a pak vyberte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="91bbd-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="91bbd-167">V **přidat vygenerované uživatelské rozhraní** dialogového okna, vyberte &quot;Kontroleru webového rozhraní API 2 OData s akcemi používající nástroj Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="91bbd-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="91bbd-168">V **přidat kontroler** dialogového okna, názvu kontroleru "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="91bbd-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="91bbd-169">Vyberte &quot;použít asynchronní akce kontroleru&quot; zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="91bbd-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="91bbd-170">V **modelu** rozevíracího seznamu vyberte třídu produktu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="91bbd-171">Klikněte na tlačítko **nový kontext dat...**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="91bbd-171">Click the **New data context...** button.</span></span> <span data-ttu-id="91bbd-172">Ponechte výchozí název typu datového kontextu a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="91bbd-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="91bbd-173">V dialogovém okně Přidat kontroler, přidat kontroler, klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="91bbd-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="91bbd-174">Poznámka: Pokud se zobrazí chybová zpráva, která uvádí, že &quot;došlo k chybě při získávání typu... &quot;, ujistěte se, že jste sestavili projekt sady Visual Studio po přidání třídy produktu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="91bbd-175">Základní kostry aplikace používá reflexi k nalezení třídy.</span><span class="sxs-lookup"><span data-stu-id="91bbd-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="91bbd-176">Základní kostry aplikace přidá do projektu dva soubory kódu:</span><span class="sxs-lookup"><span data-stu-id="91bbd-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="91bbd-177">Products.cs definuje kontroler Web API, která implementuje koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="91bbd-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="91bbd-178">ProductServiceContext.cs poskytuje metody pro dotazování databáze pomocí Entity Frameworku.</span><span class="sxs-lookup"><span data-stu-id="91bbd-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="91bbd-179">Přidat EDM a trasy</span><span class="sxs-lookup"><span data-stu-id="91bbd-179">Add the EDM and Route</span></span>

<span data-ttu-id="91bbd-180">V Průzkumníku řešení rozbalte aplikace\_spusťte složky a otevřete soubor s názvem WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="91bbd-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="91bbd-181">Tato třída obsahuje konfigurační kód pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="91bbd-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="91bbd-182">Nahraďte tento kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="91bbd-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="91bbd-183">Tento kód provede dvě věci:</span><span class="sxs-lookup"><span data-stu-id="91bbd-183">This code does two things:</span></span>

- <span data-ttu-id="91bbd-184">Vytvoří Entity Data Model (EDM) pro koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="91bbd-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="91bbd-185">Přidá trasu pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="91bbd-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="91bbd-186">EDM je abstraktní modelem data.</span><span class="sxs-lookup"><span data-stu-id="91bbd-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="91bbd-187">EDM slouží k vytvoření dokumentu metadata a definovat identifikátory URI pro službu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="91bbd-188">**ODataConventionModelBuilder** vytvoří EDM tak, že používáte sadu výchozích konvencí pojmenování EDM.</span><span class="sxs-lookup"><span data-stu-id="91bbd-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="91bbd-189">Tento přístup vyžaduje nejméně kódu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-189">This approach requires the least code.</span></span> <span data-ttu-id="91bbd-190">Pokud chcete mít větší kontrolu nad EDM, můžete použít **Tvůrce ODataModelBuilder** třídy za účelem vytvoření tak, že explicitně přidáte vlastností, klíčů a navigačních vlastností EDM.</span><span class="sxs-lookup"><span data-stu-id="91bbd-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="91bbd-191">**Objektu EntitySet** metoda přidá do modelu EDM sadu entit:</span><span class="sxs-lookup"><span data-stu-id="91bbd-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="91bbd-192">Řetězec "Produktů" definuje název sady entit.</span><span class="sxs-lookup"><span data-stu-id="91bbd-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="91bbd-193">Název kontroleru musí odpovídat názvu sady entit.</span><span class="sxs-lookup"><span data-stu-id="91bbd-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="91bbd-194">V tomto kurzu je název sady entit s názvem "Produktů" a má název kontroleru `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="91bbd-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="91bbd-195">Pokud pojmenujete "ProductSet" sada entit, bude název kontroleru `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="91bbd-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="91bbd-196">Všimněte si, že koncový bod může mít více sad entit.</span><span class="sxs-lookup"><span data-stu-id="91bbd-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="91bbd-197">Volání **objektu EntitySet&lt;T&gt;**  pro každou entitu nastavení a pak definovat odpovídající kontroler.</span><span class="sxs-lookup"><span data-stu-id="91bbd-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="91bbd-198">**MapODataRoute** metoda přidá trasu pro koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="91bbd-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="91bbd-199">První parametr je popisný název pro tuto trasu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="91bbd-200">Klienti služby se tento název nezobrazuje.</span><span class="sxs-lookup"><span data-stu-id="91bbd-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="91bbd-201">Druhý parametr je předpona identifikátoru URI pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="91bbd-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="91bbd-202">Zadaný tento kód, je identifikátor URI pro sadu entit produkty http://<em>hostname</em>  /odata/produktů.</span><span class="sxs-lookup"><span data-stu-id="91bbd-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="91bbd-203">Vaše aplikace může mít více než jeden koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="91bbd-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="91bbd-204">Pro každý koncový bod, volání <strong>MapODataRoute</strong> a zadejte název jedinečné cesty a jedinečný identifikátor URI předponu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="91bbd-205">Přidání dat do databáze (volitelné)</span><span class="sxs-lookup"><span data-stu-id="91bbd-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="91bbd-206">V tomto kroku použijete rozhraní Entity Framework pro přidání dat do databáze s daty testu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="91bbd-207">Tento krok je volitelný, ale umožňuje vám to hned otestovat na váš koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="91bbd-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="91bbd-208">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="91bbd-208">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="91bbd-209">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="91bbd-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="91bbd-210">Tento postup přidá složku s názvem migrace a s názvem Configuration.cs soubor kódu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="91bbd-211">Otevřete tento soubor a přidejte následující kód, který `Configuration.Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="91bbd-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="91bbd-212">V okně konzoly Správce balíčků zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="91bbd-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="91bbd-213">Tyto příkazy Generovat kód, který vytvoří databázi a potom tento kód spustí.</span><span class="sxs-lookup"><span data-stu-id="91bbd-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="91bbd-214">Zkoumání koncového bodu OData</span><span class="sxs-lookup"><span data-stu-id="91bbd-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="91bbd-215">V této části, použijeme [ladění Proxy webové aplikace Fiddler](http://www.fiddler2.com) posílat žádosti na koncový bod a zkontrolujte zprávy odpovědi.</span><span class="sxs-lookup"><span data-stu-id="91bbd-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="91bbd-216">To vám pomůže pochopit možnosti koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="91bbd-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="91bbd-217">V sadě Visual Studio stisknutím klávesy F5 spusťte ladění.</span><span class="sxs-lookup"><span data-stu-id="91bbd-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="91bbd-218">Ve výchozím nastavení, Visual Studio otevře prohlížeč na `http://localhost:*port*`, kde *port* je číslo portu, který je nakonfigurovaný v nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="91bbd-219">Můžete změnit číslo portu v nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="91bbd-220">V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="91bbd-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="91bbd-221">V okně Vlastnosti vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="91bbd-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="91bbd-222">Zadejte číslo portu v rámci **adresa Url projektu**.</span><span class="sxs-lookup"><span data-stu-id="91bbd-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="91bbd-223">Dokument služby</span><span class="sxs-lookup"><span data-stu-id="91bbd-223">Service Document</span></span>

<span data-ttu-id="91bbd-224">*Dokument služby* obsahuje seznam ze sady entit pro koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="91bbd-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="91bbd-225">Získat dokument služby, odeslat požadavek GET na kořenový identifikátor URI služby.</span><span class="sxs-lookup"><span data-stu-id="91bbd-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="91bbd-226">Použití aplikace Fiddler, zadejte následující identifikátor URI v **Composer** kartě: `http://localhost:port/odata/`, kde *port* je číslo portu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="91bbd-227">Klikněte na tlačítko **Execute** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="91bbd-227">Click the **Execute** button.</span></span> <span data-ttu-id="91bbd-228">Fiddler odesílá požadavek HTTP GET do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="91bbd-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="91bbd-229">Měli byste vidět odpovědi v seznamu webovými relacemi.</span><span class="sxs-lookup"><span data-stu-id="91bbd-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="91bbd-230">Pokud všechno funguje, bude se stavovým kódem 200.</span><span class="sxs-lookup"><span data-stu-id="91bbd-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="91bbd-231">Dvakrát klikněte na panel odpovědi v seznamu webových relací a zobrazit podrobnosti zprávy s odpovědí na kartě kontroly.</span><span class="sxs-lookup"><span data-stu-id="91bbd-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="91bbd-232">Nezpracované zprávy s odpovědí HTTP by měl vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="91bbd-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="91bbd-233">Ve výchozím nastavení webové rozhraní API vrátí dokument služby ve formátu AtomPub.</span><span class="sxs-lookup"><span data-stu-id="91bbd-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="91bbd-234">Požádat o JSON, přidejte následující záhlaví požadavku HTTP:</span><span class="sxs-lookup"><span data-stu-id="91bbd-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="91bbd-235">Odpověď HTTP, která nyní obsahuje datovou část JSON:</span><span class="sxs-lookup"><span data-stu-id="91bbd-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="91bbd-236">Dokument metadat služby</span><span class="sxs-lookup"><span data-stu-id="91bbd-236">Service Metadata Document</span></span>

<span data-ttu-id="91bbd-237">*Dokumentu metadat služby* popisuje datového modelu služby pomocí jazyka XML nazývaného Konceptuální schéma definici jazyka (CSDL).</span><span class="sxs-lookup"><span data-stu-id="91bbd-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="91bbd-238">Dokument metadat znázorňuje strukturu dat ve službě a slouží ke generování kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="91bbd-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="91bbd-239">K získání dokumentu metadata, odeslat požadavek GET na `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="91bbd-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="91bbd-240">Tady jsou metadata pro koncový bod je znázorněno v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="91bbd-241">Sada entit</span><span class="sxs-lookup"><span data-stu-id="91bbd-241">Entity Set</span></span>

<span data-ttu-id="91bbd-242">Pokud chcete získat sadu entit produkty, odeslat požadavek GET na `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="91bbd-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="91bbd-243">Entity</span><span class="sxs-lookup"><span data-stu-id="91bbd-243">Entity</span></span>

<span data-ttu-id="91bbd-244">Chcete-li získat jednotlivé produkty, odeslat požadavek GET na `http://localhost:port/odata/Products(1)`, kde "1" je ID produktu.</span><span class="sxs-lookup"><span data-stu-id="91bbd-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="91bbd-245">Formáty serializace OData</span><span class="sxs-lookup"><span data-stu-id="91bbd-245">OData Serialization Formats</span></span>

<span data-ttu-id="91bbd-246">OData podporuje několik formátů serializace:</span><span class="sxs-lookup"><span data-stu-id="91bbd-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="91bbd-247">Publikování a odběru Atom (XML)</span><span class="sxs-lookup"><span data-stu-id="91bbd-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="91bbd-248">JSON výraz "light" (představíme v OData v3)</span><span class="sxs-lookup"><span data-stu-id="91bbd-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="91bbd-249">JSON "verbose" (OData v2)</span><span class="sxs-lookup"><span data-stu-id="91bbd-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="91bbd-250">Ve výchozím nastavení používá formát "výraz light" AtomPubJSON webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="91bbd-250">By default, Web API uses AtomPubJSON "light" format.</span></span> 

<span data-ttu-id="91bbd-251">Získat AtomPub formát, nastavte hlavičku Accept "application/atom + xml".</span><span class="sxs-lookup"><span data-stu-id="91bbd-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="91bbd-252">Tady je text odpovědi příkladu:</span><span class="sxs-lookup"><span data-stu-id="91bbd-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="91bbd-253">Zobrazí se jednou z nevýhod zřejmé ve formátu Atom: je mnohem podrobnější než JSON light.</span><span class="sxs-lookup"><span data-stu-id="91bbd-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="91bbd-254">Nicméně pokud máte klienta, která analyzuje AtomPub upřednostňují klienta tento formát přes JSON.</span><span class="sxs-lookup"><span data-stu-id="91bbd-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="91bbd-255">Tady je JSON light verze stejné entity:</span><span class="sxs-lookup"><span data-stu-id="91bbd-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="91bbd-256">Formát JSON light byla zavedena ve verzi 3 protokolu OData.</span><span class="sxs-lookup"><span data-stu-id="91bbd-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="91bbd-257">Z důvodu zpětné kompatibility může klient požadovat starší "verbose" formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="91bbd-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="91bbd-258">Požádat o podrobné JSON, nastavte hlavičku Accept na `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="91bbd-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="91bbd-259">Tady je podrobné verze:</span><span class="sxs-lookup"><span data-stu-id="91bbd-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="91bbd-260">Tento formát přenáší další metadata v textu odpovědi, které lze přidat značné režijní náklady za celou relaci.</span><span class="sxs-lookup"><span data-stu-id="91bbd-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="91bbd-261">Kromě toho přidá určitou úroveň dereference obalením objektu ve vlastnosti s názvem "d".</span><span class="sxs-lookup"><span data-stu-id="91bbd-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="91bbd-262">Další kroky</span><span class="sxs-lookup"><span data-stu-id="91bbd-262">Next Steps</span></span>

- [<span data-ttu-id="91bbd-263">Přidání relace Entity</span><span class="sxs-lookup"><span data-stu-id="91bbd-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="91bbd-264">Přidání akcí OData</span><span class="sxs-lookup"><span data-stu-id="91bbd-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="91bbd-265">Volání služby OData z klienta .NET</span><span class="sxs-lookup"><span data-stu-id="91bbd-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
