---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: "Použití migrace Code First počáteční hodnoty databázi | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: df75a69644033cc76fee86b5a9692ab65beb4d01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="5d471-102">Použití migrace Code First počáteční hodnoty databáze</span><span class="sxs-lookup"><span data-stu-id="5d471-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="5d471-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5d471-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5d471-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="5d471-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="5d471-105">V této části budete používat [migrace Code First](https://msdn.microsoft.com/en-us/data/jj591621) v EF počáteční hodnoty databáze s testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="5d471-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/en-us/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="5d471-106">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="5d471-106">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="5d471-107">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5d471-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="5d471-108">Tento příkaz přidá složku s názvem migrace do projektu a kód soubor s názvem Configuration.cs ve složce migrace.</span><span class="sxs-lookup"><span data-stu-id="5d471-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="5d471-109">Otevřete soubor Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="5d471-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="5d471-110">Přidejte následující **pomocí** příkaz.</span><span class="sxs-lookup"><span data-stu-id="5d471-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="5d471-111">Pak přidejte následující kód, který **Configuration.Seed** metoda:</span><span class="sxs-lookup"><span data-stu-id="5d471-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="5d471-112">V okně konzoly Správce balíčků zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5d471-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="5d471-113">První příkaz vygeneruje kód, který vytvoří databázi a v druhém příkazu spustí tento kód.</span><span class="sxs-lookup"><span data-stu-id="5d471-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="5d471-114">Databáze je vytvořená místně, pomocí [LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="5d471-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="5d471-115">Prozkoumejte rozhraní API (volitelné)</span><span class="sxs-lookup"><span data-stu-id="5d471-115">Explore the API (Optional)</span></span>

<span data-ttu-id="5d471-116">Stisknutím klávesy F5 spusťte aplikaci v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="5d471-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="5d471-117">Visual Studio spustí službu IIS Express a spustí webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5d471-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="5d471-118">Visual Studio pak spustí prohlížeč a otevře domovskou stránku aplikace.</span><span class="sxs-lookup"><span data-stu-id="5d471-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="5d471-119">Po spuštění webového projektu sady Visual Studio přiřadí číslo portu.</span><span class="sxs-lookup"><span data-stu-id="5d471-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="5d471-120">Na obrázku níže číslo portu je 50524.</span><span class="sxs-lookup"><span data-stu-id="5d471-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="5d471-121">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="5d471-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="5d471-122">Domovská stránka je implementovaná pomocí ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5d471-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="5d471-123">V horní části stránky je odkaz, který uvádí "API".</span><span class="sxs-lookup"><span data-stu-id="5d471-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="5d471-124">Tento odkaz vám přináší na stránku nápovědy automaticky generovaných pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5d471-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="5d471-125">(Další způsob, jakým vygeneruje tuto stránku nápovědy a jak můžete přidat vlastní dokumentaci na stránku najdete v tématu [vytváření pomoci stránek pro ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Chcete-li zobrazit podrobnosti o rozhraní API, včetně formát požadavku a odpovědi odkazů na stránky se můžete kliknutím na tlačítko Nápověda.</span><span class="sxs-lookup"><span data-stu-id="5d471-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="5d471-126">Rozhraní API umožňuje operace CRUD v databázi.</span><span class="sxs-lookup"><span data-stu-id="5d471-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="5d471-127">Zde je souhrn rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5d471-127">The following summarizes the API.</span></span>

| <span data-ttu-id="5d471-128">Autoři</span><span class="sxs-lookup"><span data-stu-id="5d471-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="5d471-129">ZÍSKAT rozhraní api nebo autoři</span><span class="sxs-lookup"><span data-stu-id="5d471-129">GET api/authors</span></span> | <span data-ttu-id="5d471-130">Získáte všechny autory.</span><span class="sxs-lookup"><span data-stu-id="5d471-130">Get all authors.</span></span> |
| <span data-ttu-id="5d471-131">Rozhraní api nebo autoři GET / {id}</span><span class="sxs-lookup"><span data-stu-id="5d471-131">GET api/authors/{id}</span></span> | <span data-ttu-id="5d471-132">Získat Autor podle ID.</span><span class="sxs-lookup"><span data-stu-id="5d471-132">Get an author by ID.</span></span> |
| <span data-ttu-id="5d471-133">POST/api/autoři</span><span class="sxs-lookup"><span data-stu-id="5d471-133">POST /api/authors</span></span> | <span data-ttu-id="5d471-134">Vytvořte nový autora.</span><span class="sxs-lookup"><span data-stu-id="5d471-134">Create a new author.</span></span> |
| <span data-ttu-id="5d471-135">UVEĎTE /api/autoři / {id}</span><span class="sxs-lookup"><span data-stu-id="5d471-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="5d471-136">Aktualizujte stávající autora.</span><span class="sxs-lookup"><span data-stu-id="5d471-136">Update an existing author.</span></span> |
| <span data-ttu-id="5d471-137">Odstranit /api/autoři / {id}</span><span class="sxs-lookup"><span data-stu-id="5d471-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="5d471-138">Odstraňte Autor.</span><span class="sxs-lookup"><span data-stu-id="5d471-138">Delete an author.</span></span> |

| <span data-ttu-id="5d471-139">Knihy</span><span class="sxs-lookup"><span data-stu-id="5d471-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="5d471-140">ZÍSKAT /api/books</span><span class="sxs-lookup"><span data-stu-id="5d471-140">GET /api/books</span></span> | <span data-ttu-id="5d471-141">Získání všech knih.</span><span class="sxs-lookup"><span data-stu-id="5d471-141">Get all books.</span></span> |
| <span data-ttu-id="5d471-142">ZÍSKAT /api/books / {id}</span><span class="sxs-lookup"><span data-stu-id="5d471-142">GET /api/books/{id}</span></span> | <span data-ttu-id="5d471-143">Získání seznamu pomocí ID.</span><span class="sxs-lookup"><span data-stu-id="5d471-143">Get a book by ID.</span></span> |
| <span data-ttu-id="5d471-144">ODESÍLÁNÍ/api/seznamů</span><span class="sxs-lookup"><span data-stu-id="5d471-144">POST /api/books</span></span> | <span data-ttu-id="5d471-145">Vytvoření nového seznamu.</span><span class="sxs-lookup"><span data-stu-id="5d471-145">Create a new book.</span></span> |
| <span data-ttu-id="5d471-146">UVEĎTE /api/books / {id}</span><span class="sxs-lookup"><span data-stu-id="5d471-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="5d471-147">Aktualizujte stávající adresáře.</span><span class="sxs-lookup"><span data-stu-id="5d471-147">Update an existing book.</span></span> |
| <span data-ttu-id="5d471-148">Odstranit /api/books / {id}</span><span class="sxs-lookup"><span data-stu-id="5d471-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="5d471-149">Odstranění seznamu.</span><span class="sxs-lookup"><span data-stu-id="5d471-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="5d471-150">Zobrazení databáze (volitelné)</span><span class="sxs-lookup"><span data-stu-id="5d471-150">View the Database (Optional)</span></span>

<span data-ttu-id="5d471-151">Když jste spustili příkaz Update-Database, EF databáze a názvem `Seed` metoda.</span><span class="sxs-lookup"><span data-stu-id="5d471-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="5d471-152">Když aplikaci spouštíte místně, použije EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="5d471-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="5d471-153">Databázi můžete zobrazit v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5d471-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="5d471-154">Z **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="5d471-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="5d471-155">V **připojit k serveru** dialogové okno, v **název serveru** textové pole, zadejte "(localdb) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="5d471-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="5d471-156">Ponechte **ověřování** možnost jako "Ověřování systému Windows".</span><span class="sxs-lookup"><span data-stu-id="5d471-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="5d471-157">Klikněte na tlačítko **připojit**.</span><span class="sxs-lookup"><span data-stu-id="5d471-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="5d471-158">Visual Studio připojí na instanci LocalDB a ukazuje vaše stávající databáze, v okně Průzkumník objektů systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5d471-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="5d471-159">Můžete rozbalit uzly zobrazíte tabulek, které EF vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="5d471-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="5d471-160">Chcete-li zobrazit data, klikněte pravým tlačítkem na tabulku a vyberte **Data zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="5d471-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="5d471-161">Následující snímek obrazovky ukazuje výsledky pro knihy tabulky.</span><span class="sxs-lookup"><span data-stu-id="5d471-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="5d471-162">Všimněte si, že EF naplní databázi s počáteční hodnoty data a tabulka obsahuje cizí klíč k tabulce Autoři.</span><span class="sxs-lookup"><span data-stu-id="5d471-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
<span data-ttu-id="5d471-163">[Předchozí](part-2.md)
[další](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="5d471-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
