---
title: "Přidání nové pole do stránky Razor"
author: rick-anderson
description: "Ukazuje, jak přidat nové pole na stránku Razor základní Entity Framework"
keywords: ASP.NET Core Entity Framework Core, migrace
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 128b69513976a56104524bb803f2b8cb1daf1967
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="bda2e-104">Přidání nové pole do stránky Razor</span><span class="sxs-lookup"><span data-stu-id="bda2e-104">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="bda2e-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bda2e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bda2e-106">V této části budete používat [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migrace Code First a přidat nové pole do modelu migraci, které změnit na databázi.</span><span class="sxs-lookup"><span data-stu-id="bda2e-106">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="bda2e-107">Při použití EF Code First automaticky vytvořit databázi, Code First přidá tabulku k databázi chcete-li sledovat, jestli je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="bda2e-107">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="bda2e-108">Pokud nejsou synchronizované, EF vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="bda2e-108">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="bda2e-109">Díky tomu je snazší najít problémy nekonzistentní databáze nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="bda2e-109">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="bda2e-110">Přidání vlastnosti hodnocení filmu modelu</span><span class="sxs-lookup"><span data-stu-id="bda2e-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="bda2e-111">Otevřete *Models/Movie.cs* souboru a přidejte `Rating` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="bda2e-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="bda2e-112">Sestavení aplikace (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="bda2e-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="bda2e-113">Upravit *Pages/Movies/Index.cshtml*a přidejte `Rating` pole:</span><span class="sxs-lookup"><span data-stu-id="bda2e-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="bda2e-114">Přidat `Rating` pole na stránky odstranit a podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bda2e-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="bda2e-115">Aktualizace *Create.cshtml* s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="bda2e-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="bda2e-116">Vám může zkopírujte a vložte předchozí `<div>` elementu a umožňují nápovědy intelliSense aktualizovat pole.</span><span class="sxs-lookup"><span data-stu-id="bda2e-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="bda2e-117">IntelliSense pracuje s [značky Pomocníci](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="bda2e-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Vývojář zadal písmeno R hodnoty atributu ASP-pro v druhé elementu label zobrazení.](new-field/_static/cr.png)

<span data-ttu-id="bda2e-121">Následující kód ukazuje *Create.cshtml* s `Rating` pole:</span><span class="sxs-lookup"><span data-stu-id="bda2e-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="bda2e-122">Přidat `Rating` pole na stránku upravit.</span><span class="sxs-lookup"><span data-stu-id="bda2e-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="bda2e-123">Aplikace nebude fungovat, dokud nebude databáze je aktualizováno, aby zahrnovalo nové pole.</span><span class="sxs-lookup"><span data-stu-id="bda2e-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="bda2e-124">Je-li spustit nyní, vyvolává aplikaci `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="bda2e-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="bda2e-125">Tato chyba je způsobená aktualizované třídy modelu film se liší od schématu film tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="bda2e-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="bda2e-126">(Není žádná `Rating` sloupec v tabulce databáze.)</span><span class="sxs-lookup"><span data-stu-id="bda2e-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="bda2e-127">Řešení chyby několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="bda2e-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="bda2e-128">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi pomocí nové třídy schéma modelu.</span><span class="sxs-lookup"><span data-stu-id="bda2e-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="bda2e-129">Tento přístup je vhodné již v rané fázi v cyklu vývoje; umožňuje rychle společně momentální schéma modelu a databáze.</span><span class="sxs-lookup"><span data-stu-id="bda2e-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="bda2e-130">Nevýhodou je, že přijdete o stávající data v databázi.</span><span class="sxs-lookup"><span data-stu-id="bda2e-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="bda2e-131">Nechcete, aby pro tento postup u provozní databáze!</span><span class="sxs-lookup"><span data-stu-id="bda2e-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="bda2e-132">Vyřazení databáze na změny schématu a automaticky naplnit databázi daty test pomocí inicializátoru je často produktivní způsob, jak vyvíjet aplikace.</span><span class="sxs-lookup"><span data-stu-id="bda2e-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="bda2e-133">Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="bda2e-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="bda2e-134">Výhodou tohoto přístupu je, že zachováte data.</span><span class="sxs-lookup"><span data-stu-id="bda2e-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="bda2e-135">Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.</span><span class="sxs-lookup"><span data-stu-id="bda2e-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="bda2e-136">Použijte migrace Code First k aktualizaci schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="bda2e-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="bda2e-137">V tomto kurzu používejte migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="bda2e-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="bda2e-138">Aktualizace `SeedData` třídy tak, aby poskytuje hodnotu pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="bda2e-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="bda2e-139">Ukázka změnu jsou uvedeny níže, ale budete chtít tuto změnu provést pro každý `new Movie` bloku.</span><span class="sxs-lookup"><span data-stu-id="bda2e-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="bda2e-140">Najdete v článku [dokončit SeedData.cs souboru](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="bda2e-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="bda2e-141">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="bda2e-141">Build the solution.</span></span>

<a name="pmc"></a><span data-ttu-id="bda2e-142">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="bda2e-142">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="bda2e-143">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="bda2e-143">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="bda2e-144">`Add-Migration` Příkaz zjistí rozhraní pro:</span><span class="sxs-lookup"><span data-stu-id="bda2e-144">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="bda2e-145">Porovnání `Movie` modelu s `Movie` schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="bda2e-145">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="bda2e-146">Vytvoření kódu k migraci schéma databáze do nového modelu.</span><span class="sxs-lookup"><span data-stu-id="bda2e-146">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="bda2e-147">Název "Hodnocení" libovolný a slouží k názvu souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="bda2e-147">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="bda2e-148">Je vhodné použít smysluplný název souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="bda2e-148">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a><span data-ttu-id="bda2e-149">Pokud odstraníte všechny záznamy v databázi, bude inicializátoru počáteční hodnoty databáze a zahrnout `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="bda2e-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="bda2e-150">To lze provést pomocí odstranění odkazy v prohlížeči nebo z [Průzkumník objektů systému Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="bda2e-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="bda2e-151">Chcete-li odstranit databázi z SSOX:</span><span class="sxs-lookup"><span data-stu-id="bda2e-151">To delete the database from SSOX:</span></span>

* <span data-ttu-id="bda2e-152">Vyberte databázi v SSOX.</span><span class="sxs-lookup"><span data-stu-id="bda2e-152">Select the database in SSOX.</span></span>
* <span data-ttu-id="bda2e-153">Klikněte pravým tlačítkem na databázi a vyberte *odstranit*.</span><span class="sxs-lookup"><span data-stu-id="bda2e-153">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="bda2e-154">Zkontrolujte **ukončete stávající připojení**.</span><span class="sxs-lookup"><span data-stu-id="bda2e-154">Check **Close existing connections**.</span></span>
* <span data-ttu-id="bda2e-155">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="bda2e-155">Select **OK**.</span></span>
* <span data-ttu-id="bda2e-156">V [pomocí PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizovat databázi:</span><span class="sxs-lookup"><span data-stu-id="bda2e-156">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="bda2e-157">Spusťte aplikaci a ověřte, můžete vytvořit, upravit nebo zobrazení filmy s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="bda2e-157">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="bda2e-158">Pokud není nasadí databázi, zastavte službu IIS Express a pak spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bda2e-158">If the database is not seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="bda2e-159">[Předchozí: Přidání vyhledávací](xref:tutorials/razor-pages/search)
[Další: Přidání ověření](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="bda2e-159">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
