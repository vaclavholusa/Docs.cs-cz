---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: "Přidání nové pole do modelu film a tabulka | Microsoft Docs"
author: Rick-Anderson
description: "Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, mnohem jednodušší a postupujte podle ukázku..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 9965c8a755857a8e8cb8ecbc6c467a6c856aa83d
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="533d1-104">Přidání nové pole do modelu film a tabulky</span><span class="sxs-lookup"><span data-stu-id="533d1-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="533d1-105">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="533d1-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="533d1-106">Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="533d1-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="533d1-107">Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="533d1-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="533d1-108">V této části používáte migrace Code First Entity Framework pro migraci některé změny do třídy modelu tak, že použití této změny do databáze.</span><span class="sxs-lookup"><span data-stu-id="533d1-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="533d1-109">Ve výchozím nastavení při použití Entity Framework Code First automaticky vytvořit databázi, stejně jako dříve v tomto kurzu Code First přidá tabulku k databázi chcete-li sledovat, jestli je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="533d1-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="533d1-110">Pokud nejsou synchronizované, rozhraní Entity Framework vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="533d1-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="533d1-111">To usnadňuje sledovat problémy v čase vývoj, který může jinak pouze pro vás (podle skrytého chyby) v době běhu.</span><span class="sxs-lookup"><span data-stu-id="533d1-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="533d1-112">Nastavení migrace Code First pro změny modelu</span><span class="sxs-lookup"><span data-stu-id="533d1-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="533d1-113">Pokud používáte Visual Studio 2012, dvakrát klikněte *Movies.mdf* soubor v Průzkumníku řešení otevřete nástroj databáze.</span><span class="sxs-lookup"><span data-stu-id="533d1-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="533d1-114">Visual Studio Express pro Web se zobrazí databáze Průzkumníka že Visual Studio 2012 se zobrazí v Průzkumníku serveru.</span><span class="sxs-lookup"><span data-stu-id="533d1-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="533d1-115">Pokud používáte Visual Studio 2010, použijte Průzkumníka objektů systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="533d1-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="533d1-116">V nástroji databáze (Průzkumník databáze, Průzkumníka serveru nebo Průzkumník objektů systému SQL Server), klikněte pravým tlačítkem na `MovieDBContext` a vyberte **odstranit** odpojení filmy databáze.</span><span class="sxs-lookup"><span data-stu-id="533d1-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="533d1-117">Přejděte zpět do Průzkumníka řešení.</span><span class="sxs-lookup"><span data-stu-id="533d1-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="533d1-118">Klikněte pravým tlačítkem na *Movies.mdf* soubor a vyberte **odstranit** k odebrání databáze filmy.</span><span class="sxs-lookup"><span data-stu-id="533d1-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="533d1-119">Sestavení aplikace a ujistěte se, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="533d1-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="533d1-120">Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven** a potom **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="533d1-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Přidat Pack Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="533d1-122">V **Konzola správce balíčků** v okno `PM>` řádku zadejte "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="533d1-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="533d1-123">**Enable-Migrations** příkaz (viz výše) vytvoří *Configuration.cs* soubor v nové *migrace* složky.</span><span class="sxs-lookup"><span data-stu-id="533d1-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="533d1-124">Otevře se Visual Studio *Configuration.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="533d1-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="533d1-125">Nahraďte `Seed` metoda v *Configuration.cs* soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="533d1-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="533d1-126">Klikněte pravým tlačítkem na červenou vlnovkou řádku pod `Movie` a vyberte **vyřešit** pak **pomocí** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="533d1-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="533d1-127">Díky tomu přidá následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="533d1-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="533d1-128">Kód volání první migrace `Seed` metoda po každé migraci (tedy volání **update-database** v konzole Správce balíčků), a tato metoda aktualizace řádků, které již byl vložen a vloží je, pokud jejich Nemáte ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="533d1-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="533d1-129">**Stisknutím kombinace kláves CTRL-SHIFT-B a tím projekt sestavit.** (Následující postup se nezdaří, pokud vaše není v tomto okamžiku sestavení.)</span><span class="sxs-lookup"><span data-stu-id="533d1-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="533d1-130">Dalším krokem je vytvoření `DbMigration` třídu pro počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="533d1-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="533d1-131">Tato migrace vytvoří novou databázi, která je důvod, proč můžete odstranit *movie.mdf* soubor v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="533d1-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="533d1-132">V **Konzola správce balíčků** okno, zadejte příkaz "Přidat migrace počáteční" k vytvoření počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="533d1-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="533d1-133">Název "Počáteční" libovolný a slouží k názvu souboru migrace vytvořili.</span><span class="sxs-lookup"><span data-stu-id="533d1-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="533d1-134">Migrace Code First vytvoří další soubor třídy ve *migrace* složky (s názvem *{DateStamp}\_Initial.cs* ), a tato třída obsahuje kód, který vytvoří schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="533d1-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="533d1-135">Název souboru migrace je předem vyřešili s časovým razítkem usnadní řazení.</span><span class="sxs-lookup"><span data-stu-id="533d1-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="533d1-136">Zkontrolujte *{DateStamp}\_Initial.cs* souboru, obsahuje pokyny pro vytvoření tabulky filmy pro film databáze.</span><span class="sxs-lookup"><span data-stu-id="533d1-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="533d1-137">Při aktualizaci databáze v podle pokynů níže, tento *{DateStamp}\_Initial.cs* soubor bude spustit a vytvořit schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="533d1-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="533d1-138">Pak se **počáteční hodnoty** metoda spustí k naplnění databáze s testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="533d1-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="533d1-139">V **Konzola správce balíčků**, zadejte příkazu "update-database" k vytvoření databáze a spusťte **počáteční hodnoty** metoda.</span><span class="sxs-lookup"><span data-stu-id="533d1-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="533d1-140">Pokud dojde k chybě, která označuje tabulka již existuje a nelze vytvořit, je možné, že jste spustili aplikaci po odstranění databáze a před provedení `update-database`.</span><span class="sxs-lookup"><span data-stu-id="533d1-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="533d1-141">V takovém případě odstraňte *Movies.mdf* znovu a zkuste zopakovat `update-database` příkaz.</span><span class="sxs-lookup"><span data-stu-id="533d1-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="533d1-142">Pokud stále dojde k chybě, odstraňte složku migrations a obsah, pak spusťte s pokyny uvedenými v horní části této stránky (která je odstranit *Movies.mdf* soubor pak pokračujte Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="533d1-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="533d1-143">Spusťte aplikaci a přejděte do */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="533d1-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="533d1-144">Se zobrazí data počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="533d1-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="533d1-145">Přidání vlastnosti hodnocení filmu modelu</span><span class="sxs-lookup"><span data-stu-id="533d1-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="533d1-146">Začněte přidáním nové `Rating` vlastnost, která má existující `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="533d1-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="533d1-147">Otevřete *Models\Movie.cs* souboru a přidejte `Rating` vlastnost podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="533d1-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="533d1-148">Kompletní `Movie` třídy nyní vypadá podobně jako následující kód:</span><span class="sxs-lookup"><span data-stu-id="533d1-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="533d1-149">Sestavení aplikace pomocí **sestavení** &gt; **sestavení film** nabídky příkazu nebo stisknutím kombinace kláves CTRL-SHIFT-B.</span><span class="sxs-lookup"><span data-stu-id="533d1-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="533d1-150">Teď, když jste aktualizovali `Model` třídy, je také potřeba aktualizovat *\Views\Movies\Index.cshtml* a *\Views\Movies\Create.cshtml* zobrazit šablony, aby bylo možné zobrazit nové `Rating`vlastnost v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="533d1-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="533d1-151">Otevřete*\Views\Movies\Index.cshtml* souboru a přidejte `<th>Rating</th>` právě po záhlaví sloupce **cena** sloupce.</span><span class="sxs-lookup"><span data-stu-id="533d1-151">Open the*\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="533d1-152">Pak přidejte `<td>` sloupce u konce šablony k vykreslení `@item.Rating` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="533d1-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="533d1-153">Níže je co aktualizované *Index.cshtml* zobrazit šablonu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="533d1-153">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="533d1-154">Dále otevřete *\Views\Movies\Create.cshtml* souboru a přidejte následující značku u konce formuláře.</span><span class="sxs-lookup"><span data-stu-id="533d1-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="533d1-155">To vykresluje textové pole tak, aby při vytváření nové film můžete zadat hodnocení.</span><span class="sxs-lookup"><span data-stu-id="533d1-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="533d1-156">Nyní po aktualizaci kódu aplikace, které podporují nové `Rating` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="533d1-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="533d1-157">Nyní spusťte aplikaci a přejděte do */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="533d1-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="533d1-158">Když to uděláte, ale se zobrazí jednu z těchto chyb:</span><span class="sxs-lookup"><span data-stu-id="533d1-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="533d1-159">Protože se tato chyba zobrazuje aktualizovaný `Movie` třídy modelu v aplikaci je nyní liší od schématu `Movie` tabulky existující databáze.</span><span class="sxs-lookup"><span data-stu-id="533d1-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="533d1-160">(Není žádná `Rating` sloupec v tabulce databáze.)</span><span class="sxs-lookup"><span data-stu-id="533d1-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="533d1-161">Řešení chyby několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="533d1-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="533d1-162">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi na základě nové třídy schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="533d1-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="533d1-163">Tento přístup je velmi praktické, když active vývoj pro testovací databázi. umožňuje rychle společně momentální schéma modelu a databáze.</span><span class="sxs-lookup"><span data-stu-id="533d1-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="533d1-164">Nevýhodou, ale je, že přijdete o stávající data v databázi – tak můžete *nemáte* chcete použít tuto metodu na produkční databázi!</span><span class="sxs-lookup"><span data-stu-id="533d1-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="533d1-165">Pomocí inicializátoru automaticky počáteční hodnoty databázi s daty test je často produktivní způsob, jak vyvíjet aplikace.</span><span class="sxs-lookup"><span data-stu-id="533d1-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="533d1-166">Další informace o inicializátory databáze Entity Framework najdete v tématu tní Dykstra [kurz ASP.NET MVC nebo Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="533d1-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="533d1-167">Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="533d1-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="533d1-168">Výhodou tohoto přístupu je, že zachováte data.</span><span class="sxs-lookup"><span data-stu-id="533d1-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="533d1-169">Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.</span><span class="sxs-lookup"><span data-stu-id="533d1-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="533d1-170">Použijte migrace Code First k aktualizaci schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="533d1-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="533d1-171">V tomto kurzu použijeme migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="533d1-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="533d1-172">Aktualizujte metodu počáteční hodnoty tak, aby poskytuje hodnotu pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="533d1-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="533d1-173">Otevřete soubor Migrations\Configuration.cs a přidání hodnocení pole pro každý objekt film.</span><span class="sxs-lookup"><span data-stu-id="533d1-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="533d1-174">Sestavte řešení a pak otevřete **Konzola správce balíčků** okna a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="533d1-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="533d1-175">`add-migration` Příkaz zjistí migraci framework sloužící ke zkoumání aktuálního modelu film s aktuální schéma film databáze a vytvořit nezbytného kódu migrace databáze do nového modelu.</span><span class="sxs-lookup"><span data-stu-id="533d1-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="533d1-176">AddRatingMig libovolný a slouží k názvu souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="533d1-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="533d1-177">Je užitečné pro krok migrace smysluplný název.</span><span class="sxs-lookup"><span data-stu-id="533d1-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="533d1-178">Po dokončení tohoto příkazu Visual Studio otevře soubor třídy, který definuje nový `DbMIgration` odvozené třídy a v `Up` metoda vidíte kód, který vytvoří nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="533d1-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="533d1-179">Sestavte řešení a potom zadejte příkaz "update-database" v **Konzola správce balíčků** okno.</span><span class="sxs-lookup"><span data-stu-id="533d1-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="533d1-180">Následující obrázek ukazuje výstup v **Konzola správce balíčků** okno (razítka data předřazení AddRatingMig bude jiný.)</span><span class="sxs-lookup"><span data-stu-id="533d1-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="533d1-181">Znovu spusťte aplikaci a přejděte na adresu URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="533d1-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="533d1-182">Zobrazí se nové pole hodnocení.</span><span class="sxs-lookup"><span data-stu-id="533d1-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="533d1-183">Klikněte **vytvořit nový** odkaz na přidání nové videa.</span><span class="sxs-lookup"><span data-stu-id="533d1-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="533d1-184">Všimněte si, že můžete přidat hodnocení.</span><span class="sxs-lookup"><span data-stu-id="533d1-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="533d1-186">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="533d1-186">Click **Create**.</span></span> <span data-ttu-id="533d1-187">Nové film, včetně hodnocení, se zobrazí v filmy výpis:</span><span class="sxs-lookup"><span data-stu-id="533d1-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="533d1-189">Měli byste také přidat `Rating` pole pro úpravy, podrobnosti a SearchIndex šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="533d1-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="533d1-190">Můžete zadat příkaz "update-database" v **Konzola správce balíčků** znovu okno a žádné změny by být provedeny, protože schéma neodpovídá modelu.</span><span class="sxs-lookup"><span data-stu-id="533d1-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="533d1-191">V této části jste viděli, jak můžete upravit objekty modelu a synchronizujte databázi s změny.</span><span class="sxs-lookup"><span data-stu-id="533d1-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="533d1-192">Také jste zjistili, způsob, jak naplnit nově vytvořenou databázi s ukázkovými daty, můžete zkusit scénáře.</span><span class="sxs-lookup"><span data-stu-id="533d1-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="533d1-193">V dalším kroku podíváme, jak můžete přidat do třídy modelu bohatší logiku ověření a povolit některé obchodní pravidla, která budou vynucena.</span><span class="sxs-lookup"><span data-stu-id="533d1-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="533d1-194">[Předchozí](examining-the-edit-methods-and-edit-view.md)
[další](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="533d1-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
