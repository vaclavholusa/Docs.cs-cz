# <a name="adding-a-new-field"></a><span data-ttu-id="e43e0-101">Přidání nové pole</span><span class="sxs-lookup"><span data-stu-id="e43e0-101">Adding a new field</span></span>

<span data-ttu-id="e43e0-102">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e43e0-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e43e0-103">Přidá nové pole do tohoto kurzu `Movies` tabulky.</span><span class="sxs-lookup"><span data-stu-id="e43e0-103">This tutorial will add a new field to the `Movies` table.</span></span> <span data-ttu-id="e43e0-104">Jsme budete vyřaďte databázi a vytvořte novou, když nám změnit schéma (Přidat nové pole).</span><span class="sxs-lookup"><span data-stu-id="e43e0-104">We'll drop the database and create a new one when we change the schema (add a new field).</span></span> <span data-ttu-id="e43e0-105">Tento pracovní postup funguje dobře časná ve vývoj při nemáme žádná data produkční do opraveny.</span><span class="sxs-lookup"><span data-stu-id="e43e0-105">This workflow works well early in development when we don't have any production data to perserve.</span></span>

<span data-ttu-id="e43e0-106">Po nasazení vaší aplikace a máte data, která budete muset opraveny, nelze vyřadit vaší databáze, když potřebujete změnit schéma.</span><span class="sxs-lookup"><span data-stu-id="e43e0-106">Once your app is deployed and you have data that you need to perserve, you can't drop your DB when you need to change the schema.</span></span> <span data-ttu-id="e43e0-107">Rozhraní Entity Framework [migrace Code First](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) vám umožní aktualizovat schéma a migraci databáze bez ztráty dat.</span><span class="sxs-lookup"><span data-stu-id="e43e0-107">Entity Framework [Code First Migrations](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) allows you to update your schema and migrate the database without losing data.</span></span> <span data-ttu-id="e43e0-108">Migrace je oblíbených funkce při použití SQL serveru, ale SQLlite nepodporuje mnoho schématu operací migrace, takže jenom velmi jednoduše migrace se možná.</span><span class="sxs-lookup"><span data-stu-id="e43e0-108">Migrations is a popular feature when using SQL Server, but SQLlite doesn't support many migration schema operations, so only very simply migrations are possible.</span></span> <span data-ttu-id="e43e0-109">V tématu [SQLite omezení](https://docs.microsoft.com/ef/core/providers/sqlite/limitations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e43e0-109">See [SQLite Limitations](https://docs.microsoft.com/ef/core/providers/sqlite/limitations) for more information.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="e43e0-110">Přidání vlastnosti hodnocení filmu modelu</span><span class="sxs-lookup"><span data-stu-id="e43e0-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="e43e0-111">Otevřete *Models/Movie.cs* souboru a přidejte `Rating` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="e43e0-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="e43e0-112">Protože jste přidali nové pole do `Movie` třídy, je také potřeba aktualizovat bílou vazby, tato vlastnost budou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="e43e0-112">Because you've added a new field to the `Movie` class, you also need to update the binding whitelist so this new property will be included.</span></span> <span data-ttu-id="e43e0-113">V *MoviesController.cs*, aktualizovat `[Bind]` atribut pro obě `Create` a `Edit` akce metody pro zahrnutí `Rating` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="e43e0-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="e43e0-114">Můžete také potřebovat k aktualizaci zobrazit šablony, aby bylo možné zobrazit, vytvářet a upravovat nové `Rating` vlastnost v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e43e0-114">You also need to update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="e43e0-115">Upravit */Views/Movies/Index.cshtml* souboru a přidejte `Rating` pole:</span><span class="sxs-lookup"><span data-stu-id="e43e0-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="e43e0-116">Aktualizace */Views/Movies/Create.cshtml* s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="e43e0-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

<span data-ttu-id="e43e0-117">Aplikace nebude fungovat, dokud aktualizujeme DB zahrnout nové pole.</span><span class="sxs-lookup"><span data-stu-id="e43e0-117">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="e43e0-118">Pokud spustíte ji nyní, získáte následující `SqliteException`:</span><span class="sxs-lookup"><span data-stu-id="e43e0-118">If you run it now, you'll get the following `SqliteException`:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

<span data-ttu-id="e43e0-119">Tato chyba se zobrazuje, protože aktualizované třídy modelu film se liší od schématu tabulky film existující databáze.</span><span class="sxs-lookup"><span data-stu-id="e43e0-119">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="e43e0-120">(Není žádná `Rating` sloupec v tabulce databáze.)</span><span class="sxs-lookup"><span data-stu-id="e43e0-120">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="e43e0-121">Řešení chyby několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="e43e0-121">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="e43e0-122">Vyřaďte databázi a mít automaticky znovu vytvořit databázi na základě nové třídy schématu modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e43e0-122">Drop the database and have the Entity Framework automatically re-create the database based on the new model class schema.</span></span> <span data-ttu-id="e43e0-123">S tímto přístupem, přijdete o stávající data v databázi –, nelze to provést pomocí provozní databáze!</span><span class="sxs-lookup"><span data-stu-id="e43e0-123">With this approach, you lose existing data in the database — so you can't do this with a production database!</span></span> <span data-ttu-id="e43e0-124">Pomocí inicializátoru automaticky počáteční hodnoty databázi s daty test je často produktivní způsob, jak vyvíjet aplikace.</span><span class="sxs-lookup"><span data-stu-id="e43e0-124">Using an initializer to automatically seed a database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="e43e0-125">Ručně upravte schéma z existující databáze tak, aby odpovídala třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="e43e0-125">Manually modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="e43e0-126">Výhodou tohoto přístupu je, že zachováte data.</span><span class="sxs-lookup"><span data-stu-id="e43e0-126">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="e43e0-127">Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.</span><span class="sxs-lookup"><span data-stu-id="e43e0-127">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="e43e0-128">Použijte migrace Code First k aktualizaci schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="e43e0-128">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="e43e0-129">V tomto kurzu jsme budete vyřadit a znovu vytvořit databázi, když se změní schéma.</span><span class="sxs-lookup"><span data-stu-id="e43e0-129">For this tutorial, we'll drop and re-create the database when the schema changes.</span></span> <span data-ttu-id="e43e0-130">Spusťte následující příkaz z terminálu odpojení databáze:</span><span class="sxs-lookup"><span data-stu-id="e43e0-130">Run the following command from a terminal to drop the db:</span></span>

`dotnet ef database drop`

<span data-ttu-id="e43e0-131">Aktualizace `SeedData` třídy tak, aby poskytuje hodnotu pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="e43e0-131">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="e43e0-132">Ukázka změnu jsou uvedeny níže, ale budete chtít tuto změnu provést pro každý `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="e43e0-132">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="e43e0-133">Přidat `Rating` pole na `Edit`, `Details`, a `Delete` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e43e0-133">Add the `Rating` field to the `Edit`, `Details`, and `Delete` view.</span></span>

<span data-ttu-id="e43e0-134">Spusťte aplikaci a ověřte, můžete vytvořit, upravit nebo zobrazení filmy s `Rating` pole.</span><span class="sxs-lookup"><span data-stu-id="e43e0-134">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="e43e0-135">šablony.</span><span class="sxs-lookup"><span data-stu-id="e43e0-135">templates.</span></span>
