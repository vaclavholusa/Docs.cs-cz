---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Část 3: Vytvoření správce řadiče | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="4baef-102">Část 3: Vytvoření správce řadiče</span><span class="sxs-lookup"><span data-stu-id="4baef-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="4baef-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4baef-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4baef-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="4baef-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="4baef-105">Přidání správce řadiče</span><span class="sxs-lookup"><span data-stu-id="4baef-105">Add an Admin Controller</span></span>

<span data-ttu-id="4baef-106">V této části přidáme kontroleru webového rozhraní API, která podporuje CRUD (vytvářet, číst, aktualizovat a odstranit) operací na produkty.</span><span class="sxs-lookup"><span data-stu-id="4baef-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="4baef-107">Řadičem použije ke komunikaci s vrstva databáze Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4baef-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="4baef-108">Pouze Administrátoři budou moci používat tento řadič.</span><span class="sxs-lookup"><span data-stu-id="4baef-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="4baef-109">Zákazníci budou přistupovat prostřednictvím jiného řadiče produkty.</span><span class="sxs-lookup"><span data-stu-id="4baef-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="4baef-110">V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="4baef-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="4baef-111">Vyberte **přidat** a potom **řadič**.</span><span class="sxs-lookup"><span data-stu-id="4baef-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="4baef-112">V **přidat kontroler** dialogové okno, názvu kontroleru `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="4baef-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="4baef-113">V části **šablony**, vyberte &quot;kontroler API s akcemi čtení/zápisu používající rozhraní Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="4baef-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="4baef-114">V části **třída modelu**, vyberte "Produkt (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="4baef-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="4baef-115">V části **kontextu dat**, vyberte možnost "&lt;nový kontext dat&gt;".</span><span class="sxs-lookup"><span data-stu-id="4baef-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="4baef-116">Pokud **třída modelu** rozevíracího seznamu nejsou zobrazeny všechny třídy modelu, zajistěte, aby zkompilovat projektu.</span><span class="sxs-lookup"><span data-stu-id="4baef-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="4baef-117">Rozhraní Entity Framework používá reflexe, proto je nutné kompilované sestavení.</span><span class="sxs-lookup"><span data-stu-id="4baef-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="4baef-118">Výběr "&lt;nový kontext dat&gt;" se otevře **nový kontext dat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4baef-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="4baef-119">Název kontextu dat `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="4baef-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="4baef-120">Klikněte na tlačítko **OK** pro zavření **nový kontext dat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4baef-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="4baef-121">V **přidat kontroler** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4baef-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="4baef-122">Zde je, co byly přidány do projektu:</span><span class="sxs-lookup"><span data-stu-id="4baef-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="4baef-123">Třída s názvem `OrdersContext` která je odvozena od **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="4baef-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="4baef-124">Tato třída poskytuje pojidlem mezi modely objektů POCO a databází.</span><span class="sxs-lookup"><span data-stu-id="4baef-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="4baef-125">Kontroler Web API s názvem `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="4baef-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="4baef-126">Tento řadič podporujícího operace CRUD v `Product` instance.</span><span class="sxs-lookup"><span data-stu-id="4baef-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="4baef-127">Použije `OrdersContext` třídy ke komunikaci s Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4baef-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="4baef-128">Nový připojovací řetězec databáze v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="4baef-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="4baef-129">Otevřete soubor OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="4baef-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="4baef-130">Všimněte si, že konstruktoru Určuje název připojovacího řetězce databáze.</span><span class="sxs-lookup"><span data-stu-id="4baef-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="4baef-131">Tento název se odkazuje na připojovací řetězec, který byl přidán do souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="4baef-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="4baef-132">Přidejte následující vlastnosti pro `OrdersContext` třídy:</span><span class="sxs-lookup"><span data-stu-id="4baef-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="4baef-133">A **DbSet** představuje sadu entit, které může být dotazován.</span><span class="sxs-lookup"><span data-stu-id="4baef-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="4baef-134">Tady je úplný seznam pro `OrdersContext` třídy:</span><span class="sxs-lookup"><span data-stu-id="4baef-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="4baef-135">`AdminController` Třída definuje pět metody, které implementují základní funkce CRUD.</span><span class="sxs-lookup"><span data-stu-id="4baef-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="4baef-136">Každá metoda odpovídá identifikátoru URI, který lze vyvolat klienta:</span><span class="sxs-lookup"><span data-stu-id="4baef-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="4baef-137">Metoda kontroleru</span><span class="sxs-lookup"><span data-stu-id="4baef-137">Controller Method</span></span> | <span data-ttu-id="4baef-138">Popis</span><span class="sxs-lookup"><span data-stu-id="4baef-138">Description</span></span> | <span data-ttu-id="4baef-139">Identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="4baef-139">URI</span></span> | <span data-ttu-id="4baef-140">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="4baef-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4baef-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="4baef-141">GetProducts</span></span> | <span data-ttu-id="4baef-142">Získá všechny produkty.</span><span class="sxs-lookup"><span data-stu-id="4baef-142">Gets all products.</span></span> | <span data-ttu-id="4baef-143">rozhraní API nebo produkty</span><span class="sxs-lookup"><span data-stu-id="4baef-143">api/products</span></span> | <span data-ttu-id="4baef-144">GET</span><span class="sxs-lookup"><span data-stu-id="4baef-144">GET</span></span> |
| <span data-ttu-id="4baef-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="4baef-145">GetProduct</span></span> | <span data-ttu-id="4baef-146">Vyhledá produkt podle ID.</span><span class="sxs-lookup"><span data-stu-id="4baef-146">Finds a product by ID.</span></span> | <span data-ttu-id="4baef-147">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="4baef-147">api/products/*id*</span></span> | <span data-ttu-id="4baef-148">GET</span><span class="sxs-lookup"><span data-stu-id="4baef-148">GET</span></span> |
| <span data-ttu-id="4baef-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="4baef-149">PutProduct</span></span> | <span data-ttu-id="4baef-150">Aktualizace produktu.</span><span class="sxs-lookup"><span data-stu-id="4baef-150">Updates a product.</span></span> | <span data-ttu-id="4baef-151">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="4baef-151">api/products/*id*</span></span> | <span data-ttu-id="4baef-152">PUT</span><span class="sxs-lookup"><span data-stu-id="4baef-152">PUT</span></span> |
| <span data-ttu-id="4baef-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="4baef-153">PostProduct</span></span> | <span data-ttu-id="4baef-154">Vytvoří nového produktu.</span><span class="sxs-lookup"><span data-stu-id="4baef-154">Creates a new product.</span></span> | <span data-ttu-id="4baef-155">rozhraní API nebo produkty</span><span class="sxs-lookup"><span data-stu-id="4baef-155">api/products</span></span> | <span data-ttu-id="4baef-156">POST</span><span class="sxs-lookup"><span data-stu-id="4baef-156">POST</span></span> |
| <span data-ttu-id="4baef-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="4baef-157">DeleteProduct</span></span> | <span data-ttu-id="4baef-158">Odstraní produktu.</span><span class="sxs-lookup"><span data-stu-id="4baef-158">Deletes a product.</span></span> | <span data-ttu-id="4baef-159">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="4baef-159">api/products/*id*</span></span> | <span data-ttu-id="4baef-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="4baef-160">DELETE</span></span> |

<span data-ttu-id="4baef-161">Každá metoda volá do `OrdersContext` dotazu databázi.</span><span class="sxs-lookup"><span data-stu-id="4baef-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="4baef-162">Volání metody, které upravit kolekci (PUT, POST a odstranit) `db.SaveChanges` k zachování změn do databáze.</span><span class="sxs-lookup"><span data-stu-id="4baef-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="4baef-163">Řadiče jsou vytvořené za požadavku HTTP a poté odstraněny, takže je potřeba předtím, než vrátí metodu zachování změn.</span><span class="sxs-lookup"><span data-stu-id="4baef-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="4baef-164">Přidat inicializátoru databáze</span><span class="sxs-lookup"><span data-stu-id="4baef-164">Add a Database Initializer</span></span>

<span data-ttu-id="4baef-165">Rozhraní Entity Framework má dobrý funkce, která vám umožní naplnit databázi na spuštění a při každé změně modely automaticky znovu vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="4baef-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="4baef-166">Tato funkce je užitečná při vývoji, protože máte vždy nějaká testovací data, i když změníte modely.</span><span class="sxs-lookup"><span data-stu-id="4baef-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="4baef-167">V Průzkumníku řešení klikněte pravým tlačítkem na složku modely a vytvořte novou třídu s názvem `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="4baef-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="4baef-168">Vložte následující implementace:</span><span class="sxs-lookup"><span data-stu-id="4baef-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="4baef-169">Dědění ze **DropCreateDatabaseIfModelChanges** třída, jsme informace o tom, rozhraní Entity Framework pro odpojení databáze vždy, když jsme upravit třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="4baef-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="4baef-170">Když Entity Framework vytvoří (nebo vytvoří) databázi, zavolá **počáteční hodnoty** metoda k naplnění tabulky.</span><span class="sxs-lookup"><span data-stu-id="4baef-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="4baef-171">Používáme **počáteční hodnoty** metody přidat některé produkty, příklad plus Příklad pořadí.</span><span class="sxs-lookup"><span data-stu-id="4baef-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="4baef-172">Tato funkce je skvělá pro testování, ale nepoužívají **DropCreateDatabaseIfModelChanges** třídy v produkčním prostředí, protože jste mohlo dojít ke ztrátě dat pokud někdo změní třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="4baef-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="4baef-173">Potom otevřete soubor Global.asax a přidejte následující kód, který **aplikace\_spustit** metoda:</span><span class="sxs-lookup"><span data-stu-id="4baef-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="4baef-174">Odeslat požadavek na Kontroleru</span><span class="sxs-lookup"><span data-stu-id="4baef-174">Send a Request to the Controller</span></span>

<span data-ttu-id="4baef-175">V tuto chvíli jsme nebyly zapsány žádné kód klienta, ale můžete vyvolat webové rozhraní API pomocí webového prohlížeče nebo ladění HTTP nástroje, jako [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="4baef-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="4baef-176">Ve Visual Studiu stisknutím klávesy F5 spusťte ladění.</span><span class="sxs-lookup"><span data-stu-id="4baef-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="4baef-177">Otevře se webový prohlížeč k `http://localhost:*portnum*/`, kde *portnum* je některé číslo portu.</span><span class="sxs-lookup"><span data-stu-id="4baef-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="4baef-178">Odeslat požadavek HTTP "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="4baef-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="4baef-179">První požadavek může být pomalé, protože Entify Framework nutné vytvořit a naplnit databázi.</span><span class="sxs-lookup"><span data-stu-id="4baef-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="4baef-180">Odpověď by měla něco podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="4baef-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="4baef-181">[Předchozí](using-web-api-with-entity-framework-part-2.md)
> [další](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="4baef-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
