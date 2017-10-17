---
title: "Přidat model do aplikace ASP.NET MVC jádra"
author: rick-anderson
description: "Přidáte model do jednoduchou aplikaci ASP.NET Core."
keywords: "Jádro ASP.NET, MVC, vygenerovat, generování uživatelského rozhraní"
ms.author: riande
manager: wpickett
ms.devlang: csharp
ms.date: 09/22/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-1638-b903-b593059e9f39
ms.technology: aspnet
ms.prod: .net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 15fef14ab754b48935d17e754daa011acdd90b42
ms.sourcegitcommit: a73fb5161fbdbee1d2e8474205e9840a134ee2ad
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/12/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="af3e1-104">Klikněte pravým tlačítkem myši *modely* složku a potom vyberte **přidat** > **nový soubor**.</span><span class="sxs-lookup"><span data-stu-id="af3e1-104">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="af3e1-105">V **nový soubor** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="af3e1-105">In the **New File** dialog:</span></span>

  * <span data-ttu-id="af3e1-106">Vyberte **Obecné** v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="af3e1-106">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="af3e1-107">Vyberte **prázdné třídy** v problémové center.</span><span class="sxs-lookup"><span data-stu-id="af3e1-107">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="af3e1-108">Název třídy **film** a vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="af3e1-108">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="af3e1-109">Přidejte následující vlastnosti pro `Movie` třídy:</span><span class="sxs-lookup"><span data-stu-id="af3e1-109">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="af3e1-110">`ID` Pole je vyžadováno pro databázi pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="af3e1-110">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="af3e1-111">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="af3e1-111">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="af3e1-112">Nyní máte **M**odelu ve vaší **M**VC aplikace.</span><span class="sxs-lookup"><span data-stu-id="af3e1-112">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="af3e1-113">Příprava projektu pro generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="af3e1-113">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="af3e1-114">Klikněte pravým tlačítkem na soubor projektu a pak vyberte **nástroje > Upravit soubor**.</span><span class="sxs-lookup"><span data-stu-id="af3e1-114">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![zobrazení výše krok](adding-model/_static/1.png)

- <span data-ttu-id="af3e1-116">Přidejte následující zvýrazněnou balíčků NuGet *MvcMovie.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="af3e1-116">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="af3e1-117">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="af3e1-117">Save the file.</span></span>

- <span data-ttu-id="af3e1-118">Vytvoření *Models/MvcMovieContext.cs* souboru a přidejte následující `MvcMovieContext` třídy:[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="af3e1-118">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="af3e1-119">Otevřete *Startup.cs* souboru a přidejte dva direktiv Using:[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="af3e1-119">Open the *Startup.cs* file and add two usings:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="af3e1-120">Přidat kontext databáze, který má *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="af3e1-120">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="af3e1-121">Tato hodnota informuje Entity Framework, které třídy modelu jsou zahrnuty v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="af3e1-121">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="af3e1-122">Definujete jeden *sady entit* film objektů, které bude možné vyjádřit v databázi jako film tabulku.</span><span class="sxs-lookup"><span data-stu-id="af3e1-122">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="af3e1-123">Sestavte projekt a ověřte, zda že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="af3e1-123">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="af3e1-124">Vygenerované uživatelské rozhraní MovieController</span><span class="sxs-lookup"><span data-stu-id="af3e1-124">Scaffold the MovieController</span></span>

<span data-ttu-id="af3e1-125">Otevřete okno terminálu ve složce projektu a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="af3e1-125">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="af3e1-126">Pokud dojde k chybě `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span><span class="sxs-lookup"><span data-stu-id="af3e1-126">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="af3e1-127">Pokud jste v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="af3e1-127">You are in the project directory.</span></span> <span data-ttu-id="af3e1-128">Má adresáři projektu *Program.cs*, *Startup.cs* a *.csproj* soubory.</span><span class="sxs-lookup"><span data-stu-id="af3e1-128">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="af3e1-129">Je vaše dotnet verze 1.1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="af3e1-129">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="af3e1-130">Spustit `dotnet` získat verzi.</span><span class="sxs-lookup"><span data-stu-id="af3e1-130">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="af3e1-131">Jste přidali `<DotNetCliToolReference>` elment k [MvcMovie.csproj soubor](#prepare-the-project-for-scaffolding).</span><span class="sxs-lookup"><span data-stu-id="af3e1-131">You have added the `<DotNetCliToolReference>` elment to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="af3e1-132">Modul generování uživatelského rozhraní vytvoří následující:</span><span class="sxs-lookup"><span data-stu-id="af3e1-132">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="af3e1-133">Řadič filmy (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="af3e1-133">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="af3e1-134">Zobrazení syntaxe Razor soubory vytvořit, odstranit, podrobnosti, úpravy a Index stránky (*zobrazení nebo filmy nebo\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="af3e1-134">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="af3e1-135">Automatické vytváření [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení se označuje jako *generování uživatelského rozhraní*.</span><span class="sxs-lookup"><span data-stu-id="af3e1-135">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="af3e1-136">Brzy budete mít funkční webovou aplikaci, která vám umožní spravovat film databáze.</span><span class="sxs-lookup"><span data-stu-id="af3e1-136">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="af3e1-137">Přidání souborů do sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af3e1-137">Add the files to Visual Studio</span></span>

* <span data-ttu-id="af3e1-138">Přidat *MovieController.cs* souboru projektu sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="af3e1-138">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="af3e1-139">Klikněte pravým tlačítkem na *řadiče* složky a vyberte **Přidat > Přidat soubory**.</span><span class="sxs-lookup"><span data-stu-id="af3e1-139">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="af3e1-140">Vyberte *MovieController.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="af3e1-140">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="af3e1-141">Přidat *filmy* složek a zobrazení:</span><span class="sxs-lookup"><span data-stu-id="af3e1-141">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="af3e1-142">Klikněte pravým tlačítkem na *zobrazení* složky a vyberte **Přidat > Přidat existující složku**.</span><span class="sxs-lookup"><span data-stu-id="af3e1-142">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="af3e1-143">Přejděte na *zobrazení* složky, vyberte *Views\Movies*a potom vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="af3e1-143">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="af3e1-144">V **vyberte soubory, které chcete přidat z filmy** vyberte **zahrnují všechny**a potom **OK**.</span><span class="sxs-lookup"><span data-stu-id="af3e1-144">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="af3e1-145">Nyní máte databázi a stránkami zobrazit, upravit, aktualizovat a odstranit data.</span><span class="sxs-lookup"><span data-stu-id="af3e1-145">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="af3e1-146">V dalším kurzu jsme budete pracovat s databází.</span><span class="sxs-lookup"><span data-stu-id="af3e1-146">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="af3e1-147">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="af3e1-147">Additional resources</span></span>

* [<span data-ttu-id="af3e1-148">Pomocníci značky</span><span class="sxs-lookup"><span data-stu-id="af3e1-148">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="af3e1-149">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="af3e1-149">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="af3e1-150">[Předchozí přidávání zobrazení](adding-view.md)
[další práci s SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="af3e1-150">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
