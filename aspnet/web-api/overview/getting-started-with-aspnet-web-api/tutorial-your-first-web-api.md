---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Začínáme s rozhraním ASP.NET Web API 2 (C#)
author: MikeWasson
description: HTTP není právě pro poskytovat webové stránky. Je také efektivní platformu pro sestavování rozhraní API, která vystavit služby a data. HTTP je jednoduchý, flexibilní a ubiq...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: d881563cdb6449aada444ef0528061581113a925
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
ms.locfileid: "29905033"
---
<a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="ff185-105">Začínáme s rozhraním ASP.NET Web API 2 (C#)</span><span class="sxs-lookup"><span data-stu-id="ff185-105">Get Started with ASP.NET Web API 2 (C#)</span></span>
====================
<span data-ttu-id="ff185-106">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ff185-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ff185-107">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="ff185-107">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="ff185-108">HTTP není právě pro poskytovat webové stránky.</span><span class="sxs-lookup"><span data-stu-id="ff185-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="ff185-109">HTTP je také efektivní platformu pro sestavování rozhraní API, která vystavit služby a data.</span><span class="sxs-lookup"><span data-stu-id="ff185-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="ff185-110">HTTP je jednoduchý, flexibilní a všudypřítomný.</span><span class="sxs-lookup"><span data-stu-id="ff185-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="ff185-111">Téměř jakékoli platformě, která si můžete představit má knihovna protokolu HTTP, takže služeb HTTP může být využity širokou škálou klientů včetně prohlížečů a mobilních zařízení, tradiční aplikací klasické pracovní plochy.</span><span class="sxs-lookup"><span data-stu-id="ff185-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>
 
<span data-ttu-id="ff185-112">Rozhraní ASP.NET Web API je architektura pro vytváření webových rozhraní API v rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ff185-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> <span data-ttu-id="ff185-113">V tomto kurzu použije k vytvoření webové rozhraní API, který vrátí seznam produktů ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="ff185-113">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

 
 ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ff185-114">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="ff185-114">Software versions used in the tutorial</span></span>
  
 - [<span data-ttu-id="ff185-115">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="ff185-115">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)
 - <span data-ttu-id="ff185-116">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="ff185-116">Web API 2</span></span>

<span data-ttu-id="ff185-117">V tématu [vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) pro novější verze tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ff185-117">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="ff185-118">Vytvoření projektu webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ff185-118">Create a Web API Project</span></span>

<span data-ttu-id="ff185-119">V tomto kurzu použije k vytvoření webové rozhraní API, který vrátí seznam produktů ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="ff185-119">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="ff185-120">Front-endu webové stránky používá jQuery pro zobrazení výsledků.</span><span class="sxs-lookup"><span data-stu-id="ff185-120">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="ff185-121">Spuštění sady Visual Studio a vyberte **nový projekt** z **spustit** stránky.</span><span class="sxs-lookup"><span data-stu-id="ff185-121">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="ff185-122">Nebo z **soubor** nabídce vyberte možnost **nový** a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="ff185-122">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="ff185-123">V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu.</span><span class="sxs-lookup"><span data-stu-id="ff185-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="ff185-124">V části **Visual C#**, vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="ff185-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="ff185-125">V seznamu šablon projektu, vyberte **webové aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="ff185-125">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="ff185-126">Název projektu "ProductsApp" a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff185-126">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="ff185-127">V **nový projekt ASP.NET** dialogovém okně, vyberte **prázdný** šablony.</span><span class="sxs-lookup"><span data-stu-id="ff185-127">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="ff185-128">V části &quot;přidat složky a odkazy na základní&quot;, zkontrolujte **webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="ff185-128">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="ff185-129">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff185-129">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="ff185-130">Můžete také vytvořit projekt webového rozhraní API pomocí &quot;webového rozhraní API&quot; šablony.</span><span class="sxs-lookup"><span data-stu-id="ff185-130">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="ff185-131">Šablona webového rozhraní API používá rozhraní ASP.NET MVC zajistit stránky nápovědy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ff185-131">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="ff185-132">Používám prázdné šablonu pro účely tohoto kurzu, protože chcete zobrazit webového rozhraní API bez MVC.</span><span class="sxs-lookup"><span data-stu-id="ff185-132">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="ff185-133">Obecně platí nemusíte vědět, ASP.NET MVC používat webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ff185-133">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="ff185-134">Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="ff185-134">Adding a Model</span></span>

<span data-ttu-id="ff185-135">A *modelu* je objekt, který představuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ff185-135">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="ff185-136">Rozhraní ASP.NET Web API můžete automaticky serializovat modelu JSON, XML nebo jiném formátu a zapište serializovaná data do datové části zprávy s odpovědí HTTP.</span><span class="sxs-lookup"><span data-stu-id="ff185-136">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="ff185-137">Tak dlouho, dokud klient může číst formát serializace, může deserializovat objekt.</span><span class="sxs-lookup"><span data-stu-id="ff185-137">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="ff185-138">Většina klientů můžete analyzovat, XML nebo JSON.</span><span class="sxs-lookup"><span data-stu-id="ff185-138">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="ff185-139">Kromě toho klienta můžete určit formát, který se chce nastavením hlavička Accept ve zprávě požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="ff185-139">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="ff185-140">Začněme vytvořením jednoduchého modelu, který představuje produkt.</span><span class="sxs-lookup"><span data-stu-id="ff185-140">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="ff185-141">Pokud Průzkumníku již není viditelný, klikněte na tlačítko **zobrazení** nabídku a vyberte **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="ff185-141">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="ff185-142">V Průzkumníku řešení klikněte pravým tlačítkem na složku modely.</span><span class="sxs-lookup"><span data-stu-id="ff185-142">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="ff185-143">V místní nabídce vyberte **přidat** vyberte **třída**.</span><span class="sxs-lookup"><span data-stu-id="ff185-143">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="ff185-144">Název třídy &quot;produktu&quot;.</span><span class="sxs-lookup"><span data-stu-id="ff185-144">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="ff185-145">Přidejte následující vlastnosti pro `Product` třídy.</span><span class="sxs-lookup"><span data-stu-id="ff185-145">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="ff185-146">Přidání Kontroleru</span><span class="sxs-lookup"><span data-stu-id="ff185-146">Adding a Controller</span></span>

<span data-ttu-id="ff185-147">V rozhraní Web API *řadič* je objekt, který zpracovává požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="ff185-147">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="ff185-148">Přidáme kontroler, který může vrátit seznam produktů nebo jednoho produktu určeného ID.</span><span class="sxs-lookup"><span data-stu-id="ff185-148">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="ff185-149">Pokud jste použili ASP.NET MVC, jste již obeznámeni s řadiči.</span><span class="sxs-lookup"><span data-stu-id="ff185-149">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="ff185-150">Webové rozhraní API řadiče jsou podobná řadiče MVC, ale dědění **objektu ApiController** místo **řadič** – třída.</span><span class="sxs-lookup"><span data-stu-id="ff185-150">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="ff185-151">V **Průzkumníku**, klikněte pravým tlačítkem na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="ff185-151">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="ff185-152">Vyberte **přidat** a pak vyberte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="ff185-152">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="ff185-153">V **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler Web API – prázdný**.</span><span class="sxs-lookup"><span data-stu-id="ff185-153">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="ff185-154">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ff185-154">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="ff185-155">V **přidat kontroler** dialogové okno, názvu kontroleru &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="ff185-155">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="ff185-156">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ff185-156">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="ff185-157">Soubor s názvem ProductsController.cs ve složce řadiče se vytvoří generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ff185-157">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="ff185-158">Není nutné uvést řadičů do složky s názvem řadiče.</span><span class="sxs-lookup"><span data-stu-id="ff185-158">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="ff185-159">Název složky je jenom pohodlný způsob pro uspořádání zdrojové soubory.</span><span class="sxs-lookup"><span data-stu-id="ff185-159">The folder name is just a convenient way to organize your source files.</span></span>


<span data-ttu-id="ff185-160">Pokud tento soubor už není otevřený, poklikejte na soubor otevřete.</span><span class="sxs-lookup"><span data-stu-id="ff185-160">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="ff185-161">Nahraďte kód v tomto souboru s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="ff185-161">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="ff185-162">Pro zjednodušení příkladu produkty ukládají na pevné pole uvnitř třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="ff185-162">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="ff185-163">V reálné aplikaci by samozřejmě dotazu na databázi nebo použijte jiný zdroj externích dat.</span><span class="sxs-lookup"><span data-stu-id="ff185-163">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="ff185-164">Řadičem definuje dvě metody, které vracejí produkty:</span><span class="sxs-lookup"><span data-stu-id="ff185-164">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="ff185-165">`GetAllProducts` Metoda vrátí celý seznam produktů jako **rozhraní IEnumerable&lt;produktu&gt;**  typu.</span><span class="sxs-lookup"><span data-stu-id="ff185-165">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="ff185-166">`GetProduct` Metoda vyhledá jednoho produktu pomocí jeho ID.</span><span class="sxs-lookup"><span data-stu-id="ff185-166">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="ff185-167">Je to!</span><span class="sxs-lookup"><span data-stu-id="ff185-167">That's it!</span></span> <span data-ttu-id="ff185-168">Máte pracovní webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ff185-168">You have a working web API.</span></span> <span data-ttu-id="ff185-169">Každá metoda v řadiči odpovídá jeden nebo více identifikátory URI:</span><span class="sxs-lookup"><span data-stu-id="ff185-169">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="ff185-170">Metoda kontroleru</span><span class="sxs-lookup"><span data-stu-id="ff185-170">Controller Method</span></span> | <span data-ttu-id="ff185-171">Identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="ff185-171">URI</span></span> |
| --- | --- |
| <span data-ttu-id="ff185-172">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="ff185-172">GetAllProducts</span></span> | <span data-ttu-id="ff185-173">/ api/produkty</span><span class="sxs-lookup"><span data-stu-id="ff185-173">/api/products</span></span> |
| <span data-ttu-id="ff185-174">GetProduct</span><span class="sxs-lookup"><span data-stu-id="ff185-174">GetProduct</span></span> | <span data-ttu-id="ff185-175">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="ff185-175">/api/products/*id*</span></span> |

<span data-ttu-id="ff185-176">Pro `GetProduct` metody *id* v identifikátoru URI je zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="ff185-176">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="ff185-177">Například pokud chcete získat produktu s ID 5, je identifikátor URI `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="ff185-177">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="ff185-178">Další informace o tom, jak webové rozhraní API směruje požadavky HTTP na metody kontroleru najdete v tématu [směrování v rozhraní ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ff185-178">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="ff185-179">Volání webového rozhraní API s Javascript a jQuery</span><span class="sxs-lookup"><span data-stu-id="ff185-179">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="ff185-180">V této části přidáme stránku HTML, který používá rozhraní AJAX k volání webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ff185-180">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="ff185-181">Použijeme jQuery volání AJAX a také k aktualizaci stránky s výsledky.</span><span class="sxs-lookup"><span data-stu-id="ff185-181">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="ff185-182">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **přidat**, pak vyberte **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="ff185-182">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="ff185-183">V **přidat novou položku** dialogovém okně, vyberte **webové** pod uzlem **Visual C#** a pak vyberte **stránku HTML** položky.</span><span class="sxs-lookup"><span data-stu-id="ff185-183">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="ff185-184">Název stránky &quot;index.html&quot;.</span><span class="sxs-lookup"><span data-stu-id="ff185-184">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="ff185-185">Všechno, co je v tomto souboru nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="ff185-185">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="ff185-186">Chcete-li získat jQuery několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="ff185-186">There are several ways to get jQuery.</span></span> <span data-ttu-id="ff185-187">V tomto příkladu lze použít [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="ff185-187">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="ff185-188">Také si můžete stáhnout z [ http://jquery.com/ ](http://jquery.com/)a ASP.NET "Webového rozhraní API" Šablona projektu zahrnuje také jQuery.</span><span class="sxs-lookup"><span data-stu-id="ff185-188">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="ff185-189">Získávání seznam produktů</span><span class="sxs-lookup"><span data-stu-id="ff185-189">Getting a List of Products</span></span>

<span data-ttu-id="ff185-190">Pokud chcete získat seznam produktů, odeslání požadavku HTTP GET na &quot;/api/produkty&quot;.</span><span class="sxs-lookup"><span data-stu-id="ff185-190">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="ff185-191">JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) funkce odešle požadavek AJAX.</span><span class="sxs-lookup"><span data-stu-id="ff185-191">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="ff185-192">Odpověď obsahuje pole objektů JSON.</span><span class="sxs-lookup"><span data-stu-id="ff185-192">For response contains array of JSON objects.</span></span> <span data-ttu-id="ff185-193">`done` Určuje funkce zpětného volání, která je volána, pokud neproběhne.</span><span class="sxs-lookup"><span data-stu-id="ff185-193">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="ff185-194">Zpětné volání aktualizujeme modelu DOM s informace o produktu.</span><span class="sxs-lookup"><span data-stu-id="ff185-194">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="ff185-195">Získání ID produktu</span><span class="sxs-lookup"><span data-stu-id="ff185-195">Getting a Product By ID</span></span>

<span data-ttu-id="ff185-196">Potřebujete-li produkt podle ID, pošlete žádost HTTP GET na &quot;/api/produkty/*id*&quot;, kde *id* je ID produktu.</span><span class="sxs-lookup"><span data-stu-id="ff185-196">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="ff185-197">Stále říkáme `getJSON` odeslat požadavek AJAX, avšak v této verzi jsme uveďte ID v identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="ff185-197">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="ff185-198">Odpověď z této žádosti je reprezentaci JSON jednoho produktu.</span><span class="sxs-lookup"><span data-stu-id="ff185-198">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="ff185-199">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ff185-199">Running the Application</span></span>

<span data-ttu-id="ff185-200">Stisknutím klávesy F5 spusťte ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff185-200">Press F5 to start debugging the application.</span></span> <span data-ttu-id="ff185-201">Webová stránka by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="ff185-201">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="ff185-202">Získání produktu podle ID, zadejte ID a klikněte na tlačítko Hledat:</span><span class="sxs-lookup"><span data-stu-id="ff185-202">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="ff185-203">Pokud zadáte neplatné ID, server vrátí chybu HTTP:</span><span class="sxs-lookup"><span data-stu-id="ff185-203">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="ff185-204">Pomocí F12 zobrazíte HTTP žádosti a odpovědi</span><span class="sxs-lookup"><span data-stu-id="ff185-204">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="ff185-205">Při práci se službou HTTP, může být velmi užitečná pro požadavek HTTP zobrazit a vyžádat zprávy.</span><span class="sxs-lookup"><span data-stu-id="ff185-205">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="ff185-206">To provedete pomocí nástrojů pro vývojáře F12 v aplikaci Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="ff185-206">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="ff185-207">Z aplikace Internet Explorer 9, stiskněte klávesu **F12** otevřete nástroje.</span><span class="sxs-lookup"><span data-stu-id="ff185-207">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="ff185-208">Klikněte **sítě** kartě a stiskněte klávesu **spustit zachytávání**.</span><span class="sxs-lookup"><span data-stu-id="ff185-208">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="ff185-209">Nyní přejděte zpět na webovou stránku a stiskněte klávesu **F5** načtením webové stránky.</span><span class="sxs-lookup"><span data-stu-id="ff185-209">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="ff185-210">Internet Explorer bude zaznamenávat provoz protokolu HTTP mezi prohlížečem a webový server.</span><span class="sxs-lookup"><span data-stu-id="ff185-210">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="ff185-211">Souhrnné zobrazení se zobrazuje veškerý síťový provoz pro stránku:</span><span class="sxs-lookup"><span data-stu-id="ff185-211">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="ff185-212">Vyhledejte položku pro relativní identifikátor URI "rozhraní api nebo produkty /".</span><span class="sxs-lookup"><span data-stu-id="ff185-212">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="ff185-213">Vyberte tuto položku a klikněte na **přejít na podrobné zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ff185-213">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="ff185-214">V zobrazení podrobností jsou karty zobrazíte hlavičkách žádostí a odpovědí a subjektů.</span><span class="sxs-lookup"><span data-stu-id="ff185-214">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="ff185-215">Například pokud kliknete **hlavičky požadavku** kartě uvidíte, že si klient vyžádal &quot;application/json&quot; v hlavičce Accept.</span><span class="sxs-lookup"><span data-stu-id="ff185-215">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="ff185-216">Pokud kliknete na kartu text odpovědi, uvidíte, jak byl seznam produktů serializovat na JSON.</span><span class="sxs-lookup"><span data-stu-id="ff185-216">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="ff185-217">Jiné prohlížeče mají podobné funkce.</span><span class="sxs-lookup"><span data-stu-id="ff185-217">Other browsers have similar functionality.</span></span> <span data-ttu-id="ff185-218">Další užitečné nástroj je [Fiddler](http://www.fiddler2.com/fiddler2/), webové proxy ladění.</span><span class="sxs-lookup"><span data-stu-id="ff185-218">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="ff185-219">Můžete použít aplikaci Fiddler zobrazíte provozu protokolu HTTP a také na vytvoření požadavků protokolu HTTP, která umožňuje plnou kontrolu nad hlavičky protokolu HTTP v požadavku.</span><span class="sxs-lookup"><span data-stu-id="ff185-219">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="ff185-220">Tato aplikace spuštěné v Azure</span><span class="sxs-lookup"><span data-stu-id="ff185-220">See this App Running on Azure</span></span>

<span data-ttu-id="ff185-221">Chcete zobrazit dokončení web spuštěný jako živou webovou aplikaci?</span><span class="sxs-lookup"><span data-stu-id="ff185-221">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="ff185-222">Úplnou verzi aplikace můžete nasadit ke svému účtu Azure jednoduše kliknutím na následující tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff185-222">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="ff185-223">Potřebujete účet Azure k nasazení tohoto řešení do Azure.</span><span class="sxs-lookup"><span data-stu-id="ff185-223">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="ff185-224">Pokud již účet nemáte, máte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="ff185-224">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="ff185-225">[Zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ff185-225">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="ff185-226">[Aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ff185-226">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff185-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff185-227">Next Steps</span></span>

- <span data-ttu-id="ff185-228">Více kompletní příklad, jak služby protokolu HTTP, který podporuje akce POST, PUT a DELETE a zapisuje do databáze, najdete v části [pomocí webového rozhraní API 2 s Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="ff185-228">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="ff185-229">Další informace o vytváření plynulá práce a rychlé reakce webových aplikací na základě služby HTTP najdete v tématu [jedné stránky aplikace ASP.NET](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="ff185-229">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="ff185-230">Informace o tom, jak nasadit webový projekt sady Visual Studio do služby Azure App Service najdete v tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="ff185-230">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
