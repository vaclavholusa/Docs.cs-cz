---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Povolení operace CRUD v ASP.NET Web API 1 | Microsoft Docs
author: MikeWasson
description: Tento kurz ukazuje, jak pro podporu operací CRUD v službě HTTP pomocí rozhraní ASP.NET Web API. Verze softwaru používány kurz Visual Studio 2012 Web Asie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "29153005"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="ac97e-104">Povolení operace CRUD v ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="ac97e-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="ac97e-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ac97e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ac97e-106">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="ac97e-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="ac97e-107">Tento kurz ukazuje, jak pro podporu operací CRUD v službě HTTP pomocí rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="ac97e-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ac97e-108">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="ac97e-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ac97e-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ac97e-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="ac97e-110">Webové rozhraní API 1 (taky funguje u webových rozhraní API 2)</span><span class="sxs-lookup"><span data-stu-id="ac97e-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="ac97e-111">Zastupuje CRUD &quot;vytvoření, čtení, aktualizaci a odstraňování,&quot; který jsou čtyři základní databázových operací.</span><span class="sxs-lookup"><span data-stu-id="ac97e-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="ac97e-112">Mnoho služeb HTTP také model operace CRUD prostřednictvím rozhraní API REST jako nebo REST.</span><span class="sxs-lookup"><span data-stu-id="ac97e-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="ac97e-113">V tomto kurzu vytvoříte velmi jednoduché webové rozhraní API pro správu seznam produktů.</span><span class="sxs-lookup"><span data-stu-id="ac97e-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="ac97e-114">Každý produkt bude obsahovat název, ceny a kategorie (například &quot;toys&quot; nebo &quot;hardwaru&quot;), plus ID produktu.</span><span class="sxs-lookup"><span data-stu-id="ac97e-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="ac97e-115">Následující metody zveřejní produkty rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ac97e-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="ac97e-116">Akce</span><span class="sxs-lookup"><span data-stu-id="ac97e-116">Action</span></span> | <span data-ttu-id="ac97e-117">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ac97e-117">HTTP method</span></span> | <span data-ttu-id="ac97e-118">Relativní identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="ac97e-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ac97e-119">Získání seznamu všech produktů</span><span class="sxs-lookup"><span data-stu-id="ac97e-119">Get a list of all products</span></span> | <span data-ttu-id="ac97e-120">GET</span><span class="sxs-lookup"><span data-stu-id="ac97e-120">GET</span></span> | <span data-ttu-id="ac97e-121">/ api/produkty</span><span class="sxs-lookup"><span data-stu-id="ac97e-121">/api/products</span></span> |
| <span data-ttu-id="ac97e-122">Získání ID produktu</span><span class="sxs-lookup"><span data-stu-id="ac97e-122">Get a product by ID</span></span> | <span data-ttu-id="ac97e-123">GET</span><span class="sxs-lookup"><span data-stu-id="ac97e-123">GET</span></span> | <span data-ttu-id="ac97e-124">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="ac97e-124">/api/products/*id*</span></span> |
| <span data-ttu-id="ac97e-125">Získání produktu podle kategorie</span><span class="sxs-lookup"><span data-stu-id="ac97e-125">Get a product by category</span></span> | <span data-ttu-id="ac97e-126">GET</span><span class="sxs-lookup"><span data-stu-id="ac97e-126">GET</span></span> | <span data-ttu-id="ac97e-127">produkty/api /? kategorie =*kategorie*</span><span class="sxs-lookup"><span data-stu-id="ac97e-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="ac97e-128">Vytvoření nového produktu</span><span class="sxs-lookup"><span data-stu-id="ac97e-128">Create a new product</span></span> | <span data-ttu-id="ac97e-129">POST</span><span class="sxs-lookup"><span data-stu-id="ac97e-129">POST</span></span> | <span data-ttu-id="ac97e-130">/ api/produkty</span><span class="sxs-lookup"><span data-stu-id="ac97e-130">/api/products</span></span> |
| <span data-ttu-id="ac97e-131">Aktualizace produktu</span><span class="sxs-lookup"><span data-stu-id="ac97e-131">Update a product</span></span> | <span data-ttu-id="ac97e-132">PUT</span><span class="sxs-lookup"><span data-stu-id="ac97e-132">PUT</span></span> | <span data-ttu-id="ac97e-133">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="ac97e-133">/api/products/*id*</span></span> |
| <span data-ttu-id="ac97e-134">Odstranit produktu</span><span class="sxs-lookup"><span data-stu-id="ac97e-134">Delete a product</span></span> | <span data-ttu-id="ac97e-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="ac97e-135">DELETE</span></span> | <span data-ttu-id="ac97e-136">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="ac97e-136">/api/products/*id*</span></span> |

<span data-ttu-id="ac97e-137">Všimněte si, že některé identifikátory URI v cestě zahrnují ID produktu.</span><span class="sxs-lookup"><span data-stu-id="ac97e-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="ac97e-138">Chcete-li získat produktu, jejíž ID je 28, klient odešle požadavek GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="ac97e-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="ac97e-139">Prostředky</span><span class="sxs-lookup"><span data-stu-id="ac97e-139">Resources</span></span>

<span data-ttu-id="ac97e-140">Produkty API definuje identifikátory URI pro dva typy prostředků:</span><span class="sxs-lookup"><span data-stu-id="ac97e-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="ac97e-141">Prostředek</span><span class="sxs-lookup"><span data-stu-id="ac97e-141">Resource</span></span> | <span data-ttu-id="ac97e-142">Identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="ac97e-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="ac97e-143">Seznam všech produktů.</span><span class="sxs-lookup"><span data-stu-id="ac97e-143">The list of all the products.</span></span> | <span data-ttu-id="ac97e-144">/ api/produkty</span><span class="sxs-lookup"><span data-stu-id="ac97e-144">/api/products</span></span> |
| <span data-ttu-id="ac97e-145">Jednotlivé produkty.</span><span class="sxs-lookup"><span data-stu-id="ac97e-145">An individual product.</span></span> | <span data-ttu-id="ac97e-146">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="ac97e-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="ac97e-147">Metody</span><span class="sxs-lookup"><span data-stu-id="ac97e-147">Methods</span></span>

<span data-ttu-id="ac97e-148">Čtyři hlavní metody HTTP (GET, PUT, POST a DELETE) lze mapovat na operace CRUD následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ac97e-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="ac97e-149">GET načte reprezentace prostředek na zadaný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="ac97e-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="ac97e-150">GET by měl mít žádné vedlejší účinky na serveru.</span><span class="sxs-lookup"><span data-stu-id="ac97e-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="ac97e-151">PUT aktualizuje prostředek na zadaný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="ac97e-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="ac97e-152">PUT také slouží k vytvoření nového prostředku na zadaný identifikátor URI, pokud server umožňuje klientům zadejte nové identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="ac97e-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="ac97e-153">V tomto kurzu nebudou podporovat rozhraní API vytváření PUT.</span><span class="sxs-lookup"><span data-stu-id="ac97e-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="ac97e-154">POST vytvoří nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="ac97e-154">POST creates a new resource.</span></span> <span data-ttu-id="ac97e-155">Server přiřadí identifikátor URI pro nový objekt a vrátí tento identifikátor URI jako součást zprávu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ac97e-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="ac97e-156">ODSTRANĚNÍ odstraní prostředek na zadaný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="ac97e-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="ac97e-157">Poznámka: Metodu PUT nahradí celý produkt entity.</span><span class="sxs-lookup"><span data-stu-id="ac97e-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="ac97e-158">To znamená klient se očekává odeslat znázornění dokončení aktualizované produktu.</span><span class="sxs-lookup"><span data-stu-id="ac97e-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="ac97e-159">Pokud chcete, aby podporovaly částečné aktualizace, je upřednostňovanou metodu PATCH.</span><span class="sxs-lookup"><span data-stu-id="ac97e-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="ac97e-160">Tento kurz neimplementuje opravy.</span><span class="sxs-lookup"><span data-stu-id="ac97e-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="ac97e-161">Vytvořte nový projekt webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ac97e-161">Create a New Web API Project</span></span>

<span data-ttu-id="ac97e-162">Začněte tím, že spuštění sady Visual Studio a vyberte **nový projekt** z **spustit** stránky.</span><span class="sxs-lookup"><span data-stu-id="ac97e-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="ac97e-163">Nebo z **soubor** nabídce vyberte možnost **nový** a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="ac97e-164">V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu.</span><span class="sxs-lookup"><span data-stu-id="ac97e-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="ac97e-165">V části **Visual C#**, vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="ac97e-166">V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="ac97e-167">Název projektu &quot;ProductStore&quot; a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="ac97e-168">V **nový ASP.NET MVC 4 projekt** dialogovém okně, vyberte **webového rozhraní API** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="ac97e-169">Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="ac97e-169">Adding a Model</span></span>

<span data-ttu-id="ac97e-170">A *modelu* je objekt, který představuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ac97e-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="ac97e-171">V rozhraní ASP.NET Web API objektů se silným typem CLR můžete použít jako modely a budou se automaticky serializovat na XML nebo JSON pro klienta.</span><span class="sxs-lookup"><span data-stu-id="ac97e-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="ac97e-172">Pro rozhraní API ProductStore naše data se skládá z produktů, takže vytvoříme novou třídu s názvem `Product`.</span><span class="sxs-lookup"><span data-stu-id="ac97e-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="ac97e-173">Pokud Průzkumníku již není viditelný, klikněte na tlačítko **zobrazení** nabídku a vyberte **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="ac97e-174">V Průzkumníku řešení klikněte pravým tlačítkem myši **modely** složky.</span><span class="sxs-lookup"><span data-stu-id="ac97e-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="ac97e-175">Z kontextu měny, vyberte **přidat**, pak vyberte **třída**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="ac97e-176">Název třídy &quot;produktu&quot;.</span><span class="sxs-lookup"><span data-stu-id="ac97e-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="ac97e-177">Přidejte následující vlastnosti pro `Product` třídy.</span><span class="sxs-lookup"><span data-stu-id="ac97e-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="ac97e-178">Přidání úložiště</span><span class="sxs-lookup"><span data-stu-id="ac97e-178">Adding a Repository</span></span>

<span data-ttu-id="ac97e-179">Musíme uložení kolekce produkty.</span><span class="sxs-lookup"><span data-stu-id="ac97e-179">We need to store a collection of products.</span></span> <span data-ttu-id="ac97e-180">Je vhodné oddělení kolekci z naše implementace služby.</span><span class="sxs-lookup"><span data-stu-id="ac97e-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="ac97e-181">Tímto způsobem jsme Změna úložiště zálohování bez přepisování třídu služby.</span><span class="sxs-lookup"><span data-stu-id="ac97e-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="ac97e-182">Tento typ návrhu se nazývá *úložiště* vzor.</span><span class="sxs-lookup"><span data-stu-id="ac97e-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="ac97e-183">Začněte definováním generické rozhraní pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="ac97e-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="ac97e-184">V Průzkumníku řešení klikněte pravým tlačítkem myši **modely** složky.</span><span class="sxs-lookup"><span data-stu-id="ac97e-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="ac97e-185">Vyberte **přidat**, pak vyberte **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="ac97e-186">V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte uzel C#.</span><span class="sxs-lookup"><span data-stu-id="ac97e-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="ac97e-187">V části C#, vyberte **kód**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-187">Under C#, select **Code**.</span></span> <span data-ttu-id="ac97e-188">V seznamu šablon kód, vyberte **rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="ac97e-189">Název rozhraní &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="ac97e-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="ac97e-190">Přidejte následující implementace:</span><span class="sxs-lookup"><span data-stu-id="ac97e-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="ac97e-191">Teď přidejte jinou třídu do složku modely, s názvem &quot;ProductRepository.&quot; Tato třída se implementovat `IProductRespository` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ac97e-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="ac97e-192">Přidejte následující implementace:</span><span class="sxs-lookup"><span data-stu-id="ac97e-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="ac97e-193">Úložiště zajišťuje seznamu místní paměti.</span><span class="sxs-lookup"><span data-stu-id="ac97e-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="ac97e-194">Tento kurz je v pořádku, ale v reálné aplikaci byste ukládat data externě, buď databází nebo do cloudového úložiště.</span><span class="sxs-lookup"><span data-stu-id="ac97e-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="ac97e-195">Vzor úložiště bude usnadňují implementace později změnit.</span><span class="sxs-lookup"><span data-stu-id="ac97e-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="ac97e-196">Přidání Kontroleru webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ac97e-196">Adding a Web API Controller</span></span>

<span data-ttu-id="ac97e-197">Pokud jste už pracovali s architekturou ASP.NET MVC, pak jste již obeznámeni s řadiči.</span><span class="sxs-lookup"><span data-stu-id="ac97e-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="ac97e-198">V rozhraní ASP.NET Web API *řadič* je třída, která zpracovává požadavky HTTP z klienta.</span><span class="sxs-lookup"><span data-stu-id="ac97e-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="ac97e-199">Průvodce nový projekt pro můžete vytvořit dva řadiče, vytvoření projektu.</span><span class="sxs-lookup"><span data-stu-id="ac97e-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="ac97e-200">Pokud chcete zobrazit, je, rozbalte složku řadiče v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="ac97e-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="ac97e-201">HomeController je řadič tradiční ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ac97e-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="ac97e-202">Zodpovídá za obsluhující stránky HTML pro lokalitu a přímo nesouvisí se naše webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ac97e-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="ac97e-203">ValuesController je řadič WebAPI příklad.</span><span class="sxs-lookup"><span data-stu-id="ac97e-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="ac97e-204">Pokračujte a odstranit ValuesController, kliknutím pravým tlačítkem myši na soubor v Průzkumníku řešení a výběrem **odstranit.**</span><span class="sxs-lookup"><span data-stu-id="ac97e-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="ac97e-205">Nyní přidejte nový řadič, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ac97e-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="ac97e-206">V **Průzkumníku**, klikněte pravým tlačítkem na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="ac97e-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="ac97e-207">Vyberte **přidat** a pak vyberte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="ac97e-208">V **přidat kontroler** průvodce, názvu kontroleru &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="ac97e-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="ac97e-209">V **šablony** rozevíracího seznamu vyberte **prázdný kontroler API**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="ac97e-210">Pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="ac97e-211">Není nutné uvést vaše contollers do složky s názvem řadiče.</span><span class="sxs-lookup"><span data-stu-id="ac97e-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="ac97e-212">Název složky není důležité. je jenom pohodlný způsob, jak uspořádat zdrojové soubory.</span><span class="sxs-lookup"><span data-stu-id="ac97e-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="ac97e-213">**Přidat kontroler** Průvodce vytvoří soubor s názvem ProductsController.cs ve složce řadiče.</span><span class="sxs-lookup"><span data-stu-id="ac97e-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="ac97e-214">Pokud tento soubor už není otevřený, poklikejte na soubor otevřete.</span><span class="sxs-lookup"><span data-stu-id="ac97e-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="ac97e-215">Přidejte následující **pomocí** příkaz:</span><span class="sxs-lookup"><span data-stu-id="ac97e-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="ac97e-216">Přidat pole, které obsahuje **IProductRepository** instance.</span><span class="sxs-lookup"><span data-stu-id="ac97e-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="ac97e-217">Volání metody `new ProductRepository()` v kontroleru není optimální, protože se sváže řadičem pro konkrétní implementaci `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="ac97e-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="ac97e-218">Lepší způsob, najdete v článku [pomocí překladače závislostí webové rozhraní API](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ac97e-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="ac97e-219">Získávání prostředku</span><span class="sxs-lookup"><span data-stu-id="ac97e-219">Getting a Resource</span></span>

<span data-ttu-id="ac97e-220">Rozhraní API ProductStore zveřejní několik &quot;číst&quot; akce jako metody GET protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="ac97e-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="ac97e-221">Každá akce bude odpovídat metoda v `ProductsController` třídy.</span><span class="sxs-lookup"><span data-stu-id="ac97e-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="ac97e-222">Akce</span><span class="sxs-lookup"><span data-stu-id="ac97e-222">Action</span></span> | <span data-ttu-id="ac97e-223">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ac97e-223">HTTP method</span></span> | <span data-ttu-id="ac97e-224">Relativní identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="ac97e-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ac97e-225">Získání seznamu všech produktů</span><span class="sxs-lookup"><span data-stu-id="ac97e-225">Get a list of all products</span></span> | <span data-ttu-id="ac97e-226">GET</span><span class="sxs-lookup"><span data-stu-id="ac97e-226">GET</span></span> | <span data-ttu-id="ac97e-227">/ api/produkty</span><span class="sxs-lookup"><span data-stu-id="ac97e-227">/api/products</span></span> |
| <span data-ttu-id="ac97e-228">Získání ID produktu</span><span class="sxs-lookup"><span data-stu-id="ac97e-228">Get a product by ID</span></span> | <span data-ttu-id="ac97e-229">GET</span><span class="sxs-lookup"><span data-stu-id="ac97e-229">GET</span></span> | <span data-ttu-id="ac97e-230">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="ac97e-230">/api/products/*id*</span></span> |
| <span data-ttu-id="ac97e-231">Získání produktu podle kategorie</span><span class="sxs-lookup"><span data-stu-id="ac97e-231">Get a product by category</span></span> | <span data-ttu-id="ac97e-232">GET</span><span class="sxs-lookup"><span data-stu-id="ac97e-232">GET</span></span> | <span data-ttu-id="ac97e-233">produkty/api /? kategorie =*kategorie*</span><span class="sxs-lookup"><span data-stu-id="ac97e-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="ac97e-234">Chcete-li získat seznam všech produktů, přidejte tuto metodu za účelem `ProductsController` třídy:</span><span class="sxs-lookup"><span data-stu-id="ac97e-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="ac97e-235">Název metody začíná &quot;získat&quot;, takže podle konvence mapy pro požadavky GET.</span><span class="sxs-lookup"><span data-stu-id="ac97e-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="ac97e-236">Navíc vzhledem k tomu, že metoda nemá žádné parametry, mapuje k identifikátoru URI, který neobsahuje *&quot;id&quot;* segmentu v cestě.</span><span class="sxs-lookup"><span data-stu-id="ac97e-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="ac97e-237">Chcete-li získat produktu podle ID, přidejte tuto metodu za účelem `ProductsController` třídy:</span><span class="sxs-lookup"><span data-stu-id="ac97e-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="ac97e-238">Tento název metody také začíná &quot;získat&quot;, ale metoda má parametr s názvem *id*. Tento parametr je namapována na &quot;id&quot; segment cesty identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="ac97e-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="ac97e-239">ID rozhraní ASP.NET Web API automaticky převede na správného datového typu (**int**) pro parametr.</span><span class="sxs-lookup"><span data-stu-id="ac97e-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="ac97e-240">Metoda GetProduct výjimku typu **HttpResponseException** Pokud *id* není platný.</span><span class="sxs-lookup"><span data-stu-id="ac97e-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="ac97e-241">Tato výjimka se přeložit rámcem na chybu 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="ac97e-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="ac97e-242">Nakonec přidejte metody k vyhledání produkty podle kategorie:</span><span class="sxs-lookup"><span data-stu-id="ac97e-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="ac97e-243">Pokud řetězec dotazu identifikátoru URI požadavku, webového rozhraní API se pokusí vyhledat parametry dotazu na parametry na metodu řadiče.</span><span class="sxs-lookup"><span data-stu-id="ac97e-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="ac97e-244">Proto identifikátoru URI ve tvaru "rozhraní api nebo produkty? kategorie =*kategorie*" bude mapovat tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="ac97e-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="ac97e-245">Vytvoření prostředku</span><span class="sxs-lookup"><span data-stu-id="ac97e-245">Creating a Resource</span></span>

<span data-ttu-id="ac97e-246">Potom přidáme metodu pro `ProductsController` třídy za účelem vytvoření nového produktu.</span><span class="sxs-lookup"><span data-stu-id="ac97e-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="ac97e-247">Zde je jednoduchá implementace metody:</span><span class="sxs-lookup"><span data-stu-id="ac97e-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="ac97e-248">Všimněte si o této metodě dvě věci:</span><span class="sxs-lookup"><span data-stu-id="ac97e-248">Note two things about this method:</span></span>

- <span data-ttu-id="ac97e-249">Název metody začíná &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="ac97e-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="ac97e-250">Pokud chcete vytvořit nového produktu, klient odešle požadavek HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="ac97e-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="ac97e-251">Tato metoda přebírá parametr typu produktu.</span><span class="sxs-lookup"><span data-stu-id="ac97e-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="ac97e-252">V rozhraní Web API jsou v těle žádosti deserializovat parametry s komplexními typy.</span><span class="sxs-lookup"><span data-stu-id="ac97e-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="ac97e-253">Proto Očekáváme, že klientovi umožní odeslat serializovaná reprezentace objekt produktu ve formátu XML nebo JSON.</span><span class="sxs-lookup"><span data-stu-id="ac97e-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="ac97e-254">Tato implementace budou fungovat, ale není poměrně dokončení.</span><span class="sxs-lookup"><span data-stu-id="ac97e-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="ac97e-255">V ideálním případě by rádi bychom znali odpověď HTTP na zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="ac97e-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="ac97e-256">**Kód odpovědi:** ve výchozím nastavení, rozhraní Web API nastaví stavový kód odpovědi 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="ac97e-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="ac97e-257">Ale podle protokol HTTP/1.1, když požadavek POST má za následek vytvoření prostředku, server by měl odpovědět stavem 201 (vytvořeno).</span><span class="sxs-lookup"><span data-stu-id="ac97e-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="ac97e-258">**Umístění:** serveru vytvoří prostředek, měl by obsahovat identifikátor URI nového prostředku v hlavičce umístění odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ac97e-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="ac97e-259">Rozhraní ASP.NET Web API usnadňuje zpracování zprávy s odpovědí HTTP.</span><span class="sxs-lookup"><span data-stu-id="ac97e-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="ac97e-260">Tady je lepší implementace:</span><span class="sxs-lookup"><span data-stu-id="ac97e-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="ac97e-261">Všimněte si, že je návratový typ metody nyní **objekt HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="ac97e-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="ac97e-262">Vrácením **objekt HttpResponseMessage** místo produktu, jsme můžete řídit podrobnosti zprávy s odpovědí HTTP, včetně stavový kód a hlavičku Location.</span><span class="sxs-lookup"><span data-stu-id="ac97e-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="ac97e-263">**CreateResponse** metoda vytvoří **objekt HttpResponseMessage** a automaticky zapisuje serializovaná reprezentace produktu objektu do datové části fo zprávu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ac97e-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="ac97e-264">Nelze ověřit v tomto příkladu `Product`.</span><span class="sxs-lookup"><span data-stu-id="ac97e-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="ac97e-265">Informace o ověření modelu naleznete v tématu [ověření modelu v rozhraní ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ac97e-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="ac97e-266">Aktualizuje prostředek</span><span class="sxs-lookup"><span data-stu-id="ac97e-266">Updating a Resource</span></span>

<span data-ttu-id="ac97e-267">Aktualizace produktu pomocí PUT je jednoduchý:</span><span class="sxs-lookup"><span data-stu-id="ac97e-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="ac97e-268">Název metody začíná &quot;Put... &quot;, takže ho webového rozhraní API odpovídá na požadavky PUT.</span><span class="sxs-lookup"><span data-stu-id="ac97e-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="ac97e-269">Tato metoda přebírá dva parametry, ID produktu a aktualizované produktu.</span><span class="sxs-lookup"><span data-stu-id="ac97e-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="ac97e-270">*Id* parametru jsou převzaty z cesta k identifikátoru URI a *produktu* v textu požadavku se deserializovat parametr.</span><span class="sxs-lookup"><span data-stu-id="ac97e-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="ac97e-271">Ve výchozím nastavení trvá rozhraní ASP.NET Web API jednoduché typy parametrů na základě trasy a komplexní typy z textu žádosti.</span><span class="sxs-lookup"><span data-stu-id="ac97e-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="ac97e-272">Odstranění prostředku</span><span class="sxs-lookup"><span data-stu-id="ac97e-272">Deleting a Resource</span></span>

<span data-ttu-id="ac97e-273">Chcete-li odstranit resourse, definujte metodu "Odstranit...".</span><span class="sxs-lookup"><span data-stu-id="ac97e-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="ac97e-274">Pokud žádost o odstranění úspěšné, se může vrátit stav 200 (OK) s obsah entity popisující stav; Stav 202 (platných), pokud je stále odstranění čekající; stav nebo 204 (ne obsahu) s žádné obsahu entity.</span><span class="sxs-lookup"><span data-stu-id="ac97e-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="ac97e-275">V takovém případě `DeleteProduct` metoda má `void` návratový typ, tak rozhraní ASP.NET Web API automaticky změní na stav to kód 204 (ne obsahu).</span><span class="sxs-lookup"><span data-stu-id="ac97e-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
