---
title: Přidat nové pole do stránky v ASP.NET Core Razor
author: rick-anderson
description: Ukazuje, jak přidat nové pole do stránky Razor pomocí Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: d6d59ff336095e2f1b8b2e9a0338b7791605ad7a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010894"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="bb13f-103">Přidat nové pole do stránky v ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="bb13f-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="bb13f-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bb13f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bb13f-105">V této části použijete [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migrace Code First pro přidání nového pole do modelu a migraci, které změnit na databázi.</span><span class="sxs-lookup"><span data-stu-id="bb13f-105">In this section you use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="bb13f-106">Při použití automaticky vytvořit databázi, Code First EF Code First:</span><span class="sxs-lookup"><span data-stu-id="bb13f-106">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="bb13f-107">Přidá do databáze, kterou chcete sledovat, jestli je schéma databáze synchronizované s tříd modelu, které byly vygenerovány z tabulky.</span><span class="sxs-lookup"><span data-stu-id="bb13f-107">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="bb13f-108">Pokud nejsou synchronizované s databáze třídy modelu, EF vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="bb13f-108">If the model classes aren't in sync with the DB, EF throws an exception.</span></span> 

<span data-ttu-id="bb13f-109">Automatické ověření schématu a model synchronizované usnadňuje vyhledání potíží nekonzistentní databáze nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="bb13f-109">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="bb13f-110">Přidání vlastnosti do hodnocení filmů modelu</span><span class="sxs-lookup"><span data-stu-id="bb13f-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="bb13f-111">Otevřít *Models/Movie.cs* a přidejte `Rating` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="bb13f-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

<span data-ttu-id="bb13f-112">Vytvořte aplikaci (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="bb13f-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="bb13f-113">Upravit *Pages/Movies/Index.cshtml*a přidejte `Rating` pole:</span><span class="sxs-lookup"><span data-stu-id="bb13f-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="bb13f-114">Přidat `Rating` pole na stránkách Delete a podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bb13f-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="bb13f-115">Aktualizace *Create.cshtml* s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="bb13f-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="bb13f-116">Můžete zkopírovat a Vložit předchozí `<div>` aktualizujte pole elementu a umožňují nápovědy technologie intelliSense.</span><span class="sxs-lookup"><span data-stu-id="bb13f-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="bb13f-117">Technologie IntelliSense funguje s [pomocných rutin značek](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="bb13f-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Vývojář napsal písmeno R pro hodnotu atributu ASP-pro druhý popisek prvku zobrazení.](new-field/_static/cr.png)

<span data-ttu-id="bb13f-121">Následující kód ukazuje *Create.cshtml* s `Rating` pole:</span><span class="sxs-lookup"><span data-stu-id="bb13f-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="bb13f-122">Přidat `Rating` pole na stránce Upravit.</span><span class="sxs-lookup"><span data-stu-id="bb13f-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="bb13f-123">Aplikace nebude fungovat, dokud databáze je aktualizováno, aby zahrnovalo nové pole.</span><span class="sxs-lookup"><span data-stu-id="bb13f-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="bb13f-124">Je-li spustit, vyvolá aplikaci `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="bb13f-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="bb13f-125">Tato chyba je způsobena se liší od schématu tabulky Movie databáze třídy modelu aktualizované video.</span><span class="sxs-lookup"><span data-stu-id="bb13f-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="bb13f-126">(Neexistuje žádný `Rating` sloupec v tabulce databáze.)</span><span class="sxs-lookup"><span data-stu-id="bb13f-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="bb13f-127">Řešení chyby několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="bb13f-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="bb13f-128">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi pomocí nové schéma třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="bb13f-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="bb13f-129">Tento přístup je vhodný v rané fázi vývojového cyklu; umožňuje rychlý rozvoj schématu modelu a databáze společně.</span><span class="sxs-lookup"><span data-stu-id="bb13f-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="bb13f-130">Nevýhodou je, dojít ke ztrátě existujících dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="bb13f-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="bb13f-131">Nechcete tuto metodu použijte u provozní databáze.</span><span class="sxs-lookup"><span data-stu-id="bb13f-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="bb13f-132">Vyřazení databáze na změny schématu a pomocí inicializátoru automaticky naplnit databázi daty testu je často produktivní způsob, jak vyvíjet aplikace.</span><span class="sxs-lookup"><span data-stu-id="bb13f-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="bb13f-133">Explicitně upravte schéma stávající databázi tak, aby odpovídalo tříd modelu.</span><span class="sxs-lookup"><span data-stu-id="bb13f-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="bb13f-134">Výhodou tohoto přístupu je, že zachováte vaše data.</span><span class="sxs-lookup"><span data-stu-id="bb13f-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="bb13f-135">Můžete tuto změnu provést buď ručně, nebo tak, že vytvoříte databázi změnit skript.</span><span class="sxs-lookup"><span data-stu-id="bb13f-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="bb13f-136">Pomocí migrace Code First aktualizovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="bb13f-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="bb13f-137">V tomto kurzu pomocí migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="bb13f-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="bb13f-138">Aktualizace `SeedData` třídy tak, že poskytuje hodnoty pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="bb13f-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="bb13f-139">Ukázka změnu je uveden níže, ale budete chtít tuto změnu pro každou `new Movie` bloku.</span><span class="sxs-lookup"><span data-stu-id="bb13f-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bb13f-140">Zobrazit [dokončit soubor SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="bb13f-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bb13f-141">Zobrazit [dokončit soubor SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="bb13f-141">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span></span>

::: moniker-end

<span data-ttu-id="bb13f-142">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="bb13f-142">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="bb13f-143">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="bb13f-143">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="bb13f-144">V konzole PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="bb13f-144">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="bb13f-145">`Add-Migration` Příkaz říká rozhraní framework:</span><span class="sxs-lookup"><span data-stu-id="bb13f-145">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="bb13f-146">Porovnání `Movie` modelů s `Movie` schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="bb13f-146">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="bb13f-147">Vytvoření kódu pro migraci schématu databáze na nový model.</span><span class="sxs-lookup"><span data-stu-id="bb13f-147">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="bb13f-148">Název "Hodnocení" je volitelný a slouží k pojmenování souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="bb13f-148">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="bb13f-149">Je vhodné použít smysluplný název souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="bb13f-149">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="bb13f-150">Při odstranění všech záznamů v databázi, bude inicializátoru naplnit databáze a zahrnout `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="bb13f-150">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="bb13f-151">To lze provést pomocí odstranit odkazy v prohlížeči nebo z [Průzkumník objektů systému Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="bb13f-151">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="bb13f-152">Pokud chcete odstranit databázi z SSOX:</span><span class="sxs-lookup"><span data-stu-id="bb13f-152">To delete the database from SSOX:</span></span>

* <span data-ttu-id="bb13f-153">Vyberte databázi v SSOX.</span><span class="sxs-lookup"><span data-stu-id="bb13f-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="bb13f-154">Klikněte pravým tlačítkem na databázi a vyberte *odstranit*.</span><span class="sxs-lookup"><span data-stu-id="bb13f-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="bb13f-155">Zkontrolujte **ukončete stávající připojení**.</span><span class="sxs-lookup"><span data-stu-id="bb13f-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="bb13f-156">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb13f-156">Select **OK**.</span></span>
* <span data-ttu-id="bb13f-157">V [PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizovat databázi:</span><span class="sxs-lookup"><span data-stu-id="bb13f-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="bb13f-158">Spusťte aplikaci a ověřit, je možné vytvořit/upravit/zobrazit videa s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="bb13f-158">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="bb13f-159">Pokud databáze není nasazený, zastavte službu IIS Express a pak spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bb13f-159">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bb13f-160">[Předchozí: Přidání vyhledávací funkce](xref:tutorials/razor-pages/search)
> [Další: Přidání ověření](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="bb13f-160">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
