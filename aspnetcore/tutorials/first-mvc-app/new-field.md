---
title: Přidání nového pole do aplikace ASP.NET Core MVC
author: rick-anderson
description: Další informace o použití migrace Entity Framework Code First pro přidání nového pole do modelu a migrovat tuto změnu do databáze.
ms.author: riande
ms.date: 10/06/2017
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 74f7a98143c80504d534c5ee4fd06b3dd076a2f2
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312228"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="b76da-103">Přidání nového pole do aplikace ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="b76da-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="b76da-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b76da-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b76da-105">V této části použijete [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migrace Code First pro přidání nového pole do modelu a migraci, které změnit na databázi.</span><span class="sxs-lookup"><span data-stu-id="b76da-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="b76da-106">Při použití platforem EF Code First automaticky vytvořit databázi, Code First přidá tabulku do databáze pro sledování, zda je synchronizovaný s tříd modelu, které byly vygenerovány z schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="b76da-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="b76da-107">Pokud nejsou synchronizované, EF vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="b76da-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="b76da-108">Díky tomu je snazší najít problémy s nekonzistentní databáze nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="b76da-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="b76da-109">Přidání vlastnosti do hodnocení filmů modelu</span><span class="sxs-lookup"><span data-stu-id="b76da-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="b76da-110">Otevřít *Models/Movie.cs* a přidejte `Rating` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="b76da-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]
::: moniker-end

<span data-ttu-id="b76da-111">Vytvořte aplikaci (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="b76da-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="b76da-112">Protože jsme přidali nové pole do `Movie` třídy, musíte také aktualizovat vazbu seznamu povolených, aby tuto novou vlastnost budou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="b76da-112">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="b76da-113">V *MoviesController.cs*, aktualizovat `[Bind]` atribut pro obě `Create` a `Edit` metody akce, které chcete zahrnout `Rating` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="b76da-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="b76da-114">Je také potřeba aktualizovat zobrazit šablony, aby bylo možné zobrazit, vytvořit a upravit novou `Rating` vlastností v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b76da-114">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="b76da-115">Upravit */Views/Movies/Index.cshtml* a přidejte `Rating` pole:</span><span class="sxs-lookup"><span data-stu-id="b76da-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="b76da-116">Aktualizace */Views/Movies/Create.cshtml* s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="b76da-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="b76da-117">Můžete kopírovat/vložit předchozí "formuláře skupinu" a aktualizujte pole pomohou technologie intelliSense.</span><span class="sxs-lookup"><span data-stu-id="b76da-117">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="b76da-118">Technologie IntelliSense funguje s [pomocných rutin značek](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="b76da-118">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="b76da-119">Poznámka: Ve verzi RTM sady Visual Studio 2017 je potřeba nainstalovat [jazykové služby Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) pro funkce Razor intelliSense.</span><span class="sxs-lookup"><span data-stu-id="b76da-119">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="b76da-120">Tato chyba bude opravena v další vydané verzi.</span><span class="sxs-lookup"><span data-stu-id="b76da-120">This will be fixed in the next release.</span></span>

![Vývojář napsal písmeno R pro hodnotu atributu ASP-pro druhý popisek prvku zobrazení.](new-field/_static/cr.png)

<span data-ttu-id="b76da-124">Aplikace nebude fungovat, dokud aktualizujeme DB zahrnout nové pole.</span><span class="sxs-lookup"><span data-stu-id="b76da-124">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="b76da-125">Pokud jste ji nyní spustit, zobrazí se následující `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="b76da-125">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="b76da-126">Tato chyba se zobrazuje, protože aktualizované třídy modelu film se liší od schématu tabulky Movie existující databáze.</span><span class="sxs-lookup"><span data-stu-id="b76da-126">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="b76da-127">(V tabulce databáze není žádný sloupec hodnocení.)</span><span class="sxs-lookup"><span data-stu-id="b76da-127">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="b76da-128">Řešení chyby několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="b76da-128">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="b76da-129">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi založené na nové schéma třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="b76da-129">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="b76da-130">Tento přístup je velmi vhodné v rané fázi vývojového cyklu při provádění testu databáze; s aktivním vývojem umožňuje rychlý rozvoj schématu modelu a databáze společně.</span><span class="sxs-lookup"><span data-stu-id="b76da-130">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="b76da-131">Nevýhodou, je však dojít ke ztrátě existujících dat v databázi, takže nechcete tuto metodu použijte u provozní databáze.</span><span class="sxs-lookup"><span data-stu-id="b76da-131">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="b76da-132">Použití inicializátoru automaticky naplnit databázi daty testu je často produktivní způsob, jak vyvíjet aplikace.</span><span class="sxs-lookup"><span data-stu-id="b76da-132">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="b76da-133">Explicitně upravte schéma stávající databázi tak, aby odpovídalo tříd modelu.</span><span class="sxs-lookup"><span data-stu-id="b76da-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="b76da-134">Výhodou tohoto přístupu je, že zachováte vaše data.</span><span class="sxs-lookup"><span data-stu-id="b76da-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="b76da-135">Můžete tuto změnu provést buď ručně, nebo tak, že vytvoříte databázi změnit skript.</span><span class="sxs-lookup"><span data-stu-id="b76da-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="b76da-136">Pomocí migrace Code First aktualizovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="b76da-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="b76da-137">Pro účely tohoto kurzu používáme migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="b76da-137">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="b76da-138">Aktualizace `SeedData` třídy tak, že poskytuje hodnoty pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="b76da-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="b76da-139">Ukázka změnu je uveden níže, ale budete chtít tuto změnu pro každou `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="b76da-139">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="b76da-140">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="b76da-140">Build the solution.</span></span>

<span data-ttu-id="b76da-141">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="b76da-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![PMC nabídky](adding-model/_static/pmc.png)

<span data-ttu-id="b76da-143">V konzole PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="b76da-143">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="b76da-144">`Add-Migration` Příkaz říká rozhraní framework migrace ke kontrole aktuální `Movie` modelů s aktuálním `Movie` schématu databáze a vytvářet kód potřebné k migraci databáze na nový model.</span><span class="sxs-lookup"><span data-stu-id="b76da-144">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="b76da-145">Název "Hodnocení" je volitelný a slouží k pojmenování souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="b76da-145">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="b76da-146">Je vhodné použít smysluplný název souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="b76da-146">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="b76da-147">Při odstranění všech záznamů v databázi, bude inicializace naplnit databáze a zahrnout `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="b76da-147">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="b76da-148">Můžete to provést s odstranit odkazy v prohlížeči nebo z SSOX.</span><span class="sxs-lookup"><span data-stu-id="b76da-148">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="b76da-149">Spusťte aplikaci a ověřit, je možné vytvořit/upravit/zobrazit videa s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="b76da-149">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="b76da-150">Měli byste také přidat `Rating` pole `Edit`, `Details`, a `Delete` zobrazení šablon.</span><span class="sxs-lookup"><span data-stu-id="b76da-150">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b76da-151">[Předchozí](search.md)
> [další](validation.md)</span><span class="sxs-lookup"><span data-stu-id="b76da-151">[Previous](search.md)
[Next](validation.md)</span></span>  
