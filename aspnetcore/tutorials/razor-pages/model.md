---
title: Přidat model do aplikace pro stránky Razor v ASP.NET Core
author: rick-anderson
description: Zjistit, jak přidat třídy pro správu filmy v databázi pomocí Entity Framework Core (EF jader).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: edc8ceb3de7e2fe0afc106278307d3961e9d8845
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277355"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="00e7c-103">Přidat model do aplikace pro stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00e7c-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="00e7c-104">Přidat datový model</span><span class="sxs-lookup"><span data-stu-id="00e7c-104">Add a data model</span></span>

<span data-ttu-id="00e7c-105">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="00e7c-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="00e7c-106">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="00e7c-106">Name the folder *Models*.</span></span>

<span data-ttu-id="00e7c-107">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="00e7c-107">Right click the *Models* folder.</span></span> <span data-ttu-id="00e7c-108">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="00e7c-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="00e7c-109">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="00e7c-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="00e7c-110">Nahraďte obsah `Movie` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="00e7c-110">Replace the contents of the `Movie` class with the following code:</span></span>

<span data-ttu-id="00e7c-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="00e7c-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="00e7c-112">Vygenerované uživatelské rozhraní film modelu</span><span class="sxs-lookup"><span data-stu-id="00e7c-112">Scaffold the movie model</span></span>

<span data-ttu-id="00e7c-113">V této části se vygeneroval film modelu.</span><span class="sxs-lookup"><span data-stu-id="00e7c-113">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="00e7c-114">To znamená vytvoří nástroj pro generování uživatelského rozhraní stránky pro operace vytvoření, čtení, aktualizace a odstranění (CRUD) pro model film.</span><span class="sxs-lookup"><span data-stu-id="00e7c-114">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="00e7c-115">Vytvoření *stránkách nebo filmy* složky:</span><span class="sxs-lookup"><span data-stu-id="00e7c-115">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="00e7c-116">V **Průzkumníku řešení**, klikněte pravým tlačítkem na *stránky* složky > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="00e7c-116">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="00e7c-117">Název složky *filmy*</span><span class="sxs-lookup"><span data-stu-id="00e7c-117">Name the folder *Movies*</span></span>

<span data-ttu-id="00e7c-118">V **Průzkumníku řešení**, klikněte pravým tlačítkem na *stránkách nebo filmy* složky > **přidat** > **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="00e7c-118">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Bitovou kopii z předchozích pokynů.](model/_static/sca.png)

<span data-ttu-id="00e7c-120">V **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **stránky Razor pomocí Entity Framework (CRUD)** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="00e7c-120">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Bitovou kopii z předchozích pokynů.](model/_static/add_scaffold.png)

<span data-ttu-id="00e7c-122">Dokončení **přidat stránky Razor pomocí Entity Framework (CRUD)** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="00e7c-122">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="00e7c-123">V **třída modelu** rozevírací nabídku, vyberte **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="00e7c-123">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="00e7c-124">V **třída kontextu dat** řádek, vyberte **+** (a), přihlášení a přijměte vygenerovaný název **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="00e7c-124">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="00e7c-125">V **třída kontextu dat** rozevírací nabídku, vyberte **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="00e7c-125">In the **Data context class** drop down,  select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="00e7c-126">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="00e7c-126">Select **Add**.</span></span>

![Bitovou kopii z předchozích pokynů.](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="00e7c-128">Provedení počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="00e7c-128">Perform initial migration</span></span>

<span data-ttu-id="00e7c-129">V této části použijte konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="00e7c-129">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="00e7c-130">Přidáte počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="00e7c-130">Add an initial migration.</span></span>
* <span data-ttu-id="00e7c-131">Aktualizace databáze s počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="00e7c-131">Update the database with the initial migration.</span></span>

<span data-ttu-id="00e7c-132">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="00e7c-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="00e7c-134">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="00e7c-134">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="00e7c-135">Alternativně můžete použít následující příkazy .NET Core rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="00e7c-135">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="00e7c-136">Následující zpráva upozornění ignorovat, kterou vyřešíte v dalším kurzu:</span><span class="sxs-lookup"><span data-stu-id="00e7c-136">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="00e7c-137">`Add-Migration` Příkaz generuje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="00e7c-137">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="00e7c-138">Schéma je založena na zadaný ve model `DbContext` (v *Models/MovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="00e7c-138">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="00e7c-139">`Initial` Argument se používá k pojmenování byla migrace.</span><span class="sxs-lookup"><span data-stu-id="00e7c-139">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="00e7c-140">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="00e7c-140">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="00e7c-141">V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="00e7c-141">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="00e7c-142">`Update-Database` Příkaz spustí `Up` metoda v *migrace / {časové razítko} _InitialCreate.cs* souboru, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="00e7c-142">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="00e7c-143">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="00e7c-143">If you get the error:</span></span>

<span data-ttu-id="00e7c-144">SqlException: Nelze otevřít databázi "RazorPagesMovieContext identifikátor GUID" požadovaný v přihlášení.</span><span class="sxs-lookup"><span data-stu-id="00e7c-144">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="00e7c-145">Přihlášení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="00e7c-145">The login failed.</span></span>
<span data-ttu-id="00e7c-146">Přihlášení se nezdařilo pro uživatele, uživatelského jména'.</span><span class="sxs-lookup"><span data-stu-id="00e7c-146">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="00e7c-147">Je provedena [krok migrace](#pmc).</span><span class="sxs-lookup"><span data-stu-id="00e7c-147">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="00e7c-148">Přidat datový model</span><span class="sxs-lookup"><span data-stu-id="00e7c-148">Add a data model</span></span>

<span data-ttu-id="00e7c-149">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="00e7c-149">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="00e7c-150">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="00e7c-150">Name the folder *Models*.</span></span>

<span data-ttu-id="00e7c-151">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="00e7c-151">Right click the *Models* folder.</span></span> <span data-ttu-id="00e7c-152">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="00e7c-152">Select **Add** > **Class**.</span></span> <span data-ttu-id="00e7c-153">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="00e7c-153">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="00e7c-154">Přidat připojovací řetězec databáze</span><span class="sxs-lookup"><span data-stu-id="00e7c-154">Add a database connection string</span></span>

<span data-ttu-id="00e7c-155">Přidat připojovací řetězec k *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="00e7c-155">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="00e7c-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="00e7c-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="00e7c-157">Zaregistrovat kontext databáze</span><span class="sxs-lookup"><span data-stu-id="00e7c-157">Register the database context</span></span>

<span data-ttu-id="00e7c-158">Zaregistrovat kontext databáze s [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru [ConfigureServices metodu třída při spuštění](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="00e7c-158">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

<span data-ttu-id="00e7c-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span><span class="sxs-lookup"><span data-stu-id="00e7c-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span></span>

<span data-ttu-id="00e7c-160">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="00e7c-160">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="00e7c-161">Přidat vygenerované uživatelské rozhraní nástroje a provedení počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="00e7c-161">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="00e7c-162">V této části použijte konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="00e7c-162">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="00e7c-163">Přidejte balíček generování kódu webové sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="00e7c-163">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="00e7c-164">Tento balíček je potřeba spustit modul generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="00e7c-164">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="00e7c-165">Přidáte počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="00e7c-165">Add an initial migration.</span></span>
* <span data-ttu-id="00e7c-166">Aktualizace databáze s počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="00e7c-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="00e7c-167">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="00e7c-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="00e7c-169">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="00e7c-169">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="00e7c-170">Alternativně můžete použít následující příkazy .NET Core rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="00e7c-170">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="00e7c-171">Ignorujte se následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="00e7c-171">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="00e7c-172">To opravíme v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="00e7c-172">You fix that in the next tutorial.</span></span>

<span data-ttu-id="00e7c-173">`Install-Package` Příkaz nainstaluje nástroje potřebné ke spuštění modulu generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="00e7c-173">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="00e7c-174">`Add-Migration` Příkaz generuje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="00e7c-174">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="00e7c-175">Schéma je založena na zadaný ve model `DbContext` (v *Models/MovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="00e7c-175">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="00e7c-176">`Initial` Argument se používá k pojmenování byla migrace.</span><span class="sxs-lookup"><span data-stu-id="00e7c-176">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="00e7c-177">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="00e7c-177">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="00e7c-178">V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="00e7c-178">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="00e7c-179">`Update-Database` Příkaz spustí `Up` metoda v *migrace / {časové razítko} _InitialCreate.cs* souboru, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="00e7c-179">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="00e7c-180">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="00e7c-180">Test the app</span></span>

* <span data-ttu-id="00e7c-181">Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="00e7c-181">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="00e7c-182">Testovací **vytvořit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="00e7c-182">Test the **Create** link.</span></span>

  ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="00e7c-184">Testovací **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="00e7c-184">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="00e7c-185">Pokud dojde k výjimce SQL, ověřte, jestli mají spuštění migrace a aktualizovat databázi.</span><span class="sxs-lookup"><span data-stu-id="00e7c-185">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="00e7c-186">Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="00e7c-186">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="00e7c-187">[Předchozí: Začínáme](xref:tutorials/razor-pages/razor-pages-start)
> [Další: vygeneroval stránky Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="00e7c-187">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
