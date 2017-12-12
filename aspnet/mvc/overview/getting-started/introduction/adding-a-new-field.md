---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: "Přidání nové pole | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: c10eb343259b58052fd1f2411dbdc2196eafc858
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/19/2017
---
<a name="adding-a-new-field"></a><span data-ttu-id="dc50f-102">Přidání nové pole</span><span class="sxs-lookup"><span data-stu-id="dc50f-102">Adding a New Field</span></span>
====================
<span data-ttu-id="dc50f-103">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="dc50f-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="dc50f-104">V této části používáte migrace Code First Entity Framework pro migraci některé změny do třídy modelu tak, že použití této změny do databáze.</span><span class="sxs-lookup"><span data-stu-id="dc50f-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="dc50f-105">Ve výchozím nastavení při použití Entity Framework Code First automaticky vytvořit databázi, stejně jako dříve v tomto kurzu Code First přidá tabulku k databázi chcete-li sledovat, jestli je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="dc50f-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="dc50f-106">Pokud nejsou synchronizované, rozhraní Entity Framework vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="dc50f-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="dc50f-107">To usnadňuje sledovat problémy v čase vývoj, který může jinak pouze pro vás (podle skrytého chyby) v době běhu.</span><span class="sxs-lookup"><span data-stu-id="dc50f-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="dc50f-108">Nastavení migrace Code First pro změny modelu</span><span class="sxs-lookup"><span data-stu-id="dc50f-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="dc50f-109">Přejděte do Průzkumníka řešení.</span><span class="sxs-lookup"><span data-stu-id="dc50f-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="dc50f-110">Klikněte pravým tlačítkem na *Movies.mdf* soubor a vyberte **odstranit** k odebrání databáze filmy.</span><span class="sxs-lookup"><span data-stu-id="dc50f-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="dc50f-111">Pokud nevidíte *Movies.mdf* souboru, klikněte na **zobrazit všechny soubory** vidíte níže v osnově červenou ikonu.</span><span class="sxs-lookup"><span data-stu-id="dc50f-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="dc50f-112">Sestavení aplikace a ujistěte se, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="dc50f-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="dc50f-113">Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet** a potom **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="dc50f-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Přidat Pack Man](adding-a-new-field/_static/image2.png)

<span data-ttu-id="dc50f-115">V **Konzola správce balíčků** v okno `PM>` řádku zadejte</span><span class="sxs-lookup"><span data-stu-id="dc50f-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="dc50f-116">Příkaz enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="dc50f-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="dc50f-117">**Enable-Migrations** příkaz (viz výše) vytvoří *Configuration.cs* soubor v nové *migrace* složky.</span><span class="sxs-lookup"><span data-stu-id="dc50f-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="dc50f-118">Otevře se Visual Studio *Configuration.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="dc50f-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="dc50f-119">Nahraďte `Seed` metoda v *Configuration.cs* soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="dc50f-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="dc50f-120">Pozastavte ukazatel myši nad red vlnovkou řádku pod `Movie` a klikněte na tlačítko `Show Potential Fixes` a pak klikněte na **pomocí** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="dc50f-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="dc50f-121">Díky tomu přidá následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="dc50f-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="dc50f-122">Kód volání první migrace `Seed` metoda po každé migraci (tedy volání **update-database** v konzole Správce balíčků), a tato metoda aktualizace řádků, které již byl vložen a vloží je, pokud jejich Nemáte ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="dc50f-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="dc50f-123">[AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metoda v následujícím kódu provede operaci "upsert":</span><span class="sxs-lookup"><span data-stu-id="dc50f-123">The [AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="dc50f-124">Protože [počáteční hodnoty](https://msdn.microsoft.com/en-us/library/hh829453(v=vs.103).aspx) metoda se spouští se každých migrace, data, nelze právě vložit, protože řádky, které se pokoušíte přidat bude po první migrace, která vytváří databázi již existovat.</span><span class="sxs-lookup"><span data-stu-id="dc50f-124">Because the [Seed](https://msdn.microsoft.com/en-us/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="dc50f-125">"[Upsert](http://en.wikipedia.org/wiki/Upsert)" operaci brání chyb, které by mohlo dojít, pokud se pokusíte vložit řádek, který již existuje, ale přepíše všechny změny dat, která může provedení při testování aplikace.</span><span class="sxs-lookup"><span data-stu-id="dc50f-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="dc50f-126">Pro testovací data v některé tabulky nemusí Chcete to tak proběhlo: v některých případech při změně dat při testování vaše změny zůstat po aktualizace databáze.</span><span class="sxs-lookup"><span data-stu-id="dc50f-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="dc50f-127">V takovém případě budete chtít provést operace podmíněného insert: Vložit řádek pouze v případě, že ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="dc50f-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
>   
> <span data-ttu-id="dc50f-128">První parametr předaný [AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metoda určuje vlastnost, která má použít pro kontrolu, pokud řádek již existuje.</span><span class="sxs-lookup"><span data-stu-id="dc50f-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="dc50f-129">Pro testovací data film, který zadáte `Title` vlastnost lze použít pro tento účel, protože v nadpisu každé v seznamu je jedinečný:</span><span class="sxs-lookup"><span data-stu-id="dc50f-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="dc50f-130">Tento kód předpokládá, že titiles jsou jedinečné.</span><span class="sxs-lookup"><span data-stu-id="dc50f-130">This code assumes that titiles are unique.</span></span> <span data-ttu-id="dc50f-131">Pokud chcete ručně přidat duplicitní název, získáte následující výjimka při příštím provedení migrace.</span><span class="sxs-lookup"><span data-stu-id="dc50f-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
>   
>  <span data-ttu-id="dc50f-132">*Sekvence obsahuje více než jeden element.*</span><span class="sxs-lookup"><span data-stu-id="dc50f-132">*Sequence contains more than one element*</span></span>  
>   
> <span data-ttu-id="dc50f-133">Další informace o [AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodu, najdete v části [postará s EF 4.3 AddOrUpdate metoda](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...</span><span class="sxs-lookup"><span data-stu-id="dc50f-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>


<span data-ttu-id="dc50f-134">**Stisknutím kombinace kláves CTRL-SHIFT-B a tím projekt sestavit.** (Následující kroky se nezdaří, pokud není v tomto okamžiku sestavení.)</span><span class="sxs-lookup"><span data-stu-id="dc50f-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="dc50f-135">Dalším krokem je vytvoření `DbMigration` třídu pro počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="dc50f-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="dc50f-136">Tato migrace vytvoří novou databázi, která je důvod, proč můžete odstranit *movie.mdf* soubor v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="dc50f-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="dc50f-137">V **Konzola správce balíčků** okno, zadejte příkaz `add-migration Initial` vytvořit počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="dc50f-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="dc50f-138">Název "Počáteční" libovolný a slouží k názvu souboru migrace vytvořili.</span><span class="sxs-lookup"><span data-stu-id="dc50f-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="dc50f-139">Migrace Code First vytvoří další soubor třídy ve *migrace* složky (s názvem *{DateStamp}\_Initial.cs* ), a tato třída obsahuje kód, který vytvoří schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="dc50f-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="dc50f-140">Název souboru migrace je předem vyřešili s časovým razítkem usnadní řazení.</span><span class="sxs-lookup"><span data-stu-id="dc50f-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="dc50f-141">Zkontrolujte *{DateStamp}\_Initial.cs* souboru, obsahuje pokyny pro vytvoření `Movies` tabulku pro film databáze.</span><span class="sxs-lookup"><span data-stu-id="dc50f-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="dc50f-142">Při aktualizaci databáze v podle pokynů níže, tento *{DateStamp}\_Initial.cs* soubor bude spustit a vytvořit schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="dc50f-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="dc50f-143">Pak se **počáteční hodnoty** metoda spustí k naplnění databáze s testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="dc50f-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="dc50f-144">V **Konzola správce balíčků**, zadejte příkaz `update-database` vytvořit databázi a spustit `Seed` metoda.</span><span class="sxs-lookup"><span data-stu-id="dc50f-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="dc50f-145">Pokud dojde k chybě, která označuje tabulka již existuje a nelze vytvořit, je možné, že jste spustili aplikaci po odstranění databáze a před provedení `update-database`.</span><span class="sxs-lookup"><span data-stu-id="dc50f-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="dc50f-146">V takovém případě odstraňte *Movies.mdf* znovu a zkuste zopakovat `update-database` příkaz.</span><span class="sxs-lookup"><span data-stu-id="dc50f-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="dc50f-147">Pokud stále dojde k chybě, odstraňte složku migrations a obsah, pak spusťte s pokyny uvedenými v horní části této stránky (která je odstranit *Movies.mdf* soubor pak pokračujte Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="dc50f-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="dc50f-148">Pokud se pořád zobrazí chyba, otevřete Průzkumník objektů systému SQL Server a odebrat databázi ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="dc50f-148">If you still get an eror, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="dc50f-149">Spusťte aplikaci a přejděte do */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="dc50f-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="dc50f-150">Se zobrazí data počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dc50f-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="dc50f-151">Přidání vlastnosti hodnocení filmu modelu</span><span class="sxs-lookup"><span data-stu-id="dc50f-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="dc50f-152">Začněte přidáním nové `Rating` vlastnost, která má existující `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="dc50f-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="dc50f-153">Otevřete *Models\Movie.cs* souboru a přidejte `Rating` vlastnost podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="dc50f-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="dc50f-154">Kompletní `Movie` třídy nyní vypadá podobně jako následující kód:</span><span class="sxs-lookup"><span data-stu-id="dc50f-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="dc50f-155">Sestavení aplikace (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="dc50f-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="dc50f-156">Protože jste přidali nové pole do `Movie` třídy, je také potřeba aktualizovat vazby *seznamu povolených* , tato vlastnost budou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="dc50f-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="dc50f-157">Aktualizace `bind` atribut pro `Create` a `Edit` akce metody pro zahrnutí `Rating` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="dc50f-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="dc50f-158">Je také potřeba aktualizovat zobrazit šablony, aby bylo možné zobrazit, vytvořit a upravit nové `Rating` vlastnost v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="dc50f-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="dc50f-159">Otevřete *\Views\Movies\Index.cshtml* souboru a přidejte `<th>Rating</th>` právě po záhlaví sloupce **cena** sloupce.</span><span class="sxs-lookup"><span data-stu-id="dc50f-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="dc50f-160">Pak přidejte `<td>` sloupce u konce šablony k vykreslení `@item.Rating` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dc50f-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="dc50f-161">Níže je co aktualizované *Index.cshtml* zobrazit šablonu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="dc50f-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="dc50f-162">Dále otevřete *\Views\Movies\Create.cshtml* souboru a přidejte `Rating` pole s následující kód highlighed.</span><span class="sxs-lookup"><span data-stu-id="dc50f-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighed markup.</span></span> <span data-ttu-id="dc50f-163">To vykresluje textové pole tak, aby při vytváření nové film můžete zadat hodnocení.</span><span class="sxs-lookup"><span data-stu-id="dc50f-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="dc50f-164">Nyní po aktualizaci kódu aplikace, které podporují nové `Rating` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dc50f-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="dc50f-165">Spusťte aplikaci a přejděte do */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="dc50f-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="dc50f-166">Když to uděláte, ale se zobrazí jednu z těchto chyb:</span><span class="sxs-lookup"><span data-stu-id="dc50f-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="dc50f-167">Model zálohování kontext 'MovieDBContext' se od vytvoření databáze změnil.</span><span class="sxs-lookup"><span data-stu-id="dc50f-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="dc50f-168">Zvažte použití migrace Code First k aktualizaci databáze (https://go.microsoft.com/fwlink/?LinkId=238269).</span><span class="sxs-lookup"><span data-stu-id="dc50f-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="dc50f-169">Protože se tato chyba zobrazuje aktualizovaný `Movie` třídy modelu v aplikaci je nyní liší od schématu `Movie` tabulky existující databáze.</span><span class="sxs-lookup"><span data-stu-id="dc50f-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="dc50f-170">(Není žádná `Rating` sloupec v tabulce databáze.)</span><span class="sxs-lookup"><span data-stu-id="dc50f-170">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="dc50f-171">Řešení chyby několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="dc50f-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="dc50f-172">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi na základě nové třídy schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="dc50f-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="dc50f-173">Tento přístup je velmi praktické již v rané fázi v cyklu vývoje, pokud byste active vývoj pro testovací databázi. umožňuje rychle společně momentální schéma modelu a databáze.</span><span class="sxs-lookup"><span data-stu-id="dc50f-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="dc50f-174">Nevýhodou, ale je, že přijdete o stávající data v databázi – tak můžete *nemáte* chcete použít tuto metodu na produkční databázi!</span><span class="sxs-lookup"><span data-stu-id="dc50f-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="dc50f-175">Pomocí inicializátoru automaticky počáteční hodnoty databázi s daty test je často produktivní způsob, jak vyvíjet aplikace.</span><span class="sxs-lookup"><span data-stu-id="dc50f-175">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="dc50f-176">Další informace o inicializátory databáze Entity Framework najdete v tématu [kurz ASP.NET MVC nebo Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="dc50f-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="dc50f-177">Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="dc50f-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="dc50f-178">Výhodou tohoto přístupu je, že zachováte data.</span><span class="sxs-lookup"><span data-stu-id="dc50f-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="dc50f-179">Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.</span><span class="sxs-lookup"><span data-stu-id="dc50f-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="dc50f-180">Použijte migrace Code First k aktualizaci schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="dc50f-180">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="dc50f-181">V tomto kurzu použijeme migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="dc50f-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="dc50f-182">Aktualizujte metodu počáteční hodnoty tak, aby poskytuje hodnotu pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="dc50f-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="dc50f-183">Otevřete soubor Migrations\Configuration.cs a přidání hodnocení pole pro každý objekt film.</span><span class="sxs-lookup"><span data-stu-id="dc50f-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="dc50f-184">Sestavte řešení a pak otevřete **Konzola správce balíčků** okna a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dc50f-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="dc50f-185">`add-migration` Příkaz zjistí migraci framework sloužící ke zkoumání aktuálního modelu film s aktuální schéma film databáze a vytvořit nezbytného kódu migrace databáze do nového modelu.</span><span class="sxs-lookup"><span data-stu-id="dc50f-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="dc50f-186">Název *hodnocení* libovolný a slouží k názvu souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="dc50f-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="dc50f-187">Je užitečné pro krok migrace smysluplný název.</span><span class="sxs-lookup"><span data-stu-id="dc50f-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="dc50f-188">Po dokončení tohoto příkazu Visual Studio otevře soubor třídy, který definuje nový `DbMIgration` odvozené třídy a v `Up` metoda vidíte kód, který vytvoří nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="dc50f-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="dc50f-189">Sestavte řešení a potom zadejte `update-database` v příkazu **Konzola správce balíčků** okno.</span><span class="sxs-lookup"><span data-stu-id="dc50f-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="dc50f-190">Následující obrázek ukazuje výstup v **Konzola správce balíčků** okno (datum razítko předřazení *hodnocení* se budou lišit.)</span><span class="sxs-lookup"><span data-stu-id="dc50f-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="dc50f-191">Znovu spusťte aplikaci a přejděte na adresu URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="dc50f-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="dc50f-192">Zobrazí se nové pole hodnocení.</span><span class="sxs-lookup"><span data-stu-id="dc50f-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="dc50f-193">Klikněte **vytvořit nový** odkaz na přidání nové videa.</span><span class="sxs-lookup"><span data-stu-id="dc50f-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="dc50f-194">Všimněte si, že můžete přidat hodnocení.</span><span class="sxs-lookup"><span data-stu-id="dc50f-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="dc50f-196">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="dc50f-196">Click **Create**.</span></span> <span data-ttu-id="dc50f-197">Nové film, včetně hodnocení, se zobrazí v filmy výpis:</span><span class="sxs-lookup"><span data-stu-id="dc50f-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="dc50f-199">Teď, když na projekt používá migrace, nebudete muset odpojení databáze, když přidáte nové pole nebo jinak aktualizovat schéma.</span><span class="sxs-lookup"><span data-stu-id="dc50f-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="dc50f-200">V další části jsme budete provést další změny schématu a aktualizaci databáze pomocí migrace.</span><span class="sxs-lookup"><span data-stu-id="dc50f-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="dc50f-201">Měli byste také přidat `Rating` pole pro úpravy, podrobnosti a odstraňování šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dc50f-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="dc50f-202">Můžete zadat příkaz "update-database" v **Konzola správce balíčků** znovu okno a žádný kód migrace by spustit, protože schéma neodpovídá modelu.</span><span class="sxs-lookup"><span data-stu-id="dc50f-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="dc50f-203">Ale systémem "update-database" se spustí `Seed` metoda znovu, a pokud jste změnili počáteční hodnoty dat, změny budou ztraceny, protože `Seed` metoda upserts data.</span><span class="sxs-lookup"><span data-stu-id="dc50f-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="dc50f-204">Další informace o `Seed` metoda v tní Dykstra oblíbených [kurz ASP.NET MVC nebo Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="dc50f-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="dc50f-205">V této části jste viděli, jak můžete upravit objekty modelu a synchronizujte databázi s změny.</span><span class="sxs-lookup"><span data-stu-id="dc50f-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="dc50f-206">Také jste zjistili, způsob, jak naplnit nově vytvořenou databázi s ukázkovými daty, můžete zkusit scénáře.</span><span class="sxs-lookup"><span data-stu-id="dc50f-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="dc50f-207">To se právě rychlý úvod do Code First najdete v tématu [vytváření datového modelu Entity Framework pro aplikaci ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Podrobnější kurz týkající se předmět.</span><span class="sxs-lookup"><span data-stu-id="dc50f-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="dc50f-208">V dalším kroku podíváme, jak můžete přidat do třídy modelu bohatší logiku ověření a povolit některé obchodní pravidla, která budou vynucena.</span><span class="sxs-lookup"><span data-stu-id="dc50f-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dc50f-209">[Předchozí](adding-search.md)
[další](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="dc50f-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
