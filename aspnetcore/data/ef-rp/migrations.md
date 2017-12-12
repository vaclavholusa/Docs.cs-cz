---
title: "Stránky Razor s EF Core - Migrations - 4 8"
author: rick-anderson
description: "V tomto kurzu začnete používat funkci migrace EF jádra pro správu změn datových modelů v aplikaci ASP.NET MVC jádra."
keywords: ASP.NET Core Entity Framework Core, migrace
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 8549fc522bcd05a5755a449cd6d4b655f00d998b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/02/2017
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a><span data-ttu-id="c8226-104">Migrace – základní EF s stránky Razor kurzu (4 8)</span><span class="sxs-lookup"><span data-stu-id="c8226-104">Migrations - EF Core with Razor Pages tutorial (4 of 8)</span></span>

<span data-ttu-id="c8226-105">Podle [tní Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c8226-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="c8226-106">V tomto kurzu se používá funkci EF základní migrace pro správu změn datových modelů.</span><span class="sxs-lookup"><span data-stu-id="c8226-106">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="c8226-107">Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="c8226-107">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="c8226-108">Když je vyvinut novou aplikaci, model dat často změny.</span><span class="sxs-lookup"><span data-stu-id="c8226-108">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="c8226-109">Pokaždé, když změny modelu modelu získá synchronizována s databází.</span><span class="sxs-lookup"><span data-stu-id="c8226-109">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="c8226-110">Tento kurz spuštění nakonfigurováním rozhraní Entity Framework pro vytvoření databáze, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c8226-110">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="c8226-111">Pokaždé, když datový model změny:</span><span class="sxs-lookup"><span data-stu-id="c8226-111">Each time the data model changes:</span></span>

* <span data-ttu-id="c8226-112">Databáze je vyřazeno.</span><span class="sxs-lookup"><span data-stu-id="c8226-112">The DB is dropped.</span></span>
* <span data-ttu-id="c8226-113">EF vytvoří novou, který neodpovídá modelu.</span><span class="sxs-lookup"><span data-stu-id="c8226-113">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="c8226-114">Aplikace doplňuje databáze s testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="c8226-114">The app seeds the DB with test data.</span></span>

<span data-ttu-id="c8226-115">Tento přístup k udržování databáze synchronizace s datovým modelem funguje dobře, dokud můžete aplikaci nasadit do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8226-115">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="c8226-116">Když aplikace běží v produkčním prostředí, je obvykle ukládá data, která je třeba zachovat.</span><span class="sxs-lookup"><span data-stu-id="c8226-116">When the app is running in production, it is usually storing data that needs to be maintained.</span></span> <span data-ttu-id="c8226-117">Aplikace nemůže začínat testu DB pokaždé, když dojde ke změně (jako je například přidávání nové sloupce).</span><span class="sxs-lookup"><span data-stu-id="c8226-117">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="c8226-118">Tento problém řeší funkci migrace základní EF povolením EF základní aktualizovat schéma databáze místo vytvoření nové databáze.</span><span class="sxs-lookup"><span data-stu-id="c8226-118">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="c8226-119">Namísto vyřadit a znovu vytvořit databázi, když datový model změny, migrace aktualizace schématu a uchovává existující data.</span><span class="sxs-lookup"><span data-stu-id="c8226-119">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="c8226-120">Entity Framework Core NuGet balíčky pro migrace</span><span class="sxs-lookup"><span data-stu-id="c8226-120">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="c8226-121">Chcete-li pracovat s migrací, použijte **Konzola správce balíčků** (pomocí PMC) nebo rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="c8226-121">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="c8226-122">Tyto kurzy ukazují, jak používat rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c8226-122">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="c8226-123">Informace o pomocí PMC je na [konci tohoto kurzu](#pmc).</span><span class="sxs-lookup"><span data-stu-id="c8226-123">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="c8226-124">Nástroje EF jádra pro rozhraní příkazového řádku (CLI) jsou uvedeny v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="c8226-124">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="c8226-125">K instalaci tohoto balíčku, přidejte ho do `DotNetCliToolReference` kolekce v *.csproj* souboru, jak je vidět.</span><span class="sxs-lookup"><span data-stu-id="c8226-125">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="c8226-126">**Poznámka:** tento balíček musí být nainstalována úpravou *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="c8226-126">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="c8226-127">`install-package` Příkaz nebo Správce balíčků grafické uživatelské rozhraní nelze použít k instalaci tohoto balíčku.</span><span class="sxs-lookup"><span data-stu-id="c8226-127">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="c8226-128">Upravit *.csproj* kliknutím pravým tlačítkem myši na název projektu v souboru **Průzkumníku řešení** a výběrem **upravit ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="c8226-128">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="c8226-129">Následující kód ukazuje aktualizovaný *.csproj* soubor s EF základní rozhraní příkazového řádku nástroje zvýrazněná:</span><span class="sxs-lookup"><span data-stu-id="c8226-129">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="c8226-130">Čísla verzí v předchozím příkladu byly aktuální v době kurzu byla zapsána.</span><span class="sxs-lookup"><span data-stu-id="c8226-130">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="c8226-131">Použijte stejnou verzi pro EF základní rozhraní příkazového řádku nástroje v dalších balíčků.</span><span class="sxs-lookup"><span data-stu-id="c8226-131">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="c8226-132">Změnit připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="c8226-132">Change the connection string</span></span>

<span data-ttu-id="c8226-133">V *appSettings.JSON určený* souboru, změňte název databáze v připojovacím řetězci ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="c8226-133">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="c8226-134">Změna názvu DB v připojovacím řetězci způsobí, že první migrace k vytvoření nové databáze.</span><span class="sxs-lookup"><span data-stu-id="c8226-134">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="c8226-135">Nové databáze je vytvořit, protože s tímto názvem neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c8226-135">A new DB is created because one with that name does not exist.</span></span> <span data-ttu-id="c8226-136">Změna připojovací řetězec není nutné u Začínáme s migrací.</span><span class="sxs-lookup"><span data-stu-id="c8226-136">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="c8226-137">Alternativu ke změně názvu databáze je odstranění databáze.</span><span class="sxs-lookup"><span data-stu-id="c8226-137">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="c8226-138">Použití **Průzkumník objektů systému SQL Server** (SSOX) nebo `database drop` rozhraní příkazového řádku příkaz:</span><span class="sxs-lookup"><span data-stu-id="c8226-138">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="c8226-139">V následující části vysvětluje, jak spouštět příkazy příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c8226-139">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="c8226-140">Vytvoření počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="c8226-140">Create an initial migration</span></span>

<span data-ttu-id="c8226-141">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="c8226-141">Build the project.</span></span>

<span data-ttu-id="c8226-142">Otevřete okno příkazového řádku a přejděte do složky projektu.</span><span class="sxs-lookup"><span data-stu-id="c8226-142">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="c8226-143">Obsahuje složky projektu *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="c8226-143">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="c8226-144">Zadejte v příkazovém okně:</span><span class="sxs-lookup"><span data-stu-id="c8226-144">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="c8226-145">Příkazové okno se zobrazí podobná následující informace:</span><span class="sxs-lookup"><span data-stu-id="c8226-145">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="c8226-146">Pokud migrace selže se zprávou "*nemůže přistupovat k souboru... ContosoUniversity.dll vzhledem k tomu, že je stále používán jiným procesem.* "</span><span class="sxs-lookup"><span data-stu-id="c8226-146">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="c8226-147">Zobrazí se:</span><span class="sxs-lookup"><span data-stu-id="c8226-147">is displayed:</span></span>

* <span data-ttu-id="c8226-148">Zastavte službu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c8226-148">Stop IIS Express.</span></span>

   * <span data-ttu-id="c8226-149">Ukončete a restartujte Visual Studio, nebo</span><span class="sxs-lookup"><span data-stu-id="c8226-149">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="c8226-150">Najít ikonu IIS Express na hlavním panelu systému Windows.</span><span class="sxs-lookup"><span data-stu-id="c8226-150">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="c8226-151">Klikněte pravým tlačítkem na ikonu služby IIS Express a pak klikněte na tlačítko **ContosoUniversity > Zastavit lokality**.</span><span class="sxs-lookup"><span data-stu-id="c8226-151">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="c8226-152">Pokud chybová zpráva "sestavení se nezdařilo."</span><span class="sxs-lookup"><span data-stu-id="c8226-152">If the error message "Build failed."</span></span> <span data-ttu-id="c8226-153">Zobrazí se, spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="c8226-153">is displayed, run the command again.</span></span> <span data-ttu-id="c8226-154">Pokud se tato chyba, nechte poznámku v dolní části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c8226-154">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="c8226-155">Zkontrolujte nahoru a dolů metody</span><span class="sxs-lookup"><span data-stu-id="c8226-155">Examine the Up and Down methods</span></span>

<span data-ttu-id="c8226-156">Příkaz EF základní `migrations add` generovaného kódu k vytvoření databáze z.</span><span class="sxs-lookup"><span data-stu-id="c8226-156">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="c8226-157">Tento kód migrace je v *migrace\<časové razítko > _InitialCreate.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="c8226-157">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="c8226-158">`Up` Metodu `InitialCreate` třída vytvoří DB tabulky, které odpovídají sady dat modelu entity.</span><span class="sxs-lookup"><span data-stu-id="c8226-158">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="c8226-159">`Down` Metoda odstraní, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c8226-159">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="c8226-160">Migrace volání `Up` metody k implementaci změny modelu dat pro migraci.</span><span class="sxs-lookup"><span data-stu-id="c8226-160">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="c8226-161">Když zadáte příkaz k vrácení aktualizace, migrace volání `Down` metoda.</span><span class="sxs-lookup"><span data-stu-id="c8226-161">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="c8226-162">Předchozí kód je počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="c8226-162">The preceding code is for the initial migration.</span></span> <span data-ttu-id="c8226-163">Tento kód byl vytvořen, když `migrations add InitialCreate` byl spuštěn příkaz.</span><span class="sxs-lookup"><span data-stu-id="c8226-163">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="c8226-164">Parametr name migrace ("InitialCreate" v příkladu) se používá pro název souboru.</span><span class="sxs-lookup"><span data-stu-id="c8226-164">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="c8226-165">Název migrace může být jakýkoli název platný soubor.</span><span class="sxs-lookup"><span data-stu-id="c8226-165">The migration name can be any valid file name.</span></span> <span data-ttu-id="c8226-166">Doporučujeme vybrat slovo nebo frázi, která shrnuje, co probíhá při migraci.</span><span class="sxs-lookup"><span data-stu-id="c8226-166">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="c8226-167">Například migrace, který přidat tabulku oddělení může být například "AddDepartmentTable."</span><span class="sxs-lookup"><span data-stu-id="c8226-167">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="c8226-168">Pokud se vytvoří počáteční migrace a ukončí databáze:</span><span class="sxs-lookup"><span data-stu-id="c8226-168">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="c8226-169">Generování kódu vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="c8226-169">The DB creation code is generated.</span></span>
* <span data-ttu-id="c8226-170">Kód pro vytvoření databáze není nutné spustit, protože databáze již odpovídá datový model.</span><span class="sxs-lookup"><span data-stu-id="c8226-170">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="c8226-171">Pokud kód DB vytvoření běží, nemá proveďte požadované změny, protože databáze již odpovídá datový model.</span><span class="sxs-lookup"><span data-stu-id="c8226-171">If the The DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="c8226-172">Pokud chcete aplikaci nasadit do nového prostředí, pro vytvoření databáze musíte spustit kód pro vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="c8226-172">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="c8226-173">Připojovací řetězec dříve bylo změněno používat nový název databáze.</span><span class="sxs-lookup"><span data-stu-id="c8226-173">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="c8226-174">Zadaná databáze neexistuje, vytvoří migrace databáze.</span><span class="sxs-lookup"><span data-stu-id="c8226-174">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="c8226-175">Zkontrolujte snímku modelu dat</span><span class="sxs-lookup"><span data-stu-id="c8226-175">Examine the data model snapshot</span></span>

<span data-ttu-id="c8226-176">Vytvoří migrace *snímku* z aktuální schéma databáze v *Migrations/SchoolContextModelSnapshot.cs*:</span><span class="sxs-lookup"><span data-stu-id="c8226-176">Migrations creates a *snapshot* of the current DB schema in *Migrations/SchoolContextModelSnapshot.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="c8226-177">Vzhledem k tomu, že aktuální schéma databáze je reprezentována v kódu, EF základní nemá pro interakci s DB vytvořit migrace.</span><span class="sxs-lookup"><span data-stu-id="c8226-177">Because the current DB schema is represented in code, EF Core doesn't have to interact with the DB to create migrations.</span></span> <span data-ttu-id="c8226-178">Když přidáte migrace, EF základní Určuje, co se změnilo tak, že porovnáte datový model, který soubor snímku.</span><span class="sxs-lookup"><span data-stu-id="c8226-178">When you add a migration, EF Core determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="c8226-179">Základní EF komunikuje s databáze jenom v případě, že má k aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="c8226-179">EF Core interacts with the DB only when it has to update the DB.</span></span>

<span data-ttu-id="c8226-180">Soubor snímku musí být synchronizována s migrací, která ji vytvořila.</span><span class="sxs-lookup"><span data-stu-id="c8226-180">The snapshot file must be in sync with the migrations that created it.</span></span> <span data-ttu-id="c8226-181">Migrace nelze odebrat odstraněním soubor s názvem  *\<časové razítko > _\<migrationname > .cs*.</span><span class="sxs-lookup"><span data-stu-id="c8226-181">A migration can't be removed by deleting the file named *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="c8226-182">Pokud daný soubor odstraněn, zbývající migrace nejsou synchronizované s soubor snímku databáze.</span><span class="sxs-lookup"><span data-stu-id="c8226-182">If that file is deleted, the remaining migrations are out of sync with the DB snapshot file.</span></span> <span data-ttu-id="c8226-183">Chcete-li odstranit poslední migrace přidali, použijte [odebrat dotnet ef migrace](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c8226-183">To delete the last migration added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="c8226-184">Odebrat EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="c8226-184">Remove EnsureCreated</span></span>

<span data-ttu-id="c8226-185">Pro včasné vývoj `EnsureCreated` příkaz nebyl použit.</span><span class="sxs-lookup"><span data-stu-id="c8226-185">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="c8226-186">V tomto kurzu se používá migrace.</span><span class="sxs-lookup"><span data-stu-id="c8226-186">In this tutorial, migrations is used.</span></span> <span data-ttu-id="c8226-187">`EnsureCreated`má následující limatitions:</span><span class="sxs-lookup"><span data-stu-id="c8226-187">`EnsureCreated` has the following limatitions:</span></span>

* <span data-ttu-id="c8226-188">Obchází migrace a vytvoří databáze a schéma.</span><span class="sxs-lookup"><span data-stu-id="c8226-188">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="c8226-189">Nevytváří migrace tabulky.</span><span class="sxs-lookup"><span data-stu-id="c8226-189">Does not create a migrations table.</span></span>
* <span data-ttu-id="c8226-190">Můžete *není* použít s migrací.</span><span class="sxs-lookup"><span data-stu-id="c8226-190">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="c8226-191">Je určený pro testování nebo rychlé vytváření prototypů kde je databáze vyřadit a znovu vytvořit často.</span><span class="sxs-lookup"><span data-stu-id="c8226-191">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="c8226-192">Odebrat následující řádek z `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="c8226-192">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="c8226-193">Použití migrace k databázi v vývoj</span><span class="sxs-lookup"><span data-stu-id="c8226-193">Apply the migration to the DB in development</span></span>

<span data-ttu-id="c8226-194">V okně příkazového řádku zadejte následující příkaz pro vytvoření databáze a tabulky.</span><span class="sxs-lookup"><span data-stu-id="c8226-194">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="c8226-195">Poznámka: Pokud `update` příkaz vrátí chybu "Sestavení se nezdařilo.":</span><span class="sxs-lookup"><span data-stu-id="c8226-195">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="c8226-196">Příkaz spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="c8226-196">Run the command again.</span></span>
* <span data-ttu-id="c8226-197">Pokud se znovu nezdaří, Visual Studio ukončete a spusťte `update` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c8226-197">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="c8226-198">Ponechte zprávu v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="c8226-198">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="c8226-199">Výstup z tohoto příkazu je podobná `migrations add` příkaz výstupu.</span><span class="sxs-lookup"><span data-stu-id="c8226-199">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="c8226-200">V předchozím příkazu se zobrazí protokoly pro příkazy SQL, které nastavení databáze.</span><span class="sxs-lookup"><span data-stu-id="c8226-200">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="c8226-201">Většina protokolů se tento parametr vynechán následující ukázkový výstup:</span><span class="sxs-lookup"><span data-stu-id="c8226-201">Most of the logs are omitted in the following sample output:</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="c8226-202">Snížit úroveň podrobností ve zprávách protokolu, můžete změnit úroveň protokolu v *appsettings. Development.JSON* souboru.</span><span class="sxs-lookup"><span data-stu-id="c8226-202">To reduce the level of detail in log messages, can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="c8226-203">Další informace najdete v tématu [Úvod k protokolování](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="c8226-203">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="c8226-204">Použití **Průzkumník objektů systému SQL Server** Kontrola databáze.</span><span class="sxs-lookup"><span data-stu-id="c8226-204">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="c8226-205">Všimněte si, přidání `__EFMigrationsHistory` tabulky.</span><span class="sxs-lookup"><span data-stu-id="c8226-205">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="c8226-206">`__EFMigrationsHistory` Tabulky uchovává informace o migrace, které byly použity k databázi.</span><span class="sxs-lookup"><span data-stu-id="c8226-206">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="c8226-207">Zobrazení dat v `__EFMigrationsHistory` tabulky, zobrazuje jeden řádek na první migraci.</span><span class="sxs-lookup"><span data-stu-id="c8226-207">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="c8226-208">V poslední protokolu v předchozím příkladu výstupu rozhraní příkazového řádku se zobrazuje příkaz INSERT, která vytváří tento řádek.</span><span class="sxs-lookup"><span data-stu-id="c8226-208">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="c8226-209">Spusťte aplikaci a ověřte, že všechno funguje.</span><span class="sxs-lookup"><span data-stu-id="c8226-209">Run the app and verify that everything works.</span></span>

## <a name="appling-migrations-in-production"></a><span data-ttu-id="c8226-210">Migrace appling v produkčním prostředí</span><span class="sxs-lookup"><span data-stu-id="c8226-210">Appling migrations in production</span></span>

<span data-ttu-id="c8226-211">Doporučujeme, abyste měli produkční aplikace **není** volání [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8226-211">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="c8226-212">`Migrate`nelze volat z aplikace v serverové farmě.</span><span class="sxs-lookup"><span data-stu-id="c8226-212">`Migrate` should not be called from an app in server farm.</span></span> <span data-ttu-id="c8226-213">Například pokud aplikace bylo cloudové nasazení se Škálováním na více systémů (více instancí aplikace běží).</span><span class="sxs-lookup"><span data-stu-id="c8226-213">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="c8226-214">V rámci nasazení a řízené způsobem se má provést migrace databáze.</span><span class="sxs-lookup"><span data-stu-id="c8226-214">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="c8226-215">Provozní databáze migrace přístupy patří:</span><span class="sxs-lookup"><span data-stu-id="c8226-215">Production database migration approaches include:</span></span>

* <span data-ttu-id="c8226-216">Pomocí migrace vytvořit skripty SQL a pomocí skriptů SQL v nasazení.</span><span class="sxs-lookup"><span data-stu-id="c8226-216">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="c8226-217">Spuštění `dotnet ef database update` z řízené prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8226-217">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="c8226-218">Základní EF používá `__MigrationsHistory` tabulce najdete, pokud žádné migrace muset spustit.</span><span class="sxs-lookup"><span data-stu-id="c8226-218">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="c8226-219">Pokud je aktuální databáze, je spustit žádné migrace.</span><span class="sxs-lookup"><span data-stu-id="c8226-219">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="c8226-220">Rozhraní příkazového řádku (CLI) vs. Konzola správce balíčků (pomocí PMC)</span><span class="sxs-lookup"><span data-stu-id="c8226-220">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="c8226-221">Základní EF nástrojů pro správu migrace najdete na webu:</span><span class="sxs-lookup"><span data-stu-id="c8226-221">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="c8226-222">.NET core rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c8226-222">.NET Core CLI commands.</span></span>
* <span data-ttu-id="c8226-223">Rutiny prostředí PowerShell v sadě Visual Studio **Konzola správce balíčků** okno (pomocí PMC).</span><span class="sxs-lookup"><span data-stu-id="c8226-223">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="c8226-224">Tento kurz ukazuje, jak používat rozhraní příkazového řádku, někteří vývojáři přednost používání pomocí PMC.</span><span class="sxs-lookup"><span data-stu-id="c8226-224">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="c8226-225">Základní EF příkazů pro pomocí PMC jsou v [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) balíčku.</span><span class="sxs-lookup"><span data-stu-id="c8226-225">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="c8226-226">Tento balíček je součástí [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, takže není nutné ji nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="c8226-226">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="c8226-227">**Důležité:** toto není stejného balíčku jako instalace pro rozhraní příkazového řádku úpravou *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="c8226-227">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="c8226-228">Název touto končí v `Tools`, na rozdíl od název balíčku rozhraní příkazového řádku, které končí na `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="c8226-228">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="c8226-229">Další informace o rozhraní příkazového řádku najdete v tématu [.NET Core rozhraní příkazového řádku](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="c8226-229">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="c8226-230">Další informace o příkazech pomocí PMC najdete v tématu [Konzola správce balíčků (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="c8226-230">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c8226-231">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="c8226-231">Troubleshooting</span></span>

<span data-ttu-id="c8226-232">Stažení [dokončené aplikace pro tuto fázi](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="c8226-232">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="c8226-233">Aplikace generuje následující výjimky:</span><span class="sxs-lookup"><span data-stu-id="c8226-233">The app generates the following exception:</span></span>

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="c8226-234">Řešení: spuštění`dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="c8226-234">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="c8226-235">Pokud `update` příkaz vrátí chybu "Sestavení se nezdařilo.":</span><span class="sxs-lookup"><span data-stu-id="c8226-235">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="c8226-236">Příkaz spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="c8226-236">Run the command again.</span></span>
* <span data-ttu-id="c8226-237">Ponechte zprávu v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="c8226-237">Leave a message at the bottom of the page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c8226-238">[Předchozí](xref:data/ef-rp/sort-filter-page)
[další](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="c8226-238">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
