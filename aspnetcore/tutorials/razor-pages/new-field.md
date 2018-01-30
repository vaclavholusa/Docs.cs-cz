---
title: "Přidání nové pole do stránky Razor"
author: rick-anderson
description: "Ukazuje, jak přidat nové pole na stránku Razor základní Entity Framework"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 36412e9d1f3143f0d1999d0e754e6627f0984ad5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="6668c-103">Přidání nové pole do stránky Razor</span><span class="sxs-lookup"><span data-stu-id="6668c-103">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="6668c-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6668c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6668c-105">V této části budete používat [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migrace Code First a přidat nové pole do modelu migraci, které změnit na databázi.</span><span class="sxs-lookup"><span data-stu-id="6668c-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="6668c-106">Při použití EF Code First automaticky vytvořit databázi, Code First přidá tabulku k databázi chcete-li sledovat, jestli je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="6668c-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="6668c-107">Pokud nejsou synchronizované, EF vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="6668c-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="6668c-108">Díky tomu je snazší najít problémy nekonzistentní databáze nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="6668c-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="6668c-109">Přidání vlastnosti hodnocení filmu modelu</span><span class="sxs-lookup"><span data-stu-id="6668c-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="6668c-110">Otevřete *Models/Movie.cs* souboru a přidejte `Rating` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="6668c-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="6668c-111">Sestavení aplikace (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="6668c-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="6668c-112">Upravit *Pages/Movies/Index.cshtml*a přidejte `Rating` pole:</span><span class="sxs-lookup"><span data-stu-id="6668c-112">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="6668c-113">Přidat `Rating` pole na stránky odstranit a podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6668c-113">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="6668c-114">Aktualizace *Create.cshtml* s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="6668c-114">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="6668c-115">Vám může zkopírujte a vložte předchozí `<div>` elementu a umožňují nápovědy intelliSense aktualizovat pole.</span><span class="sxs-lookup"><span data-stu-id="6668c-115">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="6668c-116">IntelliSense pracuje s [značky Pomocníci](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="6668c-116">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Vývojář zadal písmeno R hodnoty atributu ASP-pro v druhé elementu label zobrazení.](new-field/_static/cr.png)

<span data-ttu-id="6668c-120">Následující kód ukazuje *Create.cshtml* s `Rating` pole:</span><span class="sxs-lookup"><span data-stu-id="6668c-120">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="6668c-121">Přidat `Rating` pole na stránku upravit.</span><span class="sxs-lookup"><span data-stu-id="6668c-121">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="6668c-122">Aplikace nebude fungovat, dokud nebude databáze je aktualizováno, aby zahrnovalo nové pole.</span><span class="sxs-lookup"><span data-stu-id="6668c-122">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="6668c-123">Je-li spustit nyní, vyvolává aplikaci `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="6668c-123">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="6668c-124">Tato chyba je způsobená aktualizované třídy modelu film se liší od schématu film tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="6668c-124">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="6668c-125">(Není žádná `Rating` sloupec v tabulce databáze.)</span><span class="sxs-lookup"><span data-stu-id="6668c-125">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="6668c-126">Řešení chyby několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="6668c-126">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="6668c-127">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi pomocí nové třídy schéma modelu.</span><span class="sxs-lookup"><span data-stu-id="6668c-127">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="6668c-128">Tento přístup je vhodné již v rané fázi v cyklu vývoje; umožňuje rychle společně momentální schéma modelu a databáze.</span><span class="sxs-lookup"><span data-stu-id="6668c-128">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="6668c-129">Nevýhodou je, že přijdete o stávající data v databázi.</span><span class="sxs-lookup"><span data-stu-id="6668c-129">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="6668c-130">Nechcete, aby pro tento postup u provozní databáze!</span><span class="sxs-lookup"><span data-stu-id="6668c-130">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="6668c-131">Vyřazení databáze na změny schématu a automaticky naplnit databázi daty test pomocí inicializátoru je často produktivní způsob, jak vyvíjet aplikace.</span><span class="sxs-lookup"><span data-stu-id="6668c-131">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="6668c-132">Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="6668c-132">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="6668c-133">Výhodou tohoto přístupu je, že zachováte data.</span><span class="sxs-lookup"><span data-stu-id="6668c-133">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="6668c-134">Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.</span><span class="sxs-lookup"><span data-stu-id="6668c-134">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="6668c-135">Použijte migrace Code First k aktualizaci schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="6668c-135">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="6668c-136">V tomto kurzu používejte migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="6668c-136">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="6668c-137">Aktualizace `SeedData` třídy tak, aby poskytuje hodnotu pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="6668c-137">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="6668c-138">Ukázka změnu jsou uvedeny níže, ale budete chtít tuto změnu provést pro každý `new Movie` bloku.</span><span class="sxs-lookup"><span data-stu-id="6668c-138">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="6668c-139">Najdete v článku [dokončit SeedData.cs souboru](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="6668c-139">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="6668c-140">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="6668c-140">Build the solution.</span></span>

<a name="pmc"></a><span data-ttu-id="6668c-141">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="6668c-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="6668c-142">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="6668c-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="6668c-143">`Add-Migration` Příkaz zjistí rozhraní pro:</span><span class="sxs-lookup"><span data-stu-id="6668c-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="6668c-144">Porovnání `Movie` modelu s `Movie` schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="6668c-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="6668c-145">Vytvoření kódu k migraci schéma databáze do nového modelu.</span><span class="sxs-lookup"><span data-stu-id="6668c-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="6668c-146">Název "Hodnocení" libovolný a slouží k názvu souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="6668c-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="6668c-147">Je vhodné použít smysluplný název souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="6668c-147">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a><span data-ttu-id="6668c-148">Pokud odstraníte všechny záznamy v databázi, bude inicializátoru počáteční hodnoty databáze a zahrnout `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="6668c-148">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="6668c-149">To lze provést pomocí odstranění odkazy v prohlížeči nebo z [Průzkumník objektů systému Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="6668c-149">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="6668c-150">Chcete-li odstranit databázi z SSOX:</span><span class="sxs-lookup"><span data-stu-id="6668c-150">To delete the database from SSOX:</span></span>

* <span data-ttu-id="6668c-151">Vyberte databázi v SSOX.</span><span class="sxs-lookup"><span data-stu-id="6668c-151">Select the database in SSOX.</span></span>
* <span data-ttu-id="6668c-152">Klikněte pravým tlačítkem na databázi a vyberte *odstranit*.</span><span class="sxs-lookup"><span data-stu-id="6668c-152">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="6668c-153">Zkontrolujte **ukončete stávající připojení**.</span><span class="sxs-lookup"><span data-stu-id="6668c-153">Check **Close existing connections**.</span></span>
* <span data-ttu-id="6668c-154">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="6668c-154">Select **OK**.</span></span>
* <span data-ttu-id="6668c-155">V [pomocí PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizovat databázi:</span><span class="sxs-lookup"><span data-stu-id="6668c-155">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="6668c-156">Spusťte aplikaci a ověřte, můžete vytvořit, upravit nebo zobrazení filmy s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="6668c-156">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="6668c-157">Pokud není nasadí databázi, zastavte službu IIS Express a pak spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6668c-157">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6668c-158">[Předchozí: Přidání vyhledávací](xref:tutorials/razor-pages/search)
[Další: Přidání ověření](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="6668c-158">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
