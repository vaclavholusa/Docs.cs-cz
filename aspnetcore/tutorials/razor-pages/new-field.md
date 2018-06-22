---
title: Přidat nové pole na stránku Razor v ASP.NET Core
author: rick-anderson
description: Ukazuje, jak přidat nové pole na stránku Razor základní Entity Framework
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: d9bf8c7cea20bf38aacf432465d7b33514bcd64d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277290"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="44159-103">Přidat nové pole na stránku Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44159-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="44159-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="44159-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="44159-105">V této části můžete použít [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migrace Code First a přidat nové pole do modelu migraci, které změnit na databázi.</span><span class="sxs-lookup"><span data-stu-id="44159-105">In this section you use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="44159-106">Při použití automaticky vytvořit databázi, Code First EF Code First:</span><span class="sxs-lookup"><span data-stu-id="44159-106">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="44159-107">Přidá tabulku k databázi a sleduje, zda je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="44159-107">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="44159-108">Pokud nejsou synchronizované s databáze třídy modelu, EF vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="44159-108">If the model classes aren't in sync with the DB, EF throws an exception.</span></span> 

<span data-ttu-id="44159-109">Automatické ověření schématu nebo modelu synchronizované usnadňuje vyhledání problémy nekonzistentní databáze nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="44159-109">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="44159-110">Přidání vlastnosti hodnocení filmu modelu</span><span class="sxs-lookup"><span data-stu-id="44159-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="44159-111">Otevřete *Models/Movie.cs* souboru a přidejte `Rating` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="44159-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="44159-112">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="44159-112">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="44159-113">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="44159-113">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span></span>

::: moniker-end

<span data-ttu-id="44159-114">Sestavení aplikace (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="44159-114">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="44159-115">Upravit *Pages/Movies/Index.cshtml*a přidejte `Rating` pole:</span><span class="sxs-lookup"><span data-stu-id="44159-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="44159-116">Přidat `Rating` pole na stránky odstranit a podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="44159-116">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="44159-117">Aktualizace *Create.cshtml* s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="44159-117">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="44159-118">Vám může zkopírujte a vložte předchozí `<div>` elementu a umožňují nápovědy intelliSense aktualizovat pole.</span><span class="sxs-lookup"><span data-stu-id="44159-118">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="44159-119">IntelliSense pracuje s [značky Pomocníci](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="44159-119">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Vývojář zadal písmeno R hodnoty atributu ASP-pro v druhé elementu label zobrazení.](new-field/_static/cr.png)

<span data-ttu-id="44159-123">Následující kód ukazuje *Create.cshtml* s `Rating` pole:</span><span class="sxs-lookup"><span data-stu-id="44159-123">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="44159-124">Přidat `Rating` pole na stránku upravit.</span><span class="sxs-lookup"><span data-stu-id="44159-124">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="44159-125">Aplikace nebude fungovat, dokud nebude databáze je aktualizováno, aby zahrnovalo nové pole.</span><span class="sxs-lookup"><span data-stu-id="44159-125">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="44159-126">Je-li spustit nyní, vyvolává aplikaci `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="44159-126">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="44159-127">Tato chyba je způsobená aktualizované třídy modelu film se liší od schématu film tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="44159-127">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="44159-128">(Není žádná `Rating` sloupec v tabulce databáze.)</span><span class="sxs-lookup"><span data-stu-id="44159-128">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="44159-129">Řešení chyby několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="44159-129">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="44159-130">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi pomocí nové třídy schéma modelu.</span><span class="sxs-lookup"><span data-stu-id="44159-130">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="44159-131">Tento přístup je vhodné již v rané fázi v cyklu vývoje; umožňuje rychle společně momentální schéma modelu a databáze.</span><span class="sxs-lookup"><span data-stu-id="44159-131">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="44159-132">Nevýhodou je, že přijdete o stávající data v databázi.</span><span class="sxs-lookup"><span data-stu-id="44159-132">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="44159-133">Nechcete, aby pro tento postup u provozní databáze!</span><span class="sxs-lookup"><span data-stu-id="44159-133">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="44159-134">Vyřazení databáze na změny schématu a automaticky naplnit databázi daty test pomocí inicializátoru je často produktivní způsob, jak vyvíjet aplikace.</span><span class="sxs-lookup"><span data-stu-id="44159-134">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="44159-135">Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="44159-135">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="44159-136">Výhodou tohoto přístupu je, že zachováte data.</span><span class="sxs-lookup"><span data-stu-id="44159-136">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="44159-137">Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.</span><span class="sxs-lookup"><span data-stu-id="44159-137">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="44159-138">Použijte migrace Code First k aktualizaci schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="44159-138">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="44159-139">V tomto kurzu používejte migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="44159-139">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="44159-140">Aktualizace `SeedData` třídy tak, aby poskytuje hodnotu pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="44159-140">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="44159-141">Ukázka změnu jsou uvedeny níže, ale budete chtít tuto změnu provést pro každý `new Movie` bloku.</span><span class="sxs-lookup"><span data-stu-id="44159-141">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="44159-142">Najdete v článku [dokončit SeedData.cs souboru](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="44159-142">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="44159-143">Najdete v článku [dokončit SeedData.cs souboru](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="44159-143">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span></span>
::: moniker-end

<span data-ttu-id="44159-144">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="44159-144">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="44159-145">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="44159-145">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="44159-146">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="44159-146">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="44159-147">`Add-Migration` Příkaz zjistí rozhraní pro:</span><span class="sxs-lookup"><span data-stu-id="44159-147">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="44159-148">Porovnání `Movie` modelu s `Movie` schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="44159-148">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="44159-149">Vytvoření kódu k migraci schéma databáze do nového modelu.</span><span class="sxs-lookup"><span data-stu-id="44159-149">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="44159-150">Název "Hodnocení" libovolný a slouží k názvu souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="44159-150">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="44159-151">Je vhodné použít smysluplný název souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="44159-151">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="44159-152">Pokud odstraníte všechny záznamy v databázi, bude inicializátoru počáteční hodnoty databáze a zahrnout `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="44159-152">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="44159-153">To lze provést pomocí odstranění odkazy v prohlížeči nebo z [Průzkumník objektů systému Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="44159-153">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="44159-154">Chcete-li odstranit databázi z SSOX:</span><span class="sxs-lookup"><span data-stu-id="44159-154">To delete the database from SSOX:</span></span>

* <span data-ttu-id="44159-155">Vyberte databázi v SSOX.</span><span class="sxs-lookup"><span data-stu-id="44159-155">Select the database in SSOX.</span></span>
* <span data-ttu-id="44159-156">Klikněte pravým tlačítkem na databázi a vyberte *odstranit*.</span><span class="sxs-lookup"><span data-stu-id="44159-156">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="44159-157">Zkontrolujte **ukončete stávající připojení**.</span><span class="sxs-lookup"><span data-stu-id="44159-157">Check **Close existing connections**.</span></span>
* <span data-ttu-id="44159-158">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="44159-158">Select **OK**.</span></span>
* <span data-ttu-id="44159-159">V [pomocí PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizovat databázi:</span><span class="sxs-lookup"><span data-stu-id="44159-159">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="44159-160">Spusťte aplikaci a ověřte, můžete vytvořit, upravit nebo zobrazení filmy s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="44159-160">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="44159-161">Pokud není nasadí databázi, zastavte službu IIS Express a pak spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="44159-161">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="44159-162">[Předchozí: Přidání vyhledávací](xref:tutorials/razor-pages/search)
> [Další: Přidání ověření](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="44159-162">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
