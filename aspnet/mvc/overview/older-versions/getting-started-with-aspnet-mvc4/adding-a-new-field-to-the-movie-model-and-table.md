---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Přidání nového pole do modelu a tabulky Movie | Dokumentace Microsoftu
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici tady, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, sledovat a ukázka mnohem jednodušší...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 0f9b659b67a9a62635091b1e87169bce1218281a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911410"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="525dc-104">Přidání nového pole do modelu a tabulky Movie</span><span class="sxs-lookup"><span data-stu-id="525dc-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="525dc-105">Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="525dc-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> > [!NOTE]
> > <span data-ttu-id="525dc-106">Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="525dc-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="525dc-107">Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="525dc-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="525dc-108">V této části použijete migrace Entity Framework Code First k migraci některých změn do tříd modelu, tak použití této změny do databáze.</span><span class="sxs-lookup"><span data-stu-id="525dc-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="525dc-109">Ve výchozím nastavení při použití platformy Entity Framework Code First automaticky vytvořit databázi, jako jste to udělali dříve v tomto kurzu Code First přidá tabulku do databáze pro sledování, zda je synchronizovaný s tříd modelu, které byly vygenerovány z schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="525dc-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="525dc-110">Pokud nejsou synchronizované, Entity Framework vyvolá chybu.</span><span class="sxs-lookup"><span data-stu-id="525dc-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="525dc-111">Díky tomu je snadněji sledovat problémy při vývoji, který může jinak pouze pro vás (pomocí skrytého chyby) v době běhu.</span><span class="sxs-lookup"><span data-stu-id="525dc-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="525dc-112">Nastavení migrace Code First pro změny modelu</span><span class="sxs-lookup"><span data-stu-id="525dc-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="525dc-113">Pokud používáte sadu Visual Studio 2012, dvakrát klikněte *Movies.mdf* souboru z Průzkumníku řešení otevřete nástroj pro databáze.</span><span class="sxs-lookup"><span data-stu-id="525dc-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="525dc-114">Visual Studio Express for Web se zobrazí Průzkumník databáze, že Visual Studio 2012 se zobrazí Průzkumník serveru.</span><span class="sxs-lookup"><span data-stu-id="525dc-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="525dc-115">Pokud používáte Visual Studio 2010, použijte Průzkumník objektů systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="525dc-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="525dc-116">V databázi nástroje (Průzkumník databáze, Průzkumníku serveru nebo Průzkumník objektů systému SQL Server), klikněte pravým tlačítkem na `MovieDBContext` a vyberte **odstranit** pro odpojení databáze filmů.</span><span class="sxs-lookup"><span data-stu-id="525dc-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="525dc-117">Přejděte zpět do Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="525dc-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="525dc-118">Klikněte pravým tlačítkem na *Movies.mdf* a vyberte možnost **odstranit** odebrání databáze filmů.</span><span class="sxs-lookup"><span data-stu-id="525dc-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="525dc-119">Vytvoření aplikace, abyste měli jistotu, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="525dc-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="525dc-120">Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet** a potom **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="525dc-120">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Přidat balíček Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="525dc-122">V **Konzola správce balíčků** v okna `PM>` řádku zadejte "Enable-migrace - ContextTypeName MvcMovie.Models.MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="525dc-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="525dc-123">**Povolení migrace** vytvoří příkaz (popsaný výš) *Configuration.cs* souboru v novém *migrace* složky.</span><span class="sxs-lookup"><span data-stu-id="525dc-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="525dc-124">Visual Studio otevře *Configuration.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="525dc-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="525dc-125">Nahradit `Seed` metodu *Configuration.cs* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="525dc-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="525dc-126">Klikněte pravým tlačítkem na řádku červená vlnovka pod `Movie` a vyberte **vyřešit** pak **pomocí** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="525dc-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="525dc-127">Tím se přidají následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="525dc-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="525dc-128">Kód volá první migrace `Seed` za každou migraci (tedy volání **aktualizace databáze** v konzole Správce balíčků), a tato metoda aktualizuje řádky, které již byl vložen a vloží je, pokud jsou dosud neexistují.</span><span class="sxs-lookup"><span data-stu-id="525dc-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="525dc-129">**Stisknutím klávesy CTRL-SHIFT-B a sestavte projekt.** (Následující kroky se nezdaří, pokud vaše není v tomto okamžiku sestavení.)</span><span class="sxs-lookup"><span data-stu-id="525dc-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="525dc-130">Dalším krokem je vytvoření `DbMigration` třídu pro počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="525dc-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="525dc-131">Tato migrace na vytvoří novou databázi, proto můžete odstranit *movie.mdf* souboru v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="525dc-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="525dc-132">V **Konzola správce balíčků** okno, zadejte příkaz "Přidat migrace počáteční" k vytvoření počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="525dc-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="525dc-133">Název "Počáteční" libovolný a slouží k pojmenování souboru migrace vytvořili.</span><span class="sxs-lookup"><span data-stu-id="525dc-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="525dc-134">Migrace Code First vytvoří další soubor třídy v *migrace* složky (s názvem *{DateStamp}\_Initial.cs* ), a tato třída obsahuje kód, který vytvoří schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="525dc-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="525dc-135">Název souboru migraci je pevně předem s časovým razítkem Nápověda k řazení.</span><span class="sxs-lookup"><span data-stu-id="525dc-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="525dc-136">Zkontrolujte *{DateStamp}\_Initial.cs* souboru, obsahuje pokyny k vytvoření tabulky filmy pro databáze filmů.</span><span class="sxs-lookup"><span data-stu-id="525dc-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="525dc-137">Při aktualizaci databáze v pokynech níže to *{DateStamp}\_Initial.cs* souboru spustí a vytvořit schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="525dc-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="525dc-138">Pak bude **počáteční hodnoty** metoda spustí k naplnění databáze se testovací data.</span><span class="sxs-lookup"><span data-stu-id="525dc-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="525dc-139">V **Konzola správce balíčků**, zadejte příkaz "update databáze" k vytvoření databáze a spusťte **počáteční hodnoty** metody.</span><span class="sxs-lookup"><span data-stu-id="525dc-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="525dc-140">Pokud dojde k chybě, která určuje již existuje a nedá se vytvořit tabulku, je možné, že jste spustili aplikaci po odstranění databáze a předtím, než jste spustili `update-database`.</span><span class="sxs-lookup"><span data-stu-id="525dc-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="525dc-141">V takovém případě odstranit *Movies.mdf* soubor znovu a zkuste to znovu `update-database` příkazu.</span><span class="sxs-lookup"><span data-stu-id="525dc-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="525dc-142">Pokud stále dojde k chybě, odstraňte složku migrace a obsah, pak spusťte v pokynech v horní části této stránky (to je odstranit *Movies.mdf* souborů pak přejděte k povolení migrace).</span><span class="sxs-lookup"><span data-stu-id="525dc-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="525dc-143">Spusťte aplikaci a přejděte */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="525dc-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="525dc-144">Počáteční data se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="525dc-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="525dc-145">Přidání vlastnosti do hodnocení filmů modelu</span><span class="sxs-lookup"><span data-stu-id="525dc-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="525dc-146">Začněte přidáním nového `Rating` vlastnost ke stávající `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="525dc-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="525dc-147">Otevřít *Models\Movie.cs* a přidejte `Rating` vlastnost podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="525dc-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="525dc-148">Kompletní `Movie` třídy teď vypadá jako v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="525dc-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="525dc-149">Sestavení aplikace pomocí **sestavení** &gt; **sestavení film** nabídky příkazu, nebo stisknutím klávesy CTRL-SHIFT-B.</span><span class="sxs-lookup"><span data-stu-id="525dc-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="525dc-150">Teď, když jste aktualizovali `Model` třídy, je také potřeba aktualizovat *\Views\Movies\Index.cshtml* a *\Views\Movies\Create.cshtml* zobrazení šablon zobrazíte nové `Rating`vlastností v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="525dc-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="525dc-151">Otevřít<em>\Views\Movies\Index.cshtml</em> a přidejte `<th>Rating</th>` hned za záhlaví sloupce <strong>cena</strong> sloupec.</span><span class="sxs-lookup"><span data-stu-id="525dc-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="525dc-152">Pak přidejte `<td>` sloupec blíží ke konci šablonu k vykreslení `@item.Rating` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="525dc-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="525dc-153">Níže je co aktualizované <em>Index.cshtml</em> zobrazit šablonu vypadá jako:</span><span class="sxs-lookup"><span data-stu-id="525dc-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="525dc-154">Dále otevřete *\Views\Movies\Create.cshtml* a přidejte následující kód na konci formuláře.</span><span class="sxs-lookup"><span data-stu-id="525dc-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="525dc-155">Tím zkopírujete textové pole tak, aby hodnocení můžete zadat, když se vytvoří nová videa.</span><span class="sxs-lookup"><span data-stu-id="525dc-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="525dc-156">Teď když jste aktualizovali kód aplikace pro podporu nového `Rating` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="525dc-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="525dc-157">Nyní spusťte aplikaci a přejděte */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="525dc-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="525dc-158">Když toto provedete, ale zobrazí se vám některé z následujících chyb:</span><span class="sxs-lookup"><span data-stu-id="525dc-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="525dc-159">Tato chyba se zobrazuje, protože aktualizovaný `Movie` třídy modelu v aplikaci je nyní liší od schématu `Movie` tabulky existující databáze.</span><span class="sxs-lookup"><span data-stu-id="525dc-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="525dc-160">(Neexistuje žádný `Rating` sloupec v tabulce databáze.)</span><span class="sxs-lookup"><span data-stu-id="525dc-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="525dc-161">Řešení chyby několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="525dc-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="525dc-162">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi založené na nové schéma třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="525dc-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="525dc-163">Tento přístup je příliš pohodlné při aktivním vývoji pro testovací databázi. umožňuje rychlý rozvoj schématu modelu a databáze společně.</span><span class="sxs-lookup"><span data-stu-id="525dc-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="525dc-164">Nevýhodou, je však dojít ke ztrátě existujících dat v databázi, tak můžete *není* chcete použít tento postup u provozní databáze!</span><span class="sxs-lookup"><span data-stu-id="525dc-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="525dc-165">Použití inicializátoru automaticky naplnit databázi daty testu je často produktivní způsob, jak vyvíjet aplikace.</span><span class="sxs-lookup"><span data-stu-id="525dc-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="525dc-166">Další informace o databázi inicializátory Entity Framework naleznete v tématu Petr Dykstra [kurz ASP.NET MVC a Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="525dc-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="525dc-167">Explicitně upravte schéma stávající databázi tak, aby odpovídalo tříd modelu.</span><span class="sxs-lookup"><span data-stu-id="525dc-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="525dc-168">Výhodou tohoto přístupu je, že zachováte vaše data.</span><span class="sxs-lookup"><span data-stu-id="525dc-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="525dc-169">Můžete tuto změnu provést buď ručně, nebo tak, že vytvoříte databázi změnit skript.</span><span class="sxs-lookup"><span data-stu-id="525dc-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="525dc-170">Pomocí migrace Code First aktualizovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="525dc-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="525dc-171">Pro účely tohoto kurzu používáme migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="525dc-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="525dc-172">Aktualizujte metodu počáteční hodnoty tak, aby poskytuje hodnoty pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="525dc-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="525dc-173">Otevřete soubor Migrations\Configuration.cs a přidat hodnocení pole do každé video.</span><span class="sxs-lookup"><span data-stu-id="525dc-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="525dc-174">Sestavte řešení a pak otevřete **Konzola správce balíčků** okna a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="525dc-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="525dc-175">`add-migration` Příkaz říká rozhraní framework migrace sloužící ke zkoumání aktuální model filmů s aktuální schéma databáze filmů a vytvářet kód potřebné k migraci databáze na nový model.</span><span class="sxs-lookup"><span data-stu-id="525dc-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="525dc-176">AddRatingMig je volitelný a slouží k pojmenování souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="525dc-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="525dc-177">Je vhodné použít smysluplný název kroku migrace.</span><span class="sxs-lookup"><span data-stu-id="525dc-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="525dc-178">Po dokončení tohoto příkazu, Visual Studio otevře soubor třídy, která definuje nový `DbMIgration` odvozené třídy a `Up` metoda se zobrazí kód, který vytvoří nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="525dc-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="525dc-179">Sestavte řešení a potom zadejte příkaz "aktualizace databáze" v **Konzola správce balíčků** okna.</span><span class="sxs-lookup"><span data-stu-id="525dc-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="525dc-180">Následující obrázek znázorňuje výstup v **Konzola správce balíčků** okno (časové razítko předřazení AddRatingMig bude lišit.)</span><span class="sxs-lookup"><span data-stu-id="525dc-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="525dc-181">Znovu spusťte aplikaci a přejděte na adresu URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="525dc-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="525dc-182">Uvidíte nové pole hodnocení.</span><span class="sxs-lookup"><span data-stu-id="525dc-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="525dc-183">Klikněte na tlačítko **vytvořit nový** odkaz na přidání nového videa.</span><span class="sxs-lookup"><span data-stu-id="525dc-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="525dc-184">Všimněte si, že můžete přidat hodnocení.</span><span class="sxs-lookup"><span data-stu-id="525dc-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="525dc-186">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="525dc-186">Click **Create**.</span></span> <span data-ttu-id="525dc-187">Tento nový film, včetně hodnocení, zobrazí se nově ve výpisu:</span><span class="sxs-lookup"><span data-stu-id="525dc-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="525dc-189">Měli byste také přidat `Rating` pole pro úpravy, podrobnosti a SearchIndex zobrazit šablony.</span><span class="sxs-lookup"><span data-stu-id="525dc-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="525dc-190">Můžete například zadat příkaz "aktualizace databáze" v **Konzola správce balíčků** znovu okno a žádné změny byste nastavit, protože schéma odpovídá schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="525dc-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="525dc-191">V této části jste viděli, jak můžete upravit objekty modelu a udržovat synchronizované s změny databáze.</span><span class="sxs-lookup"><span data-stu-id="525dc-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="525dc-192">Také jste se naučili způsob, jak naplnit nově vytvořenou databázi s ukázkovými daty, takže si můžete vyzkoušet scénáře.</span><span class="sxs-lookup"><span data-stu-id="525dc-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="525dc-193">V dalším kroku Podívejme se na jak můžete přidat bohatší logiku ověřování na třídy modelu a povolit některé obchodní pravidla, která budou vynucena.</span><span class="sxs-lookup"><span data-stu-id="525dc-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="525dc-194">[Předchozí](examining-the-edit-methods-and-edit-view.md)
> [další](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="525dc-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
