---
title: Stránky Razor s EF jádra ASP.NET Core - Migrations - 4 8
author: rick-anderson
description: V tomto kurzu začnete používat funkci migrace EF jádra pro správu změn datových modelů v aplikaci ASP.NET MVC jádra.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: f1776506ef15c75beb9f1a2579b0073f927b013a
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033248"
---
[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="1c833-103">Stránky Razor s EF jádra ASP.NET Core - Migrations - 4 8</span><span class="sxs-lookup"><span data-stu-id="1c833-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="1c833-104">Podle [tní Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1c833-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="1c833-105">V tomto kurzu se používá funkci EF základní migrace pro správu změn datových modelů.</span><span class="sxs-lookup"><span data-stu-id="1c833-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="1c833-106">Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="1c833-106">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

<span data-ttu-id="1c833-107">Když je vyvinut novou aplikaci, model dat často změny.</span><span class="sxs-lookup"><span data-stu-id="1c833-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="1c833-108">Pokaždé, když změny modelu modelu získá synchronizována s databází.</span><span class="sxs-lookup"><span data-stu-id="1c833-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="1c833-109">Tento kurz spuštění nakonfigurováním rozhraní Entity Framework pro vytvoření databáze, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="1c833-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="1c833-110">Pokaždé, když datový model změny:</span><span class="sxs-lookup"><span data-stu-id="1c833-110">Each time the data model changes:</span></span>

* <span data-ttu-id="1c833-111">Databáze je vyřazeno.</span><span class="sxs-lookup"><span data-stu-id="1c833-111">The DB is dropped.</span></span>
* <span data-ttu-id="1c833-112">EF vytvoří novou, který neodpovídá modelu.</span><span class="sxs-lookup"><span data-stu-id="1c833-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="1c833-113">Aplikace doplňuje databáze s testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="1c833-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="1c833-114">Tento přístup k udržování databáze synchronizace s datovým modelem funguje dobře, dokud můžete aplikaci nasadit do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c833-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="1c833-115">Když aplikace běží v produkčním prostředí, je obvykle ukládá data, která je třeba zachovat.</span><span class="sxs-lookup"><span data-stu-id="1c833-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="1c833-116">Aplikace nemůže začínat testu DB pokaždé, když dojde ke změně (jako je například přidávání nové sloupce).</span><span class="sxs-lookup"><span data-stu-id="1c833-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="1c833-117">Tento problém řeší funkci migrace základní EF povolením EF základní aktualizovat schéma databáze místo vytvoření nové databáze.</span><span class="sxs-lookup"><span data-stu-id="1c833-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="1c833-118">Namísto vyřadit a znovu vytvořit databázi, když datový model změny, migrace aktualizace schématu a uchovává existující data.</span><span class="sxs-lookup"><span data-stu-id="1c833-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="drop-the-database"></a><span data-ttu-id="1c833-119">Odpojení databáze</span><span class="sxs-lookup"><span data-stu-id="1c833-119">Drop the database</span></span>

<span data-ttu-id="1c833-120">Použití **Průzkumník objektů systému SQL Server** (SSOX) nebo `database drop` příkaz:</span><span class="sxs-lookup"><span data-stu-id="1c833-120">Use **SQL Server Object Explorer** (SSOX) or the `database drop` command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c833-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c833-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1c833-122">V **Konzola správce balíčků** (pomocí PMC), spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1c833-122">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
```

<span data-ttu-id="1c833-123">Spustit `Get-Help about_EntityFrameworkCore` z pomocí PMC získat informace nápovědy.</span><span class="sxs-lookup"><span data-stu-id="1c833-123">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1c833-124">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="1c833-124">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1c833-125">Otevřete okno příkazového řádku a přejděte do složky projektu.</span><span class="sxs-lookup"><span data-stu-id="1c833-125">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="1c833-126">Obsahuje složky projektu *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="1c833-126">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="1c833-127">Zadejte v příkazovém okně:</span><span class="sxs-lookup"><span data-stu-id="1c833-127">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
 ```

------

## <a name="create-an-initial-migration-and-update-the-db"></a><span data-ttu-id="1c833-128">Vytváření počáteční migrace a aktualizaci databáze</span><span class="sxs-lookup"><span data-stu-id="1c833-128">Create an initial migration and update the DB</span></span>

<span data-ttu-id="1c833-129">Sestavte projekt a vytvořte první migrace.</span><span class="sxs-lookup"><span data-stu-id="1c833-129">Build the project and create the first migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c833-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c833-130">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1c833-131">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="1c833-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

------

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="1c833-132">Zkontrolujte nahoru a dolů metody</span><span class="sxs-lookup"><span data-stu-id="1c833-132">Examine the Up and Down methods</span></span>

<span data-ttu-id="1c833-133">Základní EF `migrations add` příkaz vygeneruje kód k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="1c833-133">The EF Core `migrations add` command  generated code to create the DB.</span></span> <span data-ttu-id="1c833-134">Tento kód migrace je v *migrace\<časové razítko > _InitialCreate.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="1c833-134">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="1c833-135">`Up` Metodu `InitialCreate` třída vytvoří DB tabulky, které odpovídají sady dat modelu entity.</span><span class="sxs-lookup"><span data-stu-id="1c833-135">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="1c833-136">`Down` Metoda odstraní, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1c833-136">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

<span data-ttu-id="1c833-137">Migrace volání `Up` metody k implementaci změny modelu dat pro migraci.</span><span class="sxs-lookup"><span data-stu-id="1c833-137">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="1c833-138">Když zadáte příkaz k vrácení aktualizace, migrace volání `Down` metoda.</span><span class="sxs-lookup"><span data-stu-id="1c833-138">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="1c833-139">Předchozí kód je počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="1c833-139">The preceding code is for the initial migration.</span></span> <span data-ttu-id="1c833-140">Tento kód byl vytvořen, když `migrations add InitialCreate` byl spuštěn příkaz.</span><span class="sxs-lookup"><span data-stu-id="1c833-140">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="1c833-141">Parametr name migrace ("InitialCreate" v příkladu) se používá pro název souboru.</span><span class="sxs-lookup"><span data-stu-id="1c833-141">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="1c833-142">Název migrace může být jakýkoli název platný soubor.</span><span class="sxs-lookup"><span data-stu-id="1c833-142">The migration name can be any valid file name.</span></span> <span data-ttu-id="1c833-143">Doporučujeme vybrat slovo nebo frázi, která shrnuje, co probíhá při migraci.</span><span class="sxs-lookup"><span data-stu-id="1c833-143">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="1c833-144">Například migrace, který přidat tabulku oddělení může být například "AddDepartmentTable."</span><span class="sxs-lookup"><span data-stu-id="1c833-144">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="1c833-145">Pokud počáteční migrace je vytvořen a existuje databáze:</span><span class="sxs-lookup"><span data-stu-id="1c833-145">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="1c833-146">Generování kódu vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="1c833-146">The DB creation code is generated.</span></span>
* <span data-ttu-id="1c833-147">Kód pro vytvoření databáze není nutné spustit, protože databáze již odpovídá datový model.</span><span class="sxs-lookup"><span data-stu-id="1c833-147">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="1c833-148">Pokud kód vytvoření DB běží, nemá proveďte požadované změny, protože databáze již odpovídá datový model.</span><span class="sxs-lookup"><span data-stu-id="1c833-148">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="1c833-149">Pokud chcete aplikaci nasadit do nového prostředí, pro vytvoření databáze musíte spustit kód pro vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="1c833-149">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="1c833-150">Dříve databáze byla vyřazena a neexistuje, takže migrace vytvoří novou databázi.</span><span class="sxs-lookup"><span data-stu-id="1c833-150">Previously the DB was dropped and doesn't exist, so migrations creates the new DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="1c833-151">Snímek dat modelu</span><span class="sxs-lookup"><span data-stu-id="1c833-151">The data model snapshot</span></span>

<span data-ttu-id="1c833-152">Vytvoření migrace *snímku* z aktuální schéma databáze v *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="1c833-152">Migrations create a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="1c833-153">Když přidáte migrace, EF Určuje, co se změnilo tak, že porovnáte datový model, který soubor snímku.</span><span class="sxs-lookup"><span data-stu-id="1c833-153">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="1c833-154">K odstranění migrace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1c833-154">To delete a migration, use the following command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c833-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c833-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1c833-156">Odebrat migrace</span><span class="sxs-lookup"><span data-stu-id="1c833-156">Remove-Migration</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1c833-157">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="1c833-157">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

<span data-ttu-id="1c833-158">Další informace najdete v tématu [odebrat dotnet ef migrace](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="1c833-158">For more information, see  [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span></span>

------

<span data-ttu-id="1c833-159">Příkaz remove migrace odstraní migrace a zajišťuje, že je správně obnovení snímku.</span><span class="sxs-lookup"><span data-stu-id="1c833-159">The remove migrations command deletes the migration and ensures the snapshot is correctly reset.</span></span>

### <a name="remove-ensurecreated-and-test-the-app"></a><span data-ttu-id="1c833-160">Odeberte EnsureCreated a testování aplikací</span><span class="sxs-lookup"><span data-stu-id="1c833-160">Remove EnsureCreated and test the app</span></span>

<span data-ttu-id="1c833-161">Pro včasné vývoj `EnsureCreated` byl použit.</span><span class="sxs-lookup"><span data-stu-id="1c833-161">For early development, `EnsureCreated` was used.</span></span> <span data-ttu-id="1c833-162">V tomto kurzu se používají migrace.</span><span class="sxs-lookup"><span data-stu-id="1c833-162">In this tutorial, migrations are used.</span></span> <span data-ttu-id="1c833-163">`EnsureCreated` má následující omezení:</span><span class="sxs-lookup"><span data-stu-id="1c833-163">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="1c833-164">Obchází migrace a vytvoří databáze a schéma.</span><span class="sxs-lookup"><span data-stu-id="1c833-164">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="1c833-165">Nelze vytvořit tabulku migrace.</span><span class="sxs-lookup"><span data-stu-id="1c833-165">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="1c833-166">Můžete *není* použít s migrací.</span><span class="sxs-lookup"><span data-stu-id="1c833-166">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="1c833-167">Je určený pro testování nebo rychlé vytváření prototypů kde je databáze vyřadit a znovu vytvořit často.</span><span class="sxs-lookup"><span data-stu-id="1c833-167">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="1c833-168">Odebrat následující řádek z `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="1c833-168">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

<span data-ttu-id="1c833-169">Spusťte aplikaci a ověřte, že databáze je nasadí.</span><span class="sxs-lookup"><span data-stu-id="1c833-169">Run the app and verify the DB is seeded.</span></span>

### <a name="inspect-the-database"></a><span data-ttu-id="1c833-170">Zkontrolujte databáze</span><span class="sxs-lookup"><span data-stu-id="1c833-170">Inspect the database</span></span>

<span data-ttu-id="1c833-171">Použití **Průzkumník objektů systému SQL Server** Kontrola databáze.</span><span class="sxs-lookup"><span data-stu-id="1c833-171">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="1c833-172">Všimněte si, přidání `__EFMigrationsHistory` tabulky.</span><span class="sxs-lookup"><span data-stu-id="1c833-172">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="1c833-173">`__EFMigrationsHistory` Tabulky uchovává informace o migrace, které byly použity k databázi.</span><span class="sxs-lookup"><span data-stu-id="1c833-173">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="1c833-174">Zobrazení dat v `__EFMigrationsHistory` tabulky, zobrazuje jeden řádek na první migraci.</span><span class="sxs-lookup"><span data-stu-id="1c833-174">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="1c833-175">V poslední protokolu v předchozím příkladu výstupu rozhraní příkazového řádku se zobrazuje příkaz INSERT, která vytváří tento řádek.</span><span class="sxs-lookup"><span data-stu-id="1c833-175">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="1c833-176">Spusťte aplikaci a ověřte, že všechno funguje.</span><span class="sxs-lookup"><span data-stu-id="1c833-176">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="1c833-177">Použití migrace v produkčním prostředí</span><span class="sxs-lookup"><span data-stu-id="1c833-177">Applying migrations in production</span></span>

<span data-ttu-id="1c833-178">Doporučujeme, abyste měli produkční aplikace **není** volání [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c833-178">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="1c833-179">`Migrate` nelze volat z aplikace v serverové farmě.</span><span class="sxs-lookup"><span data-stu-id="1c833-179">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="1c833-180">Například pokud aplikace bylo cloudové nasazení se Škálováním na více systémů (více instancí aplikace běží).</span><span class="sxs-lookup"><span data-stu-id="1c833-180">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="1c833-181">V rámci nasazení a řízené způsobem se má provést migrace databáze.</span><span class="sxs-lookup"><span data-stu-id="1c833-181">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="1c833-182">Provozní databáze migrace přístupy patří:</span><span class="sxs-lookup"><span data-stu-id="1c833-182">Production database migration approaches include:</span></span>

* <span data-ttu-id="1c833-183">Pomocí migrace vytvořit skripty SQL a pomocí skriptů SQL v nasazení.</span><span class="sxs-lookup"><span data-stu-id="1c833-183">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="1c833-184">Spuštění `dotnet ef database update` z řízené prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c833-184">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="1c833-185">Základní EF používá `__MigrationsHistory` tabulce najdete, pokud žádné migrace muset spustit.</span><span class="sxs-lookup"><span data-stu-id="1c833-185">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="1c833-186">Pokud je aktuální databáze, je spustit žádné migrace.</span><span class="sxs-lookup"><span data-stu-id="1c833-186">If the DB is up-to-date, no migration is run.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1c833-187">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="1c833-187">Troubleshooting</span></span>

<span data-ttu-id="1c833-188">Stažení [dokončené aplikace](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="1c833-188">Download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="1c833-189">Aplikace generuje následující výjimky:</span><span class="sxs-lookup"><span data-stu-id="1c833-189">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="1c833-190">Řešení: spuštění `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="1c833-190">Solution: Run `dotnet ef database update`</span></span>

### <a name="additional-resources"></a><span data-ttu-id="1c833-191">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1c833-191">Additional resources</span></span>

* <span data-ttu-id="1c833-192">[.NET core rozhraní příkazového řádku](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="1c833-192">[.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>
* [<span data-ttu-id="1c833-193">Konzola Správce balíčků (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="1c833-193">Package Manager Console (Visual Studio)</span></span>](/ef/core/miscellaneous/cli/powershell)

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="1c833-194">[Předchozí](xref:data/ef-rp/sort-filter-page)
> [další](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="1c833-194">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>