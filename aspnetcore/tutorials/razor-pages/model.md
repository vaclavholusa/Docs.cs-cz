---
title: Přidání modelu do aplikace v ASP.NET Core Razor Pages
author: rick-anderson
description: Objevte, jak přidat třídy pro správu filmy v databázi pomocí Entity Framework Core (jádro EF).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 5cd1e08ac52d352be23a280419d7456f685a03ad
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045598"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="226a1-103">Přidání modelu do aplikace v ASP.NET Core Razor Pages</span><span class="sxs-lookup"><span data-stu-id="226a1-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="226a1-104">Přidání datového modelu</span><span class="sxs-lookup"><span data-stu-id="226a1-104">Add a data model</span></span>

<span data-ttu-id="226a1-105">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="226a1-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="226a1-106">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="226a1-106">Name the folder *Models*.</span></span>

<span data-ttu-id="226a1-107">Klikněte pravým tlačítkem myši *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="226a1-107">Right click the *Models* folder.</span></span> <span data-ttu-id="226a1-108">Vyberte **přidat** > **třídy**.</span><span class="sxs-lookup"><span data-stu-id="226a1-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="226a1-109">Název třídy **film** a nahraďte obsah `Movie` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="226a1-109">Name the class **Movie** and replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="226a1-110">Vygenerované uživatelské rozhraní Video modelu</span><span class="sxs-lookup"><span data-stu-id="226a1-110">Scaffold the movie model</span></span>

<span data-ttu-id="226a1-111">V této části je automaticky generovaný model video.</span><span class="sxs-lookup"><span data-stu-id="226a1-111">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="226a1-112">To znamená vytvoří nástroj pro generování uživatelského rozhraní stránky pro operace vytvoření, čtení, aktualizace a odstranění (CRUD) pro model video.</span><span class="sxs-lookup"><span data-stu-id="226a1-112">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="226a1-113">Vytvoření *stránek/filmy* složky:</span><span class="sxs-lookup"><span data-stu-id="226a1-113">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="226a1-114">V **Průzkumníka řešení**, klikněte pravým tlačítkem na *stránky* složky > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="226a1-114">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="226a1-115">Název složky *filmy*</span><span class="sxs-lookup"><span data-stu-id="226a1-115">Name the folder *Movies*</span></span>

<span data-ttu-id="226a1-116">V **Průzkumníka řešení**, klikněte pravým tlačítkem na *stránek/filmy* složky > **přidat** > **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="226a1-116">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Image z předchozích kroků.](model/_static/sca.png)

<span data-ttu-id="226a1-118">V **přidat vygenerované uživatelské rozhraní** dialogového okna, vyberte **stránky Razor pomocí Entity Frameworku (CRUD)** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="226a1-118">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Image z předchozích kroků.](model/_static/add_scaffold.png)

<span data-ttu-id="226a1-120">Dokončení **přidat stránky Razor pomocí Entity Frameworku (CRUD)** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="226a1-120">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="226a1-121">V **třída modelu** rozevírací seznam, vyberte **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="226a1-121">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="226a1-122">V **třída kontextu dat** řádek, vyberte **+** (plus) podepsat a přijměte vygenerovaný název **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="226a1-122">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="226a1-123">V **třída kontextu dat** rozevírací seznam, vyberte **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="226a1-123">In the **Data context class** drop down, select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="226a1-124">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="226a1-124">Select **Add**.</span></span>

![Image z předchozích kroků.](model/_static/arp.png)

<span data-ttu-id="226a1-126">Proces vygenerované uživatelské rozhraní vytvořit a změnit následující soubory:</span><span class="sxs-lookup"><span data-stu-id="226a1-126">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="226a1-127">Soubory vytvořené</span><span class="sxs-lookup"><span data-stu-id="226a1-127">Files created</span></span>

* <span data-ttu-id="226a1-128">*Stránky/filmy*: vytvoření, odstranění, podrobností, úpravy, Index.</span><span class="sxs-lookup"><span data-stu-id="226a1-128">*Pages/Movies*: Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="226a1-129">Tyto stránky je podrobně popsaný v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="226a1-129">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="226a1-130">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="226a1-130">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="226a1-131">Aktualizace souboru</span><span class="sxs-lookup"><span data-stu-id="226a1-131">File updates</span></span>

* <span data-ttu-id="226a1-132">*Startup.cs*: změny tohoto souboru jsou podrobně popsané v další části.</span><span class="sxs-lookup"><span data-stu-id="226a1-132">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="226a1-133">*appSettings.JSON*: přidat připojovací řetězec použitý pro připojení k místní databázi.</span><span class="sxs-lookup"><span data-stu-id="226a1-133">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="226a1-134">Prozkoumání kontextu registrovaný pomocí vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="226a1-134">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="226a1-135">ASP.NET Core využívá rozhraní [injektáž závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="226a1-135">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="226a1-136">Služby (například kontext EF Core databáze) jsou registrované pomocí vkládání závislostí při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="226a1-136">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="226a1-137">Komponenty, které vyžadují tyto služby (například stránky Razor) jsou k dispozici tyto služby prostřednictvím parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="226a1-137">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="226a1-138">Později v tomto kurzu se zobrazí kód konstruktor, který získá instanci kontext databáze.</span><span class="sxs-lookup"><span data-stu-id="226a1-138">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="226a1-139">Nástroj pro generování uživatelského rozhraní automaticky vytvoří kontext databáze a zaregistrovaného kontejneru pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="226a1-139">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="226a1-140">Zkontrolujte `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="226a1-140">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="226a1-141">Zvýrazněný řádek byl přidán modulem scaffolder:</span><span class="sxs-lookup"><span data-stu-id="226a1-141">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="226a1-142">Hlavní třída, která koordinuje EF Core funkce pro daný datový model je třídy kontextu databáze.</span><span class="sxs-lookup"><span data-stu-id="226a1-142">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="226a1-143">Kontext dat je odvozen z [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="226a1-143">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="226a1-144">Kontext dat určuje entit, které jsou zahrnuty v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="226a1-144">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="226a1-145">V tomto projektu je s názvem třídy `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="226a1-145">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="226a1-146">Předchozí kód vytvoří [DbSet\<video >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) vlastnost sady entit.</span><span class="sxs-lookup"><span data-stu-id="226a1-146">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="226a1-147">Terminologie Entity Framework obvykle sadu entit odpovídá databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="226a1-147">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="226a1-148">Entita odpovídající řádek v tabulce.</span><span class="sxs-lookup"><span data-stu-id="226a1-148">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="226a1-149">Název připojovacího řetězce je předán v rámci voláním metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) objektu.</span><span class="sxs-lookup"><span data-stu-id="226a1-149">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="226a1-150">Pro místní vývoj [ASP.NET Core konfigurační systém](xref:fundamentals/configuration/index) načte připojovací řetězec z *appsettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="226a1-150">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="226a1-151">Provést počáteční migraci</span><span class="sxs-lookup"><span data-stu-id="226a1-151">Perform initial migration</span></span>

<span data-ttu-id="226a1-152">V této části pomocí konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="226a1-152">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="226a1-153">Přidáte počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="226a1-153">Add an initial migration.</span></span>
* <span data-ttu-id="226a1-154">Aktualizujte počáteční migraci databáze.</span><span class="sxs-lookup"><span data-stu-id="226a1-154">Update the database with the initial migration.</span></span>

<span data-ttu-id="226a1-155">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="226a1-155">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="226a1-157">V konzole PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="226a1-157">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="226a1-158">Alternativně lze použít následující příkazy rozhraní příkazového řádku .NET Core ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="226a1-158">Alternatively, the following .NET Core CLI commands can be used from the project folder:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="226a1-159">Ignorovat následující zpráva upozornění, že v opravy pozdějších kurzech:</span><span class="sxs-lookup"><span data-stu-id="226a1-159">Ignore the following warning message, you fix that in a a later tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="226a1-160">`Add-Migration` Příkaz vygeneruje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="226a1-160">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="226a1-161">Schéma je založen na zadaném v modelu `RazorPagesMovieContext` (v *Data/RazorPagesMovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="226a1-161">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="226a1-162">`Initial` Argument se používá k pojmenování migrace.</span><span class="sxs-lookup"><span data-stu-id="226a1-162">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="226a1-163">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="226a1-163">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="226a1-164">Zobrazit [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="226a1-164">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="226a1-165">`Update-Database` Příkaz spustí `Up` metodu *migrace / {časové razítko} _InitialCreate.cs* soubor, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="226a1-165">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="226a1-166">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="226a1-166">If you get the error:</span></span>

`SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.`

<span data-ttu-id="226a1-167">Je provedena [kroku migrace](#pmc).</span><span class="sxs-lookup"><span data-stu-id="226a1-167">You missed the [migrations step](#pmc).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="226a1-168">Přidání datového modelu</span><span class="sxs-lookup"><span data-stu-id="226a1-168">Add a data model</span></span>

<span data-ttu-id="226a1-169">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="226a1-169">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="226a1-170">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="226a1-170">Name the folder *Models*.</span></span>

<span data-ttu-id="226a1-171">Klikněte pravým tlačítkem myši *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="226a1-171">Right click the *Models* folder.</span></span> <span data-ttu-id="226a1-172">Vyberte **přidat** > **třídy**.</span><span class="sxs-lookup"><span data-stu-id="226a1-172">Select **Add** > **Class**.</span></span> <span data-ttu-id="226a1-173">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="226a1-173">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="226a1-174">Přidat připojovací řetězec databáze</span><span class="sxs-lookup"><span data-stu-id="226a1-174">Add a database connection string</span></span>

<span data-ttu-id="226a1-175">Přidat připojovací řetězec pro *appsettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="226a1-175">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="226a1-176">Zaregistrujte kontext databáze</span><span class="sxs-lookup"><span data-stu-id="226a1-176">Register the database context</span></span>

<span data-ttu-id="226a1-177">Zaregistrujte kontext databáze s [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru v [ConfigureServices metoda spuštění třídy](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="226a1-177">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="226a1-178">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="226a1-178">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="226a1-179">Přidat vygenerované uživatelské rozhraní nástroje a provádět počáteční migraci</span><span class="sxs-lookup"><span data-stu-id="226a1-179">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="226a1-180">V této části pomocí konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="226a1-180">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="226a1-181">Přidejte balíček generování kódu web Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="226a1-181">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="226a1-182">Tento balíček je potřeba ke spouštění modulu generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="226a1-182">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="226a1-183">Přidáte počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="226a1-183">Add an initial migration.</span></span>
* <span data-ttu-id="226a1-184">Aktualizujte počáteční migraci databáze.</span><span class="sxs-lookup"><span data-stu-id="226a1-184">Update the database with the initial migration.</span></span>

<span data-ttu-id="226a1-185">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="226a1-185">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="226a1-187">V konzole PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="226a1-187">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="226a1-188">Alternativně můžete použít následující příkazy rozhraní příkazového řádku .NET Core:</span><span class="sxs-lookup"><span data-stu-id="226a1-188">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="226a1-189">Ignorujte tuto zprávu:</span><span class="sxs-lookup"><span data-stu-id="226a1-189">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="226a1-190">Opravit, který v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="226a1-190">You fix that in the next tutorial.</span></span>

<span data-ttu-id="226a1-191">`Install-Package` Nainstaluje nástroje potřebné ke spuštění modulu generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="226a1-191">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="226a1-192">`Add-Migration` Příkaz vygeneruje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="226a1-192">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="226a1-193">Schéma je založen na zadaném v modelu `DbContext` (v *Models/MovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="226a1-193">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="226a1-194">`Initial` Argument se používá k pojmenování migrace.</span><span class="sxs-lookup"><span data-stu-id="226a1-194">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="226a1-195">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="226a1-195">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="226a1-196">Zobrazit [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="226a1-196">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="226a1-197">`Update-Database` Příkaz spustí `Up` metodu *migrace / {časové razítko} _InitialCreate.cs* soubor, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="226a1-197">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="226a1-198">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="226a1-198">Test the app</span></span>

* <span data-ttu-id="226a1-199">Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="226a1-199">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="226a1-200">Test **vytvořit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="226a1-200">Test the **Create** link.</span></span>

  ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="226a1-202">Test **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="226a1-202">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="226a1-203">Pokud dojde k výjimce SQL, ověřte máte spusťte migrace a aktualizována v databázi.</span><span class="sxs-lookup"><span data-stu-id="226a1-203">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="226a1-204">V dalším kurzu vysvětluje souborů vytvořených databázovým generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="226a1-204">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="226a1-205">[Předchozí: Začínáme](xref:tutorials/razor-pages/razor-pages-start)
> [Další: generované uživatelské rozhraní pro stránky Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="226a1-205">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
