---
title: Přidat model do aplikace ASP.NET MVC jádra
author: rick-anderson
description: Přidáte model do jednoduchou aplikaci ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/8/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 4204d4e2d474db51692d42751a9f82373e9f0c0d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="b659f-103">Přidat model do aplikace ASP.NET MVC jádra</span><span class="sxs-lookup"><span data-stu-id="b659f-103">Add a model to an ASP.NET Core MVC app</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="b659f-104">Poznámka: Šablony ASP.NET 2.0 základní obsahují *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="b659f-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="b659f-105">Klikněte pravým tlačítkem myši *modely* složky > **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="b659f-105">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="b659f-106">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="b659f-106">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="b659f-107">`ID` Pole je vyžadováno pro databázi pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="b659f-107">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="b659f-108">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="b659f-108">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="b659f-109">Nyní máte **M**odelu ve vaší **M**VC aplikace.</span><span class="sxs-lookup"><span data-stu-id="b659f-109">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="b659f-110">Řadič generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="b659f-110">Scaffolding a controller</span></span>

<span data-ttu-id="b659f-111">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složky **> Přidat > řadiče**.</span><span class="sxs-lookup"><span data-stu-id="b659f-111">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![zobrazení výše krok](adding-model/_static/add_controller.png)

<span data-ttu-id="b659f-113">Pokud **přidat závislosti MVC** otevře se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="b659f-113">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="b659f-114">[Aktualizovat na nejnovější verzi sady Visual Studio](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="b659f-114">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="b659f-115">Visual Studio verze starší než 15,5 zobrazit tento dialog.</span><span class="sxs-lookup"><span data-stu-id="b659f-115">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="b659f-116">Pokud nelze aktualizovat, vyberte **přidat**a pak postupujte podle kroků řadiče přidat.</span><span class="sxs-lookup"><span data-stu-id="b659f-116">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="b659f-117">V **přidat vygenerované uživatelské rozhraní** dialogové okno, klepněte na **kontroler MVC se zobrazeními s využitím nástroje Entity Framework > Přidat**.</span><span class="sxs-lookup"><span data-stu-id="b659f-117">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Přidat vygenerované uživatelské rozhraní – dialogové okno](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="b659f-119">Dokončení **přidat kontroler** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="b659f-119">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="b659f-120">**Třída modelu:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="b659f-120">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="b659f-121">**Třída kontextu dat:** vyberte **+** ikonu a přidejte výchozí **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="b659f-121">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Přidat kontextu dat](adding-model/_static/dc.png)

* <span data-ttu-id="b659f-123">**Zobrazení:** zachovat ve výchozím nastavení mají jednotlivé možnosti zaškrtnuto</span><span class="sxs-lookup"><span data-stu-id="b659f-123">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="b659f-124">**Název řadiče:** ponechat výchozí *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="b659f-124">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="b659f-125">Klepněte na **přidat**</span><span class="sxs-lookup"><span data-stu-id="b659f-125">Tap **Add**</span></span>

![Dialogové okno řadiče přidání](adding-model/_static/add_controller2.png)

<span data-ttu-id="b659f-127">Visual Studio vytvoří:</span><span class="sxs-lookup"><span data-stu-id="b659f-127">Visual Studio creates:</span></span>

* <span data-ttu-id="b659f-128">Entity Framework Core [databáze třída kontextu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="b659f-128">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="b659f-129">Řadič filmy (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="b659f-129">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="b659f-130">Zobrazení syntaxe Razor soubory vytvořit, odstranit, podrobnosti, Edit a Index stránky (<em>zobrazení nebo filmy nebo&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="b659f-130">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="b659f-131">Automatické vytváření kontext databáze a [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení se označuje jako *generování uživatelského rozhraní*.</span><span class="sxs-lookup"><span data-stu-id="b659f-131">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="b659f-132">Brzy budete mít funkční webovou aplikaci, která vám umožní spravovat film databáze.</span><span class="sxs-lookup"><span data-stu-id="b659f-132">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="b659f-133">Pokud spustíte aplikaci a klikněte na **Mvc film** odkaz, dojde k chybě podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="b659f-133">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="b659f-134">Musíte vytvořit databázi, a budete používat jádro EF [migrace](xref:data/ef-mvc/migrations) funkci tak, aby to udělat.</span><span class="sxs-lookup"><span data-stu-id="b659f-134">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="b659f-135">Migrace umožňuje vytvořit databázi, která odpovídá datový model a aktualizovat schéma databáze, když datový model změny.</span><span class="sxs-lookup"><span data-stu-id="b659f-135">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="b659f-136">Přidejte EF nástrojů a provedení počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="b659f-136">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="b659f-137">V této části pomocí konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="b659f-137">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="b659f-138">Přidejte balíček základní nástroje Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b659f-138">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="b659f-139">Tento balíček je potřebné k přidání migrace a aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="b659f-139">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="b659f-140">Přidáte počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="b659f-140">Add an initial migration.</span></span>
* <span data-ttu-id="b659f-141">Aktualizace databáze s počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="b659f-141">Update the database with the initial migration.</span></span>

<span data-ttu-id="b659f-142">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="b659f-142">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Pomocí PMC nabídky](adding-model/_static/pmc.png)

<span data-ttu-id="b659f-144">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="b659f-144">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="b659f-145">**Poznámka:** Pokud obdržíte chybu s `Install-Package` příkaz, otevřete Správce balíčků NuGet a vyhledejte `Microsoft.EntityFrameworkCore.Tools` balíčku.</span><span class="sxs-lookup"><span data-stu-id="b659f-145">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="b659f-146">To vám umožňuje nainstalovat balíček nebo zkontrolujte, zda je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="b659f-146">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="b659f-147">Můžete si také přečíst [rozhraní příkazového řádku přístup](#cli) Pokud máte problémy s pomocí PMC.</span><span class="sxs-lookup"><span data-stu-id="b659f-147">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="b659f-148">`Add-Migration` Příkaz vytvoří kód k vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="b659f-148">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="b659f-149">Schéma je založena na zadaný ve model `DbContext`(v *Data/MvcMovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="b659f-149">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="b659f-150">`Initial` Argument se používá k pojmenování byla migrace.</span><span class="sxs-lookup"><span data-stu-id="b659f-150">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="b659f-151">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="b659f-151">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="b659f-152">V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="b659f-152">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="b659f-153">`Update-Database` Příkaz spustí `Up` metoda v *migrace nebo\<časové razítko > _Initial.cs* souboru, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="b659f-153">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="b659f-154">Místo pomocí PMC můžete provést kroky předcházející pomocí rozhraní příkazového řádku (CLI):</span><span class="sxs-lookup"><span data-stu-id="b659f-154">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="b659f-155">Přidat [EF základní nástrojů](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) k *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="b659f-155">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="b659f-156">Spusťte následující příkazy z konzoly (v adresáři projektu):</span><span class="sxs-lookup"><span data-stu-id="b659f-156">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  
  <span data-ttu-id="b659f-157">Pokud spustíte aplikaci a zobrazí chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="b659f-157">If you run the app and get the error:</span></span>
  
  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="b659f-158">Pravděpodobně nebyl spuštěn ` dotnet ef database update`.</span><span class="sxs-lookup"><span data-stu-id="b659f-158">You probably have not run ` dotnet ef database update`.</span></span>
  
[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model4.md)]

![Kontextové nabídky IntelliSense pro položku modelu, seznam dostupných vlastností pro ID, ceny, datum vydání a název](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="b659f-160">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b659f-160">Additional resources</span></span>

* [<span data-ttu-id="b659f-161">Pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="b659f-161">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="b659f-162">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="b659f-162">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="b659f-163">[Předchozí přidávání zobrazení](adding-view.md)
> [další práci s SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="b659f-163">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
