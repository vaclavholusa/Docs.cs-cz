---
title: "Přidání nové pole"
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 046e16b1a9581edb2be9a2315cf7f2677684747b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-new-field"></a><span data-ttu-id="b1c50-102">Přidání nové pole</span><span class="sxs-lookup"><span data-stu-id="b1c50-102">Adding a New Field</span></span>

<span data-ttu-id="b1c50-103">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b1c50-103">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b1c50-104">V této části budete používat [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migrace Code First a přidat nové pole do modelu migraci, které změnit na databázi.</span><span class="sxs-lookup"><span data-stu-id="b1c50-104">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="b1c50-105">Při použití EF Code First automaticky vytvořit databázi, Code First přidá tabulku k databázi chcete-li sledovat, jestli je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="b1c50-105">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="b1c50-106">Pokud nejsou synchronizované, EF vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="b1c50-106">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="b1c50-107">Díky tomu je snazší najít problémy nekonzistentní databáze nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="b1c50-107">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="b1c50-108">Přidání vlastnosti hodnocení filmu modelu</span><span class="sxs-lookup"><span data-stu-id="b1c50-108">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="b1c50-109">Otevřete *Models/Movie.cs* souboru a přidejte `Rating` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="b1c50-109">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="b1c50-110">Sestavení aplikace (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="b1c50-110">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="b1c50-111">Protože jste přidali nové pole do `Movie` třídy, je také potřeba aktualizovat seznamu povolených vazby, tato vlastnost budou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="b1c50-111">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="b1c50-112">V *MoviesController.cs*, aktualizovat `[Bind]` atribut pro obě `Create` a `Edit` akce metody pro zahrnutí `Rating` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="b1c50-112">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="b1c50-113">Je také potřeba aktualizovat zobrazit šablony, aby bylo možné zobrazit, vytvořit a upravit nové `Rating` vlastnost v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b1c50-113">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="b1c50-114">Upravit */Views/Movies/Index.cshtml* souboru a přidejte `Rating` pole:</span><span class="sxs-lookup"><span data-stu-id="b1c50-114">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[Main](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="b1c50-115">Aktualizace */Views/Movies/Create.cshtml* s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="b1c50-115">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="b1c50-116">Můžete zkopírujte a vložte předchozí "formuláře skupinu" a pomohou intelliSense aktualizovat pole.</span><span class="sxs-lookup"><span data-stu-id="b1c50-116">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="b1c50-117">IntelliSense pracuje s [značky Pomocníci](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="b1c50-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="b1c50-118">Poznámka: V verze RTM nástroje Visual Studio 2017 musíte nainstalovat [služby jazyk Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) technologie intelliSense pro Razor.</span><span class="sxs-lookup"><span data-stu-id="b1c50-118">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="b1c50-119">Tento problém bude vyřešený v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="b1c50-119">This will be fixed in the next release.</span></span>

![Vývojář zadal písmeno R hodnoty atributu ASP-pro v druhé elementu label zobrazení.](new-field/_static/cr.png)

<span data-ttu-id="b1c50-123">Aplikace nebude fungovat, dokud aktualizujeme DB zahrnout nové pole.</span><span class="sxs-lookup"><span data-stu-id="b1c50-123">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="b1c50-124">Pokud spustíte ji nyní, získáte následující `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="b1c50-124">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="b1c50-125">Tato chyba se zobrazuje, protože aktualizované třídy modelu film se liší od schématu tabulky film existující databáze.</span><span class="sxs-lookup"><span data-stu-id="b1c50-125">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="b1c50-126">(V tabulce databáze neexistuje žádný sloupec hodnocení.)</span><span class="sxs-lookup"><span data-stu-id="b1c50-126">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="b1c50-127">Řešení chyby několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="b1c50-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="b1c50-128">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi na základě nové třídy schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="b1c50-128">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="b1c50-129">Tento přístup je velmi praktické již v rané fázi v cyklu vývoje, pokud byste active vývoj pro testovací databázi. umožňuje rychle společně momentální schéma modelu a databáze.</span><span class="sxs-lookup"><span data-stu-id="b1c50-129">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="b1c50-130">Nevýhodou, ale je, že přijdete o stávající data v databázi – tak, že nechcete, aby pro tento postup u provozní databáze!</span><span class="sxs-lookup"><span data-stu-id="b1c50-130">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="b1c50-131">Pomocí inicializátoru automaticky počáteční hodnoty databázi s daty test je často produktivní způsob, jak vyvíjet aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1c50-131">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="b1c50-132">Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="b1c50-132">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="b1c50-133">Výhodou tohoto přístupu je, že zachováte data.</span><span class="sxs-lookup"><span data-stu-id="b1c50-133">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="b1c50-134">Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.</span><span class="sxs-lookup"><span data-stu-id="b1c50-134">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="b1c50-135">Použijte migrace Code First k aktualizaci schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="b1c50-135">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="b1c50-136">V tomto kurzu použijeme migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="b1c50-136">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="b1c50-137">Aktualizace `SeedData` třídy tak, aby poskytuje hodnotu pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="b1c50-137">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="b1c50-138">Ukázka změnu jsou uvedeny níže, ale budete chtít tuto změnu provést pro každý `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="b1c50-138">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="b1c50-139">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="b1c50-139">Build the solution.</span></span>

<span data-ttu-id="b1c50-140">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="b1c50-140">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Pomocí PMC nabídky](adding-model/_static/pmc.png)

<span data-ttu-id="b1c50-142">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="b1c50-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="b1c50-143">`Add-Migration` Příkaz zjistí rozhraní migrace prozkoumat aktuální `Movie` modelu s aktuálním `Movie` schéma databáze a vytvořit nezbytného kódu migrace databáze do nového modelu.</span><span class="sxs-lookup"><span data-stu-id="b1c50-143">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="b1c50-144">Název "Hodnocení" libovolný a slouží k názvu souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="b1c50-144">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="b1c50-145">Je vhodné použít smysluplný název souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="b1c50-145">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="b1c50-146">Pokud odstraníte všechny záznamy v databázi, bude inicializovat počáteční hodnoty databáze a zahrnout `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="b1c50-146">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="b1c50-147">Můžete provést s odstranit odkazy v prohlížeči nebo z SSOX.</span><span class="sxs-lookup"><span data-stu-id="b1c50-147">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="b1c50-148">Spusťte aplikaci a ověřte, můžete vytvořit, upravit nebo zobrazení filmy s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="b1c50-148">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="b1c50-149">Měli byste také přidat `Rating` do `Edit`, `Details`, a `Delete` zobrazí šablony.</span><span class="sxs-lookup"><span data-stu-id="b1c50-149">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b1c50-150">[Předchozí](search.md)
[další](validation.md)</span><span class="sxs-lookup"><span data-stu-id="b1c50-150">[Previous](search.md)
[Next](validation.md)</span></span>  
