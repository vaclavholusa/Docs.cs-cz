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
ms.openlocfilehash: 2e0d3b45fd51192d227d852dc2f05b45ca42944c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910912"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="57e45-104">Vytvoření koncového bodu OData v3 s webovým rozhraním API 2</span><span class="sxs-lookup"><span data-stu-id="57e45-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="57e45-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="57e45-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="57e45-106">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="57e45-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="57e45-107">[Open Data Protocol](http://www.odata.org/) (OData) je protokol data access pro web.</span><span class="sxs-lookup"><span data-stu-id="57e45-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="57e45-108">OData nabízí jednotné způsob, jak strukturovat data, zadávat dotazy na data a manipulovat s sady dat prostřednictvím operace CRUD (vytváření, čtení, aktualizace a odstranění).</span><span class="sxs-lookup"><span data-stu-id="57e45-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="57e45-109">OData podporuje formáty JSON a AtomPub (XML).</span><span class="sxs-lookup"><span data-stu-id="57e45-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="57e45-110">OData definuje také způsob, jak vystavit metadata o datech.</span><span class="sxs-lookup"><span data-stu-id="57e45-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="57e45-111">Klienti mohou používat metadata a zjistit informace o typu a vztahy pro datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="57e45-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
>
> <span data-ttu-id="57e45-112">Rozhraní ASP.NET Web API usnadňuje vytvoření koncového bodu OData pro datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="57e45-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="57e45-113">Můžete řídit přesně OData operace, které podporuje koncový bod.</span><span class="sxs-lookup"><span data-stu-id="57e45-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="57e45-114">Můžete hostovat víc koncových bodů protokolu OData, společně s koncovými body mimo prostředí OData.</span><span class="sxs-lookup"><span data-stu-id="57e45-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="57e45-115">Máte plnou kontrolu nad datový model, back-end obchodní logiku a data vrstev.</span><span class="sxs-lookup"><span data-stu-id="57e45-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="57e45-116">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="57e45-116">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="57e45-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="57e45-117">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="57e45-118">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="57e45-118">Web API 2</span></span>
> - <span data-ttu-id="57e45-119">OData verze 3</span><span class="sxs-lookup"><span data-stu-id="57e45-119">OData Version 3</span></span>
> - <span data-ttu-id="57e45-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="57e45-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="57e45-121">Fiddler webový ladicí proxy server (volitelné)</span><span class="sxs-lookup"><span data-stu-id="57e45-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
>
> <span data-ttu-id="57e45-122">Přidala se podpora web API OData v [technologie ASP.NET a Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="57e45-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="57e45-123">Tento kurz používá však generování uživatelského rozhraní, která byla přidána do sady Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="57e45-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="57e45-124">V tomto kurzu vytvoříte jednoduchou koncový bod OData, který můžou klienti dotazovat.</span><span class="sxs-lookup"><span data-stu-id="57e45-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="57e45-125">Pokud vytvoříte klienta jazyka C# pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="57e45-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="57e45-126">Po dokončení tohoto kurzu, další sadu kurzy ukazují, jak přidat další funkce, včetně vztahů entit, akce, a vyberte rozbalte $/ $.</span><span class="sxs-lookup"><span data-stu-id="57e45-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="57e45-127">Vytvoření projektu sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="57e45-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="57e45-128">Přidání modelu Entity</span><span class="sxs-lookup"><span data-stu-id="57e45-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="57e45-129">Přidat kontroler OData</span><span class="sxs-lookup"><span data-stu-id="57e45-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="57e45-130">Přidat EDM a trasy</span><span class="sxs-lookup"><span data-stu-id="57e45-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="57e45-131">Přidání dat do databáze (volitelné)</span><span class="sxs-lookup"><span data-stu-id="57e45-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="57e45-132">Zkoumání koncového bodu OData</span><span class="sxs-lookup"><span data-stu-id="57e45-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="57e45-133">Formáty serializace OData</span><span class="sxs-lookup"><span data-stu-id="57e45-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="57e45-134">Vytvoření projektu sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="57e45-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="57e45-135">V tomto kurzu vytvoříte koncový bod OData, který podporuje základní operace CRUD.</span><span class="sxs-lookup"><span data-stu-id="57e45-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="57e45-136">Koncový bod bude vystavovat jeden prostředek, seznam produktů.</span><span class="sxs-lookup"><span data-stu-id="57e45-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="57e45-137">Dalších kurzech přidáte další funkce.</span><span class="sxs-lookup"><span data-stu-id="57e45-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="57e45-138">Spusťte sadu Visual Studio a vyberte **nový projekt** z úvodní stránky.</span><span class="sxs-lookup"><span data-stu-id="57e45-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="57e45-139">Nebo z **souboru** nabídce vyberte možnost **nový** a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="57e45-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="57e45-140">V **šablony** vyberte **nainstalované šablony** a rozbalte uzel Visual C#.</span><span class="sxs-lookup"><span data-stu-id="57e45-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="57e45-141">V části **Visual C#** vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="57e45-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="57e45-142">Vyberte **webová aplikace ASP.NET** šablony.</span><span class="sxs-lookup"><span data-stu-id="57e45-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="57e45-143">V **nový projekt ASP.NET** dialogového okna, vyberte **prázdný** šablony.</span><span class="sxs-lookup"><span data-stu-id="57e45-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="57e45-144">V části &quot;přidat složky a základní odkazy pro... &quot;, zkontrolujte **webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="57e45-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="57e45-145">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="57e45-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="57e45-146">Přidání modelu Entity</span><span class="sxs-lookup"><span data-stu-id="57e45-146">Add an Entity Model</span></span>

<span data-ttu-id="57e45-147">A *modelu* je objekt, který představuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="57e45-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="57e45-148">Pro účely tohoto kurzu potřebujeme model, který představuje produkt.</span><span class="sxs-lookup"><span data-stu-id="57e45-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="57e45-149">Model odpovídá naše prostředí OData typu entity.</span><span class="sxs-lookup"><span data-stu-id="57e45-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="57e45-150">V Průzkumníku řešení klikněte pravým tlačítkem na složku modely.</span><span class="sxs-lookup"><span data-stu-id="57e45-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="57e45-151">V místní nabídce vyberte **přidat** vyberte **třídy**.</span><span class="sxs-lookup"><span data-stu-id="57e45-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="57e45-152">V **přidat nový** položky dialogového okna, název třídy &quot;produktu&quot;.</span><span class="sxs-lookup"><span data-stu-id="57e45-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="57e45-153">Podle konvence tříd modelu jsou umístěny ve složce modely.</span><span class="sxs-lookup"><span data-stu-id="57e45-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="57e45-154">Nemusíte si odpovídají této konvenci ve vašich vlastních projektů, ale použijeme ho pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="57e45-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="57e45-155">V souboru Product.cs přidejte následující definici třídy:</span><span class="sxs-lookup"><span data-stu-id="57e45-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="57e45-156">Vlastnost ID bude klíč entity.</span><span class="sxs-lookup"><span data-stu-id="57e45-156">The ID property will be the entity key.</span></span> <span data-ttu-id="57e45-157">Klienti mohou odesílat dotazy produkty podle ID.</span><span class="sxs-lookup"><span data-stu-id="57e45-157">Clients can query products by ID.</span></span> <span data-ttu-id="57e45-158">Toto pole by také být primární klíč v back-end databáze.</span><span class="sxs-lookup"><span data-stu-id="57e45-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="57e45-159">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="57e45-159">Build the project now.</span></span> <span data-ttu-id="57e45-160">V dalším kroku použijeme některé sady Visual Studio generování uživatelského rozhraní, který používá reflexi najít typ produktu.</span><span class="sxs-lookup"><span data-stu-id="57e45-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="57e45-161">Přidat kontroler OData</span><span class="sxs-lookup"><span data-stu-id="57e45-161">Add an OData Controller</span></span>

<span data-ttu-id="57e45-162">A *řadič* je třída, která zpracovává požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="57e45-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="57e45-163">Definujte samostatný kontrolerem pro každou sadu entit v můžete službu OData.</span><span class="sxs-lookup"><span data-stu-id="57e45-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="57e45-164">V tomto kurzu vytvoříme jediného kontroleru.</span><span class="sxs-lookup"><span data-stu-id="57e45-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="57e45-165">V Průzkumníku řešení klikněte pravým tlačítkem myši na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="57e45-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="57e45-166">Vyberte **přidat** a pak vyberte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="57e45-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="57e45-167">V **přidat vygenerované uživatelské rozhraní** dialogového okna, vyberte &quot;Kontroleru webového rozhraní API 2 OData s akcemi používající nástroj Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="57e45-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="57e45-168">V **přidat kontroler** dialogového okna, názvu kontroleru "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="57e45-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="57e45-169">Vyberte &quot;použít asynchronní akce kontroleru&quot; zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="57e45-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="57e45-170">V **modelu** rozevíracího seznamu vyberte třídu produktu.</span><span class="sxs-lookup"><span data-stu-id="57e45-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="57e45-171">Klikněte na tlačítko **nový kontext dat...**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="57e45-171">Click the **New data context...** button.</span></span> <span data-ttu-id="57e45-172">Ponechte výchozí název typu datového kontextu a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="57e45-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="57e45-173">V dialogovém okně Přidat kontroler, přidat kontroler, klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="57e45-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="57e45-174">Poznámka: Pokud se zobrazí chybová zpráva, která uvádí, že &quot;došlo k chybě při získávání typu... &quot;, ujistěte se, že jste sestavili projekt sady Visual Studio po přidání třídy produktu.</span><span class="sxs-lookup"><span data-stu-id="57e45-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="57e45-175">Základní kostry aplikace používá reflexi k nalezení třídy.</span><span class="sxs-lookup"><span data-stu-id="57e45-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="57e45-176">Základní kostry aplikace přidá do projektu dva soubory kódu:</span><span class="sxs-lookup"><span data-stu-id="57e45-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="57e45-177">Products.cs definuje kontroler Web API, která implementuje koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="57e45-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="57e45-178">ProductServiceContext.cs poskytuje metody pro dotazování databáze pomocí Entity Frameworku.</span><span class="sxs-lookup"><span data-stu-id="57e45-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="57e45-179">Přidat EDM a trasy</span><span class="sxs-lookup"><span data-stu-id="57e45-179">Add the EDM and Route</span></span>

<span data-ttu-id="57e45-180">V Průzkumníku řešení rozbalte aplikace\_spusťte složky a otevřete soubor s názvem WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="57e45-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="57e45-181">Tato třída obsahuje konfigurační kód pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="57e45-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="57e45-182">Nahraďte tento kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="57e45-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="57e45-183">Tento kód provede dvě věci:</span><span class="sxs-lookup"><span data-stu-id="57e45-183">This code does two things:</span></span>

- <span data-ttu-id="57e45-184">Vytvoří Entity Data Model (EDM) pro koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="57e45-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="57e45-185">Přidá trasu pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="57e45-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="57e45-186">EDM je abstraktní modelem data.</span><span class="sxs-lookup"><span data-stu-id="57e45-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="57e45-187">EDM slouží k vytvoření dokumentu metadata a definovat identifikátory URI pro službu.</span><span class="sxs-lookup"><span data-stu-id="57e45-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="57e45-188">**ODataConventionModelBuilder** vytvoří EDM tak, že používáte sadu výchozích konvencí pojmenování EDM.</span><span class="sxs-lookup"><span data-stu-id="57e45-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="57e45-189">Tento přístup vyžaduje nejméně kódu.</span><span class="sxs-lookup"><span data-stu-id="57e45-189">This approach requires the least code.</span></span> <span data-ttu-id="57e45-190">Pokud chcete mít větší kontrolu nad EDM, můžete použít **Tvůrce ODataModelBuilder** třídy za účelem vytvoření tak, že explicitně přidáte vlastností, klíčů a navigačních vlastností EDM.</span><span class="sxs-lookup"><span data-stu-id="57e45-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="57e45-191">**Objektu EntitySet** metoda přidá do modelu EDM sadu entit:</span><span class="sxs-lookup"><span data-stu-id="57e45-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="57e45-192">Řetězec "Produktů" definuje název sady entit.</span><span class="sxs-lookup"><span data-stu-id="57e45-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="57e45-193">Název kontroleru musí odpovídat názvu sady entit.</span><span class="sxs-lookup"><span data-stu-id="57e45-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="57e45-194">V tomto kurzu je název sady entit s názvem "Produktů" a má název kontroleru `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="57e45-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="57e45-195">Pokud pojmenujete "ProductSet" sada entit, bude název kontroleru `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="57e45-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="57e45-196">Všimněte si, že koncový bod může mít více sad entit.</span><span class="sxs-lookup"><span data-stu-id="57e45-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="57e45-197">Volání **objektu EntitySet&lt;T&gt;**  pro každou entitu nastavení a pak definovat odpovídající kontroler.</span><span class="sxs-lookup"><span data-stu-id="57e45-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="57e45-198">**MapODataRoute** metoda přidá trasu pro koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="57e45-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="57e45-199">První parametr je popisný název pro tuto trasu.</span><span class="sxs-lookup"><span data-stu-id="57e45-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="57e45-200">Klienti služby se tento název nezobrazuje.</span><span class="sxs-lookup"><span data-stu-id="57e45-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="57e45-201">Druhý parametr je předpona identifikátoru URI pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="57e45-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="57e45-202">Zadaný tento kód, je identifikátor URI pro sadu entit produkty http://<em>hostname</em>  /odata/produktů.</span><span class="sxs-lookup"><span data-stu-id="57e45-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="57e45-203">Vaše aplikace může mít více než jeden koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="57e45-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="57e45-204">Pro každý koncový bod, volání <strong>MapODataRoute</strong> a zadejte název jedinečné cesty a jedinečný identifikátor URI předponu.</span><span class="sxs-lookup"><span data-stu-id="57e45-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="57e45-205">Přidání dat do databáze (volitelné)</span><span class="sxs-lookup"><span data-stu-id="57e45-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="57e45-206">V tomto kroku použijete rozhraní Entity Framework pro přidání dat do databáze s daty testu.</span><span class="sxs-lookup"><span data-stu-id="57e45-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="57e45-207">Tento krok je volitelný, ale umožňuje vám to hned otestovat na váš koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="57e45-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="57e45-208">Z **nástroje** příkaz **Správce balíčků NuGet**, vyberte **konzoly Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="57e45-208">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="57e45-209">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="57e45-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="57e45-210">Tento postup přidá složku s názvem migrace a s názvem Configuration.cs soubor kódu.</span><span class="sxs-lookup"><span data-stu-id="57e45-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="57e45-211">Otevřete tento soubor a přidejte následující kód, který `Configuration.Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="57e45-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="57e45-212">V okně konzoly Správce balíčků zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="57e45-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="57e45-213">Tyto příkazy Generovat kód, který vytvoří databázi a potom tento kód spustí.</span><span class="sxs-lookup"><span data-stu-id="57e45-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="57e45-214">Zkoumání koncového bodu OData</span><span class="sxs-lookup"><span data-stu-id="57e45-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="57e45-215">V této části, použijeme [ladění Proxy webové aplikace Fiddler](http://www.fiddler2.com) posílat žádosti na koncový bod a zkontrolujte zprávy odpovědi.</span><span class="sxs-lookup"><span data-stu-id="57e45-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="57e45-216">To vám pomůže pochopit možnosti koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="57e45-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="57e45-217">V sadě Visual Studio stisknutím klávesy F5 spusťte ladění.</span><span class="sxs-lookup"><span data-stu-id="57e45-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="57e45-218">Ve výchozím nastavení, Visual Studio otevře prohlížeč na `http://localhost:*port*`, kde *port* je číslo portu, který je nakonfigurovaný v nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="57e45-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="57e45-219">Můžete změnit číslo portu v nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="57e45-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="57e45-220">V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="57e45-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="57e45-221">V okně Vlastnosti vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="57e45-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="57e45-222">Zadejte číslo portu v rámci **adresa Url projektu**.</span><span class="sxs-lookup"><span data-stu-id="57e45-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="57e45-223">Dokument služby</span><span class="sxs-lookup"><span data-stu-id="57e45-223">Service Document</span></span>

<span data-ttu-id="57e45-224">*Dokument služby* obsahuje seznam ze sady entit pro koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="57e45-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="57e45-225">Získat dokument služby, odeslat požadavek GET na kořenový identifikátor URI služby.</span><span class="sxs-lookup"><span data-stu-id="57e45-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="57e45-226">Použití aplikace Fiddler, zadejte následující identifikátor URI v **Composer** kartě: `http://localhost:port/odata/`, kde *port* je číslo portu.</span><span class="sxs-lookup"><span data-stu-id="57e45-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="57e45-227">Klikněte na tlačítko **Execute** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="57e45-227">Click the **Execute** button.</span></span> <span data-ttu-id="57e45-228">Fiddler odesílá požadavek HTTP GET do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="57e45-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="57e45-229">Měli byste vidět odpovědi v seznamu webovými relacemi.</span><span class="sxs-lookup"><span data-stu-id="57e45-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="57e45-230">Pokud všechno funguje, bude se stavovým kódem 200.</span><span class="sxs-lookup"><span data-stu-id="57e45-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="57e45-231">Dvakrát klikněte na panel odpovědi v seznamu webových relací a zobrazit podrobnosti zprávy s odpovědí na kartě kontroly.</span><span class="sxs-lookup"><span data-stu-id="57e45-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="57e45-232">Nezpracované zprávy s odpovědí HTTP by měl vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="57e45-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="57e45-233">Ve výchozím nastavení webové rozhraní API vrátí dokument služby ve formátu AtomPub.</span><span class="sxs-lookup"><span data-stu-id="57e45-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="57e45-234">Požádat o JSON, přidejte následující záhlaví požadavku HTTP:</span><span class="sxs-lookup"><span data-stu-id="57e45-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="57e45-235">Odpověď HTTP, která nyní obsahuje datovou část JSON:</span><span class="sxs-lookup"><span data-stu-id="57e45-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="57e45-236">Dokument metadat služby</span><span class="sxs-lookup"><span data-stu-id="57e45-236">Service Metadata Document</span></span>

<span data-ttu-id="57e45-237">*Dokumentu metadat služby* popisuje datového modelu služby pomocí jazyka XML nazývaného Konceptuální schéma definici jazyka (CSDL).</span><span class="sxs-lookup"><span data-stu-id="57e45-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="57e45-238">Dokument metadat znázorňuje strukturu dat ve službě a slouží ke generování kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="57e45-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="57e45-239">K získání dokumentu metadata, odeslat požadavek GET na `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="57e45-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="57e45-240">Tady jsou metadata pro koncový bod je znázorněno v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="57e45-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="57e45-241">Sada entit</span><span class="sxs-lookup"><span data-stu-id="57e45-241">Entity Set</span></span>

<span data-ttu-id="57e45-242">Pokud chcete získat sadu entit produkty, odeslat požadavek GET na `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="57e45-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="57e45-243">Entity</span><span class="sxs-lookup"><span data-stu-id="57e45-243">Entity</span></span>

<span data-ttu-id="57e45-244">Chcete-li získat jednotlivé produkty, odeslat požadavek GET na `http://localhost:port/odata/Products(1)`, kde "1" je ID produktu.</span><span class="sxs-lookup"><span data-stu-id="57e45-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="57e45-245">Formáty serializace OData</span><span class="sxs-lookup"><span data-stu-id="57e45-245">OData Serialization Formats</span></span>

<span data-ttu-id="57e45-246">OData podporuje několik formátů serializace:</span><span class="sxs-lookup"><span data-stu-id="57e45-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="57e45-247">Publikování a odběru Atom (XML)</span><span class="sxs-lookup"><span data-stu-id="57e45-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="57e45-248">JSON výraz "light" (představíme v OData v3)</span><span class="sxs-lookup"><span data-stu-id="57e45-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="57e45-249">JSON "verbose" (OData v2)</span><span class="sxs-lookup"><span data-stu-id="57e45-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="57e45-250">Ve výchozím nastavení používá formát "výraz light" AtomPubJSON webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="57e45-250">By default, Web API uses AtomPubJSON "light" format.</span></span>

<span data-ttu-id="57e45-251">Získat AtomPub formát, nastavte hlavičku Accept "application/atom + xml".</span><span class="sxs-lookup"><span data-stu-id="57e45-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="57e45-252">Tady je text odpovědi příkladu:</span><span class="sxs-lookup"><span data-stu-id="57e45-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="57e45-253">Zobrazí se jednou z nevýhod zřejmé ve formátu Atom: je mnohem podrobnější než JSON light.</span><span class="sxs-lookup"><span data-stu-id="57e45-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="57e45-254">Nicméně pokud máte klienta, která analyzuje AtomPub upřednostňují klienta tento formát přes JSON.</span><span class="sxs-lookup"><span data-stu-id="57e45-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="57e45-255">Tady je JSON light verze stejné entity:</span><span class="sxs-lookup"><span data-stu-id="57e45-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="57e45-256">Formát JSON light byla zavedena ve verzi 3 protokolu OData.</span><span class="sxs-lookup"><span data-stu-id="57e45-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="57e45-257">Z důvodu zpětné kompatibility může klient požadovat starší "verbose" formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="57e45-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="57e45-258">Požádat o podrobné JSON, nastavte hlavičku Accept na `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="57e45-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="57e45-259">Tady je podrobné verze:</span><span class="sxs-lookup"><span data-stu-id="57e45-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="57e45-260">Tento formát přenáší další metadata v textu odpovědi, které lze přidat značné režijní náklady za celou relaci.</span><span class="sxs-lookup"><span data-stu-id="57e45-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="57e45-261">Kromě toho přidá určitou úroveň dereference obalením objektu ve vlastnosti s názvem "d".</span><span class="sxs-lookup"><span data-stu-id="57e45-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="57e45-262">Další kroky</span><span class="sxs-lookup"><span data-stu-id="57e45-262">Next Steps</span></span>

- [<span data-ttu-id="57e45-263">Přidání relace Entity</span><span class="sxs-lookup"><span data-stu-id="57e45-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="57e45-264">Přidání akcí OData</span><span class="sxs-lookup"><span data-stu-id="57e45-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="57e45-265">Volání služby OData z klienta .NET</span><span class="sxs-lookup"><span data-stu-id="57e45-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
