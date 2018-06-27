---
title: Přidat model do aplikace pro stránky Razor v ASP.NET Core
author: rick-anderson
description: Zjistit, jak přidat třídy pro správu filmy v databázi pomocí Entity Framework Core (EF jader).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: ed8faf8b3049adc7bcc7953d63ad805b0a836bd9
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961172"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="26504-103">Přidat model do aplikace pro stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26504-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="26504-104">Přidat datový model</span><span class="sxs-lookup"><span data-stu-id="26504-104">Add a data model</span></span>

<span data-ttu-id="26504-105">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="26504-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="26504-106">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="26504-106">Name the folder *Models*.</span></span>

<span data-ttu-id="26504-107">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="26504-107">Right click the *Models* folder.</span></span> <span data-ttu-id="26504-108">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="26504-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="26504-109">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="26504-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="26504-110">Nahraďte obsah `Movie` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="26504-110">Replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="26504-111">Vygenerované uživatelské rozhraní film modelu</span><span class="sxs-lookup"><span data-stu-id="26504-111">Scaffold the movie model</span></span>

<span data-ttu-id="26504-112">V této části se vygeneroval film modelu.</span><span class="sxs-lookup"><span data-stu-id="26504-112">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="26504-113">To znamená vytvoří nástroj pro generování uživatelského rozhraní stránky pro operace vytvoření, čtení, aktualizace a odstranění (CRUD) pro model film.</span><span class="sxs-lookup"><span data-stu-id="26504-113">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="26504-114">Vytvoření *stránkách nebo filmy* složky:</span><span class="sxs-lookup"><span data-stu-id="26504-114">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="26504-115">V **Průzkumníku řešení**, klikněte pravým tlačítkem na *stránky* složky > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="26504-115">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="26504-116">Název složky *filmy*</span><span class="sxs-lookup"><span data-stu-id="26504-116">Name the folder *Movies*</span></span>

<span data-ttu-id="26504-117">V **Průzkumníku řešení**, klikněte pravým tlačítkem na *stránkách nebo filmy* složky > **přidat** > **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="26504-117">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Bitovou kopii z předchozích pokynů.](model/_static/sca.png)

<span data-ttu-id="26504-119">V **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **stránky Razor pomocí Entity Framework (CRUD)** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="26504-119">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Bitovou kopii z předchozích pokynů.](model/_static/add_scaffold.png)

<span data-ttu-id="26504-121">Dokončení **přidat stránky Razor pomocí Entity Framework (CRUD)** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="26504-121">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="26504-122">V **třída modelu** rozevírací nabídku, vyberte **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="26504-122">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="26504-123">V **třída kontextu dat** řádek, vyberte **+** (a), přihlášení a přijměte vygenerovaný název **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="26504-123">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="26504-124">V **třída kontextu dat** rozevírací nabídku, vyberte **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="26504-124">In the **Data context class** drop down, select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="26504-125">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="26504-125">Select **Add**.</span></span>

![Bitovou kopii z předchozích pokynů.](model/_static/arp.png)

<span data-ttu-id="26504-127">Proces vygenerované uživatelské rozhraní vytvořené a změněné následující soubory:</span><span class="sxs-lookup"><span data-stu-id="26504-127">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="26504-128">Vytvořené soubory</span><span class="sxs-lookup"><span data-stu-id="26504-128">Files created</span></span>

* <span data-ttu-id="26504-129">*Stránky/filmy* vytvořit, odstranit, podrobnosti, úpravy, Index.</span><span class="sxs-lookup"><span data-stu-id="26504-129">*Pages/Movies* Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="26504-130">Tyto stránky jsou podrobně popsané v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="26504-130">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="26504-131">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="26504-131">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="26504-132">Soubory aktualizací</span><span class="sxs-lookup"><span data-stu-id="26504-132">Files updates</span></span>

* <span data-ttu-id="26504-133">*Startup.cs*: změny tohoto souboru v jsou podrobně popsané v další části.</span><span class="sxs-lookup"><span data-stu-id="26504-133">*Startup.cs*: Changes to this file in are detailed the next section.</span></span>
* <span data-ttu-id="26504-134">*appSettings.JSON určený*: Přidání připojovací řetězec použitý pro připojení k místní databázi.</span><span class="sxs-lookup"><span data-stu-id="26504-134">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="26504-135">Zkontrolujte kontext zaregistrována vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="26504-135">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="26504-136">ASP.NET Core je vytvořené s [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="26504-136">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="26504-137">Služby (například kontext databáze základní EF) jsou registrovány vkládání závislostí při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="26504-137">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="26504-138">Součásti, které vyžadují tyto služby (například stránky Razor) jsou k dispozici tyto služby prostřednictvím konstruktor parametry.</span><span class="sxs-lookup"><span data-stu-id="26504-138">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="26504-139">Později v tomto kurzu se zobrazí kód konstruktor, který získá kontext instanci databáze.</span><span class="sxs-lookup"><span data-stu-id="26504-139">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="26504-140">Nástroj pro generování uživatelského rozhraní automaticky vytvořen kontext databáze a zaregistrována kontejneru pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="26504-140">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="26504-141">Zkontrolujte `Startup.ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="26504-141">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="26504-142">Zvýrazněný řádek byl přidán modulem scaffolder:</span><span class="sxs-lookup"><span data-stu-id="26504-142">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="26504-143">Hlavní třída, která koordinuje EF základní funkce pro daný datový model je třídy kontextu databáze.</span><span class="sxs-lookup"><span data-stu-id="26504-143">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="26504-144">Data kontextu je odvozený od [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="26504-144">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="26504-145">Data kontextu určuje entit, které jsou zahrnuty v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="26504-145">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="26504-146">V tomto projektu je třída s názvem `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="26504-146">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="26504-147">Předchozí kód vytvoří [DbSet\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) vlastnost pro sadu entit.</span><span class="sxs-lookup"><span data-stu-id="26504-147">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="26504-148">V terminologii rozhraní Entity Framework obvykle sadu entit odpovídá do databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="26504-148">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="26504-149">Entity odpovídá na řádek v tabulce.</span><span class="sxs-lookup"><span data-stu-id="26504-149">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="26504-150">Název připojovacího řetězce, je předaná do kontextu voláním metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) objektu.</span><span class="sxs-lookup"><span data-stu-id="26504-150">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="26504-151">Pro místní vývoj [ASP.NET Core konfigurační systém](xref:fundamentals/configuration/index) čte připojovací řetězec z *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="26504-151">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="26504-152">Provedení počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="26504-152">Perform initial migration</span></span>

<span data-ttu-id="26504-153">V této části použijte konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="26504-153">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="26504-154">Přidáte počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="26504-154">Add an initial migration.</span></span>
* <span data-ttu-id="26504-155">Aktualizace databáze s počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="26504-155">Update the database with the initial migration.</span></span>

<span data-ttu-id="26504-156">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="26504-156">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="26504-158">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="26504-158">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="26504-159">Alternativně můžete použít následující příkazy .NET Core rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="26504-159">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="26504-160">Následující zpráva upozornění ignorovat, kterou vyřešíte v dalším kurzu:</span><span class="sxs-lookup"><span data-stu-id="26504-160">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="26504-161">`Add-Migration` Příkaz generuje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="26504-161">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="26504-162">Schéma je založena na zadaný ve model `RazorPagesMovieContext` (v *Data/RazorPagesMovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="26504-162">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="26504-163">`Initial` Argument se používá k pojmenování byla migrace.</span><span class="sxs-lookup"><span data-stu-id="26504-163">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="26504-164">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="26504-164">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="26504-165">V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="26504-165">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="26504-166">`Update-Database` Příkaz spustí `Up` metoda v *migrace / {časové razítko} _InitialCreate.cs* souboru, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="26504-166">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="26504-167">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="26504-167">If you get the error:</span></span>

<span data-ttu-id="26504-168">SqlException: Nelze otevřít databázi "RazorPagesMovieContext identifikátor GUID" požadovaný v přihlášení.</span><span class="sxs-lookup"><span data-stu-id="26504-168">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="26504-169">Přihlášení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="26504-169">The login failed.</span></span>
<span data-ttu-id="26504-170">Přihlášení se nezdařilo pro uživatele, uživatelského jména'.</span><span class="sxs-lookup"><span data-stu-id="26504-170">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="26504-171">Je provedena [krok migrace](#pmc).</span><span class="sxs-lookup"><span data-stu-id="26504-171">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="26504-172">Přidat datový model</span><span class="sxs-lookup"><span data-stu-id="26504-172">Add a data model</span></span>

<span data-ttu-id="26504-173">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="26504-173">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="26504-174">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="26504-174">Name the folder *Models*.</span></span>

<span data-ttu-id="26504-175">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="26504-175">Right click the *Models* folder.</span></span> <span data-ttu-id="26504-176">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="26504-176">Select **Add** > **Class**.</span></span> <span data-ttu-id="26504-177">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="26504-177">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="26504-178">Přidat připojovací řetězec databáze</span><span class="sxs-lookup"><span data-stu-id="26504-178">Add a database connection string</span></span>

<span data-ttu-id="26504-179">Přidat připojovací řetězec k *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="26504-179">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="26504-180">Zaregistrovat kontext databáze</span><span class="sxs-lookup"><span data-stu-id="26504-180">Register the database context</span></span>

<span data-ttu-id="26504-181">Zaregistrovat kontext databáze s [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru [ConfigureServices metodu třída při spuštění](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="26504-181">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="26504-182">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="26504-182">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="26504-183">Přidat vygenerované uživatelské rozhraní nástroje a provedení počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="26504-183">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="26504-184">V této části použijte konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="26504-184">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="26504-185">Přidejte balíček generování kódu webové sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26504-185">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="26504-186">Tento balíček je potřeba spustit modul generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="26504-186">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="26504-187">Přidáte počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="26504-187">Add an initial migration.</span></span>
* <span data-ttu-id="26504-188">Aktualizace databáze s počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="26504-188">Update the database with the initial migration.</span></span>

<span data-ttu-id="26504-189">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="26504-189">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="26504-191">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="26504-191">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="26504-192">Alternativně můžete použít následující příkazy .NET Core rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="26504-192">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="26504-193">Ignorujte se následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="26504-193">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="26504-194">To opravíme v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="26504-194">You fix that in the next tutorial.</span></span>

<span data-ttu-id="26504-195">`Install-Package` Příkaz nainstaluje nástroje potřebné ke spuštění modulu generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="26504-195">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="26504-196">`Add-Migration` Příkaz generuje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="26504-196">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="26504-197">Schéma je založena na zadaný ve model `DbContext` (v *Models/MovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="26504-197">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="26504-198">`Initial` Argument se používá k pojmenování byla migrace.</span><span class="sxs-lookup"><span data-stu-id="26504-198">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="26504-199">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="26504-199">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="26504-200">V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="26504-200">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="26504-201">`Update-Database` Příkaz spustí `Up` metoda v *migrace / {časové razítko} _InitialCreate.cs* souboru, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="26504-201">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="26504-202">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="26504-202">Test the app</span></span>

* <span data-ttu-id="26504-203">Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="26504-203">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="26504-204">Testovací **vytvořit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="26504-204">Test the **Create** link.</span></span>

  ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="26504-206">Testovací **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="26504-206">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="26504-207">Pokud dojde k výjimce SQL, ověřte, jestli mají spuštění migrace a aktualizovat databázi.</span><span class="sxs-lookup"><span data-stu-id="26504-207">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="26504-208">Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="26504-208">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="26504-209">[Předchozí: Začínáme](xref:tutorials/razor-pages/razor-pages-start)
> [Další: vygeneroval stránky Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="26504-209">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
