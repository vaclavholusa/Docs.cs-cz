---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Část 3: Vytvoření Kontroleru pro správce | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: b9d21edec7b5006beea83395cdfc5ae181992e7d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365042"
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="b0bf9-102">Část 3: Vytvoření Kontroleru pro správce</span><span class="sxs-lookup"><span data-stu-id="b0bf9-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="b0bf9-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b0bf9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b0bf9-104">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="b0bf9-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="b0bf9-105">Přidání Kontroleru pro správce</span><span class="sxs-lookup"><span data-stu-id="b0bf9-105">Add an Admin Controller</span></span>

<span data-ttu-id="b0bf9-106">V této části přidáme kontroler Web API, která podporuje CRUD (vytváření, čtení, aktualizace a odstranění) týkající se produktů.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="b0bf9-107">Kontroler pomocí Entity Framework komunikovat s databázové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="b0bf9-108">Pouze Administrátoři budou moci použít tento kontroler.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="b0bf9-109">Zákazníci budou přistupovat prostřednictvím jiného řadiče produktů.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="b0bf9-110">V Průzkumníku řešení klikněte pravým tlačítkem myši na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="b0bf9-111">Vyberte **přidat** a potom **řadič**.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="b0bf9-112">V **přidat kontroler** dialogového okna, názvu kontroleru `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="b0bf9-113">V části **šablony**vyberte &quot;kontroler API s akcemi čtení/zápisu, používá nástroj Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="b0bf9-114">V části **třída modelu**, vyberte "Produktu (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="b0bf9-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="b0bf9-115">V části **kontext dat**vyberte "&lt;nový kontext dat&gt;".</span><span class="sxs-lookup"><span data-stu-id="b0bf9-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="b0bf9-116">Pokud **třída modelu** rozevíracího seznamu nezobrazuje žádné třídy modelu, ujistěte se, že kompilaci projektu.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="b0bf9-117">Entity Framework používá reflexi, proto musí zkompilovaného sestavení.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="b0bf9-118">Výběr "&lt;nový kontext dat.&gt;" se otevře **nový kontext dat** dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="b0bf9-119">Název kontextu dat `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="b0bf9-120">Klikněte na tlačítko **OK** zrušíte **nový kontext dat** dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="b0bf9-121">V **přidat kontroler** dialogového okna, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="b0bf9-122">Zde je, co byl přidán do projektu:</span><span class="sxs-lookup"><span data-stu-id="b0bf9-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="b0bf9-123">Třída s názvem `OrdersContext` , která je odvozena z **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="b0bf9-124">Tato třída poskytuje skutečným pojidlem mezi modely POCO a databází.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="b0bf9-125">Kontroler Web API s názvem `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="b0bf9-126">Tento kontroler podporuje operace CRUD v `Product` instancí.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="b0bf9-127">Používá `OrdersContext` třídy ke komunikaci s Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="b0bf9-128">Nový připojovací řetězec databáze v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="b0bf9-129">Otevřete soubor OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="b0bf9-130">Všimněte si, že konstruktor Určuje název připojovacího řetězce databáze.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="b0bf9-131">Tento název se odkazuje na připojovací řetězec, který byl přidán do souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="b0bf9-132">Přidejte následující vlastnosti pro `OrdersContext` třídy:</span><span class="sxs-lookup"><span data-stu-id="b0bf9-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="b0bf9-133">A **DbSet** představuje sadu entit, které je možné zadávat dotazy.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="b0bf9-134">Tady je úplný seznam pro `OrdersContext` třídy:</span><span class="sxs-lookup"><span data-stu-id="b0bf9-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="b0bf9-135">`AdminController` Třída definuje pět metod, které implementují základních funkcí CRUD.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="b0bf9-136">Každá metoda odpovídá identifikátoru URI, který může vyvolat klienta:</span><span class="sxs-lookup"><span data-stu-id="b0bf9-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="b0bf9-137">Metoda kontroleru</span><span class="sxs-lookup"><span data-stu-id="b0bf9-137">Controller Method</span></span> | <span data-ttu-id="b0bf9-138">Popis</span><span class="sxs-lookup"><span data-stu-id="b0bf9-138">Description</span></span> | <span data-ttu-id="b0bf9-139">Identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="b0bf9-139">URI</span></span> | <span data-ttu-id="b0bf9-140">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="b0bf9-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b0bf9-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="b0bf9-141">GetProducts</span></span> | <span data-ttu-id="b0bf9-142">Získá všechny produkty.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-142">Gets all products.</span></span> | <span data-ttu-id="b0bf9-143">rozhraní API a produktů</span><span class="sxs-lookup"><span data-stu-id="b0bf9-143">api/products</span></span> | <span data-ttu-id="b0bf9-144">ZÍSKAT</span><span class="sxs-lookup"><span data-stu-id="b0bf9-144">GET</span></span> |
| <span data-ttu-id="b0bf9-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="b0bf9-145">GetProduct</span></span> | <span data-ttu-id="b0bf9-146">Vyhledá produktů podle ID.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-146">Finds a product by ID.</span></span> | <span data-ttu-id="b0bf9-147">rozhraníAPI/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="b0bf9-147">api/products/*id*</span></span> | <span data-ttu-id="b0bf9-148">ZÍSKAT</span><span class="sxs-lookup"><span data-stu-id="b0bf9-148">GET</span></span> |
| <span data-ttu-id="b0bf9-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="b0bf9-149">PutProduct</span></span> | <span data-ttu-id="b0bf9-150">Aktualizace produktu.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-150">Updates a product.</span></span> | <span data-ttu-id="b0bf9-151">rozhraníAPI/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="b0bf9-151">api/products/*id*</span></span> | <span data-ttu-id="b0bf9-152">PUT</span><span class="sxs-lookup"><span data-stu-id="b0bf9-152">PUT</span></span> |
| <span data-ttu-id="b0bf9-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="b0bf9-153">PostProduct</span></span> | <span data-ttu-id="b0bf9-154">Vytvoří nový produkt.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-154">Creates a new product.</span></span> | <span data-ttu-id="b0bf9-155">rozhraní API a produktů</span><span class="sxs-lookup"><span data-stu-id="b0bf9-155">api/products</span></span> | <span data-ttu-id="b0bf9-156">PŘÍSPĚVEK</span><span class="sxs-lookup"><span data-stu-id="b0bf9-156">POST</span></span> |
| <span data-ttu-id="b0bf9-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="b0bf9-157">DeleteProduct</span></span> | <span data-ttu-id="b0bf9-158">Odstraní produktu.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-158">Deletes a product.</span></span> | <span data-ttu-id="b0bf9-159">rozhraníAPI/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="b0bf9-159">api/products/*id*</span></span> | <span data-ttu-id="b0bf9-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="b0bf9-160">DELETE</span></span> |

<span data-ttu-id="b0bf9-161">Každá metoda volá `OrdersContext` dáte dotaz na databázi.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="b0bf9-162">Volání metody, které upravit kolekci (PUT, POST a DELETE) `db.SaveChanges` a zachová tak změny do databáze.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="b0bf9-163">Řadiče jsou vytvořené na požadavek HTTP a poté odstraněn, proto je potřeba předtím, než vrátí metodu zachování změn.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="b0bf9-164">Přidat inicializační výraz databáze</span><span class="sxs-lookup"><span data-stu-id="b0bf9-164">Add a Database Initializer</span></span>

<span data-ttu-id="b0bf9-165">Entity Framework obsahuje skvělé funkce, která vám umožní naplnit databázi při spuštění a při každé změně modelů automaticky znovu vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="b0bf9-166">Tato funkce je užitečná při vývoji, protože budete mít vždy nějaká testovací data, i když změníte modely.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="b0bf9-167">V Průzkumníku řešení klikněte pravým tlačítkem na složku modely a vytvořte novou třídu s názvem `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="b0bf9-168">Vložte následující implementaci:</span><span class="sxs-lookup"><span data-stu-id="b0bf9-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="b0bf9-169">Dědění z **DropCreateDatabaseIfModelChanges** třídy, jsme o tom, Entity Framework pro odpojení databáze pokaždé, když upravíme tříd modelu.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="b0bf9-170">Pokud rozhraní Entity Framework vytvoří (nebo znovu vytvoří) databáze, volání **počáteční hodnoty** metoda naplnění tabulek.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="b0bf9-171">Používáme **počáteční hodnoty** způsob, jak přidat některé produkty příklad plus příklad objednávky.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="b0bf9-172">Tato funkce se skvěle hodí pro testování, ale nepoužívají **DropCreateDatabaseIfModelChanges** třídy v produkčním prostředí, protože pokud někdo přechází na jiné třídy modelu může přijít o data.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="b0bf9-173">Dále otevřete Global.asax a přidejte následující kód, který **aplikace\_Start** metody:</span><span class="sxs-lookup"><span data-stu-id="b0bf9-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="b0bf9-174">Odeslat požadavek na Kontroleru</span><span class="sxs-lookup"><span data-stu-id="b0bf9-174">Send a Request to the Controller</span></span>

<span data-ttu-id="b0bf9-175">V tuto chvíli jsme nenapsali jakýkoli kód klienta, ale můžete vyvolat webové rozhraní API pomocí webového prohlížeče nebo ladění HTTP nástroj, jako [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="b0bf9-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="b0bf9-176">V sadě Visual Studio stisknutím klávesy F5 spusťte ladění.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="b0bf9-177">Ve webovém prohlížeči se otevře na `http://localhost:*portnum*/`, kde *portnum* je nějaké číslo portu.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="b0bf9-178">Odeslání požadavku HTTP "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="b0bf9-179">První požadavek může být pomalé dokončit, protože Entify Framework potřebuje k vytvoření a přidání dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="b0bf9-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="b0bf9-180">Odpověď by měla něco podobného následujícímu:</span><span class="sxs-lookup"><span data-stu-id="b0bf9-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="b0bf9-181">[Předchozí](using-web-api-with-entity-framework-part-2.md)
> [další](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="b0bf9-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
