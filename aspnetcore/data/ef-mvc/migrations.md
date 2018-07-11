---
title: ASP.NET Core MVC s EF Core – migrace - 4 z 10
author: rick-anderson
description: V tomto kurzu začnete používat funkci migrace EF Core ke správě změn datových modelů v aplikaci ASP.NET Core MVC.
ms.author: tdykstra
ms.date: 03/15/2018
uid: data/ef-mvc/migrations
ms.openlocfilehash: f710b33ac1a6017b0e3d7e8c3e528675a41424bb
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38194167"
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a><span data-ttu-id="8b6b1-103">ASP.NET Core MVC s EF Core – migrace - 4 z 10</span><span class="sxs-lookup"><span data-stu-id="8b6b1-103">ASP.NET Core MVC with EF Core - Migrations - 4 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8b6b1-104">Podle [Petr Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8b6b1-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8b6b1-105">Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET Core MVC pomocí Entity Framework Core a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="8b6b1-106">Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="8b6b1-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="8b6b1-107">V tomto kurzu začnete používat funkci migrace EF Core ke správě změn datových modelů.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="8b6b1-108">V dalších kurzech přidáte další migrace po provedení změny datového modelu.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="8b6b1-109">Úvod do migrace</span><span class="sxs-lookup"><span data-stu-id="8b6b1-109">Introduction to migrations</span></span>

<span data-ttu-id="8b6b1-110">Při vývoji nových aplikací, datového modelu mění často a pokaždé, když změny modelu, získá synchronizován s databází.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="8b6b1-111">Tyto kurzy spustil(a) konfigurace technologie Entity Framework pro vytvoření databáze, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="8b6b1-112">Pokaždé, když změníte datový model – přidat, odebrat, nebo změňte tříd entit nebo změnit vaší třídy DbContext – potom můžete odstranit databázi a EF vytvoří nový, který odpovídá modelu a nasazení se nasazuje s testovací data.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="8b6b1-113">Tato metoda zachování databáze synchronizované s datovým modelem funguje dobře, dokud nasadit aplikaci do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="8b6b1-114">Když je aplikace spuštěna v produkčním prostředí je obvykle ukládá data, která chcete zachovat, a nechcete ztratit všechno, co pokaždé, když provedete změnu např. přidejte nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-114">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="8b6b1-115">Funkce migrace EF Core tento problém řeší tím, že EF aktualizovat schéma databáze místo vytvoření nové databáze.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="8b6b1-116">Entity Framework Core NuGet balíčky pro migrace</span><span class="sxs-lookup"><span data-stu-id="8b6b1-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="8b6b1-117">Chcete-li pracovat s migrací, můžete použít **Konzola správce balíčků** (PMC) nebo rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="8b6b1-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="8b6b1-118">Tyto kurzy vám ukážou, jak používat příkazy rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="8b6b1-119">Informace o konzole PMC je na [konci tohoto kurzu](#pmc).</span><span class="sxs-lookup"><span data-stu-id="8b6b1-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="8b6b1-120">EF nástroje pro rozhraní příkazového řádku (CLI) jsou k dispozici v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="8b6b1-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="8b6b1-121">K instalaci tohoto balíčku, přidejte ji tak `DotNetCliToolReference` kolekce v *.csproj* souboru, jak je znázorněno.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="8b6b1-122">**Poznámka:** je potřeba nainstalovat tento balíček úpravou *.csproj* soubor; nelze použít `install-package` příkaz nebo grafické uživatelské rozhraní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="8b6b1-123">Můžete upravit *.csproj* kliknutím pravým tlačítkem myši na název projektu v souboru **Průzkumníka řešení** a vyberete **upravit ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="8b6b1-124">(Čísla verze v tomto příkladu byly aktuální v době tento kurz je napsán.)</span><span class="sxs-lookup"><span data-stu-id="8b6b1-124">(The version numbers in this example were current when the tutorial was written.)</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="8b6b1-125">Změňte připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="8b6b1-125">Change the connection string</span></span>

<span data-ttu-id="8b6b1-126">V *appsettings.json* změňte název databáze v připojovacím řetězci ContosoUniversity2 nebo jiný název, který jste ještě nepoužívali v počítači, který používáte.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="8b6b1-127">Tato změna nastaví projekt tak, aby první migrací vytvoří novou databázi.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="8b6b1-128">To není nutné, abyste mohli začít s migrací, ale zobrazí se vám později důvod, proč je vhodné.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-128">This isn't required to get started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="8b6b1-129">Jako alternativu k změně názvu databáze je odstranit databázi.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="8b6b1-130">Použití **Průzkumník objektů systému SQL Server** (SSOX) nebo `database drop` příkazu rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="8b6b1-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="8b6b1-131">Následující část vysvětluje, jak spouštět příkazy rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="8b6b1-132">Vytvoření počáteční migraci</span><span class="sxs-lookup"><span data-stu-id="8b6b1-132">Create an initial migration</span></span>

<span data-ttu-id="8b6b1-133">Uložte změny a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-133">Save your changes and build the project.</span></span> <span data-ttu-id="8b6b1-134">Pak otevřete okno příkazového řádku a přejděte do složky projektu.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="8b6b1-135">Tady je rychlý způsob, jak to udělat:</span><span class="sxs-lookup"><span data-stu-id="8b6b1-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="8b6b1-136">V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt a zvolte **otevřít v Průzkumníkovi souborů** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Otevřít v Průzkumníku souborů položky nabídky](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="8b6b1-138">Do panelu Adresa zadejte "cmd" a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Otevřete příkazové okno](migrations/_static/open-command-window.png)

<span data-ttu-id="8b6b1-140">V příkazovém okně zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8b6b1-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="8b6b1-141">Zobrazí se výstup podobný tomuto v příkazovém okně:</span><span class="sxs-lookup"><span data-stu-id="8b6b1-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="8b6b1-142">Pokud se zobrazí chybová zpráva *žádná spustitelný soubor se nenašel odpovídající příkaz "dotnet-ef"*, naleznete v tématu [tento příspěvek na blogu](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) pomoc při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="8b6b1-143">Pokud se zobrazí chybová zpráva "*nelze získat přístup k souboru... ContosoUniversity.dll protože je používán jiným procesem.* ", vyhledejte službu IIS Express ikonu na hlavním panelu systému Windows a pravým tlačítkem myši a potom klikněte na tlačítko **ContosoUniversity > zastavení webu**.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="8b6b1-144">Prozkoumání nahoru a dolů metody</span><span class="sxs-lookup"><span data-stu-id="8b6b1-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="8b6b1-145">Při spouštění `migrations add` příkazu EF vygeneruje kód, který se vytvoří databáze od začátku.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="8b6b1-146">Tento kód je v *migrace* složku, v souboru s názvem  *\<časové razítko > _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="8b6b1-147">`Up` Metodu `InitialCreate` třída vytvoří databázové tabulky, které odpovídají sady entit datového modelu a `Down` metoda odstraní, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="8b6b1-148">Migrace volání `Up` metody k implementaci změn datových modelů pro migraci.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="8b6b1-149">Po zadání příkazu vrácení zpět aktualizace, volání migrace `Down` metody.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="8b6b1-150">Tento kód je pro počáteční migraci, která byla vytvořena, když jste zadali `migrations add InitialCreate` příkazu.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="8b6b1-151">Název parametru migrace ("InitialCreate" v příkladu) se používá pro název souboru a může být cokoliv, co chcete.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="8b6b1-152">Doporučujeme vybrat slovo nebo slovní spojení, které shrnuje, co se provádí v migraci.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="8b6b1-153">Můžete například pojmenovat pozdější migraci "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="8b6b1-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="8b6b1-154">Pokud jste vytvořili počáteční migraci databáze již existuje, vygeneruje kód pro vytvoření databáze, ale nemusí spustit, protože databáze již odpovídá datového modelu.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="8b6b1-155">Když nasadíte aplikaci do jiného prostředí, kde databáze neexistuje ale tento kód spustí k vytvoření databáze, proto je vhodné se nejdřív otestovat.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="8b6b1-156">To je důvod, proč jste změnili název databáze v připojovacím řetězci dříve – tak, aby migrace můžete vytvořit nový úplně od začátku.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="8b6b1-157">Snímek dat modelu</span><span class="sxs-lookup"><span data-stu-id="8b6b1-157">The data model snapshot</span></span>

<span data-ttu-id="8b6b1-158">Migrace vytvoří *snímku* aktuální schéma databáze v *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-158">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="8b6b1-159">Při přidání migrace EF Určuje, co se změnilo porovnáním datový model, který soubor snímku.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-159">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="8b6b1-160">Při odstranění migrace, použijte [migrace ef dotnet odebrat](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) příkazu.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-160">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="8b6b1-161">`dotnet ef migrations remove` Odstraní migraci a zajistí, že je správně obnovit snímek.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-161">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="8b6b1-162">Zobrazit [migrace EF Core v prostředí Team](/ef/core/managing-schemas/migrations/teams) Další informace o tom, jak použít soubor snímku.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-162">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="8b6b1-163">Použití migrace do databáze</span><span class="sxs-lookup"><span data-stu-id="8b6b1-163">Apply the migration to the database</span></span>

<span data-ttu-id="8b6b1-164">V příkazovém řádku zadejte následující příkaz k vytvoření databáze a tabulky v ní.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-164">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="8b6b1-165">Výstup z příkazu je podobný `migrations add` příkazu, s tím rozdílem, že pro SQL příkazy, které nastavení databáze naleznete v protokolech.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-165">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="8b6b1-166">Většina protokolů jsou vynechány v následujícím ukázkovém výstupu.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-166">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="8b6b1-167">Pokud nechcete zobrazit tato úroveň podrobností ve zprávách protokolu, můžete změnit úroveň protokolu v *appsettings. Development.JSON* souboru.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-167">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="8b6b1-168">Další informace najdete v tématu [Úvod k protokolování](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="8b6b1-168">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

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

<span data-ttu-id="8b6b1-169">Použití **Průzkumník objektů systému SQL Server** ke kontrole databáze, jako jste to udělali v prvním kurzu.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-169">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="8b6b1-170">Můžete si všimnout přidání __EFMigrationsHistory tabulku, která uchovává informace o migraci, které se použily k databázi.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-170">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="8b6b1-171">Zobrazení dat v této tabulce, zobrazí jeden řádek pro první migraci.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-171">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="8b6b1-172">(Poslední protokolu v předchozím příkladu výstupu rozhraní příkazového řádku se zobrazí, která vytváří tento řádek příkazu INSERT.)</span><span class="sxs-lookup"><span data-stu-id="8b6b1-172">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="8b6b1-173">Spuštění aplikace pro ověření, že všechno funguje stále stejná jako předtím.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-173">Run the application to verify that everything still works the same as before.</span></span>

![Studenti indexová stránka](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="8b6b1-175">Rozhraní příkazového řádku (CLI) vs. Konzola správce balíčků (PMC)</span><span class="sxs-lookup"><span data-stu-id="8b6b1-175">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="8b6b1-176">EF nástroje pro správu migrace je k dispozici z příkazů rozhraní příkazového řádku .NET Core nebo z rutin prostředí PowerShell v sadě Visual Studio **Konzola správce balíčků** okno (PMC).</span><span class="sxs-lookup"><span data-stu-id="8b6b1-176">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="8b6b1-177">Tento kurz ukazuje, jak používat rozhraní příkazového řádku, ale pokud dáváte přednost, můžete použít konzolu PMC.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-177">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="8b6b1-178">EF příkazů pro příkazy PMC jsou v [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) balíčku.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-178">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="8b6b1-179">Tento balíček je už součástí [metabalíček](xref:fundamentals/metapackage) Microsoft.aspnetcore.all, takže není nutné k jeho instalaci.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-179">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="8b6b1-180">**Důležité:** to není stejného balíčku, jako je instalace rozhraní příkazového řádku tak, že upravíte *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-180">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="8b6b1-181">Název tohoto objektu končí `Tools`, na rozdíl od názvu balíčku rozhraní příkazového řádku, které končí na `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-181">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="8b6b1-182">Další informace o příkazech rozhraní příkazového řádku najdete v tématu [rozhraní příkazového řádku .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="8b6b1-182">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="8b6b1-183">Další informace o příkazech PMC najdete v tématu [Konzola správce balíčků (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="8b6b1-183">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="8b6b1-184">Souhrn</span><span class="sxs-lookup"><span data-stu-id="8b6b1-184">Summary</span></span>

<span data-ttu-id="8b6b1-185">V tomto kurzu jste viděli, jak vytvořit a použít první migraci.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-185">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="8b6b1-186">V dalším kurzu se zobrazí za přibližně pohledu na pokročilejší témata tak, že rozbalíte datového modelu.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-186">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="8b6b1-187">Na cestě můžete vytvářet a použít další migrace.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-187">Along the way you'll create and apply additional migrations.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="8b6b1-188">[Předchozí](sort-filter-page.md)
> [další](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="8b6b1-188">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>
