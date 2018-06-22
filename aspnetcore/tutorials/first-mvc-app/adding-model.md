---
title: Přidat model do aplikace ASP.NET MVC jádra
author: rick-anderson
description: Přidáte model do jednoduchou aplikaci ASP.NET Core.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 1e562116de8e6a88666f578f7255e325735c10a9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272319"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="70f3f-103">Přidat model do aplikace ASP.NET MVC jádra</span><span class="sxs-lookup"><span data-stu-id="70f3f-103">Add a model to an ASP.NET Core MVC app</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="70f3f-104">Klikněte pravým tlačítkem myši *modely* složky > **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="70f3f-104">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="70f3f-105">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="70f3f-105">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="70f3f-106">`ID` Pole je vyžadováno pro databázi pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="70f3f-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="70f3f-107">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="70f3f-107">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="70f3f-108">Nyní máte **M**odelu ve vaší **M**VC aplikace.</span><span class="sxs-lookup"><span data-stu-id="70f3f-108">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="70f3f-109">Řadič generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="70f3f-109">Scaffolding a controller</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="70f3f-110">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složky **> Přidat > novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="70f3f-110">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![zobrazení výše krok](adding-model/_static/add_controller21.png)

<span data-ttu-id="70f3f-112">V **přidat vygenerované uživatelské rozhraní** dialogové okno, klepněte na **kontroler MVC se zobrazeními s využitím nástroje Entity Framework > Přidat**.</span><span class="sxs-lookup"><span data-stu-id="70f3f-112">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Přidat vygenerované uživatelské rozhraní – dialogové okno](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="70f3f-114">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složky **> Přidat > řadiče**.</span><span class="sxs-lookup"><span data-stu-id="70f3f-114">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![zobrazení výše krok](adding-model/_static/add_controller.png)

<span data-ttu-id="70f3f-116">Pokud **přidat závislosti MVC** otevře se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="70f3f-116">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="70f3f-117">[Aktualizovat na nejnovější verzi sady Visual Studio](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="70f3f-117">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="70f3f-118">Visual Studio verze starší než 15,5 zobrazit tento dialog.</span><span class="sxs-lookup"><span data-stu-id="70f3f-118">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="70f3f-119">Pokud nelze aktualizovat, vyberte **přidat**a pak postupujte podle kroků řadiče přidat.</span><span class="sxs-lookup"><span data-stu-id="70f3f-119">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="70f3f-120">V **přidat vygenerované uživatelské rozhraní** dialogové okno, klepněte na **kontroler MVC se zobrazeními s využitím nástroje Entity Framework > Přidat**.</span><span class="sxs-lookup"><span data-stu-id="70f3f-120">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Přidat vygenerované uživatelské rozhraní – dialogové okno](adding-model/_static/add_scaffold2.png)

::: moniker-end

<span data-ttu-id="70f3f-122">Dokončení **přidat kontroler** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="70f3f-122">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="70f3f-123">**Třída modelu:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="70f3f-123">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="70f3f-124">**Třída kontextu dat:** vyberte **+** ikonu a přidejte výchozí **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="70f3f-124">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Přidat kontextu dat](adding-model/_static/dc.png)

* <span data-ttu-id="70f3f-126">**Zobrazení:** zachovat ve výchozím nastavení mají jednotlivé možnosti zaškrtnuto</span><span class="sxs-lookup"><span data-stu-id="70f3f-126">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="70f3f-127">**Název řadiče:** ponechat výchozí *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="70f3f-127">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="70f3f-128">Klepněte na **přidat**</span><span class="sxs-lookup"><span data-stu-id="70f3f-128">Tap **Add**</span></span>

![Dialogové okno řadiče přidání](adding-model/_static/add_controller2.png)

<span data-ttu-id="70f3f-130">Visual Studio vytvoří:</span><span class="sxs-lookup"><span data-stu-id="70f3f-130">Visual Studio creates:</span></span>

* <span data-ttu-id="70f3f-131">Entity Framework Core [databáze třída kontextu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="70f3f-131">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="70f3f-132">Řadič filmy (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="70f3f-132">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="70f3f-133">Zobrazení syntaxe Razor soubory vytvořit, odstranit, podrobnosti, Edit a Index stránky (<em>zobrazení nebo filmy nebo&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="70f3f-133">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="70f3f-134">Automatické vytváření kontext databáze a [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení se označuje jako *generování uživatelského rozhraní*.</span><span class="sxs-lookup"><span data-stu-id="70f3f-134">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="70f3f-135">Brzy budete mít funkční webovou aplikaci, která vám umožní spravovat film databáze.</span><span class="sxs-lookup"><span data-stu-id="70f3f-135">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="70f3f-136">Pokud spustíte aplikaci a klikněte na **Mvc film** odkaz, dojde k chybě podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="70f3f-136">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="70f3f-137">Musíte vytvořit databázi, a budete používat jádro EF [migrace](xref:data/ef-mvc/migrations) funkci tak, aby to udělat.</span><span class="sxs-lookup"><span data-stu-id="70f3f-137">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="70f3f-138">Migrace umožňuje vytvořit databázi, která odpovídá datový model a aktualizovat schéma databáze, když datový model změny.</span><span class="sxs-lookup"><span data-stu-id="70f3f-138">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="70f3f-139">Přidejte EF nástrojů a provedení počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="70f3f-139">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="70f3f-140">V této části pomocí konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="70f3f-140">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="70f3f-141">Přidejte balíček základní nástroje Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="70f3f-141">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="70f3f-142">Tento balíček je potřebné k přidání migrace a aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="70f3f-142">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="70f3f-143">Přidáte počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="70f3f-143">Add an initial migration.</span></span>
* <span data-ttu-id="70f3f-144">Aktualizace databáze s počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="70f3f-144">Update the database with the initial migration.</span></span>

<span data-ttu-id="70f3f-145">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="70f3f-145">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<span data-ttu-id="70f3f-146"><!-- following image shared with uid: tutorials/razor-pages/model --> ![Pomocí PMC nabídky](adding-model/_static/pmc.png)</span><span class="sxs-lookup"><span data-stu-id="70f3f-146"><!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC menu](adding-model/_static/pmc.png)</span></span>

<span data-ttu-id="70f3f-147">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="70f3f-147">In the PMC, enter the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="70f3f-148">Ignorovat se následující chybová zpráva, jsme opravte v dalším kurzu:</span><span class="sxs-lookup"><span data-stu-id="70f3f-148">Ignore the following error message, we fix it in the next tutorial:</span></span>

<span data-ttu-id="70f3f-149">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span><span class="sxs-lookup"><span data-stu-id="70f3f-149">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span></span>  
      <span data-ttu-id="70f3f-150">*Žádný typ byla zadána decimal sloupce "Cenou" u typu entity, film'. To způsobí, že hodnoty, které mají být bezobslužně zkrácen, pokud jejich se nevejdou do výchozí přesnost a měřítko. Explicitně zadáte typ sloupce serveru SQL, který zvládne všechny hodnoty pomocí 'ForHasColumnType()'.*</span><span class="sxs-lookup"><span data-stu-id="70f3f-150">*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="70f3f-151">**Poznámka:** Pokud obdržíte chybu s `Install-Package` příkaz, otevřete Správce balíčků NuGet a vyhledejte `Microsoft.EntityFrameworkCore.Tools` balíčku.</span><span class="sxs-lookup"><span data-stu-id="70f3f-151">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="70f3f-152">To vám umožňuje nainstalovat balíček nebo zkontrolujte, zda je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="70f3f-152">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="70f3f-153">Můžete si také přečíst [rozhraní příkazového řádku přístup](#cli) Pokud máte problémy s pomocí PMC.</span><span class="sxs-lookup"><span data-stu-id="70f3f-153">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

::: moniker-end

<span data-ttu-id="70f3f-154">`Add-Migration` Příkaz vytvoří kód k vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="70f3f-154">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="70f3f-155">Schéma je založena na zadaný ve model `DbContext`(v *Data/MvcMovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="70f3f-155">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="70f3f-156">`Initial` Argument se používá k pojmenování byla migrace.</span><span class="sxs-lookup"><span data-stu-id="70f3f-156">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="70f3f-157">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="70f3f-157">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="70f3f-158">V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="70f3f-158">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="70f3f-159">`Update-Database` Příkaz spustí `Up` metoda v *migrace nebo\<časové razítko > _Initial.cs* souboru, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="70f3f-159">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="70f3f-160">Místo pomocí PMC můžete provést kroky předcházející pomocí rozhraní příkazového řádku (CLI):</span><span class="sxs-lookup"><span data-stu-id="70f3f-160">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="70f3f-161">Přidat [EF základní nástrojů](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) k *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="70f3f-161">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="70f3f-162">Spusťte následující příkazy z konzoly (v adresáři projektu):</span><span class="sxs-lookup"><span data-stu-id="70f3f-162">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  <span data-ttu-id="70f3f-163">Pokud spustíte aplikaci a zobrazí chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="70f3f-163">If you run the app and get the error:</span></span>

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="70f3f-164">Pravděpodobně nebyl spuštěn `dotnet ef database update`.</span><span class="sxs-lookup"><span data-stu-id="70f3f-164">You probably have not run `dotnet ef database update`.</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="70f3f-165">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span><span class="sxs-lookup"><span data-stu-id="70f3f-165">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="70f3f-166">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="70f3f-166">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span></span>
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Kontextové nabídky IntelliSense pro položku modelu, seznam dostupných vlastností pro ID, ceny, datum vydání a název](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="70f3f-168">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="70f3f-168">Additional resources</span></span>

* [<span data-ttu-id="70f3f-169">Pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="70f3f-169">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="70f3f-170">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="70f3f-170">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="70f3f-171">[Předchozí přidávání zobrazení](adding-view.md)
> [další práci s SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="70f3f-171">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
