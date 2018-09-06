---
title: Přidání modelu do aplikace v ASP.NET Core Razor Pages
author: rick-anderson
description: Objevte, jak přidat třídy pro správu filmy v databázi pomocí Entity Framework Core (jádro EF).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: fb3a287725fa68ff9feb9935d7e6c5c2b8316517
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893117"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="bf8f5-103">Přidání modelu do aplikace v ASP.NET Core Razor Pages</span><span class="sxs-lookup"><span data-stu-id="bf8f5-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="bf8f5-104">Přidání datového modelu</span><span class="sxs-lookup"><span data-stu-id="bf8f5-104">Add a data model</span></span>

<span data-ttu-id="bf8f5-105">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="bf8f5-106">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-106">Name the folder *Models*.</span></span>

<span data-ttu-id="bf8f5-107">Klikněte pravým tlačítkem myši *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-107">Right click the *Models* folder.</span></span> <span data-ttu-id="bf8f5-108">Vyberte **přidat** > **třídy**.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="bf8f5-109">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="bf8f5-110">Nahraďte obsah `Movie` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-110">Replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="bf8f5-111">Vygenerované uživatelské rozhraní Video modelu</span><span class="sxs-lookup"><span data-stu-id="bf8f5-111">Scaffold the movie model</span></span>

<span data-ttu-id="bf8f5-112">V této části je automaticky generovaný model video.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-112">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="bf8f5-113">To znamená vytvoří nástroj pro generování uživatelského rozhraní stránky pro operace vytvoření, čtení, aktualizace a odstranění (CRUD) pro model video.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-113">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="bf8f5-114">Vytvoření *stránek/filmy* složky:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-114">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="bf8f5-115">V **Průzkumníka řešení**, klikněte pravým tlačítkem na *stránky* složky > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-115">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="bf8f5-116">Název složky *filmy*</span><span class="sxs-lookup"><span data-stu-id="bf8f5-116">Name the folder *Movies*</span></span>

<span data-ttu-id="bf8f5-117">V **Průzkumníka řešení**, klikněte pravým tlačítkem na *stránek/filmy* složky > **přidat** > **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-117">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Image z předchozích kroků.](model/_static/sca.png)

<span data-ttu-id="bf8f5-119">V **přidat vygenerované uživatelské rozhraní** dialogového okna, vyberte **stránky Razor pomocí Entity Frameworku (CRUD)** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-119">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Image z předchozích kroků.](model/_static/add_scaffold.png)

<span data-ttu-id="bf8f5-121">Dokončení **přidat stránky Razor pomocí Entity Frameworku (CRUD)** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-121">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="bf8f5-122">V **třída modelu** rozevírací seznam, vyberte **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-122">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="bf8f5-123">V **třída kontextu dat** řádek, vyberte **+** (plus) podepsat a přijměte vygenerovaný název **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-123">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="bf8f5-124">V **třída kontextu dat** rozevírací seznam, vyberte **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="bf8f5-124">In the **Data context class** drop down, select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="bf8f5-125">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-125">Select **Add**.</span></span>

![Image z předchozích kroků.](model/_static/arp.png)

<span data-ttu-id="bf8f5-127">Proces vygenerované uživatelské rozhraní vytvořit a změnit následující soubory:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-127">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="bf8f5-128">Soubory vytvořené</span><span class="sxs-lookup"><span data-stu-id="bf8f5-128">Files created</span></span>

* <span data-ttu-id="bf8f5-129">*Stránky/filmy* vytvoření, odstranění, podrobností, úpravy, Index.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-129">*Pages/Movies* Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="bf8f5-130">Tyto stránky je podrobně popsaný v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-130">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="bf8f5-131">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="bf8f5-131">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="bf8f5-132">Soubory aktualizace</span><span class="sxs-lookup"><span data-stu-id="bf8f5-132">Files updates</span></span>

* <span data-ttu-id="bf8f5-133">*Startup.cs*: změny tohoto souboru jsou podrobně popsané v další části.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-133">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="bf8f5-134">*appSettings.JSON*: přidat připojovací řetězec použitý pro připojení k místní databázi.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-134">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="bf8f5-135">Prozkoumání kontextu registrovaný pomocí vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="bf8f5-135">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="bf8f5-136">ASP.NET Core využívá rozhraní [injektáž závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bf8f5-136">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="bf8f5-137">Služby (například kontext EF Core databáze) jsou registrované pomocí vkládání závislostí při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-137">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="bf8f5-138">Komponenty, které vyžadují tyto služby (například stránky Razor) jsou k dispozici tyto služby prostřednictvím parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-138">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="bf8f5-139">Později v tomto kurzu se zobrazí kód konstruktor, který získá instanci kontext databáze.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-139">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="bf8f5-140">Nástroj pro generování uživatelského rozhraní automaticky vytvoří kontext databáze a zaregistrovaného kontejneru pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-140">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="bf8f5-141">Zkontrolujte `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-141">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="bf8f5-142">Zvýrazněný řádek byl přidán modulem scaffolder:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-142">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="bf8f5-143">Hlavní třída, která koordinuje EF Core funkce pro daný datový model je třídy kontextu databáze.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-143">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="bf8f5-144">Kontext dat je odvozen z [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="bf8f5-144">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="bf8f5-145">Kontext dat určuje entit, které jsou zahrnuty v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-145">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="bf8f5-146">V tomto projektu je s názvem třídy `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-146">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="bf8f5-147">Předchozí kód vytvoří [DbSet\<video >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) vlastnost sady entit.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-147">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="bf8f5-148">Terminologie Entity Framework obvykle sadu entit odpovídá databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-148">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="bf8f5-149">Entita odpovídající řádek v tabulce.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-149">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="bf8f5-150">Název připojovacího řetězce je předán v rámci voláním metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) objektu.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-150">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="bf8f5-151">Pro místní vývoj [ASP.NET Core konfigurační systém](xref:fundamentals/configuration/index) načte připojovací řetězec z *appsettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-151">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="bf8f5-152">Provést počáteční migraci</span><span class="sxs-lookup"><span data-stu-id="bf8f5-152">Perform initial migration</span></span>

<span data-ttu-id="bf8f5-153">V této části pomocí konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-153">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="bf8f5-154">Přidáte počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-154">Add an initial migration.</span></span>
* <span data-ttu-id="bf8f5-155">Aktualizujte počáteční migraci databáze.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-155">Update the database with the initial migration.</span></span>

<span data-ttu-id="bf8f5-156">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-156">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="bf8f5-158">V konzole PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-158">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="bf8f5-159">Alternativně lze použít následující příkazy rozhraní příkazového řádku .NET Core ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-159">Alternatively, the following .NET Core CLI commands can be used from the project folder:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="bf8f5-160">Ignorovat následující zpráva upozornění, že v opravy pozdějších kurzech:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-160">Ignore the following warning message, you fix that in a a later tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="bf8f5-161">`Add-Migration` Příkaz vygeneruje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-161">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="bf8f5-162">Schéma je založen na zadaném v modelu `RazorPagesMovieContext` (v *Data/RazorPagesMovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="bf8f5-162">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="bf8f5-163">`Initial` Argument se používá k pojmenování migrace.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-163">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="bf8f5-164">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-164">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="bf8f5-165">Zobrazit [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-165">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="bf8f5-166">`Update-Database` Příkaz spustí `Up` metodu *migrace / {časové razítko} _InitialCreate.cs* soubor, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-166">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="bf8f5-167">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-167">If you get the error:</span></span>

<span data-ttu-id="bf8f5-168">SqlException: Databázi "RazorPagesMovieContext identifikátor GUID" požadovaný v přihlášení nelze otevřít.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-168">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="bf8f5-169">Přihlášení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-169">The login failed.</span></span>
<span data-ttu-id="bf8f5-170">Přihlašovací jméno uživatele 'Jméno uživatele' se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-170">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="bf8f5-171">Je provedena [kroku migrace](#pmc).</span><span class="sxs-lookup"><span data-stu-id="bf8f5-171">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="bf8f5-172">Přidání datového modelu</span><span class="sxs-lookup"><span data-stu-id="bf8f5-172">Add a data model</span></span>

<span data-ttu-id="bf8f5-173">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-173">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="bf8f5-174">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-174">Name the folder *Models*.</span></span>

<span data-ttu-id="bf8f5-175">Klikněte pravým tlačítkem myši *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-175">Right click the *Models* folder.</span></span> <span data-ttu-id="bf8f5-176">Vyberte **přidat** > **třídy**.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-176">Select **Add** > **Class**.</span></span> <span data-ttu-id="bf8f5-177">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-177">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="bf8f5-178">Přidat připojovací řetězec databáze</span><span class="sxs-lookup"><span data-stu-id="bf8f5-178">Add a database connection string</span></span>

<span data-ttu-id="bf8f5-179">Přidat připojovací řetězec pro *appsettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-179">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="bf8f5-180">Zaregistrujte kontext databáze</span><span class="sxs-lookup"><span data-stu-id="bf8f5-180">Register the database context</span></span>

<span data-ttu-id="bf8f5-181">Zaregistrujte kontext databáze s [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru v [ConfigureServices metoda spuštění třídy](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="bf8f5-181">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="bf8f5-182">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-182">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="bf8f5-183">Přidat vygenerované uživatelské rozhraní nástroje a provádět počáteční migraci</span><span class="sxs-lookup"><span data-stu-id="bf8f5-183">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="bf8f5-184">V této části pomocí konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-184">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="bf8f5-185">Přidejte balíček generování kódu web Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-185">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="bf8f5-186">Tento balíček je potřeba ke spouštění modulu generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-186">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="bf8f5-187">Přidáte počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-187">Add an initial migration.</span></span>
* <span data-ttu-id="bf8f5-188">Aktualizujte počáteční migraci databáze.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-188">Update the database with the initial migration.</span></span>

<span data-ttu-id="bf8f5-189">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-189">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="bf8f5-191">V konzole PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-191">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="bf8f5-192">Alternativně můžete použít následující příkazy rozhraní příkazového řádku .NET Core:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-192">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="bf8f5-193">Ignorujte tuto zprávu:</span><span class="sxs-lookup"><span data-stu-id="bf8f5-193">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="bf8f5-194">Opravit, který v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-194">You fix that in the next tutorial.</span></span>

<span data-ttu-id="bf8f5-195">`Install-Package` Nainstaluje nástroje potřebné ke spuštění modulu generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-195">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="bf8f5-196">`Add-Migration` Příkaz vygeneruje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-196">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="bf8f5-197">Schéma je založen na zadaném v modelu `DbContext` (v *Models/MovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="bf8f5-197">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="bf8f5-198">`Initial` Argument se používá k pojmenování migrace.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-198">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="bf8f5-199">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-199">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="bf8f5-200">Zobrazit [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-200">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="bf8f5-201">`Update-Database` Příkaz spustí `Up` metodu *migrace / {časové razítko} _InitialCreate.cs* soubor, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-201">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="bf8f5-202">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="bf8f5-202">Test the app</span></span>

* <span data-ttu-id="bf8f5-203">Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="bf8f5-203">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="bf8f5-204">Test **vytvořit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-204">Test the **Create** link.</span></span>

  ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="bf8f5-206">Test **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-206">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="bf8f5-207">Pokud dojde k výjimce SQL, ověřte máte spusťte migrace a aktualizována v databázi.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-207">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="bf8f5-208">V dalším kurzu vysvětluje souborů vytvořených databázovým generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bf8f5-208">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bf8f5-209">[Předchozí: Začínáme](xref:tutorials/razor-pages/razor-pages-start)
> [Další: generované uživatelské rozhraní pro stránky Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="bf8f5-209">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
